# Zookeeper

### 教学目标

- 了解zookeeper
- 了解zookeeper的应用场景
- 了解zookeeper的基本概念和数据模型
- 能够搭建和配置zookeeper
- 熟练操作zookeeper服务端和客户端命令
- 能够使用java api 操作zookeeper
- 理解zookeeper watch机制
- 能搭建zookeeper集群

### 1、介绍zookeeper

### 【目标】

1：了解Zookeeper的概念

2：了解分布式的概念

### 【路径】

1：Zookeeper概述

2：Zookeeper的发展历程

3：什么是分布式

4：Zookeeper的应用场景

### 【讲解】

#### 1.1、zookeeper概述

​	ZooKeeper从字面意思理解，【Zoo - 动物园，Keeper - 管理员】动物园中有很多种动物，这里的动物就可以比作分布式环境下多种多样的服务，而ZooKeeper做的就是管理这些服务。
​	Apache ZooKeeper的系统为分布式协调是构建分布式应用的高性能服务。
​	ZooKeeper 本质上是一个分布式的小文件存储系统。提供基于类似于文件系统的目录树方式的数据存储，并且可以对树中的节点进行有效管理。从而用来维护和监控你存储的数据的状态变化。通过监控这些数据状态的变化，从而可以达到基于数据的集群管理。
​	ZooKeeper 适用于存储和协同相关的关键数据，不适合用于大数据量存储。

是一个分布式的小文件管理系统，管理分布式服务(Web Service)

#### 1.2、zookeeper的发展历程

​	ZooKeeper 最早起源于雅虎研究院的一个研究小组。当时研究人员发现，在雅虎内部很多大型系统基本都需要依赖一个系统来进行分布式协同，但是这些系统往往都存在分布式单点问题。
​	所以，雅虎的开发人员就开发了一个通用的无单点问题的分布式协调框架，这就是ZooKeeper。ZooKeeper之后在开源界被大量使用，很多著名开源项目都在使用zookeeper，例如：

- Hadoop：使用ZooKeeper 做Namenode 的高可用。
- HBase：保证集群中只有一个master，保存hbase:meta表的位置，保存集群中的RegionServer列表。
- Kafka：集群成员管理，controller 节点选举。

#### 1.3、什么是分布式

##### 1.3.1、集中式系统

```
集中式系统，集中式系统中整个项目就是一个独立的应用，整个应用也就是整个项目，所有的东西都在一个应用里面。部署到一个服务器上。
布署项目时，放到一个tomcat里的。也称为单体架构
```

##### 1.3.2、分布式系统

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是==**利用更多的机器，处理更多的数据**==

随着公司的发展，应用的客户变多，功能也日益完善，加了很多的功能，整个项目在一个tomcat上跑，tomcat说它也很累，能不能少跑点代码，这时候 就产生了。我们可以把大项目按功能划分为很多的模块，比如说单独一个系统处理订单，一个处理用户登录，一个处理后台等等，然后每一个模块都单独在一个tomcat中跑，合起来就是一个完整的大项目，这样每一个tomcat都非常轻松。

![image-20200906120941732](img/image-20200906120941732.png)		

分布式系统的描述总结是：
  - 多台计算机构成
  - 计算机之间通过网络进行通信 
  - 彼此进行交互 
  - 共同目标 有共同的功能

小结：

集中式：开发项目时都是在同一个应用里，布署到一个tomcat，只有一个tomcat即可

分布式：分多个工程开发，布署多个tomcat里， 单个拆成多个

#### 1.4、zookeeper的应用场景【面试 知道】

##### 	1.4.1、 注册中心

​	分布式应用中，通常需要有一套完整的命名规则，既能够产生唯一的名称又便于人识别和记住，通常情况下用树形的名称结构是一个理想的选择，树形的名称结构是一个有层次的目录结构。通过调用Zookeeper提供的创建节点的API，能够很容易创建一个全局唯一的path，这个path就可以作为一个名称。
​	阿里巴巴集团开源的分布式服务框架Dubbo中使用ZooKeeper来作为其命名服务，==维护全局的服务地址列表==。

##### 	1.4.2、配置中心

​	数据发布/订阅即所谓的配置中心：发布者将数据发布到ZooKeeper一系列节点上面，订阅者进行数据订阅，当数据有变化时，可以及时得到数据的变化通知，达到**动态获取数据**的目的。

![1573197592193](img/1573197592193.png) ZooKeeper 采用的是**推拉结合**的方式。

  1、推: 服务端会推给注册了监控节点的客户端 Wathcer 事件通知

  2、拉: 客户端获得通知后，然后主动到服务端拉取最新的数据

##### 	1.4.3、分布式锁（了解）

​	分布式锁是控制分布式系统之间同步访问共享资源的一种方式。在分布式系统中，常常需要协调他们的动作。如果不同的系统或是同一个系统的不同主机之间共享了一个或一组资源，那么访问这些资源的时候，往往需要互斥来防止彼此干扰来保证一致性，在这种情况下，便需要使用到分布式锁。

##### 1.4.4、分布式队列（了解）

​	在传统的单进程编程中，我们使用队列来存储一些数据结构，用来在多线程之间共享或传递数据。分布式环境下，我们同样需要一个类似单进程队列的组件，用来实现跨进程、跨主机、跨网络的数据共享和数据传递，这就是我们的分布式队列。

RabbitMQ

##### 1.4.5、负载均衡

​	负载均衡是通过负载均衡算法，用来把对某种资源的访问分摊给不同的设备，从而**减轻单点**的压力。

