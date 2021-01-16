---
	typora-copy-images-to: img
---

# day21-连接池和DBUtils

# 今日内容

- 连接池
  - 自定义连接池
  - **使用第三方连接池**
    - C3P0
    - DRUID
- **DBUtils**

# 学习目标

- [ ] 能够理解连接池解决现状问题的原理
- [ ] 能够使用C3P0连接池
- [ ] 能够使用DRUID连接池
- [ ] 能够编写C3P0连接池工具类 
- [ ] 能够使用DBUtils完成CRUD
- [ ] 能够理解元数据
- [ ] 能够自定义DBUtils

# 第一章-自定义连接池 #

## 知识点-连接池概念

### 1.目标

+ 能够理解连接池解决现状问题的原理

### 2.讲解

#### 2.1为什么要使用连接池 ####

​	Connection对象在JDBC使用的时候就会去创建一个对象,使用结束以后就会将这个对象给销毁了(close).每次创建和销毁对象都是耗时操作.需要使用连接池对其进行优化.

​	程序初始化的时候，初始化多个连接,将多个连接放入到池(集合)中.每次获取的时候,都可以直接从连接池中进行获取.使用结束以后,将连接归还到池中.

#### 2.2.生活里面的连接池例子  

- 老方式:

  ​	下了地铁需要骑车, 跑去生产一个, 然后骑完之后,直接把车销毁了.

- 连接池方式 摩拜单车:

  ​	骑之前, 有一个公司生产了很多的自行车, 下了地铁需要骑车, 直接扫码使用就好了, 然后骑完之后, 还回去


![img](img/tu_9.png)

#### 2.3连接池原理【重点】 ####

![img](img/tu_1.png)

1. 程序一开始就创建一定数量的连接，放在一个容器(集合)中，这个容器称为连接池。
2. 使用的时候直接从连接池中取一个已经创建好的连接对象, 使用完成之后 归还到池子
3. 如果池子里面的连接使用完了, 还有程序需要使用连接, 先等待一段时间(eg: 3s), 如果在这段时间之内有连接归还, 就拿去使用; 如果还没有连接归还, 新创建一个, 但是新创建的这一个不会归还了(销毁)
4. 集合选择LinkedList
   + 增删比较快
   + LinkedList里面的removeFirst()和addLast()方法和连接池的原理吻合

### 3.小结

1. 使用连接池的目的: 可以让连接得到复用, 避免浪费



## 自定义连接池-初级版本

### 1.目标

​	根据连接池的原理, 使用LinkedList自定义连接池

### 2.分析

1. 创建一个类MyDataSource, 定义一个集合LinkedList
2. 程序初始化的时候, 创建5个连接 存到LinkedList
3. 定义getConnection() 从LinkedList取出Connection返回
4. 定义addBack()方法归还Connection到LinkedList



### 3.实现

```java
public class MyDataSource1 {
    // 1.定义一个LinkedList集合,用来存储初始化的连接
    private static LinkedList<Connection> list = new LinkedList<>();

    // 2.初始化5个连接,并存储到集合中
    static {
        for (int i = 0; i < 5; i++) {
            try {
                // 获得连接
                Connection connection = JDBCUtils.getConnection();
                // 添加到集合中
                list.add(connection);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    // 3.定义getAbc方法,用来获得连接
    public Connection getAbc(){
        Connection connection = list.removeFirst();
        return connection;
    }

    // 4.定义addBack方法,用来归还连接
    public void addBack(Connection connection){
        list.addLast(connection);
    }

    // 5.定义getCount方法,返回连接池中连接数量
    public static int getCount(){
        return list.size();
    }
}

// 测试
public class CRUDTest1 {
    // 查询记录
    @Test
    public void select() throws Exception {
        // 1.创建连接池对象
        MyDataSource1 dataSource = new MyDataSource1();
        System.out.println("获得连接之前,连接池中连接的数量:"+MyDataSource1.getCount());// 5

        // 2.获得连接
        Connection connection = dataSource.getAbc();

        // 2.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);
        // 3.设置参数
        ps.setInt(1,3);
        // 4.执行sql语句
        ResultSet resultSet = ps.executeQuery();
        // 封装,处理数据
        User user = null;
        while (resultSet.next()){
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println("获得连接之后,连接池中连接的数量:"+MyDataSource1.getCount());// 4
        System.out.println(user);

        // 5.释放资源
        // 归还连接
        dataSource.addBack(connection);
        JDBCUtils.release(resultSet,ps,null);

        System.out.println("归还连接之后,连接池中连接的数量:"+MyDataSource1.getCount());// 5

    }
}

```

### 4.小结

1. 创建一个类MyDataSource, 定义一个集合LinkedList
2. 程序初始化(静态代码块)里面  创建5个连接存到LinkedList
3. 定义提供Connection的方法
4. 定义归还Connection的方法



## 自定义连接池-进阶版本

### 1.目标

​	实现datasource完成自定义连接池

### 2.分析

​	在初级版本版本中, 我们定义的方法是getAbc(). 因为是自定义的.如果改用李四的自定义的连接池,李四定义的方法是getCon(), 那么我们的源码就需要修改, 这样不方便维护. 所以sun公司定义了一个接口Datasource,让自定义连接池有了规范

### 3.讲解

#### 3.1datasource接口概述

​	Java为数据库连接池提供了公共的接口：javax.sql.DataSource，各个厂商(用户)需要让自己的连接池实现这个接口。这样应用程序可以方便的切换不同厂商的连接池！ 

![img](img/tu_4.png)

#### 3.2代码实现

