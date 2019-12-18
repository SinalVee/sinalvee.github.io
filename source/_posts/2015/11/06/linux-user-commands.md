title: Linux用户相关操作
date: 2015-11-06 18:21:22
updated: 2015-11-06 18:21:22
categories:
  - Linux
tags:
  - linux
---


## 添加用户

`useradd 选项 用户名`
```
-c comment 指定一段注释性描述。
-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
-g 用户组 指定用户所属的用户组。
-G 用户组，用户组 指定用户所属的附加组。
-s Shell文件 指定用户的登录Shell。
-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。
```

## 删除用户

`userdel 选项 用户名`
```
-f, --force    force removal of files, even if not owned by user
-h, --help     display this help message and exit
-r, --remove   remove home directory and mail spool
```

## 修改用户

`usermod 选项 用户名`
选项与useradd基本一致  

## 用户密码管理

`passwd 选项 用户名`
```
-l 锁定密码，即禁用账号。
-u 密码解锁。
-d 使账号无密码。
-f 强迫用户下次登录时修改密码。
如果不使用用户名，则修改当前用户的密码
```
