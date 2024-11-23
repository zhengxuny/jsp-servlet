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
//@WebServlet(name = "s3", urlPatterns = {"/s3"})
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
 ## 补充知识：Java的封装、继承、多态
Java作为一门面向对象的编程语言，其核心特性包括封装、继承和多态。以下将分别介绍这三个特性。
## #封装
封装是面向对象编程的基本原则之一，它意味着将对象的实现细节隐藏起来，只对外暴露有限的接口。
#### 优点
- **安全性**：防止外部直接访问对象内部数据，提高数据安全性。
- **简化性**：用户无需关心对象内部复杂实现，只需通过暴露的接口进行操作。
- **可维护性**：当内部实现需要修改时，只要接口保持不变，外部调用者无需修改代码。
#### 实现
在Java中，可以使用以下方式实现封装：
- **访问修饰符**：public、protected、private和默认（没有修饰符），用于控制类、属性和方法的访问级别。
- **getter和setter方法**：用于获取和设置对象属性。
```java
public class Person {
    private String name;  // 私有属性
    public String getName() {  // 公共方法，获取name属性
        return name;
    }
    public void setName(String name) {  // 公共方法，设置name属性
        this.name = name;
    }
}
```
### 继承
继承是面向对象编程的另一个基本原则，它允许子类继承父类的属性和方法，实现代码的复用。
#### 优点
- **代码复用**：子类可以继承父类的属性和方法，避免重复编写代码。
- **扩展性**：子类可以在父类的基础上添加新的属性和方法。
#### 实现
在Java中，使用`extends`关键字实现继承。
```java
public class Animal {
    public void eat() {
        System.out.println("动物吃东西");
    }
}
public class Dog extends Animal {
    public void bark() {
        System.out.println("狗叫");
    }
}
```
### 多态
多态是指同一个行为具有多个不同表现形式或形态的能力。在Java中，多态通常是指同一方法在不同类型的对象上表现出不同的行为。
#### 优点
- **可扩展性**：可以在不修改原有代码的基础上，增加新的功能。
- **可替换性**：同一接口的不同实现可以相互替换。
#### 实现
在Java中，多态通常通过以下方式实现：
- **方法重写**：子类重写父类的方法。
- **对象造型**：父类引用指向子类对象。
```java
public class Animal {
    public void sound() {
        System.out.println("动物叫声");
    }
}
public class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("汪汪汪");
    }
}
public class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("喵喵喵");
    }
}
public class Test {
    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();
        
        dog.sound();  // 输出：汪汪汪
        cat.sound();  // 输出：喵喵喵
    }
}
```
通过以上示例，我们可以看到，虽然`dog`和`cat`都是`Animal`类型，但它们调用`sound`方法时，却表现出不同的行为，这就是多态的体现。

 
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



# 第3章：会话管理技术
会话管理是Web应用中的一项重要技术，它允许服务器跟踪用户的状态和信息。以下是几种常见的会话管理技术：
## 1. 隐藏表单域
隐藏表单域是一种简单的会话管理技术，它通过在HTML表单中插入一个不可见的表单字段来传递信息。
```html
<input type="hidden" name="hiddenField" value="value">
```
## 2. URL重写
URL重写是在URL中加入额外的参数来维持会话状态的方法。这些参数通常包含在查询字符串中。
```java
response.encodeRedirectURL("nextPage.jsp?param=value");
```
## 3. Cookie
Cookie是一种存储在客户端计算机上的小型文本文件，用于记录用户的信息。
### Cookie操作
- **添加Cookie**：
```java
Cookie cookie = new Cookie("name", "value");
response.addCookie(cookie);
```
- **获取Cookie**：
```java
Cookie[] cookies = request.getCookies();
if (cookies != null) {
    for (Cookie cookie : cookies) {
        if (cookie.getName().equals("name")) {
            // 处理cookie
        }
    }
}
```
- **修改Cookie**：
```java
for (Cookie cookie : cookies) {
    if (cookie.getName().equals("name")) {
        cookie.setValue("newValue");
        response.addCookie(cookie);
        break;
    }
}
```
- **删除Cookie**：
通常通过设置Cookie的过期时间为过去的时间来删除Cookie。
```java
cookie.setMaxAge(0);
response.addCookie(cookie);
```
## 4. Session（会话）
Session是一种服务器端的会话管理技术，它允许服务器在多个请求之间存储和检索用户信息。
### Session操作
- **设置Session属性**：
```java
session.setAttribute("key", "value");
```
- **获取Session属性**：
```java
Object value = session.getAttribute("key");
```
- **删除Session属性**：
```java
session.removeAttribute("key");
```
- **销毁Session**：
```java
session.invalidate();
```
# 第4章：请求转发与过滤器
## 请求转发（Forward）
请求转发是服务器端的一种操作，它允许Servlet将请求转发到另一个资源（如另一个Servlet、JSP页面或HTML文件）进行处理。
### 请求分发器（RequestDispatcher）
请求分发器是Servlet API的一部分，它提供了以下两个主要方法：
1. **include()**：将其他Servlet的内容包含到当前Servlet的响应中。
```java
RequestDispatcher dispatcher = request.getRequestDispatcher("otherServlet");
dispatcher.include(request, response);
```
2. **forward()**：将当前请求转发给其他Servlet。
```java
RequestDispatcher dispatcher = request.getRequestDispatcher("otherServlet");
dispatcher.forward(request, response);
```
## 过滤器（Filter）
过滤器是Servlet技术中用于拦截请求和响应的对象，可以在请求到达目标资源之前或响应返回客户端之前进行预处理。
### 实现过滤器
1. **实现Filter接口**：
```java
public class FilterA implements Filter {
    // ...
}
```
2. **重写三个方法**：
- **init()**：过滤器初始化时调用。
- **doFilter()**：执行过滤逻辑，通过`chain.doFilter(request, response)`来控制请求是否被放行。
- **destroy()**：过滤器被销毁时调用。
```java
@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
    throws IOException, ServletException {
    // 过滤逻辑
    chain.doFilter(request, response); // 继续执行后续过滤器或目标资源
}
```
3. **配置过滤器**：
在`web.xml`中注册过滤器并指定其拦截的URL。
```xml
<!-- 注册过滤器 -->
<filter>
    <filter-name>FilterA</filter-name>
    <filter-class>com.niit.FilterA</filter-class>
</filter>
<!-- 配置过滤器所拦截的URL -->
<filter-mapping>
    <filter-name>FilterA</filter-name>
    <url-pattern>/ServletFilterTest</url-pattern>
</filter-mapping>
```

