---
typora-copy-images-to: img
---

# day29-Ajax-json

# 学习目标

- [ ] 了解Ajax的概念和作用

- [ ] 了解同步和异步的区别

- [ ] 能够使用jQuery的`$.get()`和`$.post()`发送Ajax请求

- [ ] 能够使用jQuery的`$.ajax()`方法发送Ajax请求

- [ ] 能够定义和解析json

- [ ] 能够使用Jackson转换json格式

- [ ] 能够使用fastjson转换json格式

  

# 第一章-JS的AJAX

## 知识点- AJAX的概述

### 1.目标

+ 能够理解异步的概念

### 2.路径

+ 什么是Ajax
+ 什么是异步
+ 为什么要学习Ajax

### 3.讲解

#### 3.1 什么是AJAX

![img](img/tu_2.png)



​	说白了:   ==AJax是可以做异步的请求,实现局部刷新一种客户端技术 ；不会整个页面都刷新（局部刷新）==

**局部刷新示例**：

https://passport.wanmei.com/reg/

输入 zs@qq.com

![1593690965659](img/1593735532168.png)

#### 3.2什么是异步



- 同步  

  ​	

![img](img/tu_3.png)

- 异步

  
  
  ![img](img/tu_4.png)



#### 3.3为什么要学习AJAX   

​	提升用户的体验。(异步)

​	实现页面局部刷新。

### 4.小结

1. ajax： 概念，异步 的js和xml
2. 作用： 
   1. 异步--提升用户体验
   2. 局部刷新





## 知识点-JS的Ajax入门【了解】

### 1.目标

​		在网页上点击按钮, 点击按钮, 发送Ajax请求服务器,响应 success!

### 2. 路径

- API
- get请求
- post请求

### 3.讲解

#### 3.1涉及到的API

##### 3.1.1 获取XMLHttpRequest对象

​	**不同的浏览器对该对象的创建的方式不一样**，MS微软的IE浏览器，比较早的浏览器，创建这个对象的时候将这个对象封装到ActiveXObject的插件中。像火狐或者谷歌浏览器则直接new出来。

```javascript
// 这段代码固定写法，抄就行了
var xmlHttp = null;
if (window.XMLHttpRequest) {// all modern browsers
    xmlHttp = new XMLHttpRequest();
} else if (window.ActiveXObject) {// for IE5, IE6
    xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```

##### 3.1.2 XMLHttpRequest的对象的API

- 方法

​	==**open(请求方式,请求url)**==：打开连接。传递三个参数。第一个是请求方式(GET/POST)，第二个是请求路径，第三个是否是异步的(默认就是异步,不需要这个参数)。示例： xmlHttp.open("GET", "linkman?method=findAll")

​	==**send([post请求的参数])**==: 发送请求。  示例： xmlHttp.send()；  get请求send就不用参数，post可以在send发送参数。

​	setRequestHeader()：解决POST请求参数的问题。 

​			key和值(表单提交参数的形式)， content-type；setRequestHeader("Content-Type", "application/x-www-form-urlencoded")

- 事件属性

​	**==onreadystatechange==**：监听该对象的状态的改变，事件函数： 

xmlHttp.onreadystatechange = function(){

   // 是一个事件监听函数，当**readyState**发生改变则会触发该事件函数

}

​	==**readyState**==：该属性就记录这个对象的状态。属性值如下：

![img](img/tu_5.png)

​	**==status==**：该属性是响应状态码 。值有 200、404、500等整数值

​	**==responseText==: 该属性获得字符串形式的响应数据(响应体)。获取后台返回的数据**

​	responseXML :获得 XML 形式的响应数据(响应体)

#### 3.2.XMLHttpRequest编程步骤

​	第一步：创建ajax对象。

```js
var xmlHttp = null;
if (window.XMLHttpRequest) {// all modern browsers
	xmlHttp = new XMLHttpRequest();
} else if (window.ActiveXObject) {// for IE5, IE6
	xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```
​	第二步：打开连接。

```jsp
xmlHttp.open("GET","demo01?username=zs&password=123456");
```

​	第三步：发送请求。

```js
xmlHttp.send();
```



​	第四步：设置监听对象改变所触发的事件函数,处理结果

```js
xmlHttp.onreadystatechange = function(){
    // readyState=4表示响应完成
    // status =200表示正确响应
    if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
        // responseText可以获得响应的数据
        var result = xmlHttp.responseText;
        alert(result);
    }

};
```



### 3.讲解

#### 3.1GET请求方式的入门

get请求参数直接在url后面拼接

在网页上点击按钮, 点击按钮, 发送Ajax请求服务器,响应success!

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<input type="button" value="js的ajax请求" onclick="sendData()"><br>
</body>

