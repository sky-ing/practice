
#####servlet常见错误
+ 404错误（资源未找到）
  - 原因一：在请求地址中的servlet的别名书写错误
  - 原因二：虚拟项目名称拼写错误
+ 500错误（内部服务器错误）
  - 原因一：java.lang.ClassNotFoundExceptio
    - 解决：对应的serlet处理类找不到，类路径是否拼写错误
  - 原因尔：因为service方法体的代码执行错误导致
    - 解决：根据错误提示修改代码
+ 405错误（请求方式不支持）
  - 原因一：请求方式和servlet中的方法不匹配所造成
    - 解决：尽量用service方法进行请求处理，不要在service方法中调用父类的service

#####请求中文乱码问题
1. 使用String进行重新编码(比较保险不会出问题)：
  + uname=new String(uname.getBytes("iso8859-1"),"utf-8");
2. 使用公共配置
  1. get方式
    + 步骤一：req.setCharacterEncoding(“utf-8”)；
    + 步骤二：在tomcat的目录下的conf目录中修改server.xml文件：在Connector标签中增加属性useBodyEncodingForURI="true"
  2. post方式
    + req.setCharacterEncoding(“utf-8”)；

#####Servlet流程总结：
1. 浏览器发起请求到服务器（请求）
2. 服务器接收浏览器的请求，进行解析，创建request对象存储请求数据
3. 服务器调用对应的servlet进行请求处理，并将request对象作为实参传递给servlet的方法
4. servlet的方法执行进行请求处理
  1. 设置请求编码
  2. 设置想要编码格式
  3. 获取请求信息
    1. 获取Cookie信息 req.getCookies();
    2. 获取用户信息
  4. 处理请求信息
    1. 创建业务层对象
    2. 调用业务层对象的方法
  5. 响应处理结果
    1. 直接响应
    2. 请求转发
    3. 重定向

#####请求转发
+ 作用：实现多个servlet联动操作处理请求，这样避免代码冗余，让servlet的职责更加明确
+ 使用：req.getRequestDispatcher("要转发的地址").forward(req,resp);地址：相对路径，直接书写servlet的别名即可
+ 特点：一次请求，浏览器地址栏信息不改变
+ 注意：
  - 请求转发后直接return结束即可
  - 因为是一次请求所以url不变，导致刷新页面会导致频繁提交，会造成表单数据重复提交的不能用请求转发

#####request对象的作用域
+ 作用：基于请求转发，一次请求中的所有 Servlet 共享。解决了一次请求内的servlet的数据共享问题
+ 使用：
  - request.setAttribute(objectname,Objectvalue);
  - request.getAttribute(Objectobj)
  - 作用：解决了一次请求内的不同 Servlet 的数据(请求数据+其他数
据)共享问题
+ 注意：使用 Request 对象进行数据流转，数据只在一次请求内有效。
+ 特点：
  - 服务器创建
  - 每次请求都会创建
  - 生命周期一次请求

#####请求重定向
+ 作用：解决了表单重复提交的问题，以及当前servlet无法处理的请求的问题
+ 使用：
  - response.sendRedirect(“路径”).
  - 本地路径为：uri
  - 网络路径为：定向资源的 URL 信息
+ 特点：
  - 两次请求
  - 浏览器地址栏信息改变
  - 避免表单重复提交

####cookie
+ 作用：解决了发送的不同请求的数据共享问题
+ 使用：
  - 创建Cookie对象 Cookie c=new Cookie(String name, String value);
  - 设置有效期（不设置就是放内存，浏览器关闭就没了，设置了就是存在硬盘里了）c.setMaxAge(秒数)
  - 设置有效路径(不设置就是所有路径都带，只有这个路径才会带此cookie) c.setPath("URI")
  - 响应Cookie信息给客户端 resp.addCookie(c);
  - cookie获取：req.getCookies();然后遍历，记得空判断
+ 注意：一个Cookie对象存储一天数据，多条数据，可以多创建几个Cookie对象进行存储
+ 特点：
  - 浏览器端的数据存储技术
  - 存储的数据声明在服务器端
  - 临时存储：存储在浏览器的运行内存中，浏览器关闭即时效
  - 定时存储：设置了Cookie的有效期，存储在客户端的硬盘中，在有效期内符合路径要求的请求都会附带该信息
  - 默认cookie信息存储好之后，每次请求都会附带，除非设置有效路径


####session
+ 作用：解决了一个用户的不同请求处理的数据共享问题，只要在JSESSIONID不失效和session对象不失效的情况下。用户的任意请求在处理时都能够获取到同一个session对象
+ 作用域：
  - 一次会话
  - 在JSESSIONID和SESSION对象不是小的情况下为整个项目内
+ 原理：
  - 用户第一次访问服务器，服务器会创建一个session对象给此用户，并将该session对象的JSESSIONID使用Cookie技术存储到浏览器中，保证用户的其他请求能够获取到同一个session对象，也就保证了不同请求能够获取到共享数据
