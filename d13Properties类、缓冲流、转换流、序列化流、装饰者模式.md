# day13【Properties类、缓冲流、转换流、序列化流、装饰者模式、commons-io工具包】

## 今日内容

- IO异常处理
- Properties类
- 缓冲流
- 转换流
- 序列化\反序列化流
- 装饰者模式
- commons-io工具包

## 教学目标  

- [ ] 能够使用Properties的load方法加载文件中配置信息
- [ ] 能够使用字节缓冲流读取数据到程序
- [ ] 能够使用字节缓冲流写出数据到文件
- [ ] 能够明确字符缓冲流的作用和基本用法
- [ ] 能够使用缓冲流的特殊功能
- [ ] 能够阐述编码表的意义
- [ ] 能够使用转换流读取指定编码的文本文件
- [ ] 能够使用转换流写入指定编码的文本文件
- [ ] 能够使用序列化流写出对象到文件
- [ ] 能够使用反序列化流读取文件到程序中
- [ ] 能够理解装饰模式的实现步骤
- [ ] 能够使用commons-io工具包

# 第一章 IO资源的处理

## 知识点-- JDK7前处理

### 目标

- 掌握jdk7之前处理IO异常的方式

### 路径

- jdk7之前处理IO异常的方式

### 讲解

之前的入门练习，我们一直把异常抛出，而实际开发中并不能这样处理，建议使用`try...catch...finally` 代码块，处理异常部分，代码使用演示：

```java  
public class Test {
    public static void main(String[] args) {
        /*
            jdk7前IO异常的处理:
                try{
                    可能会出现异常的代码
                }catch(异常类型 变量名){

                }finally{
                    一般正常情况永远都会执行(释放资源)
                }
         */
        // IO核心代码
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            // 1.创建输入流对象,关联数据源文件路径
            fis = new FileInputStream("day13\\aaa\\hb.jpg");

            // 2.创建输出流对象,关联目的地文件路径
            fos = new FileOutputStream("day13\\aaa\\hbCopy1.jpg");

            // 3.定义变量,用来存储读取到的数据
            byte[] bys = new byte[8192];
            int len;

            // 4.循环读取数据
            while ((len = fis.read(bys)) != -1) {
                // 5.在循环中,写出数据
                fos.write(bys, 0, len);
            }

        } catch (Exception e) {

        } finally {
            // 6.关闭流,释放资源
            if (fos != null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (fis != null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

### 小结

略

## 知识点-- JDK7的处理

### 目标

- 掌握jdk7处理IO异常的方式

### 路径

- jdk7处理IO异常的方式

### 讲解

还可以使用JDK7优化后的`try-with-resource` 语句，该语句确保了每个资源在语句结束时关闭。所谓的资源（resource）是指在程序完成后，必须关闭的对象。

格式：

```java
try (创建流对象语句，如果多个,使用';'隔开) {
	// 读写数据
} catch (IOException e) {
	e.printStackTrace();
}
```

代码使用演示：

```java
public class Test {
    public static void main(String[] args) {
        /*
            jdk7IO异常处理: 使用try...with..resource处理,确保了每个资源在语句结束时自动关闭
                try(创建资源对象的代码){
                }catch(异常类型 变量名){
                }
         */
        try(
                // 1.创建输入流对象,关联数据源文件路径
                FileInputStream fis = new FileInputStream("day13\\aaa\\hb.jpg");

                // 2.创建输出流对象,关联目的地文件路径
                FileOutputStream fos = new FileOutputStream("day13\\aaa\\hbCopy2.jpg");
         ){

            // 3.定义变量,用来存储读取到的数据
            byte[] bys = new byte[8192];
            int len;

            // 4.循环读取数据
            while ((len = fis.read(bys)) != -1) {
                // 5.在循环中,写出数据
                fos.write(bys, 0, len);
            }
        }catch (Exception e){
            e.printStackTrace();
        }

    }
}

```

### 小结

略

# 第二章 属性集

## 知识点-- Properties类

### 目标

- 掌握Properties类的使用

### 路径

- Properties类的概述
- Properties类的构造方法
- Properties类成员方法
- Properties类与流相关的方法

### 讲解

#### Properties类的概述

`java.util.Properties ` 继承于` Hashtable` ，来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应值都是一个字符串。该类也被许多Java类使用，比如获取系统属性时，`System.getProperties` 方法就是返回一个`Properties`对象。

#### Properties类的构造方法

- `public Properties()` :创建一个空的属性列表。

#### Properties类存储方法

- `public Object setProperty(String key, String value)` ： 保存一对属性。  
- `public String getProperty(String key) ` ：使用此属性列表中指定的键搜索属性值。
- `public Set<String> stringPropertyNames() ` ：所有键的名称的集合。

```java
public class Test1_Properties类的概述和常用方法 {
    public static void main(String[] args) {
        /*
            Test1_Properties类的概述和常用方法:
                Properties类的概述
                    概述:java.util.Properties 继承于Hashtable ，来表示一个持久的属性集。
                    特点:用键值结构存储数据，每个键及其对应值都是一个字符串。
                Properties类的常用方法
                    构造方法:
                        Properties() 创建一个空的属性集
                    成员方法:
                        - public Object setProperty(String key, String value) ： 保存一对属性。
                        - public String getProperty(String key) ：使用此属性列表中指定的键搜索属性值。
                        - public Set<String> stringPropertyNames() ：所有键的名称的集合。
         */
        // 当成属性集使用---键和值的类型为String类型,常用
        // 创建一个空的属性集
        Properties pro = new Properties();

        // 往属性集中添加键值对
        pro.setProperty("k1","v1");
        pro.setProperty("k2","v2");
        pro.setProperty("k3","v3");
        pro.setProperty("k4","v4");

        // 获取所有的键
        Set<String> keys = pro.stringPropertyNames();

        // 循环遍历所有的键
        for (String key : keys) {
            // 根据键得到值
            String value = pro.getProperty(key);
            System.out.println(key+"="+value);
        }

        System.out.println("=========================");
        // 当成Map集合使用-----键和值的类型为Object类型,一般不用
        Properties pro2 = new Properties();

        // 往集合中添加键值对
        pro2.put("k","v");
        pro2.put(1,2);

        // 获取所有的键
        Set<Object> keySet = pro2.keySet();

        // 循环遍历所有的键
        for (Object k : keySet) {
            // 根据键找值
            Object v = pro2.get(k);
            System.out.println(k+":"+v);
        }

    }
}