<script>
    // 发送ajax请求
    function sendData() {
        // 1. 获得对象
        var xmlHttp = null;
        if (window.XMLHttpRequest) {// all modern browsers
            xmlHttp = new XMLHttpRequest();
        } else if (window.ActiveXObject) {// for IE5, IE6
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        }

        // 2. 打开连接(请求方式,请求url)
        xmlHttp.open("GET", "demo01?username=zs&password=123");

        //3. 发送请求
        xmlHttp.send();

        //4. 查看响应数据
        xmlHttp.onreadystatechange =  function () {
            // 判断是否完成、正确响应
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                // 拿到数据
                alert(xmlHttp.responseText);
            }
        }
    }


    
</script>
</html>

```

#### 3.2POST请求方式的入门

- 表单上传编码格式为`application/x-www-form-urlencoded`(Request Headers中)，参数的格式为`key=value&key=value`

post请求参数可以在send()里面添加。

```javascript
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<input type="button" value="js的ajax请求" onclick="sendData()"><br>
<input type="button" value="js的ajax请求-post" onclick="sendData4Post()"><br>
</body>

<script>
    // 发送ajax请求
    function sendData() {
        // 1. 获得对象
        var xmlHttp = null;
        if (window.XMLHttpRequest) {// all modern browsers
            xmlHttp = new XMLHttpRequest();
        } else if (window.ActiveXObject) {// for IE5, IE6
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        }

        // 2. 打开连接(请求方式,请求url)
        xmlHttp.open("GET", "demo01?username=zs&password=123");

        //3. 发送请求
        xmlHttp.send();

        //4. 查看响应数据
        xmlHttp.onreadystatechange =  function () {
            // 判断是否完成、正确响应
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                // 拿到数据
                alert(xmlHttp.responseText);
            }
        }
    }


    // 发送post请求
    function sendData4Post() {
        // 1. 获得对象
        var xmlHttp = null;
        if (window.XMLHttpRequest) {// all modern browsers
            xmlHttp = new XMLHttpRequest();
        } else if (window.ActiveXObject) {// for IE5, IE6
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        }

        // 2. 打开连接(请求方式,请求url)
        xmlHttp.open("POST", "demo01");

        //3. 发送请求（post方式可以把参数放到send方法中）
        // 吼后台要使用request.getParameter来获取参数，需要前端提交格式为表单格式
        xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
        xmlHttp.send("username=zs&password=123");

        //4. 查看响应数据
        xmlHttp.onreadystatechange =  function () {
            // 判断是否完成、正确响应
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                // 拿到数据
                alert(xmlHttp.responseText);
            }
        }
    }
</script>
</html>

```



### 4.小结

1. 获取ajax对象：API是固定的，抄

2. 打开连接 open(请求方式,请求url)

3. 发送请求： send()

4. 获得响应的数据：监听属性状态变更onreadystatechange

   

   





## 案例-使用JS的AJAX完成用户名的异步校验 ##

### 1.需求 ###

​	我们有一个网站，网站中都有注册的页面，当我们在注册的页面中输入用户名的时候(失去焦点的时候)，这个时候会提示，如果用户名已存在，则提示被占用即可。

![img](img/tu_1.png)

### 2.思路分析 ###

1. ![1600317097269](img/1600317097269.png)


### 3.代码实现

#### 3.1.环境的准备

+    创建数据库和表

     ```mysql
     create database day31;
     use day31;
     create table user(
     	id int primary key auto_increment,
     	username varchar(20),
     	password varchar(20),
     	email varchar(50),
     	phone varchar(20)
     );
     insert into user values (null,'aaa','123','aaa@163.com','15845612356');
     insert into user values (null,'bbb','123','bbb@qq.com','15845612356');
     insert into user values (null,'ccc','123','ccc@163.com','15845612356');
     ```

+    创建实体类       

     ```java
       public class User implements Serializable{
     
           private int id;
           private String username;
           private String password;
           private String email;
           private String phone;
       }
     ```



+ 导入jar包 (驱动,c3p0,DBUtils)


+ 引入工具类(C3P0Utils),配置文件(c3p0-config.xml)
+ 页面的准备

#### 3.2代码实现

+ 页面
+ ==**响应给前端的true或者false 到了页面变成字符串类型**==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-3.3.1.min.js"></script>
</head>
<body>
<center>
    <h1>用户注册页面</h1>
    <form>
        <table border="1px" width="500px">
            <tr>
                <td>用户名:</td>
                <!--1.给文本框增加失去焦点事件-->
                <td><input type="text" id="username" name="username"
                           onblur="checkUsername()" />
                    <span id="usernamespan"></span>
                </td>
            </tr>

            <tr>
                <td>密码:</td>
                <td><input type="password" name="password"/>
                </td>
            </tr>

            <tr>
                <td>邮箱:</td>
                <td><input type="text" name="email"/>
                </td>
            </tr>

            <tr>
                <td>电话:</td>
                <td><input type="text" name="phone"/>
                </td>
            </tr>

            <tr>
                <td><input id="submitId" type="button" value="注册"/></td>
            </tr>

        </table>
    </form>
</center>
</body>

<script>
    // 发送ajax请求
    function checkUsername() {
        //1. 获得ajax对象
        var xmlHttp = null;
        if (window.XMLHttpRequest) {// all modern browsers
            xmlHttp = new XMLHttpRequest();
        } else if (window.ActiveXObject) {// for IE5, IE6
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        }
        //2.打开连接  //需要传入用户名作为参数
        xmlHttp.open("GET", "registerServlet?username=" + $("#username").val());

        //3. 发送请求
        xmlHttp.send();

        //4. 查看响应数据
        xmlHttp.onreadystatechange =  function () {
            // 判断是否完成、正确响应
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                //获得响应后，判断用户名是否可用，不可用则需要在需要在文本框后面的span里面提示文案
                var responseText = xmlHttp.responseText;
                //alert(responseText)
                // 判断(后台返回的是一个字符串)
                if(responseText == "false"){
                    $("#usernamespan").html("<font color='red'>用户名不可用</font>")
                }
            }
        }



    }
</script>
</html>
```

