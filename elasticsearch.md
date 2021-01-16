# 第1天 Elasticsearch入门

## 学习目标

- ==能够理解ElasticSearch的作用（第一章）==

- 能够安装ElasticSearch服务（第二章）

- ==能够理解ElasticSearch的相关概念（第三章）==

- ==能够使用java客户端操作ElasticSearch（第四章）==

- 能够理解分词器的作用（第五章）

- ==能够使用ElasticSearch集成IK分词器（第五章）==

- 能够使用restful技术操作操作ElasticSearch（附加）



## 1 Elasticsearch简介

### 1.1 目标

- 了解什么是Elasticsearch

  ```
  ElaticSearch，简称为es， es是一个开源的高扩展的分布式全文检索引擎,它可以近乎实时的存储、检索数据,处理PB级别的数据,提供了一套RESTful API操作索引库。
  ```

- 了解Elasticsearch使用案例

- 了解Elasticsearch与Solr(zookeeper)的区别(面试点) PB级别以上的数据



### 1.2 讲解

#### 1.2.1 什么是Elasticsearch

![1563519084291](images\1563519084291.png)

ElaticSearch，简称为es， es是一个开源的==高扩展==的分布式全文检索引擎，它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。es也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

什么是PB级别的数据？

数据单位：B、KB、MB、GB、TB、PB

1PB=1024TB，依次类推。



#### 1.2.2 Elasticsearch使用案例

2013年初，GitHub抛弃了Solr，采取ElasticSearch 来做PB级的搜索。 “GitHub使用ElasticSearch搜索20TB的数据，包括13亿文件和1300亿行代码”

维基百科：启动以elasticsearch为基础的核心搜索架构

SoundCloud：“SoundCloud使用ElasticSearch为1.8亿用户提供即时而精准的音乐搜索服务”

百度：百度目前广泛使用ElasticSearch作为文本数据分析，采集百度所有服务器上的各类指标数据及用户自定义数据，通过对各种数据进行多维分析展示，辅助定位分析实例异常或业务层面异常。目前覆盖百度内部20多个业务线（包括casio、云析分、网盟、预测、文库、直达号、钱包、风控等），单集群最大100台机器，200个ES节点，每天导入30TB+数据

新浪：使用ES 分析处理32亿条实时日志

阿里：使用ES 构建挖财自己的日志采集和分析体系



#### 1.2.3 Elasticsearch VS Solr

==Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能;==

==Solr 支持更多格式的数据，而 Elasticsearch 仅支持json文件格式；==

Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多由第三方插件提供；

Solr 在传统的搜索应用中表现好于 Elasticsearch，但在处理实时搜索应用时效率明显低于 Elasticsearch；

Elasticsearch支持RestFul风格编程（uri的地址，就可以检索），Solr暂不支持；



## 2 Elasticsearch的安装与启动

### 2.1 目标

- 安装Elasticsearch
- 安装Elasticsearch-head图形化界面



### 2.2 讲解

#### 2.2.1 下载ES安装包

ElasticSearch分为Linux和Window版本，基于我们主要学习的是ElasticSearch的Java客户端的使用，所以我们课程中使用的是安装较为简便的Window版本，项目上线后，公司的运维人员会安装Linux版的ES供我们连接使用。

ElasticSearch的官方下载地址:`<https://www.elastic.co/cn/downloads/elasticsearch>`

![1563519798565](images\1563519798565.png)

![1563519926030](images\1563519926030.png)

![1563519994746](images\1563519994746.png)

在资料中已经提供了下载好的5.6.8的压缩包：

![1563520067635](images\1563520067635.png)





#### 2.2.2 安装ES服务

Window版的ElasticSearch的安装很简单，类似Window版的Tomcat，解压开即安装完毕，解压后的ElasticSearch的目录结构如下：

注意：ES的目录不要出现中文，也不要有特殊字符。

![1563520383848](images\1563520383848.png)



#### 2.2.3 启动ES

![1563520485574](images\1563520485574.png)

启动

![1563520790748](images\1563520790748.png)

解释：

