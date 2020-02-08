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

