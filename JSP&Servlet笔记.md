*4

## Tomcat

### 什么是Tomcat

是一个轻量级应用服务器，从功能上讲，就是一个用来运行WEB应用程序的环境(容器)，通过这个容器，WEB程序能够获得满足自身运行的条件，同时使得外界与WEB程序交互

### 目录下的文件含义

- #### 目录

  **bin** : 存放了Tomcat的可执行文件

  **conf**: 存放tomcat服务器的各种配置文件,比较重要的四个如下

  ​		-- server.xml //配置整个服务器的信息，例如修改端口号
  ​		-- tomcatusers.xml //存储tomcat用户的文件
  ​		-- web.xml //部署描述符，这个文件中注册了很多MIME类型
  ​		-- context.xml //对应于统一的配置，通常不用修改

  **lib** :存放tomcat服务器所需要的各种jar文件
  **logs** : 存放tomcat服务器执行的日志文件

  **temp** : 存放tomcat服务器的临时文件
  **webapps** : tomcat主要的web发布目录，默认情况下把web应用文件存放在此目录下
  **work**: 存放jsp编译后产生的.class文件

- #### 文件

  ​	**license**: 许可证
  ​	**notice**: 说明文件

### Web项目结构

![alt WEB项目结构](E:\学习笔记\基本知识笔记\2.编程语言\8.JAVA\img\web项目结构.jpg)

项目存放在tomcat下的webapps目录下，我们可以通过把项目打成war包，然后放到该目录下，tomcat会自动帮我们解压到当前目录

## Servlet

### 什么是Servlet

​	Servlet是一种服务器端的JAVA应用程序，专门用来处理客户端请求，响应数据给浏览器显示
本质就是JAVA类，通过JAVA类的方法动态的向客户端输出内容



​	Servlet和jsp都属于三层结构中的web层，也就是说，Servlet本身不做业务处理，只负责接受请求，并调用哪个javaBean去处理请求，并且确定使用哪个jsp页面来处理返回的数据

### 如何使用Servlet

Servlet的源码还是写在src目录当中，它实际上就是JAVA代码

三种实现方式:

* 实现Servlet接口(是所有java Servlet类的基础接口)
* 继承GenericServlet类(这种方法很少用，它其实就是Servlet的实现类)
* 继承HttpServlet类(HttpServlet类是在GenericServlet类基础上拓展HTTP协议的servlet)



不论使用何种方式，都需要对Servlet进行配置才能使用，两种配置方式 web.xml  和 注解



#### 实现Servlet接口

**Servlet接口有5个方法需要重写，下面分别介绍这5个方法的作用**

* **void init(ServletConfig var1)**
  * Servlet被创建会被自动调用，对Servlet进行初始化
  * 比它先调用的是构造方法
  * init()方法只会被调用一次，就是Servlet被创建的时候，后面除非服务器关闭，否则不会调用了
  * Servlet的两种创建情况
    * 1.第一次被用户请求
    * 2.在配置文件中的<servlet></servlet>标签中配置了<load-on-startup></load-on-startup>且值>=0就会在服务器启动时自动创建
  * ServletConfig参数的作用，这是一个保存了配置文件信息的变量，我们可以通过它来获取配置文件内容，例如初始化变量

* **ServletConfig getServletConfig()**
  * 获取ServletConfig对象

* **void service(ServletRequest var1, ServletResponse var2)**
  * 获取请求，并决定如何处理请求的方法
  * 这个方法，每当有用户请求一次的时候就会被调用一次
  * ServletRequest 这个对象表示的是用户的请求信息
  * ServletResponse 这个对象表示的是要响应给用户的响应信息
* **String getServletInfo()**
  * 获取Servlet的相关信息
* **void destroy()**
  * Servlet对象的销毁方法，一般是在服务器被关闭时会调用

##### 代码演示

```java
//这里使用了注解的方式对servlet进行配置，下面会说
@WebServlet(
        urlPatterns = {"/test"},
        initParams = {@WebInitParam(name="name",value="张三")},
        loadOnStartup = 0
        )

public class Test implements Servlet {
    private ServletConfig servletConfig;

    @Override
    public void init(ServletConfig servletConfig){
        //init方法一般是不会被用到的，但是如果想要读取配置文件信息时，可以获取这个参数
        this.servletConfig = servletConfig;
        System.out.println("init()被调用");
    }

    @Override
    public ServletConfig getServletConfig(){
        return servletConfig;
    }

    @Override
    public void service(ServletRequest request, ServletResponse response){
        //这里也演示了servletConfig的一些功能
        System.out.println(servletConfig.getServletName() + "的service被调用,它里面的初始化参数有" + 	servletConfig.getInitParameter("name"));
    }

    @Override
    public String getServletInfo(){
        return null;
    }

    @Override
    public void destroy(){
        System.out.println("destroy()被调用");
    }
}
```

#### GenericServlet类

这个类的出现是为了简化Servlet的实例化步骤的，它里面帮我们实现了一些平时基本不需要实现的方法，然后帮我们封装了一些方法,直接看它

