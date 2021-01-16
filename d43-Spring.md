---
typora-copy-images-to: img
---

# day43-Spring

# 学习目标

- [ ] 能够实现spring基于注解的IoC案例

- [ ] 能够编写spring的IOC的注解代码

- [ ] 能够编写spring使用注解实现组件扫描

- [ ] 能够说出spring的IOC的相关注解的含义

- [ ] 能够掌握Spring整合Junit

- [ ] 能够理解AOP相关概念

- [ ] 能够说出AOP相关术语的含义

  

  

# 第一章-Spring的IOC注解开发

​	学习基于注解的 IoC 配置，大家脑海里首先得有一个认知，即注解配置和 xml 配置要实现的功能都是一样的，都是要降低程序间的耦合。只是配置的形式不一样。
​	关于实际的开发中到底使用 xml 还是注解，每家公司有不同的习惯。所以这两种配置方式我们都需要掌握。 

​	==总而言之：注解方式和前面的XML方式实现的是一样的功能，所以很多配置都是可以一一对应的。==

## 案例-注解开发入门案例

### 1.目标

- [ ] 能够编写spring的IOC的注解代码(代替xml里面配置的Bean标签)

### 2.分析

1. 创建工程, 添加spring-context依赖坐标
2. 在类上面添加**@Component**
3. 在applicationContext.xml 使用context名称空间开启注解包扫描



### 3.实现

#### 3.1创建Maven工程,添加依赖

```xml
<dependencies>
    <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.2.RELEASE</version>
    </dependency>
 </dependencies>
```

#### 3.2使用@Component注解配置管理的资源

- eg: 需要给AccountServiceImpl

```java
package com.itheima.service.impl;


import com.itheima.service.AccountService;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

// 不写属性则默认为类名首字母小写：accountServiceImpl
// <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"/>
@Component("accountService")
public class AccountServiceImpl implements AccountService {
	// ......
}

```

#### 3.3引入context的命名空间

+ applicationContext.xml中需要引入context的命名空间,可在xsd-configuration.html中找到

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
">

    <!--开启注解组件扫描： base-package表示会扫描到它以及所有的子包的所有被注解标记的bean-->
    <context:component-scan base-package="com.itheima"/>
</beans>
```

#### 3.4 配置扫描

在bean标签内部，使用context:component-scan ，让spring扫描该基础包下的所有子包注解

```xml
<!--开启注解组件扫描： base-package表示会扫描到它以及所有的子包的所有被注解标记的bean-->
<context:component-scan base-package="com.itheima"/>
```
### 4.小结

1. 引入依赖坐标 spring-context
2. 需要在类加上注解@Component，如果不写属性值则bean名字就是类名首字母小写
3. 需要在配置文件中开启注解包扫描：`<context:component-scan>`



## 案例-注解开发进阶（更多注解）

### 1.目标

- [ ] 能够说出spring的IOC的相关注解的含义

### 2.路径

1. 用于创建对象的注解
2. 用于作用范围的注解
3. 和生命周期相关的注解

### 3.讲解

![1595509030229](img/1595509030229.png)

- ==@Component== ： 
  - 相当于在 xml 中配置一个 bean。
  - value属性：指定 bean 的 id。==如果不指定，默认是当前类的类名且首字母小写==。 

------------------

- ==@Controller== ：
  - Component 的子注解，习惯性为web层服务的
- ==@Service== ：
  - Component 的子注解，习惯性为service业务层服务的
- ==@Repository==：
  - Component 的子注解，习惯性为dao持久层服务的

-------------------

- ==@Scope==：
  - 表示bean的范围的注解，和xml中的scope属性一样的含义
  - 值可以写singleton、prototype

```java
@Scope("prototype")
@Component("accountService")
public class AccountServiceImpl implements AccountService {}
```
+ ==@PostConstruct==：
+ 作用在方法；和xml中的init-method属性一样的含义
  + 不是spring的注解；是java的注解；

- @PreDestroy：
  - 作用在方法；和xml中的destroy-method属性一样的含义
  - 不是spring的注解；是java的注解；



### 4.小结

1. spring把分层架构使用了对应的注解来区分
   1. web： @Controller
   2. service： @Service
   3. dao：  @Respository
2. scope属性对应的注解： @Scope
3. init-method属性对应的注解： @PostConstruct

## 知识点-注入属性的注解

### 1.目标

- [ ] 掌握使用注解注入属性

### 2.路径

1. @Value : 注入简单类型的数据
2. @Autowired 
3. @Qualifier
4. @Resource
5. @Primary

### 3.讲解

- ==@Value==：
  - 注入基本数据类型和 String 类型数据的
  - 属性value：用于指定值 , 可以通过表达式动态获得内容再赋值


```java
@Value("奥巴马")
private String name;  
```
- ==@Autowired==：
  - 自动按照==类型==注入。==**当使用注解注入属性时， set 方法可以省略**。它只能注入其他 bean 类型==。
  - 如果只有一个实现类, 可以自动注入成功
  - ==如果有两个或者两个以上的实现类, 找到变量名一致的id对象给注入进去, 如果找不到,就报错==

- 实例:

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Autowired
    //当有多个AccountDao实现类时候, @Autowired会在在Spring容器里面找id为accountDao的对象注入,找不到就报错
    private AccountDao accountDao;
    @Override
    public void save() {
        System.out.println("AccountServiceImpl---save()");
        accountDao.save();
    }
}
```

