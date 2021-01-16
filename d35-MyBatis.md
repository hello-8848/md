---
typora-copy-images-to: img
---



# day37-MyBatis

# 学习目标

- [ ] 理解Mybatis一级缓存
- [ ] 理解Mybatis二级缓存
- [ ] 掌握Mybatis一对一关系
- [ ] 掌握Mybatis一对多关系
- [ ] 掌握Mybatis多对多关系
- [ ] 理解Mybatis的延迟加载
- [ ] 了解Mybatis注解开发



# 第一章-MyBatis缓存【理解】

## 知识点-缓存概述

### 1.目标

- [ ] 掌握MyBatis缓存类别

### 2.路径

1. 缓存概述
2. 为什么使用缓存
3. 缓存的适用情况
4. MyBatis缓存类别

### 3.讲解

#### 3.1缓存概述

​	缓存就是一块内存空间.保存临时数据

![1578647652447](img/1578647652447.png)

#### 3.2为什么使用缓存

​	将数据源（数据库或者文件）中的数据读取出来存放到缓存中，再次获取的时候 ,直接从缓存中获取，可以减少和数据库交互的次数作,这样可以提升程序的==性能==！

#### 3.3缓存的适用情况

+ 适用于缓存的：==经常查询但不经常修改的==(eg: 省市,类别数据)，数据的正确与否对最终结果影响不大的
+ 不适用缓存的：经常改变的数据（实时波动的数据） , 敏感数据（例如：股市的牌价，银行的汇率，银行卡里面的钱)等等, 

#### 3.4MyBatis缓存类别

​	**一级缓存**：它是sqlSession对象的缓存，自带的(不需要配置)不可卸载的(不想使用还不行).  一级缓存的生命周期默认与sqlSession一致。

​	**二级缓存**：它是SqlSessionFactory（**mapper级别、名称空间级别、映射文件级别**）的缓存。同一个sqlsessionFactroy从同一个名称空间获取的sqlsession可以共享二级缓存的数据。二级缓存如果要使用的话，需要我们自己手动开启(需要配置的)。

​	**MyBatis缓存示意图**

![1578899379988](img/1578899379988.png)

### 4.小结

1. 缓存： 就是内存里面开辟的空间，存储一些临时数据
2. 适合缓存的数据： 经常访问，不经常变更的数据
3. mybatis的缓存：
   1. 一级缓存：是sqlsession级别
   2. 二级缓存：是映射文件级别、名称空间级别（sqlsessionFactory级别）

## 知识点-一级缓存 

### 1.目标

- [ ] 掌握MyBatis一级缓存

### 2.路径

1. 证明一级缓存的存在
2. 一级缓存分析
3. 测试一级缓存清空

### 3.讲解

#### 3.1证明一级缓存的存在

一个sqlsession，执行多次一样的查询语句。

```java
/**
     * 一个sqlsession，多次查询
     */
    @Test
    public void test(){
        SqlSession sqlSession = SqlSessionFactoryUtil.openSqlSession();


        UserDao userDao = sqlSession.getMapper(UserDao.class);


        User user = userDao.findById(1);
        System.out.println(user);


        // 再次查询，使用同一个sqlSession【不会去查询数据库，而是从缓存中获取了】
        // 一级缓存缓存的是一个对象，二者用等于符号比较得到true
        User user2 = userDao.findById(1);

        System.out.println(user == user2);

        SqlSessionFactoryUtil.commitAndClose(sqlSession);

    }
```

#### 3.2一级缓存分析

​	![img](img/tu_2-1572949193589.png)

​		第一次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，如果没有，从数据库查询用户信息。得到用户信息，将用户信息存储到一级缓存中。

​		第二次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，缓存中有，直接从缓存中获取用户信息。 

​		==如果 sqlSession 去执行 commit、close操作（执行插入、更新、删除DML操作），会清空 SqlSession 中的一级缓存==，这样做的目的为了让缓存中存储的是最新的信息，避免读到脏数据。

#### 3.3测试一级缓存清空

+ 一个sqlsession， 2次查询， 再搞一次（commit、DML操作）， 再来一次查询

```java
/**
     * 测试一级缓存清空
     */
    @Test
    public void testCacheClear(){
        SqlSession sqlSession = SqlSessionFactoryUtil.openSqlSession();


        UserDao userDao = sqlSession.getMapper(UserDao.class);


        User user = userDao.findById(1);
        System.out.println(user);


        // 再次查询，使用同一个sqlSession【不会去查询数据库，而是从缓存中获取了】
        // 一级缓存缓存的是一个对象，二者用等于符号比较得到true
        User user2 = userDao.findById(1);

        System.out.println(user == user2);


        /**
         * 测试清空开始.............
         * 执行DML操作、commit、close操作时都会清空一级缓存
         */
        /*User paramUser = new User();
        paramUser.setUid(1);
        paramUser.setUsername("zsf");
        userDao.updateById(paramUser);*/
        sqlSession.commit();


        // 再做一次查询
        User user3 = userDao.findById(1);

        System.out.println(user3 == user2);


        //SqlSessionFactoryUtil.commitAndClose(sqlSession);
        sqlSession.close();

    }
```



