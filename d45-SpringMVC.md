---
typora-copy-images-to: img
---

# day45-SpringMVC

# 学习目标

- [ ] 了解SpringMVC框架
- [ ] 能够实现SpringMVC的环境搭建
- [ ] 掌握@RequestMapping的使用
- [ ] 掌握SpringMVC的参数绑定
- [ ] 掌握SpringMVC的自定义类型转换器的使用
- [ ] 掌握SpringMVC的常用注解
- [ ] 掌握PathVariable注解

- [ ] 掌握Controller的返回值使用
- [ ] 掌握Controller中的转发和重定向使用
- [ ] 掌握SpringMVC与json交互

# 第一章-SpringMVC介绍

## 知识点-SpringMVC概述

### 1.目标

- [ ] 介绍SpringMVC框架

### 2.路径

1. 三层架构(SpringMVC在三层架构的位置)
2. 什么是SpringMVC

3. SpringMVC 的优点

### 3.讲解

#### 3.1三层架构

![img](img/tu_2.png)



​	咱们开发服务器端程序，一般都基于两种形式，一种C/S架构程序，一种B/S架构程序. 使用Java语言基本上都是开发B/S架构的程序，B/S架构又分成了三层架构.

- 三层架构

  ​	表现层：WEB层，用来和客户端进行数据交互的。表现层一般会采用MVC的设计模型

  ​	业务层：处理公司具体的业务逻辑的

  ​	持久层：用来操作数据库的

- MVC全名是Model View Controller 模型视图控制器，每个部分各司其职。

  ​	Model：数据模型，JavaBean的类，用来进行数据封装。   

  ​	View：指JSP、HTML用来展示数据给用户				   

  ​	Controller：用来接收用户的请求，整个流程的控制器。用来进行数据校验等

#### 3.2什么是SpringMVC

![img](img/tu_1.png)

- 用我们自己的话来说:SpringMVC 是一种基于Java实现的MVC设计模型的请求驱动类型的轻量级WEB层框架。

- 作用:  
  
  1. 参数绑定(获得请求参数)
  2. 调用业务
  3. 响应
  
  

#### 3.3SpringMVC 的优点(现阶段不用看)

1.清晰的角色划分：
​	前端控制器（DispatcherServlet）  
​	请求到处理器映射（HandlerMapping）
​	处理器适配器（HandlerAdapter）
​	视图解析器（ViewResolver）
​	处理器或页面控制器（Controller）
​	验证器（ Validator）
​	命令对象（Command 请求参数绑定到的对象就叫命令对象）
​	表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）。
2、分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。
3、由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。
4、和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。
5、可适配，通过 HandlerAdapter 可以支持任意的类作为处理器。
6、可定制性， HandlerMapping、 ViewResolver 等能够非常简单的定制。
7、功能强大的数据验证、格式化、绑定机制。
8、利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试。
9、本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。
10、强大的 JSP 标签库，使 JSP 编写更容易。
………………还有比如RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配
置支持等等。 

### 4.小结

1. SpringMVC认知：
   1. 就是一个web层框架，就是对技术的封装servlet、listener、filter...
   2. 作用：
      1. 获得参数-参数绑定
      2. 做出响应



# 第二章-SpringMVC入门

## 案例-SpringMVC的快速入门

### 1.需求

​		浏览器请求服务器(SpringMVC), 响应成功页面

### 2.分析

1. 创建Maven工程(==war==),导入坐标mvc、webmvc
2. 配置文件springmvc.xml
   1. 开启IOC注解包扫描
   2. 配置视图解析器bean
3. 写一个类，加一个注解 @Controller
4. 在Controller类里面写一个方法，使用@RequestMapping注解
5. ==在web.xml配置servlet：DispatcherServlet==

### 3.实现

 ![1536046221380](img/1536046221380.png)

#### 3.1 创建web项目,引入坐标

![img](img/tu_3.png)

```xml

	<!-- 版本锁定 -->
	<properties>
		<spring.version>5.0.2.RELEASE</spring.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.0</version>
			<scope>provided</scope>
		</dependency>
	</dependencies>
```

#### 3.2编写Controller

```java
package com.itheima.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {

    @RequestMapping("/hello")
    public String sayHello(){
        System.out.println("sayHello");
        return "success";
    }
}

```

#### 3.3编写SpringMVC的配置文件

+ 在classpath目录下创建springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启IOC的注解包扫描-->
    <context:component-scan base-package="com.itheima"/>

    <!--配置内置的视图解析器（解析jsp）-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置视图前缀：  会去/WEB-INF/pages/找资源，找后缀为.jsp的资源-->
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

