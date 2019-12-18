title: scanf，printf输入输出float和double型数据
date: 2012-10-29 22:15:00
updated: 2012-10-29 22:15:00
categories:
  - Notes
tags:
---

**1.printf的%f说明符既可以输出float型又可以输出double型。**

根据“默认参数提升”规则（在printf这样的函数的可变参数列表中，不论作用域内有没有原型，都适用这一规则）float型会被提升为double型。因此printf()只会看到双精度数。

**2.scanf对于float类型必须用%f，double必须用%lf。**

对于scanf，情况就完全不同了，它接受指针，这里没有类似的类型提升。（通过指针）向float存储和向double存储大不一样，因此，scanf区别%f和%lf。

**3.事实上，printf中没有定义%lf，但是很多系统可能会接受它。要确保可移植性，就要坚持使用%f。**

要想保留小数位数，请使用：  
`printf("%.8f", a);`保留8位小数
