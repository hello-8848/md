---
typora-copy-images-to: img
---

# day26-cookie&session&jsp

# 教学目标

- [ ] 能够说出会话的概念
- [ ] 能够创建、发送、接收、删除cookie
- [ ] 能够完成记录用户各自的上次访问时间案例
- [ ] 能够获取session对象、添加、删除、获取session中的数据
- [ ] 能够完成登录验证码案例
- [ ] 了解JSP的脚本
- [ ] 能够完成记住用户名案例

# 第一章-会话的概念

## 知识点-会话的概念

### 1. 目标

- [ ] 理解会话的概念,知道有哪些会话技术

### 2. 路径

1. 会话的概念
2. 为什么要使用会话技术
3. 常用的会话技术

### 3. 讲解

#### 3.1会话的概念

​	**会话概念**：用户打开浏览器访问某个网站，到关闭浏览器的整个过程，称为用户和这个网站的一次会话。

​	一次会话里面可能包含多个请求！（浏览了多个网页）

#### 3.2为什么要使用会话技术

​	保存==用户各自(以浏览器为单位)==的数据。

![1597107667971](img/1597107667971.png)

#### 3.3常用的会话技术

常用会话技术2种：

- cookie：客户端浏览器会话技术，数据保存在浏览器中
- session：服务器端会话技术，数据保存在服务器端

### 4. 小结

1. 会话：用户打开浏览器，直到关闭浏览器的整一个过程。（包含多个请求）
2. 会话技术：
   1. cookie：浏览器端的会话技术，数据保存在浏览器
   2. session：服务器端的会话技术，数据保存在服务器端



# 第二章-Cookie

## 知识点-Cookie的原理和应用场景

### 1. 目标

- [ ] 理解会话cookie概念原理、Cookie的应用场景

### 2. 路径

1. Cookie的原理
2. Cookie的作用
3. 了解Cookie的应用场景

### 3. 讲解

#### 3.1 Cookie的原理

​	cookie 是 服务器端的servlet 发送到浏览器的少量信息，这些信息由浏览器保存，在后面的请求中可以通过请求头发送回服务器。

​	**servlet 通过使用 ==HttpServletResponse#addCookie==方法将 cookie 发送到浏览器，该方法将字段自动添加到 HTTP 响应头**，以便一次一个地将 cookie 发送到浏览器。浏览器应该支持每台 Web 服务器有  20 个 cookie，总共有 300 个 cookie，并且可能将每个 cookie 的大小限定为 4 KB。  

​	**浏览器通过向 HTTP 请求头添加字段将 cookie 提交给 servlet。servlet可以使用 ==HttpServletRequest#getCookies==方法从请求中获取 cookie。**一些 cookie 可能有相同的名称，但却有不同的路径属性。 

​	Cookie是一种客户端的会话技术,它是服务器存放在浏览器的一小份数据,浏览器以后每次访问该服务器的时候都会将这小份数据携带到服务器去。

![1597050654779](img/1597050654779.png)

#### 3.2 Cookie的作用

1. 在浏览器中存放数据(少量)
2. 请求时可以被浏览器自动带给服务器

#### 3.3 Cookie的应用场景

1.记住用户名
	当我们在用户名的输入框中输入完用户名后,浏览器记录用户名,下一次再访问登录页面时,用户名自动填充到用户名的输入框.
![img](img/2.png)

2.自动登录（记住用户名和密码）
	当用户在淘宝网站登录成功后,浏览器会记录登录成功的用户名和密码,下次再访问该网站时,自动完成登录功能.
以上这些场景都可以使用会话cookie实现的,将上次的信息保存到了cookie中,下次直接从cookie中获取数据信息
![img](img/3.png)

3.保存网站的上次访问时间
	访问网站的时候,通常会看到网站上显示上次访问时间,这些信息就是在用户访问网站的时候保存在cookie中的

4.保存电影的播放进度  

​	优酷、爱奇艺、腾讯视频

### 4. 小结

![1599700596431](img/1599700596431.png)

1. 第一次请求时，没有cookie
2. 在服务器端，通过new Cookie进行创建Cookie
3. 需要发送给浏览器： response.addCookie，浏览器进行保存
4. 后面的请求时，浏览器会带上cookie给到服务器
5. 服务器可以获取cookie：request.getCookies