```properties
注意：
9300是tcp通讯端口，集群间和TCPClient都执行该端口，可供java程序调用；
9200是http协议的RESTful接口 。
通过浏览器访问ElasticSearch服务器，看到如下返回的json信息，代表服务启动成功：
```

访问地址：

![1563520913284](images\1563520913284.png)

注意事项一：ElasticSearch5是使用java开发的，且本版本的es需要的jdk版本要是1.8以上，所以安装ElasticSearch之前保证JDK1.8+安装完毕，并正确的配置好JDK环境变量，否则启动ElasticSearch失败。

注意事项二：出现闪退，通过路径访问发现“空间不足”

![1563521480411](images\1563521480411.png)

【解决方案】

![1563521555662](images\1563521555662.png)

修改jvm.options文件的22行23行，把2改成1，让Elasticsearch启动的时候占用1个G的内存。

![1563521613051](images\1563521613051.png)

如果1个g还是不行：

![1563521669504](images\1563521669504.png)

-Xmx512m：设置JVM最大可用内存为512M。
-Xms512m：设置JVM初始内存为512m。此值可以设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。



#### 2.2.4 ES图形化界面安装

ElasticSearch不同于Solr自带图形化界面，我们可以通过安装ElasticSearch的head插件，完成图形化界面的效果，完成索引数据的查看。安装插件的方式有两种，在线安装和本地安装。本课程采用在线的安装。elasticsearch-5-*以上版本安装head需要安装node和grunt。

![1563521754717](images\1563521754717.png)

https://www.gruntjs.net/

![1563521964028](images\1563521964028.png)



```properties
npm 为你和你的团队打开了连接整个 JavaScript 天才世界的一扇大门。它是世界上最大的软件注册表，每星期大约有 30 亿次的下载量，包含超过 600000 个 包（package） （即，代码模块）。来自各大洲的开源软件开发者使用 npm 互相分享和借鉴。包的结构使您能够轻松跟踪依赖项和版本。
```



1）下载head插件：<https://github.com/mobz/elasticsearch-head> 

![1563522011170](images\1563522011170.png)

在资料中已经提供了elasticsearch-head-master插件压缩包：

![1563522057931](images\1563522057931.png)

2）将elasticsearch-head-master压缩包解压到任意目录（我这里是D:\software\elasticsearch-5.6.8），但是要和elasticsearch的安装目录区别开

3）进入elasticsearch-head-master压缩包执行命令安装

```
npm install
```

4）下载nodejs：<https://nodejs.org/en/download/> 

![1563522134055](images\1563522134055.png)

要想使用npm命令，需要先安装nodejs。

在资料中已经提供了nodejs安装程序：

![1563522192417](images\1563522192417.png)

双击安装程序，步骤截图如下：

![1563522351819](images\1563522351819.png)



![1563522404527](images\1563522404527.png)



![1563522425037](images\1563522425037.png)



![1563522447044](images\1563522447044.png)



![1563522474891](images\1563522474891.png)



安装完毕，可以通过cmd控制台输入：node -v 查看版本号

![1563522573637](images\1563522573637.png)

5）将grunt安装为全局命令 ，Grunt是基于Node.js的项目构建工具；是基于javaScript上的一个很强大的前端自动化工具，基于NodeJs用于自动化构建、测试、生成文档的项目管理工具。

在cmd控制台中输入如下执行命令：

```properties
npm install -g grunt-cli
```

-g表示全局（globle）变量，让grunt-cli的客户端使用全局安装

执行结果如下图：

效果如下：

![1563522638852](images\1563522638852.png)

![1563522664141](images\1563522664141.png)

【了解淘宝镜像安装】

ps:如果安装不成功或者安装速度慢，可以使用淘宝的镜像进行安装：

```properties
npm install -g cnpm –registry=https://registry.npm.taobao.org
```

后续使用的时候，只需要把npm xxx   换成  cnpm xxx 即可

6）进入head目录启动head，在命令提示符下输入命令：

```properties
grunt server
```

![1563523877086](images\1563523877086.png)

根据提示访问，效果如下：

![1563523832631](images\1563523832631.png)



如果出现如下问题，就需要重装grunt

