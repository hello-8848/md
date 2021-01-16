# 第2天 ES & SpringData ES

## 学习目标

- 能够完成创建索引的操作
- 能够完成删除索引的操作
- 能够完成创建映射的操作
- 能够完成文档的增删改查
- 能够完成文档的分页操作
- ==能够完成文档的高亮查询操作==
- ==能够搭建Spring Data ElasticSearch的环境==
- ==能够完成Spring Data ElasticSearch的基本增删改查操作==
- ==能够掌握基本条件查询的方法命名规则==
- Elasticsearch集群



## 1 Elasticsearch编程操作

### 1.1 目标

### 1.2 讲解

#### 1.2.1 工程搭建

(1)搭建工程

我们首先搭建一个新的工程，坐标如下

```xml
<groupId>com.itheima</groupId>
<artifactId>elasticsearch-day2-demo1</artifactId>
<version>1.0-SNAPSHOT</version>
```

(2)pom.xml依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>elasticsearch-day2-demo1</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
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
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
</project>
```



(3)创建测试类`com.itheima.test.ElasticsearchTest`



#### 1.2.2 数据操作

##### 1.2.2.1 创建/删除索引

(1)创建索引

![1563655099836](images\1563655099836.png)



(2)删除索引

![1563655143937](images\1563655143937.png)



##### 1.2.2.2 创建映射

这里的映射表示创建索引类型结构，如果不创建映射，Elasticsearch会默认根据创建的文档中的数据用来创建映射。

查看之前的blog的索引

![1563655447905](images\1563655447905.png)

要想指定对每个字段是否使用分词器，需要手动创建映射。

![1563655353450](images\1563655353450.png)

使用代码创建映射，代码如下：

![1563655646698](images\1563655646698.png)

查看最新的blog3的索引

![1563655697447](images\1563655697447.png)



##### 1.2.2.3 创建索引数据

(1)通过XContentBuilder对象创建

![1563655841905](images\1563655841905.png)

代码如下：

```java
/***
 * 创建索引数据
 */
@Test
public void testPutIndexDataDemo4() throws Exception{
    //创建TransportClient，并设置不使用集群
    TransportClient client = new PreBuiltTransportClient(Settings.EMPTY);
    //设置IP和端口
    client.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"),9300));
    //通过XContentBuilder构建数据
    XContentBuilder builder = XContentFactory.jsonBuilder();
    builder.startObject();
    builder.field("id","1");
    builder.field("title","ElasticSearch是一个基于Lucene的搜索服务器,深圳黑马训练营！");
    builder.field("content","它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。");
    builder.endObject();
    //使用TransportClient对象增加数据
    client.prepareIndex("blog3","article","1").setSource(builder).get();
    //关闭资源
    client.close();
}
```

查看数据如下：

![1563655951303](images\1563655951303.png)



(2)使用Jackson转换实体

a.创建Article

创建`com.itheima.domain.Article`，代码如下：

```java
public class Article implements Serializable {

    private Integer id;
    private String title;
    private String content;
    
    //..get..set..toString
}
```

b.引入坐标

Jackson的包，可以将Java对象转换成Json字符串；也可以将Json的字符串，转化成Java对象,引入如下坐标：

```xml
<!--jackson JSON转换包-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.8.1</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.8.1</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.8.1</version>
</dependency>
```

c.代码实现

![1563656313333](images\1563656313333.png)



##### 1.2.2.4 修改索引数据

(1)使用prepareUpdate修改

![1563656457824](images\1563656457824.png)



(2)直接使用update()修改

![1563656513220](images\1563656513220.png)



##### 1.2.2.5 删除索引数据

(1)通过prepareDelete 删除

![1563656572829](images\1563656572829.png)



(2)直接使用delete() 删除

![1563656616437](images\1563656616437.png)



##### 1.2.2.6 批量增加数据

![1563656724810](images\1563656724810.png)

数据如下：

![1563656782566](images\1563656782566.png)



#### 1.2.3 查询

##### 1.2.3.1 字符串查询

![1563656923105](images\1563656923105.png)



##### 1.2.3.2 词条查询

![1563657035062](images\1563657035062.png)



##### 1.2.3.3 jackson转换JavaBean

![1563657139997](images\1563657139997.png)



##### 1.2.3.4 多种查询方式实现

![1563657218206](images\1563657218206.png)



##### 1.2.3.5 组合查询

布尔查询

```properties
     must(QueryBuilders) : AND，求交集 且
     mustNot(QueryBuilders): NOT 非
     should(QueryBuilders):OR ，求并集 或
