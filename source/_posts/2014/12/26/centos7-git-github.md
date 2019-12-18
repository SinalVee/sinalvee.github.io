title: CentOS7编译安装git并配置使用github
date: 2014-12-26 14:34:33
updated: 2014-12-26 14:34:33
categories:
  - Linux
  - CentOS
tags:
  - linux
  - centos
  - git
  - github
---

## 一、编译安装git

CentOS7默认安装的git版本为1.8.3.1，而git官方的版本已经到2.2.0了，所以选择下载源码编译安装git

### 1.clone github上的git源码

`$ git clone https://github.com/git/git.git`

###  2.进入git目录进行配置及安装

```
$ cd git
$ autoconf
$ ./configure
$ make
$ make install
```
期间可能会提示缺少文件，是因为git的依赖包没有安装，缺什么装什么就好，或者直接  
`yum -y install curl curl-devel zlib-devel openssl-deve perl perl-devel cpio expat-devel gettext-deve`

### 3.查看版本

```
$ git --version
git version 1.8.3.1
```
竟然还是原来的版本而不是最新安装的2.2.0，那只能先把旧版本卸载了
```
$ rpm -q git
git-1.8.3.1-4.el7.x86_64
$ sudo rpm -e --nodeps git-1.8.3.1-4.el7.x86_64
```
然后执行
`sudo ln -s /usr/local/bin/* /usr/bin/`
再次查看git版本
```
$ git --version
git version 2.2.0.GIT
```
git版本已经是我们新安装的2.2.0了

### 4.设置自己的用户名和邮箱

命令行输入：（引号加不加无所谓）
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

### 5.在本地创建仓库并初始化

```
$ mkdir test
$ cd test
$ git init
```

初始化空的 Git 版本库于 `/home/username/github/test/.git/`  
至此git安装完成

##  二、使用github

1.注册github账号

我这里创建了一个账号：`SinalVee`，并创建了一个`Test`仓库用于测试

2.用ssh-kengen生成公钥

`$ ssh-keygen -t rsa -C xxx@xxx.com`
xxx@xxx.com为你的邮箱地址

设定存放目录和密码后在github设置中找到SSH Keys，选择Add SSH Key，设置名称并把`*/.ssh/id_rsa.pub`的文件内容粘贴进去

3.测试是否成功

`$ ssh -T git@github.com`
出现下面的信息并提示验证私钥则成功，输入密码解锁私钥
```
The authenticity of host 'github.com (192.30.252.130)' can't be established.
RSA key fingerprint is 16:27:ac:axxxxxxxxxxxxxxxxx:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.252.130' (RSA) to the list of known hosts.
Hi SinalVee! You've successfully authenticated, but GitHub does not provide shell access.
```
在本地任意目录新建同步文件夹
```
$ mkdir github
$ cd github
$ git clone git@github.com:SinalVee/Test（此处SinalVee为用户名，Test是你在github上创建的仓库）
```
```
正克隆到 'Test'...
Warning: Permanently added the RSA host key for IP address '192.30.252.131' to the list of known hosts.
remote: Counting objects: 6, done.
remote: Total 6 (delta 0), reused 0 (delta 0)
接收对象中: 100% (6/6), 完成.
检查连接... 完成。
```
因为我很久之前就创建了这个Test仓库并测试过，所以这里共有6个objects（一共两个文件，之所以这里是6是因为Git跟踪并管理的是修改，而非文件）。

接下来测试以下上传
```
$ cd Test/
$ ls
README.md  test.txt
$ touch hello.txt
$ vim hello.txt
你好，这是一个测试文件。
$ ls
hello.txt  README.md  test.txt
$ git add hello.txt
$ git commit -m "add file hello.txt"
[master 0e4004c] add file hello.txt
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
$ git push
对象计数中: 3, 完成.
Delta compression using up to 4 threads.
压缩对象中: 100% (2/2), 完成.
写入对象中: 100% (3/3), 354 bytes | 0 bytes/s, 完成.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:sinalvee/Test
   984cabc..0e4004c  master -&gt; master
```
在github的网页上刷新一下仓库，发现hello.txt已经上传成功了，至此我们的github已经配置成功，并可以使用了。

github的使用这里不多赘述了。
