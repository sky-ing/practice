
####EL
#####传统方式获取作用域数据
+ 导入包
+ 需要强转
+ 获取数据的代码过于麻烦
``` html
  <h3>EL表达式学习：使用传统方式获取作用域对象的数据</h3>
  <b><%=request.getParameter("uname")%></b><br />
  <b><%=request.getAttribute("str") %></b><br />
  <b><%=((User)request.getAttribute("user")).getAddr().getTown() %></b><br />
  <b><%=((ArrayList)request.getAttribute("list")).get(2)%></b><br />
  <b><%=((User)(((ArrayList)request.getAttribute("list2")).get(0))).getAddr().getPre() %></b><br />
  <b><%=((HashMap)request.getAttribute("map")).get("c") %></b><br />
  <b><%=((User)(((HashMap)request.getAttribute("map2")).get("a1"))).getAddr().getCity() %></b><br />

  <h3>EL表达式学习：使用EL表达式获取作用域对象的数据</h3>
  <b>${param.uname}</b><br />
  <b>${paramValues.fav[0]}</b><br />
  <b>${str}</b><br />
  <b>${user}</b><br />
  <b>${list[2]}</b><br />
  <b>${list2[0].addr.pre}</b><br />
  <b>${map.c}</b><br />
  <b>${map2.a1.addr.city}</b><br />
  <b>-${str2}</b>
```

#####定义
全称：Expression Language，一种写法非常简介的表达式。语法 简单易懂，便于使用。表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言

#####作用
+ 获取作用域数据：
  - 注意：获取的事pageContext、request、session、application四个对象中的数据，其他数据一概不理，找到了则返回，找不到则什么都不做，也不报错
+ 让jsp书写起来更加的方便。简化在jsp中获取作用域或者请 求数据的写法。也会搭配Jstl来进行使用。

#####语法
+ 语法结构：${表达式},提供.和[ ]两种运算符来存取数据。
+ 表达式：
  - 获取请求数据
    - request对象存储了请求数据-->param. 返回值
    - request对象存储了请求数据-->paramvalues.键名 返回的是数组
  - 通过setAttribute方法存储到作用域对象中的数据
    - ${键名} 返回键名所对应的值
    - 获取对象中的数据：
      - 普通对象{键名.属性名.属性名...}
      - 集合对象
        - list集合--->${键名[角标]}
        - map集合--->${键名.map集合存储的键名}
    - 注意：
      - 如果存储的是普通字符串则直接返回
      - 如果存储的是对象，则返回的是对象
  - 请求头数据：
    - ${header}-->返回所有的请求头数据
    - ${header["键名"]}--->返回指定的键名的请求头数据
    - ${hedaerValues["键名"]}--->返回指定的键名(同键不同值)的值的数组。
  - 获取cookie数据
    - ${cookie}--->返回存储了所有的cookie对象的map集合
    - ${cookie.键名}---->返回指定的cookie对象
    - ${cookie.键名.name}--->返回指定的cookie对象存储的数据的键名
    - ${cookie.键名.value}--->返回指定的cookie对象存储的数据的值。

#####作用域查找顺序:
- pageContext-->request-->session-->application
- 注意：每次查找都是从小到大进行查找，找到了则获取，不再继续找了
- 制定域查找:
  - ${pageScope.键名}
  - ${requestScope.键名}
  - ${sessionScope.键名}
  - ${applicationScope.键名}

#####EL表达式的逻辑运算
+ ${逻辑表达式}：&& || ！
+ ${算术表达式}：+ - * %
+ ${关系表达式}：> < >= <= == != %
+ 特殊：三目表达式
+ 注意：+表示加法运算，不表示字符链接。使用EL必报大师进行字符链接会报错，'1'会当做数字1，'a'会报错

#####EL表达式空值判断empt
+ %{empty s}判断键名对象的值是否存有数据，返回true/false
+ new 对象 认为不为空，返回true，对象属性有默认值

