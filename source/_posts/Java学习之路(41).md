---
title:  Java学习之路--接口学习
tags: [编程,学习,Java]
categories: [Java]
date: 2022-10-30

---
[![xI7rNV.png](https://s1.ax1x.com/2022/10/30/xI7rNV.png)](https://imgse.com/i/xI7rNV)

说明：接口中的字段默认为 **static+final** 修饰
补充：
- 关于字段--> only **public, static & final** are permitted Java(33554775)

- 接口的方法默认为**public**类型且只能为**public**类型

- 接口可以声明为**public**或者不声明(**包访问**)

- JDK9中接口的**default方法和static方法**可以为**private**

- 当一个类，既继承一个父类，又实现若干个接口时 (重点)：父类中的成员方法与接口中的默认方法重名，子类就近选择执行父类的成员方法。

- JDK 1.8开始之后接口新增了如下三种方法：

  ​	 *a.默认方法（就是之前写的普通实例方法）*

  ​        	*-- 必须用default修饰，默认会public修饰*

  ​       	 *-- 只能用接口的实现类的对象来调用（所以当一个类实现多个接口时，多个接口中存在同名的默认方法，实现类必须重写这个方法）。*

  ​	*b.静态方法*

  ​        	*-- 默认会public修饰*

  ​       	 *-- 注意：接口的静态方法只能用接口的类名本身来调用（*所以如果实现了多个接口，多个接口中存在同名的静态方法并不会冲突*）。*

  ​	*c.私有方法（就是私有的实例方法）: JDK 1.9才开始有的。*

  ​        *-- 只能在本类中被其他的默认方法或者私有方法访问。*

---

**2022/10/31 更新**

[![xTuYsU.png](https://s1.ax1x.com/2022/10/31/xTuYsU.png)](https://imgse.com/i/xTuYsU)

**2022/11/1 更新**

[![xTWTIS.png](https://s1.ax1x.com/2022/11/01/xTWTIS.png)](https://imgse.com/i/xTWTIS)