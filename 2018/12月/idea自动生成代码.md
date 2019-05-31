>优秀大学生毕业设计,从代码自动生成开始;

现今开发框架愈发完善,如果你基本的entity, dao, service, mapper都自己写的话,你的开发速度就会比别人慢一大截;企业开发很多无关业务的代码都是一键生成...一把梭;本教程是介绍IDEA上的一款优秀的代码生成插件;

###环境

>- IDE: idea 2018.3
>- 框架: Srping boot 1.5.9.RELEASE
>- JDK: java 1.8

###正经教程

- **下载插件:**
idea->File->Setting->Plugins
![Easy Code](https://zxx.im/wp-content/uploads/2018/12/N_GG35646@0AOLPGQ.png)

**下载,安装,重启IDEA;**

---

- **配置数据库**
![配置数据库](https://zxx.im/wp-content/uploads/2018/12/7IAIO7HWXO7WTQK1IVG.png)
![配置](https://zxx.im/wp-content/uploads/2018/12/WRJRRRB_0930MI5LO90Q.png)

**如果Test Connection无法点击请安装驱动;如下图所示,点击安装驱动**

![安装驱动](https://zxx.im/wp-content/uploads/2018/12/SINZFNBOI0EO_HDK.png)

**选择数据库**
![选择数据库](https://zxx.im/wp-content/uploads/2018/12/Y8WRY4466NM80_L4@7UIE-1.png)

**选择数据库,确认即可;**

---

- **快乐生成代码**

**选择需要生成的表(可多选)**
![选择表](https://zxx.im/wp-content/uploads/2018/12/AG@RTWK5SOPDURQ1ID.png)

![配置生成目录,以及需要生成的代码](https://zxx.im/wp-content/uploads/2018/12/4LSQA@7QIWD1U1IGFV3Q.png)

**上图圈起来的路径,就是文件生成后的位置,确认后,代码就自动生成好了.**

![生成的代码](https://zxx.im/wp-content/uploads/2018/12/FJRQ98EJVFLRCERTNOMO.png)

**根据数据库中的表,即可生成对应的代码;**

---

###测试可用性

![生成的dao](https://zxx.im/wp-content/uploads/2018/12/YOSLKPABQ_66GFCEJYYSR.png)
**自动生成的代码已经具备了最常用的增删改查,还有多条数据查询**

---
简单的写一个form往数据库插入数据,发现了问题;
- 控制层: @RestController应修改为@Controller
- mapper: 插入数据的时候,如果id不是自增长,则需要自行添加ID;

![controller](https://zxx.im/wp-content/uploads/2018/12/AVRKQZ1WI_VIH7NGP6TP.png)
![修改xml](https://zxx.im/wp-content/uploads/2018/12/EWYB@L0M3L48@L.png)

###总结

还是很好用的一个自动生成代码的一个插件,对于优秀大学生毕业设计来说.这个插件足够了,省去很多麻烦事.我把这个demo放在Github,下载后修改数据库,开箱即可使用;即使是使用springMVC也是可以用个这个demo去生成代码,然后拷贝到自己的项目中.

###Github项目地址:
**[https://github.com/Zo3i/demo](https://github.com/Zo3i/demo)**


###最后的彩蛋:

**一键生成代码后,看了一下代码量统计.毕设还没开始写,已经有3000多行的代码量了..**

![代码统计](https://zxx.im/wp-content/uploads/2018/12/HY9YGEXJKCB4CQCHN4B8W.png)










