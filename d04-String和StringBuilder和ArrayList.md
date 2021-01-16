# day04-String和StringBuilder和ArrayList

## 今日内容

- String类
- StringBuilder类
- ArrayList集合

## 教学目标

- 能够知道字符串对象通过构造方法创建和直接赋值的区别
- 能够完成用户登录案例
- 能够完成统计字符串中大写,小写,数字字符的个数
- 能够知道String和StringBuilder的区别
- 能够完成String和StringBuilder的相互转换
- 能够使用StringBuilder完成字符串的拼接
- 能够使用StringBuilder完成字符串的反转
- 能够知道集合和数组的区别
- 能够完成ArrayList集合添加字符串并遍历
- 能够完成ArrayList集合添加学生对象并遍历

## 知识点--1.String类的常用方法

### 知识点--1.1 String类概述

#### 目标

- 理解String类概述

#### 路径

- String类的概述

#### 讲解

##### String类的概述

​	String 类代表字符串，Java 程序中的所有字符串文字（例如“abc”）都被实现为此类的实例。也就是说，Java 程序中所有的双引号字符串，都是 String 类的对象。String 类在 java.lang 包下，所以使用的时候不需要导包！

#### 小结

- String类表示字符串,在java程序中所有双引号的字符串都是String类的对象
- String类在java.lang包下,所以使用的时候不需要导包

### 知识点--1.2 String类的构造方法

#### 目标

- 掌握String类构造方法的使用

#### 路径

- String类常用的构造方法
- 使用String类的构造方法

#### 讲解

##### String类常用的构造方法

- 常用的构造方法

  ```java
  public String() 创建一个空字符串对象  例如: ""
  public String(byte[] bytes) 根据byte数组中的内容,来创建对应的字符串对象
  public String(byte[] bytes, int offset, int length) 指定byte数组的范围,根据指定范围中的内容,来创建对应的字符串对象
  public String(char[] value) 根据char数组中的内容,来创建对应的字符串对象
  public String(String original) 新创建的字符串是该参数字符串的副本。
  直接写字符串字面值也是String类对象  例如: String str = "abc";    常用
  ```


##### 使用String类的构造方法

```java
public class Test {
    public static void main(String[] args) {
        /*
            public String() 创建一个空字符串对象  例如: ""
            public String(byte[] bytes) 根据byte数组中的内容,来创建对应的字符串对象
            public String(byte[] bytes, int offset, int length) 指定byte数组的范围,根据指定范围中的内容,来创建对应的字符串对象
            public String(char[] value) 根据char数组中的内容,来创建对应的字符串对象
            public String(String original) 新创建的字符串是该参数字符串的副本。
            直接写字符串字面值也是String类对象  例如: String str = "abc";    常用
         */
        // 创建一个空字符串对象
        String str1 = new String();// str1表示的字符串是:""

        // 根据byte数组中的内容,来创建对应的字符串对象
        byte[] bys = {97,98,99,100};
        String str2 = new String(bys);// str2表示的字符串是:"abcd"

        // 指定byte数组的范围,根据指定范围中的内容,来创建对应的字符串对象
        String str3 = new String(bys,1,2);// str3表示的字符串是:"bc"

        // 根据char数组中的内容,来创建对应的字符串对象
        char[] chs = {'a','b','c'};
        String str4 = new String(chs);// str4表示的字符串是:"abc"

        // 新创建的字符串是该参数字符串的副本
        String str5 = new String("abc");// str5表示的字符串是:"abc"

        // 直接写字符串字面值也是String类对象
        String str6 = "abc";

        System.out.println("str1:"+str1+"=");
        System.out.println("str2:"+str2);
        System.out.println("str3:"+str3);
        System.out.println("str4:"+str4);
        System.out.println("str5:"+str5);
        System.out.println("str6:"+str6);
    }
}

```

#### 小结

略

### 知识点--1.3 创建字符串对象两种方式的区别

#### 目标

- 能够知道通过**构造方法创建字符串对象**与**直接赋值方式创建字符串对象**的区别

#### 路径

- 通过构造方法创建

- 直接赋值方式创建
- 绘制对比内存图

#### 讲解

##### 通过构造方法创建

- 通过 new 创建的字符串对象，每一次 new 都会申请一个内存空间，虽然字符串内容相同，但是地址值不同

  ```java
  char[] chs = {'a','b','c'};
  String s1 = new String(chs);// s1字符串的内容: abc
  String s2 = new String(chs);// s2字符串的内容: abc
  // 上面的代码中,JVM首先会先创建一个字符数组,然后每一次new的时候都会有一个新的地址,只不过s1和s2参考的字符串内容是相同的
  ```

##### 直接赋值方式创建

- 以“”方式给出的字符串，只要字符序列相同(顺序和大小写)，无论在程序代码中出现几次，JVM 都只会建立一个 String 对象，并在字符串池中维护

```java
String s3 = "abc";
String s4 = "abc";
// 上面的代码中,针对第一行代码,JVM会建立一个String对象放在字符串池中,并给s3参考;第二行代码,则让s4直接参考字符串池中String对象,也就是说他们本质上是同一个对象
```

##### 绘制内存图

```java
public class Test {
    public static void main(String[] args) {
        // - 通过构造方法创建
        char[] chs = {'a','b','c'};
        String str1 = new String(chs);// str1表示的字符串是: "abc"
        String str2 = new String(chs);// str2表示的字符串是: "abc"
        System.out.println(str1 == str2);// false  ==比较的是2个对象的地址值

        // - 直接赋值方式创建
        String str3 = "abc";// str3表示的字符串是: "abc"
        String str4 = "abc";// str4表示的字符串是: "abc"
        System.out.println(str3 == str4);// true  直接赋值方式创建的字符串是字符串常量

    }
}

```

![image-20200810090102966](img\image-20200810090102966.png)



#### 小结

- 通过new创建的字符串对象,每一次都会新开辟空间
- 通过""方式直接创建的字符串对象,是常量,在常量池中,只有一份

### 知识点--1.4 String类的特点

#### 目标

- 能够理解String类的特点

#### 路径

- String类的特点

#### 讲解

##### String类的特点

- String类的字符串不可变，它们的值在创建后不能被更改

  ```java
  String s1 = "abc";
  s1 += "d";// s1 = s1 + "d"
  System.out.println(s1); 
  // 内存中有"abc"，"abcd","d",3个对象，s1从指向"abc"，改变指向，指向了"abcd"。
  ```

- 虽然 String 的常量值是不可变的，但是它们可以被共享

  ```java
  String s1 = "abc";
  String s2 = "abc";
  // 内存中只有一个"abc"对象被创建，同时被s1和s2共享。
  ```

- 字符串效果上相当于字符数组( char[] )

  ```java
  例如： 
  String str = "abc";
  
  相当于： 
  char[] data = {'a', 'b', 'c'};     
  String str = new String(data);// str表示的字符串内容: abc
  // String底层是靠字符数组实现的jdk8,jdk9底层是字节数组。
  
  byte[] bys = {97,98,99};
  String str = new String(bys);// str表示的字符串内容: abc
  ```

#### 小结

- ```java
     String类的特点:
                  1.String类不可变
                  2.String类字符串常量对象可以共享
                  3. String底层是靠字符数组实现的,jdk9底层是字节数组。
  ```

### 知识点--1.5 字符串的比较