![1573198893298](img/1573198893298.png)	

上图中左侧为ZooKeeper集群，右侧上方为工作服务器，下面为客户端。每台工作服务器在启动时都会去ZooKeeper的servers节点下注册临时节点，每台客户端在启动时都会去servers节点下取得所有可用的工作服务器列表，并通过一定的负载均衡算法计算得出一台工作服务器，并与之建立网络连接

### 【小结】

Zookeeper: 分布式小文件存储系统，管理的是分布式中服务，树形层级目录结构

作用：

* 注册中心 房产中介 协调服务提供者与消费者 正常的调用

* 配置中心 分布式的所有应该都来中心读取配置,订阅，中心一旦修改，就会通知服务，服务就会去获取新的配置，所有应该都更新配置

* 分布式锁：多个应用在同一时刻只能有一个应用能访问资源。

  zookeeper: 一旦获得资源，标记已使用（ 创建节点数据），被标记了则订阅。释放（删除数据），订阅者就会收到通知，竞争资源

* 分布式队列：使用zookeeper有序节点数据，惊群-监听上一个

* 负载均衡: 多个应用被调用的次数相对平均
  * 随机
  * 轮循
  * 最小活跃数
  * 一致性哈希

### 2、zookeeper环境搭建【重点】

### 【目标】

1：在window上安装Zookeeper

### 【路径】

1：安装Zookeeper的前提

2：在window上安装Zookeeper

2：Zookeeper的发展历程

3：什么是分布式

4：Zookeeper的应用场景

### 【讲解】

#### 2.1、 前提

​	必须安装==**jdk 1.8**==，配置jdk环境变量，**步骤略**

#### 2.2、安装zookeeper

##### 2.2.1、下载

下载地址：http://zookeeper.apache.org

![1572748541760](img/1572748541760.png)

查看zookeeper的更新历史

![1572748661456](img/1572748661456.png)	

zookeeper下载页的地址

![1572748713160](img/1572748713160.png)

zookeeper选择下载的版本

![1572749742022](img/1572749742022.png)

##### 2.2.2、解压

【注意】==解压到没有中文路径的目录下（不要出现中文和空格）==

![1572830233256](img/1572830233256.png)

##### 2.2.3、修改配置文件

**在zookeeper路径下创建一个data目录**

![1572830285075](img/1572830285075.png)		

**修改配置文件**

conf路径中复制一份zoo_sample.cfg,改名为 zoo.cfg

![1572830357546](img/1572830357546.png)	

指定保存数据的目录：data目录和log存储日志

 ![image-20200616110359375](./img/image-20200616110359375.png)	

如果需要日志，可以创建log文件夹，指定dataLogDir属性

##### 2.2.4、启动zookeeper

打开bin路径，双击zkServer.cmd,启动zookeeper服务

![1572830611836](img/1572830611836.png)	

##### 2.2.5、启动客户端测试

启动客户端，看到 ‘Welcome to Zookeeper!’ 说明成功

![1572830681143](img/1572830681143.png)

### 【小结】

1：安装Zookeeper的前提 环境变量 jdk1.8

2：在window上安装Zookeeper 解压到没有中文没有空格的目录

​      创建data目录与bin同级

​      复制conf/zoo_simple.cfg，改名为zoo.cfg(启动时加载)

​      zoo.cfg -> dataDir=../data

​     log日志，创建log目录与bin同级

​    zoo.cfg -> dataLogDir=../log

3: 【注意】：服务端 窗口不要选中任何区域（阻塞请求，客户端连不上），如果选中了则按 ESC 键

右击窗口的属性，去除编辑与插入模式钩选

![image-20200616102146913](./img/image-20200616102146913.png)

### 3、zookeeper基本操作【次重点，存小抄】

### 【目标】

1：Zookeeper的客户端命令

2：Zookeeper的java的api操作

### 【路径】

1：Zookeeper的数据结构

2：节点的分类

* 持久性
* 临时性

3：客户端命令（创建、查询、修改、删除）

4：Zookeeper的java的api介绍（创建、查询、修改、删除）

5：Zookeeper的watch机制

* NodeCache
* PathChildrenCache
* TreeCache

### 【讲解】

#### 3.1、zookeeper数据结构

​		ZooKeeper 的数据模型是层次模型。层次模型常见于文件系统。例如：我的电脑可以分为多个盘符（例如C、D、E等），每个盘符下可以创建多个目录，每个目录下面可以创建文件，也可以创建子目录，最终构成了一个树型结构。通过这种树型结构的目录，我们可以将文件分门别类的进行存放，方便我们后期查找。而且磁盘上的每个文件都有一个唯一的访问路径，例如：C:\Windows\itcast\hello.txt。

![1572746945875](img/1572746945875.png)		层次模型和key-value 模型是两种主流的数据模型。ZooKeeper 使用文件系统模型主要基于以下两点考虑：

1. 文件系统的树形结构便于表达数据之间的层次关系。

2. 文件系统的树形结构便于为不同的应用分配独立的命名空间（namespace 路径url）。



​	ZooKeeper 的层次模型称作data tree。Datatree 的每个节点叫作znode（Zookeeper node）。不同于文件系统，每个节点都可以保存数据。每个节点都有一个版本(version)。版本从0 开始计数。

![1572831517484](img/1572831517484.png)	

如图所示，data tree中有两个子树，用于应用1( /app1)和应用2（/app2）。

每个客户端进程pi 创建一个znode节点 p_i 在 /app1下， /app1/p_1就代表一个客户端在运行。

