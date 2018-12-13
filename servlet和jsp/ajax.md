
####AJAX
#####定义
+ AJAX全称为“Asynchronous JavaScript and XML”（异步JavaScript和XML），是一种创建交互式网页应用的网页开发技术。
+ 不是一种新技术，是如下几种技术的组合应用：
  - 基于web标准（standards-based presentation）XHTML+CSS的表示；
  - 使用 DOM（Document Object Model）进行动态显示及交互；
  - 使用 XML 和 XSLT 进行数据交换及相关操作；
  - 使用 XMLHttpRequest 进行异步数据查询、检索；
  - 使用 JavaScript 将所有的东西绑定在一起。
+ Ajax本质上是一个浏览器端的技术。

#####原理:
+ 请求由ajax引擎对象发送，响应数据，浏览器不会直接进行处理，而是流转给发请求的ajax引擎对象。
+ 这样我们可以通过操作ajax引擎对象变相的实现在页面中显示新的响应资源。

#####本质：
js的DOM操作中的数据由程序员自己写死声明，变成从服务器动态的获取

#####状态码
+ ajax状态码:
  - readyState：
    - 0:表示XMLHttpRequest已建立，但还未初始化，这时尚未调用open方法
    - 1:表示open方法已经调用，但未调用send方法（已创建，未发送）
    - 2:表示send方法已经调用，其他数据未知
    - 3:表示请求已经成功发送，正在接受数据
    - 4: 表示响应内容被成功接收
+ 响应状态码:
  - status
    - 200:表示一切OK
    - 404:资源未找到
    - 500：内部服务器错误
    -
#####使用：
1. 创建ajax引擎对象
``` js
  var ajax;
		if(window.XMLHttpRequest){//火狐
			ajax=new XMLHttpRequest();
		}else if(window.ActiveXObject){//ie
			ajax=new ActiveXObject("Msxml2.XMLHTTP");
		}
```
2. 声明ajax监听函数
``` js
  //复写onreadystatement函数
	ajax.onreadystatechange=function(){
		//判断Ajax状态吗
		if(ajax.readyState==4){
			//判断响应状态吗
			if(ajax.status==200){
				//获取响应内容
				var result=ajax.responseText;
				//处理响应内容
					//获取元素对象
					var showdiv=document.getElementById("showdiv");
					showdiv.innerHTML=result;
			}else if(ajax.status==404){
				//获取元素对象
				var showdiv=document.getElementById("showdiv");
				showdiv.innerHTML="请求资源不存在";
			}else if(ajax.status==500){
				//获取元素对象
				var showdiv=document.getElementById("showdiv");
				showdiv.innerHTML="服务器繁忙";
			}
		}else{
			//获取元素对象
			var showdiv=document.getElementById("showdiv");
			showdiv.innerHTML="<img src='img/2.gif' width='200px' height='100px'/>";
		}
	}
```
3. 创建并发送请求
``` js
		ajax.open("get","ajax",true);
		ajax.send(null);
```
  + 解释:
    - method：表示请求的方式，值为get/post
    - url:表示请求地址，一般为要请求的servlet的别名。
    - async:表示异步还是同步请求， true表示异步， false表示同步，默认为异步。
  + 注意：
    - 如果请求方式是get方式，则请求数据需要拼接在url的后面，以？隔开，键值对。并且send中要写null
    - 如果是post请求方式，则在send方法中书写请求数据即可。并且要声明数据的提交格式为键值对。
  + 示例：
    ``` js
      //使用get方式
      ajax.open("get","user?uname="+uname,true);
      ajax.send(null);

      //post方式
      ajax.open("post","user", true);
      //设置请求数据的格式
      ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded
      ");
      ajax.send("un="+uname);
    ```
4. ajax的响应数据格式
  + 普通文本：后台在接收到ajax请求后，处理后直接响应普通字符串给ajax
  + josn数据：
    + 后台在接收到ajax请求后，处理后响应json格式的字符串给ajax
    + ajax处理代码中使用eval()方法将json数据转换为js对的对象，将对象中的数据通过js的dom操作显示到页面中。
    + 注意：
      - json格式：
        var 对象名={
        键名:值,
        键名:值,
        ........
        }
  + xml数据：
    - 后台在接收到ajax请求后，处理后响应xml格式的字符串给ajax。
    - 前台使用ajax.responseXML进行数据接收，返回的是xml文档对象(document)。
    - 使用document对象将xml中取出并显示到页面中即可
    - 注意：
      - 后台的响应数据格式必须设置成xml格式：
      - resp.setContentType("text/xml;charset=utf-8");
5. 注意：ajax是前端的技术，由浏览器进行解析执行。

#####使用流程
1. 创建ajax引擎对象
2. 覆写onreadystatechange函数
  + 判断数据状态码
    + 判断响应状态码
      + 获取响应数据
      + 处理响应信息
3. 创建并发送请求
4. 封装：相同的保留，不同的传参。最终封装成一个js文件。