```java
public abstract class GenericServlet implements Servlet, ServletConfig, Serializable {
    private static final long serialVersionUID = 1L;
    //就像上面的Servlet实现类一样，它也获取了congfig对象
    private transient ServletConfig config;
	
    public GenericServlet() {}
    
    //基础5大方法
    //可以看出它对destroy以及getServletInfo也没又做什么特殊处理
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }

    public void init() throws ServletException {
    }
    
    public void destroy() {
    }
	
     public ServletConfig getServletConfig() {
        return this.config;
    }
    
    public String getServletInfo() {
        return "";
    }
    //这个方法是抽象方法，必须要实现，在这里写的就是我们该如何处理请求和响应
    public abstract void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
    
    
    //多出来的方法
    //可以看到主要就是获取一些配置的信息，因为多数都是基于ServletConfig对象来的
    public String getInitParameter(String name) {
        return this.getServletConfig().getInitParameter(name);
    }

    public Enumeration<String> getInitParameterNames() {
        return this.getServletConfig().getInitParameterNames();
    }

    public ServletContext getServletContext() {
        return this.getServletConfig().getServletContext();
    }

    public String getServletName() {
        return this.config.getServletName();
    }
}
```

#### HttpServlet类

HttpServlet是基于Http协议上对Servlet进行扩展的，它继承于GenericServlet类，除了重写了service方法以外，就是自己实现的额外方法，其他基础方法都是用父类的。

##### HttpServlet的service方法做了什么

HttpServlet的service方法会对用户传来的HTTP请求的**请求方式**进行判断，然后根据不同的请求方式进行不同的处理，这些请求方式的处理逻辑要我们自己去实现(继承HttpServlet然后重写对应方法),HTTP协议有哪些请求方式，它就有哪些方法，格式为doXXX,XXX为请求的方式，例如Post请求, doPost,get请求就是doGet,也并不是所有的请求方式我们都要实现，只需要实现自己想要的方式就好了

##### 代码演示

```java
public class MyServlet extends HttpServlet {
    //这是一种实现方式,这种方式中可以重写doGet和doPost方法，对应使用两种请求提交的执行代码
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        /*
        * 在容器接受到来自客户的HTTP请求后，会收集HTTP请求中的信息，并分别创建代表请求和响应JAVA对象
        *  request参数的表示的是客户端请求信息
        *  response参数的表示的是传递给客户端的响应信息
        * */
        //response代表响应对象， 最好是设置内容格式，不然有中文的话会乱码
        response.setContentType("text/html;charset=UTF-8");
        String name = request.getParameter("name"); //通过getParameter()方法获取到对应的参数

        PrintWriter writer = response.getWriter(); //获取响应的输出对象
        writer.write(name); //将提交的参数输出
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response){

    }
}
```

#### 如何配置Servlet

##### web.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
	<!--配置Servlet-->
    <servlet>
        <!--设置Servlet名字，最好和类名一致，也可以不一致-->
        <servlet-name>MyServlet</servlet-name>
        <!--Servlet所在路径-->
        <servlet-class>OneServlet.MyServlet</servlet-class>
        <!--设置init方法在服务器启动时就调用，值可以>=0都有效-->
        <load-on-startup>1</load-on-startup>
    </servlet>
	<!--建立映射-->
    <servlet-mapping>
        <!--这里是上面定义的那个Servlet名-->
        <servlet-name>MyServlet</servlet-name>
        <!--访问Servlet时填的路径-->
        <url-pattern>/MyServlet</url-pattern>
    </servlet-mapping>
</web-app>
<!--访问/MyServlet这个路径时，它会根据servlet-name去寻找对应的servlet根据指定路径调用Servlet-->
```

##### WebServlet

除了web.xml其实还有更简便的配置方法，就是使用标注，直接在类上面写

```
@WebServlet("servlet-pattern") //直接在这里写定外部访问路径也可以访问到

它还可以通过key=value的方式设置更多信息
@WebServlet(
	name = "Hello", //设置servlet的名称是hello
	urlPatterns = {"/hello"}, //如果url请求/hello这个路径就调用这个servlet,这个可以同时设置多个路径，
							 //value的作用于等同于urlPatterns，二者只能选一个用
	loadOnStartup = 1 //服务器打开自动初始化
	initParams = {@WebInitParam(name="key",value="value")} //设置初始化参数
)
```

### Servlet生命周期

Servlet是被Web容器创建的(Tomcat),当第一次被请求或者在Tomcat启动(load-on-startup)时被创建，先调用构造方法，然后要读取Servlet的配置信息(xml or 标注),Web容器会生成一个ServletConfig对象，并从该对象取得Servlet初始参数，然后调用init方法，然后再调用service方法



init()方法:初始化方法
	当浏览器第一次请求Servlet时，服务器自动创建Servlet对象,同时调用init()方法完成对象初始化
	该方法只调用一次，后面不管谁来访问，只要Servlet对象没有被释放，那么就不会在调用init()了

service()方法: 处理浏览器请求的方法，每来一次请求service()方法就执行一次对请求的处理

destroy()方法: 销毁方法，当服务器关闭时，在销毁Servlet对象之前，调用该方法，释放资源，该方法只调用一次



默认情况下，第一次请求Servlet时，服务器会创建一个Servlet对象，并创建它的init()方法，完成对象的初始化,在配置文件web.xml中修改<load-on-startup>标签，参数的值设置为>=0数字，默认第一次请求时创建Servlet对象会修改为服务器启动时就创建Servlet对象，每次请求Servlet时，都会调用service()方法,关闭服务器时，调用一次destroy()方法

### 如何设置Servlet初始化参数

#### web.xml中配置

```xml
<servlet>
    <!--多个初始化参数要写多个init-param标签组-->
    <init-param>
        <param-name>key值</param-name>
        <param-value>value值</param-value>
    </init-param>
    <init-param>
        <param-name>key2值</param-name>
        <param-value>value2值</param-value>
    </init-param>
