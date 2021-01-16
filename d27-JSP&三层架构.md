---
typora-copy-images-to: img
---

# day25-JSP&三层架构

# 教学目标

- [ ] 能够使用el表达式获取javabean的属性
- [ ] 能够使用jstl标签库的if标签
- [ ] 能够使用jstl标签库的foreach标签
- [ ] 能够完成转账案例
- [ ] 能够使用ThreadLocal类

# 第一章-EL表达式

## 知识点-EL表达式概述

### 1.目标

- 能够说出el表达式的作用

### 2.讲解

#### 2.1 EL表达式的作用

-  **最主要的作用就是**： 获取==域对象(request,session,ServletContext)==中通过`setAttribute()`存储的数据

-  也可以在里面执行运算

<hr>

语法：  ==${el表达式}==，在现在的jsp中可以直接使用。

#### 2.2 EL入门示例

使用示例：

如果在域对象设置了：   request.setAttribute("username", "zs")

那么可以jsp中使用：  ${username}  获取到值 "zs"



```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--写java代码--%>

<%
    /*设置域对象里面的值*/
    request.setAttribute("username", "zs");
%>


<%--EL初体验：${域对象里面设置的key}--%>
用户名为：${username}
</body>
</html>

```



### 3.小结

1. EL表达式：   就是获取域对象里面的数据



## 知识点-El获取数据【掌握】

### 1.目标

- 能够使用el表达式获取域里面的数据(==先要把数据存到域对象里面==)

### 2.路径

- 获取简单数据类型数据(基本类型,字符串)
- 获取数组
- 获取list
- 获取Map
- 获取javabean属性

### 3.讲解

EL入门：

​	语法:${requestScope|sessionScope|applicationScope.key};   分别获取request、session、servletContext 3个域对象里面存取的key值，这里的==**属性名或者key**==代表 setAttribute(key, value)的key

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--往3个域对象中存储值--%>
<%
    request.setAttribute("rkey", "rvalue");

    request.setAttribute("skey", "sssss");

    session.setAttribute("skey", "svalue");
    // application 代表上下文对象 servletContext
    application.setAttribute("akey", "avalue");
%>

<%--EL获取数据--%>
获取request里面的数据： ${requestScope.rkey}<br>
获取session里面的数据： ${sessionScope.skey}<br>
获取application（上下文对象）里面的数据： ${applicationScope.akey}<br>
<hr>
<%--简化的书写方式  ${key}
    获取的优先规则：
        先从最小范围中找从request找，找到了直接返回打印输出；
        request找不到，继续去session找，找到了直接返回打印输出；
        session找不到，才去application找，找到了直接返回打印输出，找不到则是空内容显示；
        范围：request < session < application
--%>
获取request里面的数据： ${rkey}<br>
获取session里面的数据： ${skey}<br>
获取application（上下文对象）里面的数据： ${akey}<br>
</body>
</html>

```

#### 语法汇总

1. EL获取简单数据类型

   ```jsp
   ${key}
   ```

2. 获取数组元素

   ```jsp
   ${key[下标]}
   ```

3. 获取list元素

   ```jsp
   ${key.get(索引值)}
   ```

4. 获取map元素

   ```jsp
   ${key.get("map里面的key")}
   ```

5. 获取javabean属性

   ```jsp
   ${key.属性名}
   ```

   ==备注： ${key}  的 key值是setAttribute("key", value)中的key==



#### 3.1获取简单数据类型数据

==${key},  key就是存在域对象里面的key： setAttribute(key, value)中的key==

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>简单类型</title>
</head>
<body>
<%
    request.setAttribute("akey", "aaa");
    request.setAttribute("bkey", 12);

%>

<%--基本类型、string：${key}--%>
获取简单类型：${akey}、 ${bkey}

</body>
</html>

```



#### 3.2获取数组

​	语法: ==${key[下标]}==    key就是域对象里面存的key：  setAttribute(key, value)