### 4.小结

1. 一级缓存：sqlsession级别
2. 一级缓存：在执行DML操作、commit、close会清除一级缓存



## 知识点-二级缓存

### 1.目标

- [ ] 掌握MyBatis二级缓存

### 2.路径

1. 二级缓存的结构
2. 二级缓存的使用
3. 二级缓存的测试

### 3.讲解

​	二级缓存是SqlSessionFactory（**mapper级别、名称空间级别、映射文件级别**）的缓存。只要是同一个SqlSessionFactory创建的SqlSession就共享二级缓存的内容，并且可以操作二级缓存.

#### 3.1二级缓存的结构

![img](img/tu_3-1572949193589.png)

#### 3.2二级缓存的使用

- 二级缓存的生成时机
  -  一级缓存清除时（close）会同步到二级缓存

+ 在mybatis核心配置文件中文件开启二级缓存 

![img](img/tu_4-1572949193589.png)

```xml	
<!--开启二级缓存-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>
```



==因为 cacheEnabled 的取值默认就为 true==，所以这一步可以省略不配置。为 true 代表开启二级缓存；为 false 代表不开启二级缓存。  

+ 配置相关的 Mapper 映射文件 

  `<cache/>` 标签表示当前这个 mapper 映射将使用二级缓存，区分的标准就看 mapper 的 namespace 值。 

  ![img](img/tu_5-1572949193589.png)

+ 配置 statement 上面的 useCache 属性 

  ​	将 UserDao.xml 映射文件中的 `<select>`标签中设置 ==useCache=”true”==代表当前这个 statement 要使用二
  级缓存，如果不使用二级缓存可以设置为 false。
  ​	注意： 针对每次查询都需要最新的数据 sql，要设置成 useCache=false，禁用二级缓存。 

  ```xml
  <cache/> 
  
  <!--useCache="true"表示该语句采用二级缓存-->
      <select id="findById" resultType="com.itheima.pojo.User" useCache="true">
          select * from t_user where uid = #{uid}
      </select>
  ```
  
  
  
  ![img](img/tu_6-1572949193589.png)

#### 3.3证明二级缓存

2个sqlsession各自从同一个mapper（或者dao映射文件）中获取代理对象，然后分别执行查看结果是否一致。

==当我们在使用二级缓存时，缓存的类一定要实现 java.io.Serializable 接口； **因为二级缓存的对象是序列化产生的，所以和一级缓存不一样，如果用等于符号比较可能永远都是false**==



【注意事项： sqlsession在close时才会同步数据到你的二级缓存】

```java
package com.itheima;

import com.itheima.bean.User;
import com.itheima.dao.UserDao;
import com.itheima.util.SqlSessionFactoryUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

/**
 * 二级缓存测试
 */
public class CacheLevel2Test {


    /**
     * 1. 在核心配置文件中开启全局的二级缓存配置，默认就是true
     * 2. 在映射文件中开启二级缓存 <cache/>
     * 3. 在语句中开启二级缓存  useCache="true"
     * 【注意事项】：javabean必须实现序列化接口
     */
    @Test
    public void test(){
        // 2个sqlsession对象，都操作同一个映射文件的同一个方法
        SqlSession sqlSession1 = SqlSessionFactoryUtil.openSqlSession();
        UserDao userDao1 = sqlSession1.getMapper(UserDao.class);
        User user1 = userDao1.findById(1);
        //【注意事项： sqlsession在close时才会同步数据到你的二级缓存】
        SqlSessionFactoryUtil.commitAndClose(sqlSession1);

        SqlSession sqlSession2 = SqlSessionFactoryUtil.openSqlSession();
        UserDao userDao2 = sqlSession2.getMapper(UserDao.class);
        User user2 = userDao2.findById(1);
        SqlSessionFactoryUtil.commitAndClose(sqlSession2);

        System.out.println(user1);
        System.out.println(user2);
        // 二级缓存的对象是经过序列化后得到的对象（你会用==符号判断，可能永远都是false）
        System.out.println(user1 == user2);


        /**
         * 继续操作
         */
        SqlSession sqlSession3 = SqlSessionFactoryUtil.openSqlSession();
        UserDao userDao3 = sqlSession3.getMapper(UserDao.class);
        User user3 = userDao3.findById(1);

        SqlSession sqlSession4 = SqlSessionFactoryUtil.openSqlSession();
        UserDao userDao4 = sqlSession4.getMapper(UserDao.class);
        User user4 = userDao4.findById(1);





    }
}

```