## 知识点-Cookie的快速入门

### 1.目标

- [ ] 掌握Cookie的基本使用

### 2.路径

1. 相关的API
2. 入门代码

### 3.讲解

#### 3.1相关的API

+ ==创建Cookie==     // 注意事项，cookie只能保存string类型，不要存储中文

```java
new Cookie(String name,String value);
```

+  ==发送cookie== 给浏览器

```java
response.addCookie(cookie); 
```

+ ==获得Cookie==

```java
Cookie[] cookies = request.getCookies() ; //得到所有的cookie对象。是一个数组，开发中根据key得到目标cookie
```

+ cookie 的 API

```java
cookie.getName() ; //返回cookie中设置的key
cookie.getValue(); //返回cookie中设置的value
```

#### 3.2入门代码

需求：

![1599704915623](img/1599704915623.png)

```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/cookieDemo")
public class CookieDemoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 1. 创建cookie（cookie的值只能是String类型）
        Cookie cookie = new Cookie("akey", "avalue");

        // 2. 发送cookie
        // 发送cookie到浏览器（响应头发送的）
        response.addCookie(cookie);


        // 3. 获取所有的cookie
        // 我们只关注自己需要的cookie---想获取akey这个cookie
        // 获取自身设定的cookie这个场景比较常见。
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie ele : cookies) {
                if("akey".equals(ele.getName())){
                    System.out.println(ele.getName() + "=" + ele.getValue());
                }

            }
        }

    }
}

```



- CookieUtil

```java
package com.itheima.util;

import javax.servlet.http.Cookie;

public class CookieUtil {

    // 根据名字获取目标cookie
    public static Cookie getTargetCookie(Cookie[] cookies, String cookieName){

        Cookie cookie = null;
        // 获取自身设定的cookie这个场景比较常见。
        if(cookies != null){
            for (Cookie ele : cookies) {
                if(cookieName.equals(ele.getName())){
                    //System.out.println(ele.getName() + "=" + ele.getValue());
                    cookie = ele;
                    break;
                }

            }
        }
        return cookie;
    }
}

```

- 工具类测试‘

```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/cookieDemo")
public class CookieDemoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 1. 创建cookie（cookie的值只能是String类型）
        Cookie cookie = new Cookie("akey", "avalue");

        // 2. 发送cookie
        // 发送cookie到浏览器（响应头发送的）
        response.addCookie(cookie);


        // 3. 获取所有的cookie
        // 我们只关注自己需要的cookie---想获取akey这个cookie
        // 获取自身设定的cookie这个场景比较常见。
        /*Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie ele : cookies) {
                if("akey".equals(ele.getName())){
                    System.out.println(ele.getName() + "=" + ele.getValue());
                }

            }
        }*/

        Cookie targetCookie = CookieUtil.getTargetCookie(request.getCookies(), "akey");
        if(targetCookie != null){
            System.out.println(targetCookie.getName() + "=" + targetCookie.getValue());
        }
    }
}

```



### 4.小结

1. 服务器创建Cookie： new Cookie(name,value)
2. 在浏览器保存，发送给浏览器： response.addCookie(cookie)
3. 浏览器发送给服务器，服务器可以获取：request.getCookies()
4. 常用的就是根据名字获取cookie：CookieUtil





## 知识点-Cookie进阶

### 1.目标

+ 掌握设置Cookie的有效时长和有效路径

### 2.步骤

+ cookie的分类
+ cookie的有效路径

### 3.讲解

#### 3.1cookie的分类

​	在==默认的情况==下，当浏览器进程结束(浏览器关闭,会话结束)的时候，cookie就会消失。

​	有时候，需要保存久一些，则可以给cookie设置有效期。

给cookie设置有效期的API：

```java
/*
-1：默认。代表Cookie数据存到浏览器关闭（保存在浏览器文件中）。
 0：代表删除Cookie。
*/
cookie.setMaxAge(int expiry)   // 一周：  60*60*24*7
```

==删除cookie==：

```java
cookie.setMaxAge(0)  // 有效期设置为数字0
    
    
// 删除cookie（删除akey）
Cookie aCookie = CookieUtil.getTargetCookie(request.getCookies(), "akey");
if (aCookie != null) {
    // 删除（此时并不会影响浏览器）
    aCookie.setMaxAge(0);

    // 发送给浏览器才会在浏览器删除
    response.addCookie(aCookie);
}
```



