---
typora-copy-images-to: img
---

# day22-http&tomcat&Servlet

# 学习目标

- [ ] 能够理解软件的架构 
- [ ] 能够理解WEB资源概念 
- [ ] 能够理解WEB服务器
- [ ] 能够启动关闭Tomcat服务器 
- [ ] 能够运用Tomcat服务器部署WEB项目 【掌握】
- [ ] 能够使用idea编写servlet【掌握】
- [ ] 能够使用idea配置tomcat方式并发布项目【掌握】
- [ ] 能够使用XML开发servlet、注解开发servlet【掌握】
- [ ] 能够说出servlet生命周期方法执行流程【掌握】
- [ ] 能够使用Servletcontext对象

# 第一章-WEB开发介绍

## 知识点-WEB资源分类

###    1.目标

- 能够理解什么是WEB资源

### 2.讲解

#### 2.1什么是web

​	WEB，在英语中web即表示网页的意思，它用于表示==Internet主机(服务器)上供外界访问的资源==

#### 2.2WEB资源分类

##### 2.2.1静态资源

- web页面中供人们浏览的数据始终是不变。eg. html、js、css、图片

##### 2.2.2动态资源

- 指web页面中供人们浏览的数据是由程序产生的，不同的用户或者不同时间点访问web页面看到的内容各不相同。(eg: servlet、jsp；php、asp)

### 3.小结

1. web资源： 服务器给用户访问的一切资源
2. 静态：用户看到的内容一样的，img、css、html、js
3. 动态：因时因人因地产生变化，servlet、jsp



## 知识点-软件架构

### 1.目标

- 能够理解软件的架构

### 2.讲解

#### 2.1架构类别

##### 2.1.1C/S架构

​	Client / Server,客户端和服务器端，==用户需要安装专门客户端程序。==  eg。 百度网盘、QQ、lol、CF穿越火线、盛大传奇

##### 2.2.2B/S架构

​	Browser / Server,浏览器和服务器端，==不需要安装专门客户端程序，浏览器是操作系统内置。==， eg：我是渣渣辉--网页游戏

#### 2.2B/S 和C/S交互模型的比较

- 相同点

  ​	都是基于==请求-响应交互模型==:即浏览器（客户端） 向 服务器发送 一个 请求。服务器 向 浏览器（客户端）回送 一个 响应 。

  ​	==必须先有请求 再有响应==（砍---》掉血）

  ​	==请求和响应成对出现==

- 不同点

  ​	实现C/S模型需要用户在自己的操作系统安装各种客户端软件（百度网盘、腾讯QQ等）；实现B/S模型，只需要用户在操作系统中安装浏览器即可。



### 3.小结

1. web开发：  
   1. B/S架构的软件系统，淘宝、京东的后台系统
2. java平台开发：
   1. javaee

![1572265073370](img/1572265073370.png)



## 知识点-web通信【掌握】

### 1.目标

- 掌握web通讯机制

### 2.讲解

​	web通信是： 基于http协议的请求响应的机制

​	请求一次响应一次

​	先有请求后有响应		

​	![1599441666881](img/1599441666881.png)

### 3.小结

1. web通讯的机制
   1. 基于http协议的请求响应机制
   2. 先有请求再有响应
   3. 一次请求--一次响应

# 第二章-服务器

## 知识点-服务器介绍

### 1.目标

- 掌握什么是服务器

### 2.讲解

#### 2.1 什么是服务器

​	服务器就是一个软件，任何电脑只需要安装上了服务器软件， 我们的电脑就可以当做一台服务器了. 

​	服务器: mysql、nginx 、tomcat +计算机

#### 2.2常见web服务器

- WebLogic

  ​	是BEA公司的产品，后来被Oracle公司收购，支持J2EE企业级规范。WebLogic是用于开发、集成、部署和管理大型分布式Web应用、网络应用和数据库应用的Java应用服务器。

  ![1555895183498](img/1555895183498.png) 

- WebSphere

  ​       IBM公司的WebSphere，支持JavaEE规范。WebSphere 是随需应变的电子商务时代的最主要的软件平台，可用于企业开发、部署和整合新一代的电子商务应用，提供了编写、运行和监视等基础设施。

  ![1555895215122](img/1555895215122.png) 

- Glass Fish 

  ​      最早是Sun公司的产品，后来被Oracle收购，开源，中型服务器。

- JBoss

  ​      JBoss公司产品，开源，支持JavaEE规范，占用内存、硬盘小，安全性和性能高。

  ![1555895293155](img/1555895293155.png) 

- ==Tomcat==

  并发： 双11，选课系统

  ​    中小型的应用系统，免费,开源,效率特别高, ==适合扩展(搭集群)支持JSP和Servlet.==

  ![1555895400407](img/1555895400407.png) 

### 3.小结

1. 服务器： 一台电脑安装了服务器软件

2. tomcat

   

## 知识点-tomcat介绍,安装和使用

### 1.目标

- 能够安装,启动,关闭Tomcat服务器

### 2.讲解

#### 2.1概述 	

​	Tomcat服务器是一个免费的开放源代码的Web应用服务器。Tomcat是Apache软件基金会（Apache Software Foundation）的Jakarta项目中的一个核心项目，由Apache、Sun和其他一些公司及个人共同开发而成。由于有了Sun的参与和支持，最新的Servlet 和JSP规范总是能在Tomcat中得到体现。

​	因为Tomcat技术先进、性能稳定，而且免费，因而深受Java爱好者的喜爱并得到了部分软件开发商的认可，是目前比较流行的Web应用服务器。

#### 2.2tomcat与nginx的区别 (nginx部分再讲)

> 等我们讲nginx时候, 再比较

- 设计目的 

  ​	Tomcat是一个免费的开源的Servlet(动态资源)容器，实现了JAVAEE规范，遵循http协议的的服务器

  ​	Nginx是一款轻量级的电子邮件（电子邮件遵循IMAP/POP3协议）代理服务器，后来又发展成可以部署静态应用程序和进行反向代理的服务器

- 存放内容

  ​	tomcat可以存放静态和动态资源

  ​	nginx可以存放静态资源

- 应用场景

  ​	tomcat用来开发和测试javaweb应用程序

  ​	nginx用来做负载均衡服务器

#### 2.3tomcat的下载

1. **先去官网下载：http://tomcat.apache.org/，选择tomcat8版本（红框所示）**：

   ![](img/01.png)

2. 选择要下载的文件（红框所示）：

   ![](img/02.png)

    tar.gz 文件 是linux操作系统下的安装版本

    exe文件是window操作系统下的安装版本

    zip文件是window操作系统下压缩版本（我们选择zip文件）

   

3. **下载完成**：

![](img/03.png)

#### 2.4tomcat服务器软件安装

1. 直接解压当前这个tomcat压缩包：(==不要有中文,不要有空格==)

2. 配置**==系统==环境变量**：

   tomcat运行依赖于java环境：==JAVA_HOME系统环境变量==
   
   配置 `JAVA_HOME` 系统环境变量：
   
   ![1596899815392](img/1596899815392.png)		
   
   ==在Path系统环境变量中引入  `%JAVA_HOME%\bin`===
   
   ![1596900045068](img/1596900045068.png)

#### 2.6tomcat的目录结构

![img](img/1571818780481.png)

#### 2.7启动与关闭tomcat服务器

1. 启动tomcat服务器

   查找tomcat目录下==bin==目录，查找其中的==startup.bat==命令，双击启动服务器：
   ![](img/1571818880110.png)

   启动效果：
   ![](img/1571818962695.png)

   

   

2. 测试访问tomcat服务器

   打开浏览器在，在浏览器的地址栏中输入：

   http://127.0.0.1:8080或者http://localhost:8080

   ![](img/08.png)

   注： localhost相当于127.0.0.1

3. 关闭tomcat服务器

   查找tomcat目录下bin目录，查找其中的shutdown.bat命令，双击关闭服务器：
   ![](img/1571819012074.png)



### 3.小结(常见问题)

#### 3.0 安装步骤

- 解压到一个目录（没有中文、空格、+、￥、#....）
- 配置系统环境变量： JAVA_HOME、在path变量中引入  % JAVA_HOME%\bin
- 启动运行，在tomcat的bin目录执行startup.bat即可



#### 3.1 可能遇到的问题(遇到了再来看)

​	1. 报如下异常 端口8080被占用: `java.net.BindException: Address already in use: JVM_Bind   8080`

​	解决办法:

​	第一种:修改Tomcat的端口号   

​	![img](img/1571819050327.png)

​		修改tomcat的conf/server.xml ,  第70行左右

![1596900307327](img/1596900307327.png)

第二种:查询出来哪一个进程把8080占用了, 结束掉占用8080端口后的程序

​		打开命令行输入:  `netstat -ano`  （或者直接查找  `netstat -aon|findstr "8080"`）

​		找到占用了8080 端口的 进程的id

​		kill掉这个id对应的程序：  `taskkill /F /pid 3464`

​	

​		

2. JAVA_HOME 环境变量 没有配置

- 会出现闪退  (如果 JAVA_HOME 配置了还是闪退  就忽略它。 后面在IDEA里面配置tomcat进行启动)

## 知识点-运用Tomcat服务器部署WEB项目

### 1.目标

- 能够运用Tomcat服务器部署WEB项目 

### 2.讲解

#### 2.1 ==标准的JavaWeb项目目录结构==

```
  项目目录(文件夹,项目)  
   		|---静态资源: html,css,js,图片(它们可以以文件存在,也可以以文件夹存在)  
   		|---WEB-INF 目录，固定写法，不要自己随意搞成小写！。此目录下的文件不能被外部(浏览器)直接访问
   			|---lib: jar包存放的目录
   			|---web.xml: 当前项目的配置文件(3.0规范之后可以省略)
   			|---classes目录: java类编译后生成class文件存放的路径
```

知道一下：WEB-INF目录下的所有资源受保护，不会被用户（浏览器）访问，但是项目的程序代码是可以访问。

#### 2.2发布项目到tomcat

##### 2.2.1 方式一:直接发布

​	只要将准备好的web资源直接复制到`tomcat/webapps`文件夹下，启动tomcat，就可以通过浏览器使用http协议访问获取；  目录名就是要访问的项目的名字。以下结构访问方式： http://localhost:8080/LY

![1599444746317](img/1599444746317.png)

##### 2.2.2方式二: 虚拟路径的方式发布项目

1. 第一步：在tomcat/conf目录下新建一个Catalina目录（如果已经存在无需创建）

![](img/26.png)

2. 第二步：在Catalina目录下创建localhost目录（如果已经存在无需创建）

![](img/27.png)

3. 第三步：在localhost中创建xml配置文件，名称为：szheima（注意：==这个名称就是浏览器访问时的名称==）

![1596901013884](img/1596901013884.png)

4. 第四步：添加szheima.xml文件的内容如下：  docBase 属性指向你要部署的项目的目录的地址。这里指向的是F盘的LY目录作为项目

   ```xml
   <?xml version = "1.0" encoding = "utf-8"?>
   <Context docBase="F:\LY" />
   ```

   ![1596901170863](img/1596901170863.png)

5. 第五步：测试访问项目的资源(通过写配置文件的路径来访问):  ==访问的项目名是XX.xml里面的XX==

   http://localhost:8080/szheima/    访问的就是 F盘的LY目录的index.html页面；

   http://localhost:8080/szheima/img/girl.jpg   访问F盘的LY目录的img目录的girl.jpg图片资源。

### 3.小结

1. WEB项目标准结构

   1. ```
      项目
      	+---静态资源 html、css、js、img
      	+---WEB-INF目录（用户在浏览器中不能直接通过url地址访问）
      		+---lib目录： 依赖jar包
      		+---web.xml
      		+---classes目录
      ```

2. 部署方式

   1. 拷贝项目到tomcat/webapps目录
   2. 虚拟路径（对着讲义做一遍即可）

# 第三章-http协议

## 知识点-http协议概述

### 1.目标

- 掌握什么是HTTP协议, 以及HTTP协议的作用

### 2.讲解

#### 2.1什么是HTTP协议

​	HTTP是HyperText Transfer Protocol(超文本传输协议)的简写。.

#### 2.2HTTP协议的作用

​	HTTP作用：用于定义WEB浏览器与WEB服务器之间==交换数据的过程==和数据本身的==内容==

​	浏览器和服务器交互过程: 浏览器请求, 服务器响应

​	请求(请求行,请求头,请求体)

​	响应(响应行,响应头,响应体)

### 3.小结

1. http协议是什么？  超文本传输协议； 规范了请求、响应的数据格式
2. 分为请求、响应



## 知识点-请求部分

### 1.目标

- 掌握请求行,常见请求头,请求体

### 2.讲解

请求行

请求头

请求体

查看浏览器控制台的头信息输出：

![1599446914261](img/1599446914261.png)



- get方式请求
  - ==请求体（参数）一般在url后面拼接的，get请求一般没有请求体==
  - 请求行、请求头之间有空行分隔
  - 请求行： 请求方式  请求路径  协议版本

```
-- 请求行: get请求可以在请求行看到参数  : 请求方式、url地址不包含主机端口号、协议版本
GET /myApp/success.html?username=zs&password=123456 HTTP/1.1

-- 请求头
Accept: text/html, application/xhtml+xml, */*
X-HttpWatch-RID: 97294-10085
Referer: http://localhost:8080/myApp/login.html
Accept-Language: zh-Hans-CN,zh-Hans;q=0.5
User-Agent: Mozilla/5.0 (MSIE 9.0; qdesk 2.4.1266.203; Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko
Accept-Encoding: gzip, deflate
Host: localhost:8080
Connection: Keep-Alive
```

- post请求  ：  ==请求参数放在请求体==
  - 请求行： 请求方式  请求路径  协议版本
  - 请求体是一个单独的部分
  - 请求行、请求头、请求体之间有空行分隔

```
-- 请求行
POST /myApp/success.html HTTP/1.1

-- 请求头
Accept: text/html, application/xhtml+xml, */*
X-HttpWatch-RID: 28521-10109
Referer: http://localhost:8080/myApp/login.html
Accept-Language: zh-Hans-CN,zh-Hans;q=0.5
User-Agent: Mozilla/5.0 (MSIE 9.0; qdesk 2.4.1266.203; Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate
Host: localhost:8080
Content-Length: 27
Connection: Keep-Alive
Cache-Control: no-cache

--请求体
username=zs&password=123456
```

#### 2.1请求行

- 请求行

```
GET  /myApp/success.html?username=zs&password=123456 HTTP/1.1	
POST /myApp/success.html HTTP/1.1
```

- 请求方式(8种,put,delete等)

  ​	GET:明文传输, 不安全,参数跟在请求路径后面,对请求参数大小有限制, 

  ​	POST: 暗文传输,安全一些,请求参数在请求体里,对请求参数大小没有有限制,

- URI:统一资源标识符（即：去掉协议http和IP地址部分）

- 协议版本:HTTP/1.1

#### 2.2请求头

​	从第2行到空行处，都叫请求头,以键值对的形式存在,但存在==一个key对应多个值的请求头==.

​	==作用:浏览器告诉服务器自己相关的设置.==

- Accept:浏览器可接受的MIME类型 ,告诉服务器客户端能接收什么样类型的文件。
- **User-Agent**:浏览器信息.(浏览器类型, 浏览器的版本....)
- Accept-Charset: 浏览器通过这个头告诉服务器，它支持哪种字符集
- Content-Length:表示请求参数的长度 
- Host:初始URL中的主机和端口 
- **Referrer**:表示之前在哪个页面进来的；==防盗链==
- **Content-Type**:内容类型,告诉服务器,浏览器传输数据的MIME类型，文件传输的类型,application/x-www-form-urlencoded . 
- Accept-Encoding:浏览器能够进行解码的数据编码方式，比如gzip 
- Connection:表示是否需要持久连接。如果服务器看到这里的值为“Keep -Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接 )
- **Cookie**:这是最重要的请求头信息之一(会话技术, 后面会有专门的时间来讲的)  
- Date：Date: Mon, 22Aug 2011 01:55:39 GMT请求时间GMT

#### 2.3请求体

​	只有请求方式是post的时候,才有请求体. 即post请求时,请求参数(提交的数据)所在的位置

### 3.小结

1. 行： 请求方式、uri
2. 头： 
   1. **User-Agent**:浏览器信息.(浏览器类型, 浏览器的版本....)
   2. **Referrer**:表示之前在哪个页面进来的；==防盗链==
3. 体: 请求参数（post才会有请求体）

## 知识点-响应部分

### 1.目标

- 掌握响应行, 常见的响应头,响应体

### 2.讲解

![img](img/tu_15.png)

- 响应部分

  ```
  -- 响应行 
  HTTP/1.1 200 OK
  
  -- 响应头
  Accept-Ranges: bytes
  ETag: W/"143-1557909081579"
  Last-Modified: Wed, 15 May 2019 08:31:21 GMT
  Content-Type: text/html
  Content-Length: 143
  Date: Wed, 15 May 2019 08:35:01 GMT
  
  -- 响应体
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
      Success
  </body>
  </html>
  ```

#### 2.1响应行 

```
HTTP/1.1 200 OK 
```

- 协议/版本

- ==响应状态码==  (记住-背诵下来)

  ![img](img/tu_20.png)

  

  ​	200:正常,成功

  ​	302:重定向：   

  张三（浏览器）向李四（服务器资源一）借钱，告诉张三去找王五； 张三找到王五，借到了钱；

  ​	304:表示客户机缓存的版本是最新的，客户机可以继续使用它，无需到服务器请求. 读取缓存  

  ​	404:客户端错误(一般是路径写错了,没有这个资源)

  ​	500:服务器内部错误---代码写错了！

- 响应码的描述

  

#### 2.2响应头

响应头以key:vaue存在, 可能多个value情况. ==服务器指示浏览器去干什么,去配置什么.==

- **Location**: [http://www.it315.org/index.jsp指示新的资源的位置](http://www.it315.org/index.jsp%E6%8C%87%E7%A4%BA%E6%96%B0%E7%9A%84%E8%B5%84%E6%BA%90%E7%9A%84%E4%BD%8D%E7%BD%AE),通常和状态,码302一起使用，完成请求重定向
- **Content-Type**: text/html; charset=UTF-8; 设置服务器发送的内容的MIME类型,文件下载时候   

- **Refresh**: 5;url=http://www.baidu.com 指示客户端刷新频率。单位是秒  eg: 告诉浏览器5s之后跳转到百度

- **Content-Disposition**: attachment; filename=a.jpg  指示客户端(浏览器)下载文件 
- Content-Length:80 告诉浏览器正文的长度
- Server:apachetomcat 服务器的类型
- Content-Encoding: gzip服务器发送的数据采用的编码类型
- Set-Cookie:SS=Q0=5Lb_nQ;path=/search服务器端发送的Cookie
- Cache-Control: no-cache (1.1)  
- Pragma: no-cache  (1.0)  表示告诉客户端不要使用缓存
- Connection:close/Keep-Alive   
- Date:Tue, 11 Jul 2000 18:23:51 GMT

#### 2.3响应体

​	服务器返回给浏览器的数据

   页面展示内容, 和网页右键查看的源码一样

### 3.小结

1. 行
   1. 响应状态码
      1. 200 正常返回
      2. 302  重定向
      3. 304  浏览器读取自身缓存即可
      4. 404  客户端错误，请求的地址不存在
      5. 500   服务器内部错误--代码写错了
2. 头： 服务器告诉浏览器，让浏览器去做设么
   1. Location
   2. Content-Type
   3. Refresh
   4. Content-Disposition
3. 体: 就是服务器响应的数据



# 第四章-Servlet入门

## 知识点-Servlet概述

### 1.目标

- 知道什么是Servlet,Servlet作用

### 2.讲解

#### 2.1什么是Servlet

​	Servlet ==运行在服务端(tomcat)==的Java程序，是sun公司提供一套规范. 就是动态资源

   就是一个java类。

#### 2.2Servlet作用

​	用来处理客户端请求、响应给浏览器的动态资源。但servlet的实质就是java代码，通过java的API动态的向客户端输出内容

#### 2.3 servlet与普通的java程序的区别

1. 必须实现Servlet接口
2. 必须在servlet容器（服务器 tomcat）中运行
3. servlet程序可以接收用户请求参数以及向浏览器输出数据

### 3.小结

1. servlet 就是一个java类，实现一个Servlet接口
2. 需要运行在sevlet容器中（tomcat）
3. 作用：获取请求参数、做出响应结果



## 实操-使用IDEA创建web工程、配置tomcat

### 1.目标

- 能够在IDEA配置tomcat 、创建web工程

### 2.讲解

#### 2.1配置tomcat

我们要将idea和tomcat集成到一起，可以通过idea就控制tomcat的启动和关闭。

 步骤截图如下：

![1596942634253](img/1596942634253.png)

![1596942681553](img/1596942681553.png)



![1596942788285](img/1596942788285.png)





#### 2.2创建JavaWeb工程

##### 2.2.1web工程创建

![1596894768411](img/1596894768411.png)



![1596894840333](img/1596894840333.png)



![1596895132328](img/1596895132328.png)

web模块的目录结构：

![1596895327587](img/1596895327587.png)

==**手动生成atifacts**==

IDEA中部署到tomcat的web项目，格式要求是![1596895434311](img/1596895434311.png)

默认创建好web模块后，会自动生成对应的war explodes包，也可以自己手动在工程结构的Atifacts 菜单创建：

![1596895747858](img/1596895747858.png)

选择你的目标模块：

![1596895642834](img/1596895642834.png)

生成了对应的 Atifacts：

![1596895694881](img/1596895694881.png)





热加载方式配置：

![1599451203298](img/1599451203298.png)



##### 2.2.2 发布

![1596895925739](img/1596895925739.png)

部署添加刚才的模块的Atifacts：

![1596896022123](img/1596896022123.png)



![1596896197059](img/1596896197059.png)

**启动tomcat服务器**：

![1596896324507](img/1596896324507.png)

**打开浏览器测试：**

输入： http://localhost:8080/day24/     ，当然前提是你按上面的步骤把 tomcat的菜单`Deployment--》Application context`改成了  /day24 了。

**默认会访问项目的 ==web==目录的index.jsp 或者 index.html 文件。你可以删除index.jsp（后面才学），创建一个index.html文件。**

![1596896573107](img/1596896573107.png)





### 3.小结

1. 配置tomcat(选择local)
2. 创建javaweb项目,选择Java Enterprise

![1557912694615](img/1557912694615.png) 

3. 发布

> 先看笔记 配置发布一下, 如果看笔记配置不出来, 看视频 ( 自己操作<=30分钟), 问同桌, 助教, 老师

## 案例-Servlet入门案例

### 1.需求

​	在IDEA编写Servlet,发布到Tomcat. 在浏览器输入路径请求, IDEA控制台打印Hello... 

### 2.分析

#### 2.1配置文件方式实现

1. 创建一个模块
2. 编写servlet类，需要实现一个接口 Servlet接口
3. 在web.xml文件中进行配置路径

#### 2.2注解方式实现

1. 创建一个模块
2. 编写一个类，需要实现一个接口 Servlet接口
3. 类添加一个注解  @WebServlet("路径")

### 3.实现  

#### 3.1配置文件方式实现  

- 创建一个类实现Servlet接口

```java
package com.itheima.a_servlet;

import javax.servlet.*;
import java.io.IOException;

/**
 * 1. 编写一个类实现Servlet接口、在service中写逻辑
 * 2. 在web.xml中配置
 */
public class ServletDemo01 implements Servlet {

    /**
     * 服务方法，接收请求、处理响应的方法
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("ServletDemo01--Hello...");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }



    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```



- web.xml配置（该文件在web/WEB-INF 文件夹下）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           version="2.5">

    <servlet>
        <!--名字，可以随便写-->
        <servlet-name>ServletDemo01</servlet-name>
        <!--配置servlet类的全限定名（包名.类名）-->
        <servlet-class>com.itheima.a_servlet.ServletDemo01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>ServletDemo01</servlet-name>
        <!--配置servlet资源的访问路径
            从此之后访问  localhost:8080/项目名/demo01就可以访问
            com.itheima.a_servlet.ServletDemo01的service方法
        -->
        <url-pattern>/demo01</url-pattern>
    </servlet-mapping>

</web-app>

```



#### 3.2注解方式实现

- 创建一个类实现Servlet接口, 添加注解

```java
package com.itheima.a_servlet;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;


/**
 * 1. 编写一个类实现Servlet接口、在service中写逻辑
 * 2. 添加注解即可  @WebServlet("/demo02")
 */
@WebServlet("/demo02")
public class ServletDemo02 implements Servlet {

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("ServletDemo02--Hello...");
    }


    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }



    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

