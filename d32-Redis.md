---
typora-copy-images-to: img
---

# day32-Redis 

# 学习目标

- [ ] 能够说出redis的常用数据类型

- [ ] 能够使用redis的string操作命令

- [ ] 能够使用redis的hash操作命令

- [ ] 能够使用redis的list操作命令

- [ ] 能够使用redis的set操作命令

- [ ] 能够使用redis的zset操作命令

- [ ] 能够使用jedis操作Redis

- [ ] 能够理解Redis持久化

  
  
  



# 第一章-Redis介绍和安装

## 知识点-Nosql概述

### 1.目标

+ 能够理解nosql的概念

### 2.路径

+ 什么是NOSQL
+ 为什么需要NOSQL
+ 主流的NOSQL产品
+ NOSQL的特点

### 3.讲解

#### 3.1 什么是NOSQL 

​	NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指==非关系型的数据库==。



#### 3.2.为什么需要学习NOSQL

​	随着互联网的高速崛起，网站的用户群的增加，访问量的上升，传统(关系型)数据库上都开始出现了性能瓶颈，web程序不再仅仅专注在功能上，同时也在追求性能。所以NOSQL数据库应运而上，具体表现为对如下三高问题的解决：

- High performance - 对数据库高并发读写的需求    

  ​	web2.0网站要根据用户个性化信息来实时生成动态页面和提供动态信息，所以基本上无法使用动态页面静态化技术，因此数据库并发负载非常高，往往要达到每秒上万次读写请求。关系数据库应付上万次SQL查询还勉强顶得住，但是应付上万次SQL写数据请求，硬盘IO就已经无法承受了。其实对于普通的BBS网站，往往也存在对高并发写请求的需求，例如网站的实时统计在线用户状态，记录热门帖子的点击次数，投票计数等，因此这是一个相当普遍的需求。

- Huge Storage - 对海量数据的高效率存储和访问的需求    

  ​	类似Facebook，twitter，Friendfeed这样的SNS网站，每天用户产生海量的用户动态，以Friendfeed为例，一个月就达到了2.5亿条用户动态，对于关系数据库来说，在一张2.5亿条记录的表里面进行SQL查询，效率是极其低下乃至不可忍受的。再例如大型web网站的用户登录系统，例如腾讯，盛大，动辄数以亿计的帐号，关系数据库也很难应付。 

- High Scalability && High Availability- 对数据库的高可扩展性和高可用性的需求     

  ​	在基于web的架构当中，数据库是最难进行横向扩展的，当一个应用系统的用户量和访问量与日俱增的时候，你的数据库却没有办法像web server和app server那样简单的通过添加更多的硬件和服务节点来扩展性能和负载能力。对于很多需要提供24小时不间断服务的网站来说，对数据库系统进行升级和扩展是非常痛苦的事情，往往需要停机维护和数据迁移，为什么数据库不能通过不断的添加服务器节点来实现扩展呢？

  


#### 3.3 主流的NOSQL产品

![](img/1.png)

-  键值(Key-Value)存储数据库： redis

-  列存储数据库(分布式)

-  文档型数据库 (Web应用与Key-Value类似，Value是结构化的）

-  图形(Graph)数据库(图结构)



#### 3.4NOSQL的特点

​	在大数据存取上具备关系型数据库无法比拟的性能优势，例如：

-  易扩展

   ​	  NoSQL数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。数据之间无关系，这样就非常容易扩展。也无形之间，在架构的层面上带来了可扩展的能力。

-  大数据量，高性能   

      	   NoSQL数据库都具有非常高的读写性能，尤其在大数据量下，同样表现优秀。这得益于它的无关系性，数据库的结构简单。

-  灵活的数据模型  

  ​	NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式。而在关系数据库里，增删字段是一件非常麻烦的事情。如果是非常大数据量的表，增加字段简直就是一个噩梦。这点在大数据量的Web2.0时代尤其明显。


- 高可用  

  ​	NoSQL在不太影响性能的情况，就可以方便的实现高可用的架构。比如Cassandra，HBase模型，通过复制模型也能实现高可用。

### 4.小结

1. nosql： 非关系型数据库
2. 主要是学习redis（键值存储数据库）





## 知识点-Redis概述

### 1.目标

+ 知道什么是Redis以及Redis的应用场景

### 2.路径

+ 什么是Redis
+  redis的应用场景

### 3.讲解

#### 3.1.什么是Redis

​	Redis是用C语言开发的一个开源的高性能==键值对（key-value）数据库==，==数据是保存在内存里面的==. 官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下： 

- 字符串类型 string
- 散列类型 hash  
- 列表类型 list
- 集合类型 set   
- 有序集合类型 zset

#### 3.2 redis的应用场景

- ==缓存（数据查询、短连接、新闻内容、商品内容等等）==    
- ==任务队列。（秒杀、抢购、12306等等）==
- ==数据过期处理（可以精确到毫秒, 短信验证码)==        
- ==分布式集群架构中的session分离  session 服务器里面==
- 聊天室的在线好友列表 
- 应用排行榜
- 网站访问统计

### 4.小结

1. redis： 是一个开源的键值对存储的MoSQL数据库，数据是在内存中的
2. 场景：缓存、数据过期处理





## 实操-window版Redis的安装

### 1.目标

+ 掌握window版Redis的安装与使用

### 2.路径

1. Redis的下载

2. Redis的安装
3. 启动


### 3.讲解

#### 3.1.windows版Redis的下载

​	官方提倡使用Linux版的Redis，所以官网值提供了Linux版的Redis下载，我们可以从GitHub上下载window版的Redis，具体链接地址如下：

- 官网下载地址：<http://redis.io/download>


- github下载地址：https://github.com/MSOpenTech/redis/tags

![img](img/8.png)

在今天的课程资料中提供的下载完毕的window版本的Redis：

![img](img/2.png)

#### 3.2 Redis的安装

解压Redis压缩包后，见到如下目录机构：