```jsp

<%

    // 存储了一个数组
    String[] arr = {"aaa","bbb","ccc"};
    request.setAttribute("arr", arr);
%>

<%--获取数组中的元素： ${key[下标从0开始]}--%>
获取数组中的元素：<br>
${arr[0]}、${arr[1]}、${arr[2]}

```



#### 3.3获取list

​	语法：==${key.get(index)}==  或者${key[index]};   key就是存入域对象里面的key

```jsp
<%
    List<String> list = new ArrayList<>();
    list.add("aaa");
    list.add("bbb");
    list.add("ccc");
    request.setAttribute("lkey", list);

    
%>

<%--获取list里面的元素：${key.get(index从0开始)}
    也可以使用  ${key[下标]}
--%>
获取list里面的元素： <br>
${lkey.get(0)}、${lkey.get(1)}、${lkey.get(2)}
<br>
获取list里面的元素--当做数组来用： <br>
${lkey[0]}、${lkey[1]}、${lkey[2]}
```



#### 3.4获取Map

​	 语法:${key.键}或者==${key.get("键")}==。

​	**key**就是存入域对象里面的key, **键**是map对象中的key。

```jsp
<%
    

    // 存储map格式的数据
    Map<String,String> map = new HashMap<>();
    map.put("username", "zs");
    map.put("password", "123");
    request.setAttribute("map", map);
%>

<%--获取map里面的元素：${key.get("键")}  --%>
获取map里面的元素： <br>
${map.get("username")}、${map.get("password")}<br>

<%--${key.map里面的键名}--%>
获取map里面的元素： <br>
${map.username}、${map.password}
```



#### 3.5 获取bean

​	==语法:${key.javabean属性}==

​	javabean的属性依赖getXXX()方法; eg: getPassword()---去掉get-->Password()----首字母小写--->password



- javabean

```java
/**
 * 1. 字段私有
 * 2. 方法公共
 * 3. 空构造
 * 4. 实现序列化接口
 */
public class UserBean implements Serializable {
    private Integer id;
    private String username;
    private String password;
	
    //...
}
```

- jsp

```jsp
<%@ page import="com.itheima.bean.UserBean" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%
    UserBean userBean = new UserBean();
    userBean.setId(1);
    userBean.setUsername("zsf");
    userBean.setPassword("123");
    request.setAttribute("user", userBean);
%>

<%--获取javabean的属性： ${key.javabean的属性名}
    获取javabean的属性，调用了你的getXXX方法
--%>
获取javabean的属性：<br>
${user.id}、${user.username}、${user.password}
</body>
</html>

```





### 4.小结

#### 4.1语法

1. 获取简单类型（基本类型、字符串）： ${key}
2. 获取数组： ${key[下标]}
3. 获取list： ${key.get(索引值)}、${key[下标]}
4. 获取map：  ${key.get("键名")}、${key.键名}
5. 获取javabean的属性： ${key.javabean的属性名}

#### 4.2注意事项

[]和.方式的区别: 有特殊字符的要用[];  **能够用.获取的都可以用[]获取**

- 若key中出现了"."、"+"、"-" 等特殊的符号的时候（==这种做法禁止发生，源头拒绝！！==），使用[]获取：


![1597284035798](img/1597284035798.png)

```jsp
${map["a.b.c"]}
```

## 知识点-EL执行运算

### 1.目标

- 掌握empty运算符

### 2.讲解

运算符和java中的 +、-、*、/、>、<、<=、>=......一样的。

关注和java不一样的  ==empty== 运算符

#### 2.1算数运算

​	+,-,*,/

- ==注意事项==：+不能拼接字符串：  ${1+"aaaa"} 这种写法是错误的！

#### 2.2逻辑运算

​	< >= <= != ==

#### 2.3关系运算

​	&& || !

#### 2.4 判空

​	**empty：**空。以下3种情况返回true，表示empty：

1. 对象为null

2. 集合长度为0
3. 一个字符串为空串 ""

​	**not empty：** 非空

​	语法: ==${empyt     表达式或者域对象的key}==  属性名 就是域对象里面的key值;