![1563524004247](images\1563524004247.png)

执行以下命令（比较慢，需耐心等待）

```
npm install grunt
```

再次启动grunt server

![1563523657386](images\1563523657386.png)

上面问题解决方法，将下面命令分别执行安装：

```properties
npm install grunt-contrib-clean
npm install grunt-contrib-watch
npm install grunt-contrib-connect
npm install grunt-contrib-copy
npm install grunt-contrib-jasmine
```

7）跨域问题：

访问http://localhost:9100/

![1563524116355](images\1563524116355.png)

修改elasticsearch/config下的配置文件：elasticsearch.yml，增加以下三句命令：

```properties
http.cors.enabled: true
http.cors.allow-origin: "*"
network.host: 127.0.0.1
```



三个命令解释：

```properties
http.cors.enabled: true：此步为允许elasticsearch跨域访问，默认是false。
http.cors.allow-origin: "*"：表示跨域访问允许的域名地址（*表示任意）。
network.host:127.0.0.1：主机域名
```

【重启Elasticsearch】

![1563524270755](images\1563524270755.png)



### 2.3 小结

- 安装Elasticsearch

  ```
  1.解压安装包
  2.进入到bin目录，双击elasticsearch.bat脚本
  3.http://localhost:9200/查看ES版本信息(闪退解决---修改esconfig中的jvm.options文件)
  ```

- 安装Elasticsearch-head图形化界面

  ```
  1.安装Nodejs开发平台
  2.安装grunt
  3.在Elasticsearch-head目录输入grunt server命令启动
  4.配置ES，允许ES跨域
  ```




## 3 ElasticSearch相关概念(术语)

### 3.1 目标

- 了解ES与传统关系型数据库之间的结构关系
- ES的核心概念

### 3.2 讲解

#### 3.2.1 概述

Elasticsearch是面向文档(document poriented)的，这意味着它可以存储整个对象或文档(document)。然而它不仅仅是存储（store），还会索引(index)每个文档的内容使之可以被搜索。在Elasticsearch中，你可以对文档（而非成行成列的数据）进行索引、搜索、排序、过滤。Elasticsearch比较传统关系型数据库如下：

```properties
Relational DB -> Databases -> Tables -> Rows -> Columns
Elasticsearch -> Indices(索引)   -> Types(类型)  -> Documents(文档) -> Fields(域)(是否分词 是否索引 是否存储 类型) mapping(映射)
```

![1563524454527](images\1563524454527.png)

以下表示2个文档（document），3个字段（field）

![1563524513038](images\1563524513038.png)



#### 3.2.2 Elasticsearch核心概念

(1)索引 index

一个索引就是一个拥有几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母的），并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。

(2)类型 type

在一个索引中，你可以定义一种或多种类型。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。比如说，我们假设你运营一个博客平台并且将你所有的数据存储到一个索引中。在这个索引中，你可以为用户数据定义一个类型，为博客数据定义另一个类型，当然，也可以为评论数据定义另一个类型。

(3)文档 document

一个文档是一个可被索引的基础信息单元。比如，你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以JSON（Javascript Object Notation）格式来表示，而JSON是一个到处存在的互联网数据交互格式。

在一个index/type里面，你可以存储任意多的文档。注意，尽管一个文档，物理上存在于一个索引之中，文档必须被索引/赋予一个索引的type。

(4)字段 Field(域)

相当于是数据表的字段，对文档数据根据不同属性进行的分类标识

(5)映射 mapping

mapping是处理数据的方式和规则方面做一些限制，如某个字段的数据类型、默认值、分词器、是否被索引等等，这些都是映射里面可以设置的，其它就是处理es里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好。

例如：

![1563524716499](images\1563524716499.png)



(6)接近实时 NRT

Elasticsearch是一个接近实时的搜索平台。这意味着，从索引一个文档直到这个文档能够被搜索到有一个轻微的延迟（通常是1秒以内）



(7)集群 cluster

一个集群就是由一个或多个节点组织在一起，它们共同持有整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是“elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群



(8)节点 node

