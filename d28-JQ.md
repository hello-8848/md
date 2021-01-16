---
typora-copy-images-to: img
---

# day28-JQ           #
# 学习目标

- [ ] 能够引入jQuery
- [ ] 能够掌握JQ和JS对象的转换【重点】
- [ ] 能够掌握JQ中事件的使用
- [ ] 能够掌握JQ中常用的选择器
- [ ] 能够使用JQ操作样式
- [ ] 能够使用JQ操作属性
- [ ] 能够使用JQ操作DOM
- [ ] 能够掌握JQ遍历

# 第一章-JQ知识点

## 知识点-JQ介绍

### 1.目标

- [ ] 知道什么是JQ

### 2.路径

1.  jQuery的概述 
2.  jQuery的作用
3. jQuery框架的下载
4. jQuery的版本

### 3.讲解

#### 3.1 jQuery的概述 

​	jQuery是一个优秀的javascript库(类似Java里面的jar包)，兼容css3和各大浏览器，提供了dom、events、animate、ajax等简易的操作。  

**==jquery是一个js库（工具），让我们使用js更加简单==**

#### 3.2 jQuery的作用

​	jQuery最主要的作用是简化**js操作DOM的代码 **

#### 3.3 jQuery框架的下载

​	jQuery的官方下载地址：http://www.jquery.com

#### 3.4 jQuery的版本

+ **1.x**：兼容IE678，使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 (2016年5月20日)
+ **2.x**：不兼容IE678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，最终版本：2.2.4 (2016年5月20日)
+ **3.x**：不兼容IE678，很多老的jQuery插件不支持这个版本。目前该版本是官方主要更新维护的版本.
+ 注:开发版本与生产版本，命名为jQuery-x.x.x.js为开发版本，命名为jQuery-x.x.x.min.js为生产版本，开发版本源码格式良好，有代码缩进和代码注释，方便开发人员查看源码，但体积稍大。而生产版本没有代码缩进和注释，且去掉了换行和空行，不方便发人员查看源码，但体积很小 

![1593580409054](img/1593580409054.png)

### 4.小结

1. jquery： 就是一个js工具，让你操作DOM更加简洁



## 案例-JQ快速入门

### 1.需求

​	等页面加载完成之后 弹出 hello...  

语法使用：==**$()括弧里面写一个匿名函数**==

```js
// 页面加载函数类似于   body的onload
$(function(){
    // 执行代码
});
```



### 2.步骤

**技术**：

1. 引入jquery文件
2. 在`<script>`标签编写js代码

### 3.实现

- 文档加载完成事件

  - ```js
    <!--页面加载事件  onload-->
    <script>
        
        // 页面加载完成、类似body的onload事件
        $(function () {
            alert("hello...")
        })
    
    </script>
    ```
    
    
    
  

#### 常见错误

![image-20191217091454159](img/image-20191217091454159.png) 

90%以上的几率是JQ库没有导入或者JQ库路径写错了

### 4.小结

#### 4.1步骤

1. 引入jq.js文件到当前html中
2. 在script标签写js代码： $()



## 知识点-JQ和JS对象转换【掌握】

### 1.目标

- [ ] 掌握JQ和JS对象转换

### 2.分析

#### 2.1对象说明

​	==JS对象==:  `document.getElementByXXX()`  获得的都是JS对象        我们使用js对象一般是使用它的属性。

​	==JQ对象==: `$()`        我们==使用jq对象，主要就是使用jq对象的方法==。

​	jQuery本质上虽然也是js，但如果==**使用jQuery的属性和方法那么必须保证对象是jQuery对象而不是js方式获得的DOM对象**==。使用js方式获取的对象是js的DOM对象，使用jQuery方式获取的对象是jQuery对象。两者的转换关系如下

#### 2.2 ==转换语法==

​	js的DOM对象`--->`jQuery对象，==语法：$(js对象)==

​	**示例：** 

```js
// 获得js对象
var jsObj = document.getElementById("inputId");
// js对=对象转jq对象
var jqObj = $(jsObj)
```

​	jQuery对象`--->`js对象，==语法：jquery对象[0]== 或 jquery对象.get(索引); 一般索引写0

**示例：**

```js
// jq对象转js对象
var jsObj = jqObj[0]
```



### 3.讲解-实例

- 分别使用js、jquery对象获取文本输入框的内容
- 实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>


</head>
<body>
用户名: <input  id="inputId" type="text"><br>
<input   type="button" value="获取值" onclick="getusername()"><br>

</body>

<script>
    // 分别使用js、jquery获取文本框输入内容
    function getusername() {
        //1. 使用js获取文本框输入内容
        // 根据id属性值获得唯一的元素
        var jsObj = document.getElementById("inputId");
        // .value属性获得文本框的输入内容---[使用js对象一般是使用它的属性]
        console.log(jsObj.value);

        //2. 使用jquery对象获取文本框输入内容
        // js---》转jq对象：  $(js对象)
        var jqObj = $(jsObj);
        // 提前用到jq的语法-----[使用jq对象，主要就是使用jq对象的方法]
        console.log(jqObj.val());


        // jq对象转成js对象： jq对象[0]
        var jsObj2 = jqObj[0];
        console.log(jsObj2.value);

    }
