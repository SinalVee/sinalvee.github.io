title: 配置gvim使之不自动生成备份文件的方法
date: 2015-03-26 14:59:01
updated: 2015-03-26 14:59:01
categories:
  - Notes
tags:
---

默认情况下使用gvim，在保存文件时会自动产生带~号的备份文件，强迫症选手真是受不了，现给出解决方案：

1.打开gvim，选择菜单栏的编辑-启动设定
2.在“behave mswin”下方添加一行 set nobackup
3.:wq 退出并保存

以上，解决。