```

代码实现如下：

![1563657416474](images\1563657416474.png)

这里发现：

.must(QueryBuilders.*wildcardQuery*("content", "elastics*ch")) // 模糊查询

能搜索到到结果。

而使用

.must(QueryBuilders.*wildcardQuery*("content", "ElasticS*ch")) // 模糊查询

不能搜索到结果。为什么呢？

分词查询如下：

`<http://127.0.0.1:9200/_analyze?analyzer=ik_max_word&pretty=true&text=ElasticSearch是一个全文检索的框架>`

![1563657492854](images\1563657492854.png)

因为IK分词器，在建立索引的时候将英文都变成了小写，这样方便我们在搜索的时候可以实现“不区分大小写”的搜索，只要我们在搜索的条件中添加.toLowerCase()的方法即可。

例如：

.must(QueryBuilders.*wildcardQuery*("content", "ELASTics*ch".toLowerCase()))

将输入的值都先变成小写，再来搜索结果



##### 1.2.3.6 使用DSL表达式

在定义json：放置到Elasticsearch的HEAD插件（PostMan工具）中（DSL表达式），使用restful风格编程，传递消息体

使用head插件查看索引库的信息，进行查询：

![1563657667887](images\1563657667887.png)

JSON数据如下：

```json
{
  "query" : {
    "bool" : {
      "must" : {
        "term" : {
          "title" : "elasticsearch"
        }
      },
      "must" : {
        "range" : {
          "id" : {
            "from" : 5,
            "to" : 55
          }
        }
      }
    }
  }
}
```



##### 1.2.3.7 分页排序

![1563657773418](images\1563657773418.png)



#### 1.2.4 高亮查询

##### 1.2.4.1 什么是高亮

在进行关键字搜索时，搜索出的内容中的关键字会显示不同的颜色，称之为高亮

百度搜索关键字"传智播客"

![1563657971609](images\1563657971609.png)

高亮源码分析：

![1563658066656](images\1563658066656.png)



##### 1.2.4.2 高亮显示的html分析

通过开发者工具查看高亮数据的html代码实现：

ElasticSearch可以对查询出的内容中关键字部分进行标签和样式的设置，但是你需要告诉ElasticSearch使用什么标签对高亮关键字进行包裹呢？

经过测试：使用`<em>高亮内容</em>`



##### 1.2.4.3 高亮实现代码

![1563658212508](images\1563658212508.png)

输出结果如下：

![1563658266992](images\1563658266992.png)





## 2 SpringData Elasticsearch

### 2.1 目标

### 2.2 讲解

#### 2.2.1 SpringDataJpa介绍

JPA是一个规范，真正操作数据库的是Hibernate，而springdatajpa是对jpa的封装，将CRUD的方法封装到指定的方法中，操作的时候，只需要调用方法即可。

Spring Data Jpa的实现过程：

```properties
1：定义实体，实体类添加Jpa的注解
2：定义接口，接口要继承JpaRepository的接口
3：配置spring容器，applicationContext.xml
```



#### 2.2.2 Spring Data ElasticSearch简介

(1)SpringData介绍

Spring Data是一个用于简化数据库、非关系型数据库、索引库访问，并支持云服务的开源框架。其主要目标是使得对数据的访问变得方便快捷，并支持map-reduce框架和云计算数据服务。 Spring Data可以极大的简化JPA（Elasticsearch...）的写法，可以在几乎不用写实现的情况下，实现对数据的访问和操作。除了CRUD外，还包括如分页、排序等一些常用的功能。

Spring Data的官网：<http://projects.spring.io/spring-data/>

Spring Data常用的功能模块如下：

![1563658940128](images\1563658940128.png)

![1563658984464](images\1563658984464.png)



(2)SpringData Elasticsearch介绍

Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装 。Spring Data为Elasticsearch项目提供集成搜索引擎。Spring Data Elasticsearch POJO的关键功能区域为中心的模型与Elastichsearch交互文档和轻松地编写一个存储索引库数据访问层。

官方网站：<http://projects.spring.io/spring-data-elasticsearch/> 



#### 2.2.3 SpringData Elasticsearch入门

##### 2.2.3.1 搭建工程

(1)搭建工程

工程坐标如下：

```xml
<groupId>com.itheima</groupId>
<artifactId>elasticsearch-day2-demo2-springdata-es</artifactId>
<version>1.0-SNAPSHOT</version>
```



(2)pom.xml依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>elasticsearch-day2-demo2-springdata-es</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--依赖包-->
    <dependencies>
        <!--ES依赖包-->
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

        <!--日志依赖-->
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
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.8.RELEASE</version>
        </dependency>

        <!--springdata-es-->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-elasticsearch</artifactId>
            <version>3.0.7.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```



##### 2.2.3.2 增加索引数据

(1)编写实体类

创建`com.itheima.domain.Article`，代码如下：

```java
public class Article {

    private Integer id;
    private String title;
    private String content;
    //..set..get..toString   
}
```



(2)Dao创建

创建`com.itheima.es.dao.ArticleDao`，代码如下：

```java
public interface ArticleDao extends ElasticsearchRepository<Article,Integer> {
}
```



(3)创建Service

创建`com.itheima.service.ArticleService`接口，并添加一个保存数据方法，代码如下：

```java
public interface ArticleService {

    /***
     * 增加数据
     * @param article
     */
    void save(Article article);
}
```

创建`com.itheima.service.impl.ArticleServiceImpl`，并实现保存数据的方法，代码如下：

```java
@Service
public class ArticleServiceImpl implements ArticleService {

    @Autowired
    private ArticleDao articleDao;

    /***
     * 增加数据
     * @param article
     */
    @Override
    public void save(Article article) {
        articleDao.save(article);
    }
}
```



(4)配置映射

修改`com.itheima.domain.Article`配置索引映射关系，代码如下：

```java
@Document(indexName = "blog4",type = "article")
public class Article {

    //@Id 文档主键 唯一标识
    @Id
    private Integer id;
    @Field(index = true,analyzer = "ik_smart",store=false,searchAnalyzer="ik_smart",type = FieldType.Text)
    private String title;
    @Field(index = true,analyzer = "ik_smart",store=false,searchAnalyzer="ik_smart",type = FieldType.Text)
    private String content;
    
    //..get..set..toString
}
```

其中，注解解释如下：
@Document(indexName="blob4",type="article")：
    indexName：索引的名称（必填项）
    type：索引的类型
@Id：主键的唯一标识
@Field(index=true,analyzer="ik_smart",store=true,searchAnalyzer="ik_smart",type = FieldType.Text)
    index：是否设置分词
    analyzer：存储时使用的分词器
    searchAnalyze：搜索时使用的分词器
    store：是否存储，默认值是false。如果默认设置为false，Elasticsearch默认使用_source存放我们数据内容。
    type: 数据类型



(5)核心配置文件配置

在resources下创建`springdata-es.xml`，在配置文件中配置包扫描以及ESDao包扫描，`TransportClient`对象创建，`elasticsearchTemplate`对象创建，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:elasticsearch="http://www.springframework.org/schema/data/elasticsearch"
       xsi:schemaLocation="
      http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/data/elasticsearch
      http://www.springframework.org/schema/data/elasticsearch/spring-elasticsearch-1.0.xsd
      ">

    <!--包扫描-->
    <context:component-scan base-package="com.itheima.service" />

    <!--扫描Dao包，自动创建实例，扫描所有继承ElasticsearchRepository接口的接口-->
    <elasticsearch:repositories base-package="com.itheima.es.dao"/>

    <!--配置elasticSearch的连接对象Client-->
    <elasticsearch:transport-client id="client" cluster-nodes="localhost:9300" cluster-name="elasticsearch"/>

    <!--ElasticSearch模版对象（底层使用模板操作，需要用spring创建，并注入client）-->
    <bean id="elasticsearchTemplate" class="org.springframework.data.elasticsearch.core.ElasticsearchTemplate">
        <constructor-arg name="client" ref="client"></constructor-arg>
    </bean>
</beans>
```



(6)创建测试类

创建测试类`com.itheima.test.SpringDataEsTest`，代码如下：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:springdata-es.xml")
public class SpringDataEsTest {

    @Autowired
    private ArticleService articleService;

    @Autowired
    private ElasticsearchTemplate elasticsearchTemplate;

    /**
     * 创建索引和映射信息
     */
    @Test
    public void testCreateMapping(){
        elasticsearchTemplate.createIndex(Article.class);
        elasticsearchTemplate.putMapping(Article.class);
    }

    /**
     * 添加文档数据
     */
    @Test
    public void testSave(){
        Article article = new Article();
        article.setId(100);
        article.setTitle("测试SpringData ElasticSearch");
        article.setContent("Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装Spring Data为Elasticsearch Elasticsearch项目提供集成搜索引擎");
        articleService.save(article);
    }
}
```

查看elasticsearch-head，索引如下：

![1563659782306](images\1563659782306.png)

数据如下：

![1563659834127](images\1563659834127.png)



#### 2.2.4 SpringData Elasticsearch常用操作

使用SpringDataElasticsearch实现数据的增删改查操作。

##### 2.2.4.1 Service层

（1）Service接口

修改`com.itheima.service.ArticleService`，添加更多操作方法:

```java
public interface ArticleService {

    /***
     * 增加数据
     * @param article
     */
    void save(Article article);

    /**
     * 批量保存
     * @param articles
     */
    void saveAll(List<Article> articles);

    /**
     * 根据ID删除数据
     * @param article
     */
    void delete(Article article);

    /**
     * 查询所有
     * @return
     */
    Iterable<Article> findAll();

    /**
     * 分页查询
     * @param pageable：分页封装对象
     * @return
     */
    Page<Article> findAll(Pageable pageable);

}
```



(2)Service实现类

修改`com.itheima.service.impl.ArticleServiceImpl`实现上面添加的接口方法，代码如下：

```java
@Service
public class ArticleServiceImpl implements ArticleService {

    @Autowired
    private ArticleDao articleDao;

    /***
     * 增加数据
     * @param article
     */
    @Override
    public void save(Article article) {
        articleDao.save(article);
    }

    /**
     * 批量保存
     * @param articles
     */
    @Override
    public void saveAll(List<Article> articles) {
        articleDao.saveAll(articles);
    }

    /**
     * 根据ID删除数据
     * @param article
     */
    @Override
    public void delete(Article article) {
        articleDao.delete(article);
    }

    /**
     * 查询所有
     * @return
     */
    @Override
    public Iterable<Article> findAll() {
        return articleDao.findAll();
    }

    /**
     * 分页查询
     * @param pageable：分页封装对象
     * @return
     */
    @Override
    public Page<Article> findAll(Pageable pageable) {
        return articleDao.findAll(pageable);
    }
}
```



##### 2.2.4.2 测试

修改`com.itheima.test.SpringDataEsTest`，将刚才添加的方法一一测试：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:springdata-es.xml")
public class SpringDataEsTest {

    @Autowired
    private ArticleService articleService;

    @Autowired
    private ElasticsearchTemplate elasticsearchTemplate;

    /**
     * 创建索引和映射信息
     */
    @Test
    public void testCreateMapping(){
        elasticsearchTemplate.createIndex(Article.class);
        elasticsearchTemplate.putMapping(Article.class);
    }

    /**
     * 添加文档数据
     */
    @Test
    public void testSave(){
        Article article = new Article();
        article.setId(100);
        article.setTitle("测试SpringData ElasticSearch");
        article.setContent("Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装Spring Data为Elasticsearch Elasticsearch项目提供集成搜索引擎");
        articleService.save(article);
    }


    /**
     * 更新测试
     */
    @Test
    public void testUpdate(){
        Article article = new Article();
        article.setId(100);
        article.setTitle("elasticSearch 3.0版本发布...更新");
        article.setContent("ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
        articleService.save(article);
    }


    /**
     * 批量保存
     */
    @Test
    public void testSaveAll(){
        List<Article> articles = new ArrayList<Article>();
        for (int i = 0; i <100 ; i++) {
            Article article = new Article();
            article.setId(i+1);
            article.setTitle((i+1)+"elasticSearch 3.0版本发布...更新");
            article.setContent((i+1)+"ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
            articles.add(article);
        }
        articleService.saveAll(articles);
    }

    /***
     * 根据ID删除
     */
    @Test
    public void testDelete(){
        Article article = new Article();
        article.setId(100);
        articleService.delete(article);
    }

    /**
     * 查询所有
     */
    @Test
    public void testFindAll(){
        //查询所有
        Iterable<Article> articles = articleService.findAll();
        for (Article article : articles) {
            System.out.println(article);
        }
    }

    /**
     * 分页查询
     */
    @Test
    public void testFindAllPage(){
        //分页查询
        Page<Article> page = articleService.findAll(PageRequest.of(0, 3));
        //总记录数
        long totalElements = page.getTotalElements();
        //总页数
        int totalPages = page.getTotalPages();
        //获取数据
        List<Article> articles = page.getContent();
        for (Article article : articles) {
            System.out.println(article);
        }
    }
}
```



#### 2.2.5 常用查询命名规则

| **关键字**    | **命名规则**          | **解释**                   | **示例**              |
| ------------- | --------------------- | -------------------------- | --------------------- |
| and           | findByField1AndField2 | 根据Field1和Field2获得数据 | findByTitleAndContent |
| or            | findByField1OrField2  | 根据Field1或Field2获得数据 | findByTitleOrContent  |
| is            | findByField           | 根据Field获得数据          | findByTitle           |
| not           | findByFieldNot        | 根据Field获得补集数据      | findByTitleNot        |
| between       | findByFieldBetween    | 获得指定范围的数据         | findByPriceBetween    |
| lessThanEqual | findByFieldLessThan   | 获得小于等于指定值的数据   | findByPriceLessThan   |



##### 2.2.5.1 根据标题查询

(1)Dao层

修改`com.itheima.es.dao.ArticleDao`添加一个根据标题查询方法：

```java
/***
 * 根据标题查询
 * @param title
 * @return
 */
List<Article> findByTitle(String title);
```





(2)Service层

修改`com.itheima.service.ArticleService`，添加一个根据标题查询方法，代码如下：

```java
/***
 * 根据标题查询数据
 * @param title
 * @return
 */
List<Article> findByTitle(String title);
```

修改`com.itheima.service.impl.ArticleServiceImpl`，添加根据标题查询实现方法，代码如下：

```java
/***
 * 根据标题查询数据
 * @param title
 * @return
 */
@Override
public List<Article> findByTitle(String title) {
    return articleDao.findByTitle(title);
}
```

(3)测试

```java
/**
 * 根据标题查询
 */
@Test
public void testFindByTitle(){
    //查询所有
    List<Article> articles = articleService.findByTitle("版本");
    for (Article article : articles) {
        System.out.println(article);
    }
}
```

结果如下：

```properties
Article{id=19, title='19elasticSearch 3.0版本发布...更新', content='19ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口'}
Article{id=22, title='22elasticSearch 3.0版本发布...更新', content='22ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口'}
Article{id=24, title='24elasticSearch 3.0版本发布...更新', content='24ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口'}
Article{id=25, title='25elasticSearch 3.0版本发布...更新', content='25ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口'}
```



##### 2.2.5.2 根据标题分页查询

(1)Dao层

修改`com.itheima.es.dao.ArticleDao`添加一个根据标题分页查询方法：

```java
/***
 * 根据标题分页查询
 */
Page<Article> findByTitle(String title, Pageable pageable);
```





(2)Service层

修改`com.itheima.service.ArticleService`，添加一个根据标题分页查询方法，代码如下：

```java
/***
 * 根据标题分页查询数据
 * @param title
 * @param pageable
 * @return
 */
Page<Article> findByTitle(String title,Pageable pageable);
```

修改`com.itheima.service.impl.ArticleServiceImpl`，添加根据标题分页查询实现方法，代码如下：

```java
/***
 * 根据标题分页查询数据
 * @param title
 * @param pageable
 * @return
 */
@Override
public Page<Article> findByTitle(String title, Pageable pageable) {
    return articleDao.findByTitle(title,pageable);
}
```