</script>
</html>
```



### 4.小结

1. js对象转jq对象： $(js对象)
2. jq对象转js对象：jq对象[0]



## 知识点- 基本选择器【重点】

### 1.目标

掌握基本选择器获取目标元素

### 2.路径

- id选择器
- 类class选择器
- 标签选择器

### 3.讲解

选择器可以获得页面的目标元素，jquery中的基本选择器的语法如下：

- 语法

| 选择器名称               | 语法                    | 解释                                                         |
| ------------------------ | ----------------------- | ------------------------------------------------------------ |
| id选择器                 | ==$("#id的属性值")==    | 获得与指定id属性值匹配的元素；等同于document.getElementById("id属性值") |
| 类选择器                 | ==$(".class的属性值")== | 获得与指定的class属性值匹配的元素；等同于document.getElementsByClassName("class属性值") |
| 标签选择器（元素选择器） | ==$("html标签名")==     | 获得所有匹配标签名称的于元素；等同于document.getElementsByTagName("html标签名") |



- 示例
  - 获取文本框元素
  - 获取所有段落元素
  - 获取段落一元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
请输入内容：<input type="text" id="inputId">
<p class="pClass">段落一</p>
<p>段落二</p>
<p>段落三</p>

<input type="button" value="执行操作" onclick="exe()">
</body>

<script>



    function exe() {
        //- 获取文本框元素： id选择器：   $("#id属性值")
        var $inputEle = $("#inputId");
        console.log($inputEle.val()); // 获取jq对象的内容（文本框的输入值）
        console.log($inputEle);
        //- 获取所有段落元素： 标签选择器：  $("标签名")
        var aaaa = $("p");
        console.log(aaaa);
        //- 获取段落一元素： 类class选择器： $(".class属性值")
        var $pClass = $(".pClass");
        console.log($pClass);
    }
</script>
</html>
```



### 4.小结

1. id选择器：  $("#id属性值")
2. 标签选择器： $("标签名字")
3. class选择器： $(".class属性值")

## 知识点-事件的使用【掌握】

### 1.目标

- [ ] 掌握jq中事件的使用

### 2.路径

1. 基本事件的使用
2. jQuery的事件绑定与解绑 
3. 事件切换  

### 3.讲解

#### 3.1 基本事件的使用【重点】

+ 事件在jq里面都封装成了方法. ==去掉了JS里面on==.  语法【按照语法依葫芦画瓢】:


```js
jq对象.事件方法名(function(){});

eg:点击事件
button对象.click(function(){});

XXX.focus(function(){})
XXX.blur(function(){})
XXX.change(function(){})
XXX.mouseenter(function(){})
XXX.keydown(function(){})
// ......
```

+ 点击事件--给按钮增加点击事件

+ 获得焦点和失去焦点--给文本框增加焦点事件


+ 内容改变事件 ---给下拉框增加事件


+ 鼠标相关的事件


+ 键盘相关事件

==**需求：**==

1. 给按钮增加点击事件
2. 给用户名文本框增加获得焦点、失去焦点事件
3. 给下拉框增加内容改变事件
4. 给文本框增加键盘按下、弹起事件
5. 给div增加增加鼠标移入、移出事件

- 页面准备

```html
<input type="button" value="点击我" id="btId"><br>

用户名： <input type="text" id="textId"><br>
籍贯：
<select id="selectId">
    <option>北京</option>
    <option>广东</option>
    <option>上海</option>
</select>
<br>
键盘事件：<input type="text" id="keyId">
<div id="divid">
    我是一个中国人，我爱我的祖国！
</div>
```



- 事汇总代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
<input type="button" value="点击我" id="btId" ><br>

用户名： <input type="text" id="textId" ><br>
籍贯：
<select id="selectId">
    <option>北京</option>
    <option>广东</option>
    <option>上海</option>
</select>
<br>
键盘事件：<input type="text" id="keyId">
<div id="divid">
    我是一个中国人，我爱我的祖国！
</div>
</body>
    <script>
        // 事件方法名 就是原生js的事件名去掉了on， onclick---》click
        // 语法： jq对象.事件方法名(函数function(){写逻辑})

        //1. 给按钮增加点击事件
        $("#btId").click(function () {
            alert("被点击了...")
        });
        //2. 给用户名文本框增加获得焦点、失去焦点事件
        $("#textId").focus(function () {
            console.log("获得了焦点...");
        });
        $("#textId").blur(function () {
            console.log("失去了焦点...");
        });
        //3. 给下拉框增加内容改变事件
        $("#selectId").change(function () {
            console.log("内容改变了...");
        });
        //4. 给文本框增加键盘按下、弹起事件
        $("#keyId").keydown(function () {
            console.log("按下了...");
        });
        $("#keyId").keyup(function () {
            console.log("弹起了...");
        });
        //5. 给div增加增加鼠标移入、移出事件---打印一句话
        $("#divid").mouseenter(function () {
            console.log("进入了...");
        });
        $("#divid").mouseout(function () {
            console.log("出去了...");
        });

    </script>
</html>
```



- jq的基本事件： 就是把原来的js事件名onXXX---》XXX()方法



#### 3.2jQuery的事件绑定与解绑（了解）

+ 事件的绑定

```js
jQuery元素对象.on(事件的类型,function(){} );
其中：事件名称是jQuery的事件方法的方法名称，例如：click、mouseover、mouseout、focus、blur等

eg:点击事件
-- 基本使用  jq对象.click(function(){})
-- 绑定发送  jq对象.on("click",function(){});
```

+ 事件的解绑

```js
jQuery元素对象.off(事件名称);
//示例
jq对象.off("click")

其中：参数事件名称如果省略不写，可以解绑该jQuery对象上的所有事件
```

+ 实例代码

```html
<input type="button" id="btId" value="点击"><br>
<input type="button" id="btId2" value="解绑第一个按钮的点击事件" onclick="unbind()">
</body>