```java
public class MyDataSource2 implements DataSource {
    // 1.定义一个LinkedList集合,用来存储初始化的连接
    private static LinkedList<Connection> list = new LinkedList<>();

    // 2.初始化5个连接,并存储到集合中
    static {
        for (int i = 0; i < 5; i++) {
            try {
                // 获得连接
                Connection connection = JDBCUtils.getConnection();
                // 添加到集合中
                list.add(connection);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    /*// 3.定义getAbc方法,用来获得连接
    public Connection getAbc(){
        Connection connection = list.removeFirst();
        return connection;
    }*/

    // 4.定义addBack方法,用来归还连接
    public void addBack(Connection connection){
        list.addLast(connection);
    }

    // 5.定义getCount方法,返回连接池中连接数量
    public static int getCount(){
        return list.size();
    }

    @Override
    public Connection getConnection() throws SQLException {
        Connection connection = list.removeFirst();
        return connection;
    }

    @Override
    public Connection getConnection(String username, String password) throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        return null;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {

    }

    @Override
    public int getLoginTimeout() throws SQLException {
        return 0;
    }

    @Override
    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
        return null;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return null;
    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return false;
    }
}


// 测试
public class CRUDTest2 {

    // 查询记录
    @Test
    public void select() throws Exception {
        // 1.创建连接池对象
        DataSource dataSource = new MyDataSource2();
        System.out.println("获得连接之前,连接池中连接的数量:"+MyDataSource2.getCount());// 5

        // 2.获得连接
        Connection connection = dataSource.getConnection();

        // 2.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);
        // 3.设置参数
        ps.setInt(1,3);
        // 4.执行sql语句
        ResultSet resultSet = ps.executeQuery();
        // 封装,处理数据
        User user = null;
        while (resultSet.next()){
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println("获得连接之后,连接池中连接的数量:"+MyDataSource2.getCount());// 4
        System.out.println(user);

        // 5.释放资源
        // 归还连接
        // 解决办法: 1.向下转型  2.增强Connection的close方法(默认是销毁连接,增强后变成归还连接)
        ((MyDataSource2)dataSource).addBack(connection);
        JDBCUtils.release(resultSet,ps,null);

        System.out.println("归还连接之后,连接池中连接的数量:"+MyDataSource2.getCount());// 5

    }
}


```

### 4.小结

#### 4.1编写连接池遇到的问题

- 实现DataSource接口后,addBack()不能调用了.
- 能不能不引入新的api,直接调用之前的connection.close(),但是这个close不是关闭,是归还

#### 4.2解决办法

- 继承

  + 条件:可以控制父类, 最起码知道父类的名字


- 装饰者模式

  + 作用：改写已存在的类的某个方法或某些方法
  + 条件:
    + 增强类和被增强类实现的是同一个接口
    + 增强类里面要拿到被增强类的引用


- 动态代理

## 自定义连接池-终极版本

### 1.目标

使用装饰者模式改写connection的close()方法, 让connection归还

### 2.讲解

#### 2.1装饰者模式

##### 2.1.1概述

- 什么是装饰者模式

  ​	装饰者模式，是 23种常用的面向对象软件的设计模式之一.  动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更加有弹性的替代方案。

  ​	装饰者的作用：改写已存在的类的某个方法或某些方法, 增强方法的逻辑

- 使用装饰者模式需要满足的条件

  - 增强类和被增强类实现的是同一个接口
  - 增强类里面要拿到被增强类的引用

##### 2.2.2装饰者模式的使用【重点】

实现步骤:

1. 增强类和被增强类需要实现同一个接口
2. 增强类里面需要得到被增强类的引用,
3. 对于不需要改写的方法，调用被增强类原有的方法。
4. 对于需要改写的方法写自己的代码

+ 接口: 

```java
public interface Star {
    public void sing();
    public void dance();
}
```

+ 被增强的类: 

```java
public class LiuDeHua implements Star {
    @Override
    public void sing() {
        System.out.println("刘德华唱忘情水...");
    }

    @Override
    public void dance() {
        System.out.println("刘德华跳霹雳舞...");
    }
}

```

+ 增强的类: 

```java
public class LiuDeHuaWrapper implements Star {

    // 增强类中获得被增强类的引用
    LiuDeHua ldh;

    public LiuDeHuaWrapper(LiuDeHua ldh) {
        this.ldh = ldh;
    }

    @Override
    public void sing() {
        System.out.println("刘德华唱冰雨...");
        ldh.sing();
        System.out.println("刘德华唱练习...");
    }

    @Override
    public void dance() {
        // 不增强
        ldh.dance();
    }
}
```

- 测试

  ```java
  public class Test {
      public static void main(String[] args) {
          // 没有使用装饰者
          LiuDeHua ldh = new LiuDeHua();
          //ldh.sing();
          //ldh.dance();
  
          // 使用装饰者
          LiuDeHuaWrapper ldhw = new LiuDeHuaWrapper(ldh);
          ldhw.sing();
          ldhw.dance();
      }
  }
  ```

  

#### 2.2自定义连接池终极版本

##### 2.2.1分析

​	增强connection的close()方法, 其它的方法逻辑不改

![image-20191203101709735](img/image-20191203101709735.png)



1. 创建MyConnection实现Connection
2. 在MyConnection里面需要得到被增强的connection对象(通过构造方法传进去)
3. 改写close()的逻辑, 变成归还
4. 其它方法的逻辑, 还是调用被增强connection对象之前的逻辑



##### 2.2.2实现

+ MyConnection