(3)测试

```java
/**
 * 根据标题分页查询
 */
@Test
public void testFindByTitlePage(){
    //查询所有
    Page<Article> page = articleService.findByTitle("版本", PageRequest.of(0, 20));

    //获取集合数据
    List<Article> articles = page.getContent();
    for (Article article : articles) {
        System.out.println(article);
    }
}
```

结果如下：

![1563661052906](images\1563661052906.png)



## 3 Elasticsearch集群

### 3.1 集群介绍

(1)集群Cluster

一个集群就是由一个或多个节点组织在一起，它们共同持有整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是“elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群



(2)节点Node

一个节点是集群中的一个服务器，作为集群的一部分，它存储数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于Elasticsearch集群中的哪些节点。

一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此，它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。

在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何Elasticsearch节点，这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的集群。





(3)分片和主从复制

一个索引可以存储超出单个结点硬件限制的大量数据。比如，一个具有10亿文档的索引占据1TB的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。分片很重要，主要有两方面的原因： 1）允许你水平分割/扩展你的内容容量。 2）允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量。

至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由Elasticsearch管理的，对于作为用户的你来说，这些都是透明的。

在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。

复制之所以重要，有两个主要原因： 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行。总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。

默认情况下，Elasticsearch中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。



### 3.2 集群搭建

(1)准备3台ES

可以将之前的Elasticsearch拷贝3份，每次拷贝不需要拷贝data

![1563662981129](images\1563662981129.png)

拷贝三分后分别存储到node1,node2,node3中。

![1563662920984](images\1563662920984.png)



(2)修改配置文件

修改node1节点中的`node1\config\elasticsearch.yml`配置文件，添加如下配置：

```properties
#节点1的配置信息：
#集群名称，保证唯一
cluster.name: my-elasticsearch
#节点名称，必须不一样
node.name: node-1
#服务端口号，在同一机器下必须不一样
http.port: 9201
#集群间通信端口号，在同一机器下必须不一样
transport.tcp.port: 9301
#设置集群自动发现机器ip集合
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
```

修改node2节点中的`node2\config\elasticsearch.yml`配置文件，添加如下配置：

```properties
#节点2的配置信息：
#集群名称，保证唯一
cluster.name: my-elasticsearch
#节点名称，必须不一样
node.name: node-2
#服务端口号，在同一机器下必须不一样
http.port: 9202
#集群间通信端口号，在同一机器下必须不一样
transport.tcp.port: 9302
#设置集群自动发现机器ip集合
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
```

修改node3节点中的`node3\config\elasticsearch.yml`配置文件，添加如下配置：

```properties
#节点3的配置信息：
#集群名称，保证唯一
cluster.name: my-elasticsearch
#节点名称，必须不一样
node.name: node-3
#服务端口号，在同一机器下必须不一样
http.port: 9203
#集群间通信端口号，在同一机器下必须不一样
transport.tcp.port: 9303
#设置集群自动发现机器ip集合
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
```



### 3.3 SpringDataElasticsearch配置

创建配置文件springdata-cluster.xml，配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:elasticsearch="http://www.springframework.org/schema/data/elasticsearch"
       xsi:schemaLocation="
      http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/data/elasticsearch
      http://www.springframework.org/schema/data/elasticsearch/spring-elasticsearch-1.0.xsd
      ">

    <!--包扫描-->
    <context:component-scan base-package="com.itheima.service" />

    <!--扫描Dao包，自动创建实例，扫描所有继承ElasticsearchRepository接口的接口-->
    <elasticsearch:repositories base-package="com.itheima.es.dao"/>

    <!--配置elasticSearch的连接对象Client-->
    <elasticsearch:transport-client id="client" cluster-nodes="localhost:9301,localhost:9302,localhost:9303" cluster-name="my-elasticsearch"/>

    <!--ElasticSearch模版对象（底层使用模板操作，需要用spring创建，并注入client）-->
    <bean id="elasticsearchTemplate" class="org.springframework.data.elasticsearch.core.ElasticsearchTemplate">
        <constructor-arg name="client" ref="client"></constructor-arg>
    </bean>
</beans>
```

其他的都不变即可。



