docker 入门的时候碰到的一个问题，让我研究了很久，虽然是一个很简单的问题。但是困扰了我很多天。今天写教程把它记录下来。

### 内容简介

多个docker部署微服务的时候，一些服务必须要依赖另外一个服务才可以运行。例如Java项目中必须先运行数据库才可以运行Java项目。但是docker官方的docker-compose并不支持编排服务的启动顺序。所以官方就出现了一个脚本。让系统监控你启动的服务端口对否开启，来监听服务是否开启。

### 抛出解决方案

```
version : '2'
services:
  mysql:
    build: mysql
    image: jo/mysql
    volumes:
           - ./src/main/docker/mysql/my.cnf:/root/mysql/my.cnf mysql
    command: --max_allowed_packet=32505856
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
    entrypoint: "./w.sh mysql:3306 -- java -jar /app.jar"
```

以上为最后的解决方案，其中 entrypoint: "./w.sh mysql:3306 -- java -jar /app.jar"

为解决核心，使用官方的脚本，监控mysql服务的3306端口是否启动，启动之后才允许Java程序。即可解决启动顺序的问题。

### 附上脚本链接

[https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh](https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh)