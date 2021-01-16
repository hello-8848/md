---
typora-copy-images-to: img
---

# day44-Spring

# 学习目标

- [ ] 能够掌握基于注解的AOP配置
- [ ] 能够使用spring的jdbc的模板
- [ ] 能够配置spring的连接池
- [ ] 能够使用jdbc模板完成增删改查的操作
- [ ] 能够应用声明式事务
- [ ] 能够理解事务的传播行为

# 第一章-Spring中的AOP

## 知识点-基于XML的AOP配置

### 1.目标

- [ ] 能够编写Spring的AOP中不同通知的代码

### 2.路径

1. 前置通知
2. 后置通知
3. 环绕通知
4. 异常通知
5. 最终通知

### 3.讲解

#### 3.0 AOP配置技术点

##### 3.0.1 基础配置

- 需要引入aop名称空间
- 使用`<aop:config>`标签进行aop配置
  - 配置切入点（实际被增强的方法）:
    - 切点配置使用子标签`<aop:pointcut>`
  - 配置切面（是一个类，是一个bean）：
    - 切面配置使用子标签`<aop:aspect>`切面中配置通知、切入点
    - 切面中`<aop:aspect>`也可以配置切入点`<aop:pointcut>`
    - 切面中配置通知：
      - 前置通知：`<aop:before>`
      - 后置通知：`<aop:after-returning>`， 方法正常执行才会执行这个通知
      - 最终通知：`<aop:after>`，方法不管是不是正常结束都会执行，类似于finally
      - 环绕通知：`<aop:around>`
      - 异常通知：`<aop:after-throwing>`

![1595645749052](img/1595645749052.png)

- 配置示例：

![1595652014479](img/1595652014479.png)

![1595652168912](img/1595652168912.png)

##### 3.0.2 额外配置（了解）

- 如果在`<aop:config>`中配置属性`proxy-target-class`为true可以强制使用cglib代理。不配置的话spring自动选择，接口则JDK，否则cglib。



##### 3.0.3 切入点的表达式

- 语法：?表示不写或者写都可以

>execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern)
>       throws-pattern?)
>
>-----------------
>
>execution(方法修饰符模式匹配     返回类型模式匹配==不能不写==    声明类型模式匹配   方法名模式==不能不写==(参数模式==不能不写==)     throws异常模式匹配)
>
>---------------------
>
>==*==  号可以匹配任意。
>
>- 返回类型可以用*
>- 方法名字可以用 *，或者方法名字可以写一部分另一部分用 *:   
>- 声明的类型可以用==.==号连接
>- ==()== 可以匹配无参方法
>- ==(..)==  可以匹配任意的数量的参数
>- ==(*)== 可以匹配一个参数，任意类型
>- ==(*,String)== 可以匹配匹配两个参数的方法，第一个参数任意类型，第二个参数是String类型
>
>示例表达式：
>
>- 匹配任意的public方法：
>
>   - execution(public * *(..))
>- 匹配任意的方法，方法名以set开头的：
>  - execution(* set*(..))
>
>- 匹配AccountService中的任一方法：
>- execution(* com.xyz.service.AccountService.*(..))
>- 匹配service包下面的所有的类的所有方法：
>  - execution(* com.xyz.service.*.*(..))
>
>- 匹配service包或者其子包的所有方法：
>- execution(* com.xyz.service..*.*(..))
>
>用的多的： execution(* com.itheima.service.impl.AccountServiceImpl.save(..))

- ==返回、方法名、参数必须要写==。

- 参考资料
  - https://www.eclipse.org/aspectj/doc/released/progguide/semantics-pointcuts.html
  - https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#aop-common-pointcuts

#### 准备工作

- AccountDao

```java
package com.itheima.dao;

public interface AccountDao {
    void save();
    void findAll();
    void delete();

    void findById();
    void update();
}

```

- AccountDaoImpl

```java
package com.itheima.dao.impl;

import com.itheima.dao.AccountDao;

public class AccountDaoImpl implements AccountDao {
    @Override
    public void save() {
        System.out.println("持久层-AccountDaoImpl-save()...");
    }

    @Override
    public void findAll() {
        System.out.println("持久层-AccountDaoImpl-findAll()...");
    }

    @Override
    public void delete() {
        System.out.println("持久层-AccountDaoImpl-delete()...");
    }

    @Override
    public void findById() {
        System.out.println("持久层-AccountDaoImpl-findById()...");
    }

    @Override
    public void update() {
        System.out.println("持久层-AccountDaoImpl-update()...");
    }
}

```





#### 3.1前置通知

需求: 在dao的save()方法调用之前进行权限的校验（打印一句话）

前置通知使用：<aop:before>

实现:

实现步骤: 

1. 导入`aspectjweaver`的依赖坐标
2. 引入aop名称空间
3. aop:config写配置
   1. 切入点
   2. 切面：通知



- 导入坐标

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

- 引入名称空间

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
  
      
  </beans>
  ```

- 编写切面类

  ```java
  package com.itheima.aspect;
  
  
  public class MyAspect {
  
      public void checkPrivilege(){
          System.out.println("MyAspect#checkPrivilege-权限校验");
      }
  }
  
  ```
  
- xml中注册bean

  ```xml
  <!--注册一个bean，将会被设置为切面bean-->
      <bean id="myAspect" class="com.itheima.aspect.MyAspect"/>
  
      <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>
  ```
  
- 完整的aop的配置

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
  
      <!--开启aop配置-->
      <aop:config>
          <!--配置切入点、配置切面-->
  
          <!--切入点配置：
              id是唯一标志，随便写
              expression是切入点表达式
              需求：在dao的save()方法调用之前进行权限的校验
                  简化：必须得写的： 返回类型  方法名  参数
          -->
          <aop:pointcut id="pointcut01" expression="execution(* com.itheima.dao.impl.AccountDaoImpl.save(..))"/>
  
          <!--配置通知，可以在切面里面配置-->
          <!--
              ref引用另外一个bean，作为切面bean（之后这个bean的方法就可以配置为通知了）
          -->
          <aop:aspect ref="myAspect">
              <!--
                  aop:before:配置前置通知，
                      method属性：指定切面的方法作为前置增强逻辑
                      pointcut-ref属性：对切入点匹配的方法进行增强
              -->
              <aop:before method="checkPrivilege" pointcut-ref="pointcut01"/>
          </aop:aspect>
  
  
  
      </aop:config>
  
      <!--注册一个bean，将会被设置为切面bean-->
      <bean id="myAspect" class="com.itheima.aspect.MyAspect"/>
  
      <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>
  </beans>
  ```

