---
typora-copy-images-to: img
---

# day25-request&response

# 学习目标

- [ ] 能够使用Request对象获取HTTP协议请求内容
- [ ] 能够处理HTTP请求参数的乱码问题
- [ ] 能够使用Request域对象
- [ ] 能够使用Request对象做请求转发
- [ ] 能够使用Response对象操作HTTP响应内容 
- [ ] 能够处理响应乱码 
- [ ] 能够完成文件下载案例 
- [ ] 能够完成注册案例 
- [ ] 能够完成登录案例 

# 第一章-request

## 知识点-request概述

### 1.目标

+ 知道什么是request以及作用

### 2.讲解

#### 2.1什么是request

    在Servlet API中，定义了一个HttpServletRequest接口，它继承自ServletRequest接口，专门用来封装HTTP请求消息。由于HTTP请求消息分为请求行、请求头和请求体三部分，因此，在HttpServletRequest接口中定义了获取请求行、请求头和请求消息体的相关方法.

​	![img](img/tu_2.png)

​	==Web服务器收到客户端的http请求，会针对每一次请求，分别创建一个用于代表请求的request对象、和代表响应的response对象。==

#### 2.2request作用

+ 操作请求三部分(行,头,体)
+ 请求转发
+ 作为域对象存数据   

### 3.小结

1. request： 表示请求，HttpServletRequest类型
2. 作用：
   1. API可以操作请求三部分（行、头、体）
   2. 请求转发
   3. 域对象存取值







## 知识点-操作请求行和请求头

### 1.目标

+ 掌握使用request对象获取客户机信息(操作请求行)和获得请求头信息(操作请求头)

### 2.路径

+ 获取客户机信息(操作请求行)
+ 获得请求头信息(操作请求头)

### 3.讲解

#### 3.1获取客户机信息(操作请求行)

​	请求方式  请求路径(URI)  协议版本

​	GET  /day17Request/WEB01/register.htm?username=zs&password=123456   HTTP/1.1	

- **getMethod()**;获取请求方式  
- **getContextPath()**;获得当前项目名(部署的路径); 
- **getRequestURI();获得请求地址，不带主机名**    
- **getRequestURL()；获得请求地址，带主机名** (浏览器上面的地址)
- getRemoteAddr() ；获取客户机的IP地址(知道是谁请求的)
- getServerPort()；获得服务端的端口     
- getQueryString()；获的请求参数(get请求的,URL的?后面的. eg:username=zs&password=123456)



示例代码：

```java
@WebServlet("/demo01")
public class ServletDemo01 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑--获取请求行信息

        // 请求方式GET、POST
        String method = request.getMethod();
        System.out.println("method="+method);


        // 获得项目名
        String contextPath = request.getContextPath();
        System.out.println("contextPath="+contextPath);

        // URI： /day25/demo01
        System.out.println(request.getRequestURI());
        // URL：http://localhost:8080/day25/demo01
        System.out.println(request.getRequestURL());
    }
}
```



#### 3.2.获得请求头信息(操作请求头)

请求头: ==浏览器告诉服务器，浏览器自己的属性,配置的==, 以key value存在, 可能一个key对应多个value



![img](img/tu_3.png)

​	==getHeader(String name);==

- User-Agent: 浏览器信息
- Referer:来自哪个网站(防盗链)

示例代码：

```java
@WebServlet("/demo02")
public class ServletDemo02 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 获取头信息

        // 浏览器本身信息（浏览器的版本、类型）
        String userAgent = request.getHeader("User-Agent");
        System.out.println(userAgent);

        // Referer： 请求当前资源之前的那个网页的地址
        // 演示2个网站（2个项目--从day24超链接来访问当前资源）
        String referer = request.getHeader("Referer");
        System.out.println(referer);

        // 防盗链
        // 如果不是当前项目则不予许访问--直接返回..
        // 当前项目、referer里面的项目比较(http://localhost:8080/day24/、/day25)
        // day25就是百度网站（很多的美女图片...）
        // 有一个其他网站想来访问我的美女....打回去不允许
        if(!referer.contains(request.getContextPath())){
            System.out.println("you are a bad man....");
        }


    }
}
```





### 4.小结

1. 操作请求行(获得客户机信息)
   + 获得请求方式： getMethod()
   + 获得项目名称： getContextPath()
   + 获得URI：getRequestURI()
   + 获得URL：getRequestURL()
2. 操作请求头
   + getHeader("你想要的头")