​	Cache Hit： 缓存命中的意思。

经过上面的测试，我们发现执行了两次查询，并且在执行第一次查询后，我们关闭了一级缓存，再去执行第二次查询时，我们发现并没有对数据库发出 sql 语句，所以此时的数据就只能是来自于我们所说的二级缓存。 



==实际生产中，mybatis二级缓存一般不用！！==

### 4.小结

#### 4.1注意事项 

​	当我们在使用二级缓存时，缓存的类一定要实现 java.io.Serializable 接口，这种就可以使用序列化	方式来保存对象。 

![img](img/tu_7-1572949193590.png)





# 第二章-Mybatis 的多表关联查询【掌握】

## 知识点-一(多)对一

### 1.需求

​	本次案例以简单的用户和账户的模型来分析 Mybatis 多表关系。用户为 User 表，账户为Account 表。一个用户（User）可以有多个账户（Account）,但是一个账户(Account)只能属于一个用户(User)。具体关系如下： 

 ![img](img/tu_19.png)

​	==查询所有账户信息， 并且关联查询出该账户的用户名和地址==。

​	因为一个账户信息只能供某个用户使用，所以从查询账户信息出发关联查询用户信息为一对一查询。

- 数据库的准备

```java
CREATE TABLE t_account(
		aid INT PRIMARY KEY auto_increment,
		money DOUBLE,
		uid INT
);
ALTER TABLE t_account ADD FOREIGN KEY(uid) REFERENCES t_user(uid);

INSERT INTO `t_account` VALUES (null, '1000', '1');
INSERT INTO `t_account` VALUES (null, '2000', '1');
INSERT INTO `t_account` VALUES (null, '1000', '2');
INSERT INTO `t_account` VALUES (null, '2000', '2');
INSERT INTO `t_account` VALUES (null, '800', '3');
```

### 2.分析

+ 查询语句

```mysql
#  查询所有账户信息， 并且关联查询出该账户的用户名和地址

# 多表查询技术： 连接（内连接、外连接）、子查询

# 有账户肯定有用户！---你有我还有大家有---内连接

# 内连接：隐式、显示
#隐式
SELECT ...  FROM a,b WHERE a.列=b.列
#显示
SELECT ... FROM a JOIN b ON a.列=b.列


SELECT a.*, u.username, u.address FROM t_account AS a, t_user AS u WHERE a.uid = u.uid;

SELECT a.*, u.username, u.address FROM t_account AS a JOIN t_user AS u ON a.uid = u.uid;



```

### 3.实现

#### 3.1方式一： 使用增加字段的实体类

+ Account.java

```java
@Getter
@Setter
public class Account {
	private Integer aid;
	private Integer uid;
	private Double money;
}
```

+ AccountVo.java

  ​	为了能够封装上面 SQL 语句的查询结果，定义 AccountCustom类中要包含账户信息同时还要包含用户信息。

```java
package com.itheima.bean;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import java.io.Serializable;

@Getter
@Setter
@ToString
public class AccountVo  implements Serializable {
    private Integer aid;
    private Integer uid;
    private Double money;
    private String username;// 用户姓名
    private String address;// 地址
}

```

+ AccountDao.java

```java
public interface AccountDao {

    List<AccountVo> findAccountInfo();

}

```

+ AccountDao.xml

```xml
<!--namespace: 名称空间，接口的全限定名-->
<mapper namespace="com.itheima.dao.AccountDao">

    <!--方式一： 一对一实现-->
    <select id="findAccountInfo" resultType="AccountVo">
        SELECT a.*, u.username, u.address FROM t_account AS a , t_user AS u  WHERE a.uid = u.uid
    </select>

</mapper>
```

#### 3.2方式二 association 标签映射

==使用resultMap完成映射关系的配置==

==`<association >`====标签可以放在resultMap中用作子标签，可以用来处理一对一映射关系==。association 标签拥有以下：

- 属性
  - property属性：映射到的java实体的属性名
  - javaType属性：对应的java类型
  - column属性：需要关联的表字段
- 子标签
  - id标签：唯一标志
    - 属性property：java属性
    - 属性column：表的字段
  - result标签：其他普通字段
    - 属性property：java属性
    - 属性column：表的字段

==Account-----属于一个User：如何在类里面表示？在Account类里面写一个字段就是User类型即可！！！==

+ 修改Account.java

  在 Account 类中加入 User类的对象作为 Account 类的一个属性。 

 ![1540005275991](img/1540005275991.png)

+ AccountDao.java

```java
public interface AccountDao {
    // 方式二：使用association标签查询
    // 用Account对象封装数据（因为此时的Account可以表示所属哪个用户了）
    List<Account> findAccountInfo2();

}
```

+ AccountDao.xml

