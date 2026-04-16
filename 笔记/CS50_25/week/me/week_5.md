

# 数据结构

> 在c中我们可以使用**struct**，**\***,**.**来创建任何数据结构

## 队列（queue）

先进先出



## 栈（stack）

后进先出



## 链表(node)

```c
typedef struct node{
    int  number;
    node *next;
}node;
```

### 反向链表

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
    int number;
    struct node *next;
} node;

int main()
{
    node *list = NULL;

    int n; // 你需要输入几个数字
    scanf("%d", &n);

    for (int i = 0; i < n; i++)
    {
        node *temp = malloc(sizeof(node)); // 临时的
        if (temp == NULL)
        {
            return 1;
        }
        scanf("%d", &temp->number);
        temp->next = list;
        list = temp;
    }

    // 释放链表逻辑
    node *curr = list;
    while (curr != NULL)
    {
        node *next = curr->next; // 先存下一个节点的地址，避免free后找不到
        free(curr);
        curr = next;
    }
    list = NULL; // 释放后置空，避免野指针
    return 0;
}
```











## realloc

为你重新分配一些内存

如

```c
int *list = malloc(3 * sizeof(int));
//但是我们现在要存4个数字
int * temp = realloc(list,4 * sizeof(int));
list = temp;
```



## 树（tree）

### 二叉搜索树

```c
typedef struct node{
    int number;
    struct node *left;
    struct node *right;
} node;
```





### 字典

分桶思想（大变小）

哈希表





