#### 3.2、节点的分类【重点】

一：一个znode可以是持久性的，也可以是临时性的

1. 持久性znode[PERSISTENT]，这个znode一旦创建不会丢失，无论是zookeeper宕机，还是client宕机。

2. 临时性的znode[EPHEMERAL]，如果zookeeper宕机了，或者client在指定的timeout时间内没有连接server，都会被认为丢失。 -e


二：znode也可以是顺序性的，每一个顺序性的znode关联一个唯一的单调递增整数。这个单调递增整数是znode名字的后缀。

3. 持久顺序性的znode(PERSISTENT_SEQUENTIAL):znode 处理具备持久性的znode的特点之外，znode的名称具备顺序性。 -s

4. 临时顺序性的znode(EPHEMERAL_SEQUENTIAL):znode处理具备临时性的znode特点，znode的名称具备顺序性。-s

#### 3.2、客户端命令

##### 3.2.1、查询所有命令

```bash
help
```

![1572833091672](img/1572833091672.png)

##### 3.2.2、查询跟路径下的节点

```bash
ls /zookeeper
```

查看zookeeper节点

![1572833196371](img/1572833196371.png)

##### 3.2.3、创建普通永久节点

```bash
create /app1 "helloworld"
```

创建app1节点，值为helloworld

![1572833278911](img/1572833278911.png)

##### 3.2.4、创建带序号永久节点

```bash
create -s /hello "helloworld"
```

![1572834218189](img/1572834218189.png)

##### 3.2.5、创建普通临时节点

```bash
create -e /app3 'app3'
```

-e:表示普通临时节点

![1572834321407](img/1572834321407.png)

**关闭客户端，再次打开查看 app3节点消失**

##### 3.2.6、创建带序号临时节点

```bash
create -e -s /app4 'app4'
```

-e:表示普通临时节点

-s:表示带序号节点

![1572836701139](img/1572836701139.png)	**关闭客户端，再次打开查看 app4节点消失**	

##### 3.2.7、查询节点数据

```bash
get /app1
```

![1572837605335](img/1572837605335.png)		

```shell
# ­­­­­­­­­­­节点的状态信息，也称为stat结构体­­­­­­­­­­­­­­­­­­­
# 创建该znode的事务的zxid(ZooKeeper Transaction ID)
# 事务ID是ZooKeeper为每次更新操作/事务操作分配一个全局唯一的id，表示zxid，值越小，表示越先执行
cZxid = 0x4454 # 0x0表示十六进制数0
ctime = Thu Jan 01 08:00:00 CST 1970  # 创建时间
mZxid = 0x4454 						  # 最后一次更新的zxid
mtime = Thu Jan 01 08:00:00 CST 1970  # 最后一次更新的时间
pZxid = 0x4454 						  # 最后更新的子节点的zxid
cversion = 5 						  # 子节点的变化号，表示子节点被修改的次数
dataVersion = 0 					  # 表示当前节点的数据变化号，0表示当前节点从未被修改过
aclVersion = 0  					  # 访问控制列表的变化号 access control list
# 如果临时节点，表示当前节点的拥有者的sessionId
ephemeralOwner = 0x0				  # 如果不是临时节点，则值为0
dataLength = 13 					  # 数据长度
numChildren = 1 					  # 子节点的数量
```

##### 3.2.8、修改节点数据

```bash
set /app1 'hello'
```

![1572837791218](img/1572837791218.png)	

##### 3.2.9、删除节点

```bash
delete /hello0000000006
```

![1572838085864](img/1572838085864.png)

##### 3.2.10、递归删除节点

```bash
delete /hello

rmr /hello
```

![1572838221537](img/1572838221537.png)

##### 3.2.11、查看节点状态

```bash
stat /zookeeper
```

![1572838262570](img/1572838262570.png)

##### 3.4.12、日志的可视化

- 这是日志的存储路径

  ![1573200254799](img/1573200254799.png)

- 日志都是以二进制文件存储的，使用记事本打开，无意义。

  ![1573200289376](img/1573200289376.png) 

- 为了能正常查看日志，把查看日志需要的jar包放到统一路径下

   ![image-20200616110916612](C:/Users/Eric/AppData/Roaming/Typora/typora-user-images/image-20200616110916612.png)

   ![image-20200616110938746](C:/Users/Eric/AppData/Roaming/Typora/typora-user-images/image-20200616110938746.png)

![1573200519812](img/1573200519812.png)

- 在当前目录下进入cmd，执行以下命令可以直接查看正常日志

```java
java -classpath ".;*" org.apache.zookeeper.server.LogFormatter log.1
```

![1573200368970](img/1573200368970.png)

#### 小结

【注意】路径必须以/打头

ls 查看

help查看所有命令

create 路径 数据    -s 代表有序   -e 代临时

get 路径 查询

set 路径  新的数据

delete 路径 单一路径，没有子节点

rmr 路径 递归删除

stat 路径 查看节点状态 没有数据显示

日志 配置日志存储路径，依赖2个zookeeper-xxx.jar, sfl4-api.jar 

​    java -classpath ".;*" org.apache.zookeeper.server.LogFormatter log.1  4

#### 3.3、zookeeper  的java  Api介绍

##### 3.3.1、ZooKeeper常用Java API

- **原生Java API**（不推荐使用）

​       ZooKeeper 原生Java API位于org.apache.ZooKeeper包中

​       ZooKeeper-3.x.x. Jar （这里有多个版本）为官方提供的 java API

- **Apache Curator（推荐使用）**

