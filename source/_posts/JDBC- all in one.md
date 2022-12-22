---
title: JDBC 快速回顾
tags: [Java, JDBC, 数据库]
categories: [JDBC]
date: 2022-12-30
---

## JDBC概述

- JDBC：Java Database Connectivity ,是sun公司为了简化和统一java连接数据库定义的一套规范

## JDBC和数据库驱动的关系

- 接口(JDBC)和实现(驱动jar)的关系

## JDBC编程6步

- 注册驱动
- 获得连接
- 获取执行sql语句的对象
- 执行sql语句
- 处理结果
- 释放资源

```
Class.forName("驱动的路径");
DriverManager.getConnection("数据库路径","数据库用户名","数据库密码");
statement : 执行SQL语句的对象
Connection:连接数据库的对象
ResultSet:结果集对象,作用:保存查询后的结果集
Close:释放资源
```

```java
package top.simba1949.jdbc;
import com.mysql.jdbc.Driver;
import org.junit.Test;
import java.sql.*;
public class JDBCTest {
    @Test
    public void tt01() throws SQLException {
        ResultSet resultSet = null;
        Statement statement = null;
        Connection connection = null;
        try {
            //1.注册驱动 DriverManger
            DriverManager.registerDriver(new Driver());
            //2.建立连接Connection
            String url = "jdbc:mysql://localhost:3306/test";
            String user = "root";
            String password = "19491001";
            connection = DriverManager.getConnection(url, user, password);
            //3.获取执行sql语句的对象Statement
            statement = connection.createStatement();
            //4.执行sql语句
            String sql = "select * from USER ";
            resultSet = statement.executeQuery(sql);
            //5.处理结果
            while (resultSet.next()){
                System.out.println(resultSet.getString("uid"));
                System.out.println(resultSet.getString(2));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6.释放资源
            //关闭结果集
            resultSet.close();
            //关闭执行sql语句的对象statement
            statement.close();
            //关闭连接
            connection.close();
        }
    }
}
```

## JDBC API详解

**1.java.sql.DriverManager**

作用：主要是用于加载驱动，并且创建和数据库的连接



1.1 RegisterDriver(Driver driver) ;注册驱动



- 翻阅源码发现,通过API的方式注册驱动,Driver会new两次,所有推荐这种写法:



```java
Class.forName("com.mysql.jdbc.Driver");
```



1.2 getConnection(String url, String user, String password) ;与数据库建立连接

常见的几种数据库连接字符串的写法:

```java
MySql写法:  jdbc:mysql://localhost:3306/sid
Oracle写法：     jdbc:oracle:thin:@localhost:1521:sid
SqlServer写法:    jdbc:microsoft:sqlserver://localhost:1433; DatabaseName=sid
```

**2.java.sql.Connection接口**


- 接口的实现在数据库驱动中。所有与数据库交互都是基于连接对象的。



2.1 createStatement() ;创建执行sql语句对象



2.2 prepareStatement(String sql) ;创建预编译执行sql语句的对象



**3.java.sql.Statement接口**



- 接口的实现在数据库驱动中
- 操作sql语句，并返回相应结果对象

3.1 Statement; 执行sql语句对象



- ResultSet executeQuery(String sql) 根据查询语句返回结果集。只能执行select语句。
- int executeUpdate(String sql) 根据执行的DML（insert update delete）语句，返回受影响的行数。
- boolean execute(String sql) 此方法可以执行任意sql语句。返回boolean值，表示是否返回的是ResultSet结果集。仅当执行select语句，且有返回结果时返回true, 其它语句都返回false



**4.java.sql.ResultSet接口**

4.1封装结果集,查询结果表的对象



- 提供一个游标，默认游标指向结果集第一行之前。
- 调用一次next()，游标向下移动一行。
- 提供一些getXXX方法。XXX代表的是数据类型



4.2ResultSet接口常用API

- boolean next() ;将光标从当前位置向下移动一行
- XXX getXXX(int columnIndex) : 根据列的序号获取XXX类型的值,列的序号从1开始
- XXX getXXX(String columnName) : 根据列名去获取XXX类型的值
- void close()关闭ResultSet 对象

## SQL注入问题解决：preparedStatement

### 什么是SQL注入:

SQL 注入是用户利用某些系统没有对输入数据进行充分的检查，从而进行恶意破坏的行为。



**1.preparedStatement概述**

预编译对象， 是Statement对象的子类。



特点：



- 性能要高
- 会把sql语句先编译,格式固定好,
- sql语句中的参数会发生变化，过滤掉用户输入的关键字(or)。



**2.用法**

2.1通过connection对象创建



- prepareStatement(String sql) ;创建prepareStatement对象
- sql表示预编译的sql语句,如果sql语句有参数通过?来占位



2.2通过setxxx方法来指定参数



- setxxx(int i,Obj obj); i 指的就是问号的索引(指第几个问号,从1开始),xxx是类型(eg:int,String,Long)

eg:

```java
String sql = "select *from user where username =? and password =?";
//创建prepareStatement
statement = connection.prepareStatement(sql);
//设置参数
statement.setString(1, username);
statement.setString(2, password);
resultSet = statement.executeQuery();
```

## 连接池技术

### 自定义连接池

```java
package top.simba1949.jdbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.LinkedList;
public class DataSourceTest {
    //1.创建容器，用于存放连接Connection
    private static LinkedList<Connection> pool = new LinkedList<>();
    private static String url = "jdbc:mysql://localhost:3306/test";
    private static String user = "root";
    private static String password = "19491001";
    //1.1初始化连接池中的连接
    static {
        try {
            Class.forName("com.mysql.jdbc.Driver");
            for (int i = 0;i < 3;i++){
                //1.2获取连接，放入池子
                Connection connection = DriverManager.getConnection(url, user, password);
                pool.add(connection);
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    /**
     * 2.获取连接，从连接池中获取连接
     * @return
     */
    public static Connection getConnection(){
        //LinkedList:移除并返回此列表的第一个元素。
        return pool.removeFirst();
    }
   
    public static void release(Connection connection){
        //将从连接池获取的连接归还连接池
        if (null != connection){
            pool.add(connection);
        }
    }
}
```



### 自定义规范连接池（装饰者设计模式）

**装饰者设计模式**

固定结构：接口A，已知实现类C，需要装饰者创建代理类B



- 创建类B，并实现接口A
- 提供类B的构造方法，参数类型为A，用于接受A的其他实现类
- 给类B添加类型为A的成员变量，用于存放A接口的实现类
- 增强需要的方法
- 实现不需要增强的方法，方法体重新调用成员变量存放的其他实现类对应的方法

```java
package top.simba1949.jdbc;
import java.sql.*;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.Executor;
public class ConnectionTest implements Connection{
    private Connection connection;
    private List<Connection> pool;
    public ConnectionTest(Connection connection) {
        this.connection = connection;
    }
    public ConnectionTest(Connection connection, List<Connection> pool) {
        this.connection = connection;
        this.pool = pool;
    }
    //需要增强的方法
    @Override
    public void close() throws SQLException {
        this.pool.add(this);
    }
    //不需要增强的方法调用原来的方法返回
    @Override
    public Statement createStatement() throws SQLException {
        return this.connection.createStatement();
    }
  ......
 
}
```