+ 特点：
  - 存储在服务器端
  - 服务器进行创建
  - 依赖cookie技术
  - 一次会话
  - 默认存储时间是30分钟
+ 使用：
  - 创建session对象/获取session对象
    - HttpSession hs = req.getSession();
    - 如果请求中拥有session的标识符也就是JSESSIONID.则返回其对应的session队形
    - 如果请求中没有session的标识符也就是JSESSIONID。则创建新的session对象，并将其JSESSIONID作为从cookie数据存储到浏览器内存中
    - 如果session对象失效了，也会重新创建一个session对象，并将其JSESSIONID存储在浏览器内存中
  - 设置session存储时间
    - hs.setMaxInactiveInterval(int seconds)
    - 注意：在制定的时间内session对象没有被使用则销毁，如果使用了则重新计时
  - 设置session强制失效
    - hs.invalidate
  - 存储和获取数据
    - 存储：hs.setSttribute(g name,Object value);
    - 获取：hs.getAttribute(String name);
    - 注意：存储的动作和取出的动作发生在不同的请求中，但是存储要先于取出执行
  - 使用时机
    - 一般用户在登录web项目时会将用户的个人信息存储到Session中，供该用户的其他请求使用
+ session失效处理：
  - 将用户请求中的JSESSIONID和后台获取到的SESSION对象的JSESSSIONID进行对比，如果一致则session没有失效，如果不一致则证明session失效了。重定向到登录页面，让用户重新登录
+ 注意：
  - JSESSIONID存储在了Cookie的临时存储空间中，浏览器关闭即失效


####ServletContext
+ 作用：解决了不同用户的数据共享问题
+ 原理：ServletContext对象由服务器进行创建，一个项目只有一个对象。不管在项目的任意位置进行获取得到的都是同一个对象，那么不同用户发起的请求获取到的也就是同一个对象了，该对象由用户共同拥有
+ 特点：
  - 服务器进行创建
  - 用户共享
  - 一个项目只有一个
+ 生命周期：服务器启动到服务器关闭
+ 作用域：项目内
+ 使用
  - 获取ServletContext对象
    - ServletContext sc = this.getServletContext();
    - ServletContext sc2 = this.getServletConfig().getServletContext();
    - ServletContext sc3 = req.getSession().getServletContext();
  - 使用ServletContext对象完成数据共享
    - 数据存储：sc.setAttribute(String name,String value);
    - 数据获取：sc.getAttribute(String name);返回的是Object类型
    - 注意：不同的用户可以给ServeltContext对象进行数据的存储
  - 获取web.xml文件中的全局配置数据
    - 配置：注意一组<context-param>标签只能存储一组键值对数据，多组用声明多个<context-param>
      - <context-param><param-name></param-name><param-value></param-value></context-param>
    - 获取
      - sc.getInitParameter(String name);根据键的名字获取web.xml中配置的全局数据的值，返回String,如果数据不存在返回null
      - sc.getInitParameterNames();返回键名的枚举
    - 作用：将静态数据和代码进行解耦
  - 获取Webroot下的资源的绝对路劲
    - String path = sc.getRealPath("/doc/1.txt");获取路径为项目根目录，参数为项目根目录中的路径
  - 获取Webroot下的资源的流对象
    - InputStream is = sc.getResourceAdStream("/doc/1.txt");
    - 注意：此种方式只能获取项目根目录下的资源流对象，class文件的流对象需要使用类加载器获取

#####getServletConfig相当于servlet的秘书，一个servlet一个servletConfig
+ 作用：获取web.xml中给每个servlet单独配置的数据
+ 使用：
  - 获取servletConfig对象：ServletConfig = this.getServletConfig();
  - 获取web.xml中的配置数据sc.getInitParameter("config");
+ 配置：配置在servlet里面
  - <servlet><init-param><param-name>config</param-name><param-value>utf-8</param-value></init-param></servlet>


#####Web.xml 文件使用总结：
+ 作用：存储项目相关的配置信息，保护Servlet。解耦一些数据对程序的依赖。
+ 使用位置：
  - 每个 Web 项目中
  - Tomcat 服务器中(在服务器目录 conf 目录中)
+ 区别：
  - Web 项目下的 web.xml 文件为局部配置，针对本项目的位置。
  - Tomcat 下的 web.xml 文件为全局配置，配置公共信息。
+ 内容(核心组件)：
  - 全局上下文配置(全局配置参数)
  - Servlet 配置
  - 过滤器配置
  - 监听器配置
+ 加载顺序：Web 容器会按 ServletContext -> context-param -> listener ->
filter-> servlet 这个顺序加载组件，这些元素可配置在 web.xml
文件中的任意位置。
+ 加载时机：服务器启动时。
