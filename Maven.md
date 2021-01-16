# Maven高级

教学目标

- Maven基础回顾
- jar包冲突（解决方案：直接排除，直接依赖）


- 工程分解（一个父工程，四（n）个子工程）  --重点



操作系统要关闭自动更新


# 1. Maven基础回顾

### 【目标】

回顾Maven基础课程

### 【路径】

1：Maven好处

2：安装配置Maven

3：三种仓库

4：常见命令

5：坐标的书写规范

6：如何添加坐标

7：依赖范围

### 【讲解】

## 1.1.  回顾[理解]

什么是Maven？

![img](./img/001.png) 

### 1.1.1. Maven好处

1. 节省磁盘空间

2. 可以一键构建

![img](./img/002.png) 

3. 可以跨平台

4. 应用在大型项目时可以提高开发效率（jar包管理，maven的工程分解，可分模块开发）

### 1.1.2. 安装配置 maven

注意： 3.5+版本需要 jdk1.8+以上的支持



MAVEN_HOME

JDK

JAVA_HOME =>1.8  64位

path:%JAVA_HOME%/bin;

### 1.1.3. 三种仓库

1. 本地仓库 conf/settings.xml localRepository

   idea 本地仓库跟settings.xml配置要一致

2. 远程仓库（私服）, 公司内部   工作中：找私服ip和端口

3. 中央仓库

### 1.1.4. 常见的命令

1. Compile
2. Test
3. Package（jar，war）
4. Install（安装到本地仓库）
5. Deploy（部署到远程仓库（私服））

6. Clean（target下生成的文件）

### 1.1.5. 坐标的书写规范

1. groupId 公司或组织域名的倒序

2. artifactId 项目名或模块名

3. version 版本号

![img](./img/003.png) 

### 1.1.6. 如何添加坐标

1.在本地仓库中搜索

2.互联网上搜，推荐网址 <http://www.mvnrepository.com/>  

或百度 httpclient3.1 maven

![img](./img/004.png) 

### 1.1.7. 依赖范围

<scope>属性：

1. Compile（默认）


2. Test


3. Provided


4. Runtime

![img](./img/005.png) 

例如：

![img](./img/006.png) 

### 【小结】

1：Maven好处（了解）  依赖的管理，一键构建

2：安装配置Maven（熟练）

3：三种仓库（本地仓库(阿里镜像)，中央仓库（远程仓库），私服（代理仓库））

4：常见命令（clean compile test package install deploy）

5：坐标的书写规范（groupId artifactId version）

6：如何添加坐标（http://www.mvnrepository.com/）

7：依赖范围 compile test provided runtime

8 : 私服的配置（settings.xml配置私服 工作中大概率要配置的）

# 2.  Maven中jar包冲突问题

### 【目标】

使用maven依赖导入jar包，会不会出现导入相同的jar包，且版本不一致呢？

**直接引入a.jar  依赖   b-1.0.jar**

**直接引入c.jar  依赖    b-2.0.jar**

### 【路径】

1：演示jar包冲突问题：遵循第一声明者优先原则

2：解决jar包冲突问题，路径近者优先

3：解决jar包冲突问题，直接排除法

### 【讲解】

创建一个jar包工程，流程如下图：

![img](./img/007.png) 

## 2.1. 第一声明优先原则

​	哪个jar包在靠上的位置，这个jar包就是先声明的，先声明的jar包下的依赖包，可以优先引入项目中。

​	我们在pom.xml中引入如下坐标，分别是spring中不同的版本。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>maven_day01_demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>jar</packaging>

    <!--导入相关依赖包-->
    <dependencies>
        <!--引入spring-context，它所以来的包都会导入进来-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

 

我们在控制面板的maven面板，点击查看依赖关系按钮，看到了包和包之间的依赖关系存在冲突，都使用了spring-core包，关系图如下：

![img](./img/008.png) 

我们再来看看他们依赖包的导入，发现导入的包却没有问题，包使用的都是5.0.2的版本。

![img](./img/009.png) 

我们把上面2个包的顺序调换后就变成了低版本的依赖导入。

