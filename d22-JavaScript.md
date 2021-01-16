---
typora-copy-images-to: img
---

# day21-JavaScript

# 学习目标

- [ ] 了解js的作用
- [ ] 能够在HTML里引入js
- [ ] 能够使用浏览器开发者工具调试js代码
- [ ] 掌握js的基本语法
- [ ] 能够定义和调用函数==【重要】==
- [ ] 能够使用js的内置对象
- [ ] 能够绑定js事件==【重要】==
- [ ] 能够使用正则表达式校验字符串格式
- [ ] 能够使用js的bom操作浏览器
- [ ] 能够使用js的dom操作网页==【掌握】==

# 第一章-JS基础

## 知识点-JS简介

### 1.目标

* 了解JS的概念和特点

### 2.路径

1. 什么是JS

2. JS的作用

3. JS的组成

### 3.讲解

#### 3.1. 什么是JS

* JS，全称JavaScript，是一种==解释性==脚本语言，是一种动态类型、==弱类型（根据值可以推导数据类型）==、基于原型的语言，内置支持类型。 

* JS语言和Java语言对比：

| 对比           | Java                   | JS                       |
| -------------- | ---------------------- | ------------------------ |
| 运行环境       | JVM虚拟机              | JS引擎，是浏览器的一部分 |
| 是否跨平台运行 | 跨平台                 | 跨平台                   |
| 语言类型       | 强类型语言             | 弱类型，动态类型语言     |
| 是否需要编译   | 需要编译，是编译型语言 | 不需要编译，是解释型语言 |
| 是否区分大小写 | 区分大小写             | 区分大小写               |

#### 3.2 JS的作用

具体来说，有两部分作用：

* JS代码操作浏览器：进行网址跳转、历史记录切换、浏览器弹窗等等

* JS代码操作网页：操作HTML的标签、标签的属性、样式等等




就是可以通过js代码操作网页。

#### 3.3 JS的组成

* ECMAScript(核心)：是JS的基本语法规范
* BOM：Browser Object Model，浏览器对象模型，提供了与浏览器进行交互的方法
* DOM：Document Object Model，文档对象模型，提供了操作网页的方法

### 4. 小结

1. js： 解释性、弱类型的脚本语言
2. 组成：
   1. 语法核心
   2. BOM
   3. DOM

## 知识点-HTML引入JS

### 1.目标

* 能够在HTML里引入js

### 2. 分析

1. 内部js(内嵌式)
2. 外部js(外联式)
3. 注意事项

### 3.讲解

#### 3.1. 内部JS

##### 3.1.1语法

* 在html里增加`<script>`标签，把js代码写在标签体里;  ==一般可以写在body的后面。==

```html
<script>
	//在这里写js代码
</script>

示例：
<script>
    //操作浏览器弹窗
    alert("hello, world");
</script>
```

##### 3.1.2示例

* 创建html页面，编写js代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>js引入方式-内部js</title>
    <script>
        //操作浏览器弹窗
        alert("hello, world");
    </script>
</head>
<body>

</body>
</html>
```

* 打开页面，浏览器会弹窗

![1571279916433](img/1571279916433.png)

#### 3.2. 外部JS

##### 3.2.1语法

* 把js代码写在单独的js文件中，js文件后缀名是`.js`
* 在HTML里使用`<script>`标签引入外部js文件

```html
<script src="js文件的路径"></script>
```

##### 3.2.2示例 

* 创建一个`my.js`文件，编写js代码

  * 第1步：创建js文件

  ![1599367387198](img/1599367387198.png)

  * 第2步：编写js代码

![1599367410517](img/1599367410517.png)

* 创建一个html，引入外部的js文件--直接拖拽即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--外部方式，引入外部的js文件-->
    <script src="a.js"></script>
    <script src="js/b.js"></script>
</head>
<body>

</body>
<script>
    // js代码（内联式）
    alert("警告框！")
</script>
</html>
```

* 打开页面，浏览器会弹窗，会分别执行a.js、b.js、script标签的js代码



#### 3.3 注意事项

- 一个`script`标签，不能既引入外部js文件，又在标签体内写js代码。

  - 错误演示

  ```html
  <script src="../js/my.js">
  	alert("hello");
  </script>
  ```

  - 正确演示

  ```html
  <script src="../js/my.js"></script>
  <script>
  	alert("hello");
  </script>
  ```



### 4.小结

#### 4.1语法

1. html中引入js
   1. 内部方式，在body后面写一个标签<script>
   1. 外部方式，写一个js文件，拖拽进来



## 实操-JS小功能和JS调试

### 1. 目标

* 能够使用浏览器的F12调试js

### 2.路径

1. 小功能
2. 调试

### 3.讲解

#### 3.1小功能

+ alert(): 弹出警示框
+ console.log(): 浏览器控制台打印日志 

![](img/1592788843521.png)



+ document.write(); 文档打印. 向页面输出内容. 

#### 3.2.调试

1. 按`F12`打开开发者工具
2. 找到`Source`窗口，在左边找到页面，如下图
   * 打断点之后，当代码执行到断点时，会暂时执行
   * 在窗口右侧可以查看表达式的值、单步调试、放行等等

![1571577895543](img/1571577895543.png)



3. 如果代码执行中出现异常信息，会在控制台`Console`窗口显示出来

![1571578078067](img/1571578078067.png)

4. 点击可以定位到异常位置

![1571578109301](img/1571578109301.png)

### 4.小结

1. 小功能：
   1. 警告框  alert()
   2. 浏览器控制台打印日志 console.log()
   3. 在页面输出内容：document.write()
2. 调试
   1. F12打开控制台，找到sources，代码中打断点



## 知识点-JS基本语法

### 1.目标

- [ ] 掌握JS基本语法

### 2.路径

1. 变量
2. 数据类型
3. 运算符
4. 语句

### 3.讲解

#### 3.1变量

+ JavaScript 是一种==弱类型语言==，javascript的变量类型由它的值来决定。 定义变量需要用关键字 `var`、`let`

```js
int i = 10;   		var i = 10;        或者 i = 10;
String a = "哈哈";   var str = "哈哈";  或者 str = "哈哈";  或者 str = "哈哈"
...


注意:
	1.var可以省略不写,建议保留
	2.最后一个分号可以省略,建议保留
	3.同时定义多个变量可以用","隔开，公用一个‘var’关键字. var c = 45,d='qwe',f='68';
```

#### 3.2数据类型

1.五种原始数据类型

| 数据类型    | 描述       | 示例                  |
| ----------- | ---------- | --------------------- |
| `number`    | 数值类型   | `1`, `2`, `3`, `3.14` |
| `boolean`   | 布尔类型   | `true`, `false`       |
| `string`    | 字符串类型 | `"hello"`, 'hello'    |
| `object`    | 对象类型   | `new Date()`,  `null` |
| `undefined` | 未定义类型 | var a;                |

