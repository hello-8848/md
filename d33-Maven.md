---
typora-copy-images-to: img
---

# day33-Maven&Lombok

# 学习目标

- [ ] 能够了解Maven的作用

- [ ] 能够理解Maven仓库的作用

- [ ] 能够理解Maven的坐标概念

- [ ] ==能够掌握Maven的安装==

- [ ] ==能够掌握IDEA配置本地Maven==

- [ ] ==能够使用IDEA创建javase的Maven工程==

- [ ] ==能够使用IDEA创建javaweb的Maven工程==

- [ ] 能够理解依赖范围

- [ ] 了解搭建私服的使用

- [ ] 能够掌握Lombok的使用

  



# 第一章-Maven相关的概念

## 知识点-Maven介绍

### 1.目标

+ 能够了解Maven的作用

### 2.路径

+ 什么是Maven
+ Maven的作用
+ Maven的好处

### 3.讲解

#### 3.1什么是Maven

​	Maven是项目进行模型抽象，充分运用的面向对象的思想，Maven可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。

​	说白了: ==Maven是由Apache开发的一个工具。用来管理java项目(依赖(jar)管理, 项目构建, 分模块开发 ).==

#### 3.2Maven的作用

- 依赖管理: maven对项目的第三方构件（jar包）进行统一管理。向工程中加入jar包不要手工从其它地方拷贝，通过maven定义jar包的坐标，自动从maven仓库中去下载到工程中。  


- 项目构建: maven提供一套对项目生命周期管理的标准，开发人员、和测试人员统一使用maven进行项目构建。项目生命周期管理：编译、测试、打包、部署、运行。
- maven对工程分模块构建，提高开发效率。 (后面Maven高级会涉及)

#### 3.3 Maven的好处

+ 使用普通方式构建项目

![img](img/tu_2.png)

+ 使用Maven构建项目

  ![img](img/tu_3.png)





### 4.小结

1. maven就是一个可以对项目进行依赖管理（jar）、构建的一个工具




## 知识点-Maven仓库和坐标

### 1.目标

+ 能够理解Maven仓库的作用

### 2.路径

1. Maven的仓库
2. Maven的坐标

### 3.讲解

#### 3.1Maven的仓库

| 仓库名称 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| 本地仓库 | 相当于缓存，工程第一次会从远程仓库（互联网）去下载jar 包，将jar包存在本地仓库（在程序员的电脑上）。<br>第二次不需要从远程仓库去下载。先从本地仓库找，如果找不到才会去远程仓库找。 |
| 中央仓库 | 仓库中jar由专业团队（maven团队）统一维护。<br>中央仓库的地址：http://repo1.maven.org/maven2/ |
| 远程仓库 | 在公司内部架设一台私服，其它公司架设一台仓库，对外公开。     |

#### 3.2 Maven的坐标

​	Maven的一个核心的作用就是管理项目的依赖，引入我们所需的各种jar包等。为了能自动化的解析任何一个Java构件，Maven必须将这些Jar包或者其他资源进行唯一标识，这是管理项目的依赖的基础，也就是我们要说的坐标。包括我们自己开发的项目，也是要通过坐标进行唯一标识的，这样才能才其它项目中进行依赖引用。坐标的定义元素如下：

==坐标的作用：定位需要的jar包==

- ==groupId==:项目、组织唯一的标识符，实际对应JAVA的包的结构 (==一般而言写域名倒着写，  alibab.com---》com.alibab==)  
- ==artifactId==: 项目的名称
- **version**：定义项目的当前版本 



==在百度搜索中输入：  mvn druid  可以直接定位搜索到maven中央仓库快速找到坐标。==

例如：要引入druid，只需要在`pom.xml`配置文件中配置引入druid的坐标即可：

```xml
<!--druid连接池-->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>    
  <version>1.0.9</version>  
</dependency>
```

### 4.小结

1. 仓库（存放jar包）
   1. 本地仓库
   2. 远程仓库
   3. 中央仓库
2. 坐标（作用：定位依赖的jar包）
   1. groupId： 组织、集团唯一标志
   2. artifactId： 项目名
   3. version： 版本号



# 第二章-Maven的安装【重要】

## 知识点-Maven的安装

### 1.目标

+ 能够掌握Maven的安装

### 2.路径

1. 下载Maven
2. 安装Maven
3. Maven目录介绍
4. 配置环境变量  
5. 配置本地仓库
6. 测试Maven是否安装成功

### 3.讲解

#### 3.1下载Maven

![](img\2.png)

  http://maven.apache.org/

#### 3.2 安装Maven

将Maven压缩包解压，即安装完毕

解压到没有中文、空格、特殊字符的一个目录（层次不要太深）

![](img\4.png)

#### 3.3 Maven目录介绍

![](img\43.png)

#### 3.4 配置环境变量