#### 3.4在web.xml里面配置核心控制器 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         version="2.5">


   <servlet>
     <servlet-name>DispatcherServlet</servlet-name>
     <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

     <!--初始化参数contextConfigLocation指定容器启动时会去加载配置文件-->
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
      </init-param>
     <load-on-startup>1</load-on-startup>
   </servlet>
  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <!-- / 拦截所有，但是忽略jsp
        /*  拦截所有
        *.do  拦截.do结尾的请求
        /demo01 完全路径匹配
    -->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

#### 3.5测试

```
<a href="${pageContext.request.contextPath}/hello">访问hello</a>
```

### 4.小结

1. 引入依赖web、webmvc
2. 需要写配置文件springmvc.xml
   1. 开启注解包扫描
   2. 需要配置视图解析器
3. 需要写类加注解@Controller、方法加注解@RequestMapping("/路径")
4. 在webx.xml中配置总控制器DispatcherServlet
   1. 配置初始化参数：contextConfigLocation指定你的配置文件
   2. 加启动项：`<load-on-startup>1</load-on-startup>`



## 知识点-入门案例的执行过程及原理分析

### 1.目标

- [ ] 掌握入门案例的执行过程

### 2.路径

1. 入门案例加载流程
2. SpringMVC 的请求响应流程 
3. 入门案例中涉及的组件 [了解]

### 3.讲解 

#### 3.1入门案例加载流程

1、服务器启动，应用被加载。 读取到 web.xml 中的配置创建 spring 容器并且初始化容器中的对象。 
2、浏览器发送请求，被 DispatherServlet 捕获，该 Servlet 并不处理请求，而是把请求转发出去。转发的路径是根据请求 	URL，匹配@RequestMapping 中的内容。
3、匹配到了后，执行对应方法。该方法有一个返回值。
4、根据方法的返回值，借助 InternalResourceViewResolver 找到对应的结果视图。
5、渲染结果视图，响应浏览器 

#### 3.2SpringMVC 的请求响应流程 

![img](img/tu_7.jpg)

#### 3.3入门案例中涉及的组件

+ DispatcherServlet：总控制器；前端控制器 

  ​	用户请求到达前端控制器，它就相当于 mvc 模式中的 c， dispatcherServlet 是整个流程控制的中心，由
  它调用其它组件处理用户的请求， dispatcherServlet 的存在降低了组件之间的耦合性。 

+ HandlerMapping：处理器映射器    

  ​	HandlerMapping 负责根据用户请求找到 Handler 即处理器， SpringMVC 提供了不同的映射器实现不同的
  映射方式，例如：配置文件方式，实现接口方式，注解方式等。 

+ Handler：处理器 (自己写的Controller类)

  ​	它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由
  Handler 对具体的用户请求进行处理。 

+ HandlAdapter：处理器适配器 

  ​	通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理
  器进行执行。 

  ![img](img/tu_5.png)

+ View Resolver：视图解析器  

  ​	View Resolver 负责将处理结果生成 View 视图， View Resolver 首先根据逻辑视图名解析成物理视图名
  即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。 

+ View：视图 

  ​	SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jsp ,  jstlView、 freemarkerView、 pdfView等。我们最常用的视图就是 jsp。
  ​	一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。 

==`<mvc:annotation-driven>`== 配置说明 :

​	在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件。使 用 	`<mvc:annotation-driven>`  自 动 加 载 RequestMappingHandlerMapping （ 处 理 映 射 器 ）RequestMappingHandlerAdapter （ 处 理 适 配 器 ） ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用
  `<mvc:annotation-driven/>` 替代注解处理器和适配器的配置(默认情况下不配置也是可以使用的)。

----------------------

--------------------

![1595808486887](img/1595808486887.png)

### 4.小结

1. 项目启动时，会加载总控制器DispatcherServlet，配置了初始化参数（指定了spring配置），会初始化spring容器以及相关组件（自己写的控制器类controller、视图解析器、处理器映射器...）
2. 请求来了后，DispatcherServlet接收请求，然后去找对应的处理器（自己写的控制器类controller的方法）--反射调用方法--得到逻辑视图---》视图解析器加工得到物理视图---渲染--用户



## 知识点-RequestMapping注解详解

### 1.目标

- [ ] 掌握RequestMapping的使用

### 2.路径

1. 介绍RequestMapping作用
2. RequestMapping的使用的位置
3. RequestMapping的属性

### 3.讲解

#### 3.1RequestMapping作用

​	RequestMapping注解的作用是建立==请求URL==和==处理方法==之间的对应关系

​	RequestMapping注解可以作用在**方法**和**类**上

#### 3.2RequestMapping的使用的位置

+ 使用在类上:

  ​	 请求 URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。 写的话需要以/开头 .它出现的目的是为了使我们的 URL 可以按照模块化管理 

+ 使用在方法上:

  ​	请求 URL 的第二级访问目录 

```java
@Controller
@RequestMapping("/account")
public class AccountController {
    @RequestMapping("/add")
    public String add(){
        System.out.println("添加账户");
        return "success";
    }
    @RequestMapping("/delete")
    public String deletet(){
        System.out.println("删除账户");
        return "success";
    }
    @RequestMapping("/update")
    public String update(){
        System.out.println("更新账户");
        return "success";
    }
}

<h3>绝对路径的写法</h3>
<a href="${pageContext.request.contextPath }/account/add">添加账户</a><br/>
<a href="${pageContext.request.contextPath }/account/delete">删除账户</a><br/>
<a href="${pageContext.request.contextPath }/account/update">更新账户</a><br/>
<hr/>
<h3>相对路径的写法</h3>
<a href="account/add">添加账户</a><br/>
<a href="account/delete">删除账户</a><br/>
<a href="account/update">更新账户</a><br/>
```

#### 3.3RequestMapping的属性

##### 3.3.1掌握的属性

+ path:	 指定请求路径的url

```java
@RequestMapping(path = "/findByName")
public String findByName(){
    System.out.println("findByName账户");
    return "success";
}
```

+ value:       value属性和path属性是一样的

```java
@RequestMapping(value = "/findByid")
public String findByid(){
    System.out.println("findByid账户");
    return "success";
}
```

+ method :    指定该方法的请求方式

```java
// 默认情况就是任意类型都可以访问； 限制访问方式
// 意味着只能使用post，如果是get则报错：405 – Method Not Allowed
@RequestMapping(path = "/findByPassword", method = RequestMethod.POST)
public String findByPassword(){
    System.out.println("findByPassword账户");
    return "success";
}

<a href="${pageContext.request.contextPath}/account/findByPassword">访问findByPassword账户(post才行)</a><br>
```

##### 3.3.2了解的属性

+ params:   指定限制请求参数的条件

```java
// 了解的属性----params
// 意味着必须有请求参数"name=zs"，否则不行了
@RequestMapping(path = "/findAll", params = {"name=zs"})
public String findAll(){
    System.out.println("findfindAll账户");
    return "success";
}

<a href="${pageContext.request.contextPath}/account/findAll?name=zs">访问findAll账户(params)</a><br>


```

+ headers:  用于指定发送的请求中必须得包含的请求头

```java
// headers
// 必须包含请求头，包含指定的值
// 可能遇到错误：415 – Unsupported Media Type
@RequestMapping(path = "/findNickname", headers = {"content-type=text/*"} )
public String findNickname(){
    System.out.println("findNickname账户");
    return "success";
}

<a href="${pageContext.request.contextPath}/account/findNickname">访问findNickname账户(headers)</a><br>

```

### 4.小结

1. RequetMapping
   1. 可以用在类、方法中
   2. 掌握的属性： value、path、method
   3. 了解的属性： params、headers



# 第三章-SpringMVC进阶

## 知识点-请求参数的绑定【重点】

### 1.目标

- [ ] 掌握SpringMVC的参数绑定

### 2.分析

+ 绑定机制

  ​	表单提交的数据都是key=value格式的(username=zs&password=123),SpringMVC的==参数绑定==过程是把表单提交的请求参数，作为控制器中方法的参数进行绑定的(==要求：提交表单的name和参数的名称是相同的==)

  

  ```
  页面：username=zs&password=123
  
  方法： hello(String username, String  password)
  ```

  

  

+ 支持的数据类型

  ​	基本数据类型和字符串类型

  ​	实体类型（JavaBean）

  ​	集合数据类型（List、map集合等）

+ 使用要求

  + 如果是基本类型或者 String 类型： 要求我们的参数名称必须和控制器中方法的形参名称保持一致。 (严格区分大小写) .
  + 如果是 POJO 类型，或者它的关联对象： 要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型 .
  + 如果是集合类型,有两种方式： 
    + 第一种：要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。给 List 集合中的元素赋值， 使用下标。给 Map 集合中的元素赋值， 使用键值对。
    + 第二种：接收的请求参数是 json 格式数据。需要借助一个注解实现 

### 3.讲解

#### 3.1基本类型和 String 类型作为参数

==页面的name属性值和方法的参数名一致==

+ 前端页面

```jsp
<h3>一,简单类型</h3>
<form  action="account/bindSimpleType" method="post">
    账户名称： <input type="text" name="name" ><br/>
    账户金额： <input type="text" name="money" ><br/>

    <input type="submit" value="保存">
</form>
```

+ AccountController

```java
@Controller
@RequestMapping("/account")
public class AccountController {
    // 参数绑定--简单类型
    @RequestMapping("/bindSimpleType")
    public String bindSimpleType(String name, Double money){
        System.out.println("bindSimpleType账户："+ name+","+money);
        return "success";
    }
}
```

#### 3.2POJO 类型作为参数

+ pojo(Account和Address)

==页面的name值和pojo参数的属性名对应==

```java
public class Address implements Serializable{

    private String provinceName;
    private String cityName;
    
	//省略了set/get...
}

public class Account implements Serializable {

    private Integer id;
    private String name;
    private double money;

    private Address address;
    
	//省略了set/get...
}
```

+ 前端页面

```html
<h3>二,pojo类型</h3>
<form  action="account/bindPojo" method="post">
    账户名称： <input type="text" name="name" ><br/>
    账户金额： <input type="text" name="money" ><br/>
    账户省份： <input type="text" name="address.provinceName" ><br/>
    账户城市： <input type="text" name="address.cityName" ><br/>
    <input type="submit" value="保存">
</form>
```

+ AccountController.java

```java
@RequestMapping("/bindPojo")
public String bindPojo(Account account){
    System.out.println("bindPojo账户："+ account);
    return "success";
}
```

 

#### 3.3POJO 类中包含集合类型参数

##### 3.3.1 POJO 类中包含List

==页面的元素name值写pojo的属性[索引下标]==

+ User

```java
public class User implements Serializable {
    private String username;
    private String password;
    private Integer age;
    private List<Account> accounts;
 	//省略了set/get... 	 
 }
```

+ 前端页面

```html
<h3>1.POJO 类中包含List</h3>
<form action="user/bindPojoList" method="post">
    用户名称： <input type="text" name="username" ><br/>
    用户密码： <input type="password" name="password" ><br/>
    用户年龄： <input type="text" name="age" ><br/>
    账户 1 名称： <input type="text" name="accounts[0].name" ><br/>
    账户 1 金额： <input type="text" name="accounts[0].money"><br/>
    账户 2 名称： <input type="text" name="accounts[1].name"><br/>
    账户 2 金额： <input type="text" name="accounts[1].money" ><br/>
    <input type="submit" value="保存">
</form>
```

+ UserController.java

```java

@Controller
@RequestMapping("/user")
public class UserController {

    @RequestMapping("/bindPojoList")
    public String bindPojoList(User user){
        System.out.println("bindPojoList:"+user);
        return "success";
    }
}
```

##### 3.3.2 POJO 类中包含Map

==页面的元素的name值写pojo的属性['key名']==

+ User.java  添加map类型属性测试

```java
public class User {
    private String username;
    private String password;
    private Integer age;
    private List<Account> accounts;
	// 添加map类型属性
    private Map<String,Account> accountMap;
}
```

+ 页面：==accountMap的内容必须得输入值==

```html
<h3>1.POJO 类中包含Map</h3>
<form action="user/bindPojoMap" method="post">
    用户名称： <input type="text" name="username" ><br/>
    用户密码： <input type="password" name="password" ><br/>
    用户年龄： <input type="text" name="age" ><br/>
    账户 1 名称： <input type="text" name="accountMap['akey'].name" ><br/>
    账户 1 金额： <input type="text" name="accountMap['akey'].money"><br/>
    账户 2 名称： <input type="text" name="accountMap['bkey'].name"><br/>
    账户 2 金额： <input type="text" name="accountMap['bkey'].money" ><br/>
    <input type="submit" value="保存">
</form>
```

+ UserController.java

```java
@RequestMapping("/bindPojoMap")
    public String bindPojoMap(User user){
        System.out.println("bindPojoMap:"+user);
        return "success";
    }
```

### 4.小结

1. 参数绑定
   1. 简单类型： 保证页面参数名和方法参数名一致即可
   2. pojo：保证页面参数名和pojo类的属性名一致即可
   3. pojo里面包含list：页面中这么写： list属性名[下标].属性
   4. pojo里面包含map：页面中这么写： map属性名['key'].属性



## 知识点-请求参数细节和特殊情况

### 1.目标

- [ ] 掌握乱码处理和自定义类型转换器

### 2.路径

1. 请求参数乱码处理
2. 自定义类型转换器
3. 使用 ServletAPI 对象作为方法参数

### 3.讲解

#### 3.1请求参数乱码处理

+ 在web.xml里面配置编码过滤器 

```xml
<!--配置处理乱码的过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--指定编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

#### 3.2自定义类型转换器 

​	默认情况下,SpringMVC已经实现一些数据类型自动转换。 内置转换器全都在：		`org.springframework.core.convert.support ` 包下 ,如遇特殊类型转换要求，需要我们自己编写自定义类型转换器。 

```java
java.lang.Boolean -> java.lang.String : ObjectToStringConverter
java.lang.Character -> java.lang.Number : CharacterToNumberFactory
java.lang.Character -> java.lang.String : ObjectToStringConverter
java.lang.Enum -> java.lang.String : EnumToStringConverter
java.lang.Number -> java.lang.Character : NumberToCharacterConverter
java.lang.Number -> java.lang.Number : NumberToNumberConverterFactory
java.lang.Number -> java.lang.String : ObjectToStringConverter
java.lang.String -> java.lang.Boolean : StringToBooleanConverter
java.lang.String -> java.lang.Character : StringToCharacterConverter
java.lang.String -> java.lang.Enum : StringToEnumConverterFactory
java.lang.String -> java.lang.Number : StringToNumberConverterFactory
java.lang.String -> java.util.Locale : StringToLocaleConverter
java.lang.String -> java.util.Properties : StringToPropertiesConverter
java.lang.String -> java.util.UUID : StringToUUIDConverter
java.util.Locale -> java.lang.String : ObjectToStringConverter
java.util.Properties -> java.lang.String : PropertiesToStringConverter
java.util.UUID -> java.lang.String : ObjectToStringConverter
 ....
```

##### 3.2.1场景 

+ 页面

```html
<a href="${pageContext.request.contextPath}/user/dateType?date=1999-09-09">访问dateType</a><br>

```

+ UserController.java

```java
@RequestMapping("/dateType")
    public String dateType(Date date){
        System.out.println("dateType:"+date);
        return "success";
    }
```

+ 报错了

##### 3.2.2自定义类型转换器 

步骤:

1. 创建一个类实现Converter 接口
2. 配置类型转换器，自定义的类型转换器需要作为`ConversionServiceFactoryBean`的属性注入
3.  ==mvc:annotation-driven== 标签中引用配置的类型转换服务

实现:

+ 定义一个类，实现 Converter 接口

  该接口有两个泛型,,S:表示接受的类型， T：表示目标类型(需要转的类型)

```java
package com.itheima;

import org.springframework.core.convert.converter.Converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

// 实现Converter接口重写convert方法
// 泛型一：源类型； 泛型二：目标类性
public class StringToDateConverter implements Converter<String, Date> {
    @Override
    public Date convert(String source) {
        try {
            SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
            return dateFormat.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}

```

+ 在springmvc.xml里面配置转换器

  spring 配置类型转换器的机制是，将自定义的转换器注册到类型转换服务中去 

```xml
<!--配置类型转换器: 把自定义的转换器注册成为ConversionServiceFactoryBean的属性-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.itheima.StringToDateConverter"/>
            </set>
        </property>
    </bean>
```

+ 在 annotation-driven 标签中引用配置的类型转换服务 

```xml
<!--开启mvc注解驱动；引入配置好的转换器-->
<mvc:annotation-driven conversion-service="conversionService"/>
```

#### 3.3使用 ServletAPI 对象作为方法参数

​	SpringMVC 还支持使用原始 ServletAPI 对象作为控制器方法的参数。我们可以把它们==直接写在控制的方法参数中使用==。 支持原始 ServletAPI 对象有 : 

​	**HttpServletRequest**

​	**HttpServletResponse**

​	**HttpSession**

​	java.security.Principal

​	Locale

​	InputStream

​	OutputStream

​	Reader

​	Writer

+ 页面

```html
<a href="user/testServletAPI?name=zs">使用 ServletAPI 对象作为方法参数</a>
```

+ UserController.java

```java
/**
     * 可以使用servlet API的对象作为方法的参数
     * @param name
     * @param request
     * @param response
     * @param session
     * @return
     */
    @RequestMapping("/testServletAPI")
    public String testServletAPI(String name,
                                 HttpServletRequest request,
                                 HttpServletResponse response,
                                 HttpSession session){
        System.out.println(name+","+request+"," + response+","+session);
        return "success";
    }