</servlets>
```

#### 使用标注

```java
@WebServlet(
    urlPatturn = {"/init"},
    //设置初始化参数
    initParams = {
    	@WebInitParam(name="key",value="value"),
    	@WebInitParam(name="key",value="value")
		}
    )
```

### ServletConfig对象

ServletConfig对象存储的是Servlet的相关配置信息，它的创建时间在Servlet被实例化之后，在构造方法之后在init之前，它也只会在创建时被调用一次

这个对象的主要作用就是在于读取一些初始化数据，这些数据可以通过 xml文件 or 标注 来设置

#### 常用方法

* String getInitParamter(String s)
  * 根据参数名，获取初始化变量值
* ServletContext getServletContext()
  * 获取ServletContext对象
* Enumeration<String> getInitParameterNames()
  * 获取一个拥有所有初始化变量名的枚举类
* String getServletName()
  * 获取Servlet名

### ServletContext对象

ServletContext表示的是整个应用程序，它由容器创建，当WEB服务器启动时，会为每一个WEB应用程序(webapps下每个目录就是一个应用程序)创建一块共享的存储区域，服务器启动创建，服务器关闭销毁,ServletContext可以获取请求资源的URI，设置，存储属性，应用程序初始化参数，甚至动态设置Servlet实例

#### 获取ServletContext对象

```
ServletContext sc = getServletConfig().getServletContext()
```

#### 常用方法

* getRequestDispatcher("uri").forward(request,response) //转发

* Set<String> getResourcePaths("uri")//获取WEB应用程序目录下的文件，包括WEB-INF中的文件，uri必须以/开头,注意是web目录下的
* InputStream getResourceAsStream("uri")//读取WEB应用程序下文件的内容，可以读取WEB-INF

### Servlet线程安全问题

由于所有请求使用的是同一个Servlet对象，那么当这个Servlet对象拥有成员变量之后，那么这个变量也是被共享的，因此会引发线程安全问题
最好的解决方法就是尽量不用成员变量

### Web容器到底做了什么(Tomcat)

当请求来到HTTP服务器(Tomcat内置了一个小型的HTTP服务器，但是它只适合用来做测试),容器会创建一个代表当次请求的HttpServletRequest对象，并将请求的相关信息给该对象，同时，容器会创建一个HttpServletResponse对象，作为稍后要对浏览器进行响应的JAVA对象(注意HttpServletRequest和HttpServletResponse都是接口，它们的实现类都是由容器提供的),接着容器会根据@WebServlet标注或者web.xml的设置，找出处理该请求的Servlet，并调用它的service()方法，然后将创建的HttpServletRequest和HttpServletResponse对象传入作为参数，service()方法中会根据HTTP请求的方式，调用对应的doXXX()方法，当容器转换HttpServletResponse对象为HTTP响应的，然后在由HTTP服务器传送给浏览器之后，容器就会将HttpServletRequest和HttpServletResponse对象销毁回收,这里也很契合HTTP的无状态属性，每次请求都是用新的对象，使用完之后都会被销毁

### request对象

HttpServletRequest对象表示的是用户请求，里面存储的也是用户有关的请求信息

#### 常用方法

其实到现在为止，用的比较多的也就**转发(getRequestDisPatcher().forward**) 和 **get/setAttribute()**

* String getRequestURI() //获取URL中的资源路径
* String getQueryString() //返回请求行中的参数部分,key=val&key=val
* String getContextPath() //返回项目名称
* Enumeration<String> getHeaderNames() //返回所有请求头的key
* String getHeader("请求头key") //通过key获取对应的请求头值
* Enumeration<String> getHeaders("请求头key"); // 这个方法应该是适用于当这个头部有多个值时
* String getParameter("参数名") //根据参数名获取提交参数的数据，对于参数污染，它只获取第一个参数值
* String[] getParameterValues("参数名") // 返回一个String[]数组，可以获取参数名下所有的值,对于参数污染会，会获取所有值
* Map<String, String[]> getParameterMap() //返回一个有所有参数和值的map集合，参数名为key,内容为val,对于参数污染，会获取所有值
* String getServerName() //获取服务器名
* String getServerPort() //获取服务器端口
* String getRemoteAddr() //客户端IP地址
* String getRemoteHost() //获取客户端主机名
* String getRemotePort() //获取客户端端口号
* void setAttribute(String key, Object value) //设置数据,key和value由自己定义
* Object getAttribute(String key, Object value) //获取数据
* setCharacterEncoding("UTF-8") //指定请求的数据编码格式
* getRequestDispatcher("转发路径").forward(request,response) //请求转发



下面这两个方法要注意，如果调用的了话，那么在他下面调用getParameter方法获取参数会失效

* BufferedReader getReader() //获取输入内容的字符流
* BufferedInputStream getInputStream() //获取输入内容字节流

java.net.URLDecoder.decode(内容,"UTF-8") //进行utf-8解码
java.net.URLEncoder.encode(内容,"UTF-8") //进行utf-8编码

### response对象

HttpServletResponse对象表示的是响应对象，里面存储的是要返回给客户端的响应信息

#### 常用方法

* setStatus(int statusCode); //手动设置状态码,其实就是一个伪装，并不会影响程序执行，具体的还是得看服务器如何处理

* sendError(int statusCode, String message) //向页面发送错误提示信息
* setHeader(String key,String value) //设置响应头
* addHeader(String key,String value) //添加响应头
* sendRedirect("url") //重定向
* setCharacterEncoding("UTF-8") //指定响应的编码格式
* PrintWriter getWriter(); //获取流
* setAttribute("key","value"); 
* getAttribute("key","value");

### 请求转发 & 重定向

#### 请求转发

使用 RequestDispatcher 对象，它可以将当前页面的request对象传递到另外一个页面
它的转发属于服务器路径，默认的访问到项目的web一级



//请求转发，将资源转发给目标页面
 RequestDispatcher requestDispatcher = request.getRequestDispatcher("/request/login.jsp");
 requestDispatcher.forward(request,response); 

//包含文件,在一个servlet中包含另一个指定的servlet，并将response和request两个参数传递给它，包含相当于把那个的代码直接插进来
 RequestDispatcher requestDispatcher = request.getRequestDispatcher("/request/servlet2");
 requestDispatcher.include(request,response);



特点:
 	1. 一次请求，可以使用request对象共享数据
 	2. 地址栏不变(服务器内部行为)
 	3. 转发只能访问当前服务器下的资源

转发是容器中控制权的转向，是服务器请求资源，服务器直接访问目标地址的url，把这个url相应的内容读取过来，然后把这些内容再发给浏览器，浏览器不知道服务器发送的内容是从哪里来的，所以地址栏还是原来的地址。转发只能访问当前服务器的资源

#### 重定向

重定向的HTTP 状态码:302

使用:sendRedirect() //进行重定向



//还有一种更为原始的方法
repsonse.setStatus(HTTPServletResponse.SC_MOVED_PERMANENTLY)
response.addHeader("Location","重定向url")

重定向是服务器根据逻辑，发送一个状态码，告诉浏览器重新去请求那个地址，一般来说浏览器会用刚才请求的所有参数重新请求，所以request，session参数都可以获取，并且可以在浏览器的地址栏中看到跳转后的链接地址

#### 请求转发和重定向的区别

1.转发只能访问服务器资源，重定向可以访问外部资源
2.转发不会改变地址栏，重定向是二次访问会改变地址栏
3.关键字的区别
	重定向 response.sendRedirect("uri");
	转发 request.getRequsetDispatcher("uri").forward(request,response);

### Servlet代码实例

#### 文件上传

```
利用servlet3.0之后加入的Part类进行文件上传