+ RegisterServlet

```java
package com.itheima.web;

import com.itheima.service.UserService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/registerServlet")
public class RegisterServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");


        try {
            //1. 获取参数，用户名
            String username = request.getParameter("username");

            //2. 调用业务层，获得一个布尔值
            UserService userService = new UserService();
            boolean flag = userService.checkUsername(username);

            //3. 响应;把结果布尔值返回给客户端:返回的是一个字符串"true"、"false"
            response.getWriter().write(flag+"");
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙!");
        }

    }
}

```

- service

```java
package com.itheima.service;

import com.itheima.bean.User;
import com.itheima.dao.UserDao;

public class UserService {

    public boolean checkUsername(String username) throws Exception{
        //1.处理业务，判断用户名是否可用
        // 如果存在任意一个则认为被占用，根据用户名查询用户
        UserDao userDao = new UserDao();
        User user = userDao.findByUsername(username);


        if(user != null){
            // 用户存在,不可用
            return false;
        }else{
            return true;
        }

    }
}

```



- dao

```java
package com.itheima.dao;

import com.itheima.bean.User;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

public class UserDao {
    // DBUtils技术
    public User findByUsername(String username) throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());


        // 你想得到一个结果，那么可以加上LIMIT 1
        String sql = "select * from user where username = ? LIMIT 1";
        User query = queryRunner.query(sql, new BeanHandler<>(User.class), username);

        return query;
    }
}

```



### 4.小结

#### 4.1 思路

![1600309473234](img/1600309473234.png)



#### 4.2注意事项

1. 响应给前端的true或者false 到了页面变成了字符串类型

# 第二章-JQ的AJAX【重点】

## 知识点-JQ的AJAX介绍

### 1.目标

+ 知道为什么要学习JQ的AJAX原因

### 2.讲解

#### 2.1 为什么要使用JQuery的AJAX

​	因为传统(js里面)的AJAX的开发中，AJAX有两个主要的问题：

- 浏览器的兼容的问题 , 编写AJAX的代码太麻烦而且很多都是雷同的。

- 在实际的开发通常使用JQuery的Ajax  ,或者Vue里面的axios

#### 2.2JQuery的Ajax的API

| 请求方式            | 语法                                                 |
| ------------------- | ---------------------------------------------------- |
| ==GET请求==         | $.get(url, *[data]*, *[callback回调函数]*, *[type]*) |
| ==POST请求==        | $.post(url, *[data]*, *[callback]*, *[type]*)        |
| ==AJAX请求==        | $.ajax([settings])                                   |
| GET请求(3.0新特性)  | $.get([settings])                                    |
| POST请求(3.0新特性) | $.post([settings])                                   |

### 3.小结

1. 为何使用jq的ajax： 因为原生js的ajax步骤太多
2. jq的API：
   1. $.get
   2. $.post
   3. $.ajax







## 知识点-JQ的AJAX入门【重点】

### 1.目标

​	在网页上点击按钮, 发送Ajax请求服务器,响应

​	分别使用get,post,ajax三种方法

### 2.讲解

#### 2.1 $.get()

- get方式,  语法	`$.get(url, [data], [callback], [type]);`

  | 参数名称 | 解释                                                         |
  | -------- | ------------------------------------------------------------ |
  | url      | 请求的服务器端url地址                                        |
  | data     | 发送给服务器端的请求参数，格式可以是key=value(username=zs&pws=123)，也可以是js对象{username:"zs"} |
  | callback | 成功后的回调函数，可以在函数体中编写我们的逻辑代码==搞一个匿名函数来接收响应数据，函数的参数就是后台的响应数据function(obj){}== |
  | type     | 预期的返回数据的类型，取值可以是 xml, html, script, json, text, _defaul等 |