#### 3.2后置通知

需求: 在delete()方法调用之后, 打印日志删除

后置通知使用 *<aop:after-returning>*

实现：

切面中编写增强方法：

```java
public void showLog(){
        System.out.println("MyAspect#showLog-删除日志");
    }
```

配置切入点、通知：

```xml
<!--
            在delete()方法调用之后, 打印日志删除
        -->
        <aop:pointcut id="pointcut02" expression="execution(* com.itheima.dao.impl.AccountDaoImpl.delete(..))"/>



<!--
                aop:after-returning: 配置后置通知（方法正常执行完毕后会执行的逻辑）
                        如果被增强的方法里面存在异常并且该方法异常终止了则不会执行后置通知的逻辑
            -->
            <aop:after-returning method="showLog" pointcut-ref="pointcut02"/>
```



#### 3.3环绕通知

需求: 计算出dao里面的findAll()方法执行的时间.

环绕通知：<aop:around>

环绕通知可以获得一个对象 ==ProceedingJoinPoint==， 通过它可以得到目标方法。



切面中编写增强方法：

```java
/**
     * 环绕处理
     * @param proceedingJoinPoint
     */
    public void showTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        // 开始时间
        long time = System.currentTimeMillis();

        // 执行方法【放行方法】
        // 执行原有被增强的方法的逻辑
        Object proceed = proceedingJoinPoint.proceed();

        // 打印执行时间
        System.out.println("方法耗时:" + (System.currentTimeMillis() - time) + " ms");

    }
```

配置切入点、通知：

```xml
<!--
            计算出dao里面的findAll()方法执行的时间.
        -->
        <aop:pointcut id="pointcut03" expression="execution(* com.itheima.dao.impl.AccountDaoImpl.findAll())"/>



<!--
                aop:around：配置环绕通知（前置、后置）
            -->
            <aop:around method="showTime" pointcut-ref="pointcut03"/>

```





#### 3.4异常通知【了解】

- 方法出现异常执行. 如果方法没有异常,不会执行.
- 特点:可以获得异常的信息，在通知方法中加入参数Throwable获取异常信息
- <aop:after-throwing>

需求： 对update方法增加异常通知处理

切面中编写增强方法：

```java
// 异常信息
    public void showThrowable(Throwable throwable){
        System.out.println("MyAspect#showThrowable-异常: " + throwable.getMessage());
    }
```

配置切入点、通知：

```xml
<!--
            对update方法增加异常通知处理
        -->
        <aop:pointcut id="pointcut04" expression="execution(* com.itheima.dao.impl.AccountDaoImpl.update())"/>


<!--
                异常通知：出现异常终止才会执行增强的逻辑
                    throwing：代表指定方法的参数名
            -->
            <aop:after-throwing method="showThrowable" pointcut-ref="pointcut04" throwing="throwable"/>


```



#### 3.5最终通知【了解】

- 指的是无论是否有异常，总是被执行的，相当于finally。 
- <aop:after>

需求： 给findById增加最终通知



切面中编写增强方法：

```java
public void showFinally(){
        System.out.println("MyAspect#showFinally... ");
    }

```

配置切入点、通知：

```xml
<!--
            给findById增加最终通知
        -->
        <aop:pointcut id="pointcut05" expression="execution(* com.itheima.dao.impl.AccountDaoImpl.findById())"/>



<!--
                aop:after最终通知
            -->
            <aop:after method="showFinally" pointcut-ref="pointcut05"/>
```

- 测试汇总

```java
package com.itheima;

import com.itheima.dao.AccountDao;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest01 {

    @Autowired
    AccountDao accountDao;

    /**
     * 测试前置通知
     */
    @Test
    public void testBefore(){
        accountDao.save();
    }

    /**
     * 测试后置通知
     */
    @Test
    public void testAfterReturning(){
        accountDao.delete();

    }


    /**
     * 测试环绕通知
     */
    @Test
    public void testAround(){
        accountDao.findAll();

    }

    /**
     * 测试异常通知
     */
    @Test
    public void testAfterThrowing(){
        accountDao.update();

    }

    /**
     * 测试最终通知
     */
    @Test
    public void testAfter(){
        accountDao.findById();

    }
}

```





### 4.小结

1. XML配置AOP的步骤
   1. 引入相关依赖
   2. 在配置文件中引入aop名称空间
   3. 配置aop： <aop:config>
      1. 配置切入点
      2. 配置切面【切面类需要注册进spring容器里面】
         1. 配置通知





## 知识点-基于注解的AOP配置

### 1.目标

- [ ] 能够理解spring的AOP的注解

### 2.分析

#### 2.1路径

1. 前置通知
2. 后置通知
3. 环绕通知
4. 异常通知
5. 最终通知

#### 2.2 相关注解、技术点

和IOC的注解方式差不多，也需要在xml中开启AOP注解启动配置。

<!--开启AOP注解-->
    `<aop:aspectj-autoproxy/>`

- @Aspect： 声明类是一个切面类
- @Before()：前置通知
- @AfterReturning()：后置通知
- @Around()：环绕通知
- @AfterThrowing()：异常通知
- @After()：最终通知



### 3.讲解

+ 添加坐标

```xml
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.7</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
    </dependency>
  </dependencies>
```

- 配置文件中开启AOP注解+**注册切面类**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">


    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>
    <!--注册一个bean-->
    <bean id="myAspect" class="com.itheima.aspect.MyAspect"/>


    <!--开启AOP注解-->
    <aop:aspectj-autoproxy/>

</beans>
```

#### 注解式AOP练习

需求：

- 在dao的save()方法调用之前进行权限的校验（打印一句话）
- 在delete()方法调用之后, 打印日志是谁删除的
- 计算出dao里面的findAll()方法执行的时间.
- 对update方法增加异常通知处理
- 给findById增加最终通知

实现：

- ==确保开启了AOP注解式启动==

<!--开启aop注解-->
   ` <aop:aspectj-autoproxy/>`

- ==注册切面类==

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">


    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"/>
    <!--注册一个bean-->
    <bean id="myAspect" class="com.itheima.aspect.MyAspect"/>


    <!--开启AOP注解-->
    <aop:aspectj-autoproxy/>

</beans>
```