```java
public class MyConnection implements Connection {

    // 获得被增强的连接对象的引用
    Connection con;

    // 获得连接池
    LinkedList<Connection> list;

    public MyConnection(Connection con, LinkedList<Connection> list) {
        this.con = con;
        this.list = list;
    }

    @Override
    public void close() throws SQLException {
        // 归还连接
        list.addLast(con);
    }

    @Override
    public Statement createStatement() throws SQLException {
        return con.createStatement();
    }
	// 必须写
    @Override
    public PreparedStatement prepareStatement(String sql) throws SQLException {
        return con.prepareStatement(sql);
    }
	// 剩余的重写方法,使用被增强的连接对象调用原有方法...
    // ...
}

```

+ MyDataSource03

```java
public class MyDataSource3 implements DataSource {
    // 1.定义一个LinkedList集合,用来存储初始化的连接
    private static LinkedList<Connection> list = new LinkedList<>();

    // 2.初始化5个连接,并存储到集合中
    static {
        for (int i = 0; i < 5; i++) {
            try {
                // 获得连接
                Connection connection = JDBCUtils.getConnection();
                // 添加到集合中
                list.add(connection);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    /*// 3.定义getAbc方法,用来获得连接
    public Connection getAbc(){
        Connection connection = list.removeFirst();
        return connection;
    }*/

    /*// 4.定义addBack方法,用来归还连接
    public void addBack(Connection connection){
        list.addLast(connection);
    }*/

    // 5.定义getCount方法,返回连接池中连接数量
    public static int getCount(){
        return list.size();
    }

    @Override
    public Connection getConnection() throws SQLException {
        Connection connection = list.removeFirst();// 返回的是被增强的连接对象
        // 增强
        MyConnection myConnection = new MyConnection(connection,list);
        return myConnection;
    }

    @Override
    public Connection getConnection(String username, String password) throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        return null;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {

    }

    @Override
    public int getLoginTimeout() throws SQLException {
        return 0;
    }

    @Override
    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
        return null;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return null;
    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return false;
    }
}

```

- 测试类

  ```java
  public class CRUDTest3 {
  
      // 查询记录
      @Test
      public void select() throws Exception {
          // 1.创建连接池对象
          DataSource dataSource = new MyDataSource3();
          System.out.println("获得连接之前,连接池中连接的数量:"+MyDataSource3.getCount());// 5
  
          // 2.获得连接
          Connection connection = dataSource.getConnection();// 返回的是增强的连接对象 MyConnection
  
          // 2.书写sql语句,预编译sql语句,得到预编译对象
          String sql = "select * from user where id = ?";
          PreparedStatement ps = connection.prepareStatement(sql);
          // 3.设置参数
          ps.setInt(1,3);
          // 4.执行sql语句
          ResultSet resultSet = ps.executeQuery();
          // 封装,处理数据
          User user = null;
          while (resultSet.next()){
              user = new User();
              user.setId(resultSet.getInt("id"));
              user.setUsername(resultSet.getString("username"));
              user.setPassword(resultSet.getString("password"));
              user.setNickname(resultSet.getString("nickname"));
          }
          System.out.println("获得连接之后,连接池中连接的数量:"+MyDataSource3.getCount());// 4
          System.out.println(user);
  
          // 5.释放资源
          // 归还连接
          //connection.close();// 归还连接
          JDBCUtils.release(resultSet,ps,connection);
  
          System.out.println("归还连接之后,连接池中连接的数量:"+MyDataSource3.getCount());// 5
  
      }
  }
  ```

  

### 3.小结

1. 创建一个MyConnection实现Connection
2. 在MyConnection得到被增强的connection对象
3. 改写MyConnection里面的close()方法的逻辑为归还
4. MyConnection里面的其它方法 调用被增强的connection对象之前的逻辑
5. 在MyDataSource03的getConnection()方法里面 返回了myConnection(增强的连接对象)



# 第二章-第三方连接池

## 知识点-常用连接池

### 1.目标

+ 常用连接池

### 2.分析

​	通过前面的学习，我们已经能够使用所学的基础知识构建自定义的连接池了。其目的是锻炼大家的基本功，帮助大家更好的理解连接池的原理, 但现实是残酷的，我们所定义的 连接池 和第三方的连接池相比，还是显得渺小. 工作里面都会用第三方连接池. 

### 3.讲解

 常见的第三方连接池如下: 

+ **C3P0是一个开源的JDBC连接池**，它实现了数据源和JNDI绑定，支持JDBC3规范和JDBC2的标准扩展。C3P0是异步操作的，所以一些操作时间过长的JDBC通过其它的辅助线程完成。目前使用它的开源项目有Hibernate，Spring等。**C3P0有自动回收空闲连接功能**
+ **阿里巴巴-德鲁伊druid连接池**：Druid是阿里巴巴开源平台上的一个项目，整个项目由数据库连接池、插件框架和SQL解析器组成。该项目主要是为了扩展JDBC的一些限制，可以让程序员实现一些特殊的需求。
+ DBCP(DataBase Connection Pool)数据库连接池，是Apache上的一个Java连接池项目，也是Tomcat使用的连接池组件。dbcp没有自动回收空闲连接的功能。

### 4.小结

我们工作里面用的比较多的是:

+ C3P0
+ druid
+ 光连接池

## 知识点-C3P0 

### 1.目标

+ 掌握C3P0的使用

### 2.路径

1. c3p0介绍
2. c3p0的使用(硬编码)
3. c3p0的使用(配置文件)---工作中使用
4. 使用c3p0改写工具类

### 3.讲解

#### 3.1 c3p0介绍

 ![img](img/tu_3.jpg)