配置tomcat服务器启动测试（配置过程之前已经展示，这里不再重复）

浏览器地址栏输入：http://localhost:8080/day24/demo02



### 4.小结

#### 4.1 如果出现实现Servlet报错

- 检查当前的工程是否依赖了Tomcat

![1555904633352](img/1555904633352.png)

- 如果没有依赖, 依赖tomcat

![1555905478996](img/1555905478996.png)

![1555905493524](img/1555905493524.png) 

#### 4.2配置文件方式与注解方式比较

​	注解方式简化的javaweb代码开发，可以省略web.xml配置文件. 

​	但是==配置文件方式必须掌握的==(后面在框架或者大项目里面会使用到的)  

#### 4.3步骤回顾

- 文件方式
  - 写一个类，实现 Servlet接口
  - 在web.xml中配置
- 注解方式
  - 写一个类，实现 Servlet接口
  - 添加注解： @WebServlet("/demo02")





## 知识点-入门案例原理和路径

### 1.目标

- 掌握Servlet执行原理和Servlet路径的配置url-pattern

### 2.讲解

#### 2.1Servlet执行原理

![1599453762681](img/1599453762681.png)



![img](img/tu_31.png)



通过上述流程图我们重点需要掌握如下几个点：

- Servlet对象是由服务器创建
- request与response对象也是由tomcat服务器创建
- service()方法也是服务器调用的

#### 2.3Servlet路径的配置url-pattern【重要】

url-pattern配置方式共有三种: 

- 完全路径匹配:  ==以 / 开始==.      注: 访问的路径不能多一个字母也不能少一个

```
例如: 配置了/demo01  请求的时候必须是: /demo01  
```

- 目录匹配"==以 / 开始需要以 * 结束==.    注: Servlet里面用的 不多, 但是过滤器里面通常就使用目录匹配 

```
例如:  配置 /* 访问/a, /aa, /aaa; 配置 /aa/*  访问 /aa/b , /aa/cc,/aa/bb/ccc
```

- 扩展名匹配，以`*`开头（文件的后缀名匹配）==不可以写`*.jsp,*.html`==

```
例如:  *.action;  访问: aa.action, bb.action, c.action;   
重要： 错误写法: /*.do, 不可以写*.jsp,*.html	
```

示例：

```java
//@WebServlet("/demo03")    // 方式一：完全路径匹配:  以/开始    注: 访问的路径不能多一个字母也不能少一个
//@WebServlet("/*")         // 方式二：目录匹配"以 / 开始需要以 * 结束"；      可以匹配  /a 、 /b 、 /a/b/c
@WebServlet("*.do")         // 方式三：后缀（扩展名）匹配 ； 可以匹配 a.do、c.do、hello.do
public class ServletDemo03 implements Servlet {

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println("ServletDemo03-Hello...");
    }
}
```



==注意的地方(了解)==:

- 一个路径只能对应一个servlet, 但是一个servlet可以有多个路径 

- tomcat获得匹配路径时，优先级顺序：完全路径匹配> 目录匹配 > 扩展名匹配 

### 3.小结

1. 路径
   1. 完全路径： /demo01
   
   2. 目录匹配： /开始,*结束：  
   
      1. ```
         /*
         /aa/*
         ```
   
   3. 扩展名匹配
   
      1. ```
         *.do
         *.aaa
         ```
   
         



# 第五章-Servlet进阶

## 知识点-Servlet的生命周期【理解】

### 1.目标

- 掌握Servlet的生命周期

### 2.讲解

#### 2.1生命周期概述

​	一个对象从创建到销毁的过程

#### 2.2Servlet生命周期方法

​	servlet从创建到销毁的过程

​	出生：（初始化）用户第一次访问时执行。

​	活着：（服务）应用活着。每次访问都会执行。

​	死亡：（销毁）应用关闭。

​	serrvlet生命周期方法:

