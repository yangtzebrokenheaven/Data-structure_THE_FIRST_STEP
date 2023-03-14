# Data-structure_THE_FIRST_STEP
It;'s my first project and a note in github ,just for myself study

# 线性表

## 顺序表

### 1.顺序表的定义

相同数据类型的有限序列

使用sizeof关键字查看数据大小

```cpp
sizeof(int)=4B
```

**静态分配**

```cpp
#define Maxsize 10
typedef srtuct{
    ElemType data[Maxsize] //用静态的数组存放数据元素
    int length//顺序表当前长度
}SqList;//顺序表的类型定义
```

具体代码如下

```cpp
#include <iostream>
#define MaxSize 10
using namespace std;
typedef struct {
	int data[MaxSize];
	int length;
}SqList;

void InitList(SqList& L) {
	for (int i = 0; i < MaxSize; i++)
		L.data[i] = 0;
	L.length = 0;
}
int main() {
	SqList L;
	InitList(L);
	return 0;
}
```

数据元素未初始化直接打印会出现残留数据

顺序表的存储空间是静态的

#### 动态分配

```cpp
#define InitSize 10
typedef struct{
    int *data;
    int maxsize;
    int length;
}
```

C-malloc,free函数 

mallco函数会返回一片连续的存储空间的指针

```cpp
L.data = (int*)malloc(sizeof(int)*InitSize)
```

CPP-new,delete关键字

如下为代码实现

 ```cpp
#include <iostream>
#include <stdlib.h>
using namespace std;
#define InitSize 10
typedef struct {
    int* data;               //开辟空间
    int maxsize;                
    int length;
}SeqList;
void InitList(SeqList& L) {
    L.data = (int*)malloc(InitSize * sizeof(int));//申请一片存储空间，返回指针类型转换为int，指针指向为malloc开辟的内存的头地址
    L.length = 0;
    L.maxsize = InitSize;
}
void IncreaseSize(SeqList& L, int len) {//多开辟的len长度的内存
    int* p = L.data;
    L.data = (int*)malloc((InitSize+len) * sizeof(int));//*p起到中继作用，用完后free
    for (int i = 0; i < L.length; i++)//将原表赋值到新区域
    {
        L.data[i] = p[i];
    }
    L.maxsize = L.maxsize + len;
    free(p);
}
int main(){
    SeqList L;
    InitList(L);
    IncreaseSize(L,5);
    return 0;
}
 ```

**顺序表的特点**

1.随机访问，在O（1）时间内找到第i个元素

2.存储密度高，每个节点只存储数据元素

3.拓展不方便

4.删除，插入操作不方便

### 2.顺序表的插入和删除

#### 插入

ListInsert(&L,int i,int e)在L表中第i个元素插入元素e

```cpp
#include <iostream>
#include <stdlib.h>
#define InitSize 10
using namespace std;
typedef struct {
	int data[InitSize];
	int length;
}SqList;
void ListInsert(SqList& L, int i, int a) {
	for (int s = L.length; s >= i; s--) 
		L.data[s] = L.data[s - 1];
	L.data[i - 1] = a;
	L.length++;
}
```

**注意：顺序表中的数据元素是相邻的，插入不能隔空插入以及要判断原始顺序表是否为满**

使用if语句判断，修改后代码如下

```cpp
bool ListInsert(SqList& L, int i, int a) {
	if (i<1 || i>L.length + 1)
		return false;
	if (L.length >= InitSize)
		return false;
	for (int s = L.length; s >= i; s--) 
		L.data[s] = L.data[s - 1];
	L.data[i - 1] = a;
	L.length++;
}
```

时间复杂度：最好情况O（1），最坏情况，O（n），平均情况，O（2/n）

#### 删除

ListDelete（&L,&e）：删除操作，删除表L中第i个位置的元素

代码实现如下：

```cpp
bool ListDelete(SqList& L, int i, int& e) {
	if (i<1 || i>L.length + 1)
		return false;
	e = L.data[i - 1];
	for (int i = i; i < L.length; i++)
		L.data[j - 1] = L.data[j];
	L.length--;
	return true;
}
```

与插入基本同理

#### 查找

GetElem（L，i）：按位查找，获取表L中第i个位置的元素的值

```cpp
int GetElem(SeqList L,int i){
    return L.data[i-1];
}
```

时间复杂度为O（1）

**按值查找**

LocateElem（L，e）

```cpp
int LocateElem(SeqList L,ElemType e){
    for(int i=0;i<L.length;i++)
        if(L.data[i] == e)
            return i+1;
}
```

## 链表

### 1.单链表

单链表每个节点不仅需要包含数据还要包含下个节点的指针

代码定义

```cpp
struct LNode{
    ElemType data;
    struct LNode *next;
}
```

包含一个数据域和一个只想下个节点的指针

初始化不带头节点的单链表

```cpp
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}
bool InitList(LinkList &L){
    L = NULL;
    return true;
}
```

带头结点的链表

```cpp
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode，*LinkList；
bool InitList(LinkList &L){
    L = (LNode *)malloc(sizeof(LNode));
    if(L == NULL)
        return false;
    L->next = NULL;
    return true;
}
```

### 2.单链表的插入和删除

#### 插入

按位序插入（带头结点）

LIstInsert(&L,i,e):插入操作，在表L中第i个位置上插入指定元素e

