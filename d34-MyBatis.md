---
typora-copy-images-to: img
---

# day34-MyBatis 

# 学习目标

- [ ] 能够了解什么是框架
- [ ] 掌握Mybatis框架开发快速入门
- [ ] 掌握Mybatis框架的基本CRUD操作
- [ ] 掌握mybatis核心配置文件的编写 
- [ ] 掌握Mybatis的resultType的配置
- [ ] 了解Mybatis连接池与事务操作
- [ ] 掌握Mybatis动态SQL

# 第一章-框架概述 

## 知识点-框架概述

### 1.目标

- [ ] 能够了解什么是框架

### 2.路径

1. 什么是框架 
2. 框架要解决的问题 

### 3.讲解

#### 3.1什么是框架 

​	框架（Framework）是整个或部分系统的可重用设计，表现为一组抽象构件及构件实例间交互的方法;另一种定义认为，框架是可被应用开发者定制的应用骨架。前者是从应用方面而后者是从目的方面给出的定义。

​	==简而言之，框架是软件(系统)的半成品，框架封装了很多的技术细节，使开发者可以使用简单的方式实现功能,大大提高开发效率。==

​	开发好比表演节目, 开发者好比演员, 框架好比舞台.

#### 3.2框架要解决的问题 

​	==框架要解决的最重要的一个问题是技术整合的问题==，在 J2EE 的 框架中，有着各种各样的技术，不同的软件企业需要从J2EE 中选择不同的技术，这就使得软件企业最终的应用依赖于这些技术，技术自身的复杂性和技术的风险性将会直接对应用造成冲击。而应用是软件企业的核心，是竞争力的关键所在，因此应该将应用自身的设计和具体的实现技术解耦。这样，软件企业的研发将集中在应用的设计上，而不是具体的技术实现，技术实现是应用的底层支撑，它不应该直接对应用产生影响。

​	 ==框架一般处在低层应用平台（如 J2EE）和高层业务逻辑之间的中间层。==

![1594429577991](img/1594429577991.png)

### 4.小结

1. 框架： 就是封装了一些基础技术，是软件的半成品
2. SSM框架

![image-20191226084539029](img/image-20191226084539029.png) 



## 知识点-MyBatis框架概述

### 1.目标

- [ ] 能够了解什么是MyBatis

### 2.路径

1. jdbc 程序回顾
2. MyBatis框架概述 

### 3.讲解

#### 3.1jdbc 程序回顾

##### 3.1.1 jdbc预编译编程步骤回顾

+ 引入jar包
+ 注册驱动
+ 获取连接
+ 创建预编译语句对象
+ 设置参数
+ 执行，处理结果
+ 关闭资源

```java
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        try {
            //1.加载数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.通过驱动管理类获取数据库链接
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "123456"); 
            //3.定义 sql 语句 ?表示占位符
            String sql = "select * from user where username = ?";
            //4.获取预处理 statement
            preparedStatement = connection.prepareStatement(sql);
            //5.设置参数，第一个参数为 sql 语句中参数的序号（从 1 开始），第二个参数为设置的参数值
            preparedStatement.setString(1, "王五");
            //6.向数据库发出 sql 执行查询，查询出结果集
            resultSet = preparedStatement.executeQuery();
            //7.遍历查询结果集
            while (resultSet.next()) {
                System.out.println(resultSet.getString("id") + "
                        "+resultSet.getString(" username"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
		//8.释放资源
            if (resultSet != null) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (preparedStatement != null) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

##### 3.1.2 jdbc 问题分析

1. 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。
2. ==Sql 语句在代码中硬编码==，造成代码不易维护，实际应用 sql 变化的可能较大， sql 变动需要改变java 代码。 
3. 使用 preparedStatement 向占有位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
4. 对结果集解析存在硬编码（查询列名）， sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成 pojo 对象解析比较方便  

#### 3.2MyBatis框架概述

​	**mybatis 是一个优秀的基于 java 的==持久层框架==，它内部封装了 jdbc，使开发者只需要关注 sql 语句本身，而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。**

​	mybatis 通过xml 或注解的方式将要执行的各种statement 配置起来，并通过java 对象和statement 中sql的动态参数进行映射生成最终执行的 sql语句，最后由 mybatis 框架执行 sql并将==结果==映射为 java 对象并返回。

![1578280975994](img/1578280975994.png)

​	采用 ORM 思想（object relation mapping对象关系映射）解决了实体和数据库映射的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与 jdbc api打交道，就可以完成对数据库的持久化操作。

​	官网: http://www.mybatis.org/mybatis-3/

### 4.小结

1. mybatis： 持久层DAO的一个框架，封装了jdbc技术

# 第二章-Mybatis入门【重要】

## 案例-Mybatis快速入门【新知识的快速入门案例要认真对待】

### 1.需求

- [ ] 使用MyBatis查询所有的用户, 封装到List集合

### 2.分析

1. 引入依赖坐标（mybatis、jdbc驱动包）
2. 创建javabean（和表进行对应、映射）
3. ==创建接口UserDao，可以放在com.itheima.dao包里面==
4. ==创建一个mybatis映射文件UserDao.xml（写sql语句），放在resources目录的com/itheima/dao目录。==
5. 创建mybatis核心配置文件：放在==resources目录==，mybatis-config.xml
6. 写代码测试

### 3.实现

#### 3.1准备工作

+ 数据库

```sql
CREATE DATABASE mybatis_day01;
USE mybatis_day01;
CREATE TABLE t_user(
		uid int PRIMARY KEY auto_increment,
		username varchar(40),
	 	sex varchar(10),
		birthday date,
		address varchar(40)
);

INSERT INTO `t_user` VALUES (null, 'zs', '男', '2018-08-08', '北京');
INSERT INTO `t_user` VALUES (null, 'ls', '女', '2018-08-30', '武汉');
INSERT INTO `t_user` VALUES (null, 'ww', '男', '2018-08-08', '北京');



CREATE TABLE `t_product` (
  `f_id` int(11) NOT NULL AUTO_INCREMENT,
  `f_name` varchar(20) DEFAULT NULL,
  `f_price` decimal(10,0) DEFAULT NULL,
  PRIMARY KEY (`f_id`)
) ;


