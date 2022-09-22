---
title:  Java学习之路--java.lang.comparable接口实战
tags: [编程,学习,Java]
categories: [Java]
date: 2020-3-16

---
# 代码展示
```java
/**
 * @author 葛宇
 * 实现java.lang.comparable接口并测试Array.sort()
 */
class Employee implements java.lang.Comparable<Employee>{

	private int id;
	private int salary;
	private String name;
	
	public Employee(int id,int salary,String name){
		this.id = id;
		this.salary = salary;
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public int getSalary() {
		return salary;
	}
	public String getName() {
		return name;
	}
	
	@Override
	public int compareTo(Employee o) {
		if(name.compareTo(o.name)!=0) {
			return name.compareTo(o.name);
		}else {
		if (salary<o.salary) 
			return - 1 ;  
		if (salary>o.salary)  
			return 1 ;  
		return 0 ;  			      
		}
	}
}

class TestClass{
	public static void main(String[] args) {
		 Employee[] staff =  new Employee[ 4 ];  
	        staff[ 0 ] =  new Employee(001,5000,"harry");  
	        staff[ 1 ] =  new Employee(002,3000,"carl ");  
	        staff[ 2 ] =  new Employee(003,4000,"tony ");  
	        staff[ 3 ] =  new Employee(004,6000,"carl ");  
		
	    java.util.Arrays.sort(staff);
		for(Employee e: staff)  
        System.out.println( "id=" +e.getId()+ "  name=" +e.getName()+  "  salary=" +e.getSalary());  
	}  
}
```
**输出结果：**
![java](https://s3.bmp.ovh/imgs/2022/09/05/8939f59ba6ee01f6.png "运行结果")

**上述代码是某次Java作业，题目描述如下：**
> 编写员工Employee类，包含id、姓名、工资salary等属性。令Employee类实现java.lang.comparable接口，并实现compareTo（）方法，制定比较规则。在测试类中创建几个Employee对象，利用java.util.Array类中的方法对其进行排序并打印输出结果。PS:关于比较的规则，建议分别按照name和salary进行升序排列。

**题目出自接口技术一章，实则难点不在接口技术，Java新手在阅读上述代码往往会感到困惑，搞不懂为何对compareTo的自定义实现会影响sort方法的效果？**

# Array.Sort()
>即使是小白，在阅读完代码后，也应该会产生这样的猜测：line48的java.util.Arrays.sort(staff);的sort方法内会不会用到了compareTo方法了呢？

很好的猜想，让我们进入sort（）内部看看：
![java](https://s3.bmp.ovh/imgs/2022/09/05/e82a45de2e7c9fd7.png "内部实现")
进入sort（）内部实现后根据关键信息我们又能很轻易的追溯到mergeSort方法，那就继续进入mergeSort（）的内部实现：
![java](https://s3.bmp.ovh/imgs/2022/09/05/6e1c7b20b16b9dd9.png "内部实现")
**如果你看到这里：那么，恭喜你花生🥜，你发现的盲点！**
很显然，mergeSort内部的排序依赖了参与排序的对象的两两比较（dest[]是对象数组）for循环最里层做的事情是将对象转换为Compareble类型并调用其compareTo()方法，所以需要被比较的对象实现Comparable接口。感兴趣的话可以搜一下模板方法设计模式，这算其中一个典型的应用，个人认为并不属于纯接口技术的编程题。
# Comparable接口compareTo方法的总结
关于升序还是降序的问题，总结如下：
* 正序

```java
    @Override
    public int compareTo(Test o) {
        return this.i - o.i;
```
* 逆序

```java
    @Override
    public int compareTo(Test o) {
        return  o.i-this.i ;

```