```

#### Properties类与流相关的方法

- `public void load(InputStream inStream)`： 从字节输入流中读取键值对。 

参数中使用了字节输入流，通过流对象，可以关联到某文件上，这样就能够加载文本中的数据了。

加载代码演示：

```java
public class Test2_Properties与流一起操作 {
    public static void main(String[] args) throws Exception{
        /*
            Properties与流一起操作:
              加载配置文件中的数据:
                public void load(InputStream inStream)： 从字节输入流中读取键值对。
                参数中使用了字节输入流，通过流对象，可以关联到某文件上，这样就能够加载文本中的数据了。

             写回数据到配置文件中
               void store(OutputStream out, String comments) 写回键值对到配置文件中

            注意:
                文本中的数据，必须是键值对形式，可以使用空格、等号、冒号等符号分隔。
         */
        // 需求:读取a.txt文件中的数据到程序中,并打印输出
        // 创建一个空的属性集
        Properties pro = new Properties();

        // 创建字节输入流,关联数据源文件路径
        //FileInputStream fis = new FileInputStream("day13\\aaa\\a.txt");
        FileInputStream fis = new FileInputStream("day13\\aaa\\b.properties");

        // 加载文件中的数据
        pro.load(fis);

        // 关闭流,释放资源
        fis.close();

        // 打印加载的文件数据
        System.out.println(pro);// {password=123456, username=root}


        // 获取所有的键
        Set<String> keys = pro.stringPropertyNames();
        System.out.println(keys);// [password, username]

        System.out.println("=============================");
        // 需求:为a.txt添加一对键值对(url=http://www.boxuegu.com)
        // 1.为pro属性集对象添加键值对
        pro.setProperty("url","http://www.boxuegu.com");

        // 2.创建字节输出流对象,关联目的地文件路径
        //FileOutputStream fos = new FileOutputStream("day13\\aaa\\a.txt");
        FileOutputStream fos = new FileOutputStream("day13\\aaa\\b.properties");

        // 3.写回数据到文件中
        pro.store(fos,"zs");

        // 4.关闭流,释放资源
        fos.close();


    }
}