- ==编写一个类使用 **@Aspect**注解标记，编写方法使用通知的注解标记，注解的值就是切入点表达式==

```java
package com.itheima.aspect;


import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;

@Aspect   // 表示是一个切面类;  并不会把类注册为spring的bean；
public class MyAspect {

    // 通知的value属性值就是切入点
    // 前置
    @Before("execution(* com.itheima.dao.impl.AccountDaoImpl.save())")
    public void checkPrivilege(){
        System.out.println("MyAspect#checkPrivilege-权限校验");
    }

    // 后置
    @AfterReturning("execution(* com.itheima.dao.impl.AccountDaoImpl.delete())")
    public void showLog(){
        System.out.println("MyAspect#showLog-删除日志");
    }


    @Around("execution(* com.itheima.dao.impl.AccountDaoImpl.findAll()))")
    // 可以获得被增强的方法：proceedingJoinPoint： 代表切入点方法
    public void showTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        // 记录开始时间
        long startTime  = System.currentTimeMillis();

        // 表示执行切入点方法
        // 如果不写proceedingJoinPoint.proceed(),那么可以阻止原来的方法执行
        proceedingJoinPoint.proceed();

        // 打印耗时
        long endTime = System.currentTimeMillis() - startTime;
        System.out.println("耗时:" + endTime + " ms");

    }


    // 异常通知， throwing 写方法的变量（拿到异常信息）
    @AfterThrowing(value = "execution(* com.itheima.dao.impl.AccountDaoImpl.update()))",
            throwing = "throwable"
    )
    // 异常信息
    public void showThrowable(Throwable throwable){
        System.out.println("MyAspect#showThrowable-异常: " + throwable.getMessage());
    }

    // 最终通知
    @After("execution(* com.itheima.dao.impl.AccountDaoImpl.findById()))")
    public void showFinally(){
        System.out.println("MyAspect#showFinally... ");
    }


}

```



- 测试

```java
package com.itheima;

import com.itheima.dao.AccountDao;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest01 {

    @Autowired
    AccountDao accountDao;

    /**
     * 测试前置通知
     */
    @Test
    public void testBefore(){
        accountDao.save();
    }

    /**
     * 测试后置通知
     */
    @Test
    public void testAfterReturning(){
        accountDao.delete();

    }


    /**
     * 测试环绕通知
     */
    @Test
    public void testAround(){
        accountDao.findAll();

    }

    /**
     * 测试异常通知
     */
    @Test
    public void testAfterThrowing(){
        accountDao.update();

    }

    /**
     * 测试最终通知
     */
    @Test
    public void testAfter(){
        accountDao.findById();

    }
}

```



- 注解时AOP小结
  - 引入依赖坐标
  - 在xml中引入名称空间，开启aop注解配置： <aop:aspectj-autoproxy/>
  - 在一个类中使用@Aspect注解标记，把这个类作为切面类【==需要注册进spring容器==】
  - 在切面类中写方法（被一些通知注解标记--注解的属性写切入点表达式）
    - @Before： 前置
    - @AfterReturning：后置
    - @Around：环绕
    - @AfterThrowing：异常
    - @After：最终





## 知识点-纯注解的AOP配置【扩展】

- 编写配置类@Configuration，开启包扫描@ComponentScan，添加允许AOP的注解==@EnableAspectJAutoProxy==
- 编写切面类，添加==@Aspect、@Component注解==，编写AOP相关通知的注解（值写切入点表达式）

实现示例：

AopConfiguration配置类； 注意包所在位置。

```java
package com.itheima;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@ComponentScan
// 允许、开启AOP注解配置【等效于<aop:aspectj-autoproxy/>】
@EnableAspectJAutoProxy
public class AopConfiguration {
}

```

切面bean：

```java
package com.itheima.aspect;


import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Aspect   // 就等效于aop:aspect标签配置项
@Component  // 注册成eban
public class MyAnnoAspect {
    // 配置通知、切入点


    @Before("execution(* com.itheima.dao.impl.AccountDaoImpl.save())")
    public void checkPrivilege(){
        System.out.println("MyAspect#checkPrivilege-权限校验");
    }

    @AfterReturning("execution(* com.itheima.dao.impl.AccountDaoImpl.delete())")
    public void showLog(){
        System.out.println("MyAspect#showLog-删除日志");
    }


    @Around("execution(* com.itheima.dao.impl.AccountDaoImpl.findAll())")
    public void showTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {

        // 记录开始时间
        long startTime  = System.currentTimeMillis();

        // 执行原有方法的逻辑
        proceedingJoinPoint.proceed();

        // 打印耗时时间
        long endTime = System.currentTimeMillis() - startTime;
        System.out.println("耗时:" + endTime + " ms");
    }



    @AfterThrowing(value = "execution(* com.itheima.dao.impl.AccountDaoImpl.update())"
        , throwing = "throwable"
    )
    // 异常信息
    public void showThrowable(Throwable throwable){
        System.out.println("MyAspect#showThrowable-异常: " + throwable.getMessage());
    }

    @After("execution(* com.itheima.dao.impl.AccountDaoImpl.findById())")
    public void showFinally(){
        System.out.println("MyAspect#showFinally... ");
    }

}

```

业务bean：

```java
package com.itheima.dao.impl;

import com.itheima.dao.AccountDao;
import org.springframework.stereotype.Repository;
// 注册bean
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {
    @Override
    public void save() {
        System.out.println("持久层-AccountDaoImpl-save()...");
    }

    @Override
    public void findAll() {
        System.out.println("持久层-AccountDaoImpl-findAll()...");

    }

    @Override
    public void delete() {

        System.out.println("持久层-AccountDaoImpl-delete()...");
    }

    @Override
    public void findById() {
        System.out.println("持久层-AccountDaoImpl-findById()...");

    }

    @Override
    public void update() {
        System.out.println("持久层-AccountDaoImpl-update()...");
        int i = 1/0;
    }
}

```

测试：

