---
title:  SpringMVC - 获取Get请求参数详解
tags: [JavaWeb,后端,SpringMVC,RESTful]
categories: [后端]
date: 2022-9-13

---

## 引言

**<center>📨本篇主要对比两种获取Get请求参数方法的区别📨</center>**

### 代码

先看如下两段基于@GetMapping注解的方法👇

```java
    @GetMapping("/page")
    public R<Page> page(int page, int pageSize, String name){
        log.info("page={}, pageSize={}, name={}", page, pageSize, name);

        //创建页面对象
        Page pageInfo = new Page(page,pageSize);
        //配置分页条件
        LambdaQueryWrapper<Employee> queryWrapper = new LambdaQueryWrapper();
        queryWrapper.like(StringUtils.isNotEmpty(name), Employee::getName, name);
        queryWrapper.orderByDesc(Employee::getUpdateTime);

        employeeService.page(pageInfo, queryWrapper);

        return R.success(pageInfo);
    }
```
```java
    @GetMapping("/{id}")
    public R<Employee> getByID(@PathVariable Long id){
        Employee employee = employeeService.getById(id);
        if (employee != null) {
            return R.success(employee);
        }
        return R.error("查询失败！");
    }
```

### 说明
上面两段代码中：
- 共同点是都是针对Get请求的后端代码逻辑处理
- 主要的不同之处是对于Get请求参数的获取方式
- 第一段代码的完整请求路径是：**http://localhost:8080/employee/page?page=1&pageSize=2**
- 第二段代码的完整请求路径是：**http://localhost:8080/employee/141242344443454456**

## 关于第一种方式

第一种属于传统方式的Get请求参数获取方法，即参数跟在问号后面。
我们假有这样一段请求需要处理：**http://localhost:8080/helloworld?name=张三**
则Controller中的处理代码如下👇
```java
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;
 
@RestController
public class HelloController {
    @GetMapping("/helloworld")
    public String helloworld1(@RequestParam("name") String name) {
        return "获取到的name是：" + name;
    }
}
```
### @RequestParam使用

这种方式涉及了@RequestParam注解的使用，关于该注解总结如下👇
- 不加@RequestParam，前端的参数名需要和后端控制器的变量名保持一致才能生效

- 不加@RequestParam，参数为非必传，加@RequestParam写法参数为必传。但@RequestParam可以通过@RequestParam(required = false)设置为非必传。

- @RequestParam可以通过@RequestParam(“userId”)或者@RequestParam(value = “userId”)指定传入的参数名。

- @RequestParam可以通过@RequestParam(defaultValue = “0”)指定参数默认值

- 如果接口除了前端调用还有后端RPC调用，则不能省略@RequestParam，否则RPC会找不到参数报错

- 访问时：
  - 不加@RequestParam注解：url可带参数也可不带参数
  - 加@RequestParam注解：url必须带有参数

## 关于第二种方式
第二种是典型的 RESTful 风格，ID参数值直接放在路径里面。
我们假有这样一段请求需要处理：**http://localhost:8080/helloworld/张三**
则Controller中的处理代码如下👇
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {

    @GetMapping("/helloworld/{name}")
    public String helloworld(@PathVariable("name") String name) {
        return "获取到的name是：" + name;
    }
}
```
### @PathVariable使用
通过 @PathVariable 可以将 URL 中占位符参数绑定到控制器处理方法的形参中。

- 若方法参数名称和需要绑定的url中变量名称一致时,可以简写：
  ```java   
    @GetMapping("/{id}")
    public R<Employee> getByID(@PathVariable Long id){
        Employee employee = employeeService.getById(id);
        if (employee != null) {
            return R.success(employee);
        }
        return R.error("查询失败！");
    }
  ```
- 若方法参数名称和需要绑定的url中变量名称不一致时，写成：
  ```java
    @GetMapping("/{id}")
    public R<Employee> getByID(@PathVariable("id") Long empId){
        Employee employee = employeeService.getById(id);
        if (employee != null) {
            return R.success(employee);
        }
        return R.error("查询失败！");
    }
  ```

 ---
 ## 2022/9/14 更新

 ### 第一种方式的补充：使用对象来接收参数

 此处定义一个简单的POJO如下👇
 ```java
 public class User {
    private String name;
    private Integer age;
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public Integer getAge() {
        return age;
    }
 
    public void setAge(Integer age) {
        this.age = age;
    }
}
 ```
定义一个RestController如下👇
```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;
 
@RestController
public class HelloController {
    @GetMapping("/helloworld")
    public String helloworld(User user) {
        return "name：" + user.getName();
    }
}
```
现在给出一个GET请求：**http://localhost:8080/helloworld?name=张三**
则浏览器可以看到：**name: 张三**