<script>
    // 给按钮绑定事件
    $("#btId").on("click", function () {
        alert(5555)
    })

    // 解绑
    function unbind() {
        $("#btId").off("click")
    }
</script>
```

#### 3.3事件轮换（了解）



+ 链式写法 

```js
<div id="divid">
    我是一个中国人，我爱我的祖国！
</div>

//给div增加增加鼠标移入、移出事件---改编文字颜色（提前用一下  css(属性，值)  方法）
    $("#divid").mouseenter(function () {
            console.log("进入了...");
        }).mouseout(function () {
            console.log("出去了...");
        });

```

### 4.小结

1. 基本事件

   1. 点击   jquery对象.click(function(){})
   2. 焦点：    jquery对象.focus(function(){})、 jquery对象.blur(function(){})
   3. 内容改变：   jquery对象.change(function(){})
   4. 键盘：   jquery对象.keyup(function(){})、 jquery对象.keydown(function(){})
   5. 鼠标：   jquery对象.mouseenter(function(){})、  jquery对象.mouseout(function(){})




## 知识点-动画【了解】

### 1.目标

- [ ] 了解JQ的动画

### 2.路径

1. 基本效果
2. 滑动效果
3. 淡入淡出效果

### 3.讲解

#### 3.1基本效果【显隐效果】

+ 方法

| 方法名称                      | 解释                                         |
| ----------------------------- | -------------------------------------------- |
| show([speed],[easing],[fn]])  | 显示元素方法                                 |
| hide([speed,[easing],[fn]])   | 隐藏元素方法                                 |
| toggle([speed],[easing],[fn]) | 切换元素方法，显示的使之隐藏，隐藏的使之显示 |

+ 参数

| 参数名称  | 解释                                                         |
| --------- | ------------------------------------------------------------ |
| **speed** | 三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000) |
| easing    | 用来指定切换效果，默认是"swing"，可用参数"linear"            |
| **fn**    | ==在动画完成时执行的函数，每个元素执行一次==                 |

+ 实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
<div style="width:300px; color:red">我是中国人，我爱我的祖国</div>
<input type="button" value="缓慢消失" onclick="hide()">
<input type="button" value="显示" onclick="show()">
<input type="button" value="toggle切换" onclick="toggle()">
</body>

<script>
    //实现3个js函数
    function hide() {
        // 参数一： 毫秒值，表示时间内执行完整个动画；
        // 参数二： 函数，动画完成时会执行这个函数
        $("div").hide(2000, function () {
            //alert("动画已完成")
            console.log("动画已完成");
        });
    }

    function show() {
        // 显示
        $("div").show(1000);
    }

    function toggle() {
        // 显隐之间来回切换，原来是显示的，就隐藏； 原来是隐藏的，就显示
        $("div").toggle();
    }
</script>
</html>
```

#### 3.2滑动效果（有兴趣自己课后练着玩）

+ 方法-----用法参数和【基本效果】使用方式差不多

| 方法名称                           | 解释                                         |
| ---------------------------------- | -------------------------------------------- |
| slideDown([speed,[easing],[fn]])   | 向下滑动方法                                 |
| slideUp([speed,[easing],[fn]])     | 向上滑动方法                                 |
| slideToggle([speed],[easing],[fn]) | 切换元素方法，显示的使之隐藏，隐藏的使之显示 |

+ 参数

| 参数名称 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| speed    | 三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000) |
| easing   | 用来指定切换效果，默认是"swing"，可用参数"linear"            |
| fn       | 在动画完成时执行的函数，每个元素执行一次                     |

#### 3.3 淡入淡出效果（有兴趣自己课后练着玩）

+ 方法-----用法参数和【基本效果】使用方式差不多

| 方法名称                          | 解释                                         |
| --------------------------------- | -------------------------------------------- |
| fadeIn([speed,[easing],[fn]])     | 淡入显示方法                                 |
| fadeOut([speed,[easing],[fn]])    | 淡出隐藏方法                                 |
| fadeToggle([speed],[easing],[fn]) | 切换元素方法，显示的使之隐藏，隐藏的使之显示 |

+ 参数

| 参数名称 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| speed    | 三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000) |
| easing   | 用来指定切换效果，默认是"swing"，可用参数"linear"            |
| fn       | 在动画完成时执行的函数，每个元素执行一次                     |



### 4.小结

1. 显隐
   1. 显示： show(速度，函数)
   2. 隐藏： hide(速度,函数)
   3. 显隐之间切换： toggle(速度,函数)





## 知识点-其他选择器（了解）

### 1.目标

- [ ] 掌握JQ常用的选择器

### 2.路径

1. 基本选择器
2. 层级选择器 
3. 属性选择器
4. 基本过滤选择器
5. 表单属性选择器

### 3.讲解

#### 3.1.基本选择器【重点】（上面已讲）

+ 语法

| 选择器名称               | 语法                | 解释                              |
| ------------------------ | ------------------- | --------------------------------- |
| 标签选择器（元素选择器） | $("html标签名")     | 获得所有匹配标签名称的于元素      |
| id选择器                 | $("#id的属性值")    | 获得与指定id属性值匹配的元素      |
| 类选择器                 | $(".class的属性值") | 获得与指定的class属性值匹配的元素 |

#### 3.2 层级选择器

+ 语法

| 选择器名称     | 语法       | 解释                                              |
| -------------- | ---------- | ------------------------------------------------- |
| ==后代选择器== | $("A B ")  | 选择A元素内部的所有B元素； ==A  B可以使用选择器== |
| 子选择器       | $("A > B") | 选择A元素内部的所有B子元素                        |
| 兄弟选择器     | $("A + B") | 获得A元素同级的下一个B元素                        |
| 兄弟选择器     | $("A ~ B") | 获得A元素同级的所有B元素                          |

