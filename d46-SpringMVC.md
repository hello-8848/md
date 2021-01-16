# day46-SpringMVC

# 学习目标

- [ ] 掌握文件上传
- [ ] ==掌握SpringMVC的统一异常处理==
- [ ] 了解SpringMVC拦截器
- [ ] ==掌握SSM整合（SpringMVC+spring+mybatis)== 

# 第一章-SpringMVC 实现文件上传

## 知识点-文件上传介绍

### 1.目标

- [ ] 掌握文件上传的要求

### 2.路径

1. 文件上传概述
2. 文件上传要求
3. 常见的文件上传jar包和框架

### 3.讲解

#### 3.1文件上传概述

​	就是把客户端(浏览器)的文件保存一份到服务器 说白了就是文件的拷贝

#### 3.2文件上传要求

##### 3.2.1浏览器端要求(通用浏览器的要求)

- **表单提交方式 post**      
- **提供文件上传框(组件)  input type="file"**
- **表单的enctype属性必须为 `multipart/form-data`(没有这个属性值的话, 文件的内容是提交不过去的)**

```jsp
<h3>一：文件上传要求</h3>
<form action="upload/upload01" method="post" enctype="multipart/form-data">
    文件名: <input type="text" name="desc"><br>
    文件: <input type="file" name="fileName"><br>
    <input type="submit" value="提交">
</form>
```





- 搭建springMVC环境进行开发

```java
package com.itheima.controller;


import com.itheima.utils.UploadUtils;
import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.WebResource;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@Controller
@RequestMapping("/upload")
public class UploadController {


    /*@RequestMapping("/upload01")
    public String upload01(String desc, String fileName){
        System.out.println("desc="+desc + ",fileName="+fileName);
        return "success";
    }*/

    @RequestMapping("/upload01")
    public String upload01(HttpServletRequest request){
        String desc = request.getParameter("desc");
        String fileName = request.getParameter("fileName");
        System.out.println("desc="+desc + ",fileName="+fileName);
        return "success";
    }
}

```





##### 3.2.2服务器端要求

**注意:**

+ ==若表单使用了 multipart/form-data ,使用原生request.getParameter()去获取参数的时候都为null==--->因为参数位置变了；  需要解析输入流来获取数据，比如使用request.getInputStream()来获取数据，所以一般会借助第三方的工具来解析. 
+ ![1595949561507](img/1595949561507.png)

> 我们做文件上传一般会借助第三方组件(jar, 框架 SpringMVC)实现文件上传.

#### 3.3常见的文件上传jar包和框架

​	commons-fileupload :  apache出品的一款专门处理文件上传的工具包 

​	struts2(底层封装了:commons-fileupload)   

​	SpringMVC(底层封装了:commons-fileupload)   

### 4.小结

- 文件上传的要求
  - 表单提交方式指定为post
  - 表单属性encType值指定为multipart/form-data
  - 写一个标签imput的type属性写file即可

## 案例-springmvc 传统方式文件上传

### 1.需求

- [ ] 使用springmvc 完成传统方式文件上传 

### 2.步骤

1. 把**commons-fileupload**坐标导入进来
2. 注册文件解析器bean：==CommonsMultipartResolver==
3. 在控制器的方法的形参里面定义变量参数绑定==MultipartFile==
4. 把文件存到服务器

### 3.实现

==注意事项，页面的file的name属性值需要和方法参数类型为MultipartFile的参数名一致==

+ 创建Maven工程,添加依赖

```html
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```

+ 创建前端页面

```html

<h3>二：SpringMVC文件上传</h3>
<form action="upload/upload02" method="post" enctype="multipart/form-data">
    文件名: <input type="text" name="desc"><br>
    <%--file的name的值要和方法的MultipartFile类型的参数变量名一致--%>
    文件: <input type="file" name="multipartFile"><br>
    <input type="submit" value="提交">
</form>
```

+ 控制器

```java
@RequestMapping("/upload02")
    public String upload02(HttpServletRequest request, MultipartFile multipartFile) throws IOException {

        // multipartFile 通过参数绑定获得了文件信息
        String originalFilename = multipartFile.getOriginalFilename();
        System.out.println(originalFilename);


        // 上传文件--就是保存文件到服务器所在的电脑
        // 目的：放到D盘upload目录名字也为 原始文件名originalFilename

        File dir = new File("D:\\upload");
        // 文件资源(第一个参数是指定父目录的File对象， 第二个参数是你的当前要操作的名字)
        // 类似于获得了 D:\\upload\\originalFilename
        //File uploadFile = new File(dir, originalFilename);
        File uploadFile = new File(dir, UploadUtils.getUUIDName(originalFilename));



        multipartFile.transferTo(uploadFile);


        return "success";
    }
```

