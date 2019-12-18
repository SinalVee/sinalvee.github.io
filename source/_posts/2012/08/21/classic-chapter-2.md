title: 《算法竞赛入门经典》习题——Chapter 2
date: 2012-08-21 16:28:00
updated: 2012-08-21 16:28:00
categories:
  - Algorithm
tags:
  - 算法竞赛入门经典
---

## 习题2-1 位数（digit）

题目：输入一个不超过10^9的正整数，输出它的位数。例如12735的位数是5。请不要使用任何数学函数，只用四则运算和循环语句实现。  
分析：考察循环的使用。

源码：

```c
// 习题2-1 位数（digit）

#include <stdio.h>
int main(void)
{
	int n, digit = 0;
	scanf("%d", &n);
	while(n)
	{
		digit++;
		n /= 10;
	}
	printf("%dn", digit);
	return 0;
}
```

## 习题2-2 水仙花数(daffodil)

题目：输出100 ~ 999中的所有水仙花数，若3位数ABC满足ABC = A^3 + B^3 + C^3(*书中题目有误，均写成了平方*)，则称其为水仙花数。例如153 = 1^3 + 5^3 + 3^3，所以153是水仙花数。  
分析：循环。

源码：

```c
// 习题2-2 水仙花数（daffodil）

#include <stdio.h>
int main(void)
{
	int i, a, b, c;
	for(i = 100; i <= 999; i++)
	{
		a = i/100;
		b = i/10%10;
		c = i%10;
		if(i == a*a*a + b*b*b + c*c*c)
			printf("%d is a daffodil number.n", i);
	}
	return 0;
}
```

## 习题2-3 韩信点兵(hanxin)

题目：韩信才智过人，从不直接清点自己军队的人数，只要让士兵先后以三人一排、五人一排、七人一排地变换队形，而他每次都只是掠一眼队伍的排位就知道人数了。输入3个非负整数a，b，c，表示每种队形排尾的人数（a<3，b<5，c<7），输出总人数的最小值（或报告无解）。已知总人数不小于10，不超过100。  
```
样例输入： 2 1 6
样例输出： 41
样例输入： 2 1 4
样例输出： No Answer
```
分析：利用循环分别对3、5、7取余后与a,b,c比较，若全相等则输出。若没有则报告无解。

源码：

```c
// 习题2-3 韩信点兵（hanxin）

#include <stdio.h>
int main(void)
{
	int i, a, b, c;
	scanf("%d%d%d", &a, &b, &c);
	for(i = 10; i <= 100; i++)
	{
		if(i%3 == a && i%5 ==b && i%7 ==c)
		{
			printf("%dn", i);
			break;
		}
	}
	if(i == 101)
		printf("No answer");
	return 0;
}
```

## 习题2-4 倒三角形(triangle)

题目：输入正整数n<=20，输出一个n层的倒三角形。例如n=5时输出如下：
```
#########
 #######
  #####
   ###
    #
```
分析：依旧是循环的用法，注意空格和#之间的关系。

源码：

```c
// 习题2-4 倒三角形（triangle）

#include <stdio.h>
int main(void)
{
	int i, j, k, n;
	scanf("%d", &n);
	for(i = 0; i < n; i++)
	{
		k = i;
		for(j = 0; j < k; j++)
			printf(" ");
		for(j = 0; j < 2*n-2*i-1; j++)
			printf("#");
        printf("n");
	}
	return 0;
}
```

## 习题2-5 统计(stat)

题目：输入一个正整数n，然后读取n个正整数a1, a2, a3...,an，最后再读取一个正整数m。统计数列中多少个正整数的值小于m。  
分析：对freopen和fopen的使用。

源码略。

## 习题2-6 调和级数（harmony）

题目：输入正整数n，输出H(n) = 1 + 1/2 + 1/3 + …… + 1/n的值，保留3位小数。例如n=3时答案为1.833.  
分析：简单的循环累加，注意输出格式。