2.typeof操作符

+ 作用： 用来判断变量是什么类型

+ 写法：typeof(变量名) 或 typeof 变量名

+ null与undefined的区别：

  ​	null: 对象类型，已经知道了数据类型，但对象为空。
  ​	undefined：未定义的类型，并不知道是什么数据类型。

3.小练习

+ 定义不同的变量,输出类型,

![img](img/tu_13.png)

+ 代码

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
    <script>
        /*
    ctrl+shift+/  注释快捷键
     */
    var aa = 10;       // number
    var bb = 3.14;     // number
    var cc = true;     // boolean
    var dd = "你好";    // string
    var ee = null;      // object
    var ff;             // undefined
    // 打印每个变量的数据类型
    console.log(typeof(aa));
    console.log(typeof(bb));
    console.log(typeof(cc));
    console.log(typeof(dd));
    console.log(typeof(ee));
    console.log(typeof(ff));

    </script>
</html>
```

字符串转换成数字类型 

+ 全局函数(方法)，就是可以在JS中任何的地方直接使用的函数，不用导入对象。不属于任何一个对象

![img](img/tu_14.png)



#### 3.3运算符

+ 关系运算符:> >= < <=

- number类型和字符串做`-,*,/`的时候,字符串自动的进行类型转换,前提字符串里面的数值要满足**number**类型。  那  `+`  号呢-----------------加号会拼接。

```js
<script>
        var a = 3;
        var b = "2";

        console.log(a * b);  //  6
        console.log(a / b);  //  1.5   ： js的/符号，得到小数结果
        console.log(a - b);  //  1

        console.log(a + b);  //  32   ： js的+号，存在字符串，则成了拼接
    </script>
```

- 除法,保留小数  

```js
<script>
        var a = 3;
        var b = "2";

        console.log(a * b);  //  6
        console.log(a / b);  //  1.5   ： js的/符号，得到小数结果
        console.log(a - b);  //  1

        console.log(a + b);  //  32   ： js的+号，存在字符串，则成了拼接
    </script>
```

- `==` 比较数值,	`=== `  恒等于符号，比较数值和类型

```js
var c = 123;
var d = "123";
console.log(c == d);   //  true，值的比较
console.log(c === d);  //  false， 值、数据类型的比较


```

#### 3.4语句

+ for循环

```js
// 在页面输出一个九九乘法表
    // document.write
    for (var i = 1; i <= 9 ; i++) {
        for (var j = 1; j <= i ; j++) {
            document.write(j+"*"+i+"="+i*j+"  ");
        }
        // 换行
        document.write("<br>")
    }
```

+ if... else

```js
var a = 6;
if(a==1)
{
    alert('语文');
}
else if(a==2)
{
    alert('数学');
}
else
{
    alert('不补习');
}
```

+ switch

```js
<script>
	var str = "java"; 

	switch (str){
		case "java":
			alert("java");
			break;
		case "C++":
			alert("C++");
			break;

		case "Android":
			alert("Android");
			break;	}
</script>
```



### 4.小结

1. 变量： 全部使用var关键字定义
2. 数据类型
   1. 数值 number
   2. 字符串string
   3. 布尔boolean
   4. 对象oject
   5. 未定义类型undefined（仅仅定义，没有赋值）
3. 运算符
   1. `+` 拼接数值、字符串
   2. `==`、`===`（值、类型）
4. 语句



## 知识点-函数【重要】

### 1. 目标

* 掌握js函数的定义和调用

### 2.路径

1. 什么是函数
2. 普通函数
3. 匿名函数

### 3.讲解

#### 3.1. 什么是函数

* 函数： 是被设计为执行特定任务的代码块 ，在被调用时会执行 
* 函数类似于Java里的方法，用于封装一些可重复使用的代码块

#### 3.2. 普通(有名)函数

##### 3.2.1语法

* 定义普通函数

```js
function 函数名(形参列表){
    函数体
    [return 返回值;]
}
```

* 调用普通函数

```js
var result = 函数名(实参列表);
```

##### 3.2.2示例

* 计算两个数字之和

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>

<script>
    // 定义函数，语法
    // 获取2个数值的和
    function sum(a, b) {
        // 需要返回值，则写return语句
        return a+b;
    }

    // 函数调用
    console.log(sum(1, 2));

    // 匿名函数
    // 把一个函数赋值给变量sum1，意味着sum1就是一个函数
    var sum1 = function(a,b) {
        return a+b;
    }
    console.log(sum1(1, 2));



    function sum(a,b,c) {
        return a-b;
    }

    // 被后面的sum(a,b,c)覆盖了： 1-2 = -1
    console.log(sum(1, 2));
</script>
</html>
```



#### 3.3匿名函数

匿名函数，也叫回调函数，类似于Java里的函数式接口里的方法，主要用在后面的事件绑定中。

```js
var a = function(形参列表){
    函数体
    [return 返回值;]
}
```



#### 3.4 不存在重载函数，会被覆盖

```js
// 获取2个数值的和
function sum(a, b) {
    // 需要返回值，则写return语句
    return a+b;
}

function sum(a,b,c) {
        return a-b;
    }

    // 被后面的sum(a,b,c)覆盖了： 1-2 = -1
    console.log(sum(1, 2));
```



### 4.小结

1. 函数声明语法

   ```js
function 函数名(形参列表){
       函数体
       return 值;
   }
   ```
   
   





## 知识点-JS事件【重要】

### 1. 目标

* 掌握事件的使用

### 2.路径

1. 事件介绍
2. 常见事件
3. 事件绑定

### 3.讲解

#### 3.1. 事件介绍

* HTML 事件是发生在 HTML 元素上的“事情”， 是浏览器或用户做的某些行为的概括，比如点击一个按钮就发生了一个点击事件。
* **事件通常与函数配合使用，这样就可以通过发生的事件来驱动函数执行。** 

#### 3.2 常见事件

| 属性        | 此事件发生在何时...                  |
| ----------- | ------------------------------------ |
| onclick     | 当用户点击某个对象时调用的事件句柄。 |
| ondblclick  | 当用户双击某个对象时调用的事件句柄。 |
| onchange    | 域的内容被改变。                     |
| onblur      | 元素失去焦点。                       |
| onfocus     | 元素获得焦点。                       |
| onload      | 一张页面或一幅图像完成加载。         |
| onsubmit    | 确认按钮被点击；表单被提交。         |
| onkeydown   | 某个键盘按键被按下。                 |
| onkeypress  | 某个键盘按键被按住。                 |
| onkeyup     | 某个键盘按键被松开。                 |
| onmousedown | 鼠标按钮被按下。                     |
| onmouseup   | 鼠标按键被松开。                     |
| onmouseout  | 鼠标从某元素移开。                   |
| omouseover  | 鼠标移到某元素之上。                 |
| onmousemove | 鼠标被移动。                         |