## 知识点-操作请求体(获得请求参数)【重点】

### 1.目标

+ 掌握获得请求参数, 以及乱码的解决

### 2.路径

+ 获得请求参数  
+ 请求参数乱码的处理
+ 使用BeanUtils封装请求参数到JavaBean

### 3.讲解

#### 3.1获得请求参数

| 法名                                     | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| String getParameter(String name)         | 获得指定参数名对应的值。如果没有则返回null，如果有多个获得第一个。  例如：username=jack |
| String[] getParameterValues(String name) | 获得指定参数名对应的所有的值。此方法专业为复选框提供的。  例如：hobby=抽烟&hobby=喝酒&hobby=敲代码 |
| Map<String,String[]> getParameterMap()   | 获得所有的请求参数。key为参数名,value为key对应的所有的值。   |



示例代码

- paramForm.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--action是提交表单的目标地址
    默认是get请求
-->
<form action="demo03">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    爱好：<input type="checkbox" name="hobby" value="basketball">篮球
    <input type="checkbox" name="hobby" value="football">足球
    <input type="checkbox" name="hobby" value="coding">敲代码<br>
    <input type="submit">
</form>
</body>
</html>
```





#### 3.2 请求参数乱码处理

如果你的IDEA出现了黑块的乱码形式，可以在tomcat的菜单加参数  ==-Dfile.encoding=UTF-8==：

![1597024751424](img/1597024751424.png)



tomcat8之后get请求没有了乱码；

但是post存在中文乱码。	





我们在输入一些中文数据提交给服务器的时候，服务器解析显示出来的一堆无意义的字符，就是乱码。
那么这个乱码是如何出现的呢？如下图所示：

![1599531522995](img/1599531522995.png) 

1. get方式, 我们现在使用的tomcat>=8.0了, 乱码tomcat已经处理好了
2. post方式, 就需要自己处理

```java
// 乱码处理，需要在获取参数之前解决！！！
request.setCharacterEncoding("UTF-8");
```



#### 3.3使用BeanUtils封装

​	现在我们已经可以使用request对象来获取请求参数，但是，如果参数过多，我们就需要将数据封装到对象。

​	以前封装数据的时候，实体类有多少个字段，我们就需要手动编码调用多少次setXXX方法，因此，我们需要BeanUtils来解决这个问题。

​	BeanUtils是Apache Commons组件的成员之一，主要用于==简化JavaBean封装数据==的操作。

使用步骤:

1. 导入jar
2. 编写一个javabean和表单输入项对应（==属性、和name的值要对应==）
3. 获取所有参数得到一个map： Map<String, String[]> parameterMap = request.getParameterMap();
4. 使用BeanUtils.populate(javabean,map)

​	

+ 表单parameterPopulate.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单提交</title>
</head>
<body>
<!--action指向的是一个servlet的地址-->
    <form action="parameterPopulateServlet" method="post">
        用户名： <input type="text" name="username"><br>
        密码： <input type="password" name="password"><br>
        性别：<input type="radio" name="sex" value="mail">男
        <input type="radio" name="sex" value="femail">女
        <br>
        爱好：<input type="checkbox" name="hobby" value="basketball">篮球
        <input type="checkbox" name="hobby" value="football">足球
        <input type="checkbox" name="hobby" value="pingpong">乒乓球
        <br>
        <input type="submit">
    </form>
</body>
</html>
```

+ ParameterPopulateServlet

```java
package com.itheima.a_request;

import com.itheima.bean.UserBean;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/parameterPopulateServlet")
public class ParameterPopulateServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 处理请求乱码
        request.setCharacterEncoding("UTF-8");


        try {
            // BeanUtils来封装参数

            // 1.导入、依赖jar
            // 2. 写一个javabean---表单输入项的name属性值一一对相应
            //3. 获取所有参数
            Map<String, String[]> parameterMap = request.getParameterMap();
            // 4. 封装
            UserBean userBean = new UserBean();

            // 方法： 就是根据来封装的（javabean的属性名必须和参数的名字一致）
            // 属性（set方法去除set前缀后面内容首字母小写： setId----》Id--->id）
            BeanUtils.populate(userBean,parameterMap);

            // 打印一下userBean
            System.out.println(userBean);


        } catch (Exception e) {
            e.printStackTrace();
        }


    }
}

```



- UserBean

```java
/*
1. 字段私有
2. 公共的set、get方法
3. 空构造器
4. 实现序列化接口
 */
public class UserBean implements Serializable {
    private String username;
    private String password;
    private String sex;
    private String[] hobby;
    ...
}
```