```xml
<!--方式二：一对一，association标签-->
    <select id="findAccountInfo2" resultMap="findAccountInfo2Map">
        SELECT a.*, u.username, u.address
        FROM t_account AS a, t_user AS u
        WHERE a.uid = u.uid
    </select>
    <resultMap id="findAccountInfo2Map" type="Account">
        <!--设置自身属性-->
        <id column="aid" property="aid"/>
        <result column="money" property="money"/>
        <result column="uid" property="uid"/>


        <!--association标签：
            property： 表示一对一的属性的名字，account的user属性
            javaType：表示一对一的属性的类型
            column： 关联的表的字段名
        -->
        <association property="user" javaType="User" column="uid">
            <id column="uid" property="uid"/>
            <result column="username" property="username"/>
            <result column="address" property="address"/>
        </association>

    </resultMap>
```



#### 3.3方式三 association 标签嵌套查询

同样使用resultMap结合association标签，但是使用association的另外属性，可以进行嵌套查询。

- column属性：嵌套查询使用的匹配条件字段
- select属性：写==名称空间.select标签的id==，可能关联引用另外的映射文件的select标签
- property属性：关联的属性

原始语句：

SELECT a.*, u.username, u.address FROM t_account AS a, t_user AS u   WHERE a.uid = u.uid

分离步骤来做：

- 查账户: SELECT * FROM t_account 
- 再根据账户去查用户信息： SELECT *  FROM t_user WHERE uid = 2； uid来自于账户的uid字段

上面的案例可以改写成：

- AccountDao.java

```java
// 方式三： 使用association标签进行嵌套查询
List<Account> findAccountInfo3();
```



- AccountDao.xml

```xml
<!--方式三：association标签嵌套查询-->
    <select id="findAccountInfo3" resultMap="findAccountInfo3Map">
        select * from t_account
    </select>
    <resultMap id="findAccountInfo3Map" type="Account">
        <!--设置自身属性-->
        <id column="aid" property="aid"/>
        <result column="money" property="money"/>
        <result column="uid" property="uid"/>
        

        <!--
            association的
                    select属性： 写名称空间.select标签的id值
                                        根据账户的uid字段去用户表查询用户信息
                    column属性： 关联的字段
                    注意事项： 需要在UserDao定义findByUid方法
        -->
        <association property="user" column="uid"
                     select="com.itheima.dao.UserDao.findByUid"/>
    </resultMap>
```

- UserDao.xml

```xml
<!--一对一嵌套查询需要的一个方法：根据账户的uid字段去用户表查询用户信息-->
<select id="findByUid" resultType="User">
    select * from t_user where uid = #{uid}
</select>
```

- UserDao.java

```java
// 一对一嵌套查询需要的一个方法：根据账户的uid字段去用户表查询用户信息
User findByUid(Integer uid);
```



### 4.小结

1. 表达关系:

   + 实体类Account类里面增加一个属性User（一对一关系：账户所属一个用户的含义）

   ![image-20191227102112748](img/image-20191227102112748.png)  

   + 映射文件

   采用来了 association标签：3种，哪种舒服就用哪种。
   
    



## 知识点-一对多

### 1.需求

​		查询所有用户信息及用户关联的账户信息。

### 2.分析


​	分析： 用户信息和他的账户信息为一对多关系，并且查询过程中如果用户没有账户信息，此时也要将用户信息查询出来，我们想到了左外连接查询比较合适。 

+ sql语句

```mysql
#  查询所有用户信息及用户关联的账户信息。

SELECT ... FROM a LEFT JOIN b ON a.列=b.列

# 左外连接查询所有用户信息及用户关联的账户信息
SELECT * FROM t_user AS u LEFT JOIN t_account AS a ON u.uid = a.uid;
```

### 3.实现

#### 3.1 方式一 collection标签映射

resultMap的子标签==collection==可以实现一对多的映射配置。collection 标签如下：

- 属性
  - property属性： 一般是集合属性名
  - ==ofType==属性： 集合的泛型
- 子标签
  - id标签：唯一标志
    - 属性property：java属性
    - 属性column：表的字段
  - result标签：其他普通字段
    - 属性property：java属性
    - 属性column：表的字段



+ Account.java

```java
public class Account {
    private Integer aid;
    private Integer uid;
    private Double money;
}
```

+ User.java

  ​	为了能够让查询的 User 信息中，带有他的个人多个账户信息，我们就==需要在 User 类中添加一个集合，
  用于存放他的多个账户信息==，这样他们之间的关联关系就保存了。 

```java
public class User implements Serializable{
    private int uid;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址

    /// 配置一对多关系映射：一个用户拥有多个账户
    List<Account> accounts;
}
```

+ UserDao.java

```java
public interface UserDao {
   // 方式一：一对多关系实现
    List<User> findUserInfoMultiTable();

}
```

+ UserDao.xml