+ C3P0开源免费的连接池！目前使用它的开源项目有：Spring、Hibernate等。使用第三方工具需要导入jar包，c3p0使用时还需要添加配置文件c3p0-config.xml.
+ 使用C3P0需要添加c3p0-0.9.1.2.jar

#### 3.2c3p0的使用

##### 3.2.1通过硬编码来编写【了解】

步骤

1. 拷贝jar
2. 创建C3P0连接池对象
3. 从C3P0连接池对象里面获得connection
4. ....


实现:

```java
public class Test1_通过硬编码来编写 {
    public static void main(String[] args) throws Exception {
        // 1.拷贝c3p0jar包到模块下,并添加到classpath路径中
        // 2.创建连接池对象
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        // 连接池设置参数
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/day21_1");
        dataSource.setUser("root");
        dataSource.setPassword("root");
        // 设置初始化连接数量
        dataSource.setInitialPoolSize(5);

        // 3.获得连接
        Connection connection = dataSource.getConnection();

        // 4.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 5.设置参数
        ps.setInt(1, 3);

        // 6.执行sql语句
        ResultSet resultSet = ps.executeQuery();

        // 封装,处理数据
        User user = null;
        while (resultSet.next()) {
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println(user);

        // 7.释放资源---连接归还
        JDBCUtils.release(resultSet,ps,connection);
    }
}

```

##### 3.2.2 通过配置文件来编写【重点】

步骤:

1. 拷贝jar
2. 拷贝配置文件(c3p0-config.xml)到src目录【**名字不要改**】
3. 创建C3P0连接池对象【自动的读取】
4. 从池子里面获得连接

实现:

+ 编写配置文件c3p0-config.xml，放在src目录下（注：文件名一定不要改)

```xml
<c3p0-config>
	<default-config>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/day21_1</property>
		<property name="user">root</property>
		<property name="password">root</property>
		<property name="initialPoolSize">5</property>
	</default-config>
</c3p0-config>
```

+ 编写Java代码 (会自动读取src目录下的c3p0-config.xml,所以不需要我们解析配置文件)

```java
public class Test2_通过配置文件来编写 {
    public static void main(String[] args) throws Exception{
        // 1.拷贝c3p0jar包到模块下,并添加到classpath路径中
        // 2.创建连接池对象
        ComboPooledDataSource dataSource = new ComboPooledDataSource();

        // 3.获得连接
        Connection connection = dataSource.getConnection();

        // 4.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 5.设置参数
        ps.setInt(1, 3);

        // 6.执行sql语句
        ResultSet resultSet = ps.executeQuery();

        // 封装,处理数据
        User user = null;
        while (resultSet.next()) {
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println(user);

        // 7.释放资源---连接归还
        JDBCUtils.release(resultSet,ps,connection);
    }
}

```

#### 3.3使用c3p0改写工具类【重点】

​	我们之前写的工具类(JdbcUtils)每次都会创建一个新的连接, 使用完成之后, 都给销毁了; 所以现在我们要使用c3p0来改写工具类. 也就意味着,我们从此告别了JdbcUtils. 后面会使用c3p0写的工具类

思路:

1. 创建C3P0Utils这个类
2. 定义DataSource, 保证DataSource全局只有一个
3. 定义getConnection()方法从DataSource获得连接
4. 定义closeAll()方法 释放资源



```java
package com.itheima.utils;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * @Author：pengzhilin
 * @Date: 2020/9/3 11:29
 */
public class C3P0Utils {
    // 创建连接池
    private static final ComboPooledDataSource dataSource = new ComboPooledDataSource();

    // 提供一个获取连接池的方法
    public static DataSource getDataSource(){
        return dataSource;
    }

    // 提供一个获取连接的方法
    public static Connection getConnection() throws Exception{
        return dataSource.getConnection();
    }

    // 释放资源
    /**
     * 释放资源
     *
     * @param resultSet  结果集
     * @param statement  执行sql语句对象
     * @param connection 连接对象
     */
    public static void release(ResultSet resultSet, Statement statement, Connection connection) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (statement != null) {
            try {
                statement.close();
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

### 4.小结

1. C3P0 配置文件方式使用

   + 拷贝jar
   + 拷贝配置文件到src【配置文件的名字不要改】
   + 创建C3P0连接池对象
   + ...

2. C3P0工具类

   + 保证DataSource连接池只有一个【static final】
   + 提供connection
   + 提供DataSource
+ 释放资源
   
   > 代替昨天写的JdbcUtils



## 知识点-DRUID

### 1.目标

+ 掌握DRUID连接池的使用

### 2.路径

1. DRUID的介绍
2. DRUID的使用(硬编码方式)
3. DRUID的使用(配置文件方式)
4. DRUID抽取成工具类(作业)

### 3.讲解

#### 3.1DRUID介绍

​	Druid是阿里巴巴开发的号称为监控而生的数据库连接池，Druid是国内目前最好的数据库连接池。在功能、性能、扩展性方面，都超过其他数据库连接池。Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。如：一年一度的双十一活动，每年春运的抢火车票。

Druid的下载地址：[https://github.com/alibaba/druid](https://github.com/alibaba/druid)  

DRUID连接池使用的jar包：druid-1.0.9.jar

![img](img/tu_7.png)



#### 3.2DRUID的使用

##### 3.2.1通过硬编码方式【了解】

步骤:

1. 导入DRUID jar 包
2. 创建Druid连接池对象, 配置4个基本参数
3. 从Druid连接池对象获得Connection

实现:

```java
public class Test1_硬编码方式使用 {
    @Test
    public void select() throws Exception{
        // 1.拷贝jar包,添加classpath路径
        // 2.创建Druid连接池对象
        DruidDataSource dataSource = new DruidDataSource();
        // 设置参数
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day21_1");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        dataSource.setInitialSize(5);

        // 3.获得连接
        DruidPooledConnection connection = dataSource.getConnection();
78s
        // 4.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 5.设置参数
        ps.setInt(1, 3);

        // 6.执行sql语句
        ResultSet resultSet = ps.executeQuery();

        // 封装,处理数据
        User user = null;
        while (resultSet.next()) {
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println(user);

        // 7.释放资源---连接归还
        JDBCUtils.release(resultSet,ps,connection);
    }
}

```

##### 3.2.2 通过配置文件方式【重点】

步骤:

1. 导入DRUID jar 包
2. 拷贝配置文件到src目录
3. 根据配置文件创建Druid连接池对象
4. 从Druid连接池对象获得Connection

实现:

+ 创建druid.properties, 放在src目录下

```properties
url=jdbc:mysql://localhost:3306/day21_1
username=root
password=root
driverClassName=com.mysql.jdbc.Driver
```

+ 编写Java代码

```java
public class Test2_配置文件方式使用 {
    @Test
    public void select() throws Exception{
        // 1.拷贝jar包,添加classpath路径
        // 2.创建Druid连接池对象
        Properties prop = new Properties();
        // 加载配置文件
        InputStream is = Test2_通过配置文件来编写.class.getClassLoader().getResourceAsStream("druid.properties");
        prop.load(is);
        // 创建Druid连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        // 3.获得连接
        Connection connection = dataSource.getConnection();

        // 4.书写sql语句,预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 5.设置参数
        ps.setInt(1, 3);

        // 6.执行sql语句
        ResultSet resultSet = ps.executeQuery();

        // 封装,处理数据
        User user = null;
        while (resultSet.next()) {
            user = new User();
            user.setId(resultSet.getInt("id"));
            user.setUsername(resultSet.getString("username"));
            user.setPassword(resultSet.getString("password"));
            user.setNickname(resultSet.getString("nickname"));
        }
        System.out.println(user);

        // 7.释放资源---连接归还
        JDBCUtils.release(resultSet,ps,connection);
    }
}

```

#### 3.3作业(必做)

+ 把druid连接池也抽取一个工具类(参照C3P0Util)

### 4.小结

1. Druid配置文件使用
   + 拷贝jar
   + 拷贝配置文件到src(文件名可以自己取,文件中的键不能随便取)
   + 读取配置文件成properties对象
   + 使用工厂根据properties创建DataSource
   + 从DataSource获得Connection
   + ...





# 第三章-DBUtils

## 知识点-DBUtils的介绍

### 1.目标

- [ ] 知道什么是DBUtils

### 2.路径

1. DBUtils的概述
2. DBUtils的常用API介绍

### 3.讲解

#### 3.1 DBUtils的概述

​	DbUtils是Apache组织提供的一个对JDBC进行简单封装的开源工具类库，使用它能够简化JDBC应用程序的开发，同时也不会影响程序的性能

#### 3.2 DBUtils的常用API介绍

1. 创建QueryRunner对象的API

   `public QueryRunner(DataSource ds)` ,提供数据源（连接池），DBUtils底层自动维护连接connection

2. QueryRunner执行增删改的SQL语句的API

   `int update(String sql, Object... params)`,执行增删改的SQL语句,  params参数就是可变参数,参数个数取决于语句中问号的个数

3. 执行查询的SQL语句的API

   `query(String sql, ResultSetHandler<T> rsh, Object... params)`,其中ResultSetHandler是一个接口,表示结果集处理者

### 4.小结

1. DBUtils: Apache开发的一个数据库工具包, 用来简化JDBC操作数据库的步骤
2. 步骤:
   1. 创建QueryRunner对象,传入连接池
   2. 调用update---增删改
   3. 调用query---查询



## 知识点-使用DBUtils完成增删改

### 1.目标

- [ ] 掌握使用DBUtils完成增删改

### 2.步骤

1. 拷贝jar包
2. 创建QueryRunner()对象,传入dataSource
3. 调用update()方法

### 3.实现

```java
public class UpdateTest {
    // 插入记录
    @Test
    public void insert() throws Exception {
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用update方法
        String sql = "insert into user values(null,?,?,?)";
        int rows = qr.update(sql, "tianqi", "123456", "田七");
        System.out.println("受影响的行数:" + rows);

    }

    // 删除记录  删除id为4的记录
    @Test
    public void delete() throws Exception {
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用update方法
        String sql = "delete from user where id = ?";
        int rows = qr.update(sql, 4);
        System.out.println("受影响的行数:" + rows);
    }

    // 修改记录  修改id为5的记录
    @Test
    public void update() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用update方法
        String sql = "update user set password = ? where id = ?";
        int rows = qr.update(sql,"abcd",3);
        System.out.println("受影响的行数:" + rows);
    }
}

```

### 4.小结

1. 创建QueryRuner()对象, 传入DataSource
2. 调用update(String sql, Object...params)

## 知识点-JavaBean

### 1.目标

+ 理解JavaBean的字段和属性

### 2.讲解

 1. JavaBean说白了就是一个类, 用来封装数据用的

 2. JavaBean要求

    + 私有字段  
    + 提供公共的get/set方法
    + 无参构造
    + 建议满参构造
    + 实现Serializable

3. 字段和属性

    + 字段:  全局/成员变量  eg: `private String username`
    + 属性: 去掉get或者set首字母变小写  eg: `setUsername-去掉set->Username-首字母变小写->username`

    > 一般情况下,我们通过IDEA直接生成的set/get 习惯把字段和属性搞成一样而言

### 3.小结

1. JavaBean用来封装数据用的
2. 字段和属性
   + 字段:  全局/成员变量  eg: `private String username`
   + 属性: 去掉get或者set首字母变小写  eg: `setUsername-去掉set->Username-首字母变小写->username`





## 知识点-使用DBUtils完成查询

### 1.目标

- [ ] 掌握使用DBUtils完成查询

### 2.步骤

1. 拷贝jar包
2. 创建QueryRunner()对象 传入DataSource
3. 调用query(sql, resultSetHandler,params)方法

### 3.实现

#### 3.1ResultSetHandler结果集处理接口的实现类:

![img](img/05.png)

#### 3.2代码实现

##### 3.2.1 查询一条记录(使用ArrayHandler)

```java
		// 查询一条记录(ArrayHandler)
    // 一条记录封装成一个数组,数组中的元素就是每列的值
    @Test
    public void select7() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select * from user where id = ?";
        Object[] arr = qr.query(sql, new ArrayHandler(), 3);
        System.out.println(Arrays.toString(arr));
    }
```

##### 3.2.2 查询一条数据封装到JavaBean对象中(使用BeanHandler)

```java
 @Test
    public void select1() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用query(sql,resultSetHandler,args)方法
        String sql = "select * from user where id = ?";
        User user = qr.query(sql, new BeanHandler<User>(User.class), 3);
        System.out.println(user);

    }
```

##### 3.2.3 查询一条数据,封装到Map对象中(使用MapHandler)

```java
  // 查询一条数据,封装到Map对象中(使用MapHandler)
    // 列名作为key,列中的值作为value
    @Test
    public void select3() throws Exception {
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select * from user where id = ?";
        Map<String, Object> map = qr.query(sql, new MapHandler(), 3);
        for (String key : map.keySet()) {
            System.out.println(key + ":" + map.get(key));
        }
    }
```

##### 3.2.4 查询多条数据封装到List<Object[]>中(使用ArrayListHandler)

```java
 @Test  // 查询所有记录(ArrayListHandler)
    public void select04() throws Exception {
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用query方法
        String sql = "select * from user";
        List<Object[]> list = qr.query(sql, new ArrayListHandler());
        for (Object[] arr : list) {
            System.out.println(Arrays.toString(arr));
        }

    }
```



##### 3.2.5 查询多条数据封装到List<JavaBean>中(使用BeanListHandler)

```java
	 @Test
    public void select2() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select * from user";
        List<User> list = qr.query(sql, new BeanListHandler<User>(User.class));
        for (User user : list) {
            System.out.println(user);
        }
    }
```



##### 3.2.6 查询多条数据,封装到`List<Map>`对象中(使用MapListHandler)

```java
  // 查询多条数据,封装到List<Map>对象中(使用MapListHandler)
    @Test
    public void select4() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select * from user";
        List<Map<String, Object>> list = qr.query(sql, new MapListHandler());
        for (Map<String, Object> map : list) {
            // map--->每条记录
            for (String key : map.keySet()) {
                System.out.println(key + ":" + map.get(key));
            }
            System.out.println("=================================================");
        }
    }

```

##### 3.2.7 查询单个数据(使用ScalarHandler())

```java
    // 查询单个数据(使用ScalarHandler())
    @Test
    public void select5() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select count(*) from user";
        Long count = (Long) qr.query(sql, new ScalarHandler());
        System.out.println("记录数:"+count);
    }
```

##### 3.2.8 查询单列多个值(使用ArrayListHandler)

```java
    // 查询单列多个值(ArrayListHandler)
    // 单列多值: 每列的数据封装成一个数组,再把所有的数组存储到List集合中
    // 多条记录: 每条记录的数据封装成一个数组,再把所有的数组存储到List集合中
    @Test
    public void select6() throws Exception{
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        // 2.调用query方法
        String sql = "select username from user";
        List<Object[]> list = qr.query(sql, new ArrayListHandler());
        for (Object[] arr : list) {
            System.out.println(Arrays.toString(arr));
        }
    }
```

##### 3.2.8 查询单列多个值(使用ColumnListHandler)

```java
// 查询单列多个值(使用ColumnListHandler)
    @Test
    public void select09() throws Exception {
        // 1.创建QueryRunner对象,传入连接池
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());

        // 2.调用query方法
        String sql = "select username from user";
        List<Object> list = qr.query(sql, new ColumnListHandler());
        System.out.println(list);
    }
