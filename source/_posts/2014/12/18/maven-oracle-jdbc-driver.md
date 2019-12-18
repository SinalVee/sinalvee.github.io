title: Maven仓库中添加Oracle JDBC驱动
date: 2014-12-18 16:10:08
updated: 2014-12-18 16:10:08
categories:
  - Java
tags:
  - java
  - jdbc
  - oracle
---

原文地址：http://maosheng.iteye.com/blog/2001615

由于Oracle授权问题，Maven不提供Oracle JDBC driver，为了在Maven项目中应用Oracle JDBC driver,必须手动添加到本地仓库。

## 一.获得Oracle JDBC Driver

1.通过Oracle官方网站下载相应版本：http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html

2.通过Oracle的安装目录获得，位置在“{ORACLE_HOME}jdbclibojdbc14.jar”

## 二.手动安装到本地仓库

命令如下：
```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.1.0 -Dpackaging=jar -Dfile=D:ojdbc14.jar
```

## 三.在pom.xml文件中添加引用

```xml
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc14</artifactId>
    <version>10.2.0.1.0</version>
</dependency>
```

之后就可以正常引用了。