```html
<span>
    <font color="red">你好</font>
    <font color="green">hello</font>
</span>

<script>
    // 1. 获得span里面的所有font： 后代选择器  $("A B")
    console.log($("span font"));``
</script>
```



#### 3 属性选择器

+ 语法：  可以用在多个表单输入项input中获取type属性值来区分

| 选择器名称     | 语法              | 解释                                                         |
| -------------- | ----------------- | ------------------------------------------------------------ |
| 属性选择器     | $("A[属性名]")    | 包含指定属性的选择器： $("input[type]")                      |
| ==属性选择器== | $("A[属性名=值]") | 包含指定属性等于指定值的选择器；<br>==$("input[type=text]")==:可以获得文本框元素对应的jq对象 |

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
用户名：<input type="text"><br>
密码：<input type="password"><br>
<input id="btId" type="button" value="获取文本框的内容">
</body>

<script>
    //1. 给按钮增加事件
    //jq对象.事件方法名(函数)
    $("#btId").click(function () {
        // 获取文本框的内容---属性选择器获得文本框
        var $input = $("input[type=text]");
        console.log($input.val());
    })
</script>
</html>
```



#### 4.基本过滤选择器

+ 语法

| 选择器名称         | 语法           | 解释                                        |
| ------------------ | -------------- | ------------------------------------------- |
| 首元素选择器       | :first         | 获得选择的元素中的第一个元素： $("p:first") |
| 尾元素选择器       | :last          | 获得选择的元素中的最后一个元素              |
| 非元素选择器       | :not(selecter) | 不包括指定内容的元素                        |
| ==**偶数选择器**== | :even          | 偶数;  $("tr:even")，获取偶数行             |
| ==**奇数选择器**== | :odd           | 奇数；$("tr:odd")，获取奇数行               |
| 等于索引选择器     | :eq(index)     | 指定索引元素                                |
| 大于索引选择器     | :gt(index)     | 大于指定索引元素                            |
| 小于索引选择器     | :lt(index)     | 小于指定索引元素                            |

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/jquery-3.3.1.min.js"></script>
</head>
<body>
<table border="1px">
    <tr>
        <td>1</td>
    </tr>
    <tr>
        <td>2</td>
    </tr>
    <tr>
        <td>3</td>
    </tr>
    <tr>
        <td>4</td>
    </tr>
</table>
</body>
    <script>
        // 3. 表格的奇数行(红色)、偶数行（绿色）颜色不一样
        // 表格的索引  0、1、2、3
        $("tr:odd").css("color", "red")
        $("tr:even").css("color", "green")
    </script>
</html>
```



#### 5.表单属性选择器

+ 语法

| 选择器名称       | 语法      | 解释                                                        |
| ---------------- | --------- | ----------------------------------------------------------- |
| 可用元素选择器   | :enabled  | 获得可用元素                                                |
| 不可用元素选择器 | :disabled | 获得不可用元素                                              |
| **选中选择器**   | :checked  | 获得单选/复选框选中的元素<br>==$("input:checked")==         |
| **选中选择器**   | :selected | 获得下拉框选中的选项元素<br>==$("select option:selected")== |

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
<form>
    用户名：<input type="text" ><br>
    密码：<input type="password"><br>
    爱好：<input type="checkbox" value="basketball">篮球
    <input type="checkbox" value="football">足球<br>
    籍贯：
    <select id="address">
        <option value="bj">北京</option>
        <option value="nj">南京</option>
        <option value="xj">西京</option>
    </select><br>

<!--    获取被选中的元素-->
    <input type="button" value="按钮1" onclick="printVal()">
    <!--disabled不可用，禁用某个元素-->
    <input type="button" value="按钮2" disabled>
</form>
</body>

<script>
    // 1. 获得可用的input元素
    console.log($("input:enabled"));

    // 2. 获得不可用的input元素
    console.log($("input:disabled"));


    function printVal(){
        //3. 获取被选中的input元素（checked对单选、复选进行处理获取）
        console.log($("input:checked"));

        //4. 获取下拉框中被选中的元素
        console.log($("#address option:selected"));
    }


</script>
</html>
```



### 4.小结

1. 后代选择器： $("A B")
2. 奇偶选择器:     $("tr:even") 、 $("tr:odd")
3. 属性选择器： $("input[type=text]")，获取文本框
4. 表单属性选择器： :checked 获取单选、复选被选中、:selected 获取下拉框被选中的option



## 知识点-操作样式

### 1.目标

- [ ] 掌握JQ操作样式

### 2.讲解

name参数名css里面怎么写这个就怎么写

| API方法         | 解释                                          |
| --------------- | :-------------------------------------------- |
| css(name)       | 获取目标CSS样式<br>==jq对象.css("color")==    |
| css(name,value) | 设置CSS样式<br>==jq对象.css("color", "red")== |

需求：设置段落字体颜色为红色

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
<p>段落一</p>
<p>段落二</p>
<p>段落三</p>
</body>

<script>


    // 设置段落字体颜色为红色
    // css("属性名","属性值")，属性名在css中怎么写，这里就怎么写;  还可以取出中横线-，写驼峰命名
    $("p").css("color","green");
    $("p").css("background-color", "yellow");
    $("p").css("backgroundColor", "blue");


    // 获取css属性
    console.log($("p").css("color"));

</script>
</html>
```





### 3.小结

1. 获取： css("属性名")
2. 设置：css("属性名", "属性值")



## 知识点-操作属性

### 1.目标

- [ ] 掌握JQ操作属性