#### 3.3. 事件绑定

##### 3.3.1普通函数方式

==方式一: 设置标签的 事件属性==

```html
<标签  事件属性="js代码，函数名"></标签>

// 示例:
<input type="button" onclick="funcName()"/>
```

##### 3.3.2匿名函数方式

==方式二: 派发事件 匿名函数;  标签对象.事件属性 = function(){}==

```html
<script>
    标签对象.事件属性 = function(){
        //执行一段代码
    }
    
    // 示例
    元素.onclick = function(){
        // 代码
    }
</script>
```

#### 3.4事件使用

##### 3.4.1重要的事件

1. ==点击事件==  onclick
2. ==焦点事件== onfocus--获得焦点、onblur --失去焦点（一般用在文本输入框中）
3. ==内容改变事件== onchange（针对select标签）
4. 键盘事件  onkeydown (键盘按下) 、  onkeyup （键盘弹起）
5. 鼠标事件  onmouseover （鼠标移入）、onmouseout （移出）、onmousedown  （按下）
6. 页面加载完成事件  onload（body的事件）



+ 点击事件

  需求: 点击一次按钮 弹出信息

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--方式一：
    给元素增加事件属性，比如点击事件，  onclick="调用某个函数的js语法"
 -->
<input type="button" value="点击我" onclick="clickFun01()"><br>
<input type="button" value="点击我2" id="bt2"><br>
</body>

<script>
    // 定义函数
    function clickFun01() {
        alert("被点击了...")
    }


    // 方式二： 使用匿名函数派发事件
    // 标签对象.事件属性 = function(){...}
    // 获得对象（根据id属性的值获得标签dom对象）
    var btEle = document.getElementById("bt2");
    btEle.onclick = function () {
        alert("被点击了222...")
    }


</script>
</html>
```

+ 获得焦点(onfocus)和失去焦点(onblur)

  需求:给输入框设置获得和失去焦点
  
  ==this表示产生事件的当前对象（标签）==

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--获得焦点事件 onfocus、失去焦点事件 onblur-->
<input type="text" onfocus="_onfocus()" onblur="_onblur()"><br>

<!--this就是当前文本框对象-->
<input type="text" onblur="printValue(this)" id="inputId" ><br>

</body>

<script>
    /*js代码*/
    // 1. 获得焦点操作
    function _onfocus() {
        // 在控制台打印一句话
        console.log("获得了焦点...")
    }
    // 2. 失去焦点操作
    function _onblur() {
        console.log("失去了焦点...")
    }

    // 3. 在失去焦点时打印文本框里面的输入内容
    // this 关键字;  obj就是this就是文本输入框
    function printValue(obj) {
        // 打印文本框里面的内容
        // 获得id值的元素
        var inputEle = document.getElementById("inputId");
        // 拿到输入的内容
        console.log(inputEle.value);

        console.log("obj.value:"+obj.value)
    }


</script>
</html>
```

+ 内容改变(onchange)

  需求: 给select设置内容改变事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--内容改变事件：onchange-->
<select onchange="_onchange(this.value)">
    <option value="bj">北京</option>
    <option value="sz">深圳</option>
    <option value="sh">上海</option>
</select>
</body>

<!--给下拉框增加内容改变事件，内容一旦改变，则打印输出当前选择的值-->
<script>
    function _onchange(val) {
        console.log(val);
    }
</script>
</html>
```

+ 等整个页面加载完成(onload)  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<script>
    alert("1111")
</script>
<!--页面加载事件-->
<body onload="loadFunc()">

</body>
<script>
    alert("2222")

    function loadFunc() {
        alert("3333")
    }
</script>
</html>
```

##### 3.4.2掌握的事件

+ 键盘相关的, 键盘按下(onkeydown)  键盘抬起(onkeyup)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" onkeydown="_onkeydown()" onkeyup="_onkeyup()">
</body>

<script>
    // 给文本输入框增加键盘按下、弹起事件，分别输出信息即可
    function _onkeydown() {
        console.log("按下了...")
    }

    function _onkeyup() {
        console.log("弹起了...")
    }
</script>

</html>
```

+ 鼠标相关的, 鼠标在xx之上(onmouseover ), 鼠标按下(onmousedown),鼠标离开(onmouseout)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        /*标签选择器*/
        div{
            width: 500px;
            height: 300px;
            background-color: red;
        }
    </style>
</head>
<body>
<div onmouseover="_onmouseover()" onmouseout="_onmouseout()" onmousedown="_onmousedown()">

</div>
</body>

<script>
    // 给一个div增加鼠标移入、移出、鼠标按下事件
    function _onmouseover() {
        console.log("移入了...")
    }
    function _onmouseout() {
        console.log("移出了...")
    }
    function _onmousedown() {
        console.log("按下了...")
    }
</script>
</html>
````



### 4.小结

1. 事件绑定语法

   1. 方式一： 标签中增加事件属性

      1. ```html
         <input type="button" onclick="函数()"/>
         ```
   
   2. 方式二： 匿名函数派发事件
   
      1. ```js
         // 方式二： 使用匿名函数派发事件
             // 标签对象.事件属性 = function(){...}
             // 获得对象（根据id属性的值获得标签dom对象）
             var btEle = document.getElementById("bt2");
             btEle.onclick = function () {
                 alert("被点击了222...")
             }
         ```
   
         
   
2. 常用的事件

   1. 重要
      1. 点击：  onclick
      2. 获得焦点： onfocus、失去焦点onblur
      3. 内容改变： onchange
   2. 了解
      1. 键盘相关：按下、弹起 onkeydown、onkeyup
      2. 鼠标事件：移入、移出、按下  onmouseover、onmouseout、onmousedown
      3. 页面加载事件onload



## 知识点-正则表达式

### 1. 目标

- [ ] 掌握js正则表达式校验字符串格式的方法

### 2.路径

1. 正则表达式概述
2. 正则表达式的语法
3. 使用示例

### 3.讲解

#### 3.1正则表达式概述

​	正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

​	用我们自己的话来说: ==正则表达式用来校验字符串是否满足一定的规则的公式==

#### 3.2 正则表达式的语法

##### 3.2.1创建对象

* 对象形式：`var reg = new RegExp("正则表达式")`
* ==直接量形式：`var reg = /^正则表达式$/;`==

##### 3.2.2调用方法

| 方法                          | 描述             | 参数           | 返回值                    |
| ----------------------------- | ---------------- | -------------- | ------------------------- |
| `正则表达式变量.test(string)` | 校验字符串的格式 | 要校验的字符串 | boolean，校验通过返回true |

##### 3.2.3常见正则表达式规则

| **符号**    | **作用**                              |
| ----------- | ------------------------------------- |
| \d          | 数字                                  |
| \D          | 非数字                                |
| \w          | 单词：a-zA-Z0-9_                      |
| \W          | 非单词                                |
| .           | 通配符，匹配任意字符                  |
| {n}         | 匹配n次                               |
| {n,}        | 大于或等于n次                         |
| {n,m}       | 在n次和m次之间，==n,m之间不要有空格== |
| +           | 1~n次                                 |
| *           | 0~n次                                 |
| ?           | 0~1次                                 |
| ^           | 匹配开头                              |
| $           | 匹配结尾                              |
| [a-zA-Z]    | 英文字母；[]表示里面字符任意取一个    |
| [a-zA-Z0-9] | 英文字母和数字                        |
| [*xyz*]     | 字符集合, 匹配所包含的任意一个字符    |



#### 3.3使用示例

需求:

1. 出现任意数字3次
2. 只能是英文字母的, 出现6~10次之间
3. 只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
4. 手机号码: 以1开头, 第二位是3,4,5,6,7,8,9的11位数字

步骤:

1. 创建正则表达式
2. 调用test()方法

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>

<script>
    //需求:
    // 1. 出现任意数字3次
    // 步骤
    // 创建对象
    var reg1 = /^\d{3}$/;
    // 调用test方法
    console.log(reg1.test("123"));

    //2. 只能是英文字母的, 出现6~10次之间
    var reg2 = /^[a-zA-Z]{6,10}$/;
    console.log(reg2.test("qwerty"));
    //3. 只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
    var reg3 = /^[a-zA-Z][a-zA-Z0-9]{3,15}$/;
    console.log(reg3.test("y4bjh"));
    //4. 手机号码: 以1开头, 第二位是3,4,5,6,7,8,9的11位数字
    var reg4 = /^1[3456789]\d{9}$/;
    console.log(reg4.test(13999999999));
</script>
</html>
```