```

### 4.小结

1. 中文乱码处理：配置过滤器
2. 自定义类型转换器（了解）
3. servlet的API对象可以直接作为方法的参数

## 知识点-常用注解 

### 1.目标

- [ ] 掌握常用注解 

### 2.路径

1. @RequestParam 
2. @RequestBody
3. @PathVariable  
4. @RequestHeader【了解】
5. @CookieValue【了解】



### 3.讲解

#### 3.1 RequestParam

##### 3.1.1使用说明

+ 作用：

  （页面参数名和方法参数名不一致也可以通过这个注解绑定） 

+ 属性

  value： 请求参数中的名称。
  required：请求参数中是否必须提供此参数。 默认值： true。表示必须提供，如果不提供将报错。 

  defaultValue:默认值

##### 3.1.2使用实例

+ 页面

```html
<a href="user/testRequestParam?name=张三">测试RequestParam </a><br>
```

+ UserController.java

```java
/*注解*/
// 可以把请求参数名为name的值给到username
@RequestMapping("/testRequestParam")
public String testRequestParam(@RequestParam("name") String username){
    System.out.println("testRequestParam:"+username);
    return "success";
}
```

#### 3.2.RequestBody

请求体: post方式的请求参数,get方式没有请求体

Get和Post区别

1. get方式 请求参数拼接在请求路径后面, post 方式 请求参数在请求体里面
2. get方式 请求参数浏览器地址栏可见,  post 方式 请求参数浏览器地址栏不可见
3. get方式 请求参数大小有限制的, post 方式请求参数大小没有限制的

##### 3.2.1使用说明

+ 作用

  1.用于获取请求体内容。 直接使用得到是 key=value&key=value...结构的字符串。

  2.把获得json类型的数据转成pojo对象(后面再讲)

  注意: get 请求方式不适用。 

+ 属性

  required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值为 false， get 请求得到是 null。 

##### 3.2.2使用实例

+ 页面

```html
<form  action="user/testRequestBody " method="post">
    用户名:<input type="text" name="username"/><br/>
    密码:<input type="password" name="password"/><br/>
    <input type="submit" value="测试RequestBody"/>