- 实例

```html
<script>

    // jq
    function sendData() {
        $.get(
            "demo01",   // url地址
            "username=zs&password=123", // 参数
            function (result) {   // result就是后台的响应数据
                alert(result)
            }
        );
    }


</script>
```

#### 2.2post()

- post方式, 语法 `$.post(url, [data], [callback], [type])`

  | 参数名称 | 解释                                                         |
  | -------- | ------------------------------------------------------------ |
  | url      | 请求的服务器端url地址                                        |
  | data     | 发送给服务器端的请求参数，格式可以是key=value，也可以是js对象 |
  | callback | 当请求成功后的回掉函数，可以在函数体中编写我们的逻辑代码     |
  | type     | 预期的返回数据的类型，取值可以是 xml, html, script, json, text, _defaul等 |

- 实例

```html
<script>
    $.post(
        "demo01",   // url地址
        "username=ls&password=123", // 参数
        function (result) {   // result就是后台的响应数据
            alert(result)
        }
    );
</script>
```

#### 2.3ajax()

- 语法   `$.ajax([settings])`

  其中，settings是一个js对象形式，格式是=={"属性名":"属性值","属性名":"属性值"... ...}==，常用的name属性名如下

  | 属性名称    | 解释                                                         |
  | ----------- | ------------------------------------------------------------ |
  | ==url==     | 请求的服务器端url地址                                        |
  | async       | (默认: true) 默认设置下，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为 false |
  | ==data==    | 发送到服务器的数据，可以是键值对形式（username=zs&pwd=123），也可以是js对象形式 |
  | ==type==    | (默认: "GET") 请求方式 ("POST" 或 "GET")， 默认为 "GET"      |
  | dataType    | 预期的返回数据的类型，取值可以是 xml, html, script, json, text, _defaul等 |
  | ==success== | 请求成功后的回调函数                                         |
  | error       | 请求失败时调用此函数                                         |

  ```json
  $.ajax({
      url:"demo01",
      data: "username=zs&password=123",
      type:"GET",
      success:function (response) {
          alert(response)
      }
  })
  ```

  

- 实例

```html
<script>
    $.ajax(
        {
            url: "demo01",   // url地址
            data: "username=ww&password=123", // 请求参数
            type: "POST", // 请求方式
            success: function (result) {  // 请求成功会回调的函数， 形参result就是返回的数据
                alert(result)
            }
        }
    );

</script>
```

### 3.小结

1. jq的ajax的语法的API

   1. $.get

   2. $.post

   3. $.ajax替换

      1. ```js
         $.ajax({
             url: "url路径",
             data: "参数值",
             type: "POST",
             success: function(result){}
         })
         ```





## 案例-使用JQ的Ajax完成用户名异步校验

### 1.需求分析

​	我们有一个网站，网站中都有注册的页面，当我们在注册的页面中输入用户名的时候，这个时候会提示，用户名是否存在。

​	![img](img/tu_1.png)

###  2,思路分析

![1600316765151](img/1600316765151.png)

- 使用$.ajax方法发起请求

  ```js
  // jq发送ajax请求
      function checkUsername() {
          $.ajax({
              url: "registerServlet",
              //需要传入用户名作为参数
              data: "username=" + $("#username").val(),
              success: function (response) {  // response就是响应数据
                  //获得响应后，判断用户名是否可用，不可用则需要在需要在文本框后面的span里面提示文案
                  if(response == "false"){
                      // 不可用
                      $("#usernamespan").html("<font color='red'>用户名被占用</font>")
                  }
              }
  
          });
      }
  ```
  
  

### 3.代码实现

+ 前端

```js
// jq发送ajax请求
    function checkUsername() {
        $.ajax({
            url: "registerServlet",
            //需要传入用户名作为参数
            data: "username=" + $("#username").val(),
            success: function (response) {  // response就是响应数据
                //获得响应后，判断用户名是否可用，不可用则需要在需要在文本框后面的span里面提示文案
                if(response == "false"){
                    // 不可用
                    $("#usernamespan").html("<font color='red'>用户名被占用</font>")
                }
            }

        });
    }
```



### 4.小结

1. 案例回顾

   1. 在事件函数里面使用jq的$.ajax发送请求

      1. ```js
         // jq发送ajax请求
             function checkUsername() {
                 $.ajax({
                     url: "registerServlet",
                     //需要传入用户名作为参数
                     data: "username=" + $("#username").val(),
                     success: function (response) {  // response就是响应数据
                         //获得响应后，判断用户名是否可用，不可用则需要在需要在文本框后面的span里面提示文案
                         if(response == "false"){
                             // 不可用
                             $("#usernamespan").html("<font color='red'>用户名被占用</font>")
                         }
                     }
         
                 });
             }
         ```



