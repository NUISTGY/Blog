---
title:  Java学习之路--循环语句中标签的continue
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-26

---
```java
/**
 * 测试带标签的continue
 * @author 葛宇
 */
package 控制语句;

//Java中保留了goto关键字但并不允许使用goto语句
public class TestLableContinue {
	public static void main(String[] args) {
		
		//打印101到150之间所有质数
		outer:for(int i=101;i<=150;i++) {
			for(int j=2;j<i/2;j++) {
				if(i%j==0) {
					continue outer;
				}
			}
			System.out.println(i+" ");
		}
		
	}
}

```