```xml
<select id="findUserInfoMultiTable" resultMap="userMap">
        SELECT u.*, a.aid, a.money FROM t_user AS u LEFT JOIN t_account AS a ON u.uid = a.uid
    </select>
    <resultMap id="userMap" type="User">
        <!--user自身属性-->
        <id column="uid" property="uid"/>
        <result column="username" property="username"/>
        <result column="address" property="address"/>
        <result column="sex" property="sex"/>
        <result column="birthday" property="birthday"/>

        <!--collection标签：  一对多的关系
                    property属性： 一对多关系的属性字段accounts
                    ofType属性： 一对多关系的属性字段accounts的类型
        -->
        <collection property="accounts" ofType="Account" column="uid">
            <!--配置一对多关系的account的属性字段-->
            <id column="aid" property="aid"/>
            <result column="money" property="money"/>
            <result column="uid" property="uid"/>
        </collection>
    </resultMap>
```

#### 3.2 方式二 collection标签嵌套查询

同样使用collection标签，但是属性改变一下：

- column属性：嵌套查询使用的匹配条件字段
- select属性：写==名称空间.select标签的id==，可能关联引用另外的映射文件的select标签
- property属性：关联的属性
- ofType属性： 泛型的类型



分离步骤：

先查用户：SELECT * FROM t_user

根据用户查询它的账户：select * from t_account where uid = 1

- UserDao.java

```java
//方式二：一对多关系实现(collection标签的嵌套查询)
 List<User> findUserInfoMultiTable2();

```



- UserDao.xml

```xml
<!--方式二：collection标签的嵌套查询-->
    <select id="findUserInfoMultiTable2" resultMap="findUserInfoMultiTable2Map">
        SELECT * FROM t_user
    </select>
    <resultMap id="findUserInfoMultiTable2Map" type="User">
        <!--配置用户自身属性-->
        <id column="uid" property="uid"/>
        <result column="username" property="username"/>
        <result column="sex" property="sex"/>
        <result column="birthday" property="birthday"/>
        <result column="address" property="address"/>
        
        <!--嵌套查询一对多的属性 accounts
            collection标签：
                    property： 表示一对多的属性名，在这里就是accounts
                    column： 关联的字段
                    ofType： 一对多的列表的泛型的类型
                    select： 写名称空间.select的id属性值，表示嵌套查询

                    fetchType="lazy"表示懒加载
        -->
        <collection property="accounts" column="uid" ofType="Account"
                    select="com.itheima.dao.AccountDao.findByUid" fetchType="lazy"/>



    </resultMap>
```

- AccountDao.xml

```xml
<!--一对多的实现：根据uid查询账户信息列表-->
<select id="findByUid" resultType="Account">
    select * from t_account where uid = #{uid}
</select>
```





### 4.小结

1. 语句, 建议使用外连接. 用户可以没有账户的, 但是用户信息需要查询出来, 账户的个数就为0

```
SELECT u.*, a.aid, a.money  FROM t_user u LEFT OUTER JOIN t_account a ON u.uid = a.uid
```

2. 表达关系

   + User实体类

   ![image-20191227105521900](img/image-20191227105521900.png) 

   + 映射文件 使用collection标签实现（方式一、方式二可选）


## 知识点-多对多（实现就是一对多）

### 1. 需求

​	通过前面的学习，我们使用 Mybatis 实现一对多关系的维护。多对多关系其实我们看成是==**双向的一对多关系**==。用户与角色的关系模型就是典型的多对多关系.

![img](img/tu_21.png)

​	需求：实现查询所有角色并且获取它所分配的用户信息。 

回忆技术：

```mysql
#  实现查询所有角色并且获取它所分配的用户信息。 

SELECT * FROM t_role;  #查询所有角色

# 角色表没有用户信息，所以只能根据关系表去查用户信息了

# 根据rid去查询关系表数据（就包含了用户的id）
SELECT * FROM user_role  WHERE rid = 1

# 根据用户的id'获取用户信息
SELECT * FROM t_user WHERE uid = 1


#  多表连接查询
SELECT r.*, u.* FROM t_role AS r 
LEFT JOIN user_role AS ur 
ON r.rid = ur.rid
LEFT JOIN t_user AS u 
ON u.uid = ur.uid
```

![1594527399296](img/1594527399296.png)

+ 建表语
+ 句

```sql
CREATE TABLE t_role(
	rid INT PRIMARY KEY AUTO_INCREMENT,
	rName varchar(40),
	rDesc varchar(40)
);
INSERT INTO `t_role` VALUES (null, '校长', '负责学校管理工作');
INSERT INTO `t_role` VALUES (null, '副校长', '协助校长负责学校管理');
INSERT INTO `t_role` VALUES (null, '班主任', '负责班级管理工作');
INSERT INTO `t_role` VALUES (null, '教务处主任', '负责教学管理');
INSERT INTO `t_role` VALUES (null, '班主任组长', '负责班主任小组管理');


-- 中间表(关联表)
CREATE TABLE user_role(
	uid INT,
	rid INT
);

ALTER TABLE  user_role ADD FOREIGN KEY(uid) REFERENCES t_user(uid);
ALTER TABLE  user_role ADD FOREIGN KEY(rid) REFERENCES t_role(rid);

INSERT INTO `user_role` VALUES ('1', '1');
INSERT INTO `user_role` VALUES ('3', '3');
INSERT INTO `user_role` VALUES ('2', '3');
INSERT INTO `user_role` VALUES ('2', '5');
INSERT INTO `user_role` VALUES ('3', '4');
```