+ 进入环境变量（win徽标键+S，搜索）

![img](img/tu_5.png)

+ 配置==MAVEN_HOME==和==Path==

![img](img/tu_6.png)

![img](img/tu_7.png)

#### 3.5 配置本地仓库

##### 3.5.1 可以将资料中的repository.zip解压作为自己的本地仓库目录

可以将资料中的repository.zip解压作为自己的本地仓库目录，里面已经包含了很多的可用的依赖jar包。

==解压后的.m2文件建议放到某个磁盘的根目录，然后可以把本地仓库地址指向该目录的repository目录==



![1600738294162](img/1600738294162.png)

##### 3.5.2 ==配置本地仓库==

在maven的安装目录中conf/ settings.xml文件，在这里配置本地仓库

![1594257600831](img/1594257600831.png)

+ 示例代码

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
 <localRepository>F:\.m2\repository</localRepository>
```

#### 3.6  测试Maven安装成功

打开cmd本地控制台，输入mvn -version

![](img\6.png)

### 4.小结

1. 下载
2. 安装（解压到一个正常的目录）
3. 配置环境变量（MAVEN_HOME、Path）
4. 配置本地仓库：maven根目录/conf/settings.xml配置：`<localRepository>写你要作为本地仓库的一个文件夹路径</localRepository>`
5. 测试： mvn -version

## 知识点-IDEA集成Maven

### 1.目标

+ 能够掌握IDEA配置本地Maven

### 2.路径

1. 在IDEA配置Maven
2. 配置默认的Maven环境

### 3.讲解

#### 3.1配置Maven

+ 配置Maven

![1600739220859](img/1600739220859.png)

+ 配置参数(创建工程不需要联网,解决创建慢的问题)  ==-DarchetypeCatalog=internal==

![img](img/tu_8.png)



#### 3.2. 配置默认Maven环境

​	每次创建Maven工程的时候，总是需要重新选择Maven配置信息，那是因为默认的Maven环境不是我们当前的maven环境，所以需要配置。配置流程如下图：

- 选择默认的配置

![1594259243358](img/1594259243358.png)



- 进入配置

![1594259305791](img/1594259305791.png)

- 配置参数(创建工程不需要联网,解决创建慢的问题)  -DarchetypeCatalog=internal

![img](img/tu_8.png)

- ==重启IDEA, 就可以生效了==



#### 3.3 配置maven镜像为aliyun，加快速度

==**配置maven的阿里云镜像**==：

![1598234379231](img/1598234379231.png)

- 1. 在你的IDEA中配置的==settings.xml==中增加配置
- ![1596452001893](img/1596452001893.png)

- 2. 修改IDEA自带的maven插件的镜像地址：

  - 你的IDEA安装目录，比如：D:\installDirectory\IntelliJ IDEA 2019.2.2\plugins\maven\lib\maven3\conf里面的==settings.xml==  增加配置

- 在==mirrors标签==里面增加：

```xml
<mirror>
    <id>alimaven</id>
    <mirrorOf>central</mirrorOf>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>

<mirror>  
    <id>ui</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Human Readable Name for this Mirror.</name>  
    <url>http://uk.maven.org/maven2/</url>  
</mirror>  

<mirror>
    <id>mirrorId</id>
    <mirrorOf>repositoryId</mirrorOf>
    <name>SlaveName</name>
    <url>http://search.maven.org</url>

</mirror>
<mirror>
    <id>mvnrepositoryMID</id>
    <mirrorOf>mvnrepositoryRID</mirrorOf>
    <name>mvnrepository</name>
    <url>http://mvnrepository.com</url>

</mirror>



<mirror>  
    <id>repo2</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Human Readable Name for this Mirror.</name>  
    <url>http://repo2.maven.org/maven2/</url>  
</mirror>  
<mirror>  
    <id>net-cn</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Human Readable Name for this Mirror.</name>  
    <url>http://maven.NET.cn/content/groups/public/</url>   
</mirror>  

<mirror>  
    <id>ibiblio</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Human Readable Name for this Mirror.</name>  
    <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>  
</mirror>  
<mirror>  
    <id>jboss-public-repository-group</id>  
    <mirrorOf>central</mirrorOf>  
    <name>JBoss Public Repository Group</name>  
    <url>http://repository.jboss.org/nexus/content/groups/public</url>  
