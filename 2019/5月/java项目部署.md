> 前言： 如何部署一个java项目？其实这是一个java后端的基本素质。但是在多次重装服务器之后，我慢慢厌烦了一次次地部署环境。所以让那些该死的配置都交给一键脚本吧。

### 内容简介

我们部署一个跑java项目的环境，往往需要配置JDK，然后就是Tomcat,数据库。所以我就写了一个一键部署JDK+Tomcat+Mysql的脚本。用于新的服务器安装环境。在安装完环境之后，只需要把war包导入到**/usr/local/tomcat/webapp/**目录底下然后重启Tomcat即可直接访问项目。

### 环境版本

- Centos7
- JDK8
- Tomcat8.5
- Mysql5.7

### 配置环境

```
wget https://raw.githubusercontent.com/Zo3i/OCS/master/jdTomK%26Auto/JdTomK-Auto.sh
source JdTomK-Auto.sh
```

### 注意事项

- 项目包的数据库需配置正确，并且确保数据导入。
- 无法访问的时候检查防火墙端口是否打开。