```

> 小贴士：文本中的数据，必须是键值对形式，可以使用空格、等号、冒号等符号分隔。

### 小结

略

# 第三章 缓冲流

## 知识点--缓冲流

### 目标

- 理解缓冲流的概述

### 路径

- 缓冲流的概述

### 讲解

昨天学习了基本的一些流，作为IO流的入门，今天我们要见识一些更强大的流。比如能够高效读写的缓冲流，能够转换编码的转换流，能够持久化存储对象的序列化流等等。这些功能更为强大的流，都是在基本的流对象基础之上创建而来的，就像穿上铠甲的武士一样，相当于是对基本流对象的一种增强。

缓冲流,也叫高效流，是对4个基本的`FileXxx` 流的增强，所以也是4个流，按照数据类型分类：

* **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
* **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。

### 小结

略

## 知识点--字节缓冲流

### 目标

- 掌握字节缓冲流的使用

### 路径

- 字节缓冲流的构造方法
- 拷贝文件效率测试

### 讲解

#### 字节缓冲流的构造方法

* `public BufferedInputStream(InputStream in)` ：创建一个 新的缓冲输入流。 
* `public BufferedOutputStream(OutputStream out)`： 创建一个新的缓冲输出流。

构造举例，代码如下：

```java
// 创建字节缓冲输入流
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("bis.txt"));
// 创建字节缓冲输出流
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("bos.txt"));
```

#### 拷贝文件效率测试

查询API，缓冲流读写方法与基本的流是一致的，我们通过复制大文件（375MB），测试它的效率。

1. 基本流，代码如下：

```java
public class BufferedDemo {
    public static void main(String[] args) throws FileNotFoundException {
        // 记录开始时间
      	long start = System.currentTimeMillis();
		// 创建流对象
        try (
        	FileInputStream fis = new FileInputStream("jdk8.exe");
        	FileOutputStream fos = new FileOutputStream("copy.exe")
        ){
        	// 读写数据
            int b;
            while ((b = fis.read()) != -1) {
                fos.write(b);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
		// 记录结束时间
        long end = System.currentTimeMillis();
        System.out.println("普通流复制时间:"+(end - start)+" 毫秒");
    }
}

十几分钟过去了...
```

2. 缓冲流，代码如下：

```java
public class BufferedDemo {
    public static void main(String[] args) throws FileNotFoundException {
        // 记录开始时间
      	long start = System.currentTimeMillis();
		// 创建流对象
        try (
        	BufferedInputStream bis = new BufferedInputStream(new FileInputStream("jdk8.exe"));
	     BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.exe"));
        ){
        // 读写数据
            int b;
            while ((b = bis.read()) != -1) {
                bos.write(b);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
		// 记录结束时间
        long end = System.currentTimeMillis();
        System.out.println("缓冲流复制时间:"+(end - start)+" 毫秒");
    }
}

缓冲流复制时间:8016 毫秒
```

如何更快呢？

使用数组的方式，代码如下：

```java
public class BufferedDemo {
    public static void main(String[] args) throws FileNotFoundException {
      	// 记录开始时间
        long start = System.currentTimeMillis();
		// 创建流对象
        try (
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream("jdk8.exe"));
		 BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.exe"));
        ){
          	// 读写数据
            int len;
            byte[] bytes = new byte[8*1024];
            while ((len = bis.read(bytes)) != -1) {
                bos.write(bytes, 0 , len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
		// 记录结束时间
        long end = System.currentTimeMillis();
        System.out.println("缓冲流使用数组复制时间:"+(end - start)+" 毫秒");
    }
}
缓冲流使用数组复制时间:666 毫秒
```

### 小结

略

## 知识点--字符缓冲流

### 目标

- 掌握字符缓冲流的使用

### 路径

- 字符缓冲流的构造方法
- 字符缓冲流的特有方法

### 讲解

#### 字符缓冲流的构造方法

* `public BufferedReader(Reader in)` ：创建一个 新的缓冲输入流。 
* `public BufferedWriter(Writer out)`： 创建一个新的缓冲输出流。

构造举例，代码如下：

```java
// 创建字符缓冲输入流
BufferedReader br = new BufferedReader(new FileReader("br.txt"));
// 创建字符缓冲输出流
BufferedWriter bw = new BufferedWriter(new FileWriter("bw.txt"));
```

#### 字符缓冲流的特有方法

字符缓冲流的基本方法与普通字符流调用方式一致，不再阐述，我们来看它们具备的特有方法。

* BufferedReader：`public String readLine()`: 读一行文字。 
* BufferedWriter：`public void newLine()`: 写一行行分隔符,由系统属性定义符号。 

`readLine`方法演示，代码如下：

```java
public class BufferedReaderDemo {
     // 字符缓冲输入流: 读取一行文字
    private static void method01() throws IOException {
        // 创建字符缓冲输入流对象,关联数据源文件路径
        FileReader fr = new FileReader("day13\\ccc\\a.txt");
        BufferedReader br = new BufferedReader(fr);

        // 读取行数据
        /*String line1 = br.readLine();
        System.out.println(line1);

        String line2 = br.readLine();
        System.out.println(line2);

        String line3 = br.readLine();
        System.out.println(line3);

        String line4 = br.readLine();
        System.out.println(line4);

        String line5 = br.readLine();
        System.out.println(line5);

        String line6 = br.readLine();
        System.out.println(line6);// null*/
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }

        // 关闭流,释放资源
        br.close();
    }
}
```

`newLine`方法演示，代码如下：

  ```java
public class BufferedWriterDemo throws IOException {
    public static void main(String[] args) throws IOException  {
      	// 创建流对象
		BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
      	// 写出数据
        bw.write("黑马");
      	// 写出换行
        bw.newLine();
        bw.write("程序");
        bw.newLine();
        bw.write("员");
        bw.newLine();
		// 释放资源
        bw.close();
    }
}
输出效果:
黑马
程序
员
  ```

### 小结

略

## 实操--文本排序

### 需求

请将文本信息恢复顺序。

```
3.侍中、侍郎郭攸之、费祎、董允等，此皆良实，志虑忠纯，是以先帝简拔以遗陛下。愚以为宫中之事，事无大小，悉以咨之，然后施行，必得裨补阙漏，有所广益。
8.愿陛下托臣以讨贼兴复之效，不效，则治臣之罪，以告先帝之灵。若无兴德之言，则责攸之、祎、允等之慢，以彰其咎；陛下亦宜自谋，以咨诹善道，察纳雅言，深追先帝遗诏，臣不胜受恩感激。
4.将军向宠，性行淑均，晓畅军事，试用之于昔日，先帝称之曰能，是以众议举宠为督。愚以为营中之事，悉以咨之，必能使行阵和睦，优劣得所。
2.宫中府中，俱为一体，陟罚臧否，不宜异同。若有作奸犯科及为忠善者，宜付有司论其刑赏，以昭陛下平明之理，不宜偏私，使内外异法也。
1.先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。然侍卫之臣不懈于内，忠志之士忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气，不宜妄自菲薄，引喻失义，以塞忠谏之路也。
9.今当远离，临表涕零，不知所言。
6.臣本布衣，躬耕于南阳，苟全性命于乱世，不求闻达于诸侯。先帝不以臣卑鄙，猥自枉屈，三顾臣于草庐之中，咨臣以当世之事，由是感激，遂许先帝以驱驰。后值倾覆，受任于败军之际，奉命于危难之间，尔来二十有一年矣。
7.先帝知臣谨慎，故临崩寄臣以大事也。受命以来，夙夜忧叹，恐付托不效，以伤先帝之明，故五月渡泸，深入不毛。今南方已定，兵甲已足，当奖率三军，北定中原，庶竭驽钝，攘除奸凶，兴复汉室，还于旧都。此臣所以报先帝而忠陛下之职分也。至于斟酌损益，进尽忠言，则攸之、祎、允之任也。
5.亲贤臣，远小人，此先汉所以兴隆也；亲小人，远贤臣，此后汉所以倾颓也。先帝在时，每与臣论此事，未尝不叹息痛恨于桓、灵也。侍中、尚书、长史、参军，此悉贞良死节之臣，愿陛下亲之信之，则汉室之隆，可计日而待也。
```

### 分析

1. 逐行读取文本信息。
2. 解析文本信息到集合中。
3. 遍历集合，按顺序，写出文本信息。

### 实现

```java
public class Test {
    public static void main(String[] args) throws Exception {
        /*
            需求:请将文本信息恢复顺序。
         */
        // 1.把文件中的数据以行的形式读取处理,存储到集合中
        // 1.1 创建字符缓冲输入流对象,关联数据源文件路径
        FileReader fr = new FileReader("day13\\ccc\\c.txt");
        BufferedReader br = new BufferedReader(fr);

        // 1.2 创建ArrayList集合,限制集合中元素的类型为String类型
        ArrayList<String> list = new ArrayList<>();

        // 1.3 循环读取文件中的行数据
        String line;
        while ((line = br.readLine()) != null) {
            // 1.4 在循环中,把读取到的行数据保存到集合中
            list.add(line);
        }
        // 1.5 关闭流,释放资源
        br.close();

        // 2.对集合中的行数据进行排序
        Collections.sort(list);// 按照字符串的升序排序
        System.out.println(list);

        // 3.把排好序的集合数据写回文件中
        // 3.1 创建字符缓冲输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day13\\ccc\\c.txt");
        BufferedWriter bw = new BufferedWriter(fw);

        // 3.2 循环遍历排好序的集合
        for (String s : list) {
            // 3.3 在循环中,把遍历出来的行数据写回文件中
            bw.write(s);
            // 换行
            bw.newLine();
        }
        // 3.4 关闭流,释放资源
        bw.close();
    }
}

```

### 小结

略

# 第四章 转换流

## 知识点--字符编码和字符集

### 目标

- 理解字符编码和字符集的概念

### 路径  

- 字符编码的概述  
- 字符集的概述

### 讲解

#### 字符编码的概述

计算机中储存的信息都是用二进制数表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果。按照某种规则，将字符存储到计算机中，称为**编码** 。反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码** 。比如说，按照A规则存储，同样按照A规则解析，那么就能显示正确的文本f符号。反之，按照A规则存储，再按照B规则解析，就会导致乱码现象。

* **字符编码`Character Encoding`** : 就是一套自然语言的字符与二进制数之间的对应规则。

#### 字符集的概述

* **字符集 `Charset`**：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符号、数字等。

计算机要准确的存储和识别各种字符集符号，需要进行字符编码，一套字符集必然至少有一套字符编码。常见字符集有ASCII字符集、GBK字符集、Unicode字符集等。![](img/1_charset.jpg)

可见，当指定了**编码**，它所对应的**字符集**自然就指定了，所以**编码**才是我们最终要关心的。

* **ASCII字符集** ：
  * ASCII（American Standard Code for Information Interchange，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统，用于显示现代英语，主要包括控制字符（回车键、退格、换行键等）和可显示字符（英文大小写字符、阿拉伯数字和西文符号）。
  * 基本的ASCII字符集，使用7位（bits）表示一个字符，共128字符。ASCII的扩展字符集使用8位（bits）表示一个字符，共256字符，方便支持**欧洲常用字符**。
* **ISO-8859-1字符集**：
  * 拉丁码表，别名Latin-1，用于**显示欧洲使用的语言，包括荷兰、丹麦、德语、意大利语、西班牙语等。**
  * ISO-5559-1使用单字节编码，兼容ASCII编码。
* **GBxxx字符集**：
  * GB就是国标的意思，是为了显示中文而设计的一套字符集。
  * **GB2312**：简体中文码表。一个小于127的字符的意义与原来相同。但两个大于127的字符连在一起时，就表示一个汉字，这样大约可以组合了**包含7000多个简体汉字**，此外数学符号、罗马希腊的字母、日文的假名们都编进去了，连在ASCII里本来就有的数字、标点、字母都统统重新编了两个字节长的编码，这就是常说的"全角"字符，而原来在127号以下的那些就叫"半角"字符了。
  * **GBK**：最常用的中文码表。是在GB2312标准基础上的扩展规范，使用了双字节编码方案，共收录了**21003个汉字**，完全兼容GB2312标准，同时支持繁体汉字以及日韩汉字等。
  * **GB18030**：最新的中文码表。**收录汉字70244个，**采用多字节编码，每个字可以由1个、2个或4个字节组成。支持中国国**内少数民族的文字，同时支持繁体汉字以及日韩汉字等。**
* **Unicode字符集** ：
  * Unicode编码系统为表达任意语言的任意字符而设计，是业界的一种标准，也**称为统一码、标准万国码。**
  * 它最多使用4个字节的数字来表达每个字母、符号，或者文字。有三种编码方案，UTF-8、UTF-16和UTF-32。**最为常用的UTF-8编码。**
  * UTF-8编码，可以用来表示Unicode标准中任何字符，它是电子邮件、网页及其他存储或传送文字的应用中，优先采用的编码。互联网工程工作小组（IETF）要求所有互联网协议都必须支持UTF-8编码。所以，我们开发Web应用，也要使用UTF-8编码。它使用一至四个字节为每个字符编码，编码规则：
    1. 128个US-ASCII字符，只需一个字节编码。
    2. 拉丁文等字符，需要二个字节编码。 
    3. 大部分常用字（含中文），使用三个字节编码。
    4. 其他极少使用的Unicode辅助字符，使用四字节编码。

### 小结

略

## 知识点--编码引出的问题

### 目标

- 了解编码引出的问题

### 路径

- 演示编码引出的问题

### 讲解

在IDEA中，使用`FileReader` 读取项目中的文本文件。由于IDEA的设置，都是默认的`UTF-8`编码，所以没有任何问题。但是，当读取Windows系统中创建的文本文件时，由于Windows系统的默认是GBK编码，就会出现乱码。

```java
public class ReaderDemo {
    public static void main(String[] args) throws IOException {
        FileReader fileReader = new FileReader("E:\\File_GBK.txt");
        int read;
        while ((read = fileReader.read()) != -1) {
            System.out.print((char)read);
        }
        fileReader.close();
    }
}
输出结果：
���
```

那么如何读取GBK编码的文件呢？ 

### 小结

略

## 知识点--InputStreamReader类  

### 目标

- 掌握InputStreamReader类 的使用

### 路径

- InputStreamReader类的概述
- InputStreamReader类的构造方法
- InputStreamReader类指定编码读取

### 讲解

#### InputStreamReader类的概述

转换流`java.io.InputStreamReader`，是Reader的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

#### InputStreamReader类的构造方法

* `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。 
* `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。

构造举例，代码如下： 

```java
InputStreamReader isr = new InputStreamReader(new FileInputStream("in.txt"));
InputStreamReader isr2 = new InputStreamReader(new FileInputStream("in.txt") , "GBK");
```

#### InputStreamReader类指定编码读取

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            InputStreamReader类:
                概述:转换流java.io.InputStreamReader，是Reader的子类，是从字节流到字符流的桥梁。
                特点:
                      1.它读取字节，并使用指定的字符集将其解码为字符。
                      2.它的字符集可以由名称指定，也可以接受平台的默认字符集。

                构造方法:
                    - InputStreamReader(InputStream in): 创建一个使用默认字符集的字符流。 idea默认编码utf8
                    - InputStreamReader(InputStream in, String charsetName): 创建一个指定字符集的字符流。 推荐

                成员方法: 使用Reader类的方法
         */
        /*// 创建转换输入流对象,关联数据源文件路径,指定编码为gbk
        FileInputStream fis = new FileInputStream("day13\\ddd\\gbk.txt");
        InputStreamReader isr = new InputStreamReader(fis,"gbk");

        // 定义一个变量,用来存储读取到的字符数据
        int len;

        // 循环读取
        while ((len = isr.read()) != -1){
            System.out.println((char) len);
        }
        // 关闭流,释放资源
        isr.close();*/

        System.out.println("====================================== ");
        // 创建转换输入流对象,关联数据源文件路径,指定编码为utf8
        FileInputStream fis = new FileInputStream("day13\\ddd\\utf8.txt");
        InputStreamReader isr = new InputStreamReader(fis,"utf8");

        // 定义一个变量,用来存储读取到的字符数据
        int len;

        // 循环读取
        while ((len = isr.read()) != -1){
            System.out.println((char) len);
        }
        // 关闭流,释放资源
        isr.close();

    }
}

```

### 小结

略

## 知识点--OutputStreamWriter类

### 目标

- 掌握OutputStreamWriter类的使用

### 路径

- OutputStreamWriter类的概述
- OutputStreamWriter类的构造方法
- OutputStreamWriter类指定编码读取

### 讲解

#### OutputStreamWriter类的概述

转换流`java.io.OutputStreamWriter` ，是Writer的子类，是从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

#### OutputStreamWriter类的构造方法

- `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。 idea默认的是utf8
- `OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。

构造举例，代码如下： 

```java
OutputStreamWriter isr = new OutputStreamWriter(new FileOutputStream("out.txt"));
OutputStreamWriter isr2 = new OutputStreamWriter(new FileOutputStream("out.txt") , "GBK");
```

#### OutputStreamWriter类指定编码读取

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            OutputStreamWriter类:
                概述:转换流java.io.OutputStreamWriter ，是Writer的子类
                特点: 可以指定编码写出字符数据,也可以接受平台默认的字符编码

                构造方法:
                    - OutputStreamWriter(OutputStream in): 创建一个使用默认字符集的字符流。 idea默认的是utf8
                    - OutputStreamWriter(OutputStream in, String charsetName): 创建一个指定字符集的字符流。


                成员方法: 使用Writer类的方法
         */
        // 需求:指定gbk编码,写出数据到文件中
        /*// 创建转换输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day13\\ddd\\gbk2.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");

        // 写出数据
        osw.write("中国好!");// 7个字节

        // 关闭流,释放资源
        osw.close();*/

        // 需求:指定utf8编码,写出数据到文件中
        // 创建转换输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day13\\ddd\\utf82.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos,"utf8");

        // 写出数据
        osw.write("中国好!");// 10个字节

        // 关闭流,释放资源
        osw.close();
    }
}

```

#### 转换流理解图解

**转换流是字节与字符间的桥梁！**![](img/2_zhuanhuan.jpg)

### 小结

略

## 实操--转换文件编码

### 需求

- 将GBK编码的文本文件，转换为UTF-8编码的文本文件。

### 分析

1. 指定GBK编码的转换流，读取文本文件。
2. 使用UTF-8编码的转换流，写出文本文件。

### 实现

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            需求: 将GBK编码的文本文件，转换为UTF-8编码的文本文件。
         */
        // 1.创建转换输入流对象,关联数据源文件路径,指定编码为gbk
        FileInputStream fis = new FileInputStream("day13\\ddd\\gbk.txt");
        InputStreamReader isr = new InputStreamReader(fis,"gbk");

        // 2.创建转换输出流对象,关联目的地文件路径,指定编码为utf8
        FileOutputStream fos = new FileOutputStream("day13\\ddd\\utf-8.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos,"utf8");

        // 3.定义变量,用来存储读取到的数据
        int len;

        // 4.循环读取
        while ((len = isr.read()) != -1) {
            // 5.在循环中,写出数据
            osw.write(len);
        }
        // 6.关闭流,释放资源
        osw.close();
        isr.close();
    }
}

```

### 小结

略

# 第五章 序列化

## 知识点--序列化和反序列化的概念

### 目标

- 理解序列化和反序列化的概念

### 路径

- 序列化
- 反序列化

### 讲解

Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。 

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行**反序列化**。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象。看图理解序列化： ![](img/3_xuliehua.jpg)

### 小结

略

## 知识点--ObjectOutputStream类

### 目标

- 掌握ObjectOutputStream类的使用

### 路径

- ObjectOutputStream类的概述
- ObjectOutputStream类构造方法
- ObjectOutputStream类序列化操作

### 讲解

#### ObjectOutputStream类的概述

`java.io.ObjectOutputStream ` 类，将Java对象的原始数据类型写出到文件,实现对象的持久存储。

#### ObjectOutputStream类构造方法

* `public ObjectOutputStream(OutputStream out) `： 创建一个指定OutputStream的ObjectOutputStream。

构造举例，代码如下：  

```java
FileOutputStream fileOut = new FileOutputStream("employee.txt");
ObjectOutputStream out = new ObjectOutputStream(fileOut);
```

#### ObjectOutputStream类序列化操作

1. 一个对象要想序列化，必须满足两个条件:

* 该类必须实现`java.io.Serializable ` 接口，`Serializable` 是一个标记接口

```java
public class Student implements Serializable {
    public String name;
    public int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}


```

2.写出对象方法

* `public final void writeObject (Object obj)` : 将指定的对象写出。

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            ObjectOutputStream类: 序列化流
                概述:java.io.ObjectOutputStream 类，将Java对象的原始数据类型写出到文件,实现对象的持久存储。
                构造方法:
                    public ObjectOutputStream(OutputStream out)： 创建一个指定OutputStream的ObjectOutputStream。
                成员方法:
                    public final void writeObject (Object obj) : 将指定的对象写出。

                注意事项:
                    1.要序列化的对象所属的类必须实现Serializable接口
         */
        // 创建学生对象
        Student stu = new Student("张三",18);
        // 需求:把stu对象保存到day13\\eee\\a.txt文件中
        // 1.创建序列化流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day13\\eee\\a.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);

        // 2.写出对象到文件中
        oos.writeObject(stu);

        // 3.关闭流,释放资源
        oos.close();
    }
}


```

### 小结

略

## 知识点--ObjectInputStream类

### 目标

- 掌握ObjectInputStream类的使用

### 路径

- ObjectInputStream类的概述
- ObjectInputStream类构造方法
- ObjectInputStream类反序列化操作

### 讲解

#### ObjectInputStream类的概述

java.io.ObjectInputStream反序列化流，将之前使用ObjectOutputStream序列化的原始数据恢复为对象。 

#### ObjectInputStream类构造方法

* `public ObjectInputStream(InputStream in) `： 创建一个指定InputStream的ObjectInputStream。

#### ObjectInputStream类反序列化操作1

如果能找到一个对象的class文件，我们可以进行反序列化操作，调用`ObjectInputStream`读取对象的方法：

- `public final Object readObject ()` : 读取一个对象。

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            ObjectInputStream类: 反序列化流
                概述:java.io.ObjectInputStream反序列化流，将之前使用ObjectOutputStream序列化的原始数据恢复为对象。
                构造方法:
                    public ObjectInputStream(InputStream in)： 创建一个指定InputStream的ObjectInputStream。
                成员方法:
                    public final Object readObject () : 读取一个对象。
         */
        // 需求:把day11中序列化的学生对象重构出来
        // 1.创建反序列化流对象,关联数据源文件路径
        FileInputStream fis = new FileInputStream("day13\\eee\\a.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);

        // 2.重构对象
        Student stu  = (Student) ois.readObject();

        // 3.关闭流,释放资源
        ois.close();

        System.out.println("重构的对象:"+stu);// Student{name='张三', age=18}
    }
}

```

### 小结

略

## 知识点--序列化和反序列化注意事项

### 目标

- 理解序列化和反序列化注意事项

### 路径

- 序列化的注意事项
- 反序列化的注意事项

### 讲解

#### 序列化的注意事项

* 该类必须实现`java.io.Serializable ` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。

* 该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。