​       Apache Curator是 Apache ZooKeeper的Java客户端库。 

​       Curator.项目的目标是简化ZooKeeper客户端的使用。

​      另外 Curator为常见的分布式协同服务提供了高质量的实现。
​      Apache Curator最初是Netflix研发的,后来捐献了 Apache基金会,目前是 Apache的顶级项目

- **ZkClient（不推荐使用）**

​       Github上一个开源的ZooKeeper客户端，由datameer的工程师Stefan Groschupf和Peter Voss一起开发。        zkclient-x.x.Jar也是在源生 api 基础之上进行扩展的开源 JAVA 客户端。

##### 3.3.2、创建java 工程，导入依赖 

![1572839451720](img/1572839451720.png)	

![1572839476251](img/1572839476251.png)	

![1572839494820](img/1572839494820.png)		

导入依赖

```xml
<!--zookeeper的依赖-->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.7</version>
</dependency>
<!--	zookeeper CuratorFramework 是Netflix公司开发一款连接zookeeper服务的框架，通过封装的一套高级API 简化了ZooKeeper的操作，提供了比较全面的功能，除了基础的节点的操作，节点的监听，还有集群的连接以及重试。-->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.0.1</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.0.1</version>
</dependency>
<!--测试-->
<dependency>
  	<groupId>junit</groupId>
  	<artifactId>junit</artifactId>
  	<version>4.12</version>
</dependency>
```

##### 3.3.3、创建节点

```java
//1. 创建一个空节点(a)（只能创建一层节点）
//2. 创建一个有内容的b节点（只能创建一层节点）
//3. 创建持久节点，同时创建多层节点
//4. 创建带有的序号的持久节点
//5. 创建临时节点（客户端关闭，节点消失），设置延时5秒关闭（Thread.sleep(5000)）
//6. 创建临时带序号节点（客户端关闭，节点消失），设置延时5秒关闭（Thread.sleep(5000)）
```



```java
/**
 *  RetryPolicy： 失败的重试策略的公共接口
 *  ExponentialBackoffRetry是 公共接口的其中一个实现类
 *      参数1： 初始化sleep的时间，用于计算之后的每次重试的sleep时间
 *      参数2：最大重试次数
        参数3（可以省略）：最大sleep时间，如果上述的当前sleep计算出来比这个大，那么sleep用这个时间
 */
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,3,10);
//创建客户端
/**
 * 参数1：连接的ip地址和端口号
 * 参数2：会话超时时间，单位毫秒
 * 参数3：连接超时时间，单位毫秒
 * 参数4：失败重试策略
 */
CuratorFramework client =  CuratorFrameworkFactory.newClient("127.0.0.1:2181",3000,1000,retryPolicy);
//开启客户端(会阻塞到会话连接成功为止)
client.start();
/**
 * 创建节点
 */
//1. 创建一个空节点(a)（只能创建一层节点）
// client.create().forPath("/a");
//2. 创建一个有内容的b节点（只能创建一层节点）
// client.create().forPath("/b", "这是b节点的内容".getBytes());
//3. 创建多层节点
// （creatingParentsIfNeeded）是否需要递归创建节点
// withMode(CreateMode.PERSISTENT)  创建持久性 b节点
// client.create().creatingParentsIfNeeded().withMode(CreateMode.PERSISTENT).forPath("/g");
//4. 创建带有的序号的节点
//  client.create().creatingParentsIfNeeded().withMode(CreateMode.PERSISTENT_SEQUENTIAL).forPath("/e");
//5. 创建临时节点（客户端关闭，节点消失），设置延时5秒关闭
// client.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL).forPath("/f");
//6. 创建临时带序号节点（客户端关闭，节点消失），设置延时5秒关闭
client.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL_SEQUENTIAL).forPath("/f");
Thread.sleep(5000);
//关闭客户端
client.close();
```

##### 3.3.4、修改节点数据

```java
//创建失败策略对象
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
//
CuratorFramework client = CuratorFrameworkFactory.newClient("127.0.0.1:2181",1000,1000,retryPolicy);
client.start();
//修改节点
client.setData().forPath("/a/b", "abc".getBytes());
client.close();
```

##### 3.3.5、节点数据查询

```java
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 1);
CuratorFramework client = 
        CuratorFrameworkFactory.newClient("127.0.0.1",1000,1000, retryPolicy);
client.start();
// 查询节点数据
byte[] bytes = client.getData().forPath("/a/b");
System.out.println(new String(bytes));
```

##### 3.3.6、删除节点

```java
//重试策略
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
//创建客户端
CuratorFramework client =
        CuratorFrameworkFactory.newClient("127.0.0.1",1000,1000, retryPolicy);
//启动客户端
client.start();
//删除一个子节点
  client.delete().forPath("/a");
// 删除节点并递归删除其子节点
  client.delete().deletingChildrenIfNeeded().forPath("/a");
//强制保证删除一个节点
//只要客户端会话有效，那么Curator会在后台持续进行删除操作，直到节点删除成功。
// 比如遇到一些网络异常的情况，此guaranteed的强制删除就会很有效果。
client.delete().guaranteed().deletingChildrenIfNeeded().forPath("/a");
//关闭客户端
client.close();
```

【小结】

1. Curator是Appache封装操作Zookeeper的客户端, 操作zookeer数据变得更简单