源码：

```c
// 习题2-6 调和级数（harmory）

#include <stdio.h>
int main(void)
{
	int n, i;
	double sum = 0;
	scanf("%d", &n);
	for(i = 1; i <= n; i++)
		sum += 1.0/i;
	printf("%.3lfn", sum);
	return 0;
}
```

## 习题2-7 近似计算（approximation）

题目：计算派/4 = 1 - 1/3 + 1/5 - 1/7 +……，直到最后一项小于10^-6.
分析：循环。

源码：

```c
// 习题2-7 近似计算（opproximation）

#include <stdio.h>
#include <math.h>
int main(void)
{
	int i;
	double t = 1, pi = 0, x = 1;
	for(i = 1; t >= pow(10, -6); i++)
	{
		t = 1.0/(2*i-1);
		pi += t*x;
		x *= -1;
	}
	printf("%lfn", 4*pi);
	return 0;
}
```

## 习题2-8 子序列的和（subsequence）

题目：输入两个正整数n<m<10^6，输出1/n^2 + 1/(n+1)^2 + …… + 1/m^2，保留5位小数。例如n=2,m=4时答案是0.42361;n=65536,m=655360时答案为0.00001。注意：本题有陷阱。  
分析：还是for循环累加。本题陷阱在于n比较大时，n*n会溢出，所以 1/n^2 应该用 1/n/n 而不是 1/(n*n)。

源码：

```c
// 习题2-8 子序列的和（subsequence）

#include <stdio.h>
int main(void)
{
  int n, m, i;
	double sum = 0;
  scanf("%d%d", &n, &m);
	for(i = n; i <= m; i++)
		sum += 1.0/i/i;
	printf("%.5lfn", sum);
	return 0;
}

/*
  本题陷阱在于1/(n)^2 + ...
  用1/(n*n)会溢出
  改为1/n/n就好
*/
```

## 习题2-9 分数化小数（decimal）

题目：输入正整数a,b,c，输出a/b的小数形式，精确到小数点后c位。a,b <= 10^6，c <= 100。例如a=1,b=6,c=4时应输出0.1667.  
分析：考察格式化输出，printf("%*.*lf", x, y, z); 中两个*可用后边的变量表示。

源码：

```c
// 习题2-9 分数化小数（decimal）

#include <stdio.h>
int main(void)
{
	int a, b, c;
	double x;
	scanf("%d%d%d", &a, &b, &c);
	x = 1.0*a/b;
	printf("%.*lfn", c, x);    
	//printf("%*.*lf", x, y, z) 第一个*对应x，第二个*对应y，lf对应z
	return 0;
}
```

## 习题2-10 排列（permutition）

题目：用1，2，3……9组成3个三位数abc,def和ghi，每个数字恰好使用一次，要求abc:def:ghi = 1:2:3。输出所有解。提示：不必太动脑筋。  
分析：上学期院里组织比赛的一道题，利用数组，a[1]~a[9]赋值为0，令a[出现的数字] = 1，若a[1] + a[2] + …… +a[9] == 9，则全部数字都出现。

源码：

```c
// 习题2-10 排列（permutation）

#include <stdio.h>
int main(void)
{
	int x, y, z, a[10] = {0};
	for(x = 100; x < 333; x++)
	{
		y = 2*x;
		z = 3*x;
		//令a[出现的数字] = 1
		a[x/100] = a[x/10%10] = a[x%10] = 1;
		a[y/100] = a[y/10%10] = a[y%10] = 1;
		a[z/100] = a[z/10%10] = a[z%10] = 1;
		int i, s = 0;
		for(i = 1; i < 10; i++)
			s += a[i];
		if(s == 9)
			printf("%dt%dt%dn", x, y, z);
		for(i = 1; i < 10; i++)	//重新赋值为0
			a[i] = 0;
	}
	return 0;
}
```