| 目录或文件             | 作用                              |
| ---------------------- | --------------------------------- |
| redis-benchmark        | 性能测试工具                      |
| redis-check-aof        | AOF文件修复工具                   |
| redis-check-dump       | RDB文件检查工具（快照持久化文件） |
| ==redis-cli==          | 命令行客户端                      |
| ==redis-server==       | redis服务器启动命令               |
| ==redis.windows.conf== | redis核心配置文件                 |

#### 3.3 启动

![img](img/tu_10.png)

+ 安装:window版的安装及其简单，解压Redis压缩包完成即安装完毕
+ 启动与关闭: 双击Redis目录中redis-server.exe可以启动redis服务，Redis服务占用的端口是==6379==



![](![img](img/3.png)

+ 点击redis-cli



 ![img](img/4.png)) 



==启动时可能遇到的问题：==

```
D:\softwares\redis-2.8.9>redis-server.exe redis.windows.conf
VirtualAlloc/COWAlloc fail!
[38944] 07 Jul 20:07:55.647 # Out Of Memory allocating 16 bytes!
```

磁盘空间不足！！

- 可以换到更大的磁盘启动；

- 修改配置文件（redis.windows.conf），将启动的空间需求改小（字节为单位）

  - 启动方式变更，命令行进入你的redis目录，输入：  .\redis-server.exe  .\redis.windows.conf

  - 比如：
  
    ```
    maxheap 314572800
  maxmemory 268435456
    ```
  
    



### 4.小结

1. redis目录结构

![1560064246957](img/1560064246957.png) 

2. 启动
  
   + 启动redis-server.exe文件（服务器）
   + 点击redis-cli.exe





## 实操-Linux版本Redis的安装

### 1.目标

- [ ] 掌握Redis的安装

### 2.讲解

1. 在虚拟机中安装c++环境 ==该步骤可能需要联网！！！==

```bash
yum -y install gcc-c++

如果出现错误：  Error: database disk image is malformed
输入以下内容解决：
yum clean all
yum clean headers
yum clean packages
yum clean dbcache

yum -y install gcc-c++
```

2. 下载Redis
3. 上传到Linux
4. 解压

```
直接在上传后的所在目录/root目录解压即可
tar -zxf redis-4.0.14.tar.gz    
```

5. 编译

```shell
在root目录（redis上传后的所在的目录）
cd redis-4.0.14
make

# make时出现错误可以这样：  
make MALLOC=libc
再执行make
```

6.  安装

```
make install PREFIX=/usr/local/redis
```

7. 进入安装好的redis目录,复制配置文件

```
cd /usr/local/redis/bin
cp /root/redis-4.0.14/redis.conf ./
```

8. 修改配置文件redis.conf

```bash
# 修改配置文件
vi redis.conf
# Redis后台启动
修改 daemonize 为 yes
# Redis服务器可以跨网络访问 
修改 bind 为 0.0.0.0
# 开启aof持久化
appendonly yes
```

9. 启动redis

```bash
./redis-server redis.conf
```



### 3.小结

1. 对着笔记装一下, 装不成功了, 别装了 



## 实操-Redis的客户端安装

### 1.目标

​	装Redis的客户端

### 2.步骤

 ![image-20191223100124678](img/image-20191223100124678.png)

+ 双击, 下一步 ...
+ 安装没有中文和空格目录、不要是纯数字



# 第二章-Redis的数据类型

## 知识点-redis中数据结构/类型

### 1.目标

+ 能够说出Redis中的数据类型

### 2.路径

+ Redis的数据类型类型介绍
+ Redis中的key

### 3.讲解

#### 3.1Redis的数据类型【面试】

​	redis中存储的数据是以==key-value==的形式存在的.==其中value支持5种数据类型== .在日常开发中主要使用比较多的有字符串、哈希、列表list、集合set、有序集合zset这5种类型，其中最为常用的是字符串类型。   

​	字符串(string)           

​	哈希(hash)     		  		类似HashMap

​	列表(list)           				是一个双向链表

​	集合(set) 	                     类似HashSet		      

​	有序的集合(zset)       

#### 3.2key

- key不要太长(不能>1024个字节)

- 也不要太短 .  可读性差.   

  ```
  项目名_模块_key  eg: jd_user_name
  ```


### 4.小结 

1. 数据类型
   1. string
   2. hash
   3. list
   4. set
   5. zset
## 知识点-Redis字符串(string)

### 1.目标

+ 能够使用redis的string操作命令

### 2.路径

1. Redis字符串(String)概述
2. 应用场景
3. 常见命令
4. 应用举例

### 2.讲解

#### 2.1概述

​	string是redis最基本的类型,用的也是最多的，一个key对应一个value。 一个键最大能存储512MB. 

#### 2.2应用场景

1. 缓存功能：字符串最经典的使用场景，redis最为缓存层，Mysql作为储存层，绝大部分请求数据都是在redis中操作，由于redis具有支撑高并发特性，所以缓存通常能起到加速读写和降低 后端压力的作用。 

2. 计数器功能：比如视频播放次数，点赞次数。

3. ID递增

  

#### 2.3常见命令

| 命令                    | 描述                                                         |
| ----------------------- | :----------------------------------------------------------- |
| ==SET key value==       | 设置指定 key 的值                                            |
| ==GET key==             | 获取指定 key 的值（如果key不存在则返回空nil）                |
| ==DEL key==             | 删除key                                                      |
| GETSET key value        | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。   |
| SETEX key seconds value | 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| ==SETNX key value==     | 只有在 key 不存在时设置 key 的值，如果key存在则不设置。      |
| ==INCR key==            | 将 key 中储存的数字值增一。                                  |
| INCRBY key increment    | 将 key 所储存的值加上给定的增量值（increment） 。            |
| ==DECR key==            | 将 key 中储存的数字值减一。                                  |
| DECRBY key decrement    | key 所储存的值减去给定的减量值（decrement） 。               |

#### 2.4应用举例