#### 目标

- 能够比较2个字符串内容是否相同

#### 路径

- ==号的比较
- equals方法的作用   
- equalsIgnoreCase 方法的使用 

#### 讲解

##### ==号的比较

- 比较基本数据类型：比较的是具体的值

  ```java
  int num1 = 10;
  int num2 = 20;
  num1 == num2  ===> 10==20  结果:false
  ```

- 比较引用数据类型：比较的是对象地址值

  ```java
  String str1 = new String("abc");
  String str2 = new String("abc");
  str1 == str2 ===> str1存储的对象地址值 == str2存储的对象地址值  结果: false
  ```

  ```java
    // ==比较运算符:
      private static void method01() {
          // 基本数据类型比较的是2个变量中的值是否相同
          int num1 = 10;
          int num2 = 10;
          System.out.println(num1 == num2);// true
  
          // 引用数据类型比较的是2个对象的地址值是否相同
          String str1 = "abc";
  
          char[] chs = {'a','b','c'};
          String str2 = new String(chs);// "abc"
  
          System.out.println(str1 == str2);// false
      }
  ```

##### equals方法的作用

- 字符串是对象,它比较内容是否相同,是通过一个方法来实现的,就是equals()方法

- 方法介绍

  ```java
  public boolean equals(String s)     比较两个字符串内容是否相同、区分大小写
  ```

- 示例代码

  ```java
  public class StringDemo02 {
      public static void main(String[] args) {
          String str1 = "abc";
          String str2 = new String("abc");
          String str3 = "abc";
          String str4 = "Abc";
          // 比较str1和str2中的字符串内容是否相同(区分大小写)
          System.out.println(str1.equals(str2));// true
  
          // 比较str1和str3中的字符串内容是否相同(区分大小写)
          System.out.println(str1.equals(str3));// true
  
          // 比较str1和str4中的字符串内容是否相同(区分大小写)
          System.out.println(str1.equals(str4));// false
  
      }
  }
  ```

##### 扩展

- ` public boolean equalsIgnoreCase (String anotherString)` ：将此字符串与指定对象进行比较，忽略大小写。

- 示例

  ```java
   public static void main(String[] args) {
          String str1 = "abc";
          String str2 = new String("abc");
          String str3 = "abc";
          String str4 = "Abc";    
  
          // 比较str1和str2中的字符串内容是否相同(不区分大小写)
          System.out.println(str1.equalsIgnoreCase(str2));// true
  
          // 比较str1和str3中的字符串内容是否相同(不区分大小写)
          System.out.println(str1.equalsIgnoreCase(str3));// true
  
          // 比较str1和str4中的字符串内容是否相同(不区分大小写)
          System.out.println(str1.equalsIgnoreCase(str4));// true
      }
  
  ```

#### 小结

- 比较字符串内容是否相等,区分大小写,需要使用String类的equals方法,千万不要用 == 比较
- 如果比较字符串内容是否相等,不区分大小写,需要使用String类的equalsIgnoreCase()方法

### 知识点--1.6 String类获取功能的方法

#### 目标

- 理解String类中各个方法的作用\调用

#### 路径

- 介绍获取功能的方法
- 使用获取功能的方法

#### 讲解

##### 获取功能的方法

```java
- public int length () ：返回此字符串的长度。
- public String concat (String str) ：将指定的字符串连接到该字符串的末尾。
- public char charAt (int index) ：返回指定索引处的 char值。
- public int indexOf (String str) ：返回指定子字符串第一次出现在该字符串内的索引。
- public int indexOf (String str,int fromIndex) ：返回指定子字符串第一次出现在该字符串内的索引,从指定的索引开始。
- public  int lastIndexOf(String str)  ：返回指定子字符串最后一次出现在该字符串内的索引。
- public  int lastIndexOf(String str,int fromIndex)  ：返回指定子字符串最后一次出现在该字符串内的索引,从指定的索引开始反向搜索。
- public String substring (int beginIndex) ：返回一个子字符串，从beginIndex开始截取字符串到字符串结尾。
- public String substring (int beginIndex, int endIndex) ：返回一个子字符串，从beginIndex到endIndex截取字符串。含beginIndex，不含endIndex。
```



##### 使用获取功能的方法

```java
  public class Test {
    public static void main(String[] args) {
        /*
            String类获取功能的方法:
            - public int length () ：返回此字符串的长度。
            - public String concat (String str) ：将指定的字符串连接到该字符串的末尾。
            - public char charAt (int index) ：返回指定索引处的 char值。
            - public int indexOf (String str) ：返回指定子字符串第一次出现在该字符串内的索引。
            - public int indexOf (String str,int fromIndex) ：返回指定子字符串第一次出现在该字符串内的索引,从指定的索引开始。
            - public  int lastIndexOf(String str)  ：返回指定子字符串最后一次出现在该字符串内的索引。
            - public  int lastIndexOf(String str,int fromIndex)  ：返回指定子字符串最后一次出现在该字符串内的索引,从指定的索引开始反向搜索。
            - public String substring (int beginIndex) ：返回一个子字符串，从beginIndex开始截取字符串到字符串结尾。
            - public String substring (int beginIndex, int endIndex) ：返回一个子字符串，从beginIndex到endIndex截取字符串。含beginIndex，不含endIndex。
         */
        String str1 = "hello";

        // 需求1:获取str1字符串的长度
        int len = str1.length();
        System.out.println("str1字符串的长度:"+len);// 5

        // 需求2:在str1字符串的后面拼接上world
        String str2 = str1.concat("world");
        System.out.println("原字符串:"+str1);// str1:  hello
        System.out.println("拼接后的字符串:"+str2);// str2: helloworld

        // 需求3:获取str1字符串的第二个字符
        char c = str1.charAt(1);
        System.out.println("索引为1的字符:"+c);// e

        // 需求4:循环遍历str1字符串的每一个字符
        for (int i = 0; i < str1.length(); i++) {
            System.out.println(str1.charAt(i));
        }
        System.out.println("=================================");

        // public int indexOf (String str) ：返回指定子字符串第一次出现在该字符串内的索引。
        String str3 = "hello-java-hello-world-hello-heima";
        // 需求5:查找hello子字符串在str3中第一次出现的位置对应的索引
        int index1 = str3.indexOf("hello");
        System.out.println("index1:"+index1);// 0

        // public int indexOf (String str,int fromIndex) ：返回指定子字符串第一次出现在该字符串内的索引,从指定的索引开始。
        // 需求5:查找hello子字符串在str3中第二次出现的位置对应的索引
        int index2 = str3.indexOf("hello", index1+1);
        System.out.println("index2:"+index2);// 11

        // public  int lastIndexOf(String str)  ：返回指定子字符串最后一次出现在该字符串内的索引。
        // 需求5:查找hello子字符串在str3中最后一次出现的位置对应的索引
        int index3 = str3.lastIndexOf("hello");
        System.out.println("index3:"+index3);// 23

        // public  int lastIndexOf(String str,int fromIndex)  ：返回指定子字符串最后一次出现在该字符串内的索引,从指定的索引开始反向搜索。
        // 需求5:查找hello子字符串在str3中倒数第二次出现的位置对应的索引
        int index4 = str3.lastIndexOf("hello", index3 - 1);
        System.out.println("index4:"+index4);// 11

        // public String substring (int beginIndex) ：返回一个子字符串，从beginIndex开始截取字符串到字符串结尾。
        // 需求6:从str3中截取出"java-hello-world-hello-heima"
        String subStr1 = str3.substring(6);
        System.out.println("subStr1:"+subStr1);// "java-hello-world-hello-heima"

        // public String substring (int beginIndex, int endIndex) ：返回一个子字符串，从beginIndex到endIndex截取字符串。含beginIndex，不含endIndex。
        // 需求6:从str3中截取出"hello-world"
        String subStr2 = str3.substring(11, 22);
        System.out.println("subStr2:"+subStr2);// hello-world
    }
}

```