//需要在类前面使用标注 @MultipartConfig
Part photo = request.getPart("loadFile"); //文件上传的文件
OutputStream out = new FileOutputStream("E:/1.jpg");
InputStream in = photo.getInputStream();
//如果使用photo.write("上传路径")这个方法，那么下面的都不用写
//写入到本地
byte[] buffer = new byte[1024];
int length = -1;
while((length = in.read(buffer)) != -1){
    out.write(buffer,0,length);
}

out.close();
in.close();
```

#### Servlet实现验证码

```
 //在内存中创建一张图片
 BufferedImage bi = new BufferedImage(WIDTH,HEIGHT,BufferedImage.TYPE_3BYTE_BGR);
 //得到图片
 Graphics g = bi.getGraphics();
 
 //设置图片的背景色
 //设置颜色
 g.setColor(Color.WHITE);
 //填充区域
 g.fillRect(0, 0, WIDTH, HEIGHT); //前两个参数是开始的位置,后两个参数是宽和高
 
 //设置边框
 //设置边框颜色
 g.setColor(Color.BLUE);
 //边框区域
 g.drawRect(1, 1, WIDTH - 2, HEIGHT -2);
 
//画干扰线
 //设置颜色,每次画东西都要设置颜色
 g.setColor(Color.GREEN);
 //设置线条个数并画线
 for ( int i = 0 ; i < 3 ; i++ ) {
     int x1 = new Random().nextInt(WIDTH);
     int y1 = new Random().nextInt(HEIGHT);
     int x2 = new Random().nextInt(WIDTH);
     int y2 = new Random().nextInt(HEIGHT);
     g.drawLine(x1, y1, x2, y2); //画线
 }
 
