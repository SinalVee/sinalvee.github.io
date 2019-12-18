title: ubuntu安装nasm
date: 2013-05-15 13:15:48
updated: 2013-05-15 13:15:48
categories:
  - Linux
  - Ubuntu
tags:
  - linux
  - ubuntu
---

## 1、在Ubuntu上安装nasm方法

首先在网站http://www.nasm.us/pub/nasm/releasebuilds/2.10.07/下面去下载2.10.07.tar.gz这个版本（一般在ubuntu上面都是使用这个压缩形式的）。  
如果要下其他版本的nasm可以通过http://www.nasm.us/来进行选择进行下载。

## 2、安装方法：使用如下的命令：

解压：`tar zxvf nasm-2.10.07.tar.gz`  
进入刚解压的目录  
然后执行命令：
```
./configure
make
sudo make install
```

通过以上的步骤nasm就在ubuntu上安装好了。
也可以通过使用命令：`nasm -version`来查看是否安装成功。
如果出现了nasm的版本信息则说明安装成功，否则还需进一步安装。
