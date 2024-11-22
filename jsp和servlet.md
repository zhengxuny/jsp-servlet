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
通过上述Markdown排版的复习资料，可以更清晰地理解Servlet的创建与配置过程。在复习时，可以重点关注两个注册Servlet的方法，并理解其背后的原理。

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTc3NjE1MDc0LC0yMDg4NzQ2NjEyLDQzOT
c1NjU1MV19
-->