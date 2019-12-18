title: git添加多个远程url
date: 2015-11-06 11:11:14
updated: 2015-11-06 11:11:14
categories:
  - Git
tags:
  - git
---

假设要添加的三个url为
```
<url1>: https://github.com/sinalvee/111.git
<url2>: https://github.com/sinalvee/222.git
<url3>: https://github.com/sinalvee/222.git
```

使用如下方法
```
git remote add origin <url1>
git remote set-url --add origin <url2>
git remote set-url --add origin <url3>
```

push的时候直接`git push origin master`即可依次向三个地址提交