#### 小结

略

### 实操--1.7 用户登录案例【应用】

#### 需求

​	已知用户名和密码，请用程序实现模拟用户登录。总共给三次机会，登录之后，给出相应的提示

#### 分析

1. 已知用户名和密码，定义两个字符串表示即可
2. 键盘录入要登录的用户名和密码，用 Scanner 实现
3. 拿键盘录入的用户名、密码和已知的用户名、密码进行比较，给出相应的提示。
4. 用循环实现多次机会，这里的次数明确，采用for循环实现，并在登录成功的时候，使用break结束循环

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        // 需求:	已知用户名和密码，请用程序实现模拟用户登录。总共给三次机会，登录之后，给出相应的提示
        // 1.定义2个字符串存储已知的用户名和密码
        String username = "admin";
        String password = "123456";

        // 由于次数确定,所以使用for循环,当输入成功时,可以使用break提前结束循环
        for (int i = 0; i < 3; i++) {
            // 2.模拟用户输入用户名和密码,使用Scanner
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入用户名:");
            String name = sc.next();
            System.out.println("请输入密码:");
            String pwd = sc.next();

            // 3.判断用户输入的用户名和密码是否与已知的用户名和密码完全相同
            if (username.equals(name) && password.equals(pwd)) {
                // 4.如果相同,那么就显示: 恭喜您,登录成功!
                System.out.println("恭喜您,登录成功!");
                break;
            }else {
                // i: 0,1,2
                if (i == 2){
                    System.out.println("很遗憾,您的账户已被锁定,请充钱解锁!");
                }else{
                    // 4.如果不相同,那么就显示: 很遗憾您,登录失败,请重新输入!
                    System.out.println("很遗憾,登录失败,您还有"+(2-i)+"次机会");
                }
            }
        }

    }
}

```

#### 小结

- 略

### 实操--1.8 遍历字符串案例

#### 需求

​	键盘录入一个字符串，使用程序实现在控制台遍历该字符串

#### 分析

1. 键盘录入一个字符串，用 Scanner 实现
2. 遍历字符串，首先要能够获取到字符串中的每一个字符
3. 遍历字符串，其次要能够获取到字符串的长度
4. 遍历字符串的通用格式

#### 实现

```java
/*
    思路：
        1:键盘录入一个字符串，用 Scanner 实现
        2:遍历字符串，首先要能够获取到字符串中的每一个字符
            public char charAt(int index)：返回指定索引处的char值，字符串的索引也是从0开始的
        3:遍历字符串，其次要能够获取到字符串的长度
            public int length()：返回此字符串的长度
            数组的长度：数组名.length
            字符串的长度：字符串对象.length()
        4:遍历字符串的通用格式
 */
public class StringTest02 {
    public static void main(String[] args) {
        //键盘录入一个字符串，用 Scanner 实现
        Scanner sc = new Scanner(System.in);

        System.out.println("请输入一个字符串：");
        String line = sc.nextLine();

        for(int i=0; i<line.length(); i++) {
            System.out.println(line.charAt(i));
        }
    }
}
```

#### 小结

略

### 实操--1.9 统计字符次数案例

#### 需求

​	键盘录入一个字符串，统计该字符串中大写字母字符，小写字母字符，数字字符出现的次数(不考虑其他字符)

#### 分析

1. 键盘录入一个字符串，用 Scanner 实现
2. 要统计三种类型的字符个数，需定义三个统计变量，初始值都为0 
3. 遍历字符串，得到每一个字符
4. 在循环中,判断该字符属于哪种类型，然后对应类型的统计变量+1
   1. 大写字母：ch>='A' && ch<='Z'
   2. 小写字母： ch>='a' && ch<='z'
   3. 数字： ch>='0' && ch<='9'
5. 输出三种类型的字符个数

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        // 需求:	键盘录入一个字符串，统计该字符串中大写字母字符，小写字母字符，数字字符出现的次数(不考虑其他字符)
        // 1.键盘录入一个字符串
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串:");
        String str = sc.next();

        // 2.定义三个int类型的变量,分别用来统计大写,小写,数字字符出现的个数,初始值默认为0
        int count1 = 0;
        int count2 = 0;
        int count3 = 0;

        // 3.循环遍历字符串
        for (int i = 0; i < str.length(); i++) {
            // 4.在循环中,获取遍历出来的字符
            char c = str.charAt(i);
            // 5.在循环中,判断该字符
            if (c >= 'A' && c <= 'Z') {
                // 5.如果该字符是大写字母字符,那么就统计大写字母字符个数的变量自增1
                count1++;
            }else if (c >= 'a' && c <= 'z') {
                // 5.如果该字符是小写字母字符,那么就统计小写字母字符个数的变量自增1
                count2++;
            }else if (c >= '0' && c <= '9') {
                // 5.如果该字符是数字字符,那么就统计数字字符个数的变量自增1
                count3++;
            }
        }
        // 6.打印结果
        System.out.println("大写字母字符个数:"+count1);
        System.out.println("小写字母字符个数:"+count2);
        System.out.println("数字字符个数:"+count3);
    }
}

```

#### 小结

略

### 实操--1.10 字符串拼接案例

#### 需求

​	定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，

​	并在控制台输出结果。例如，数组为 int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]

#### 分析

1. 定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
2. 定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回。返回值类型 String，参数列表 int[] arr
3. 在方法中遍历数组，按照要求进行拼接
4. 调用方法，用一个变量接收结果
5. 输出结果

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求
            	定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，
            	并在控制台输出结果。例如，数组为 int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]

         */
        int[] arr = {1,2,3};
        System.out.println(concat(arr));
    }

    /**
     * 按照指定格式拼接数组中的元素
     *
     * @param arr 数组
     * @return 拼接后的字符串
     */
    public static String concat(int[] arr) {
        // 1.定义一个空字符串对象
        String str = "";
        // 2.循环遍历数组
        for (int i = 0; i < arr.length; i++) {
            // 3.在循环中,获取数组的元素
            int e = arr[i];
            // 4.判断遍历出来的元素
            if (i == 0) {
                // 4.如果该元素是数组中第一个元素,那么拼接的格式: [+元素+逗号空格
                str += "["+e+", ";
            } else if (i == arr.length - 1) {
                // 4.如果该元素是数组中最后一个元素,那么拼接的格式: 元素+]
                str += e+"]";
            } else {
                // 4.如果该元素是数组的中间元素,那么拼接的格式: 元素+逗号空格
                str += e + ", ";
            }
        }
        // 5.返回拼接后的字符串对象
        return str;
    }
}