![img](./img/010.png) 

## 2.2. 路径近者优先原则

直接依赖比传递依赖路径近，你那么最终进入项目的jar包会是路径近的直接依赖包。

直接依赖：项目中直接导入的jar包就是项目的直接依赖包。

传递依赖（间接依赖）：项目中没有直接导入的jar包，可以通过中直接依赖包传递到项目中去。

修改jar包，直接引入依赖spring-core

```xml
<!--导入相关依赖包-->
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>4.2.4.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>

    <!--引入直接依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.2.8.RELEASE</version>
    </dependency>
</dependencies>
```

此时优先引入的是直接依赖的引用

![img](./img/011.png) 

## 2.3. 直接排除法【重点】 

javassist 它的依赖 动态字节码修改 

问题：整合项目需要用到5的版本，引入4的版本，会不会出现异常（类没有找到，方法没有找到）

解决方案：？

当我们需要排除某个jar包的依赖时，在配置exclusions标签的时候，内部可以不写版本号。pom.xml依赖如下：

```xml
<!--导入相关依赖包-->
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>4.2.4.RELEASE</version>
        <!--直接排除-->
        <exclusions>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>
</dependencies>
```

快捷操作：

![img](./img/012.png) 

依赖导入的jar包如下：

没有添加exclusion之前

![img](./img/013.png) 

添加exclusion之后，因为排除了4.2.4的版本spring-core的jar包

![img](./img/014.png) 

### 【小结】

真实项目中，出现1个项目存在多个同种jar包的时候，需要我们进行解决maven的jar包冲突问题（异常：Class not found， class not defined、Method not found, NoSuchField等）

1. 路径近者优先

2. 节点路径相同时，使用第一声明优先（xml上下顺序有关）

3. 无法通过依赖管理原则排除的，使用直接排除法

   ```xml
   <exclusions>
       <exclusion>
           <groupId>org.springframework</groupId>
           <artifactId>spring-core</artifactId>
       </exclusion>
   </exclusions>
   ```

   

# 3. 工程分层【重点】

回顾之前项目开发：

![img](./img/015.png) 

工程分层后的开发：所有的service和dao的代码都在一起，1：增强程序的通用性，2：降低了代码的耦合性

![img](./img/016.png) 

### 【目标】

实现项目工程分解，掌握聚合和继承的作用

### 【路径】

1：聚合和继承

2：准备数据库环境

3：ssm_parent（父工程）  pom   引入依赖

4：ssm_model（子工程） 实体类

5：ssm_dao（子工程）  数据库操作

6：ssm_service（子工程） 完业务操作

7：ssm_web（子工程）  收集请求参数，调用业务，返回结果给前端

8：测试（工程发布tomcat）

### 【讲解】

## 3.1. 聚合（多模块）和继承 【思想重点】

继承和聚合结构图：

## ![img](./img/017.png) 

特点1（继承）：

ssm_parent父工程：存放项目的所有jar包。

ssm_web和ssm_service和ssm_dao有选择的继承jar包，并在自己的工程中使用。这样可以消除jar包重复，并锁定版本

特点2（聚合）：

​        ssm_web依赖于ssm_service，ssm_service依赖于ssm_dao，我们启动ssm_web，便可以访问我们的程序。

​	执行安装的时候，执行ssm_parent，就可以将所有的子工程全部进行安装。

理解继承和聚合总结：

​	通常继承和聚合同时使用。

· 何为继承？

​	继承是为了消除重复，如果将 dao、 service、 web 分开创建独立的工程则每个工程的 pom.xml 文件中的内容存在重复，比如：设置编译版本、锁定 spring 的版本的等，可以将这些重复的 配置提取出来在父工程的 pom.xml 中定义。

· 何为聚合？

​	项目开发通常是分组分模块开发， 每个模块开发完成要运行整个工程需要将每个模块聚合在 一起运行，比如： dao、 service、 web 三个工程最终会打一个独立的 war 运行。

![img](./img/018.png) 

 ![image-20200906103747473](img/image-20200906103747473.png)

## 3.2. 准备数据库环境

准备工作，导入sql

