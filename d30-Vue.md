---
typora-copy-images-to: img
---

# day30-Vue

# 学习目标

- [ ] 了解vue
- [ ] 掌握vue常用系统指令
- [ ] 了解vue生命周期： created
- [ ] 掌握vue的ajax的使用
- [ ] 能够完成显示所有联系人案例
- [ ] 能够完成增加联系人案例
- [ ] 能够完成删除联系人案例

# 第一章-VueJS介绍与快速入门

## 知识点-VueJS介绍   

### 1.目标

- [ ] 了解vue（音标同  View）

### 2.路径

1. 什么是VueJS
2. VueJS特点
3. MVVM模式

### 3.讲解

#### 3.1什么是VueJS

​	Vue.js是一个渐进式JavaScript 框架。Vue.js 的目标是通过尽可能简单的 API==实现响应的数据绑定==和组合的视图组件。它不仅易于上手，还便于与第三方库或既有项目整合。

​	渐进式：主张最少使用-- 意思就是你可以只用我的一部分，而不是用了我这一点就必须用我的所有部分。

​	官网:https://cn.vuejs.org/

#### 3.2特点

+ 易用
+ 灵活
+ 高效

#### 3.3MVVM模式

**MVC**: （方式一：用户从view操作请求controller，返回model，渲染view；  方式二：用户请求controller，返回model，渲染view）



![1593826634912](img/1593826634912.png)

![1593826881445](img/1593826881445.png)

**MVVM**:  （==采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然==）

![1593826759289](img/1593826759289.png)

​	MVVM是Model-View-ViewModel的简写。它本质上就是MVC 的改进版。

​	MVVM 就是将其中的View 的状态和行为抽象化，让我们将视图UI 和业务逻辑分开. MVVM模式和MVC模式一样，主要目的是分离视图（View）和模型（Model）。

​	Vue.js 是一个提供了 MVVM 风格的==双向数据绑定==的 Javascript 库，专注于View 层。它的核心是 MVVM 中的 VM，也就是 ViewModel（==**vue就是ViewModel**==）。——==**意味着vue对象一旦发生改变，则视图view和模型model同时发生改变！，三者任意一个改变，其他二者随之改变！**==

​	 ViewModel负责连接 View 和 Model，保证视图和数据的一致性，这种轻量级的架构让前端开发更加高效、便捷.

![1552546278610](img/1552546278610.png)



### 4.小结

1. vue就是一个渐进式的js框架





## 案例-VueJs快速入门【重视】

### 1.需求

​	使用vue，对message赋值，并把值显示到页面

### 2.步骤

1. 引入vue
2. 写一个html标签，div一般会设置id属性值为app
3. 在js里面创建Vue对象，和div进行绑定

### 3.实现

0. 技术

==创建vue对象语法==：

```js
//创建一个Vue实例(VM)
var vue = new Vue({
    // div通常用作容器
    // 表示当前vue对象接管了div区域， 这里的#app是页面id属性值为app的一个div元素
    // el 表示vue对象接管的目标元素
    el: '#app',

    //data定义数据，是js对象格式{key:value,key:value}
    data: {
        message: 'hello world'
    },
    // methods定义方法，是对象形式，对象里面每一个属性都是方法名
    // 定义了一个init的方法
    methods: {
        init: function () {
            console.log("init方法");
            // 返回值
            return "haha"
        }
    }
});
```

==html里面获取vue对象的数据语法：使用插值表达式：  {{ 表达式 }}==

```html
{{ data里面的对象的key获取 }}
如： {{ message }}。
```

==【扩展一下：】js里面获取vue对象的数据语法：vue对象.$data.属性名  或者 vue对象.data里面的属性名==

**实现：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--1. 引入vue.js-->
    <script src="js/vuejs-2.5.16.js"></script>
</head>
<body>
<!--2. 写一个div，id设置为app-->

<!--快捷键：   div#app tab键-->
<div id="app">
    <!--插值表达式-->
    {{message}}
</div>

<!--只有在绑定的div里面才能够使用插值表达式获取值-->
{{message}}
</body>

<script>
    //  3. 写js，创建Vue对象
    var vue = new Vue({
        /*
        el属性： 可以绑定Vue对象到目标元素
         */
        el: "#app",
        /**
         * data属性： 定义Vue对象自身数据的属性
         */
        data: {
           message: "hello..."
        },
        /**
         * methods属性：定义Vue对象自身的方法
         */
        methods:{
            init: function () {
                alert("哈哈哈init...")
            }
        }

    });

    // 【扩展一下：】js里面获取vue对象的数据
    console.log(vue.$data.message);
    console.log(vue.message);
</script>
</html>
```





![1593828733537](img/1593828733537.png)

### 4.小结

​	步骤

1. 引入vuejs

2. 写一个div，id属性设置为app

3. 在js中写代码，创建Vue对象

   1. ```js
      new Vue({
          el: "#app",
          data: {
              key:value
          },
          methods:{
              aaa:function(){
                  
              }
          }
      })
      ```

      

   

# 第二章-VueJS 常用系统指令

​		html标签的属性值不能使用插值表达式、直接使用vue定义的方法，比如`<input type="text" value="{{message}}"`行不通，所以要是用==vue提供的指令——也就是vue的属性(元素的属性)==。



## 知识点-常用的事件【重点】

### 1.目标

- [ ] 掌握常用的事件（属性）

### 2.路径

1. @click
2. @keydown
3. @mouseover

### 3.讲解

#### 3.1@click

说明: 点击事件(等同于**v-on:click**属性)

==用法：@click="调用函数的语法init()"==

==类似于添加了一个点击事件，但是要求函数在vue对象的methods里面定义==。

```html
<div id="app">
    <input type="button" value="点击改变" @click="fun01"/>
</div>

<script>
        //创建vue实例
        new Vue({
            //表示当前vue对象接管了div区域
            el:'#app',

            //定义数据
            data:{
                message:'hello world'
            },
            //定义函数
            methods:{
                fun01:function () {
                    // 这里的this是vue对象
                    this.message = '你好,世界...';
                }
            }
        });

    </script>
```



【需求】：点击按钮事件，改变message的值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>
</head>
<body>
<!--写一个div，id属性设置为app-->
<div id="app">

    {{message}}
    <!--vue的指令 @click=“调用函数的语法vue对象中定义的方法”
        vue的事件指令就是把原来的js的事件名去除了on，加了@符号：
            @click、@focus、@blur.....
    -->
    <input type="button" @click="changeVal()" value="改变值">
</div>
</body>

<script>
    // 创建vue对象
    new Vue({
        el: "#app",
        data:{
            message: "hello..."
        },
        methods:{
            init:function () {
               alert(111) 
            },
            changeVal: function () {
                // 改变message的内容
                // this就是当前Vue对象
                this.message = "哈哈哈"
            }
        }
    });

</script>
</html>
```

#### 3.2@keydown

说明: 键盘按下事件(等同于v-on:keydown)

@keydown="函数名()"， 还可以传入一个参数：$event------获取当前事件对象比如：`@keydown="函数名($event)"`

==事件本身使用 `$event` 来获取; 事件.preventDefault()方法可以阻止事件执行。==

```js
func02: function (e) {
    // 如果是键盘事件，则可以获得ascii码
    // 如果你的电脑不标准、用了外设也不标准。。。你的键盘的值不一样
    var keyCode = e.keyCode;   
    if(keyCode >= 48 && keyCode<= 57){   
        // 阻止事件执行
        e.preventDefault()
    }
}
```



【需求】：对文本输入框做校验，使用键盘按下事件，如果按下0-9的数字，阻止事件执行；其他按键则，正常显示。

+ demo03.js

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>
</head>
<body>


<div id="app">
    请输入：<input type="text" @keydown="checkInput($event)">
</div>

</body>

<script>
    /*
    对文本输入框做校验，使用键盘按下事件，如果按下0-9的数字，阻止事件执行；其他按键则，正常显示。
     */
    new Vue({
        el: "#app",
        data:{

        },
        methods:{
            checkInput: function (e) {
                // e代表当前事件本身
                // 键盘事件，存在一个属性 keyCode获取键盘的ASCII码值
                var keyCode = e.keyCode;
                console.log(keyCode);
                // 判断
                if(keyCode>=48 && keyCode<=57){
                    // 0-9的数字，需要阻止事件
                    e.preventDefault()
                }
            }
        }
    })
</script>
</html>
```

#### 3.3@mouseover

说明:鼠标移入区域事件(等同于v-on:mouseover)

【需求1】：鼠标移到div中，弹出窗口。

+ demo04.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>
    <style>
        .box {
            width: 300px;
            height: 400px;
            border: 1px solid red;
        }

    </style>

</head>
<body>
<div id="app">
    <div class="box" @mouseover="tips()">
        div
    </div>
</div>
</body>

<script>

    var vue = new Vue({
        // el 表示接管了id为app对应的div对象
        el: "#app",
        data: {},
        methods: {
            tips:function () {
                alert("进入了...")
            }
        }
    })


</script>
</html>
```

### 4.小结

1. 事件指令就是把原来的js的事件名去除了on，加了@：  onclick----》@click
2. 语法：  @click="调用函数的语法func()"， func在vue对象的methoos中定义




## 知识点- v-text与v-html

### 1.目标

- [ ] 掌握v-text与v-html的使用

### 2.讲解

==v-text：输出文本内容，不会解析html元素==

==v-html：输出文本内容，会解析html元素==

==***使用：用在标签的属性里面：  v-text="值是vue的data中定义的数据"***==

【需求】：分别使用v-text, v-html 获取vue中定义的 `<font>hello world<font>` ，查看页面输出内容。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>

</head>
<body>
<div id="app">
    <!--
        v-text是把内容直接显示，以纯文本方式显示，如果包含html不会渲染
        v-html是把内容渲染显示，如果包含html则会渲染，展示效果
    -->
    <span v-text="textContent"></span>
    <span v-html="textContent"></span>
</div>
</body>

<script>

    var vue = new Vue({
        // el 表示接管了id为app对应的div对象
        el: "#app",
        data: {
            textContent:"<font color='red'>hello world</font>"
        },
        methods: {}
    })


</script>
</html>
```

### 3.小结

1. v-text： 以纯文本显示数据，不会渲染html标签
2. v-html：会渲染html标签
3. 使用位置：  放在标签中，作为属性，属性值写的是vue对象中data中定义的数据的key

## 知识点-v-bind和v-model【重点】

### 1.目标

- [ ] 掌握v-bind和v-model

### 2.路径

1. v-bind
2. v-model

### 3.讲解

#### 3.1v-bind

==插值语法不能作用在HTML 属性上==，遇到这种情况应该使用 v-bind指令

==用法： 在html'属性前加上v-bind:，比如 v-bind:color，就可以获取vue定义的数据用了。==

==**也可以简写为:color，省略v-bind**==

【需求】：使用vue的数据分别对字体颜色、超链接地址赋值。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>
</head>
<body>

<div id="app">


    <!--
        v-bind 指令，可以在属性中，把属性值设置为vue对象中定义的数据
        用法：  v-bind:属性名、     :属性名="vue的data中定义的数据"
    -->
    <font v-bind:color="sz102">我是中国人，我爱我的祖国！</font><br>
    <a :href="sz102link">连接到百度</a>

</div>

</body>

<script>
    new Vue({
        el: "#app",
        data: {
            sz102: "red",
            sz102link: "https://www.baidu.com/"
        },
        methods: {

        }
    })
</script>
</html>
```

#### 3.2v-model【重要】

用于数据的绑定,数据的读取

==v-model可以实现页面元素数据、vue对象数据的双向绑定，双向变化==

```html
<form>
    <!-- 直接用v-model属性替换value，实现数据的双向绑定了;  -->
    用户名:<input v-model="user.name">
</form>
```