</form>
```

+ UserController.java

```java
// @RequestBody获得请求体内容（获得参数:结果形式为：username=zs&password=w35345）
@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String queryStr){
    try {
        System.out.println("queryStr:"+ URLDecoder.decode(queryStr, "UTF-8"));
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }
    return "success";
}
```



#### 3.3.PathVariable

##### 3.3.1REST 风格 URL

​	REST（英文： Representational State Transfer，简称 REST）描述了一个架构样式的网络系统，比如 web 应用程序。它首次出现在 2000 年 Roy Fielding 的博士论文中，他是 HTTP 规范的主要编写者之一。在目前主流的三种 Web 服务交互方案中， REST 相比于 SOAP（Simple Object Access protocol，简单对象访问协议）以及 XML-RPC 更加简单明了，无论是对 URL 的处理还是对 Payload 的编码， REST 都倾向于用更加简单轻量的方法设计和实现。值得注意的是 REST 并没有一个明确的标准，而更像是一种设计的风格。它本身并没有什么实用性，其核心价值在于如何设计出符合 REST 风格的网络接口。

+ restful 的优点

  它结构清晰、符合标准、易于理解、 扩展方便，所以正得到越来越多网站的采用。

+ restful 的特性：

  ​	==资源（Resources）== ： 网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。可以用一个 URI（统一资源定位符）指向它，每种资源对应一个特定的 URI 。要获取这个资源，访问它的 URI 就可以，因此 URI 即为每一个资源的独一无二的识别符。

  ​	表现层（Representation） ： 把资源具体呈现出来的形式，叫做它的表现层 （Representation）。比如，文本可以用 txt 格式表现，也可以用 HTML 格式、 XML 格式、 JSON 格式表现，甚至可以采用二进制格式。	状态转化（State Transfer） ： 每 发出一个请求，就代表了客户端和服务器的一次交互过程。HTTP 协议，是一个无状态协议，即所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段， 让服务器端发生“ 状态转化” （State Transfer）。而这种转化是建立在表现层之上的，所以就是 “ 表现层状态转化” 。

  ​	具体说，就是 HTTP 协议里面，四个表示操作方式的动词： GET 、 POST 、 PUT、DELETE。它们分别对应四种基本操作： GET 用来获取资源， POST 用来新建资源， PUT 用来更新资源， DELETE 用来删除资源 .

  

+ 实例      

```
保存
	传统：http://localhost:8080/user/save
	REST：http://localhost:8080/user						    POST方式	执行保存