```

#### 小结

略

### 实操--1.11 字符串反转案例

#### 需求

​	定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出反转后的结果

​	例如，键盘录入 abc，输出结果 cba

#### 分析

1. 键盘录入一个字符串，用 Scanner 实现
2. 定义一个方法，实现字符串反转。返回值类型 String，参数 String s
3. 在方法中把字符串倒着遍历，然后把每一个得到的字符拼接成一个字符串并返回
4. 调用方法，用一个变量接收结果
5. 输出结果

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求
            	定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出反转后的结果
            	例如，键盘录入 abc，输出结果 cba
         */
        // 键盘录入一个字符串
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串:");
        String str = sc.next();

        // 调用reverse方法,打印结果
        System.out.println(reverse(str));
    }

    public static String reverse(String str) {
        // 1.创建一个空字符串对象
        String newStr = "";

        // 2.循环倒叙遍历传入的str字符串 abc
        for (int i = str.length() - 1; i >= 0; i--) {
            // 3.在循环中,获取遍历出来的字符
            char c = str.charAt(i);
            // 4.在循环中,拼接
            newStr += c;
        }
        // 5.返回拼接后的字符串(反转后的字符串)
        return newStr;
    }
}

```

#### 小结

略

### 希望--有时间的同学

```java
 常用的方法：
        concat();  //把两个字符串拼接起来，获取一个新的字符串
        ★length();  //获取字符串的长度(其实就是获取字符串中 字符的个数)      
        ★equals();  //比较两个字符串的内容是否相同。    //区分大小写
        equalsIgnoreCase();  //比较两个字符串的内容是否相同。    //忽略大小写
        ★charAt();  //根据给定的索引，获取对应位置的字符
        ★indexOf(); //获取指定的字符 在字符串中 第一次出现的位置(索引)，找不到返回-1
            //int index = a1.indexOf('h');   从头找，'h'第一次出现的位置
            //int index = a1.indexOf('h',3); 从索引为3的元素开始往后找，'h'第一次出现的位置
       lastIndexOf();  //获取指定的字符 在字符串中 最后一次出现的位置(索引)，找不到返回-1
            //int index = a1.lastIndexOf('h');   从尾部找，'h'最后一次出现的位置
            //int index = a1.lastIndexOf('h',3); 从索引为3的元素开始往前找，'h'最后一次出现的位置 
       ★substring(); //截取字符串，返回新的字符串
                    //String newStr = a1.substring(2);       //从给定索引，直接截取到字符串末尾
                    //String newStr = a1.substring(2,5);     //包左不包右(前闭后开), 能取索引2的元素，不能取索引5的元素
		★isEmpty(); //判断字符串是否为空(长度为0返回true，不为0返回false)
        ★contains();    //判断字符串中是否包含 给定的字符串。
        endsWith(); //判断字符串是否以 给定的字符串 结尾。
        startsWith(); //判断字符串是否以 给定的字符串 开头。
		
        ★replace(); //用新内容替代旧内容，返回新的字符串    
        toLowerCase();  //把字母都转成其对应的小写形式。
        toUpperCase();  //把字母都转成其对应的大写形式。
		toCharArray() // 把字符串转换为数组
		getBytes() // 把字符串转换为字节数组        
		★trim();            //移除首尾空格。
		★split();   //根据给定的内容，切割字符串，返回字符串数组

```

```java
public class Test {
    public static void main(String[] args) {
        /*
            ★boolean isEmpty(); 判断字符串是否为空字符串(长度为0返回true，不为0返回false)
            ★boolean contains(String str);    判断字符串中是否包含 给定的字符串。
            boolean endsWith(String str); 判断字符串是否以 给定的字符串 结尾。
            boolean startsWith(String str); 判断字符串是否以 给定的字符串 开头。

            ★String replace(String oldStr,String newStr); 用新内容替代旧内容，返回新的字符串
            String toLowerCase();  把字母都转成其对应的小写形式。
            String toUpperCase();  把字母都转成其对应的大写形式。
            char[] toCharArray() 把字符串转换为数组
            byte[] getBytes()  把字符串转换为字节数组
            String trim();            移除首尾空格。
            String[] split(String str);   根据给定的内容，切割字符串，返回字符串数组
         */
        // String replace(String oldStr,String newStr); 用新内容替代旧内容，返回新的字符串
        String str = "hello-heima";
        // 需求:把str中的heima换成itcast
        String replaceStr = str.replace("heima", "itcast");
        System.out.println("返回的字符串:"+replaceStr);// hello-itcast
        System.out.println("原字符串:"+str);// hello-heima

        // String toLowerCase();  把字母都转成其对应的小写形式。
        String str1 = "ABCaD";
        // 需求:把str1中所有的字符都变成小写字母
        System.out.println(str1.toLowerCase());// abcad

        // String toUpperCase();  把字母都转成其对应的大写形式。
        // 需求:把str1中所有的字符都变成大写字母
        System.out.println(str1.toUpperCase());// ABCAD

        // char[] toCharArray() 把字符串转换为数组
        // 需求:把str1转换为字符数组
        char[] chs = str1.toCharArray();
        for (int i = 0; i < chs.length; i++) {
            System.out.print(chs[i]+" ");
        }
        System.out.println();

        // byte[] getBytes()  把字符串转换为字节数组
        // 需求:把str1转换为字节数组
        byte[] bys = str1.getBytes();// 65,66,67,97,68
        for (int i = 0; i < bys.length; i++) {
            System.out.print(bys[i]+" ");
        }
        System.out.println();


        // String trim();            移除首尾空格。
        String str2 = " admin ";
        System.out.println(str2.trim()+"-----");

        // String[] split(String str);   根据给定的内容，切割字符串，返回字符串数组
        String str3 = "itheima-itcast-java-hello";
        // 需求:以-对str3字符串进行分割
        String[] arr = str3.split("-");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]+" ");
        }
        System.out.println();

        System.out.println("===========了解,后面正则表达式================");
        String str4 = "itheima.itcast.java.hello";
        // 需求:以.对str4字符串进行分割
        String[] split = str4.split("\\.");
        System.out.println("split数组的长度:"+split.length);
        for (int i = 0; i < split.length; i++) {
            System.out.print(split[i]+" ");
        }
    }

    private static void method01() {
        // boolean isEmpty(); 判断字符串是否为空字符串(长度为0返回true，不为0返回false)
        String str1 = null;// null表示什么都没有
        String str2 = "";// ""表示空字符串对象
        System.out.println(str2.isEmpty());// true

        // boolean contains(String str);    判断字符串中是否包含 给定的字符串。
        String str3 = "hello-world";
        // 需求:判断str3字符串中是否包含world
        System.out.println(str3.contains("world"));// true
        // 需求:判断str3字符串中是否包含heima
        System.out.println(str3.contains("heima"));// false

        // boolean startsWith(String str); 判断字符串是否以 给定的字符串 开头。
        String str4 = "itcast-itheima-java-world";
        // 需求:判断str4字符串是否以it开头
        System.out.println(str4.startsWith("it"));// true

        // 需求:判断str4字符串是否以itcast开头
        System.out.println(str4.startsWith("itcast"));// true

        // 需求:判断str4字符串是否以itheima开头
        System.out.println(str4.startsWith("itheima"));// false;

        // boolean endsWith(String str); 判断字符串是否以 给定的字符串 结尾。
        String fileName = "Person.java";
        // 需求:判断fileName是否以.java结尾(判断文件是否是java文件)
        System.out.println(fileName.endsWith(".java"));// true
    }
}

```