创建数据库itcastmaven

执行items.sql

![img](./img/019.png) 

```sql
SET FOREIGN_KEY_CHECKS=0;
-- ----------------------------
-- Table structure for `items`
-- ----------------------------
DROP TABLE IF EXISTS `items`;
CREATE TABLE `items` (
  `id` int(10) NOT NULL auto_increment,
  `name` varchar(20) default NULL,
  `price` float(10,0) default NULL,
  `pic` varchar(40) default NULL,
  `createtime` datetime default NULL,
  `detail` varchar(200) default NULL,
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of items
-- ----------------------------
INSERT INTO `items` VALUES ('1', '传智播客web课程', '1000', null, '2018-03-13 09:29:30', '带我走上人生巅峰');
INSERT INTO `items` VALUES ('2', '黑马之路', null, null, '2018-03-28 10:05:52', '插入测试');
INSERT INTO `items` VALUES ('3', '黑马之路二', '199', null, '2018-03-07 10:08:04', '插入测试');
INSERT INTO `items` VALUES ('7', '插入测试', null, null, null, null);
INSERT INTO `items` VALUES ('8', '插入测试', null, null, null, null);
```

![img](./img/020.png) 

## 3.3. ssm_parent

![img](./img/021.png) 

删除src文件夹

![img](./img/022.png) 

### 3.3.1. 依赖==版本==管理pom.xml

 父工程：<packaging>pom</packaging>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>ssm_parent</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>pom</packaging>

    <!--
        特殊属性定义，一般是版本号
    -->
    <properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <mysql.version>5.1.6</mysql.version>
        <mybatis.version>3.4.5</mybatis.version>
        <aspectjweaver.version>1.6.8</aspectjweaver.version>
        <junit.version>4.12</junit.version>
        <jsp-api.version>2.0</jsp-api.version>
        <servlet-api.version>2.5</servlet-api.version>
        <jstl.version>1.2</jstl.version>
        <mybatis-spring.version>1.3.0</mybatis-spring.version>
        <druid.version>1.0.9</druid.version>
        <!--文件的编码格式-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>

    <!--
        【注意】dependencyManagement:并非导入依赖，而只是管理依赖的版本
    -->
    <dependencyManagement>
        <!--引入版本管理-->
        <dependencies>
            <!-- spring（切面） -->
            <dependency>
                <groupId>org.aspectj</groupId>
                <artifactId>aspectjweaver</artifactId>
                <version>${aspectjweaver.version}</version>
            </dependency>
			<!-- spring（aop） -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aop</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <!--spring包（核心）-->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <!--用于SpringMVC-->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <!--用于数据库源相关操作-->
            <!-- spring（整合jdbc） -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jdbc</artifactId>
                <version>${spring.version}</version>
            </dependency>
			<!-- spring（事务） -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-tx</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <!--Servlet相关API（可以使用Request、Response）-->
            <dependency>
                <groupId>javax.servlet</groupId>
                <artifactId>servlet-api</artifactId>
                <version>${servlet-api.version}</version>
                <scope>provided</scope>
            </dependency>

            <dependency>
                <groupId>javax.servlet.jsp</groupId>
                <artifactId>jsp-api</artifactId>
                <version>${jsp-api.version}</version>
                <scope>provided</scope>
            </dependency>

            <!--jstl标签-->
            <dependency>
                <groupId>jstl</groupId>
                <artifactId>jstl</artifactId>
                <version>${jstl.version}</version>
            </dependency>

            <!--MySQL数据库驱动-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
				<scope>runtime</scope>
            </dependency>

            <!--spring测试-->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <scope>test</scope>
            </dependency>


            <!-- log日志 start -->
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
                <version>${slf4j.version}</version>
            </dependency>
            <!-- log end -->

            <!--mybatis-->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
            </dependency>

            <!--MyBatis集成Spring-->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis-spring</artifactId>
                <version>${mybatis-spring.version}</version>
            </dependency>

            <!--数据源-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>