insert  into `t_product`(`f_id`,`f_name`,`f_price`) values (1,'JAVA从入门到放弃','39'),(2,'MySQL从入门到删库跑路','99');

```

#### 3.2.MyBatis快速入门

 目录结构图如下：

![1600915003464](img/1600915003464.png)



##### 3.2.1创建Maven工程(jar)导入坐标

```xml
  <dependencies>
    <!--MyBatis坐标-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.5</version>
    </dependency>
    <!--mysql驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
    </dependency>
  	<!--单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
      <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.8</version>
      <scope>provided</scope>
    </dependency>

  </dependencies>
```

##### 3.2.2创建User实体类

+ User .java

```java
public class User implements Serializable{
    private Integer uid; //用户id
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址
	
}
```

##### 3.2.3创建 UserDao 接口

- UserDao 接口就是我们的持久层接口（有些习惯也可以写成 UserMapper) .我们就写成UserDao ,具体代码如下： 

```java
public interface UserDao {
    public List<User> findAll();
}
```

##### 3.2.4创建 UserDao.xml 映射文件

注意:  ==在src/main/resources资源文件目录创建，如果接口是com.itheima.dao包下，则对应的映射文件也要处于com/itheima/dao目录中==。该文件要放在com/itheima/dao里面, 不要写成com.itheima.dao; **保证编译后在classpath目录下，接口和映射文件在同一个目录下。**

 ![1544705827942](img/1544705827942-1574303439289.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace属性：名称空间， 写接口的全限定名-->
<mapper namespace="com.itheima.dao.UserDao">

    <!--select标签用于插叙；  id属性值就是接口的方法名，
        resultType ：返回值的类型
                    如果是集合，写泛型即可；
    -->
    <select id="findAll" resultType="com.itheima.bean.User">
        select * from t_user
    </select>
</mapper>
```

##### 3.2.5创建 mybatis-config.xml 配置文件

- mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--引入外部的属性配置文件-->
    <properties resource="jdbc.properties"/>



    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--数据库连接的配置-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_day01"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射文件所在位置 -->
    <mappers>
        <!--指定映射文件所在位置-->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

##### 3.2.6测试

- MyBatisDemo01.java

```java
package com.itheima;

import com.itheima.bean.User;
import com.itheima.dao.UserDao;
import com.sun.org.apache.bcel.internal.generic.NEW;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class MyBatisTest01 {
    @Test
    public void test() throws Exception{
        //1. 读取文件核心配置文件（Resources内置的一个类）
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");

        //2.获取工厂(构造者模式)
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);


        //3. 获取sqlSession（类似于你的连接对象）
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4. 获取接口的代理对象（jdk动态代理）
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        //5. 操作方法
        List<User> users = userDao.findAll();

        System.out.println(users);



        //6. 关闭资源
        sqlSession.close();

    }
}

```

### 4.小结

#### 4.1步骤

1. 引入依赖jar包（mybatis、jdbc驱动）
2. 写一个javabean和表进行对应
3. 写一个接口UserDao，放在com.itheima.dao包下面
4. 在resources目录下面的com/itheima/dao写UserDao.xml（sql映射文件）
5. 在resources目录下面写mybatis核心配置文件

#### 4.2注意事项

Dao的映射文件的路径

![image-20191226092307424](img/image-20191226092307424.png) 





## 知识点-Mapper动态代理方式规范（UserDao.xml的规范）

### 1.目标

- [ ] 掌握Mapper(Dao)动态代理方式规范

### 2.路径

1. 入门案例回顾
2. 规范

### 3.讲解

#### 规范

Mapper（或者Dao）接口开发需要遵循以下规范： 

1. Mapper.xml文件中的==namespace==必须和mapper(Dao)==接口的全限定名==相同。
2. Mapper.xml文件中select,update等的==标签id的值必须和mapper(Dao)接口的方法名相同==
3. Mapper.xml文件中select,update等的标签的parameterType必须和mapper(Dao)接口的方法的形参类型对应；  **parameterType可以不写，mybatis会自动推断，如果写了但是写错了会出错。**
4. Mapper.xml文件中select,update等的==标签的resultType==必须和mapper(Dao)==接口的方法的返回值类型==对应，集合`list<User>`可以写User（泛型）
5. Mapper.xml文件的文件名和mapper(Dao)接口的名字一样
6. Mapper.xml文件的路径和mapper(Dao)接口的路径在同一层目录

![1594434811075](img/1594434811075.png)

#### 3.1入门案例回顾

##### 3.1.2Mapper.xml(映射文件)

​	定义mapper映射文件UserDao.xml，需要修改namespace的值为 UserDao接口全限定名。将UserDao.xml放在classpath的xxx.xxx.dao目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace属性：名称空间， 写接口的全限定名-->
<mapper namespace="com.itheima.dao.UserDao">

    <!--select标签用于插叙；  id属性值就是接口的方法名，
        resultType ：返回值的类型
                    如果是集合，写泛型即可；
    -->
    <select id="findAll" resultType="com.itheima.bean.User">
        select * from t_user
    </select>

</mapper>
```

##### 3.1.2Mapper.java(dao接口)

```java
public interface UserDao {
    List<User> findAll();
}

```

##### 3.2.3测试

```java
package com.itheima;

import com.itheima.bean.User;
import com.itheima.dao.UserDao;
import com.sun.org.apache.bcel.internal.generic.NEW;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class MyBatisTest01 {
    @Test
    public void test() throws Exception{
        //1. 读取文件核心配置文件（Resources内置的一个类）
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");

        //2.获取工厂(构造者模式)
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);


        //3. 获取sqlSession（类似于你的连接对象）
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4. 获取接口的代理对象（jdk动态代理）
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        //5. 操作方法
        List<User> users = userDao.findAll();

        System.out.println(users);



        //6. 关闭资源
        sqlSession.close();

    }
}

```



### 4.小结

1. 我们使用MyBatis 遵循这些规范

## 知识点-核心配置文件详解

### 1.目标

- [ ] 掌握mybatis-config.xml配置文件

### 2.路径

1. 核心配置文件的顺序
2. properties
3. typeAliases
4. Mapper

### 3.讲解

#### 3.1.核心配置文件的顺序

![image-20191226095424936](img/image-20191226095424936.png)  

**properties（引入外部properties文件）**

settings（全局配置参数）

**typeAliases（类型别名）**

typeHandlers（类型处理器）

objectFactory（对象工厂）

plugins（插件）

**environments（环境集合属性对象）**： 

​	environment（环境子属性对象）

​		transactionManager（事务管理）

​		dataSource（数据源）

**mappers（映射器）**

#### 3.2.properties

- jdbc.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis_day01
username=root
password=root
```

- 引入到核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--引入外部的属性配置文件-->
    <properties resource="jdbc.properties"/>


    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--数据库连接的配置-->
                <!--<property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_day01"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>-->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射文件所在位置 -->
    <mappers>
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

#### 3.3.typeAliases（类型别名）

别名不区分大小写。

##### 3.3.1定义单个别名

- 核心配置文件

```xml
<typeAliases>
        <!--取别名，在映射文件中就可以写user-->
        <typeAlias type="com.itheima.bean.User" alias="user"/>
        
    </typeAliases>
```

- 修改UserDao.xml

```java
<select id="findAll" resultType="User">
        select * from t_user
    </select>
```

##### 3.3.2批量定义别名

使用package定义的别名：就是pojo的类名，大小写都可以

- 核心配置文件

```xml
<typeAliases>
        
        <!--批量设置别名， 别名就是类名-->
        <package name="com.itheima.bean"/>
    </typeAliases>
```

- 修改UserDao.xml

```java
<select id="findAll" resultType="User">
        select * from t_user
    </select>
```

#### 3.4.Mapper

##### 3.4.1方式一:引入映射文件路径

```xml
<mappers>
     <mapper resource="com/itheima/dao/UserDao.xml"/>
 </mappers>
```

##### 3.4.2方式二:扫描接口（推荐）

注: 此方式只能用作:代理开发模式【要求接口、mapper文件名字、目录要一致】

- 配置单个接口

```xml
<mappers>
 	<mapper class="com.itheima.dao.UserDao"></mapper>
</mappers>
```

- ==**批量配置【要求接口、mapper文件名字、目录要一致】**==

```xml
<mappers>
   <package name="com.itheima.dao"></package>
</mappers>
```

#### 3.5 environments环境配置

==尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境==；所以，如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。而如果是三个数据库，就需要三个实例，依此类推；<u>可以这么理解一个数据库一个SqlSessionFactory 实例</u>。

```xml
<environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
```

​		可以选择使用指定id的数据库实例：is 表示核心配置文件输入流， 第二个字符串类型参数是配置environment的id属性值。

 ```java
new SqlSessionFactoryBuilder().build(is, "test");
 ```

​		

扩展：mybatis核心配置文件的内容如transactionManager、dataSource的配置信息可以在Configuration类的构造器中查看：

```java
public Configuration() {
    // transactionManager
    typeAliasRegistry.registerAlias("JDBC", JdbcTransactionFactory.class);
    typeAliasRegistry.registerAlias("MANAGED", ManagedTransactionFactory.class);
	
    // dataSource
    typeAliasRegistry.registerAlias("JNDI", JndiDataSourceFactory.class);
    typeAliasRegistry.registerAlias("POOLED", PooledDataSourceFactory.class);
    typeAliasRegistry.registerAlias("UNPOOLED", UnpooledDataSourceFactory.class);
    // .......
}
```



### 4.小结

1. 核心配置文件的顺序

![image-20191226101317581](img/image-20191226101317581.png) 



2. properties

```xml
<!--引入外部的属性配置文件-->
    <properties resource="jdbc.properties"/>
```

3. 别名typeAliases

```xml
<typeAliases>
        <!--取别名，在映射文件中就可以写user-->
        <typeAlias type="com.itheima.bean.User" alias="user"/>
        
        <!--批量设置别名， 别名就是类名-->
        <package name="com.itheima.bean"/>
    </typeAliases>
```

4. mappers

```xml
<mappers>
        <!--配置映射文件、接口所在目录（二者目录结构一致）-->
        <package name="com.itheima.dao"/>
    </mappers>
```

5. 环境

```xml
<!--默认采用default属性development-->
    <environments default="development">
        <environment id="development">
            <!--事务管理器-->
            <transactionManager type="JDBC"/>
            <!--数据库配置项-->
            <dataSource type="POOLED">
                <!--OGNL表达式来引入外部属性资源文件的配置项${}-->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
        <environment id="id2">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--OGNL表达式来引入外部属性资源文件的配置项${}-->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
```



# 第三章-MyBatis进阶【重要】

## 案例-使用Mybatis完成CRUD

### 1.需求

- [ ] 使用Mybatis完成CRUD

### 2.分析

1. 在Dao接口定义方法
2. 在Dao映射文件配置

### 3.实现

#### 3.1新增用户

##### 3.1.1实现步骤

- UserDao中添加新增方法 

```java
public interface UserDao {
    // 保存用户
    void save(User user);
}
```

- 在 UserDao.xml 文件中加入新增配置 

```xml
<!--#{}是占位符表达式，就类似于获得对应的参数值（类似于给预编译语句的？占位符设置对应的参数）-->
<insert id="save">
        insert into t_user values(
            null, #{username},
            #{sex}, #{birthday}, #{address}
        )
</insert>
```

- 测试

```java
@Test
    public void testInsert() throws Exception{
        //1. 读取mybatis核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");

        //2. 获取sqlsessionFactory
        SqlSessionFactory  sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        //3. 获取sqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4. 获取代理对象'
        UserDao userDao = sqlSession.getMapper(UserDao.class);


        //5. 执行方法
        User user = new User();
        user.setUsername("zsf");
        user.setAddress("武当");
        userDao.save(user);

        // mybatis默认是手动开启了事务，DML操作执行后需要手动提交事务
        sqlSession.commit();


        //6. 资源关闭
        sqlSession.close();

    }
```

##### 3.1.2新增用户 id 的返回值 

​	新增用户后， 同时还要返回当前新增用户的 id 值，因为 id 是由数据库的自动增长来实现的，所以就相当于我们要在新增后将自动增长 auto_increment 的值返回。 

- 方式一 ==SelectKey标签获取主键==，写在insert标签里面

```xml
<selectKey keyProperty="自增id属性" order="AFTER" resultType="int">
    select last_insert_id()
</selectKey>
```