**设置有效时长代码示例：**

```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo01")
public class ServletDemo01 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // cookie保存时间为一周

        // 1. 创建cookie
        Cookie cookie = new Cookie("bkey", "bvalue");
        // 设置时长(秒为单位)
        cookie.setMaxAge(60*60*24*7);


        // 2. 发送cookie
        response.addCookie(cookie);

        // 3. 获取cookie打印一下
        Cookie targetCookie = CookieUtil.getTargetCookie(request.getCookies(), "bkey");
        if(targetCookie != null){
            System.out.println("ServletDemo01获得的cookie:"+targetCookie.getName() + "=" + targetCookie.getValue());
        }
    }
}

```



- 删除Cookie示例

```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo02")
public class ServletDemo02 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // cookie保存时间为一周

        // 1. 创建cookie
        Cookie cookie = new Cookie("ckey", "cvalue");

        // 设置时长（为0表示删除cookie）
        cookie.setMaxAge(0);
        // 2. 发送cookie
        response.addCookie(cookie);

        // 3. 获取cookie
        Cookie targetCookie = CookieUtil.getTargetCookie(request.getCookies(), "ckey");
        if(targetCookie != null){
            System.out.println(targetCookie.getName() + "=" + targetCookie.getValue());
        }
    }
}

```





#### 3.2cookie设置有效路径

```java
cookie.setPath(String url) ;设置路径
```

​	**有效路径作用** :   保证不会提交给其他项目；

​	**注意事项**： 有效路径一样的话，cookie的名字是唯一的； 有效路径不一样，则可以存在同名的cookie。



- cookie的默认有效路径,例如: ==默认情况，是当前路径的上一目录级别的路径==，示例如下：

  - 访问http://localhost:8080/day26/demo01;  那么这个servlet设置的cookie的默认路径就是  [/day26]()
  
  + 访问http://localhost:8080/day26/aaa/demo01; cookie默认路径 [/day26/aaa]()
  
  + 访问http://localhost:8080/day26/aaa/bbb/demo01; cookie默认路径就是  [/day26/aaa/bbb]()
  
- 浏览器请求携带Cookie给服务器需要满足的条件: ==只有当访问资源的url包含此cookie的有效path的时候,才会携带这个cookie;==反之不会。  

  下次访问路径:http://localhost:8080/day26/demo02/ccc;  cookie是可以带过来

  下次访问路径:http://localhost:8080/day26/demo03;  cookie带不过来

- ==cookie的路径通常设置成 / 或者 **/项目名**。则当前项目下的Servlet都可以使用该cookie。==



示例代码：

- ServletDemo03路径为/demo03;  
  - 设置一个demo03的cookie
  - 遍历输出所有的cookie

- ServletDemo04路径为/demo04/aaa；
  - 设置一个demo04的cookie
  - 遍历输出所有的cookie
- 测试按步骤访问，查看是否获取到了cookie：demo03、demo04：
  - 访问demo03
  - 访问demo04/aaa
  - 访问demo03
  - 访问demo04/aaa



- **cookie的有效路径一般这么设置: ==cookie.setPath(request.getContextPath());==**

   那么只要是当前项目里面的资源，则必然路径中包含该项目名，则都可以使用这个cookie了。

  request.getContextPath()： 获得项目名。

所以==**一般的习惯做法是**==：

```java
// 1. 创建cookie： new Cookie(name,value)
// 2. 设置时长： cookie.setMaxAge(时间)
// 3. 设置有效路径为： cookie.setPath(request.getContextPath())
// 4. 发送cookie： response.addCookie(cookie)
```



```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo03")
public class ServletDemo03 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建cookei
        Cookie cookie = new Cookie("demo03", "demo03");

        // 设置有效路径：在发送之前设置才有效---设置路径为当前项目名
        cookie.setPath(request.getContextPath());

        // 发送cookie
        response.addCookie(cookie);





        // 获取所有cookie
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie ele : cookies) {
                System.out.println("ServletDemo03:"+ele.getName() + "=" + ele.getValue());
            }
        }
    }
}

```