### 4.小结

1. 获取请求参数
   1. 获得一个参数：request.getParameter("页面的name属性值")
   2. 获得多个值：String[] request.getParameterValues("页面的name属性值")
   3. 获得所有参数：Map<String, String[]> parameterMap = request.getParameterMap()
   4. 封装数据到javabean： BeanUtils
      1. 引入jar包
      2. 写一个javabean（属性和页面的name属性值一致）
      3. BeanUtils.populate(javabean， parameterMap )



## 知识点-请求转发【重点】

### 1.目标

+ 掌握请求转发

### 2.讲解

```java
request.getRequestDispatcher(url).forward(request, response);  //转发
```

### 3.小结

+ 转发

```java
request.getRequestDispatcher("转发的路径").forward(request,response); 
```

+ 特点
  
  ![1599556213382](img/1599556213382.png)
  
  ![1599535002983](img/1599535002983.png)

示例代码：

- html

```html
<!--张三找李四借钱-请求转发-->
<a href="lsForwardServlet">张三找李四借钱-请求转发</a><br>
```



- LsForwardServlet

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


/**
 * 张三找李四借钱-请求转发
 */
@WebServlet("/lsForwardServlet")
public class LsForwardServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        System.out.println("请求转发--我是李四，我没钱，我有老婆....");

        // 李四转发给他老婆
        // 转发的地址只能写当前项目的资源的地址
        request.getRequestDispatcher("lsWifeForwardServlet").forward(request, response);
    }
}

```



- LsWifeForwardServlet

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 李四老婆--请求转发借钱
 */
@WebServlet("/lsWifeForwardServlet")
public class LsWifeForwardServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        System.out.println("请求转发--我是李四老婆...");

    }
}

```





## 知识点-作为域对象存取值

### 1.目标

+ 掌握requet作为域对象存取值

### 2.讲解

​	ServletContext: 范围 整个应用(无论多少次请求,只要是这个应用里面的都是可以==共享==的)

​	request范围: 当前这一次请求有效  



直接请求：先请求ServletA（存值）、再请求ServletB（取值-取不到）

==**请求转发**是一次请求，所以可以在转发后的资源里面获得request设置的值。==

- Object getAttribute(String name) ;
- void setAttribute(String name,Object object)  ;
- void removeAttribute(String name)  ;

 

示例代码：

```html
<a href="servletA">作为域对象存取值-ServletA（存值）</a><br>
<a href="servletB">作为域对象存取值-ServletB（取值）</a><br>
<a href="lsForwardServlet2">作为域对象存取值--张三找李四借钱-请求转发</a><br>
```



- ServletA

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/servletA")
public class ServletA extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // request作为域对象存取值
        request.setAttribute("rkey", "rvalue");
        System.out.println("servletA--存值");
    }
}

```

- ServletB

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/servletB")
public class ServletB extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 取值--request作为域对象存取值（一次请求内有效）
        Object rkey = request.getAttribute("rkey");
        System.out.println(rkey);
        System.out.println("servletA--取值结束.");
    }
}

```



- LsForwardServlet2

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


/**
 * 张三找李四借钱-请求转发
 */
@WebServlet("/lsForwardServlet2")
public class LsForwardServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        System.out.println("请求转发--我是李四，我没钱，我有老婆....");


        // 李四存值
        request.setAttribute("money", 100);

        // 李四转发给他老婆
        // 转发的地址只能写当前项目的资源的地址
        request.getRequestDispatcher("lsWifeForwardServlet2").forward(request, response);
    }
}

```



- LsWifeForwardServlet2

```java
package com.itheima.b_request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 李四老婆--请求转发借钱
 */
@WebServlet("/lsWifeForwardServlet2")
public class LsWifeForwardServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        System.out.println("请求转发--我是李四老婆...");

        System.out.println("李四老婆获得的信息-金额："+request.getAttribute("money"));

    }
}

```





### 3.小结

1. 作为域对象存取值
   
   ```java
   setAttribute("", "")