//设置响应头通知浏览器以图片的形式打开
resp.setContentType("image/jpeg");

//设置响应头控制浏览器不要缓存
resp.setDateHeader("expries", -1);
resp.setHeader("Cache-Control", "no-cache");
resp.setHeader("Pragma", "no-cache");

//将图片传给浏览器
ImageIO.write(bi, "jpg", resp.getOutputStream());
```


## JSP

### 什么是JSP

**JSP(Java Server Pages),是运行在服务器端的 java页面,这个页面使用了HTML嵌套JAVA代码实现**

### JSP代码格式

第一行: page指令

​	<%@ page contentType="text/html;charset=UTF-8" language="java" %>

​	contentType 属性 : 标识当前jsp页面是一个html页面，使用编码格式是utf-8
​	language 属性: 标签当前jsp页面内嵌的是java代码
​	
JSP页面的JAVA代码写在 <% %> 中

### 实例

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
	<head>
        <title> 我是一个JSP页面</title>
    </head>
    <body>
        <% out.println("我是一个JSP页面"); %>
    </body>
</html>
```

### JSP指令

格式: <%@ 指令名 属性1="" 属性2="" %>一般情况下会把jsp指令放在jsp文件最上方
	jsp中最常用的三大指令 page include taglib
	

1.page指令
	-- page指令是最常用的执行，也是属性最多的指令，没有必须属性，都是可选指令
	-- 标识页面 
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	-- 导包
		<%@ page import="java.util.*" %>

2.include指令
	(1) include指令(静态包含)
		-- include指令表示静态包含，把多个jsp合并成一个jsp文件
			<%@ include file="页面"%> //包含指定页面
		-- include指令是在编译级别完成的包含，即当前jsp页面和被包含的jsp页面合并成一个jsp,然后再编译成servlet
	(2) include标签(动态包含)
		-- include标签是在运行级别完成的包含，即当前jsp和被包含的jsp都会各自生产servlet,然后再执行当前jsp的servlet时完成包含另一个jsp的servlet
			<jsp:include page="页面"></jsp:include>

3.taglib指令
		-- 在jsp页面中使用第三方标签库时，需要使用taglib指令来引用

### jsp标签

#### 动态包含页面

```jsp
<jsp:include page="页面"></jsp:include> //动态包含一个页面
```

#### 转发

```jsp
//无参
<jsp:forward page={"相对路径 " | " java程序段"}> //将当前请求转发到这个可以处理请求的文件上,这个文件也可以是jsp文件也可以是其他处理方法

//带参数
<jsp:forward page={"relativeURL" | "<%= expression %>"} >
<jsp:param name="parameterName" value="{parameterValue | <%= expression %>}" />
</jsp:forward>
```

### B/S通讯过程

1.浏览器对WEB服务器发出请求

2.WEB服务器收到请求，将请求专由WEB容器(Tomcat)处理,WEB容器会剖析HTTP请求内容，创建各种对象，例如(HttpServletRequest,HttpServeletResponse,HttpSession等)

3.WEB容器由请求的URI决定要使用哪个Servlet来处理请求(开发人员要定义URI和Servlet的对应关系)

4.Servlet根据请求对象(HttpServletrequest)的信息决定如何处理,通过响应对象(HttpServletsponse)来创建响应

5.WEB容器与HTTP服务器沟通，HTTP服务器将相关响应对象转为HTTP响应并传回给浏览器

### JSP和Servlet的关系

Servlet和JSP是一体两面

因为JSP会被WEB容器转译为Servlet的.java源文件，编译为.class文件，然后加载容器，因此最后提供的服务器还是Servlet实例

### JSP的九个内置对象

```
//如果没有的话，要添加Tomcat的依赖
```

* **out**（JspWriter）：等同与response.getWriter()，用来向客户端发送文本数据；

* **config**（ServletConfig）：对应Servlet中的ServletConfig；（保存 servlet 配置信息）    

* **page**（当前JSP的类型 Object 类型）：当前JSP页面的“this”，即当前对象；（当前页面是什么类型就是什么类型）  

* **pageContext**（PageContext）：页面上下文对象，它是最后一个没讲的域对象；（page域对象）

* **exception**（Throwable）：只有在错误页面中可以使用这个对象；（异常对象）   

* **request**（HttpServletRequest）：即HttpServletRequest类的对象； 

* **response**（HttpServletResponse）：即HttpServletResponse类的对象；

* **application**（ServletContext）：即ServletContext类的对象；   

* **session**（HttpSession）：即HttpSession类的对象，不是每个JSP页面中都可以使用

### 变量