+ 在springmvc.xml配置文件解析器 

  注意：文件上传的解析器 ==id 是固定的写死为 multipartResolver，不能起别的名称==，否则无法实现请求参数的绑定。（不光是文件，其他字段也将无法绑定） 

```xml
<!--注册文件上传解析器
        bean的id要写死为 multipartResolver
    -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--指定文件上传的最大值 5M-->
        <property name="maxUploadSize" value="5242880"/>
    </bean>
```

![1602902000647](img/1602902000647.png)

### 4.小结

- 文件上传的细节
  - 引入依赖包
  - 在配置文件中注册一个bean：CommonsMultipartResolver，id要指定为multipartResolver
  - 在方法中写一个参数类型为MultipartFile，保证页面的文件的name属性值和方法的参数名一致（参数绑定）
  - transforTo(file对象)



## 案例-springmvc 跨服务器方式的文件上传

### 1.需求

- [ ] 了解使用springmvc 跨服务器方式的文件上传 

### 2.分析

#### 2.1分服务器的目的 

在实际开发中，我们会有很多处理不同功能的服务器(注意：此处说的不是服务器集群)。 例如：

​	应用服务器（tomcat、nginx、jetty...）：负责部署我们的应用 
​	数据库服务器：运行我们的数据库
​	缓存和消息服务器(redis、MQ)：负责处理大并发访问的缓存和消息
​	文件服务器(ftp、FastDFS)：负责存储用户上传文件的服务器。 

分服务器处理的目的是让服务器各司其职，从而提高我们项目的运行效率。 

![img](img/tu_2.png)

#### 2.2跨服务器方式的文件上传图解

​	==使用2个tomcat来模拟应用服务器、文件服务器，需求：将文件上传保存到第二个tomcat的某个应用的upload目录中。==

![1602901735472](img/1602901735472.png)

+ **准备2个tomcat服务器, 修改tomcat的的conf目录下的web.xml， 添加==readonly==参数为==false==（==在默认的DefaultServlet里面添加初始化参数==）**

![img](img/tu_4.png)

### 3. 使用jersey的API实现跨服务器上传

**API**

- 创建一个Client： Client.create()
- 利用client与远程建立关系：WebResource resource = client.resource(tomcat2的应用里面的文件路径名);
- 上传：resource.put( multipartFile.getBytes() )

-----------------------

+ 添jersey依赖  

```xml
  		<dependency>
  			<groupId>com.sun.jersey</groupId>
  			<artifactId>jersey-core</artifactId>
  			<version>1.18.1</version>
  		</dependency>
  		<dependency>
  			<groupId>com.sun.jersey</groupId>
  			<artifactId>jersey-client</artifactId>
  			<version>1.18.1</version>
  		</dependency>
```

+ 前端页面

```html

<h3>三：SpringMVC跨服务器文件上传</h3>
<form action="upload/upload03" method="post" enctype="multipart/form-data">
    文件名: <input type="text" name="desc"><br>
    <%--file的name的值要和方法的MultipartFile类型的参数变量名一致--%>
    文件: <input type="file" name="multipartFile"><br>
    <input type="submit" value="提交">
</form>
```

+ 控制器

```java
@RequestMapping("/upload03")
    public String upload03(MultipartFile multipartFile) throws IOException {

        // 保证你的tomcat2的aaa项目的upload目录要存在【409错误码--你的upload目录不存在】
        String remoteDir = "http://localhost:8082/aaa/upload/";

        // 1. 创建client
        Client client = Client.create();
        //2. 和远程建立联系，准备一个web资源：参数就是你要上传的文件在远程所在的位置
        // http://localhost:8082/aaa/upload/multipartFile.getOriginalFilename()
        WebResource resource = client.resource(remoteDir + multipartFile.getOriginalFilename());
        //3. 资源放上去： 传入代表文件的方法的参数的字节数组
        resource.put(multipartFile.getBytes());
        return "success";
    }
```

### 4.小结