```java
package com.itheima;

import com.itheima.dao.AccountDao;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {AopConfiguration.class})
public class AopTest02 {

    @Autowired
    AccountDao accountDao;

    /**
     * 测试前置通知
     */
    @Test
    public void testBefore(){
        accountDao.save();
    }

    /**
     * 测试后置通知
     */
    @Test
    public void testAfterReturning(){
        accountDao.delete();

    }


    /**
     * 测试环绕通知
     */
    @Test
    public void testAround(){
        accountDao.findAll();

    }

    /**
     * 测试异常通知
     */
    @Test
    public void testAfterThrowing(){
        accountDao.update();

    }

    /**
     * 测试最终通知
     */
    @Test
    public void testAfter(){
        accountDao.findById();

    }
}

```

- 小结：纯注解
  - 需要在IOC纯注解基础上进行配置（@Configuration、@ComponentScan）一个允许AOP注解配置的注解（==@EnableAspectJAutoProxy==）
  - 需要在切面类使用@Aspect+@Componet注册
  - 和注解开发一样的通知写法....

### 4.小结

1. AOP注解方式
   1. 引入依赖aspectjweaver包
   2. 开启AOP的注解配置： <aop:aspectj-autoproxy/>
   3. 需要一个切面bean（@Aspect，还需要注册到spring容器）
   4. 在切面里面进行编写通知（属性就是切入点）
      1. @Before
      2. @AfterReturning
      3. ....



# 第二章-Spring中的JdbcTemplate 【了解】

## 知识点-JdbcTemplate基本使用

### 1.目标

- [ ] 能够使用JdbcTemplate

### 2.路径

1. JdbcTemplate介绍
2. JdbcTemplate入门
3. 使用JdbcTemplate完成CRUD

### 3.讲解

#### 3.1JdbcTemplate介绍

​	Spring除了IOC 和 AOP之外，还对Dao层的提供了支持。 就算没有框架，也对原生的JDBC提供了支持。它是spring框架中提供的一个对象，是对原始Jdbc API对象的简单封装。spring框架为我们提供了很多的操作模板类。

|   持久化技术   |                   模版类                    |
| :-------: | :--------------------------------------: |
|   JDBC    | org.springframework.jdbc.core.JdbcTemplate |
| Hibrenate | org.springframework.orm.hibernate5.HibernateTemplate |
|  MyBatis  |  org.mybatis.spring.SqlSessionTemplate   |

==需要引入新的依赖：spring-jdbc、spring-tx 模块。==

==学JdbcTemplate，和学DBUtils类似，构造器、执行CRUD的API也大同小异。==



**使用：**

- 创建JDBC模版JdbcTemplate 对象

```java
// 或者直接使用含参构造器---DBUtils?差不多
JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
```



- 使用API操作进行CRUD

```java		
String sql = "insert into account values(?,?,?)";
jdbcTemplate.update(sql, null,account.getName(),account.getMoney())
```



- jdbcTemplate的一些核心API

| 方法                                       | 作用                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| public JdbcTemplate(DataSource dataSource) | 构造方法，传递数据源做为参数![1563848623186](img\1563848623186.png) |
| int update(String sql, Object…args)        | DML操作---增加、删除、修改使用的方法；args可以传入多个参数   |
| queryForMap()                              | 返回Map<String,Object>的查询结果，其中键是列名，值是表中对应的记录。 |
| ==queryForObject()==                       | ==查询单个javabean对象，需要使用**BeanPropertyRowMapper**==  |
| queryForList()                             | 返回多条记录的查询结果，封装成List<Map<String,Object>>       |
| ==query()==                                | 通用的查询方法，有多个同名方法的重载，可以自定义查询结果集封装成什么样的对象。==可以获得`List<javabean>`需要使用**BeanPropertyRowMapper**== |

==javabean与结果集的映射需要使用**BeanPropertyRowMapper**==，这个和DBUtils的ResultSetHandler类似。示例如下：

```java
String sql = "select * from account where id = ?";
User user = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<>(Account.class),id);
```



#### 3.2JDBC模版入门案例

+ 创建Maven工程,导入坐标

```xml
  <dependencies>
    <!--Spring核心容器-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--SpringJdbc-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--事务相关的-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
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

+ 表结构创建和JavaBean


```mysql
CREATE DATABASE day46;
USE day46;
CREATE TABLE account(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(40),
    money DOUBLE
)CHARACTER SET utf8 COLLATE utf8_general_ci;

INSERT INTO account(NAME,money) VALUES('zs',1000);
INSERT INTO account(NAME,money) VALUES('ls',1000);
INSERT INTO account(NAME,money) VALUES('ww',1000);
```

+ 实体

```java
public class Account implements Serializable{
    private Integer id;
    private String name;
    private Double money;
  ...
}
```

+ JdbcTemplate的API的基本使用

```java
package com.itheima;

import com.itheima.bean.Account;
import org.junit.Test;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import javax.sound.midi.Soundbank;
import java.util.List;
import java.util.Map;

/**
 * 了解JdbcTemplate的一些编程API
 */
public class A_Test {

    /**
     * 测试DML操作--以插入数据作为样例演示
     */
    @Test
    public void test01(){

        // 可以使用内置的一个简单的数据库连接池对象
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        //1. 创建JdbcTemplate对象
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

        //2. 使用API
        jdbcTemplate.update("insert into account values(null,?,?)", "zsf", 129.0 );
    }


    // 获得一个map格式的结果
    @Test
    public void test02(){
        // 可以使用内置的一个简单的数据库连接池对象
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        //1. 创建JdbcTemplate对象
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

        //2. 使用API
        // 结果集中的字段作为key， 值作为value
        Map<String, Object> map = jdbcTemplate.queryForMap("select * from account where id = ?", 1);
        System.out.println(map);
    }

    // 得到一个List<Map>
    @Test
    public void test03(){
        // 可以使用内置的一个简单的数据库连接池对象
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        //1. 创建JdbcTemplate对象
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

        //2. 使用API
        List<Map<String, Object>> list = jdbcTemplate.queryForList("select * from account");
        System.out.println(list);
    }