### 2.讲解

| API方法            | 解释                                 |
| ------------------ | ------------------------------------ |
| attr(name,[value]) | 获得/设置属性的值                    |
| prop(name,[value]) | 获得/设置属性的值(checked，selected) |

+ attr与prop的注意问题

  ​	attr与prop是以jquery-1.6为界限

  ​	==checked 和 selected 建议 使用prop；获取其他使用attr获取==

```html
<body>
用户名：<input type="text" value="默认值" id="inpuId"><br>
密码：<input type="password"><br>
爱好：<input type="checkbox" checked>敲代码<input type="checkbox" >篮球<br>
籍贯：
<select>
    <!--默认第一个是包含了selected属性-->
    <option>北京</option>
    <option>深圳</option>
</select>
<br>
<input type="button" onclick="printData()" value="打印一些信息">
</body>
```

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
用户名：<input type="text" value="默认值" id="inpuId"><br>
密码：<input type="password"><br>
爱好：<input type="checkbox" checked>敲代码<input type="checkbox" >篮球<br>
籍贯：
<select>
    <!--默认第一个是包含了selected属性-->
    <option>北京</option>
    <option>深圳</option>
</select>
<br>
<input type="button" onclick="printData()" value="打印一些信息">
</body>

<script>
    function printData() {
        // 1. 打印用户名文本框的value属性
        // attr获得的属性值是设置好的属性值，并不是输入内容
        console.log($("#inpuId").attr("value"));
        // 2. 打印用户名文本框的输入内容
        console.log($("#inpuId").val());

        // 3. 获得复选框的属性值checked： 值是一个false布尔值
        console.log($("input[type=checkbox]").prop("checked"));

        // 4. 获得下拉框中被选中的selected的属性
        console.log($("option").prop("selected"));
    }

</script>
</html>
```




### 3.小结

1. jq对象.attr： 获取除了checked、selected属性之外的属性
2. jq对象.prop：获取checked、selected属性



## 知识点-操作DOM(操作内容、节点)

### 1.目标

- [ ] 掌握使用JQ操作DOM

### 2.路径

1. jQuery对DOM树中的文本和值进行操作 
2. jQuery创建,插入节点
3. jQuery移除节点

### 3.讲解

#### 3.1jQuery对DOM树中的文本和值进行操作

+ API

| API方法       | 解释                               |
| ------------- | ---------------------------------- |
| val([value])  | 获得/设置标签里面value属性相应的值 |
| text([value]) | 获得/设置元素的文本内容            |
| html([value]) | 获得/设置元素的标签体内容          |

+ 解释

```
val()       获得标签里面value属性的值   value属性的封装  
val("...")    给标签value属性设置值

html()      获得标签的内容，如果有标签，一并获得。    
html("....")  设置html代码，如果有标签，将进行解析。支持标签 封装了innerHTML


text()		获得标签的内容，如果有标签，忽略标签。
text("...")	设置文本，如果含有标签，把标签当做字符串.不支持标签
```

页面：

```html
<span style="width: 300px; height: 100px; border: 1px red solid">
        <font color="red">哈哈哈</font>
    </span>

用户名：<input type="text" id="inputId">
<input type="button" value="点击" onclick="setData()">

```

需求：

获得文本框输入内容、获得span的内容（分别采用text()、html()）、设置span的内容

实现：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../jquery-3.3.1.min.js"></script>
</head>
<body>
<span style="width: 300px; height: 100px; border: 1px red solid">
        <font color="red">哈哈哈</font>
    </span>

用户名：<input type="text" id="inputId">
<input type="button" value="点击" onclick="setData()">
</body>

<script>
    // 需求
    function setData() {
        //1. val()：获得文本框的输入内容，值； 传参就是设置值
        console.log($("#inputId").val());
        $("#inputId").val("哈哈哈") ;

        //2. 从span里面获取文本内容
        // text()获取的是纯文本内容  哈哈哈
        console.log($("span").text());
        //html() 获得包含html标签的内容    <font color="red">哈哈哈</font>
        console.log($("span").html());

        //3. 设置span的内容
        // text(内容)； 设置时是纯文本显示，不会渲染html标签
        //$("span").text("<font color='green'>呵呵呵</font>");

        // html(内容)； 设置时会渲染html标签
        $("span").html("<font color='green'>呵呵呵</font>");
    }

</script>
</html>
```



#### 3.2jQuery创建,插入

+ API   

| API方法                  | 解释                                                         |
| ------------------------ | ------------------------------------------------------------ |
| $("`<span>内容</span>`") | 创建span元素对象，并且包含"内容"<br>==$("<input>")创建一个input元素== |
| ==append(element)==      | 添加成最后一个子元素，两者之间是父子关系==主体是父元素==     |

+ 示例： 点击按钮给表格增加一行

- 准备

```html
<input type="button" value="添加一行" onclick="addLine()">
<table border="1px">
    <tr>
        <td>1</td>
    </tr>
</table>
```

- 实现

```js
<script>

    var i = 2;

    // 一点击则往table增加一行
    function addLine() {
        // 1. 创建一行
        var aaa = $("<tr><td>"+ (i++) + "</td></tr>");
        //2. 把这一行追加到table中
        $("table").append(aaa);
    }
</script>
```



#### 3.3jQuery移除节点(对象)

+ API

| API方法  | 解释                                     |
| -------- | ---------------------------------------- |
| remove() | 删除指定元素(自己移除自己)，调用者是自身 |
| empty()  | 清空指定元素的所有子元素，本身还在       |

- 准备

```html
<div>
    <span>span</span>
    <p>段落</p>
</div>

<input type="button" value="移除" onclick="remove()">
```