</project>
```

 【小结】

- <package>pom</pcakage>

* 管理所有的jar包（并锁定版本）
* 父工程不需要开发的代码，src的文件夹可以删除

## 3.4. ssm_model

【路径】

1：配置pom.xml

2：创建Items.java

选择Module

![img](./img/023.png) 

![img](./img/024.png) 

对应数据库的表和字段

![img](./img/025.png) 

### 3.4.1. pom.xml

查看子工程pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ssm_parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ssm_model</artifactId>
    <packaging>jar</packaging>

</project>
```

查看父工程pom.xml

表示聚合了ssm_model

```xml
<modules>
    <module>ssm_model</module>
</modules>
```

 

### 3.4.2. 创建Items.java

```java
package com.itheima.pojo;

import java.util.Date;

public class Items {

    private Integer id;
    private String name;
    private Float price;
    private String pic;
    private Date createtime;
    private String detail;
    //get..set..
}
```

【小结】

* <package>jar</pcakage>
* 管理项目中所有的实体类

## 3.5. ssm_dao

【路径】

1：配置pom.xml

2：创建spring-mybatis.xml（spring整合mybatis的配置）

3：创建ItemsDao.java

4：创建ItemsDao.xml

5：创建mybatis.xml（mybatis的配置文件）（可以省略）

![img](./img/026.png) 

### 3.5.1. 引入依赖pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ssm_parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ssm_dao</artifactId>

    <!--jar包-->
    <packaging>jar</packaging>

    <!--引入依赖-->
    <dependencies>
        <!--model的依赖-->
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>ssm_model</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
        </dependency>

        <!--MyBatis集成Spring-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
        </dependency>

        <!--数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>

        <!--MySQL数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!--SpringJdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
        </dependency>

        <!-- log start -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </dependency>
        <!-- log end -->
        
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
    </dependencies>
</project>
```

查看父工程pom.xml

```xml
<modules>
    <module>ssm_model</module>
    <module>ssm_dao</module>
</modules>
```

 

### 3.5.2. 创建spring-mybatis.xml

【路径】

1：数据库连接池

2：SqlSessionFactoryBean

3：Dao层接口扫描，让Dao被spring管理

在resources下创建spring-mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--1.数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/你的数据库?characterEncoding=utf8"/>
        <property name="username" value="root"/>
        <property name="password" value="root用户的密码"/>
    </bean>

    <!--2.配置sqlSessionFactoryBean-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--配置数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!--别名配置 【注意】包名的拼写是否正确-->
        <property name="typeAliasesPackage" value="com.itheima.pojo"/>
    </bean>
    <!--3.dao接口扫描-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--【注意】包名的拼写是否正确-->
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>
</beans>
```



### 3.5.3. 创建ItemsDao.java

```java
public interface ItemsDao {

    /***
     * 查询所有
     * @return
     */
    List<Items> findAll();

    /***
     * 保存操作
     * @param items
     * @return
     */
    int save(Items items);
}
```

 

### 3.5.4. 创建ItemsDao.xml

在com/itheima/dao/ItemsDao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    namespace="Dao接口的全限定名"
-->
<mapper namespace="com.itheima.dao.ItemsDao">

    <!--保存操作-->
    <insert id="save" parameterType="Items">
        INSERT  INTO items(name,price,pic,createtime,detail) VALUES(#{name},#{price},#{pic},#{createtime},#{detail})
    </insert>

    <!--查询所有 【注意】这里用的是resultType，不是resultMap -->
    <select id="findAll" resultType="Items">
        SELECT * FROM  items
    </select>

</mapper>
```

 

### 3.5.5. 创建sqlMapConfig.xml（可以省略）

在resources下创建mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
</configuration>
```

### 3.5.6 dao测试

在test下创建DaoTest

```java
public class DaoTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:spring-mybatis.xml");
        ItemsDao itemsDao = (ItemsDao)applicationContext.getBean("itemsDao");
        System.out.println("商品列表：：："+itemsDao.findAll());

        Items items = new Items();
        items.setName("商品名称");
        items.setPrice(16666f);
        items.setCreatetime(new Date());
        itemsDao.save(items);
    }
}
```

【小结】

* 创建子模块ssm_dao
* 引入依赖
* 创建dao接口类与映射文件
* 配置与spring与mybatis的整合配置文件
* 测试

## 3.6. ssm_service

【路径】

1：引入依赖pom.xml

2：创建spring-service.xml（spring的声明式事务处理）

3：创建ItemsService接口

4：创建ItemsServiceImpl实现类

![img](./img/027.png) 

### 3.6.1. pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ssm_parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ssm_service</artifactId>

    <!--jar包-->
    <packaging>jar</packaging>

    <!--依赖-->
    <dependencies>
        <!--依赖dao-->
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>ssm_dao</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!-- spring -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
        </dependency>
    </dependencies>
</project>
```

查看父工程pom.xml

```xml
<modules>
    <module>ssm_model</module>
    <module>ssm_dao</module>
    <module>ssm_service</module>
</modules>
```

 

### 3.6.2. 创建spring-service.xml

【路径】

1：创建一个事务管理器

2：配置事务的通知，及传播特性，对切入点方法的细化

3：AOP声明式事务配置（配置切入点，通知关联切入点）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">
    <!--1.创建一个事务管理器-->
    <bean id="txtManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--指定数据源-->
        <property name="dataSource" ref="dataSource" />
    </bean>
<!--【注意】【注意】【注意】
    配置文件引入的约束头tx必须为spring-tx
                    aop必须为spring-aop
                    context必须为spring-context
    建议直接复制约束头信息
-->
    <!--2.方式一：声明式事务配置-->
    <tx:advice id="txAdvice" transaction-manager="txtManager">
        <!--配置传播特性属性-->
        <tx:attributes>
            <!--
                对应方法参与事务并且在事务下执行，事务隔离剂别使用默认隔离级别,发生异常需要事务回滚
            -->
            <tx:method name="add*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <tx:method name="insert*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <tx:method name="save*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <tx:method name="delete*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <tx:method name="update*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <tx:method name="edit*" isolation="DEFAULT" propagation="REQUIRED" rollback-for="java.lang.Exception" />
            <!--
                只读操作
            -->
            <tx:method name="*" read-only="true" />
        </tx:attributes>
    </tx:advice>
     <!--AOP声明式事务配置（配置切入点，通知关联切入点）-->
    <aop:config>
        <!--切点指点 【注意】 实现类的包结构必须是在接口包下。接口com.itheima.service，则实现类的包必须在com.itheima.service下 【注意】注意包名拼写是否正确-->
        <aop:pointcut id="tranpointcut" expression="execution(* com.itheima.service..*.*(..))" />
        <!--配置通知-->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="tranpointcut" />
    </aop:config>
    <!--方式二：注解方式事务配置-->
    <!--<tx:annotation-driven transaction-manager="txtManager"/>-->
    <!--3.扫描service 【注意】注意包名拼写是否正确 -->
    <context:component-scan base-package="com.itheima.service"/>
    <!--4.引入spring-mybatis.xml 【注意】引入的配置文件拼写是否正确-->
    <import resource="classpath:spring-mybatis.xml" />
</beans>
```

 

### 3.6.3. 创建ItemsService接口

```java
public interface ItemsService {

    /***
     * 列表查询
     * @return
     */
    List<Items> findAll();

    /***
     * 增加商品
     * @param items
     * @return
     */
    int save(Items items);
}
```

 

### 3.6.4. 创建ItemsServiceImpl

```java
@Service
public class ItemsServiceImpl implements ItemsService {

    @Autowired
    private ItemsDao itemsDao;

    /***
     * 集合查询
     * @return
     */
    public List<Items> findAll() {
        return itemsDao.findAll();
    }

    /***
     * 增加商品测试事务
     * @param items
     * @return
     */
    public int save(Items items) {
        int acount = itemsDao.save(items);

        System.out.println("acount:"+acount);

        //测试事务，如果出错，是否回滚
        //int q=10/0;

        return acount;
    }
}
```

### 3.6.5. service测试

```java
public class ServiceTest {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-service.xml");
        ItemsService itemsService = (ItemsService)applicationContext.getBean("itemsServiceImpl");
       Items items = new Items();
       items.setName("测试事务");
       itemsService.save(items);
    }
}
```

 【小结】

* 创建子模块工程ssm_service
* 引入依赖
* 创建接口与实现类，实现类的包名必须包含接口的包名
* 配置spring-service.xml
  * 事务管理器
  * aop事务
  * 注解支持事务
  * 扫service的包
  * 导入dao的配置文件
* 测试

## 3.7. ssm_web

【路径】

1：pom.xml

2：创建web.xml

3：创建springmvc.xml

4：创建ItemsController.java

5：创建页面items.jsp

![img](./img/028.png) 

### 3.7.1. pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ssm_parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ssm_web</artifactId>

    <!--war包-->
    <packaging>war</packaging>

    <!--依赖引入-->
    <dependencies>
        <!--依赖service-->
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>ssm_service</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

	    <!--导入springmvc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
        </dependency>

        <!--servletAPI -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <scope>provided</scope>
        </dependency>
        
        <!--jstl表达式 -->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
        </dependency>
    </dependencies>

    <build>
        <!--插件-->
        <plugins>
            <!--tomcat插件-->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <!--插件使用的相关配置-->
                <configuration>
                    <!--端口号-->
                    <port>18081</port>
                    <!--写当前项目的名字(虚拟路径),如果写/，那么每次访问项目就不需要加项目名字了-->
                    <path>/</path>
                    <!--解决get请求乱码-->
                    <uriEncoding>UTF-8</uriEncoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

查看父工程pom.xml

```xml
<modules>
    <module>ssm_model</module>
    <module>ssm_dao</module>
    <module>ssm_service</module>
    <module>ssm_web</module>
</modules>
```

 

### 3.7.2.创建web.xml

【路径】

1:配置编码过滤器

2:springmvc前端核心控制器

![img](./img/029.png) 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <!--1.配置编码过滤器-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--编码设置-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--2.springmvc核心控制器-->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!--启动优先级 【注意】 不要漏了这个启动项-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--3.指定映射拦截-->
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

### 3.7.3. 创建springmvc.xml

【路径】

1：包扫描

2：视图解析器

3：springmvc注解驱动，自动配置mvc的处理器适配器和处理映射器

4：静态资源不过滤

5：import导入spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--
    【注意】context的约束为spring-context.xsd
          mvc的约束为spring-mvc.xsd
    建议直接复制约束头信息
    -->
    <!--1：包扫描-->
    <context:component-scan base-package="com.itheima.controller"/>
    <!--2：视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <!--3：springmvc注解驱动，自动配置mvc的处理器适配器和处理映射器-->
    <mvc:annotation-driven/>
    <!--4：静态资源不过滤-->
    <mvc:default-servlet-handler/>
    <!--5：导入spring.xml 【注意】 导入配置文件名的拼写是否正确-->
    <import resource="classpath:spring-service.xml"/>
</beans>
```

 

### 3.7.4. 创建ItemsController

```java
@Controller
@RequestMapping(value = "/items")
public class ItemsController {

    @Autowired
    private ItemsService itemsService;

    /**
     * 列表访问
     * @return
     */
    @RequestMapping(value = "/list")
    public String list(Model model){
        //集合查询
        List<Items> items = itemsService.findAll();
        //存入回显
        model.addAttribute("items",items);
        return "items";
    }

    /***
     * 事务测试
     * 增加商品
     * @return
     */
    @RequestMapping(value = "/save")
    public String save(Items items){
       int acount =  itemsService.save(items);
       //集合列表页跳转
       return "redirect:/items/list";
    }

}
```

 

### 3.7.5. 创建页面items.jsp

在/WEB-INF/pages/创建items.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>新增/查询</title>
</head>
<body>
<table>
    <form action="/items/save" method="post">
        <table>
            <tr>
                <td>名称</td>
                <td><input type="text" name="name"/></td>
            </tr>
            <tr>
                <td>价格</td>
                <td><input type="text" name="price"/></td>
            </tr>
            <tr>
                <td>图片</td>
                <td><input type="text" name="pic"/></td>
            </tr>
            <tr>
                <td>创建日期</td>
                <td><input type="text" name="createtime"/></td>
            </tr>
            <tr>
                <td>详情</td>
                <td><input type="text" name="detail"/></td>
            </tr>
            <tr>
                <td colspan="2">
                    <input type="submit" value="提交"/>
                </td>
            </tr>
        </table>
    </form>
</table>
<hr>
<table border="1">
    <tr>
        <td>ID</td>
        <td>name</td>
        <td>price</td>
        <td>pic</td>
        <td>createTime</td>
        <td>detail</td>
        <td></td>
    </tr>
    <c:forEach items="${items}" var="item">
        <tr>
            <td>${item.id}</td>
            <td>${item.name}</td>
            <td>${item.price}</td>
            <td>${item.pic}</td>
            <td>${item.createtime}</td>
            <td>${item.detail}</td>
        </tr>
    </c:forEach>
</table>

</body>
</html>
```

 【小结】

* 

## 3.8. 测试

【路径】

1：测试打包package、安装install（演示maven聚合）

2：发布到外部tomcat

3：使用maven的tomcat插件发布

### 3.8.1. 方式一：tomcat插件发布（配置父工程）

![img](img\035.png) 

使用http://localhost:18081/items/list测试：

![img](img\036.png) 



### 3.8.2. 方式二：tomcat插件发布（配置web工程）

在父工程打包

![img](./img/030.png) 

所有的子工程都会被打包（package）。这就是“聚合”的作用。

也可以同时安装（install），同时部署（deploy）

![img](./img/031.png) 

【小结】

使用maven内置的tomcat插件的时候 ：

第一种：配置D:\ideaProjects\ssm_parent：parent是有聚合的功能，不需要将ssm_parent，ssm_model，ssm_dao，ssm_service安装到本地仓库。

第二种：配置D:\ideaProjects\ssm_parent\ssm_web：需要将ssm_parent，ssm_model，ssm_dao，ssm_service安装到本地仓库。

### 3.8.3. 方式三：发布到外部tomcat

![image-20200906103139249](img/image-20200906103139249.png) 

【注意】：要选择xxx_web:war exploded的，不要选择错了

![image-20200906102744757](img/image-20200906102744757.png) 

使用http://localhost:8080/items/list测试：

![image-20200906103257798](img/image-20200906103257798.png) 



### 【小结】

1：拆分与聚合和继承 

​	拆分：解耦，代码最大程度的重复使用，维护方便

​	聚合：靠父工程来聚合，单一工程是没法完成工作。

​    继承：父工程引入依赖，子工程就不需要再引了, 父式程依赖的版本管理

2: Maven的jar包冲突解决

* 路径近者优先
* 路径相同，第一声明优先
* 路径相同，使用强制排除exclusions

3：准备数据库环境

* ssm_parent（父工程）聚合，引入依赖管理

* ssm_model（子工程） 实体类

* ssm_dao（子工程） mybatis, dataSource, sqlSessionFactorybean, MapperScannerConfigurer

  注：映射文件的路径必须与包名路径一样

* ssm_service（子工程）

  扫service包，事务管理器，通知、切面。

* ssm_web（子工程）
  * SpringMVC.xml 扫controller, 视图解析器，注解驱动，静态资源过滤
  * web.xml 编码过滤器，前端核心控制器(load-on-startup)

8：测试（工程发布tomcat）

* 推荐使用本地tomcat

# 4. 补充

**当下载jar的时候，如果断网，或者连接超时的时候，会自动在文件夹中创建一个名为*.lastupdate的文件，当有了这个文件之后，当你再次联网的时候，那么该文件不会再自动联网，必须手动删除，才能正常下载使用。如何删除呢？**

使用工具：

![img](./img/104.png) 

 

如何使用？打开工具

![img](./img/105.png) 

双击运行

![img](./img/106.png) 

 

Maven项目，如果正在下载一个jar，但是突然断网，此时，就会产生一个m2e-lastUpdated.，别指望下次上网就会自动下载，必须手动删除该文件，然后再进行下载。

