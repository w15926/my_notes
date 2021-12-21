# Tomcat

**启动**

```shell
cd /library/Tomcat/bin
./startup.sh
```

如果运行`./startup.sh`提示`Cannot find /Library/Tomcat/bin/setclasspath.sh`，则在当前窗口运行`unset CATALINA_HOME`然后再次启动。



**退出**

```shell
./shutdown.sh
```



**配置端口**

1. 进入/Library/Tomcat/conf/server.xml
2. 修改`Connector`
3. 重启Tomcat



**配置localhost**

1. 进入/Library/Tomcat/conf/Catalina/localhost里新建abc.xml
2. 进入新建的xml文件写入配置`<Context path="/abc" docBase="/Library/Tomcat/webapps/test" />`
3. 重启Tomcat
4. 浏览器输入localhost:8080/test就可以直接访问（原本是localhost:8080/test/index.html）
5. **注意：**此时只能访问localhost:8080和localhost:8080/test，其他不生效



# web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--  给Tomcat配置Servlet程序  -->
    <servlet>
        <!-- Servlet别名（一般类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!-- Servlet全类名 -->
        <servlet-class>com.example.web.HelloServlet</servlet-class>
    </servlet>

    <!-- Servlet程序配置访问地址 -->
    <servlet-mapping>
        <!-- 上面别名对应这里 -->
        <servlet-name>HelloServlet</servlet-name>
        <!--
        /hello中
        / 表示 http://ip:port （当前工程路径）
        /hello 表示 http://ip:port/hello
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <!-- HelloServlet2 -->
    <servlet>
        <servlet-name>HelloServlet2</servlet-name>
        <servlet-class>com.example.web.HelloServlet2</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet2</servlet-name>
        <url-pattern>/hello2</url-pattern>
    </servlet-mapping>

</web-app>
```



# Servlet

## 生命周期

```tex
生命周期 - 1 - constructor
生命周期 - 2 - init
生命周期 - 3 - servece Hello Servlet 被访问了
生命周期 - 4 - destroy
```

```java
public class HelloServlet extends HttpServlet {
  public HelloServlet() {
    System.out.println("生命周期 - 1 - 构造器");
  }

  @Override
  public void init(ServletConfig config) throws ServletException {
    System.out.println("生命周期 - 2 - init");

    System.out.println("HelloServlet程序的别名是：" + config.getServletName()); // HelloServlet （xml里配置）
    System.out.println("初始化username的值是：" + config.getInitParameter("username")); // <init-param> （xml里配置）
    System.out.println("url的值是：" + config.getInitParameter("url")); // <init-param> （xml里配置）

    System.out.println(config.getServletContext());

    super.init(config);
  }

  @Override
  public ServletConfig getServletConfig() {
    return super.getServletConfig();
  }

  /**
   * @Description 处理请求和响应
   * @Author wangLeXiang
   * @Date 2021/8/5 3:22 下午
   * @Param
   * @Return
   * @Since version-1.0
   */
  @Override
  public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {

    // 类型转型（因为他有method方法）
    HttpServletRequest httpServletRequest = (HttpServletRequest) req;
    // 获取请求方式（get、post、...）
    String method = httpServletRequest.getMethod();

    if ("GET".equals(method)) System.out.println("GET请求");
    if ("POST".equals(method)) System.out.println("POST请求");


    System.out.println("生命周期 - 3 - service Hello Servlet 被访问了");
    super.service(req, res);
  }

  @Override
  public String getServletInfo() {
    return super.getServletInfo();
  }

  @Override
  public void destroy() {
    System.out.println("生命周期 - 4 - destroy");
    super.destroy();
  }
}
```



## context

**xml**

```xml
    <!-- 上下文（属于整个web工程参数） -->
    <context-param>
        <param-name>username</param-name>
        <param-value>context</param-value>
    </context-param>
    <context-param>
        <param-name>password</param-name>
        <param-value>root</param-value>
    </context-param>
