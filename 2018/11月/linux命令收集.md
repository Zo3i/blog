>懒人自有懒人法,不想记住的Linux命令,慢慢收集咯;

###正紧代码

>查看端口占用情况:
netstat -anp|grep 80

---
>解压缩文件:
>>.tar
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）

>---

>>.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName

>---

>>.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName

>---

>>.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName

---
>后台启动
nohup process >/dev/null 2>/dev/null &





