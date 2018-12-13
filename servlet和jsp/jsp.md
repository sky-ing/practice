
####JSP
#####概念：
JSP 全名为 JavaServerPages，中文名叫 java 服务器页面，其根
本是一个简化的 Servlet 设计，它 [1] 是由 SunMicrosystems 公司
倡导、许多公司参与一起建立的一种动态网页技术标准。

#####特点：
  - 本质上还是 Servlet
  - 跨平台，一次编写处处运行
  - 组件跨平台
  - 健壮性和安全性


#####Jsp的访问原理：
浏览器发起请求，请求 JSP，请求被 Tomcat 服务器接收，执行
JspServlet 将请求的 JSP 文件转义成为对应的 java 文件(也是
Servlet)，然后执行转义好的 java 文件。

#####Jsp 的语法和指令：
+ JSP 的 page 指令：配置jsp转译相关参数
    - <%@page 属性名=“属性值” 属性名=“属性值” ...%>
    - language:声明jsp要被转译的语言
    - import:声明转译的java文件要导入的包，不同的包使用逗号隔开
    - pageEncoding:设置jsp文件的数据编码格式
    - contentType="text/html; charset=utf-8":设置jsp数据响应给浏览器时，浏览器的解析和编码格式
    - session：true/false设置转译的servlet中是否开启session支持，默认开启。
    - errorPage：设置jsp运行错误跳转的页面
    - extends：设置jsp转译的java文件要继承的父类（包名+类名）
+ JSP 的 taglib 指令
+ Jsp 的局部代码块
  - 特点：
    - 局部代码块中声明的java代码会被原样转译到jsp对应的servlet文件的_JspService方法中
    - 代码块中声明的变量都是局部变量
  - 使用：<% java代码 %>
  - 缺点：使用局部代码块在jsp中进行逻辑判断，书写麻烦，阅读困难
  - 开发：servlet进行请求逻辑处理，使用jsp进行页面展现
+ Jsp 的全局代码块
  - 特点：
    - 声明的java代码作为全局代码转译到对应的servlet类中
  - 使用：<%! java代码 %>
  - 注意：全局代码块声明的代码，需要使用局部代码块调用
+ Jsp 的脚本段语句
  - 特点：帮助我们快速的获取变量或者方法的返回值作为数据响应给浏览器
  - 使用：<%=变量名或者返回值的方法%>
  - 注意：不要在变量名或者方法后使用分号
  - 位置：除jsp语法幽囚以外的任意位置
+ Jsp 的注释
  + 前端语言注释<!-- -->:
    - 会被转译，也会被发送，但是不会被浏览器执行
  + JAVA语言注释：
    - 会被转译，但是不会被servlet执行
  + Jap注释<%-- --%>：
    - 不会被转译
+ Jsp 的静态引入(JSP 的 include 指令)
  - 用法：<%@include file="要引入的jsp文件的相对路径" %>
  - 特点：
    - 会将引入的jsp文件和当前jsp文件转译成一个java（Servlet）文件使用
    - 在网页中也就显示了合并后的显示效果
  - 注意：
    - 今天太引入的jsp文件不会单独转译成java(servlet)文件
    - 当前文件和静态引入的jsp文件中不能够使用java代码声明同名变量
+ Jsp 的动态引入
  - 用法：<jsp:include page="要引入的jsp文件的相对路径"></jsp:include>
  - 特点：
    - 会将引入的jsp文件单独转译，在当前文件转译好的java文件中调用引入的jsp文件的转译文件
    - 在网页中显示合并后的显示效果
  - 注意：动态引入允许文件中声明同名变量
+ 静态引入和动态引入优点：降低jsp代码的冗余，便于维护升级
+ 页面转发(forword 标签)
  - 使用：<jsp:forward page="要转发的jsp文件的相对路径"></jsp:forward>
  - 特点：
    - 一次请求
    - 地址栏信息不改变
  - 注意：
    - 在转发标签的两个标签中间除了写<jsp:param value="aaa" name="str"/>子标签不会报错，其他任意字符都会报错
    - <jsp:param value="aaa" name="str"/>：name属性为附带的数据的键名，value为附带的数据内容，会将数据以？的形式拼接在转发路径的后面

#####Jsp 的内置对象：
+ 定义：jsp文件在转译成其对应的Servlet文件的时候自动生成的并声明的对象。我们jsp页面中直接使用即可
+ 注意：内置对象在jsp页面中使用，使用局部代码块或者脚本段语句来使用。不能够在全局代码块中使用
+ 内容：9个对象
  - PageContext 对象：
    - 定义：页面上下文对象，封存了其他内置对象。封存了当前jsp的运行信息
    - 注意：每个jsp文件单独拥有一个pageContext对象
    - 作用域：当前页面
  - Request 对象：封存当前请求数据的对象。有tomcat服务器创建。一次请求
  - Session 对象：此对象用来存储用户的不同请求的共享数据的。一次会话
  - Application 对象：也就是ServletContext对象，一个项目只有一个，存储用户共享数据的对象，以及完成其他操作。项目内
  - Response 对象：响应对象，用来响应请求处理结果给浏览器的对象。设置响应头，重定向
  - Out 对象：响应对象，JSP内部使用。带有缓冲区的响应对象，效率高于response对象（out.write("")）
  - Page 对象：代表当前jsp的对象。就是java中的this
  - Exception 对象：
    - 定义：异常对象，存储了当前运行的异常信息
    - 注意：使用此对象需要在page制定中使用属性isErrorPage="true"开启
  - Config 对象：也就是servletConfig，主要用来获取web.xml中的配置数据，完成一些初始化数据的读取

#####使用：
+ JSP 负责页面展现，Servlet 负责业务逻辑处理。

#####资源路径总结：
+ Jsp 中路径：
  - 相对路径:../../资源
  - 绝对路径:/虚拟项目名/资源路径

#####四个作用域对象（作用：数据流转）：
+ pageContext：当前页面。解决了在当前页面内的数据共享问题。获取其他内置对象
+ request:一次请求。一次骑牛的servlet的数据共享。通过请求转发，将数据流转给下一个servlet
+ session:一次会话。一个用户的不同请求的数据共享。将数据从一次请求流转给其他请求
+ application:项目内，不同用户的数据共享将数据从一个用户流转给其他用户

#####JSP路径问题
+ 在jsp中资源路径可以使用相对路径完成跳转
  - 问题1：资源的位置不可随意更改
  - 问题2：需要使用../进行文件夹的跳出。使用比较麻烦
+ 使用绝对路径（/开头代表服务器根目录loccalhost:8080）：
  - /虚拟项目名/项目资源路径
    - 例如：<a href="/jsp/jspPro.jsp">jspPro.jsp</a>
+ 使用jsp中自带的全局路径声明：
``` jsp
  <%
  String path = request.getContextPath();
  String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
  %>
  <base href="<%=basePath%>"><%-- 给资源前面添加项目路径 --%>
```