```jsp
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%
    String[] arr = {"aaa","bbb","ccc"};
    request.setAttribute("ekey", arr);

    List<String> list = new ArrayList<>();
    request.setAttribute("lkey" , list);

    request.setAttribute("skey", "");
%>

<%--语法：  ${empty 表达式、域对象里面的key}
    empty的含义是判空：
        1. 对象为null
        2. 集合、数组元素为0
        3. 字符串内容为""

--%>

${empty null}、${empty lkey}、${empty skey}

<%--演示错误示范：
${1+"aaa"}--%>
<hr>
判断是否不为空： ${not empty ekey}
</body>
</html>

```

### 3.小结

1. empty运算符: 判空运算符
   1. 对象为null
   2. 集合、数组没有元素
   3. 字符串是空串""





​	

# 第二章-JSTL标签库



## 知识点-JSTL核心标签库

### 1.目标

- 掌握if,foreach标签的使用

### 2.讲解

​		JSTL（JSP Standard Tag Library，JSP标准标签库)是一个不断完善的开放源代码的JSP标签库，是由apache的jakarta小组来维护的。==**这个JSTL标签库没有集成到JSP的, 要使用的话, 需要导jar包**==.

​		JSTL标签库可以简化在jsp页面上操作数据的写法：  eg:  遍历数据 判断数据等

​		我们学核心标签库if、foreach。

#### 2.1核心标签库使用步骤

1. 导入jar包 ![img](img/tu_12.png)

2. 在JSP页面上导入核心标签库

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

> ==**编程习惯**：因为jsp本质上就是servlet，所以一般而言，在后台servlet添加域对象数据，然后转发到jsp页面，jsp里面通过EL表达式获取域对象里面的值。==

#### 2.3if标签

- 语法： **test属性值为true，则会执行标签体里面的内容**。

```jsp
<c:if test="写el表达式${..}，返回true、false" 
      var="变量名，可以保存test的返回结果" 
      scope="把var变量保存到对应的域对象中">
    		标签体的内容
</c:if>
```

- 实例

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <%--if 标签：

        test属性必须： EL表达式，结果为true，则会执行if标签体的内容
        了解听听的属性：
            var： 把test属性的结果true、false用户变量保存起来
            scope： 把a存储到对应的对象中去
                    request、session、application
                    page：当前jsp页面对象



    --%>

    <c:if test="${100>1}" var="a" scope="page">
        100确实是大于1的
    </c:if>
    <%--没有else标签，想实现else效果，需要写多个if开实现--%>
    <c:if test="${100<=1}">
        100确实是<=1的
    </c:if>


</body>
</html>

```

- 小结

  - 语法格式

  - ```jsp
    // test属性为true则会显示标签体的内容
    <c:if test="${el表达式}" >
  	内容
    </c:if>
    ```
  

#### 2.5foreach标签

- **简单的使用**: 属性   **begin、end、var 、step**的使用      ==【在标签体里面需要使用${}获取变量的值】==
  - 案例：  遍历输出1-10，且随之输出下划线<hr>

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--引入核心标签库--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--
    遍历输出1-10
    forEach标签的简单使用场景的属性：
        begin： 从哪个值开始，写数值形式
        end：  到哪个值结束，最大不能超过这个值，写数值形式
        var： 保存每次遍历的值的变量名，可以在标签体中使用${}获取值
        step（了解）: 步长的意思，每次增加几
--%>
<c:forEach begin="1" end="10" var="a" step="2">
    ${a}<hr>
</c:forEach>
</body>
</html>

```

​				

- ==**复杂的使用**==: 属性 ==**items、var、varStatus** 的使用==           ==【在标签体里面需要使用${}获取变量的值】==

  -  items属性：  就是一个可迭代的对象；域对象里面的key---==写EL表达式==
  -  var： 是每次遍历的元素
  -  varStatus： 对象；保存了是每次遍历的元素的一些状态信息
    - index:返回索引。从0开始
    - count:返回计数。从1开始
    - last:是否是最后一个元素，布尔类型
    - first:是否是第一个元素，布尔类型