| **属性**    | **描述**                                                     |
| ----------- | ------------------------------------------------------------ |
| keyProperty | selectKey 语句结果应该被设置的目标属性。                     |
| resultType  | 结果的类型。MyBatis 通常可以算出来,但是写上也没有问题。MyBatis 允许任何简单类型用作主键的类型,包括字符串。 |
| order       | 这可以被设置为 BEFORE 或 AFTER。如果设置为 BEFORE,那么它会首先选择主键,设置 keyProperty 然后执行插入语句。==如果设置为 AFTER,那么先执行插入语句，再获取id，mysql数据库方式==,然后是 selectKey 元素-这和如  Oracle 数据库相似,可以在插入语句中嵌入序列调用。 |

  UserDao.xml

```xml
    <!--parameterType属性: 参数的类型 ;  赋值的时候直接写对象里面的属性名-->
    <insert id="save" >
        <!--presultType: 主键类型; keyProperty:pojo里面对应的id的属性名; order属性: 指定是在目标的sql语句之前还是之后执行 -->
        <selectKey resultType="int" keyProperty="uid" order="AFTER">
            SELECT LAST_INSERT_ID()
        </selectKey>
        INSERT INTO t_user(username,sex,birthday,address)VALUES(#{username},#{sex},#{birthday},#{address})
    </insert>
```

- 方式二属性配置（**==mysql最常用的方式==**）

  在insert标签中添加属性：==keyProperty="id属性" useGeneratedKeys="true"==
  
  UserDao.xml

```xml
<!--keyProperty属性id   useGeneratedKeys使用自动生成的key-->
    <insert id="save" keyProperty="uid" useGeneratedKeys="true">
        insert into t_user values(
            null, #{username},
            #{sex},#{birthday},#{address}
        )
    </insert>
```

##### 3.1.3新增用户 id 的返回值(字符串类型)-仅作了解--不用看

```xml
<insert id="save" parameterType="com.itheima.pojo.User">
    <selectKey  keyProperty="id" resultType="string" order="BEFORE">
        select uuid()
    </selectKey>
        INSERT INTO t_user(username,sex,birthday,address) VALUES (#{username},#{sex},#{birthday},#{address})
</insert>
```

#### 3.2修改用户

- UserDao中添加新增方法 

```java
public interface UserDao {
    int update(User user);
}
```

- 在 UserDao.xml 文件中加入新增配置 

```xml
<!--修改使用update标签-->
    <update id="update">
        update t_user set sex = #{sex}, address = #{address}
        where uid = #{uid}
    </update>
```

- 测试

```java
@Test
    public void testUpdate() throws Exception{

        // 1. 加载核心配置文件
        InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");

        // 2. 得到一个sqlsession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);


        // 3. 获取sqlsession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 4. 获得代理对象
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        // 5. 操作
        User user = new User();
        user.setUid(9);
        user.setSex("女");
        user.setAddress("上海");
        System.out.println(userDao.update(user));

        // mybatis默认不会自动提交事务，需要手动处理
        sqlSession.commit();

        // 6. 关闭sqlsession
        sqlSession.close();
    }
```

#### 3.3删除用户

- UserDao中添加新增方法 

```java
public interface UserDao {

    /*
    * 当只有一个参数时，在映射文件中写sql语句时，可以随便写参数名去获取参数
    * 当有多个参数时，可以使用一个注解给参数取名，则可以在sql语句中使用参数名获取参数了
    * */
    int deleteById(@Param("uid") Integer uid, String aaa);
}
```

- 在 UserDao.xml 文件中加入新增配置 

```xml
<delete id="deleteById">
/*一个参数时，在你的映射文件中可以随意写*/
        delete from t_user where uid = #{id}
    </delete>
```

- 测试

```java
@Test
    public void testDeleteById() throws Exception{

        // 1. 加载核心配置文件
        InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");

        // 2. 得到一个sqlsession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);


        // 3. 获取sqlsession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 4. 获得代理对象
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        // 5. 操作
        userDao.deleteById(8, "bbb");
        // mybatis默认不会自动提交事务，需要手动处理
        sqlSession.commit();

        // 6. 关闭sqlsession
        sqlSession.close();
    }
```

#### 3.4模糊查询

查询名字以王开头的用户。 like '王%'

##### 3.4.1 方式一（只做了解）

- UserDao 中添加新增方法 

```java
public interface UserDao {
    /**
     * 模糊查询
     * @param firstName
     * @return
     */
    List<User> findByFirstName(String firstName);
}
```

- 在 UserDao.xml 文件中加入新增配置 

```xml
<select id="findByFirstName" resultType="User">
        select * from t_user where username like #{firstName}
    </select>
```

- 添加测试类中的测试方法 

```java
@Test
    public void testLikeQuery() throws Exception{

        // 1. 加载核心配置文件
        InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");

        // 2. 得到一个sqlsession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);


        // 3. 获取sqlsession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 4. 获得代理对象
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        // 5. 操作
        System.out.println(userDao.findByFirstName("王%"));


        // mybatis默认不会自动提交事务，需要手动处理
        sqlSession.commit();

        // 6. 关闭sqlsession
        sqlSession.close();
    }
```

##### 3.4.2 方式二

映射文件中固定写法：==LIKE '${value}%'==

- 在 UserMapper.xml 文件中加入新增配置 

```xml
<select id="findByFirstName" resultType="User">
        select * from t_user where username like '${value}%'
    </select>
```

​	` 我们在上面将原来的#{}占位符，改成了${value}。`

注意==如果用模糊查询的这种写法，那么${value}的写法就是固定的，不能写成其它名字。==

- 添加测试类中的测试方法 

```java
// 测试传参
System.out.println(userDao.findByFirstName("王"));
```

##### 23.4.3 方式三（推荐使用）

直接拼接参数和模式匹配符：==LIKE #{firstName}"%"==

```xml
<select id="findByFirstName" resultType="User">
        select * from t_user where username like #{firstName}"%"
    </select>
```

- 测试传参

```java
System.out.println(userDao.findByFirstName("王"));
```



##### 3.4.4 #{}与${}的区别【面试】

1. `#{}`表示一个占位符号
   + 通过`#{}`可以实现 preparedStatement 向占位符中设置值,自动进行 java 类型和 数据库 类型转换 
   +  ==`#{}`可以有效防止 sql 注入==
   +  `#{}`可以接收简单类型值或 pojo 属性值
   +   如果 parameterType 传输单个简单类型值(String,基本类型)， `#{}` 括号中可以是 value 或其它名称。
2. `${}`表示拼接 sql 串
   + 通过`${}`可以将 parameterType 传入的内容拼接在 sql 中且不进行 jdbc 类型转换. 
   + ==`${}`不能防止 sql 注入==
   + `${}`可以接收简单类型值或 pojo 属性值
   + 如果 parameterType 传输单个简单类型值.`${}`括号中只能是 value 
3. ==**选择使用#{}**==

#### 3.5.SqlSessionFactory工具类的抽取

工具类提供获取sqlsession的方法。

**步骤:**

1. SqlSessionFactoryUtils类
   1. 把读取文件、获得sqlsessionFactory的代码封装起来
   2. 提供一个获取sqlSession的方法
   3. 提供一个提交并且关闭资源的方法

**实现**

```java
package com.itheima.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class SqlSessionFactoryUtil {
    static SqlSessionFactory sqlSessionFactory = null;

    static {
        //1. 读取mybatis核心配置文件
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        } catch (IOException e) {
            e.printStackTrace();
        }  //2. 获取sqlsessionFactory
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

    }


    /**
     * 获取sqlSession
     * @return
     */
    public static SqlSession openSqlSession(){
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }


    /**
     * 提交且关闭
     * @param sqlSession
     */
    public static void commitAndClose(SqlSession sqlSession){
        if(sqlSession != null){
            sqlSession.commit();
            sqlSession.close();
        }
    }
}

```

### 4.小结

- CRUD
  - 新增insert 标签
    - 获取自增主键值： 在inset标签中增加属性  keyProperty="javabean里面的属性名" useGerneratedKeys="true"
  - 修改update标签
  - 删除delete
  - 给参数取名注解： @Param("参数名")
  - 模糊查询： like  "%"#{参数}"%"
- 工具类： 就是把一些冗余的代码封装起来

## 知识点-sql文件中理解传参、获取参数

### 1.目标

- [ ] 掌握Mybatis的参数深入

### 2.路径

1. 传递简单类型
2. 传递 pojo 对象（即javabean对象，不同的叫法）
3. 传递 pojo 包装对象类型

### 3.讲解

#### 3.1传递简单类型

​	基本的类型,字符串

​	直接写==#{任意字段}==

```xml
/*给参数取一个名字，可以在映射文件里面通过该注解的值获取*/
int deleteById(@Param("uid") Integer uid, String a);




<!--一个参数，则取值时可以随便写-->
    <delete id="deleteById">

        delete from t_user where uid = #{uid}
    </delete>
```



#### 3.2传递 pojo 对象

==如果参数是javabean（pojo），name可以在映射文件里面直接写类的属性名获取对应的参数值==

```xml
int update(User user);

<update id="update">
        update t_user set username = #{username}, sex=#{sex},
        birthday = #{birthday}, address=#{address} where uid = #{uid}
    </update>
```



#### 3.3传递 pojo 包装对象类型（理解即可）

```java
class A{
    private B b;
    private Map map;
}

class B{
    private User user;
}

class User{
    private String username;
    private Integer age;
}


 // 参数为javabean包装类型： javabean里面还包含其他复杂类型
    List<User> findByA(A a);


 <select id="findByA" resultType="User">
        select * from t_user where username = #{b.user.username}
</select>
```

- 测试

```java
@Test
public void testA(){
    SqlSession sqlSession = SqlSessionFactoryUtil.openSqlSession();

    UserDao mapper = sqlSession.getMapper(UserDao.class);

    A a = new A();
    B b = new B();
    User user = new User();
    user.setUsername("zs");
    b.setUser(user);
    a.setB(b);

    System.out.println(mapper.findByA(a));

    SqlSessionFactoryUtil.commitAndClose(sqlSession);
}

```



#### 3.4 参数是map

```java
List<User> findByMap(Map<String, String> map);
```

```xml
<select id="findByMap" resultType="User">
        select * from t_user where username = #{user_name}
    </select>
```

```java
/*map作为参数时，sql语句获取参数名就是map的key名*/
map.put("user_name", "zs");
System.out.println(mapper.findByMap(map));
```



### 4.小结

1. 传递简单类型、string：  #{}
2. 传递javabean：   #{javabean属性名}
3. 传递复杂的类型里面包含复杂的javabean类型： #{一级属性.二级属性}
4. 传递map： #{map的key名}



## 知识点-resultType深入

### 1.目标

- [ ] 掌握Mybatis的参数深入(resultType)

### 2.路径

1. 输出简单类型
2. 输出pojo对象
3. 输出pojo集合列表List
4. resultMap结果类型 

### 3.讲解

#### 3.1输出简单类型

​	直接写对应的Java类型. eg: 返回int

```xml
<select id="findCount" resultType="int">
     SELECT  COUNT(*) FROM t_user
</select>
```

#### 3.2输出pojo对象

​	直接写当前pojo类的全限定名 eg: 返回User

```xml
<select id="findByUid"  resultType="com.itheima.pojo.User">
        select * from t_user where uid = #{uid}
</select>
```

#### 3.3输出pojo列表

​	直接写当前泛型pojo类的全限定名 eg: 返回 `List<User> list`;

```xml
<select id="findAll" resultType="com.itheima.bean.User">
        select * from t_user
</select>
```

#### 3.4 ==resultMap结果类型==

      ==当sql语句的结果集的字段和实体类的属性不一致时，可以使用resultMap标签来做结果集的映射==。还可以使用resultMap完成一对多、多对多等复杂查询（后面讲到）。

环境准备：

- Product.java

```java
@Data
public class Product {
    private Integer fId;
    private String fName;
    private Double fPrice;
}
```

- ProductDao.java

```java
public interface ProductDao {

    List<Product> findAll();
    List<Product> findAll2();
    List<Product> findAll3();
}
```

- ==ProductDao.xml==

```xml
 <select id="findAll" resultMap="findAllMap">
        select * from t_product
    </select>


<!--resultMap将sql结果集字段和javabean的属性名做一个映射-->
    <resultMap id="findAllMap" type="Product">
        <!--进行查询结果集字段、javabean属性名的一个映射关系配置-->

        <!--id是主键，唯一的字段-->
        <!--result就是普通字段-->
        <!--property属性就是代表javabean的属性名，
            column代表查询结果集中的列名
        -->
        <id property="fId" column="f_id"/>
        <result property="fName" column="f_name"/>
        <result property="fPrice" column="f_price"/>
    </resultMap>
```



**结果集映射：取别名**

```xml
<select id="findAll2" resultType="Product">
        select 
            f_id AS fId, 
            f_name AS fName, 
            f_price AS fPrice 
    	from t_product
    </select>

    
```



**结果集映射：驼峰命名**

核心配置文件中mybatis-config.xml设置：

```xml
<!--下划线自动转驼峰命名法配置-->
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```

sql映射文件：

```xml
<select id="findAll3" resultType="Product">
    select * from t_product
</select>
```



### 4.小结

1. 输出简单类型  直接写 `java类型名`  eg: int 
2. 输出pojo对象  直接写 `pojo类型名`  eg: User
3. 输出pojo列表类型  写 `列表里面的泛型的类型` eg: List<User>    写User
4. ResultMap
   + 解决查询出来的结果的列名和javaBean属性不一致的请求
   + 复杂的pojo复杂(明天讲)





# 第四章-日志【会用就可以】

## 知识点-日志的使用

### 1.目标

​	我们在使用MyBatis的时候, 其实MyBatis框架会打印一些必要的日志信息, 在开发阶段这些日志信息对我们分析问题,理解代码的执行是特别有帮助的; 

​	包括项目上线之后,我们也可以收集项目的错误日志到文件里面去;  所有我们采用专门的日志系统来处理.

### 2.步骤

1. 导入坐标
2. 拷贝日志配置文件到项目
3. 运行代码看到效果

### 3.讲解

- 导入坐标

```xml
<!-- log start -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.12</version>
</dependency>

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.6.6</version>
</dependency>

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.6.6</version>
</dependency>
```

- 拷贝 ==log4j.properties== 到 resources 目录

```properties
##设置日志记录到控制台的方式
log4j.appender.std=org.apache.log4j.ConsoleAppender
log4j.appender.std.Target=System.err
log4j.appender.std.layout=org.apache.log4j.PatternLayout
log4j.appender.std.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %5p %c{1}:%L - %m%n

##设置日志记录到文件的方式
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=mylog.txt
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

##日志输出的级别，以及配置记录方案
log4j.rootLogger= debug,info,std,file
```

  级别:error > warn > info>debug>trace  



- 扩展（了解）

  - 你也可以将日志的记录方式从接口级别切换到语句级别，从而实现更细粒度的控制。如这样配置只对 `findAll` 语句记录日志：`log4j.logger.com.itheima.dao.UserDao.findAll=TRACE`

  - 输出日志时，可能看到DefaultVFS输出内容显示乱码，可以不打印该类的日志，可以设置一下：

    - > log4j.logger.org.apache.ibatis.io.DefaultVFS=error

> 关于log4j日志配置有兴趣可以查看：https://logging.apache.org/log4j/2.x/manual/configuration.html



- IDEA设置编码为UTF-8

![1600930026113](img/1600930026113.png)

### 4.小结

1. 引入依赖jar
2. 在resources目录写一个log4j.properties文件，拷贝内容进去



# 第五章-Mybatis 连接池与事务【了解】

## 知识点-Mybatis 的连接池技术【了解】

### 1.目标

​	这一章仅作了解，实际生产中会用后面spring来接管连接池、事务。

​	我们在前面的 WEB 课程中也学习过类似的连接池技术，而在 Mybatis 中也有连接池技术，但是它采用的是自己的连接池技术 。

​	在 Mybatis 的 mybatis-config.xml 配置文件中， 通过  `<dataSource type=”pooled”>`来实现 Mybatis 中连接池的配置.

### 2.路径

1. Mybatis 连接池的分类 
2. Mybatis 中数据源的配置
3. Mybatis 中 DataSource 配置分析 

### 3.讲解

#### 3.1Mybatis 连接池的分类 

+ 可以看出 Mybatis 将它自己的数据源分为三类：

  +  UNPOOLED 不使用连接池的数据源
  +  ==POOLED 使用连接池的数据源==
  +  JNDI 使用 JNDI 实现的数据源,不要的服务器获得的DataSource是不一样的. 注意: 只有是web项目或者Maven的war工程, 才能使用. 我们用的是tomcat, 用的连接池是dbcp.

  ![img](img/tu_2-1572948297061.png)

+ **在这三种数据源中，我们目前阶段一般采用的是 POOLED 数据源（很多时候我们所说的数据源就是为了更好的管理数据库连接，也就是我们所说的连接池技术）,等后续学了Spring之后,会整合一些第三方连接池**。 

#### 3.2Mybatis 中数据源的配置 

+ 我们的数据源配置就是在 mybatis核心配置文件中， 具体配置如下： 

  ![img](img/tu_3-1572948297061.png)

+ MyBatis 在初始化时，解析此文件，根据`<dataSource>`的 type 属性来创建相应类型的的数据源DataSource，即：

  ​	type=”POOLED”： MyBatis 会创建 PooledDataSource 实例, 使用连接池
  ​	type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例, 没有使用的,只有一个连接对象的
  ​	type=”JNDI”： MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用. 只有在web项目里面才有的,用的是服务器里面的.  默认会使用tomcat里面的dbcp

#### 3.3Mybatis 中 DataSource 配置分析

+ 代码,在21行加一个断点, 当代码执行到21行时候,我们根据不同的配置(POOLED和UNPOOLED)来分析DataSource

![1533783469075](img/1533783469075.png)

+ 当配置文件配置的是type=”POOLED”, 可以看到数据源连接信息

  ![1533783591235](img/1533783591235.png)



+ 当配置文件配置的是type=”UNPOOLED”, 则可以看到没有使用连接池

  ![1533783672422](img/1533783672422.png)



#### 3.4 【扩展-了解】自己添加第三方数据源（C3p0）

​		dataSource 的type属性除了可以使用 **POOLED | UNPOOLED | JNDI** 3个值之外，还可以直接指定类的全限定名，所以也可以这么写;

