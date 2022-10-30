# Servlet

## 什么是 Servlet 

- Servlet 是 JavaEE 规范之一。规范就是接口 
- Servlet 就 JavaWeb 三大组件之一。
- 三大组件分别是：**Servlet 程序、Filter 过滤器、Listener 监听器**。 
- Servlet 是运行在服务器上的一个 java 小程序，它可以**接收客户端发送过来的请求，并响应数据给客户端**。 

## 手动实现 Servlet 程序 
①编写一个类去实现 Servlet 接口 , 实现 service 方法，处理请求，并响应数据

```java 
package com.x17.servlets;

import jakarta.servlet.*;

import java.io.IOException;

public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    //service方法是专门用来处理和请求的
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("HelloServlet被访问了！");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

② 到 web.xml 中去配置 servlet 程序的访问地址

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
    <servlet>
        <!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class 是 Servlet 程序的全类名-->
        <servlet-class>com.x17.servlets.HelloServlet</servlet-class>
    </servlet>

    <!--servlet-mapping 标签给 servlet 程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern 标签配置访问地址
        /hello 表示地址为：http://ip:port/工程路径/hello <br/>
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

## Servlet 的生命周期 

1、执行 Servlet 构造器方法 

2、执行 init 初始化方法 

第一、二步，是在第一次访问的时候创建 Servlet 程序会调用。 

3、执行 service 方法 第三步，每次访问都会调用。 

4、执行 destroy 销毁方法 ，在 web工程停止的时候调用

## GET 和 POST 请求的分发处

由于**service**方法无论是get请求还是post请求都会响应，但是**我们想要的是get请求执行一种方法，post请求可以执行另一种方法**。**ServletRequest的子类HttpServletRequest中有getMethod()方法，用于获取是哪种请求。**

```java
@Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service方法");
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
//        getMethod()能获取请求的方式
        String method = httpServletRequest.getMethod();
        if ("GET".equals(method)) {
            System.out.println("get请求");
        } else if ("POST".equals(method)) {
            System.out.println("post请求");
        }
    }
```

由于get或者post请求执行的方法肯定不止于一两句代码，可以做出优化：

```java
@Override
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
    System.out.println("service方法");
    HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
//        getMethod()能获取请求的方式
    String method = httpServletRequest.getMethod();
    if ("GET".equals(method)) {
    doGet();
    } else if ("POST".equals(method)) {
    doPost();
    }
}
/**
* 做 get 请求的操作
*/
public void doGet(){
//   方法体
}
/**
* 做 post 请求的操作
*/
public void doPost(){
//   方法体
}
```

## 通过继承 HttpServlet 实现 Servlet 程序 

**一般在实际项目开发中，都是使用继承 HttpServlet 类的方式去实现 Servlet 程序。** 

①编写一个类去继承 HttpServlet 类 ，根据业务需要重写 doGet 或 doPost 方法

```java
package com.x17.servlets;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class HelloServlet2 extends HttpServlet {
    //    doGet()在get请求时调用
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
        System.out.println("HelloServlet当中的doGet方法");
    }

    //    doPost()在Post请求时调用
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
        System.out.println("HelloServlet当中的doPost方法");
    }
}

```

②到 web.xml 中的配置 servlet 程序的访问地址

```XML
<servlet>
    <servlet-name>HelloServlet2</servlet-name>
    <servlet-class>com.x17.servlets.HelloServlet2</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>HelloServlet2</servlet-name>
    <url-pattern>/hello2</url-pattern>
</servlet-mapping>
```

## Servlet类的继承体系

Interface Servlet : Servlet接口，只负责定义Servlet程序的访问规范

Class GenericServlet：GenericServlet类实现了Servlet接口，做了很多空的实现，并持有一个ServletConfig类的引用，并对ServletConfig的使用做一些方法。

Class HttpServlet：HttpServlet继承于GenericServlet。HttpServlet抽象类实现了service()方法，并实现了请求的分发处理，并未具体写出doGet()/doPost()，负责抛异常。

自定义的Servlet程序：重写具体的doGet()/doPost()方法。

# ServletConfig类

ServletConfig类是Servlet 程序的配置信息类。

 Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建，我们负责使用。 Servlet 程序默认是第一次访问的时候创建，**ServletConfig 在每个 Servlet 程序创建时，就创建一个对应的 ServletConfig 对象**。

## ServletConfig 类的三大作用 

1、可以获取 Servlet 程序的别名 servlet-name 的值 

2、获取初始化参数 init-param 

3、获取 ServletContext

**web.xml配置**

```XML
<servlet>
     <servlet-name>HelloServlet</servlet-name>
     <servlet-class>com.x17.servlets.HelloServlet</servlet-class>
    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>username</param-name>
        <!--是参数值-->
        <param-value>root</param-value>
    </init-param>
    <!--init-param 是初始化参数-->
        <init-param>
        <!--是参数名-->
        <param-name>url</param-name>
        <!--是参数值-->
        <param-value>jdbc:mysql://localhost:3306/hello</param-value>
    </init-param>