​		init

​		service

​		destroy

#### 2.3Servlet生命周期描述

1. 常规

   ​	默认情况下, 第一次请求会调用init()方法 进行初始化【调用一次】

   ​    任何一次请求, 都会调用service()方法进行处理请求

   ​	服务器正常关闭/项目从服务器移除, 会调用destory()方法进行销毁

2. 扩展点

   ​	==Servlet是单例多线程的==. ==一般情况下, 不要在Servlet里面使用全局(成员)变量, 可能会导致线程不安全==.

   ​	单例: 只有一个Servlet对象. (init()调用一次, 生一次)

   ​	多线程: 服务器会针对每次请求, 开启一个线程调用service()处理这个请求



#### 2.4.ServletConfig

​	Servlet的配置对象, 可以使用用ServletConfig来获得Servlet的初始化参数, 在SpringMVC里面会遇到

- 先在配置文件里面配置初始化参数

![1537837644263](img/1537837644263.png)

- 在init方法里面可以通过akey获得aaa

![1537837616004](img/1537837616004.png)

#### 2.5启动项

​	Servlet默认情况下什么时候创建? 默认情况下是第一次请求的时候.   

​	如果我想让Servlet提前创建(服务器器的时候), 这个时候就可以使用启动项  ==在后面的SpringMVC里面会遇到==

![1555981767256](img/1555981767256.png)



**代码：**



```java
package com.itheima.a_servlet;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/demo04")
public class ServletDemo04 implements Servlet {




    /**
     * 初始化： 第一次请求时执行，仅仅一次
     * @param servletConfig
     * @throws ServletException
     */
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("ServletDemo04-init...");
    }


    /**
     * 服务：每次请求都会执行
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("ServletDemo04-service...");

    }

    /**
     * 销毁：服务器关闭时执行一次
     */
    @Override
    public void destroy() {
        System.out.println("ServletDemo04-destroy...");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }



    @Override
    public String getServletInfo() {
        return null;
    }


}

```

web.xml

```xml
<!--配置/demo05-->
    <servlet>
        <servlet-name>ServletDemo05</servlet-name>
        <servlet-class>com.itheima.a_servlet.ServletDemo05</servlet-class>

        <!--配置servlet初始化参数-->
        <init-param>
            <param-name>akey</param-name>
            <param-value>avalue</param-value>
        </init-param>

        <!--启动项配置：修改默认初始化行为
            0  1  2 ...   在服务器启动时执行init方法
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>ServletDemo05</servlet-name>
        <url-pattern>/demo05</url-pattern>
    </servlet-mapping>
```







### 3.小结

1. 生命周期
   1. init： 初始化，默认情况下，第一次请求时执行
   2. service：每次请求都执行
   3. destroy：销毁，服务器关闭时执行
2. 初始化参数、启动项
   1. ![1599462832332](img/1599462832332.png)



## 知识点-Servlet体系结构

### 1.目标

- 掌握Servlet的继承关系,能够使用IDEA直接创建Servlet

### 2.讲解

![img](img/tu_30.png)



- **Servlet接口**

  ​	前面我们已经学会创建一个类实现sevlet接口的方式开发Servlet程序，实现Servlet接口的时候，我们必须实现接口的所有方法。但是，在servlet中，真正执行程序逻辑的是service，对于servlet的初始化和销毁，由服务器调用执行，开发者本身不需要关心。因此，有没有一种更加简洁的方式来开发servlet程序呢？

我们先来查阅API查看Servlet接口：

>Servlet接口定义所有 servlet 都必须实现的方法。  
>
>servlet 是运行在 Web 服务器中的小型 Java 程序。servlet 通常通过 HTTP（超文本传输协议）接收和响应来自 Web  客户端的请求。  
>
>要实现此接口，可以编写一个扩展 `javax.servlet.GenericServlet` 的一般  servlet，或者编写一个扩展 `javax.servlet.http.HttpServlet` 的 HTTP servlet。  
>
>此接口定义了初始化 servlet 的方法、为请求提供服务的方法和从服务器移除 servlet 的方法。这些方法称为生命周期方法，它们是按以下顺序调用的：   
>
>1. 构造 servlet，然后使用 `init` 方法将其初始化。  
>2. 处理来自客户端的对 `service` 方法的所有调用。  
>3. 从服务中取出 servlet，然后使用 `destroy` 方法销毁它，最后进行垃圾回收并终止它。 
>
>除了生命周期方法之外，此接口还提供了 `getServletConfig` 方法和  `getServletInfo` 方法，servlet 可使用前一种方法获得任何启动信息，而后一种方法允许 servlet  返回有关其自身的基本信息，比如作者、版本和版权。 

​	由上图可知在servlet接口规范下，官方推荐使用继承的方式，继承GenericServlet 或者HttpServlet来实现接口，那么我们接下来再去查看一下这两个类的API：

- **GenericServlet 类**

>定义一般的、与协议无关的 servlet。要编写用于 Web 上的 HTTP servlet，请改为扩展 [`javax.servlet.http.HttpServlet`](../javax.servlet.http.HttpServlet.html)
>
>`GenericServlet` 实现 `Servlet` 和  `ServletConfig` 接口。servlet 可以直接扩展  `GenericServlet`，尽管扩展特定于协议的子类（比如 `HttpServlet`）更为常见。  
>
>`GenericServlet` 使编写 servlet 变得更容易。它提供生命周期方法 `init` 和  `destroy` 的简单版本，以及 `ServletConfig`  接口中的方法的简单版本。`GenericServlet` 还实现 `log` 方法，在  `ServletContext` 接口中对此进行了声明。  
>
>要编写一般的 servlet，只需重写抽象 `service` 方法即可。 

​	阅读上图API可知，GenericServlet 是一个类，它简化了servlet的开发，已经提供好了一些servlet接口所需的方法，我们开发者只需要重写service方法即可

