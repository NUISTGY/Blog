---
title: JDBC-JDBC 加载数据库驱动，获取数据库连接
tags: [Java, JDBC, 数据库]
categories: [JDBC]
date: 2022-12-22
---

## 加载数据库驱动

通常来说，JDBC 使用 Class 类的 forName() 静态方法来加载驱动，需要输入数据库驱动代表的字符串。

例如： 

加载 MySQL 驱动：

```java
Class.forName("com.mysql.jdbc.Driver");
```

加载 Oracle 驱动：

```java
Class.forName("oracle.jdbc.driver.OracleDriver");
```

以及之后我 demo 使用的 SQLite 驱动

```java
Class.forName("org.sqlite.JDBC");
```

这些数据库驱动的字符串，可以在数据库厂商提供的驱动（jar 包）找到，例如我使用的 SQLite：

```xml
<dependency>
    <groupId>org.xerial</groupId>
    <artifactId>sqlite-jdbc</artifactId>
    <version>3.8.11.2</version>
</dependency>
```

在下面这个文件内，就存放着该驱动的字符串：

org\xerial\sqlite-jdbc\3.8.11.2\sqlite-jdbc-3.8.11.2.jar!\META-INF\services\java.sql.Driver

## 获取数据库连接

DriverManagement 提供了一系列的方法，来获取 Connection，每一个 Connection 代表一个对数据库的物理连接：

```java
public static Connection getConnection(String url, java.util.Properties info) throws SQLException；
public static Connection getConnection(String url, String user, String password) throws SQLException；
public static Connection getConnection(String url) throws SQLException；
```

 

对于大多数的数据而言，获取 Connection 需要数据库 URL，登陆数据库的用户和密码。

数据库的用户和密码，通常由 DBA（Database Administrator）来分配，同时需要具有登录数据库的权限。

数据库 URL 遵循如下写法：

```
jdbc:subprotocol:other stuff
```

上面的写法中，jdbc 是固定的，subprotocol 指特定的数据库驱动，而后面的 other 和 stuff 则不固定，不同的数据库写法也存在差异。

 

而我 demo 的 SQLite 数据库，由于只是一个很小型的数据库，所以只需要 URL 就可以获取连接：

```java
DriverManager.getConnection("jdbc:sqlite:G:/github-workspace/jdbc/src/main/resources/db/test.db");
```

 

特定的数据库 URL 写法，在 JDBC 驱动文档中会提供。


## 使用 properties 文件

一般来说，数据库驱动字符串，数据库连接 URL，数据库用户和密码这些信息，都不会直接在代码中写死，而是使用一个 properties 文件去存储这些信息。

例如 Hibernate, MyBatis 这些 ORM 框架都是采取了这种做法。

 

下面 demo 一个我写的例子：

 

**sql.properties**

```java
driver=org.sqlite.JDBC
url=jdbc:sqlite:{0}
```

 

**SqlProperties.java**

```java
package com.gerrard.util;
 
import com.gerrard.constants.ErrorCode;
import com.gerrard.exception.JdbcSampleException;
import lombok.Getter;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.text.MessageFormat;
import java.util.Properties;
 
@Getter
public final class SqlProperties {
 
    private static final String PROPERTIES_PATH = new File("").getAbsolutePath().replace("\\", "/") + "/src/main/resources/sql.properties";
    private static final String DB_ROOT_URL = new File("").getAbsolutePath().replace("\\", "/") + "/src/main/resources/db/";
    private static SqlProperties sqlProps;
 
    private static final String DB_TEST = "test.db";
 
    private String driver;
    private String url;
 
    private SqlProperties() {
 
    }
 
    public static final SqlProperties getInstance() {
        if (sqlProps == null) {
            try {
                Properties props = new Properties();
                props.load(new FileInputStream(PROPERTIES_PATH));
                transferProperties(props);
            } catch (IOException e) {
                String errorMessage = "Fail to load properties file";
                throw new JdbcSampleException(ErrorCode.LOAD_PROPERTIES_ERROR, errorMessage);
            }
        }
        return sqlProps;
    }
 
    private static void transferProperties(Properties props) {
        sqlProps = new SqlProperties();
        sqlProps.driver = props.getProperty("driver");
        sqlProps.url = props.getProperty("url");
        transferDbUrl();
    }
 
    private static void transferDbUrl() {
        String params[] = {DB_ROOT_URL + DB_TEST};
        sqlProps.url = MessageFormat.format(sqlProps.url, params);
    }
}
```

 

**DriverLoader.java**

```java
package com.gerrard.util;
 
import com.gerrard.constants.ErrorCode;
import com.gerrard.exception.JdbcSampleException;
 
public final class DriverLoader {
 
    private DriverLoader() {
 
    }
 
    public static void loadSqliteDriver() {
        try {
            Class.forName(SqlProperties.getInstance().getDriver());
        } catch (ClassNotFoundException e) {
            throw new JdbcSampleException(ErrorCode.LOAD_DRIVER_EXCEPTION, "Fail to load sqlite driver");
        }
    }
}
```

 
**Connertor.java**

```java
package com.gerrard.util;
 
import com.gerrard.constants.ErrorCode;
import com.gerrard.exception.JdbcSampleException;
 
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
 
public final class Connector {
 
    private Connector() {
 
    }
 
    public static Connection getSqlConnection() {
        String url = SqlProperties.getInstance().getUrl();
        try {
            return DriverManager.getConnection(url);
        } catch (SQLException e) {
            String errorMessage = "Fail to get SQL connection with url " + url;
            throw new JdbcSampleException(ErrorCode.GET_CONNECTION_EXCEPTION, errorMessage);
        }
    }
}
```