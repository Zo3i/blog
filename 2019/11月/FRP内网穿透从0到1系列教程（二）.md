> 前言：如果你已经看完上篇的Frp介绍，那么恭喜你可以开始跟着本教程来使用Frp的各种内网穿透功能了。

### 防火墙关闭

对于新手来说很多时候，明明跟着教程走，但是就是不行，很可能是防火墙问题。这里建议先关闭防火墙（**相对不安全**)；可能需要在服务器购买的地方开放所有端口，然后在服务器上关闭防火墙。

### 泛域名解析

哪里买的域名，就去哪里设置泛域名解析。![](https://zxx.one/imgs/2019/11/e953ea07900c0d73.png)

### 程序下载

在作者的github下载任意发行版，但是要注意版本很多不要下载错。[前往官网]([https://github.com/fatedier/frp/releases])

这里服务器系统以**Centos7**为例，下载**linux_amd64**版本。

![](https://zxx.one/imgs/2019/11/f73f5b1ae76bcefa.png)

解压之后我们就得到了在linux 环境可运行的服务端以及客户端，这里我们只需要服务端的程序即可。

注：frpc(frp client) 是客户端。frps(frp server)是服务端。

---

### 配置服务器端frps.ini

```ini
[common]
bind_port = 7000
bind_addr = 0.0.0.0
#控制面板配置,网址:7500可查看详情
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin
#日志
log_file = ./frps.log
log_level = info
log_max_days = 3
#特权模式,开通后web,ssh等使用都可以直接在客户端设置
privilege_mode = true
#特权密码
privilege_token = QeWer
#鉴权密码
auth_token = token
#链接池,每个代理预先与后端服务器建立起指定数量的最大链接数
max_pool_count = 50
#连接协议
type = http
#web网站访问端口
vhost_http_port = 88
#泛域名
subdomain_host = 你的域名
```

以上为服务端的配置代码，稍微有点复杂。只需要记住鉴权密码，特权密码与客户端一致即可和填写泛域名。其余部分可以先不管。复制粘贴一把嗦。

### linux运行程序

首先我们需要把文件上传到服务器，需要用到的工具**xshell**,**xftp**（自行下载）

![](https://zxx.one/imgs/2019/11/3695dd258397e23a.png)

首先登录服务器，然后按图示，点击按钮（新建文件传输）。就可以登录到ftp的界面。把本地的frps，frps.ini，frps_full.ini 复制粘贴到服务器。

---

完成以上操作之后你在xshell执行ls就可以看到

![](https://zxx.one/imgs/2019/11/ce2f662a76a2b029.png)

还需要给这些文件运行的权限，你需要执行

```
chmod +x ./*
```

然后就可以愉快的运行程序了。

---

执行运行代码

```
./frps -c frps.ini
```

![](https://zxx.one/imgs/2019/11/6abe99fcff8ed3b6.png)

看到屏幕输出这些，说明你运行成功了。

---

还需要注意的是，此时为前台运行。无法执行其他操作，界面不是卡住了。就是这样的。如果你想后台运行程序的话需要执行以下代码

```
nohup ./frps -c frps.ini >/dev/null 2>/dev/null &
```

![](https://zxx.one/imgs/2019/11/4419d07d541c40f4.png)

运行之后会有一个任务ID给你，如果你想停止运行，执行

```
kill -9 任务id 
```

### 总结

服务端部署大概就是这样，期间涉及到一些命令和域名解析。但是不难，碰到不懂得问题，留言即可~

