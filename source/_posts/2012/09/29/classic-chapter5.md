title: 《算法竞赛入门经典》 第五章 基础题目选解
date: 2012-09-29 17:29:00
updated: 2012-09-29 17:29:00
categories:
  - Algorithm
tags:
  - 算法竞赛入门经典
---

## 5.1 字符串

### 5.1.1 WERTYU

```c
// 5.1.1 WERTYU

#include <stdio.h>
char *s = "1234567890-=QWERTYUIOP[]\ASDFGHJKL;'ZXCVBNM,./";
int main(void)
{
	int i, c;
	while((c = getchar()) != EOF)
	{
		for(i = 1; s[i] && s[i] != c; i++)
			;
		if(s[i])
			putchar(s[i-1]);
		else
			putchar(c);
	}
	return 0;
}
```

### 5.1.2 TeX括号

```c
// 5.1.2 TeX括号

#include <stdio.h>
int main(void)
{
	int c, q = 1;
	while((c = getchar()) != EOF)
	{
		if(c == '"')
		{
			printf("%s", q ? "" : "''");
			q = !q;
		}
		else
			printf("%c", c);
	}
	return 0;
}
```

### 5.1.3 周期串
```c
// 5.1.3 周期串

#include <stdio.h>
#include <string.h>
int main(void)
{
	char word[100];
	scanf("%s", word);
	int len = strlen(word);
	for(int i = 1; i <= len; i++)
		if(len % i == 0)
		{
			int ok = 1;
			for(int j = 1; j < len; j++)
				if(word[j] != word[j%i])
				{
					ok = 0;
					break;
				}
			if(ok)
			{
				printf("%dn", i);
				break;
			}
		}
}
```

## 5.2 高精度运算

### 5.2.1 小学生算术
```c
// 5.2.1 小学生算术

#include <stdio.h>
int main(void)
{
	int a, b;
	while(scanf("%d%d", &a, &b) == 2)
	{
		if(!a && !b)
			return 0;
		int c = 0, ans = 0;
		for(int i = 9; i >= 0; i--)
		{
			c = (a%10 + b%10) > 9 ? 1 : 0;
			ans += c;
			a /= 10;
			b /= 10;
		}
		printf("%dn", ans);
	}
	return 0;
}
```
### 5.2.2 阶乘的精确值
```c
// 5.2.2 阶乘的精确值

#include <stdio.h>
#include <string.h>
#define MAXN 3000
int f[MAXN];
/*
  const int maxn = 3000;
  int f[maxn];

  这样写会报错
  .c|6|error: variably modified 'f' at file scope|
  因为const在C中定义的时可读变量，maxn依旧是变量，不能用变量声明数组长度
*/
int main(void)
{
	int i, j, n;
	scanf("%d", &n);
	memset(f, 0, sizeof(f));
	f[0] = 1;
	for(i = 2; i <= n; i++)
	{	//乘以i
		int c = 0;
		for(j = 0; j < MAXN; j++)
		{
			int s = f[j] * i + c;
			f[j] = s % 10;
			c = s/10;
		}
	}
	for(j = MAXN-1; j >= 0; j--)
		if(f[j])
			break;
	for(i = j; i >= 0; i--)
		printf("%d", f[i]);
	printf("n");
	return 0;
}
```
