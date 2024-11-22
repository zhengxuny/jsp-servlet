
### 第1章：创建Servlet的过程
#### 1. 在web.xml中注册Servlet
这个过程分为两步：
**（1）创建Servlet类**
- 首先，你需要创建一个Java类，这个类需要继承自`HttpServlet`类。
- 在你的类中，你需要重写`doGet`和`doPost`方法。这两个方法是用来处理客户端发送的GET和POST请求的。
示例代码：
```java
public class Servlet1 extends HttpServlet {
    // 类的实现
}
```
**（2）在web.xml中注册Servlet**
- 在`web.xml`文件中，你需要配置两个主要的标签：`<servlet>`和`<servlet-mapping>`。
- `<servlet>`标签用于定义Servlet的名称和全路径。
- `<servlet-name>`标签内的内容是Servlet的名称，这个名称可以是任意的，但通常与Servlet的类名保持一致。
- `<servlet-class>`标签用于指定Servlet类的全限定名（包括包名）。
示例配置：
```xml
<servlet>
    <servlet-name>Servlet1</servlet-name>
    <servlet-class>com.niit.Servlet1</servlet-class>
</servlet>
```
- `<servlet-mapping>`标签用于配置Servlet的URL映射。
- `<servlet-name>`标签用于指定需要映射的Servlet名称。
- `<url-pattern>`标签用于指定访问该Servlet的URL路径。
示例配置：
```xml
<servlet-mapping>
    <servlet-name>Servlet1</servlet-name>
    <url-pattern>/s1</url-pattern>
</servlet-mapping>
```
这样配置后，当用户访问`/s1`路径时，就会调用`Servlet1`这个Servlet。
#### 2. 使用@WebServlet注解
- 从Servlet 3.0开始，你可以使用`@WebServlet`注解来简化Servlet的注册过程。
- 你可以直接在Servlet类上使用`@WebServlet`注解，并指定URL映射。
示例代码：
```java
@WebServlet("/s2")
public class Servlet2 extends HttpServlet {
    // 类的实现
}
```
或者你也可以这样写：
```java
@WebServlet(urlPatterns = { "/s2" })
public class Servlet2 extends HttpServlet {
    // 类的实现
}
```
#### 代码示例：
下面是一个使用`@WebServlet`注解的Servlet类示例：
```java
@WebServlet("/s3")
public class Servlet3 extends HttpServlet {
    private static final long serialVersionUID = 1L;
    public Servlet3() {
        // 构造方法
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 设置响应内容类型和字符集
        response.setContentType("text/html;charset=utf-8");
        // 获取PrintWriter对象，用于向客户端发送字符数据
        PrintWriter writer = response.getWriter();
        // 向客户端发送HTML内容
        writer.print("<h1>Hello, Servlet3! 你好, Servlet3!</h1>");
        // 关闭PrintWriter对象
        writer.close();
    }
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 在这个示例中，POST请求的处理和GET请求相同
        doGet(request, response);
    }
}
```
这段代码定义了一个名为`Servlet3`的Servlet，它通过`@WebServlet("/s3")`注解映射到`/s3`这个URL。在`doGet`方法中，它设置了响应的内容类型为`text/html`，并使用`PrintWriter`对象向客户端发送了一个简单的HTML页面。`doPost`方法调用了`doGet`方法，这意味着POST请求和GET请求将得到相同的处理。



### 第2章：Servlet基础
#### 一、Servlet生命周期
1. 初始化阶段（init()方法）
   - 调用时机：Servlet容器加载Servlet类并创建其实例后，将调用init()方法进行初始化。
   - 特点：init()方法在整个生命周期中只会被调用一次。
   - 作用：用于完成一些初始化操作，如加载配置文件、建立数据库连接等。
2. 处理请求阶段（service()方法）
   - 调用时机：每次客户端请求到达Servlet时，容器都会调用service()方法。
   - 特点：service()方法可以被多次调用。
   - 作用：根据请求类型（如GET、POST等）处理客户端请求，并生成响应。
3. 销毁阶段（destroy()方法）
   - 调用时机：在删除Servlet实例前，容器会调用destroy()方法。
   - 作用：用于释放资源，如关闭数据库连接、清理临时文件等。
#### 二、获取URL中的参数值
- 方法：使用HttpServletRequest对象的getParameter()方法。
- 示例：String paramName = request.getParameter("paramName");
#### 三、重定向方法
- 方法：使用HttpServletResponse对象的sendRedirect()方法。
- 示例：response.sendRedirect("newURL");
#### 四、web.xml参数配置和使用
1. 配置Servlet
```xml
<!-- 配置Servlet -->
<servlet>
    <servlet-name>ServletTest2</servlet-name>
    <servlet-class>com.niit.ServletTest2</servlet-class>
    <!-- 定义当前Servlet的初始化参数，作用范围是当前的ServletTest2 -->
    <init-param>
        <param-name>name</param-name>
        <param-value>张三</param-value>
    </init-param>
</servlet>
```
2. 定义上下文参数
```xml
<!-- 定义上下文参数，作用范围是整个项目 -->
<context-param>
    <param-name>title</param-name>
    <param-value>支付宝V2.0</param-value>
</context-param>
```
3. Servlet映射
```xml
<servlet-mapping>
    <servlet-name>ServletTest2</servlet-name>
    <url-pattern>/ServletTest2</url-pattern>
</servlet-mapping>
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbNDM5NzU2NTUxXX0=
-->