    // 得到一个javabean
    @Test
    public void test04(){
        // 可以使用内置的一个简单的数据库连接池对象
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        //1. 创建JdbcTemplate对象
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

        //2. 使用API
        Account account = jdbcTemplate.queryForObject("select * from account where id = ?",
                new BeanPropertyRowMapper<>(Account.class), 1);

        System.out.println(account);
    }

    // 获取List<javabean>
    @Test
    public void test05(){
        // 可以使用内置的一个简单的数据库连接池对象
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        //1. 创建JdbcTemplate对象
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

        //2. 使用API
        List<Account> query = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<>(Account.class));

        System.out.println(query);
    }
}

```

#### 3.3.使用JDBC模版完成CRUD

1. 增加

   ```java
   String sql = "insert into account values(?,?,?)";
   jdbcTemplate.update(sql, null,account.getName(),account.getMoney());
   ```

2. 修改

   ```java
   String sql ="update account set name = ? where id = ?";
   Object[] objects = {user.getName(), user.getId()};
   jdbcTemplate.update(sql,objects);
   ```

3. 删除

   ```java
   String sql = "delete from account where id=?";
   jdbcTemplate.update(sql, 6);
   ```

4. 查询单个值

   ```java
   String sql = "select count(*) from t_user";
   Long n = jdbcTemplate.queryForObject(sql, Long.class);
   ```

   ```java
   String sql = "select name from t_user where id=?";
   String name = jdbcTemplate.queryForObject(sql, String.class, 4);
   System.out.println(name);
   ```

5. 查询单个对象

   ```java
   String sql = "select * from account where id = ?";
   User user = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<>(Account.class),id);
   ```

6. 查询集合

   ```
   String sql = "select * from account";
   List<Map<String, Object>> list = jdbcTemplate.queryForList(sql);
   System.out.println(list);
   ```

   ```java
   String sql = "select * from account";
   List<Account> list = List<User> list = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(Account.class));
   ```

### 4.小结

1. 引入依赖spring-jdbc、spring-tx
2. 创建JdbcTemplate，传入连接池作为参数
3. 使用API



## 知识点-在dao中使用JdbcTemplate

### 1.目标

- [ ] 能够使用spring的jdbc的模板

### 2.路径

1. 方式一在dao中定义JdbcTemplate（就是把JdbcTemplate作为属性依赖注入）
2. 方式二dao继承JdbcDaoSupport

### 3.讲解

#### 3.1方式一

​	这种方式我们采取在dao中定义JdbcTemplate

步骤：

​	注册bean： dao、注入JdbcTemplate、连接池bean（XML、注解）

- AccountDao

```java
package com.itheima.dao;

public interface AccountDao {
    void save();
    void findAll();
    void delete();

    void findById();
    void update();
}

```



+ AccountDaoImpl.java


```java
package com.itheima.dao.impl;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void save() {
        jdbcTemplate.update("insert into account values(null, ?,?)", "zsf", 9999);

    }

    @Override
    public void findAll() {
        // 查询得到一个List<javabean>
        List<Account> query = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<>(Account.class));
        System.out.println(query);
    }

    @Override
    public void delete() {
        jdbcTemplate.update("delete from account where id = ?", 8);
    }

    @Override
    public void findById() {
        // 查询单个javabean： queryForObject
        Account account = jdbcTemplate.queryForObject("select * from account where id = ?",
                new BeanPropertyRowMapper<>(Account.class), 1);
        System.out.println(account);
    }

    @Override
    public void update() {
        jdbcTemplate.update("update account set money = ? where id =?", 9999, 1);
    }
}

```

+ applicationContext.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">


    <!--开启注解包扫描-->
    <context:component-scan base-package="com.itheima"/>


    <!--注册JdbcTemplate bean-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <constructor-arg name="dataSource" ref="dataSource"/>
    </bean>

    <!--注册一个dataSource：就是一个简单的数据库连接池实现-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <!--四件套-->
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day46"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    </bean>

</beans>
```

#### 3.2方式二（了解，不推荐）

​	这种方式我们采取让dao继承spring提供的一个的抽象类 ： JdbcDaoSupport。

![1595664903967](img/1595664903967.png)

+ AccountDaoImpl2.java

```java
package com.itheima.dao.impl;

import com.itheima.bean.Account;
import com.itheima.dao.AccountDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import org.springframework.stereotype.Repository;

import java.util.List;

// 可以继承spring提供的一个抽象类JdbcDaoSupport（仅仅做了解）
public class AccountDaoImpl2 extends JdbcDaoSupport implements AccountDao {


    @Override
    public void save() {
        getJdbcTemplate().update("insert into account values(null, ?,?)", "zsf", 9999);

    }

    @Override
    public void findAll() {
        // 查询得到一个List<javabean>
        List<Account> query = getJdbcTemplate().query("select * from account", new BeanPropertyRowMapper<>(Account.class));
        System.out.println(query);
    }

    @Override
    public void delete() {
        getJdbcTemplate().update("delete from account where id = ?", 8);
    }

    @Override
    public void findById() {
        // 查询单个javabean： queryForObject
        Account account = getJdbcTemplate().queryForObject("select * from account where id = ?",
                new BeanPropertyRowMapper<>(Account.class), 1);
        System.out.println(account);
    }

    @Override
    public void update() {
        getJdbcTemplate().update("update account set money = ? where id =?", 9999, 1);
    }
}

```

+ applicationContext2.xml ==配置注入从JdbcDaoSupport继承过来的属性dataSource。==

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">


    <!--开启注解包扫描-->
    <context:component-scan base-package="com.itheima"/>




    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day46"/>
    </bean>


    <bean id="accountDaoImpl2" class="com.itheima.dao.impl.AccountDaoImpl2">
        <!--属性注入调用你的set方法
            继承了JdbcDaoSupport之后，只需要依赖注入一个父类的属性dataSource即可，
            会在父类的setDataSource方法中初始化jdbcTemplate属性。
        -->
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```

### 4.小结

1. 方式一: 自己注册JDBCTemplate, 自己在Dao注入JDBCTemplate

   




# 第三章-Spring配置第三方连接池

## 知识点-Spring配置第三方连接池

### 1.目标

- [ ] 能够配置spring的连接池

### 2.路径

1. Spring配置C3P0连接池
2. Spring配置Druid连接池  
4. Spring配置HikariCP连接池【扩展】

### 3.讲解

#### 3.1Spring配置C3P0连接池

官网:http://www.mchange.com/projects/c3p0/	

步骤:

1. 导入c3p0的坐标
2. 注册连接池(修改DataSource的class以及四个基本项)



实现:

+ 导入坐标 


```xml
  <dependency>
    	<groupId>c3p0</groupId>
    	<artifactId>c3p0</artifactId>
    	<version>0.9.1.2</version>
  </dependency>