getAttribute("")
   ```
   
2. request只在一次请求内有效； 所以可以用在请求转发（转发就是一次请求）里面





# 第二章-Response

## 知识点-Response概述

### 1.目标

+ 掌握什么是Response,以及Response的作用

### 2.讲解

#### 2.1HttpServletResponse概述

​	在Servlet API中，定义了一个HttpServletResponse接口(doGet,doPost方法的参数)，它继承自ServletResponse接口，专门用来封装HTTP响应消息。由于HTTP响应消息分为响应行、响应头、响应体三部分，因此，在HttpServletResponse接口中定义了向客户端发送响应状态码、响应头、响应体的方法

#### 2.2作用

+ 操作响应的三部分(响应行,响应头,响应体)

### 3.小结

1. Response: HttpServletResponse对象，代表的是当前这次请求对应的响应
2. Response的作用
   + 操作响应的三部分(行, 头, 体)





## 知识点-操作响应行（了解，一般不用自己设定状态码）

### 1.目标

+ 掌握操作响应行的方法

### 2.讲解

```
HTTP/1.1 200 OK
```



​	![img/](img/tu_6.png)

​	常用的状态码：

​		200：成功

​		302：重定向

​		304：访问缓存

​		404：客户端错误

​		500：服务器错误

### 3.小结

1. 设置的API:  response.setStatus(int code);  

2. 一般不需要设置, 可能302 重定向需要设置

3. 常见的响应状态码

   + 200 成功
   + 302 重定向
   + 304 读缓存
   + 404 客户端错误
   + 500 服务器错误

   





## 知识点-操作响应头

### 1.目标

- 掌握操作响应头的方法, 能够进行定时刷新和重定向

### 2.路径

+ 操作响应头的API介绍
+ 定时刷新
+ 重定向

### 3.讲解

#### 3.1操作响应头的API

==响应头: 是服务器指示浏览器去做什么==

​	一个key对应一个value

​		![img/](img/tu_7.png)

​	一个key对应多个value

​		![img/](img/tu_8.png)

  关注的方法: ==setHeader(String name,String value);==

​	常用的响应头 

​		Refresh:定时跳转 (eg:服务器告诉浏览器5s之后跳转到百度)

​		Location:重定向地址(eg: 服务器告诉浏览器跳转到xxx)

​		Content-Disposition: 告诉浏览器下载

​		Content-Type：设置响应内容的MIME类型(服务器告诉浏览器内容的类型)

#### 3.2定时刷新

请求一个servlet，等待响应后，再过5秒钟跳转到百度。

```java
// 5秒后，浏览器跳转到百度
response.setHeader("Refresh", "5; url=http://www.baidu.com");
```

#### 3.3 重定向【重点】



![img/](img/tu_4.png)



1. // 重顶向是2次请求，所以王五拿不到李四设置的域对象的值 ；  转发是1次请求
2. // 重定向浏览器地址栏改变;  转发的地址不会改变
3. // 重定向可以指向外部地址；  转发只能是项目内部的资源

![1599556263471](img/1599556263471.png)

```java
//方式一: 重定向-不推荐
//1.设置状态码
//response.setStatus(302);
//2.设置重定向的路径(绝对路径,带域名/ip地址的,如果是同一个项目里面的,域名/ip地址可以省略)
//response.setHeader("Location","http://localhost:8080/day28/demo08");
//response.setHeader("Location","/day28/demo08");
//response.setHeader("Location","http://www.baidu.com");

//直接调用sendRedirect方法, 内部封装了上面两行
response.sendRedirect("http://localhost:8080/day28/demo08");
```

+ ==重定向的API==

```java
 response.sendRedirect("重定向的路径");
