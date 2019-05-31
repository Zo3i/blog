> 前言: 如何快速部署一个前端项目,你可能只需要一台服务器和一键脚本...

### 内容简介

只需三番钟,这是你没玩过的全新版本,升级打怪,一刀999

### 正经教程

- **一键安装docker**

  [点此查看](https://dev.tencent.com/u/Tionsin/p/OCS/git/raw/master/docker/dockerInstall.sh)

- **项目结构**

  ![](https://i.bmp.ovh/imgs/2019/03/cb74812c42760476.png)

- **重要配置**

Dockerfile:

```dockerfile
FROM hub.c.163.com/library/nginx
MAINTAINER jo "tionsin@live.com"
RUN rm -rf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```

nginx.conf:

```conf
server {
        listen 80;
        server_name  localhost;
        charset utf-8;
        error_page   500 502 503 504  /50x.html;
        location = / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
   }
```

然后上传到服务器即可

------

### 正式部署

cd 到上传文件的根目录:

- 打包镜像:

```
docker build -t jo/nginx .
```

- 运行docker

```
docker run -p 80:80 -v /root/docker/html/:/etc/nginx/html/ jo/nginx
```

PS:  -p 为使用的端口; -v 为本地挂载到docker的宿主文件夹; 冒号前面使用绝对路径(本地文件夹)  最后jo/nginx 为镜像的名字;

### 最后

docker有很多可玩之处,换一个端口又是一个容器,而且不会占用太多资源,部署简单.真是项目部署的一大神器!