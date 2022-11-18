---
title:  Java学习之路--静态成员的继承问题
tags: [编程,学习,Java]
categories: [Java]
date: 2022-11-15

---

**看两段代码：**

```java
public class ExtendsDemo {
    public static void main(String[] args) {
        Cat c = new Cat();
        c.test();
    }
}

class Cat extends Animal{}

class Animal{
    public static String schoolName ="黑猫";
    public static void test(){
        System.out.println(schoolName);
    }
}

/**
 * 输出结果：
 * 黑猫
 */
```

这段代码表明静态的父类成员依旧是可以被继承到子类的，否则不可能通过子类引用访问到这个静态的父类方法。

---


```java
public class ExtendsDemo {
    public static void main(String[] args) {
        Cat c = new Cat();
        Animal a = new Cat();

        c.test();
        a.test();

    }
}

class Cat extends Animal{
    public static String schoolName = "白猫";
    public static void test(){
        System.out.println(schoolName);
    }
}

class Animal{
    public static String schoolName ="黑猫";
    public static void test(){
        System.out.println(schoolName);
    }
}

/**
 * 输出结果：
 * 白猫
 * 黑猫
 */
```

这段代码表明，虽然静态成员可以被继承，但是和子类的同名静态方法并不构成重写关系，子类的同名静态方法会将父类同名静态方法隐藏，因此也不支持多态。