2. 使用步骤：

   * 创建重试策略 

   * 创建客户端  ip:port  sessionTimeout, connectionTimeout, retryPolicy

   * 启动客户端 start

   * 使用客户端对节点操作

     -- create forPath, creatingparent........, withMode(CreateMode.持久，临时，有序)

     -- setData 修改数据

     -- getData 查询数据

     -- delete 删除数据， deletingChildrenIfNeeded递归删除

   * 关闭客户端, 测试临时数据时要睡眠一下

#### 3.4、watch机制【机制重点】

回顾：Zookeeper的应用场景中配置中心，其中看到watch机制

![1573197592193](img/1573197592193.png)

##### 3.4.1、什么是watch机制

​	zookeeper作为一款成熟的分布式协调框架，订阅-发布功能是很重要的一个。所谓订阅发布功能，其实说白了就是观察者模式。观察者会订阅一些感兴趣的主题，然后这些主题一旦变化了，就会自动通知到这些观察者。

​	zookeeper的订阅发布也就是watch机制，是一个轻量级的设计。因为它采用了一种==推(服务端通知应用)拉（客户端获取服务端数据）==结合的模式。一旦服务端感知主题变了，那么只会发送一个事件类型和节点信息给关注的客户端，而不会包括具体的变更内容，所以事件本身是轻量级的，这就是所谓的“推”部分。然后，收到变更通知的客户端需要自己去拉变更的数据，这就是“拉”部分。watche机制分为添加数据和监听节点。

​	Curator在这方面做了优化，Curator引入了Cache的概念用来实现对ZooKeeper服务器端进行事件监听。Cache是Curator对事件监听的包装，其对事件的监听可以近似看做是一个本地缓存视图和远程ZooKeeper视图的对比过程。而且Curator会自动的再次监听，我们就不需要自己手动的重复监听了。

Curator中的cache共有三种

- NodeCache（监听和缓存根节点变化） 只监听单一个节点(变化 添加，修改，删除)
- PathChildrenCache（监听和缓存子节点变化） 监听这个节点下的所有子节点(变化 添加，修改，删除)
- TreeCache（监听和缓存根节点变化和子节点变化） NodeCache+ PathChildrenCache 监听当前节点及其下的所有子节点的变化

下面我们分别对三种cache详解

##### 3.4.2、 NodeCache

- **介绍**

  NodeCache是用来监听节点的数据变化的，当监听的节点的数据发生变化的时候就会回调对应的函数。

- ##### 增加监听

```java
//创建重试策略
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
//创建客户端
CuratorFramework client = CuratorFrameworkFactory.newClient("127.0.0.1:2181", 1000, 1000, retryPolicy);
//开启客户端
client.start();
System.out.println("连接成功");
//创建节点数据监听对象
final NodeCache nodeCache = new NodeCache(client, "/hello");
//开始缓存
/**
 * 参数为true：可以直接获取监听的节点，System.out.println(nodeCache.getCurrentData());为ChildData{path='/aa', stat=607,765,1580205779732,1580973376268,2,1,0,0,5,1,608
, data=[97, 98, 99, 100, 101]}
 * 参数为false：不可以获取监听的节点，System.out.println(nodeCache.getCurrentData());为null
 */
nodeCache.start(true);
System.out.println(nodeCache.getCurrentData());
//添加监听对象
nodeCache.getListenable().addListener(new NodeCacheListener() {
    //如果节点数据有变化，会回调该方法
    public void nodeChanged() throws Exception {
        String data = new String(nodeCache.getCurrentData().getData());
        System.out.println("数据Watcher：路径=" + nodeCache.getCurrentData().getPath()
                + ":data=" + data);
    }
});
System.in.read();
```

- **测试**

修改节点的数据

![1573266836331](img/1573266836331.png)	

控制台显示

![1573266903227](img/1573266903227.png)	

##### 3.4.3、 PathChildrenCache

- **介绍**

   PathChildrenCache是用来监听指定节点 的子节点变化情况

- **增加监听**

```java
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
CuratorFramework client = CuratorFrameworkFactory.newClient("127.0.0.1:2181", 1000, 1000, retryPolicy);
client.start();
//监听指定节点的子节点变化情况包括͹新增子节点 子节点数据变更 和子节点删除
//true表示用于配置是否把节点内容缓存起来，如果配置为true，客户端在接收到节点列表变更的同时，也能够获取到节点的数据内容（即：event.getData().getData()）ͺ如果为false 则无法取到数据内容（即：event.getData().getData()）
PathChildrenCache childrenCache = new PathChildrenCache(client,"/hello",true);
/**
 * NORMAL:  普通启动方式, 在启动时缓存子节点数据
 * POST_INITIALIZED_EVENT：在启动时缓存子节点数据，提示初始化
 * BUILD_INITIAL_CACHE: 在启动时什么都不会输出
 *  在官方解释中说是因为这种模式会在start执行执行之前先执行rebuild的方法，而rebuild的方法不会发出任何事件通知。
 */
childrenCache.start(PathChildrenCache.StartMode.POST_INITIALIZED_EVENT);
System.out.println(childrenCache.getCurrentData());
//添加监听
childrenCache.getListenable().addListener(new PathChildrenCacheListener() {
    @Override
    public void childEvent(CuratorFramework client, PathChildrenCacheEvent event) throws Exception {
        if(event.getType() == PathChildrenCacheEvent.Type.CHILD_UPDATED){
            System.out.println("子节点更新");
            System.out.println("节点:"+event.getData().getPath());
            System.out.println("数据" + new String(event.getData().getData()));
        }else if(event.getType() == PathChildrenCacheEvent.Type.INITIALIZED ){
            System.out.println("初始化操作");
        }else if(event.getType() == PathChildrenCacheEvent.Type.CHILD_REMOVED ){
            System.out.println("删除子节点");
            System.out.println("节点:"+event.getData().getPath());
            System.out.println("数据" + new String(event.getData().getData()));
        }else if(event.getType() == PathChildrenCacheEvent.Type.CHILD_ADDED ){
            System.out.println("添加子节点");
            System.out.println("节点:"+event.getData().getPath());
            System.out.println("数据" + new String(event.getData().getData()));
        }else if(event.getType() == PathChildrenCacheEvent.Type.CONNECTION_SUSPENDED ){
            System.out.println("连接失效");
        }else if(event.getType() == PathChildrenCacheEvent.Type.CONNECTION_RECONNECTED ){
            System.out.println("重新连接");
        }else if(event.getType() == PathChildrenCacheEvent.Type.CONNECTION_LOST ){
            System.out.println("连接失效后稍等一会儿执行");
        }
    }
});
System.in.read(); // 使线程阻塞
```

