---
title: JDBC-JDBC 编程六步
tags: [Java, JDBC, 数据库]
categories: [JDBC]
date: 2022-12-21
---

## JDBC 常用接口和类

- **DriverManager**
　　负责管理 JDBC 驱动的服务类，程序中主要的功能是获取连接数据库的 Connection 对象。

 

- **Connection**
　　代表一个数据库连接对象，要访问数据库，首先需要获得数据库连接。
    同时，Connection 接口提供了获取执行 SQL 语句的 Statement 和 PreparedStatement 对象，以及控制事物（Transaction）的方法。

 

- **Statement**
　　用于执行 SQL 语句的接口，同时支持执行 DDL（Data Definition Language），DCL（Data Control Language），DML（Data Manipulation Language）语句。
    我们常用的 CRUD 操作是 DML 语句。

 

- **PreparedStatement**
　　Statement 的子接口，代表一个预编译的 Statement 对象。
　　　　所谓预编译，就是事先将 SQL 传入到 PreparedStatement 对象中，不必每一次在执行的时候加载 SQL 语句，所以在性能上会有所提高。
　　　　常用来执行带参数的 SQL 语句。

 

- **ResultSet**
　　SQL 语句执行后的结果集对象。
　　　　当 Statement 或者其子接口，执行的是一个查询的 DML 语句，其返回就会是一个 ResultSet 对象。
　　　　ResultSet 中包含访问查询结果的方法。

 

## JDBC 编程步骤

通过以上介绍的 JDBC API，可以大致得知 JDBC 的编程步骤：

1. 加载数据库驱动。
2. 通过 DriverManager 获取数据库连接 Connection。
3. 通过 Connection 对象创建 Statement 对象（Statement，PreparedStatement，CallableStatement）。
4. 外部传入 SQL 语句，使用 Statement 执行 SQL。
5. 如果 SQL 是一个查询语句，操作结果集 ResultSet。
6. 回收数据库资源（实现 Closeable/AutoCloseable 的 Connection，Statement 和 ResultSet）。