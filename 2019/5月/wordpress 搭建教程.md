> 前言：种一棵树最好的时间是十年前，而后是现在。做技术的维护一个博客，一方面是帮助他人成长，一方面也可以让自己的思维更加完善。把你学会的东西表达给别人，才是正在的学会了这个技术。

### 内容简介

今天介绍的是，如何搭建一个属于自己的博客。只要你的服务器性能达到基本需求，都可以用来写博客。本博客就是使用**LNMP+WORDPRESS**搭建的，可以说是非常方便了。

### 教程

- 安装环境(自行选择需要的版本)

  ```
  wget http://soft.vpser.net/lnmp/lnmp1.5.tar.gz -cO lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && ./install.sh lnmp
  ```

- 此版本的web根路径为：**/home/wwwroot/default**

- 下载wordpress中文版：[https://cn.wordpress.org/download/](https://cn.wordpress.org/download/)

- 上传到web根目录

  > 第一种：下载到本地之后，解压。使用XFTP上传到web根目录
  >
  > 第二种（推荐）：使用wget命令直接在linux上下载解压。使用mv命令移动到web根目录

- 如果放在根目录需要将index.html改名，避免冲突。之后访问ip:80即可访问。

- 访问ip/phpmyadmin/然后创建数据库，后面的操作需要。

- 访问ip:80即可访问wordpress。第一次访问会需要你配置网站基本信息。如网站名字，数据库之类。按照之前设置的填写即可。

### 权限设置

如上设置完成即可访问自己的博客了。但是为了更换主题或是安装插件，我们需要给目录读写权限。

```
chown -R www:www /home/wwwroot/default
chmod -R 755 /home/wwwroot/default/wp-content
```

### 大功告成

最后开心的博客吧~