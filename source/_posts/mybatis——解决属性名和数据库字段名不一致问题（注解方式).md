---
title:  MyBatis——解决属性名和数据库字段名不一致问题（注解方式)
tags: [编程,学习,Java,MyBatis]
categories: [MyBatys]
date: 2022-9-6

---
## 提问与解答：
**提问：**当我们使用注解开发时有时会遇到数据库字段名与实体类属性名不一致的问题。xml方式开发可以通过结果集映射的方式解决，那注解方式开发要怎么解决呢？

**解答：**利用 **@Results()**注解！

Results注解中有两个常用的参数，一个是id，另一个是value。

- **id**：这个参数的主要作用在于唯一标记这个Results注解，如果接口中的其他抽象方法也需要通过result注解来解决属性名和数据库字段名不一致问题，那么重新写一个Results注解就太麻烦了，这时我们就可以通过@ResultMap()注解中传入Results注解的参数id来引用Results注解中的内容。

- **value**：这个参数用于建立实体类与数据库表的映射关系，其中可以填写多个@Result注解，用来将实体类的属性名和数据库字段名一一对应。⚠**需要注意如果是主键字段，@Result注解中需要设置id=true。**

## 代码示例：

```java
public interface UserMapper {
    @Select("select * from user")
    @Results(id="aaa",value={
            @Result(id=true,column = "id",property = "userId"),
            @Result(column = "name",property = "userName"),
            @Result(column = "age",property = "userAge")
    })
    List<User> getUsers();


    @Select("select count(id) from user")
    @ResultMap(value={"aaa"})
    int findTotalUser();
}
```