</mirror>
```



### 4.小结

+ IDEA集成maven配置
  + File--》setting--》Build...---》Build Tools---》Maven
    + maven根目录
    + users settings配置文件： 指向maven的conf/settings.xml
    + 本地仓库路径
  + 默认配置
    + File ---》other settings--》......

# 第三章-使用IDEA创建Maven工程【重要】

## 实操-创建javase工程

### 1.目标

+ 能够使用IDEA创建javase的Maven工程

### 2.路径

1. 创建java工程
2. java工程目录结构
3. 编写Hello World！

### 3.讲解

#### 3.1创建java工程

![1600739967036](img/1600739967036.png)



![1600740075521](img/1600740075521.png)



![1600740153158](img/1600740153158.png)



![1600740173135](img/1600740173135.png)













#### 3.2 java工程目录结构【重要的点】

+ 需要main/java文件夹变成 源码的目录(存放java源码)

![1535073660314](img/1535073660314.png)



+ 需要test/java文件夹变成 测试源码的目录(存放单元测试)

![1535073720297](img/1535073720297.png)

+ 创建resources目录, 变成资源的目录

  ![1535073892837](img/1535073892837.png)

+ ==整体结构==

+ ![1600740712977](img/1600740712977.png)

  





#### 3.3 编写Hello World！



![1600740869467](img/1600740869467.png)

### 4.小结

1. JavaSe工程的骨架

![image-20191224102043184](img/image-20191224102043184.png) 

2. 项目的结构

![image-20191224102316809](img/image-20191224102316809.png) 

3. pom.xml指定编码

```xml

  <properties>
    <!--指定编码为UTF-8-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--源代码编译、编译后的都是jdk1.8-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>


```



## 实操-创建javaweb工程

### 1.目标

+ 能够使用IDEA创建javaweb的Maven工程

### 2.路径

1. 创建javaweb工程
2. 发布javaweb工程
3. 浏览器访问效果

### 3.讲解

#### 3.1 创建javaweb工程

- 创建javaweb工程与创建javase工程类似，但在选择Maven骨架时，选择==maven-archetype-webapp==即可：

![1600741939193](img/1600741939193.png)

- 创建好的javaweb工程如下：

![1600742525016](img/1600742525016.png)

- 所以，要手动创建一个java目录用于编写java代码：

![](img\17.png)

- 还要将java目录添加为Source Root：

![](img\18.png)

#### 3.2 发布javaweb工程

![](img\37.png)

#### 3.3 浏览器访问效果

![](img\21.png)

![1598237232040](img/1598237232040.png)



### 4.小结

1. 选择骨架选择webapp 

![image-20191224105135578](img/image-20191224105135578.png) 

2. pom.xml

![image-20191224105159688](img/image-20191224105159688.png) 



3. web工程结构 

![image-20191224105359670](img/image-20191224105359670.png) 



==注意事项==在pom.xml里面要打包成war形式。

## 实操-不使用骨架创建工程

### 1.目标

​	上面是使用骨架来创建工程的，如果不使用骨架，怎样创建工程呢?

### 2.路径

1. 不使用骨架创建javase项目
2. 不使用骨架创建javaweb项目

### 3.讲解

#### 3.1.不使用骨架创建javase项目

- 第一步

![1535183606179](img/1535183606179.png)

- 第二步

![1535183637210](img/1535183637210.png)

- 第三步

![1535183659568](img/1535183659568.png)

- 第四步

![1535183718460](img/1535183718460.png)

#### 3.2.不使用骨架创建javaweb项目

- 第一步

![1535183799998](img/1535183799998.png)

- 第二步

![1535183825097](img/1535183825097.png)

- 第三步

![1535183856928](img/1535183856928.png)

- 第四步, 在pom文件里面添加标签packaging

  ![1535183911294](img/1535183911294.png)

  

- 第五步

![1535183960773](img/1535183960773.png)



- 第六步

![1535184028659](img/1535184028659.png)

### 4.小结

不使用骨架更快的, 单独有视频和笔记

 ![1557886823099](img/1557886823099.png)

# 第四章-Maven的常用命令

## 知识点-Maven的常用命令

### 1.目标

+ 掌握Maven的常用命令

### 2.路径

1. clean命令
2. compile命令
3. test命令
4. package命令  
5. install命令
6. deploy命令（发布到远程仓库）

### 3.讲解

==在pom.xml所在目录执行mvn相关命令==

或者在IDEA中右键执行

#### 3.1clean命令

==清除编译产生的target文件夹内容==，可以配合相应命令一起使用，如mvn clean package， mvn clean test

![1535077199437](img/1535077199437.png)



![](img\22.png)

#### 3.2 compile命令

该命令可以对src/main/java目录的下的代码进行编译

![1535077363500](img/1535077363500.png)



![](img\32.png)

#### 3.3 test命令

==测试命令,或执行src/test/java/下所有junit的测试用例==

![1535077528168](img/1535077528168.png)

- 在src/test/java下创建测试类DemoTest

![](img\33.png)

- 执行test命令测试

![](img\34.png)

- 控制台显示测试结果

![](img\35.png)

#### 3.4 package命令

mvn package，打包项目

+ 如果是JavaSe的项目,打包成jar包
+ 如果是JavaWeb的项目,打包成war包

![1535077767079](img/1535077767079.png)



![](img\25.png)

打包后的项目会在target目录下找到

![](img\26.png)





#### 3.5 install命令

==mvn install，打包后将其安装在本地仓库，要求是jar包==。

![1535078133186](img/1535078133186.png)

![](img\23.png)

安装完毕后，在本地仓库中可以找到itheima_javase_demo的信息

![](img\24.png)



### 4.小结

1. 命令

   1. clean： 清除target目录内容
   2. compile：编译代码到target目录
   3. test： 执行src/test/java中的代码（单元测试代码）
   4. package： 打包，把项目打包（jar、war）
   5. install： 把打包好的jar包安装到本地仓库
   6. 后面的阶段执行时，都会执行前面的阶段。

   

# 第五章-依赖管理和插件

## 知识点-依赖管理

### 1.目标

+ 能够掌握依赖引入的配置方式

### 2.路径

1. 导入依赖
2. 导入依赖练习
3. 依赖范围

### 3.讲解

#### 3.1导入依赖

​	导入依赖坐标，无需手动导入jar包就可以引入jar。在pom.xml中使用`<dependency>`标签引入依赖。

​	做项目/工作里面 都有整套的依赖的, 不需要背诵的. 

​	去Maven官网找, 赋值,粘贴.  `http://mvnrepository.com/`