## 知识点--2 StringBuilder类

### 知识点--2.1 String类字符串拼接问题

#### 目标:

- String类字符串拼接问题

#### 步骤:

- 字符串拼接问题

#### 讲解:

##### 字符串拼接问题

由于String类的对象内容不可改变，所以每当进行字符串拼接时，总是会在内存中创建一个新的对象。例如：

```java
public class StringDemo {
    public static void main(String[] args) {
        String s = "Hello";
        s += "World";// s = s + "World";
        System.out.println(s);// HelloWorld
        // 在内存中有三个字符串对象,分别是:"Hello"  "World"  "HelloWorld"
    }
}
```

在API中对String类有这样的描述：字符串是常量，它们的值在创建后不能被更改。

根据这句话分析我们的代码，其实总共产生了三个字符串，即`"Hello"`、`"World"`和`"HelloWorld"`。引用变量s首先指向`Hello`对象，最终指向拼接出来的新字符串对象，即`HelloWord` 。

![](img\String拼接问题.bmp)

由此可知，如果对字符串进行拼接操作，每次拼接，都会构建一个新的String对象，**既耗时，又浪费空间**。为了解决这一问题，可以使用`java.lang.StringBuilder`类。

#### 小结

- 结论:
  - String类的字符串拼接,每一次拼接完都会得到一个新的字符串对象,所以比较耗时,也浪费空间

### 知识点--2.2 StringBuilder类概述以及与String类的区别

#### 目标

- 理解StringBuilder的概述和与String类的区别

#### 路径

- StringBuilder类的概述
- StringBuilder类和String类的区别

#### 讲解

##### StringBuilder类概述

​	**StringBuilder 是一个可变的字符串类**，我们可以把它看成是一个容器，这里的可变指的是 StringBuilder 对象中的内容是可变的

##### StringBuilder类和String类的区别

- String类：内容是不可变的

- StringBuilder类：内容是可变的

  ![06-StringBuilder的原理](img\06-StringBuilder的原理.png)

#### 小结

- StringBuilder 是一个可变的字符串类,表示字符串
- StringBuilder 拼接是直接在本身的字符串末尾进行追加的

### 知识点--2.3 StringBuilder类的构造方法

#### 目标:

- StringBuilder构造方法

#### 路径:

- StringBuilder构造方法

#### 讲解:

- 常用的构造方法

  ```java
      public StringBuilder()创建一个空白可变字符串对象，不含有任何内容
      public StringBuilder(String   str)根据字符串的内容，来创建可变字符串对象
  ```

- 示例代码

```java
public class Test {
    public static void main(String[] args) {
        /*
            StringBuilder类的构造方法
                    public StringBuilder()创建一个空白可变字符串对象，不含有任何内容
                    public StringBuilder(String   str)根据字符串的内容，来创建可变字符串对象
         */
        // 创建一个空的可变字符串对象
        StringBuilder sb1 = new StringBuilder();
        System.out.println("sb1:"+sb1+"===");// ""
        System.out.println("sb1的字符串长度:"+sb1.length());// 0

        // 根据字符串的内容，来创建可变字符串对象
        StringBuilder sb2 = new StringBuilder("itheima");
        System.out.println("sb2:"+sb2);// itheima
        System.out.println("sb2的字符串长度:"+sb2.length());// 7
    }
}

```

#### 小结

略

### 知识点--2.4 StringBuilder类拼接和反转方法

#### 目标:

- StringBuilder类拼接和反转方法

#### 路径:

- StringBuilder类拼接和反转方法

#### 讲解:

- 添加和反转方法

  ```java
  public StringBuilder append(任意类型) 拼接数据，并返回对象本身
  public StringBuilder insert(int offset, 任意类型) 在指定位置插入数据,并返回对象本身
  public StringBuilder reverse()  反转字符串,并返回对象本身
  ```

- 拼接数据

  ```java
  public class Test {
      public static void main(String[] args) {
          /*
              StringBuilder类拼接方法:
                  public StringBuilder append(任意类型) 拼接数据，并返回对象本身
           */
          // 创建可变的字符串对象
          StringBuilder sb = new StringBuilder("Hello");
  
          // sb可变字符串拼接数据
          StringBuilder sb1 = sb.append("World");
          System.out.println("sb:"+sb);// HelloWorld
          System.out.println("sb1:"+sb1);// HelloWorld
          System.out.println(sb == sb1);// true
  
          // sb.append(100);// 拼接整数
          // sb.append(true);// 拼接boolean值
          // sb.append(3.14);// 拼接小数
          // sb.append('a');//  拼接字符
          // 链式编程
          sb.append(100).append(true).append(3.14).append('a');
          System.out.println("拼接后的字符串:"+sb);// HelloWorld100true3.14a
      }
  }
  
  ```

  

- 插入数据

  ```java
  public class Test {
      public static void main(String[] args) {
          /*
              public StringBuilder insert(int offset, 任意类型) 在指定位置插入数据,并返回对象本身
           */
          // 创建可变的字符串对象
          StringBuilder sb = new StringBuilder("Hello-World");
  
          // 往sb对象中插入数据
          StringBuilder sb1 = sb.insert(5, "-Hello-java");
          System.out.println("sb:"+sb);// Hello-Hello-java-World
          System.out.println("sb1:"+sb1);// Hello-Hello-java-World
          System.out.println(sb == sb1);// true
  
          // 插入数据
          sb.insert(6,100).insert(6,3.14).insert(6,true).insert(6,'a');
          System.out.println("sb:"+sb);// Hello-atrue3.14100Hello-java-World
      }
  }
  
  ```

  

- 反转字符串

  ```java
  public class Test {
      public static void main(String[] args) {
          /*
              StringBuilder类反转的方法:
                  public StringBuilder reverse()  反转字符串,并返回对象本身
           */
          // 创建可变的字符串对象
          StringBuilder sb = new StringBuilder("Hello-World");
          // 反转
          StringBuilder sb1 = sb.reverse();
          System.out.println("sb:"+sb);// dlroW-olleH
          System.out.println("sb1:"+sb1);// dlroW-olleH
          System.out.println(sb == sb1);// true
      }
  }
  ```

  

#### 小结

```java
StringBuilder类常用方法:
    public StringBuilder   append(任意类型)  添加数据，并返回对象本身
    public StringBuilder   reverse()  返回相反的字符序列
    public StringBuilder insert(int offset, 任意类型) 在指定位置插入数据,并返回对象本身
```

### 知识点--2.5 StringBuilder和String相互转换

#### 目标:

- StringBuilder和String相互转换

#### 路径:

- StringBuilder转换为String
- String转换为StringBuilder

#### 讲解:

##### String转换为StringBuilder

​        `public StringBuilder(String s)`：通过StringBuilder的构造方法就可以实现把 String 转换为 StringBuilder

##### StringBuilder转换为String

​        `public String toString()`：通过StringBuilder类中的 toString() 就可以实现把 StringBuilder 转换为 String

- 示例代码