### 2.分析

​	查询角色我们需要用到 Role 表，但角色分配的用户的信息我们并不能直接找到用户信息，而是要通过中间表(USER_ROLE 表)才能关联到用户信息。
下面是实现的 SQL 语句： 

```mysql
#  实现查询所有角色并且获取它所分配的用户信息。 

SELECT * FROM t_role;  #查询所有角色

# 角色表没有用户信息，所以只能根据关系表去查用户信息了

# 根据rid去查询关系表数据（就包含了用户的id）
SELECT * FROM user_role  WHERE rid = 1

# 根据用户的id'获取用户信息
SELECT * FROM t_user WHERE uid = 1


#  多表连接查询
#  多表连接查询
SELECT r.*, u.* FROM t_role AS r 
LEFT JOIN user_role AS ur 
ON r.rid = ur.rid
LEFT JOIN t_user AS u 
ON u.uid = ur.uid
```

### 3.实现

+ User.java

```java
public class User implements Serializable{
    private int uid;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址
}
```

+ Role.java

```java
public class Role {
    private Integer rid;
    private String rName;
    private String rDesc;

    // 一个角色--》多个用户
    List<User> users;
	
}
```

+ RoleDao.java

```java
public interface RoleDao {
   List<Role> findAllMultiTable();
}
```

+ RoleDao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace: 名称空间，接口的全限定名-->
<mapper namespace="com.itheima.dao.RoleDao">
    
    <select id="findAllMultiTable" resultMap="findAllMultiTableMap">
        SELECT r.*, u.* FROM t_role AS r
        LEFT JOIN user_role AS ur
        ON r.rid = ur.rid
        LEFT JOIN t_user AS u
        ON u.uid = ur.uid
    </select>
    <resultMap id="findAllMultiTableMap" type="Role">
        <!--角色表自身属性配置-->
        <id column="rid" property="rid"/>
        <result column="rName" property="rName"/>
        <result column="rDesc" property="rDesc"/>


        <!--配置角色对应的多个用户： 一对多： collection标签-->
        <collection property="users" ofType="User">
            <id column="uid" property="uid"/>
            <result column="username" property="username"/>
            <result column="sex" property="sex"/>
            <result column="birthday" property="birthday"/>
            <result column="address" property="address"/>
        </collection>
    </resultMap>

</mapper>


```

### 4.小结

1. ==多对多本质就是两个1对多. 实现起来基本和1对多是一样的==

2. 表达关系

   + 实体类

   ![image-20191227112219584](img/image-20191227112219584.png) 

   + 映射文件

   ![image-20191227112238204](img/image-20191227112238204.png) 
   
   

# 第三章-Mybatis 延迟加载策略 

## 知识点-Mybatis 延迟加载策略

### 1.目标

​	通过前面的学习，我们已经掌握了 Mybatis 中一对一，一对多，多对多关系的配置及实现，可以实现对象的关联查询。

​	实际开发过程中很多时候我们并不需要总是在加载一方信息时就一定要加载另外一方的信息。 此时就是我们所说的延迟加载。

​	等用到另外一方(和当前关联的那一份)的数据的时候, 再去查询;

​	如果用不到, 不查询.

### 2.路径

1. 什么是延迟加载 
2. 使用 Assocation 实现延迟加载 (多对一,一对一)
3. Collection 实现延迟加载  (一对多)

### 3.讲解

#### 3.1什么是延迟加载

​	==延迟加载：就是在需要用到数据时才进行加载，不需要用到数据时就不加载数据。延迟加载也称懒加载.==

==**配置：在嵌套查询中的association、collection标签中添加属性fetchType="lazy"可以开启懒加载（延迟加载）**==

​	坏处： 因为只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，因为查询工作也要消耗时间，所以可能造成用户等待时间变长，造成用户体验下降。  

​	好处： 先从单表查询，需要时再从关联表去关联查询，大大提高数据库查询性能，因为查询单表要比关联查询多张表速度要快.

#### 3.2使用 Assocation 实现延迟加载 (多对一,一对一)

==**在嵌套查询**中,给association增加属性fetchType="lazy"==

#### 3.3Collection 实现延迟加载  (一对多,多对多)

==在**嵌套查询**中给Collection增加属性fetchType="lazy"==

### 4.总结

| 类别     | 特点                                               |
| -------- | -------------------------------------------------- |
| 立即加载 | 只要一调用方法，则马上发起查询                     |
| 延迟加载 | 只有在真正使用时，才发起查询，如果不用，则不查询。 |

# 第四章-MyBatis注解开发（了解）

​	mybatis的注解方式仅作了解，实际上互联网公司真实开发场景中没人会用注解。注解适合demo级别、业务量很低的不活跃的项目。

## 知识点-使用 Mybatis 注解实现基本CRUD 

### 1.目标

- [ ] 使用 Mybatis 注解标记方法来实现基本CRUD

### 2.路径

1. @Insert:实现新增
2. @Update:实现更新
3. @Delete:实现删除
4. @Select:实现查询
5. @SelectKey:保存之后 获得保存的id

### 3.实现

+ Dao

```java
public interface UserDao {