* 定义变量是和JAVA语法一样，变量的声明一定要在使用前面

* 接受变量语法:
  	<%=变量名%>
  	<%=常量值%>
  
* 局部变量与成员变量
  	<% 类型 变量名 = 值 %> 这种方式声明的是局部变量
  	<%!类型 变量名 = 值 %> 这种方式声明的是成员变量

### 域对象

**对象名**						**类型**								**作用范围**
pageContext			pageContext			     jsp当前页面
request					 HttpServletRequest     一次请求
session 					Session 				          一次会话
application 			 ServletContext			 整个服务器范围



从上到下，范围以此增大



域对象的通用方法
    setAttribute(String key, Object value) //设置数据
    getAttribute(String key) //通过key获取数据
    removeAttribute(String key) //通过key删除数据
    getAttributeNames() //获取所有的key

### Cookie

#### 什么是cookie

cookie是由键值对构成的，随着服务器端的响应发送给客户端浏览器，客户端浏览器会把cookie保存起来，当下一次再访问服务器的时候，把cookie再发送给服务器大小上线为4KB，一个服务器最多存在20个cookie



cookie是由服务器创建，然后通过响应发送给客户端的一个键值对
客户端保存cookie,并标注cookie的来源
当客户端向服务器发出请求时会把所有这个服务器的cookie包含在请求信息中发送给服务器
这个服务器就可以识别客户端

#### 创建cookie

```java
Cookie cookie = new Cookie(String key, String val) //cookie的value中不能有空格
response.addCookie(cookie); //一定要要把cookie添加到响应

//获取所有cookie
Cookie[] cookies = request.getCookies();
if(cookies != null){
for(Cookie each : cookies){
    cookie.getName()
	cookie.getValue()
}
```

#### cookie方法

* cookie方法(先调用方法设置cookie属性，然后再添加进响应):
* setMaxAge(int time) //设置最长存活时间,单位是秒,默认是-1,它表示浏览器关闭时到期,设置为0就是立马关闭，设置为正数就是结束时间
* setDomain(String pattern) //设置cookie存在的域名
* setHttpOnly(boolean httpOnly) //设置cookie是否可以跨域
* setPath(String uri) //设置cookie的有效路径，可以设置当访问哪个路径时才会发送cookie,例如 cookie.setPath("/a")

有set方法就有对应的get方法



#### Cookie的生命周期

​	在Cooie对象中有一个setMaxAge(int time)方法，它是用于设置Cookie的生命周期的方法
​	time默认是-1,这时表示它的生存时间是当前会话，关闭浏览器就销毁
​	time=0 让cookie失效
​	time>0 指定cookie的有效时间，单位是秒

#### Cookie存储中文数据的问题

由于中文和英文的字符不同，在Cookie中进行存储中文的时候需要对它进行编码，否则会出现乱码的问题，可以使用以下两个方法对key和name进行操作

java.net.URLEncoder.encode(str,encoding) //进行编码

java.net.URLDecoder.decode(str,encoding)//进行解码

#### 小结

1.cookie是通过http响应和请求在客户端和服务器之间传递的
    响应头: set-cookie:key=value;key=value
    请求头: cookie:key=value;key=value
2.发送重复的cookie，会覆盖原来的cookie

### Session

#### 什么是Session

什么是Session
	session jsp 域对象之一，代表的是一次会话,它是客户端和服务器之间联系的一个对象,是服务器端会话技术，在服务器端保存数据在一次会话中(第一次请求服务器到	浏览器关闭)的多次请求中可以共享session中的数据,session内部数据的使用范围是一次会话(只要浏览器不关，session的数据一直存在)

##### session的生命周期

​	当有用户第一次请求时会创建Session，当用户关闭浏览器时再次访问会创建新的Session，但是它之前的Session是不会消亡的，而是保存在服务器的内存当中，等时间到了才会消亡，在Tomcat服务器上,Session的默认存活时间是30分钟，因为在Tomcat的配置文件web.xml中有配置，我们可以进行修改，那个标签为session-config或者通过session对象的setMaxInactiveInterval(time) 方法进行修改，单位为秒,除了关闭浏览器以外，还有长时间未发送请求也会导致Session的消亡

##### 浏览器和服务器之间如何维护session

浏览器第一次请求服务器时，服务器会调用request.getSession()方法，创建一个Session对象，每次服务器只要创建一个session对象就会偷偷地创建一个名为JSESSIONID的Cookie返回，这个key的值是新创建出来的session的ID值，然后在响应信息里面添加一个set-Cookie请求头，浏览器收到Cookie之后保存，后面访问中都会把这个cookie添加到请求中发送，当服务器收到请求后会从请求头中获取session的id值，然后去内存中查找，然后去获取对应的session对象

##### session常用方法

* setMaxInactiveInterval(int time) //设置多久没有访问就失效,单位是秒
* invalidate() //让session立即失效

##### 什么情况下Session会失效

1.超过了设置的超时时间
2.程序主动调用了session.invalidate()方法
3.服务器主动或异常关闭

