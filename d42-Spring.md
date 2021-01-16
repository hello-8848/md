---
typora-copy-images-to: img
---

# day42-Spring

# 学习目标

- [ ] 能够描述spring框架
- [ ] 能够编写spring的IOC的入门案例【掌握】
- [ ] 能够说出spring的bean标签的配置【掌握】
- [ ] 能够理解Bean的属性注入方法
- [ ] 能够理解复杂类型的属性注入

# 第一章-Spring概述  

## 知识点-Spring介绍

### 1.目标

- [ ] 能够描述spring框架

### 2.路径

1. 什么是Spring
2. Spring 的发展历程 
3. Spring的优点   
4. Spring的体系结构

### 3.讲解

#### 3.1什么是Spring

​	Spring 是一个开源框架，是为了解决企业应用程序开发复杂性而创建的。

​	框架的主要优势之一就是其分层架构，分层架构允许您选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。

​	简单来说，Spring是一个分层的JavaSE/EE full-stack(一站式) 轻量级开源框架。

​	==一站式:Spring提供了三层解决方案.==



![1602404375605](img/1602404375605.png)

#### 3.2Spring 的发展历程 

1997 年 IBM 提出了 EJB（企业级javabean） 的思想

1998 年， SUN 制定开发标准规范 EJB1.0  
1999 年， EJB1.1 发布
2001 年， EJB2.0 发布

2003 年， EJB2.1 发布 
2006 年， EJB3.0 发布
​

Rod Johnson（spring 之父） 
​	Expert One-to-One J2EE Design and Development(2002),阐述了 J2EE 使用 EJB 开发设计的优点及解决方案
​	Expert One-to-One J2EE Development without EJB(2004),阐述了 J2EE 开发不使用 EJB 的解决方式（Spring 雏形）

2017 年 9 月份发布了 spring 的最新版本 spring 5.0 通用版（GA）



#### 3.3Spring的优点

1.**方便解耦，简化开发**

​	通过Spring提供的IoC容器，我们可以将对象之间的依赖关系交由Spring进行控制，避免硬编码所造成的过度程序耦合。有了Spring，用户不必再为单实例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。

2.**AOP编程的支持**

​	通过Spring提供的AOP功能，方便进行面向切面的编程，许多不容易用传统OOP实现的功能可以通过AOP轻松应付。

3.**声明式事务的支持 **

​	在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量。

4.**方便程序的测试  **

​	可以用非容器依赖的编程方式进行几乎所有的测试工作，在Spring里，测试不再是昂贵的操作，而是随手可做的事情。例如：Spring对Junit4支持，可以通过注解方便的测试Spring程序。

5.**方便集成各种优秀框架** 

​	Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了对各种优秀框架（如Struts,Hibernate、Hessian、Quartz）等的直接支持。

6.**降低Java EE API的使用难度 **

​	Spring对很多难用的Java EE API（如JDBC，JavaMail，远程调用等）提供了一个薄薄的封装层，通过Spring的简易封装，这些Java EE API的使用难度大为降低。

#### 3.4Spring的体系结构

![img](img/tu_0.png)

### 4.小结

1. Spring： 一站式的分层框架

2. 学spring framework：IOC、AOP

   

   



# 第二章-IOC

## 知识点-工厂模式解耦

### 1.目标

- [ ] 能够使用工厂模式进行解耦

### 2.路径

1. 程序耦合的概述

2. 使用工厂模式解耦 

### 3.讲解

#### 3.1程序的耦合

##### 3.1.1程序耦合的概述

​	耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。耦合的强弱取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差( 降低耦合性，可以提高其独立性)。 耦合性存在于各个领域，而非软件设计中独有的，但是我们只讨论软件工程中的耦合。
​	==在软件工程中， 耦合指的就是就是对象之间的依赖性。对象之间的耦合越高，维护成本越高。因此对象的设计
应使类和构件之间的耦合最小==。 软件设计中通常用耦合度和内聚度作为衡量模块独立程度的标准。 划分模块的一个
准则就是==高内聚低耦合==。



---------------------------



它有如下分类：
（1） 内容耦合。当一个模块直接修改或操作另一个模块的数据时，或一个模块不通过正常入口而转入另一个模块时，这样的耦合被称为内容耦合。内容耦合是最高程度的耦合，应该避免使用之。
（2） 公共耦合。两个或两个以上的模块共同引用一个全局数据项，这种耦合被称为公共耦合。在具有大量公共耦合的结构中，确定究竟是哪个模块给全局变量赋了一个特定的值是十分困难的。
（3） 外部耦合 。一组模块都访问同一全局简单变量而不是同一全局数据结构，而且不是通过参数表传递该全局变量的信息，则称之为外部耦合。
（4） 控制耦合 。一个模块通过接口向另一个模块传递一个控制信号，接受信号的模块根据信号值而进行适当的动作，这种耦合被称为控制耦合。
（5） 标记耦合 。若一个模块 A 通过接口向两个模块 B 和 C 传递一个公共参数，那么称模块 B 和 C 之间存在一个标记耦合。
（6） 数据耦合。模块之间通过参数来传递数据，那么被称为数据耦合。数据耦合是最低的一种耦合形式，系统中一般都存在这种类型的耦合，因为为了完成一些有意义的功能，往往需要将某些模块的输出数据作为另一些模块的输入数据。
（7） 非直接耦合 。两个模块之间没有直接关系，它们之间的联系完全是通过主模块的控制和调用来实
现的。
总结：
​	耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上我们应采用以下原则：如果模块间必须存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合。内聚与耦合内聚标志一个模块内各个元素彼此结合的紧密程度，它是信息隐蔽和局部化概念的自然扩展。内聚是从功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事。它描述的是模块内的功能联系。耦合是软件
结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通过接口的数据。 程序讲究的是低耦合，高内聚。就是同一个模块内的各个元素之间要高度紧密，但是各个模块之
间的相互依存度却要不那么紧密。 

​	内聚和耦合是密切相关的，同其他模块存在高耦合的模块意味着低内聚，而高内聚的模块意味着该模块同其他
模块之间是低耦合。在进行软件设计时，应力争做到高内聚，低耦合。 

##### 3.1.2在代码中体现

​	早期我们的 JDBC 操作，注册驱动时，我们为什么不使用 DriverManager 的 register 方法，而是采
用 Class.forName 的方式？

​	原因就是： 我们的类依赖了数据库的具体驱动类（MySQL） ，如果这时候更换了数据库（比如 Oracle） ，
需要修改源码来重新数据库驱动。这显然不是我们想要的。 

```java
/**
* 程序的耦合
* 耦合：程序间的依赖关系
* 包括：
* 		类之间的依赖
* 		方法间的依赖
* 解耦：
* 		降低程序间的依赖关系
* 实际开发中：
* 		应该做到：编译期不依赖，运行时才依赖。
* 解耦的思路：
*		第一步：使用反射来创建对象，而避免使用 new 关键字。
*		第二步：通过读取配置文件来获取要创建的对象全限定类名
*/
public class JdbcDemo1 {
    public static void main(String[] args) throws Exception{
        //1.注册驱动 
        //DriverManager.registerDriver(new Driver());
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接
        //3.获取操作数据库的预处理对象
        //4.执行 SQL，得到结果集
        //5.遍历结果集
        //6.释放资源
    }
}
```

##### 3.1.3解决程序耦合的思路

​	之前我们讲解 jdbc 时，是通过反射来注册驱动的，代码如下：

​	`Class.forName("com.mysql.jdbc.Driver");//此处只是一个字符串`

​	此时的好处是，我们的类中不再依赖具体的驱动类，此时就算删除 mysql 的驱动 jar 包，依然可以编译（运行就不要想了没有驱动不可能运行成功的） 。 

​	同时，也产生了一个新的问题， mysql 驱动的全限定类名字符串是在 java 类中写死的，一旦要改还是要修改源码。`解决这个问题也很简单，使用配置文件配置 `

#### 3.2.自定义对象工厂

```java
//需求：我们以一个保存信息到账户的代码示例，来一步一步了解获取对象实例的方式。

//业务场景：保存账户信息；

//设计：业务层 AccountService、持久层 AccountDao
```



1. 常规--原始方式（**直接在使用者里面new出来要用的对象**）
   + 操作:  创建类, 直接根据类new对象
   + 优点:  好写, 简单
   + 缺点:  耦合度太高, 不好维护
2. 接口方式（可以认为就是多态面向接口编程）
   + 操作: **定义接口, 创建对应的实现类.  ==多态： 接口、实现类的对象==**
   + 优点: 耦合度相对原始方式 减低了一点
   + 缺点: 多写了接口, 还是需要改源码 不好维护
3. 自定义IOC（从一个容器里面获取）
   + 方式: 使用对象的话, 不直接new()了,直接从工厂里面取; 不需要改变源码

##### 3.2.1原始方式

 代码略

##### 3.2.2接口方式

定义业务层接口、实现； dao层接口、实现。

![1595408527383](img/1595408527383.png)

+ 业务层

  + AccountService

  ```java
  package com.itheima.service;
  
  /**
   * 接口定义了什么规范（业务层有什么功能！）
   */
  public interface AccountService {
      void save();
  }
  
  ```
  
- AccountServiceImpl
  
```java
  package com.itheima.service.impl;
  
  import com.itheima.dao.AccountDao;
  import com.itheima.dao.impl.AccountDaoImpl;
  import com.itheima.service.AccountService;
  
  public class AccountServiceImpl implements AccountService {
      // 多态（接口-实现类）
      private AccountDao accountDao = new AccountDaoImpl();
  
      @Override
      public void save() {
          System.out.println("AccountServiceImpl的保存方法,准备调用dao层");
          // 业务层仅仅关注接口，我不需要知道具体实现类的逻辑。
          accountDao.save();
      }
  }
  
```

- 持久层Dao

  - AccountDao

  ```java
  package com.itheima.dao;
  
  public interface AccountDao {
      void save();
  }
  
  ```

  - AccountDaoImpl

  ```java
  package com.itheima.dao.impl;
  
  import com.itheima.dao.AccountDao;
  
  public class AccountDaoImpl implements AccountDao {
      @Override
      public void save() {
          System.out.println("AccountDaoImpl的保存方法");
      }
  }
  
  ```

- 测试：模拟web层-客户端

  - UserClient

  ```java
  package com.itheima.web;
  
  import com.itheima.service.AccountService;
  import com.itheima.service.impl.AccountServiceImpl;
  
  public class UserClient {
      public static void main(String[] args) {
          // 调用业务层
          AccountService accountService = new AccountServiceImpl();
          accountService.save();
      }
  }
  
  ```

  

##### 3.2.3自定义对象工厂(BeanFactory)

###### 3.2.2.1工厂模式解耦思路

​	我们可以将三层架构里面的类通过文件方式配置起来。——这里主要是配置Service、Dao层的类。

​	然后使用一个类去读取加载解析这个文件，并将获得的类实例对象（通过反射获取）保存到一个容器里面（可以是一个map-key只要唯一即可，value是类对应的对象实例）。其它类要使用这些配置好的类，则从这个容器里面获取就行了。

- ==写一个XML文件，配置类的全限定名==
- ==读取文件，反射获取类的实例==
- ==提供方法，供外界获取类实例对象==

![1590462076636](img/1590462076636.png)

###### 3.2.2.2实现

+ 添加坐标依赖

```xml
  <dependencies>
    <!-- 解析 xml 的 dom4j -->
    <dependency>
      <groupId>dom4j</groupId>
      <artifactId>dom4j</artifactId>
      <version>1.6.1</version>
    </dependency>
    <!-- dom4j 的依赖包 jaxen -->
    <dependency>
      <groupId>jaxen</groupId>
      <artifactId>jaxen</artifactId>
      <version>1.1.6</version>
    </dependency>
    <!--单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

+ 编写配置文件

  + applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
    <!--id就是接口的名字, class实现类的全限定名  -->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
</beans>
```

- 编写BeanFactory.java

  下面的通过 BeanFactory 中 getBean 方法获取对象就解决了我们代码中对具体实现类的依赖。 

```java
package com.itheima.factory;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 解析配置文件得到类的对象，存储起来--自定义IOC容器
 */
public class BeanFactory {
    // 容器
    private static Map<String, Object> beanMap = new HashMap<>();

    static {
        try {
            InputStream resourceAsStream =
                    BeanFactory.class.getClassLoader().getResourceAsStream("applicationContext.xml");


            SAXReader saxReader = new SAXReader();
            Document document = saxReader.read(resourceAsStream);

            // 读取所有的bean标签
            List<Element> list = document.selectNodes("//bean");

            for (Element element : list) {
                beanMap.put(element.attributeValue("id"),
                        Class.forName(element.attributeValue("class")).newInstance());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Object getBean(String beanName){
        return beanMap.get(beanName);
    }

}

```