```

![1599538669611](img/1599538669611.png)



### 4.小结

#### 4.1操作响应头

```
刷新  Refresh
```

#### 4.2重定向

==重定向  response.sendRedirect("重定向的路径")==

+ 重定向两次请求
+ 重定向的地址栏路径改变
+ 重定向的路径写绝对路径, 带域名/ip地址的(如果路径在同一个项目里面,域名/ip地址可以省略的)
+ 重定向的路径可以写项目内部的, 也可以写项目外部的(eg: 百度)

#### 4.3转发和重定向区别【面试】

1. 转发是一次请求, 重定向是二次请求
2. 转发的路径不会改变,重定向的路径会改变
3. 转发只能转发到项目的内部资源,重定向可以重定向到项目的内部资源, 也可以是项目外部资源(eg:百度)
4. 转发可以转发到WEB-INF下面的资源, 重定向不可以重定向到WEB-INF下面的资源
   1. 因为转发是一次请求，转发时是服务器内部去访问WEB-INF的资源
   2. 重定向是2次请求，类似于在浏览器新发起请求去访问WEB-INF的资源，该目录受保护的，所以不能访问。
5. 转发的路径写相对的(内部地址), 重定向的路径写绝对的(带http,带ip,带项目名-外部地址)





## 知识点-操作响应体

### 1.目标

+ 掌握操作响应体以及响应乱码的解决

### 2.步骤

+ 操作响应体的API介绍
+ 响应乱码的解决

### 3.讲解

#### 3.1操作响应体的API

​	![img/](img/tu_9.png)

​	

​	**注意事项**： ==页面输出只能使用其中的一个流实现,两个流是互斥的.==

#### 3.2响应乱码处理

- 解决字符流输出中文乱码问题:  ==解决响应乱码：  需要在响应之前设置！==

```java
//  解决响应乱码：  需要在响应之前设置！  用这个！
response.setContentType("text/html;charset=utf-8");
```

示例代码

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 在控制台打印
        System.out.println("demo06.....");


        // 响应之前设置乱码处理（设置字符流乱码处理）
        response.setContentType("text/html; charset=UTF-8");

        // 输出到页面，必须使用resposne进行响应
        response.getWriter().write("哈哈哈哈,hello....");

    }
```



- 使用字节输出流输出中文乱码问题（字节流用得少）

```java
//1.设置浏览器打开方式
response.setHeader("Content-type", "text/html;charset=utf-8");

//2. 得到字节输出流
ServletOutputStream outputStream = response.getOutputStream();
outputStream.write("你好".getBytes("utf-8"));// 使用平台的默认字符(utf-8)集将此 String 编码为 byte 序列
```

### 4.小结

1. 就是操作响应数据-响应体
1. 字符流
   1. response.getWriter().write("内容")
   1. 乱码处理：response.setContentType("text/html;charset=utf-8");
1. 字节流
   1. response.getgetOutputStream()：少用






## 案例-完成文件下载 ##

### 1.需求分析

- 创建文件下载的列表的页面,点击列表中的某些链接,下载文件.

![1599556310611](img/1599556310611.png)

### 2.文件下载分析

#### 2.1什么是文件下载

​	将服务器上已经存在的文件,输出到客户端浏览器.

​	说白了就是把服务器端的文件拷贝一份到客户端, 文件的拷贝---> 流(输入流和输出流)的拷贝

#### 2.2文件下载的方式

+ ==请求后台，编码方式（推荐）== 

  手动编写代码实现下载.无论浏览器是否识别该格式的文件,都会下载.




### 3.思路分析 ###

#### 编码方式下载【掌握】

##### 3.2.1手动编码方式要求

套路：

​	设置两个头和一个流

​	设置的两个响应头:

    		Content-Disposition: 服务器告诉浏览器去下载

    		Content-Type: 告诉浏览器文件类型.(MIME的类型)  ：  上下文对象可以获得mime类型

​	设置一个流：

    		 获得要下载的文件的输入流.（其实还有响应输出流）

![1599549834268](img/1599549834268.png)

-----------------------------

- 设置Content-Type：  上下文对象可以获得mime类型
- 设置Content-Disposition： 看资料的头，示例拷贝修改即可
- 读取文件输入流：上下文对象获得web资源的输入流
- 输出到响应输出流： 

##### 3.2.2思路

![1599549799527](img/1599549799527.png)





### 4.代码实现 ###

页面download.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h3>下载页面</h3>
<a href="downloadServlet?filename=a.txt">下载a.txt</a><br>
<a href="downloadServlet?filename=a.jpg">下载a.jpg</a><br>
<a href="downloadServlet?filename=美女.jpg">下载美女.jpg</a><br>
</body>
</html>
```





```java
package com.itheima.c_response;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

@WebServlet("/downloadServlet")
public class DownloadServletOld extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");


        // 获取文件名字
        String filename = request.getParameter("filename");

        // 设置2头
        response.setHeader("Content-Disposition", "attachment;filename="+filename);

        // 获取上下文对象
        ServletContext servletContext = getServletContext();
        // 获取mime类型
        String mimeType = servletContext.getMimeType(filename);

        // 设置MIME类型
        response.setHeader("Content-Type", mimeType);

        // 处理1流
        // 获取web资源文件输入流（download/a.txt）
        InputStream resourceAsStream = servletContext.getResourceAsStream("download/" + filename);


        // 最后：响应到页面：字节流
        OutputStream outputStream = response.getOutputStream();

        byte[] bytes = new byte[1024];
        int len = 0;


        // 一直没读完，则一直读
        while((len = resourceAsStream.read(bytes)) != -1){
            // 读取的数据写入输出流
            outputStream.write(bytes,0, len);
        }


        // 关闭流
        resourceAsStream.close();
        outputStream.close();


    }
}

