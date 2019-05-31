> 这都9102了还不用docker部署项目? 所以果断把毕设部署打包部署docker..

---
###容器
**我们需要4个容器合集: java, mysql, nginx, redis**

###准备:
- **修改pom.xml**
```xml
<artifactId>jsweb</artifactId>
<packaging>jar</packaging>
...
<properties>
    <!--docker支持-->
    <!-- mvn clean package docker:build-->
    <docker.image.prefix>jsWeb</docker.image.prefix>
</properties>
...
<plugins>
    <!-- Docker maven plugin -->
    <plugin>
        <groupId>com.spotify</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>1.0.0</version>
            <configuration>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <dockerDirectory>src/main/docker</dockerDirectory>
            <resources>
                <resource>
                    <targetPath>/</targetPath>
                        <directory>${project.build.directory}</directory>
                        <include>${project.build.finalName}.jar</include>
                </resource>
            </resources>
            </configuration>
    </plugin>
    <!-- Docker maven plugin -->
</plugins>
```
---
- **修改aplication.yml,项目中需要依赖mysql和redis**
```yml
jdbc: #docker
  type: mysql
  driver: com.mysql.jdbc.Driver
  url: jdbc:mysql://mysql:3306/js?useSSL=false&useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&serverTimezone=GMT
  username: docker
  password: root
  testSql: SELECT 1
redis:
  enabled: true
  host: redis
  port: 6379
  database: 0
  password: 1234
  pool:
    maxIdle: 3
    maxTotal: 20
```
将连接名改为需要连接的依赖名(在docker-compose.yml中配置);

---

- **本地打包jar包-jsweb.jar**

进入到项目目录下运行：```mvn clean package```

---

- **导出数据库-js.sql**
将数据库中的数据导出;

---
###容器编排

目录结构:
```
docker
│   docker-compose.yml
│   w.sh    
│
└───jsweb
│   │   Dockerfile
│   │   jsWeb.jar
│   
└───mysql
    │   Dockerfile
    │   js.sql
    │   privileges.sql
    │   my.cnf
└───nginx
    │   Dockerfile
    │   nginx.conf
```

---
###好戏开始了,买好瓜子汽水慢慢看吧

- **java的Dockerfile**
```
FROM java:8
#VOLUME /tmp
ADD jsWeb.jar app.jar
RUN bash -c 'touch /app.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```
---

- **mysql的Dockerfile**
```
FROM mysql:5.7

#设置免密登录
ENV MYSQL_ALLOW_EMPTY_PASSWORD yes

#将所需文件放到容器中
COPY js.sql /docker-entrypoint-initdb.d
COPY privileges.sql /docker-entrypoint-initdb.d
```

---

- **mysql数据库授权-privileges.sql**
```sql
use mysql;
select host, user from user;
-- 因为mysql版本是5.7，因此新建用户为如下命令：
-- 将js数据库的权限授权给创建的docker用户，密码为root：
grant all on js.* to docker@'%' identified by 'root' with grant option;
-- 这一条命令一定要有：
flush privileges;
```
---

**my.cnf**
```
[mysql]
default-character-set=utf8

[mysqld]
character-set-server=utf8
```

---

**nginx的Dockerfile**
```
FROM hub.c.163.com/library/nginx

MAINTAINER jo "tionsin@live.com"

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY ./dist/ /usr/share/nginx/html/
EXPOSE 99

CMD ["nginx", "-g", "daemon off;"]

```
---

**nginx.conf**
```
server {
        listen 99;
        server_name  localhost;
        charset utf-8;
        error_page   500 502 503 504  /50x.html;
        location = / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        location /api {
        proxy_set_header   X-Real-IP $remote_addr; #转发用户IP
            proxy_pass http://jsweb:8090/jsweb/a/third;
        }
   }

```
---
**w.sh脚本**
[https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh](https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh)

---

**最后的硬菜 docker-compose.yml**
```
version : '2'
services:

  redis:
    image: redis:3
    command: redis-server --requirepass 1234
    ports:
      - "6379:6379"
  nginx:
    build: nginx
    image: jo/nginx
    ports:
      - "99:99"
    expose:
      - "99"  
    volumes:
      - ./nginx/dist/:/etc/nginx/html/
    links:
      - jsweb

  mysql:
    image: js/mysql
    volumes:
           - ./src/main/docker/mysql/my.cnf:/root/mysql/my.cnf mysql
    environment:
       - MYSQL_ROOT_PASSWORD=123456
    ports:
       - "3306"
    expose:
       - "3306"

  jsweb:
    build: jsweb
    image: jo/jsweb
    volumes:
      - ./w.sh:/w.sh
    ports:
      - "8090:8090"
    restart: always
    depends_on:
      - mysql
    links:
      - mysql
      - redis
    entrypoint: "./w.sh mysql:3306 -- java -jar /app.jar"

```


以上所有文件夹上传到服务器,然后先将mysql,打包为镜像;然后运行**docker-compose build**打包所有镜像,运行**docker-compose**就可以看到所以服务都成功运行了!

###几个注意点
- docker启动顺序: 因为mysql需要导入数据所以启动的比java程序慢,所以会导致java程序报错,这里采用的是官方的做法使用wait-for-it.sh监控mysql是否已经运行,之后再运行java程序;
- 数据库只需要构建一次,之后不需要构建;
- nginx的静态目录是vue项目;使用```./dist/ /usr/share/nginx/html/```复制文件到容器中;
- 注意当docker销毁时,数据也就销毁了,注意备份;