- 使用第三方的工具来完成跨服务器上传文件jersey【文件上传的代码抄就可以了】
  - 引入依赖坐标
  - 使用时3步走
    - 创建client
    - 创建远程资源
    - 把资源放上去

# 第二章-SpringMVC 中的异常处理【重点】 

## 知识点-SpringMVC 中的异常处理

### 1.目标

- [ ] 掌握SpringMVC的统一异常处理

### 2.分析

​	系统中异常包括两类：预期异常和运行时异常RuntimeException，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。

       系统的dao、service、controller出现都通过throws Exception向上抛出，最后由springmvc前端控制器交由异常处理器进行异常处理，如下图：

​	![img](img/tu_5.png)

​	springmvc在处理请求过程中出现异常信息交由异常处理器进行处理，自定义异常处理器可以实现一个系统的异常处理逻辑。



### 3.代码实现

- 写一个类实现**HandlerExceptionResolver**接口
- 注册自定义的异常处理器bean

#### 3.1自定义异常类

目的: 统一的管理异常, 方便统一管理错误提示语

```java
package com.itheima.exception;

/**
 * 表示自己系统的一个业务异常类（封装了错误码、错误信息）
 */
public class BusinessException extends RuntimeException {

    public static int SYSTEM_ERROR_CODE = -1;
    public static String SYSTEM_ERROR_MSG = "系统繁忙，稍后再试";

    private int code; // 错误码
    private String msg; // 错误信息


    public BusinessException(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}

```

#### 3.2自定义异常处理器

```java
package com.itheima.exceptionhandler;

import com.itheima.exception.BusinessException;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyExceptionRolver implements HandlerExceptionResolver {

    /**
     * 异常统一处理的逻辑：可以在发生异常时返回一个错误页面
     * 
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response,
                                         Object handler, Exception ex) {


        ModelAndView modelAndView = new ModelAndView();
        // 如果异常是自定义的业务异常，就获取其码、信息返回
        if(ex instanceof BusinessException){
            BusinessException businessException = (BusinessException) ex;

            modelAndView.addObject("code", businessException.getCode());
            modelAndView.addObject("msg", businessException.getMsg());
            modelAndView.setViewName("msg");
        }else{
            // 如果不是，统一处理成系统繁忙稍后再试
            ex.printStackTrace();

            modelAndView.addObject("code", BusinessException.SYSTEM_ERROR_CODE);
            modelAndView.addObject("msg", BusinessException.SYSTEM_ERROR_MSG);
            modelAndView.setViewName("msg");
        }
        return modelAndView;
    }
}

```

- UserController

```java
package com.itheima.controller;

import com.itheima.exception.BusinessException;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/user")
public class UserController {
    @Autowired
    UserService userService;

    @RequestMapping("/queryUser")
    public String queryUser(){
        System.out.println("queryUser...");
        try {
            userService.queryUser();
        } catch (Exception e) {
            e.printStackTrace();
            throw new BusinessException(222, "查询用户异常");
        }
        return "success";

    }

}

```



- 准备业务层、dao层代码

```java
package com.itheima.service;

public interface UserService {
    void queryUser();
}


----------------------------------------------
package com.itheima.service.impl;

import com.itheima.dao.UserDao;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    UserDao userDao;

    @Override
    public void queryUser() {
        System.out.println("业务层查询用户");
        int i = 1/0;

    }
}

=================================================
package com.itheima.dao;

public interface UserDao {
    void queryUser();
}

-------------------------------------------
    
package com.itheima.dao.impl;

import org.springframework.stereotype.Repository;

@Repository
public class UserDaoImpl implements com.itheima.dao.UserDao {
    @Override
    public void queryUser() {
        System.out.println("dao层曾查询用户");
    }
}



```



#### 3.3配置异常处理器

+ 在springmvc.xml配置

```xml
<!--自定义异常处理器-->
    <bean class="com.itheima.exceptionhandler.MyExceptionRolver"/>
```

### 4.小结

- 写一个类实现处理器异常解析器HandlerExceptionResolver
- 注册这个bean即可

# 第三章-SpringMVC 中的拦截器

## 知识点-拦截器入门【掌握】

### 1.目标

- [ ] 掌握自定义拦截器入门

### 2.路径

1. 拦截器概述 
2. 自定义拦截器入门

### 3.讲解

#### 3.1.拦截器概述 

