title: 跨函数使用内存示例
date: 2013-01-29 12:14:30
updated: 2013-01-29 12:14:30
categories:
  - Algorithm
  - Data Structure
tags:
  - data-structure
---

跨函数使用内存需用到动态内存分配，即c语言中的malloc和c++中的new。

代码如下：
```cpp
//跨函数使用内存示例

#include
#include

struct Student
{
    int sid;
    int age;
};

struct Student * CreateStudent(void);
void ShowStudent(struct Student *);
void ChangeStudent(struct Student *);

int main(void)
{
    struct Student * ps;

    ps = CreateStudent();
    ShowStudent(ps);
    ps->sid = 77;
    ps->age = 66;
    ShowStudent(ps);
    ChangeStudent(ps);
    ShowStudent(ps);

    return 0;
}

struct Student * CreateStudent(void)
{
    struct Student * p = (struct Student *)malloc(sizeof(struct Student));
    p->sid = 99;
    p->age = 88;
    return p;
}

void ShowStudent(struct Student * pst)
{
    printf("%d %dn", pst->sid, pst->age);
}

void ChangeStudent(struct Student * pst)
{
    pst->sid = 55;
    pst->age = 44;
}
```