# 第三章 JSON

## 知识点-json的定义和解析【重点】

### 1.目标

- [ ] 掌握json定义和解析的语法

### 2.路径

1. json简介
2. 语法介绍
3. 使用示例

### 3. 讲解

#### 3.1.json简介

![img](img/tu_7.png)



==JSON是一种数据格式; 常用作客户端和服务器之间的数据交换。==

![1600312975227](img/1600312975227.png)

#### 3.2. 语法介绍【掌握】

* 定义方式：
  * 对象形式：`var obj = {"key":value,"key":value...}`
    + ==key是字符串==
    + value是任意的合法数据类型
    + 多个keyvalue之间使用逗号,隔开
    + key和value之间使用冒号:连接
  * 数组形式：`[element1, element2, ...]`
  * 混合(嵌套)形式：以上两种类型任意混合
    * json对象的value，可以是任意类型，当然也可以是数组
    * 数组里的element，可以是任意类型，当然也可以是json对象
* 解析语法：
  * 获取json对象里的value值：`json对象.key`
  * 获取数组里指定索引的元素：`数组[索引]`

#### 3.3. 使用示例

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>

<script>
    // JSON格式定义、解析的语法
    // 1. 对象形式： {"属性名":值,"属性名":值}
    // 1.1 表示一个用户，获得哟用户的年龄
    var user = {"username":"zs","age":18};
    console.log(user.age);

    // 2. 数组形式： [元素,元素]
    var arr = [1,2,3.14,"aaa", false]
    console.log(arr[2]);

    //3. 混合模式（嵌套模式）
    //3.1 对象中有数组--本质还是对象
    var user2 = {"username":"zsf", "age":149, "hobby":["taiji", "smoking", "drinking"]}
    // 获得第二个爱好
    console.log(user2.hobby[1]);

    //3.2 数组中包含对象--本质还是数组
    var arr2 = [
        {"username":"ls", "age":18},
        {"username":"zsf", "age":149, "hobby":["taiji", "smoking", "drinking"]}]
    // 获取第二个用户的年龄
    console.log(arr2[1].age);

</script>
</html>
```



#### 4.小结

1. JSON: 一种容易生成和解析的数据格式, 通常用作数据的交换

![image-20191218122123393](img/image-20191218122123393.png) 

2. 格式
   1. 对象格式： {"属性名":值,"属性名":值}
   2. 数组： [元素,元素]
   3. 混合模式
      1. 对象中包含数组，本质还是对象： {"hobby":["抽烟", "打牌"]}
      2. 数组中包含对象，本质还是数组：[{"name":"zs"}]



## 知识点-Jackson转换工具

### 1. 目标

* 能够使用转换工具Jackson，转换Java对象和json格式

### 2. 路径

1. 常见工具类介绍
2. Jackson的API介绍
3. Jackson的使用示例

### 3.讲解

#### 3.1. 常见工具类

* 在Ajax使用过程中，服务端返回的数据可能比较复杂，比如`List<User>`；这些数据通常要转换成json格式，把json格式字符串返回客户端
* 常见的转换工具有：
  * Jackson：SpringMVC内置的转换工具
  * jsonlib：Java提供的转换工具
  * gson：google提供的转换工具  
  * fastjson：Alibaba提供的转换工具

#### 3.2.Jackson的API介绍

* Jackson提供了转换的核心类：`ObjectMapper`
* `ObjectMapper`的构造方法：无参构造， ==new ObjectMapper()==
* `ObjectMapper`的常用方法：

| 方法                                                | 说明                                     |
| --------------------------------------------------- | ---------------------------------------- |
| ==objectMapper.writeValueAsString(Object obj)==     | 把obj对象里的数据转换成json格式          |
| ==objectMapper.readValue(String json, Class type)== | 把json字符串，还原成type类型的Java对象   |
| `readValue(String json, TypeReference reference)`   | 把json字符串，还原成带泛型的复杂Java对象 |



#### 3.3.Jackson使用方式

##### 3.3.1java对象转成JSON

**步骤:**

1. 导入jar包

![image-20191104111038508](img/image-20191104111038508.png) 

2. 创建ObjectMapper对象
3. 调用==writeValueAsString(Object obj)==

```java
package com.itheima.jsontest;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.itheima.bean.User;
import org.junit.Test;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 使用jackson的步骤：
 *  1. 引入jar包
 *  2. 创建对象 ObjectMapper
 *  3. 使用API
 */
public class JacksonDemo {