- 实现

```js
<script>
    function remove() {
        //1. 移除span---自己移除自己remove
        //$("span").remove();

        //2. 移除div自身内部元素，自身还在
        $("div").empty();
    }
</script>
```







### 4.小结

- 获取内容
  - val([value])： 获取/设置value的值，文本框的输入内容
  - text([value])：获取/设置文本内容；  内容包含html标签，原样展示
  - html([value])：获取/设置文本内容；  内容包含html标签，会在页面渲染显示效果
- 创建节点、追加节点
  - 创建：  `$("<span>内容</span>")`
  - 追加：  父对象.append(子对象)，都是jq对象
- 移除节点
  - remove()： 元素自身移除自身
  - empty()： 元素清空自身内部的内容，自身还在



## 知识点-遍历【掌握】

### 1.目标

- [ ] 掌握JQ遍历

### 2.路径

1. 复习JS方式遍历
2.  jquery对象方法遍历【关注】
3.  jquery的全局方法遍历
4. jQuery3.0新特性for  of语句遍历【关注】

### 3.讲解

#### 3.1 复习JS方式遍历

+ 语法

```js
for(var i=0;i<元素数组.length;i++){
  	元素数组[i];
}
```

+ eg

```
//定义了一个数组
var array = [1,2,3,4,"aa"];
//a. 使用原来的方式遍历
for(var i = 0; i < array.length;i++){
  console.log("array["+i+"]="+array[i]);
}
```

#### 3.2 jquery对象方法遍历【掌握】

+ 语法：

```js
jquery对象.each(
    function(index,element){
        
    }
);
```

- 需求：遍历输出一个数组的元素

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/jquery-3.3.1.min.js"></script>
</head>
<body>

</body>
    <script>
        // JS对象
        var arr = ["A", "b", 1223,   3.14]

        // js方式
        for (var i = 0; i < arr.length; i++) {
            console.log(i + ":" + arr[i]);
        }


        // jq对象
        // jq对象方式遍历
        $(arr).each(function (idx, ele) {
            console.log(idx + ":" + ele);
        })
    </script>
</html>
```

#### 3.3 jquery的全局方法遍历--了解

+ 语法

```js
$.each(
       jquery对象,
       function(index,element){
    
		}	
 );

其中，
index:就是元素在集合中的索引
element：就是集合中的每一个元素对象
```

+ 示例（该方式不太方便，知道就行）

```js
$.each($(arr), function (idx, ele) {
	console.log(idx + ":" + ele);
})
```

#### 3.4 jQuery3.0新特性for  of语句遍历【掌握】

+ 语法

```js
for(变量 of jquery对象){
  	变量；
}

其中，
变量:定义变量依次接受jquery数组中的每一个元素
jquery对象：要被遍历的jquery对象
```

+ eg

```js
// jq3语法
// ele就是每次遍历的元素； 遍历的是jq对象
for(ele of $(arr)){
    console.log(ele);
}
```

- 需要设置：

![1593609119140](img/1593609119140.png)

### 4.小结

1. jq对象的遍历

```js
jq对象.each(function(idx,ele){...})
```



2. for...of

```js
for(ele of jq对象){..}
```





# 第二章-JQ案例

## 案例:使用JQuery完成页面定时弹出广告 ##

### 一,需求

![img](img/tu_18-1574242791512.png)

- 需求描述： 进入页面3s后弹出广告，再过3s后广告隐藏 

### 二,思路分析 ###

//1. 过3秒钟显示图片， setTimeout("showImg()", 3000)

//2. showImg()函数里面就是要显示图片

```js
function showImg(){
    // 使用显示动画 
    // 弹出广告，再过3s后广告隐藏 ---就是显示动画完成后，再做事情
    jq对象.show(
        function(){
            //就是显示动画完成后，再做事情---再过3s后广告隐藏 
            setTimeout("hideImg()", 3000)
        }
    )
}


function hideImg(){
            // （逻辑就写隐藏图片）
            jq对象.hide()
        }
```



### 三,代码实现 ###

- 准备资料放在页面上面，有一张图一开始是隐藏的

```html
<!--display: none 隐藏-->
<div style="display: none" id="adId">

    <img src="../../img/ad.jpg" height="300px" width="100%"/>
</div>
```

- 实现

```js
/**
     * 广告案例代码。。。。
     */

    //进入页面3s后弹出广告，再过3s后广告隐藏
    setTimeout("showImg()", 3000);

    function showImg(){
        // 使用显示动画
        // 弹出广告，再过3s后广告隐藏 ---就是显示动画完成后，再做事情
        $("#adId").show(
            // function就是动画完成后要执行的逻辑
            function(){
                //就是显示动画完成后，再做事情---再过3s后广告隐藏
                setTimeout("hideImg()", 3000)
            }
        )
    }

    function hideImg(){
        // （逻辑就写隐藏图片）
        $("#adId").hide()
    }
