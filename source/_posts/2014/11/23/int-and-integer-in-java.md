title: Java中int和Integer的区别及如何相互转换
date: 2014-11-23 16:00:27
updated: 2014-11-23 16:00:27
categories:
  - Java
tags:
  - java
---

两周前参加了一场比试，笔试中有`int`和`Integer`相互转换的题，自己答的不是很有把握，不过没有及时查阅资料，后来就忘记了。
今天看笔试题的时候看到`int`和`Integer`的区别突然想到之前的那道题，查了下资料，现在把它记录下来。

## Java中int和Integer的区别

int是Java提供的8种原始数据类型之一。Java为每个原始数据类型提供了封装类，Integer是Java为int提供的封装类。

int的默认值为0，而Integer的默认值为null，即Integer可以区分出未赋值和值为0的区别，int则无法表达出未赋值的情况。例如，要想表达出没有参加考试和考试成绩为0的区别，则只能用Integer。

另外，Integer提供了多个与整数相关的操作方法，例如，将一个字符串转换成整数`Integer.parseInt(String str)`，Integer中还定义了表示整数的最大值和最小值的常量。

## int和Integer的互相转换

### 1.int转换为Integer
```java
1)
int a = 3;
Integer A = new Integer(a);
2)
int a = 3;
Integer A = Integer.valueOf(a);
```
### 2.Integer转换为int
```java
Integer A = new Integer(5);
int a = A.valueOf();
```
