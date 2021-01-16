# 第8章 图形报表、Excel报表

- 了解Echarts
- 掌握Echarts实现会员数量折线图的实现过程
- 掌握Echarts实现套餐预约占比饼形图的实现过程
- 掌握运营数据统计的实现过程
- 掌握运营数据统计报表导出的实现过程
- 掌握报表sql语句的编写步骤


# 1. 图形报表ECharts

### 【目标】

了解Echarts

### 【路径】

1：ECharts简介

2：5分钟上手ECharts

3：查看ECharts官方实例

### 【讲解】

## 1.1. ECharts简介

ECharts缩写来自Enterprise Charts，商业级数据图表，是百度的一个开源的使用JavaScript实现的数据可视化工具，可以流畅的运行在 PC 和移动设备上，兼容当前绝大部分浏览器（IE8/9/10/11，Chrome，Firefox，Safari等），底层依赖轻量级的矢量图形库 [ZRender](https://github.com/ecomfe/zrender)，提供直观、交互丰富、可高度个性化定制的数据可视化图表。

官网：<https://echarts.baidu.com/>

下载地址：<https://echarts.baidu.com/download.html>

![img](./img/010.png) 

下载完成可以得到如下文件：

![img](./img/011.png) 

解压上面的zip文件：

![img](./img/012.png) 

我们只需要将dist目录下的echarts.js文件（已经压缩）引入到页面上就可以使用了

![img](./img/013.png) 

## 1.2. **5分钟上手ECharts**

我们可以参考官方提供的5分钟上手ECharts文档感受一下ECharts的使用方式，地址如下：

[https://www.echartsjs.com/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts](#5 %E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B ECharts)

第一步：在health_web中创建html页面并引入echarts.js文件

![img](./img/014.png) 

echartsDemo.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<!-- 引入 ECharts 文件 -->
<script src="js/echarts.js"></script>
<body>

</body>
</html>
```

 

第二步：在页面中准备一个具备宽高的DOM容器。

```html
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

 

第三步：通过echarts.init方法初始化一个 echarts 实例并通过setOption方法生成一个简单的柱状图

```javascript
<script type="text/javascript">
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'));

    // 指定图表的配置项和数据
    var option = {
        title: {
            text: 'ECharts 入门示例'
        },
        tooltip: {},
        legend: {
            data:['销量']
        },
        xAxis: {
            data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
        },
        yAxis: {},
        series: [{
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
        }]
    };
    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option);
</script>
```

 

效果如下：

![img](./img/015.png) 

## 1.3. 查看ECharts官方实例

ECharts提供了很多官方实例，我们可以通过这些官方实例来查看展示效果和使用方法。

官方实例地址：<https://www.echartsjs.com/examples/>

![img](./img/016.png) 

可以点击具体的一个图形会跳转到编辑页面，编辑页面左侧展示源码（js部分源码），右侧展示图表效果，如下：

![img](./img/017.png) 

要查看完整代码可以点击右下角的Download按钮将完整页面下载到本地。

通过官方案例我们可以发现，使用ECharts展示图表效果，关键点在于确定此图表所需的数据格式，然后按照此数据格式提供数据就可以了，我们无须关注效果是如何渲染出来的。

在实际应用中，我们要展示的数据往往存储在数据库中，所以我们可以发送ajax请求获取数据库中的数据并转为图表所需的数据即可。

### 【小结】

1：ECharts简介

* ECharts百度开发的一套商业报表工具(库)，数据加载使用JavaScript

2：5分钟上手ECharts  引入js, 创建一个div, 编写js代码(初始化, 配置信息，设置配置信息) xAxis data, yAxis data

3：查看ECharts官方实例  

学习方法

* 看官网, 关注案例，无需关注api，根据项目需求，选择合适报表，从数据库中构造需要的数据
* 页面加载时发送请求获取数据，再设置echarts的配置项

# 2. 会员数量折线图

### 【目标】

使用ECharts实现注册会员数量折线图

### 【路径】

1：需求分析

2：前台代码

（1）导入ECharts库

（2）参考官方实例导入折线图

3：后台代码

（1）ReportController类 getMemberReport

* 创建12个月的数据

* 使用Calendar 对日期操作 对年-1，循环12次
* 每次+1个月，就得到12个月 List<String> months 2020-01
* 调用memberService 查询12个月的数据 List<Integer>  memberCount
* 把得到的12个月的月份数据及统计数据封装到map
* result{data: {months, memberCount}

（2）MemberService服务接口

（3）MemberServiceImpl服务实现类

* 遍历12个月，每个月拼接-31，查询，返回的数据添加到List<Integer>
* 返回List<Integer>

（4）MemberDao接口

（5）Mapper映射文件（MemberDao.xml）, 大小于号要转义

```sql
-- 大于号转义 &gt;        小于号转义 &lt;
select count(1) from t_member where regTime <='2020-09-31'
```

### 【讲解】

## 2.1. **需求分析**

会员信息是体检机构的核心数据，其会员数量和增长数量可以反映出机构的部分运营情况。通过折线图可以直观的反映出会员数量的增长趋势。本章节我们需要展示过去一年时间内每个月的会员总数据量。展示效果如下图：

到某个月最后一天为止时，会员总数量

![img](./img/018.png) 

需要的sql：

```sql
#2019年05月31日之前注册会员的人数(11)

SELECT COUNT(id) FROM t_member WHERE regTime <= '2019-05-31'

#2019年06月31日之前注册会员的人数(12)

SELECT COUNT(id) FROM t_member WHERE regTime <= '2019-06-31'

#2019年07月31日之前注册会员的人数(16)

SELECT COUNT(id) FROM t_member WHERE regTime <= '2019-07-31'
```



## 2.2. **前台代码**

会员数量折线图对应的页面为/pages/report_member.html。复制资料中的

 ![image-20200702171345665](img/image-20200702171345665.png)

到health_web中webapp/pages下

 ![image-20200702171428753](img/image-20200702171428753.png)

### 2.2.1. **导入ECharts库**

第一步：将echarts.js文件复制到health_web工程中

![image-20200702171506435](img/image-20200702171506435.png) 

第二步：在report_member.html页面引入echarts.js文件

![image-20200702171528489](img/image-20200702171528489.png)

### 2.2.2. **参照官方实例导入折线图**

1：定义<div>

```html
<div class="app-container">
    <div class="box">
        <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
        <div id="chart1" style="height:600px;"></div>
    </div>
</div>
```

2：定义script

```html
<script type="text/javascript">
        // 基于准备好的dom，初始化echarts实例
        var myChart1 = echarts.init(document.getElementById('chart1'));

        // 使用刚指定的配置项和数据显示图表。
        //myChart.setOption(option);

        axios.get("/report/getMemberReport.do").then((res)=>{
            if(res.data.flag) {
                myChart1.setOption(
                    {
                        title: {
                            text: '会员数量'
                        },
                        tooltip: {},
                        legend: {
                            data: ['会员数量']
                        },
                        xAxis: {
                            data: res.data.data.months
                        },
                        yAxis: {
                            type: 'value'
                        },
                        series: [{
                            name: '会员数量',
                            type: 'line',
                            data: res.data.data.memberCount
                        }]
                    });
            }else{
                this.$message.error(res.data.message);
            }
        });
    </script>
```

 

根据折现图对数据格式的要求，我们发送ajax请求，服务端需要返回如下格式的数据：

其中months和memberCount可以使用hashmap封装

返回Result：data、flag、message

其中data，是map集合

map集合的key：months；            map集合的值：List<String>

map集合的key：memberCount；      map集合的值：List<Integer>

```json
{
    "data":{
        "months":["2019-01","2019-02","2019-03","2019-04"],
        "memberCount":[3,4,8,10]
    },
    "flag":true,
    "message":"获取会员统计数据成功"
}
```

 

## 2.3. **后台代码**

### 2.3.1. **Controller**

在health_web工程中创建ReportController并提供getMemberReport方法

```java
package com.itheima.health.controller;

import com.alibaba.dubbo.config.annotation.Reference;
import com.itheima.health.constant.MessageConstant;
import com.itheima.health.entity.Result;
import com.itheima.health.service.MemberService;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.text.SimpleDateFormat;
import java.util.*;

/**
 * 统计报表
 */
@RestController
@RequestMapping("/report")
public class ReportController {
    @Reference
    private MemberService memberService;

    /**
     * 会员折线图
     * @return
     */
    @GetMapping("/getMemberReport")
    public Result getMemberReport(){
        // 组装过去12个月的数据, 前端是一个数组
        List<String> months = new ArrayList<String>();
        // 使用java中的calendar来操作日期, 日历对象
        Calendar calendar = Calendar.getInstance();
        // 设置过去一年的时间  年-月-日 时:分:秒, 减去12个月
        calendar.add(Calendar.MONTH, -12);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM");
        // 构建12个月的数据
        for (int i = 0; i < 12; i++) {
            // 每次增加一个月就
            calendar.add(Calendar.MONTH, 1);
            // 过去的日期, 设置好的日期
            Date date = calendar.getTime();
            // 2020-06 前端只展示年-月
            months.add(sdf.format(date));
        }

        // 调用服务查询过去12个月会员数据 前端也是一数组 数值
        List<Integer> memberCount =memberService.getMemberReport(months);
        // 放入map
        Map<String,Object> resultMap = new HashMap<String,Object>();
        resultMap.put("months",months);
        resultMap.put("memberCount",memberCount);

        // 再返回给前端
        return new Result(true, MessageConstant.GET_MEMBER_NUMBER_REPORT_SUCCESS,resultMap);
    }
} 
```



### 2.3.2. **服务接口**

在MemberService服务接口中扩展方法findMemberCountByMonth

```java
    /**
     * 统计过去一年的会员总数
     * @param months
     * @return
     */
    List<Integer> getMemberReport(List<String> months);
```



### 2.3.3. **服务实现类**

在MemberServiceImpl服务实现类中实现findMemberCountByMonth方法

```java
/**
 * 统计过去一年的会员总数
 * @param months
 * @return
 */
@Override
public List<Integer> getMemberReport(List<String> months) {
    //select count(1) from t_member where regTime<='2020-06-31';  <= Before
    List<Integer> memberCount = new ArrayList<Integer>();
    if(months != null) {
        // 循环12个，每个月查询一下
        for (String month : months) {
            // 到这个month为，一会有多少个会员
            Integer count = memberDao.findMemberCountBeforeDate(month + "-31");
            memberCount.add(count);
        }
    }
    return memberCount;
}
```

 

### 2.3.4. **Dao接口**

在MemberDao接口中扩展方法findMemberCountBeforeDate

```java
public Integer findMemberCountBeforeDate(String date);
```

 

### 2.3.5. **Mapper映射文件**

在MemberDao.xml映射文件中提供SQL语句

```xml
<!--根据日期统计会员数，统计指定日期之前的会员数-->
<select id="findMemberCountBeforeDate" parameterType="string" resultType="int">
    select count(id) from t_member where regTime &lt;= #{value}
</select>
```

```
注意：在xml文件中 ，  <号需要转义”&lt;”
				   >号需要转义”&gt;”
				   &号需要专业 ”&amp;”
```

 

测试

1：修改main.html

![image-20200702171856199](img/image-20200702171856199.png)

2：测试效果

![img](./img/024.png) 

###  【小结】

1：需求分析

会员数量统计， t_member。每个月的会员【总】数量。到某个月的最后一天为止时会员表中总数量

2：前台页面

（1）导入ECharts库

（2）参考官方实例导入折线图案例（折线图、柱状图、饼图...），参考API完成开发

res.data.data.months => [] => List<String>

res.data.data.memberCount => [] => List<Integer>

3：后台代码

（1）ReportController类

计算出过去12个月，利用Calendar日历工具对月份-12， 再循环12次，每次加1个月

（2）MemberService服务接口

（3）MemberServiceImpl服务实现类

（4）MemberDao接口

（5）Mapper映射文件（MemberDao.xml）



# 3.套餐预约占比饼形图

## 3.1. **需求分析**

### 【目标】

会员可以通过移动端自助进行体检预约，在预约时需要选择预约的体检套餐。本章节我们需要通过饼形图直观的展示出会员预约的各个套餐占比情况。展示效果如下图：

![img](./img/0001.png) 

### 【路径】

分析：

写sql报表的步骤

1. 找出数据所在的表及其字段 t_setmeal.name, t_order
2. 找出条件所在表 没条件
3. 找出数据表之间的关系，如果没有直接关系，则找中间表  t_setmeal.id=t_order.setmeal_id
4. 找出条件所在表之间的关系，如果没有直接关系，则找中间表
5. 找出数据表与条件表之间的关系，如果没有直接关系，则找中间表
6. 如果是多条合成一条，就用分组统计

```sql
-- 统计每个套餐的数量，同一个套餐有多条记录
-- 每个套餐只有一条记录, 把多条合成一条记录。用聚合函数,就得用分组group by
-- 语法不严格的写法，mysql5.7以下可以这样写,5.7以上就会报错
-- 严格的语法：分组统计时，select后的列必须在group by后出现过的
-- 这里的查询出来的列名必须与前端需要的数据格式一致(data:[{name:?,value:0}])
select s.name,count(1) value from t_setmeal s, t_order o 
where s.id=o.setmeal_id group by s.id,s.name
```
1：前台页面

（1）修改main.html

（2）导入ECharts库

（3）参照官方实例导入饼形图

（4）分析需要构造的数据格式和sql语句

```js
{
    flag:true,
    message:"",
    data:{
        setmealNames:List<String>,
        setmealCount:List<map<String,Object>>
    }
}
```



（5）饼图API介绍

2：后台代码

（1）ReportController

Map<String,Object> {setmealNames, setmealCount}, setmealNames从setmealCount提取

（2）SetmealService

（3）SetmealServiceImpl

（4）SetmealDao.java

（5）SetmealDao.xml

### 【讲解】

## 3.2. **前台页面**

套餐预约占比饼形图对应的页面为/pages/report_setmeal.html。

![img](./img/0002.png) 

### 3.2.1. **修改main.html**

添加report_setmeal.html的url

 ![image-20200702172037386](img/image-20200702172037386.png)

 

### 3.2.2. 导入ECharts库

![image-20200702171528489](img/image-20200702171528489.png)



### 3.2.3. 参照官方实例导入饼形图

```html
<div class="app-container">
    <div class="box">
        <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
        <div id="chart1" style="height:600px;"></div>
    </div>
</div>
```

Js代码：

```html
<script type="text/javascript">
    // 基于准备好的dom，初始化echarts实例
    var myChart1 = echarts.init(document.getElementById('chart1'));

    // 使用刚指定的配置项和数据显示图表。
    //myChart.setOption(option);

    axios.get("/report/getSetmealReport.do").then((res) => {
        if (res.data.flag) {
            myChart1.setOption({
                title: {
                    text: '套餐预约占比',
                    subtext: '',
                    x: 'center'
                },
                tooltip: {//提示框组件
                    trigger: 'item',//触发类型，在饼形图中为item
                    formatter: "{a} <br/>{b} : {c} ({d}%)"//提示内容格式
                },
                legend: {
                    orient: 'vertical',
                    left: 'left',
                    data: res.data.data.setmealNames
                },
                series: [
                    {
                        name: '套餐预约占比',
                        type: 'pie',
                        radius: '55%',
                        center: ['50%', '60%'],
                        data: res.data.data.setmealCount,
                        itemStyle: {
                            emphasis: {
                                shadowBlur: 10,
                                shadowOffsetX: 0,
                                shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        }
                    }
                ]
            });
        } else {
            alert(res.data.message);
        }
    });
</script>
```

### 3.2.4 .分析需要构造的数据格式和sql语句

1：根据饼形图对数据格式的要求，我们发送ajax请求，服务端需要返回如下格式的数据：

```json
{
    "data":{
            "setmealNames":["套餐1","套餐2","套餐3"],
            "setmealCount":[
                            {"name":"套餐1","value":10},
                            {"name":"套餐2","value":30},
                            {"name":"套餐3","value":25}
                           ]
           },
    "flag":true,
    "message":"获取套餐统计数据成功"
}
```

2：组织数据结构：

```java
Map<String,Object>;
map.put(“setmealNames”,List<String>);
map.put(“setmealCount”,List<Map>)

```

3：只需要把List<Map> setmealCount查询出来, setmealNames的数据也就有了

```sql
SELECT s.name, COUNT(o.id) value FROM t_order o, t_setmeal s WHERE o.setmeal_id = s.id GROUP BY s.name
```





### 3.2.5 .饼图：API介绍：

第一步：查看图例

![img](./img/0003.png) 

第二步：查看代码

![img](./img/0004.png) 

第三步：查看api

![img](./img/0005.png) 

## 3.3. **后台代码**

### 3.3.1. **Controller**

在health_web工程的ReportController中提供getSetmealReport方法

```java
/**
 * 套餐预约占比
 */
@GetMapping("/getSetmealReport")
public Result getSetmealReport(){
    // 调用服务查询
    // 套餐数量
    List<Map<String,Object>> setmealCount = setmealService.findSetmealCount();
    // 套餐名称集合
    List<String> setmealNames = new ArrayList<String>();
    // [{name:,value}]
    // 抽取套餐名称
    if(null != setmealCount){
        for (Map<String, Object> map : setmealCount) {
            //map {name:,value}
            // 获取套餐的名称
            setmealNames.add((String) map.get("name"));
        }
    }
    // 封装返回的结果
    Map<String,Object> resultMap = new HashMap<String,Object>();
    resultMap.put("setmealNames",setmealNames);
    resultMap.put("setmealCount",setmealCount);

    return new Result(true, MessageConstant.GET_SETMEAL_COUNT_REPORT_SUCCESS,resultMap);
}
```



### 3.3.2. **服务接口**

在SetmealService服务接口中扩展方法findSetmealCount

```java
/**
 * 获取套餐的预约数量
 * @return
 */
List<Map<String, Object>> findSetmealCount();
```



### 3.3.3. **服务实现类**

在SetmealServiceImpl服务实现类中实现findSetmealCount方法

```java
    /**
     * 获取套餐的预约数量
     * @return
     */
    @Override
    public List<Map<String, Object>> findSetmealCount() {
        return setmealDao.findSetmealCount();
    }
```

 

### 3.3.4. **Dao接口**

在SetmealDao接口中扩展方法findSetmealCount

```java
/**
 * 获取套餐的预约数量
 * @return
 */
List<Map<String, Object>> findSetmealCount();
```

 

### 3.3.5. **Mapper映射文件**

在SetmealDao.xml映射文件中提供SQL语句

```xml
<select id="findSetmealCount" resultType="map">
select s.name,t.value from t_setmeal s, (
    select setmeal_id,count(1) value from t_order group by setmeal_id
) t where s.id=t.setmeal_id
</select>
```

### 【小结】

核心地方 sql语句的编写，分析 预约套餐的占比  预约(t_order), 占比 (个数/总数) 每个套餐的数量/总数量

   每个套餐的数量 在订单表中，一个套餐是有多条记录, 多条记录要合计数(count), 分组, 按套餐来计算，按套餐来分组

1. 分析出数据格式，返回Result

```json
{
	flag:true,
	message:'成功',
	data:{
		setmealNames:['套餐A','套餐B','套餐C'],
		setmealCount:[
			 {value:1, name:'套餐A'},
			 {value:2, name:'套餐B'},
			 {value:1, name:'套餐C'},
		]
	
	}
}
```

1.数据封装

```json
data--->Map
setmealNames--->List<String>
setmealCount--->List<Map>
```

2.数据怎么来，分析出查询的sql语句 mysql 5.7 会报错

```sql
-- 报错
SELECT s.name, COUNT(1) value FROM t_order o, t_setmeal s WHERE o.setmeal_id = s.id GROUP BY o.setmal_id
-- 不会报错
SELECT s.name, COUNT(1) value FROM t_order o, t_setmeal s WHERE o.setmeal_id = s.id GROUP BY s.name
```

注意事项

在SetmealDao.xml映射文件中提供SQL语句

写SQL的返回值的时候，resultType="对象实体",也可以使用resultType="map"

```xml
<select id="findSetmealCount" resultType="map">
select s.name,t.value from t_setmeal s, (
    select setmeal_id,count(1) value from t_order group by setmeal_id
) t where s.id=t.setmeal_id
</select>
```



# 4. **运营数据统计**

## 4.1. **需求分析**

### 【目标】

通过运营数据统计可以展示出体检机构的运营情况，包括会员数据、预约到诊数据、热门套餐（前4）等信息。本章节就是要通过一个表格的形式来展示这些运营数据。效果如下图：

![img](./img/0006.png) 

### 【路径】

1：前台代码

（1）修改main.html

（2）定义数据模型

（3）对应后台sql语句

（4）发送请求获取动态数据

2：后台代码

（1）ReportController.java

（2）ReportService.java

（3）ReportServiceImpl.java

（4）OrderDao.java

​         MemberDao.java

（5）OrderDao.xml

​         MemberDao.xml



### 【讲解】

## 4.2. **前台代码**

运营数据统计对应的页面为/pages/report_business.html。

![img](./img/0007.png) 

### 4.2.1. **修改main.html**

```json
{
    "path": "/5-1",
    "title": "会员数量统计",
    "linkUrl":"report_member.html",
    "children":[]
},
{
    "path": "/5-2",
    "title": "预约套餐占比统计",
    "linkUrl":"report_setmeal.html",
    "children":[]
},
{
    "path": "/5-3",
    "title": "运营数据统计",
    "linkUrl":"report_business.html",
    "children":[]
}
```



### 4.2.2. **定义模型数据**

（1）：定义数据模型，通过VUE的数据绑定展示数据

```javascript
<script>
    var vue = new Vue({
        el: '#app',
        data:{
            reportData:{
                reportDate:null,
                todayNewMember :0,
                totalMember :0,
                thisWeekNewMember :0,
                thisMonthNewMember :0,
                todayOrderNumber :0,
                todayVisitsNumber :0,
                thisWeekOrderNumber :0,
                thisWeekVisitsNumber :0,
                thisMonthOrderNumber :0,
                thisMonthVisitsNumber :0,
                hotSetmeal :[
                    {name:'升级肿瘤12项筛查（男女单人）体检套餐',setmeal_count:200,proportion:0.222},
                    {name:'升级肿瘤12项筛查体检套餐',setmeal_count:200,proportion:0.222}
                ]
            }
        }
    })
</script>
```

（2）：数据展示

```html
<div class="app-container">
    <div class="box" style="height: 900px">
        <div class="excelTitle" >
            <el-button @click="exportExcel">导出Excel</el-button>运营数据统计
        </div>
        <div class="excelTime">日期：{{reportData.reportDate}}</div>
        <table class="exceTable" cellspacing="0" cellpadding="0">
            <tr>
                <td colspan="4" class="headBody">会员数据统计</td>
            </tr>
            <tr>
                <td width='20%' class="tabletrBg">新增会员数</td>
                <td width='30%'>{{reportData.todayNewMember}}</td>
                <td width='20%' class="tabletrBg">总会员数</td>
                <td width='30%'>{{reportData.totalMember}}</td>
            </tr>
            <tr>
                <td class="tabletrBg">本周新增会员数</td>
                <td>{{reportData.thisWeekNewMember}}</td>
                <td class="tabletrBg">本月新增会员数</td>
                <td>{{reportData.thisMonthNewMember}}</td>
            </tr>
            <tr>
                <td colspan="4" class="headBody">预约到诊数据统计</td>
            </tr>
            <tr>
                <td class="tabletrBg">今日预约数</td>
                <td>{{reportData.todayOrderNumber}}</td>
                <td class="tabletrBg">今日到诊数</td>
                <td>{{reportData.todayVisitsNumber}}</td>
            </tr>
            <tr>
                <td class="tabletrBg">本周预约数</td>
                <td>{{reportData.thisWeekOrderNumber}}</td>
                <td class="tabletrBg">本周到诊数</td>
                <td>{{reportData.thisWeekVisitsNumber}}</td>
            </tr>
            <tr>
                <td class="tabletrBg">本月预约数</td>
                <td>{{reportData.thisMonthOrderNumber}}</td>
                <td class="tabletrBg">本月到诊数</td>
                <td>{{reportData.thisMonthVisitsNumber}}</td>
            </tr>
            <tr>
                <td colspan="4" class="headBody">热门套餐</td>
            </tr>
            <tr class="tabletrBg textCenter">
                <td>套餐名称</td>
                <td>预约数量</td>
                <td>占比</td>
                <td>备注</td>
            </tr>
            <tr v-for="s in reportData.hotSetmeal">
                <td>{{s.name}}</td>
                <td>{{s.setmeal_count}}</td>
                <td>{{s.proportion}}</td>
                <td>{{s.remark}}</td>
            </tr>
        </table>
    </div>
</div>
```

### 4.2.3.对应后台sql语句

需求

```sql
-- 今天新增会员数

-- 总会员数

-- 本周新增会员数(>=本周的周一的日期)

-- 本月新增会员数(>=本月的第一天的日期)

-------------------------------------------------------------------------------
-- 今日预约数

-- 今日到诊数

-- 本周预约数(>=本周的周一的日期 <=本周的周日的日期)  

-- 本周到诊数(>=本周的周一的日期 <=本周的周日的日期)  +   （状态=已到诊）

-- 本月预约数(>=每月的第一天的日期 <=每月的最后一天的日期)

-- 本月到诊数(>=每月的第一天的日期 <=每月的最后一天的日期)  +   （状态=已到诊）

-- 热门套餐
```

所需sql语句

```sql
-- 今天新增会员数
SELECT COUNT(*) FROM t_member WHERE regTime = '2019-06-26'
-- 总会员数
SELECT COUNT(*) FROM t_member
-- 本周新增会员数(>=本周的周一的日期)
SELECT COUNT(*) FROM t_member WHERE regTime >= '2019-06-24'
-- 本月新增会员数(>=本月的第一天的日期)
SELECT COUNT(*) FROM t_member WHERE regTime >= '2019-06-01'
-------------------------------------------------------------------------------
-- 今日预约数
SELECT COUNT(*) FROM t_order WHERE orderDate = '2019-06-26'
-- 今日到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate = '2019-06-26' AND orderStatus = '已到诊'
-- 本周预约数(>=本周的周一的日期 <=本周的周日的日期)  
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-24' and '2019-06-31'  
-- 本周到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-24' and '2019-06-31' AND orderStatus = '已到诊'
-- 本月预约数(>=每月的第一天的日期 <=每月的最后一天的日期)
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-01' and '2019-06-31'
-- 本月到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-01' and '2019-06-31' AND orderStatus = '已到诊'

-- 热门套餐
SELECT s.name, COUNT(o.id) setmeal_count, COUNT(o.id)/(SELECT COUNT(id) FROM t_order ) proportion FROM t_setmeal s, t_order o WHERE s.id = o.setmeal_id
GROUP BY s.name ORDER BY  setmeal_count DESC LIMIT 0,4
```



### 4.2.4. **发送请求获取动态数据**

（1）添加VUE的created()钩子函数中发送ajax请求获取动态数据，通过VUE的数据绑定将数据展示到页面

```javascript
created() {
    axios.get("/report/getBusinessReportData.do").then((res)=>{
        if(res.data.flag){
            this.reportData = res.data.data;
        }else{
            this.$message.error(res.data.message);
        }
    });
},
```

（2）根据页面对数据格式的要求，我们发送ajax请求，服务端需要返回如下格式的数据：

```json
{
  "data":{
    "reportDate":"2019-04-25",
    "todayNewMember":0,
    "totalMember":10,
    "thisMonthNewMember":2,
    "thisWeekNewMember":0,
    "todayOrderNumber":0,
    "todayVisitsNumber":0,
    "thisWeekVisitsNumber":0,
    "thisWeekOrderNumber":0,
    "thisMonthOrderNumber":2,
    "thisMonthVisitsNumber":0,
    "hotSetmeal":[
      {"proportion":0.4545,"name":"粉红珍爱(女)升级TM12项筛查体检套餐","setmeal_count":5},
      {"proportion":0.1818,"name":"美丽爸妈升级肿瘤12项筛查体检套餐","setmeal_count":2},
      {"proportion":0.1818,"name":"珍爱高端升级肿瘤12项筛查","setmeal_count":2},
      {"proportion":0.0909,"name":"孕前检查套餐","setmeal_count":1}
    ],
  },
  "flag":true,
  "message":"获取运营统计数据成功"
}
```



## 4.3. **后台代码**

### 4.3.1. **Controller**

在ReportController中提供getBusinessReportData方法

```java
@Reference
private ReportService reportService;

/**
 * 运营统计数据
 * @return
 */
@GetMapping("/getBusinessReportData")
public Result getBusinessReportData() {
    Map<String, Object> businessReport = reportService.getBusinessReport();
    return new Result(true, MessageConstant.GET_BUSINESS_REPORT_SUCCESS,businessReport);

}
```

 

### 4.3.2. **服务接口**

在health_interface工程中创建ReportService服务接口并声明getBusinessReport方法

```java
public interface ReportService {

    /**
     * 获得运营统计数据
     * Map数据格式：
     *      reportDate（当前时间）--String
     *      todayNewMember（今日新增会员数） -> number
     *      totalMember（总会员数） -> number
     *      thisWeekNewMember（本周新增会员数） -> number
     *      thisMonthNewMember（本月新增会员数） -> number
     *      todayOrderNumber（今日预约数） -> number
     *      todayVisitsNumber（今日到诊数） -> number
     *      thisWeekOrderNumber（本周预约数） -> number
     *      thisWeekVisitsNumber（本周到诊数） -> number
     *      thisMonthOrderNumber（本月预约数） -> number
     *      thisMonthVisitsNumber（本月到诊数） -> number
     *      hotSetmeal（热门套餐（取前4）） -> List<Map<String,Object>>
     */
    Map<String,Object> getBusinessReport() throws Exception;
}
```

 复制传智健康day06中资料的DateUtils到health_common中的utils包下

 ![image-20200702173042795](img/image-20200702173042795.png)

给DateUtils添加 getLastDayOfThisMonth方法

```java
//获得本月最后一日的日期
public static Date getLastDayOfThisMonth(){
    Calendar calendar = Calendar.getInstance();
    calendar.set(Calendar.DAY_OF_MONTH,1);
    // 增加一个月
    calendar.add(Calendar.MONTH,1);
    // 再减去1天
    calendar.add(Calendar.DATE,-1);
    return calendar.getTime();
}
```



### 4.3.3. **服务实现类**

在health_service工程中创建服务实现类ReportServiceImpl并实现ReportService接口

```java
package com.itheima.health.service.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.itheima.health.dao.MemberDao;
import com.itheima.health.dao.OrderDao;
import com.itheima.health.service.ReportService;
import com.itheima.health.utils.DateUtils;
import org.springframework.beans.factory.annotation.Autowired;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * Description: No Description
 * User: Eric
 */
@Service(interfaceClass = ReportService.class)
public class ReportServiceImpl implements ReportService {

    @Autowired
    private MemberDao memberDao;
    @Autowired
    private OrderDao orderDao;

    @Override
    public Map<String, Object> getBusinessReport() {
        Map<String, Object> reportData = new HashMap<String,Object>();
        //reportDate
        Date today = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        // 星期一
        String monday = sdf.format(DateUtils.getFirstDayOfWeek(today));
        // 星期天
        String sunday = sdf.format(DateUtils.getLastDayOfWeek(today));
        // 1号
        String firstDayOfThisMonth = sdf.format(DateUtils.getFirstDay4ThisMonth());
        // 本月最后一天
        String lastDayOfThisMonth = sdf.format(DateUtils.getLastDayOfThisMonth());


        String reportDate = sdf.format(today);
        //=======================会员数量===========================
        //todayNewMember 今日新增会员
        int todayNewMember = memberDao.findMemberCountByDate(reportDate);
        //totalMember
        int totalMember = memberDao.findMemberTotalCount();
        //thisWeekNewMember 本周新增会员数
        int thisWeekNewMember = memberDao.findMemberCountAfterDate(monday);
        //thisMonthNewMember 本月新增会员数
        int thisMonthNewMember = memberDao.findMemberCountAfterDate(firstDayOfThisMonth);
        //==================================================

        //========================订单统计============================
        //todayOrderNumber 今日预约数
        int todayOrderNumber = orderDao.findOrderCountByDate(reportDate);
        //todayVisitsNumber 今日到诊数
        int todayVisitsNumber = orderDao.findVisitsCountByDate(reportDate);
        //thisWeekOrderNumber 本周预约数
        int thisWeekOrderNumber = orderDao.findOrderCountBetweenDate(monday, sunday);
        //thisWeekVisitsNumber 本周到诊数
        int thisWeekVisitsNumber = orderDao.findVisitsCountAfterDate(monday);
        //thisMonthOrderNumber 本月预约数
        int thisMonthOrderNumber = orderDao.findOrderCountBetweenDate(firstDayOfThisMonth, lastDayOfThisMonth);
        //thisMonthVisitsNumber 本月到诊数
        int thisMonthVisitsNumber = orderDao.findVisitsCountAfterDate(firstDayOfThisMonth);

        //========================== 热门套餐========================
        //hotSetmeal
        List<Map<String,Object>> hotSetmeal = orderDao.findHotSetmeal();

        reportData.put("reportDate",reportDate);
        reportData.put("todayNewMember",todayNewMember);
        reportData.put("totalMember",totalMember);
        reportData.put("thisWeekNewMember",thisWeekNewMember);
        reportData.put("thisMonthNewMember",thisMonthNewMember);
        reportData.put("todayOrderNumber",todayOrderNumber);
        reportData.put("todayVisitsNumber",todayVisitsNumber);
        reportData.put("thisWeekOrderNumber",thisWeekOrderNumber);
        reportData.put("thisWeekVisitsNumber",thisWeekVisitsNumber);
        reportData.put("thisMonthOrderNumber",thisMonthOrderNumber);
        reportData.put("thisMonthVisitsNumber",thisMonthVisitsNumber);
        reportData.put("hotSetmeal",hotSetmeal);

        return reportData;
    }
}

```

 

### 4.3.4. **Dao接口**

在OrderDao和MemberDao中声明相关统计查询方法

#### 4.3.4.1. **OrderDao.java**

```java
@Repository
public interface OrderDao {

    Integer findOrderCountByDate(String today);

    Integer findOrderCountBetweenDate(Map<String,Object> map);

    Integer findVisitsCountByDate(String today);

    Integer findVisitsCountAfterDate(Map<String,Object> map);

    List<Map> findHotSetmeal();
    
   /**
    * 统计日期范围内预约数量
    * @param startDate
    * @param endDate
    * @return
    */
    int findOrderCountBetweenDate(@Param("startDate") String startDate, @Param("endDate")String endDate);
}
```

 

#### 4.3.4.2. **MemberDao.java**

```java
@Repository
public interface MemberDao {
    public Integer findMemberCountBeforeDate(String date);
    public Integer findMemberCountByDate(String date);
    public Integer findMemberCountAfterDate(String date);
    public Integer findMemberTotalCount();
}
```

 

### 4.3.5. Mapper映射文件

在OrderDao.xml和MemberDao.xml中定义SQL语句

#### 4.3.5.1. **OrderDao.xml 添加：**

```xml
<!--统计日期范围内预约数量-->
<select id="findOrderCountBetweenDate" parameterType="string" resultType="int">
    select count(id) from t_order where orderDate between #{startDate} and #{endDate}
</select>

<!-- 修改查询热门套餐的sql 如下-->
<!--热门套餐，查询前5条-->
    <select id="findHotSetmeal" resultType="map">
        select s.name, count(o.id) setmeal_count ,count(o.id)/t.total proportion,s.remark
        from t_order o, t_setmeal s,(select count(id) total from t_order) t
        where s.id = o.setmeal_id
        group by o.setmeal_id
        order by setmeal_count desc limit 0,4
    </select>
```

 

#### 4.3.5.2. **MemberDao.xml：**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.itheima.health.dao.MemberDao">
  
    <!--根据日期统计会员数，统计指定日期之前的会员数-->
    <select id="findMemberCountBeforeDate" parameterType="string" resultType="int">
        select count(id) from t_member where regTime &lt;= #{value}
    </select>

    <!--根据日期统计会员数-->
    <select id="findMemberCountByDate" parameterType="string" resultType="int">
        select count(id) from t_member where regTime = #{value}
    </select>

    <!--根据日期统计会员数，统计指定日期之后的会员数-->
    <select id="findMemberCountAfterDate" parameterType="string" resultType="int">
        select count(id) from t_member where regTime &gt;= #{value}
    </select>

    <!--总会员数-->
    <select id="findMemberTotalCount" resultType="int">
        select count(id) from t_member
    </select>
</mapper>
```

### 【小结】

1. 数据查询的比较多, 数据怎么封装 --->选择Map
2. 数据怎么查询出来，并放置到vue中的模型

```sql
-- 今天新增会员数
SELECT COUNT(*) FROM t_member WHERE regTime = '2019-06-26'
-- 总会员数
SELECT COUNT(*) FROM t_member
-- 本周新增会员数(>=本周的周一的日期)
SELECT COUNT(*) FROM t_member WHERE regTime >= '2019-06-24'
-- 本月新增会员数(>=本月的第一天的日期)
SELECT COUNT(*) FROM t_member WHERE regTime >= '2019-06-01'

-- 今日预约数
SELECT COUNT(*) FROM t_order WHERE orderDate = '2019-06-26'
-- 今日到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate = '2019-06-26' AND orderStatus = '已到诊'
-- 本周预约数(>=本周的周一的日期 <=本周的周日的日期)  
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-24' and '2019-06-31'  
-- 本周到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-24' and '2019-06-31' AND orderStatus = '已到诊'
-- 本月预约数(>=每月的第一天的日期 <=每月的最后一天的日期)
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-01' and '2019-06-31'
-- 本月到诊数
SELECT COUNT(*) FROM t_order WHERE orderDate between '2019-06-01' and '2019-06-31' AND orderStatus = '已到诊'

-- 热门套餐
select s.name,t.setmeal_count,t.setmeal_count/t1.total proportion,s.remark from (
    select setmeal_id,count(1) setmeal_count from t_order group by setmeal_id
) t, (select count(1) total from t_order) t1, t_setmeal s
where s.id=t.setmeal_id
order by t.setmeal_count desc limit 0,4
```

### 编写报表sql的步骤

1. 找出数据所在表, 看到的数据在哪些表里

2. 找出条件所在的表，【注意】隐藏条件 本周，本月

3. 找出数据表之间的关系，如果没有则找中间表

4. 找出条件表之间的关系，如果没有则找中间表

5. 找出数据表与条件表之间的关系，如果没有则找中间表

6. 如果是多条合成一条就使用聚合函数，使用分组

7. 如果不能用一条语句完成，就使用子查询，都可以放到from后面 

8. 如果要从小到大，从大到小。。。。使用排序

9. 尽量不要在where后面使用函数，可以在select后使用

   

# 5. 运营数据统计报表导出

### 【目标】

运营数据统计报表导出就是将统计数据写入到Excel并提供给客户端浏览器进行下载，以便体检机构管理人员对运营数据的查看和存档。

### 【路径】

1：提供模板文件

2：前台代码

在report_business.html页面提供“导出”按钮并绑定事件

3：后台代码

ReportController 

* 获取模板所在
* 获取报表数据
* 创建Workbook传模板所在路径
* 获取工作表
* 获取行，单元格，设置相应的数据
* 热门套餐，遍历输出填值
  * 数量的类型为Long
  * 占比的值的类型为bigdecimal，转成dubbo

* 设置响应体内容的格式application/vnd.ms-excel
* 设置响应头信息，告诉浏览器下载的文件名叫什么 Content-Disposition, attachment;filename=文件名
* Workbook.write响应输出流

* 关闭workbook

### 【讲解】

## 5.1. **提供模板文件**

本章节我们需要将运营统计数据通过POI写入到Excel文件，对应的Excel效果如下：

![img](./img/0008.png) 

通过上面的Excel效果可以看到，表格比较复杂，涉及到合并单元格、字体、字号、字体加粗、对齐方式等的设置。如果我们通过POI编程的方式来设置这些效果代码会非常繁琐。

在企业实际开发中，对于这种比较复杂的表格导出一般我们会提前设计一个Excel模板文件，在这个模板文件中提前将表格的结构和样式设置好，我们的程序只需要读取这个文件并在文件中的相应位置写入具体的值就可以了。

在本章节资料中已经提供了一个名为report_template.xlsx的模板文件

![img](./img/0009.png) 

需要将这个文件复制到	health_web工程中

![img](./img/0010.png) 

## 5.2. **前台代码**

（1）在report_business.html页面提供“导出”按钮并绑定事件

```html
<div class="excelTitle" >
    <el-button @click="exportExcel">导出Excel</el-button>运营数据统计
</div>
```

（2）导出方法

```javascript
methods:{
    exportExcel(){
        window.location.href = '/report/exportBusinessReport.do';
    }
}
```

 

## 5.3. **后台代码**

在ReportController中提供exportBusinessReport方法，基于POI将数据写入到Excel中并通过输出流下载到客户端

```java
/**
 * 导出运营统计数据报表
 */
@GetMapping("/exportBusinessReport")
public void exportBusinessReport(HttpServletRequest req, HttpServletResponse res){
    // 获取模板的路径, getRealPath("/") 相当于到webapp目录下
    String template = req.getSession().getServletContext().getRealPath("/template/report_template.xlsx");
    // 创建工作簿(模板路径)
    try (// 写在try()里的对象，必须实现closable接口，try()cathc()中的finally
        OutputStream os = res.getOutputStream();
        XSSFWorkbook wk = new XSSFWorkbook(template);){
        // 获取工作表
        XSSFSheet sht = wk.getSheetAt(0);
        // 获取运营统计数据
        Map<String, Object> reportData = reportService.getBusinessReport();
        // 日期 坐标 2,5
        sht.getRow(2).getCell(5).setCellValue(reportData.get("reportDate").toString());
        //======================== 会员 ===========================
        // 新增会员数 4,5
        sht.getRow(4).getCell(5).setCellValue((Integer)reportData.get("todayNewMember"));
        // 总会员数 4,7
        sht.getRow(4).getCell(7).setCellValue((Integer)reportData.get("totalMember"));
        // 本周新增会员数5,5
        sht.getRow(5).getCell(5).setCellValue((Integer)reportData.get("thisWeekNewMember"));
        // 本月新增会员数 5,7
        sht.getRow(5).getCell(7).setCellValue((Integer)reportData.get("thisMonthNewMember"));

        //=================== 预约 ============================
        sht.getRow(7).getCell(5).setCellValue((Integer)reportData.get("todayOrderNumber"));
        sht.getRow(7).getCell(7).setCellValue((Integer)reportData.get("todayVisitsNumber"));
        sht.getRow(8).getCell(5).setCellValue((Integer)reportData.get("thisWeekOrderNumber"));
        sht.getRow(8).getCell(7).setCellValue((Integer)reportData.get("thisWeekVisitsNumber"));
        sht.getRow(9).getCell(5).setCellValue((Integer)reportData.get("thisMonthOrderNumber"));
        sht.getRow(9).getCell(7).setCellValue((Integer)reportData.get("thisMonthVisitsNumber"));

        // 热门套餐
        List<Map<String,Object>> hotSetmeal = (List<Map<String,Object>> )reportData.get("hotSetmeal");
        int row = 12;
        for (Map<String, Object> setmealMap : hotSetmeal) {
            sht.getRow(row).getCell(4).setCellValue((String)setmealMap.get("name"));
            sht.getRow(row).getCell(5).setCellValue((Long)setmealMap.get("setmeal_count"));
            BigDecimal proportion = (BigDecimal) setmealMap.get("proportion");
            sht.getRow(row).getCell(6).setCellValue(proportion.doubleValue());
            sht.getRow(row).getCell(7).setCellValue((String)setmealMap.get("remark"));
            row++;
        }

        // 工作簿写给reponse输出流
        res.setContentType("application/vnd.ms-excel");
        String filename = "运营统计数据报表.xlsx";
        // 解决下载的文件名 中文乱码
        filename = new String(filename.getBytes(), "ISO-8859-1");
        // 设置头信息，告诉浏览器，是带附件的，文件下载
        res.setHeader("Content-Disposition","attachement;filename=" + filename);
        wk.write(os);
        os.flush();
    } catch (IOException e) {
        e.printStackTrace();
    }

}
```

 

### 【小结】

​	如果发现导出Excel有些复杂, 一般先把Excel制作一个模版. 把模版通过POI读取到内存里面. 获得数据, 动态的给模版里面填充数据, 再响应(Response)文件，使用IO流的方式输出。

## 登陆修改为ajax

login.html

```html
<script>
    var vue = new Vue({
        el:'#app',
        data:{
            username:'admin',
            password:'admin'
        },
        methods:{
            login(){
                axios.post('/login.do?username=' + this.username  + "&password=" + this.password).then(res => {
                    if(res.data.flag){
                        window.location.href="/pages/main.html"
                    }else{
                        this.$message.error(res.data.message);
                    }
                })
            }
        }
    });
</script>
```

添加数据绑定和事件绑定

 ![image-20200928165523950](img/image-20200928165523950.png)

修改spring-security.xml

 ![image-20200928165625734](img/image-20200928165625734.png)

UserController中添加方法

```java
    @RequestMapping("/loginSuccess")
    public Result loginSuccess(){
        return new Result(true, MessageConstant.LOGIN_SUCCESS);
    }

    @RequestMapping("/loginFail")
    public Result loginFail(){
        return new Result(false, "用户名或密码不正确");
    }
```