一个节点是集群中的一个服务器，作为集群的一部分，它存储数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于Elasticsearch集群中的哪些节点。

一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此，它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。

在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何Elasticsearch节点，这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的集群。



(9)分片和复制 shards&replicas

一个索引可以存储超出单个节点硬件限制的大量数据。比如，一个具有10亿文档的索引占据1TB的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。分片很重要，主要有两方面的原因： 1）允许你水平分割/扩展你的内容容量。 2）允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量。

至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由Elasticsearch管理的，对于作为用户的你来说，这些都是透明的。

在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。

（后面大家自己看即可）

复制之所以重要，有两个主要原因： 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行。总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。

默认情况下，Elasticsearch中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。

![1563524937637](images\1563524937637.png)

下节课我们会一起搭建集群的环境。

单机版，只要1个Elasticsearch的服务：

![1563524982215](images\1563524982215.png)



### 3.3 总结

- 了解ES与传统关系型数据库之间的结构关系

  ```properties
  Relational DB -> Databases -> Tables -> Rows -> Columns-->table属性
  Elasticsearch -> Indices   -> Types  -> Documents -> Fields--->mapping映射
  ```

- ES的核心概念

  ```
  索引(index),类型(types),文档(document),域(Feild),映射(mapping)，近似实时搜索，集群，节点，分片，复制（主从）
  ```

  

## 4 ElasticSearch操作入门

### 4.1 目标

- 创建文档(JSONBuilder/Map)
- ES数据搜索

### 4.2 讲解

#### 4.2.1 工程搭建

(1)创建Maven工程

工程坐标如下：

```xml
<groupId>com.itheima</groupId>
<artifactId>elasticsearch-day1-demo1</artifactId>
<version>1.0-SNAPSHOT</version>
```



(2)pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>elasticsearch-day1-demo1</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!--ES包-->
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch</artifactId>
            <version>5.6.8</version>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>transport</artifactId>
            <version>5.6.8</version>
        </dependency>
        <!--日志包-->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-to-slf4j</artifactId>
            <version>2.9.1</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.24</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.21</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <!--测试包-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
</project>
```



#### 4.2.2 新建索引+添加文档

使用创建索引（index）+类型（type）+自动创建映射（Elasticsearch帮助我们根据存储的字段自动建立映射，后续讲完分词器后，手动建立映射）

(1)基于jsonBuilder创建索引

创建测试类`com.itheima.test.ElasticsearchTest`，并在类中创建testCreateIndexJsonDemo1方法，实现创建索引，代码如下：

```java
/***
 * 组织Document数据（使用ES的api构建json）
 */
@Test
public void testCreateIndexJsonDemo1() throws Exception {
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    //构建文档对象
    XContentBuilder builder = XContentFactory.jsonBuilder().startObject();
    builder.field("id",1);
    builder.field("title","ElasticSearch是一个基于Lucene的搜索服务器。");
    builder.field("content","它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。");
    builder.endObject();

    //使用TransportClient对象创建文档
    client.prepareIndex("blog1","article","1").setSource(builder).get();

    //关闭资源
    client.close();
}
```

访问`<http://localhost:9100/>`，效果如下：

![1563526646333](images\1563526646333.png)



(2)使用Map创建索引

在测试类`com.itheima.test.ElasticsearchTest`，中创建testCreateIndexMapDemo2方法，实现创建索引，代码如下：

```java
/***
 * 使用Map创建索引
 */
@Test
public void testCreateIndexMapDemo2() throws Exception {
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的TCP通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    //构建文档对象
    Map<String,Object> map = new HashMap<String,Object>();
    map.put("id",2);
    map.put("title","2-ElasticSearch是一个基于Lucene的搜索服务器");
    map.put("content","2-它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。");

    //使用TransportClient对象创建文档
    client.prepareIndex("blog1","article","2").setSource(map).get();

    //关闭资源
    client.close();
}
```

访问`<http://localhost:9100/>`，效果如下：

![1563526895865](images\1563526895865.png)



#### 4.2.3 搜索文档数据

(1)根据ID查询(不走索引)