- ==@Qualifier==
  - **限定的意思。在自动按照类型注入(@Autowired)的基础之上，再按照Bean的id限定性注入**。
  - 它在给字段注入时不能独立使用，==必须和@Autowired一起使用==；但是给方法参数注入时，可以独立使用。

  - **属性value：指定bean的id。**

- 实例

```java
@Component("accountService")
public class AccountServiceImpl implements AccountService {
    @Autowired
    @Qualifier("accountDaoImpl")
    private AccountDao accountDao;
    @Override
    public void save() {
        System.out.println("AccountServiceImpl---save()");
        accountDao.save();
    }
}
```

- ==@Resource==
  - 不是spring的注解；是java的注解；==【基础面试题：Autowired、Qualifier、Resource的比较？】==
  - 如果上面一个接口有多种实现，那么现在需要指定找具体的某一个实现，那么可以使用@Resource
  - **name属性指定bean的 名称**

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Resource(name = "accountDaoImpl")
    private AccountDao accountDao;
    @Override
    public void save() {
        System.out.println("AccountServiceImpl---save()");
        accountDao.save();
    }
}
```

- ==@Primary==
  
  - 当一个接口存在多个实现时，可以在某一个bean中添加@Primary注解，表示优先选择。
  
  ```java
  @Service("accountService")
  public class AccountServiceImpl implements AccountService {
      @Autowired
      private AccountDao accountDao;
  }
  
  
  @Repository
  @Primary
  public class AccountDaoImpl2 implements AccountDao{...}
  
  
  
  ```
  
  



### 4.小结

1. 属性注入的注解
2. @Value：注入简单类型
3. @Autowired： 按类型自动注入
   1. 如果属性的实现类有一个，可以注入成功
   2. 如果属性的实现类有多个，则会先找和属性名一致的bean进行注入，找不到则报错
4. @Qulifier：会结合@Autowired一块使用，表示限定使用某个bean
5. @Resource： 等效于  @Qulifier+@Autowired
6. @Primary： 优先选择该实现类作为bean属性注入



## 知识点-XML、注解混合开发

### 1.目标

- [ ] 掌握混合(xml和注解)开发

### 2.路径

1.  注解和XML开发比较
2. 混合开发特点
3. 混合开发使用

### 3.讲解

#### 3.1注解和XML比较

 ![img](img/tu_3-1574384912018.png)

+ xml
  + 优点: 配置集中，方便维护, 改集中的xml文件
  + 缺点: 相对注解而言, 配置稍微麻烦一点
+ 注解
  + 优点: 简洁
  + 缺点: bean分散在各个类中，修改时需要修改java源代码
    
    

#### 3.2混合开发特点

一般而言，常规操作：

- 使用xml(==注册==bean)来管理bean

- 使用注解来==注入==属性   

#### 3.3混合开发环境搭建

+ 创建spring配置文件，编写头信息配置

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       ">
  </beans>
  ```