商品编号、订单号采用string的递增数字特性生成。

```shell
定义商品编号key：       items:id
192.168.101.3:7003> INCR items:id
(integer) 2
192.168.101.3:7003> INCR items:id
(integer) 3
```



### 4.小结

#### 4.1String

1. 常用命令：
   1. 设置值：  set key value
   2. 获取值： get key
   3. 自增： incr key
   4. 自减： decr key

#### 4.2使用string的问题

​	假设有User对象以JSON序列化的形式存储到Redis中，User对象有id，username、password、age、name等属性，存储的过程如下： 保存、更新：  User对象 ==> json(string) ==>redis 

​	如果在业务上只是更新age属性，其他的属性并不做更新我应该怎么做呢？ 如果仍然采用上边的方法在传输、处理时会造成资源浪费，下边讲的hash可以很好的解决这个问题



## 知识点-Redis 哈希(Hash)

### 1.目标

+ 能够使用redis的hash操作命令

### 2.路径

1. Redis 哈希(Hash)概述
2. 应用场景
3. 常见命令
4. 应用举例

### 3.讲解

#### 3.1 概述

​	Redis中hash 是一个键值对集合，类似于java中的hashMap。 



​	==Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象==。

​	Redis存储hash可以看成是String key 和String value的map容器.  也就是说把值看成map集合.

​	 它特别适合存储对象相比较而言，将一个对象类型存储在Hash类型里要比存储在String类型里占用更少的内存空间并方便存取整个对象 

![image-20191223104457481](img/image-20191223104457481.png) 

```
	key				value
	u1			name	zs
				age		18
				
	u2			name	ls
    			age		19
    			
    			Map<String,Map>
```



#### 3.2应用场景

​	  用一个对象来存储用户信息，商品信息，订单信息等等。 

#### 3.3常见命令

| 命令                                            | 命令描述                                                     |
| :---------------------------------------------- | ------------------------------------------------------------ |
| ==hset key filed value==                        | 将哈希表 key 中的字段 field 的值设为 value：hset user name zsf意味着存了一个key叫做user，他有1个字段name值为zsf |
| ==hmset key field1 value1 [field2  value2]...== | 同时将多个 field-value (字段-值)对设置到哈希表 key 中        |
| ==hget key filed==                              | 获取存储在哈希表中指定字段的值                               |
| ==hmget key filed1 filed2==                     | 获取多个给定字段的值                                         |
| hdel key filed1 [filed2]                        | 删除一个或多个哈希表字段                                     |
| hlen key                                        | 获取哈希表中字段的数量                                       |
| del key                                         | 删除整个hash(对象)                                           |
| HGETALL key                                     | 获取在哈希表中指定 key 的所有字段和值                        |
| HKEYS key                                       | 获取所有哈希表中的字段                                       |
| HVALS key                                       | 获取哈希表中所有值                                           |

#### 3.4应用举例

存储商品信息

+ 商品字段【商品id、商品名称、商品价格】
+ 定义商品信息的key, 商品1001的信息在 Redis中的key为：items:1001
+ 存储商品信息

```
HMSET items:1001 id 3 name apple price 999.9 
```

### 4.小结

1. 如果key的value是一个hash类型，哈希本质又是一个key-value对格式；适合存储对象（字段-字段值）
2. 命令
   1. 存储一个字段： hset key field 值
   2. 获取一个字段： hget key field
   3. 存储多个字段：hmset key field1 值1 field1 值2...
   4. 获取多个字段： hmget key field1 field2..
   5. 获取所有的key、value： hgetall key

## 知识点-Redis 列表(List)

### 1.目标

- [ ] 能够使用redis的list操作命令 

### 2.路径

1. List类型
2. Redis 列表(List)概述 
3. 应用场景
4. 常见命令
5. 应用举例

### 3.讲解

#### 3.1List类型

​	ArrayList使用数组方式存储数据，所以根据索引查询数据速度快，而新增或者删除元素时需要设计到位移操作，所以比较慢。 

​	 LinkedList使用双向链表方式存储数据，每个元素都记录前后元素的指针，所以插入、删除数据时只是更改前后元素的指针指向即可，速度非常快。然后通过下标查询元素时需要从头开始索引，所以比较慢，但是如果查询前几个元素或后几个元素速度比较快。

![img](img/wps9BB3.tmp.jpg) 

 

 

 ![img](img/wps9BB4.tmp.jpg)

#### 3.2概述

​	列表类型（list）可以存储一个`有序的字符串列表(链表)`，常用的操作是==向列表两端添加元素，或者获得列表的某一个片段。==
​	列表类型内部是使用==双向链表==（double linked list）实现的，所以向列表两端添加元素的时间复杂度为O(1)，获取越接近两端的元素速度就越快。这意味着即使是一个有几千万个元素的列表，获取头部或尾部的10条记录也是极快的。



#### 3.2应用场景

​    如好友列表，粉丝列表，消息队列，最新消息排行等。

​	rpush方法就相当于将消息放入到队列中，lpop/rpop就相当于从队列中拿去消息进行消费 



==常用方式：==

- lpush左插结合rpop右取、或者rpush右插结合lpop左取-----作为一个队列来使用
- llen 查看元素个数

#### 3.3.常见命令

| 命令                                   | 命令描述                                                     |
| -------------------------------------- | ------------------------------------------------------------ |
| ==lpush key value1 value2...==         | 将一个或多个值插入到列表头部(左边)                           |
| ==brpop key 超时时间秒==               | 阻塞从右边获取，直到拿到数据或者超时； 0表示一直阻塞等待数据。 |
| rpush key value1 value2...             | 在列表中添加一个或多个值(右边)                               |
| lpop key                               | 左边弹出一个 相当于移除第一个                                |
| ==rpop key==                           | 右边弹出一个  相当于移除最后一个                             |
| llen key                               | 返回指定key所对应的list中元素个数                            |
| LINDEX key index                       | 通过索引获取列表中的元素                                     |
| LINSERT key BEFORE\| AFTER pivot value | 在列表的元素前或者后插入元素                                 |

