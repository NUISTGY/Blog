---
title:  Java学习之路--super用法(2)
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-30

---
```java
/**
 * 测试super用法
 * @author 葛宇
 */
package 面向对象核心;

public class TestSuper1 {
	public static void main(String[] args) {
		System.out.println("创建一个Child对象：");
		new ChildClass_();
	}
}

class FatherClass_{
	public FatherClass_() {
	  //super();
		System.out.println("创建FatherClass_");
	}
}

class ChildClass_ extends FatherClass_{
	public ChildClass_() {
	  //super();
	  //所有构造方法第一句都是super();不加编译器会自动添加
		System.out.println("创建ChildClass_");
	}
}

//本例继承树为：Object（顶端）-->FatherClass_-->ChildClass_（底端）
//类的继承都是先一步一步向上追溯到继承树顶端，再从顶端向底端一个一个调用类构造器，完成初始化

```
