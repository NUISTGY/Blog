---
title:  Java学习之路--字符类型和布尔类型
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-20

---
```java
/**
 * Java字符类型和布尔类型
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestPrimitiveDateType {
	public static void main(String[] args) {
		char a = 'A';
		char b =  '中';
		char c = '1';
		/*
		 * 对于字符类型数据，Java使用UNICODE编码表示
		 * 每个字符占2字节 
		 * 字符用' '  字符串用" "
	   */
		
	  //转义字符
		System.out.println('a'+'b');  //输出195
		System.out.println(""+'a'+'b');  //输出ab
		System.out.println(""+'a'+'\''+'b');  //输出a'b
		
	  //测试布尔类型
	  //布尔类型数据占一位而不是一字节，且不可以用'0'或'1'
		boolean man = true;
		boolean woman = false;
		if(man) {
			//...
		}
	}
}


```