​	Spring MVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器(自己编写的Controller)进行预处理和后处理。用户可以自己定义一些拦截器来实现特定的功能。谈到拦截器，还要向大家提一个词——拦截器链（Interceptor Chain）。拦截器链就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。说到这里，可能大家脑海中有了一个疑问，这不是我们之前学的过滤器吗？是的它和过滤器是有几分相似，但
是也有区别，接下来我们就来说说他们的区别： 

| 类别   | 使用范围        | 拦截范围                        |
| ---- | ----------- | --------------------------- |
| 拦截器  | SpringMVC项目 | 只会拦截访问的控制器方法                |
| 过滤器  | 任何web项目     | 任何资源(servlet,控制器,jsp,html等) |

​	==我们要想自定义拦截器， 要求必须实现： HandlerInterceptor 接口==。 

#### 3.2.自定义拦截器入门

- 编写一个普通类实现 HandlerInterceptor 接口
- 配置拦截器：<mvc:interceptors> 标签

---------------------------

+ 编写一个普通类实现 HandlerInterceptor 接口 

```java
package com.itheima;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 自定义拦截器，实现HandlerInterceptor
 */
public class MyInterceptor implements HandlerInterceptor {

    // 在进行处理器之前拦截，返回false则表示被拦截，反之返回true则表示放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        System.out.println("MyInterceptor.preHandle....被执行了...");

        return true;
    }
}

```

+ 在springmvc.xml配置拦截器 

```xml
<!--配置拦截器-->
    <mvc:interceptors>

        <!--配置一个拦截器-->
        <mvc:interceptor>
            <!--mvc:mapping表示要拦截哪些资源
                /** 表示可以拦截所有资源（资源仅仅限于控制器的方法）
            -->
            <mvc:mapping path="/**"/>
            <bean class="com.itheima.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

### 4.小结

- 写一个类实现接口 HandlerInterceptor，重写方法preHandler方法
- 配置拦截器 <mvc:intercepters>

## 知识点-自定义拦截器进阶【了解】

### 1.目标

- [ ] 掌握自定义拦截器的高级使用

### 2.路径

1. 拦截器的放行
2. 拦截后跳转
3. 拦截器的路径
4. 拦截器的其它方法
5. 多个拦截器执行顺序

### 3.讲解

#### 3.1拦截器的放行

preHandle 方法返回true则表示放行：

![1602922734608](img/1602922734608.png)

#### 3.2拦截后跳转

​	拦截器的处理结果，莫过于两种:

​		放行：  如果后面还有拦截器就执行下一个拦截器，如果后面没有了拦截器，就执行Controller方法

​		拦截： 但是注意，拦截后也需要返回到一个具体的结果(页面,Controller)。 

- 在preHandle方法返回false,通过request进行转发,或者通过response对象进行重定向,输出

```java
package com.itheima;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// 自定义拦截器
public class MyInterceptor implements HandlerInterceptor {

    // 在处理之前进行拦截
    // 返回为true则表示通过，不拦截； 为false则表示被拦截了。
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...");

        // 转发
        //request.getRequestDispatcher("").forward(request, response);

        // 重定向
        //response.sendRedirect("");

        return false;
    }
}

```

#### 3.3拦截器的路径

 <mvc:exclude-mapping> 表示不拦截的资源配置

<mvc:mapping> 表示需要被拦截的资源配置

```xml
<!--配置拦截器-->
    <mvc:interceptors>

        <!--配置一个拦截器-->
        <mvc:interceptor>
            <!--mvc:mapping表示要拦截哪些资源
                /** 表示可以拦截所有资源（资源仅仅限于控制器的方法）
            -->
            <mvc:mapping path="/**"/>

            <!--配置路径：不需要被拦截的资源的路径-->
            <mvc:exclude-mapping path="/user/queryUser"/>

            <bean class="com.itheima.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```



#### 3.4拦截器的其它方法

+ afterCompletion  在目标方法完成视图层渲染后执行。
+ postHandle  在被拦截的目标方法执行完毕获得了返回值后执行。
+ preHandle 被拦截的目标方法执行之前执行。

```java
package com.itheima;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 自定义拦截器，实现HandlerInterceptor
 */
public class MyInterceptor implements HandlerInterceptor {

    // 在进行处理器之前拦截，返回false则表示被拦截，反之返回true则表示放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        System.out.println("MyInterceptor.preHandle....被执行了...");