【需求】：使用vue赋值json(对象)数据，并显示到页面的输入框中（表单回显）. 点击按钮，改变json数据，验证同时输入框的内容也发生改变。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>

</head>
<body>
<div id="app">

    <!--
        v-model 指令： 实现页面数据、vue对象数据的双向绑定
            页面变化---》vue的数据变化
            vue的数据变化---》页面变化
    -->
    用户名：<input type="text" v-model="user.username">
    <br>
    <input type="button" value="改变值" @click="changeValue()">
    <br>

    {{user}}

</div>
</body>

<script>


    /*
    使用vue赋值对象数据，并显示到页面的文本输入框中（表单回显）.
    点击按钮，改变json数据，验证同时输入框的内容也发生改变。
     */
    var vue = new Vue({
        // el 表示接管了id为app对应的div对象
        el: "#app",
        data: {
            user:{
                username:"zsf",
                password: "123",
                age: 149
            }
        },
        methods: {
            changeValue: function () {
                // 改变json数据（用户数据），验证同时输入框的内容也发生改变。
                this.user.username = "张三"
            }
        }
    })


</script>
</html>
```

### 4.小结

1. v-bind： 实现标签的任意属性使用vue定义的数据
   1. 用法：   v-bind:属性名="值就是vue中定义的数据"
   2. 简写：   :属性名="值就是vue中定义的数据"
2. v-model： 实现数据的双向绑定：vue的数据变化引起页面内容改变，页面内容改变又会影响vue的数据变化
   1. 用法： v-model="vue中定义的数据"







## 知识点-v-for,v-if,v-show

### 1.目标

- [ ] 掌握v-for,v-if,v-show的使用

### 2.路径

1. v-for
2. v-if
3. v-show

### 3.讲解

#### 3.1 v-for【重点】

用于操作array/集合，遍历

语法:  `v-for="(元素,index) in 数组/集合"`

==<标签名 v-for="(element, index)  in 可迭代对象">{{ element }}</标签名>==

```html
// arr有多少个元素，name就会遍历多少次，也就会生成多少个li标签
<li v-for="(ele, idx) in arr">
    	// 插值表达式获取
		索引 {{ idx }} 的值为 {{ ele }} 
</li>
```



【需求】：使用v-for遍历数组，并把数据遍历到页面上的`<li>`标签中。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>

</head>
<body>
<div id="app">

    <!--
        使用v-for遍历数组，并把数据遍历到页面上的`<li>`标签中。

        v-for="(ele, idx) in arr"：有多少元素就会在页面输出多少个当前的标签
        在标签体中可以使用插值表达式{{}}获取元素{{ele}}
    -->
    <ul>
        <li v-for="(ele, idx) in arr">
            索引为{{idx}}的值为:  {{ele}}
        </li>
    </ul>


    <!--
        一个表格： 展示用户数据
    -->
    <table border="1px">
        <tr>
            <td>用户名</td>
            <td>年龄</td>
        </tr>
        <!--获得vue中定义的用户数据集合，在表格中显示对应的属性-->
        <tr v-for="(ele, idx) in users">
            <td>{{ele.username}}</td>
            <td>{{ele.age}}</td>
        </tr>

    </table>

</div>
</body>

<script>

    var vue = new Vue({
        // el 表示接管了id为app对应的div对象
        el: "#app",
        data: {
            arr: [
                "aaa","bbb","ccc"
            ],
            // 定义用户集合
            users:[
                {
                    username:"zs",
                    age:18
                },
                {
                    username:"ls",
                    age:19
                },
                {
                    username:"ww",
                    age:20
                }
            ]
        },
        methods: {}
    })


</script>
</html>
```

v-for的小结：

```html
<tr v-for="(ele,idx) in vue里面定义的数组形式 arr">
    <!--如果ele是一个对象-->
	<td>{{ele.key}}</td>
    <td>{{ele.key}}</td>
</tr>
```



#### 3.2v-if【重点】与v-show

v-if是根据表达式的值来决定是否渲染元素(标签都没有了)：==是否生成元素==

v-show是根据表达式的值来切换元素的display css属性(标签还在)： ==显隐元素==

==二者的属性值都是布尔类型==，所以需要在vue对象中定义布尔类型的数据。

用法：

```html
<span v-if="flag">span1</span>
<span v-show="flag">span2</span>
```



【需求】：使用vue赋值flag变量（boolean类型），用来判断`<div>`元素中的内容是否显示。

**点击按钮时，可以进入页面源码Elements菜单看页面元素实时变化情况**。