+ 配置扫描

  ```xml
  <context:component-scan base-package="com.itheima" />
  ```

+ 在xml配置中添加Bean的管理

+ 在被管理bean对应的类中，为依赖添加注解配置

### 4.小结

1. 在我们的XML进行Bean的配置： `<bean>`标签
2. 在我们的类里面对属性使用注解注入。



## 案例-使用注解改造账户练习操作

### 1.需求

- [ ] 使用注解改造练习

### 2.分析

1. 引入相关的依赖

2. 需要编写配置文件（开启注解包扫描）

3. 在文件里面配置第三方的类（queryRunner、连接池类）

4. 比如AccountService、AccountDao可以使用注解配置

5. 属性注入采用注解：@AutoWired....

   

### 3.实现

- pom.xml

```xml
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
```



+ 使用注解配置持久层

```java
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    @Autowired
    private QueryRunner queryRunner;
	...
}
```

+ 使用注解配置业务层

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Autowired
    private AccountDao accountDao;
    ...
}
```

+ 修改applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
">

    <!--开启注解组件扫描： base-package表示会扫描到它以及所有的子包的所有被注解标记的bean-->
    <context:component-scan base-package="com.itheima"/>


    <!--配置外部的类作为bean-->
    <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
        <constructor-arg name="ds" ref="ds"/>
    </bean>


    <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day41"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
</beans>
```

### 4.小结

1. 在配置文件中开启了注解包扫描：`<context:component-scan base-package="com.itheima"/>`
2. 在配置文件中注册第三方的类作为bean
3. 自己写的类，使用注解注册：@Service、@Repository
4. 属性依赖则采用注解注入：@Autowired

## 案例-使用纯注解

### 1.需求

- [ ] 使用纯注解

### 2.分析

​	基于注解的IoC配置已经完成，但是大家都发现了一个问题：我们依然离不开spring的xml配置文件，那么能不能不写这个bean.xml，所有配置都用注解来实现呢？

​	当然，同学们也需要注意一下，我们选择哪种配置的原则是简化开发和配置方便，而非追求某种技术  

​	学习纯注解开发原因: 

1. 方便大家学习后面的SpringBoot  
2. 以防万一真遇到了Spring纯注解开发



**注解-文件配置的映射：**

| 注解                     | 文件对应内容                                  | 备注                            |
| ------------------------ | --------------------------------------------- | ------------------------------- |
| @Configuration配置类     | 对应整个applicationContext.xml                |                                 |
| @ComponentScan标记配置类 | 对应文件中开启注解包bean扫描                  |                                 |
| @Bean+方法名             | 对应文件中的bean标签+id值；<br>id值对应方法名 | 在配置类中定义被@Bean标记的方法 |
| @Import                  | 引入其他的配置类                              |                                 |
| @PropertySource          | 加载properties文件的配置内容                  |                                 |

- 原来的xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
">

    <!--开启注解组件扫描： base-package表示会扫描到它以及所有的子包的所有被注解标记的bean-->
    <context:component-scan base-package="com.itheima"/>


    <!--配置外部的类作为bean-->
    <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
        <constructor-arg name="ds" ref="ds"/>
    </bean>


    <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day41"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
</beans>
```

### 3.实现

- ==@Configuration==

  - 作用：用于指定当前类是一个spring配置类，当创建容器时会从该类上加载注解。

    获取容器时需要使用==AnnotationConfigApplicationContext==(类.class)。

- 示例代码:

```java
@Configuration
public class SpringConfiguration {
}
```

- ==@ComponentScan==
  - 作用：用于指定spring在初始化容器时要扫描的包。作用和在spring的xml配置文件中的： `<context:component-scan base-package="com.itheima"/>`是一样的。
  - 属性basePackages：用于指定要扫描的包。和该注解中的value属性作用一样。
  - 属性basePackageClasses：指定扫描的类的Class，表示该类以及以下的包都会扫描。
  - @Configuration结合@ComponentScan一块使用

- 示例代码:

```java
package com.itheima;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration  // 标记该类是一个spring配置类，等效于以前的xml配置文件

// 注解包扫描，等效于文件中的<context:component-scan base-package="com.itheima"/>
//@ComponentScan(basePackages = {"com.itheima"})   // basePackages属性是string类型，写包名

//basePackageClasses属性写Class类型，一般而言写配置类的class
// 以下配置代表： 会扫描SpringConfiguration所在的包、包下面的子包的所有被spring相关注解标记的类，注册进容器中
@ComponentScan(basePackageClasses = {SpringConfiguration.class})
public class SpringConfiguration {
}

```



**测试：**

```java
// 传入一个配置类的Class作为参数，意思就是会去加载解析这个配置类
        // 解析其他注解，比如注解包扫描，把bean注册进入到容器中
        ApplicationContext applicationContext =
                new AnnotationConfigApplicationContext(SpringConfiguration.class);

        AccountService accountService = applicationContext.getBean(AccountService.class);


```



- ==@Bean==
  - 该注解只能写在方法上，表明使用此方法创建一个对象（一般会存在这样的代码：`return new XXX()`），并且放入spring容器。
- ==@Bean标记的方法的返回值就是一个bean，bean的名字默认就是方法名==
  - 属性：name：给当前@Bean注解方法创建的对象指定一个名称(即bean的id）。不写默认就是方法名。
- 示例代码:

```java
package com.itheima;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration  // 标记该类是一个spring配置类，等效于以前的xml配置文件

// 注解包扫描，等效于文件中的<context:component-scan base-package="com.itheima"/>
//@ComponentScan(basePackages = {"com.itheima"})   // basePackages属性是string类型，写包名

//basePackageClasses属性写Class类型，一般而言写配置类的class
// 以下配置代表： 会扫描SpringConfiguration所在的包、包下面的子包的所有被spring相关注解标记的类，注册进容器中
@ComponentScan(basePackageClasses = {SpringConfiguration2Bean.class})
public class SpringConfiguration2Bean {


    private String driverClass = "com.mysql.jdbc.Driver";
    private String url = "jdbc:mysql://localhost:3306/day41";
    private String username = "root";
    private String password = "root";


    // @Bean 注解表示给方法标记，返回值会作为一个bean注册到spring容器中
    // @Bean 不显示指定属性，则表示当前的bean的名字或者id就是 方法名
    // 如果显示指定属性值，则给bean取id、名字为属性值
    @Bean("queryRunner2")
    public QueryRunner queryRunner(@Autowired DataSource dataSource){
        return new QueryRunner(dataSource);
    }


    @Bean
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        druidDataSource.setDriverClassName(driverClass);
        druidDataSource.setUrl(url);
        return druidDataSource;
    }
}

```

- ==@Import==
  - 在配置类里面用；用于==导入其他配置类==，在引入其他配置类时，可以不用再写@Configuration注解（一般就要写）。

  - 属性value[]：用于指定其他配置类的Class。

- 示例代码:

```java
package com.itheima;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import javax.sql.DataSource;

@Configuration   // 表示该类是一个配置类相当于原来的applicationContext.xml文件
//@ComponentScan(basePackages="com.itheima")
// 该类以及所属子包的所有被注解标记的类都会被扫描
@ComponentScan(basePackageClasses={SpringConfiguration2.class})
// 引入其他的配置类
@Import(JdbcConfiguration.class)
public class SpringConfiguration2 {


}

```

```java
package com.itheima;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

/**
 * 这是一个jdbc配置类
 */
@Configuration
public class JdbcConfiguration {

    private String driverClass = "com.mysql.jdbc.Driver";
    private String url = "jdbc:mysql://localhost:3306/day41";
    private String username = "root";
    private String password = "root";



    // @Bean表示注册一个bean到容器里面，默认就是方法的名字就是bean的名字
    // @Bean属性一旦配置了，就表示bean的名字是 @Bean的属性
    @Bean("queryRunner2")
    public QueryRunner queryRunner(@Autowired DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    @Bean
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        druidDataSource.setDriverClassName(driverClass);
        druidDataSource.setUrl(url);
        return druidDataSource;
    }
}

```

测试SpringConfiguration2：

```java
@Test
public void test() throws Exception{
    // 意味着加载了SpringConfiguration类，使之生效
    ApplicationContext applicationContext
        = new AnnotationConfigApplicationContext(SpringConfiguration2.class);

    AccountService accountService = applicationContext.getBean(AccountService.class);


    System.out.println(accountService.findAll());
}
```



- ==@PropertySource==
  - ==property来源的意思==。用于加载`.properties`文件中的配置。例如我们配置数据源时，可以把连接数据库的信息写到properties配置文件中，就可以使用此注解指定properties配置文件的位置。

  - 属性value[]：用于指定properties文件位置。==如果是在类路径下，需要写上classpath:==

  - 示例代码:

resources目录下有jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/day41
jdbc.username=root
jdbc.password=root
```

JdbcConfig.java

使用时可以使用==@Value注解结合占位符表达式${key}获取配置的值==

```java
package com.itheima;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScans;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

import javax.sql.DataSource;

/**
 * 这是一个jdbc配置类
 */
@Configuration
// @PropertySource可以引入我们属性配置文件的值; 如果文件在classpath路径下，必须加上classpath:
@PropertySource("classpath:jdbc.properties")
public class JdbcConfiguration3 {

    // @Value注解+占位符表达式${key}
    @Value("${jdbc.driver}")
    private String driverClass;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;



    // @Bean表示注册一个bean到容器里面，默认就是方法的名字就是bean的名字
    // @Bean属性一旦配置了，就表示bean的名字是 @Bean的属性
    @Bean("queryRunner2")
    public QueryRunner queryRunner(@Autowired DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    @Bean
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        druidDataSource.setDriverClassName(driverClass);
        druidDataSource.setUrl(url);
        return druidDataSource;
    }
}


```

SpringConfiguration3：

```java

package com.itheima;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan(basePackageClasses = SpringConfiguration3.class)
@Import(JdbcConfiguration3.class)   // 引用其他的配置类
public class SpringConfiguration3 {
}
```

#### 3.6通过注解获取容器

```java
@Test
public void test() throws Exception{
    // 意味着加载了SpringConfiguration类，使之生效
    ApplicationContext applicationContext
        = new AnnotationConfigApplicationContext(SpringConfiguration3.class);

    AccountService accountService = applicationContext.getBean(AccountService.class);


    System.out.println(accountService.findAll());
}
```

### 3.小结

![image-20200109114541503](img/image-20200109114541503.png) 

 



# 第二章-Spring整合测试

## 知识点-Spring整合测试

### 1.目标

​	在测试类中，每个测试方法都有以下两行代码：

​	    ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");

 	   AccountService as= ac.getBean(AccountService.class);

### 2.分析

- 引入spring-test依赖
- 测试类中添加==@RunWith(SpringJUnit4ClassRunner.class)==注解
- 测试类中添加==@ContextConfiguration指定配置文件、或者配置类的class==
- 然后可以使用**@Autowired等注解进行依赖注入**

### 3.实现

+ 导入spring整合Junit的坐标

  ```xml
    	<dependency>
    		<groupId>org.springframework</groupId>
    		<artifactId>spring-test</artifactId>
    		<version>5.0.2.RELEASE</version>
    	</dependency>
  ```

