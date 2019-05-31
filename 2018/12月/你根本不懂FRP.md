>你根本不懂FRP_

###正经教程

##step 1.
[github下载服务端以及客户端](https://github.com/fatedier/frp/releases)

##step 2.
配置服务端(frps.ini):
```ini
[common]
bind_port = 7000
bind_addr = 0.0.0.0
#vhost_http_port网站访问端口
vhost_http_port = 80
#vhost_https_port = 443
#dashboard_port状态以及代理统计信息展示,网址:7500可查看详情
dashboard_port = 7500
log_file = ./frps.log
log_level = info
log_max_days = 3
#privilege_mode 特权模式,开通后web,ssh等使用都可以直接在客户端设置
privilege_mode = true
#特权连接密码
privilege_token = xxx
#max_pool_count最大链接池,每个代理预先与后端服务器建立起指定数量的最大链接数
max_pool_count = 50

type = http
subdomain_host = 你的域名
auth_token = xxxx
```

##step 3.
配置客户端(frpc.ini):
```ini
[common]
#server_addr服务器ip
server_addr = 服务器IP
server_port = 7000
log_file = ./frpc.log
log_level = info
log_max_days = 3
#特权连接密码
privilege_token = 特权连接密码(同服务端)

#如果多人使用同一个服务端,下面这个v 要不一样
[v]
#privilege_mode特权模式
privilege_mode = true
type = http
#本地程序地址
local_ip = localhost
#本地程序端口
local_port = 8080
#custom_domains域名    开启后通过i.你的域名:服务端设置的端口访问
subdomain = i
pool_count = 10
auth_token = 同服务端
```

##step 4.
服务端开启服务命令:
>./frps -c ./frps.ini
服务端后台运行命令:
>nohup ./frps -c ./frps.ini >/dev/null 2>/dev/null &

客户端开启服务命令:
>frpc.exe -c frpc.ini

PS: 服务器如果没有权限的话执行:
>chmod -R 777 frp文件夹

##注意点.
- 需要有自己的服务器;
- 域名解析可设置为: *.xxx你的域名(泛域名);
- 如果没有设置泛域名,那就需要客户端服务器服务器都进行解析;