- 业务层

  - AccountService

  ```java
  package com.itheima.service;
  
  /**
   * 接口定义了什么规范（业务层有什么功能！）
   */
  public interface AccountService {
      void save();
  }
  
  ```
  
- AccountServiceImpl
  
```java
  package com.itheima.service.impl;
  
  import com.itheima.dao.AccountDao;
  import com.itheima.dao.impl.AccountDaoImpl;
  import com.itheima.factory.BeanFactory;
  import com.itheima.service.AccountService;
  
  public class AccountServiceImpl implements AccountService {
  
      private AccountDao accountDao = (AccountDao)BeanFactory.getBean("accountDao");
  
  
      @Override
      public void save() {
          System.out.println("AccountServiceImpl的保存方法,准备调用dao层");
          accountDao.save();
      }
  }
  
```

- 持久层Dao

  - AccountDao

  ```java
  package com.itheima.dao;
  
  public interface AccountDao {
      void save();
  }
  
  ```

  - AccountDaoImpl

  ```java
  package com.itheima.dao.impl;
  
  import com.itheima.dao.AccountDao;
  
  public class AccountDaoImpl implements AccountDao {
      @Override
      public void save() {
          System.out.println("AccountDaoImpl的保存方法");
      }
  }
  
  ```
  
- 测试：模拟web层-客户端

  - UserClient

  ```java
  
  package com.itheima.web;
  
  import com.itheima.factory.BeanFactory;
  import com.itheima.service.AccountService;
  
  public class UserClient {
      public static void main(String[] args) {
          // 调用业务层---从beanfactory获取对象实例
          AccountService accountService = (AccountService)BeanFactory.getBean("accountService");
  
          accountService.save();
      }
  }
  
  
  ```
### 4.小结

1. 自定义IOC容器：
   1. 配置文件解耦，配置类的全限定名
   2. 解析文件，反射获取类实例对象，保存起来（容器：map）
   3. 提供获取类实例对象的方法





## 知识点-IOC概念

### 1.目标

- [ ] 能够理解IOC概念

### 2.路径

1. 分析和IOC的引入
2. IOC概述

### 3.讲解

#### 3.1分析和IOC的引入

上一小节我们通过使用工厂模式，实现了表现层——业务层以及业务层——持久层的解耦。
它的核心思想就是：
	1、通过读取配置文件反射创建对象。
	2、把创建出来的对象都存起来，当我们下次使用时可以直接从存储的位置获取。
这里面要解释两个问题：
	第一个：存哪去？
		分析：由于我们是很多对象，肯定要找个集合来存。这时候有 Map 和 List 供选择。
		到底选 Map 还是 List 就看我们有没有查找需求。有查找需求，选 Map。
		所以我们的答案就是:在应用加载时，创建一个 Map，用于存放三层对象。我们把这个 map 称之为容器。
	第二个： 什么是工厂？
		工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。
		原来：我们在获取对象时，都是采用 new 的方式。 是主动的。 



+ 老方式

![img](img/tu_10.png)

+ 现在 我们获取对象时，同时跟工厂要，有工厂为我们查找或者创建对象。 是被动的。 

  ![img](img/tu_11.png)



#### 3.2IOC概述

IOC(inversion of control)的中文解释是“==控制反转”，对象的使用者不是创建者.  作用是将对象的创建 反转给spring框架来创建和管理。==

==**说白了，就是把对象的创建、管理操作，交给其它的容器框架比如spring去管理了，不再由使用者自身创建。**==

原来要new的对象，现在直接通过容器获取。（对象在容器内部去管理了）

 ioc 的作用：削减计算机程序的耦合(解除我们代码中的依赖关系)。 

### 4.小结

1. IOC： 控制反转，就是把依赖对象的创建由原来谁用谁new，变成了从容器中获取
2. IOC的好处： 实现了依赖的松耦合，代码维护更方便

## 案例-Spring的IOC快速入门【重要】

### 1.需求

- [ ] 能够编写spring的IOC的入门案例

### 2.分析

我们使用的案例是， 账户的业务层和持久层的依赖关系解决。

**为了快速体验，我们不写实体类**，直接看依赖关系的解决，**这里我们使用java工程，不需要使用web工程**。

### 3.实现

#### 3.0步骤

1. 创建Maven工程, 添加坐标

2. 准备好接口和实现类

3. 创建spring的配置文件 (applicationContext.xml), 配置bean标签

4. 创建工厂对象 获得bean 调用

   

#### 3.1准备工作

##### 3.1.1准备 spring 的开发包 

官网： http://spring.io/ 
下载地址：http://repo.springsource.org/libs-release-local/org/springframework/spring
解压:(Spring 目录结构:)

* docs :API 和开发规范.
* libs :jar 包和源码.
* schema :约束. 

![img](img/tu_12.png)

我们上课使用的版本是 spring5.0.2。
特别说明：

	spring5 版本是用 jdk8 编写的，所以要求我们的 jdk 版本是 8 及以上。  

spring framework的文档可以参考：https://docs.spring.io/spring/docs/

#### 3.2 实现

##### 3.2.1 创建工程

- 创建 maven 工程并导入坐标 

![img](img/tu_13.png)

- 坐标

```xml
<dependencies>
    <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.2.RELEASE</version>
    </dependency>	
</dependencies>
```