- 案例：给定一个集合设置到域对象中，jsp遍历输出集合中的元素；输出以下的表格形式
  
  ![1597158568682](img/1597158568682.png)
  
  

代码：

```jsp
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--引入核心标签库--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    List<String> list = new ArrayList<>();
    list.add("aaa");
    list.add("bbb");
    list.add("ccc");

    request.setAttribute("lkey",list);
%>


<%--遍历输出集合中的元素--%>
<%--
    forEach复杂使用场景：
        items： 写EL表达式，里面是一个可以迭代的对象比如你可以在域对象中存储一个集合、数组
        var： 保存每次遍历的那个元素，可以在标签体中使用${}来获取值
--%>

<c:forEach items="${lkey}" var="ele">
    ${ele}
</c:forEach>


<%--纯HTML可以画出来--%>
<table border="1px">
    <tr>
        <td>索引</td>
        <td>第几个</td>
        <td>值</td>
        <td>是否第一个</td>
        <td>是否最后一个</td>
    </tr>


    <%--
        forEach复杂使用场景：
        items： 写EL表达式，里面是一个可以迭代的对象比如你可以在域对象中存储一个集合、数组
        var： 保存每次遍历的那个元素，可以在标签体中使用${}来获取值
        仅作了解的属性：varStatus，对象形式
                           index: 获得索引从0开始
                           count： 获得第几个，从1开始
                           first： 获得是否第一个布尔值
                           last：获得是否最后一个布尔值
    --%>
    <c:forEach items="${lkey}" var="ele" varStatus="status">
        <%--内容是不是每次遍历都会执行--%>
        <tr>
            <td>${status.index}</td>
            <td>${status.count}</td>
            <td>${ele}</td>
            <td>${status.first}</td>
            <td>${status.last}</td>
        </tr>
    </c:forEach>

</table>
</body>
</html>

```



### 3.小结

1. 简单使用方式

   ```jsp
   <c:foreach begin="数值" end="结束值" var="a" step="步长">
       ${a}
   </c:foreach>
   ```

   

2. 复杂使用方式

   ```jsp
   <c:foreach items="域对象里面的可迭代的key： ${key}"  var="ele" varStatus="">
       ${ele}
   </c:foreach>
   ```

   

# 第三章-综合案例和开发模式

## 案例-转账的案例v0-转账无事务实现

### 一.需求

- 当单击提交按钮，付款方向收款方按照输入的金额转账。

  

![img](img/tu_1-1574219049777.png)

### 二,分析

 ![1599795494842](img/1599795494842.png)

### 三,实现

#### 1.案例的准备工作

- 数据库的准备

  ```mysql
  create database day27;
  use day27;
  create table account(
      id int primary key auto_increment,
      name varchar(20),
      money double
  );
  
  insert into account values (null,'zs',1000);
  insert into account values (null,'ls',1000);
  insert into account values (null,'ww',1000);
  ```

- 页面 transofrm.jsp

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
  </head>
  
  <body>
  <form action="transformServlet" method="post" >
    <table border="1px" width="500px" align="center">
      <tr>
        <td>付款方</td>
        <td><input type="text" name="from"></td>
      </tr>
      <tr>
        <td>收款方</td>
        <td><input type="text" name="to"></td>
      </tr>
      <tr>
        <td>金额</td>
        <td><input type="text" name="money"></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit"></td>
      </tr>
    </table>
  </form>
  
  </body>
  </html>
  
  ```

- jar包：

  - 驱动
  - c3p0
  - DUtils

- 工具类：C3p0Utils.java

- 配置文件：c3p0-config.xml

#### 2.代码实现

- TransformServlet

```java
package com.itheima.web;

import com.itheima.util.C3P0Utils;
import com.mysql.fabric.xmlrpc.base.Params;
import org.apache.commons.dbutils.QueryRunner;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.sql.SQLException;