# 第5章：JSP基础
## JSP指令
JSP指令用于提供全局性的指令信息，它们影响JSP页面的整体结构和行为。常见的JSP指令有：
1. **page指令**：用于定义页面级别的属性，如内容类型、缓存需求、错误页面等。
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```
2. **include指令**：用于静态包含其他文件的内容。
```jsp
<%@ include file="header.jsp" %>
```
3. **taglib指令**：用于引入自定义标记库。
```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```
## JSP脚本元素
JSP脚本元素允许在JSP页面中嵌入Java代码。
1. **声明**：用于声明变量和方法。
```jsp
<%! int count = 0; %>
```
2. **表达式**：用于直接输出表达式的值。
```jsp
<%= count %>
```
3. **Scriptlet**：用于编写Java代码块。
```jsp
<%
for (int i = 0; i < 10; i++) {
    out.println(i);
}
%>
```
- JSP注释：用于在JSP页面中添加不会被发送到客户端的注释。
```jsp
<%-- JSP注释 --%>
```
## JSP内置对象
JSP提供了九大内置对象，这些对象无需显式声明即可在JSP页面中使用。
- application：ServletContext对象的实例，用于表示Web应用程序的上下文。
- config：ServletConfig对象的实例，用于获取配置信息。
- exception：Throwable对象的实例，用于处理页面中的异常。
- out：JspWriter对象的实例，用于向客户端发送输出。
- page：当前JSP页面的Servlet实例。
- session：HttpSession对象的实例，用于跟踪用户会话。
- response：HttpServletResponse对象的实例，用于响应客户端请求。
- request：HttpServletRequest对象的实例，用于处理客户端请求。
- pageContext：PageContext对象的实例，用于提供对JSP页面所有对象和命名空间的访问。
## JSP动作
JSP动作元素用于在JSP页面中执行动作，它们在运行时执行。
- **<jsp:include>**：动态包含其他文件的内容。
```jsp
<jsp:include page="header.jsp" />
```
- **<jsp:forward>**：将请求转发到另一个资源。
```jsp
<jsp:forward page="target.jsp" />
```
- **<jsp:useBean>**：创建或查找Bean实例。
```jsp
<jsp:useBean id="bean" class="com.niit.BeanClass" />
```
- **<jsp:setProperty>**：设置Bean属性。
```jsp
<jsp:setProperty name="bean" property="propertyName" value="value" />
```
- **<jsp:getProperty>**：获取Bean属性。
```jsp
<jsp:getProperty name="bean" property="propertyName" />
```
# 第7章：MVC设计模式与JDBC操作
## MVC设计模式
MVC（Model-View-Controller）是一种软件设计模式，用于将应用程序分解为三个相互协作的组件。
- **Model（模型）**：处理业务逻辑和数据，通常代表数据库记录。
- **View（视图）**：呈现数据给用户，通常是指HTML页面或其他用户界面。
- **Controller（控制器）**：处理用户输入和交互，通常负责从视图读取数据，控制用户输入，并向模型发送数据。
## JDBC操作步骤
JDBC（Java Database Connectivity）是Java语言中用于数据库操作的API。以下是使用JDBC进行数据库操作的步骤：
### 方式1：使用Statement对象
1. **加载驱动**
   ```java
   Class.forName("com.mysql.cj.jdbc.Driver");
   ```
2. **建立数据库连接**
   ```java
   Connection conn = DriverManager.getConnection(url, user, password);
   ```
3. **创建Statement对象**
   ```java
   Statement statement = conn.createStatement();
   ```
4. **执行SQL语句**
   - **查询操作**
     ```java
     String sql = "SELECT * FROM xxx";
     ResultSet rs = statement.executeQuery(sql);
     ```
   - **更新操作**
     ```java
     int i = statement.executeUpdate(sql);
     ```
5. **操作结果集**（仅针对查询操作）
   ```java
   while (rs.next()) {
       int id = rs.getInt("id");
       String code = rs.getString("code");
       String name = rs.getString("name");
       String author = rs.getString("author");
       BigDecimal unitPrice = rs.getBigDecimal("unit_price");
   }
   ```
6. **释放资源**
   ```java
   rs.close(); // 释放查询结果集（针对查询操作）
   statement.close(); // 释放Statement对象
   conn.close(); // 释放连接对象
   ```
### 方式2：使用PreparedStatement对象
1. **加载驱动**
   ```java
   Class.forName("com.mysql.cj.jdbc.Driver");
   ```
2. **建立数据库连接**
   ```java
   Connection conn = DriverManager.getConnection(url, user, password);
   ```
3. **创建PreparedStatement对象**
   ```java
   String sql = "SELECT * FROM xxx WHERE a=? AND b=?";
   PreparedStatement ps = conn.prepareStatement(sql);
   ps.setString(1, code); // 设置SQL语句中的第1个问号的值
   ps.setString(2, name); // 设置SQL语句中的第2个问号的值
   ```
4. **执行SQL语句**
   - **查询操作**
     ```java
     ResultSet rs = ps.executeQuery();
     ```
   - **更新操作**
     ```java
     int i = ps.executeUpdate();
     ```
5. **操作结果集**（仅针对查询操作）
   ```java
   while (rs.next()) {
       int id = rs.getInt("id");
       String code = rs.getString("code");
       String name = rs.getString("name");
       String author = rs.getString("author");
       BigDecimal unitPrice = rs.getBigDecimal("unit_price");
   }
   ```
6. **释放资源**
   ```java
   rs.close(); // 释放查询结果集（针对查询操作）
   ps.close(); // 释放PreparedStatement对象
   conn.close(); // 释放连接对象
   ```


# 第8章：EL表达式与JSTL
## EL表达式（Expression Language）
EL表达式提供了一种在JSP页面中简化代码的方式，它可以访问页面的上下文以及JavaBean组件的属性。
### EL表达式的特点
- **变量未声明不会引发异常**：如果EL表达式中的变量未声明，它不会抛出异常，而是显示为空。
  ```jsp
  <p>用户名：${username}</p>
  ```
- **算术运算**：EL表达式在计算算术运算时，会将`null`视为`0`。
  ```jsp
  <p>${2+i}</p>
  ```
- **逻辑表达式**：在计算逻辑表达式时，EL表达式会将`null`视为`false`。
  ```jsp
  <p>${3>j}</p>
  ```
## JSTL标记（JavaServer Pages Standard Tag Library）
JSTL提供了一系列自定义标记，用于简化JSP页面的开发。
### JSTL标记的使用
- **条件判断（if）**：用于执行基于条件的操作。
  ```jsp
  <c:if test="${condition}">
    <!-- 条件为真时执行的代码 -->
  </c:if>
  ```
- **循环迭代（forEach）**：用于遍历数组、集合或Map。
  - **遍历Map的写法**：
    ```jsp
    <c:forEach items="${map}" var="item">
      <div>键=${item.key}</div>
      <div>值=${item.value}</div>
    </c:forEach>
    ```
### 格式化日期（<fmt:formatDate>）
`<fmt:formatDate>`标记用于格式化日期和时间。
- **pattern属性**：指定日期/时间的格式。
  ```jsp
  <fmt:formatDate value="${date}" pattern="yyyy-MM-dd"/>
  ```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODU2MjQ5MTksMjAzMjAwNjgyMywtMT
gyNzY2NDY4MSwtMTA1NjcxMjcyNiwtMTcyNjg1MjUyNywtMTQx
MDc0Njc5OSwyMTA2OTQyOTZdfQ==
-->