​	==比如要依赖druid，那么可以在搜索引擎输入 druid mvn 回车==

##### 3.1.1 导入junit的依赖

- 导入junit坐标依赖

```xml
 <!--依赖的jar包-->
  <dependencies>
    <!--每一个依赖-->
    <dependency>
      <!--坐标-->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <!--依赖范围-->
      <scope>test</scope>
    </dependency>
      

  </dependencies>
```

- 进行单元测试（==单元测试依赖后只能在test/java里面用，不能在main/java用==）

```java
import org.junit.Test;

public class DemoTest {
    @Test
    public void test1(){
        System.out.println("test running...");
    }
}
```

##### 3.1.2 导入servlet的依赖

- 创建Servlet，但是发现报错，原因是没有导入Servlet的坐标依赖

![](img\38.png)

- 导入Servlet的坐标依赖

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

- 原有报错的Servlet恢复正常

![](img\39.png)

#### 3.2 依赖范围

依赖范围可以用在`<scop>`标签，它的值有：

![](img\27.png)





- compile 默认值；编译、测试、运行，A在编译时依赖B，并且在测试和运行时也依赖

  例如：strus-core、spring-beans, C3P0,Druid。打到war包或jar包


- ==provided 编译、和测试有效==，A在编译和测试时需要B

   例如：==servlet-api==就是编译和测试有用，在运行时不用（tomcat容器已提供）

   不会打到war

- ==runtime：测试、运行有效==, 

   例如：==jdbc驱动包== ，在开发代码中针对java的jdbc接口开发，编译不用

   在运行和测试时需要通过jdbc驱动包（mysql驱动）连接数据库，需要的

   会打到war

- ==test：只是测试有效，只在单元测试类中用==

   例如：==junit==

   不会打到war

- 按照依赖强度，由强到弱来排序：(理解)

  compile> provided> runtime> test

### 4.小结

1. 依赖引入
   
   1. 在pom.xml中的dependencies引入一个一个dependencie标签
   
   2. ```xml
      <!--依赖的jar包-->
        <dependencies>
          <!--每一个依赖-->
          <dependency>
            <!--坐标-->
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <!--依赖范围-->
            <scope>test</scope>
          </dependency>
      
      
        </dependencies>
      ```
   
2. 依赖范围

   1. compile： 默认的不用管
   2. test： 仅仅在单元测试期有效： junit
   3. provided： 不在运行期生效：servlet-api
   4. runtime：在编译期不生效：jdbc驱动



## 知识点-Maven插件

### 1.目标

​	Maven是一个核心引擎，提供了基本的项目处理能力和建设过程的管理，以及一系列的插件是用来执行实际建设任务。maven插件可以完成一些特定的功能。例如，集成jdk插件可以方便的修改项目的编译环境；集成tomcat插件后，无需安装tomcat服务器就可以运行tomcat进行项目的发布与测试。

==在pom.xml中通过plugin标签引入maven的功能插件。plugin又在plugins标签里面，plugins标签又在build里面。build和dependencies同级别。==

![1598241176956](img/1598241176956.png)

### 2.路径

1.  JDK编译版本的插件
2. Tomcat的插件

### 3.讲解

#### 3.1 JDK编译版本的插件【了解】

```xml
<build>
    <plugins>
      <!--jdk编译插件-->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>utf-8</encoding>
        </configuration>
      </plugin>
    </plugins>
  </build>


该插件可以使用以下属性替代：
<properties>
    <!--指定编码为UTF-8-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--源代码编译、编译后的都是jdk1.8-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

![1594267186161](img/1594267186161.png)

#### 3.2 Tomcat7服务端的插件

- 添加tomcat7插件    

```xml
<plugins>
    <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <configuration>
            <!-- 指定端口 -->
            <port>8080</port>
            <!-- 部署后的项目的路径 -->
            <path>/</path>
        </configuration>
    </plugin>
