> 前言:  第一次加班真刺激!

### 教程内容

mysql 5.6以及mysql 5.7版本,shell脚本无交互设置密码并开启远程访问

### mysql 5.6 一键脚本

```shell
#!/bin/bash

echo "begin install mysql 5.6"
sleep 3

wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install -y mysql-server
service mysqld restart

echo "update password and access remote"
sleep 1

echo -e "\n" | mysql -uroot  <<EOF
SET PASSWORD FOR root@'localhost' = PASSWORD('Abc12345**');
GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "Abc12345**";
EOF
echo -e "\033[31m数据库安装完成,你的数据库密码为:Abc12345**(已开启远程访问权限)\033[0m"
```

**5.6版本不同于5.7,它初始没有密码,所以登录只需要一个回车即可,使用管道自动输入密码登录;在使用EOF作为子命令修改mysql的密码以及创建一个远程账户;**



------



### mysql 5.7一键安装脚本

```shell
sudo rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
sudo yum -y install mysql-community-server
systemctl start mysqld
mysqltmppwd=`cat /var/log/mysqld.log | grep -i 'temporary password'`
mysql  -uroot -p"${mysqltmppwd: -12}"  --connect-expired-password  -e "ALTER USER 'root'@'localhost' IDENTIFIED BY 'Abc12345**';"
mysql  -uroot -p"Abc12345**"  --connect-expired-password  -e "grant all privileges on *.* to 'root' @'%' identified by 'Abc12345**';"
echo "##########################################################################"
echo -e "\033[31m数据库安装完成,你的数据库密码为:Abc12345**(已开启远程访问权限)\033[0m"
echo "##########################################################################"
```