@WebServlet("/transformServlet")
public class TransformServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        try {
            //1. 获取页面参数：付款方、收款方、金额
            String from = request.getParameter("from");
            String to = request.getParameter("to");
            Double money = Double.valueOf(request.getParameter("money"));

            //2. 转账操作（DBUtils）
            QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());
            //2.1 付款方减少金额
            String fromSql = "update account set money = money - ? where name = ?";
            int fromCnt = queryRunner.update(fromSql, money, from);

            //2.2 收款方增加金额
            String toSql = "update account set money = money + ? where name = ?";
            int toCnt = queryRunner.update(toSql, money, to);


            //3. 响应
            //3.1 判断转账成功：2个人都执行了
            if(fromCnt>0 && toCnt>0){
                // 成功
                response.getWriter().write("转账成功");
            }else {
                response.getWriter().write("转账失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙");
        }
    }
}

```



### 四.小结

1. 转账: 一个用户的钱减少, 一个用户的钱增加

## 知识点-开发模式  

### 1.目标

- 理解模式二(MVC)和模式三(三层架构)

### 2.讲解

#### 2.1JSP的开发模式一【已成历史，仅做了解】

缺点： 所有的代码全部写在jsp里面，没法维护：业务逻辑代码、操作数据库、显示数据......



#### 2.2 传统MVC开发模式

​	JSP + Servlet + JavaBean 称为传统MVC的开发模式.

​	==MVC:开发模式==

​	M：model 模型 （javaBean：封装数据）

​	V：View 视图  （JSP：展示数据）

​	C：controller 控制器 （Servlet：处理逻辑代码，做为控制器）

优点： 各司其职：代码、控制逻辑写在servlet里面，jsp显示页面给用户。

缺点：业务功能复杂时，servlet的代码难以维护......



概念视图：



![1599799365966](img/1599799365966.png)



#### 2.3模式三: 三层架构

- 软件中分层：按照不同功能分为不同层，通常分为三层：表现层(web层--和用户打交道)，业务层（专门处理业务），持久(DAO层--Data Access Object，和数据库打交道)层。

  

  ![img](img/tu_12-1568797747077.png)

- 概念视图：

  ![1599799556083](img/1599799556083.png)

- 实施方式：不同层次包名的命名

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



### 3.小结

1. 三层架构
   1. web
      1. 获取参数
      2. 调用业务层
      3. 响应
   2. 业务
      1. 专门处理业务
      2. 调用DAO
   3. 持久层DAO
      1. 就是进行数据库的CRUD
   4. MVC思想
      1. M：数据模型javabean
      2. V：JSP
      3. C：web层

## 案例-转账的案例v1-三层架构无事务实现

### 一.需求 ###

- 当单击提交按钮，付款方向收款方按照输入的金额转账。

- 目的： 使用三层架构编写

  


![img](img/tu_1-1574219049777.png)

### 二,分析

 ![1597303169298](img/1597303169298.png)

### 三,实现

#### 代码实现

+ web层

```java
package com.itheima.web;

import com.itheima.service.TransformService;
import com.itheima.util.C3P0Utils;
import com.mysql.fabric.xmlrpc.base.Params;
import org.apache.commons.dbutils.QueryRunner;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.sql.SQLException;

@WebServlet("/transformServlet")
public class TransformServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        //1. 获得参数
        try {
            String from = request.getParameter("from");
            String to = request.getParameter("to");
            Double money = Double.valueOf(request.getParameter("money"));

            //2. 调用业务层-转账操作
            TransformService transformService = new TransformService();
            boolean flag = transformService.transform(from,to,money);

            //3. 响应
            if(flag){
                response.getWriter().write("转账成功");
            }else{
                response.getWriter().write("转账失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙");
        }

    }
}

```

- service

```java
package com.itheima.service;

import com.itheima.dao.TransformDao;

