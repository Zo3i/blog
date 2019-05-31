> 前言:在宿舍挣扎了两天,终于动手写下这本早该写了的博客...

### 内容简介

本篇,为docker入门教程,几乎等于C语言的Hello World! 学完hello world我们就算入门了hhh

### 教程

- 安装docker 环境(环境就不说了吧..如果你懒得百度下面是一键代码(**CentOs7**))

  ```shell
  ### 安装docker ###
  # 安装一些必要的系统工具
  sudo yum install -y yum-utils device-mapper-persistent-data lvm2
  # 添加软件源信息
  sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  # 更新 yum 缓存
  sudo yum makecache fast
  # 安装 Docker-ce
  sudo yum -y install docker-ce
  # 启动docker并设置为开机启动(centos7)
  systemctl  start docker.service
  systemctl  enable docker.service
  # 替换docker为国内源
  echo '{"registry-mirrors": ["https://registry.docker-cn.com"],"live-restore": true}' > /etc/docker/daemon.json
  systemctl restart docker
  # 安装dokcer-compose
  sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  chmod +x /usr/local/bin/docker-compose
  ### 安装docker结束 ###
  ```

  

- 打包java为jar包(设置数据库为**远程数据库**,本次部署为单个docker所以就不装mysql集群了.)

- 写一个Dockerfile, 以下为示例

  ```dockerfile
  FROM java:8
  VOLUME /tmp
  ADD ./你的jar包名.jar app.jar
  RUN bash -c 'touch /app.jar'
  ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
  ```

  以上修改你的jar包名即可;

- 将jar包和Dockerfile放在同一个文件夹上传linux;
- 执行 ```docker build -t jo/demo .```  (其中:jo/demo为镜像名字, . 为当前文件夹)
- 执行 ```docker run -p 8080:8080 -t jo/demo``` (8080为项目端口)
- 浏览器访问:ip + 端口 + /项目名 即可访问;