注：浏览器的关闭并不会让session立刻销毁

### cookie 和session的区别

- **cookie** 数据存放在客户端浏览器上，**session **的数据存放在服务器上
- **cookie **数据不安全，**session **的数据安全
- **cookie **的对服务器的性能要求很低，**session **占用服务器性能
- **cookie **保存的数据大小不能超过4k，很多浏览器一个站点最多保存20个cookie，**session **可以存储任意大小数据



### 文件上传 & 文件下载

#### 文件上传

#### 文件下载

#### 断点续传

## el表达式

### 什么是el表达式

el是一门表达式语言，它与java无关(脚本语言)
作用: 简化jsp的<%=%>(取值操作)

格式: ${变量名}
在这里面可以做运算

${empty""} 判断字符串长度是否为0,是返回true,不是返回false
${empty key} 判断根据key获取的值是否为空
el不显示null值

### el常用内置域对象(无需创建直接使用)

* pageScope
* requestScope
* sessionScope
* 
  applicationScope	

### el内置域对象的作用
​	内置域对象只负责取值，不存值
​	通过域对象.key的方式直接取值,${requestScope.key}

​	el全域查找:当el内置对象省略时，会从page,request,session,application 四个域中依次查找key的值(从小到大)

​	拿值的过程实际上是调用了 存储对象的gatter()方法

​	${pageContext.request.contextPath} //快速获取当前项目名

### el常用函数
​	${fn:length(var1)} //获取数组或集合或字符串的长度
​	${fn:indexOf("key","value")} //字符在字符串中的下标位置
​	${fn:substring("str",start,end)} //截取字符串不包括end
​	${fn:join(arr,"&")} //分隔符

## JSTL

### 什么是JSTL

jstl 是 apache的应用，依赖于EL, 使用jstl需要导包



jstl的四个库:
	> 核心库: core,重点
	> 格式化库: fmt,对数组和日期的格式化
	> sql库 //不用
	> xml库 //不用



### jstl使用
​	第一步: 导包
​	第二部: 引入依赖
​		在类外部写入以下代码,uri用于导入核心库，注意地址，别导错了，prefix用于给这个库取个别名，可以自定义，但是约定俗成的用 c
​		<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
​		<%@ taglib prefix="fm" uri="http://java.sun.com/jsp/jstl/fmt" %>

### 常用标签
​	1.**set标签:向域中存值，默认向page域中存值**
​		`<c:set var="变量名" value="值" scope="域"></c:set>`
​	2.**out标签:输出标签**
​		`<c:out value="输出内容(字符串/EL表达式)" default="当value为null是输出的内容"></c:out>`
​		value写null 或者 ""，都不会输出defalut，估计是只能EL表达式取不到值了
​	3.**remove标签:删除域中的变量**
​		`<c:remove var="变量名" scope="域"></c:remove>`
​	4.**if标签:判断**
​		`<c:if test="${布尔运算}">要输出的内容</c:if>`
​		 test:boolean，值由EL输出,当为true，输出if标签中间的内容,false不输出中间内容
​	5.**choose when标签:类似于java中的 if else if else**

```
<c:choose>
    <c:when test="${score >= 80}">优秀</c:when>
    <c:when test="${score >= 70}">一般</c:when>
    <c:when test="${score >= 60}">及格</c:when>
</c:choose>
```

​    6.**forEach标签:有两个用法**

```
 	1.循环值
    		<c:forEach var="变量名" begin="开始值" end="结束值(能取到)" step="步进值">${i}<br/></c:forEach>
    	2.遍历数组或集合
    		 <c:forEach items="数组或集合" var="迭代变量名" varStatus="状态变量">
        		${name}:${status.index}<br/>
    		</c:forEach>
    		状态变量有几个属性
    			count:当前循环元素的个数
    			index:循环元素的下标
```

### 格式化数字

```
fmt:formatDate标签:格式化日期
		<fmt:formatDate value="Data类型变量" pattern="yyyy-MM-dd HH:mm:ss"></fmt:formatDate>
fmt:formatNumber标签:格式化数字
		<fmt:formatNumber value="变量" pattern="格式(0.000 或者 #.### 就是保留小数点后三位)"></fmt:formatNumber>	
```

## JSON

### 什么是JSON

```
JSON(JavaScript Object Notation 对象表示法)
JSON是存储和交换文本信息的语法，类似于XML
但是JSON比XML更小，更快，更易解析
```

### JSON实例

```
{
	"sites":[
        {"name":"张三","age":18},
        {"name":"李四","age":19}
	]
}

//很容易看出来，json中全是键值对
```

### JSON语法

```
语法规则:
	数据在名称/值对中
	数据由逗号分隔
	大括号保存对象{}
	中括号保存数组[],数组中可以包含多个对象
	
	对象保存在{}中，里面都是key:value的格式
		key必须是字符串
		value可以是: 字符串，数字，对象，数组，布尔值，null
```

### 为什么用JSON

```
1.JSON是解析很方便，JavaScript就可以解析JSON
2.JSON数据可以通过AJAX传输
```

