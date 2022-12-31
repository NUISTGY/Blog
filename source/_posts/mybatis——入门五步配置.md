---
title:  MyBatis——基于XML的入门配置五步
tags: [编程,学习,Java,MyBatis]
categories: [MyBatys]
date: 2022-12-22
---
- 第一步： 配置Maven环境，打包方式设置为jar，加载MyBatis，MySQL依赖

```xml
<packaging>jar</packaging>
<dependencies>
        <!--  MyBatis  -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.11</version>
        </dependency>

        <!--  MySQL  -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>

    </dependencies>
```

- 第二步：新建，编辑mybatis-config.xml文件（放入resources文件夹）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="xxxx"/>
                <property name="password" value="xxxx"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

- 第三步：新建，配置xxxMapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
    <insert id="">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
        values (null,1003,"面包车",13.00,"2020-10-13","飞行汽车")
    </insert>
</mapper>
```

- 第四步：在xxxMapper中编写sql代码（在3中已完成）

- 第五步：把xxxMapper.txt文件路径放入mybatis-config.txt中（在2中已完成）

```xml
<mappers>
        <mapper resource="CarMapper.xml"/>
</mappers>
```

**注意⚠**
在mybatis-config.xml中有一行为
```xml
<transactionManager type="JDBC"/>
```
type类型可以写成两种，一种是JDBC另一种是MANAGED（不区分大小写）

1. JDBC：交给JDBC处理事务（默认false，表示开启事务，需要手动提交）
2. MANAGED：有用到spring框架时设置为此，表交给框架处理事务，如果没有用到框架设置为此类型，则没有人处理事务，会自动提交

**SqlSessionFactory.openSession()默认开启事务**
