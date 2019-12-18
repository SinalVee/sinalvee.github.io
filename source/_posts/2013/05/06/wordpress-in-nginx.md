title: wordpress在nginx下只能显示一个主题的解决方法
date: 2013-05-06 02:28:50
updated: 2013-05-06 02:28:50
categories:
  - Notes
tags:
  - nginx
  - php
  - wordpress
---

原文地址：http://zkeyword.com/post/wordpress_nginx_scandir/

最近买了一个vps，装了lnmp一键安装包，架了一个博客站，本来用的挺爽的，某天突然想给那个博客站换一个主题。
呀，在后台只显示一个主题其他主题都不知道跑哪去了，不管安装什么主题都只显示一个，安装同一个提示安装的目录有存在，这可真够纳闷的，后来在wordpress论坛上找到了答案：原来是php.ini禁止了scandir函数。

翻看php手册，`scandir()` 函数是这样被定义的：  
`“scandir() 函数返回一个数组，其中包含指定路径中的文件和目录”`  
wordpress可能基于这个函数去开发的，所以就只显示了一个主题。

由于我装的是lnmp0.9的安装包，其中禁用了部分危险函数：  
`“passthru, exec, system, chroot, scandir, chgrp, chown, shell_exec, proc_open, proc_get_status, ini_alter, ini_alter, ini_restore, dl, pfsockopen”`  
而`scandir`函数也在此列，所以这样问题的解决方法只能是将`scandir`从禁用函数剔除就可以了。

我们可以通过登录到winscp或是putty来修改/usr/local/php/etc下的php.ini文件，然后重启一下php进程  
`/etc/init.d/php-fpm restart`