        return true;
    }


    //在被拦截的目标方法执行完毕获得了返回值后执行
    // 先于afterCompletion执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("MyInterceptor.postHandle....被执行了...");
    }

    // 控制层方法执行完毕后且视图解析了再执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor.afterCompletion....被执行了...");
    }
}

```





#### 3.5多个拦截器执行顺序

​	我们可以配置多个拦截器, 所以就存在一个优先级问题了.==多个拦截器的优先级是按照配置的顺序决定的==。 

![1602907089943](img/1602907089943.png)

输出：

>MyInterceptor.preHandle....被执行了...
>MyInterceptor2.preHandle....被执行了...
>dao层查询用户....

+ 配置顺序

```xml
<!--配置拦截器-->
    <mvc:interceptors>

        <!--配置一个拦截器-->
        <mvc:interceptor>
            <!--mvc:mapping表示要拦截哪些资源
                /** 表示可以拦截所有资源（资源仅仅限于控制器的方法）
            -->
            <mvc:mapping path="/**"/>

            <!--配置路径：不需要被拦截的资源的路径-->
            <!--<mvc:exclude-mapping path="/user/queryUser"/>-->

            <bean class="com.itheima.MyInterceptor"/>
        </mvc:interceptor>


        <!--配置第二个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.itheima.MyInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
```



### 4.小结

- 写一个类实现HandlerInterceptor（需要重写方法）
- 配置拦截器<mvc:interceptors>

# 第四章-环境的准备

## 知识点-SSM整合环境的准备

### 1.目标

- [ ] 能够独立准备SSM环境

### 2.步骤

1. 创建数据库和表
2. 创建Maven工程(war)
   + 导入坐标（需要引入==mybatis==、以及==mybatis-spring==整合jar）
   + 创建实体类
   + 拷贝log4J日志到工程

### 3.讲解

#### 3.1创建数据库和表结构 

```mysql
create database ssm;
use ssm;
create table account(
    id int primary key auto_increment,
    name varchar(40),
    money double
)character set utf8 collate utf8_general_ci;

insert into account(name,money) values('zs',1000);
insert into account(name,money) values('ls',1000);
insert into account(name,money) values('ww',1000);
```

#### 3.2创建Maven工程

+ 创建web项目（整合完成后的目录结构）

![1602922870203](img/1602922870203.png)

+ 导入坐标 

```xml
<properties>
		<spring.version>5.0.2.RELEASE</spring.version>
		<slf4j.version>1.6.6</slf4j.version>
		<log4j.version>1.2.12</log4j.version>
		<mysql.version>5.1.6</mysql.version>
		<mybatis.version>3.4.5</mybatis.version>
</properties>

	<dependencies>

	  <!-- spring -->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.6.8</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>

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
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>compile</scope>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
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

    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <!-- log start -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j.version}</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <!-- log end -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>

      <!--mybatis和spring的整合依赖包-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.0.14</version>
    </dependency>
        
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.8</version>
            <scope>provided</scope>
        </dependency>

	</dependencies>
```

+ 编写实体类 

```java
public class Account implements Serializable {
    private Integer id;
    private String name;
    private double money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    public double getMoney() {
        return money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}
```

+ 在resources目录创建文件：log4j.properties， 拷贝log4J配置文件到工程 ： log4j.properties

```properties
log4j.appender.std=org.apache.log4j.ConsoleAppender
log4j.appender.std.Target=System.out
log4j.appender.std.layout=org.apache.log4j.PatternLayout
log4j.appender.std.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %5p %c{1}:%L - %m%n

log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

log4j.rootLogger=debug,std, file
```



### 4.小结

1. 坐标(Spring整合MyBatis)

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.0</version>
</dependency>
```

2. 工程war



# 第五章-Spring整合SpringMVC

## 知识点-初级版本(SpringMVC独立运行)

### 1.目标

- [ ] 初步版本(SpringMVC独立运行)

### 2.步骤

1. 编写配置文件springmvc.xml
   1. 注解包扫描
   2. 配置视图解析器
   3. 把静态资源过滤
   4. mvc注解驱动
2. 在web.xml中配置
   1. 总控制器
   2. 乱码处理过滤器



### 3.实现

+ 创建AccountController.java

```java
package com.itheima.controller;

import com.itheima.bean.Account;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
@RequestMapping("/account")
public class AccountController {

    

    @RequestMapping("/findAll")
    public String findAll(){
        System.out.println("AccountController.findAll...");

        
        return "success";
    }


    
}

```

+ 创建springmvc.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--开启注解包扫描-->
    <context:component-scan base-package="com.itheima"/>

    <!--视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--过滤静态资源（让静态资源可以访问）-->
    <mvc:default-servlet-handler/>

    <!--mvc注解驱动（加载默认的处理器映射器组件）-->
    <mvc:annotation-driven/>

</beans>
```

+ 在web.xml里面配置前端控制器

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
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>


    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>

```

### 4.小结

1. 创建Controller, 创建方法 添加注解
2. 创建springmvc.xml(开启包扫描, 注册视图解析器,忽略静态资源,开启注解驱动)
3. 配置web.xml(前端控制器, 编码过滤器)



## 知识点-终极版本(注入业务层)

### 1.目标

- [ ] 终极版本(注入业务层)

### 2.步骤

1. 注册Service
2. 在Controller里面注入Service

### 3.讲解

+ 编写AccountService.java

```java
public interface AccountService {
    List<Account> findAll();
}

```

+ 编写AccountServiceImpl.java

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Override
    public List<Account> findAll() {
        System.out.println("findAll...业务层");
        return new ArrayList<>();
    }
}
```

+ 在AccountController调用AccountService

```java
@RequestMapping("/account")
public class AccountController {

    @Autowired
    AccountService accountService;

    @RequestMapping("/findAll")
    public String findAll(){
        System.out.println("AccountController.findAll...");

        List<Account> accounts = accountService.findAll();
        return "success";
    }

}
```

### 4.小结

1. 包扫描 ` com.itheima`,
   + 注册Service的时候, 直接在Service类上面添加@Service
   + 在Controller注入Service的时候, 添加@AutoWired



# 第六章-Spring整合MyBatis

## 知识点-初级版本(MyBatis独立运行)

### 1.目标  

- [ ] 初级版本(MyBatis独立运行)

### 2.步骤

1. 引入相关依赖
2. 写mybatis核心配置文件
3. 写映射文件、接口（目录层级要一致）
4. 使用：获取代理对象运行

### 3.实现

+ 创建AccountDao.java, 添加注解

```java
package com.itheima.dao;

import com.itheima.pojo.Account;

import java.util.List;

public interface AccountDao {

    List<Account> findAll();
}

```

+ jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.username=root
jdbc.password=root
```

- 编写MyBatis核心配置文件:mybatis-cofig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--引入jdbc配置参数-->
    <properties resource="jdbc.properties"/>

    <!--设置下划线转驼峰命名自动转换-->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <!--配置别名-->
    <typeAliases>
        <package name="com.itheima.bean"/>
        <package name="com.itheima.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--配置扫描mapper、接口-->
    <mappers>
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

+ 测试运行结果 

```java
package com.itheima;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class MyBatisTest {
    @Test
    public void test() throws Exception{
        //1. 加载核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-cofig.xml");

        //2. 获取sqlSessionFactory
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        // 3. 获得SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4. 获得代理对象
        AccountDao accountDao = sqlSession.getMapper(AccountDao.class);


        //5. 操作执行方法
        List<Account> accounts = accountDao.findAll();
        System.out.println(accounts);

        //6. 资源关闭
        sqlSession.close();
        resourceAsStream.close();


    }
}

```

### 4.小结

1. 编写核心配置文件
2. 编写接口、映射文件（目录层级一致）



## 知识点-终极版本

### 1.目标

- [ ] 终极版本

### 2.分析

#### 2.1初级版本问题

1. 连接池----使用mybatis内置的----spring管理连接池bean
2. SqlSessionFactory-spring注册bean解决
3. Dao--手动获取代理对象----使用扫描方式自动创建代理对象
4. 事务---交给spring管理（事务管理器）

#### 2.2步骤

1. 创建一个配置文件applocationContext.xml
2. 需要配置连接池
3. 注册SqlSessionFactoryBean【在整合包里面】
4. 注册一个扫描配置的bean【在整合包里面】
5. DataSourceTransactionManage注册
6. 事务通知配置（AOP配置）

### 3.实现

​	在终极版本中:

​	1.把SqlSessionFactory交给Spring管理,也就意味着把 mybatis 配置文件（SqlMapConfig.xml）中内容配置到 spring 配置文件中同时，把 mybatis 配置文件的内容清掉。 

​	2.使用Spring管理事务

#### 3.1配置 spring 的事务

- jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.username=root
jdbc.password=root
```



+ 创建applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--配置数据库相关的信息-->

    <!--引入外部的属性配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!--配置数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
    </bean>

    <!--注册事务管理器bean-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--xml方式配置spring声明式事务-->
    <!--配置事务规则-->
    <tx:advice id="transactionInterceptor" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="select*" read-only="true"/>
            <tx:method name="query*" read-only="true"/>
            <tx:method name="update*" />
        </tx:attributes>
    </tx:advice>

    <!--配置aop-->
    <aop:config>
        <!--切入点：对service的所有方法进行切入点增强-->
        <aop:pointcut id="txPointcut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
        <!--通知-->
        <aop:advisor advice-ref="transactionInterceptor" pointcut-ref="txPointcut"/>
    </aop:config>



    
</beans>
```

> 注意:由于我们使用的是代理 Dao 的模式， Dao 具体实现类由 MyBatis 使用代理方式创建，所以此时 mybatis配置文件不能删。



#### 3.2Spring 接管MyBatis的Session工厂

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--配置数据库相关的信息-->

    <!--引入外部的属性配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!--配置数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
    </bean>

    <!--注册事务管理器bean-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--xml方式配置spring声明式事务-->
    <!--配置事务规则-->
    <tx:advice id="transactionInterceptor" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="select*" read-only="true"/>
            <tx:method name="query*" read-only="true"/>
            <tx:method name="update*" />
        </tx:attributes>
    </tx:advice>

    <!--配置aop-->
    <aop:config>
        <!--切入点：对service的所有方法进行切入点增强-->
        <aop:pointcut id="txPointcut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
        <!--通知-->
        <aop:advisor advice-ref="transactionInterceptor" pointcut-ref="txPointcut"/>
    </aop:config>



    <!--配置mybatis相关信息，用的是整合包里面的类作为bean注册到容器中-->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--mybatis中使用德鲁伊连接池-->
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-cofig.xml"/>

    </bean>

    <!--配置包扫描-->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>
</beans>
```

#### 3.3  springmvc.xml引入applicationContext.xml

1. 方式一:不需要写单独的applicationContext.xml了, 直接把applicationContext.xml里面的配置定义在springmvc.xml

2. 方式二: 名字上面下功夫.

   ![1562231916690](img/1562231916690.png) 

```xml
<!--初始化参数:加载配置文件-->
<init-param>
    <param-name>contextConfigLocation</param-name>
    <!--可以把applicationContext开头的xml文件全部加载-->
    <param-value>classpath:applicationContext*.xml</param-value>
</init-param>
```

3. ==方式三: 在springmvc.xml 引入applicationContext.xml==

​	applicationContext.xml已经写完了, 但是发现并没有被加载,也就意味着Spring整合MyBatis的部分并没有生效. 其实我们在Spring整合SpringMVC时候, 当服务器启动的时候已经加载过springmvc.xml, spring容器也就会被初始化. 我们可以通过==import标签==把applicationContext.xml导入到springmvc.xml中一起加载. 

​	在==**springmvc.xml中导入applicationContext.xml**==

```xml
<!--引入其他的spriing配置文件-->
<import resource="classpath:applicationContext.xml"/>
```

### 4.测试SSM整合结果

#### 4.1查询所有展示页面

+ AccountController.java

```java
package com.itheima.controller;

import com.itheima.bean.Account;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
@RequestMapping("/account")
public class AccountController {

    @Autowired
    AccountService accountService;

    @RequestMapping("/findAll")
    public String findAll(){
        System.out.println("AccountController.findAll...");

        List<Account> accounts = accountService.findAll();
        return "success";
    }


    // 查询所有账户的方法进行测试
    @RequestMapping("/findAllAccounts")
    public ModelAndView findAllAccounts(){
        List<Account> accounts = accountService.findAllAccounts();
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("accounts", accounts);
        modelAndView.setViewName("success");
        return modelAndView;
    }


    
}

```

+ AccountService.java

```java
package com.itheima.service.impl;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;


@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;

    @Override
    public List<Account> findAll() {
        return new ArrayList<>();
    }

    @Override
    public List<Account> findAllAccounts() {
        return accountDao.findAllAccounts();
    }

    
}

```

+ AccountDao.java

```java
package com.itheima.dao;

import com.itheima.bean.Account;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;


@Mapper
public interface AccountDao {
    List<Account> findAll();

    List<Account> findAllAccounts();

    
}

```

+ 前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
success
<hr>

<h3>账户列表</h3>
<table border="1px">
    <tr>
        <td>id</td>
        <td>name</td>
        <td>money</td>
    </tr>

    <c:forEach items="${accounts}" var="account">
        <tr>
            <td>${account.id}</td>
            <td>${account.name}</td>
            <td>${account.money}</td>
        </tr>
    </c:forEach>

</table>

</body>
</html>

```

#### 4.2添加账户

+ 页面

```html
 <h3>增加账户消息</h3>
  <form action="account/save" method="post">
    账户名：<input type="text" name="name"><br>
    金钱：<input type="text" name="money"><br>
    <input type="submit">
  </form>

```

+ AccountController.java

```java
// 保存账户然后跳转到查询所有
    @RequestMapping("/save")
    public String save(Account account){
        boolean flag = accountService.save(account);
        return "redirect:findAllAccounts";
    }
```

+ AccountServiceImpl.java

```java
@Override
    public boolean save(Account account) {
        int cnt = accountDao.save(account);
        return cnt>0;
    }
```

+ AccountDao.java

```java
void save(Account account);
```

### 5.总结

#### 5.1Controller

1. 创建Controller, 定义方法 添加注解
2. 创建springmvc.xml
   + 开启包扫描
   + 注册视图解析器
   + 忽略静态资源
   + 配置注解驱动
3. 配置web.xml
   + 配置前端控制器
   + 配置编码过滤器
4. 注入Service

#### 5.2Service

1. 创建Service接口 实现类 , 注册
2. 创建applicationContext.xml
   + 配置事务管理器(注入数据源)
   + 配置事务建议
   + 配置AOP
3. 注入Dao

#### 5.3Dao

1. 创建Dao接口 定义方法 添加注解
2. 注册数据源(配置四个基本项)
3. 注册SqlSessionFactory(注入DataSource, 注入SqlMapConfig.xml)
4. 注册Dao扫描器



> applicationContext.xml导入到springmvc.xml







# SSM整合回顾

- 步骤
  - //1. springMVC环境搭建，独立运行
  - //2. 在controller层注入业务逻辑
  - //3. mybatis的使用步骤【和spring毫无瓜葛的复习】
  - //4. mybatis、spring整合
  - //5. 基于ssm整合的项目演示案例
- 1. springMVC环境搭建，独立运行
     1. 引入相关依赖（context、aop、jdbc、tx、web、webmvc）
     2. 编写配置文件springmvc.xml
        1. 开启注解包扫描
        2. 注册视图解析器
        3. 把静态资源放行
        4. 开启mvc注解驱动
     3. 编写web.xml
        1. 配置总控制器
        2. 配置编码过滤器
     4. 写了一个controller
- 2. 在controller层注入业务逻辑
     1. 仅仅在controller中依赖注入了service的bean，调用service的方法
- 3. mybatis的使用步骤【和spring毫无瓜葛的复习】
     1. 编写核心配置文件（配置你的数据库连接参数、扫描你的包、配置别名、开启驼峰命名）
     2. 编写接口、映射文件（目录层次一致）
     3. 写代码测试
- 4. mybatis、spring整合【applicationContext.xml】
     1. 数据库连接池、事务管理、sqlsessionFactory创建通过整合来优化
     2. 步骤
        1. 注册连接池bean（引入外部的属性配置文件）
        2. 注册事务管理器bean
        3. 配置事务规则
        4. 配置aop
        5. 注册sqlsessionFactoryBean（依赖注入连接池）
        6. 配置扫描bean（指定扫描的包）
- 5. 基于ssm整合的项目演示案例







- 知识点回顾
  - 异常处理解析器
    - springMVC统一处理控制层的异常，实现接口实现方法，bean注册
    - 自定义异常类
  - 拦截器
    - 和过滤器比较
      - 拦截器： 对控制器的方法拦截；springMVC的技术
      - 过滤器：可以对任意资源拦截；servlet规范的技术，任何web项目都可以用
    - 写一个类实现HandlerInterceptor，重写preHandler即可
  - 再去抄文件上传