![1593832930401](img/1593832930401.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>

</head>
<body>
<div id="app">

    <!--
        v-if： 值表达式，布尔值； 如果值为false，则不会在页面生成
        v-show： 如果值为false，则在页面会生成，是隐藏起来了看不到
    -->
    <span v-if="flag">span1</span>
    <span v-show="flag">span2</span>

    <br>
    <!--加一个按钮，控制flag的值-->
    <input type="button" value="控制flag值" @click="changeFlag()">
</div>
</body>

<script>

    var vue = new Vue({
        // el 表示接管了id为app对应的div对象
        el: "#app",
        data: {
            flag: true
        },
        methods: {
            changeFlag: function () {
                // 控制flag的值
                this.flag = !this.flag
            }
        }
    })


</script>
</html>
```

### 4.小结

1. v-if： 值都是布尔类型，需要在vue对象中定义； 值是false，标签不会生成
2. v-show：值都是布尔类型，需要在vue对象中定义；值是false，标签会生成，但是不显示





# 第三章-VueJS生命周期

## 知识点--VueJS生命周期

### 1.目标

- [ ]  了解vue生命周期

### 2.路径

1. 什么是VueJS生命周期
2. vuejs生命周期的演示

### 3.讲解

#### 3.1.什么是VueJS生命周期

​	就是vue实例从创建到销毁的过程.

​	每个 Vue 实例在被创建到销毁都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做==生命周期钩子的函数==，这给了用户在不同阶段添加自己的代码的机会。官网示意图（先别看）：

![1552550270204](img/1552550270204.png)

**画图/生命周期简图：**

![](img/02-生命周期简图.png)



==**vue的生命周期函数和data、methods同级别**==

+ beforeCreate ：数据还没有监听，没有绑定到vue对象实例，同时也没有挂载对象
  + 此时， data、methods 都还没有初始化完成
+ ==**created**== ：数据已经绑定到了vue对象实例，但是还没有挂载div对象（使用ajax可在此方法中查询数据，调用函数）
  + ==此时， vue自身的data、methods 都已经初始化完成了==。
  + **==初始化页面数据的动作！==**
+ beforeMount: 模板已经编译好了，根据数据和模板已经生成了对应的元素对象，准备将el挂载到vue对象中去
  + 此时，el 还没有挂载上去；  div里的元素还没有被替换（此时，在div里面通过插值表达式获取不到data里的数据）
+ ==mounted==:将el的内容挂载到了el，相当于我们在jquery执行了(el).html(el),生成页面上真正的dom
  + ==此时，el 挂载好了==
  + 该阶段，表示vue、与之关联的div已经被彻底构建完成了，可以使用了
+ beforeUpdate ：数据更新到dom之前，我们可以看到$el对象已经修改，但是我们页面上dom的数据还
  没有发生改变
  + data数据信息变更，但是div里的还没有变更
+ updated: dom结构会通过虚拟dom的原则，找到需要更新页面dom结构的最小路径，将改变更新到
  dom上面，完成更新
  + data数据信息变更，并且div里的也已经被更新
+ beforeDestroy、destroyed :实例的销毁，vue实例还是存在的，只是解绑了事件的监听、还有watcher对象数据与view的绑定，即数据驱动
  + 手动销毁vue的方法：  `vue对象.$destroy()`

#### 3.2.vuejs生命周期的演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vuejs-2.5.16.js"></script>

</head>
<body>
<div id="app">
    {{message}}
    <input v-model="message">
    <input type="button" value="销毁vue" @click="destroyVue">
</div>
</body>

<script>
    new Vue({
        el: "#app",
        data: {
            message: "hello"
        },
        methods: {
            destroyVue: function () {
                // 手动销毁vue对象
                this.$destroy()
            }
        },
        // 生命周期函数和methods是同级别的
        beforeCreate: function () {
            console.log("[beforeCreate]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        // 此时vue对象有数据了，但是div没有绑定
        // 做一些初始化动作：初始化vue的data
        created: function () {
            console.log("[created]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        beforeMount: function () {
            console.log("[beforeMount]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        mounted: function () {
            console.log("[mounted]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        beforeUpdate : function () {
            console.log("[beforeUpdate ]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        updated: function () {
            console.log("[updated]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        beforeDestroy: function () {
            console.log("[beforeDestroy]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },
        destroyed : function () {
            console.log("[destroyed ]:" + this.message +
                ",[div]:" + document.getElementById("app").innerText);
        },

    })



</script>
</html>
```

+ 结果

 ![1593834517982](img/1593834517982.png)

### 4.小结

1. 一般情况下 我们可以在==`created`或者mounted进行初始化(请求服务器获得数据绑定)== 

   
   
   

# 第四章-VueJS ajax

## 知识点-VueJS ajax

### 1.目标

- [ ] 掌握vue的ajax的使用

### 2.路径

2. ==axios【重要】==
   + 什么是axios
   + axios的语法
   + axios的使用

### 3.讲解

#### 3.1vue-resource【了解】

有兴趣可以去了解：vue-resource的github: [https://github.com/pagekit/vue-resource](https://github.com/pagekit/vue-resource)

+ Get方式

![1552552968981](img/1552552968981.png)

+ Post方式

![1552552988792](img/1552552988792.png)

#### 3.2.axios【重点】

##### 3.2.1什么是axios

Axios是一个基于 promise （promise 是目前 JS 异步编程的主流解决方案）的 HTTP 库，可以用在浏览器和 node.js 中

注: Promise 对象用于表示一个异步操作的最终状态（完成或失败），以及其返回的值。==这些是不是就是ajax的特征==

axios的github:https://github.com/axios/axios

中文说明: https://www.kancloud.cn/yunye/axios/234845

**使用步骤：**

先引用vue；

==使用时需要引入axios.js文件==

##### 3.2.2axios的语法

+ get请求

```js
// 为给定 ID 的 user 创建请求
axios.get('user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  );

// 可选地，上面的请求可以这样做
axios.get('user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

+ post请求

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

+ axios方式(原始方式)

 ![1552553529807](img/1552553529807.png)



​    为方便起见，为所有支持的请求方法提供了别名

```
axios.get(url[, config])
axios.post(url[, data[, config]])

axios.request(config)
axios.delete(url[, config])
axios.head(url[, config])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```

##### 2.3 axios的使用【掌握】

==1.在axios里面如果写普通的函数(ES5),this不是vue对象==
==2.在axios里面如果写的是箭头函数(ES6), this是vue对象==

需求:准备一个文件user.json，在里面写一个json对象表示用户信息，使用axios读取user.json文件的内容，并在页面上输出内容

步骤:

1. 创建user.json文件
2. 把vue、axios导入项目 引入到页面
3. 使用get(), post() 请求

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../js/vuejs-2.5.16.js"></script>
    <script src="../js/axios-0.18.0.js"></script>
</head>
<body>

<div id="app">

    <input type="button" value="获取数据" @click="getData()"><br>
    <input type="button" value="获取数据post" @click="getData4post()"><br>

    {{user}}
</div>

</body>

<script>
    var vue = new Vue({
        el: "#app",
        data:{
            user:{}
        },
        methods:{
            getData: function () {
                // 获取user.json中的数据
                /**
                 * axios.get("路径").then(function(response){
                 *     response.data 才是响应数据
                 * })
                 */
                axios.get("user.json").then(function (response) {
                    //alert(response)
                    // 打印成json格式： JSON.stringify可以把参数打印成成json格式串
                    console.log(JSON.stringify(response));

                    // response.data才是响应数据
                    // this.user = response.data  不可用（this不是vue对象）
                    console.log("this:"+this)
                    vue.user = response.data
                })
            },
            getData4post: function () {
                /*axios.post("user.json").then(function (response) {
                    vue.user = response.data
                })*/

                // 新的写法，要求js语法核心在ECMAScipt6+
                axios.post("user.json").then(response=>{
                    //vue.user = response.data

                    // 这个写法可以用this操作vue对象
                    this.user = response.data
                })
            }
        }
    })
</script>
</html>
```



### 4.小结

1. axios的语法： 引入vue、引入axios

   1. get

      1. ```js
         axios.get("路径?username=zs").then(response=>{
             // response.data 才是响应数据
         })
         ```

   2. post

      1. ```js
         axios.post("路径", "参数").then(response=>{
             console.log(response)
             // response.data 才是响应数据
         })
         ```

         

# 知识点小结

- Vue

  - 一个渐进式的js框架、工具

- Vue的快速入门【重要】

  - 引入vue

  - 写一个div，id属性值设置为app

  - 可以在js中创建Vue对象了

    - ```html
      <body>
          <div id="app">
              {{message}}
          </div>
      </body>
      
      <script>
      	new Vue({
              el:"#app",
              data:{
                  message:"hello"
              },
              methods:{
                  方法名: function(){
                      
                  }
              }
          })
      </script>
      ```

- Vue的指令（Vue提供给我们的一些属性，可以简便操作vue中的数据）

  - 事件指令

    - 把原来的js的事件去除了on，加了@：   点击事件：  @click="函数调用语法func()"

  - 内容相关

    - v-text： 仅仅支持纯文本，不会渲染html标签
    - v-html： 可以渲染内容html标签
    - 用作标签的属性：  `<span v-text="vue中data定义的数据">`

  - v-bind

    - 绑定，v-bind:属性名，比如  v-bind:color
    - 简写：   :color，示例：  `<font :color="vue中定义的data中的数据”>`

  - v-model

    - 实现vue、页面数据的双向绑定
    - 示例： `<input v-model="user.username"/>`

  - v-for 循环、遍历

    - 语法

    - ```html
      arr有3条数据，name就会显示3个li标签
      <li v-for="(ele,idx) in arr">
      	{{ele}}
      </li>
      ```

  - v-if、v-show

    - 值都是布尔值
    - 值为false时，v-if这个标签不会再页面生成
    - 值为false时，v-shw这个标签在页面生成，但是不显示

- axios发送ajx请求

  - get

    - ```js
      axios.get("路径?参数名=参数值").then(response=>{
          // response.data 才是响应数据
      })
      ```

      

  - post

    - ```js
      axios.post("路径?参数名=参数值", "参数可以写key-value对，也可以写json格式").then(response=>{
          // response.data 才是响应数据
      })
      ```

- 生命周期

  - created函数即可：初始化数据时常用



# 第五章-综合案例

**准备工作**

- 数据库的创建

```mysql
create database day32;
use day32;
CREATE TABLE linkman (
  id int primary key auto_increment,
  name varchar(50),
  sex varchar(50),
  age int,
  address varchar(50),
  qq varchar(50),
  email varchar(50)
);

INSERT INTO `linkman`  (`id`, `name`, `sex`, `age`, `address`, `qq`, `email`) VALUES
(null, '张三', '男', 11, '广东', '766335435', '766335435@qq.com'),
(null, '李四', '男', 12, '广东', '243424242', '243424242@qq.com'),
(null, '王五', '女', 13, '广东', '474574574', '474574574@qq.com'),
(null, '赵六', '女', 18, '广东', '77777777', '77777777@qq.com'),
(null, '钱七', '女', 15, '湖南', '412132145', '412132145@qq.com'),
(null, '王八', '男', 25, '广西', '412132775', '412132995@qq.com');
```

- JavaBean的创建

```java
public class LinkMan implements Serializable{
    /**
     *   id int primary key auto_increment,
     name varchar(50),
     sex varchar(50),
     age int,
     address varchar(50),
     qq varchar(50),
     email varchar(50)
     */
    private int id;
    private String name;
    private String sex;
    private int age;
    private String address;
    private String qq;
    private String email;
}
```

- jar包

  - 驱动

  - 连接池

  - DButils

  - jackson、fastjson

    

- 工具类

- 配置文件

- 页面





**规范**：

- 采用json交互，部分采用表单形式
- 后台返回的数据，全部用Result类返回（json格式）
- 前端判断后台响应是否正确：通过Result的flag来判断



## 案例一-显示所有联系人案例

### 一，案例需求

![img](img/tu_1.png)

+ 查询数据库里面所有的联系人, 展示在页面

### 二, 思路分析

1. ![1600414992876](img/1600414992876.png)


### 三, 代码实现

#### 代码

+ list.html

```html
<!DOCTYPE html>
<!-- 网页使用的语言 -->
<html lang="zh-CN">
<head>
    <!-- 指定字符集 -->
    <meta charset="utf-8">
    <!-- 使用Edge最新的浏览器的渲染方式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- viewport视口：网页可以根据设置的宽度自动进行适配，在浏览器的内部虚拟一个容器，容器的宽度与设备的宽度相同。
    width: 默认宽度与设备的宽度相同
    initial-scale: 初始的缩放比，为1:1 -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap模板</title>

    <!-- 1. 导入CSS的全局样式 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
    <script src="js/jquery-2.1.0.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>
    <style type="text/css">
        td, th {
            text-align: center;
        }
    </style>
    <!--1. 引入vue、axios-->
    <script src="js/vuejs-2.5.16.js"></script>
    <script src="js/axios-0.18.0.js"></script>
</head>
<body>

<!--2. 添加id为app-->
<div class="container" id="app">
    <h3 style="text-align: center">显示所有用户</h3>
    <table border="1" class="table table-bordered table-hover">
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
            <th>籍贯</th>
            <th>QQ</th>
            <th>邮箱</th>
            <th>操作</th>
        </tr>

        <!--
            遍历数据 v-for循环显示用户信息
        -->
        <tr v-for="(ele, idx ) in linkmans">
            <td>{{ele.id}}</td>
            <td>{{ele.name}}</td>
            <td>{{ele.sex}}</td>
            <td>{{ele.age}}</td>
            <td>{{ele.address}}</td>
            <td>{{ele.qq}}</td>
            <td>{{ele.email}}</td>
            <td><a class="btn btn-default btn-sm" href="修改联系人.html">修改</a>&nbsp;<a class="btn btn-default btn-sm" href="修改联系人.html">删除</a></td>
        </tr>

        <tr>
            <td colspan="8" align="center"><a class="btn btn-primary" href="${pageContext.request.contextPath }/add.jsp">添加用户</a></td>
        </tr>
    </table>
</div>
</body>

<!--3. 写js代码-->
<script>
    new Vue({
        el:"#app",
        data:{
            linkmans:[]
        },
        methods:{
            
        },
        // 生命周期函数
        created: function () {
            //1.1 在created生命周期函数中获取数据
            //1.2 发送请求，使用axios发送请求获得数据(数组形式，可以在vue中定义一个数组)
            axios.get("linkManServlet?method=findAll").then(response=>{
                console.log(response);

                // response.data 才是响应数据
                // response.data 是一个Result对象
                // response.data.result 才是我们想要的数据内容
                if(response.data.flag){
                    this.linkmans = response.data.result
                }else{
                    alert("失败")
                }

            })
        }
    })
</script>
</html>

```

+ LinkManServlet

```java
package com.itheima.web;

import com.itheima.bean.LinkMan;
import com.itheima.bean.Result;
import com.itheima.service.LinkManService;
import com.itheima.util.JsonUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.List;

@WebServlet("/linkManServlet")
public class LinkManServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 写逻辑

        // 让客户端传入method参数（客户端是表单形式提交）
        String method = request.getParameter("method");

        try {
            // 反射三板斧
            //1. 获得Class
            Class clazz = this.getClass();

            // 2. 获得方法
            Method targetMethod =
                    clazz.getMethod(method, HttpServletRequest.class, HttpServletResponse.class);

            // 3. 调用方法
            targetMethod.invoke(this, request, response);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }


    /**
     * 查询所有联系人
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void findAll(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            //1. 获得参数
            //2. 调用业务层，得到一个List<LinkMan>
            LinkManService linkManService = new LinkManService();
            List<LinkMan> list = linkManService.findAll();
            //3. 响应（规范：返回json，用Result返回）
            //3.1 得到一个Result
            Result result = new Result(true, "查询所有联系人成功", list);
            //3.2 把result响应成json
            JsonUtils.printResult(response, result);
        } catch (Exception e) {
            e.printStackTrace();
            Result result = new Result(false, "系统繁忙");
            JsonUtils.printResult(response, result);
        }
    }

    /**
     * 添加联系人
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    /**
     * 删除联系人
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void delete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

### 四,小结

1. 页面
   1. 进入list.html就要看到数据
   2. 在vue的created函数里面发起了请求，获得了联系人列表数据
   3. 数据格式：![1597907828916](img/1597907828916.png)
   4. 获取数据之后，赋值给vue的linkmans数组属性，在表格中使用v-for遍历循环
2. 后端（web层）
   1. 获得了联系人列表List<LinkMan>
   2. 响应时使用了工具类，返回了一个Result对象给页面（json格式）





## 案例二:添加联系人

### 一,案例需求

1. 点击添加联系人跳转添加联系人页面

   ![1571568952279](img/1571568952279.png)

2. 在添加联系人页面，点击提交按钮,把数据提交到服务器,保存到数据库

   ![1571568994445](img/1571568994445.png)

3. 在添加完成，可以查看到新建的联系人信息

   ![1571569023876](img/1571569023876.png) 



**修改页面点击提交时（该按钮不再是submit，采用一个普通的button，绑定一个事件）**

页面的字段使用v-model双向绑定到vue对象

![1597910050639](img/1597910050639.png)

==注意事项：如果后台使用表单方式获取参数request.getParameterMap()，那么需要对请求参数额外引入qs.js来处理==：

```js
// 前台让步
<script src="js/qs.js"></script>

add: function () {
    // 发送请求，保存用户
    // Qs.stringify(this.linkman) 序列化js对象：   username=zs&age=18
    axios.post('linkMan?method=add', Qs.stringify(this.linkman)).then(
        response=>{
            if(response.data.flag){
                location.href = "list.html"
            }else{
                alert(response.data.msg)
            }
        }
    )
}
```

或者直接解析流得到参数使用工具类：

```java
axios.post("linkmanServlet?method=add",
           this.linkman
                ).then(response=>{
                    if( response.data.flag ){
                        location.href = "list.html"
                    }
                })
// 或者后台妥协-解析流得到参数    
JsonUtils.parseJSON2Object(request, LinkMan.class);
```



### 二,思路分析

![1600417741828](img/1600417741828.png)




### 三,代码实现

- list.html

```html
<tr>
 <td colspan="8" align="center"><a class="btn btn-primary" href="add.html">添加用户</a></td>
        </tr>
```



+ add.html
  + 首先去除的form表单的action、method属性
  + 把提交按钮由submit改为了普通的button，给他添加一个点击事件
  + v-model数据双向绑定
  + 在事件方法中发起请求，传输的是json'格式的数据

```html

<body>
<div class="container" id="app">
    <center><h3>添加用户页面</h3></center>
    <!--<form action="user" method="post">-->
    <!--不使用表单了-->
    <form>
        <input type="hidden" name="method" value="add"/>
        <div class="form-group">
            <label for="name">姓名：</label>
            <input type="text" class="form-control" id="name" name="name" v-model="linkMan.name" placeholder="请输入姓名">
        </div>

        <div class="form-group">
            <label>性别：</label>
            <input type="radio" name="sex" v-model="linkMan.sex" value="男" checked="checked"/>男
            <input type="radio" name="sex" v-model="linkMan.sex"  value="女"/>女
        </div>

        <div class="form-group">
            <label for="age">年龄：</label>
            <input type="text" class="form-control" id="age" name="age" v-model="linkMan.age" placeholder="请输入年龄">
        </div>

        <div class="form-group">
            <label for="address">籍贯：</label>
            <select name="address" v-model="linkMan.address" class="form-control" id="jiguan">
                <option value="广东">广东</option>
                <option value="广西">广西</option>
                <option value="湖南">湖南</option>
            </select>
        </div>

        <div class="form-group">
            <label for="qq">QQ：</label>
            <input type="text" class="form-control" name="qq" v-model="linkMan.qq" placeholder="请输入QQ号码"/>
        </div>

        <div class="form-group">
            <label for="email">Email：</label>
            <input type="text" class="form-control" name="email" v-model="linkMan.email" placeholder="请输入邮箱地址"/>
        </div>

        <div class="form-group" style="text-align: center">
            <!--提交按钮改成普通button-->
            <input class="btn btn-primary" type="button" value="提交" @click="addLinkMan()"/>
            <input class="btn btn-default" type="reset" value="重置" />
            <input class="btn btn-default" type="button" value="返回" />
        </div>
    </form>

    {{linkMan}}
</div>
</body>

<script>
    new Vue({
        el:"#app",
        data:{
            // 对象绑定输入项的属性
            linkMan:{}
        },
        methods:{
            addLinkMan: function () {
                // 添加联系人
                //3.1 使用axios.post("路径", 参数使用json格式).then(response=>{
                // 3.2 判断添加是否成功，成功才到查询所有页面
                axios.post("linkManServlet?method=add", this.linkMan).then(response=>{
                    console.log("response:"+JSON.stringify(response));
                    console.log("response.data:"+JSON.stringify(response.data));


                    if(response.data.flag){
                        // 跳转页面
                        location.href = "list.html"
                    }else{
                        alert("失败")
                    }
                })
            }
        }
    })
</script>
</html>
```

+ LinkManServlet

```java
/**
     * 添加联系人
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            // axios的默认方式是：application/json;charset=UTF-8
            System.out.println("ContentType:"+request.getContentType());

            //1. 获得参数，得到一个LinkMan对象（获得表单提交方式的参数）
            /*Map<String, String[]> parameterMap = request.getParameterMap();
            LinkMan linkMan = new LinkMan();
            BeanUtils.populate(linkMan, parameterMap);*/

            // 格式为application/json;charset=UTF-8，参数在请求体中--》网络中的输入流--》
            // request对象的输入流中---》借助JSON去解析读取字节，
            // ---》字节变成json格式的字符串{"name":"zsf"}----》解析成java对象
            LinkMan linkMan = JsonUtils.parseJSON2Object(request, LinkMan.class);

            //2. 调用业务层，去保存数据，得到一个布尔值
            LinkManService linkManService = new LinkManService();
            boolean flag = linkManService.add(linkMan);

            //3. 响应

            //3.1 获得一个Result对象
            //3.2 响应出去
            JsonUtils.printResult(response, new Result(flag, flag?"成功":"失败"));
        } catch (Exception e) {
            e.printStackTrace();
            JsonUtils.printResult(response, new Result(false, "系统繁忙"));
        }

    }