```

### 四,小结

1. 采用了定时器setTimeout，过3秒钟去执行逻辑

2. 图片资源（div）显示、隐藏（show、hide）

   1. 显示完成之后做事情 

      1. ```js
         jq.show(function(){
             // 再过3秒钟隐藏
             //定时器setTimeout
             setTimeout("hideImg()", 3000)
         })
         
         function hideImg(){
             jq.hide()
         }
         ```



## 案例:使用JQuery完成表格的隔行换色 ##

### 一,需求分析 ###

![img](img/tu_15.png)

### 二,思路分析

1. 奇数偶数行背景色不一致
2. 奇偶选择器(:even、:odd)+css方法设置背景色



### 三,代码实现 ###

- 页面资料准备

```html
<table id="tab1" border="1" width="800" align="center" >

			<tr>
				<th><input type="checkbox"></th>
				<th>分类ID</th>
				<th>分类名称</th>
				<th>分类描述</th>
				<th>操作</th>
			</tr>
			<tr>
				<td><input type="checkbox"></td>
				<td>1</td>
				<td>手机数码</td>
				<td>手机数码类商品</td>
				<td><a href="">修改</a>|<a href="">删除</a></td>
			</tr>
			<tr>
				<td><input type="checkbox"></td>
				<td>2</td>
				<td>电脑办公</td>
				<td>电脑办公类商品</td>
				<td><a href="">修改</a>|<a href="">删除</a></td>
			</tr>
			<tr>
				<td><input type="checkbox"></td>
				<td>3</td>
				<td>鞋靴箱包</td>
				<td>鞋靴箱包类商品</td>
				<td><a href="">修改</a>|<a href="">删除</a></td>
			</tr>
			<tr>
				<td><input type="checkbox"></td>
				<td>4</td>
				<td>家居饰品</td>
				<td>家居饰品类商品</td>
				<td><a href="">修改</a>|<a href="">删除</a></td>
			</tr>
		</table>
```

- 实现

```html
<script>
    // 实现各行换色
    // 奇偶选择器+css操作样式
    $("tr:odd").css("background-color", "#ffb6c1");
    $("tr:even").css("background-color", "#4f81bd");
</script>
```

### 四,小结

1. 奇偶选择器
2. 操作样式  css(css属性,css属性值)



## 案例:使用JQuery完成复选框的全选效果 ##

### 一,需求分析 ###

![img](img/tu_14.png)

需求： 点击标题的复选框，可以实现其他行的复选框选中或者不选中

### 二.思路分析 ###

1. 给顶部行的复选框增加一个点击事件， changeChecked()

2. 实现changeChecked()

   1. ```js
      function changeChecked(){
          // 选中就是对checked属性赋值--操作checked属性-- 用prop方法
          // 可以通过class获取其他的复选框
          // 值就是顶部行的复选框的checked属性值  $("#id值").prop("checked")
          $(".clss属性值").prop("checked", 值就是顶部行的复选框的checked属性值)
      }
      ```

      

### 三,代码实现 ###

- 准备资料

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../../jquery-3.3.1.min.js"></script>


</head>
<body>
<table id="tab1" border="1" width="800" align="center" >

    <tr>
        
        <th><input type="checkbox"  id="topInput"></th>
        <th>分类ID</th>
        <th>分类名称</th>
        <th>分类描述</th>
        <th>操作</th>
    </tr>
    <tr>
        <td><input type="checkbox" class="inputClass"></td>
        <td>1</td>
        <td>手机数码</td>
        <td>手机数码类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>2</td>
        <td>电脑办公</td>
        <td>电脑办公类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>3</td>
        <td>鞋靴箱包</td>
        <td>鞋靴箱包类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>4</td>
        <td>家居饰品</td>
        <td>家居饰品类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
</table>
</body>

<script>
    // 实现各行换色
    // 奇偶选择器+css操作样式
    $("tr:odd").css("background-color", "#ffb6c1");
    $("tr:even").css("background-color", "#4f81bd");

</script>
</html>
```



- 实现


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../../jquery-3.3.1.min.js"></script>


</head>
<body>
<table id="tab1" border="1" width="800" align="center" >

    <tr>
        <!--1. 给顶部复选框增加点击事件函数-->
        <th><input type="checkbox" onclick="changeChecked()" id="topInput"></th>
        <th>分类ID</th>
        <th>分类名称</th>
        <th>分类描述</th>
        <th>操作</th>
    </tr>
    <tr>
        <td><input type="checkbox" class="inputClass"></td>
        <td>1</td>
        <td>手机数码</td>
        <td>手机数码类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>2</td>
        <td>电脑办公</td>
        <td>电脑办公类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>3</td>
        <td>鞋靴箱包</td>
        <td>鞋靴箱包类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox"  class="inputClass"></td>
        <td>4</td>
        <td>家居饰品</td>
        <td>家居饰品类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
</table>
</body>

<script>
    // 实现各行换色
    // 奇偶选择器+css操作样式
    $("tr:odd").css("background-color", "#ffb6c1");
    $("tr:even").css("background-color", "#4f81bd");



    // 2. 实现全选、全不选效果
    function changeChecked() {
        // 选中就是对checked属性赋值--操作checked属性-- 用prop方法
        // 可以通过class获取其他的复选框
        // 值就是顶部行的复选框的checked属性值  $("#id值").prop("checked")
        $(".inputClass").prop("checked", $("#topInput").prop("checked"))
    }
</script>
</html>
```

### 四,小结

1. 给顶部的复选框增加了一个点击事件，绑定一个函数
2. 函数里面写逻辑
   1. 设置其他复选框的checked属性值：  .prop("checked", 属性值)
   2. 属性值其实就是顶部的复选框的checked属性值



## 案例:使用JQuery完成省市联动效果

### 一,需求分析

![img](img/tu_2.png)



### 二,思路分析

1. 给第一个下拉框增加一个事件，onchange事件，写一个函数changeCitys()

2. 实现函数changeCitys()

   1. ```js
      function changeCitys(){
          // 就是把省对应的城市变成  <opton>城市名</option>
          //1. 获得省对应的城市列表  var citys = arr[下标]
          //2. 遍历城市列表数组
          for(ele of $(citys)){
              // ele就是城市名字 深圳，惠州，广州
              // 2.1 创建节点
              var optionObj = $("<option>"+ele+"</option>")
              // 2.2 追加到第二个下拉框
              第二个下拉框.append(optionObj)
          }
      }
      ```

      

### 三,代码实现

- 准备资料

```html
 <select id="provinceId">
        <option value="0">广东</option>
        <option value="1">湖南</option>
        <option value="2">湖北</option>
    </select>

    <select id="cityId">
        <option>请选择</option>
    </select>