#####使用：
- a) 使用EL表达式获取请求数据
  - i. 获取用户请求数据
  - ii. 获取请求头数据
  - iii. 获取Cookie数据
- b) 使用EL表达式获取作用域数据
  - i. 获取作用域数据
  - ii. 作用域查找顺序
  - iii. 获取指定作用域中的数据
- c) 使用EL表达式进行运算
  - i. 算术运算
  - ii. 关系运算
  - iii. 逻辑运算
- d) EL表达式空值判断empt


####JSTL
#####定义
JSTL是apache对EL表达式的扩展（也就是说JSTL依赖EL）， JSTL是标签语言！JSTL标签使用以来非常方便，它与JSP动作 标签一样，只不过它不是JSP内置的标签，需要我们自己导包，以 及指定标签库而已！jstl-1.2.jar

#####作用
提高在jsp中的逻辑代码的编写效率，使用标签，依赖于EL，取变量也是在四个域内

#####使用：
+ JSTL的核心标签库（重点）
+ JSTL的格式化标签库（讲解）
+ JSTL的SQL标签库（了解）
+ JSTL的函数标签库（了解）
+ JSTL的XML标签库（了解）

#####JSTL的核心标签库：
1. 导入jar包
2. 声明jstl标签库的引入（核心标签库）
  + <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
3. 内容：
+ 基本标签：
  - <c:out value="数据" default="默认值"></c:out>
    - 数据可以为常量值也可以是EL表达式。
    - 作用：将数据输出给客户端。
  - <c:set var="hello" value="hello pageContext" scope="page"></c:set>
    - 作用：存储数据到作用域对象中
    - var：表示存储的键名
    - value：表示存储的数据
    - scope：表示要存储的作用域对象 page request session application
  - <c:remove var="hello" scope="page"/>
    - 作用：删除作用域中的指定键的数据。
    - var：表示要删除的键的名字
    - scope：表示要删除的作用域（可选）
    - 注意：如果在不指定作用域的情况使用该标签删除数据，会将四个作用域对象中的符合要求的数据全部删除。
+ 逻辑标签：
``` html
  <c:if test="${表达式}">
  				前端代码
  		</c:if>
```
  - 作用：进行逻辑判断，相当于java代码的单分支判断。
  - 注意：逻辑判断标签需要依赖于EL的逻辑运算，也就是表达式中涉及到的数据必须从作用域中获取。
``` html
  <c:choose>
  	<c:when test="">执行内容</c:when>
  	<c:when test="">执行内容</c:when>
  	...
  	<c:otherwise>执行内容</c:otherwise>
  </c:choose>
```
  - 作用：用来进行多条件的逻辑判断，类似java中的多分支语句
  - 注意：条件成立只会执行一次，都不成立则行otherwise

  ``` html
    <!-- 循环标签: -->
  	<c:forEach begin="1" end="4" step="2">
  			循环体
  	</c:forEach>
    <!--遍历实体  -->
    <c:forEach items="${list}" var="s">
  		<tr>
  			<td>${s.name}</td>
  			<td>${s}</td>
  			<td>${s}</td>
  			<td>${s}</td>
  		</tr>
  	</c:forEach>
    <!--遍历map集合  -->
    <c:forEach items="${map}" var="m">
    	${m.key}--${m.value} <br />
    </c:forEach>
  ```
  - 作用：循环内容进行处理
  - 使用:
    - begin:声明循环开始位置
    - end:声明循环结束位置
    - step：设置步长
    - varStatus:声明变量记录每次循环的数据(角标，次数，是否是第一次循环，是否是最后一次循环)
      - 注意:数据存储在作用域中，需要使用EL表达式获取。
      - 例如：${vs.index}--${vs.count}--${vs.first}--${vs.last}
    - items:声明要遍历的对象。结合EL表达式获取对象
    - var:声明变量记录每次循环的结果。存储在作用域中，需要使用EL表达式获取。