```

+ 配置数据源连接池

```xml
<bean id="c3p0Ds" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--四件套-->
        <property name="password" value="root"/>
        <property name="user" value="root"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/day46"/>
        <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    </bean>
```

> c3p0的连接池类：com.mchange.v2.c3p0.ComboPooledDataSource

#### 3.2.Spring配置Druid连接池

官网:http://druid.io/

步骤:

1. 导入Druid坐标
2. 修改DataSource的class(四个基本项)

实现:

- 导入坐标

  ```xml
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.0.9</version>
  </dependency>
  ```

- 配置数据源连接池


```xml
<!--druid连接池-->
    <bean id="druidDs" class="com.alibaba.druid.pool.DruidDataSource">
        <!--四件套-->
        <property name="password" value="root"/>
        <property name="username" value="root"/>
        <property name="url" value="jdbc:mysql://localhost:3306/day46"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    </bean>
```

> 连接池类：com.alibaba.druid.pool.DruidDataSource

#### 3.3.Spring配置HikariCP连接池【扩展】

官网:https://github.com/brettwooldridge/HikariCP

步骤:

1. 添加HikariCP坐标
2. 修改连接池为HikariDataSource

![1595665649164](img/1595665649164.png)

实现: 

+ 导入HikariCP坐标

  ```xml
  <dependency>
     <groupId>com.zaxxer</groupId>
     <artifactId>HikariCP</artifactId>
     <version>3.1.0</version>
  </dependency>
  ```

+ 配置数据源连接池

  ```xml
  <!--配置HikariCP-->
      <bean id="hikariDs" class="com.zaxxer.hikari.HikariDataSource">
          <!--四件套-->
          <property name="password" value="root"/>
          <property name="username" value="root"/>
          <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/day46"/>
          <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      </bean>
  ```
  
  >  连接池类：`com.zaxxer.hikari.HikariDataSource`


+ 为什么HikariCP为什么越来越火?   
  + 效率高
  + 一直在维护  
  + ==SpringBoot2.0 现在已经把HikariCP作为默认的连接池了==


+ 为什么HikariCP被号称为性能最好的Java数据库连接池?  

  + 优化并精简字节码: HikariCP利用了一个第三方的Java字节码修改类库Javassist来生成委托实现动态代理  

  + 优化代理和拦截器：减少代码，代码量越少。一般意味着运行效率越高、发生bug的可能性越低.

    

    

    ![1540557295765](img/1540557295765.png)

  

  

  
  
  

### 4.小结

1. 引入依赖
2. 进行注册bean：在文件里面注册



## 知识点-Spring引入Properties配置文件

### 1.目标

- [ ] 掌握Spring引入Properties配置文件

### 2.步骤

1. 定义jdbc.properties
2. 把jdbc.properties引入applicationContext.xml
3. 使用表达式根据key获得value

 

### 3.实现

#### 3.1jdbc.properties

```properties
jdbc.url=jdbc:mysql://localhost:3306/day46
jdbc.driver=com.mysql.jdbc.Driver
jdbc.username=root
jdbc.password=root
```

#### 3.2.在applicationContext.xml页面使用

* 引入配置文件方式一(不推荐使用)

```xml
 <!-- 引入properties配置文件：方式一 (繁琐不推荐使用) -->
<bean id="properties" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"> 
	<property name="location" value="classpath:jdbc.properties" />
</bean>
```

* 引入配置文件方式二【推荐使用】

和纯注解的 **@PropertySource**对应的，location写法和@PropertySource的属性值一样，需要添加==classpath:==

```xml
<!--引入外部的属性配置文件  location需要写classpath:-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

```

* bean标签中使用占位符表达式==${key}==引用配置文件内容
```xml
<!--引入外部的属性配置文件  location需要写classpath:-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <bean id="hikariDs" class="com.zaxxer.hikari.HikariDataSource">
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

```

> 注意:在使用`<context:property-placeholder/>`标签时，properties配置文件中的key如果存在分隔符，则需要使用==.==点分隔的。例如`jdbc.url`

### 4.小结

1. 引入外部属性配置文件的步骤
   1. 写一个属性文件
   2. 在xml中使用一个标签<context:property-placeholder>，属性location指定属性文件位置
   3. 在其他配置项中使用${key}来获取值



# 第四章-Spring管理事务   

## 知识点-Spring管理事务概述

### 1.目标

- [ ] 知道Spring管理事务相关的API

### 2.路径

1. 概述
2. 相关的API

### 3.讲解

#### 3.1概述

​	由于Spring对持久层的很多框架都有支持 ， Hibernate 、 jdbc 、JdbcTemplate , MyBatis 由于使用的框架不同，所以使用事务管理操作API 也不尽相同。 

​	==为了规范这些操作， Spring统一定义一个事务的规范 ，这其实是一个接口==。

​	==这个接口的名称 ： PlatformTransactionManager.并且它对已经做好的框架都有支持==.

​	==如果dao层使用的是**JDBC,  JdbcTemplate 或者mybatis**,那么 可以使用**DataSourceTransactionManager** 来处理事务==

​	如果dao层使用的是 Hibernate, 那么可以使用 HibernateTransactionManager 来处理事务

#### 3.2相关的API

##### 3.2.1PlatformTransactionManager

​	==PlatformTransactionManager==：平台事务管理器，是一个接口，实现类就是Spring真正管理事务的对象。

​	常用的实现类:

​		==**DataSourceTransactionManager**==    :连接池事务管理器。JDBC开发的时候(JDBCTemplate,MyBatis)，使用事务管理。

​		HibernateTransactionManager       :Hibernate开发的时候，使用事务管理。

##### 3.2.2 TransactionDefinition

​	==TransactionDefinition==：事务定义信息中定义了事务的隔离级别，传播行为，超时信息，只读。

##### 3.2.3 TransactionStatus:事务的状态信息

​	==TransactionStatus==：事务状态信息中定义事务是否是新事务，是否有保存点。

### 4.小结

1. Spring管理事务是通过事务管理器. 定义了一个接口PlatformTransactionManager 
   + ==DataSourceTransactionManager==              JDBC,MyBatis,JDBCTemplate
   + HibernateTransactionManager                 Hibernate



## 知识点-编程式(硬编码)事务【了解】

### 1.目标

- [ ] 了解Spring编程式事务

### 2.步骤

1. 创建连接池
2. 创建JDBCTemplate
3. 创建事务管理器DataSourceTransactionManager，管理连接（构造参数传入连接池）
4. 创建一个事务模板类TransacrtionTemplate，接管（构造参数传入事务管理器）
5. 调用TransacrtionTemplate的execute方法

 ![1595671745282](img/1595671745282.png)

### 3.实现

需求：张三给李四转账100块钱。

+ 添加依赖

```xml
  <dependencies>
    <!--Spring核心容器-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--SpringJdbc-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--事务相关的-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
    <!--SpringAOP相关的坐标-->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.7</version>
    </dependency>
  <!--数据库驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
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
	 <!--连接池-->
    <dependency>
      <groupId>com.zaxxer</groupId>
      <artifactId>HikariCP</artifactId>
      <version>3.1.0</version>
    </dependency>
  </dependencies>