### JAVA处理JSON数据

```
使用FastJson库，这个库需要导包，它可以把JSON转换为JAVA对象，把JAVA对象转换为JSON字符串
方法:
	JSON.toJSONStringWithDateFormat(list,"yyyy-MM-dd") //传递对象或集合，并进行格式化
	JSON.toJSONString(Object object) //传递对象或集合
	JSON.parseObject(json数据,类.class)
```

## AJAX(JQuery实现)

```
$.ajax({
    type:"POST",  //提交数据的类型 POST GET
    url:"testLogin.aspx", //提交的网址
    data:{Name:"sanmao",Password:"sanmaoword"},//提交的数据
    datatype: "html", //"xml", "html", "script", "json", "jsonp", "text".返回的数据格式类型
    beforeSend:function(){$("#msg").html("logining");},//在请求之前调用的函数 
    success:function(data){$("#msg").html(decodeURI(data));}, //成功返回之后调用的函数
    complete: function(XMLHttpRequest, textStatus){ //调用执行后调用的函数
    alert(XMLHttpRequest.responseText);
    alert(textStatus);
    }, 
    error: function(){  //调用出错执行的函数
    //请求出错处理
    }         
});


$.get(url, [data], [callback], [type])
$.post(url, [data], [callback], [type])
```

## Filter

### 什么是Filter

```
Filter为过滤器，它可以过滤所有WEB资源

过滤器的本质是一个JAVA类

作用:
	1.向web应用程序的请求和响应添加功能的web服务器组件
	2.过滤器可以统一集中处理请求和响应
	3.使用过滤器技术实现对请求数据过滤
	
使用步骤:
	实现javax.servlet.Fileter 接口
		重写 init, doFilter, destroy 三个方法
	配置web.xml
		<filter>
			<filter-name>过滤器名</filter-name>
			<filter-class>路径</filter-class>
		</filter>
		<filter-mapping>
			<filter-name>过滤器名</filter-name>
			<url-pattern>/访问路径</url-pattern> //过滤访问路径
			<servlet-name>servlet名</servlet-name> //对指定servlet过滤，指定多个servlet需要写多个servlet-name
		<filter-mapping>

使用WebFilter进行配置
	@WebFilter("/path") //直接对path路径进行过滤
	它里面的其他参数
		servletNames = {"servletName1","servletName2"} //通过servlet名设置过滤，可以设置多个
		urlPattern={"uri","uri"} //通过urlPattern配置多个过滤访问路径
		filterName= "" //设置过滤器名
		initParams = {
			@WebInitParam(name="",value=""),
			@WebInitParam(name="",value="")
		} //设置初始化参数

xml和注解的优先级
    xml配置的过滤器优先级高于注解
    注解与注解的之间的优先级是按照Filter的类名来排序的，小的先执行
    包之间的注解，按照包名来排序，小的先执行
```

### 过滤器的生命周期

```
当服务器启动时，创建filter对象，调用Init方法完成初始化，只执行一次

每拦截一次请求，doFilter方法就执行一次

关闭服务器时，调用destroy方法释放资源
```

### 执行流程

```
启动服务器就会调用Filter的init方法，完成初始化

当有请求到来时，会先执行过滤代码，当满足条件后执行,filterChain.doFilter(servletRequest,servletResponse);方法就会进入到对应的Servlet对象中执行它的service方法，在执行完service方法过后，就会回到filterChain.doFilter()方法的位置继续执行它下面的代码

在服务器关闭时会调用destroy方法

当有多个Filter时，按照xml中的顺序来执行doFilter方法
```

### 初始化参数

```
web.xml
    <filter>
        <init-param>
            <param-name>参数名</param-name>
            <param-value>参数值</param-value>
        </init-param>
    </filter>
注解
	@WebFilter(initParams = {
            @WebInitParam(name="key",value="value"),
            @WebInitParam(name="key2",value="value2")
		}
	)

使用初始化参数
	在filter的init方法中有个FilterConfig对象，我们可以通过声明一个私有的成员变量来获取这个对象
```

## Listener

### 什么是Listener

```li
Listener为监听器
	
监听器本质是JAVA类

作用:
	监听三大域中的对象变化(对象的出生和死亡,对象中属性的增删改)当对象发送变化时，服务器会自动调用监听器的方法

分类:
	ServletContext(下有两个):
		ServletContextListener: 监听服务器对象的出生和死亡
		ServletContextAttributeListener: 监听属性的增删改
	ServletRequestListener: 监听请求对象
	ServletSessionListener: 监听session对象

实现步骤:
	创建ServletContextListener的监听器
	实现javax.servlet.ServletContextListener
	编写xml配置
		<listener>
			<listener-class>监听器的路径</listener-class>
		</listener>
		当服务器启动时就会启动监听器，无需其他配置
	重写	contextInitialized 和 contextDestroyed 方法
	当ServletContext对象被创建时就会调用contextInitialized方法
	销毁时就会调用contextDestroyed方法
```