```

### 5. 中文名称下载优化

+ 告诉浏览器设置的响应头里面不支持中文的, 抓包来看:

![1555993058675](img/1555993058675.png) 

+ 解决方案: 手动进行编码再设置进去就ok了
+ **警示**：==在设置Content-Disposition响应头之前设置编码，但是要在读取输入流之后设置==

==【注意事项】读完之后再编码==

中文文件在不同的浏览器中编码方式不同：火狐是Base64编码, 其它浏览器(谷歌)是URL的utf-8编码

```java
// 获得User-Agent请求头
String agent = request.getHeader("User-Agent");

if(agent.contains("Firefox")){
    // 火狐浏览器
    filename = base64EncodeFileName(filename);
}else{
    // IE，其他浏览器
    filename = URLEncoder.encode(filename, "UTF-8");
}


// 火狐浏览器
public static String base64EncodeFileName(String fileName) {
    BASE64Encoder base64Encoder = new BASE64Encoder();
    try {
        return "=?UTF-8?B?"
            + new String(base64Encoder.encode(fileName
                                              .getBytes("UTF-8"))) + "?=";
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
        throw new RuntimeException(e);
    }
}

```

**优化后的版本**：

```java
package com.itheima.c_response;

import sun.management.resources.agent;
import sun.misc.BASE64Encoder;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;