```





### 4. 小结

1. 步骤

   + 创建QueryRunner() 对象传入DataSource
   + 调用query(sql,ResultSetHandler, params…)

2. ResultSetHandler

   + **BeanHandler()   		查询一条记录封装到JavaBean对象**
   + **BeanListHandler()     查询多条记录封装到List<JavaBean> list**
   + **ScalarHandler()        封装单个记录的 eg:统计数量**
   + **ArrayListHandler()    封装单列多行记录**
   + MapHandler()            查询一条记录封装到Map对象
   + MapListHandler()     查询多条记录封装到List<Map> list
   + ArrayHandler()    查询一条记录封装到数组中

   > 原理: 使用了反射+内省

3. 注意实现

   封装到JavaBean条件, 查询出来的数据的列名必须和JavaBean属性一致



# 第四章-自定义DBUtils

## 知识点-元数据  

### 1.目标

+ 能够说出什么是数据库元数据

### 2.分析

​	我们要自定义DBUtils, 就需要知道列名, 参数个数等, 这些可以通过数据库的元数据库进行获得.元数据在建立框架和架构方面是特别重要的知识,我们可以使用数据库的元数据来创建自定义JDBC工具包, 模仿DBUtils.

### 3.讲解

#### 3.1什么是元数据

​	元数据(MetaData)，即定义数据的数据。打个比方，就好像我们要想搜索一首歌(歌本身是数据)，而我们可以通过歌名，作者，专辑等信息来搜索，那么这些歌名，作者，专辑等等就是这首歌的元数据。因此数据库的元数据就是一些注明数据库信息的数据。

歌曲:凉凉

作词:刘畅

演唱: 杨宗纬 张碧晨

时长: 3分28秒

...

​	



​	简单来说: 数据库的元数据就是 数据库、表、列的定义信息。

​		①参数的元数据ParameterMetaData: 由PreparedStatement对象的getParameterMetaData ()方法来获取参数元数据对象

　　② 结果集的元数据ResultSetMetaData: 由ResultSet对象的getMetaData()方法来获取结果集元数据对象

#### 3.2ParameterMetaData

##### 3.2.1概述

​	ParameterMetaData是由preparedStatement对象通过getParameterMetaData方法获取而来，`ParameterMetaData`可用于获取有关`PreparedStatement`对象和`其预编译sql语句` 中的一些信息. eg:参数个数,获取指定位置占位符的SQL类型

​	![img](img/tu_10.png)

​	获得ParameterMetaData:

 	`ParameterMetaData parameterMetaData =  preparedStatement.getParameterMetaData ()`

##### 3.2.2ParameterMetaData相关的API

- int getParameterCount(); 获得参数个数
- int getParameterType(int param) 获取指定参数的SQL类型。 (注:MySQL不支持获取参数类型)

##### 3.2.3实例代码

```java
public class Test1_参数元数据 {
    public static void main(String[] args) throws Exception{
        /*
            ParameterMetaData类:
                概述: 是一个参数元数据类,可以用来获取参数的元数据
                使用:
                    1.获取参数元数据类对象
                        PreparedStatement对象调用getParameterMetaData()方法
                    2.获取参数的元数据
                        ParameterMetaData相关的API
                            - int getParameterCount(); 获得参数个数
                            - int getParameterType(int param) 获取指定参数的SQL类型。 (注:MySQL不支持获取参数类型)

         */
        // 1.获取连接
        Connection connection = C3P0Utils.getConnection();

        // 2.预编译sql语句
        String sql = "select * from user where username = ? and password = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 3.通过PreparedStatement对象获取参数元数据对象
        ParameterMetaData pmd = ps.getParameterMetaData();// 参数元数据对象

        // 4.获取参数个数元数据   参数个数就是参数的元数据
        int count = pmd.getParameterCount();
        System.out.println("sql语句的参数个数:"+count);// 2

        // 5.获取参数类型元数据  参数类型也是参数的元数据
        //int parameterType = pmd.getParameterType(1);
        //System.out.println(parameterType);

        //String typeName = pmd.getParameterTypeName(1);
        //System.out.println(typeName);

    }
}
```

#### 3.3ResultSetMetaData

##### 3.3.1概述

​	ResultSetMetaData是由ResultSet对象通过getMetaData方法获取而来，`ResultSetMetaData`可用于获取有关`ResultSet`对象中列的类型和属性的信息。

![img](img/tu_9-1573632938379.png)

​	获得ResultSetMetaData:

 	`ResultSetMetaData resultSetMetaData =  resultSet.getMetaData()`

##### 3.3.2resultSetMetaData 相关的API

- getColumnCount(); 获取结果集中列项目的个数
- getColumnName(int column); 获得数据指定列的列名
- getColumnTypeName();获取指定列的SQL类型
- getColumnClassName();获取指定列SQL类型对应于Java的类型

##### 3.2.3实例代码

```java
     public class Test2_结果集元数据 {
    public static void main(String[] args) throws Exception {
        /*
            ResultSetMetaData类:
                概述:是一个结果集元数据类,可以用来获取结果集的元数据
                使用:
                    1.获取结果集元数据类的对象
                        ResultSet的对象调用getMetaData()方法
                    2.获取结果集的元数据
                        ResultSetMetaData 相关的API
                        - getColumnCount(); 获取结果集中列项目的个数
                        - getColumnName(int column); 获得数据指定列的列名
                        - getColumnTypeName();获取指定列的SQL类型
                        - getColumnClassName();获取指定列SQL类型对应于Java的类型

         */
        // 1.获取连接
        Connection connection = C3P0Utils.getConnection();

        // 2.预编译sql语句
        String sql = "select * from user where username = ? and password = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 3.设置参数
        ps.setString(1, "zs");
        ps.setString(2, "123456");

        // 4.执行sql语句
        ResultSet resultSet = ps.executeQuery();

        // 5.获取结果集的元数据对象
        ResultSetMetaData rsmd = resultSet.getMetaData();

        // 1.获取列的数量
        int columnCount = rsmd.getColumnCount();
        System.out.println("列的个数:" + columnCount);//4

        // 2.获取列的名字
        for (int i = 1; i <= columnCount; i++) {
            System.out.println(rsmd.getColumnName(i));
        }
        System.out.println("==================================");
        // 3.获取列的MySQL类型
        for (int i = 1; i <= columnCount; i++) {
            System.out.println(rsmd.getColumnTypeName(i));
        }
        System.out.println("==================================");


        // 4.获取列在Mysql中的类型对应于java中的类型
        for (int i = 1; i <= columnCount; i++) {
            System.out.println(rsmd.getColumnClassName(i));
        }
    }
}