```java
/***
 * 根据ID查询
 */
@Test
public void testQueyById() throws Exception{
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    //查询数据
    GetResponse response = client.prepareGet("blog1", "article", "1").get();
    //获取结果集数据
    String result = response.getSourceAsString();
    System.out.println(result);
}
```

此时直接根据ID查询，不会走索引域，直接根据ID定位数据，执行后结果如下：

```json
{"id":1,"title":"ElasticSearch是一个基于Lucene的搜索服务器。","content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"}
```



(2)查询全部(不走索引)

```java
/***
 * 查询所有，不走索引
 */
@Test
public void testQueryAll() throws Exception{
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    /**
     * 搜索数据
     */
    SearchResponse response = client
            .prepareSearch("blog1")
            .setTypes("article")
            .setQuery(QueryBuilders.matchAllQuery())    //设置查询条件,QueryBuilders.matchAllQuery()查询所有
            .get();

    //处理结果
    SearchHits hits = response.getHits();
    //总记录数
    long totalHits = hits.getTotalHits();
    //获取数据结果集
    Iterator<SearchHit> iterator = hits.iterator();
    while (iterator.hasNext()){
        //获取数据
        SearchHit hit = iterator.next();
        //获取指定域数据
        String title = hit.getSource().get("title").toString();
        //获取所有数据，并转成JSON字符串
        String result = hit.getSourceAsString();
        System.out.println(title);
        System.out.println(result);
    }
}
```

查询所有数据，不走索引域，直接获取所有结果记录，控制台输出结果如下：

```json
2-ElasticSearch是一个基于Lucene的搜索服务器
{"id":2,"title":"2-ElasticSearch是一个基于Lucene的搜索服务器","content":"2-它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。"}
ElasticSearch是一个基于Lucene的搜索服务器。
{"id":1,"title":"ElasticSearch是一个基于Lucene的搜索服务器。","content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"}
```



(3)字符串查询

```java
/***
 * 字符串查询
 */
@Test
public void testQueryByString() throws Exception{
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    /**
     * 搜索数据
     */
    SearchResponse response = client
            .prepareSearch("blog1")
            .setTypes("article")
            // 默认在所有的域上进行搜索，搜索“搜索”；如果添加.field("title")：表示只在title域进行搜索
            .setQuery(QueryBuilders.queryStringQuery("搜索").field("title"))
            .get();

    //处理结果
    SearchHits hits = response.getHits();
    //总记录数
    long totalHits = hits.getTotalHits();
    //获取数据结果集
    Iterator<SearchHit> iterator = hits.iterator();
    while (iterator.hasNext()){
        //获取数据
        SearchHit hit = iterator.next();
        //获取指定域数据
        String title = hit.getSource().get("title").toString();
        //获取所有数据，并转成JSON字符串
        String result = hit.getSourceAsString();
        System.out.println(title);
        System.out.println(result);
    }
}
```

此时会走索引域，搜索结果如下：

```json
2-ElasticSearch是一个基于Lucene的搜索服务器
{"id":2,"title":"2-ElasticSearch是一个基于Lucene的搜索服务器","content":"2-它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。"}
ElasticSearch是一个基于Lucene的搜索服务器。
{"id":1,"title":"ElasticSearch是一个基于Lucene的搜索服务器。","content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"}
```



(4)词条搜索

![1563528706353](images\1563528706353.png)

![1563528732820](images\1563528732820.png)

```java
/***
 * 词条查询
 */
@Test
public void testQueryByTerm() throws Exception{
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    /**
     * 搜索数据
     */
    SearchResponse response = client
            .prepareSearch("blog1")
            .setTypes("article")
            // 设置查询条件
            .setQuery(QueryBuilders.termQuery("title","搜索"))
            .get();

    //处理结果
    SearchHits hits = response.getHits();
    //总记录数
    long totalHits = hits.getTotalHits();
    //获取数据结果集
    Iterator<SearchHit> iterator = hits.iterator();
    while (iterator.hasNext()){
        //获取数据
        SearchHit hit = iterator.next();
        //获取指定域数据
        String title = hit.getSource().get("title").toString();
        //获取所有数据，并转成JSON字符串
        String result = hit.getSourceAsString();
        System.out.println(title);
        System.out.println(result);
    }

    //关闭资源
    client.close();
}
```