#### 3.4应用举例

商品评论列表

+ 思路: 在Redis中创建商品评论列表,用户发布商品评论，将评论信息转成json存储到list中。用户在页面查询评论列表，从redis中取出json数据展示到页面。



### 4.小结

1. list的API

   1. 左插： lpush key val1 val2..

   2. 右取： rpop key

   3. 阻塞取： brpop key 超时时间(0一直等待直到拿到值)

      











## 知识点-Redis 集合(Set)

### 1.目标

+ 能够使用redis的set操作命令

### 2.路径

1. Redis 集合(Set)概述
2. 应用场景
3. 常见命令
4. 应用举例

### 3.讲解

#### 3.1概述

​	Redis的Set是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

​	Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的时间复杂度都是O(1)。集合中最大的成员数为 2的32次方 -1 (4294967295, 每个集合可存储40多亿个成员)。

​	Redis还提供了多个集合之间的交集、并集、差集的运算

​	==set类型特点:无序+唯一== 

#### 3.2应用场景

​	投票记录  

​	共同好友、共同兴趣、分类标签

#### 3.3.常见命令

| 命令                           | 命令描述                       |
| ------------------------------ | ------------------------------ |
| ==sadd key member1 [member2]== | 向集合添加一个或多个成员       |
| ==srem key member1 [member2]== | 移除一个成员或者多个成员       |
| smembers key                   | 返回集合中的所有成员,查看所有  |
| SCARD key                      | 获取集合的成员数               |
| SPOP key                       | 移除并返回集合中的一个随机元素 |
| ==SDIFF key1 [key2]==          | 返回给定所有集合的差集         |
| SUNION key1 [key2]             | 返回所有给定集合的并集         |
| SINTER key1 [key2]             | 返回给定所有集合的交集         |

#### 3.4应用举例

共同好友

+ A的好友
+ B的好友
+ A和B的共同好友

### 4.小结

1. set： 唯一 无序
2. 命令
   1. 设置： sadd key member1 member2..
   2. 取值: srem key member1 member2..
   3. 获取所有： smembers key
   4. 集合相关操作：
      1. 并集： sunion key1 key2
      2. 差集： sdiff key1 key2
      3. 交集:    sinter key key2





## 知识点-Redis 有序集合(sorted set)

### 1.目标

+ 能够使用redis的zset操作命令

### 2.路径

1. sorted set介绍
2. sorted set 应用场景
3. sorted set 命令
4. sorted set 具体示例

### 3.讲解

#### 3.1概述

Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

==不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。==

==有序集合的成员是唯一的,但分数(score)却可以重复。==

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

特点: 有序(根据分数排序)+唯一

#### 3.2应用场景

 排行榜：例如视频网站需要对用户上传的视频做排行榜. 

#### 3.3.常见命令

| 命令                                         | 命令描述                                                     |
| -------------------------------------------- | ------------------------------------------------------------ |
| ==ZADD key score member [score member ...]== | 增加元素;                                                    |
| ==ZSCORE key member==                        | 获取元素的分数                                               |
| ==ZREM key member [member ...]==             | 删除元素                                                     |
| ZCARD key                                    | 获得集合中元素的数量                                         |
| ==ZRANGE key start stop [WITHSCORES]==       | 获得排名在某个范围的元素列表；start、stop是索引值0开始；  或者负数最后一个是-1，第一个是-N |
| ZINCRBY key 1 member                         | 可以给指定元素的分数增加1                                    |

#### 3.4应用举例

商品销售量排行榜

+ 需求：根据商品销售量对商品进行排行显示
+ 思路：定义商品销售排行榜（sorted set集合），Key为items:sellsort，分数为商品销售量
+ 实现

```
--商品编号1001的销量是9，商品编号1002的销量是10
ZADD items:sellsort 9 1001 10 1002		

--商品编号1001的销量加1
ZINCRBY items:sellsort 1 1001

--商品销量前10名（从后到前---从大到小---销量高在前--商品销售排行榜）
ZRANGE items:sellsort -1 -9 
```



### 4.小结

1. zset：有序集合，根据分数来排序的

2. 存储：

   1. zadd key score member...
   
   

# 第三章-Redis通用的操作,发布订阅和持久化

## 知识点-Redis通用的操作 

### 1.目标

+ 掌握Redis通用的操作命令

### 2.路径

+ 通用操作
+ 多数据库性

### 3.讲解

#### 3.1 通用操作

- keys *: 查询所有的key（如果系统的key很多建议不要使用该命令）

- exists key:判断是否有指定的key 若有返回1,否则返回0

- ==expire key 秒数==:设置这个key的存活时间      

- ttl key:获取指定key的剩余时间

  ​	若返回值为 -1:永不过期

  ​	若返回值为 -2:已过期或者不存在

- del key:删除指定key

- rename key 新key:重命名

- type key:判断一个key的类型

- ping :测试连接是否连接，正常会返回pong

#### 3.2多数据库性

​	redis默认是16个数据库, 编号是从0~15. 【默认是0号库】

- select index:切换库，  比如 select 12  表示切换到编号为12数据库
- move key index: 把key移动到几号库(index是库的编号)
- flushdb:清空当前数据库
- flushall:清空当前实例下所有的数据库

### 4.小结

1. 设置失效时间：  expire key 秒值
2. 判断类型： type key
3. 切换数据库： select 索引值



## 知识点-订阅发布机制【了解】

### 1.目标

- [ ] 了解Redis订阅发布机制

### 2.路径

1. 什么是Redis订阅发布机制
2. 相关的命令
3. 订阅发布实操

### 3.讲解

#### 3.1什么是Redis订阅发布机制

​	Redis 发布订阅(pub/sub)是进程间一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

​	 Redis 客户端可以订阅任意数量的主题。