```java
package com.itheima.a_cookie;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo04/aaa")
public class ServletDemo04 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建cookei
        Cookie cookie = new Cookie("demo04", "demo04");

        // 设置有效路径：在发送之前设置才有效---设置路径为当前项目名
        cookie.setPath(request.getContextPath());

        // 发送cookie
        response.addCookie(cookie);


        // 获取所有cookie
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie ele : cookies) {
                System.out.println("ServletDemo04:"+ele.getName() + "=" + ele.getValue());
            }
        }
    }
}

```





### 4.小结

1. 时长
   1. cookie.setMaxAge(秒)，参数为0可以删除cookie
   2. 删除：
      1. cookie.setMaxAge(0)
      2. response.addCookie(cookie)
2. 有效路径: 一般设置为当前项目路径   cookie.setPath(request.getContextPath())

## 案例-记录用户各自的上次访问时间

### 1.需求



![img](img/tu_1.gif)



- 需求说明：
  - 在访问一个资源的时候,展示上次访问的时间
  - 若是第一次访问则展示：你是第一次访问, 若不是第一次则展示:你上次访问的时间是:xxxx

### 2.分析

![1599708563727](img/1599708563727.png)

### 3.代码实现



```java
package com.itheima.a_cookie;

import com.itheima.util.CookieUtil;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

@WebServlet("/lastTimeServlet")
public class LastTimeServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=utf-8");


        //1. 拿到cookie(lastTime)
        Cookie targetCookie = CookieUtil.getTargetCookie(request.getCookies(), "lastTime");

        // 2. 第一次
        if(targetCookie == null){
            // 2.1 设置cookie
            Cookie cookie = new Cookie("lastTime", System.currentTimeMillis() + "");
            response.addCookie(cookie);

            // 2.2 响应
            response.getWriter().write("你是第一次访问！");
        }else{
            //3. 第二次第三次...
            // 3.1 取出cookie'的时间（上一次时间）
            String value = targetCookie.getValue();

            // 3.2 设置cookie时间为当前时间
            Cookie cookie = new Cookie("lastTime", System.currentTimeMillis() + "");
            response.addCookie(cookie);

            // 3.3 响应
            // 需要把毫秒值转成日期再格式化
            Date date = new Date(Long.valueOf(value));
            String format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(date);
            response.getWriter().write("你上一次访问的时间是：" + format);
        }
    }
}

```

### 4.小结

1. 注意事项： 判断第一次：目标cookie为null

2. 第一次、其它次数时，都需要设置cookie时间为当前时间

    



# 第三章-Session

## 知识点-session概述

### 1.目标

- [ ] 掌握Session的基本概念 以及和cookie的区别

### 2.路径

1. session概述 
2. cookie和Session的不同
3. Session的执行原理

### 3.讲解

#### 3.1session概述 

​	session是服务器端的技术。==服务器为每一个浏览器开辟一块内存空间，即session对象==。

​	由于session对象是每一个浏览器特有的，所以用户的记录可以存放在session对象中。

​	同时，每一个session对象都对应一个sessionId，==服务器把sessionId写到cookie中，再次访问的时候，浏览器把sessionId带过来，找到对应的session对象==

​	session'的id的cookie路径是项目路径。

#### 3.2cookie和Session的不同

- cookie是保存在浏览器端的，大小和个数都有限制。session是保存在服务器端的， 原则上大小是没有限制(实际开发里面也不会存很大大小), 安全一些。    
- cookie不支持中文，并且只能存储字符串；session可以存储基本数据类型，集合,对象等

#### 3.3Session的执行原理

​	1、获得cookie中传递过来的SessionId(cookie)   ==request.getSession==

​	2、如果Cookie中没有sessionid,则创建session对象

​	3、如果Cookie中有sessionid,找指定的session对象

​		如果有sessionid并且session对象存在，则直接使用

​		如果有sessionid，但session对象销毁了，则执行第二步



![1597117233190](img/1597117233190.png) 

### 4.小结

1. session介绍
   1. 服务端的会话技术，数据是保存在服务器端的
2. sessionn原理
   1. 第一次请求时，没有cookie（没有sessionid）
   2. 在服务器调用request.getSession()，创建session，把sessionid给到浏览器
   3. 后面的请求
      1. 浏览器带上了cookie（sessionid），在服务器调用request.getSession()
      2. 查找session
         1. 找到了，则直接用
         2. 找不到，则创建新的session，把sessionid给到浏览器