public class TransformService {
    /**
     * 转账业务处理
     * @param from
     * @param to
     * @param money
     * @return
     */
    public boolean transform(String from, String to, Double money) throws Exception {
        TransformDao transformDao = new TransformDao();

        //1. 付款方减少金额
        int fromCnt = transformDao.reduce(from,money);

        //int i =1/0;
        //2. 收款方增加金额
        int toCnt = transformDao.add(to,money);

        // 3.返回结果布尔值
        /*if(fromCnt>0 && toCnt>0){
            return true;
        }else{
            return false;
        }*/
        return fromCnt>0 && toCnt>0;
    }
}

```



- dao层

```java
package com.itheima.dao;

import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;

public class TransformDao {
    /**
     * 付款方减少钱
     * @param from
     * @param money
     * @return
     */
    public int reduce(String from, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());

        String sql = "update account set money = money - ? where name = ?";
        int update = queryRunner.update(sql, money, from);
        return update;
    }

    /**
     * 收款方增加钱
     * @param to
     * @param money
     * @return
     */
    public int add(String to, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());

        String sql = "update account set money = money + ? where name = ?";
        int update = queryRunner.update(sql, money, to);
        return update;
    }
}

```



### 四.小结

1. 转账: 一个用户的钱减少, 一个用户的钱增加



## 案例-转账的案例v2-三层架构_事务实现

### 一.需求 ###

- 当单击提交按钮，付款方向收款方安照输入的金额转账。 使用==手动事务==进行控制

  


![img](img/tu_1-1574219049777.png)

### 二,分析

#### 0 事务API回顾

事务API基于我们的连接对象Connection

- 手动控制事务开启： setAutoCommit(false)
- 提交： commit()
- 回滚： rollback()

-------------------------------------------------

转账业务：付款方减少钱、收款方增加钱

- 思考：事务是connection的API，那么如何达到事务控制的效果？



#### 1.DBUtils实现事务管理的新API

| API                                                          | 说明                                                |
| ------------------------------------------------------------ | --------------------------------------------------- |
| QueryRunner()                                                | ==空构造器==创建QueryRunner对象. 手动提交事务时使用 |
| query(==Connection conn==, String sql, ResultSetHandler<T> rsh, Object... params) | 传入连接对象conn作为参数，表示基于当前连接DQL       |
| update(==Connection conn==，String sql, Object... params)    | 传入连接对象conn作为参数，表示基于当前连接DML       |

#### 2.思路

 ![1599809215453](img/1599809215453.png)

### 三,实现

+ web层

```java
package com.itheima.web;

import com.itheima.service.TransformService;
import com.itheima.util.C3P0Utils;
import com.mysql.fabric.xmlrpc.base.Params;
import org.apache.commons.dbutils.QueryRunner;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.sql.SQLException;

@WebServlet("/transformServlet")
public class TransformServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        //1. 获得参数
        try {
            String from = request.getParameter("from");
            String to = request.getParameter("to");
            Double money = Double.valueOf(request.getParameter("money"));

            //2. 调用业务层-转账操作
            TransformService transformService = new TransformService();
            boolean flag = transformService.transform(from,to,money);

            //3. 响应
            if(flag){
                response.getWriter().write("转账成功");
            }else{
                response.getWriter().write("转账失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙");
        }

    }
}

```

- service层

```java
package com.itheima.service;

import com.itheima.dao.TransformDao;
import com.itheima.util.C3P0Utils;

import java.sql.Connection;

public class TransformService {
    /**
     * 转账业务处理
     * @param from
     * @param to
     * @param money
     * @return
     */
    public boolean transform(String from, String to, Double money) throws Exception {
        Connection conn = null;
        try {
            //1. 获取连接、开启事务
            conn = C3P0Utils.getConnection();
            conn.setAutoCommit(false);

            //2. 转账
            TransformDao transformDao = new TransformDao();
            // 付款方减少金额
            int fromCnt = transformDao.reduce(conn, from, money);

            //int i = 1/0;

            // 收款方增加金额
            int toCnt = transformDao.add(conn, to, money);

            // 3.判断结果
            if (fromCnt > 0 && toCnt > 0) {
                // 成功，提交事务
                conn.commit();
                return true;
            }else{
                conn.rollback();

            }
        } catch (Exception e) {
            e.printStackTrace();
            // 回滚
            conn.rollback();
            return false;
        }finally {
            // 归还连接
            conn.close();
        }

        return false;
    }
}