    @Test
    public void test() throws JsonProcessingException {
        // 把java对象转成json格式字符串

        //创建对象 ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();

        //1. javabean转json格式字符串
        User user = new User();
        user.setUsername("zsf");
        user.setPassword("123");
        user.setEmail("zsf@126.com");
        String javabean4JsonStr = objectMapper.writeValueAsString(user);
        // {"id":0,"username":"zsf","password":"123","email":"zsf@126.com","phone":null}
        // javabean的属性名就是json的属性名，属性值就是json的属性值
        System.out.println(javabean4JsonStr);

        //2. list转json格式字符串
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        String list4JsonStr = objectMapper.writeValueAsString(list);
        //["aaa","bbb","ccc"]
        // 元素就是json数组的元素
        System.out.println(list4JsonStr);

        //3. map转json格式字符串
        Map<String, Object> map = new HashMap<>();
        map.put("username", "ww");
        map.put("age", 18);
        String[] hobby = {"吃饭", "看书"};
        map.put("hobby", hobby);
        String map4JsonStr = objectMapper.writeValueAsString(map);
        //{"age":18,"username":"ww","hobby":["吃饭","看书"]}
        // map的key成了json的属性名，map的value成了json的属性值
        System.out.println(map4JsonStr);

    }
}

```



##### 3.3.2json转成Java对象

**步骤:**

1. 导入jar包

![image-20191104111038508](img/image-20191104111038508.png) 

2. 创建ObjectMapper对象
3. 调用==readValue(String json串, Class type)==或者readValue(String json, TypeReference reference)

```java
@Test
    public void test02() throws IOException {
        // 把json格式字符串转成java对象

        // 创建ObjectMapper对象
        ObjectMapper objectMapper = new ObjectMapper();
        // 调用API

        //1. 把json格式字符串转成javabean对象
        User user = objectMapper.readValue("{\"id\":0,\"username\":\"zsf\",\"password\":\"123\",\"email\":\"zsf@126.com\",\"phone\":null}",
                User.class);
        System.out.println(user);

        //2. 把json格式字符串转成list对象
        List list = objectMapper.readValue("[\"aaa\",\"bbb\",\"ccc\"]", List.class);
        System.out.println(list);

        //3. 把json格式字符串转成map对象
        Map map = objectMapper.readValue("{\"age\":18,\"username\":\"ww\",\"hobby\":[\"吃饭\",\"看书\"]}", Map.class);
        System.out.println(map);
    }
```



### 4. 小结

1. jackson的使用步骤
   1. 引入jar包
   2. 创建ObjectMapper对象： new ObjectMapper();
   3. 使用
      1. java对象转json格式字符串:objectMapper.writeValueAsString(obj)
      2. json格式字符串转java对象：objectMapper.readValue(json格式字符串, Class类型)




## 知识点-fastjson转换工具

### 1.目标

- [ ] 能够使用转换工具Fastjson，转换Java对象和json格式

### 2.路径

1. Fastjson的API介绍
2. Fastjson的使用示例

### 3.讲解

#### 3.1. fastjson的API介绍

* fastjson提供了核心类：==`JSON`==
* ==JSON类==提供了一些常用的==**静态**==方法：

| 方法                                                | 说明                                     |
| --------------------------------------------------- | ---------------------------------------- |
| ==toJSONString(Object obj)==                        | 把obj对象里的数据转换成json格式          |
| ==parseObject(String json, Class type)==            | 把json字符串，还原成type类型的Java对象   |
| `parseObject(String json, TypeReference reference)` | 把json字符串，还原成带泛型的复杂Java对象 |

3.2. fastjson的使用方式

##### 3.2.1java对象转成json

**步骤**



1. 导入jar包

   ![image-20191115174142074](img/image-20191115174142074.png)

2. 调用==JSON.toJSONString(Object obj);==

```java
package com.itheima.jsontest;

import com.alibaba.fastjson.JSON;
import com.itheima.bean.User;
import org.junit.Test;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * Fastjson的使用步骤：
 *  1. 引入jar包
 *  2. 使用API： JSON.XXXX
 */
public class FastjsonDemo {
    @Test
    public void test(){
        // 把java对象转成json格式字符串

        //1. javabean转成json格式串
        User user = new User();
        user.setUsername("ls");
        user.setPassword("123");
        String javabean4JsonStr = JSON.toJSONString(user);
        // {"id":0,"password":"123","username":"ls"}
        System.out.println(javabean4JsonStr);

        //2. list转json格式串
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        String list4JsonStr = JSON.toJSONString(list);
        // ["aaa","bbb","ccc"]
        System.out.println(list4JsonStr);

        //3. map转json格式串
        Map<String, String> map = new HashMap<>();
        map.put("akey", "avalue");
        map.put("bkey", "bvalue");
        map.put("ckey", "cvalue");
        String map4JsonStr = JSON.toJSONString(map);
        // {"ckey":"cvalue","akey":"avalue","bkey":"bvalue"}
        System.out.println(map4JsonStr);


        // 4. 数组转json串
        String[] strArr = {"hello", "hahah"};
        String arr4JsonStr = JSON.toJSONString(strArr);
        // ["hello","hahah"]
        System.out.println(arr4JsonStr);


    }
}