</servlet>
```

在哪个类中使用就在哪个类的**<servlet>**中配置。ServletConfig类只能获取自己对应的初始化参数。

```java
// 1、可以获取 Servlet 程序的别名 servlet-name 的值
System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
// 2、获取初始化参数 init-param
System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username"));
System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url"));
// 3、获取 ServletContext 对象
System.out.println(servletConfig.getServletContext());
```

注意：重写public void init(ServletConfig config)方法时，一定要调用父类中的init方法，否则servletConfig.getInitParameter(String var1)将会出错（空指针异常）。

源代码：

```java

private transient ServletConfig config;
public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }
```

原因：GenericServlet类将config保存起来了。子类重写不调用父类中的init方法的话，父类保存操作就会丢失。

# ServletContext 类

## 什么是 ServletContext? 

1、ServletContext 是一个接口，它**表示 Servlet 上下文对象**。

2、一个 web 工程，只有一个 ServletContext 对象实例。 

3、ServletContext 对象是一个域对象。 

4、**ServletContext 是在 web 工程部署启动的时候创建。在 web 工程停止的时候销毁。** 

### 什么是域对象? 

域对象，是可以像 Map 一样存取数据的对象，叫域对象。 这里的**域指的是存取数据的操作范围，整个 web 工程。**

|        |     存数据     |     取数据     |     删除数据      |
| :----: | :------------: | :------------: | :---------------: |
|  Map   |     put()      |     get()      |     remove()      |
| 域对象 | setAttribute() | getAttribute() | removeAttribute() |

 

## ServletContext 类的四个作用 

1、获取 web.xml 中配置的上下文参数 context-param 

2、获取当前的工程路径，格式: /工程路径 

3、获取工程部署后在服务器硬盘上的绝对路径 

4、像 Map 一样存取数据

**web.xml配置文件**

```XML
<!--context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>
<servlet>
    <servlet-name>ContextServlet</servlet-name>
    <servlet-class>com.x17.servlets.ContextServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ContextServlet</servlet-name>
     <url-pattern>/contextServlet</url-pattern>
</servlet-mapping>
```

**java代码**

```java
//        1、获取 web.xml 中配置的上下文参数 context-param
        ServletContext context = getServletConfig().getServletContext();
/**也可直接ServletContext context = getServletContext();
原因：在GenericServlet类中
源代码：public ServletContext getServletContext() {
        return this.getServletConfig().getServletContext();
    }
*/
        String username = context.getInitParameter("username");
//        2、获取当前的工程路径，格式: /工程路径
        String contextPath = context.getContextPath();
//        3、获取工程部署后在服务器硬盘上的绝对路径
//        斜杠被服务器解析地址为:http://ip:port/工程名/ 映射到IDEA代码的web目录
        String realPath = context.getRealPath("/");
//        4、像 Map 一样存取数据
        context.setAttribute("key","nalue");
```

由于ServletContext 是在 web 工程部署启动的时候创建。在 web 工程停止的时候销毁，在setAttribute之前key拿不到，在setAttribute之后，可以一直获取，其他类中的ServletContext对象也能获取。

# http协议

### GET请求

请求行 (1) 请求的方式 GET (2) 请求的资源路径[+?+请求参数] (3) 请求的协议的版本号 HTTP/1.1

请求头 key : value 组成 不同的键值对，表示不同的含义

```
GET /javaweb1/request HTTP/1.1  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Connection: keep-alive
Cookie: JSESSIONID=9FF96178180A429BBA94B88A832A9C02
Host: localhost:8080
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36 Edg/100.0.1185.44
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="100", "Microsoft Edge";v="100"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
```

### POST请求

请求行 (1) 请求的方式 POST (2) 请求的资源路径[+?+请求参数] (3) 请求的协议的版本号 HTTP/1.1

请求头 key : value 组成 不同的键值对，表示不同的含义

```
POST /javaweb/hello HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Cache-Control: max-age=0
Connection: keep-alive
Content-Length: 0
Content-Type: application/x-www-form-urlencoded
Host: localhost:8080
Origin: http://localhost:8080
Referer: http://localhost:8080/javaweb/a.html
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36 Edg/100.0.1185.44
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="100", "Microsoft Edge";v="100"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
```



# HttpServletRequest 类 

## HttpServletRequest 类的作用 

​		每次只要有请求进入 Tomcat 服务器，Tomcat 服务器就会把请求过来的HTTP 协议信息解析好封装到 Request 对象中。然后传递到 service 方法（doGet 和 doPost）中给我们使用。**我们可以通过 HttpServletRequest 对象，获取到所有请求的信息**。 

## HttpServletRequest 类的常用方法 

方法名|说明
:-:|:-:
getRequestURI()| 获取请求的资源路径
getRequestURL() |获取请求的统一资源定位符（绝对路径）
getRemoteHost() |获取客户端的 ip 地址 
getHeader() |获取请求头 
**getParameter()** |**获取请求的参数** 
**getParameterValues()** |**获取请求的参数（多个值的时候使用）** 
**getMethod()**| **获取请求的方式 GET 或 POST** 
**setAttribute(key, value)**|**设置域数据** 
**getAttribute(key)**|**获取域数据** 
**getRequestDispatcher()**| **获取请求转发对象** 

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class RequestAPIServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        getRequestURI()获取请求的资源路径
        System.out.println("URI:" + req.getRequestURI());
//        getRequestURL()获取请求的统一资源定位符（绝对路径）
        System.out.println("URL:" + req.getRequestURL());
//        getRemoteHost()获取客户端的 ip 地址
        System.out.println("ip:" + req.getRemoteHost());
//        getHeader()获取请求头
        System.out.println("请求头：" + req.getHeader("Host"));
//        getMethod()获取请求的方式 GET 或 POST
        System.out.println("请求的方式：" + req.getMethod());
    }
}
```