+ 在测试类上面标记注解

  ```java
  package com.itheima;
  
  import com.itheima.service.AccountService;
  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.test.context.ContextConfiguration;
  import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
  
  @RunWith(SpringJUnit4ClassRunner.class)   // 指定一个运行环境
  // 用于指定一个spring配置上下文环境【xml文件、配置类】
  //@ContextConfiguration(classes = SpringConfiguration2PropertySource.class)
  @ContextConfiguration("classpath:applicationContext.xml")
  public class SpringTest01 {
  
      @Autowired
      private AccountService accountService;
  
      @Test
      public void test() throws Exception{
          accountService.findAll();
      }
  }
  
  ```



- 编写测试基类

```java
package com.itheima;

import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)   // 指定一个运行环境
// 用于指定一个spring配置上下文环境【xml文件、配置类】
//@ContextConfiguration(classes = SpringConfiguration2PropertySource.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class BaseTest {
}

```

其他的测试类只需要继承该类即可。

```java
package com.itheima;

import com.itheima.service.AccountService;
import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;

public class SpringTest02 extends BaseTest {

    @Autowired
    private AccountService accountService;

    @Test
    public void test() throws Exception{
        accountService.findAll();
    }
}

```





### 4.小结

1. ```java
   package com.itheima;
   
   import org.junit.runner.RunWith;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
@RunWith(SpringJUnit4ClassRunner.class)   // 指定一个运行环境
   // 用于指定一个spring配置上下文环境【xml文件、配置类】
   //@ContextConfiguration(classes = SpringConfiguration2PropertySource.class)
   @ContextConfiguration("classpath:applicationContext.xml")
   public class BaseTest {
   }
   
   ```
   
   



#### 4.1思考问题为什么不把测试类配到xml中

+ 在解释这个问题之前，先解除大家的疑虑，配到XML中能不能用呢？

  答案是肯定的，没问题，可以使用。

+ 那么为什么不采用配置到xml中的方式呢？

  这个原因是这样的：

    第一：当我们在xml中配置了一个bean，spring加载配置文件创建容器时，就会创建对象。

    第二：测试类只是我们在测试功能时使用，而在项目中它并不参与程序逻辑，也不会解决需求上的问题，所以创建完了，并没有使用。那么存在容器中就会造成资源的浪费。

    所以，基于以上两点，我们不应该把测试配置到xml文件中。



# 第三章-AOP相关的概念

## 知识点-AOP概述

### 1.目标

- [ ] 能够理解AOP相关概念

### 2.路径

1. 什么是AOP
2. AOP的作用和优势
3. AOP实现原理

### 3.讲解

#### 3.1什么是AOP

​	

![img](img/tu_1-1574384912033.png)

​	

​	AOP：全称是Aspect Oriented Programming, 即面向切面编程。==在不修改源码的基础上，对我们的已有方法进行增强==。

​	说白了就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，进行增强

![1602406412290](img/1602406412290.png)

#### 3.2AOP的作用和优势

**作用：**

    在程序运行期间，不修改源码对已有方法进行增强。 

**优势：**

    减少重复代码      

    提高开发效率      

    维护方便

#### 3.3AOP实现原理

​	使用动态代理技术 

+ JDK动态代理: 必须需要接口的
+ Cglib动态代理: 不需要接口的,只需要类就好了 

### 4.小结

1. AOP: 就是面向切面编程
2. 优势： 可以在不修改业务源代码前提下，可以增强其功能
3. AOP技术：动态代理实现



## 知识点-AOP的具体应用

### 1.需求

​	性能需求：打印AccountServiceImpl的每个方法的执行时间。

### 2.分析

在AOP 这种思想还没有出现的时候，我们解决 切面的问题需要修改代码。

### 3.实现

古老方式：修改代码，增加打印时间的代码。

不修改原有代码也想达到目标？装饰者模式、动态代理模式。

AOP底层实现技术采用的动态代理。