    /**
     * 注解开发---CRUD测试
     */

    // 1. 新增、插入； 获得主键值

    /**
     * keyProperty： 主键的属性名
     * before ：布尔值，为true表示插入之前获取主键值； mysql写false因为是插入之后才能获取主键值
     * resultType：主键的返回类型，类型.class
     * statement：生成主键的这个数据库里面的一个语句
     * @param user
     * @return
     */
    @SelectKey(keyProperty= "uid", before = false, resultType = Integer.class,
            statement = "SELECT LAST_INSERT_ID()"
    )
    @Insert(
            "insert into t_user values(null, #{username}, #{sex}, #{birthday}, #{address})"
    )
    int addAnno(User user);

    //2. 修改
    @Update("update t_user set address = #{address}  where uid = #{uid}")
    int updateAnno(User user);


    //3. 查询
    @Select(" select * from t_user where uid = #{uid}")
    User queryAnno(Integer uid);

    //4. 删除
    @Delete("delete from t_user where uid = #{uid}")
    int deleteAnno(Integer uid);
}

```

+ mybatis-config.xml

 ![1535361902219](img/1535361902219.png)

### 4.小结

1. 听听就可以了，有时间可以自己练习一把



## 知识点-使用Mybatis注解实现复杂关系映射开发

### 1.目标

- [ ] 掌握Mybatis注解开发

### 2.路径

1. 复杂关系映射的注解说明 
2. 使用注解实现一对一复杂关系映射及延迟加载
3. 使用注解实现一对多复杂关系映射及延迟加载 

### 3.讲解 

​	实现复杂关系映射。前面我们可以在映射文件中通过配置`<resultMap>`来实现，也有对应的注解。

​	下面我们一起来学习@Results 注解， @Result 注解， @One 注解， @Many注解。 

#### 3.1复杂关系映射的注解说明

+ ==@Results 注解 , 代替的是标签`<resultMap>`==

```java
//该注解中可以使用单个@Result 注解，也可以使用@Result 集合
@Results（{@Result()， @Result() }）或@Results（@Result())
@select("")

```

+ ==@Resutl 注解 ,代替了 `<id>`标签和`<result>`标签，用 id属性值区分可以== ,

```java
@Results(@Result(column="列名",property="属性名",one=@One(select="指定用来多表查询的 sqlmapper"),many=@Many(select="")))
@select("")


@Resutl 注解属性说明	
    column 数据库的列名
    Property 需要装配的属性名
    one 需要使用的@One 注解（@Result（one=@One）（）））
    	@One 的属性：select、fetchType
    many 需要使用的@Many 注解（@Result（many=@many）（）））
    	@Many 的属性：select、fetchType

```

+ @Result注解的子注解

  + ==@Result注解结合-属性one(是一个@One 注解表示一对一)=`<assocation>`标签==，是多表查询的关键，在注解中用来指定子查询返回单一对象。 

    ```java
    @Result(column="列名",property="属性名",one=@One(select="指定用来多表查询的 sqlmapper"))
    ```

  + ==@Result注解结合-属性many(是一个@Many 注解表示一对多)=`<Collection>`标签==,是是多表查询的关键，在注解中用来指定子查询返回对象集合 

  ​	注意：聚集元素用来处理“一对多”的关系。需要指定映射的 Java 实体类的属性，属性的 javaType（一般
  为 ArrayList） 但是注解中可以不定义； 

  ```java
  @Result(property="",column="",many=@Many(select=""))
  ```

  

#### 3.2使用注解实现一对一复杂关系映射及延迟加载

##### 3.2.1需求

​	查询账户(Account)信息并且关联查询用户(User)信息。

​	

##### 3.2.2实现

+ User.java

```java
public class User implements Serializable{
    private int uid;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址
 }

```

- Account.java

```java
public class Account {
    private Integer aid;
    private Integer uid;
    private Double money;

    //表达关系:1个用户对应1个账户
    private User user;
}