</plugins>
```

> 注意: Maven的中央仓库中只有Tomcat7.X版本的插件，而之前我们使用的是8.X的版本，如果想使Tomcat8.X的插件可以去其他第三方仓库进行寻找，或者使用IDEA集成外部Tomcat8极其以上版本，进行项目的发布。

其中的path和下面的配置一样：

![1594267301834](img/1594267301834.png)

### 4.小结

插件如何配置，在pom.xml中：

```xml
<build>
	<plugins>
    	<plugin>
        	
        </plugin>
    </plugins>
</build>
```



 

# 第六章maven 私服【了解】 

## 知识点- 私服搭建

### 1.目标

- [ ] 了解Maven私服搭建

### 2.路径

1. Maven私服概述
2. 搭建私服环境 

### 3.讲解

#### 3.1Maven私服概述

​	==公司在自己的局域网内搭建自己的远程仓库服务器，称为私服==， 私服服务器即是公司内部的 maven 远程仓库， 每个员工的电脑上安装 maven 软件并且连接私服服务器，员工将自己开发的项目打成 jar 并发布到私服服务器，其它项目组从私服服务器下载所依赖的构件（jar）。私服还充当一个代理服务器，当私服上没有 jar 包会从互联网中央仓库自动下载.



#### 3.2.搭建私服环境 

##### 3.2.1下载 nexus

​	Nexus 是 Maven 仓库管理器， 通过 nexus 可以搭建 maven 仓库，同时 nexus 还提供强大的仓库管理功能，构件搜索功能等。
​	 下载地址： http://www.sonatype.org/nexus/archived/ 

​	下载： nexus-2.12.0-01-bundle.zip 

![1536500146465](img/1536500146465.png)

##### 3.2.2安装 nexus 

解压 nexus-2.12.0-01-bundle.zip，进入 bin 目录： 

![1536500225057](img/1536500225057.png)

以==管理员权限==运行命令行,进入 ==bin== 目录，执行 ==nexus.bat install==

![1536500276373](img/1536500276373.png)

安装成功在服务中查看有 nexus 服务： 

![1536500298930](img/1536500298930.png)

##### 3.2.3卸载nexus 

cmd 进入 nexus 的 bin 目录，执行： ==nexus.bat uninstall== 

![1536500348981](img/1536500348981.png)

##### 3.2.4启动 nexus 

+ 方式一

  cmd 进入 bin 目录，执行 nexus.bat start 

+ 方式二

  直接启动 nexus 服务 

  ![1536500419782](img/1536500419782.png)

  

##### 3.2.5登录

+ 访问: http://localhost:8081/nexus/ 

  > 查看 nexus 的配置文件 conf/nexus.properties ,里面有端口号

![1536500498947](img/1536500498947.png)

+ 点击右上角的 Log in，输入账号和密码 登陆 (账号admin,密码admin123 )

![1536500542616](img/1536500542616.png)

+ 登录成功

![1536500555932](img/1536500555932.png)

##### 3.2.6仓库类型 

![1536500594312](img/1536500594312.png)

nexus 的仓库有 4 种类型： 

![1536500615935](img/1536500615935.png)

1. hosted，宿主仓库， 部署自己的 jar 到这个类型的仓库，包括 release 和 snapshot 两部
   分， Releases 公司内部发布版本仓库、 Snapshots 公司内部测试版本仓库
2. proxy，代理仓库， 用于代理远程的公共仓库，如 maven 中央仓库，用户连接私服，私
   服自动去中央仓库下载 jar 包或者插件。
3. group，仓库组，用来合并多个 hosted/proxy 仓库，通常我们配置自己的 maven 连接仓
   库组。
4. virtual(虚拟)：兼容 Maven1 版本的 jar 或者插件 

### 4.小结

1. 对着文档搭建一下就OK

2. 安装的时候需要以`管理员` 身份
3. 路径不要有中文



## 知识点-Maven私服的使用

### 1.目标

- [ ] 了解Maven私服的使用

### 2.路径

1. 将项目发布到私服 
2. 从私服下载 jar 包 

### 3.讲解

#### 3.1.将项目发布到私服

##### 3.1.1需求

​	企业中多个团队协作开发通常会将一些公用的组件、开发模块等发布到私服供其它团队或模块开发人员使用。
​	本例子假设多团队分别开发 .  某个团队开发完在一个工程， 将工程对应的额jar包发布到私服供 其它团队使用.

##### 3.1.2配置

​	==第一步==： 需要在客户端即部署 一个工程的电脑上配置 maven环境，并修改 ==settings.xml==文件(Maven配置文件)， ==配置连接私服的用户和密码==。此用户名和密码用于私服校验，因为私服需要知道上传的账号和密码是否和私服中的账号和密码一致 (配置到`<servers>`标签下)

```xml
<server>
    <id>releases</id>
    <username>admin</username>
    <password>admin123</password>