```



**java**

```java
public class ContextServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.获取 xml 中定义的 context
    // ServletContext context = getServletConfig().getServletContext();
    ServletContext context = getServletContext();
    String username = context.getInitParameter("username");
    System.out.println("context-param参数的username值为：" + username);

    // 2.获取当前的工程路径 （url参数 http://localhost:8080/web/cs 中的/web）
    System.out.println("当前的工程路径为" + context.getContextPath()); // /web

    // 3.获取工程部署在服务器上的绝对路径
    // /Users/lalala/Documents/process/2021-backend/java/web/target/web-1.0-SNAPSHOT/
    System.out.println("当前工程部署在服务器上的绝对路径为" + context.getRealPath("/"));
    // System.out.println("当前工程部署在服务器上的css绝对路径为" + context.getRealPath("/css"));
    System.out.println(context.getRealPath("/css/base.css")); // 举一反三

    // 工程全局存取（类似map）
    context.setAttribute("key1", "value1");
    System.out.println("context中域数据key1的值为：" + context.getAttribute("key1")); // value1

  }
}
```



## HttpServletRequest

### 常用方法

```java
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("url -- > " + req.getRequestURL()); // 请求资源路径 http://localhost:8080/web/hello2
    System.out.println("uri -- > " + req.getRequestURI()); // 统一资源定位符（路径）/web/hello2
    System.out.println("客户端 ip 地址 -- > " + req.getRemoteHost());
    System.out.println("请求头 User-Agent -- > " + req.getHeader("User-Agent"));
    System.out.println("请求头方式 -- > " + req.getMethod());
    super.doGet(req, resp);
  }
```



### 获取请求报文

```java
    req.setCharacterEncoding("UTF-8"); // 先设置临时编码（get可以不用）
    System.out.println("用户名" + req.getParameter("username")); // 卞卞
    System.out.println("年龄" + req.getParameter("age")); // 18
    System.out.println("爱好" + req.getParameter("hobby")); // 只能获取第一个值 eat
    System.out.println("爱好" + Arrays.toString(req.getParameterValues("hobby"))); // 获取第所有值 [eat, sleep]
```



### 请求转发

Servlet1

```java
    // 1 获取请求参数
    String username = req.getParameter("username");
    System.out.println("请求参数 username --> " + username);
    // 2 给请求参数盖章传递到另一个Servlet
    req.setAttribute("key","Servlet1的章");
    System.out.println("请求的Servlet的章 --> " + req.getAttribute("key"));
    // 3 获取转发地址  / 为 http://ip:port/工程名/ （映射到代码web目录）
    RequestDispatcher requestDispatcher = req.getRequestDispatcher("/hello3"); // xml mapping设置
    // 4 进入另一个Servlet
    requestDispatcher.forward(req,resp);
```



Servlet2

```java
    // 1 获取请求参数
    String username = req.getParameter("username");
    System.out.println("请求参数 username --> " + username);
    // 2 查看请求的Servlet是否盖章
    Object key1 = req.getAttribute("key");
    System.out.println("检测请求的Servlet的章 --> " + key1);
    // 3 处理自己的业务
    System.out.println("处理自己的业务");
```



output

```java
请求参数 username --> 卞卞
请求的Servlet的章 --> Servlet1的章
请求参数 username --> 卞卞
检测请求的Servlet的章 --> Servlet1的章
处理自己的业务
```



## HttpServletResponse

### 输出流

```java
// 不能同时使用
res.getOutputStream(); // 字节流
res.getWriter(); // 字符流
```



### 字符集

```java
// resp.setCharacterEncoding("UTF-8"); // 此字符集仅对当前响应有效
// resp.setHeader("Content-Type", "text/html; charset=UTF-8"); // 对当前浏览器使用
resp.setContentType("text/html; charset=UTF-8"); // 对浏览器和服务器同时使用（获取流之前才有效）
PrintWriter writer = resp.getWriter(); // 字符流
writer.write("response content 哈哈哈"); // 文字显示在界面上
```



### 重定向

```java
    /*
     * 方式一
     */
    // // 设置重定向响应码302
    // response.setStatus(302);
    // // 设置响应头，说明重定向的地址
    // response.setHeader("Location", "http://localhost:8080/web/red2"); // red2为XML配置

    /*
     * 方式二
     */
    response.sendRedirect("http://localhost:8080/web/red2");
```



## Cookie

```java
public class CookieServlet extends HttpServlet {
  @Override
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.setContentType("text/html; charset=UTF-8");

    // 创建Cookie对象
    Cookie cookie = new Cookie("key1", "value1");
    // 生命周期
    // cookie.setMaxAge(-1); // 默认 -1 即会话状态
    // cookie.setMaxAge(0); // 删除当前cookie
    // cookie.setMaxAge(60 * 60); // 一小时后失效（ 60S * 60S ）
    // 通知客户端保存Cookie F12
    response.addCookie(cookie);

    response.getWriter().write("key: " + cookie.getName() + " value: " + cookie.getValue());
    System.out.println("create Cookie successfully");

    // 获取客户端的cookie
    Cookie[] requestCookies = request.getCookies();
    for (Cookie item : requestCookies) {
      System.out.println("key: " + item.getName());
      System.out.println("value: " + item.getValue());
    }

    // 修改某一个 cookie 值
    //   循环 -> 取指定值 -> 修改 -> 保存
    // cookie.setValue("value"); // 修改
    // response.addCookie(cookie); // 保存

  }
}
```

