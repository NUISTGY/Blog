---
title:  Java学习之路--静态导入
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-27

---
```java
/**
   * 测试静态导入
 * @author 葛宇
 */
package 面向对象;
import static java.lang.Math.PI;

public class TestStaticImport {
	public static void main(String[] args) {
		System.out.print(PI);
	}
}

//静态导入关键字：import static xxx;
//要求导入的东西必须是静态属性如静态成员方法或静态常量等

```
