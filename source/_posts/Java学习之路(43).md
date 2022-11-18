---
title:  Java学习之路--常量初始化相关问题
tags: [编程,学习,Java]
categories: [Java]
date: 2022-11-16

---

**看这段代码：**

```java
class Test{

    static final int a = 1;
    static final int b;

    static{
        b = 1;
    }

// ------------------------- //    

    final int aa = 1;
    final int bb;
    final int cc;

    {
        bb = 1;
    }

    Test(){
        cc = 1;
        // b = 1;   ERROR
    }

// ------------------------- //    
    // {
    //     b = 1;   ERROR
    // }

    // static{
    //     bb = 1;  ERROR
    // }
    
}
```

常量初始化问题总结：

- 静态常量初始化：
  - 定义时赋值
  - 静态代码快中赋值
- 成员常量初始化：
  - 定义时赋值
  - 非静态代码快赋值
  - 构造函数中赋值
- 注意出错情况：
  - 在非静态代码快中或构造函数中为静态常量赋值
  - 在静态代码快中为成员常量赋值