##### 3.4.4、TreeCache

- **介绍**

  TreeCache有点像上面两种Cache的结合体，NodeCache能够监听自身节点的数据变化（或者是创建该节点），PathChildrenCache能够监听自身节点下的子节点的变化，而TreeCache既能够监听自身节点的变化、也能够监听子节点的变化。

- **添加监听**

```java
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
CuratorFramework client = CuratorFrameworkFactory.newClient("127.0.0.1:2181", 1000, 1000, retryPolicy);
client.start();
TreeCache treeCache = new TreeCache(client,"/hello");
treeCache.start();
System.out.println(treeCache.getCurrentData("/hello"));
treeCache.getListenable().addListener(new TreeCacheListener() {
    @Override
    public void childEvent(CuratorFramework client, TreeCacheEvent event) throws Exception {
            if(event.getType() == TreeCacheEvent.Type.NODE_ADDED){
                System.out.println(event.getData().getPath() + "节点添加");
            }else if (event.getType() == TreeCacheEvent.Type.NODE_REMOVED){
                System.out.println(event.getData().getPath() + "节点移除");
            }else if(event.getType() == TreeCacheEvent.Type.NODE_UPDATED){
                System.out.println(event.getData().getPath() + "节点修改");
            }else if(event.getType() == TreeCacheEvent.Type.INITIALIZED){
                System.out.println("初始化完成");
            }else if(event.getType() ==TreeCacheEvent.Type.CONNECTION_SUSPENDED){
                System.out.println("连接过时");
            }else if(event.getType() ==TreeCacheEvent.Type.CONNECTION_RECONNECTED){
                System.out.println("重新连接");
            }else if(event.getType() ==TreeCacheEvent.Type.CONNECTION_LOST){
                System.out.println("连接过时一段时间");
            }
    }
});
System.in.read();
```

### 【小结】

1： Zookeeper：分布小文件存储系统，树形层级数据结构，路径唯一

* 动物管理员，管的是服务
* 应用：注册中心、配置中心、分布式锁、分布式队列、负载均衡

2: 安装：没有中文没有空格，jdk1.8, 创建data,创建log 配置zoo.cfg dataPath, dataLogDir 2181

3: 节点类型：持久化（有序与无序 -s），临时（-e 临时， -s 有序与无序) crud

4: api: Curator 创建重试策略->客户端(url ip:port)->启动->使用client操作crud->关闭

5: Watch: 监听节点数据变化，一旦生变更（添加、修改、删除）时，服务端通知客户端（addListener)

NodeCache: 听监听单一节点变化

PathChildrenCache: 监听节点下的子节点

TreeCache: NodeCache+PathChildrenCache  

### 4、zookeeper集群搭建

### 【目标】

1：搭建Zookeeper集群

### 【路径】

1：了解集群

* 集群介绍
* 集群模式

2：集群原理及架构图

3：搭建集群（使用linux）

4：集群中Leader选举机制

5：测试集群

### 【讲解】

#### 4.1、了解集群

##### 4.1.1、集群介绍

​	Zookeeper 集群搭建指的是 ZooKeeper 分布式模式安装。 通常由 2n+1台 servers 组成 奇数???。 这是因为为了保证 Leader 选举（基于 Paxos 算法的实现） 能或得到多数的支持，所以 ZooKeeper 集群的数量一般为奇数。

##### 4.1.2、集群模式

**ZooKeeper集群搭建有两种方式：**