```xml
<!-- 等效于type="POOLED" -->
<dataSource type="org.apache.ibatis.datasource.pooled.PooledDataSourceFactory">
  ...
</dataSource>
```

​	所以我们可以自己参考`PooledDataSourceFactory` 编写一个支持其他数据源的类。

我们想使用C3p0数据源，怎么做呢？

- 第一步： 可以继承 UnpooledDataSourceFactory

  ```java
  public class C3P0DataSourceFactory extends UnpooledDataSourceFactory {
  
    public C3P0DataSourceFactory() {
      // 数据源使用C3p0数据源，注意引入依赖jar包
      this.dataSource = new ComboPooledDataSource();
    }
  }
  ```

- 第二步： 配置

  ```xml
  <dataSource type="com.itheima.datasource.C3P0DataSourceFactory">
    ...
  </dataSource>
  ```

  

### 4.小结

mybatis的连接池技术仅作了解




## 知识点-Mybatis 的事务控制 【了解】

### 1.目标

- [ ] 了解MyBatis事务操作

### 2.路径

1. JDBC 中事务的回顾
2. Mybatis 中事务提交方式 
3. Mybatis 自动提交事务的设置

### 3.讲解

#### 3.1JDBC 中事务的回顾 

​	我们在学习JDBC 时，可以通过`setAutoCommit(boolean auto)`方法就可以将事务的提交改为手动方式。

你应该熟悉的代码：

![1578652586251](img/1578652586251.png)

​	Mybatis 框架因为是对 JDBC 的封装，所以 Mybatis 框架的事务控制方式，本身也是用 JDBC的 setAutoCommit()方法来设置事务提交方式的。 

#### 3.2Mybatis 中事务提交方式

==DML操作就要使用sqlsession.commit()才会生效==

+ Mybatis 中事务的提交方式，本质上就是调用 JDBC 的 setAutoCommit()来实现事务控制。我们运行之前所写的代码： 

![img](img/tu_9-1572948297061.png)

+ userDao 所调用的 saveUser()方法如下： 

  ![img](img/tu_10-1572948297061.png)

+ 观察在它在控制台输出的结果： 

  ![img](img/tu_11-1572948297061.png)

  ​	这是我们的 Connection 的整个变化过程， 通过分析我们能够发现之前的 CUD操作过程中，我们都要手动进行事务的提交，原因是 setAutoCommit()方法，在执行时它的值被设置为 false 了，所以我们在CUD 操作中，必须通过 sqlSession.commit()方法来执行提交操作。 

#### 3.3 Mybatis 自动提交事务的设置 

​	通过上面的研究和分析，现在我们一起思考，为什么 CUD 过程中必须使用 sqlSession.commit()提交事务？主要原因就是在连接池中取出的连接，都会将调用 connection.setAutoCommit(false)方法，这样我们就必须使用 sqlSession.commit()方法，相当于使用了 JDBC 中的 connection.commit()方法实现事务提交。明白这一点后，我们现在一起尝试不进行手动提交，一样实现 CUD 操作。 

![img](img/tu_12-1572948297062.png)

​	我们发现，此时事务就设置为自动提交了，同样可以实现 CUD 操作时记录的保存。虽然这也是一种方式，但就编程而言，设置为自动提交方式为 false 再根据情况决定是否进行提交，这种方式更常用。因为我们可以根据业务情况来决定提交是否进行提交。 

### 4.小结

1. MyBatis的事务使用的是JDBC事务策略. 
   + 通过设置autoCommit()去控制的
   + 默认情况下, MyBatis使用的时候 就把autoCommit(false)
     + 也就是意味着, 我们要进行增删改的时候, 需要手动的commit
2. 后面做项目, 工作里面的事务管理, 基本上都是交给Spring管理. 所以此章节只做了解

# 第六章-Mybatis 的动态 SQL【掌握】

​	Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是动态变化的，此时在前面的学习中我们的 SQL 就不能满足要求了。

## 知识点-动态 SQL 之if标签

### 1.目标

​	我们根据实体类的不同取值，使用不同的 SQL 语句来进行查询。

​	比如在 uid 如果不为空时可以根据 uid查询，如果 username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到

### 2.讲解

比如在 uid 如果不为空时可以根据 uid查询，如果 username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询。

+ UserDao.java

```java
public interface UserDao {
    // if标签的使用 根据uid、用户名查询用户信息
    User findByUidAndUsername(User user);
}
```

+ UserDao.xml

```xml
<!--动态SQL之if
        test属性： 布尔值表达式，写参数的值不需要写#{}
        如果test属性值为true，则会包含标签体的内容；否则不包含
    -->
    <select id="findByUidAndUsername" resultType="User">
        select * from t_user where 1=1

        <if test="uid != null">
            AND uid = #{uid}
        </if>
        <if test="username != null">
            AND username = #{username}
        </if>
        
    </select>
```



+ 测试

```java
@Test
    public void testIf(){
        SqlSession sqlSession = SqlSessionFactoryUtil.openSqlSession();
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        User user = new User();
        //user.setUid(5);
        //user.setUsername("zsf");

        User resultUser = userDao.findByUidAndUsername(user);

        System.out.println(resultUser);

        SqlSessionFactoryUtil.commitAndClose(sqlSession);

    }
```

### 3.小结

1. if适合动态多条件查询

## 知识点-动态 SQL 之where标签

### 1.目标

​	为了简化上面 where 1=1 的条件拼装，我们可以采用`<where>`标签来简化开发。

### 2.讲解

修改 UserDao.xml 映射文件如下： 

```xml
<!--where 标签
        只要有一个条件就会带上where；
        会自动去除第一个条件前面的and关键字。
    -->
    <select id="findByUidAndUsername" resultType="User">
        select * from t_user
        <where>
            <if test="uid != null">
                AND uid = #{uid}
            </if>
            <if test="username != null">
                AND username = #{username}
            </if>
        </where>
    </select>
```



> 注意: `<where />可以自动处理第一个 and `

### 3.小结

1. where标签用在自己写sql语句的时候 where关键字不好处理的情况,代替where '1' = '1'
2. `<where />`可以==自动处理第一个 and , 建议全部加上and==



## 知识点-动态 SQL 之set标签

### 1.目标

​	学会在update标签中使用set标签