```





### 四,小结

1. 页面
   1. 首先去除的form表单的action、method属性
   2. 把提交按钮由submit改为了普通的button，给他添加一个点击事件
   3. v-model数据双向绑定
   4. 在事件方法中发起请求，传输的是json'格式的数据
2. 后台的WEB层
   1. 获得参数
      1. json'格式，就不能再使用request.getParameterXXX这些API了
      2. 用了一个工具类，借助fastjson的JSON去读取request的输入流，得到对应的json格式串，最终解析成对应的Class类型的对象
   2. 调用业务层
   3. 响应



## 案例三:删除联系人

### 一,案例需求

![1571569201344](img/1571569201344.png)

​	点击确定删除之后, 再重新查询所有全部展示,



### 二,思路分析

1. ![1600419732187](img/1600419732187.png)

### 三,代码实现

+ list.html

```html

<a class="btn btn-default btn-sm" href="#" @click="deleteById(ele.id)">删除</a>


<!--3. 写js代码-->
<script>
    new Vue({
        el:"#app",
        data:{
            linkmans:[]
        },
        methods:{
            //删除方法
            deleteById: function (id) {
                //alert(id)
                //1. 点击事件
                //2. 在事件方法中，弹出确认框 confirm
                var flag  = confirm("确认要删除吗？");
                //3. 如果点击了确定 ，才去请求后台删除数据（根据id删除）
                if(flag){
                    // 发请求
                    axios.get("linkManServlet?method=delete&id="+id).then(response=>{
                        console.log(JSON.stringify(response.data));

                        //4. 删除成功，跳转页面到查询所有
                        if(response.data.flag){
                            location.href = "list.html"
                        }else{
                            alert(response.data.message)
                        }
                    })
                }

            }
        },
        // 生命周期函数
        created: function () {
            //1.1 在created生命周期函数中获取数据
            //1.2 发送请求，使用axios发送请求获得数据(数组形式，可以在vue中定义一个数组)
            axios.get("linkManServlet?method=findAll").then(response=>{
                console.log("response:"+JSON.stringify(response));
                console.log("response.data:"+JSON.stringify(response.data));
                console.log("response.data.result:"+JSON.stringify(response.data.result));

                // response.data 才是响应数据
                // response.data 是一个Result对象(flag、message、result)
                // response.data.result 才是我们想要的数据内容
                if(response.data.flag){
                    this.linkmans = response.data.result
                }else{
                    alert("失败")
                }

            })
        }
    })