```



##### 3.2.2json转成java对象

**步骤**



1. 导入jar包

   ![image-20191115174142074](img/image-20191115174142074.png)

2. 调用==JSON.parseObject(String json,Class clazz);==

```java
@Test
    public void test02(){
        // 把json格式字符串转成java对象

        // 1. json串转javabean
        User user = JSON.parseObject("{\"id\":0,\"password\":\"123\",\"username\":\"ls\"}", User.class);
        System.out.println(user);

        //2. json串转list
        List list = JSON.parseObject("[\"aaa\",\"bbb\",\"ccc\"]", List.class);
        System.out.println(list);

        //3. json串转map
        Map map = JSON.parseObject("{\"ckey\":\"cvalue\",\"akey\":\"avalue\",\"bkey\":\"bvalue\"}", Map.class);
        System.out.println(map);
    }
```



### 4.小结

1. fastjson的API
   1. 引入jar包
   2. 使用
      1. java对象转json格式字符串： JSON.toJsonString(obj)
      2. json格式转java对象： JSON.parseObject(json串,Class)





## 案例三：能够完成自动补全的案例(返回JSON数据)

### 1.需求

​	实现一个搜索页面，在文本框中输入一个值以后(键盘抬起的时候)，给出一些提示（==数据交换采用json格式==）

​	![img](img/tu_6.gif)

### 2.思路分析

- ![1600328271091](img/1600328271091.png)


### 3.代码实现

#### 3.1.环境的准备

+ 创建数据库

```sql
create table words(
    id int primary key auto_increment,
    word varchar(50)
);
insert into words values (null, 'all');
insert into words values (null, 'after');
insert into words values (null, 'app');
insert into words values (null, 'apple');
insert into words values (null, 'application');
insert into words values (null, 'applet');
insert into words values (null, 'and');
insert into words values (null, 'animal');
insert into words values (null, 'back');
insert into words values (null, 'bad');
insert into words values (null, 'bag');
insert into words values (null, 'ball');
insert into words values (null, 'banana');
insert into words values (null, 'bear');
insert into words values (null, 'bike');
insert into words values (null, 'car');
insert into words values (null, 'card');
insert into words values (null, 'careful');
insert into words values (null, 'cheese');
insert into words values (null, 'come');
insert into words values (null, 'cool');
insert into words values (null, 'dance');
insert into words values (null, 'day');
insert into words values (null, 'dirty');
insert into words values (null, 'duck');
insert into words values (null, 'east');
insert into words values (null, 'egg');
insert into words values (null, 'every');
insert into words values (null, 'example');
```

+ 创建JavaBean


  ```java
  package com.itheima.bean;

  import java.io.Serializable;

  public class Words implements Serializable{


      private int id;
      private String word;
  }

  ```

+ 导入jar,工具类, 配置文件

+ 创建页面,demo06.html

  ```HTML
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <center>
  
      <h1>黑马</h1>

      <input id="inputId" type="text" style="width: 500px; height: 38px;" /><input
        type="button" style="height: 38px;" value="黑马一下" />
      <div id="divId"
           style="width: 500px; border: 1px red solid; height: 300px; position: absolute; left: 485;">
          <table id="tabId" width="100%" height="100%"  border="1px">
              <tr><td>aaaa</td></tr>
              <tr><td>bbbb</td></tr>
              <tr><td>cccc</td></tr>
              <tr><td>dddd</td></tr>
              <tr><td>eeee</td></tr>
          </table>
      </div>
  
  </center>
</body>
  </html>
  ```

#### 3.2实现

+ 页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-3.3.1.min.js"></script>
</head>
<body>
<center>

    <h1>黑马</h1>

    <input id="inputId" type="text" style="width: 500px; height: 38px;" onkeyup="queryWord()"/><input
        type="button" style="height: 38px;" value="黑马一下" />
    <div id="divId"
         style="width: 500px; border: 1px red solid; height: 300px; position: absolute; left: 480px;">
        <table id="tabId" width="100%" height="100%"  border="1px">
            <tr><td>aaaa</td></tr>
            <tr><td>bbbb</td></tr>
            <tr><td>cccc</td></tr>
            <tr><td>dddd</td></tr>
            <tr><td>eeee</td></tr>
        </table>
    </div>

</center>
</body>

<script>
    // jq的基本事件使用
    /*$("#inputId").keyup(function () {
        //2.1 需要发起ajax请求
        //2.2
        $.ajax({
            url: "wordServlet",
            // 参数（用户输入的内容）
            data: "keyWord="+$("#inputId").val(),
            success:function(result){
                alert(result)
                // result就是后台返回的结果
                // 遍历，并且每个元素都要成为一行tr，追加到表格中
            },
            dataType:"json"  // 期望的类型为json
        })
    })*/


    function queryWord() {
        //2.1 需要发起ajax请求
        //2.2
        $.ajax({
            url: "wordServlet",
            // 参数（用户输入的内容）
            data: "keyWord="+$("#inputId").val(),
            success:function(result){
                //【优化：进来时需要清除旧数据】
                $("#tabId").html("");


                //alert(result)
                // result就是后台返回的结果
                // 遍历，并且每个元素都要成为一行tr，追加到表格中
                for(ele of $(result)){
                    // ele 就是一个数据（包含了id、word属性）
                    // 创建一行，单元格的值为单词
                    var $tr = $("<tr><td>"+ ele.word +"</td></tr>");
                    // 追加到表格
                    $("#tabId").append($tr);
                }
            },
            dataType:"json"  // 期望的类型为json
        })
    }
</script>
</html>
```