### 2.讲解

修改数据时，如果某些字段用户没有填，没有改----应该不动（而不是改为null）



需求： 根据id修改用户信息；

- UserDao.java

```java
// set标签的使用
    int updateByCondition(User user);
```

- UserDao.xml

```xml
<!--set标签
        一般结合if一块使用，用于动态修改字段；
        自动去除最后一个条件的,符号
    -->
    <update id="updateByCondition">
        update t_user
        <set>
            <if test="username != null">
                username = #{username},
            </if>
            <if test="sex != null">
                sex = #{sex},
            </if>
            <if test="birthday != null">
                birthday = #{birthday},
            </if>
            <if test="address != null">
                address = #{address},
            </if>
        </set>

        <where>
            <if test="uid != null">
                uid = #{uid}
            </if>
        </where>
    </update>
```



### 3. 小结

set会自动去除最后一个条件的,号，所以在每一个条件中都添加,号

## 知识点-动态标签之foreach标签

### 1.目标

- [ ] 掌握foreach标签的使用

### 2.讲解

foreach标签用于遍历集合，它的属性：

- collection:代表要遍历的集合元素，注意编写时不要写#{}
- open:代表语句的开始部分(一直到动态的值之前)
- close:代表语句结束部分
- item:代表遍历集合的每个元素，生成的变量名(随便取)
- sperator:代表分隔符 (动态值之间的分割)

#### 2.1需求

+ 传入多个 id 查询用户信息，用下边sql 实现：

```
SELECT uid ,username ,birthday ,sex, address FROM t_user 
WHERE  uid IN (1,3,9,11)
```

+ UserDao.java

```java
// foreach标签的使用--根据多个id查询用户
    List<User> findByIds(@Param("ids") List<Integer> ids);
```

+ UserDao.xml

```xml
<!--
        foreach标签的属性：
            collection： 可以遍历的参数；
            open： 开始部分；
            item： 每次遍历的值的变量名，名字随便写；可以在foreach标签使用#{}获取每次遍历的值
            close： 结束部分；
            separator： 分隔符
    -->
    <select id="findByIds" resultType="User">
        SELECT uid ,username ,birthday ,sex, address FROM t_user
        WHERE  uid IN
        <foreach collection="ids" open="(" item="id" close=")" separator=",">
            #{id}
        </foreach>
    </select>
```



+ 测试

```java
@Test
    public void testForeachTag(){

        // 1. 获取sqlsession
        SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSession();
        // 2. 获得代理对象
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        // 3. 操作
        Integer[] arr = {1, 3, 9, 11};
        List<Integer> integers = Arrays.asList(arr);
        List<User> userList = userDao.findByIds(integers);
        System.out.println(userList);


        // 4. 关闭sqlsession
        SqlSessionFactoryUtils.close(sqlSession);


    }
```



### 3.小结

- forEach主要用于多条件查询，in关键字。



## 知识点-SQL 片段 

### 1.目标

​	Sql 中可将重复的 sql 提取出来，使用时用 `<include>` 引用即可，最终达到 sql 重用的目的。我们先到 UserDao.xml 文件中使用`<sql>`标签，定义出公共部分.

### 2.讲解

`<sql>`标签可以定义在该映射文件中可能多次用到的sql语句，在其他的标签（select、update...）中可以通过`<include>`标签来引用sql标签的id。

如果，查询结果集包含很多个字段，那么可以用一个sql标签包裹，这样哪里用到这些字段就在哪里引用。

用法：

```xml
<sql id="sqlId">
	XXXXX
</sql>
<select id="selectId">
    <!-- id为sqlId的sql标签定义的内容会在这里显示  -->
    <include refid = "sqlId"></include>
</select>
```



+ 使用sql标签抽取

```xml
// 查询用户信息
List<User> findUserInfo();
        
```

+ 使用include标签引入使用

```xml
<select id="findUserInfo" resultType="User">
        SELECT
         <include refid="sqlId"/>
          FROM t_user
    </select>
    <!--sql标签： 可以抽取公共的sql语句部分；
        你只需要在你的标签中使用include标签来引用这个sql标签的id值即可
    -->
    <sql id="sqlId">
        uid ,username ,birthday ,sex, address
    </sql>
```



### 3.小结

1. sql标签可以把公共的sql语句进行抽取, 再使用include标签引入. 好处:好维护, 提示效率







# 总结

- 概念
  - 框架： 就是一个软件的半成品，封装了一些基础技术
  - mybatis框架：持久层DAO层的框架，封装了jdbc
- mybatis入门：
  - 引入依赖坐标（mybatis、jdbc驱动包）
  - 写一个javabean（pojo类）
  - 写一个接口UserDao，com.itheima.dao包里面
  - 在resources目录创建com/itheima/dao目录，写一个UserDao.xml（映射文件）
  - 在resources目录写mybatis核心配置文件
  - 写测试代码（步骤不要死记）
- 开发规范：
  - 映射文件中namespace（名称空间），写接口的全限定名
  - 映射文件的标签的id属性值和接口的方法名一致
  - 写接口方法时，传了参数； 在你的映射文件中的标签中建议不要写paramaterType属性-----mybatis会自动解析参数类型
- mybatis进阶（CRUD）
  - 插入 insert标签
    - 获得了自增主键： 在标签中增加属性  keyProperty="属性名" userGerneratedKeys="true"
  - 修改update标签
  - 删除delete标签
  - 查询select标签
  - 在sql中获取参数使用 #{}
  - 可以给参数取名： @Param("参数名")
  - 结果集映射：
    - resultMap： 当你的查询结果集的字段名和你的javabean的属性名不一致时，可以做映射
      - id 标签
      - result 标签
      - 都有属性： column="查询结果集的字段名"、property="javabean的属性名"
- 动态SQL
  - if： 条件判断，适合动态条件
  - where： 去除第一个查询条件的AND关键字
  - set：主要用于修改操作，结合if；去除最后一个条件的,号
  - forEach： 用于遍历
  - sql、include： 抽取公共的sql片段

# 附录

Mybatis内部已经存在的别名配置，可直接使用。

| 别名       | 映射的类型 |
| :--------- | :--------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |





# 回顾要点

时间： 入门案例

理解编程步骤、配置项、规范







练习CRUD





练习动态sql