@WebServlet("/downloadServlet")
public class DownloadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");


        // 获取文件名字
        String filename = request.getParameter("filename");




        /*
        ============================优化==========================
         */
        // 获取上下文对象
        ServletContext servletContext = getServletContext();
        // 处理1流
        // 获取web资源文件输入流（download/a.txt）
        InputStream resourceAsStream = servletContext.getResourceAsStream("download/" + filename);

        // 获得浏览器类型
        String userAgent = request.getHeader("User-Agent");


        // 编码设置必须在获取文件输入流之后
        if(userAgent.contains("Firefox")){
            // 火狐浏览器
            filename = base64EncodeFileName(filename);
        }else{
            // IE，其他浏览器
            filename = URLEncoder.encode(filename, "UTF-8");
        }

        // 设置2头
        response.setHeader("Content-Disposition", "attachment;filename="+filename);


        // 获取mime类型
        String mimeType = servletContext.getMimeType(filename);

        // 设置MIME类型
        response.setHeader("Content-Type", mimeType);



        // 最后：响应到页面：字节流
        OutputStream outputStream = response.getOutputStream();

        byte[] bytes = new byte[1024];
        int len = 0;


        // 一直没读完，则一直读
        while((len = resourceAsStream.read(bytes)) != -1){
            // 读取的数据写入输出流
            outputStream.write(bytes,0, len);
        }


        // 关闭流
        resourceAsStream.close();
        outputStream.close();


    }


    // 火狐浏览器
    public static String base64EncodeFileName(String fileName) {
        BASE64Encoder base64Encoder = new BASE64Encoder();
        try {
            return "=?UTF-8?B?"
                    + new String(base64Encoder.encode(fileName
                    .getBytes("UTF-8"))) + "?=";
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}

```





# 第三章-综合案例

## 案例-注册

### 1. 需求

![1571732869531](img/1571732869531.png)  

### 2. 路径

2. 完成注册功能

### 3. 实现

##### 思路分析：



 ![1599556347786](img/1599556347786.png)

##### 准备工作

+ 数据库

```sql
create database day25;
use day25;
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(40) DEFAULT NULL,
  `password` varchar(40) DEFAULT NULL,
  `address` varchar(40) DEFAULT NULL,
  `nickname` varchar(40) DEFAULT NULL,
  `gender` varchar(10) DEFAULT NULL,
  `email` varchar(20) DEFAULT NULL,
  `status` varchar(10) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8
```



+ JavaBean

```java
public class User implements Serializable{
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
	//...
}
```

+ 导入jar
  + 驱动
  + 连接池C3p0
  + DButils
  + Beanutils
  + ![1597048250285](img/1597048250285.png)
+ 工具类和配置文件（==c3p0-config.xml放在src目录==）
  + ![1597048286166](img/1597048286166.png)
+ register.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <center><h1>用户注册</h1>
    <form action="register" method="post">
        姓名:<input type="text" name="username"><br>
        密码:<input type="password" name="password"><br>
        昵称:<input type="text" name="nickname"><br>
        地址:<input type="text" name="address"><br>
        邮箱:<input type="text" name="email" /><br/>
        性别:<input type="radio" name="gender" value="female">女
        <input type="radio" name="gender" value="male">男<br>
        <input type="submit" value="注册">
    </form>
    </center>
</body>
</html>
```





【补充】手动修改默认的项目欢迎页（默认是web目录的index.jsp、index.html），可以在web.xml中修改。

以下配置表示，访问项目根目录，会访问register.html页面。

![1597048878485](img/1597048878485.png)



##### 代码实现

RegisterServlet.java

```java
package com.itheima.web;

import com.itheima.bean.User;
import com.itheima.utils.C3P0Utils;
import org.apache.commons.beanutils.BeanUtils;
import org.apache.commons.dbutils.QueryRunner;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/register")
public class RegisterServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");



        try {
            //1. 获得参数
            Map<String, String[]> parameterMap = request.getParameterMap();
            User user = new User();
            BeanUtils.populate(user, parameterMap);



            // 2. 保存数据（注册）
            QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());

            String sql = "insert into user values(null,?,?,?,?,?,?,?)";
            Object[] params = {
                    user.getUsername(),
                    user.getPassword(),
                    user.getAddress(),
                    user.getNickname(),
                    user.getGender(),
                    user.getEmail(),
                    "0"
            };
            // 受影响函数
            int cnt = queryRunner.update(sql, params);

            // 3. 响应
            if(cnt > 0){
                response.getWriter().write("<font color='green'>注册成功</font>");
            }else{
                response.getWriter().write("<font color='red'>注册失败</font>");
            }



        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙，稍后再试！");
        }

    }
}

```

### 4. 小结

1. 注册本质:  往数据库插入一条数据
2. 步骤：
   1. 获得所有参数
   2. 封装到javabean对象
   3. DBUtils保存数据
   4. 如何哦判断注册OK的
      1. 受影响行数>0---》注册成功了

## 案例-登录

### 1.需求

![img](img/tu_24.png)

+ 点击登录按钮, 进行登录.
+ 登录成功,响应显示login Success
+ 登录失败,响应显示login failed



### 2.思路

![1599556026491](img/1599556026491.png) 



### 3.实现

#### 准备工作

+ 页面的准备 login.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
</head>
<body>
    <center>
        <h3>登录页面</h3>
        <form action="loginServlet" method="post">
            姓名：<input type="text" name="username"><br>
            密码：<input type="password" name="password"><br>
            <input type="submit" value="登录">
        </form>
    </center>
</body>
</html>
```

#### 代码实现

- LoginServlet

```java
package com.itheima.web;

import com.itheima.bean.User;
import com.itheima.utils.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.sql.SQLException;

@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        try {
            // 1. 获得参数
            String username = request.getParameter("username");
            String password = request.getParameter("password");

            // 2. 查询用户--DBUtils技术
            QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());
            String sql = "select * from user where username= ? and password = ?";
            User queryUser =
                    queryRunner.query(sql, new BeanHandler<>(User.class), username, password);

            // 3. 响应
            /*
            - 登录成功,响应显示login Success
            - 登录失败,响应显示login failed
             */
            if(queryUser != null){
                response.getWriter().write("login Success");
            }else{
                response.getWriter().write("login failed");
            }

        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙，稍后再试！");
        }


    }
}