```

+ 代码

```java
package com.itheima;

import com.zaxxer.hikari.HikariDataSource;
import org.junit.Test;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.TransactionStatus;
import org.springframework.transaction.support.TransactionCallbackWithoutResult;
import org.springframework.transaction.support.TransactionTemplate;

import javax.sql.DataSource;

public class A_Transform {

    /**
     * 测试转账
     */
    @Test
    public void test(){
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/day46");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");


        // 操作jdbc编程
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);


        // 1. 创建事务管理器（DataSourceTransactionManager）
        DataSourceTransactionManager dataSourceTransactionManager =
                new DataSourceTransactionManager(dataSource);


        // 2. 事务模板
        TransactionTemplate transactionTemplate = new TransactionTemplate(dataSourceTransactionManager);


        // 3. 在事务模板中操作事务逻辑
        transactionTemplate.execute(new TransactionCallbackWithoutResult() {
            @Override
            protected void doInTransactionWithoutResult(TransactionStatus status) {
                // 此处写的代码就在一个事务里面

                // 张三减少100
                jdbcTemplate.update("update account set money = money - ? where name= ?", 100.0, "zs");

                int i = 1/0;

                // 李四增加100
                jdbcTemplate.update("update account set money = money + ? where name= ?", 100.0, "ls");

            }
        });



    }
}

```



### 4.小结

1. 我们项目开发 一般不会使用编程式(硬编码)方式. 一般使用声明式(配置)事务
   + xml方式
   + 注解方式



## 知识点-Spring声明式事务-xml配置方式【重点】

​	声明式的事务管理的思想就是AOP的思想。

​	面向切面的方式完成事务的管理。

​	声明式事务有两种,==xml配置方式和注解方式.==

### 1.目标

- [ ] 使用xml方式配置事务

### 2.步骤

1. 引入 tx 名称空间
2. 注册事务管理器bean:DataSourceTransactionManager, 注入数据源连接池bean
3. **配置事务通知**(事务规则: 事务隔离级别, 遇到什么异常回滚,是否只读...)
   1. 使用<tx:advice>标签，属性`transaction-manager`去引用事务管理器bean
   1. 配置事务属性子标签 <tx:attributes>
   
4. **配置AOP**：**通知引用事务通知**。
   1. 配置切入点
   2. 配置事务通知 <aop:advisor>标签



### 3.讲解

#### 3.1实现

+ 配置连接池、事务管理器

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop
      http://www.springframework.org/schema/aop/spring-aop.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
  
  
      <!--开启注解包扫描-->
      <context:component-scan base-package="com.itheima"/>
  
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <constructor-arg name="dataSource" ref="hikariDs"/>
      </bean>
  
      <!--引入外部的属性配置文件  location需要写classpath:-->
      <context:property-placeholder location="classpath:jdbc.properties"/>
  
      <bean id="hikariDs" class="com.zaxxer.hikari.HikariDataSource">
          <property name="jdbcUrl" value="${jdbc.url}"/>
          <property name="driverClassName" value="${jdbc.driver}"/>
          <property name="username" value="${jdbc.username}"/>
          <property name="password" value="${jdbc.password}"/>
      </bean>
  
  
      
  
  
  </beans>
  ```
  
