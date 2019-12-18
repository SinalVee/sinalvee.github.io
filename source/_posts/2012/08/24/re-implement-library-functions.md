title: 《算法竞赛入门经典》——重新实现库函数
date: 2012-08-24 10:55:00
updated: 2012-08-24 10:55:00
categories:
  - Algorithm
tags:
  - 算法竞赛入门经典
---

在学习字符串时，重新实现一些库函数的功能是很有益的。

## 练习1：
只用getchar函数读入一个整数。假设它占据单独的一行，读到行末为止，包括换行符。输入保证读入的整数可以保存在int中。

```c
// 3.4.4-1 只用getchar函数读入一个整数。

#include <stdio.h>
int main(void)
{
    int a[100], i = 0, num = 0;
    while((a[i] = getchar()) && a[i] != 'n')
    {
        num = num*10 + a[i] - '0';
        i++;
    }
    printf("%dn", num);
    return 0;
}
```

## 练习2：
只用fgets函数读入一个整数。假设它占据单独的一行，读到行末为止，包括换行符。输入保证读入的整数可以保存在int中。

```c
// 3.4.4-2 只用fgets函数读入一个整数。

#include <stdio.h>
#include <string.h>
int main(void)
{
	int i, num = 0;
	char s[100];
	fgets(s, sizeof(s), stdin);
	for(i = 0; i < strlen(s)-1; i++)
		num = num*10 + s[i] - '0';
	printf("%dn", num);
	return 0;
}
```

## 练习3：
只用getchar实现fgets的功能，即用每次一个字符的方式读取整行。

```c
// 3.4.4-3 只用getchar实现fgets的功能。

#include <stdio.h>
int main(void)
{
	char s[100];
	int i = 0;
	while((s[i] = getchar()) && s[i] != '\n')
	{
		i++;
	}
	s[i] = '\0';
	printf("%sn", s);
	return 0;
}
```
练习4：实现strchr的功能，即在一个字符串中查找一个字符。

```c
// 3.4.4-4 实现strchr的功能，即在一个字符串中查找一个字符。

#include <stdio.h>
char * fun(char * s, char c);
int main(void)
{

	return 0;
}

char * fun(char * s, char c)
{
	while(*s && *s != c)
		s++;
	if(*s == c)
		return s;
	else
		return NULL;
}
```