## 知识点-Session基本使用、作为域对象存取值

### 1.目标

- [ ] 掌握Session的使用,作为域对象存取值

### 2.路径

+ Session基本使用  

### 3.讲解

​	==范围: 会话(多次请求)== 保存用户各自的数据(以浏览器为单位)   

获得session对象：

```java
HttpSession session = request.getSession(); 

// 获得id
String sessionId = session.getId();
```

使session失效（也叫删除session）：

```java
session.invalidate();
```



示例：

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet("/sessionDemo")
public class SessionDemoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取session
        // 自动通过cookie给到浏览器：名字叫做 JSESSIONID
        HttpSession session = request.getSession();

        String sessionId = session.getId();

        System.out.println("sessionId:"+sessionId);


        // 删除session，使得session失效
        session.invalidate();

    }
}

```



==**作为域对象存取值【重要】**==

- void setAttribute(String name, Object value) ;存储值
- Object getAttribute(String name) ;获取值
- void removeAttribute(String name)  ;移除

示例：

![1597120176109](img/1597120176109.png)

![1597120198314](img/1597120198314.png)

ServletA:

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet("/servletA")
public class ServletA extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置值

        // 拿到session
        HttpSession session = request.getSession();


        // 作为域对象存取值
        session.setAttribute("skey", "svalue");
    }
}

```



ServletB:

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet("/servletB")
public class ServletB extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置值

        // 拿到session
        HttpSession session = request.getSession();


        // 作为域对象存取值
        Object skey = session.getAttribute("skey");

        System.out.println(skey);
    }
}

```





### 4.小结

1. session的基本使用
   1. 获得session： request.getSession()
   1. 删除： session.invalidate()
1. 作为域对象存取值
   1. void setAttribute(String name, Object value) ;存储值
   1. Object getAttribute(String name) ;获取值
   1. void removeAttribute(String name)  ;移除

## 知识点-三个域对象比较  

### 1.目标

- [ ] 掌握三个域对象的不同

### 2.路径

1. 三个域对象比较 
2. 三个域对象怎么选择

### 3.讲解

#### 3.1三个域对象比较 

| 域对象             | 创建                                          | 销毁                                                         | 作用范围       | 应用场景                                                |
| ------------------ | --------------------------------------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------- |
| ServletContext     | 服务器启动                                    | 服务器正常关闭/项目从服务器移除                              | 整个项目       | 记录访问次数,聊天室                                     |
| HttpSession        | ==没有session 调 用==request.getSession()方法 | session过期（默认30分钟）/或者调用invalidate(）方法/服务器==异常==关闭 | 会话(多次请求) | 验证码校验, ==保存用户登录状态==等                      |
| HttpServletRequest | 来了请求                                      | 响应这个请求(或者请求已经接收了)                             | 一次请求       | servletA和jsp（servletB）之间数据传递(转发的时候存数据) |

#### 3.2三个域对象怎么选择? 

三个域对象怎么选择? 

​	一般情况下, 最小的可以解决就用最小的.

​	==但是需要根据情况(eg: 重定向, 多次请求, 会话范围, 用session;  如果是转发,一般选择request)==

范围  上下文对象 >  session  > request



### 4.小结

1. session的基本使用
   1. 获取session： request.getSession()
   2. 删除、失效：session.invalidate()
   3. 获取id值：session.getId()
2. 作为域对象存取值
   1. setAttribute(name,value)
   2. getAttribute(name)
   3. removeAttribute(name)
3. 域对象选择：重定向、多次请求之间---session；   请求转发---request





## 案例-一次性验证码校验

### 1.需求

![](img/login_validator.gif)

​	

​	需求描述：登录中增加一个验证码，每次点击验证码图片会切换新的图片。

### 2.分析

#### 2.1生成验证码

1. 拷贝验证码的jar包![1597069691859](img/1597069691859.png)

   1. API介绍【对着抄就行】

   2. ```java
      // 创建一个ValidateCode对象
      // 参数含义： 图片的宽度、高度、字符数目、干扰线数目
      ValidateCode validateCode = new ValidateCode(300, 80, 4, 10);
      // 响应到页面：  字节流
      validateCode.write(response.getOutputStream());
      
      // // API 获得生成的字符
      String code = validateCode.getCode();
      ```

   

   

**验证码初体验：**

- 创建CodeServletDemo

```
//1.生成验证码
//2.响应给客户端(浏览器)
```

- 页面：

```html
<a href="codeDemo">验证码图片体验(CodeDemoServlet)</a><br>
```



- 实现

CodeDemoServlet.java

```java
package com.itheima.c_session;