+ 配置事务通知(事务的规则)

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop
      http://www.springframework.org/schema/aop/spring-aop.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
  
  
  
      <!--
          配置事务规则：
          tx:advice 配置事务规则
                  id属性
                  transaction-manager属性，引用事务管理器bean
      -->
      <tx:advice id="transactionInterceptor" transaction-manager="txManager">
          <tx:attributes>
              <!--配置方法，哪些需要添加事务管理的方法的通配符-->
              <!--
                  * 代表任意
              -->
              <tx:method name="*"/>
              <!--
                  find开头的方法
                  tx:method相关属性：
                          name： 是配置方法的匹配模式，*代表任意
                          isolation： 事务隔离级别
                          read-only：该事务是否只读（不允许增删改操作）
                          timeout： 超时时间
                          rollback-for ： 遇到什么异常就回滚（配置业务异常去回滚）
                          no-rollback-for： 遇到该异常不会滚
                          propagation: spring自己定义的事务的传播行为
  
              -->
              <tx:method name="find*" isolation="REPEATABLE_READ"
                         read-only="true" timeout="-1" rollback-for="java.lang.Exception"
                         no-rollback-for="java.lang.IllegalAccessError"
                         propagation="REQUIRED"
              />
          </tx:attributes>
      </tx:advice>
  
  
      
  </beans>
  ```
  
+ 配置事务的AOP

  ```xml
  <!--配置AOP，是事务通知生效-->
      <aop:config>
          <!--这是匹配com.itheima.service.impl包下面的所有的类的所有方法-->
          <aop:pointcut id="pointcutTx" expression="execution(* com.itheima.service.impl.*.*(..))"/>
  
          <!--相当于前置、最终通知
              事务： 开启、提交、回滚
          -->
          <aop:advisor advice-ref="transactionInterceptor" pointcut-ref="pointcutTx"/>
      </aop:config>
  ```


#### 3.2事务的传播行为

##### 3.2.1事务的传播行为的作用

​	我们一般都是将事务设置在Service层,那么当我们调用Service层的一个方法的时候, 它能够保证我们的这个方法中,执行的所有的对数据库的更新操作保持在一个事务中， 在事务层里面调用的这些方法要么全部成功，要么全部失败。

​	如果你的Service层的这个方法中，除了调用了Dao层的方法之外， 还调用了本类的其他的Service方法，那么在调用其他的Service方法的时候， 必须保证两个service处在同一个事务中，确保事物的一致性。

​	 事务的传播特性就是解决这个问题的。

##### 3.2.2 事务的传播行为的取值(7个值)

保证在同一个事务里面:

- **PROPAGATION_REQUIRED:默认值，也是最常用的场景.**

  ==如果当前没有事务，就新建一个事务（针对的是方法里面调用另外的方法）==，
  如果已经存在一个事务中，加入到这个事务中。

  ==如果有则加入，没有则自力更生==

- PROPAGATION_SUPPORTS：

  如果当前没有事务，就以非事务方式执行。

  如果已经存在一个事务中，加入到这个事务中。

  有则加入没有则不管...

- PROPAGATION_MANDATORY

  如果当前没有事务，就抛出异常; 

  如果已经存在一个事务中，加入到这个事务中。
  
  必须得有事务。

保证不在同一个事务里:

- **PROPAGATION_REQUIRES_NEW**

  如果当前有事务，把当前事务挂起,创建新的事务但独自执行

  场景：就是另外一个方法可能与当前方法主体业务无关（单独去记录日志可以新起事务）

- PROPAGATION_NOT_SUPPORTED

  如果当前存在事务，就把当前事务挂起。不创建事务----》成了无事务控制了。

- PROPAGATION_NEVER

  以无事务方式进行；如果当前存在事务，抛出异常



![1602663864180](img/1602663864180.png)





### 4.小结

1. XML声明式事务小结
   1. 引入tx、aop名称空间
   2. 配置事务管理器
   3. 需要配置事务通知
   4. 需要配置AOP接管事务通知

## 知识点-spring声明式事务-注解方式【重点】 

### 1.目标

- [ ] 使用注解方式配置事务

### 2.步骤

1. 注册事务管理器

2. 引入tx 名称空间开启事务注解驱动

3. 在业务类(方法)上面添加@Transactional

   

### 3.实现

+ 在applicationContext里面打开注解驱动

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop
      http://www.springframework.org/schema/aop/spring-aop.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
  
  
      <context:component-scan base-package="com.itheima"/>
  
      <!--引入外部属性配置文件-->
      <context:property-placeholder location="classpath:jdbc.properties"/>
  
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
              <constructor-arg name="dataSource" ref="hikariDs"/>
      </bean>
  
      <!--
          事务管理器，管理连接：连接池
      -->
      <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="hikariDs"/>
      </bean>
  
      <!--配置光连接池-->
      <bean id="hikariDs" class="com.zaxxer.hikari.HikariDataSource">
          <property name="username" value="${jdbc.username}"/>
          <property name="password" value="${jdbc.password}"/>
          <property name="jdbcUrl" value="${jdbc.url}"/>
          <property name="driverClassName" value="${jdbc.driver}"/>
      </bean>
  
      <!--开启注解驱动配置-->
      <tx:annotation-driven transaction-manager="txManager"/>
  
  </beans>
  ```

+ 在业务逻辑类上面使用注解

  @Transactional ： 如果在类上声明，那么标记着该类中的所有方法都使用事务管理。也可以作用于方法上，	那么这就表示只有具体的方法才会使用事务。
  
  ```java
  /**
       * 需要事务控制的方法加注解即可
       * @param from
       * @param to
       * @param money
       * @throws Exception
       */
      @Transactional(rollbackFor = Exception.class, propagation = Propagation.REQUIRED)
      @Override
      public void transform(String from, String to, Double money) throws Exception {
          accountDao.reduce(from, money);
  
          int i = 1/0;
          accountDao.add(to, money);
      }
  ```
  
  

### 4.小结

#### 4.1步骤

1. 在配置文件中开启事务注解驱动：<tx:annotation-driven transaction-manager="txManager"/>
2. 在你的方法中增加注解@Transactional标记，就会纳入事务管理

#### 4.2两种方式比较

+ xml方式
  + 优点: 只需要配一次, 全局生效, 容易维护
  + 缺点: 相对注解而言 要麻烦一点
+ 注解方式
  + 优点: 配置简单
  + 缺点: 写一个业务类就配置一次, 事务细节不好处理, 不好维护

## 扩展_xml方式事务源码

1. 找入口

   `<tx:advice>`----》拦截器：TransactionInterceptor---》最终都是AOP处理。
   
   ![1561803136491](img/1561803136491.png) 



# 总结

- AOP的使用
  - XML
    - 引入依赖坐标
    - aop:config标签配置aop
      - 切入点： 切入点表达式（返回类型  方法名 参数）
      - 切面【保证这个类是一个bean】
        - 通知
          - 前置
          - 后置： after-turning
          - 环绕
          - 异常
          - 最终：after
  - 注解
    - 切面 @Aspect
    - 前置@Before("切入点表达式")
    - 后置@AfterReturning
    - 环绕@Around
    - 异常@AfterThrowing
    - 最终@After

- JdbcTemplate： 其实和你的DBUtils用法几乎一样
  - 使用步骤
    - 引入依赖
    - 创建对象newJdbcTemplate(连接池)
    - 使用API
- 配置第三方连接池
- 事务管理
  - 掌握声明式事务XML、注解
  - 事务XML
    - 引入依赖
    - 引入tx、aop名称空间
    - 配置事务管理器DataSourceTransactionManager
    - 配置事务规则： <tx:advice>
      - 事务属性配置：方法
    - 配置AOP： <aop:advisor>
  - 传播行为：
    - REQUIRED：如果有事务则加入，没有则新建（保证事务是同一个）
    - REQUIRES_NEW：如果有事务则挂起，自己新建一个事务

