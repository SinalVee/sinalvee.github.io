title: ubuntu英文环境下使用中文输入法
date: 2014-03-13 18:52:34
updated: 2014-03-13 18:52:34
categories:
  - Linux
  - Ubuntu
tags:
  - linux
  - ubuntu
---

为了和小伙伴们一起dota2，下载了steam for linux，无奈中文环境下无法在steam上敲字，于是乎换到英文环境，字是能敲了，但是另外一个问题出现了，按ctrl+space无法调出中文输入法了

一通google&baidu之后，找到了解决方案

1.依次进入System Settings > Language Support > Install / Remove Languages…
弹出窗口中选择 Chinese 并且 Apply Changes
（ps:我这里之前是中文环境，所以chinese已经安装了，所以这一步没有进行）

2.Keyboard input method system 选择 ibus

3.Dash home中搜索ibus Keyboard Input Method,打开
在IBus Preferences > Input Method 中选择 Chinese - Pinyin 就解决了

------------------------
中文输入的问题解决，But....
steam只能打英文 = =  ctrl+space无法调出中文输入法……