更新
	传统：http://localhost:8080/user/update?id=1
	REST：http://localhost:8080/user/1					    PUT方式	执行更新   1代表id

删除	
	传统：http://localhost:8080/user/delete?id=1
	REST：http://localhost:8080/user/1				       DELETE方式	执行删除 1代表id  

查询
	传统：http://localhost:8080/user/findAll
	REST：http://localhost:8080/user						  GET方式	查所有

	传统：http://localhost:8080/user/findById?id=1
	REST：http://localhost:8080/user/1 					  GET方式	根据id查1个
```

##### 3.3.2使用说明

+ 作用：

  用于绑定 url 中的占位符。 例如：方法的请求 url 中 /delete/{id}， 这个{id}就是 url 占位符。
  url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。

+ 属性：

  value： 用于指定 url 中占位符名称。
  required：是否必须提供占位符。 

##### 3.3.3使用实例

+ 页面

```jsp
<a href="user/testPathVaribale/1">测试PathVaribale</a><br/>
```

+ UserController.java

```java
// @PathVariable 注解：获取rest风格的utl路径的变量值
// 方法中使用url占位符捕获这个请求路径中的变量值
// 测试： 请求地址： user/testPathVaribale/1
@RequestMapping("/testPathVaribale/{id}")
public String testPathVaribale(@PathVariable("id") Integer id){
    System.out.println(id);
    return "success";
}
```



#### 3.4.RequestHeader【了解】

##### 3.4.1使用说明

+ 作用：
  用于获取请求消息头。
+ 属性：
  value：提供消息头名称
  required：是否必须有此消息头 

##### 3.4.2使用实例

+ 页面

```jsp
<a href="user/testRequestHeader">测试RequestHeader</a><br/>
```

+ UserController.java

```java
// 获取请求头
@RequestMapping("/testRequestHeader")
public String testRequestHeader(@RequestHeader(value = "User-Agent") String userAgent){
    System.out.println("testRequestHeader:"+userAgent);
    return "success";
}
```



#### 3.5.CookieValue【了解】

##### 3.5.1使用说明

+ 作用：

  用于把指定 cookie 名称的值传入控制器方法参数。

+ 属性：

  value：指定 cookie 的名称。
  required：是否必须有此 cookie。 

##### 3.5.2使用实例

+ 页面

```jsp
<a href="user/testCookieValue">测试CookieValue</a><br/>
```

+ UserController.java

```java
// 获取cookie中的值
@RequestMapping("/testCookieValue")
public String testCookieValue(@CookieValue(value = "JSESSIONID") String cookieValue){
    System.out.println("testCookieValue:"+cookieValue);
    return "success";
}