- 在resources目录创建配置文件,一般习惯名称为 applicationContext.xml

  ![img](img/tu_14.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        bean标签
            属性：
                id：唯一标志
                class： 类的全限定名必须是类，不能是接口、抽象类。
    -->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"/>

</beans>
```

##### 3.2.2 接口和实现类

- AccountDao .java

```java
package com.itheima.dao;

public interface AccountDao {
    void save();
}

```

- AccountDaoImpl.java

```java
package com.itheima.dao.impl;

import com.itheima.dao.AccountDao;

public class AccountDaoImpl implements AccountDao {
    @Override
    public void save() {
        System.out.println("AccountDaoImpl的保存方法");
    }
}

```

- AccountService.java

```java
package com.itheima.service;

/**
 * 接口定义了什么规范（业务层有什么功能！）
 */
public interface AccountService {
    void save();
}

```

- AccountServiceImpl.java

```java
package com.itheima.service.impl;


import com.itheima.service.AccountService;

public class AccountServiceImpl implements AccountService {


    @Override
    public void save() {
        System.out.println("AccountServiceImpl的保存方法,准备调用dao层");

    }

}

```



##### 3.2.3 单元测试

- 编写Java代码测试

```java
package com.itheima;

import com.itheima.service.AccountService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class A_SpringIOCTest {
    @Test
    public void test(){
        // 需要创建spring容器对象（工厂）--所谓ApplicationContext，应用上下文就是spring容器
        ApplicationContext applicationContext =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        // 获取bean
        AccountService accountService =
                (AccountService)applicationContext.getBean("accountService");

        accountService.save();
    }
}

```



### 4.小结

#### 4.1步骤

1. 引入依赖jar包

2. 写一个配置文件，一般习惯性命名为applicationContext.xml

3. 配置bean

4. 可以写测试代码： 

   1. ```java
      ApplicationContext applicationContext =
                      new ClassPathXmlApplicationContext("applicationContext.xml");
      ```

      

我们当前使用的Spring版本是5.0.2 jdk要求>=8的





## 知识点-Spring 基于 XML 的 IOC 细节（bean标签的属性）

### 1.目标

- [ ] 能够说出spring的bean标签的配置

### 2.路径

1. 配置文件详解(Bean标签)
2. bean的作用范围和生命周期 

### 3.讲解 

#### 3.1配置文件详解(Bean标签)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        bean标签： 表示配置一个bean（注册存储到的类）到spring容器
                id/name属性： 唯一标志，id、name随便用一个即可。
                class属性： 类的全限定名(包名.类名)； 必须是类，不能是接口、抽象类
                scope 属性： 表示bean的范围，默认就是单例的singleton,单例就是整个容器只有一个对象；
                            prototype： 多例；
                            =====================================
                            request、session（web资源的范围）-了解
            了解的属性：
                init-method属性： 是对象创建后的初始化方法，值就是该对象里面的方法名字
                destroy-method属性： 销毁单例bean的会触发的方法
    -->
    <!--一般id写接口的名字，首字母小写-->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>

    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"
          name="accountService" scope="singleton" init-method="init" destroy-method="destroy"/>

</beans>
```

**单元测试：**

- AccountServiceImpl增加初始化、销毁方法

```java
package com.itheima.service.impl;


import com.itheima.service.AccountService;

public class AccountServiceImpl implements AccountService {


    public AccountServiceImpl() {
        System.out.println("AccountServiceImpl构造器");
    }

    @Override
    public void save() {
        System.out.println("AccountServiceImpl的保存方法,准备调用dao层");

    }


    public void init(){
        System.out.println("init...");
    }

    public void destroy(){
        System.out.println("destroy...");
    }
}

```

- 测试

```java
package com.itheima;

import com.itheima.service.AccountService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 测试bean的标签
 */
public class B_SpringIOCBeanTest {
    @Test
    public void test(){
        // 获得容器
        ApplicationContext context =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        // 获取bean
        AccountService accountService = (AccountService)context.getBean("accountService");
        //AccountService accountService2 = (AccountService)context.getBean("accountService");

        // 是否同一个对象
        //System.out.println(accountService == accountService2);

        // 手动销毁---了解
        if(context instanceof AbstractApplicationContext){
            AbstractApplicationContext abstractApplicationContext =
                    (AbstractApplicationContext) context;
            abstractApplicationContext.registerShutdownHook();
            abstractApplicationContext.close();
        }
    }
}

```



+ id/name属性

  ​	用于标识bean ， 其实id 和 name都必须具备唯一标识 ，两种用哪一种都可以。但是一定要唯一、 一般开发中使用id来声明. 

+ class属性: 用来配置要实现化==类的全限定名==

+ scope属性: 用来描述bean的作用范围

  ​	singleton: 默认值，单例模式。spring创建bean对象时会以单例方式创建。(默认)

  ​	prototype: 多例模式。spring创建bean对象时会以多例模式创建。	

  ​	request: 针对Web应用。spring创建对象时，会将此对象存储到request作用域。--仅作了解，基本不用	

  ​	session: 针对Web应用。spring创建对象时，会将此对象存储到session作用域。	--仅作了解，基本不用

+ init-method属性：spring为bean初始化提供的回调方法--仅作了解	

+ destroy-method属性：spring为bean销毁时提供的回调方法. 销毁方法针对的都是单例bean ， 如果想销毁bean , 可以关闭工厂--仅作了解	

#### 3.2bean的作用范围和生命周期

+ 单例对象： scope="singleton"

  ​	一个应用只有一个对象的实例。它的作用范围就是整个应用。
  ​	生命周期：
  ​		对象出生：当应用加载，创建容器时，对象就被创建了。
  ​		对象活着：只要容器在，对象一直活着。
  ​		对象死亡：当应用卸载，销毁容器时，对象就被销毁了。

+ 多例对象： scope="prototype"

  ​	每次访问对象时，都会重新创建对象实例。
  ​	生命周期：
  ​		对象出生：当使用对象时，创建新的对象实例。
  ​		对象活着：只要对象在使用中，就一直活着。
  ​		对象死亡：当对象长时间不用时，被 java 的垃圾回收器回收了.

### 4.小结

1. 配置结构

```xml
<bean id|name="唯一标志" class="类的全限定名" scope="bean的范围" />
```

2. scope取值
   + singleton:单例【默认】, 使用的都是同一个对象(同一个id获得的). 单例的bean存到Spring容器里面
   + prototype:多例, 使用的不是同一个对象(同一个id获得的), 使用一个 创建一个.  多例的bean不会存到Spring容器里面





## 知识点-Spring工厂详解

### 1.目标

- [ ] 掌握Spring常见的工厂类

### 2.路径

1. spring 中工厂的类结构图
2. BeanFactory 和 ApplicationContext 的区别

### 3.讲解

#### 3.1spring 中工厂的类结构图

![1595417068120](img/1595417068120.png)

- **ClassPathXmlApplicationContext**：它是从类的根路径下加载配置文件 
- FileSystemXmlApplicationContext：它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。
- AnnotationConfigApplicationContext:当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

#### 3.2BeanFactory 和 ApplicationContext 的区别

- ApplicationContext 是现在使用的工厂

  ```java
  ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
  ```



- XmlBeanFactory是老版本使用的工厂,目前已经被废弃【了解】

  ```java
  // 用到时才会加载
  Resource resource = new ClassPathResource("applicationContext.xml");
  BeanFactory beanFactory = new XmlBeanFactory(resource);
  ```

两者的区别:

​	ApplicationContext加载方式是框架启动时就开始创建所有单例的bean,存到了容器里面

<u>BeanFactory加载方式是用到bean时再加载(目前已经被废弃)</u>



### 4.小结

1. 工厂类继承关系
   + BeanFactory
     + ==ApplicationContext==
       + FileSystemXmlApplicationContext   xml方式 它是从磁盘路径上加载配置文件
       + ==ClassPathXmlApplicationContext    xml方式  它是从类的根路径下加载配置文件==
       + ==AnnotationConfigApplicationContext   注解方式的==

2. 新旧工厂
   + 新工厂: 在工厂初始化的话的时候, 就会创建配置的所有的单例bean,存到Spring容器里面
   + 旧工厂(已经废弃了): 等用到对象的时候, 再创建



## 知识点-实例化Bean的三种方式【了解】

### 1.目标

- [ ] 了解实例化 Bean 的三种方式

### 2.路径

1. 无参构造方法方式 
2. 静态工厂方式
3. 实例工厂实例化的方法 

### 3.讲解

#### 3.1方式一:无参构造方法方式  

​	需要实例化的类，==提供无参构造方法==

​	配置代码

```xml
 <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
```

#### 3.2方式二:静态工厂方式--了解

​	需要额外提供工厂类 ， 工厂方法是**静态方法，返回一个对象**

```java
package com.itheima.factory;
import com.itheima.service.impl.AccountServiceImpl;

public class StaticFactory {

    public static Object getBean(){
        System.out.println("com.itheima.factory.StaticFactory.getBean");
        return new AccountServiceImpl();
    }
}

```

​	配置代码， 配置bean标签的属性**factory-method**

```xml
<!--方式二:静态工厂方式  -->	
<!--配置了 factory-method 属性的bean的class不是直接的类；
        是 factory-method 属性指定的方法的返回值的类型的实例。
    -->
    <bean id="accountService2"
          class="com.itheima.factory.StaticFactory" factory-method="getBean"/>
```

#### 3.3方式三:实例工厂实例化的方法--了解

factory-bean  值是bean的。

factory-method  值是指定的bean的方法名字。

创建实例化工厂类

```java
package com.itheima.factory;

import com.itheima.service.impl.AccountServiceImpl;

public class InstanceFactory {

    public Object getBean(){
        System.out.println("com.itheima.factory.InstanceFactory.getBean");
        return new AccountServiceImpl();
    }
}

```

配置

```xml
<!-- 方式三: 实例化工厂 -->
<!--实例工厂实例化-->
<bean id="factory" class="com.itheima.factory.InstanceFactory"/>
<bean id="accountService3" factory-bean="factory" factory-method="getBean"/>
```

### 4.小结

1. 主要是用第一种方式。后面的了解即可。



# 第三章-依赖注入

## 知识点-依赖注入概念

### 1.目标

- [ ] 掌握什么是依赖注入

### 2.讲解

​	依赖注入全称是 dependency Injection （==DI==）翻译过来是依赖注入.==其实就是如果我们托管的某一个类中存在属性，需要spring在创建该类实例的时候，顺便给这个对象里面的属性进行赋值。 这就是依赖注入==。

​	现在, Bean的创建交给Spring了, 需要在xml里面进行注册（==注册==到容器里面）

​	我们交给Spring创建的Bean里面可能有一些属性(字段), Spring帮我创建的同时也把Bean的一些属性(字段)给赋值, 这个赋值就是注入.

![1595421873536](img/1595421873536.png)

### 3.小结

1. DI,依赖注入： 如果bean被spring容器管理，那么可以在实例化这个bean的同时把依赖的属性给赋值。
2. 注册： 就是把bean交给spring容器管理



## 知识点-依赖注入实现

### 1.目标

- [ ] 能够理解Bean的属性注入方法
- [ ] 能够理解复杂类型的属性注入

### 2.路径

1. 构造方法方式注入
2. setter方法方式的注入
3. P名称空间注入（了解）
4.  SpEL的属性注入（了解）

### 3.讲解

#### 环境准备

- User： 省略了getter、setter方法

```java
package com.itheima.bean;

import java.io.Serializable;
import java.util.*;


public class User implements Serializable {

    private Integer id;
    private String username;
    // 爱好
    private String[] hobby;
    // 银行卡
    private List<String> bankCards;
    // 学历-学校（小学:深圳小学）
    private Map<String, String> studyLevelSchool;

    private Properties friends;

    // 其他bean类型
    private Address address;

    private Set<String> sets;

    // 含参构造器
    public User(String username) {
        this.username = username;
    }

    public User() {
    }
	
    // 省略toString、getter、setter
}

```

- Address

```java
package com.itheima.bean;

public class Address {
    private String name;
    private String province;
    private String country;

    // 省略toString、getter、setter
}

```



#### 3.1构造方法方式注入【掌握】

+ 配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="user" class="com.itheima.bean.User">
  
          <!--构造器注入， 用bean的子标签constructor-arg
                  name属性：参数名-value属性：参数值
                  index属性：参数的索引值从0开始-value属性：值
                  type属性：参数的类型-value属性：值
          -->
          <!--<constructor-arg name="username" value="zs"/>-->
          <!--<constructor-arg index="0" value="zs"/>-->
          <constructor-arg type="java.lang.String" value="zs"/>
      </bean>
  </beans>
  ```



#### 3.2 setter方法方式的注入【重点】

- 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.itheima.bean.User">

        <!--构造器注入， 用bean的子标签constructor-arg
                name属性：参数名-value属性：参数值
                index属性：参数的索引值从0开始-value属性：值
                type属性：参数的类型-value属性：值
        -->
        <!--<constructor-arg name="username" value="zs"/>-->
        <!--<constructor-arg index="0" value="zs"/>-->
        <!--<constructor-arg type="java.lang.String" value="zs"/>-->


        <!--setter方法的方式注入属性：
            1. 注入简单类型：  property标签： name指定属性名，value指定值
            2. 注入数组类型： property标签的子标签array，array的子标签value配置一个一个的元素值；array可以用list代替
            3. 注入list类型： property标签的子标签list，list的子标签value配置一个一个的元素值；list可以用array代替
            4. 注入map类型： property标签的子标签map， map的字标签entry，entry的key、value属性
            5. 注入properties类型：property标签的子标签props，props的子标签prop，属性key，内容写值。
            6. 注入set类型：property标签的子标签set，set里面的子标签value
            7. 注入其他的bean类型， property的name属性，ref属性指向；另一个bean的id/name值
        -->
        <property name="id" value="1001"/>
        <property name="username" value="zs"/>

        <property name="hobby">
            <array>
                <value>篮球</value>
                <value>足球</value>
            </array>
            <!--<list>
                <value>篮球</value>
                <value>足球</value>
            </list>-->
        </property>

        <property name="bankCards">
            <list>
                <value>642232433455468</value>
                <value>642232433455231</value>
            </list>
            <!--<array>
                <value>642232433455468</value>
                <value>642232433455231</value>
            </array>-->
        </property>

        <property name="studyLevelSchool">
            <map>
                <entry key="小学" value="深圳小学"/>
                <entry key="中学" value="深圳中学"/>
            </map>
        </property>

        <property name="friends">
            <props>
                <prop key="最好的">刘备</prop>
                <prop key="最不好的">曹操</prop>
            </props>
        </property>

        <property name="sets">
            <set>
                <value>1</value>
                <value>2</value>
                <value>3</value>
            </set>
        </property>

        <property name="address" ref="address"/>
    </bean>


    <bean id="address" class="com.itheima.bean.Address">
        <property name="name" value="中粮1310"/>
        <property name="country" value="中国"/>
        <property name="province" value="广东"/>
    </bean>
</beans>
```





- 小结
  - 构造器注入（需要包含含参构造器）
    - 用name-value配置比较多
    - 如果构造器参数需要其他的bean，则使用ref替代value
  - setter方法（属性注入）： 必须有set方法
    - 简单类型： name-value
    - 数组、list、set
    - map、properties
    - bean类型： 使用ref引用其他的bean（spring的bean）



#### 3.3P名称空间注入【了解】

##### 3.3.1p名称空间的方式

p名称空间可以简化属性注入。简化了property标签。


+ 在applicationContext.xml引入p命名空间`xmlns:p="http://www.springframework.org/schema/p"`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```

+ 使用方式  **bean的属性p:属性名=注入的值**

```xml
<bean name="address2" class="com.itheima.bean.Address" p:name="北京"/>
```

#### 3.4 Spring3.0之后  SpEL的属性注入【了解】

​	这种方式是使用spring 提供的一种表达式方式去赋值， 如果是给前面提过的几个类型注入赋值，那么使用spEL 表达式来注入赋值倒显得繁琐。但是这个spEL 方式赋值，最大的好处在于，它能像EL表达式一般，在里面进行运算、 逻辑判断,还可以调用其它Bean的属性和方法给当前属性赋值

+ 语法格式 ：  #{表达式} 

+ 类中存在一个属性 name  , 并且给定 set方法。 


```xml
<bean name="address3" class="com.itheima.bean.Address">
           <property name="province" value="广东"/>
           <property name="country" value="#{'China'}"/>
           <property name="name"  value="#{'Hello'.length()}"/>
    </bean>
```

### 4.小结

1. 重点： setter方式注入、构造器掌握

# 第四章-IOC练习

## 使用Spring的IoC的实现账户的CRUD 

### 1.目标

- [ ] 够实现spring基于XML的IoC案例

### 2.分析

1. 环境搭建
2. 配置applicationContext.xml
   + 原来new对象的地方改为在配置中注入
3. 启动Spring工厂

### 3.实现

#### 3.1环境搭建

1. 创建Maven工程,导入坐标

```xml
  <dependencies>
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.23</version>
        </dependency>

        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.7</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

2. 创建数据库和编写实体类

   数据库

```sql
CREATE DATABASE day41;
USE day41;
CREATE TABLE account(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(40),
    money DOUBLE
)CHARACTER SET utf8 COLLATE utf8_general_ci;

INSERT INTO account(NAME,money) VALUES('zs',1000);
INSERT INTO account(NAME,money) VALUES('ls',1000);
INSERT INTO account(NAME,money) VALUES('ww',1000);
```

​		实体

```java
public class Account implements Serializable{
    private Integer id;
    private String name;
    private Double money;
}
```

3. 编写业务层代码

AccountService.java

```java
public interface AccountService {
    void save(Account account) throws Exception;

    void update(Account account) throws Exception;

    void delete(Integer id) throws Exception;

    Account findById(Integer id) throws Exception;

    List<Account> findAll() throws Exception;

}
```

​		AccountServiceImpl.java

```java
package com.itheima.service.impl;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import com.itheima.service.AccountService;

import java.util.List;

public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;
    // set方法注入
    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public void save(Account account) throws Exception {
        accountDao.save(account);
    }

    @Override
    public void update(Account account) throws Exception {
        accountDao.update(account);
    }

    @Override
    public void delete(Integer id) throws Exception {
        accountDao.delete(id);
    }

    @Override
    public Account findById(Integer id) throws Exception {
        return accountDao.findById(id);
    }

    @Override
    public List<Account> findAll() throws Exception {
        return accountDao.findAll();
    }
}

```



4. 编写持久层代码

AccountDao.java

```java
public interface AccountDao {

    void save(Account account) throws SQLException;

    void update(Account account) throws SQLException;

    void delete(Integer id) throws SQLException;

    Account findById(Integer id) throws SQLException;

    List<Account> findAll() throws SQLException;

}
```

​		AccountDaoImpl.java

```java
package com.itheima.dao.impl;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.sql.SQLException;
import java.util.List;

public class AccountDaoImpl implements AccountDao {

    // 使用DButils技术
    private QueryRunner queryRunner;
    // 增加set方法（属性注入）
    public void setQueryRunner(QueryRunner queryRunner) {
        this.queryRunner = queryRunner;
    }

    @Override
    public void save(Account account) throws SQLException {
        String sql="insert into account values(null, ?, ?)";
        Object[] params = {account.getName(), account.getMoney()};
        queryRunner.update(sql, params);
    }

    // 改金额、名字
    @Override
    public void update(Account account) throws SQLException {
        String sql="update account set name = ?, money = ? where id = ?";
        Object[] params = {account.getName(), account.getMoney(), account.getId()};
        queryRunner.update(sql, params);
    }

    @Override
    public void delete(Integer id) throws SQLException {
        queryRunner.update("delete from account where id = ?", id);
    }

    @Override
    public Account findById(Integer id) throws SQLException {
        Account account = queryRunner.query("select * from account where id =?",
                new BeanHandler<>(Account.class), id);
        return account;
    }

    @Override
    public List<Account> findAll() throws SQLException {
        List<Account> query = queryRunner.query("select * from account", new BeanListHandler<>(Account.class));
        return query;
    }
}

```

3.2配置步骤

+ 配置applicationContext.xml： 配置bean以及注入属性。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl" scope="singleton">
        <!--引用另外的bean-->
        <property name="queryRunner" ref="queryRunner"/>
    </bean>

    <!--
        new QueryRunner(连接池对象)
    -->
    <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
        <constructor-arg name="ds" ref="ds"/>
    </bean>

    <!--配置数据库参数-->
    <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
        <!--属性注入-->
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day41"/>
    </bean>


    <!--配置service-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>
</beans>
```

#### 3.3测试案例

```java
public class DbTest {
    @Test
    public void fun01() throws Exception {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        AccountService accountService = (AccountService) applicationContext.getBean("accountService");
        List<Account> list = accountService.findAll();
        System.out.println(list);
    }
}
```

### 4.小结

1. IOC实现依赖注入： 就是把以前的new变成了配置注入



#  第五章-Spring5 的新特性【了解】

## 知识点-Spring5 的新特性

### 1.目标

- [ ] 了解Spring5 的新特性

### 2.路径

1. 与 JDK 相关的升级
2. 核心容器的更新
3. JetBrains Kotlin 语言支持
4. 响应式编程风格
5. Junit5 支持
6. 依赖类库的更新

### 3.讲解

#### 3.1与 JDK 相关的升级

##### 3.1.1jdk 版本要求

​	spring5.0 在 2017 年 9 月发布了它的 GA（通用）版本。 

​	该版本是基于 jdk8 编写的， 所以 jdk8 以下版本将无法使用。 同时，可以兼容 jdk9 版本。
注：
​	我们使用 jdk8 构建工程，可以降版编译。但是不能使用 jdk8 以下版本构建工程。

​	Spring 大量使用了反射

##### 3.1.2利用 jdk8 版本更新的内容

- 基于 JDK8 的反射增强  

```java
public class Test {
    //循环次数定义： 10 亿次
    private static final int loopCnt = 1000 * 1000 * 1000;
    public static void main(String[] args) throws Exception {
        //输出 jdk 的版本
        System.out.println("java.version=" + System.getProperty("java.version"));
        t1();
        t2();
        t3();
    }
    // 每次重新生成对象
    public static void t1() {
        long s = System.currentTimeMillis();
        for (int i = 0; i < loopCnt; i++) {
            Person p = new Person();
            p.setAge(31);
        }
        long e = System.currentTimeMillis();
        System.out.println("循环 10 亿次创建对象的时间： " + (e - s));
    }
    // 同一个对象
    public static void t2() {
        long s = System.currentTimeMillis();
        Person p = new Person();
        for (int i = 0; i < loopCnt; i++) {
            p.setAge(32);
        }
        long e = System.currentTimeMillis();
        System.out.println("循环 10 亿次给同一对象赋值的时间： " + (e - s));
    }
    //使用反射创建对象
    public static void t3() throws Exception {
        long s = System.currentTimeMillis();
        Class<Person> c = Person.class;
        Person p = c.newInstance();
        Method m = c.getMethod("setAge", Integer.class);
        for (int i = 0; i < loopCnt; i++) {
            m.invoke(p, 33);
        }
        long e = System.currentTimeMillis();
        System.out.println("循环 10 亿次反射创建对象的时间： " + (e - s));
    }
    static class Person {
        private int age = 20;
        public int getAge() {
            return age;
        }
        public void setAge(Integer age) {
            this.age = age;
        }
    }
}
```



试运行结果如下：

```java
java.version=1.7.0_80
循环 10 亿次创建对象的时间： 7
循环 10 亿次给同一对象赋值的时间： 39
循环 10 亿次反射创建对象的时间： 135652
    
    
java.version=1.8.0_211
循环 10 亿次创建对象的时间： 11
循环 10 亿次给同一对象赋值的时间： 40
循环 10 亿次反射创建对象的时间： 2660
```





- @NonNull 注解和@Nullable 注解的使用 

  ​	用 @Nullable 和 @NotNull 注解来显示表明可为空的参数和以及返回值。这样就够在编译的时候处理空值而不是在运行时抛出 NullPointerExceptions。 

- 日志记录方面 

  ​	Spring Framework 5.0 带来了 Commons Logging 桥接模块的封装, 它被叫做 spring-jcl 而不是标准的 Commons Logging。当然，无需任何额外的桥接，新版本也会对 Log4j 2.x, SLF4J, JUL( java.util.logging) 进行自动检测。 

#### 3.2.核心容器的更新

​		Spring Framework 5.0 现在支持候选组件索引作为类路径扫描的替代方案。

​		该功能已经在类路径扫描器中添加，以简化添加候选组件标识的步骤。应用程序构建任务可以定义当前项目自己的 META-INF/spring.components 文件。在编译时，源模型是自包含的， JPA 实体和 Spring 组件是已被标记的。从索引读取实体而不是扫描类路径对于小于 200 个类的小型项目是没有明显差异。

​		但对大型项目影响较大。加载组件索引开销更低。因此，随着类数的增加，索引读取的启动时间将保持不变。	加载组件索引的耗费是廉价的。因此当类的数量不断增长，加上构建索引的启动时间仍然可以维持一个常数,不过对于组件扫描而言，启动时间则会有明显的增长。	

​		这个对于我们处于大型 Spring 项目的开发者所意味着的，是应用程序的启动时间将被大大缩减。虽然 20	或者 30 秒钟看似没什么，但如果每天要这样登上好几百次，加起来就够你受的了。使用了组件索引的话，就能帮助你每天过的更加高效。

​	你可以在 Spring 的 Jira 上了解更多关于组件索引的相关信息。 

#### 3.3.JetBrains Kotlin 语言支持

​	Kolin概述：是一种支持函数式编程编程风格的面向对象语言。 Kotlin 运行在 JVM 之上，但运行环境并不
限于 JVM。    

- Kotlin 的示例代码：

```java
// 示例一：
class Greeter(val name: String) {
    fun greet() {
        println("Hello, $name")
    }
}

fun main(args: Array<String>) {
    Greeter("").greet()          // 创建一个对象不用 new 关键字
}

// 运行输出: Hello, 张三


{

  ("/movie" and accept(TEXT_HTML)).nest {GET("/", movieHandler::findAllView)

	 		 GET("/{card}", movieHandler::findOneView)
 		}

  ("/api/movie" and accept(APPLICATION_JSON)).nest {

  GET("/", movieApiHandler::findAll)

  GET("/{id}", movieApiHandler::findOne)

  }

}
```

- Kolin 注册 bean 对象到 spring 容器：

```java
val context = GenericApplicationContext {
  registerBean()
  registerBean { Cinema(it.getBean()) }
}
```

#### 3.4.响应式编程风格

>Spring WebFlux特性
>
>- 特性一 异步非阻塞
>
>众所周知，SpringMVC是同步阻塞的IO模型，资源浪费相对来说比较严重，当我们在处理一个比较耗时的任务时，例如：上传一个比较大的文件，首先，服务器的线程一直在等待接收文件，在这期间它就像个傻子一样等在那儿（放学别走），什么都干不了，好不容易等到文件来了并且接收完毕，我们又要将文件写入磁盘，在这写入的过程中，这根线程又再次懵bi了，又要等到文件写完才能去干其它的事情。这一前一后的等待，不浪费资源么？
>
>没错，Spring WebFlux就是来解决这问题的，Spring WebFlux可以做到异步非阻塞。还是上面那上传文件的例子，Spring WebFlux是这样做的：线程发现文件还没准备好，就先去做其它事情，当文件准备好之后，通知这根线程来处理，当接收完毕写入磁盘的时候（根据具体情况选择是否做异步非阻塞），写入完毕后通知这根线程再来处理（异步非阻塞情况下）。
>
>- 特性二 响应式(reactive)函数编程
>
>Spring WebFlux，因为它支持函数式编程，得益于对于reactive-stream的支持（通过reactor框架来实现的）。
>
>- 特性三 不再拘束于Servlet容器
>
>以前，我们的应用都运行于Servlet容器之中，例如我们大家最为熟悉的Tomcat, Jetty...等等。而现在Spring WebFlux不仅能运行于传统的Servlet容器中（前提是容器要支持Servlet3.1，因为非阻塞IO是使用了Servlet3.1的特性），还能运行在支持NIO的Netty和Undertow中。

- 这里有一个使用 Spring 5.0 的 REST 端点的 WebClient 实现：

```java
package com.example.demo;

import org.springframework.http.MediaType;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;



public class RESTClient {
    public static void main(final String[] args) {
        final User user = new User();
        user.setUsername("admin");
        user.setPassword("admin");
        final WebClient client = WebClient.create("http://localhost:8080/mm_backend_management/user/login.do");
        final Mono<Result> createdUser = client.post()
                .uri("")
                .accept(MediaType.APPLICATION_JSON)
                .body(Mono.just(user), User.class)
                .exchange()
                .flatMap(response -> response.bodyToMono(Result.class));
        System.out.println(createdUser.block());   // 输出：Result{flag=true, message='登录成功', result=null}
    }
}


```

#### 3.5.Junit5 支持

​	完全支持 JUnit 5 Jupiter，所以可以使用 JUnit 5 来编写测试以及扩展。此外还提供了一个编程以及扩展模型， Jupiter 子项目提供了一个测试引擎来在 Spring 上运行基于 Jupiter 的测试。
​	另外， Spring Framework 5 还提供了在 Spring TestContext Framework 中进行并行测试的扩展。针对响应式编程模型， spring-test 现在还引入了支持 Spring WebFlux 的 WebTestClient 集成测试的支持，类似于 MockMvc，并不需要一个运行着的服务端。使用一个模拟的请求或者响应， WebTestClient就可以直接绑定到 WebFlux 服务端设施。
​	你可以在这里找到这个激动人心的 TestContext 框架所带来的增强功能的完整列表。当然， Spring Framework 5.0 仍然支持我们的老朋友 JUnit! 在我写这篇文章的时候， JUnit 5 还只是发展到了 GA 版本。**对于 JUnit4， Spring Framework 在未来还是要支持一段时间的，目前还是主流**。spring5要求junit必须是4.12以及以上。

#### 3.6.依赖类库的更新

- 终止支持的类库 

  ​	Portlet.
  ​	Velocity.
  ​	JasperReports.
  ​	XMLBeans.
  ​	JDO.
  ​	Guava.

- 支持的类库

  ​	Jackson 2.6+
  ​	EhCache 2.10+ / 3.0 GA
  ​	Hibernate 5.0+
  ​	JDBC 4.0+
  ​	XmlUnit 2.x+
  ​	OkHttp 3.x+
  ​	Netty 4.1+ 

  




# 扩展-MyBatis逆向工程

1. 做什么用?

   + 根据数据库 自动生成pojo, dao, dao映射文件
2. 在`资料\mybatisgernate-tool`

![1595429739799](img/1595429739799.png)

1. 使用说明
   1. 该目录已经包含了mybatis-gernate-core、mysql的驱动 jar包
   2. src目录是一个空目录
   3. 修改**generatorConfig.xml**的内容：
      1. 主要是数据库名、连接的用户名、密码；
      2. 需要生成对应pojo的包名
      3. 需要生成的映射文件的目录、接口所在的包名
      4. 指定表名、表名对应的pojo的类名
   4. 最后在该目录打开命令行，或者进入该目录。输入`拷贝内容到命令行执行.txt`的内容到命令行，执行即可。会在src下面生成对应的pojo类、接口、映射文件。



![1595429347969](img/1595429347969.png)

**参考：**[简化版本](https://www.cnblogs.com/smileberry/p/4145872.html)



# 总结

- Spring： 是一个开源的一站式分层框架

- IOC

  - 概念： 控制反转，创建依赖对象由原来的使用者转变为spring容器
  - spring的IOC快速入门
    - 引入依赖
    - 编写配置文件，放在resources目录
    - 配置bean（id、class）
    - 测试： new ClassPathXmlApplicationContext("")
  - bean标签的详解：属性
    - id、name：bean在spring容器中的唯一标志
    - class ：类的全限定名
    - scope：bean的范围，singleton(默认，单例)、prototype（多例）
    - bean的生命周期

- 依赖注入DI

  - DI：依赖注入，在注册bean的时候，顺便把依赖的属性赋值

  - 构造器注入

  - 属性注入set方法注入

    - 简单类型： 

      - ```xml
        <property name="" value=""/>
        ```

        

    - 引用bean

      - ```xml
        <property name="" ref="另外一个bean的id、name值"/>
        ```

        

    - list、set、数组

    - map、properties