#### 3.2相关的命令

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | PUBLISH 主题名  message ：将信息发送到指定的主题。           |
| 2    | SUBSCRIBE 主题名[主题名 ...]  ：订阅给定的一个或多个主题的信息。 |
| 3    | UNSUBSCRIBE [主题名[主题名...]] 指退订给定的主题             |

#### 3.3订阅发布实操

+ 开启3个客户端
+ A客户端（订阅）

```
订阅nba的内容,等待发布消息.....
SUBSCRIBE nba
```

+ B客户端（订阅）

```
SUBSCRIBE nba
```

- C客户端【发布】

```
发布nba的内容
PUBLISH  nba 内容
```



### 4.小结

1. Redis 发布订阅(pub/sub)是进程间一种消息通信模式, 工作里面一般使用MQ



## 知识点-Redis的持久化【面试】

### 1.目标

​	Redis的高性能是由于其将所有数据都存储在了内存中，为了使Redis在重启之后仍能保证数据不丢失，需要将数据==从内存中同步到硬盘(文件)中，这一过程就是持久化==。

​	Redis支持两种方式的持久化，一种是RDB方式，一种是AOF方式。可以单独使用其中一种或将二者结合使用。

### 2.路径

+ RDB持久化机制
+ AOF持久化机制
+ 两种持久化机制比较

### 3.讲解

#### 3.1RDB持久化机制

##### 3.1.1概述

​	RDB持久化是指在==指定的时间间隔内==将内存中的数据集快照写入磁盘。这种方式是就是将内存中数据以快照的方式写入到二进制文件中,默认的文件名为dump.rdb。 这种方式是默认==已经开启了,不需要配置.==

##### 3.1.2 RDB持久化机制的配置

- 在redis.windows.conf配置文件中有如下配置：

 ![img](img/tu_14-1571712041246.png)

其中，上面配置的是RDB方式数据持久化时机：

| 关键字 | 时间(秒) | key修改数量 | 解释                                                  |
| ------ | -------- | ----------- | ----------------------------------------------------- |
| save   | 900      | 1           | 每900秒(15分钟)至少有1个key发生变化，则dump内存快照   |
| save   | 300      | 10          | 每300秒(5分钟)至少有10个key发生变化，则dump内存快照   |
| save   | 60       | 10000       | 每60秒(1分钟)至少有10000个key发生变化，则dump内存快照 |

==修改配置文件的上述的值==比如改为：

save 900 1

save 300 10

save 10 1

这样10秒内只要有一个key发生变化，就会看到有一个dump.rdb文件生成。



#### 3.2 AOF持久化机制

##### 3.2.1概述

​	 AOF持久化机制会将每一个收到的写命令都通过write函数追加到文件中,默认的文件名是appendonly.aof。 这种方式默认是没有开启的,要使用时候需要配置.

##### 3.2.2AOF持久化机制配置

###### 3.2.2.1开启配置

- 在redis.windows.conf配置文件中有如下配置：

 ![img](img/tu_15-1571712041246.png)

- ==需要将appendonly修改为yes， 然后redis-server.exe  redis.windows.conf 指定配置文件方式启动==， 但是启动redis的时候需要指定该文件,也就是意味着不能直接点击了, 需要输入命令启动:

 ![img](img/tu_12-1571712041245.png)

- 开启aof持久化机制后，默认会在目录下产生一个appendonly.aof文件

 ![img](img/tu_13-1571712041246.png)



###### 3.2.2.2配置详解

- 上述配置为aof持久化的时机，解释如下：(在redis.windows.conf配置)

 ![img](img/tu_16-1571712041246.png)

| 关键字      | 持久化时机 | 解释                           |
| ----------- | ---------- | ------------------------------ |
| appendfsync | always     | 每执行一次更新命令，持久化一次 |
| appendfsync | everysec   | 每秒钟持久化一次               |
| appendfsync | no         | 不持久化                       |

### 4,小结

#### 4.1RDB

##### 优点

- RDB 是一个非常紧凑（compact）的文件，它保存了 Redis 在某个时间点上的数据集。 这种文件非常适合用于进行备份
- ==RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快(因为其文件要比AOF的小)==
- ==RDB的性能要比AOF更好==

##### 缺点

- ==RDB的持久化不够及时(一定时间间隔),可能会存在数据丢失==
- RDB持久化时如果文件过大可能会造成服务器的阻塞,停止客户端请求

#### 4.2AOF

##### 优点

- ==AOF的持久性更加的耐久(可以每秒 或 每次操作保存一次)==
- AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。
- AOF是增量操作

##### 缺点

- ==对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积==
- ==根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB.==

#### 4.3选择

- 如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失，选择RDB 持久化。
- 如果对数据的完整性要求比较高, 选择AOF  

# 第四章-Jedis  

## 案例-Jedis的快速入门

### 1.目标

- [ ] 掌握jedis的使用步骤

### 2.路径

1. jedis的介绍

2. Jedis的入门

### 3.讲解

#### 3.1 jedis的介绍

​	Redis不仅是使用命令来操作，现在基本上主流的语言都有客户端支持，比如java、C、C#、C++、php、Node.js、Go等。 在官方网站里列一些Java的客户端，有Jedis、Redisson、Jredis、JDBC-Redis、等其中官方推荐使用Jedis和Redisson。 在企业中用的最多的就是Jedis，Jedis同样也是托管在github上.

==说白了Jedis就是使用Java操作Redis的客户端(工具包)==  

地址：https://github.com/xetorthio/jedis。

文档地址:http://xetorthio.github.io/jedis/

| 方法                                                   | 解释                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| ==创建new Jedis(host, port)==<br>关闭==jedis.close()== | 创建jedis对象，参数host是redis服务器地址，参数port是redis服务端口 |
| set(key,value)                                         | 设置字符串类型的数据                                         |
| get(key)                                               | 获得字符串类型的数据                                         |
| hset(key,field,value)                                  | 设置哈希类型的数据                                           |
| hget(key,field)                                        | 获得哈希类型的数据                                           |
| lpush(key,values)                                      | 设置列表类型的数据                                           |
| lpop(key)                                              | 列表左面弹栈                                                 |
| rpop(key)                                              | 列表右面弹栈                                                 |
| sadd(String key, String... members)                    | 设置set类型的数据                                            |
| zrange(String key, long start, long end)               | 获得在某个范围的元素列表                                     |
| del(key)                                               | 删除key                                                      |