- **伪分布式集群搭建：**

   所谓伪分布式集群搭建ZooKeeper集群其实就是在一台机器上启动多个ZooKeeper，在启动每个ZooKeeper时分别使用不同的配置文件zoo.cfg来启动,每个配置文件使用不同的配置参数(clientPort端口号、dataDir数据目录.

   在zoo.cfg中配置多个server.id，其中ip都是当前机器，而端口各不相同，启动时就是伪集群模式了。

   这种模式和单机模式产生的问题是一样的。也是用在测试环境中使用。

- **完全分布式集群搭建：**

   多台机器各自配置zoo.cfg文件，将各自互相加入服务器列表，每个节点占用一台机器


#### 4.2、架构图 

![1573475887256](img/1573475887256.png)**Leader:Zookeeper(领导者)**:

集群工作的核心
事务请求（写操作） 的唯一调度和处理者，保证集群事务处理的顺序性；
集群内部各个服务器的调度者。
对于 create， setData， delete 等有写操作的请求，则需要统一转发给leader 处理， leader 需要决定编号、执行操作，这个过程称为一个事务。
**Follower（跟随者）**

处理客户端非事务（读操作） 请求，转发事务请求给 Leader；参与集群 Leader 选举投票 2n-1台可以做集群投票。

**Observer:观察者角色**

针对访问量比较大的 zookeeper 集群， 还可新增观察者角色。观察 Zookeeper 集群的最新状态变化并将这些状态同步过来，其对于非事务请求可以进行独立处理，对于事务请求，则会转发给 Leader服务器进行处理。
不会参与任何形式的投票只提供非事务服务，通常用于在不影响集群事务处理能力的前提下提升集群的非事务处理能力。

#### 4.3、搭建过程

##### 4.3.1、准备三台电脑（虚拟机）

- 安装三个linux虚拟机

  ![1573473107153](img/1573473107153.png)

- 配置虚拟网卡（三台电脑配置一个虚拟网卡）（虚拟网卡ip为：192.168.174.1）

  ![1573473145051](img/1573473145051.png)

- 为每台虚拟机配置静态ip地址

![1573473341207](img/1573473341207.png)`

重启网络：service network restart

**三台虚拟机分别是：192.168.174.128、192.168.174.129、192.168.174.130**

**关闭防火墙:**

systemctl stop firewalld

**service iptables stop**

##### 4.3.2、安装jdk（略）

- 查看是否已经安装jdk

命令：rpm -qa|grep java

![1573473661840](img/1573473661840.png)	

- 卸载已安装的jdk

命令：rpm -e --nodeps

![1573473710517](img/1573473710517.png)

- 上传jdk到linux

  ![1573473789230](img/1573473789230.png)	

- 解压jdk

命令: tar -vxf  jdk-8u181-linux-i586.tar.gz

- 配置环境变量

命令：cd /  退回根目录

命令：cd etc

命令：vim profile （编辑etc/profile文件,将如下配置粘贴到文件中）

```
#set java environment
JAVA_HOME=/usr/local/jdk1.8.0_181
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH 
```

- 重新加载profile文件，即激活profile文件

命令：source profile

##### 4.3.3、上传zookeeper到虚拟机并解压

- 上传到虚拟机

![1573474157684](img/1573474157684.png)

- 解压

命令：tar -zxvf apache-zookeeper-3.5.5-bin.tar.gz     /usr/local(解压)

命令：cd usr/local (进入local路径)

命令：mv apache-zookeeper-3.5.5-bin zookeeper  (重命名文件夹)

##### 4.3.4、配置zookeeper

- 创建data目录

命令：cd zookeeper  (进入zookeeper目录)

命令：mkdir  data

- 修改conf/zoo.cfg

命令：cd conf  （进入conf目录）

命令：cp zoo_sample.cfg  zoo.cfg（复制zoo_sample.cfg，文件名为zoo.cfg）

![1573474979566](img/1573474979566.png)	

命令：vim zoo.cfg (修改zoo.cfg)

内容：

修改：

dataDir = /usr/local/zookeeper/data

添加：

server.1=192.168.174.128:2182:3182
server.2=192.168.174.129:2182:3182
server.3=192.168.174.130:2182:3182

![1573475069756](img/1573475069756.png)	

**参数详解**(了解)

```properties
1）tickTime：通信心跳数，Zookeeper服务器心跳时间，单位毫秒
Zookeeper使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个tickTime时间就会发送一个心跳，时间单位为毫秒。
它用于心跳机制，并且设置最小的session超时时间为两倍心跳时间。(session的最小超时时间是2*tickTime)
2）initLimit：Leader and Follwer初始通信时限
集群中的follower跟随者服务器(F)与leader领导者服务器(L)之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的Zookeeper服务器连接到Leader的时限。
投票选举新leader的初始化时间
Follower在启动过程中，会从Leader同步所有最新数据，然后确定自己能够对外服务的起始状态。
Leader允许F在initLimit时间内完成这个工作。
3）syncLimit：Leader and Follower同步通信时限
集群中Leader与Follower之间的最大响应时间单位，假如响应超过syncLimit * tickTime，
Leader认为Follwer死掉，从服务器列表中删除Follwer。
在运行过程中，Leader负责与ZK集群中所有机器进行通信，例如通过一些心跳检测机制，来检测机器的存活状态。
如果L发出心跳包在syncLimit之后，还没有从F那收到响应，那么就认为这个F已经不在线了。
4）dataDir：数据文件目录+数据持久化路径
保存内存数据库快照信息的位置，如果没有其他说明，更新的事务日志也保存到数据库。
5）clientPort：客户端连接端口
监听客户端连接的端口
6）集群中服务的列表
server.1=192.168.174.128:2182:3182
server.2=192.168.174.129:2182:3182
server.3=192.168.174.130:2182:3182
server.* 后面的数据对应myid中的数字 
ip地址对应每台虚拟机的ip地址
2182：表示的是这个服务器与集群中的 Leader 服务器交换信息的端口
3182：表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口
```

- 在data目录创建创建myid

命令：cd ..  (退出conf目录)

命令： cd data （进入data目录）

命令：vim myid（修改myid文件,分别设置为1(128),2(129),3(130)）

##### 4.3.5、启动zookeeper

命令：cd .. （退出data目录）

命令：cd bin(进入bin路径)

命令:  ./zkServer.sh start

![1573475395307](img/1573475395307.png)	

##### 4.3.6、查看zookeeper状态

命令：./zkServer.sh status

![1573475640041](img/1573475640041.png)	

![1573475588983](img/1573475588983.png)	

![1573475570125](img/1573475570125.png)	

#### 4.4、leader选举

**在领导者选举的过程中，如果某台ZooKeeper获得了超过半数的选票，则此ZooKeeper就可以成为Leader了。**

- **服务器1**启动，给自己投票，然后发投票信息，由于其它机器还没有启动所以它收不到反馈信息，服务器1的状态一直属于Looking(选举状态)。

- **服务器2**启动，给自己投票，同时与之前启动的服务器1交换结果，

   每个Server发出一个投票由于是初始情况，1和2都会将自己作为Leader服务器来进行投票，每次投票会包含所推举的服务器的myid和ZXID，使用(myid, ZXID)来表示，此时1的投票为(1, 0)，2的投票为(2, 0)，然后各自将这个投票发给集群中其他机器。
   接受来自各个服务器的投票集群的每个服务器收到投票后，首先判断该投票的有效性，如检查是否是本轮投票、是否来自LOOKING状态的服务器

   处理投票。针对每一个投票，服务器都需要将别人的投票和自己的投票进行PK，PK规则如下

   　　　　· 优先检查ZXID。ZXID比较大的服务器优先作为Leader。

   　　　　· 如果ZXID相同，那么就比较myid。myid较大的服务器作为Leader服务器

   由于服务器2的编号大，更新自己的投票为(2, 0)，然后重新投票，对于2而言，其无须更新自己的投票，只是再次向集群中所有机器发出上一次投票信息即可，此时集群节点状态为LOOKING。

   统计投票。每次投票后，服务器都会统计投票信息，判断是否已经有过半机器接受到相同的投票信息

- **服务器3**启动，进行统计后，判断是否已经有过半机器接受到相同的投票信息，对于1、2、3而言，已统计出集群中已经有3台机器接受了(3, 0)的投票信息，此时投票数正好大于半数，便认为已经选出了Leader，

   改变服务器状态。一旦确定了Leader，每个服务器就会更新自己的状态，如果是Follower，那么就变更为FOLLOWING，如果是Leader，就变更为LEADING

   所以服务器3成为领导者，服务器1,2成为从节点。

**ZXID: 即zookeeper事务id号。ZooKeeper状态的每一次改变, 都对应着一个递增的Transaction id, 该id称为zxid**

1. 各自投自己一票
2. 如果票数相同，则进入第二轮投票
3. 改投zxid为大的。
4. zxid相同，投myid为大的
5. 统计得到的票数>50%就成为leader

#### 4.5、测试集(练习)

第一步：3台集群，随便挑选一个启动，执行命令./zkCli.sh

![001](img/001.png) 

第二步：添加节点数据

![002](img/002.png) 

第三步：找另一台机器，测试查询，可以获取hello节点的数据

![003](img/003.png) 

也可以使用代码测试

```java
@Test
public void createNode() throws Exception {
  	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,3);
  	CuratorFramework client = CuratorFrameworkFactory.newClient("192.168.174.128:2181", 3000, 3000, retryPolicy);
  	client.start();
  	client.create().creatingParentsIfNeeded().withMode(CreateMode.PERSISTENT).forPath("/bbb/b1","haha".getBytes());
  	client.close();
}