```

#### 3.6.ModelAttribute 【课后自学-了解】

==提示：@ModelAttribute标记的方法优先于其他使用到该方法的对象执行==

##### 3.6.1使用说明

+ 作用：

  ​	该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。

  ​	出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。

  ​	出现在参数上，获取指定的数据给参数赋值。

+ 属性：
  value：用于获取数据的 key。 key 可以是 POJO 的属性名称，也可以是 map 结构的 key。

+ 应用场景：
  当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。
  例如：
  ​        我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数
  ​         据是肯定没有此字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题。 

##### 3.6.2使用实例

+ 页面

```html
<form action="user/testModelAttribute" method="post">
    用户名:<input type="text" name="username"/><br/>
    密码:<input type="text" name="password"/><br/>
    <input type="submit" value="testModelAttribute"/>
</form>
```

+ UserController.java(用在方法上面)

==该案例优先调用getModel方法，并且把方法的返回值给到testModelAttribute作为参数==

```java
    @RequestMapping("/testModelAttribute")
    public String  testModelAttribute(User user){
        System.out.println("testModelAttribute ...收到了请求..."+user);
        return  "success";
    }

    @ModelAttribute
    public User  getModel(String username,String password){
        System.out.println("getModel ...收到了请求...");
        //模拟查询数据库, 把性别也查询出来
        User user = new User();
        user.setUsername(username);
        user.setUsername(password);
        user.setSex("男");
        return  user;
    }
```

+ UserController.java(用在参数上面)

```java
    @RequestMapping("/testModelAttribute")
    public String  testModelAttribute(@ModelAttribute("u") User user){
        System.out.println("testModelAttribute ...收到了请求..."+user);
        return  "success";
    }

    @ModelAttribute
	//没有返回值
    public void  getModel(String username, String password, Map<String,User> map){
        System.out.println("getModel ...收到了请求...");
        //模拟查询数据库, 把性别也查询出来
        User user = new User();
        user.setUsername(username);
        user.setUsername(password);
        user.setSex("男");
        map.put("u",user);
    }
```

#### 3.7.SessionAttributes 【课后自学-了解】

##### 3.7.1使用说明

+ 作用：

  用于多次执行(多次请求)控制器方法间的参数共享。(该注解定义在类上)

+ 属性：
  value：用于指定存入的属性名称
  type：用于指定存入的数据类型。 

##### 3.7.2使用实例

+ 页面

```java
<a href="sessionController/setAttribute?name=张三&age=18">测试SessionAttributes(存)</a><br/>
<a href="sessionController/getAttribute">测试SessionAttribute(取)</a><br/>
<a href="sessionController/removeAttribute">测试SessionAttribute(清空)</a><br/>
```

+ SessionController.java

```java
@Controller
@RequestMapping("/sessionController")
@SessionAttributes(value = {"name","age"})
public class SessionController {

    @RequestMapping("/setAttribute")
    public String setAttribute(String name, int age, Model model){
        model.addAttribute("name",name);
        model.addAttribute("age",age);
        return  "success";
    }

    @RequestMapping("/getAttribute")
    public String getAttribute(ModelMap modelMap){
        System.out.println("name="+modelMap.get("name"));
        System.out.println("age="+modelMap.get("age"));
        return  "success";
    }

    @RequestMapping("/removeAttribute")
    public String removeAttribute(SessionStatus sessionStatus){
        sessionStatus.setComplete();
        return  "success";
    }

}
```

# 第四章-响应数据和结果视图

## 知识点-返回值分类 

### 1.目标

- [ ] 掌握Controller的返回值使用

### 2.路径

1. 字符串 
2. void【了解】
3. ModelAndView 

### 3.讲解

#### 3.1字符串 

controller 方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。

+ 页面

```
<a href="response/testReturnString">返回String</a><br>
```

+ Controller

```java
@Controller
@RequestMapping("/response")
public class ResponseController {
  
  	//指定逻辑视图名，经过视图解析器解析为 jsp 物理路径： /WEB-INF/pages/success.jsp
    @RequestMapping("/testReturnString")
    public String testReturnString(){
        System.out.println("testReturnString");
        return "success";
    }
}
```

#### 3.2void【了解】

控制器方法返回值是void时:

​	如果控制器方法的参数中没有response对象:

​		它的视图是以方法是RequestMapping的取值作为视图名称

​		如果类上也有RequestMapping，则类上的RequestMapping是其中的1级路径

​	

+ 页面

```html
<a href="response/testReturnVoid">void类型返回值</a><br>
```

+ Controller.java(没有response对象情况)： 会去找/WEB-INF/pages/response/testReturnVoid.jsp

```java
@Controller
@RequestMapping("/response")
public class ResponseController {