AOP 的底层动态代理实现有两种方案。 

- JDK的动态代理。 这种是早前我们在前面的基础增强说过的。 这一种主要是针对有接口实现的情况。 它的底层是创建接口的实现代理类， 实现扩展功能。也就是我们要增强的这个类，实现了某个接口，那么我就可以使用这种方式了. 

- cglib 的动态代理，这种主要是针对没有接口的方式，那么它的底层是创建被目标类的子类，实现扩展功能.

#### 3.1JDK方式  

==要求: 必须有接口==   

==API： **Proxy.newProxyInstance(...)**==

```java
package com.itheima.aop;

import com.itheima.bean.Account;
import com.itheima.service.AccountService;
import com.itheima.service.impl.AccountServiceImpl;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class JdkProxy {

    @Test
    public void test() throws Exception {
        ApplicationContext applicationContext =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        AccountService accountService = applicationContext.getBean(AccountService.class);

        // 使用JDK动态代理增强其方法行为：打印方法执行耗时
        // 参数1：对象所在的类加载器
        // 参数2：接口
        // 参数3：调用处理器对象（使用匿名内部类new InvocationHandler()）
        AccountService proxyObj = (AccountService)Proxy.newProxyInstance(accountService.getClass().getClassLoader(),
                accountService.getClass().getInterfaces(),
                new InvocationHandler() {


                    // Object proxy不要用
                    @Override
                    // 写增强逻辑：method被增强的方法、args是参数
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        long time = System.currentTimeMillis();
                        Object obj = method.invoke(accountService, args);  // 调用原有方法
                        System.out.println("方法耗时:" + (System.currentTimeMillis() - time));

                        return obj;
                    }
                });

        proxyObj.findAll();
    }
}

```

#### 3.2CgLib方式【了解】

继承原有的类，把原有的类作为父类，来达到增强的功能。

步骤：

1. 创建enhancer对象
           `Enhancer enhancer  = new Enhancer();`

2. 设置代理的父类
           `enhancer.setSuperclass(AccountDaoImpl.class);`

3. 设置回调方法：`enhancer.setCallback(new MethodInterceptor(){....//匿名内部类})`

4. 创建代理对象 `enhancer.create()`

+ 添加坐标

```xml
  <dependencies>
    <!--Spring核心容器-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--SpringAOP相关的坐标-->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.7</version>
    </dependency>
    
    <!--Spring整合单元测试-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.2.RELEASE</version>
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

- 使用CgLib方式实现: 第三方的代理机制，不是jdk自带的.  没有实现接口的类产生代理，使用的是字节码的增强技术，其实就是产生这个类的子类。

  ==不需要有接口==

```java
package com.itheima.aop;

import com.itheima.service.AccountService;
import org.junit.Test;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.beans.EventHandler;
import java.lang.reflect.Method;

public class Cglibproxy {

    @Test
    public void test() throws Exception {
        ApplicationContext applicationContext =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        AccountService accountService = applicationContext.getBean(AccountService.class);


        /**
         * Cglib动态代理演示-了解
         */
        //1. 创建增强类
        Enhancer enhancer = new Enhancer();
        //2. 设置父类为目标类型
        enhancer.setSuperclass(AccountService.class);
        //3. 设置回调方法
        enhancer.setCallback(new MethodInterceptor() {
            @Override
            public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                long time = System.currentTimeMillis();
                Object obj = method.invoke(accountService, objects);  // 调用原有方法
                System.out.println("方法耗时:" + (System.currentTimeMillis() - time));

                return obj;
            }
        });
        // 4. 得到代理对象
        AccountService proxy = (AccountService)enhancer.create();
        proxy.findAll();
    }
}

