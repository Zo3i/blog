>事情是这样的:使用zookeeper监控A后端跟新的BGM列表,B后端同步数据.达到分布式的作用.然鹅..因为编码问题困扰了好久,最后总算是有了一个解决方案,这里做一下记录.

###需求
```graph
A后端(win):  "bgm\陈奕迅 - 倾城.mp3" =>
zookeeper(linux):  "bgm\\陈奕迅 - 倾城.mp3" =>
B前端(win | linux):  "bgm/%E9%99%88%E5%A5%95%E8%BF%85%20-%20%E5%80%BE%E5%9F%8E.mp3"
```
###正题
>##方案一:Url直接编码
后端A:
>```java
>String path = "urlPath";
>path = new String(path.getBytes("UTF-8"),"GBK"));
>```
后端B:
>```java
>URLEncoder.encode(path.getBytes()),"UTF-8");
>```

---

>##方案二: 用base64编码后再解码
>后端A:
>```java
>String path = "urlPath";
>path = >>Base64.getUrlEncoder().encodeToString(path.getBytes>>("UTF-8"));
>```
>后端B:
>```java
>String bgmPath = new >>String(Base64.getUrlDecoder().decode(map.get("path">>)));
>```

###最后:
>如果URL中带有空格需要手动替换一下:
```java
finalPath = finalPath.replaceAll("\\+", "%20");
```