```



### 4.小结

1. 本质: 就是根据用户名、密码去数据库查询用户
2. 思路(LoginServlet)
   + 获得参数
   + 使用DBUtils去查询用户：返回一个user对象
   + 登录成功与否：
     + 查到了，表示登录OK





【优化】

注册OK之后，跳转到登录页面（重定向），修改RegisterServlet代码：

![1599555984162](E:\102期\day25-request&response\笔记\img\1599555984162.png)

注册OK之后，过2秒钟跳转到登录页面（refresh）





# 总结

- 请求request
  - HttpServletRequest接口类型，代表了浏览器过来的每次请求
  - 作用：
    - 操作请求三部分（行、头、体-参数）
    - 操作行
      - ![1599555128176](img/1599555128176.png)
    - 操作头： getHeader("你想要的头")： user-agent、referer
    - 【操作体-获取请求参数】
      - 获取一个参数值： request.getParameter("页面元素的name属性值")
      - 获取多个值：request.getParameterValues("页面元素的name属性值，比如爱好hobby")
      - 获取所有参数：Map<String,String[]>  request.getParameterMap()
      - beanutils封装参数到一个javabean
        - 引入jar包
        - 创建一个javabean
        - BeanUtils.populate(javabean, map)
    - 请求转发： request.getRequestDispatcher("路径").forward(request, response);
      - 特征：
        - 1次请求
        - 浏览器地址不会改变
        - 路径只能写本项目的资源路径
    - 作为域对象存取值[仅仅在一次请求中有效，请求转发可以用request域对象]
      - 设置值： setAtrribute(name, value)
      - 取-值： getAtrribute(name)
      - 移除：removeAtrribute(name)
- 响应response
  - 代表的是这一次请求对应的响应
  - 操作响应三部分（行、头、体）
    - 头： 设置头， setHeader("头"， "值")
    - 体： 响应数据
      - ==字符流==： response.getWriter().write("内容")
      - 字节流
  - 重定向： response.sendRedirect("路径")
    - 2次请求
    - 浏览器会改变
    - 路径可以是内部地址，可以是其他地址
- 处理乱码：
  - 请求：request.setCharacterEncoding("UTF-8")
  - 响应：response.setContentType("text/html;charset=utf-8")



- 下载案例【非常少用】
- 注册、登录





## 三层架构【今天只做了解】

- 软件中分层：按照不同功能分为不同层，通常分为三层：表现层(web层)，业务层，持久(DAO)层(data  accesss object)。

WEB层：和浏览器打交道， 获得参数、调用业务层、响应

service层： 专门处理你的具体业务逻辑的代码（可能要和数据库打交道--》交给DAO层）

DAO层：只和数据库打交道（CRUD）





![img](img/tu_12-1568797747077.png)

- 不同层次包名的命名

| 分层                 | 包名(公司域名倒写)  |
| -------------------- | ------------------- |
| 表现层(web层)        | com.itheima.web     |
| 业务层(service层)    | com.itheima.service |
| 持久层(数据库访问层) | com.itheima.dao     |
| JavaBean             | com.itheima.bean    |
| 工具类               | com.itheima.utils   |

- 分层的意义:
  1. 解耦：降低层与层之间的耦合性。
  2. 可维护性：提高软件的可维护性，对现有的功能进行修改和更新时不会影响原有的功能。
  3. 可扩展性：提升软件的可扩展性，添加新的功能的时候不会影响到现有的功能。
  4. 可重用性：不同层之间进行功能调用时，相同的功能可以重复使用。



三层架构视图：

![1597048388061](img/1597048388061.png)



## 【扩展】三层架构改写登录

- LoginServlet

```java
package com.itheima.web;

import com.itheima.bean.User;
import com.itheima.service.UserService;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 乱码处理
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");

        try {
            // 1. 获取参数
            Map<String, String[]> parameterMap = request.getParameterMap();
            User user = new User();
            BeanUtils.populate(user, parameterMap);
            // 2. 调用业务层
            UserService userService = new UserService();
            boolean flag = userService.login(user);

            // 3.做出响应
            if(flag){
                response.getWriter().write("登录成功");
            }else{
                response.getWriter().write("登录失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("登录失败");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}

```

- service

```java
public boolean login(User user) throws Exception{
        // 什么是登录成功
        // 就是根据用户名、密码查询
        UserDao userDao = new UserDao();
        User queryUser = userDao.query(user.getUsername(), user.getPassword());

        return queryUser != null;
}
```

- dao

```java
public User query(String username, String password) throws Exception{
        // 1. 获取查询器
        QueryRunner queryRunner = new QueryRunner(C3p0Utils.getDataSource());

        String sql = "select * from user where username = ?  and password = ?";
        User user = queryRunner.query(sql, new BeanHandler<User>(User.class), username, password);
        return user;
    }
```



# 回顾要点

请求

- 获取参数
- 处理乱码
- 请求转发
- 作为域对象存取值

响应

- 重定向
- 响应乱码
- 响应数据到页面  getWriter().write()



案例

- 注册
- 登录



事件剩余的话，了解一下三层架构

- 理解概念

# 预习重点

Cookie

- 获取Cookie
- 设置Cookie
- 原理理解

Session

- 获取Session
- 作为域对象存取值
- 原理





# IDEA离线安装插件步骤

![1597049105739](img/1597049105739.png)





![1597049138092](img/1597049138092.png)