import cn.dsna.util.images.ValidateCode;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/codeDemo")
public class CodeDemoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑  就是响应一个图片

        // 获得一个验证码对象
        ValidateCode validateCode = new ValidateCode(300, 80, 4, 10);

        // 获得字符
        String code = validateCode.getCode();
        System.out.println(code);

        // 响应输出到页面
        validateCode.write(response.getOutputStream());
    }
}

```

- 访问测试：访问项目的 /codeDemo

![1597070557819](img/1597070557819.png)





**img标签验证码体验：**

```html
<!--绝对路径：http://localhost:8080/day26_cookie_session_war_exploded/codeDemo
    相对路径： codeDemo
-->
<img src="codeDemo"><br>
```

#### 2.2登录逻辑校验验证码

**思路分析：**

![1599723142717](img/1599723142717.png)

![1599723413253](img/1599723413253.png)

### 3.实现

#### 环境准备

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
        <form action="login" method="post">
            用户名：<input type="text" name="username"><br>
            密码：<input type="password" name="password"><br>
            验证码：<input type="text" name="inputCode"><br>
            <!-- 来自于servlet-->
            <img src="codeServlet"/><br>
            <input type="submit" value="登录">
        </form>
</body>
</html>
```



#### 登录页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="login" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    验证码：<input type="text" name="inputCode"><br>
    <!-- 来自于servlet-->
    <img src="codeServlet" onclick="changeImg()" id="imgId"><br>
    <input type="submit" value="登录">

</form>
</body>

<script>
    function changeImg() {
        // 就是改变img元素的src属性
        var imgEle = document.getElementById("imgId");
        //imgEle.setAttribute("src", "codeServlet")

        // 给src设置属性值（欺骗浏览器）
        // 加一个参数为时间日期对象，让浏览器以为请求有所变化，会去请求服务器
        imgEle.src = "codeServlet?a="+new Date();

    }
</script>
</html>
```

#### CodeServlet

```java
package com.itheima.c_session;

import cn.dsna.util.images.ValidateCode;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet("/codeServlet")
public class CodeServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 1创建一个验证码
        ValidateCode validateCode = new ValidateCode(300, 80, 4, 10);

        // 2存储验证码字符到session
        String code = validateCode.getCode();

        // 获得session
        HttpSession session = request.getSession();
        session.setAttribute("code", code);

        // 3做响应
        validateCode.write(response.getOutputStream());
    }
}

```

+ #### LoginServlet

```java
package com.itheima.c_session;

import com.itheima.bean.User;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.sql.SQLException;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        // 1. 校验验证码
        // 1.1 获得输入的码
        String inputCode = request.getParameter("inputCode");
        // 1.2 获得session的码
        HttpSession session = request.getSession();
        String sessionCode = session.getAttribute("code").toString();
        // 1.3 比较
        if(!sessionCode.equalsIgnoreCase(inputCode)){
            // 验证码错误
            response.getWriter().write("验证码错误。");
            return;
        }

        try {
            // 2.开始登录处理
            // 2.1 获得输入的用户名、密码
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            // 2.2 DBUtils技术
            QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());

            String sql = "select * from user where username = ? and password = ?";

            User user = queryRunner.query(sql, new BeanHandler<>(User.class), username, password);

            // 2.3 判断登录
            if(user != null){
                response.getWriter().write("<font color='green'>登录成功</font>");
            }else{
                response.getWriter().write("登录失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙,稍后再试");
        }

    }
}

