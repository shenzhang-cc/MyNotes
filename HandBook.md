# Personal HandBook

## 调用类（实例化类）的两种方式

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

## vector中插入元素

insert(a, b) 其中a是迭代器，b是要插入的元素；

## 栈

- 对空栈使用pop()、top()等方法会报错。