我们来使用GenericServlet 创建servlet：

1. 创建一个类
2. 继承GenericServlet
3. 重写service方法

```java
@WebServlet("/demo")
public class ServletDemo extends GenericServlet {
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        // 逻辑处理
        System.out.println("demo-service");
    }
}
```

​	虽然，GenericServlet已经简化了servlet开发，但是==我们平时开发程序需要按照一种互联网传输数据的协议来开发程序——http协议，因此，sun公司又专门提供了**HttpServlet**，来适配这种协议下的开发==。

- ==**HttpServlet**==

>提供将要被子类化以创建适用于 Web 站点的 HTTP servlet 的抽象类。`HttpServlet`
>
>- `doGet`，如果 servlet 支持 HTTP GET 请求  
>- `doPost`，用于 HTTP POST 请求  
>- `doPut`，用于 HTTP PUT 请求  
>- `doDelete`，用于 HTTP DELETE 请求  
>- `init` 和 `destroy`，用于管理 servlet 的生命周期内保存的资源  
>- `getServletInfo`，servlet 使用它提供有关其自身的信息 
>
>几乎没有理由重写 `service` 方法。`service` 通过将标准 HTTP 请求分发给每个 HTTP  请求类型的处理程序方法（上面列出的 `do`*XXX* 方法）来处理它们。  
>
>同样，几乎没有理由重写 `doOptions` 和 `doTrace` 方法。  
>
>servlet 通常运行在多线程服务器上，因此应该意识到 servlet  必须处理并发请求并小心地同步对共享资源的访问。共享资源包括内存数据（比如实例或类变量）和外部对象（比如文件、数据库连接和网络连接）。

阅读上图的API可知，继承HttpServlet，我们需要重写doGet、doPost等方法中一个即可，根据Http不同的请求，我们需要实现相应的方法。

我们来使用HttpServlet创建servlet：	

1. 创建一个类
2. 继承HttpServlet
3. 重写doGet、doPost方法

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo06")
public class ServletDemo06 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("ServletDemo06-doGet...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("ServletDemo06-doPost...");
    }
}

```

​	通过以上两个API阅读，同学们注意一个细节HttpServlet是GenericServlet的子类，它增强了GenericServlet一些功能，因此，在后期使用的时候，我们都是选择继承HttpServlet来开发servlet程序。



![1582769812580](img/1582769812580.png)

### 3.小结

1. 一个接口Servlet、GerricServlet（抽象类）、HttpServlet（直接继承这个类就行了）
2. ![1582768553857](img/1582768553857.png)



# 第六章-ServletContext上下文对象

## 知识点-ServletContext概述

### 1.目标

- 知道什么是ServletContext

### 2.讲解

#### 2.1servletContext概述

​	ServletContext: 是一个对象, 上下文对象. 

​	==服务器tomcat为每一个应用(项目)都创建了一个ServletContext对象==。 ServletContext属于整个应用的，不局限于某个Servlet。 全局管理者. 

​	eg. 我们有一个班， 班主任---ServletContext，同学------每一个Servlet



### 3.小结

1. servletContext：就是上下文对象，一个项目只有一个上下文对象。

## 知识点-ServletContext的功能【掌握】

### 1.目标

- 掌握ServletContext的作用以及API

### 2.路径

- 作为域对象存取数据
- 获得文件mime类型（文件下载）
- 获得全局初始化参数
- 获取web资源路径

### 3.讲解

==ServletContext作用==

​	1. 作为域对象存取数据,让Servlet共享

2. 获得文件mime类型（文件上传和下载）   eg.  a.mp3   b.mp4

   ![1582770225739](img/1582770225739.png)

​	3. 获得全局初始化参数

​	4. 获取web资源路径  



==获得ServletContext对象==
`ServletContext servletContext = getServletContext();`

#### 3.1作为域对象存取值【重点】



![1557992863889](img/1582770386854.png)

1. API

   - setAttribute(String name, Object object) ; 添加数据到ServletContext对象里的的map集合中
   - getAttribute(String name) ;从ServletContext对象的map取数据
   - removeAttribute(String name) ;根据name移除数据

   ==servlet路径就在web目录下第一层==

2. 代码

   - ServletA

     ```java
     package com.itheima.b_servlet;
     
     import javax.servlet.ServletContext;
     import javax.servlet.ServletException;
     import javax.servlet.annotation.WebServlet;
     import javax.servlet.http.HttpServlet;
     import javax.servlet.http.HttpServletRequest;
     import javax.servlet.http.HttpServletResponse;
     import java.io.IOException;
     
     @WebServlet("/servletA")
    public class ServletA extends HttpServlet {
         protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    
         }
     
         protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
             // 存值
             // 1. 获得上下文对象
             ServletContext servletContext = getServletContext();
     
             // 2. 存值
             servletContext.setAttribute("skey", "svalue");
     
             System.out.println("ServletA--setAttribute...");
        }
     }
     
     ```
   
   - ServletB
   
     ```java
     package com.itheima.b_servlet;
     
     import javax.servlet.ServletContext;
     import javax.servlet.ServletException;
     import javax.servlet.annotation.WebServlet;
     import javax.servlet.http.HttpServlet;
     import javax.servlet.http.HttpServletRequest;
     import javax.servlet.http.HttpServletResponse;
     import java.io.IOException;
     
     @WebServlet("/servletB")
     public class ServletB extends HttpServlet {
         protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     
         }
     
         protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     
             System.out.println("ServletB--getAttribute...");
             // 取值
     
             // 获得上下文对象
             ServletContext servletContext = getServletContext();
     
             // 取值
             Object skey = servletContext.getAttribute("skey");
             System.out.println(skey);
     
     
         }
     }
     
     ```
   
   
   

#### 3.2获得文件mime类型

1. API 
   - getMimeType(String file) 
2. 代码

```java
package com.itheima.b_servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/servletC")
public class ServletC extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获得MIME类型

        // 获得上下文对象
        ServletContext servletContext = getServletContext();

        // 调用API
        String mimeType1 = servletContext.getMimeType("a.mp3");
        String mimeType2 = servletContext.getMimeType("b.mp4");
        String mimeType3 = servletContext.getMimeType("c.avi");

        System.out.println(mimeType1);
        System.out.println(mimeType2);
        System.out.println(mimeType3);


        
    }
}

