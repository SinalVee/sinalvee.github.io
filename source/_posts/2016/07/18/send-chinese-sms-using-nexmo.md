title: 使用Nexmo发送中文短信
date: 2016-07-18 02:51:31
updated: 2016-07-18 02:51:31
categories:
  - NodeJS
tags:
  - nodejs
---

最近公司的项目要发送短信通知，因为用户都是国际用户，因此国际短信是必不可少的，一开始打算选择国内的【云片】平台，但是签名不能是全英文有些尴尬，最终选择了【Nexmo】。

Nexmo发送短信的[API](https://docs.nexmo.com/messaging/sms-api)还是非常简单的，一些语言也给出了详细的使用例子。这里copy一下nodejs的例子。

需要注意的是，如果发送的内容带有中文等字符，需要将`type`设置为`unicode`，不然的话收到的短信中这些字符会全部为问号。

<!-- more --> 

```js
var https = require('https');

var data = JSON.stringify({
  api_key: 'API_KEY',
  api_secret: 'API_SECRET',
  to: '441632960960',
  from: '441632960961',
  text: '你好',
  type: 'unicode'
});

var options = {
  host: 'rest.nexmo.com',
  path: '/sms/json',
  port: 443,
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': Buffer.byteLength(data)
  }
};

var req = https.request(options);

req.write(data);
req.end();

var responseData = '';
req.on('response', function(res){
  res.on('data', function(chunk){
    responseData += chunk;
  });

  res.on('end', function(){
    console.log(JSON.parse(responseData));
  });
});
```  