## HttpServletRequest类获取请求的参数

**HTML页面**

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="parameterServlet" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="text" name="password"><br>
    兴趣爱好：<input type="checkbox" name="hobby" value="cpp">c++
    <input type="checkbox" name="hobby" value="java">java
    <input type="checkbox" name="hobby" value="js">js
    <input type="submit">
</form>
</body>
</html>
```

**java代码**

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Arrays;

public class ParameterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        getParameter()获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
//        getParameterValues()获取请求的参数（多个值的时候使用）
        String[] hobby = req.getParameterValues("hobby");
        System.out.println("username = " + username);
        System.out.println("password = " + password);
        System.out.println("hobby = " + Arrays.asList(hobby));
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("--------------------------");
//        防止post中文乱码
        req.setCharacterEncoding("utf-8");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobby = req.getParameterValues("hobby");
        System.out.println("username = " + username);
        System.out.println("password = " + password);
        System.out.println("hobby = " + Arrays.asList(hobby));
    }
}
```

## 请求的转发 

请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作叫**请求转发**。

Servlet1和Servlet2共同完整一个完整的业务功能。

**Servlet1**

```java
package com.x17.Servlet;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        获取请求的参数（办事的材料）
        String username = req.getParameter("username");
        System.out.println("Servlet1看下你的材料："+username);
//        设置域数据（给材料盖一个章，并传递到Servlet2去查看
        req.setAttribute("key1","Servlet1表示通过");
        System.out.println("Servlet通过了，去Servlet2吧");
//        问路：Servlet2怎么走
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
//        走向Servlet
        requestDispatcher.forward(req,resp);
    }
}
```

**Servlet2**

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        获取请求的参数（办事的材料）
        String username = req.getParameter("username");
        System.out.println("Servlet1看下你的材料：" + username);
//        查看域数据（Servlet是否盖章）
        Object key1 = req.getAttribute("key1");
        System.out.println("Servlet1通过了吗？" + key1);
//        处理自己的业务
        System.out.println("Servlet开始工作了");
    }
}
```

### 请求转发的特点

- 浏览器地址栏没有变化
- 他们是一次请求
- 共享Request域中的数据
- 可以转发到WEB-INF目录下（WEB-INF中的资源受保护，不被浏览器访问）
- 不可以访问工程以外的资源

# HttpServletResponse 类

​		HttpServletResponse 类和 HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个 Response 对象传递给 Servlet 程序去使用。HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息， 我们如果需要设置返回给客户端的信息，都可以通过 HttpServletResponse对象来进行设置。

## 两个输出流 

| 输出流 |       方法        |             作用             |
| :----: | :---------------: | :--------------------------: |
| 字节流 | getOutputStream() | 常用于下载（传递二进制数据） |
| 字符流 |    getWriter()    |   常用于回传字符串（常用）   |

**两个流同时只能使用一个。 使用了字节流，就不能再使用字符流，反之亦然，否则就会报错。**

## 向客户端回传数据

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.write("回传到客户端的数据");
    }
}
```

#### 解决响应的中文乱码

```java
//方案一
// 设置服务器字符集为 UTF-8
resp.setCharacterEncoding("UTF-8");
// 通过响应头，设置浏览器也使用 UTF-8 字符集
resp.setHeader("Content-Type", "text/html; charset=UTF-8");
               
//方案二
resp.setContentType("text/html; charset=UTF-8");
// 它会同时设置服务器和客户端都使用 UTF-8 字符集，还设置了响应头
// 此方法一定要在获取流对象之前调用才有效
```

