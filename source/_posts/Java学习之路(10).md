---
title:  Java学习之路--字符串连接符
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-23

---
```java
/**
 * 测试字符串连接符
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestOperator_4 {
	public static void main(String[] args) {
		int a = 1;
		int b = 2;
		String c = "3";
		System.out.println(a+b);		//输出整数3
		System.out.println(a+c);		//输出整数1和字符串3的组合
		System.out.println(c+a);		//输出字符串3和整数1的组合
		System.out.println(a+b+c);		//输出整数3和字符串3的组合
		System.out.println(c+b+a);		//输出字符串32和整数1的组合
	
		char d = '4';					//字符型变量d对应的4的Unicode编码为U+0034(十六进制)
		System.out.println(d);			//输出字符4
		System.out.println(a+d);		//输出字符4转化为整数后的数值与1的和
		System.out.println(c+d);		//将字符4纳入字符串c,输出字符串34
		
	}
}

```