</server>
<server>
    <id>snapshots</id>
    <username>admin</username>
    <password>admin123</password>
</server>
```

releases: 连接发布版本项目仓库
snapshots: 连接测试版本项目仓库 

![1536500938592](img/1536500938592.png)

​	

​	第二步： ==在需要发布配置项目 pom.xml . 配置私服仓库的地址==，本公司的自己的 jar 包会上传到私服的宿主仓库，根据工程的版本号决定上传到哪个宿主仓库，如果版本为 release 则上传到私服的 release 仓库，如果版本为snapshot 则上传到私服的 snapshot 仓库 .

```xml
<distributionManagement>
    <repository>
        <id>releases</id>
        <url>http://localhost:8081/nexus/content/repositories/releases/</url>
    </repository>
    <snapshotRepository>
        <id>snapshots</id>
        <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

+ 注意： ==pom.xml 这里`<id>` 和 settings.xml 配置 `<id>` 对应！== 

##### 3.1.3测试

1、 首先启动 nexus
2、 对 工程执行 deploy 命令 

​	根据本项目pom.xml中version定义决定发布到哪个仓库，如果version定义为snapshot，执行 deploy后查看 nexus 的 snapshot仓库， 如果 version定义为 release则项目将发布到 nexus的 release 仓库，本项目将发布到 snapshot 仓库： 

![image-20191222211914094](img/image-20191222211914094.png)

#### 3.2从私服下载 jar 包 

##### 3.2.1需求

​	没有配置 nexus 之前，如果本地仓库没有，去中央仓库下载，通常在企业中会在局域网内部署一台私服服务器， 有了私服本地项目首先去本地仓库找 jar，如果没有找到则连接私服从私服下载 jar 包，如果私服没有 jar 包私服同时作为代理服务器从中央仓库下载 jar 包，这样做的好处是一方面由私服对公司项目的依赖 jar 包统一管理，一方面提高下载速度， 项目连接私服下载 jar 包的速度要比项目连接中央仓库的速度快的多。 

本例子测试从私服下载 工程 jar 包。 

##### 3.2.2在 setting.xml 中配置仓库 

​	在==客户端的 setting.xml 中配置私服的仓库==，由于 setting.xml 中没有 repositories 的配置标签需要使用 profile 定义仓库。(==配置在`<profiles>`标签下==)

```xml
<profile>
    <!--profile 的 id-->
    <id>dev</id>
    <repositories>
        <repository>
        <!--仓库 id， repositories 可以配置多个仓库，保证 id 不重复-->
        <id>nexus</id>
        <!--仓库地址，即 nexus 仓库组的地址-->
        <url>http://localhost:8081/nexus/content/groups/public/</url>
        <!--是否下载 releases 构件-->
        <releases>
            <enabled>true</enabled>
        </releases>
        <!--是否下载 snapshots 构件-->
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
    </repositories>
    <pluginRepositories>
        <!-- 插件仓库， maven 的运行依赖插件，也需要从私服下载插件 -->
        <pluginRepository>
            <!-- 插件仓库的 id 不允许重复，如果重复后边配置会覆盖前边 -->
            <id>public</id>
            <name>Public Repositories</name>
            <url>http://localhost:8081/nexus/content/groups/public/</url>
        </pluginRepository>
    </pluginRepositories>
</profile>
```

使用 profile 定义仓库需要激活才可生效。 

```xml
<!--  和profiles 同级别 -->
<activeProfiles>
	<activeProfile>dev</activeProfile>
</activeProfiles>
```





##### 3.2.3测试从私服下载 jar 包

+ 删掉本地仓库的工程的jar包

![image-20191222212314528](img/image-20191222212314528.png) 

+ 出现如下日志

![image-20191222212255624](img/image-20191222212255624.png)

### 4.小结

1. 对着文档操作

 ==deploy可以把本地的jar上传到远程私服==

## 知识点-把第三方 jar 包放入本地仓库和私服【了解】

### 1.目标

- [ ] 掌握把第三方 jar 包放入本地仓库和私服 

### 2.路径

1. 导入本地库 
2. 导入私服 
3. 参数说明 

### 3.讲解

#### 3.1.导入本地库 

+ 随便找一个 jar 包测试， 可以先 CMD进入到 jar 包所在位置，运行(在powershell命令中可能有问题，建议在cmd中执行)

```
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dfile=fastjson-1.1.37.jar -Dpackaging=jar
```

![1536502172637](img/1536502172637.png)

![1536502183383](img/1536502183383.png)

#### 3.2.导入私服 