### 4.小结

1. 使用步骤
   1. 创建对象  var reg = /^表达式参考表格来写$/
   2. 调用reg.test()方法





## 知识点-内置对象之Array数组【重点】

### 1. 目标

* 掌握数组Array的使用

### 2. 路径

1. 创建数组对象
3. 数组的常用方法

### 3. 讲解

#### 3.1 创建数组对象

##### 3.1.1语法

* `var arr = new Array(size)`
* `var arr = new Array(element1, element2, element3, ...)`
* ==`var arr = [element1, element2, element3, ...];`   推荐方式==

##### 3.1.2数组的特点

* 数组中的每个元素可以是任意类型
* 数组的长度是可变的，更加类似于Java里的集合`List`

##### 3.1.3示例

* 创建数组，并把数组输出到浏览器控制台上
  * 说明：把数据输出到控制台：`console.log(value)`

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<script>
    // 定义数组的语法

    // 方式一： new Array(size)
    var arr1 = new Array(3);
    arr1[0] = 1;
    console.log(arr1);

    // 方式二： new Array(元素,元素..)
    var arr2 = new Array(1,2,3,4);
    console.log(arr2);

    // 方式三【推荐方式】
    var arr3 = [1,2,3];
    console.log(arr3);


    // 数组的特征演示
    // 数据类型任意
    var arr4 = [1,2,3.14,"hello"];
    console.log(arr4);

    // 长度可变，没有所谓数组越界一说
    arr4[7] = 777;
    console.log(arr4);


    // 属性 长度：length
    console.log(arr4.length);

    // 方法：
    // concat 连接
    console.log(arr3.concat(arr4));

    // join 可以把数组中的元素用指定的分隔符拼接起来得到一个字符串
    console.log(arr4.join("#"));

    // reverse颠倒数组，会改变影响原来的数组
    console.log(arr4.reverse());
    console.log(arr4);



    

</script>
</html>
```

#### 3.2数组常见的方法（了解）

##### 3.2.1API介绍

| 方法      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| concat()  | 连接两个或更多的数组，并返回结果。                           |
| join()    | 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。 |
| reverse() | 颠倒数组中元素的顺序。                                       |

##### 3.2.2示例

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<script>
    // 定义数组的语法

    // 方式一： new Array(size)
    var arr1 = new Array(3);
    arr1[0] = 1;
    console.log(arr1);

    // 方式二： new Array(元素,元素..)
    var arr2 = new Array(1,2,3,4);
    console.log(arr2);

    // 方式三【推荐方式】
    var arr3 = [1,2,3];
    console.log(arr3);


    // 数组的特征演示
    // 数据类型任意
    var arr4 = [1,2,3.14,"hello"];
    console.log(arr4);

    // 长度可变，没有所谓数组越界一说
    arr4[7] = 777;
    console.log(arr4);


    // 属性 长度：length
    console.log(arr4.length);

    // 方法：
    // concat 连接
    console.log(arr3.concat(arr4));

    // join 可以把数组中的元素用指定的分隔符拼接起来得到一个字符串
    console.log(arr4.join("#"));

    // reverse颠倒数组，会改变影响原来的数组
    console.log(arr4.reverse());
    console.log(arr4);



    

</script>
</html>
```

#### 3.3二维数组

1. 二维数组------数组里面的元素是一维数组
2. 示例

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>

    <script>
        // 二维数组： 元素是一维数组的数组
        var citys = [
            ["广州","深圳","惠州"],
            ["长沙", "常德"],
            ["武汉","宜昌"]
        ];

    </script>
</html>
```



### 4.小结

1. 数组的定义
1. 推荐用方法： var arr = []
2. 属性、方法
   1. 长度 length
   2. 方法：连接concat、拼接元素join、反转数组reverse
3. 二维数组
   1. 定义： var arr = [ [], [] ]



## 知识点-内置对象之-Date日期(了解)

### 1. 目标

* 掌握日期Date的使用

### 2.路径

1. 创建日期对象
2. 日期常用方法

### 3.讲解

#### 3.1. 创建日期对象

##### 3.1.1语法

* 创建当前日期：`var date = new Date()`

* 创建指定日期：`var date = new Date(年, 月, 日)`   

  ==注意：月从0开始，0表示1月==

* 创建指定日期时间：`var date = new Date(年, 月, 日, 时, 分, 秒)`

  注意：月从0开始，0表示1月

##### 3.1.2示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
    <script>
        // 创建当前时间的日期对象
        var date = new Date();
        console.log(date.toLocaleString());

        // 月从0开始（0-11）
        var date2 = new Date(2020, 8, 9)
        console.log(date2.toLocaleString());

        // 一些方法
        // 年
        console.log(date.getFullYear());
        // 月从0开始（0-11）
        console.log(date.getMonth());
        console.log(date.getDate());

    </script>
</html>
```

![1571473450137](img/1571473450137.png)

#### 3.2. 日期常用方法

##### 3.2.1API介绍

| 方法                 | 描述                                         |
| :------------------- | :------------------------------------------- |
| Date()               | 返回当日的日期和时间。                       |
| **getDate()**        | 从 Date 对象返回一个月中的某一天 (1 ~ 31)。  |
| getDay()             | 从 Date 对象返回一周中的某一天 (0 ~ 6)。     |
| **getMonth()**       | 从 Date 对象返回月份 (0 ~ 11)。              |
| **getFullYear()**    | 从 Date 对象以四位数字返回年份。             |
| **getHours()**       | 返回 Date 对象的小时 (0 ~ 23)。              |
| **getMinutes()**     | 返回 Date 对象的分钟 (0 ~ 59)。              |
| **getSeconds()**     | 返回 Date 对象的秒数 (0 ~ 59)。              |
| **toLocaleString()** | 根据本地时间格式，把 Date 对象转换为字符串。 |

##### 3.1.2示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<script>
    // 创建对象
    var today = new Date();
    console.log(today);


    // toLocaleString() 得到本地化的时间格式
    console.log(today.toLocaleString());

    // 方法：获取年、月、日、时分秒（月是从0开始，0-31）

    console.log(today.getFullYear());
    console.log(today.getMonth());
    console.log(today.getDate());
    console.log(today.getHours());
    console.log(today.getMinutes());
    console.log(today.getSeconds());

</script>
</html>
```

### 4.小结

1. 使用Date对象
   1. new Date()
   2. API： 本地字符串 toLocaleString()

# 第二章-JS的BOM

## 知识点-JS的BOM

### 1.目标

- [ ] 掌握BOM中常用的API

### 2.路径

1. BOM介绍
2. BOM里面的五个对象

### 3.讲解

#### 3.1概述

​	Browser Object Model ,浏览器对象模型. 为了便于对浏览器的操作，JavaScript封装了对浏览器中各个对象，使得开发者可以方便的操作浏览器中的各个对象。

#### 3.2.BOM里面的五个对象

- windows：浏览器窗口
- navigator：包含了浏览器的信息
- screen：包含用户屏幕的信息
- history：包含浏览器历史记录信息
- location：浏览器地址栏信息

![1599374217553](img/1599374217553.png)

##### 3.2.1window: 窗体对象

==windows提供了一些全局函数，使用window的函数时，windows可以省略==

| **方法**                         | **作用**                                                     |
| -------------------------------- | ------------------------------------------------------------ |
| **alert()**                      | **显示带有一段消息和一个确认按钮的警告框**                   |
| **confirm()**                    | 显示带有一段消息以及确认按钮和取消按钮的对话框；确认框（点击确定返回true，点击取消返回false） |
| **setInterval('函数名()',time)** | 按照指定的周期（以毫秒计）来调用函数或计算表达式；定时器     |
| **setTimeout('函数名()',time)**  | 在指定的毫秒数后调用函数或计算表达式；  一次执行             |
| clearInterval(参数)              | 取消由 setInterval() 设置的 Interval()。==需要传入对应的setInterval方法的返回值，作为参数== |
| clearTimeout(参数)               | 取消由 setTimeout() 方法设置的 timeout。==需要传入对应的setInterval方法的返回值，作为参数== |

需求：点击一个按钮开始在控制台不断地输出信息，点击另一个按钮可以停止输出信息。

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--需求：
    点击按钮来停止对应的setInterval、setTimeout
-->
<input type="button" onclick="stopInterval()" value="stopInterval">
<input type="button" onclick="stopTimeout()" value="stopTimeout">

</body>
<script>
    // 警告框
    //alert("这是警告框");


    // 确认框（点击确定返回true，点击取消返回false）
    //var flag = confirm("你是帅哥吗?");
    //console.log(flag);


    // 周期性执行，定时器（表示每个1秒钟就执行func01()函数）
    // 参数1： 调用某个函数的语法，参数2：毫秒值
    var interval = setInterval("func01()", 1000);

    function func01() {
        console.log("aaa");
    }


    // 到点执行一次，过5秒钟执行函数func02(),且执行一次
    var timeout = setTimeout("func02()", 5000);

    function func02() {
        console.log("bbb");
    }


    // 停止setInterval计时器
    function  stopInterval() {
        // 参数是对应的setInterval计时器的返回值
        clearInterval(interval)
    }
    // 停止setTimeout计时器
    function  stopTimeout() {
        // 参数是对应的setTimeout计时器的返回值
        clearTimeout(timeout)
    }

</script>
</html>
```



##### 3.2.2,navigator:浏览器导航对象【了解】

| 属性                 | 作用                       |
| -------------------- | :------------------------- |
| navigator.appName    | 返回浏览器的名称           |
| navigator.appVersion | 返回浏览器的平台和版本信息 |

##### 3.2.3,screen:屏幕对象【了解】

| 方法          | 作用                       |
| ------------- | -------------------------- |
| screen.width  | 返回显示器屏幕的分辨率宽度 |
| screen.height | 返回显示屏幕的分辨率高度   |

##### 3.2.4,history:历史对象【了解】

| 方法              | 作用                            |
| ----------------- | ------------------------------- |
| history.back()    | 加载 history 列表中的前一个 URL |
| history.forword() | 加载 history 列表中的下一个 URL |

##### 3.2.5,location:当前路径信息【掌握href属性用法】

| 属性         | 作用                                |
| :----------- | ----------------------------------- |
| host         | 设置或返回主机名和当前 URL 的端口号 |
| **==href==** | 设置或返回完整的 URL                |
| port         | 设置或返回当前 URL 的端口号         |

==location.href：获得路径==

==location.href = "http://www.baidu.com";  设置路径,跳转到百度页面==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
    <script>

        // 获取浏览器地址栏的URL路径 location.href
        console.log(location.href);

        setTimeout("func()", 2000)

        function func(){
            // 实现页面的跳转
            location.href = "http://www.baidu.com";
        }


    </script>
</html>
```



### 4.小结

1. BOM: 浏览器对象模型，js可以操作浏览器
2. window
   1. alert：警告框
   2. confirm:确认框
   3. 定时器 setInterval("调用函数的语法func01()", 毫秒值)
   4. 定时器 setTimeout("调用函数的语法func02()", 毫秒值)；  仅仅调用一次
3. location
   1. href可以获得浏览器地址栏信息，
   2. location.href=""，设置浏览器地址栏信息，页面跳转





# 第三章-JS的DOM

## 知识点-DOM介绍

### 目标

* 了解dom相关的概念

### 分析

1. 什么是dom
2. 什么是dom树

### 讲解

#### 1. 什么是dom

* DOM：**D**ocument **O**bject **M**odel，文档对象模型。是js提供的，用来访问网页里所有内容的(标签,属性,标签的内容)

#### 2. 什么是dom树

* 当网页被加载时，浏览器会创建页面的DOM对象。DOM对象模型是一棵树形结构：网页里所有的标签、属性、文本都会转换成节点对象，按照层级结构组织成一棵树形结构。
  * 整个网页封装成的对象叫`document`
  * 标签封装成的对象叫`Element`
  * 属性封装成的对象叫`Attribute`
  * 文本封装成的对象叫`Text`

```html
<html>
    <head>
        <title>网页</title>
    </head>
    <body>
        <font color="red">文字描述</font>
    </body>
</html>
```

加载到浏览器内存后，形成了一课文档树形式：

一切皆节点, 一切皆对象

![1596721610780](img/1596721610780.png)



### 小结

1. dom： 就是js提供的文档对象模型，提供了一些API操作页面的所有信息
2. dom树： 标签、属性、文本内容---》节点

## 知识点-操作标签

### 1. 目标

*  能够使用dom操作标签

### 2. 分析

1. 获取标签
2. 操作标签

### 3.讲解

#### 3.1. 获取标签

| 方法                                         | 描述                            | 返回值          |
| -------------------------------------------- | ------------------------------- | --------------- |
| `document.getElementById(id)`                | 根据id获取标签                  | `Element`对象   |
| `document.getElementsByName(name)`           | 根据标签name属性值获取一批标签  | `Element`类数组 |
| `document.getElementsByTagName(tagName)`     | 根据标签名称获取一批标签        | `Element`类数组 |
| `document.getElementsByClassName(className)` | 根据类名class属性值获取一批标签 | `Element`类数组 |

环境准备：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .redClass{
            color: red;
        }
    </style>
</head>
<body>
    用户名：<input id="inputId" type="text">
<br>
    性别: <input type="radio" name="sex">男<input type="radio" name="sex">女
    <br>
    <p>段落一</p>
    <p class="redClass">段落二</p>
    <p class="redClass">段落三</p>
</body>

</html>

```

实现需求：

- 通过id获取元素
- 通过name属性值获得标签
- 通过标签名字获取
- 通过类名class获取

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .redClass{
            color: red;
        }
    </style>
</head>
<body>
用户名：<input id="inputId" type="text">
<br>
性别: <input type="radio" name="sex">男<input type="radio" name="sex">女
<br>
<p>段落一</p>
<p class="redClass">段落二</p>
<p class="redClass">段落三</p>
</body>


<script>
    //实现需求：

    //1 通过id获取元素
    // document.getElementById("inputId")  通过id属性值获得一个对象
    var inputEle = document.getElementById("inputId");
    console.log(inputEle);

    //2 通过name属性值获得标签
    var elementsByName = document.getElementsByName("sex");
    console.log(elementsByName);

    //3 通过标签名字获取
    var elementsByTagName = document.getElementsByTagName("p");
    console.log(elementsByTagName);

    //4 通过类名class获取
    var elementsByClassName = document.getElementsByClassName("redClass");
    console.log(elementsByClassName);
</script>
</html>

```



#### 3.2. 操作标签

| 方法                                    | 描述                  | 返回值        |
| --------------------------------------- | --------------------- | ------------- |
| `document.createElement(tagName)`       | 创建标签              | `Element`对象 |
| `parentElement.appendChild(sonElement)` | 往父元素中插入子元素  |               |
| `element.remove()`                      | 删除标签,自己移除自己 |               |
| `document.createTextNode("文本内容")`   | 创建文本              |               |

环境准备：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
    <input type="button" value="增加文本" id="btId">
    <span id="spanId">                 </span>
    
    <hr/>
    <input type="button" value="删除节点" id="btId2">
    <p>
        <font id="pf">段落p的font内容</font>
    </p>
</body>

    
</html>
```

需求：

点击按钮一，给span增加内容：`<font>hello</font>`；

点击按钮二，删除p里面的font标签

代码实现：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
<input type="button" value="增加文本" id="btId" onclick="addTxt()">
<span id="spanId">                 </span>

<hr/>
<input type="button" value="删除节点" id="btId2" onclick="removeNode()">
<p>
    <font id="pf">段落p的font内容</font>
</p>
</body>

<script>
    // 1. 点击按钮一，给span增加内容：<font>hello</font>
    function addTxt() {
        // 给span增加内容：<font>hello</font>
        // 父元素是span，子元素需要创建

        // 拿到父元素
        var spanEle = document.getElementById("spanId");

        // 追加子元素
        // 创建子元素
        var fontEle = document.createElement("font");  // <font></font>
        // 创建文本节点
        var textNode = document.createTextNode("hello");  // hello
        // 把文本节点加进font节点中
        fontEle.appendChild(textNode);  // <font>hello</font>

        spanEle.appendChild(fontEle);


    }


    // 2. 点击按钮二，删除p里面的font标签
    function removeNode() {
        // 自己删除自己
        var fontEle = document.getElementById("pf");
        fontEle.remove();

    }
</script>
</html>
```



### 4.小结

1. 获得标签
   + document.getElementById("id”)   根据id获得
   + document.getElementsByTagName("标签名")  根据标签名获得
   + document.getElementsByClassName("类名")  根据类名获得
2. 操作节点(标签,文本)
   + `document.createElement(tagName)`  创建标签  `Element`对象  
   + `document.createTextNode("文本")`  创建文本节点
   + `parentElement.appendChild(sonElement)`  插入标签 
   +    `element.remove()`  删除标签    

## 知识点-操作标签体

### 1. 目标

* 掌握操作标签体内容的方法

### 2. 路径

* 获取/设置标签体内容

### 3. 讲解

#### 3.1语法

* 获取标签体内容：`标签对象.innerHTML`
* 设置标签体内容：`标签对象.innerHTML = "新的HTML代码";`
  * `innerHTML`是覆盖式设置，原本的标签体内容会被覆盖掉；==可以清除原来的所有内容，用新内容覆盖。==
  * 支持标签的 可以插入标签, 设置的html代码会生效
* innerHTML：
              可以获取html标签
              覆盖之前的内容
              内容可以包含html代码，会在页面渲染出来
* innerText:
              获取的是纯文本，不包含html标签
              设置的时候如果包含html代码，是不会在页面渲染的

#### 3.2示例

环境准备：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    
    <span><font color="green">AAA</font></span>
    <input type="button" value="获取内容" onclick="func()"><br>
    <input type="button" value="设置内容" onclick="func2()"><br>
   
</body>
    
</html>
```

需求：获得span标签的内容、设置span的内容

实现：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<span><font color="green">AAA</font></span>
<input type="button" value="获取内容" onclick="func()"><br>
<input type="button" value="设置内容" onclick="func2()"><br>

</body>

<script>
    // 需求：获得span标签的内容、设置span的内容

    // 获取span元素（数组的下标0表示第一个元素，就是我们的span）
    var spanEle = document.getElementsByTagName("span")[0];


    // 获取内容
    function func() {
        // innerHTML:可以获取html标签
        // <font color="green">AAA</font>
        console.log(spanEle.innerHTML);

        // innerText:获取的是纯文本，不包含html标签
        // AAA
        console.log(spanEle.innerText);
    }

    // 设置内容
    function func2() {
        // innerHTML设置内容： 把标签、属性渲染到页面
        //spanEle.innerHTML = "<font color='red'>BBB</font>";


        // innerText设置内容：以纯文本展示，不会渲染样式到页面
        spanEle.innerText = "<font color='red'>BBB</font>";
    }

</script>
</html>
```





### 4. 小结

1. 设置标签体内容
   1. innerHTML： 支持html渲染
   2. innerText：以纯文本展示

## 知识点-操作属性

### 1. 目标

* 能够操作HTML标签的属性

### 2.路径

1. 使用JS操作标签的属性

### 3. 讲解

* 每个标签`Element`对象提供了操作属性的方法

| 方法名                              | 描述       | 参数             |
| ----------------------------------- | ---------- | ---------------- |
| `getAttribute(attrName)`            | 获取属性值 | 属性名称         |
| `setAttribute(attrName, attrValue)` | 设置属性值 | 属性名称, 属性值 |
| `removeAttribute(attrName)`         | 删除属性   | 属性名称         |

环境准备：准备一张图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <img src="../img/b.jpg"/>
    <br>
    <input type="button" value="获得属性" onclick="getSrc()">
    <input type="button" value="设置属性" onclick="setSrc()">
    <input type="button" value="删除属性" onclick="removeSrc()">
</body>
</html>
```

需求：

​    点击按钮1，获得img的src属性；

​    点击按钮2设置src属性；

​    点击按钮3删除src属性

实现：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <img src="../img/b.jpg" id="imgId"/>
    <br>
    <input type="button" value="获得属性" onclick="getSrc()">
    <input type="button" value="设置属性" onclick="setSrc()">
    <input type="button" value="删除属性" onclick="removeSrc()">
</body>

    <script>
        var imgEle = document.getElementById("imgId");
        function getSrc() {
            alert(imgEle.getAttribute("src"))
        }
        function setSrc() {
            imgEle.setAttribute("src", "../img/c.png")
        }
        function removeSrc() {
            imgEle.removeAttribute("src")
        }
    </script>
</html>
```



### 4.小结

1. `getAttribute(attrName)`  获取属性值 
2.  `setAttribute(attrName, attrValue)`  设置属性值 
3. `removeAttribute(attrName)`  删除属性



# 第四章-JS案例

## 案例-使用JS完成表单的校验

### 1. 案例需求

![1596811110095](img/1596811110095.png)

- 用户名输入框,手机号码 , 获得焦点的时候给用户提示,  失去焦点进行 校验
- ==用户名==：
  - 点击时，在文本框的后面提示： 只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
  - 输入完毕，离开时，进行校验，校验结果在文本框后面展示
- 手机号【作业，课后完成】：
  - 点击时，在后面提示： 以1开头, 第二位是3,4,5,7,8的11位数字
  - 离开时，校验
  - ​    /^1[34578]\d{9}$/

### 2.思路分析

1. 用户名校验

   1. 增加获得焦点事件，在后面提示文本即可（span的内容设置成提示语）

   2. 失去焦点
   
      1. ```js
         // 1 写一个函数绑定失去焦点事件
         function checkUsername(obj){
             // 创建正则
             // test方法(obj)
             // OK， 在后面增加一个提示
             // 失败， 在后面提示
         }
         ```
   
      2. 
   
2. 手机号码自己搞定





### 3.代码实现

+ HTML

```html
<tr>
                            <td class="left">用户名：</td>
                            <td class="center">
                                 <input id="username" name="user" type="text" class="in"
                                    onfocus="showTips()"
                                        onblur="checkUsername(this.value)"
                                 />
                                <!--提示信息就可以在这儿里显示-->
                                 <span id="usernamespan"></span>
                            </td>
                        </tr>
```

+ JS代码

```js
<script>
    var spanEle = document.getElementById("usernamespan");
    // 获得焦点，进行提示
    function showTips() {
        // 使用innerHTML设置内容
        spanEle.innerHTML="只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头"

    }

    // 失去焦点，检查合法
    function checkUsername(username) {
        // 正则匹配
        // 只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
        var reg = /^[a-zA-Z][a-zA-Z0-9]{3,15}$/;
        // 匹配输入的内容
        var flag = reg.test(username);

        if(flag){
            spanEle.innerHTML = "<img src=\"img/gou.png\" height=\"31\" width=\"40\"/>"
        }else{
            spanEle.innerHTML = "<font color='red'>用户名非法</font>"
        }

    }
    </script>
```

> 做一下验证手机号码,email格式  var reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;

### 4,小结

1. 获得和失去焦点
2. 函数
3. 操作标签体
4. 获得标签, 获得标签的value

## 案例-使用JS完成图片轮播效果

### 1.需求分析

![img](img/tu_17.png)

- 实现每过3秒中切换一张图片的效果，一共3张图片，当显示到最后1张的时候，再次显示第1张。

### 2.思路分析

1. 使用setInterval("changeImg()", 3000)： 每个3秒调用函数

2. 实现函数：changeImg()

   1. ```js
      function changeImg(){
          // 本质就是修改img的src属性(根据id获取元素)
          // 资源： banner_1.jpg、banner_2.jpg、banner_3.jpg
          // banner_i.jpg ， i从1--2--3---1--2--3...
          // 维护一个变量i,大于3时重新改为1
      }
      ```

      



### 3.代码实现

```js
// 前提给第四行图片设置id属性值为 imgId
<!--第四行，一个td，一个img-->
        <tr>
            <td>
                <img src="../../img/banner_1.jpg" height="300" width="100%" id="imgId">
            </td>
        </tr>