#### 3.2 Jedis的入门

需求: 使用java代码操作Redis 进行操作

步骤:

1. ==导入jar==
2. 创建一个Jedis对象： new Jedis(host, port)
3. 操作，IDEA提示即可
4. 关闭资源  jedis.close()



- 基本操作

```java
package com.itheima.test;

import org.junit.Test;
import redis.clients.jedis.Jedis;

public class JedisTest {

    @Test
    public void test(){
        //1. 创建Jedis对象
        Jedis jedis = new Jedis("localhost", 6379);

        //2. 操作

        // string
        jedis.set("akey", "avalue");
        System.out.println(jedis.get("akey"));
        System.out.println(jedis.get("bkey"));

        // hash
        jedis.hset("user", "name", "zsf");

        // list
        jedis.lpush("lkey", "a", "b");

        //set
        jedis.sadd("skey", "a", "b", "c");

        //zset
        jedis.zadd("zkey", 98.5, "zs");


        //3. 关闭资源
        jedis.close();
    }
}

```

### 4.小结

1. jedis操作步骤
   1. 引入jar包（2个）
   2. 创建Jedis对象： new Jedis(主机host,post)
   3. 操作
   4. 关闭资源： jedis.close()









## 知识点-Jedis进阶

### 1.目标

- [ ] 掌握Jedis连接池的使用

### 2.路径

1.  jedis连接池介绍
2.  jedis连接池的使用
3.  工具类的抽取

### 3.讲解

#### 3.1 jedis连接池的基本概念

​	jedis连接资源的创建与销毁是很消耗程序性能，所以jedis为我们提供了jedis的池化技术，jedisPool在创建时初始化一些连接资源存储到连接池中，使用jedis连接资源时不需要创建，而是从连接池中获取一个资源进行redis的操作，使用完毕后，不需要销毁该jedis连接资源，而是将该资源归还给连接池，供其他请求使用。

#### 3.2jedis连接池的使用

需求: 从Jedis的连接池里面获得jedis

步骤:

1. ==创建JedisPool配置对象==  JedisPoolConfig poolConfig = new JedisPoolConfig();
2. ==创建JedisPool对象==   JedisPool jedisPool = new JedisPool(poolConfig, host, port);
3. ==从JedisPool获得jedis==     Jedis jedis = jedisPool.getResource();
4. 操作Redis
5. 释放资源                        



- 基本使用

```java
@Test
    public void test(){
        // 配置对象
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();


        //1.创建连接池
        JedisPool pool = new JedisPool(jedisPoolConfig, "localhost", 6379);

        //2. 获取连接jedis
        Jedis jedis = pool.getResource();

        //3. 操作
        System.out.println(jedis.get("ak47"));

        //4. 关闭
        jedis.close();

    }
```

#### 3.3Jedis工具类的抽取

目的: 池子--一个项目一个就可以了-----池子对象可以作为一个静态的变量。

如果其他类要使用这个池子-----工具类  JedisUtil类。



工具类抽取：

- 创建连接池的代码放入工具类
- 提供获取连接的方法
- 提供关闭资源的方法









- JedisUtil.java

```java
package com.itheima.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

/**'
 * 1 创建连接池的代码放入工具类
 * 2 提供获取连接的方法
 * 3 提供关闭资源的方法
 */
public class JedisUtil {

    // 配置对象
    private static JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();


    //1.创建连接池
    private static JedisPool pool = new JedisPool(jedisPoolConfig, "localhost", 6379);


    // 2 提供获取连接的方法
    public static Jedis getConn() {
        Jedis jedis = pool.getResource();
        return jedis;

    }


    //3 提供关闭资源的方法
    public static void close(Jedis jedis){
        jedis.close();
    }

}

```

#### 3.4.3 作业

>  把JedisUtils里面 host和port抽取到 jedis.properties里面

### 4.小结

1. 使用JedisPool目的: 为了jedis复用
2. 步骤
   1. 创建连接池对象： pool = new JedisPool(new JedisPoolConfig(), host, port)
   2. 从池子获取连接： jedis = pool.getResource()
   3. 操作
   4. 关闭资源




## 案例-使用Redis优化省份的展示

### 1.需求

​	访问index.html页面，使用ajax请求加载省份列表(响应json)

+ 先从Redis里面获得
  + 有 就直接返回
  + 没有 从Mysql获得,再存到Redis

![1571210295308](img/1571210295308.png) 

### 2.分析  

#### 2.1 思路

- ![1600673944179](img/1600673944179.png)

### 3.代码实现

#### 3.1准备工作

- 数据库

```mysql
create database day34;
use day34;
CREATE TABLE `province` (
  `pid` int NOT NULL AUTO_INCREMENT,
  `pname` varchar(40) DEFAULT NULL,
  PRIMARY KEY (`pid`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

INSERT INTO `province` VALUES ('1', '广东');
INSERT INTO `province` VALUES ('2', '湖北');
INSERT INTO `province` VALUES ('3', '湖南');
INSERT INTO `province` VALUES ('4', '四川');
INSERT INTO `province` VALUES ('5', '山东');
INSERT INTO `province` VALUES ('6', '山西');
INSERT INTO `province` VALUES ('7', '广西');
```

- 创建工程(web)
- 导入jar包, 导入配置文件, 导入工具类,导入页面
- Province.java

```java
public class Province {
	
	private Integer pid;
	private String pname;
	public Integer getPid() {
		return pid;
	}
	public void setPid(Integer pid) {
		this.pid = pid;
	}
	public String getPname() {
		return pname;
	}
	public void setPname(String pname) {
		this.pname = pname;
	}
	@Override
	public String toString() {
		return "Province [pid=" + pid + ", pname=" + pname + "]";
	}
}
```