```java
public class Test {
    public static void main(String[] args) {
        /*
             StringBuilder和String相互转换:
                String--->StringBuilder:  public StringBuilder(String str);
                StringBuilder--->String:  public String toString();
         */
        // String--->StringBuilder
        String str = "Hello-World";
        StringBuilder sb = new StringBuilder(str);// sb: "Hello-World"

        // StringBuilder--->String
        String s = sb.toString();// s: "Hello-World"
        System.out.println("s:"+s);
        System.out.println("sb:"+sb);
    }
}
```

#### 小结

- ```java
  StringBuilder:
  	public StringBuilder(String str);  String--->StringBuilder
      public String toString();    StringBuilder--->String
  ```

  

### 实操--2.6 字符串拼接升级版案例

#### 需求

​	定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，

​	并在控制台输出结果。例如，数组为int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]

#### 分析

1. 定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
2. 定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回。返回值类型 String，参数列表 int[] arr
3. 在方法中用 StringBuilder 按照要求进行拼接，并把结果转成 String 返回
4. 调用方法，用一个变量接收结果
5. 输出结果

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求
                定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，
                并在控制台输出结果。例如，数组为int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]
         */
        int[] arr = {1,2,3};
        System.out.println(concat(arr));
    }

    /**
     * 按照指定格式拼接数组元素
     *
     * @param arr
     * @return 拼接后的字符串
     */
    public static String concat(int[] arr) {
        // 1.创建一个空的可变字符串对象
        StringBuilder sb = new StringBuilder();

        // 2.循环遍历arr数组中的元素
        for (int i = 0; i < arr.length; i++) {
            // 3.在循环中,获取数组元素
            int e = arr[i];
            // 4.在循环中,判断遍历出来的元素
            if (i == 0) {
                // 4.如果遍历出来的元素是第一个元素,拼接的格式为: [+元素+逗号空格
                sb.append("[").append(e).append(", ");
            } else if (i == arr.length - 1) {
                // 4.如果遍历出来的元素是最后一个元素,拼接的格式为: 元素+]
                sb.append(e).append("]");
            } else {
                // 4.如果遍历出来的元素是中间元素,拼接的格式为: 元素+逗号空格
                sb.append(e).append(", ");
            }
        }
        // 5.返回字符串
        return sb.toString();
    }
}

```

#### 小结

略

### 实操--2.7 字符串反转升级版案例

#### 需求

​	定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出结果

​	例如，键盘录入abc，输出结果 cba

#### 分析

1. 键盘录入一个字符串，用 Scanner 实现
2. 定义一个方法，实现字符串反转。返回值类型 String，参数 String s
3. 在方法中用StringBuilder实现字符串的反转，并把结果转成String返回
4. 调用方法，用一个变量接收结果
5. 输出结果

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求
            	定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出结果
            	例如，键盘录入abc，输出结果 cba
         */
        // 键盘录入一个字符串
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串:");
        String str = sc.next();

        // 调用reverse反转方法
        // 打印输出
        System.out.println(reverse(str));
    }

    public static String reverse(String str){
        // 1.创建一个可变的字符串,字符串中的内容和参数str一样
        //StringBuilder sb = new StringBuilder(str);

        // 2.调用可变字符串的reverse方法
        //sb.reverse();

        // 3.返回
        //return sb.toString();

        // 链式编程
        return new StringBuilder(str).reverse().toString();
    }
}

```

#### 小结

略

## 知识点--3. ArrayList

### 知识点--3.1 集合的概述以及与数组的区别

#### 目标

- 能够了解集合的概念和知道集合与数组的区别

#### 路径

- 集合的概述
- 集合与数组的区别

#### 讲解

##### 集合的概述

- 思考: 编程的时候如果要存储多个数据,首先我们想到的就是数组,但是数组的特点是长度固定,所以数组不一定能满足我们的需求,更适应不了变化的需求,那么应该如何选择呢? 
- 什么是集合
  - 集合其实就是一个大小可变的容器,可以用来存储多个引用数据类型的数据

##### 集合与数组的区别

- 数组：数组大小固定
- 集合：集合大小可动态扩展

#### 小结

- 集合: 是一个大小可变的容器,可以用来存储多个引用类型的数据
- 集合和数组的区别:
  - 数组大小是固定
  - 集合大小是可变

### 知识点--3.2 ArrayList类概述

#### 目标

- 能够理解ArrayList类的概念

#### 路径

- ArrayList类概述

#### 讲解

集合类有很多,目前我们先学习一个: `ArrayList`

##### ArrayList类概述

- 通过查看ArrayList类的API:
  - `java.util.ArrayList <E>` ：该类需要 import导入使后使用。  
  - <E> 表示一种未知的数据类型,叫做泛型,用于约束集合中存储元素的数据类型
  - 怎么用呢?
    - 在出现E的地方我们使用引用数据类型替换即可，表示我们将存储哪种引用类型的元素。
    - 例如: 
      - ArrayList<String> 表示ArrayList集合中只能存储String类型的对象
      - ArrayList<Student> 表示ArrayList集合中只能存储Student类型的对象
      - .....
      - ArrayList<int> 编译报错,因为E这个位置只能写引用数据类型
  - 概述:
    - ArrayList类底层是大小可变的数组的实现，存储在内的数据称为元素。也就是说ArrayList 中可以不断添加元素，其大小会自动增长。

#### 小结

- 概述: ArrayList类底层是一个大小可变的数组实现
- 使用ArrayList<E>类的时候,在E出现的位置使用引用数据类型替换,表示该集合可以存储哪种引用类型的元素

### 知识点--3.3 ArrayList类构造方法

#### 目标

- 能够使用ArrayList类的构造方法创建ArrayList集合对象

#### 路径

- 介绍ArrayList的构造方法
- 案例演示

#### 讲解

- 介绍ArrayList的构造方法

  - `ArrayList()`  构造一个初始容量为 10 的空列表。 

- 案例演示

  ```java
  class Student{}
  public class Test {
      public static void main(String[] args) {
          /*
              ArrayList<E>类构造方法:
                  public ArrayList()  构造一个初始容量为 10 的空列表。
                  由于集合中只能存储引用数据类型的数据,所以如果需要存储基本数据类型的数据必须把该基本数据类型转换为对应的包装类类型
                  基本数据类型          对应          包装类类型
                  byte                                Byte
                  short                               Short
                  int                                 Integer
                  long                                Long
                  float                               Float
                  double                              Double
                  char                                Character
                  boolean                             Boolean
           */
          // 创建一个ArrayList集合对象,限制集合中元素的类型为String类型
          ArrayList<String> list1 = new ArrayList<>();
  
          // 创建一个ArrayList集合对象,限制集合中元素的类型为Student类型
          ArrayList<Student> list2 = new ArrayList<>();
  
          // 创建一个ArrayList集合对象,限制集合中元素的类型为整数类型
          //ArrayList<int> list3 = new ArrayList<>();// 编译报错
          ArrayList<Integer> list3 = new ArrayList<>();
  
          // 创建一个ArrayList集合对象,限制集合中元素的类型为小数类型
          ArrayList<Double> list4 = new ArrayList<>();
  
          // 创建一个ArrayList集合对象,限制集合中元素的类型为字符类型
          ArrayList<Character> list5 = new ArrayList<>();
  
  
          // 扩展: 基本类型和包装类类型如何相互转换
          // 基本类型--->对应的包装类类型
          Integer i = 10;// 会自动把int类型的10转换为Integer类型的对象(自动装箱)
  
          // 包装类类型--->对应的基本类型
          int num = i;// 会自动把Integer类型的对象转换为int类型的数据(自动拆箱)
      }
  }
  
  ```