```



- dao层

```java
package com.itheima.dao;

import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;

import java.sql.Connection;

public class TransformDao {
    /**
     * 付款方减少钱
     *
     * @param conn
     * @param from
     * @param money
     * @return
     */
    public int reduce(Connection conn, String from, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner();

        String sql = "update account set money = money - ? where name = ?";
        int update = queryRunner.update(conn, sql, money, from);
        return update;
    }

    /**
     * 收款方增加钱
     *
     * @param conn
     * @param to
     * @param money
     * @return
     */
    public int add(Connection conn, String to, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner();

        String sql = "update account set money = money + ? where name = ?";
        int update = queryRunner.update(conn,sql, money, to);
        return update;
    }
}

```



### 四.小结

1. 事务版本
   1. 控制连接是同一个对象来达到事务控制（方法传参做到的）
   2. dao层的方法接收了业务层传过去的连接对象。





## 案例-转账的案例v3-ThreadLocal优化案例

### 一.需求 ###

- 当单击提交按钮，付款方向收款方按照输入的金额转账。 使用事务进行控制

  


![img](img/tu_1-1574219049777.png)

### 二,分析

#### 1.ThreadLocal 介绍

- ThreadLocal 介绍
- **ThreadLocal的API**
- set： 可以把数据存储到当前线程；存储的是key（ThreadLocal）-value（共享的数据）
  - get：从当前线程获取数据
  - remove：移除数据
  - ==注意事项==： 用哪个ThreadLocal 存的，那么就用哪个ThreadLocal取，因为key是ThreadLocal对象。
- 自定义简化版本的：

```java
package com.itheima.threadlocal;

import java.util.HashMap;
import java.util.Map;

public class MyThreadLocal {
    private static Map<Thread, Object> map = new HashMap<>();

    // 存储是数据
    public static void set(Object object){
        map.put(Thread.currentThread(), object);
    }

    public static  Object get(){
        return  map.get(Thread.currentThread());
    }

    public static void main(String[] args) {
        MyThreadLocal.set("111");
        System.out.println(MyThreadLocal.get());
    }
}

```



#### 2.ThreadLocal优化事务案例思路

![1599813633088](img/1599813633088.png)

#### 3. 工具类的抽取

按分析封装一个工具类：

- ConnectionManagerUtil

```java
package com.itheima.util;

import java.sql.Connection;

public class ConnectionManagerUtil {
    private static ThreadLocal<Connection> th = new ThreadLocal<>();


    // 存储连接对象到当前线程
    public static void setConn(Connection conn){
        th.set(conn);
    }

    // 从当前线程获取连接对象
    public static Connection getConn(){
        return th.get();
    }

    // 从当前线程移除连接对象
    public static void removeConn(){
        th.remove();
    }
}

```



### 三. 实现

+ service层

```java
package com.itheima.service;

import com.itheima.dao.TransformDao;
import com.itheima.util.C3P0Utils;
import com.itheima.util.ConnectionManagerUtil;

import java.sql.Connection;

public class TransformService {
    /**
     * 转账业务处理
     * @param from
     * @param to
     * @param money
     * @return
     */
    public boolean transform(String from, String to, Double money) throws Exception {
        Connection conn = null;
        try {
            //1. 获取连接、开启事务
            conn = C3P0Utils.getConnection();
            conn.setAutoCommit(false);


            // 把连接存储到当前线程里面
            ConnectionManagerUtil.setConn(conn);

            //2. 转账
            TransformDao transformDao = new TransformDao();
            // 付款方减少金额
            int fromCnt = transformDao.reduce(from, money);

            //int i = 1/0;

            // 收款方增加金额
            int toCnt = transformDao.add(to, money);

            // 3.判断结果
            if (fromCnt > 0 && toCnt > 0) {
                // 成功，提交事务
                conn.commit();
                return true;
            }else{
                conn.rollback();

            }
        } catch (Exception e) {
            e.printStackTrace();
            // 回滚
            conn.rollback();
            return false;
        }finally {
            // 归还连接
            ConnectionManagerUtil.removeConn();
            conn.close();
        }

        return false;
    }
}