#### 3.2代码实现

+ 页面index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vuejs-2.5.16.js"></script>
    <script src="js/axios-0.18.0.js"></script>
</head>
<body>
    <div id="app">
        <select>
            <option>选择省份</option>
            <option v-for="(ele, idx) in provinces">
                {{ele.pname}}
            </option>
        </select>
    </div>
</body>

<script>
    new Vue({
        el:"#app",
        data:{
            provinces: []
        },
        methods:{

        },
        created: function () {
            //1.  一访问页面就看到数据（在页面初始化时获得数据--vue的created函数）
            //2. 发送axios的请求获取省份列表数据
            axios.get("provinceServlet").then(response=>{

                JSON.stringify("response:"+response);


                if(response.data.flag){
                    // 成功获得了省份数据
                    //3. 遍历（v-for技术遍历显示<option v-for>）
                    this.provinces = response.data.result;
                }else {
                    alert(response.data.message);
                }
            })

        }
    })
</script>
</html>
```



+ ProvinceServlet

```java
package com.itheima.web;

import com.itheima.bean.Province;
import com.itheima.bean.Result;
import com.itheima.service.ProvinceService;
import com.itheima.util.JsonUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet("/provinceServlet")
public class ProvinceServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            // 写逻辑
            //1. 获得参数

            //2. 调用业务层，省份列表List
            ProvinceService provinceService = new ProvinceService();
            List<Province> list =  provinceService.findAll();


            //3. 响应
            //3.1 封装一个Result
            Result result = new Result(true, "成功", list);
            //3.2 返回json格式（JsonUtils）
            JsonUtils.printResult(response, result);
        } catch (Exception e) {
            e.printStackTrace();
            Result result = new Result(false, "系统繁忙");
            JsonUtils.printResult(response, result);
        }
    }
}

```

+ ProvinceService

```java
package com.itheima.service;

import com.alibaba.fastjson.JSON;
import com.itheima.bean.Province;
import com.itheima.dao.ProvinceDao;
import com.itheima.util.JedisUtil;
import redis.clients.jedis.Jedis;

import java.util.List;

public class ProvinceService {
    public List<Province> findAll() throws Exception {

        Jedis jedis = null;
        try {
            //1. 处理业务，返回一个省份列表

            //2. 判断redis中有没有缓存数据


            //2.1 从redis中获取数据【存储json格式到redis中】
            jedis = JedisUtil.getConn();
            String proviceData = jedis.get("provinceKey");


            //2.2 有，则直接返回
            if (proviceData != null) {
                // 把json格式串转成List
                return JSON.parseObject(proviceData, List.class);
            } else {
                //2.3 没有，调用dao层，存到redis，再返回
                ProvinceDao provinceDao = new ProvinceDao();
                List<Province> list = provinceDao.findAll();

                jedis.set("provinceKey", JSON.toJSONString(list));

                return list;
            }
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        } finally {
            JedisUtil.close(jedis);
        }
    }
}

```

+ ProvinceDao

```java
package com.itheima.dao;

import com.itheima.bean.Province;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.util.List;

public class ProvinceDao {
    public List<Province> findAll() throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());
        List<Province> query = queryRunner.query("select * from province", new BeanListHandler<>(Province.class));

        return query;
    }
}

```



### 4.小结

1. 使用redis优化查询方式获取数据
   1. 业务层
      1. 先从redis获取数据，拿到了则直接返回
      2. 没拿到，再去mysql中查询，存储到redis中，再返回



## 案例-使用Redis完成邮箱验证码的校验

### 1.需求

![1571727394934](img/1571727394934.png)  



+ 输入邮箱, 注册成功时发送激活邮件, 完成校验、激活
+ 把激活码存24小时

### 2.分析

#### 2.1思路

1. ![1600673846223](img/1600673846223.png)
2. ![1600673870405](img/1600673870405.png)



### 3.代码实现



环境准备：

```mysql
CREATE TABLE USER(
	id INT PRIMARY KEY AUTO_INCREMENT,
	username VARCHAR(20),
	PASSWORD VARCHAR(20),
	nickname VARCHAR(20),
	address VARCHAR(20),
	email VARCHAR(20),
	sex VARCHAR(20),
    # 注册时写0， 激活成功设置为1即可
    status int   
);


```

bean：

```java
public class User implements Serializable {
    private Integer id;
    private String username;
    private String password;
    private String nickname;
    private String address;
    private String email;
    private String sex;
    private Integer status;
}
```



页面：

register.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<center>
    <h1>用户注册</h1>
    <form action="userServlet?method=register" method="post">
        姓名:<input type="text" name="username"><br>
        密码:<input type="password" name="password"><br>
        昵称:<input type="text" name="nickname"><br>
        地址:<input type="text" name="address"><br>
        邮箱:<input type="text" name="email" /><br/>
        性别:<input type="radio" name="sex" value="female">女
        <input type="radio" name="sex" value="male">男<br>
        <input type="submit" value="注册">
    </form>
</center>
</body>
</html>

```



#### 3.1发送邮件代码

**注册**

+ UserServlet.java