```

- AccountDao.java

![img](img/tu_9-1572949193590.png)

```java
public interface AccountDao {
    // 注解实现一对一映射关联查询

    /**
     * @Select+@Results一块实现
     * @Results： resultMap标签，属性value是一个@Result注解的数值类型
     *              id标签： @Result表示，使用id属性值为true
     *              result标签：@Result表示，使用id属性值为false
     *              assoiation标签： @Result+设置one属性为一个@One注解（select属性为名称空间.select的id属性值）
     *
     * @return
     */
    @Results(value={
            @Result(id = true, property="aid", column="aid"),
            @Result(property="money", column="money"),
            @Result(property="uid", column="uid"),
            @Result(property="user", column = "uid",
                    one=
                        @One(select="com.itheima.dao.UserDao.findByUid", fetchType= FetchType.LAZY)
            )
    })
    @Select("select * from t_account")
    List<Account> findAnno();
}

```

+ UserDao.java

```java
public interface UserDao {
    // 一对一嵌套查询的语句
    User findUserByUid(Integer uid);
}

```

#### 3.3使用注解实现一对多复杂关系映射及延迟加载

##### 3.3.1需求

​	完成加载用户对象时，查询该用户所拥有的账户信息。 

​	等账户信息使用的时候再查询.

##### 3.3.2实现

+ User.java

```java
public class User {
    private Integer uid;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址

    //用于保存用户的多个账户信息
    private List<Account> accounts;
    
	//...    
}

```

- UserDao.java

![img](img/tu_10-1572949193590.png)

```java
public interface UserDao {
    /**
     * 注解实现一对多关系映射
     */
    @Results(value = {
            @Result(id = true, property = "uid", column = "uid"),
            @Result(property = "username", column = "username"),
            @Result(property = "sex", column = "sex"),
            @Result(property = "birthday", column = "birthday"),
            @Result(property = "address", column = "address"),
            /*一对多的账户信息列表*/
            @Result(property = "accounts", column = "uid",
                    many=@Many(select = "com.itheima.dao.AccountDao.findByUid", fetchType = FetchType.LAZY)
            )
    })
    @Select("SELECT * FROM t_user")
    List<User> findAnno();

}

```

+ AccountDao.java

```java
public interface AccountDao {

    /**
     * 根据uid查询当前用户的账户
     *
     * @return
     */
   // 一对多的嵌套查询： 根据uid查询账户信息
    List<Account> findByUid(Integer uid);

}

```

### 4.小结

1. @Results注解代替ResultMap标签
2. @Result注解代替了id、result、assosiation、collection标签
   1. id、result区分是使用@Result注解的属性id的值决定的
   2. assosiation： @Result注解+one属性（@One注解）
   3. collection：@Result注解+many属性（@Many注解）

```
@Results(value={
	@Result(column = "列名",property = "属性名",id = boolean值),
	@Result(column = "列名",property = "属性名", one=@One(select="", fetchType="") ),
  @Result(column = "列名",property = "属性名", many=@Many(select="", fetchType="") )
})
```





 **开启下划线自动转驼峰命名配置**

有些开发规范，在数据库中的字段可能单词之间以下划线分隔，而在java中通常是驼峰命名。

所以我们可能会通过取别名、或者resultMap进行额外的映射配置。

其实还可以更简化，我们在mybatis核心配置文件开启配置：

```xml
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```





# 总结

- 缓存
  - 内存中的一块空间，存放临时数据---》提升程序的查询性能
  - 适合缓存的数据： 经常查询、不容易变更的数据
- mybatis的缓存
  - 一级缓存
    - sqlSession级别
    - 在进行DML、commit、close的时候会清空一级缓存
  - 二级缓存【一级缓存close会同步到二级缓存】
    - 名称空间、映射文件级别
    - 使用二级缓存(javabean需要实现序列化接口)
      - 在核心配置文件中开启
      - 在映射文件中开启`<cache/>`
      - 在sql标签中加上useCache="true"
- 多表复杂查询
  - 一对一
    - 方式一：通过给类增加字段和结果集字段一致，直接可以映射
    - 方式二：association标签
    - 方式三：association标签嵌套查询
  - 一对多
    - 方式一： collection标签
    - 方式二：collection标签嵌套查询
  - 多对多： 就是一对多，collection标签实现
- 延迟加载：
  - 在进行一对一、一对多嵌套查询时只需要在association标签、collection标签增加fetchType="lazy"
- 注解：不常用，练习完毕后可以练习注解







复习方向：

- 在页面的输入项中检查用户输入的内容格式要求： 用户名长度大于5个字符、不超过20个字符【正则】
- 发送数据时要求发送ajax、要求发送json格式的数据，在后台要接收到
  - vue的axios的ajax
  - axiox.post("路径"， {}).then(response=>{...})
  - JsonUtils可以用
- 创建数据库、表