```

+ dao层

```java
package com.itheima.dao;

import com.itheima.util.C3P0Utils;
import com.itheima.util.ConnectionManagerUtil;
import org.apache.commons.dbutils.QueryRunner;

import java.sql.Connection;

public class TransformDao {
    /**
     * 付款方减少钱
     *
     *
     * @param from
     * @param money
     * @return
     */
    public int reduce(String from, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner();

        String sql = "update account set money = money - ? where name = ?";
        Connection conn = ConnectionManagerUtil.getConn();
        int update = queryRunner.update(conn, sql, money, from);
        return update;
    }

    /**
     * 收款方增加钱
     *
     *
     * @param to
     * @param money
     * @return
     */
    public int add(String to, Double money) throws Exception{
        QueryRunner queryRunner = new QueryRunner();

        String sql = "update account set money = money + ? where name = ?";
        Connection conn = ConnectionManagerUtil.getConn();
        int update = queryRunner.update(conn,sql, money, to);
        return update;
    }
}

```



### 四.小结

1. Threadlocal  类一些API
   1. set： 把数据存放到当前线程中
   2. get： 从当前线程中获取set方法设置的值
   3. remove：从当前线程中移除set方法设置的值
2. 在service中存储连接到当前线程中
3. 在DAO中从当前线程中获取连接



## 补充_路径问题

### 1.绝对的路径

+ 以http开始的
+ 如果是同一个服务器同一个项目里面的,以 /项目的部署路径

### 2.相对路径

+ 以当前的资源为参考, 相对当前资源的
  +  ==不以/开头== 
  + 以./(可以省略) 或者  ../

```
http://localhost:8080/day25jsp/index.jsp
http://localhost:8080/day25jsp/demo04

index.jsp的层级和dmeo04层级一样的 就直接写demo04或者./demo04


http://localhost:8080/day25jsp/aaaaaa/a.jsp
http://localhost:8080/day25jsp/demo04

demo04的层级在a.jsp的上一层. 所以在a.jsp里面通过 ../demo04
```



# 回顾要点

- EL表达式： key就是域对象.setAttribute(key,value)中的key

  - 作用： 获取域对象存储的数据， ${}
  - 获取简单类型： ${key}
  - 获取数组: ${key[下标]}
  - 获取list： ${key.get(索引)}、${key[下标]}
  - 获取map： ${key.get("键")}、${key.键名}
  - 获取javabean的属性： ${key.javabean的属性}

- jstl标签

  - 步骤

    - 引入jar包
    - 在你的jsp中引入标签库（抄）

  - if

    - ```jsp
      <c:if test="${}">
          test属性值为true是会显示
      </c:if>
      ```

  - forEach

    - 简单

      - ```jsp
        <c:forEach begin="数值形式" end="数值型是" var="a">
            获取每次遍历的元素值： ${a}
        </c:forEach>
        ```

        

    - 复杂

      - ```jsp
        <c:forEach items="${可迭代的对象}" var="a">
            获取每次遍历的元素值： ${a}
        </c:forEach>
        ```

- 三层架构

  - 一种理念
  - 落地实施：包来区分
    - WEB层： com.itheima.web
    - 业务层： com.itheima.service
    - dao层：com.itheima.dao
  - WEB层：
    - 获取参数
    - 调用业务层
    - 响应
  - 业务层
    - 处理具体的业务
    - 调用DAO层
  - DAO层
    - 和数据库打交道，进行CRUD

- 案例必须做

  



# 预习要点

filter

JDK8

- 安装多个JDK
  - 修改JAVA_HOME指向你想要用的jdk的目录
  - ![1597308269776](img/1597308269776.png)





​		
