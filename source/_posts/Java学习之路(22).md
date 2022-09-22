---
title:  Java学习之路--构造器
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-27

---
```java
/**
    * 测试构造器
 * @author 葛宇
 */
package 面向对象;

class Point{
	double x,y;
	//构造方法名称和类名必须保持一致
	//构造器会返回对象的地址但是无需指定返回值，可以单独使用return表示结束方法体
	public Point(double x,double y) {
		this.x = x;
		this.y = y;
		return; 
	}
	
	public Point() {
		//无参构造器，如果有了自己定义的构造器，系统便不再生成，想要无参构造器的话还得自己写上
	}
	
	public double getDitance(Point p) {
		System.out.println("传入对象的地址："+p);
		return Math.sqrt((this.x-p.x)*(this.x-p.x)+(this.y-p.y)*(this.y-p.y));
	}
}

public class TestConstructor {
	public static void main(String[] args) {
		Point a = new Point(3.0,4.0);
		Point b = new Point(0.0,0.0);
		System.out.println("创建的对象的地址："+b);
		System.out.println(a.getDitance(b));
	}
}


```
