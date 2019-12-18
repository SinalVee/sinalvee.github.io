title: 链表的创建、遍历、排序、插入、删除等算法演示
date: 2013-01-29 15:15:58
updated: 2013-01-29 15:15:58
categories:
  - Algorithm
  - Data Structure
tags:
---

实现代码：
```c
//链表算法演示

#include <stdio.h>
#include <malloc.h>
#include <stdlib.h> //exit()函数

typedef struct Node
{
    int data;   //数据域
    struct Node * pNext;    //指针域
}NODE, * PNODE; //NODE等价于struct Node    PNODE等价于struce Node *

PNODE creat_list(void); //创建
void traverse_list(PNODE pHead);    //遍历
int is_empty(PNODE pHead);  //判断是否为空
int length_list(PNODE pHead);   //求链表长度
int insert_list(PNODE pHead, int pos, int val); //插入
int delete_list(PNODE pHead, int pos, int * pVal);   //删除
void sort_list(PNODE pHead);    //排序

int main(void)
{
    PNODE pHead = NULL;
    int val;

    pHead = creat_list();
    traverse_list(pHead);

    if(is_empty(pHead))
        printf("链表为空n");
    else
        printf("链表不空n");

    int len = length_list(pHead);
    printf("链表长度为：%dn", len);

    printf("链表排序：n");
    sort_list(pHead);
    traverse_list(pHead);

    printf("插入：n");
    insert_list(pHead, 3, 55);
    traverse_list(pHead);

    printf("删除：n");
    if(delete_list(pHead, 3, &amp;val))
        printf("删除成功，删除的元素为：%dn", val);
    else
        printf("删除失败。");
    traverse_list(pHead);

    return 0;
}

PNODE creat_list(void)
{
    int len;    //用来存放有效节点的个数
    int i;
    int val;    //用来临时存放用户输入的节点的值

    PNODE pHead = (PNODE)malloc(sizeof(NODE));
    if(pHead == NULL)
    {
        printf("内存分配失败，程序终止。n");
        exit(-1);
    }
    PNODE pTail = pHead;
    pTail->pNext = NULL;

    printf("请输入您需要生成的链表节点的个数：len = ");
    scanf("%d", &amp;len);

    for(i = 0; i < len; i++)
    {
        printf("请输入第%d个节点的值：", i+1);
        scanf("%d", &amp;val);

        PNODE pNew = (PNODE)malloc(sizeof(NODE));
        if(pNew == NULL)
        {
            printf("内存分配失败，程序终止。n");
            exit(-1);
        }
        pNew->data = val;
        pTail->pNext = pNew;
        pTail = pNew;
        pNew->pNext = NULL;
    }

    return pHead;
}

void traverse_list(PNODE pHead)
{
    PNODE p = pHead->pNext;

    while(NULL != p)
    {
        printf("%d ", p->data);
        p = p->pNext;
    }
    printf("n");
}

int is_empty(PNODE pHead)
{
    if(pHead->pNext == NULL)
        return 1;
    else
        return 0;
}

int length_list(PNODE pHead)
{
    PNODE p = pHead->pNext;
    int len = 0;

    while(p != NULL)
    {
        len++;
        p = p->pNext;
    }

    return len;
}

void sort_list(PNODE pHead)
{
    int i, j, t, len;
    len = length_list(pHead);
    PNODE p, q;
    for(i = 0, p = pHead->pNext; i < len-1; i++, p = p->pNext)
    {
        for(j = i+1, q = p->pNext; j < len; j++, q = q->pNext)
            if(p->data > q->data)
            {
                t = p->data;
                p->data = q->data;
                q->data = t;
            }
    }
}

int insert_list(PNODE pHead, int pos, int val)
{
    int i = 0;
    PNODE p = pHead;

    while(NULL != p &amp;&amp; i < pos-1)
    {
        p = p->pNext;
        i++;
    }
    if(i > pos-1 || p == NULL)
        return 0;
    PNODE pNew = (PNODE)malloc(sizeof(NODE));
    if(NULL == pNew)
    {
        printf("动态内存分配失败。n");
        exit(-1);
    }
    pNew->data = val;
    PNODE q = p->pNext;
    p->pNext = pNew;
    pNew->pNext = q;

    return 1;
}

int delete_list(PNODE pHead, int pos, int * pVal)
{
    int i = 0;
    PNODE p = pHead;

    while(NULL != p->pNext &amp;&amp; i < pos-1)
    {
        p = p->pNext;
        i++;
    }
    if(i > pos-1 || p->pNext == NULL)
        return 0;

    PNODE q = p->pNext;
    *pVal = q->data;
    p->pNext = p->pNext->pNext;
    free(q);
    q = NULL;

    return 1;
}
```
