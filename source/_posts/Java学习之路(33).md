---
title:  Java学习之路--super用法(1)
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-30

---
```java
/**
 * 测试super用法
 * @author 葛宇
 * super是对直接父类的引用，可以通过super来访问被子类覆盖的父类的方法或属性
 */
package 面向对象核心;

public class TestSuper {
	public static void main(String[] args) {
		new ChildClass().func();
	}
}

class FatherClass{
	public int value;
	
	public void func() {
		value = 100;
		System.out.println("FatherClass.value="+value);
	}
}

class ChildClass extends FatherClass{
	public int value;		//覆盖了父类的value属性
	
	public void func() {	//重写了父类func（）方法
		super.func();		//调用父类value
		value = 200;		//指子类的value
		System.out.println("ChildClass.value="+value);
		System.out.println(super.value);
	}
}

```