<script>
    // 定时器
    setInterval("changeImg()", 3000);


    var i = 1;
    function changeImg() {
        // 本质就是修改img的src属性(根据id获取元素)
        var imgEle = document.getElementById("imgId");


        // 资源： banner_1.jpg、banner_2.jpg、banner_3.jpg
        // banner_i.jpg ， i从1--2--3---1--2--3...
        // 维护一个变量i,大于3时重新改为1

        i++;
        if(i > 3){
            // 维护一个变量i,大于3时重新改为1
           i = 1;
        }
        imgEle.setAttribute("src", "../../img/banner_" + i + ".jpg");

    }

    </script>
```

### 4.小结

1. 定时任务  

```
setInterval("", 毫秒值)
```

2. 操作属性

```
设置img的src属性： imgEle.setAttribute("src", "../img/banner_"+ i + ".jpg")
```



## 案例-JS控制二级联动

### 1.需求分析

![1596804462521](img/1596804462521.png)

- 需求：
  - 第一个下拉框改变时，第二个下拉框随之显示其对应的城市列表
  - 二维数组来实现
- 环境，初始页面素材如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    籍贯：
    <select id="pId">
        <option value="0">广东</option>
        <option value="1">湖南</option>
        <option value="2">湖北</option>
    </select>
    <select id="citySelect">
        <option>--请选择--</option>

    </select>
</body>

    <script>
        var arr = [
            ["深圳", "惠州", "东莞"],
            ["长沙", "衡阳", "常德", "郴州"],
            ["武汉", "黄冈", "宜昌"]
        ]
    </script>
</html>
```



### 2.思路分析

1. 第一个下拉框增加内容改变事件，onchange="changeCitys()"

2. 函数changeCitys()逻辑

3. ```js
   // 获取省对应的城市列表（数组）
   // 遍历--》每一个城市名字： 深圳、惠州、东莞
   // 转化成 <option>深圳</option
   // 需要把option追加到第二个下拉框
   ```
### 3.代码实现

1. 给第一个select设置onchange事件函数changeCity()

2. 在changeCity()函数里面实现逻辑：

3. ```js
   function changeCity(provinceValue){
       // 通过省的值，获得城市列表
       // 遍历城市列表--》每一个城市变成 <option>城市名称</option>
           // 创建文本节点，内容就是城市名称
           // 创建option节点
           // 追加文本节点到option节点 ---  <option>城市名称</option>
       // 追加option节点到select节点
   }
   ```

4. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
籍贯：
<select id="pId" onchange="changeCitys(this.value)">
    <option value="0">广东</option>
    <option value="1">湖南</option>
    <option value="2">湖北</option>
</select>
<select id="citySelect">
    <option>--请选择--</option>

</select>
</body>

<script>
    var arr = [
        ["深圳", "惠州", "东莞"],
        ["长沙", "衡阳", "常德", "郴州"],
        ["武汉", "黄冈", "宜昌"]
    ]

    // 实现函数逻辑
    function changeCitys(pVal){
        // 获取省对应的城市列表（数组）
        var citys = arr[pVal];

        // 获取第二个下拉框
        var citySelect = document.getElementById("citySelect");

        // 遍历--》每一个城市名字： 深圳、惠州、东莞
        for (var i = 0; i < citys.length; i++) {
            // 每一个元素都是一个城市的名字
            // citys[i]  // 深圳、惠州、东莞

            // 转化成 <option>深圳</option>

            // 创建节点   <option></option>
            var optionEle = document.createElement("option");
            // 创建文本节点
            var textNode = document.createTextNode(citys[i]);
            optionEle.appendChild(textNode); //  <option>深圳</option> 格式


            // 需要把option追加到第二个下拉框
            citySelect.appendChild(optionEle)


        }


    }
</script>
</html>
```



- 优化版本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
籍贯：
<select id="pId" onchange="changeCitys(this.value)">
    <option value="0">广东</option>
    <option value="1">湖南</option>
    <option value="2" selected>湖北</option>
</select>
<select id="citySelect">
    <option>--请选择--</option>

</select>
</body>

<script>
    var arr = [
        ["深圳", "惠州", "东莞"],
        ["长沙", "衡阳", "常德", "郴州"],
        ["武汉", "黄冈", "宜昌"]
    ]


    // 获取第二个下拉框
    var citySelect = document.getElementById("citySelect");


    // 【初始化--页面一旦加载，则把下拉框的省对应的城市初始化】
    //changeCitys(0);  不能写死
    // 获得第一个下拉框的默认选项的值
    var value = document.getElementById("pId").value;
    // 调用函数进行初始化
    changeCitys(value);


    // 实现函数逻辑
    function changeCitys(pVal){

        // 【优化-----每次一进来就把旧的内容给清除】
        // 每次省一旦变更，则清除第二框的内容
        citySelect.innerHTML = "<option>--请选择--</option>"



        // 获取省对应的城市列表（数组）
        var citys = arr[pVal];



        // 遍历--》每一个城市名字： 深圳、惠州、东莞
        for (var i = 0; i < citys.length; i++) {
            // 每一个元素都是一个城市的名字
            // citys[i]  // 深圳、惠州、东莞

            // 转化成 <option>深圳</option>

            // 创建节点   <option></option>
            var optionEle = document.createElement("option");
            // 创建文本节点
            var textNode = document.createTextNode(citys[i]);
            optionEle.appendChild(textNode); //  <option>深圳</option> 格式


            // 需要把option追加到第二个下拉框
            citySelect.appendChild(optionEle)


        }


    }
</script>
</html>
```



### 4.小结

1. 内容改变事件

```
onchange事件
```

2. 二维数组
3. innerHTML
   + 可以把旧内容删除，新内容覆盖
4. dom
   + 创建元素节点、文本、追加子节点





# 回顾要点

1. 事件函数绑定重要

   ```html
   函数
   		1. 定义语法
   			function 函数名(参数列表){
   				// 函数体代码
   				[return 语句]
   			}
   	事件
   		2. 常用事件
   			重要事件
   			1. 点击：  onclick
   			2. 焦点事件： 获得焦点 onfocus、失去焦点 onblur
   			3. 内容改变事件： 发生在select标签， onchange
   			普通事件
   			4. 键盘事件  按下去 onkeydown、 弹起来 onkeyup
   			5. 鼠标事件 onmouseover 移入、onmouseout 移出、onmousedown 按下
   			6. 页面加载完成后  onload，会用在body里面
   	事件绑定
   		方式一： 设置事件属性  
   			onXXX =  "函数名()"
   			定义函数
   		方式二： 给对象设置事件属性
   			// 获取元素
   			var ele = document.getElementById("id属性值")
   			// 设置属性
   			ele.onclick = function(){...}
   ```

2. 直接做二级联动--综合练习

3. 回顾DOM操作获取标签





# 预习要点

- 安装tomcat、环境配置
- IDEA中配置tomcat
- XML方式配置servlet、注解方式配置servlet