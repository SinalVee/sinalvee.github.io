title: Sails+wechat(解决sails无法处理微信消息)
date: 2015-09-18 14:06:04
updated: 2015-09-18 14:06:04
categories:
  - NodeJS
tags:
  - nodejs
  - sails
  - wechat
---

[sails](https://github.com/balderdashy/sails)框架使用[wechat](https://github.com/node-webot/wechat) 的时候能够通过验证却不能处理微信消息，原因是sails默认的bodyPaser([skipper](https://github.com/balderdashy/skipper))不支持'text/xml'形式的请求([sails issues#2714](https://github.com/balderdashy/sails/issues/2714))

YunnuY同学使用[express-xml-bodyparser](https://www.npmjs.com/package/express-xml-bodyparser)和[body-parser](https://www.npmjs.com/package/body-parser)覆盖了sails默认的bodyParser

[https://github.com/YunnuY/sweat/blob/master/config/http.js](https://github.com/YunnuY/sweat/blob/master/config/http.js)

由于body-paser后来更新，所以这里的代码要稍微修改一下,将`var bodyParser = require('body-parser')();`修改为`var bodyParser = require('body-parser')().json();`

* * *

但是公司的项目中还有包含文件的请求，此时body-paser就不能满足需求了，因此还是要重新使用sails默认的skipper，代码如下：

    bodyParser: function() {  
      var xmlParser = require('express-xml-bodyparser')();
      var skipper = require('skipper')();
      return function(req, res, next) {
        if (req.headers &amp;&amp; (req.headers['content-type'] == 'text/xml' || req.headers['content-type'] == 'application/xml')) {
          return xmlParser(req, res, next);
        }
        return skipper(req, res, next);
      };
    }

目前程序能够正常处理微信消息和常规请求，如果出现问题我会及时更新。
