title: __int64 类型（VC中）与long long 型（gcc中，C99标准）
date: 2012-08-02 17:40:00
updated: 2012-08-02 17:40:00
categories:
  - Notes
tags:
---

原文地址：http://blog.csdn.net/luxuejuncarl/article/details/1568457

int64 是有符号 64 位整数数据类型，也就是 C# 中的 long 和 SQL Server 中的 bigint，范围为 -2^63 (-9,223,372,036,854,775,808) 到 2^63-1 (9,223,372,036,854,775,807)，存储空间占 8 字节。用于整数值可能超过 int 数据类型支持范围的情况。

c#中:

Int64 值类型表示值介于 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 之间的整数。

Int64 为比较此类型的实例、将实例的值转换为它的字符串表示形式以及将数字的字符串表示形式转换为此类型的实例提供了相应的方法。

警告 在 32 位 Intel 计算机上分配 64 位值不是原子操作；即该操作不是线程安全的。这意味着，如果两个人同时将一个值分配给一个静态 Int64 字段，则该字段的最终值是无法预测的。

有关格式规范代码如何控制值类型的字符串表示形式的信息，请参见格式化化概述。此类型实现接口 IComparable、IFormattable 和 IConvertible。使用 Convert 类进行转换，而不是使用此类型的 IConvertible 显式接口成员实现。

```
C语言INT64  （VC中）
__int64 是一个关键字，用_int64 来可以指定一个64位的整型变量
__int8 nSmall;      // 声明 8位 整数
__int16 nMedium;    // 声明 16位 整数
__int32 nLarge;     // 声明 32位 整数
__int64 nHuge;      // 声明 64位 整数
```
```
long   long是C99标准的C语言内置类型。需要符合C99的编译器
g++ /gcc中

long long a;
printf("%lld",a);  

mingw32 中

__int64 a;
printf("%I64d",a);  
```
