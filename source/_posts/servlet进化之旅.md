---
title:  Servlet进化之旅 —— 从ServerSocket编程到SpringMVC框架开发
tags: [JavaWeb,后端,Servlet,SpringMVC,Tomcat]
categories: [后端]
date: 2022-9-8

---

# 前言
**<center>本篇主要记录Java后端开发学习过程中关于Servlet相关技术路线的梳理和总结</center>**

按技术迭代顺序依次从下述几个Web技术开展梳理：
- ServerSocket编程
- Web服务器
- Servlet规范
- SpringMCV框架

# ServerSocket编程

**<center>本部分通过手撸一个基于Java的简易HTTP服务器来体会最原初的JavaWeb后端开发过程</center>**

## 客户端

这里客户端就不使用Java代码编写了，可以考虑使用最基本的浏览器或者后端测试工具如PostMan。

## 服务端

- 第一步我们先创建ServerSocket，监听8080端口
- 第二步接收到请求后把流在控制台进行输出

```java
public class HttpServer {

    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        InputStreamReader isr = null;
        BufferedReader br = null;
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            serverSocket = new ServerSocket(8080);
            //调用accept()方法开始监听，等待客户端的连接
            while ((socket = serverSocket.accept()) != null ){
                List<String> lines = IOUtils.readLines(socket.getInputStream(), "utf-8");
                for (String line : lines) {
                    System.out.println(line);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            pw.close();
            os.close();
            br.close();
            isr.close();
            is.close();
            socket.close();
            serverSocket.close();
        }
    }
}
```
打印到控制台是以下这段文本:
![](https://img.gejiba.com/images/e49002a3b302ec04645132ea159a3f8a.png)

**解释一下：** 上述代码监听了8080端口并将客户端的访问请求打印为字符串，即控制台中的文本信息，只要我们的服务器解析这段字符串然后拼接成request，就能封装HTTP请求。但是这时观察浏览器界面发现没有任何响应。因为我们编写的HTTP服务器什么都没返回就关闭了连接。如果要浏览器正确的显示我们想要看到的helloword。同样也要拼接成浏览器能理解的http协议响应格式才能正确被解析。

下面继续改动代码，让http返回200成功，并返回helloword：
```java
public class HttpServer {

    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        InputStreamReader isr = null;
        BufferedReader br = null;
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            serverSocket = new ServerSocket(8080);
            //调用accept()方法开始监听，等待客户端的连接
            while ((socket = serverSocket.accept()) != null ){
                //获取输出流，响应客户端的请求
                os = socket.getOutputStream();
                pw = new PrintWriter(os);
                pw.write("HTTP/1.1 200 OK\n" +
                        "Date: Fri, 7 Sept 2022 06:07:21 GMT\n" +
                        "Content-Type: text/html; charset=UTF-8\n" +
                        "\n" +
                        "<html>\n" +
                        "      <head></head>\n" +
                        "      <body>\n" +
                        "            helloWord!\n" +
                        "      </body>\n" +
                        "</html>");
                pw.flush();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            pw.close();
            os.close();
            br.close();
            isr.close();
            is.close();
            socket.close();
            serverSocket.close();
        }
    }
}
```
浏览器访问：localhost:8080 可见：
![](https://img.gejiba.com/images/e252a903e9cf4d35eaa77788489e4a4c.png)

## 总结

上述示例实现了一个最简单的基于HTTP协议的Java服务器后端，里面并不包含对request请求字符串的处理，仅仅是打印了它，response也仅仅是按HTTP协议的规定封装了一句“helloworld！”。如此简易原始的HTTP服务器的整个访问和回复过程是如此繁琐，可见新技术出现的必要性。

# Web服务器

**<center>
Web服务器是一个应该程序(软件)，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是"提供网上信息浏览服务"</center>**

![](https://img.gejiba.com/images/d2eca8a0000dc6e254f5bcf9300b8418.png)

下面以Apache Tomcat服务器为例展示一个最简单的HTTP Web服务。

## 创建展示页面

在Tomcat根目录的**Webapps**下创建一个test文件夹，内部创建hello.html文件用于返回请求页面👇👇

![](https://img.gejiba.com/images/ba7d619e9f80190fa9b379c01b9ac67d.png)

## 启动服务器并请求资源

- 启动Tomcat根目录下的**bin/startup.bat**
- 使用Post访问**localhost:8080/test/hello.html**

![](https://img.gejiba.com/images/2c4d42954c3df74e89901c3eac879458.png)

## 总结

1. Web服务器作用?
➢封装HTTP协议操作，简化开发
➢可以将Web项目部署到服务器中，对外提供网上浏览服务
2. 什么是Tomcat?
Tomcat是一个轻量级的Web服务器， 支持Servlet/JSP少量JavaEE规范，也称为Web容器，Servlet容器，Servlet依赖于它或其他Web服务器才能正常运行。

# Servlet规范

## Servlet规范简介

Servlet（Server Applet，服务端小程序，是服务端的一小部分），全称Java Servlet，未有中文译文。是用Java编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态Web内容。狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。

### 补充：Servlet和Web服务器的关系
首先要明白我们从来不会在Servlet中写什么监听8080端口的代码，Servlet不会直接和客户端打交道！这些全被Web服务器封装了。

那请求是怎么来到Servlet的呢？答案是Servlet容器，比如我们最常用的Tomcat。Servlet都是部署在一个容器中的，不然你的Servlet根本不起作用。

Tomcat才是与客户端直接打交道的家伙，它监听了端口，请求过来后，根据URL等信息，确定要将请求交给哪个Servlet去处理，然后调用那个Servlet的service方法，service方法返回一个response对象，Tomcat再把这个respond返回给客户端。

## 接口代码
```Java
package Javax.Servlet;

import Java.io.IOException;

public interface Servlet {
    void init(ServletConfig var1) throws ServletException;

    ServletConfig getServletConfig();

    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    String getServletInfo();

    void destroy();
}
```

Servlet接口定义的是一套处理网络请求的规范，所有实现Servlet的类，都需要实现它的那五个方法，其中最主要的是两个声明周期方法init()和destory()，还有一个处理请求的service()。也就是说，所有实现Servlet接口的类，或者说，所有想要处理网络请求的类，都需要回答这三个问题：

- 你初始化时要做什么？
- 你销毁时要做什么？
- 你接收到请求时要做什么？

## 生命周期

1、客户端请求该 Servlet；

2、Tomcat加载 Servlet 类到内存；

3、Tomcat实例化并调用init()方法初始化该 Servlet；

4、Tomcat调用service()（根据请求方法不同调用doGet() 或者 doPost()，此外还有doHead()、doPut()、doTrace()、doDelete()、doOptions()）；

5、destroy()；

6、加载和实例化 Servlet。这项操作一般是动态执行的。然而，Server 通常会提供一个管理的选项，用于在 Server 启动时强制装载和初始化特定的 Servlet；

7、Server 创建一个 Servlet的实例；

8、第一个客户端的请求到达 Server；

9、Server 调用 Servlet 的 init() 方法（可配置为 Server 创建 Servlet 实例时调用）；

10、一个客户端的请求到达 Server；

11、Server 创建一个请求对象，处理客户端请求；

12、Server 创建一个响应对象，响应客户端请求；

13、Server 激活 Servlet 的 service() 方法，传递请求和响应对象作为参数；

14、service() 方法获得关于请求对象的信息，处理请求，访问其他资源，获得需要的信息；

15、service() 方法使用响应对象的方法，将响应传回Server，最终到达客户端。service()方法可能激活其它方法以处理请求，如 doGet() 或 doPost() 或程序员自己开发的新的方法；

## 继承体系
[![](https://img.gejiba.com/images/c4ed97e0e4b645e0968dc28a9fd82b22.png)](https://img.gejiba.com/image/EAAFoV)

### HttpServlet源码
```java
package javax.servlet.http;

import java.io.IOException;
import java.io.Serializable;
import java.lang.reflect.Method;
import java.text.MessageFormat;
import java.util.Enumeration;
import java.util.ResourceBundle;
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public abstract class HttpServlet extends GenericServlet implements Serializable {
    private static final String METHOD_DELETE = "DELETE";
    private static final String METHOD_HEAD = "HEAD";
    private static final String METHOD_GET = "GET";
    private static final String METHOD_OPTIONS = "OPTIONS";
    private static final String METHOD_POST = "POST";
    private static final String METHOD_PUT = "PUT";
    private static final String METHOD_TRACE = "TRACE";
    private static final String HEADER_IFMODSINCE = "If-Modified-Since";
    private static final String HEADER_LASTMOD = "Last-Modified";
    private static final String LSTRING_FILE = "javax.servlet.http.LocalStrings";
    private static ResourceBundle lStrings = ResourceBundle.getBundle("javax.servlet.http.LocalStrings");

    public HttpServlet() {
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_get_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }

    protected long getLastModified(HttpServletRequest req) {
        return -1L;
    }

    protected void doHead(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        NoBodyResponse response = new NoBodyResponse(resp);
        this.doGet(req, response);
        response.setContentLength();
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_post_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }

    protected void doPut(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_put_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }

    protected void doDelete(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_delete_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }

    private Method[] getAllDeclaredMethods(Class c) {
        if (c.equals(HttpServlet.class)) {
            return null;
        } else {
            Method[] parentMethods = this.getAllDeclaredMethods(c.getSuperclass());
            Method[] thisMethods = c.getDeclaredMethods();
            if (parentMethods != null && parentMethods.length > 0) {
                Method[] allMethods = new Method[parentMethods.length + thisMethods.length];
                System.arraycopy(parentMethods, 0, allMethods, 0, parentMethods.length);
                System.arraycopy(thisMethods, 0, allMethods, parentMethods.length, thisMethods.length);
                thisMethods = allMethods;
            }

            return thisMethods;
        }
    }

    protected void doOptions(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Method[] methods = this.getAllDeclaredMethods(this.getClass());
        boolean ALLOW_GET = false;
        boolean ALLOW_HEAD = false;
        boolean ALLOW_POST = false;
        boolean ALLOW_PUT = false;
        boolean ALLOW_DELETE = false;
        boolean ALLOW_TRACE = true;
        boolean ALLOW_OPTIONS = true;

        for(int i = 0; i < methods.length; ++i) {
            Method m = methods[i];
            if (m.getName().equals("doGet")) {
                ALLOW_GET = true;
                ALLOW_HEAD = true;
            }

            if (m.getName().equals("doPost")) {
                ALLOW_POST = true;
            }

            if (m.getName().equals("doPut")) {
                ALLOW_PUT = true;
            }

            if (m.getName().equals("doDelete")) {
                ALLOW_DELETE = true;
            }
        }

        String allow = null;
        if (ALLOW_GET && allow == null) {
            allow = "GET";
        }

        if (ALLOW_HEAD) {
            if (allow == null) {
                allow = "HEAD";
            } else {
                allow = allow + ", HEAD";
            }
        }

        if (ALLOW_POST) {
            if (allow == null) {
                allow = "POST";
            } else {
                allow = allow + ", POST";
            }
        }

        if (ALLOW_PUT) {
            if (allow == null) {
                allow = "PUT";
            } else {
                allow = allow + ", PUT";
            }
        }

        if (ALLOW_DELETE) {
            if (allow == null) {
                allow = "DELETE";
            } else {
                allow = allow + ", DELETE";
            }
        }

        if (ALLOW_TRACE) {
            if (allow == null) {
                allow = "TRACE";
            } else {
                allow = allow + ", TRACE";
            }
        }

        if (ALLOW_OPTIONS) {
            if (allow == null) {
                allow = "OPTIONS";
            } else {
                allow = allow + ", OPTIONS";
            }
        }

        resp.setHeader("Allow", allow);
    }

    protected void doTrace(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String CRLF = "\r\n";
        String responseString = "TRACE " + req.getRequestURI() + " " + req.getProtocol();

        String headerName;
        for(Enumeration reqHeaderEnum = req.getHeaderNames(); reqHeaderEnum.hasMoreElements(); responseString = responseString + CRLF + headerName + ": " + req.getHeader(headerName)) {
            headerName = (String)reqHeaderEnum.nextElement();
        }

        responseString = responseString + CRLF;
        int responseLength = responseString.length();
        resp.setContentType("message/http");
        resp.setContentLength(responseLength);
        ServletOutputStream out = resp.getOutputStream();
        out.print(responseString);
        out.close();
    }

    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();
        long lastModified;
        if (method.equals("GET")) {
            lastModified = this.getLastModified(req);
            if (lastModified == -1L) {
                this.doGet(req, resp);
            } else {
                long ifModifiedSince = req.getDateHeader("If-Modified-Since");
                if (ifModifiedSince < lastModified / 1000L * 1000L) {
                    this.maybeSetLastModified(resp, lastModified);
                    this.doGet(req, resp);
                } else {
                    resp.setStatus(304);
                }
            }
        } else if (method.equals("HEAD")) {
            lastModified = this.getLastModified(req);
            this.maybeSetLastModified(resp, lastModified);
            this.doHead(req, resp);
        } else if (method.equals("POST")) {
            this.doPost(req, resp);
        } else if (method.equals("PUT")) {
            this.doPut(req, resp);
        } else if (method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if (method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if (method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }

    private void maybeSetLastModified(HttpServletResponse resp, long lastModified) {
        if (!resp.containsHeader("Last-Modified")) {
            if (lastModified >= 0L) {
                resp.setDateHeader("Last-Modified", lastModified);
            }

        }
    }

    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request;
        HttpServletResponse response;
        try {
            request = (HttpServletRequest)req;
            response = (HttpServletResponse)res;
        } catch (ClassCastException var6) {
            throw new ServletException("non-HTTP request or response");
        }

        this.service(request, response);
    }
}
```

## Servlet示例

**<center>👇此处展示一段处理登录业务的Servlet代码👇</center>**

```java
package com.geyu.web;

import com.geyu.web.mapper.UserMapper;
import com.geyu.web.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintWriter;
import java.util.Map;

@WebServlet("/loginServlet")
public class ServletDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        SqlSession sqlSession = sqlSessionFactory.openSession();

        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        User user = userMapper.select(username, password);

        sqlSession.close();

        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        if (user != null){
            //登陆成功
            writer.write("登陆成功");
        }else {
            //登陆失败
            writer.write("登录失败");
        }

    }
}
```

## 总结
讲了这么多废话，总结来说Servlet就是一群人来制定Java应用中使用Web时的各种规范，统一接口，其他内部实现由厂商自己实现，tomcat jetty jboss等等应运而生。

关于他如何工作的：一个http请求到来，容器将请求封装成Servlet中的request对象，在request中你可以得到所有的http信息，然后你可以取出来操作，最后你再把数据封装成Servlet的response对象，应用容器将respose对象解析之后封装成一个http response。

Web服务器习惯处理静态页面，所以需要一个程序来帮忙处理动态请求(如当前时间)。Web服务器程序会将动态请求转发给帮助程序，帮助程序处理后，返回处理后的静态结果给Web服务器程序。这样就避免了Web服务器程序处理动态页面。

# SpringMVC框架

## 简介
Spring MVC是一个基于Java的实现了MVC设计模式的请求驱动类型的轻量级Web框架，通过把Model，View，Controller分离，将web层进行职责解耦，把复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。

### SpringMVC和Servlet的区别与联系

- Servlet：性能最好，处理HTTP请求的标准和规范。

- SpringMVC：开发效率高（好多共性的东西都封装好了，是对Servlet的封装，核心的DispatcherServlet最终继承自HttpServlet）

这两者的关系，就如同MyBatis和JDBC，一个性能好，一个开发效率高，是对另一个的封装。

### Controller层示例

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @Autowired
    IUserService service;

    @RequestMapping(value = "/login.do", method = {RequestMethod.GET})
    public String login(User user) {
        User result = service.login(user);
        if (result != null) {
            return "redirect:/pages/success.html";
        } else {
            return "redirect:/pages/error.html";
        }
    }
}
```
<center>
👀开发过程肉眼可见的简介♥
</center>