+ WordServlet

```java
package com.itheima.web;

import com.alibaba.fastjson.JSON;
import com.itheima.bean.Words;
import com.itheima.service.WordService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet("/wordServlet")
public class WordServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        try {
            //1. 获取参数,输入的内容
            String keyWord = request.getParameter("keyWord");


            //2. 调用业务层，List集合
            WordService wordService = new WordService();
            List<Words> list =  wordService.findByKeyword(keyWord);

            //3. 响应：json格式
            // 需要把list转成json格式串，再响应出去
            String jsonString = JSON.toJSONString(list);
            response.getWriter().write(jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

+ WordService

```java
package com.itheima.service;

import com.itheima.bean.Words;
import com.itheima.dao.WordDao;

import java.util.List;

public class WordService {
    public List<Words> findByKeyword(String keyWord) throws Exception{

        // 调用dao
        WordDao wordDao = new WordDao();
        List<Words> list = wordDao.findByKeyword(keyWord);
        return list;
    }
}

```

+ WordDao

```java
package com.itheima.dao;

import com.itheima.bean.Words;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.util.List;

public class WordDao {

    /**
     * 根据关键字查询相关内容--模糊查询
     * @param keyWord
     * @return
     */
    public List<Words> findByKeyword(String keyWord) throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());


        String sql = "select * from words where word like ?";
		// 模糊匹配  like '%输入的值%'
        List<Words> query = queryRunner.query(
                sql, new BeanListHandler<>(Words.class), "%"+keyWord+"%");
        return query;
    }
}

```

### 4. 小结

 页面： 给文本框增加了一个键盘弹起事件，onkeyup

事件函数里面写逻辑：

```js
$.ajax({
    url: "地址",
    data: "keyWord=" + 输入框的内容,
    success:function(result){
        // 【优化：清除表格旧内容】 html("")
        
        // result就是一个数组形式
        // 遍历
        for(ele of $(result)){
            // ele 是一个对象，包含id、word字段，我们只想用word
            // 创建tr、td、td里面内容才是ele.word
            // 追加到表格
        }
    }
})
```







# 总结

- ajax

  - 概念： 异步的js和xml

  - 作用：

    - 异步
    - 局部刷新

  - jq的ajax

    - $.get

      - ```js
        $.get(url地址, "参数username=zs&pws=123",
              function(result){
            		// result响应数据
        })
        ```

    - $.post

      - ```js
        $.post(url地址, "参数username=zs&pws=123",
              function(result){
            		// result响应数据
        })
        ```

    - $.ajax

      - ```js
        $.ajax({
            url: "地址",
            data: "username=zs&pws=123",
            type:"GET\POST",
            success: function(result){
                // result响应数据
            },
            dataType: "json"  // 浏览器期望服务器返回json格式
        })
        ```

    - 案例： 检查用户名是否被占用

- json

  - json：一种数据交换格式，客户端、服务端
  - json格式的语法：
    - 对象： {"属性名":值, "属性名":值}
    - 数组： [元素,元素]
    - 混合模式：
      - 对象中包含数组： {"属性名": [元素,元素]}
      - 数组中包含对象：[元素1, {}]
    - 解析json：
      - 对象获取属性值：   json对象.key
      - 数组： 对象[下标]
  - json转换工具
    - jackson
      - 引入jar包
      - 使用API
        - 创建ObjectMapper
        - java对象转json：objectMapper.writeValueAsString(obj)
        - json转java对象：objectMapper.readValue(json串, Class)
    - fastjson
      - 引入jar包
      - 使用API
        - java对象转json： JSON.soJSONString(obj)
        - json转java对象: JSON.parseObject(json串, Class)
    - 词汇联想
      - jq的DOM操作：创建元素、追加、遍历+ ajax



# 要点回顾

- jq的ajax'请求（直接做jq的ajax的案例）
- json的格式、语法
- jackson、fastjson
- 补全案例





# 预习要点

vue的入门案例

vue的created生命周期函数（这一个就可以了）

vue的重要指令

vue发送ajax请求【重点】