```

### 4.小结   ###

1.  切换图片
   
   1.  给img增加点击事件
   2.  欺骗浏览器，就是加请求参数值每次不一样
   3.  ![1599722096377](img/1599722096377.png)
   
2. 验证码登录逻辑校验

   1.  先校验验证码
       1.  获得了输入的码
       2.  从session中获得的码
       3.  二者做匹配（不区分大小写）
   2.  如果验证码匹配的上，才进行登录操作
    1.  根据用户名、密码去查询用户
       2.  查到了表示登录OK
   
   



# 第四章_JSP入门

## 知识点-JSP概述

### 1.目标

+ 掌握什么是JSP,   知道JSP产生的原因

### 2.讲解

#### 2.1 什么是JSP

​	Java server page(java服务器页面).  ==JSP本质就是Servlet==    

​	它和servle技术一样，都是SUN公司定义的一种用于开发动态web资源的技术。

​	JSP=html(js,css)+java+jsp特有的内容

#### 2.2.JSP产生的原因

**需求**: 我们要向页面动态输出一个表格. 发现特别的繁琐

servlet在展示页面的时候，相当的繁琐。sun公司为了解决这个问题，参照asp开发了一套动态网页技术jsp。

#### 2.3 JSP执行原理[了解]

`C:\Users\byron4j\.IntelliJIdea2019.2\system\tomcat\Tomcat_8_5_27_p100_3\work\Catalina\localhost\day26\org\apache\jsp`

JSP会翻译(通过默认的JspServlet,JSP引擎)成Servlet(.java),Servlet编译成class文件

​	JSP执行流程

​		第一次访问的xxx.jsp时候,服务器收到请求,会去查找对应的jsp文件

​		找到之后,服务器会将这个jsp文件转换成.java文件(Servlet)

​		服务器编译java文件,生成class文件

​		服务器运行class文件,生成动态的内容

​		服务器收到内容之后,返回给浏览器

 ![img](img/tu_5.png)



### 3.小结

1. JSP本质上是servlet
2. 可以写html、js、css、java、jsp独有的内容

## 知识点-JSP基本语法

### 1.目标

+ 掌握JSP脚本和注释

### 2.讲解

#### 2.1JSP脚本

我们可以通过JSP脚本在JSP页面上编写Java代码. 一共有三种方式:

| 类型                  | 翻译成Servlet对应的部分                             | 注意                      |
| --------------------- | --------------------------------------------------- | ------------------------- |
| <%...%>:Java程序片段  | 翻译成Service()方法里面的内容, 局部的               |                           |
| <%=...%>:输出表达式   | 翻译成Service()方法里面的内容,相当于调用out.print() | ==输出表达式不能以;结尾== |
| <%!...%>:声明成员变量 | 翻译成Servlet类里面的内容                           |                           |

JSP初体验：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>


<%--注释ctrl+shift+/

    <%...java代码，java类里面怎么写这里就怎么写
        变成了方法的局部内容
    %>
--%>
<%
    int i = 10;
    System.out.println(i);
%>


<%--
    <%=..写表达式，但是不能以;结尾.
        变成了方法的局部内容，表达式结果会在页面输出

    %>
--%>
<%=100+200%>


<%--
    <%!...声明字段、方法
        变成了类的成员变量。
    ..%>
--%>
<%!
    String hello = "hello,world...";

    public String sayHello(){
        return  "hello...";
    }
%>
</body>
</html>

```

#### 2.2JSP注释

| 注释类型                      |
| ----------------------------- |
| HTML注释<!--HTML注释-->       |
| JAVA注释     //; /* */        |
| JSP注释;     <%--注释内容--%> |

==注释快捷键:Ctrl+Shift+/==

### 3.小结

1. JSP脚本片段
   1. java代码段： <%....%>，内容成了局部内容
   2. 表达式： <%=表达式%>，在页面输出表达式的结果
   3. 声明： <%!声明字段、方法%>, 类的成员变量

## 案例-记住用户名案例

### 1.需求

![image-20191120112234968](img/image-20191120112234968.png)

-  如果勾选上了复选框，并且登录OK了，那么下一次进入这个页面会自动填充用户名
-  要求使用JSP技术实现

### 2.分析

jsp本身是一个servlet，可以通过session共享数据。

![1599727319040](img/1599727319040.png)



### 3.实现

- 页面 login.jsp，前端可以先抄

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录</title>
</head>
<body>

${username}

<center>
    <h1>用户登录</h1>
    <form action="loginServlet4Jsp" method="post">

        <%--文本框的默认值： value属性
              提前用到了明天的内容： EL表达式--获得域对象里面的值
        --%>
        姓名：<input type="text" name="username" value="${username}"><br>
        密码：<input type="password" name="password"><br>
        <input type="checkbox" name="rememberName" value="rememberName">记住用户名<br>
        <input type="submit" value="登录">
    </form>