搜不到结果：为什么？



(5)模糊查询(通配符查询)

*：表示所有的任意的多个字符组成

?：表示1个任意的字符

```java
/***
 * 通配符查询
 */
@Test
public void testQueryByKeyword() throws Exception{
    /***
     * 创建TransportClient对象
     * Settings：表示集群的设置
     * EMPTY：表示没有集群的配置
     * Settings.EMPTY：不使用集群
     */
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    /****
     * 配置链接信息
     * 127.0.0.1：Elasticsearch的IP地址
     * 9300：Elasticsearch的通信端口
     */
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));

    /**
     * 搜索数据
     */
    SearchResponse response = client
            .prepareSearch("blog1")
            .setTypes("article")
            // 设置查询条件
            .setQuery(QueryBuilders.wildcardQuery("title","*搜索*"))
            .get();

    //处理结果
    SearchHits hits = response.getHits();
    //总记录数
    long totalHits = hits.getTotalHits();
    //获取数据结果集
    Iterator<SearchHit> iterator = hits.iterator();
    while (iterator.hasNext()){
        //获取数据
        SearchHit hit = iterator.next();
        //获取指定域数据
        String title = hit.getSource().get("title").toString();
        //获取所有数据，并转成JSON字符串
        String result = hit.getSourceAsString();
        System.out.println(title);
        System.out.println(result);
    }

    //关闭资源
    client.close();
}
```

搜素不到结果。

词条查询和通配符查询不能查询到数据，为什么呢？

#### 4.2.4 词条总结

词条： 就是将文本内容存入搜索服务器，搜索服务器进行分词之后的最小的词（不能再切分）

例如：“ElasticSearch是一个基于Lucene的搜索服务器”

​	分词（好的）： ElasticSearch、是、一个、基于、Lucene、搜索、服务、服务器 

​	默认单字分词（差的）： ElasticSearch、 是、一、个、基、于、搜、索、服、务、器

![1563529248784](images\1563529248784.png)



### 4.3 小结

- 创建文档(JSON/Map)

  ```
  创建文档数据有2种方法，一种是JSON方式，另外一种是基于Map方式
  ```

- ES数据搜索

  ```
  根据ID搜索、搜索全部不走索引
  字符串搜索、词条搜索、通配符搜索走索引
  ```

  



## 5 IK分词器

### 5.1 目标

- 能安装IK分词器
- 会配置扩展词库和停用词库



### 5.2 讲解

在进行词条查询时，我们搜索"搜索"却没有搜索到数据！

原因：lucene默认是单字分词，在开发中不符合查询的需求，需要定义一个支持中文的分词器。

解决方案：IK分词器



(1)IK分词器简介

IKAnalyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包。

![1563529574529](images\1563529574529.png)



(2)ElasticSearch集成IK分词器

```properties
1.解压elasticsearch-analysis-ik-2.x后,将文件夹拷贝到elasticsearch-5.6.8\plugins下，并重命名文件夹为ik
2. 重新启动ElasticSearch，即可加载IK分词器
```

![1563529668476](images\1563529668476.png)

![1563529947061](images\1563529947061.png)

在使用ik分词器之前：使用rest方式访问elasticsearch的分词效果，默认的分词是standard

`http://127.0.0.1:9200/_analyze?analyzer=standard&pretty=true&text=我是中国人` 效果如下：

![1563530014018](images\1563530014018.png)



(3)IK分词器测试

IK提供了两个分词算法ik_smart（最小切分） 和 ik_max_word（最细切分）

**最小切分：在浏览器地址栏输入地址**

`http://127.0.0.1:9200/_analyze?analyzer=ik_smart&pretty=true&text=我是程序员` 效果如下：

![1563530223307](images\1563530223307.png)



**最细切分：在浏览器地址栏输入地址**

`http://127.0.0.1:9200/_analyze?analyzer=ik_max_word&pretty=true&text=我是程序员` 效果如下：