</script>
```



+ LinkManServlet

```java
/**
     * 删除联系人
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void delete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            // 1. 获得参数，获得id
            Integer id = Integer.valueOf(request.getParameter("id"));
            //2. 调用业务层，得到一个布尔值
            LinkManService linkManService = new LinkManService();
            boolean flag = linkManService.delete(id);
            //3. 响应：一个Result对象
            JsonUtils.printResult(response, new Result(flag, flag?"成功":"失败"));
        } catch (Exception e) {
            e.printStackTrace();
            JsonUtils.printResult(response, new Result(false, "系统繁忙"));
        }
    }
```



### 四,小结

1. 页面：
   1. 给删除按钮变成了假连接，增加了事件函数，传入id
   2. 在事件函数里面，发送ajax删除联系人

## 案例四:更新联系人【作业】

### 一,案例需求

![1571569357806](img/1571569357806.png) 

![1571569369845](img/1571569369845.png) 

### 二,思路分析

1. 点击修改按钮时，把id通过浏览器地址带给update.html
2. 在update.html，  在created函数里面解析浏览器地址栏的id值，根据id查询数据，回显在表单里面
3. 修改数据，才是点击提交，发送请求进行修改！

### 三,代码实现

==注意事项（工具）==

+ list.html

  ```html
  <td><a :href="'update.html?uid='+ele.id">修改</a></td>
  
  
  ```

+ update.html 解析获取浏览器地址栏的参数

```js
var getParams = function(param) {
    url = location.href
    if(url === undefined || typeof(url) != 'string'){
        return null;
    }
    items = url.split('?')[1].split('&');

    for(var i=0; i<items.length; i++) {
        var item = items[i].split('=');
        if( item[0] == param ){
            return item[1]
        }
    }
}
// 获取浏览器地址栏中uid参数的值
var uid = getParams("uid");
```

 

+ update.html

```js
<script>
    var uid = getParameter("uid");
    //创建vue实例
    var vue = new Vue({
        el: "#app",
        data: {
            linkMan: {}
        },
        methods: {
            update:function () {
                axios.post('linkMan?method=update', this.linkMan).then(response=>{
                    if (response.data.flag){
                        location.href = "list.html";
                    }else{
                        alert(response.data.message)
                    }
                });
            }
        },
        created:function () {
            axios.get('linkMan?method=findByUid&uid='+uid).then(response=>{
                if(response.data.flag){
                    this.linkMan = response.data.result;
                }else{
                    alert(response.data.message);
                }


            })
        }
    });

</script>
```

+ LinkManServlet

```java
    //根据id查询
    public void findByUid(HttpServletRequest request, HttpServletResponse response) throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();
        String data=null;
        ResultInfo resultInfo = null;
        try {
            //1.调用业务 获得List<linkMan>
            int uid = Integer.parseInt(request.getParameter("uid"));
            LinkMan linkMan =  linkManService.findByUid(uid);
            //2.转成json
            resultInfo = new ResultInfo(true,linkMan,"查询成功");
        } catch (Exception e) {
            e.printStackTrace();
            resultInfo = new ResultInfo(false,"查询失败");
        }finally {
            data = objectMapper.writeValueAsString(resultInfo);
            response.getWriter().print(data);
        }
    }
    //更新联系人
    private void update(HttpServletRequest request, HttpServletResponse response) throws IOException {
            try {
            //1.获得请求参数,封装成LinkMan对象
            LinkMan linkMan = JsonUtils.parseJSON2Object(request, LinkMan.class);

            //2.调用业务, 进行新增
            linkManService.add(linkMan);
            //3.封装Result, 响应
            Result result = new Result(true, "更新成功");
            JsonUtils.printResult(response,result);
        } catch (Exception e) {
            e.printStackTrace();
            //3.封装Result, 响应
            Result result = new Result(false, "更新失败");
            JsonUtils.printResult(response,result);
        }
    }
```

+ linkManService

```java
    public LinkMan findByUid(int uid) throws Exception {
        return linkManDao.findByUid(uid);
    }

    public void update(LinkMan linkMan) throws Exception {
        linkManDao.update(linkMan);
    }
```

+ linkManDao

````java
    public LinkMan findByUid(int uid) throws Exception {
        String sql = "SELECT * FROM linkman where id = ?";
        return queryRunner.query(sql,new BeanHandler<LinkMan>(LinkMan.class),uid);
    }

    public void update(LinkMan linkMan) throws Exception {
        String sql  ="UPDATE linkman SET name = ?,sex =?,age = ?,address = ?,qq =?,email = ? WHERE id = ?";
        Object[] params= {
                linkMan.getName(),
                linkMan.getSex(), linkMan.getAge(),
                linkMan.getAddress(), linkMan.getQq(),
                linkMan.getEmail(),linkMan.getId()
        };
        queryRunner.update(sql,params);

    }
````



### 四,小结





# 案例梳理

- 请求

  - 规范： 采用 json格式请求后台

  - 后台根据：request.getContentType()，请求内容格式

  - json格式获取参数：JSON的API解析请求对象request的输入流

  - ```
    JsonUtils.parseJSON2Object(request, LinkMan.class);
    ```

- 响应

  - 规范： 统一采用Result类返回，要求响应json格式
  - ![1600419578259](img/1600419578259.png)
  - 使用JsonUtils.printResult方法响应json格式数据





# 附录

## 1.键盘ascii码

[http://tool.oschina.net/commons?type=4](http://tool.oschina.net/commons?type=4)

![1552547539189](img/1552547539189.png)







# 预习概要

安装软件

虚拟机、centos等软件

常用命令-









# 附录-创建页面模板

- ![1600396043201](img/1600396043201.png)

- ![1600396143590](img/1600396143590.png)
- ![1600396192515](img/1600396192515.png)