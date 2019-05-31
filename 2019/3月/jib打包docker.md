> 前言: 沉迷于Docker部署,无法自拔;

### 本篇内容

这段时间折腾了docker部署项目,几乎把手上的项目都用了docker部署了遍,但是考虑到生产环境的时候还是过于繁琐; 就在昨天看到JIB,谷歌团队研发的一个maven插件;用了一下,我哭了,部署竟能这么轻松;所谓真 DevOps;

### 正紧教程

- **在docker hub创建一个账号,并创建仓库(这里演示用的是docker-hub,阿里docker仓库同理)**

- **导入Maven插件(最简配置)**

  ```xml
  <!--jib  mvn compile jib:build-->
  <plugin>
     <groupId>com.google.cloud.tools</groupId>
      <artifactId>jib-maven-plugin</artifactId>
      <version>1.0.2</version>
      <configuration>
          <from>
              <image>openjdk:alpine</image>
          </from>
          <to>
              <image>registry.hub.docker.com/zxx267/frp</image>
          </to>
      </configuration>
      <executions>
          <execution>
              <phase>package</phase>
              <goals>
                  <goal>build</goal>
              </goals>
          </execution>
      </executions>
  </plugin>
  ```

- **构建镜像**

```mvn compile jib:build``` 首次会报错: 401 Unauthorized!(是叫你登录账号...)

执行以下代码:

> docker login --username 用户名 registry.hub.docker.com
>
> docker login --username 用户名 registry.cn-hangzhou.aliyuncs.com

push成功之后就大功告成啦!!!

- **运行**

本地,或者linux,在任何拥有docker环境的机子上执行:

```docker run -p 8080:8080 zxx267/frp```

- **最后的友情提示**

  我在打包镜像的时候由于网络的问题,经常time out,请相信它是可以的,只要你多试几次;