     // 方法返回类型是void现象：
    // /day47_springmvc_01/WEB-INF/pages/response/testReturnVoid.jsp
    @RequestMapping("/testReturnVoid")
    public void testReturnVoid(){
        System.out.println("testReturnVoid");
    }
}
```

+ Controller.java(有response对象情况)

```java
    // 方法返回类型是void现象：
    // 方法有response作为参数，没有视图（既然用到了resposne，让开发人员自己决定是否跳转页面）
    @RequestMapping("/testReturnVoidWithResponse")
    public void testReturnVoidWithResponse(HttpServletResponse response){
        System.out.println("testReturnVoidWithResponse");

    }

```

#### 3.3ModelAndView  

ModelAndView 是 SpringMVC 为我们提供的一个对象，该对象也可以用作控制器方法的返回值。

可以绑定数据到request域对象中。 

+ 页面

```html
<a href="response/testReturnModelAndView">ModelAndView类型返回值</a><br>
```

+ Controller

```java
// ModelAndView可以设置域对象的值（request）
@RequestMapping("/testReturnModelAndView")
public ModelAndView testReturnModelAndView(){

    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("name", "zs");
    modelAndView.addObject("age", 18);

    // 设置视图名---相当于前面的return "success"---》/WEB-INF/pages/success.jsp
    modelAndView.setViewName("success");

    return modelAndView;
}
```

### 4.小结

1. 返回值为字符串，默认就是视图名字
2. void（了解）
3. 返回类型为ModelAndView：保存数据到域对象、指定视图



## 知识点-转发和重定向

### 1.目标

- [ ] 掌握Controller中的转发和重定向使用

### 2.路径

1. forward 转发 
2. Redirect 重定向 

### 3.讲解

#### 3.1forward 转发 

​	controller 方法在提供了 String 类型的返回值之后，默认就是请求转发。我们也可以加上  `forward:`  可以转发到页面,也可以转发到其它的controller方法

==`forward:url`  替代  `request.getRequestDispatcher(url).forward(request,response)`，好处是不需要在方法里面添加参数：request,response==

+ 转发到页面

  ​	==**需要注意的是，如果用了 forward： 则路径必须写成`实际视图 url`，不能写逻辑视图**==。它相当于“request.getRequestDispatcher("url").forward(request,response)” 

```java
// 把请求转发到jsp
@RequestMapping("/forwardTest01")
public String forwardTest01(){
    return "forward:/WEB-INF/pages/success.jsp";
}
```

+ 也可以转发到其它的controller方法

  语法: `forward:/类上的RequestMapping/方法上的RequestMapping`

```java
//转发到其它controller
@RequestMapping("forwardToOtherController")
public String forwardToOtherController(){
    System.out.println("forwardToOtherController...");
    return "forward:/response/testReturnModelAndView";
}
```

#### 3.2Redirect 重定向 

​	contrller 方法提供了一个 String 类型返回值之后， 它需要在返回值里使用: ` redirect: ` 同样可以重定向到页面,也可以重定向到其它controller

+ 重定向到页面

  ​	它相当于“response.sendRedirect(url)” 。
  
  ==**需要注意的是，重定向是新发起了二次请求，所以如果是重定向到 jsp 页面，则 jsp 页面不能写在 WEB-INF 目录中，否则无法找到。**==（WEB-INF受保护）

```java
// 不能重定向到/WEB-INF的资源
@RequestMapping("redirectToPage")
public String redirectToPage(){
    System.out.println("redirectToPage...");
    return "redirect:/redirect.jsp";
    //return "redirect:/WEB-INF/pages/success.jsp";
}
```

+ 也可以重定向到其它的controller方法

  语法: `redirect:/类上的RequestMapping/方法上的RequestMapping`

```java
    //重定向到其它Controller
    @RequestMapping("redirectToOtherController")
    public String redirectToOtherController(){
        System.out.println("redirectToOtherController...");
        return "redirect:/response/testReturnModelAndView";
    }
```



### 4.小结

#### 4.1转发和重定向区别

1. 转发是一次请求, 重定向是两次请求
2. 转发路径不会变化, 重定向的路径会改变
3. 转发只能转发到内部的资源,重定向可以重定向到内部的(当前项目里面的)也可以是外部的(项目以外的)
4. 转发可以转发到WEB-INF里面的资源, 重定向不可以重定向到WEB-INF里面的资

#### 4.2 转发和重定向(返回String)

1. 转发到页面  

```
forward:/页面的路径
```

2. 转发到Controller

```
forward:/类上面的RequestMapping/方法上面的RequestMapping
```

3. 重定向到页面  

```
redirect:/页面的路径
```

4. 重定向到Controller

```
redirect:/类上面的RequestMapping/方法上面的RequestMapping
```





## 知识点-ResponseBody响应 json数据【重点】

### 1.目标

- [ ] 掌握SpringMVC与json交互

### 2.路径

1. 使用说明 
2. 使用示例 

### 3.讲解

#### 3.1使用说明 

​	该注解@ResponseBody 用于将 Controller 的方法返回的对象，通过 `HttpMessageConverter` 接口转换为指定格式的数据如： json,xml 等，通过 Response 响应给客户端 。

Springmvc 默认用 MappingJacksonHttpMessageConverter 对 json 数据进行转换，仅仅需要添加jackson依赖即可。

#### 3.2使用示例 

需求: 发送Ajax请求,  使用@ResponseBody 注解实现将 controller 方法返回java对象转换为 json 响应给客户端

步骤:

1. ==引入jackson依赖包==
2. 在我们的方法添加@ResponseBody 注解
3. 在方法里面返回一个对象

实现:

- 在springmvc.xml里面设置过滤资源

  **DispatcherServlet会拦截到所有的资源(除了JSP)，导致一个问题就是静态资源（img、css、js）也会被拦截到，从而不能被使用。解决问题就是需要配置静态资源不进行拦截.**

  ==语法: `<mvc:resources location="/css/" mapping="/css/**"/>`, location:webapp目录下的目录,mapping:匹配请求路径的格式==

  ==在springmvc.xml配置文件添加如下配置==

==【注意事项：当使用mvc:resources...标签之后，默认的处理器映射器就不会被加载，那么你需要在配置中增加<mvc:annotation-driven/>】==

```xml
<!-- 设置静态资源不过滤 -->
<mvc:resources location="/css/" mapping="/css/**"/>  <!-- 样式 -->
<mvc:resources location="/img/" mapping="/img/**"/>  <!-- 图片 -->
<mvc:resources location="/js/" mapping="/js/**"/>  <!-- javascript -->
```

- 页面

```html
<a href="queryUser.jsp">查询用户界面</a>


