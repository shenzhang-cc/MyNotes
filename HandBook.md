# Personal HandBook(c++)

## 常用
### 调用类（实例化类）的两种方式

```c++
class example {
    void test () {
		...
    }
}

void main () {
    //  实例化一个对象
    example obj;
    obj.test();
    // 定义一个指针
    example* pointer = new example();
    pointer->test();
}
```

### INT_MIN

INT_MIN在标准头文件limits.h中定义

1.#define INT_MAX 2147483647

2.#define INT_MIN (-INT_MAX - 1)

## vector

- 插入元素：insert(a, b)，其中a是迭代器，b是要插入的元素；
- 反转链表：reverse(a, b)，a、b为迭代器，指示要反转的元素范围

## 栈

- 对空栈使用pop()、top()等方法会报错。

## 优先队列

c++中使用优先队列：

```c++
#include<queue>
#include<vector>
using namespace std;

// maxheap(default)
priority_queue<int> q_min;
//is equal to↓↓↓
priority_queue<int, vector<int>, less<int> > q_min;

// minheap
priority_queue<int, vector<int>, greater<int> > q_max;
```

基本操作：与普通队列基本一致。

> top 访问队头元素 
>
> empty 队列是否为空 
>
> size 返回队列内元素个数 
>
> push 插入元素到队尾 (并排序) 
>
> emplace 原地构造一个元素并插入队列 
>
> pop 弹出队头元素 
>
> swap 交换内容



## 哈希表

## 力扣错误记录

>solution.cpp: In member function isMirror
>Line 24: Char 5: ==error: control reaches end of non-void function [-Werror=return-type]==
>     }
>     ^
>cc1plus: some warnings being treated as errors

**解决办法：**给函数最后添加一句“return”（虽然可能用不上）

# Personal HandBook(python)

## 批量初始化

```python
a, b, c, d = [0] * 4
```

注意，不能利用这种写法用于对象的赋值。

```python
from tkinter import*
a, b, c, d = [Entry()] * 4
// 此时的a、b、c、d其实指的是一个对象
```

