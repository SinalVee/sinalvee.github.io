title: 《算法竞赛入门经典》习题——Chapter 
date: 2012-08-10 19:12:001
updated: 2012-08-10 19:12:00
categories:
  - Algorithm
tags:
  - 算法竞赛入门经典
---

## 习题1-1 平均数（average）

题目：输入3个整数，输出他们的平均值，保留3位小数。

分析：主要考察的是C语言打印函数(printf)的输出格式。

C中格式字符串的一般形式为： %[标志][输出最小宽度][.精度][长度]类型， 其中方括号[]中的项为可选项。各项的意义介绍如下：

|格式字符|格式字符意义|
|:----:|----|
|a|浮点数、十六进制数字和p-计数法(C99)|
|A|浮点数、十六进制数字和p-计数法(C99)|
|c|输出单个字符|
|d|以十进制形式输出带符号整数(正数不输出符号)|
|e|以指数形式输出单、双精度实数|
|E|以指数形式输出单、双精度实数|
|f|小数形式输出单、双精度实数|
|g|以%f%e中较短的输出宽度输出单、双精度实数,%e格式在指数小于-4或者大于等于精度时使用|
|G|以%f%e中较短的输出宽度输出单、双精度实数,%e格式在指数小于-4或者大于等于精度时使用|
|i|有符号十进制整数(与%d相同)|
|o|以八进制形式输出无符号整数(不输出前缀O)|
|p|指针|
|s|输出字符串|
|x|以十六进制形式输出无符号整数(不输出前缀OX)|
|X|以十六进制形式输出无符号整数(不输出前缀OX)|
|u|以十进制形式输出无符号整数|

源码：
```c
// 习题1-1 平均数（average）

#include <stdio.h>
int main(void)
{
	int a, b, c;
	double aver;
	scanf("%d%d%d", &a, &b, &c);
	aver = (a+b+c) / 3.0;
	printf("%.3lfn", aver);
	return 0;
}
```

## 习题1-2 温度（temperature）

题目：输入华氏温度f，输出对应的摄氏温度c，保留3位小数。提示：`c = 5(f-32)/9`。  
分析：同上题，主要考察的是C语言打印函数(printf)的输出格式。  

源码：
```c
// 习题1-2 温度（temperature）

#include <stdio.h>
int main(void)
{
	float f, c;
	scanf("%f", &f);
	c = 5*(f-32)/9;
	printf("%.3fn", c);
	return 0;
}
```

## 习题1-3 连续和（sum）

题目：输入正整数n，输出1+2+3+……+n的值。提示：目标是解决问题，而不是练习编程。  
分析：1+2+3=......+n = (1+n)*n/2  

源码：
```c
// 习题1-3 连续和（sum）

#include <stdio.h>
int main(void)
{
	int i, n, sum = 0;
	scanf("%d", &n);
	sum = (1+n)*n/2;
	printf("%dn", sum);
	return 0;
}
```

## 习题1-4 正弦和余弦（sincos）

题目：输入正整数n（n<360），输出n度的正弦、余弦函数值。提示：使用数学函数。  
分析；考察的是数学函数的使用，注意角度和弧度的换算关系：180度 = pi * 1弧度。

源码：

```c
// 习题1-4 正弦和余弦（sin&cos）

#include <stdio.h>
#include <math.h>
#define PI 3.1415926
int main(void)
{
	int n;
	double x;
	scanf("%d", &n);
	x = n*(PI/180.0);
	printf("%lfn", sin(x));
	printf("%lfn", cos(x));
	return 0;
}
```

## 习题1-5 距离（distance）

题目：输入4个浮点数x1, y1, x2, y2，输出平面坐标系点(x1, y1)到点(x2, y2)的距离。  
分析：  
1、平面直角坐标系中，两点间的距离公式为：根号下（|X1-X2|的平方+|Y1-Y2|的平方）。
2、平方函数sqrt属于math.h文件中。

源码：

```c
// 习题1-5 距离（distance）

#include <stdio.h>
#include <math.h>
int main(void)
{
	float x1, x2, y1, y2, dis;
	scanf("%f%f%f%f", &x1, &y1, &x2, &y2);
	dis = sqrt((x1-x2)*(x1-x2) + (y1-y2)*(y1-y2));
	printf("%fn", dis);
	return 0;
}
```

## 习题1-6 偶数（odd）

题目：输入一个整数，判断它是否为偶数。如果是，则输出"yes"，否则输出"no"。提示：可以用多种方法判断。  
分析：偶数的判断方法。  

源码：

```c
// 习题1-6 偶数（odd）

#include <stdio.h>
int main(void)
{
	int n;
	scanf("%d", &n);
	if(n%2 == 0)
		printf("yesn");
	else
		printf("non");
	return 0;
}
```

## 习题1-7 打折（discount）

题目：一件衣服95元，若消费满300元，可打八五折。输入购买衣服件数，输出需要支付的金额（单位：元），保留两位小数。  
分析：if 的运用，easy。  

源码：

```c
// 习题1-7 打折（discount）

#include <stdio.h>
int main(void)
{
	int n, money;
	scanf("%d", &n);
	money = 95*n;
	if(money >= 300)
		money *= 0.85;
	printf("%dn", money);
}
```

## 习题1-8 绝对值（abs）

题目：输入一个浮点数，输出它的绝对值，保留两位小数。  
分析：if 运用，printf 输出格式。  

源码：

```c
// 习题1-8 绝对值（abs）

#include <stdio.h>
int main(void)
{
	float x;
	scanf("%f", &x);
	if(x>0)
		printf("%fn", x);
	else
		printf("%.2fn", -x);
	return 0;
}
```

## 习题1-9 三角形（triangle）

题目：输入三角形三边长度值（均为正整数），判断它是否能为直角三角形的三个边长。如果可以，则输出“yes”，如果不能，则输出“no”。如果根本无法构成三角形，则输出“not a triangle”。  
分析：if 嵌套使用。  

源码：

```c
// 习题1-9 三角形（triangle）

#include <stdio.h>
int main(void)
{
	int a, b, c;
	scanf("%d%d%d", &a, &b, &c);
	if(a+b>c && a+c>b && b+c>a)
    {
        if(a*a + b*b == c*c || a*a + c*c == b*b || b*b + c*c == a*a)
            printf("yesn");
        else
            printf("non");
    }
	else
		printf("not a trianglen");
	return 0;
}
```

## 习题1-10 年份（year）

题目：输入年份，判断是否为闰年。如果是，则输出“yes”，否则输出“no”。提示：简单地判断除以4的余数是不够的。  
分析：if 嵌套使用，闰年的判断方法。  

源码：

```c
// 习题1-10 年份（year）

#include <stdio.h>
int main(void)
{
	int n;
	scanf("%d", &n);
	if(n%4 == 0)
	{
		if(n%100 == 0 && n%400 != 0)
			printf("non");
		else
			printf("yesn");
	}
	else
		printf("non");
	return 0;
}
```