@Test
public void updateNode() throws Exception {
  	//创建失败策略对象
  	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,1);
  	CuratorFramework client = CuratorFrameworkFactory.newClient("192.168.174.129:2181",1000,1000,retryPolicy);
  	client.start();
  	//修改节点
  	client.setData().forPath("/bbb/b1", "fff".getBytes());
  	client.close();
}

@Test
public void getData() throws Exception {
  	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 1);
  	CuratorFramework client =
    CuratorFrameworkFactory.newClient("192.168.174.130:2181",1000,1000, retryPolicy);
  	client.start();
  	// 查询节点数据
  	byte[] bytes = client.getData().forPath("/bbb/b1");
  	System.out.println(new String(bytes));
  	client.close();
}

@Test
public void deleteNode() throws Exception {
  	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 1);
  	CuratorFramework client =
    CuratorFrameworkFactory.newClient("192.168.174.130:2181",1000,1000, retryPolicy);
  	client.start();
  	// 递归删除节点
  	client.delete().deletingChildrenIfNeeded().forPath("/bbb");
  	client.close();
}
```

测试使用任何一个IP都可以获取

测试如果有个机器宕机，（./zkServer.sh stop），会重新选取领导者。

### 【小结】

1. 集群？多台相同功能服务器组成的集群
2. 为什么？解决单点问题，高并发，高可用（故障转移）
3. zookeeper有哪些角色?
   * leader 领导者，读写数据，同步数据到其它服务器（追随者，观察者）
   * follower追随者，读，写时转给leader，同时参与投票与选举
   * observer观察者。读，不参与投票与选举

4. 集群的搭建

   * 三台虚拟机，分别修改网络与ip地址, service network restart
   * 安装zookeeper
     * 上传安装包
     * 解压，重命名，移动到/usr/local
     * 创建data目录, data目录下创建myid内容为server.id
     * 到conf复制zoo.cfg 修改存储data目录，添加集群节点信息

   * 启动 ./zkServer.sh start

5. 选举机制
   * 投自己一票
   * 事务id log文件里 最后一行zxid 16进制
   * 看myid
   * 谁大改投谁
   * 票数过半，票数/集群数量>50%成为leader
   * 避免选不了leader 集群数量为2n+1
   * 机器性能比较好的myid要调大一点

https://www.cnblogs.com/stateis0/category/1206895.html ZAB Zookeeper原子广播

2pc,3pc