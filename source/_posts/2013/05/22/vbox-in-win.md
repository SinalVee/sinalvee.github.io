title: win8下无法安装VirtualBox弹出系统找不到指定的路径
date: 2013-05-22 13:02:18
updated: 2013-05-22 13:02:18
categories:
  - Notes
tags:
  - virtualbox
  - win8
---

Windows 7/8用户在安装VirtualBox时弹出了`Installation failed!Error:系统找不到指定的路径`的错误提示，导致VirtualBox安装失败。

原因：主要是因为驱动的打包程序解压过程出错，应该是无法在中文用户名的系统配置中找到解压路径造成的。  
解决办法：  
1、单击开始菜单或者WIN+R，在搜索框输入`CMD`命令打开命令提示符窗口。  
2、按住Shift键后右键单击VirtualBox文件选择“复制为路径”命令，接着，在命令提示符窗口右键单击选择“粘贴”命令。  
3、在命令后添加`-extract -path c:vbox`，回车即可将安装文件解压到“C:vbox”文件夹了。注：-extract是解压缩的意思。  
4、找到c:vbox，打开该文件夹发现有三个文件，有32位和64位两种安装程序.msi文件，按照需要安装即可。注：x86为32位系统的，X64位64位系统的，双击即可安装。  
这样，安装VirtualBox，出现系统找不到指定的路径的故障就可以解决了，就能正确安装VirtualBox了。

参考文章：http://www.wzlu.com/article/3/2011/2011062142998.html
