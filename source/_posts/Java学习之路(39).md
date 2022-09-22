---
title:  Java学习之路--String类初识
tags: [编程,学习,Java]
categories: [Java]
date: 2020-2-19

---
```java
package com.github.nuistgy.teststring;
/**
 * 测试字符串类
 * @author 葛宇
 *
 */
public class TestString {
	public static void main(String[] args) {
		String str = "abc";
		String str1 = new String("def");
		String str2 = "abc"+"def";
		String str3 = "1"+1;
		System.out.println(str3);
		
		////////////////////////////////////
		
		String str4 = "xyz";
		String str5 = "xyz";
		String str6 = new String("xyz");
		System.out.println(str4==str5);				//输出true
		System.out.println(str5==str6);				//输出false
		System.out.println(str5.equals(str6));		//输出true
	}
}

```
> 关于String类既没什么好讲的又有太多东西可讲，因为相关知识点很多、很细小，建议适合配合document使用。博客里仅推送一些必要的以及学习中应值得注意的知识点，后续也会进行补充。

关于上述代码，注意三点：“+”号的作用是充当字符串连接符；str4和str5均指向字符串常量池的同一个字符串“xyz”，但是new String得到的字符串对象是一个全新的，故“==”判别为false；equals用来对字符串内容作比较，==是对地址做比较。