```java
package com.itheima.web;

import com.itheima.bean.User;
import com.itheima.service.UserService;
import com.itheima.util.JedisUtil;
import org.apache.commons.beanutils.BeanUtils;
import redis.clients.jedis.Jedis;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Map;

@WebServlet("/userServlet")
public class UserServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 处理乱码
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");

        try {
            String method = request.getParameter("method");
            // 反射三板斧
            Class clazz = this.getClass();
            Method targetMethod =
                    clazz.getMethod(method, HttpServletRequest.class, HttpServletResponse.class);

            targetMethod.invoke(this, request, response);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    /**
     * 注册
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void register(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            //2.1 获取参数（用户信息--封装到javabean中User对象）
            Map<String, String[]> parameterMap = request.getParameterMap();

            User user = new User();
            BeanUtils.populate(user, parameterMap);

            //2.2 调用业务层，返回一个布尔值
            UserService userService = new UserService();
            boolean flag = userService.register(user);
            //2.3 响应，注册成功还是失败
            if(flag){
                response.getWriter().write("注册成功");
            }else{
                response.getWriter().write("注册失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
            response.getWriter().write("系统繁忙");
        }
    }

    /**
     * 激活
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void active(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        Jedis jedis = null;
        try {
            //3.1 判断激活码有没有失效（码还在不在）
            String username = request.getParameter("username");
            String code = request.getParameter("code");

            jedis = JedisUtil.getConn();
            // 判断 码是否存在（key是否还在redis中）
            if (jedis.exists(username + "#" + code)) {
                // //3.2 码还在，则可以激活（调用业务层）
                UserService userService = new UserService();
                userService.active(username);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JedisUtil.close(jedis);
        }
    }
}

```

- UerService

```java
package com.itheima.service;

import com.itheima.bean.User;
import com.itheima.dao.UserDao;
import com.itheima.util.JedisUtil;
import com.itheima.util.MailUtil;
import redis.clients.jedis.Jedis;

import java.util.Random;

public class UserService {

    /**
     * 注册
     * @param user
     * @return
     */
    public boolean register(User user) throws Exception{
        Jedis jedis = null;
        try {
            //1.1 调用dao层进行插入，受影响行数
            UserDao userDao = new UserDao();
            int cnt = userDao.register(user);
            //1.2 插入成功才发邮件
            if (cnt > 0) {

                // code随机几个字符即可
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < 4; i++) {
                    sb.append(new Random().nextInt(10));
                }
                String code = sb.toString();

                //1.3 工具类发送邮件【发什么内容】

                //1.3.1 邮件内容是一个超链接（访问到web层的active方法、包含哪个用户的标志name、激活码）

                String content = "http://localhost:8080/day34/userServlet?method=active"
                        + "&username=" + user.getUsername() + "&code=" + code;



                MailUtil.sendMail(user.getEmail(), "<a href='" + content + "'>点击激活</a>");

                //1.3.2 需要保存激活码到redis中，设置超时时间为24小时
                jedis = JedisUtil.getConn();
                // 先存储key，才能设置超时时间：  zs#1345
                jedis.set(user.getUsername() + "#" + code, user.getUsername() + "#" + code);
                jedis.expire(user.getUsername() + "#" + code, 60 * 60 * 24);


                return true;
            } else {
                return false;
            }
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        } finally {
            JedisUtil.close(jedis);
        }

    }

    public void active(String username) throws Exception {
        UserDao userDao = new UserDao();
        userDao.active(username);
    }
}

```

- UserDao

```java
package com.itheima.dao;

import com.itheima.bean.User;
import com.itheima.util.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;

public class UserDao {
    public int register(User user) throws Exception{

        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());

        String sql = "insert into user values(null,?,?,?,?,?,?,?)";
        Object[] params = {
                user.getUsername(),
                user.getPassword(),
                user.getNickname(),
                user.getAddress(),
                user.getEmail(),
                user.getSex(),
                0
        };
        int cnt = queryRunner.update(sql, params);
        return cnt;
    }

    public void active(String username) throws Exception{
        QueryRunner queryRunner = new QueryRunner(C3P0Utils.getDataSource());
        String sql = "update user set status = 1 where username = ?";
        queryRunner.update(sql, username);
    }
}

```



### 4.小结

注册+邮箱激活

- 注册
  - 注册成功（插入的影响行数>0），才发送邮件
  - 发邮件
    - 内容包含：
      - 访问active方法的绝对路径-超链接
      - 哪个用户（username）
      - 码（随机4位数字码）
      - 存储时 ：  usermane#code，设置时间为24小时
- 激活
  - 获取了username、code
  - 判断key是否还存在，存在则修改status为1





# 总结

- 概念：
  - nosql： 泛指非关系型数据库
  - redis： 键值存储，基于内存的nosql数据库产品
- 数据类型（5种）
  - string：字符串
    - 存储值：set key value
    - 获取值：get key
  - hash：哈希，又是一个key-value结构（存储对象形式）
    - 存储单个值： hset key field val1
    - 获取单个值： hget key field
    - 存储多个值：hmset key field val1 field2 val2
    - 获取多个值：hmget key field field2 
  - list： 列表（双向列表）
    - 左插右取： lpush key val...、rpop key
    - 阻塞获取： brpop key 超时时间
  - set：集合，唯一、无序
    - 存储值： sadd key member
    - 移除值： srem key member
    - 集合运算： 交集、并集、差集
  - zset：有序集合，根据分数排序的
    - 存值： zadd key  score member
    - 范围获取数据： zrange key 0 -1： 获取所有元素
- 通用操作
  - 设置超时：   expire key 秒
  - 判断key是否存在： exists key
  - 数据库相关的： select 索引（0-15）
- 持久化
  - RDB
    - 二进制存储，效率高；时间间隔持久化导致数据会丢失风险
  - AOF
    - 文本形式存储，效率低； 实际间隔可控，持久化粒度较细。
- Jedis
  - java操作redis的工具
    - 引入jar
    - 创建Jedis
    - 操作
    - 关闭资源
  - 连接池jedisPool
    - 创建一个配置对象jedisPoolConfig
    - 创建jedisPool(jedisPoolConfig, host, port)
    - 从池子获取连接jedis：  pool.getResource()
    - 操作
    - 关闭资源





# 附录

redis命令行可以采用`help @数据类型`查看对应的数据类型的命令帮助：比如 help @string 可以查看string类型对应的所有的命令，以及帮助解释。

![1600655637190](img/1600655637190.png)





# 预习要点

maven

- 本地仓库是什么？
- maven的配置：   
- idea里面创建maven模块（java、webapp模块）





.m2文件夹发给你们：   没有中文、没有特殊字符