需要在 maven 软件的核心配置文件 settings.xml 中配置第三方仓库的 server 信息 

```xml
<server>
    <id>thirdparty</id>
    <username>admin</username>
    <password>admin123</password>
</server>
```

才能执行一下命令 

```
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=fastjson-1.1.37.jar -Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ -DrepositoryId=thirdparty
```

![1536502272558](img/1536502272558.png)

![1536502293709](img/1536502293709.png)

#### 3.3.参数说明 

DgroupId 和 DartifactId 构成了该 jar 包在 pom.xml 的坐标，项目就是依靠这两个属性定位。自己起名字也行。

Dfile 表示需要上传的 jar 包的绝对路径。

Durl 私服上仓库的位置，打开 nexus——>repositories 菜单，可以看到该路径。

DrepositoryId 服务器的表示 id，在 nexus 的 configuration 可以看到。

Dversion 表示版本信息。

关于 jar 包准确的版本：

​	包的名字上一般会带版本号，如果没有那可以解压该包，会发现一个叫 MANIFEST.MF 的文件 

​	这个文件就有描述该包的版本信息。

​	比如 Specification-Version: 2.2 可以知道该包的版本了。

​	上传成功后，在 nexus 界面点击 3rd party 仓库可以看到这包。 

### 4.小结

1. 有些jar中央仓库没有(eg:oracle驱动), 从官网/网络上下载下来, 安装到本地仓库. 我们的Maven项目就可以使用了
2. 具体操作参考文档



# 第七章-Lombok

## 知识点-Lombok介绍和配置

### 1.目标

- [ ] 掌握Lombok的配置

### 2.路径

1. 什么是Lombok
2. Lombok的作用
3. Lombok的配置

### 3.讲解

#### 3.1什么是Lombok

​	 Lombok是一个Java库，能自动插入编辑器并构建工具，简化Java开发。

​	官网: https://www.projectlombok.org/ 

#### 3.2Lombok的作用

​	通过==添加注解==的方式，Lombok能以简单的注解形式来简化java代码，提高开发人员的开发效率。

​	例如开发中经常需要写的javabean，都需要花时间去添加相应的getter/setter，也许还要去写构造器、equals等方法，而且需要维护，当属性多时会出现大量的getter/setter方法，这些显得很冗长也没有太多技术含量，一旦修改属性，就容易出现忘记修改对应方法的失误,使代码看起来更简洁些。

#### 3.3Lombok的配置

+ 添加maven依赖

```xml
<dependency>
	<groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
	<version>1.18.8</version>
	<scope>provided</scope>
</dependency>
```



+ ==安装插件==

  ​	使用Lombok还需要插件的配合，我使用开发工具为idea. 打开idea的设置，点击Plugins，点击Browse repositories，在弹出的窗口中搜索lombok，然后安装即可

  ![image-20191121092349714](img/image-20191121092349714.png) 

+ ==解决编译时出错问题==

  ​	编译时出错，可能是没有enable注解处理器。Annotation Processors > Enable annotation processing。设置完成之后程序正常运行。

  ![1600758879143](img/1600758879143.png)
  
  ![image-20191121092543928](img/image-20191121092543928.png)
  
  

### 4.小结

1. lombok安装
   1. 安装lombok插件
   2. 允许注解处理器开启，勾选Enable annotation processing

## 知识点-Lombok的常用注解

### 1.目标

- [ ] 掌握Lombok的常用注解

### 2.路径

1. @Data
2. @Getter/@Setter
3. @ToString
4. @NoArgsConstructor, @AllArgsConstructor

### 3.讲解



```xml
<!--引入依赖-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.8</version>
    <scope>provided</scope>
</dependency>

<!--
这是因为lombok基于字节码替换的方式，所以只要class生成后，那么就可以不用了。
-->
```

#### 3.1@Data

​	 @Data注解在类上，会为类的所有属性自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。 

==等效于：@Getter @Setter @RequiredArgsConstructor @ToString @EqualsAndHashCode.==

```java
@Data
public class User implements Serializable{
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
}

```

#### 3.2@Getter/@Setter

​	 如果觉得@Data太过残暴不够精细，可以使用@Getter/@Setter注解，此注解在属性上，可以为相应的属性自动生成Getter/Setter方法.

```java
public class User implements Serializable{
    @Setter
    @Getter
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
}
```



#### 3.3@ToString

​	 类使用@ToString注解，Lombok会生成一个toString()方法，默认情况下，会输出类名、所有属性（会按照属性定义顺序），用逗号来分割。 通过exclude属性指定忽略字段不输出,

```
@ToString(exclude = {"id"}) 
public class User implements Serializable{
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
}

```



#### 3.4@xxxConstructor

+ @NoArgsConstructor: 无参构造器 

```java
@NoArgsConstructor
public class User implements Serializable{
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
}
```

