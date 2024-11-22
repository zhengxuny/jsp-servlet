# 第1章：Servlet的创建与配置
## 【重点】创建Servlet的过程
### 1. 通过web.xml注册Servlet
#### 步骤1：创建Servlet类
首先，创建一个Java类并继承自`HttpServlet`类。
```java
public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 处理GET请求的逻辑
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 处理POST请求的逻辑
    }
}
```
#### 步骤2：在web.xml中注册Servlet
然后，在`web.xml`配置文件中注册该Servlet。
```xml
<servlet>
    <!-- Servlet的名称，通常与类名一致 -->
    <servlet-name>Servlet1</servlet-name>
    <!-- Servlet的完整类路径 -->
    <servlet-class>com.niit.Servlet1</servlet-class>
</servlet>
<!-- Servlet的URL映射 -->
<servlet-mapping>
    <!-- 映射的Servlet名称 -->
    <servlet-name>Servlet1</servlet-name>
    <!-- Servlet对应的URL -->
    <url-pattern>/s1</url-pattern>
</servlet-mapping>
```
### 2. 使用@WebServlet注解
可以使用`@WebServlet`注解来简化Servlet的注册过程。
```java
@WebServlet("/s2")
public class Servlet2 extends HttpServlet {
    // 类的实现
}
```
或者指定多个URL模式：
```java
@WebServlet(urlPatterns = { "/s2" })
public class Servlet2 extends HttpServlet {
    // 类的实现
}
```
## 示例代码
以下是一个使用`@WebServlet`注解的Servlet示例：
```java
@WebServlet("/s3")
public class Servlet3 extends HttpServlet {
    private static final long serialVersionUID = 1L;
    public Servlet3() {
        // 构造函数
    }
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 设置响应内容类型
        response.setContentType("text/html;charset=utf-8");
        // 获取输出流
        PrintWriter writer = response.getWriter();
        // 向客户端输出数据
        writer.print("<h1>Hello, Servlet3! 你好, Servlet3!</h1>");
        // 关闭输出流
        writer.close();
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 调用doGet方法处理POST请求
        doGet(request, response);
    }
}
```
 
 # 第2章：Servlet的生命周期与配置
## Servlet生命周期
Servlet的生命周期主要包括以下几个阶段：
### 1. 初始化（init()）
- **描述**：Servlet容器在加载Servlet类之后，只会调用一次`init()`方法。
- **用途**：用于完成初始化操作，如加载配置文件、建立数据库连接等。
### 2. 服务（service()）
- **描述**：每次客户端请求到达时，Servlet容器都会调用`service()`方法。
- **用途**：处理客户端请求，如处理HTTP GET、POST请求等。
### 3. 销毁（destroy()）
- **描述**：在删除Servlet实例之前，Servlet容器会调用`destroy()`方法。
- **用途**：用于释放资源，如关闭数据库连接、释放内存等。
## 获取URL参数值
在Servlet中，可以通过`HttpServletRequest`对象获取URL中的参数值：
```java
String parameterValue = request.getParameter("parameterName");
```
## 重定向方法
重定向客户端请求到另一个URL或Servlet，使用`HttpServletResponse`对象的`sendRedirect()`方法：
```java
response.sendRedirect("newURL");
```
## web.xml参数的配置和使用
### 1. 配置Servlet初始化参数
在`web.xml`中为Servlet配置初始化参数：
```xml
<servlet>
    <servlet-name>ServletTest2</servlet-name>
    <servlet-class>com.niit.ServletTest2</servlet-class>
    <!-- 定义当前Servlet的初始化参数 -->
    <init-param>
        <param-name>name</param-name>
        <param-value>张三 zhangsan</param-value>
    </init-param>
</servlet>
```
在Servlet中获取初始化参数：
```java
String name = getServletConfig().getInitParameter("name");
```
### 2. 配置上下文参数
在`web.xml`中为整个Web应用配置上下文参数：
```xml
<context-param>
    <param-name>title</param-name>
    <param-value>支付宝V2.0</param-value>
</context-param>
```
在Servlet中获取上下文参数：
```java
String title = getServletContext().getInitParameter("title");
```
### 3. Servlet映射
将Servlet映射到URL：
```xml
<servlet-mapping>
    <servlet-name>ServletTest2</servlet-name>
    <url-pattern>/ServletTest2</url-pattern>
</servlet-mapping>
```
通过上述Markdown排版的复习资料，可以清晰地了解Servlet的生命周期、参数获取、重定向以及`web.xml`的配置方法。在复习时，重点关注Servlet生命周期的三个阶段以及如何配置和使用初始化参数。




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MTA3NDY3OTksLTg5NTc4MjkyMywyMT
A2OTQyOTZdfQ==
-->