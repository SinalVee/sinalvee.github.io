title: CentOS7安装SQL Developer读条后闪退解决方案
date: 2014-12-26 10:30:39
updated: 2014-12-26 10:30:39
categories:
  - Linux
  - CentOS
tags:
  - linux
  - centos
---

问题描述：

在CentOS7下安装了sqldeveloper,启动时读条后闪退

sudo执行正常启动

错误信息：
```
[xxx@xxx sqldeveloper]$ ./sqldeveloper.sh

 Oracle SQL Developer
 Copyright (c) 1997, 2014, Oracle and/or its affiliates. All rights reserved.

 LOAD TIME : 525#
# A fatal error has been detected by the Java Runtime Environment:
#
#  SIGSEGV (0xb) at pc=0x00007f94149ab140, pid=19381, tid=140274573408000
#
# JRE version: Java(TM) SE Runtime Environment (7.0_71-b14) (build 1.7.0_71-b14)
# Java VM: Java HotSpot(TM) 64-Bit Server VM (24.71-b01 mixed mode linux-amd64 compressed oops)
# Problematic frame:
# C  0x00007f94149ab140
#
# Failed to write core dump. Core dumps have been disabled. To enable core dumping, try "ulimit -c unlimited" before starting Java again
#
# An error report file with more information is saved as:
# /tmp/hs_err_pid19381.log
#
# If you would like to submit a bug report, please visit:
#   http://bugreport.sun.com/bugreport/crash.jsp
#
/opt/sqldeveloper/sqldeveloper/bin/../../ide/bin/launcher.sh: 行 1193: 19381 已放弃               (吐核)${JAVA} "${APP_VM_OPTS[@]}" ${APP_ENV_VARS} -classpath ${APP_CLASSPATH} ${APP_MAIN_CLASS} "${APP_APP_OPTS[@]}"
```

解决方案：

进入`{sqldeveloper_home}/sqldeveloper/bin/`目录，编辑该目录下的sqldeveloper文件，添加如下两行：
```
unset GNOME_DESKTOP_SESSION_ID
unset DBUS_SESSION_BUS_ADDRESS
```
再次运行./sqldeveloper.sh，问题解决，成功运行。