+ @AllArgsConstructor: 全参构造器 

```java
@AllArgsConstructor
public class User implements Serializable{
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;
}
```

### 4.小结

- @Data： 会生成get、set、tostring、equals、hashcode...
- @Setter、@Getter
- @Tostring
- @NoArgsConstructor 无参构造器
- @AllArgsConstructor 包含所有字段的构造器
- @Builder  可以方便、链式设置属性字段

总体而言，优大于劣。建议习惯使用lombok。

```java
package com.itheima.bean;

import lombok.*;

import java.io.Serializable;

/**
 * 1. 添加get、set方法： @Getter、@Setter
 * 2. 添加toString：@ToString
 * 3. 添加所有参数构造器：  @AllArgsConstructor
 * 4. 添加空参构造器：@NoArgsConstructor
 * 5. @Data注解: 包含@Getter、@Setter、@ToString、hashCode、equals方法
 * 6. @Builder：使用构造器模式创建对象的注解
 */
@Getter
@Setter
@ToString
//@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class User implements Serializable {
    private Integer id;
    private String username;
    private String password;
    private String address;
    private String nickname;
    private String gender;
    private String email;
    private String status;

}

```



- 扩展 自定义Builde实现（构造者模式）

```java
package com.itheima.bean;

public class UserBean {
    private Integer id;
    private String username;

    /**
     * 1. 静态方法builder()返回一个内部静态内部类实例UserBeanBuilder对象
     * @return
     */
    public static  UserBeanBuilder builder(){
        return new UserBeanBuilder();
    }


    /**
     * 2. 定义内部静态内部类UserBeanBuilder
     */
    public static class UserBeanBuilder{

        /**
         * 3. 持有一个要作用的javabean对象
         */
        private static UserBean userBean = new UserBean();


        /**
         * 4. 提供一系列的和字段同名的方法设置对应的javabeaan的属性值
         * @param id
         * @return
         */
        public UserBeanBuilder id(Integer id){
            userBean.id = id;
            return this;
        }

        public UserBeanBuilder username(String username){
            userBean.username = username;
            return this;
        }


        /**
         * 5. 提供一个build()方法，返回一个对应的javabean对象
         * @return
         */
        public UserBean build(){
            return userBean;
        }
    }


    public static void main(String[] args) {
        UserBean userBean = UserBean.builder()
                .id(1)
                .username("123")
                .build();
    }


}

```





# 总结

- 概念
  - maven仓库（就是保存jar包的文件夹或者远程服务）
    - 本地仓库
    - 远程仓库
    - 中央仓库
  - 坐标（定位依赖的jar）
    - groupId：组织、公司名字
    - atifactId：项目名
    - version： 版本号
- 实操【重点】
  - maven的安装
    - 下载、解压到一个正常的目录
    - 配置环境变量： MAVEN_HOME、Path
    - 配置本地仓库目录
    - 测试： mvn -version
  - IDEA中配置maven
    - 当前工程的配置
      - File-》settings--》Build--》Build Tools--》Maven
        - maven根目录
        - 用户settings.xml文件目录
        - 本地仓库
        - Runner：拷贝那个值
  - 默认配置
      - File-》other settings--》Build--》Build Tools--》Maven
  - idea创建javase工程
    - ![1598254000219](img/1598254000219.png)
    - ![1598254060640](img/1598254060640.png)
  - idea创建javaweb工程
    - ![1598254100193](img/1598254100193.png)
  
- 常用命令[MAVEN的生命周期]

  - clean： 清除target目录的内容
  - compile： 编译src/main/java下面的代码、src/main/resources的资源----》target
  - test： 单元测试，执行src/test/java所有的单元测试案例
  - package： 打包（jar、war包）
  - install： 安装到本地仓库
  - deploy：安装上传到远程仓库

- 依赖管理

  - 引入jar包依赖

    - ```xml
      <dependencies>
          <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
          </dependency>
      </dependencies>
      ```

      

  - 依赖范围

    - juint： `<scope>test</scope>`  仅仅在测试期有效
    - servlet-api： `<scope>provided</scope>` 在运行时不生效
    - jdbc驱动： `<scope>runtime</scope>` 在编译期不生效

- lombok
  - 引入依赖坐标
  - 安装插件
  - 允许Enable annotation processing
  - ![1598254702030](img/1598254702030.png)



# 预习要点

mybatis框架01

- 快速入门，掌握mybatis的编程步骤
- 配置直接抄、然后改！
- 重点关注： 代理方式、映射文件的CRUD编程

mybatis框架02

- 理解一级、二级缓存
- 多表关联的语句编写
- 延迟加载理解
- 注解【上课听一下。。。有这么回事】



==web阶段算是告一段落：最低要求：==

能够使用mybatis+maven+servlet+jsp+vue+html

完成注册、登录功能（加上过滤器过滤权限）