```

#### 4.小结

1. 元数据: 描述数据的数据. mysql元数据: 用来定义数据库, 表 ,列信息的 eg: 参数的个数, 列的个数, 列的类型...
2. mysql元数据: 
   + ==ParameterMetaData==
   + ResultSetMetaData



## 案例-自定义DBUtils增删改

### 1.需求

- [ ] 模仿DBUtils, 完成增删改的功能

### 2.分析

1. 创建MyQueryRunner类, 定义dataSource, 提供有参构造方法
2. 定义int update(String sql, Object...params)方法

```
//0.非空判断
//1.从dataSource里面获得connection
//2.根据sql语句创建预编译sql语句对象
//3.获得参数元数据对象, 获得参数的个数
//4.遍历, 从params取出值, 依次给参数? 赋值
//5.执行
//6.释放资源
```





### 3.实现

```java
public class MyQueryRunner {

    private DataSource dataSource;

    public MyQueryRunner(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public MyQueryRunner() {
    }

    /**
     * 增删改的方法
     * @param sql
     * @param args
     * @return rows
     */
    public int update(String sql,Object... args) throws Exception{
        // 非空判断
        // 连接池为空
        if (dataSource == null){
            throw new RuntimeException("连接池不能为null");
        }

        if (sql == null){
            throw  new RuntimeException("sql语句不能为空");
        }

         // 1.获得连接
        Connection connection = dataSource.getConnection();

        // 2.预编译sql语句
        PreparedStatement ps = connection.prepareStatement(sql);

        // 3.设置参数
        // 3.1 获得参数元数据类的对象
        ParameterMetaData pmd = ps.getParameterMetaData();

        // 3.2 根据参数元数据类的对象,获取参数个数(元数据)
        int count = pmd.getParameterCount();

        // 3.3 给参数赋值
        for (int i = 0; i < count; i++) {
            ps.setObject(i+1,args[i]);
        }

        // 4.执行sql语句
        int rows = ps.executeUpdate();

        // 5.释放资源
        C3P0Utils.release(null,ps,connection);

        // 6.返回影响的行数
        return  rows;
    }
}

```



### 4.小结

1. 先模拟DBUtils写出架子
2. update()  
   + 封装了PrepareStatement操作
   + 用到了参数元数据



# 总结

```java
必须练习:
	1.通过配置文件使用C3P0连接池
    2.通过配置文件使用DRUID连接池
    3.编写C3P0工具类   
    4.编写DRUID工具类
    5.DBUtils的增删查改-------->重点
        
- 能够理解连接池解决现状问题的原理
      问题: 连接被春节,使用完毕后就会被销毁,浪费资源
      连接池作用: 使得连接可以重复利用
      连接池原理:
		1. 程序一开始就创建一定数量的连接，放在一个容器(集合)中，这个容器称为连接池。
        2. 使用的时候直接从连接池中取一个已经创建好的连接对象, 使用完成之后 归还到池子
        3. 如果池子里面的连接使用完了, 还有程序需要使用连接, 先等待一段时间(eg: 3s), 如果在这段时间之内有连接归还, 就拿去使用; 如果还没有连接归还, 新创建一个, 但是新创建的这一个不会归还了(销毁)  
            
- 能够使用C3P0连接池
   1.导入相关jar包
   2.拷贝配置文件到src路径下(配置的文件的文件名只能是c3p0-config.xml)
   3.创建C3P0连接池对象
   4.获得连接
   5....
            
- 能够使用DRUID连接池
   1.导入相关jar包
   2.拷贝配置文件到src路径下
   3.创建Properties对象,加载配置文件中的数据
   4.创建DRUID连接池对象
   5.获得连接
   6....
            
- 能够编写C3P0连接池工具类
   1.在工具类中创建一个C3P0连接池对象(private static final修饰)
   2.提供一个获得连接池的静态方法
   3.提供一个获得连接的静态方法
   4.提供一个释放资源的方法
            
- 能够使用DBUtils完成CRUD
    1.创建QueryRunner对象,传入连接池
    2.调用update(String sql,Object... args)
      调用query(String sql,ResultSetHandle rsh,Object... args)
      
      查询结果:
		 单个值: ScalarHandler
         单列多行:ColumnListHandler\ArrayListHandler
         多行列多行:
       		一条记录:BeanHandler        ArrayHandler   MapHandler
			多条记录:BeanListHandler    ArrayListHandler   MapListHandler
- 能够理解元数据
     定义数据的数据
     参数元数据\结果集元数据
	参数元数据:ParameterMetaData类
     1.获取参数元数据对象
            由PreparedStatement对象调用getParameterMetaData()方法来获取参数元数据对象
     2.通过参数元数据对象获取参数的元数据
           - int getParameterCount(); 获得参数个数
 	结果集元数据: ResultSetMetaData类
      1.获取结果集元数据对象
             由ResultSet结果集对象调用getMetaData()方法来获取结果集元数据对象
       2.通过结果集元数据对象获取结果集的元数据
             - getColumnCount(); 获取结果集中列项目的个数
             - getColumnName(int column); 获得数据指定列的列名
             - getColumnTypeName();获取指定列的SQL类型
             - getColumnClassName();获取指定列SQL类型对应于Java的类型                
- 能够自定义DBUtils
      创建MyQueryRunner类
      在MyQueryRunner类中定义update方法(封装了jdbc操作,使用了参数元数据)
      
  
```



