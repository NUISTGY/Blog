---
title:  Java学习之路--抽象类
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试抽象类
 * @author 葛宇
 */
package com.github.nuistgy.testabstract;

abstract public class Animal {
  //有抽象方法必须是抽象类
	abstract public void shout();	
  //抽象方法没有实现过程
	
	static public void breath() {
		//抽象类可以包含普通方法或静态方法
		System.out.println("呼吸！");
	}
	
	static int a;
	int b;
	
	public static void main(String[] args) {
	  //Animal a = new Animal();	抽象类无法实例化
		Animal.breath();		  //抽象类的普通静态方法可以直接使用
		Animal.a = 1;			  //抽象类的普通静态成员变量可以直接使用
	}
}

class Dog extends Animal{

	@Override
	public void shout() {
		System.out.println("汪汪！");
		//子类必须对父类抽象方法进行实现
	}	
}

/*总结：
 * 有抽象方法的类必须定义为抽象类
 * 抽象类可以包含属性、方法、构造器，但是不能被实例化
 * 普通属性和方法只能被子类调用，静态方法或属性可以直接使用
 * 抽象方法必须被子类实现
 * 抽象类的意义在于为子类提供统一的、规范的模板
 */



```