// queryUser.jsp页面
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
</head>
<body>
    <input type="button" value="查询用户" id="btId">
</body>

    <script>
        //绑定事件，发起请求
        $("#btId").click(function () {
            $.ajax({
                url: "response/queryUser",
                data:{},
                dataType: "json",
                type:"GET",
                success: function (res) {
                    alert(JSON.stringify(res))
                }
            })
        })
    </script>
</html>

```

+ Springmvc 默认用 MappingJacksonHttpMessageConverter 对 json 数据进行转换，需要添加jackson依赖。

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.9.0</version>
</dependency>
```

+ JsonController.java

```java
// 获得响应json格式数据
@RequestMapping("/queryUser")
@ResponseBody
public User queryUser(){
    User user = new User();
    user.setUsername("zs");
    user.setAge(18);
    return user;
}
```

### 4.小结

#### 4.1实现步骤

1. 引入jackson依赖坐标
2. 在方法中增加@ResposeBode注解即可（返回类型应该是一个可以格式化json串的对象）
3. 你需要让静态资源放行，可以使用mvc:resources、也可以使用<mvc:default-servlet-handler/>
4. ==用了mvc:XXX标签之后记得加上<mvc:annotation-driven/>==

#### 4.2注意事项

1. Dispacher的路径是/ , 除了jsp以为所有的资源都匹配, 要使用js、html、img、css等资源时 ，拦截时需要忽略静态资源

![1562054865331](img/1562054865331.png) 







# **部分源码（有兴趣可以作了解）**

> 1. 初始化：DispatcherServlet#initStrategies
> 2. 请求时：DispatcherServlet#doService ---》DispatcherServlet#doDispatch
>    1. 获得处理器链：HandlerExecutionChain、HandlerMethod（coontroller的哪个方法来处理请求）
>       1. AbstractHandlerMapping#getHandlerExecutionChain
>       2. 处理器链包含了一个属性handler就是处理请求的方法（此时是一个HandlerMethod对象）
>    2. 根据处理器链的handler属性获得处理器适配器：HandlerAdapter
>       1. 会根据handler判断是否符合初始化时的HandlerAdapter：比如RequestMappingHandlerAdapter符合就找到了HandlerAdapter
>       2. 调用AbstractHandlerMethodAdapter#handle（RequestMappingHandlerAdapter的父类）方法==调用真正的处理器----->RequestMappingHandlerAdapter#handleInternal--》RequestMappingHandlerAdapter#invokeHandlerMethod--》ServletInvocableHandlerMethod#invokeAndHandle---》桥接模式拿到一个目标方法的method对象，反射调用（InvocableHandlerMethod#doInvoke）== 处理完毕可能会得到一个ModelAndView（如果有的话）
>       3. ![1595791108955](img/1595791108955.png)
>       4. 处理器链会做一些后置处理：比如应用拦截器（如：类型转换拦截器）
>    3. 收尾工作:处理分发结果：DispatcherServlet#processDispatchResult、还原数据：清除request的数据信息等DispatcherServlet#restoreAttributesAfterInclude
>
> 

![1595767762094](img/1595767762094.png) 

![](img/springmvc.png)



# 总结

- SpringMVC介绍： 是web层的一个框架
  - 作用： 获取参数、做出响应
- springMVC入门案例
  - 引入依赖web、webmvc模块
  - 写一个类@Controller注解、写方法@RequestMapping标记
  - 写springmvc.xml
    - 开启注解包扫描
    - 配置视图解析器
  - 写web.xml
    - 配置总控制器DispatcherServlet
      - 启动项
      - 初始化参数，指定spring配置文件
- 请求
  - 参数绑定
    - 简单类型：页面参数名和方法参数名一致即可
    - pojo类型：页面参数名和pojo的属性名一致即可
    - pojo包含list
    - pojo包含map
  - requestParam：获取请求参数
  - requestBody：获取请求体，针对post请求的
  - pathVariable：获取url路径的变量
    - REST风格，可以使用url占位符  ${id}
- 响应
  - 方法返回值类型
    - 字符串： 代表逻辑视图
    - vod（了解）
    - ModelAndView：数据模型和视图
      - 可以存储数据到域对象
      - 指定视图名字
  - 请求转发、重定向
    - 转发： 方法中返回字符串：   "forward:url路径"
    - 重定向：方法中返回字符串：   "redirect:url路径"
  - 返回json格式
    - 引入jackson依赖坐标
    - 在方法中添加@ResponseBody
    - 你需要让静态资源放行，可以使用mvc:resources、也可以使用<mvc:default-servlet-handler/>
    - ==用了mvc:XXX标签之后记得加上<mvc:annotation-driven/>==