```



- 实现


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../../jquery-3.3.1.min.js"></script>
</head>
<body>
<select id="provinceId" onchange="changeCity(this.value)">
    <option value="0">广东</option>
    <option value="1">湖南</option>
    <option value="2">湖北</option>
</select>

<select id="cityId">
    <option>请选择</option>
</select>
</body>

<script>
    var arr = [
        ["深圳", "广州", "惠州"],
        ["长沙", "岳阳", "常德"],
        ["武汉", "黄冈", "荆州"]
    ]

    //1. 采用二维数组实现
    //2. 给第一个select下拉框增加内容改变事件，函数叫做  changeCity()
    //3. changeCity实现逻辑
        //1. 获得省对应的城市列表（数组）
        //2. 遍历数组
            //1. 每次遍历的元素就是城市的名字-----》创建一个<option>城市名</option>
            //2. 追加到第二个select标签中

    function changeCity(pValue){
        // 【优化点1】每次进来需要清除旧的数据
        $("#cityId").html("<option>请选择</option>")


        // 1. 获得省对应的城市列表（数组）
        var citys = arr[pValue];
        // 2. 遍历数组
        for(cityName of $(citys)){
            // 创建元素
            var optionEle = $("<option>"+cityName+"</option>");
            // 追加到第二个select标签中
            $("#cityId").append(optionEle)
        }
    }

    // 【优化点2】 初始化
    // 参数是省下拉框的value值
    changeCity($("#provinceId").val());
</script>
</html>
```

### 四.小结

1. 首先要给第一个下拉框增加点击事件onchange事件
2. 事件函数
   1. 获得省对应的城市列表  arr[下标]，下标就是第一个下拉框的value值
   2. 遍历城市列表， jq3的for...of循环
   3. 每一个元素都是一个城市名字，创建节点变成<option>城市名</option>
   4. 追加到第二个下拉框
3. 优化
   1. 每次变更省都需要清除旧的数据
   2. 页面一进来就需要初始化数据，调用一次函数



## 扩展案例_表格换色

### 1.需求

![1564740946909](img/1564740946909.png) 



+ 鼠标进入tr的时候, 当前tr的背景色改成red
+ 鼠标离开tr的时候, 当前tr的背景色还原

### 2.技术

+ 选择器
+ 事件
+ css()

### 3.思路

1. 给tr设置鼠标进入事件

```
tr.mouseenter(function(){
  //1.设置背景色为red
});
```

2. 给tr设置鼠标离开事件

```
tr.mouseout(function(){
  //1.设置背景色为tr之前的颜色
});
```



## 扩展案例_电子时钟

### 1.需求

![image-20191217160708414](img/image-20191217160708414.png) 

### 2,思路

![image-20191217160914193](img/image-20191217160914193.png) 

### 3.实现

```
<body>
    <span id="spanId"></span>
</body>

<script>
    //1.创建定时任务
    setInterval("getTime()",1000);
    //2.创建getTime()
    function getTime() {
        //获得时间
        var timeStr =  new Date().toLocaleString();
        //向span插入
        $("#spanId").html(timeStr);
    }
</script>
```



# 总结

- jquery： 就是一个js，简化了js操作页面的代码

- 掌握的知识点

  - js、jq对象转换

    - js---》jq： $(js对象)
    - jq---》js： jq对象[0]

  - 基本选择器

    - id： $("#id属性值")
    - 类：$(".class属性值")
    - 标签: $("标签名")

  - 基本事件使用

    - 语法：  jq对象.事件方法名(function(){})

    - 示例：

      - ```js
        jq对象.click(
        	function(){
                ...
            }
        )
        ```

  - 显隐动画（广告案例用到了）

    - 显示： show(毫秒值, function(){})
    - 隐藏： hide(毫秒值, function(){})

  - 其他选择器

    - 后代： $("A B")
    - 奇偶：  选择奇数行、偶数行示例：$("tr:odd")、$("tr:even")
    - 属性： 获得文本框元素， $("input[type=text]")

  - jq操作样式：  css(name[,value])： 获取/设置css属性值

  - 操作属性：

    - attr()
    - prop()： 获取checked、selected属性

  - 操作DOM

    - 创建节点： `$("<span>内容</span>")`
    - 追加节点： 父元素.append(子元素)
    - 删除节点： remove、empty

  - jq的遍历

    - jq对象的方法遍历

      - ```js
        jq对象.each(function(idx,ele){
            
        })
        ```

    - jq3的特性：for...of

      - ```js
        for(ele of jq对象){
            
        }
        ```

- 案例：每一个都做一下





# 回顾要点

1. jq、js对象转换

2. 基本选择器

3. 事件

5. 显隐动画  show 、hide

6. 操作样式  css方法

7. 操作内容    html、text、val

9. 创建元素、追加元素

8. 操作属性   attr、prop

9. 后代、属性、奇偶选择器

10. 遍历

    ```js
    // jq对象遍历
    jq对象.each(function(index,element){})
    
    // jq3用法
    for(element of jq对象){}
    ```

    

案例都要做





# 预习要点

jq的ajax用法

json语法格式







jquery插件网站：  https://www.jq22.com/