</center>
</body>
</html>

```

+ LoginServlet4Jsp

```java
package com.itheima.c_session;

import com.itheima.bean.User;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.sql.SQLException;

/**
 * java圈子：  4 表示  for、 2 表示 to
 */
@WebServlet("/loginServlet4Jsp")
public class LoginServlet4Jsp extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 乱码处理
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=utf-8");

        try {
            // 1. 获得参数： 用户名、密码、复选框的值
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            // 如果勾选了这个值就是 rememberName
            String rememberName = request.getParameter("rememberName");

            // 2. 登录、判断 勾选
            // 登录--使用dbutils技术
            QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());
            String sql = "select * from user where username = ? and password = ?";
            User user = queryRunner.query(sql, new BeanHandler<>(User.class), username, password);

            // 判断有没有勾选
            if("rememberName".equals(rememberName)){
                // 勾选则保存用户名到session中
                HttpSession session = request.getSession();
                session.setAttribute("username", username);
            }


            response.getWriter().write("登录成功");

        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙");
        }
    }
}

```





### 4.小结

1.  页面是否勾选了记住用户名这个复选框，勾选了就在登录后保存数据
2.  页面的写法![1599726430132](img/1599726430132.png)



# 回顾要点

1. 会话技术： 一次会话：用户打开浏览器，浏览网页，关闭浏览器
	
	1. 可以保存用户自身数据
	
	

2. Cookie浏览器端的会话技术，数据保存在浏览器

   1. 原理流程
      1. 第一次请求时，此时没有cookie
      2. 如果在服务器中使用new Cookie(name, value)
      3. 发送给浏览器： response.addCookie(cookie)
      4. 后面的请求，浏览器可以带上cookie，给到服务器
         1. 服务器获取所有的cookie： request.getCookies()
   2. Cookie的使用，API
      1. 创建：new Cookie(name, value)，value只能是字符串，不支持中文
      2. 发送：response.addCookie(cookie)
      3. 默认情况，cookie是关闭浏览器失效
      4. 设置时长： cookie.setMaxAge(秒)
      5. 删除cookie
         1. cookie.setMaxAge(0)
         2. response.addCookie(cookie)
      6. cookie的有效path： 一般而言设置为项目路即可：cookie.setPath(request.getContextPath())

   3. Session服务器端的会话技术，数据保存在服务器
      1. 原理流程
         1. 第一次请求服务器，没有cookie（没有session的id）
         2. 服务器调用request.getSession()
            1. 创建session
            2. 把session的id值通过cookie给到浏览器
         3. 再次访问，如果cookie中包含了session的id值
         4. 服务器调用request.getSession()
            1. 进行查找
               1. 找到了直接用
               2. 找不到，已经销毁则进行创建新的session，把session的id值通过cookie给到浏览器
      2. session的API
         1. 获取session：request.getSession()
         2. 获取session的id：session.getId()
         3. 删除session、失效：session.invalidate()
         4. 作为域对象存取值
            1. 设置值 setAttrributer(name,value)
            2. 获取值 getAttrributer(name)
      3. 三个域对象的选择：
         1. 重定向、多次请求------session
         2. 一次、转发----request
   4. JSP入门：
      1. 就是一个servlet
      2. 三个代码段

3. 案例

   1. 显示上一次访问时间： 判断是不是第一次（cookie是否为null）
   2. 一次性验证码： 先校验验证码，再登录
   3. 记住用户名：判断是否勾选了复选框，勾选了才保存到session







# 预习要点

- EL表达式获取数据的方式（简单类型、list、map、数组、对象）
- jsp标签
  - if
  - foreach
- 三层架构（开发模式）---转账案例





【扩展内容-session钝化、活化】：了解即可

正常关闭服务器时，可以在以下目录看到一个`SESSIONS.ser`文件，保存了session相关数据。

`C:\Users\用户名\.IntelliJIdea20XXX\system\tomcat\Tomcat_8_5_27_XXX\work\Catalina\localhost\项目` 

- 如果是正常关闭服务器,

​	把session钝化到服务器磁盘上,再次启动,把磁盘上的文件活化到内存里面

​	Session钝化：把内存中的session序列化到硬盘上

​	Session活化：从硬盘上读取序列化的session到内存中