```

### 4.小结

1. AOP的底层就是封装了动态代理

   + JDK的动态代理
   + CgLib的动态代理
2. JDK动态代理、Cglib动态代理比较：
1. JDK动态代理必须要求实现接口，成为接口的实现来进行增强的
   2. Cglib动态代理只要有类即可，成为目标类型的子类来增强的

## 知识点-AOP术语

### 1.目标

- [ ] 能够说出AOP相关术语的含义

### 2.路径

1. Spring中的AOP说明
2. AOP中的术语

### 3.讲解

#### 3.1 学习spring中的AOP要明确的事

+ 开发阶段（我们做的）

  ​	编写核心业务代码（开发主线）：大部分程序员来做，要求熟悉业务需求。
  ​	是主体业务之外的需求，制作成==通知==。
  ​	在配置文件中，声明==切入点==与通知间的关系，即==切面==。

+ 运行阶段（Spring框架完成的）

  ​	Spring框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

​	在spring中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

#### 3.2.AOP中的术语

- **JoinPoint: 连接点**

  ​	类里面哪些方法可以被增强，这些方法称为连接点. ==在spring的AOP中，指的是所有现有的方法==。

- **Pointcut: 切入点**

  ​	 ==实际被增强的方法就称之为切入点==-----比如对哪个方法进行了处理耗时的增强，那么这个方法就可以称为切入点

- **Advice: 通知/增强**

  ​	==增加的额外处理逻辑==----比如记录耗时操作，那么就是一个通知。

  ​	通知分为:

  ​		**前置通知**： 在原来方法之前执行. 

  ​		**后置通知**： 在原来方法之后执行. 特点: 可以得到被增强方法的返回值

  ​		**环绕通知**：在方法之前和方法之后执行. 特点:可以阻止目标方法执行

  ​		**异常通知**： 目标方法出现异常执行. 如果方法没有异常,不会执行. 特点:可以获得异常的信息

  ​		**最终通知**： 指的是无论是否有异常，总是被执行的。


- **Aspect: 切面（切面类@Aspect）**

  ​	切入点和通知的结合。

  ==spring可以声明切面类@Aspect，在切面类里面定义通知（方法）、切入点（实际被增强的方法）。==
  
  ![1595578559992](img/1595578559992.png)

### 4.小结

1. 使用AOP: 逻辑需要我们写的, 怎么切是Spring做(运行阶段), 源码阶段看不到
2. 相关术语：
   1. 连接点
   2. 切入点
   3. 通知
   4. 切面



# 今天所有的注解清单

---------------------

- 注解方式开发IOC步骤
  - 引入依赖坐标
  - 需要在配置文件中开启注解包扫描
  - 可以给你的类加注解@Component



开启注解组件扫描： base-package表示会扫描到它以及所有的子包的所有被注解标记的bean
<context:component-scan base-package="com.itheima"/>

----------------------

@Component
@Controller : 为web层服务
@Service： 为service层服务
@Repository： 为dao层服务
@Scope： 对应xml中的bean标签的scope属性
@PostConstruct： 对应xml中的bean标签的init-method属性
@PreDestroy: 对应xml中的bean标签的desyroy-method属性

-------------

@Value： 注入简单类型，类似于property子标签的name-value属性配置
@Autowired : 根据类型自动注入， 如果接口类型只有一个实现类则可以注入成功；存在多个实现类的bean，会先去找和属性同名的bean注入，找不到则报错
@Qualifier： 限定。结合@Autowired一块使用
@Resource：显示指定注入某个bean
@Primary：优先选择

--------------------------

@Configuration：代表一个配置类，等效于原来的xml配置文件
@ComponentScan：开启注解包扫描，等效于<context:component-scan base-package="com.itheima"/>
@Bean：可以注册任意的类作为bean
@Import：引入其他的配置类
@PropertySource：引入外部的属性配置文件，使用${key}获取值注入结合@Value

----------------------------

@RunWith
@ContextConfiguration

--------------------------------------



- AOP
  - 面向切面编程
  - AOP实现技术：动态代理
  - AOP术语：
    - 连接点：任意方法
    - 切入点： 实际被增强的方法
    - 通知：实际增强的逻辑
    - 切面：是一个类，切面类，定义通知、切入点