#### 小结

- 略

### 知识点-- 3.4 ArrayList类添加元素方法

#### 目标

- 能够往ArrayList集合中添加元素

#### 路径

- ArrayList添加元素的方法

#### 讲解

- ArrayList添加元素的方法

  -  public boolean add(E e)：将指定的元素追加到此集合的末尾
  -  public void add(int index,E element)：在此集合中的指定位置插入指定的元素

- 案例演示:

  ```java
  public class Test {
      public static void main(String[] args) {
          /*
              ArrayList<E>类添加元素方法:
                  - public boolean add(E e)：将指定的元素追加到此集合的末尾
                  - public void add(int index,E element)：在此集合中的指定位置插入指定的元素
           */
          // 创建ArrayList集合对象,限制集合中元素的类型为String类型
          ArrayList<String> list = new ArrayList<>();
  
          // 往集合中添加元素
          list.add("谢霆锋");
          list.add("贾乃亮");
          list.add("王宝强");
          System.out.println("list:"+list);// [谢霆锋, 贾乃亮, 王宝强]
  
          // 往索引为1的位置添加陈羽凡
          list.add(1, "陈羽凡");
          System.out.println("list:"+list);// [谢霆锋, 陈羽凡, 贾乃亮, 王宝强]
  
      }
  }
  
  ```

#### 小结

- 略

### 扩展--集合存储基本数据类型以及指定位置添加元素的注意事项

```java
public class Test {
    public static void main(String[] args) {
        // 创建ArrayList集合对象,限制集合中元素的类型为Integer类型
        ArrayList<Integer> list = new ArrayList<>();

        // 往集合中添加元素
        list.add(10);
        list.add(20);
        list.add(30);
        list.add(40);
        System.out.println("list:"+list);// list:[10, 20, 30, 40]

        // 思考一:指定索引为4的位置添加一个元素50,是否可以?---可以
        list.add(4,50);// 相当于list.add(50);
        System.out.println("list:"+list);// list:[10, 20, 30, 40, 50]

        // 思考二:指定索引为6的位置添加一个元素100,是否可以?----不可以
        //list.add(6,100);// 运行报异常,IndexOutOfBoundsException索引越界异常

    }
}

```



### 知识点--3.5 ArrayList类常用方法

#### 目标

- 会使用ArrayList常用方法

#### 步骤

- ArrayList常用方法

#### 讲解

##### ArrayList常用方法

```java
public boolean   remove(Object o) 删除指定的元素，返回删除是否成功
public E   remove(int   index) 删除指定索引处的元素，返回被删除的元素
public E   set(int index, E  element) 修改指定索引处的元素，返回被修改的元素
public E   get(int   index) 返回指定索引处的元素
public int   size() 返回集合中的元素的个数
```



##### 示例代码

```java
public class Test {
    public static void main(String[] args) {
        /*
            ArrayList类常用方法:
                public boolean  remove(Object o) 删除指定的元素，返回删除是否成功
                public E   remove(int   index) 删除指定索引处的元素，返回被删除的元素
                public E   set(int index, E  element) 修改指定索引处的元素，返回被修改的元素
                public E   get(int   index) 返回指定索引处的元素
                public int   size() 返回集合中的元素的个数
         */
        // 创建ArrayList集合对象,限制集合中的元素类型为String类型
        ArrayList<String> list = new ArrayList<>();
        // 往集合中添加元素
        list.add("张柏芝");
        list.add("白百何");
        list.add("李小璐");
        list.add("马蓉");
        System.out.println("list:"+list);// list:[张柏芝, 白百何, 李小璐, 马蓉]

        // 需求:删除白百何这个元素
        list.remove("白百何");
        System.out.println("删除后list:"+list);// list:[张柏芝, 李小璐, 马蓉]

        // 需求:删除索引为1的元素
        String removeE = list.remove(1);
        System.out.println("被删除的元素:"+removeE);// 李小璐
        System.out.println("删除后list:"+list);// list:[张柏芝, 马蓉]

        // 需求:使用董洁去替换索引为1的元素
        String setE = list.set(1, "董洁");
        System.out.println("被替换的元素:"+setE);// 马蓉
        System.out.println("替换后list:"+list);// list:[张柏芝, 董洁]

        // 需求:获取索引为1的元素
        String e = list.get(1);
        System.out.println("索引为1的元素:"+e);// 董洁

        // 需求:获取集合中元素的个数
        int size = list.size();
        System.out.println("集合中元素个数:"+size);// 2
    }
}

```

- 注意事项

  ```java
  public class Test2_remove方法的注意事项 {
      public static void main(String[] args) {
          // 创建ArrayList集合对象,限制集合中元素的类型为Integer类型
          ArrayList<Integer> list = new ArrayList<>();
  
          // 往集合中添加元素
          list.add(1);
          list.add(2);
          list.add(3);
          list.add(4);
          System.out.println("list:"+list);// list:[1, 2, 3, 4]
          // public boolean  remove(Object o) 删除指定的元素，返回删除是否成功
          // public E   remove(int   index) 删除指定索引处的元素，返回被删除的元素
          // 问题: list.remove(1);??? 到底是删除1这个元素,还是删除1索引位置对应的元素???????
          list.remove(1);
          System.out.println(list);// [1, 3, 4]
  
      }
  }
  ```

  

#### 小结

```java
ArrayList常用方法:
    public boolean remove(Object o)   删除指定的元素，返回删除是否成功
    public E   remove(int   index)    删除指定索引处的元素，返回被删除的元素
    public E   set(int index, E  element) 修改指定索引处的元素，返回被修改的元素
    public E   get(int   index) 返回指定索引处的元素
    public int   size() 返回集合中的元素的个数
```

### 实操-3.6 ArrayList存储字符串并遍历

#### 需求

- 创建一个存储字符串的集合，存储3个字符串元素，使用程序实现在控制台遍历该集合

#### 分析

1. 创建集合对象
2. 往集合中添加字符串对象
3. 遍历集合，首先要能够获取到集合中的每一个元素，这个通过get(int index)方法实现
4. 遍历集合，其次要能够获取到集合的长度，这个通过size()方法实现
5. 遍历集合的通用格式

#### 实现

```java
ppublic class Test {
    public static void main(String[] args) {
        /*
            需求
                - 创建一个存储字符串的集合，存储3个字符串元素，使用程序实现在控制台遍历该集合
         */
        // 创建ArrayList集合,限制集合中元素的类型为String类型
        ArrayList<String> list = new ArrayList<>();

        // 往集合中存储3个字符串元素
        list.add("张三丰");
        list.add("张翠山");
        list.add("张无忌");

        // 循环遍历集合中的元素,打印输出
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}

```

#### 小结

略

### 实操--3.7 ArrayList存储学生对象并遍历

#### 需求

- 创建一个存储学生对象的集合，存储3个学生对象，使用程序实现在控制台遍历该集合

#### 分析