![1563530282056](images\1563530282056.png)



(4)拓展词库

传智播客，默认IK分词是单字分词，想让它做为一个词。

`<http://127.0.0.1:9200/_analyze?analyzer=ik_smart&pretty=true&text=我是程序员,来自于传智播客>`

![1563530523915](images\1563530523915.png)

需要配置，IKAnalyzer.cfg.xml：

![1563530603310](images\1563530603310.png)

配置文件中添加一个ext.dic和stop.dic，如果多个拓展词用逗号分开,如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">ext.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">stop.dic</entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

ext.dic使用UTF-8编辑

![1563531034889](images\1563531034889.png)

stop.dic使用UTF-8编辑

![1563531140234](images\1563531140234.png)

重启Elasticsearch，再访问测试

`<http://127.0.0.1:9200/_analyze?analyzer=ik_smart&pretty=true&text=我是程序员,来自于传智播客>`效果如下：

![1563531349020](images\1563531349020.png)



在ElasticSearch没有索引的情况下，插入文档，默认自动创建索引和索引映射是无法使用IK分词器的。

要想让索引库可以使用ik分词器，必须要手动创建映射的配置。如何手动创建映射呢？下回分解。

Java代码手动建立映射（添加ik分词器）（下回分解）

使用restful手动建立映射（添加ik分词器）（一会分解）



## 6 RESTFul操作索引

### 6.1 目标

- 了解什么是RESTFul --编码风格
- 会使用Postman工具
- 能使用Postman工具操作索引

### 6.2 讲解

#### 6.2.1 RESFFul介绍

什么是restful？

传统方式：

```properties
URL                                        服务器上资源
http://localhsot:8080/save                访问的资源save();
http://localhsot:8080/update              访问的资源update();
http://localhsot:8080/delete              访问的资源delete();
http://localhsot:8080/find                访问的资源find();
```

Restful：表现层状态转移

```properties
HTTP的请求状态                URI                                  服务器上资源
Http的协议POST              http://localhsot:8080/list          访问的资源save();
Http的协议PUT               http://localhsot:8080/list          访问的资源update();
Http的协议DELETE            http://localhsot:8080/list          访问的资源delete();
Http的协议GET               http://localhsot:8080/list          访问的资源find();
```

#### 6.2.2 安装Postman工具

Postman中文版是postman这款强大网页调试工具的windows客户端，提供功能强大的Web API & HTTP 请求调试。软件功能非常强大，界面简洁明晰、操作方便快捷，设计得很人性化。Postman中文版能够发送任何类型的HTTP 请求 (GET, HEAD, POST, PUT，DELETE..)，且可以附带任何数量的参数。



下载Postman工具

Postman官网：<https://www.getpostman.com>

课程资料中已经提供了安装包

![1563577458850](images\1563577458850.png)

双击安装完成后需要自行注册一个账号，这里就不演示了。



#### 6.2.3 使用Postman工具进行Restful接口访问

ElasticSearch的接口语法

```properties
curl -X<VERB> '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
```

案例：

```properties
curl -XPUT http://localhost:9200/blog/article/1 -d '{"title": "New version of
Elasticsearch released!", content": "Version 1.0 released today!", "tags": ["announce",
"elasticsearch", "release"] }'
```



参数解释如下：

| **参数**     | **解释**                                                     |
| ------------ | ------------------------------------------------------------ |
| VERB         | 适当的 HTTP *方法* 或 *谓词* : GET、 POST、 PUT、 HEAD 或者 DELETE。 |
| PROTOCOL     | http 或者 https（如果你在 Elasticsearch 前面有一个 https 代理） |
| HOST         | Elasticsearch 集群中任意节点的主机名，或者用 localhost 代表本地机器上的节点。 |
| PORT         | 运行 Elasticsearch HTTP 服务的端口号，默认是 9200 。         |
| PATH         | API 的终端路径（例如 _count 将返回集群中文档数量）。Path 可能包含多个组件，例如：_cluster/stats 和 _nodes/stats/jvm 。 |
| QUERY_STRING | 任意可选的查询字符串参数 (例如 ?pretty 将格式化地输出 JSON 返回值，使其更容易阅读) |
| BODY         | 一个 JSON 格式的请求体 (如果请求需要的话)                    |



