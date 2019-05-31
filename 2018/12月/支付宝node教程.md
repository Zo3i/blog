>因为机缘巧合,写了支付宝的支付模块,其中深坑无数,故写篇文章记录下来;

###技术&框架
- **前端**: VUE
- **后端**: node(KOA)
###流程
**获取沙箱环境->配置密钥->后端生成URL->前端调用**
###正经教程
#第一步:
[登录支付宝开发者中心](https://openhome.alipay.com/platform/appDaily.htm?tab=info)  申请一个沙箱环境;然后就可以配置秘钥啦;

#第二步
获取公钥和私钥
![公钥和私钥](https://zxx.im/wp-content/uploads/2018/12/1@4YJG890IVUXFZD92JW-1.png)
根据[支付宝官方文档](https://docs.open.alipay.com/291/105971)生成私钥和公钥,并保存;

**注意点:**
- 编辑保存请使用编辑器,系统自带的文本编辑器,可能会影响格式(多加空格啥的)
- 私钥格式
> -----BEGIN RSA PRIVATE KEY----- (换行)
##########你的私钥  (换行)
-----END RSA PRIVATE KEY-----

- 公钥格式
不允许加入空格;
#第三步:
代码结构如下:
![代码结构](https://zxx.im/wp-content/uploads/2018/12/2ZL6CTPDR4RFY0DPF.png)
其中用到的是:
- lib/pem: 公钥,私钥
- lib/alipay.js & util.js大佬封装的工具类
- routes/alipay.js 路由
- view/alipay 前端表单
- app.js 主入口

**我会把代码打包,一些工具类自取;**

---
主要代码逻辑:

```javascript
var Alipay = require('./lib/alipay');
//初始化
var ali = new Alipay({
  appId:"你的APPID",
  rsaPrivate:path.resolve('./lib/pem/pre.pem'),//私钥
  rsaPublic:path.resolve('./lib/pem/pub.pem'),//公钥
  sandbox:true,//是否沙箱
  signType:"RSA2"//加密方式
});

//付款请求
app.post('/submit/orders',function(req,res){
  var param = {};
  param.subject = req.body.subject;
  param.outTradeId = req.body.outTradeId;
  param.amount = req.body.amout;
  param.body = req.body.body;
  param.charset = "utf-8"
  param.method = "alipay.trade.page.pay"
  param.product_code = "FAST_INSTANT_TRADE_PAY"
  var urlparm = ali.webPay(
    param
  );
  var url_API = ('https://openapi.alipaydev.com/gateway.do?'+ urlparm)
  console.log(url_API)
});
```
注意点:
- param是一个对象:以上是必须的参数,请参考[官方文档](https://docs.open.alipay.com/270/alipay.trade.page.pay),参数请严格按照官方文档写;
- 时间格式 yyyy-MM-dd HH:mm:ss
- url编码(记得编码,也不要多次编码)

---

#第三步:

前端拿到代码就可以调用咯!但是ajax使用window.open窗口会被拦截,于是有了以下解决方案:
```javascript
var winHandler = window.open('', '_blank')
$.ajax(function () {
    url: xxx,
}).then(res => {
    winHandler.location.href = res.data
})
```

***完美结束!***
后端代码:[下载](https://zxx.im/file/ali.rar)















