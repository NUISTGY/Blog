---
title: Spring依赖注入注解@Resource和@Autowired
tags: [Spring, Java]
date: 2022-12-18
categories: [Spring]
---
## 1. 前言

`@Resource`和`@Autowired`注解都可以在**Spring Framework**应用中进行声明式的依赖注入。而且面试中经常涉及到这两个注解的知识点。今天我们来总结一下它们。

## 2. @Resource

全称`javax.annotation.Resource`,它属于**JSR-250**规范的一个注解，包含**Jakarta EE**（**J2EE**）中。**Spring**提供了对该注解的支持。我们来详细了解一下该注解的规则。

**该注解使用在成员属性和setter方法上。默认情况下`@Resource`按照名称注入，如果没有显式声明名称则按照变量名称或者方法中对应的参数名称进行注入。**

![4vdxb.png](https://i.328888.xyz/2022/12/18/4vdxb.png)

@Resource注解也可以完成非简单类型注入。那它和@Autowired注解有什么区别？

- @Resource注解是JDK扩展包中的，也就是说属于JDK的一部分。所以该注解是标准注解，更加具有通用性。(JSR-250标准中制定的注解类型。JSR是Java规范提案。)
- @Autowired注解是Spring框架自己的。
- **@Resource注解默认根据名称装配byName，未指定name时，使用属性名作为name。通过name找不到的话会自动启动通过类型byType装配。**
- **@Autowired注解默认根据类型装配byType，如果想根据名称装配，需要配合@Qualifier注解一起用。**
- @Resource注解用在属性上、setter方法上。
- @Autowired注解用在属性上、setter方法上、构造方法上、构造方法参数上。

@Resource注解属于JDK扩展包，所以不在JDK当中，需要额外引入以下依赖：【**如果是JDK8的话不需要额外引入依赖。高于JDK11或低于JDK8需要引入以下依赖。**】

```xml
如果你是Spring6+版本请使用这个依赖👇
<dependency>
  <groupId>jakarta.annotation</groupId>
  <artifactId>jakarta.annotation-api</artifactId>
  <version>2.1.1</version>
</dependency>
```

```xml
如果你是spring5-版本请使用这个依赖👇
<dependency>
  <groupId>javax.annotation</groupId>
  <artifactId>javax.annotation-api</artifactId>
  <version>1.3.2</version>
</dependency>
```

## 3. @Autowired

- 第一点注意：该注解可以标注在哪里？

- - 构造方法上
  - 方法上
  - 形参上
  - 属性上
  - 注解上

- 第二点注意：该注解有一个required属性，默认值是true，表示在注入的时候要求被注入的Bean必须是存在的，如果不存在则报错。如果required属性设置为false，表示注入的Bean存在或者不存在都没关系，存在的话就注入，不存在的话，也不报错。

![4vGld.png](https://i.328888.xyz/2022/12/18/4vGld.png)

- @Autowired注解可以出现在：属性上、构造方法上、构造方法的参数上、setter方法上。
- 当带参数的构造方法只有一个，@Autowired注解可以省略。
- @Autowired注解默认根据类型注入。如果要根据名称注入的话，需要配合@Qualifier注解一起使用。

## 4. 使用细节和其他注意事项

负责声明Bean的注解，常见的包括四个：

- @Component
- @Controller
- @Service
- @Repository

注意可以在声明Bean的注解中设置Bean的**name**，例如：

```java
@Component("xxxBean")
```

**问：如果把value属性彻底去掉，spring会被Bean自动取名吗？**

**答：会的。并且默认名字的规律是：<u>Bean类名首字母小写即可</u>。**