## 请求重定向

​		我们的某个程序的接口因为某些原因不可使用（比如被废弃了），我们用另一个程序来替代这个接口，然后客户端再次请求我们原程序，那么我们就要返回给客户端我们新程序的地址同时使客户端访问到我们这个新地址，这个过程叫做请求重定向。

### 请求重定向的特点

- 浏览器地址栏会发生变化
- 两次请求
- 不能共享Request域中的数据
- 不能访问WEB-INF下的资源
- 可以访问工程外的资源

**Response1**

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class Response1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("浏览器先来到 Response1这个废弃程序");
/*        方式一
       设置响应状态码302，表示已废弃
        resp.setStatus(302);
       设置响应头，说明新地址的位置
        resp.setHeader("location","response2");*/
//        方式二
        resp.sendRedirect("response2");
    }
}
```

**Response2**

```java
package com.x17.Servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

public class Response2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html; charset=UTF-8");
        System.out.println("再来到Response2");
        resp.getWriter().write("这是新程序");
    }
}
```

# 会话

###   1） Http是无状态的

- HTTP 无状态 ：服务器无法判断这两次请求是同一个客户端发过来的，还是不同的客户端发过来的

- 无状无状态带来的现实问题：第一次请求是添加商品到购物车，第二次请求是结账；如果这两次请求服务器无法区分是同一个用户的，那么就会导致混乱

- 通过会话跟踪技术来解决无状态的问题。


### 2） 会话跟踪技术

-   客户端第一次发请求给服务器，服务器获取session，获取不到，则创建新的，然后响应给客户端

-    下次客户端给服务器发请求时，会把sessionID带给服务器，那么服务器就能获取到了，那么服务器就判断这一次请求和上次某次请求是同一个客户端，从而能够区分开客户端

  常用的API：
          **request.getSession() -> 获取当前的会话，没有则创建一个新的会话**
          request.getSession(true) -> 效果和不带参数相同
          request.getSession(false) -> 获取当前会话，没有则返回null，不会创建新的
          **session.getId() -> 获取sessionID**
          **session.isNew( ）-> 判断当前session是否是新的**
          session.getMaxInactiveInterval() -> session的非激活间隔时长，默认1800秒
          session.setMaxInactiveInterval()
          session.invalidate() -> 强制性让会话立即失效

###  3） session保存作用域

​      session保存作用域是和具体的某一个session对应的
​      常用的API：
​        void session.setAttribute(k,v)
​        Object session.getAttribute(k)
​        void removeAttribute(k)

```java
public class Demo01Session extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //得到session
        HttpSession session = req.getSession();
        //向session中存入key，value
        session.setAttribute("name","tom");
        //得到sessionID
        String id = session.getId();
        System.out.println(id);
        //得到session中的value
        Object name = session.getAttribute("name");
        System.out.println(name);
    }
}
```

### 4）cookie

#### 如何创建 Cooki

```java
protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //1 创建 Cookie 对象
    Cookie cookie = new Cookie("key4", "value4");
    //2 通知客户端保存 Cookie
    resp.addCookie(cookie);
    //1 创建 Cookie 对象
    ookie cookie1 = new Cookie("key5", "value5");
    //2 通知客户端保存 Cookie
    resp.addCookie(cookie1);
    resp.getWriter().write("Cookie 创建成功");
}
```

#### 服务器如何获取 Cookie

工具类

```java
public class CookieUtils {
    /**
     * 查找指定名称的 Cookie 对象
     *
     * @param name
     * @param cookies
     * @return
     */
    public static Cookie findCookie(String name, Cookie[] cookies) {
        if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                return cookie
            }
        }
        return null;
    }
}
```

Servlet

```java
protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Cookie[] cookies = req.getCookies();
    for (Cookie cookie : cookies) {
        // getName 方法返回 Cookie 的 key（名）
        // getValue 方法返回 Cookie 的 value 值
        resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
    }
    Cookie iWantCookie = CookieUtils.findCookie("key1", cookies);
    // for (Cookie cookie : cookies) {
        // if ("key2".equals(cookie.getName())) {
        // iWantCookie = cookie;
        // break;
        // }
    // }
    // 如果不等于 null，说明赋过值，也就是找到了需要的 Cookie
    if (iWantCookie != null) {
    	resp.getWriter().write("找到了需要的 Cookie");
	}
}
```

#### Cookie 值的修改

```java
// 方案一：
// 1、先创建一个要修改的同名的 Cookie 对象
// 2、在构造器，同时赋于新的 Cookie 值。
Cookie cookie = new Cookie("key1", "newValue1");
// 3、调用 response.addCookie( Cookie ); 通知 客户端 保存修改
resp.addCookie(cookie);
// 方案二：
// 1、先查找到需要修改的 Cookie 对象
Cookie cookie = CookieUtils.findCookie("key2", req.getCookies());
if (cookie != null) {
// 2、调用 setValue()方法赋于新的 Cookie 值。
	cookie.setValue("newValue2");
// 3、调用 response.addCookie()通知客户端保存修改
	resp.addCookie(cookie);
}
```

#### Cookie 生命控制 

Cookie 的生命控制指的是如何管理 Cookie 什么时候被销毁（删除） 

setMaxAge() 正数，表示在指定的秒数后过期 负数，表示浏览器一关，Cookie 就会被删除（默认值是-1） 零，表示马上删除 Cookie

```java
 /**
     * 设置存活 1 个小时的 Cooie
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        Cookie cookie = new Cookie("life3600", "life3600");
        cookie.setMaxAge(60 * 60); // 设置 Cookie 一小时之后被删除。无效
        resp.addCookie(cookie);
        resp.getWriter().write("已经创建了一个存活一小时的 Cookie");
    }

    /**
     * 马上删除一个 Cookie
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
// 先找到你要删除的 Cookie 对象
        Cookie cookie = CookieUtils.findCookie("key4", req.getCookies());
        if (cookie != null) {
// 调用 setMaxAge(0);
            cookie.setMaxAge(0); // 表示马上删除，都不需要等待浏览器关闭
// 调用 response.addCookie(cookie);
            resp.addCookie(cookie);
            resp.getWriter().write("key4 的 Cookie 已经被删除");
        }
    }

    /**
     * 默认的会话级别的 Cookie
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
            IOException {
        Cookie cookie = new Cookie("defalutLife", "defaultLife");
        cookie.setMaxAge(-1);//设置存活时间
        resp.addCookie(cookie);
    }
```

# JSP

​		jsp 的全换是 java server pages。Java 的服务器页面。 **jsp 的主要作用是代替 Servlet 程序回传 html 页面的数据**。 因为 Servlet 程序回传 html 页面数据是一件非常繁锁的事情。开发成本和维护成本都极高。

## jsp 的本质

​		 **jsp 页面本质上是一个 Servlet 程序**。 当我们第一次访问 jsp 页面的时候。Tomcat 服务器会帮我们把 jsp 页面翻译成为一个 java 源文件。并且对它进行编译成 为.class 字节码程序。**其底层实现，也是通过输出流。把 html 页面数据回传 给客户端**

## jsp 的三种语法

### 头部的 page 指令 

jsp 的 page 指令可以修改 jsp 页面中一些重要的属性，或者行为。

```jsp
 <%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

- language 属性 ：表示 jsp 翻译后是什么语言文件。暂时只支持 java。 
- contentType 属性 ：表示 jsp 返回的数据类型是什么。也是源码中 response.setContentType()参数值 
- pageEncoding 属性 ：表示当前 jsp 页面文件本身的字符集。 
- import 属性 ：跟 java 源代码中一样。用于导包，导类。

 ========================以下两个属性是给 out 输出流使用============================= 

- autoFlush 属性： 设置当 out 输出流缓冲区满了之后，是否自动刷新冲级区。默认值是 true。
- buffer 属性： 设置 out 缓冲区的大小。默认是 8kb



- errorPage 属性： 设置当 jsp 页面运行时出错，自动跳转去的错误页面路径。
- isErrorPage 属性 ：设置当前 jsp 页面是否是错误信息页面。默认是 false。如果是 true ，可以获取异常信息。
- session 属性 ：设置访问当前 jsp 页面，是否会创建 HttpSession 对象。默认是 true。
- extends 属性： 设置 jsp 翻译出来的 java 类默认继承谁

### jsp 中的常用脚本

#### 声明脚本(极少使用) 

```jsp
 <%! 声明 java 代码 %> 
```

作用：可以给 jsp 翻译出来的 java 类定义属性和方法甚至是静态代码块。内部类等。

```jsp
<%--1、声明类属性--%>
<%!
    private Integer id;
    private String name;
    private static Map<String,Object> map;
%>
<%--2、声明 static 静态代码块--%>
<%!
    static {
        map = new HashMap<String,Object>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
    }
%>
<%--3、声明类方法--%>
<%!
    public int abc(){
        return 12;
    }
%>
<%--4、声明内部类--%>
<%!
    public static class A {
        private Integer id = 12;
        private String abc = "abc";
    }
%>
```

#### 表达式脚本（常用）

```jsp
<%=表达式 % > 
```

作用：在 jsp 页面上输出数据。

 表达式脚本的特点：

- 所有的表达式脚本都会被翻译到 _jspService() 方法中 
- 表达式脚本都会被翻译成为 out.print()输出到页面上
- 由于表达式脚本翻译的内容都在 _jspService() 方法中,所以 _jspService()方法中的对象都可以直接使用。 
- **表达式脚本中的表达式不能以分号结束。**

#### 代码脚本 

```jsp
 <% java 语句 %>
```

作用：可以在 jsp 页面中，编写我们自己需要的功能（写的是 java 语句）。

代码脚本的特点： 

- 代码脚本翻译之后都在 _jspService 方法中 

- 代码脚本由于翻译到 _jspService()方法中，所以在 _jspService()方法中的现有对象都可以直接使用。
- **还可以由多个代码脚本块组合完成一个完整的 java 语句**。
- 代码脚本还可以和表达式脚本一起组合使用，在 jsp页面上输出数据

```jsp
<%--2.代码脚本----if语句--%>
<%
    int i = 13 ;
    if (i == 12) {
%>
    <h1>你好</h1>
<%
    } else {
%>
    <h1>hello</h1>
<%
	}
%>
<br>
<%--2.代码脚本----for循环语句--%>
    <table border="1" cellspacing="0">
<%
   for (int j = 0; j < 10; j++) {
%>
    <tr>
    <td>第 <%=j + 1%>行</td>
    </tr>
<%
	}
%>
	</table>
<%--3.翻译后java文件中_jspService方法内的代码都可以写--%>
<%
    String username = request.getParameter("username");
    System.out.println("用户名的请求参数值是：" + username)
%>
```

## jsp 四大域对象

 域对象是可以像 Map 一样存取数据的对象。四个域对象功能一样。不同的是它们对数据的存取范围
域对象|数据存取范围
-|-
pageContext (PageContextImpl 类)| 当前 jsp 页面范围内有效 
request (HttpServletRequest 类)| 一次请求内有效 
session (HttpSession 类)| 一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器）
application (ServletContext 类) |整个 web 工程范围内都有效（只要 web 工程不停止，数据都在）

 虽然四个域对象都可以存取数据。在使用上它们是有优先顺序的。 

pageContext ====>>> request ====>>> session ====>>> application

## jsp 的常用标签

### 静态包含

```jsp
<%@ include file="路径"%>
<%-- file属性指定你要包含的jsp页面的路径
地址中第一个斜杠 / 表示为 http://ip:port/工程路径/ 映射到代码的 web 目录
静态包含的特点：
1、静态包含不会翻译被包含的jsp页面。
2、静态包含其实是把被包含的jsp页面的代码拷贝到包含的位置执行输出。
--%>
```

### 动态包含 

```jsp
<jsp:include page=""></jsp:include>
```

动态包含的特点： 

1、动态包含会把包含的 jsp 页面也翻译成为 java 代码 

2、动态包含底层代码使用如下代码去调用被包含的 jsp 页面执行输出。 

​	JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false); 

3、动态包含，还可以传递参数

### 请求转发

```jsp
<jsp:forward page=""></jsp:forward>
```

## Listener监听器

### ServletContextListener 监听器 

ServletContextListener 可以监听 ServletContext 对象的创建和销毁。

ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。

监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。

```java
/**
* 在 ServletContext 对象创建之后马上调用，做初始化
*/
public void contextInitialized(ServletContextEvent sce);
/**
* 在 ServletContext 对象销毁之后调用
*/
public void contextDestroyed(ServletContextEvent sce)
```

使用步骤如下： 

1、编写一个类去实现 ServletContextListener

 2、实现其两个回调方法

3、到 web.xml 中去配置监听器

**监听器实现类：**

```java
public class MyServletContextListenerImpl implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被创建了");
	}
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被销毁了");
    }
}
```

**web.xml 中的配置：**

```xml
<!--配置监听器-->
<listener>
<listener-class>MyServletContextListenerImpl</listener-class>
</listener>
```

# EL表达式

EL 表达式的全称是：Expression Language，是表达式语言。

作用：EL 表达式主要是代替 jsp 页面中的表达式脚本在 jsp 页面中进行数据的输出。 因为 EL 表达式在输出数据的时候，要比 jsp 的表达式脚本要简洁很多

EL 表达式的格式是：${表达式} 

EL 表达式在输出 null 值的时候，输出的是空串。jsp 表达式脚本输出 null 值的时候，输出null

**EL表达式和jsp对比**

```jsp
<body>
<%
request.setAttribute("key","值");
%>
表达式脚本输出 key 的值是：
<%=request.getAttribute("key1")==null?"":request.getAttribute("key1")%><br/>
EL 表达式输出 key 的值是：${key1}
</body>
```

## EL 表达式输出 JavaBean 的普通属性

Person 类

```java
public class Person {
    private String name;
    private String[] phones;
    private List<String> cities;
    private Map<String,Object> map;
    public int getAge() {
    return 18;
}
```

jsp页面

```jsp
<body>
<%
    Person person = new Person();
    person.setName("张三！");
    person.setPhones(new String[]{"123456789123","18888886666","19999998888"});
    List<String> cities = new ArrayList<String>();
    cities.add("北京");
    cities.add("上海");
    cities.add("深圳");
    person.setCities(cities);
    Map<String,Object>map = new HashMap<>();
    map.put("key1","value1");
    map.put("key2","value2");
    map.put("key3","value3");
    person.setMap(map);
    pageContext.setAttribute("p", person);
%>
    输出 Person：${ p }<br/>
    输出 Person 的 name 属性：${p.name} <br>
    输出 Person 的 pnones 数组属性值：${p.phones[2]} <br>
    输出 Person 的 cities 集合中的元素值：${p.cities} <br>
    输出 Person 的 List 集合中个别元素值：${p.cities[2]} <br>
    输出 Person 的 Map 集合: ${p.map} <br>
    输出 Person 的 Map 集合中某个 key 的值: ${p.map.key3} <br>
    输出 Person 的 age 属性：${p.age} <br>
    <!--找的是get方法而不是属性 -->
</body>
```

## EL 表达式的 11 个隐含对象 

 变量| 类型 |作用 
-|-|-
pageContext |PageContextImpl |它可以获取 jsp 中的九大内置对象
 pageScope| Map |它可以获取 pageContext 域中的数据 
requestScope| Map |它可以获取 Request 域中的数据 
sessionScope |Map| 它可以获取 Session 域中的数据
 applicationScope |Map |它可以获取 ServletContext 域中的数据
 param |Map| 它可以获取请求参数的值
 paramValues| Map |它也可以获取请求参数的值，获取多个值的时候使用
 header| Map| 它可以获取请求头的信息
 headerValues |Map| 它可以获取请求头的信息，它可以获取多个值的情况
 cookie| Map| 它可以获取当前请求的 Cookie 信息

# JSTL 标签库

 JSTL 标签库， 全称是指 JSP Standard Tag Library， JSP 标准标签库。是一个不断完善的开放源代码的 JSP 标 签库。 

EL 表达式主要是为了替换 jsp 中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个 jsp 页面 变得更佳简洁。

 JSTL 由五个不同功能的标签库组成。 

功能范围 |URI |前缀
-|-|-
核心标签库--重点 |http://java.sun.com/jsp/jstl/core |c 
格式化| http://java.sun.com/jsp/jstl/fmt |fmt
函数| http://java.sun.com/jsp/jstl/functions |fn 
数据库(不使用) |http://java.sun.com/jsp/jstl/sql |sql 
XML(不使用) |http://java.sun.com/jsp/jstl/xml| x

## core 核心库使用

**<c:set />（使用很少）** 

作用：set 标签可以往域中保存数据

```jsp
 <%-- 
    作用：set 标签可以往域中保存数据 
    域对象.setAttribute(key,value);
	scope 属性设置保存到哪个域 
        page 表示 PageContext 域（默认值）
        request 表示 Request 域 
        session 表示 Session 域 
        application 表示 ServletContext 域 
    var 属性设置 key 是多少 value 属性设置值
 --%>
保存之前：${ sessionScope.abc } 
<c:set scope="session" var="abc" value="abcValue"/>
保存之后：${ sessionScope.abc }
```

 **<c:if />**

if 标签用来做 if 判断

```jsp
<%--
    <c:if />
    if 标签用来做 if 判断。
    test 属性表示判断的条件（使用 EL 表达式输出）
--%>
<c:if test="${ 12 == 12 }">
<h1>12 等于 12</h1>
</c:if>
<c:if test="${ 12 != 12 }">
<h1>12 不等于 12</h1>
</c:if>
```

**<c:choose > < c:when > < c:otherwise >**

```jsp
<%--
    作用：多路判断。跟 switch ... case .... default 非常接近
    choose 标签开始选择判断
    when 标签表示每一种判断情况
    test 属性表示当前这种判断情况的值
    otherwise 标签表示剩下的情况
    <c:choose> <c:when> <c:otherwise>标签使用时需要注意的点：
    1、标签里不能使用 html 注释，要使用 jsp 注释
    2、when 标签的父标签一定要是 choose 标签
--%>
<%
	request.setAttribute("height", 180);
%>
<c:choose>
    <%-- 这是 html 注释 --%>
    <c:when test="${ requestScope.height > 190 }">
    <h2>小巨人</h2>
    </c:when>
    <c:when test="${ requestScope.height > 180 }">
    <h2>很高</h2>	
    </c:when>
    <c:when test="${ requestScope.height > 170 }">
    <h2>还可以</h2>
    </c:when>
    <c:otherwise>
        <c:choose>
            <c:when test="${requestScope.height > 160}">
            <h3>大于 160</h3>
            </c:when>
            <c:when test="${requestScope.height > 150}">
            <h3>大于 150</h3>
            </c:when>
            <c:when test="${requestScope.height > 140}">
            <h3>大于 140</h3>
            </c:when>
            <c:otherwise>
            其他小于 140
            </c:otherwise>
        </c:choose>
	</c:otherwise>
</c:choose>
```

**<c:forEach />**

```jsp
<%--
    遍历 1 到 10，输出
    begin 属性设置开始的索引
    end 属性设置结束的索引
    var 属性表示循环的变量(也是当前正在遍历到的数据)
--%>
<table border="1">
    <c:forEach begin="1" end="10" var="i">
    <tr>
    	<td>第${i}行</td>
    </tr>
    </c:forEach>
</table>
```

# 文件的上传和下载

## 文件的上传

1. 要有一个 form 标签，method=post 请求 
2. form 标签的 encType 属性值必须为 **multipart/form-data** 值
3. 在 form 标签中使用 input type=file 添加上传的文件
4. 编写服务器代码（Servlet 程序）接收，处理上传的数据。

encType=multipart/form-data 表示提交的数据，以多段（每一个表单项一个数据段）的形式进行拼接，然后以二进制流的形式发送给服务器

### commons-fileupload.jar 常用 API 介绍说明 

commons-fileupload.jar 需要依赖 commons-io.jar 这个包，所以两个包我们都要引入。

```java
ServletFileUpload 类，用于解析上传的数据。

FileItem 类，表示每一个表单项。

boolean ServletFileUpload.isMultipartContent(HttpServletRequest request); 判断当前上传的数据格式是否是多段的格式。 

public List parseRequest(HttpServletRequest request) 解析上传的数据

boolean FileItem.isFormField() 判断当前这个表单项，是否是普通的表单项。还是上传的文件类型。 

true 表示普通类型的表单项 

false 表示上传的文件类型

String FileItem.getFieldName() 获取表单项的 name 属性值

String FileItem.getString() 获取当前表单项的值。 

String FileItem.getName(); 获取上传的文件名

void FileItem.write( file ); 将上传的文件写到 参数 file 所指向抽硬盘位
```

示例

```java
package com.x17.jsp;

import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.util.List;

public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) {

        //先判断上传的数据是否是多段数据（只有是多段的数据，才是文件上传的）
        if (ServletFileUpload.isMultipartContent(req)) {
            //            创建FileItemFactory工厂实现类
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            //            创建用于解析上传数据的工具类ServletFileUpload类
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                //            解析上传的数据，得到每一个表单项FileItem
                List<FileItem> list = servletFileUpload.parseRequest(req);
                //            循环判断每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : list) {
                    if (fileItem.isFormField()) {
                        //                    普通表单项
                        System.out.println("表单项的name属性值为" + fileItem.getFieldName());
                        System.out.println("表单项的Value属性值" + fileItem.getString("UTF-8"));
                    } else {
                        //                    上传的文件
                        System.out.println("表单项的name属性值为" + fileItem.getFieldName());
                        System.out.println("上传的文件名" + fileItem.getName());
                        fileItem.write(new File("d:\\" + fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 文件下载

下载的常用 API 说明：

 response.getOutputStream(); 

servletContext.getResourceAsStream(); 

servletContext.getMimeType();

 response.setContentType();

 response.setHeader("Content-Disposition", "attachment; fileName=1.jpg"); 

​		这个响应头告诉浏览器。这是需要下载的。而 attachment 表示附件，也就是下载的一个文件。fileName=后面， 表示下载的文件名。 完成上面的两个步骤，下载文件是没问题了。

但是如果我们要下载的文件是中文名的话。你会发现，下载无法正确 显示出正确的中文名。 

原因是在响应头中，不能包含有中文字符，只能包含 ASCII码。

示例

```java
package com.x17.jsp;

import org.apache.commons.io.IOUtils;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;


public class DownloadServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        1.获取要下载的文件名
        String downloadFilename = "a.jpg";
//        2.读取要下载的文件内容（通过ServletContext对象可以读取）
        /*
          /斜杠被服务器解析表示地址为http://ip:port/工程名/  映射到web目录
         */
        InputStream resourceAsStream = getServletContext().getResourceAsStream("/file/" + downloadFilename);
        //获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
//        3.在回传前，通过响应头告诉客户端返回的数据类型
        ServletContext servletContext = getServletContext();
        //获取要下载的文件类型
        String mimeType = getServletContext().getMimeType("/file/" + downloadFilename);
        resp.setContentType(mimeType);
        System.out.println(mimeType);
//        4.在回传前，通过响应头告诉客户端返回的数据类型用于下载使用
        /*
        Content-Disposition响应头：表示收到的数据怎么处理
        attachment表示附件，表示下载使用
        filename= 表示指定的下载名
        * */
        resp.setHeader("Content-Disposition", "attachment;filename=" + downloadFilename);
//        5.把下载的文件内容回传到客户端
        //读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);

    }
}
```