1. 定义学生类
2. 创建集合对象
3. 创建学生对象
4. 添加学生对象到集合中
5. 遍历集合，采用通用遍历格式实现

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求
                - 创建一个存储学生对象的集合，存储3个学生对象，使用程序实现在控制台遍历该集合
         */
        // 创建ArrayList集合,限制集合中元素的类型为Student类型
        ArrayList<Student> list = new ArrayList<>();

        // 往集合中存储3个学生对象
        Student stu1 = new Student("张三",18);
        Student stu2 = new Student("李四",19);
        Student stu3 = new Student("王五",20);

        list.add(stu1);
        list.add(stu2);
        list.add(stu3);

        // 循环遍历集合元素,打印输出
        for (int i = 0; i < list.size(); i++) {
            // 获取元素
            Student stu = list.get(i);
            System.out.println("姓名:"+stu.getName()+",年龄:"+stu.getAge());
        }

    }
}

```

![image-20200711163051074](img\image-20200711163051074.png)

#### 小结

略

### 实操--3.8 ArrayList存储学生对象并遍历升级版

#### 需求

- 创建一个存储学生对象的集合，存储3个学生对象，使用程序实现在控制台遍历该集合 ( 学生的姓名和年龄来自于键盘录入)

#### 分析

1. 定义学生类，为了键盘录入数据方便，把学生类中的成员变量都定义为String类型
2. 创建集合对象
3. 键盘录入学生对象所需要的数据
4. 创建学生对象，把键盘录入的数据赋值给学生对象的成员变量
5. 往集合中添加学生对象
6. 遍历集合，采用通用遍历格式实现

#### 实现

```java
public class Test {
    public static void main(String[] args) {
        /*
            需求:
                创建一个存储学生对象的集合，存储3个学生对象，使用程序实现在控制台遍历该集合
                ( 学生的姓名和年龄来自于键盘录入)
         */
        // 创建ArrayList集合,限制集合中元素的类型为Student类型
        ArrayList<Student> list = new ArrayList<>();

        // 往集合中存储3个学生对象
        addStudent(list);
        addStudent(list);
        addStudent(list);

        // 循环遍历集合元素,打印输出
        for (int i = 0; i < list.size(); i++) {
            // 获取元素
            Student stu = list.get(i);
            System.out.println("姓名:"+stu.getName()+",年龄:"+stu.getAge());
        }
    }

    // 定义一个方法,用来添加学生对象到集合中
    public static void addStudent(ArrayList<Student> list){
        // 创建Scanner对象,键盘录入姓名和年龄
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入学生的姓名:");
        String name = sc.next();
        System.out.println("请输入学生的年龄:");
        int age = sc.nextInt();

        // 创建学生对象
        Student stu1 = new Student(name,age);

        // 把学生对象添加到集合中
        list.add(stu1);
    }
}

```

![image-20200810163509635](img\image-20200810163509635.png)

#### 小结

略

## 总结

```java
String类:
	概述: 表示不可变的字符串
    创建字符串对象:
		1.通过String类的构造方法     堆区
            public String();
			public String(char[] chs)
            public String(byte[] bys);
			public String(String str); 例如:String str = new String("abc");// str:"abc"

        2.通过双引号直接赋值的方式    常量池
           String str = "abc";
     常用方法: 功能
         concat();  //把两个字符串拼接起来，获取一个新的字符串
        ★length();  //获取字符串的长度(其实就是获取字符串中 字符的个数)      
        ★equals();  //比较两个字符串的内容是否相同。    //区分大小写
        equalsIgnoreCase();  //比较两个字符串的内容是否相同。    //忽略大小写
        ★charAt();  //根据给定的索引，获取对应位置的字符
        ★indexOf(); //获取指定的字符 在字符串中 第一次出现的位置(索引)，找不到返回-1
            //int index = a1.indexOf('h');   从头找，'h'第一次出现的位置
            //int index = a1.indexOf('h',3); 从索引为3的元素开始往后找，'h'第一次出现的位置
       lastIndexOf();  //获取指定的字符 在字符串中 最后一次出现的位置(索引)，找不到返回-1
            //int index = a1.lastIndexOf('h');   从尾部找，'h'最后一次出现的位置
            //int index = a1.lastIndexOf('h',3); 从索引为3的元素开始往前找，'h'最后一次出现的位置 
       ★substring(); //截取字符串，返回新的字符串
                    //String newStr = a1.substring(2);       //从给定索引，直接截取到字符串末尾
                    //String newStr = a1.substring(2,5);     //包左不包右(前闭后开), 能取索引2的元素，不能取索引5的元素
		★isEmpty(); //判断字符串是否为空(长度为0返回true，不为0返回false)
        ★contains();    //判断字符串中是否包含 给定的字符串。
        endsWith(); //判断字符串是否以 给定的字符串 结尾。
        startsWith(); //判断字符串是否以 给定的字符串 开头。
		
        ★replace(); //用新内容替代旧内容，返回新的字符串    
        toLowerCase();  //把字母都转成其对应的小写形式。
        toUpperCase();  //把字母都转成其对应的大写形式。
		toCharArray() // 把字符串转换为数组
		getBytes() // 把字符串转换为字节数组        
		★trim();            //移除首尾空格。
		★split();   //根据给定的内容，切割字符串，返回字符串数组

   StringBuilder:
		概述: 表示可变的字符串
        创建StringBuilder对象:
			 StringBuilder();
			 StringBuilder(String str)
        常用方法:
		    StringBuilder append(任意数据)  拼接
            StringBuilder insert(int index,任意数据) 插入
            StringBuilder reverse() 反转
            String toString()  转换为String对象 
   ArrayList<E>:
		使用: 在出现E的位置写指定的引用数据类型替换,用来限制集合中元素的数据类型
		概述: 表示集合对象
        作用: 可以存储多个不固定数量的引用数据类型的数据
        构造方法:
				ArrayList(); 
		成员方法:
			add(E e) 在末尾增加元素
            add(int index, E e) 在指定位置添加元素
            remove(Object e) 删除指定的元素
            remove(int index) 删除指定索引位置上的元素
            set(int index,E e) 替换指定索引位置上的元素
            get(int index)  根据指定索引获取元素
            size() 获取集合元素个数(大小,长度)
                
- 能够知道字符串对象通过构造方法创建和直接赋值的区别
       构造方法: 在堆区
       直接赋值: 常量池
           
- 能够完成用户登录案例
      1.定义2个字符串变量,存储已知的用户名和密码
      2.键盘录入用户名和密码
      3.比较判断用户输入的用户名和密码是否和已知的已知
      4.判断后,给出相应的提示
           
- 能够完成统计字符串中大写,小写,数字字符的个数
       1.键盘录入一个字符串数据
       2.定义三个统计变量,分别统计大写,小写,数字字符的个数
       3.循环遍历字符串,获取每一个字符
       4.判断,累加
           
- 能够知道String和StringBuilder的区别
        String: 不可变字符串
        StringBuilder:可变字符串
            
- 能够完成String和StringBuilder的相互转换
        String--->StringBuilder:  public StringBuilder(String str);
		StringBuilder-->String:  public String toString();
- 能够使用StringBuilder完成字符串的拼接
      append(任意类型的数据);

- 能够使用StringBuilder完成字符串的反转
     reverse();

- 能够知道集合和数组的区别
    集合:大小不固定
    数组:大小固定
        
- 能够完成ArrayList集合添加字符串并遍历
    泛型指定为String类型   add(E e)\  size()  \  get(int index)
- 能够完成ArrayList集合添加学生对象并遍历
    泛型指定为Student类型  add(E e)\  size()  \  get(int index)
```

