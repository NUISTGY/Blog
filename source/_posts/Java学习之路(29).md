---
title:  Java学习之路--重写
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试重写
 * @author 葛宇
 */
package 面向对象核心;

public class TestOverride {
	public static void main(String[] args) {
		Airplane obj = new Airplane();
		obj.run();
	}
}

class Transportation{

	public void run() {
		System.out.println("Running");
	}
	
	public Person whoIsPsg() {
		return new Person();
	}
}

class Airplane extends Transportation{
	
	public void run() {
		System.out.println("Flying");
		//重写了父类的run()
	}	
	
 	public Student whoIsPsg() {
		return new Student();
		//重写了父类的whoIsPsg()
	}

/*	
* 	public Student whoIsPsg() {
*		return new Student();
*		//重写了父类的whoIsPsg()
*	}
*/
/*	
 * 	public Object whoIsPsg() {
 *		return new Object();
 *		//重写了父类的whoIsPsg()但报错
 *	}
 */
 	
}

/* 构成重写的条件：
 * 子类继承父类
 * 重写的方法名，形参列表要相同
 * 返回值类型和声明异常的类型，子类应<=父类
 * 方法访问权限：子类>=父类
 */

```
