---
title:  Maven中的dependencies和dependencyManagement标签
tags: [编程,学习,Java,Maven]
categories: [Maven]
date: 2022-9-6

---

本文主要记录学习SSM过程中对于Maven的pom.xml文件内**dependencies**和**dependencyManagement**两个标签的区别和理解。

闲言少叙，直奔主题👇

## 情景一：

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>web-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.32</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>

```
上述pom文件中同时在**dependencies**和**dependencyManagement**中引入了mysql依赖，但是版本不同。
<center>这里我们查看项目的外部库👇

![](https://img.gejiba.com/images/07d8c055577d9f4a3378869033e9df15.png)

可见同时作用时版本以**dependencies**中指定的为准
</center>

## 情景二：

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>web-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <!-- <version>8.0.25</version> -->
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.32</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>

```
此处我们将**dependencies**中的**version**标签进行注释，即不指定版本信息再进行观察。
<center>这里我们查看项目的外部库👇

![](https://img.gejiba.com/images/44c0e5bc68a6c84b0ef1f0d2a5f13fe2.png)

可见不在**dependencies**中指定版本时，外部库版本以**dependencyManagement**中定义的版本为准
</center>

## 情景三：

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>web-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <!-- <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>
    </dependencies> -->

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.32</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>

```
此处我们将**dependencies**标签的全部内容进行注释。
<center>这里我们查看项目的外部库👇

![](https://img.gejiba.com/images/1c6fd77878b719965c31b7dd0b6fc9bd.png)

可见在**dependencyManagement**中仅仅进行了对外部库的声明而非引用
</center>

## 总结：

- **dependencyManagement**其实只是一个管理jar的作用的声明标签,是管理jar的版本的,其他他的什么作用都没有,只是定义找到该jar的三维坐标,并不是真正的去执行下载的jar的功能
- 在**dependencies**中的依赖中如果没有声明jar的版本,就到**dependenciesManagement**中去找,找到就使用,没有就报错
- 在**dependencies**中声明jar的版本,则使用该版本,不管在**dependenciesManagement**中有没有声明jar的version,都以该jar的版本为主