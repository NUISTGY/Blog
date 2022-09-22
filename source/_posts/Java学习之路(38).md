---
title:  Java自增自减运算符天坑笔试题
tags: [编程,学习,Java,坑]
categories: [Java]
date: 2020-2-19

---
**问：下面程序运行的结果是什么？**
---
```java
int count = 0;
for(int i = 0; i < 100; i++)
    count = count++;
System.out.println("count = " + count);
```
**答：count = 0**
---
首先 count++ 是一个有返回值的表达式，返回值是 count 自加前面的值，java 对自加处理的流程是先把 count 的值（不是引用），拷贝到一个临时变量区，然后对 count 变量加 1，接着返回临时变量区的值。

所以上面代码中第一次循环执行的步骤是 JVM 把 count 的值（0）拷贝到临时变量区，然后 count 值加 1，这时 count 的值是 1，接着返回临时变量区的值（值还是 0），最后赋值给 count，此时 count 值被重置成 0。所以上面代码语句，count = count++可以按照如下代码来理解：
```java
int autoAdd(int count) {
    int temp = count;
    count = coutn + 1;
    return temp;
}
```
第一次循环后 count 的值还是 0，其他 99 次的循环也是一样，最终导致 count 的值始终没变，任然保持最初的状态，如果想要打印 100，则把语句count = count++改为count++即可。**不过这个问题在不同的语言环境中是不一样的，在 c++ 中count = count++与count++是等效的，但在 Java 中是不等效的。**