  ```java
  public class Student implements Serializable {
      public String name;// 姓名
      public transient int age;// 年龄  transient:表示不序列化,瞬态
      public Animal anl;// 宠物
  
      public Student() {
      }
  
      public Student(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", age=" + age +",anl="+anl+
                  '}';
      }
  }
  
  public class Test1_序列化 {
      public static void main(String[] args) throws Exception{
          /*
                  序列化注意事项:
                      1.要序列化的对象所属的类必须实现Serializable接口
                      2.该类的所有属性必须是可序列化的
                      3.如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用transient 关键字修饰。
           */
          // 创建学生对象
          Student stu = new Student("张三",18);
          // 给stu对象的宠物属性赋值
          stu.anl = new Animal();
  
          // 需求:把stu对象保存到day13\\eee\\a.txt文件中
          // 1.创建序列化流对象,关联目的地文件路径
          FileOutputStream fos = new FileOutputStream("day13\\eee\\b.txt");
          ObjectOutputStream oos = new ObjectOutputStream(fos);
  
          // 2.写出对象到文件中
          oos.writeObject(stu);
  
          // 3.关闭流,释放资源
          oos.close();
      }
  }
  ```

  

#### 反序列化的注意事项

- 对于JVM可以反序列化对象，它必须是能够找到class文件的类。如果找不到该类的class文件，则抛出一个 `ClassNotFoundException` 异常。

- 另外，当JVM反序列化对象时，能找到class文件，但是class文件在序列化对象之后发生了修改，那么反序列化操作也会失败，抛出一个`InvalidClassException`异常。发生这个异常的原因如下：
  - 该类的序列版本号与从流中读取的类描述符的版本号不匹配 
  - 该类包含未知数据类型 
  - 该类没有可访问的无参数构造方法 

`Serializable` 接口给需要序列化的类，提供了一个序列版本号。`serialVersionUID` 该版本号的目的在于验证序列化的对象和对应类是否版本匹配。

```java
public class Student implements Serializable {
    // 版本号
    static final long serialVersionUID = 2L;

    public String name;
    public int age;

    public String sex;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class Test2_反序列化 {
    public static void main(String[] args) throws Exception{
        /*
             反序列化注意事项:
                 1.对于JVM可以反序列化对象，它必须是能够找到class文件的类。如果找不到该类的class文件，则抛出一个 ClassNotFoundException 异常。
                 2.当JVM反序列化对象时，能找到class文件，但是class文件在序列化对象之后发生了修改，那么反序列化操作也会失败，抛出一个InvalidClassException异常。
         */
        // 1.创建反序列化流对象,关联数据源文件路径
        FileInputStream fis = new FileInputStream("day13\\eee\\c.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);

        // 2.重构对象
        Student stu = (Student) ois.readObject();

        // 3.关闭流,释放资源
        ois.close();

        System.out.println("重构的对象:"+stu);// Student{name='张三', age=18}
    }
}
```

### 小结

略

## 实操--序列化集合

### 需求

1. 将存有多个自定义对象的集合序列化操作，保存到`list.txt`文件中。
2. 反序列化`list.txt` ，并遍历集合，打印对象信息。

### 分析

1. 把若干学生对象 ，保存到集合中。
2. 把集合序列化。
3. 反序列化读取时，只需要读取一次，转换为集合类型。
4. 遍历集合，可以打印所有的学生信息

### 实现

```java
public class SerTest {
	public static void main(String[] args) throws Exception {
		// 创建 学生对象
		Student student = new Student("老王", "laow");
		Student student2 = new Student("老张", "laoz");
		Student student3 = new Student("老李", "laol");

		ArrayList<Student> arrayList = new ArrayList<>();
		arrayList.add(student);
		arrayList.add(student2);
		arrayList.add(student3);
		// 序列化操作
		// serializ(arrayList);
		
		// 反序列化  
		ObjectInputStream ois  = new ObjectInputStream(new FileInputStream("list.txt"));
		// 读取对象,强转为ArrayList类型
		ArrayList<Student> list  = (ArrayList<Student>)ois.readObject();
		
      	for (int i = 0; i < list.size(); i++ ){
          	Student s = list.get(i);
        	System.out.println(s.getName()+"--"+ s.getPwd());
      	}
	}

	private static void serializ(ArrayList<Student> arrayList) throws Exception {
		// 创建 序列化流 
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("list.txt"));
		// 写出对象
		oos.writeObject(arrayList);
		// 释放资源
		oos.close();
	}
}
```

### 小结

略

#  第六章 打印流

### 目标

- 理解打印流的使用

### 路径

- 打印流的概述
- 打印流的使用

### 讲解

#### 打印流的概述

平时我们在控制台打印输出，是调用`print`方法和`println`方法完成的，这两个方法都来自于`java.io.PrintStream`类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。

#### 打印流的使用

* `public PrintStream(String fileName)  `： 使用指定的文件名创建一个新的打印流。

构造举例，代码如下：  

```java
PrintStream ps = new PrintStream("ps.txt")；
```

`System.out`就是`PrintStream`类型的，只不过它的流向是系统规定的，打印在控制台上。不过，既然是流对象，我们就可以玩一个"小把戏"，将数据输出到指定文本文件中。

```java
public class Test {
    public static void main(String[] args) throws Exception {
        /*
            PrintStream打印流:  字节输出流
                概述:java.io.PrintStream类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。
                构造方法:
                    public PrintStream(String fileName)： 使用指定的文件名创建一个新的打印流。
                成员方法:
                    println(任务类型的数据); 打印数据后换行
                    print(任意类型数据);打印数据后不换行
         */
        // 创建打印流对象,关联目的地文件路径
        PrintStream ps = new PrintStream("day13\\eee\\d.txt");

        // 打印数据
        ps.println(97);
        ps.println(3.14);
        ps.println('a');
        ps.println(true);
        ps.println("itheima");

        ps.print(98);
        ps.print(1.2);
        ps.print(false);
        ps.print("itcast");

        // 关闭流,释放资源
        ps.close();

        System.out.println("================================");
        // System类中的打印流,默认是打印到控制台
        System.out.println(99);

        // 上面这行代码,等价于下面这2行代码
        PrintStream ps1 = System.out;
        ps1.println(99);

        System.out.println("================================");
        // 需求:改变系统打印流的目的地文件
        PrintStream ps2 = new PrintStream("day13\\eee\\e.txt");
        System.setOut(ps2);

        System.out.println(99);


    }
}

```

### 小结

略

# 第七章 装饰设计模式

### 目标

- 会使用装饰设计模式

### 路径

- 装饰模式概述
- 案例演示

### 讲解

#### 装饰模式概述

在我们今天所学的缓冲流中涉及到java的一种设计模式，叫做装饰模式，我们来认识并学习一下这个设计模式。

**装饰模式指的是在不改变原类, 不使用继承的基础上，动态地扩展一个对象的功能。**

**装饰模式遵循原则:**

1. 装饰类和被装饰类必须实现相同的接口
2. 在装饰类中必须传入被装饰类的引用
3. 在装饰类中对需要扩展的方法进行扩展
4. 在装饰类中对不需要扩展的方法调用被装饰类中的同名方法

#### 案例演示

##### 准备环境:

1. 编写一个Star接口, 提供sing 和 dance抽象方法
2. 编写一个LiuDeHua类,实现Star接口,重写抽象方法

```java
public interface Star {
    public void sing();
    public void dance();
}
```

```java
public class LiuDeHua implements Star {
    @Override
    public void sing() {
        System.out.println("刘德华在唱忘情水...");
    }
    @Override
    public void dance() {
        System.out.println("刘德华在跳街舞...");
    }
}
```

##### 需求:

​	在不改变原类的基础上对LiuDeHua类的sing方法进行扩展

##### 实现步骤:

1. 编写一个LiuDeHuaWarpper类, 实现Star接口,重写抽象方法
2. 提供LiuDeHuaWarpper类的有参构造, 传入LiuDeHua类对象
3. 在LiuDeHuaWarpper类中对需要增强的sing方法进行增强
4. 在LiuDeHuaWarpper类对不需要增强的方法调用LiuDeHua类中的同名方法

##### 实现代码如下

LiuDeHua类: 被装饰类

LiuDeHuaWarpper类: 我们称之为装饰类

```java
/*
	装饰模式遵循原则:
		装饰类和被装饰类必须实现相同的接口
		在装饰类中必须传入被装饰类的引用
		在装饰类中对需要扩展的方法进行扩展
		在装饰类中对不需要扩展的方法调用被装饰类中的同名方法
*/
public class LiuDeHuaWarpper implements Star {
    // 存放被装饰类的引用
    private LiuDeHua liuDeHua;
    // 通过构造器传入被装饰类对象
    public LiuDeHuaWarpper(LiuDeHua liuDeHua){
        this.liuDeHua = liuDeHua;
    }
    @Override
    public void sing() {
        // 对需要扩展的方法进行扩展增强
        System.out.println("刘德华在鸟巢的舞台上演唱忘情水.");
    }
    @Override
    public void dance() {
        // 不需要增强的方法调用被装饰类中的同名方法
        liuDeHua.dance();
    }
}
```

##### 测试结果

```java
public static void main(String[] args) {
    // 创建被装饰类对象
    LiuDeHua liuDeHua = new LiuDeHua();
    // 创建装饰类对象,被传入被装饰类
    LiuDeHuaWarpper liuDeHuaWarpper = new LiuDeHuaWarpper(liuDeHua);
    // 调用装饰类的相关方法,完成方法扩展
    liuDeHuaWarpper.sing();
    liuDeHuaWarpper.dance();
}
```

### 小结

装饰模式可以在不改变原类的基础上对类中的方法进行扩展增强,实现原则为:

1. 装饰类和被装饰类必须实现相同的接口
2. 在装饰类中必须传入被装饰类的引用
3. 在装饰类中对需要扩展的方法进行扩展
4. 在装饰类中对不需要扩展的方法调用被装饰类中的同名方法



# 第八章 commons-io工具包

### 目标

- 掌握commons-io工具包的使用

### 路径

- commons-io工具包的概述
- commons-io工具包的使用
- commons-io工具包常用api介绍

### 讲解

#### commons-io工具包的概述

commons-io是apache开源基金组织提供的一组有关IO操作的类库，可以挺提高IO功能开发的效率。commons-io工具包提供了很多有关io操作的类，见下表：

| 包                                  | 功能描述                                     |
| ----------------------------------- | :------------------------------------------- |
| org.apache.commons.io               | 有关Streams、Readers、Writers、Files的工具类 |
| org.apache.commons.io.input         | 输入流相关的实现类，包含Reader和InputStream  |
| org.apache.commons.io.output        | 输出流相关的实现类，包含Writer和OutputStream |
| org.apache.commons.io.serialization | 序列化相关的类                               |

#### commons-io工具包的使用

步骤：

1. 下载commons-io相关jar包；http://commons.apache.org/proper/commons-io/
2. 把commons-io-2.6.jar包复制到指定的Module的lib目录中
3. 将commons-io-2.6.jar加入到classpath中

#### commons-io工具包的使用

- commons-io提供了一个工具类  org.apache.commons.io.IOUtils，封装了大量IO读写操作的代码。其中有两个常用方法：

> 1. public static int copy(InputStream in, OutputStream out)； 把input输入流中的内容拷贝到output输出流中，返回拷贝的字节个数(适合文件大小为2GB以下)
> 2. public static long copyLarge(InputStream in, OutputStream out)；把input输入流中的内容拷贝到output输出流中，返回拷贝的字节个数(适合文件大小为2GB以上)

文件复制案例演示：

~~~java
 // IOUtils工具类拷贝文件
    private static void method01() throws IOException {
        FileInputStream fis = new FileInputStream("day17\\aaa\\jdk11.exe");
        FileOutputStream fos = new FileOutputStream("day17\\aaa\\jdk11Copy4.exe");
        IOUtils.copy(fis,fos);
        fos.close();
        fis.close();
    }
~~~

- commons-io还提供了一个工具类org.apache.commons.io.FileUtils，封装了一些对文件操作的方法：

> 1. public static void copyFileToDirectory(final File srcFile, final File destFile) //复制文件到另外一个目录下。
> 2. public static void copyDirectoryToDirectory( file1 , file2 );//复制file1目录到file2位置。

案例演示：

~~~java
public static void main(String[] args) throws IOException {
		// FileUtils工具类拷贝文件到指定文件夹
        // File srcFile = new File("day17\\aaa\\a.txt");
        // File destFile = new File("day17\\eee");
        // FileUtils.copyFileToDirectory(srcFile,destFile);

        // FileUtils工具类拷贝文件夹到指定文件夹
        File srcFile = new File("day17\\ddd");
        File destFile = new File("day17\\eee");
        FileUtils.copyDirectoryToDirectory(srcFile,destFile);
    }
~~~

### 小结

略

# 总结

```java
- 能够使用Properties的load方法加载文件中配置信息
    load(InputStream is);
    load(Reader r);

- 能够使用字节缓冲流读取数据到程序
    BufferedInputStream(InputStream is)
    读取数据的方法是使用InputStream的方法
    
- 能够使用字节缓冲流写出数据到文件
     BufferedOutputStream(OutputStream os) 
      写数据的方法是使用OutputStream的方法
    
- 能够明确字符缓冲流的作用和基本用法
    BufferedReader: 读取一行------>基本的读数据使用的是Reader类的方法
    BufferedWriter: 根据系统自定义换行--->基本的写数据使用的是Writer类的方法
        
- 能够使用缓冲流的特殊功能
    读取一行: BufferedReader的readLine()方法
    写换行:  BufferedWriter的newLine()方法
- 能够阐述编码表的意义
     规定了字符与二进制数对应的规则
        
- 能够使用转换流读取指定编码的文本文件
       InputStreamReader(InputStream is, String charset)
        读取数据的方法是使用Reader的方法
- 能够使用转换流写入指定编码的文本文件
        OutputStreamWriter(OutputStream os, String charset)
        写出数据的方法是使用Writer的方法
        
- 能够使用序列化流写出对象到文件
     ObjectOutputStream: writeObject(Object obj)
         
- 能够使用反序列化流读取文件到程序中
     ObjectInputStream: Object readObject();

- 能够理解装饰模式的实现步骤
      1.装饰者类和被装饰者类需要实现同一个接口
      2.在装饰类中需要获取被装饰类的引用
      3.在装饰类中对需要增强的方法进行增强
      4.在装饰类中对不需要增强的方法,就调用被装饰类的同名方法
    
- 能够使用commons-io工具包
    1.下载commons-io工具包
    2.导包工具包到模块下
    3.把工具包添加到classpath路径中
    4.使用工具包中的工具类调用工具方法进行操作
    
```