(1)创建映射

请求URL:

```properties
PUT     http://localhost:9200/blog2
```

请求体：

```properties
{
    "mappings": {
		"article": {
			"properties": {
				"id": {
					"type": "long",
					"store": false,
					"index":"not_analyzed"
				},
				"title": {
					"type": "text",
					"store": false,
					"index":"analyzed",
					"analyzer":"ik_smart"
				},
				"content": {
					"type": "text",
					"store": false,
					"index":"analyzed",
					"analyzer":"ik_smart"
				}
			}
		}
	}
}
```

注意：

"analyzer":"standard"：表示单字分词（默认值）

"analyzer":"ik_smart"：使用ik分词器（ik的最小切分）

"store": true：表示是否存储，只有存储到索引库，才能检索到结果（Elasticsearch的默认值flase）

关键字高亮实质上是根据倒排记录中的词项偏移位置，找到关键词，加上前端的高亮代码。这里就要说到store属性，store属性用于指定是否将原始字段写入索引，默认取值为no。如果在Lucene中，高亮功能和store属性是否存储息息相关，因为需要根据偏移位置到原始文档中找到关键字才能加上高亮的片段。在Elasticsearch，因为_source中已经存储了一份原始文档，可以根据_source中的原始文档实现高亮，在索引中再存储原始文档就多余了，所以Elasticsearch默认是把store属性设置为no。

示例如下：

![1563577922202](images\1563577922202.png)

Elasticsearch-head

![1563578064426](images\1563578064426.png)





(2)删除索引

请求URL

```properties
DELETE      http://localhost:9200/blog2
```

Postman如下：

![1563578109800](images\1563578109800.png)

elasticsearch-head查看：

![1563578143367](images\1563578143367.png)



(3)创建文档document

把上面的映射重新创建一次，然后再执行创建文档

创建文档URL：

```properties
POST    http://localhost:9200/blog2/article/1
```

请求体数据：

```json
{
"id":1,
"title":"ElasticSearch是一个基于Lucene的搜索服务器",
"content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTfulweb接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够时搜索。"
}
```

Postman如下：

![1563578489298](images\1563578489298.png)



elasticsearch-head查看：

![1563578397998](images\1563578397998.png)



(4)修改文档对象Document

请求URL：

```properties
http://localhost:9200/blog2/article/1
```

请求体：

```json
{
"id":1,
"title":"ElasticSearch是一个基于Lucene的搜索服务器-深圳黑马训练营",
"content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTfulweb接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够时搜索。"
}
```

Postman如下：

![1563578556279](images\1563578556279.png)

elasticsearch-head查看：

![1563578599379](images\1563578599379.png)



(5)删除文档Document

请求URL：

```properties
DELETE    http://localhost:9200/blog2/article/1
```

Postman如下：

![1563578689198](images\1563578689198.png)



(6)查询文档-根据id查询

请求URL：

```properties
GET      http://localhost:9200/blog2/article/1
```



Postman如下：

![1563578777549](images\1563578777549.png)



(7)查询文档-querystring查询

请求url：

```properties
POST       http://localhost:9200/blog2/article/_search
```

请求体：

```properties
{
	"query": {
		"query_string": {
			"default_field": "title",
			"query": "搜索服务器"
		}
	}
}
```



Postman如下：

![1563578889289](images\1563578889289.png)



(8)查询文档-term查询

请求URL：

```properties
POST     http://localhost:9200/blog2/article/_search
```

请求体：

```properties
{
	"query": {
		"term": {
			"title": "搜索"
		}
	}
}
```



Postman如下：

![1563578991421](images\1563578991421.png)



### 6.3 小结

- 了解什么是RESTFul

  ```properties
  RESTful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。
  ```

- 能使用Postman工具操作索引

  ```properties
  使用Postman可以创建索引映射、删除索引映射、根据ID查询索引数据、删除索引数据、字符串查找数据、词项查找数据等。
  ```

  

