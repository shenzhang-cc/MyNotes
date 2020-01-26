# Data Structure

* [DataStructure](#datastructure)
  * [知识网络](#%E7%9F%A5%E8%AF%86%E7%BD%91%E7%BB%9C)
    * [复杂度渐进表示法](#%E5%A4%8D%E6%9D%82%E5%BA%A6%E6%B8%90%E8%BF%9B%E8%A1%A8%E7%A4%BA%E6%B3%95)
    * [复杂度分析技巧](#%E5%A4%8D%E6%9D%82%E5%BA%A6%E5%88%86%E6%9E%90%E6%8A%80%E5%B7%A7)
    * [应用实例：最大子列和问题](#%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B%E6%9C%80%E5%A4%A7%E5%AD%90%E5%88%97%E5%92%8C%E9%97%AE%E9%A2%98)
  * [线性表](#%E7%BA%BF%E6%80%A7%E8%A1%A8)
    * [多项式的表示](#%E5%A4%9A%E9%A1%B9%E5%BC%8F%E7%9A%84%E8%A1%A8%E7%A4%BA)
    * [线性表的ADT描述](#%E7%BA%BF%E6%80%A7%E8%A1%A8%E7%9A%84adt%E6%8F%8F%E8%BF%B0)
      * [顺序存储实现](#%E9%A1%BA%E5%BA%8F%E5%AD%98%E5%82%A8%E5%AE%9E%E7%8E%B0)
      * [链式存储实现](#%E9%93%BE%E5%BC%8F%E5%AD%98%E5%82%A8%E5%AE%9E%E7%8E%B0)
    * [二元多项式的表示](#%E4%BA%8C%E5%85%83%E5%A4%9A%E9%A1%B9%E5%BC%8F%E7%9A%84%E8%A1%A8%E7%A4%BA)
    * [十字链表表示稀疏矩阵](#%E5%8D%81%E5%AD%97%E9%93%BE%E8%A1%A8%E8%A1%A8%E7%A4%BA%E7%A8%80%E7%96%8F%E7%9F%A9%E9%98%B5)
  * [栈](#%E6%A0%88)
      * [栈的ADT描述](#%E6%A0%88%E7%9A%84adt%E6%8F%8F%E8%BF%B0)
        * [顺序存储实现](#%E9%A1%BA%E5%BA%8F%E5%AD%98%E5%82%A8%E5%AE%9E%E7%8E%B0-1)
        * [链式存储实现](#%E9%93%BE%E5%BC%8F%E5%AD%98%E5%82%A8%E5%AE%9E%E7%8E%B0-1)
      * [应用实例：中缀表达式求值](#%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B%E4%B8%AD%E7%BC%80%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC)
  * [队列](#%E9%98%9F%E5%88%97)
      * [队列的ADT描述](#%E9%98%9F%E5%88%97%E7%9A%84adt%E6%8F%8F%E8%BF%B0)

## 知识网络

<img src="Data%20Structure.assets/%E7%9F%A5%E8%AF%86%E7%BD%91%E7%BB%9C.jpg" style="zoom:80%;" />

<div style="page-break-after: always;"></div>
## 基本概念

<img src="Data%20Structure.assets/%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5.jpg" style="zoom:80%;" />


### 复杂度渐进表示法

<img src="Data%20Structure.assets/image-20200114210100803.png" alt="image-20200114210100803" style="zoom:80%;" />

*T(n) = n! 的算法不可用*

### 复杂度分析技巧

<img src="Data%20Structure.assets/image-20200114210442483.png" alt="image-20200114210442483" style="zoom:80%;" />

### 应用实例：最大子列和问题

给定n个整数的序列<img src="https://latex.codecogs.com/gif.latex?${A_1,&space;A_2,&space;...,A_n}$" title="${A_1, A_2, ...,A_n}$" />, 求函数<img src="https://latex.codecogs.com/gif.latex?$f(i,&space;j)&space;=&space;max(0,\sum_{k=i}^jA_k)$" title="$f(i, j) = max(0,\sum_{k=i}^jA_k)$" />的最大值。

**法一**

遍历法，找出所有子列的和，求最大值

<img src="Data%20Structure.assets/20200117_111218541_iOS.png" style="zoom: 30%;">

```c++
int MaxSubSeqSum_1(vector<int> A) { // 遍历法，找出所有子列的和，求最大值
    int maxSum = 0, tempSum;
    for (int i = 0; i < A.size(); i++)
    {
        for (int j = i; j < A.size(); j++)
        {
            tempSum = 0;
            for (int k = i; k < j; k++)
            {
                tempSum += A[k];
            }
            if (tempSum > maxSum) maxSum = tempSum;
        }
    }
    return maxSum;
}
```
**法二**

优化法一，去掉*k*的循环

```c++
int MaxSubSeqSum_2(vector<int> A) { //遍历法改进，去掉k的循环
    int maxSum = 0, tempSum;
    for (int i = 0; i < A.size(); i++)
        {
            tempSum = 0;
            for (int j = i; j < A.size(); j++)
            {
                tempSum += A[j];
                if (tempSum > maxSum) maxSum = tempSum;
            }
        }
    return maxSum;
}
```

**法三**

分而治之（递归式）

<img src="Data%20Structure.assets/20200117_111255118_iOS.png" style="zoom: 50%;" />

<img src="Data%20Structure.assets/20200117_111328530_iOS.png" style="zoom: 25%;" />

```c++
int MaxSubSeqSum_3(vector<int> A) {  //分而治之，递归式
    return divideAndConquer(A, 0, A.size() - 1);
}

int divideAndConquer(vector<int>& A, int left, int right) {
    int maxLeftSum = 0, maxRightSum = 0, 
        maxLeftBorderSum = 0, maxRightBorderSum = 0, maxBorderSum = 0;
    int tempSumL = 0, tempSumR = 0; 
    if (left == right)  // 递归出口
    {
        if (A[left] > 0) return A[left];
        else return 0;
    }
    int center = (left + right) / 2;
    maxLeftSum = divideAndConquer(A, left, center); // 左区域子列和最大值
    maxRightSum = divideAndConquer(A, center + 1, right);  // 右区域子列和最大值
    for (int i = center; i >= left; i--)  // 由分界线向左扫描找最大值
    {
        tempSumL += A[i];
        if (tempSumL > maxLeftBorderSum) maxLeftBorderSum = tempSumL;
    }
    for (int i = center + 1; i <= right; i++)  // 由分界向右扫描找最大值
    {
        tempSumR += A[i];
        if (tempSumR > maxRightBorderSum) maxRightBorderSum = tempSumR;
    }
    maxBorderSum = maxLeftBorderSum + maxRightBorderSum;
    return maxLeftSum > maxRightSum ? (maxLeftSum > maxBorderSum ? maxLeftSum : maxBorderSum) 
        : (maxRightSum > maxBorderSum ? maxRightSum : maxBorderSum);
}
```

**法四**
在线处理：当前累加和小于0时就丢弃重新将tempSum归于0，继续累加。每次迭代都与当前的最大值比较。

```c++
int MaxSubSeqSum_4(vector<int> A) {  // 在线处理
    int tempSum = 0, maxSum = 0;
    for (int i = 0; i < A.size(); i++)
    {
        tempSum += A[i];
        if (tempSum > maxSum) maxSum = tempSum;
        else if (tempSum < 0) tempSum = 0;			
    }
    return maxSum;
}
```

## 线性表

![](Data%20Structure.assets/012017481363_0%E7%BA%BF%E6%80%A7%E8%A1%A8_1.jpg)

### 多项式的表示

*表示多项式$f(x) = a_{0}x_{0} + a_{1}x_{1} + ... + a_{n}x_{n}$*

**顺序存储结构直接表示**
使用一维数组A[a~1~, a~2~, ... , a~n~], 其中a~i~表示x^i^的系数。

缺点：空间浪费，可能有很多0项。

**顺序存储结构表示非零项**

二元组(a~i~, i), a~i~表示系数， i是指数。

**链式结构存储非零项**

结点为：

```c++
struct Node {
    int coef;  //系数
    int expon; //指数
    struct Node* next;
}
```

### 线性表的ADT描述

线性表其实就是一个序列，可有序可无序，所谓“线性”主要指的是每个元素之间的关系是线性的。即第 *i* 个元素只与第 *i - 1* 和第 *i + 1* 个元素相关联。线性表可以采用数组存储，当然也可以使用链表存储。

#### 顺序存储实现

```c++
// 结点定义
const int MAXSIZE = 100;
typedef int ElementType;
typedef struct LNode* List;
class LNode {
public:
    ElementType data[MAXSIZE];
    int last;  // 指明线性表最后一个元素
};

List CreateStack() {
    List L;
    L = (List)malloc(sizeof(class LNode));
    L->last = -1;
    return L;
}

ElementType FindKth(int k, List L) {
    if (k < 0 || L->last < k)
    {
        printf("L->Data[%d]不存在元素", k);
    }
    return L->data[k];
}

int Find(ElementType x, List L) {
    int i = 0;
    for (i; (i <= L->last) && (L->data[i] != x); i++);
    if (i <= L->last) return i;
    else return -1;
}

void Insert(int i, ElementType x, List L) {
    if (L->last + 1 > MAXSIZE)
    {
        printf("表已满");
        return;
    }
    if (i < 1 || i > L->last + 2) 
    {
        printf("位置不合法");
        return;
    }
    for (int j = L->last; j >= i - 1; j--)
    {
        L->data[j + 1] = L->data[j];
    }
    L->data[i - 1] = x;
    L->last++;
    return;
} 

void Delete(int i, List L) {
    int j;
    if (i < 0 || i > L->last)
    {
        printf("位置不合法");
        return;
    }
    for (j = i; j <= L->last; j++)
    {
        L->data[j] = L->data[j + 1];
    }
    L->last--;
    return;
}

int Length(List L) {
    return L->last + 1;
}
```

#### 链式存储实现

```c++
// 结点定义
typedef int ElementType;
typedef struct LNode* List;
struct LNode {
    ElementType data;
    List next;
};

List CreateStack() {
    List L = (List)malloc(sizeof(struct LNode));
    L->next = NULL;
    return L;
}

int Length(List L) {
    int len = 0;
    List p = L;
    while (p)
    {
        p = p->next;
        len++;
    }
    return len;
};

List KthFind(int k, List L) {
    List p = L;
    int i = 1;
    while (p && i < k)
    {
        p = p->next;
        i++;
    }
    if (i == k)
    {
        return p;
    }
    else return NULL;
}

//List Find(ElementType x, List L) {
//	List p = L;
//	while (p)
//	{
//		if (p->data != x)
//		{
//			p = p->next;
//		}
//		else return p;
//			
//	}
//	return p;
//};

List Find(ElementType x, List L) {
    List p = L;
    while (p && p->data != x) p = p->next;
    return p;
}

List Insert(int i, ElementType x, List L) {
    List p, s;  //s指向新插入的节点, p指向第i-1个节点
    if (i == 1)
    {
        s = (List)malloc(sizeof(struct LNode));
        s->data = x;
        s->next = L;
        return s;
    }
    p = KthFind(i - 1, L);
    if (!p)
    {
        printf("序号错误");
        return NULL;
    }
    else
    {
        s = (List)malloc(sizeof(struct LNode));
        s->data = x;
        s->next = p->next;
        p->next = s;
        return L;
    }
};

List Delete(int i, List L) {
    List p, s;
    if (i == 1)
    {
        s = L;
        if (L)
        {
            L = L->next;
            free(s);
            return L;
        }
        else return NULL;
    }
    p = KthFind(i - 1, L);
    if (!p)
    {
        printf("结点错误");
        return NULL;
    }
    else
    {
        s = p->next; // s指向第i个结点
        p->next = s->next;
        free(s);
        return L;
    }
};
```

### 二元多项式的表示

*$p(x, y) = (9y^2 + 4)x^{12} + (15y^3 - y)x^8 + 3x^2$*

<img src="Data%20Structure.assets/20200120_125006968_iOS-1579939073579.png" alt="20200120_125006968_iOS"  />

### 十字链表表示稀疏矩阵

**结点定义**

![](Data%20Structure.assets/IMG_0964.JPG)

**链表示意**

![QQ截图20200120205257](Data%20Structure.assets/QQ%E6%88%AA%E5%9B%BE20200120205257.png)

## 栈

#### 栈的ADT描述

栈本质就是加了一定限制的线性表，这个限制即只允许在一端插入或删除元素。

##### 顺序存储实现

一维数组 + 记录栈顶元素位置的变量

```c++
typedef struct SNode* Stack;
typedef int ElementType;
const int maxsize = 100;

struct SNode {
    ElementType data[maxsize];
    int top; // top用于记录栈顶元素的位置
};

Stack CreateStack() {
    Stack s = (Stack)malloc(sizeof(struct SNode));
    s->top = -1;
    return s;
}

int IsEmpty(Stack s) {
    if (s->top == -1) return 1;
    else return 0;
}

int IsFull(Stack s) {
    if (s->top == maxsize - 1) return 1;
    else return 0;
}

void Push(ElementType x, Stack s) {
    if (s->top == maxsize)
    {
        printf("栈已满");
        return;
    }
    else
    {
        s->data[++(s->top)] = x;
        return;
    }
}

ElementType Pop(Stack s) {
    if (s->top == -1)
    {
        printf("栈空");
        return -1;
    }
    else
    {
        return s->data[(s->top)--];
    }
}
```

**用一个数组实现两个栈**

![](Data%20Structure.assets/IMG_0965.JPG)

##### 链式存储实现

单向链表，在头节点一侧进行插入、删除结点操作（单向链表无法向前寻址）

```c++
typedef int ElementType;
typedef struct SNode* Stack;

struct SNode {
    ElementType data;
    Stack next;
};

Stack CreateStack() {
    Stack s = (Stack)malloc(sizeof(struct SNode));
    s->next = NULL;
    return s;
}

int IsEmpty(Stack s) {
    return (s->next == NULL);
}

void Push(Stack s, ElementType x) {
    Stack temp = (Stack)malloc(sizeof(struct SNode));
    temp->data = x;
    temp->next = s->next;
    s->next = temp;
}

ElementType Pop(Stack s) {
    if (IsEmpty(s))
    {
        printf("堆栈为空");
        return NULL;
    }
    else 
    {
        Stack firstNode = s->next;
        s->next = firstNode->next;
        ElementType top = firstNode->data;
        free(firstNode);
        return top;
    }
}

```

#### 应用实例：中缀表达式求值

*中缀表达式*：$ 2 + 9 / 3 - 5$

*后缀表达式*:  $293 / + 5 -$

step 1. 将中缀表达式转换为后缀表达式

![](Data%20Structure.assets/IMG_0966.JPG)

**summary**：如何将中缀表达式转化为后缀表达式

![](Data%20Structure.assets/QQ%E6%88%AA%E5%9B%BE20200126171332.png)

step 2. 利用栈求解后缀表达式

![](Data%20Structure.assets/IMG_0967.JPG)

## 队列

#### 队列的ADT描述

队列是加了另一种限制的线性表，只允许在一端插入，另一端删除。