```



#### 3.3.获得全局初始化参数

- **String getInitParameter(String name)** ; //根据配置文件中的key得到value; 

1. 在web.xml配置

![1537841534534](img/1537841534534.png)

2. 通过ServletContext来获得

 ![1537841552654](img/1537841552654.png)

#### 3.4获取web资源路径

1. API

   - String  getRealPath(String path);根据资源名称得到资源的绝对路径（该方法到web目录了）.

   - getResourceAsStream(String path) ;返回指定路径文件的流；参数是getRealPath方法的返回值。

     - ==path参数的资源路径按web目录相对地址填写==

     - 例如以下目录结构的代码形式：

       - servletContext.getResourceAsStream("a.txt")
       - servletContext.getResourceAsStream("WEB-INF/b.jpg")

       ![1582725802381](img/1582725802381.png)

> 注意: filepath:直接从项目的根目录开始写(==servlet就已经在web目录了==)

2. 代码

```java
package com.itheima.b_servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;

@WebServlet("/servletD")
public class ServletD extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑
        // 获取上下文对象
        ServletContext servletContext = getServletContext();

        // 获得a.txt的输入流
        InputStream resourceAsStream = servletContext.getResourceAsStream("a.txt");
        System.out.println(resourceAsStream);

        // 获得b.jpg的路径
        String realPath = servletContext.getRealPath("WEB-INF/b.jpg");
        // D:\007\p102\out\artifacts\day24_servlet_war_exploded\WEB-INF\b.jpg
        System.out.println(realPath);
    }
}

```



### 4.小结

1. 上下文对象的作用

   1. 域对象存取值

      ```java
      setAttribute("", "")
      getAttribute("")
      ```

   2. 获取mime类型：   getMimeType()

   3. 获取全局初始化参数：  getInitParameter("")

   4. 获取资源路径==我们的servlet编译打包部署后就和web目录里面的第一层同一级别==

      1. getRealPath()
      2. getResourceAsStream()

## 案例-统计网站被访问的总次数

### 1.需求

![img](img/tu_3.gif)

- 访问count路径时，仅仅输出一句话
- 访问show路径时，显示你是第几个访问的

### 2.思路分析



![1555987265777](img/1582773510520.png)

- 写2个servlet
- 一个计数
- 一个展示

### 3.代码实现 

- CountServlet

```java
package com.itheima.b_servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/count")
public class CountServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 获得上下文对象
        ServletContext servletContext = getServletContext();


        // 获取前一次的数字

        // 第一次要判断一下
        if (servletContext.getAttribute("count") == null) {

            servletContext.setAttribute("count", 1);
        }else{
            Integer count = Integer.valueOf(servletContext.getAttribute("count").toString());


//            count++;
//            ++count;
            // 再累加并存储进去
            servletContext.setAttribute("count", ++count);
        }



        // 页面打印文字是响应--提前用一下后面的API
        response.getWriter().write("Welcome...");
    }
}

```

- ShowServlet

```java
package com.itheima.b_servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/show")
public class ShowServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 取出显示

        // 获得对象
        ServletContext servletContext = getServletContext();

        // 取出（打印不再强转）
        Object count = servletContext.getAttribute("count");


        // 页面打印文字是响应--提前用一下后面的API
        response.getWriter().write("You are " + count + " accessed people.");
    }
}

```

### 4.小结

1. 2个servlet共享数据

   1. 1个写： 先取出来，判断是否为空，再累加，再写
   2. 1个读，直接读取，然后响应到页面  response.getWriter().write("您是第" + cnt + "位访问的用户！");

2. ```
   //  System.out.println("Hello... ");  仅仅打印在IDEA控制台，不会输出到浏览器页面
   //  只有通过响应对象response才可以输出到浏览器页面
   ```





# 总结

架构

- ​	C/S、B/S

web资源

- 可以被浏览器访问的资源
- 静态、动态



服务器： 电脑+服务器软件



实操： 安装tomcat、启动、关闭、在IDEA中配置tomcat、在IDEA中创建web模块



知识点：

- servlet
  - java类实现了Sevlet接口---》在IDEA中直接new...
  - 生命周期：
    - init：默认是第一次请求时被初始化
    - service：服务方法每次请求都会被访问
    - destroy：销毁
  - 配置servlet的初始化参数
  - 启动项配置
  - ![1599469077871](img/1599469077871.png)

- servletContext： 一个项目一个对象--全局的对象--所有的servlet都可以操作它，共享数据
  - 作为域对象存取值
    - setAttribute
    - getAttribute
    - removeAttribute
  - 获取文件的MIME类型
  - 获取全局初始化参数
  - 获得web资源文件流、路径
- 案例（学习、案例的思路---踩着老师的肩膀）



![1599469575382](img/1599469575382.png)



# 回顾要点

1.  tomcat安装、启动要会
2. IDEA里面配置tomcat
3. IDEA里面创建web项目、部署、访问（默认有一个index.jsp，删掉写一个index.html）
4. 编写servlet（文件、注解）
5. 上下文对象的用法
6. 案例



响应码：

200

302

304

404

500







# 预习要点

request的作用

- 获取参数
- 请求转发
- 乱码处理



response对象

- 重定向
- 操作响应体
- 文件下载（响应头设置）
- 乱码处理


