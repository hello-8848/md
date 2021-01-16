# day12【File类、递归、IO流、字节流、字符流】

## 今日内容

- File类
- 递归
- IO流概述
- 字节流
- 字符流

## 教学目标  

- [ ] 能够说出File对象的创建方式
- [ ] 能够使用File类常用方法
- [ ] 能够辨别相对路径和绝对路径
- [ ] 能够遍历文件夹
- [ ] 能够解释递归的含义
- [ ] 能够使用递归的方式计算5的阶乘
- [ ] 能够说出使用递归会内存溢出隐患的原因
- [ ] 能够说出IO流的分类和功能
- [ ] 能够使用字节输出流写出数据到文件
- [ ] 能够使用字节输入流读取数据到程序
- [ ] 能够理解读取数据read(byte[])方法的原理
- [ ] 能够使用字节流完成文件的复制
- [ ] 能够使用FileWriter写数据的5个方法
- [ ] 能够说出FileWriter中关闭和刷新方法的区别
- [ ] 能够使用FileWriter写数据实现换行和追加写
- [ ] 能够使用FileReader读数据一次一个字符
- [ ] 能够使用FileReader读数据一次一个字符数组

# 第一章 File类

## 知识点-- File类的概述和构造方法

### 目标

- 理解File类的概述和创建File类对象

### 路径

- File类的概述
- File类的构造方法

### 讲解

#### File类的概述

`java.io.File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

#### File类的构造方法

- `public File(String pathname) ` ：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File实例。  
- `public File(String parent, String child) ` ：从**父路径名字符串和子路径名字符串**创建新的 File实例。
- `public File(File parent, String child)` ：从**父抽象路径名和子路径名字符串**创建新的 File实例。  


- 构造举例，代码如下：

```java
public class Test {
    public static void main(String[] args) {
        /*
             File类的概述:
                概述:java.io.File 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

             File类的构造方法:
                - public File(String pathname) ：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。
                - public File(String parent, String child) ：从父路径名字符串和子路径名字符串创建新的 File实例。
                - public File(File parent, String child) ：从父抽象路径名和子路径名字符串创建新的 File实例。
         */
        // 文件路径存在
        // 创建File对象,表示H:\aaa\bbb\hb.jpg文件路径
        File f1 = new File("H:\\aaa\\bbb\\hb.jpg");
        File f2 = new File("H:\\aaa\\bbb","hb.jpg");

        File parentF = new File("H:\\aaa\\bbb");
        File f3 = new File(parentF,"hb.jpg");

        System.out.println("f1:"+f1);
        System.out.println("f2:"+f2);
        System.out.println("f3:"+f3);


        System.out.println("=============================================");

        // 文件夹路径存在
        // 创建File对象,表示H:\aaa\bbb文件夹路径
        File f4 = new File("H:\\aaa\\bbb");
        File f5 = new File("H:\\aaa","bbb");

        File parent = new File("H:\\aaa");
        File f6 = new File(parent,"bbb");

        System.out.println("f4:"+f4);
        System.out.println("f5:"+f5);
        System.out.println("f6:"+f6);

        System.out.println("=============================================");
        // 文件路径不存在
        File f7 = new File("H:\\aaa\\bbb\\b.txt");

        // 文件夹路径不存在
        File f8 = new File("H:\\aaa\\bbb\\ddd");

        System.out.println("f7:"+f7);
        System.out.println("f8:"+f8);
        // 结论:无论该路径下是否存在文件或者目录，都不影响File对象的创建。

    }
}

```

> 小贴士：
>
> 1. 一个File对象代表硬盘中实际存在的一个文件或者目录。
> 2. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。

### 小结

略

## 知识点-- File类常用方法

### 目标

- 掌握File类常用方法的使用

### 路径

- 获取功能的方法
- 绝对路径和相对路径
- 判断功能的方法
- 创建删除功能的方法

### 讲解

#### 获取功能的方法

- `public String getAbsolutePath() ` ：返回此File的绝对路径名字符串。

- ` public String getPath() ` ：将此File转换为路径名字符串。 

- `public String getName()`  ：返回由此File表示的文件或目录的名称。  

- `public long length()`  ：返回由此File表示的文件的长度。 不能获取目录的长度。

  方法演示，代码如下：

  ```java
  public class Test1_获取功能的方法 {
      public static void main(String[] args) {
          /*
              File类:
                  - public String getAbsolutePath() ：返回此File的绝对路径名字符串。获取绝对路径
                  - public String getPath() ：将此File转换为路径名字符串。获取构造路径
                  - public String getName()  ：返回由此File表示的文件或目录的名称。
                  - public long length()  ：返回由此File表示的文件的字节大小。 不能获取文件夹的字节大小。
  
              绝对路径和相对路径:
                  - 绝对路径：从盘符开始的路径，这是一个完整的路径。
                  - 相对路径：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。
  
                  绝对路径:G:\\szitheima102\\day12\\aaa\\hb.jpg
                  相对路径:day12\\aaa\\hb.jpg
  
                  生活中的例子: 你在中粮商务公园2栋608教室, 你女朋友在中粮商务公园
                  绝对路径:中国广东省深圳市宝安区留仙二路中粮商务公园2栋608教室
                  相对路径:2栋608教室
  
           */
          // 创建File对象,表示day12\\aaa\\hb.jpg文件路径
          File f1 = new File("day12\\aaa\\hb.jpg");
          System.out.println("f1的绝对路径:"+f1.getAbsolutePath());// G:\szitheima102\day12\aaa\hb.jpg
          System.out.println("f1的构造路径:"+f1.getPath());// day12\aaa\hb.jpg
          System.out.println("f1表示的文件的文件名:"+f1.getName());// hb.jpg
          System.out.println("f1表示的文件的字节大小:"+f1.length());// 24,666 字节
  
          System.out.println("==========================================");
          // 创建File对象,表示day12\\aaa文件夹路径
          File f2 = new File("day12\\aaa");
          System.out.println("f2的绝对路径:"+f2.getAbsolutePath());// G:\szitheima102\day12\aaa
          System.out.println("f2的构造路径:"+f2.getPath());// day12\aaa
          System.out.println("f2的文件夹名:"+f2.getName());// aaa
          System.out.println("f2文件夹的字节大小:"+f2.length());// 0   length()方法不能获取文件夹的字节大小
  
      }
  }
  
  ```

> API中说明：length()，表示文件的长度。但是File对象表示目录，则返回值未指定。

#### 绝对路径和相对路径

- **绝对路径**：从盘符开始的路径，这是一个完整的路径。
- **相对路径**：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。

```java
  public static void main(String[] args) {
        /*
            - 绝对路径：从盘符开始的路径，这是一个完整的路径。
            - 相对路径：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。

           举例:
            生活中的例子:假设你在深圳市宝安区中粮商务公园3栋1701教室  你女朋友在深圳市宝安区中粮商务公园楼下
            绝对路径: 中国广东省深圳市宝安区中粮商务公园3栋1701教室
            相对路径: 3栋1701教室

            程序中的例子:
                绝对路径: G:\szitheima0414\day15\aaa\a.txt
                相对路径: day15\aaa\a.txt

           注意: 开发中使用相对路径
         */
        // 绝对路径
        File file1 = new File("G:\\szitheima0414\\day15\\aaa\\a.txt");
        System.out.println("file1的绝对路径:"+file1.getAbsoluteFile());// G:\szitheima0414\day15\aaa\a.txt
        System.out.println("file1的构造路径:"+file1.getPath());// G:\szitheima0414\day15\aaa\a.txt

        // 相对路径
        File file2 = new File("day15\\aaa\\a.txt");
        System.out.println("file2的绝对路径:"+file2.getAbsoluteFile());// G:\szitheima0414\day15\aaa\a.txt
        System.out.println("file2的构造路径:"+file2.getPath());// day15\aaa\a.txt
    }
```

#### 判断功能的方法

- `public boolean exists()` ：此File表示的文件或目录是否实际存在。
- `public boolean isDirectory()` ：此File表示的是否为目录。
- `public boolean isFile()` ：此File表示的是否为文件。

方法演示，代码如下：

```java
public class Test2_判断功能的方法 {
    public static void main(String[] args) {
        /*
            File类:
            - public boolean exists() ：此File表示的文件或目录是否实际存在。
            - public boolean isDirectory() ：此File表示的是否为目录。
            - public boolean isFile() ：此File表示的是否为文件。

         */
        // 文件存在
        // 创建File对象,表示day12\\aaa\\hb.jpg文件路径
        File f1 = new File("day12\\aaa\\hb.jpg");
        System.out.println("f1表示的文件是否存在:"+f1.exists());// true
        System.out.println("f1表示的路径是否是一个文件夹:"+f1.isDirectory());// false
        System.out.println("f1表示的路径是否是一个文件"+f1.isFile());// true

        System.out.println("======================================================");

        // 文件夹存在
        // 创建File对象,表示day12\\aaa文件夹路径
        File f2 = new File("day12\\aaa");
        System.out.println("f2表示的文件夹是否存在:"+f2.exists());// true
        System.out.println("f2表示的路径是否是一个文件夹:"+f2.isDirectory());//true
        System.out.println("f2表示的路径是否是一个文件:"+f2.isFile());//false


        System.out.println("======================================================");

        // 文件不存在:
        // 创建File对象,表示day12\\aaa\\a.txt文件路径
        File f3 = new File("day12\\aaa\\a.txt");
        System.out.println("f3表示的文件是否存在:"+f3.exists());// false
        System.out.println("f3表示的路径是否是一个文件夹:"+f3.isDirectory());// false
        System.out.println("f3表示的路径是否是一个文件"+f3.isFile());// false

        System.out.println("======================================================");

        // 文件夹不存在
        // 创建File对象,表示day12\\aaa\\bbb文件路径
        File f4 = new File("day12\\aaa\\bbb");
        System.out.println("f4表示的文件是否存在:"+f4.exists());// false
        System.out.println("f4表示的路径是否是一个文件夹:"+f4.isDirectory());// false
        System.out.println("f4表示的路径是否是一个文件"+f4.isFile());// false

    }
}

```

#### 创建删除功能的方法

- `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。 
- `public boolean delete()` ：删除由此File表示的文件或目录。  
- `public boolean mkdir()` ：创建由此File表示的目录。
- `public boolean mkdirs()` ：创建由此File表示的目录，包括任何必需但不存在的父目录。

方法演示，代码如下：

```java
public class Test3_创建和删除功能 {
    public static void main(String[] args) throws IOException {
        /*
            File类:
            - public boolean createNewFile() ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
            - public boolean delete() ：删除由此File表示的文件或目录。 只能删除文件或者空文件夹,不走回收站
            - public boolean mkdir() ：创建由此File表示的目录。
            - public boolean mkdirs() ：创建由此File表示的目录，包括任何必需但不存在的父目录。
         */
        // 创建一个空的文件
        // 创建File对象,表示day12\\aaa\\a.txt文件(不存在)
        /*File f1 = new File("day12\\aaa\\a.txt");
        boolean res1 = f1.createNewFile();
        System.out.println("res1:"+res1);// true*/

        // 删除文件
        // 创建File对象,表示day12\\aaa\\a.txt文件
        /*File f2 = new File("day12\\aaa\\a.txt");
        boolean res2 = f2.delete();
        System.out.println("res2:"+res2);// true*/

        // 删除文件夹
        // 创建File对象,表示day12\\aaa\\ccc文件夹  空文件夹
        /*File f3 = new File("day12\\aaa\\ccc");
        boolean res3 = f3.delete();
        System.out.println("res3:"+res3);// true*/

        // 创建File对象,表示day12\\aaa\\bbb文件夹  非空文件夹
        /*File f4 = new File("day12\\aaa\\bbb");
        boolean res4 = f4.delete();
        System.out.println("res4:"+res4);// false*/
        // 结论:delete方法只能删除文件或者空文件夹

        //System.out.println("===================================================");

        // 创建文件夹
        // 创建File对象,表示day12\\aaa\\ddd文件夹
        /*File f5 = new File("day12\\aaa\\ddd");
        boolean res5 = f5.mkdir();
        System.out.println("res5:"+res5);// true*/

        // 创建File对象,表示day12\\aaa\\aaa\\bbb\\ccc\\ddd\\eee文件夹
        File f6 = new File("day12\\aaa\\aaa\\bbb\\ccc\\ddd\\eee");
        boolean res6 = f6.mkdirs();
        System.out.println("res6:"+res6);// true

    }
}

```

> API中说明：delete方法，如果此File表示目录，则目录必须为空才能删除。

### 小结

略

## 知识点-- File类遍历目录方法

### 目的

- 掌握File类遍历目录方法的使用

### 路径

- File类遍历目录方法

### 讲解

- `public String[] list()` ：返回一个String数组，表示该File目录中的所有子文件或目录。


- `public File[] listFiles()` ：返回一个File数组，表示该File目录中的所有的子文件或目录。  

```java
public class Test4_遍历文件夹的方法 {
    public static void main(String[] args) {
        /*
            File类:
                - public String[] list() ：返回一个String数组，表示该File目录中的所有子文件或子目录。
                - public File[] listFiles() ：返回一个File数组，表示该File目录中的所有的子文件或子目录。
         */
        // 创建File对象,表示day12\aaa文件夹
        File f = new File("day12\\aaa");

        // 获取day12\aaa文件夹中的所有子文件和子文件夹的名称
        String[] arr1 = f.list();
        for (String s : arr1) {
            System.out.println(s);// aaa  bbb  ddd  hb.jpg
        }

        System.out.println("=====================================");

        // 获取day12\aaa文件夹中的所有子文件和子文件夹的路径
        File[] arr2 = f.listFiles();
        for (File file : arr2) {
            System.out.println(file);// day12\aaa\aaa   day12\aaa\bbb  day12\aaa\ddd  day12\aaa\hb.jpg
        }

        System.out.println("=====================================");
        // 注意:
        // 1.如果File对象表示的文件夹不存在:list()或者listFile()方法返回的都是null
        File f1 = new File("day12\\aaa\\eee");
        String[] arr3 = f1.list();
        File[] arr4 = f1.listFiles();
        System.out.println(arr3);// null
        System.out.println(arr4);// null

        // 报空指针异常,为了代码的健壮性,所以一般要加上非空判断
        if (arr3 != null) {
            for (String s : arr3) {

            }
        }
        System.out.println("=====================================");

        // 2.如果File对象表示的文件夹没有访问权限:list()或者listFile()方法返回的都是null
        File f2 = new File("H:\\System Volume Information");
        String[] arr5 = f2.list();
        File[] arr6 = f2.listFiles();
        System.out.println(arr5);// null
        System.out.println(arr6);// null

        System.out.println("=====================================");


        // 3.如果File对象表示的文件夹是空文件夹:list()或者listFile()方法返回的是长度为0的数组
        File f3 = new File("day12\\aaa\\ddd");
        String[] arr7 = f3.list();// ==> String[] arr7 = new String[0];
        File[] arr8 = f3.listFiles();// ==> File[] arr8 = new File[0];
        System.out.println(arr7.length);// 0
        System.out.println(arr8.length);// 0

        for (String s : arr7) {

        }

    }
}

```

> 小贴士：
>
> 调用listFiles方法的File对象，表示的必须是实际存在的目录，否则返回null，无法进行遍历。

### 小结

略

# 第二章 递归

## 知识点--递归的概述

### 目标

- 理解递归的概述

### 路径

- 递归的概述
- 案例演示

### 讲解

#### 递归的概述

- 生活中的递归: 放羊--赚钱--盖房子--娶媳妇--生娃--放羊--赚钱--盖房子--娶媳妇--生娃--放羊...
- 程序中的递归: 指在当前方法内调用自己的这种现象。
-  递归的注意事项:
  - 递归要有出口(结束方法),否则会报栈内存溢出错误StackOverflowError
  - 递归的出口不能太晚了

#### 案例演示

```java
public class Test {

    // 旗帜变量
    static int count = 0;

    public static void main(String[] args) {
        /*
            递归的概述:
                生活中的递归: 放羊-->赚钱-->盖房子-->娶媳妇-->生娃--->放羊-->赚钱-->盖房子-->娶媳妇-->生娃.....
                程序中的递归: 指在当前方法内调用自己的这种现象。
                递归的注意事项:
                    1.如果无限递归,就会出现栈内存溢出错误StackOverflowError
                    2.递归一定要有出口,并且出口不能太晚,否则还是会出现栈内存溢出错误StackOverflowError
                    解决办法: 就是合理递归
         */
        method();// 调用method()方法
    }

    public static void method(){
        if (count == 5){// 出口
            return;// 当count加到5的时候,就结束递归
        }
        count++;
        System.out.println("执行了吗...");
        method();
    }
}
```

### 小结

略

## 实操-- 递归累和

### 需求

- 计算1 ~ n的累加和

### 分析

- num的累加和 = num + (num-1)的累和，所以可以把累加和的操作定义成一个方法，递归调用。

### 实现

#### 代码实现

```java
public class Test1 {
    public static void main(String[] args) {
        /*
            练习一:使用递归计算1 ~ n的和
                分析:
                        1 的累加和 = 1                      1的累加和=1
                        2 的累加和 = 1 + 2                  2的累加和=2+1的累加和
                        3 的累加和 = 1 + 2 + 3              3的累加和=3+2的累加和
                        4 的累加和 = 1 + 2 + 3 + 4          4的累加和=4+3的累加和
                        .....
                        n 的累加和                          n的累加和=n+(n-1)的累加和
         */
        // 调用getSum方法计算5的累加和
        int sum = getSum(5);
        System.out.println("5的累加和:"+sum);// 15
    }

    /**
     * 计算一个数的累加和
     * @param n
     * @return
     */
    public static int getSum(int n){
        // 出口
        if(n == 1){
            return 1;
        }
        return n + getSum(n-1);// 规律
    }
}
```

#### 代码执行图解

![](img/day08_01_递归累和.jpg)

### 小结

略

## 实操-- 递归求阶乘

### 需求

- 计算n的阶乘

### 分析

- **阶乘**：所有小于及等于该数的正整数的积。

```java
n的阶乘：n! = n * (n-1) *...* 3 * 2 * 1 
```

n的阶乘 = n * (n1)的阶乘，所以可以把阶乘的操作定义成一个方法，递归调用。

```
推理得出：n! = n * (n-1)!
```

### 实现

**代码实现**：

```java
public class Test2 {
    public static void main(String[] args) {
        /*
            递归求阶乘:
                规律:
                    1! = 1                                      1的阶乘 : 1
                    2! = 2 * 1                                  2的阶乘 : 2 * 1的阶乘
                    3! = 3 * 2 * 1                              3的阶乘 : 3 * 2的阶乘
                    4! = 4 * 3 * 2 * 1                          4的阶乘 : 4 * 3的阶乘
                    5! = 5 * 4 * 3 * 2 * 1                      5的阶乘 : 5 * 4的阶乘
                    ....
                    num! = num * (num-1) * (num-2) *...* 1      num的阶乘 : num * num-1的阶乘
         */
        int res = jieCheng(5);
        System.out.println("5的阶乘:"+res);// 5的阶乘:120
    }

    /**
     * 计算一个数的阶乘
     * @param num
     * @return
     */
    public static int jieCheng(int num){
        // 出口
        if (num == 1){
            return 1;
        }
        return num *  jieCheng(num-1); // 计算阶乘的规律
    }
}

```

### 小结

## 实操-- 文件搜索

### 需求

-  输出day15\\src目录中的所有.java文件的绝对路径。

### 分析

1. 目录搜索，无法判断多少级目录，所以使用递归，遍历所有目录。
2. 遍历目录时，获取的子文件，通过文件名称，判断是否符合条件。

###  实现

```java
public class Test3_递归搜索java文件 {
    public static void main(String[] args) {
        /*
            需求: 输出day12\src目录中的所有.java文件的绝对路径。
         */
        File file = new File("day12\\src");
        findJavaFile(file);
    }

    /**
     * 搜索.java文件的绝对路径
     *
     * @param file 文件夹路径
     */
    public static void findJavaFile(File file) {
        // 1.获取该文件夹下的所有子文件和子文件夹
        File[] arr = file.listFiles();
        // 2.循环遍历获取到的所有子文件和子文件夹
        if (arr != null) {
            for (File file1 : arr) {
                // 3.在循环中,判断遍历出来的元素:
                if (file1.isFile() && file1.getName().endsWith(".java")) {
                    // 3.1 如果是文件,并且文件名是以.java结尾,那么就直接打印其绝对路径
                    System.out.println(file1.getAbsolutePath());
                }
                // 3.2 如果是文件夹,就递归
                if (file1.isDirectory()) {
                    findJavaFile(file1);
                }
            }
        }
    }
}
```

### 小结

略

# 第三章 IO概述

### 目标

- 理解IO的概述以及IO的分类

### 路径

- IO的概述
- IO的分类
- IO的顶层父类
- 注意事项

### 讲解

#### IO的概述

- I : Input  输入   从其他存储设备读数据到内存中就是输入
- O : Output 输出   从内存中写数据到其他存储设备

![](img/1_io.jpg)

#### IO的分类

根据数据的流向分为：**输入流**和**输出流**。

- **输入流** ：把数据从`其他设备`上读取到`内存`中的流。 
  -  字节输入流:以字节为基本单位,读数据
  -  字符输入流:以字符为基本单位,读数据
- **输出流** ：把数据从`内存` 中写出到`其他设备`上的流。
  -  字节输出流:以字节为基本单位,写出数据
  - 字符输出流:以字符为基本单位,写出数据

根据数据的类型分为：**字节流**和**字符流**。

- **字节流** ：以字节为单位，读写数据的流。
  - 字节输入流:以字节为基本单位,读数据
  -  字节输出流:以字节为基本单位,写出数据
- **字符流** ：以字符为单位，读写数据的流。
  - 字符输入流:以字符为基本单位,读数据
  - 字符输出流:以字符为基本单位,写出数据

#### IO的顶层父类

- 字节输入流:顶层父类 InputStream      抽象类
-  字节输出流:顶层父类 OutputStream     抽象类
- 字符输入流:顶层父类 Reader           抽象类
- 字符输出流:顶层父类 Writer           抽象类

#### 注意事项

- utf8编码一个中文占3个字节,gbk编码一个中文占2个字节
- idea默认编码是utf8

### 小结

略

# 第四章 字节流

## 知识点-- 字节输出流【OutputStream】

### 目标

- 理解OutputStream类的概述以及常用方法

### 路径

- OutputStream类的概述
- OutputStream类的常用方法

### 讲解

#### OutputStream类的概述

`java.io.OutputStream `抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

#### OutputStream类的常用方法

- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。    
- `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流。  
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。  
- `public abstract void write(int b)` ：将指定的字节输出流。

> 小贴士：
>
> close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### 小结

略

## 知识点-- FileOutputStream类

### 目标

- 掌握FileOutputStream类的使用

### 路径

- FileOutputStream类的概述
- FileOutputStream类的构造方法
- FileOutputStream类的写出数据
- 数据追加续写
- 写出换行

### 讲解

#### FileOutputStream类的概述

`java.io.FileOutputStream `类是OutputStream类的子类,用来表示是文件输出流，用于将数据写出到文件。

#### FileOutputStream类的构造方法

- `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File对象表示的文件。 
- `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件。  

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有这个文件，会创建该文件。如果有这个文件，会清空这个文件的数据。

- 构造举例，代码如下：

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            FileOutputStream类:
                概述:java.io.FileOutputStream类是OutputStream类的子类,用来表示是文件输出流，用于将字节数据写出到文件。
                构造方法:
                    - public FileOutputStream(String name)： 创建文件输出流以指定的名称写入文件。
                    - public FileOutputStream(File file)：创建文件输出流以写入由指定的 File对象表示的文件。
                    注意:
                        当你创建一个流对象时，必须传入一个文件路径。
                        该路径下，如果没有这个文件，会创建该文件。
                        如果有这个文件，会清空这个文件的数据。
         */
        // 文件存在--->清空源文件中的数据
        // 创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos1 = new FileOutputStream("day12\\bbb\\a.txt");// 文件中有数据,就会清空
        FileOutputStream fos2 = new FileOutputStream(new File("day12\\bbb\\b.txt"));// 文件中没有数据

        // 文件不存在--->就会创建一个新的空文件
        FileOutputStream fos3 = new FileOutputStream("day12\\bbb\\c.txt");

    }
}

```

#### FileOutputStream类的写出数据

1. **写出字节**：`write(int b)` 方法，每次可以写出一个字节数据，代码使用演示：

```java
public class FOSWrite {
    public static void main(String[] args) throws IOException {
       // 创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day15\\aaa\\a.txt");
        // 写出数据
        fos.write('a');
        fos.write(98);// 写整数到文本文件中其实写的是该整数对应的字符
        // 关闭流,释放资源
        fos.close();
    }
}
写出结果：
ab
```

> 小贴士：
>
> 1. 虽然参数为int类型四个字节，但是只会保留一个字节的信息写出。
> 2. 流操作完毕后，必须释放系统资源，调用close方法，千万记得。

1. **写出字节数组**：`write(byte[] b)`，每次可以写出数组中的数据，代码使用演示：

```java
public class Test2_写出多个字节数据 {
    public static void main(String[] args) throws Exception{
        /*
            - public void write(byte[] b)：将 b.length字节从指定的字节数组写入此输出流。写出一个字节数组
         */
        // 1.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day12\\bbb\\b.txt");

        // 2.创建字节数组,存储字节数据
        byte[] bys = {97,98,99,100,101,102};

        // 3.写出字节数组中的字节数据
        fos.write(bys);

        // 4.关闭流,释放资源
        fos.close();
    }
}
```

1. **写出指定长度字节数组**：`write(byte[] b, int off, int len)` ,每次写出从off索引开始，len个字节，代码使用演示：

```java
public class Test3_写出指定范围的字节数组 {
    public static void main(String[] args) throws Exception{
        /*
            - public void write(byte[] b, int off, int len) ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。
         */
        // 1.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day12\\bbb\\c.txt");

        // 2.创建字节数组,存储字节数据
        byte[] bys = {97,98,99,100,101,102};

        // 3.写出指定范围的字节数组数据
        fos.write(bys,1,4);

        // 4.关闭流,释放资源
        fos.close();
    }
}

写出结果：
bcde
```

#### 数据追加续写

经过以上的演示，每次程序运行，创建输出流对象，都会清空目标文件中的数据。如何保留目标文件中数据，还能继续添加新数据呢？

- `public FileOutputStream(File file, boolean append)`： 创建文件输出流以写入由指定的 File对象表示的文件。  
- `public FileOutputStream(String name, boolean append)`： 创建文件输出流以指定的名称写入文件。  

这两个构造方法，参数中都需要传入一个boolean类型的值，`true` 表示追加数据，`false` 表示清空原有数据。这样创建的输出流对象，就可以指定是否追加续写了，代码使用演示：

```java
public class FOSWrite {
    public static void main(String[] args) throws IOException {
       // 创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day15\\aaa\\a.txt",true);// 可以追加续写
        //FileOutputStream fos = new FileOutputStream("day15\\aaa\\a.txt",false);// 覆盖
        // 写出数据
        fos.write(99);// 写字符c到a.txt文件中
        // 关闭流,释放资源
        fos.close();
    }
}
文件操作前：ab
文件操作后：abc
```

#### 写出换行

Windows系统里，换行符号是`\r\n` 。把

以指定是否追加续写了，代码使用演示：

```java
public class Test5_写出换行和字符串 {
    public static void main(String[] args) throws Exception{
        /*
             需求: 写一首诗到day12\bbb\e.txt文件中
                好诗
             看这风景美如画
             吟诗一首赠天下
             奈何自己没文化
             只能卧槽浪好大

         */
        // 1.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day12\\bbb\\e.txt");

        // 2.写出数据
        fos.write("    好诗   ".getBytes());
        fos.write("\r\n".getBytes());
        fos.write("看这风景美如画".getBytes());
        fos.write("\r\n".getBytes());
        fos.write("吟诗一首赠天下".getBytes());
        fos.write("\r\n".getBytes());
        fos.write("奈何自己没文化".getBytes());
        fos.write("\r\n".getBytes());
        fos.write("只能卧槽浪好大".getBytes());
        fos.write("\r\n".getBytes());

        // 3.关闭流,释放资源
        fos.close();
    }
}
```

> - 回车符`\r`和换行符`\n` ：
>   - 回车符：回到一行的开头（return）。
>   - 换行符：下一行（newline）。
> - 系统中的换行：
>   - Windows系统里，每行结尾是 `回车+换行` ，即`\r\n`；
>
>   - Unix系统里，每行结尾只有 `换行` ，即`\n`；
>
>   - Mac系统里，每行结尾是 `回车` ，即`\r`。从 Mac OS X开始与Linux统一。
>

### 小结

略

## 知识点-- 字节输入流【InputStream】

### 目标

- 理解InputStream类的概述以及常用方法

### 路径

- InputStream类的概述
- InputStream类的常用方法

### 讲解

#### InputStream类的概述

`java.io.InputStream `抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

#### InputStream类的常用方法

- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。    
- `public abstract int read()`： 从输入流读取数据的下一个字节。 
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。

> 小贴士：
>
> close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### 小结

略

## 知识点-- FileInputStream类

### 目标

- 掌握FileInputStream类的使用

### 路径

- FileInputStream类的概述
- FileInputStream类的构造方法
- FileInputStream类的读取数据

### 讲解

#### FileInputStream类的概述

`java.io.FileInputStream `类是InputStream类的子类 , 用来表示文件输入流，从文件中读取字节。

#### FileInputStream类的构造方法

- `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的文件对象 file命名。 
- `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。  

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有该文件,会抛出`FileNotFoundException` 

- 构造举例，代码如下：

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            FileInputStream类:
                概述:java.io.FileInputStream类是InputStream类的子类 , 用来表示文件输入流，从文件中读取字节。
                构造方法:
                        - FileInputStream(File file)： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的文件对象 file命名。
                        - FileInputStream(String name)： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。
         */
        // 文件是存在的
        // 创建字节输入流对象,关联数据源文件路径
        FileInputStream fis1 = new FileInputStream("day12\\ccc\\a.txt");
        FileInputStream fis2 = new FileInputStream(new File("day12\\ccc\\a.txt"));

        // 文件不存在:
        // 报文件找不到异常FileNotFoundException
        //FileInputStream fis3 = new FileInputStream("day12\\ccc\\b.txt");
    }
}

```

#### FileInputStream类的读取数据

1. **读取字节**：`read`方法，每次可以读取一个字节的数据，提升为int类型，读取到文件末尾，返回`-1`，代码使用演示：

```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象
       	FileInputStream fis = new FileInputStream("read.txt");
      	// 读取数据，返回一个字节
        int read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
      	// 读取到末尾,返回-1
       	read = fis.read();
        System.out.println( read);
		// 关闭资源
        fis.close();
    }
}
输出结果：
a
b
c
d
e
-1
```

循环改进读取方式，代码使用演示：

```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象
       	FileInputStream fis = new FileInputStream("read.txt");
      	// 定义变量，保存数据
        int b ；
        // 循环读取
        while ((b = fis.read())!=-1) {
            System.out.println((char)b);
        }
		// 关闭资源
        fis.close();
    }
}
输出结果：
a
b
c
d
e
```

> 小贴士：
>
> 1. 虽然读取了一个字节，但是会自动提升为int类型。
> 2. 流操作完毕后，必须释放系统资源，调用close方法，千万记得。

1. **使用字节数组读取**：`read(byte[] b)`，每次读取b的长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回`-1` ，代码使用演示：

```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象.
       	FileInputStream fis = new FileInputStream("read.txt"); // 文件中为abcde
      	// 定义变量，作为有效个数
        int len ；
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组变成字符串打印
            System.out.println(new String(b));
        }
		// 关闭资源
        fis.close();
    }
}

输出结果：
ab
cd
ed
```

错误数据`d`，是由于最后一次读取时，只读取一个字节`e`，数组中，上次读取的数据没有被完全替换，所以要通过`len` ，获取有效的字节，代码使用演示：

```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象.
       	FileInputStream fis = new FileInputStream("read.txt"); // 文件中为abcde
      	// 定义变量，作为有效个数
        int len ；
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组的有效字节部分，变成字符串打印
            System.out.println(new String(b，0，len));//  len 每次读取的有效字节个数
        }
		// 关闭资源
        fis.close();
    }
}

输出结果：
ab
cd
e
```

> 小贴士：
>
> 使用数组读取，每次读取多个字节，减少了系统间的IO操作次数，从而提高了读写的效率，建议开发中使用。

### 小结

略

## 实操-- 字节流练习：图片复制

### 需求

- 使用字节流拷贝一张图片

### 分析

![1588696319583](img\1588696319583.png)

```java
一次读写一个字节拷贝文件思路:
                    1.创建字节输入流对象,关联数据源文件路径
                    2.创建字节输出流对象,关联目的地文件路径
                    3.定义一个变量,用来存储读取到的字节数据
                    4.循环读取
                    5.在循环中,写出数据
                    6.关闭流,释放资源
一次读写一个字节数组拷贝文件
                    1.创建字节输入流对象,关联数据源文件路径
                    2.创建字节输出流对象,关联目的地文件路径
                    3.定义一个字节数组,用来存储读取到的字节数据
                    3.定义一个变量,用来存储读取到的字节个数
                    4.循环读取
                    5.在循环中,写出数据
                    6.关闭流,释放资源
```

### 实现

复制图片文件，代码使用演示：

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            需求: 使用字节流拷贝一张图片
         */
        // 方式一: 一次读写一个字节拷贝文件
        /*// 1.创建字节输入流对象,关联数据源文件路径
        FileInputStream fis = new FileInputStream("day12\\aaa\\hb.jpg");

        // 2.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day12\\ccc\\hbCopy1.jpg");

        // 3.定义一个变量用来存储读取到的字节数据
        int len;

        // 4.循环读取字节数据
        while ((len = fis.read()) != -1) {
            // 5.写出数据到目的地文件中
            fos.write(len);
        }

        // 6.关闭流,释放资源
        fos.close();
        fis.close();*/



        // 方式二: 一次读写一个字节数组拷贝文件
        // 1.创建字节输入流对象,关联数据源文件路径
        FileInputStream fis = new FileInputStream("day12\\aaa\\hb.jpg");

        // 2.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day12\\ccc\\hbCopy2.jpg");

        // 3.1 定义一个字节数组,用来存储读取到的字节数据
        byte[] bys = new byte[8192];

        // 3.定义一个int类型的变量,用来存储读取到的字节个数
        int len;

        // 4.循环读取字节数据
        while ((len = fis.read(bys)) != -1) {
            // 5.写出数据到目的地文件中(write(byte[] b, int off, int len))
            fos.write(bys,0,len);
        }

        // 6.关闭流,释放资源
        fos.close();
        fis.close();
    }
}

```

> 小贴士：
>
> 流的关闭原则：先开后关，后开先关。

### 小结

略

# 第五章 字符流

当使用字节流读取文本文件时，可能会有一个小问题。就是遇到中文字符时，可能不会显示完整的字符，那是因为一个中文字符可能占用多个字节存储。所以Java提供一些字符流类，以字符为单位读写数据，专门用于处理文本文件。

## 知识点-- 字符输入流【Reader】

### 目标

- 字符输入流Reader类的概述和常用方法

### 目标

- 字符输入流Reader类的概述
- 字符输入流Reader类的常用方法

### 讲解

#### 字符输入流Reader类的概述

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法。

#### 字符输入流Reader类的常用方法

- `public void close()` ：关闭此流并释放与此流相关联的任何系统资源。    
- `public int read()`： 从输入流读取一个字符。 
- `public int read(char[] cbuf)`： 从输入流中读取一些字符，并将它们存储到字符数组 cbuf中 。

### 小结

略

## 知识点-- FileReader类

### 目标

- 掌握FileReader类的使用

### 路径

- FileReader类的概述
- FileReader类的构造方法
- FileReader类读取数据

### 讲解

#### FileReader类的概述

`java.io.FileReader `类是读取字符文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

#### FileReader类的构造方法

- `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的File对象。   
- `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称。  

当你创建一个流对象时，必须传入一个文件路径。类似于FileInputStream 。

- 构造举例，代码如下：

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            概述: java.io.FileReader类是字符输入流,可以用来读取字符数据
            构造方法:
                - FileReader(String fileName)： 创建一个新的 FileReader ，给定要读取的文件的名称。
                - FileReader(File file)： 创建一个新的 FileReader ，给定要读取的File对象。
         */
        // 文件存在
        FileReader fr1 = new FileReader("day12\\ddd\\a.txt");
        FileReader fr2 = new FileReader(new File("day12\\ddd\\a.txt"));

        // 文件不存在
        // 报文件找不到异常FileNotFoundException
        FileReader fr3 = new FileReader("day12\\ddd\\b.txt");
    }
}
```

#### FileReader类读取数据

1. **读取字符**：`read`方法，每次可以读取一个字符的数据，提升为int类型，读取到文件末尾，返回`-1`，循环读取，代码使用演示：

```java
public class FRRead {
    public static void main(String[] args) throws IOException {
      	// 使用文件名称创建流对象
       	FileReader fr = new FileReader("read.txt");
      	// 定义变量，保存数据
        int b ；
        // 循环读取
        while ((b = fr.read())!=-1) {
            System.out.println((char)b);
        }
		// 关闭资源
        fr.close();
    }
}
输出结果：
黑
马
程
序
员
```

> 小贴士：虽然读取了一个字符，但是会自动提升为int类型。

1. **使用字符数组读取**：`read(char[] cbuf)`，每次读取多个字符到数组中，返回读取到的有效字符个数，读取到末尾时，返回`-1` ，代码使用演示：

```java
public class FRRead {
    public static void main(String[] args) throws IOException {
      	// 使用文件名称创建流对象
       	FileReader fr = new FileReader("read.txt");
      	// 定义变量，保存有效字符个数
        int len ；
        // 定义字符数组，作为装字符数据的容器
         char[] cbuf = new char[2];
        // 循环读取
        while ((len = fr.read(cbuf))!=-1) {
            System.out.println(new String(cbuf));
        }
		// 关闭资源
        fr.close();
    }
}
输出结果：
黑马
程序
员序
```

获取有效的字符改进，代码使用演示：

```java
public class FISRead {
    public static void main(String[] args) throws IOException {
      	// 使用文件名称创建流对象
       	FileReader fr = new FileReader("read.txt");
      	// 定义变量，保存有效字符个数
        int len ；
        // 定义字符数组，作为装字符数据的容器
        char[] cbuf = new char[2];
        // 循环读取
        while ((len = fr.read(cbuf))!=-1) {
            System.out.println(new String(cbuf,0,len));
        }
    	// 关闭资源
        fr.close();
    }
}

输出结果：
黑马
程序
员
```

### 小结

略

## 知识点--字符输出流【Writer】

### 目标

- 字符输出流Writer类的概述和常用方法

### 目标

- 字符输出流Writer类的概述
- 字符输出流Writer类的常用方法

### 讲解

#### 字符输出流Writer类的概述

`java.io.Writer `抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法。

#### 字符输出流Writer类的常用方法

- `public abstract void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。  
- `public abstract void flush() ` ：刷新此输出流并强制任何缓冲的输出字符被写出。  
- `public void write(int c)` ：写出一个字符。
- `public void write(char[] cbuf)`：将 b.length字符从指定的字符数组写出此输出流。  
- `public abstract void write(char[] b, int off, int len)` ：从指定的字符数组写出 len字符，从偏移量 off开始输出到此输出流。  
- `public void write(String str)` ：写出一个字符串。
- `public void write(String str,int off,int len)` ：写出一个字符串的一部分。

### 小结

略

## 知识点--FileWriter类

### 目标

- 掌握FileWriter类的使用

### 路径

- FileWriter类的概述
- FileWriter类的构造方法
- FileWriter类写出数据
- 关闭和刷新

### 讲解

#### FileWriter类的概述

`java.io.FileWriter `类是写出字符到文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

#### FileWriter类的构造方法

- `FileWriter(File file)`： 创建一个新的 FileWriter，给定要读取的File对象。   
- `FileWriter(String fileName)`： 创建一个新的 FileWriter，给定要读取的文件的名称。  
- `FileWriter(File file,boolean append)`： 创建一个新的 FileWriter，给定要读取的File对象。   
- `FileWriter(String fileName,boolean append)`： 创建一个新的 FileWriter，给定要读取的文件的名称。  

当你创建一个流对象时，必须传入一个文件路径，类似于FileOutputStream。

- 构造举例，代码如下：

```java
public class Test {
    public static void main(String[] args) throws Exception {
        /*
             概述:java.io.FileWriter类是Writer类的子类,用来表示是文件输出流，用于将字符数据写出到文件。
             构造方法:
                    - public FileWriter(String name)： 创建文件输出流以指定的名称写入文件。
                    - public FileWriter(File file)：创建文件输出流以写入由指定的 File对象表示的文件。
                    注意:
                        当你创建一个流对象时，必须传入一个文件路径。
                        该路径下，如果没有这个文件，会创建该文件。
                        如果有这个文件，会清空这个文件的数据。


                - public FileWriter(File file, boolean append)： 创建文件输出流以写入由指定的 File对象表示的文件。
                - public FileWriter(String name, boolean append)： 创建文件输出流以指定的名称写入文件。
                append参数: true表示追加续写,false表示清空
                参数1: 文件路径
                   该路径下，如果没有这个文件，会创建该文件。
                   如果有这个文件，第二个参数是true,就追加续写,如果第二个参数是false,就清空文件中的数据
         */
        // 文件存在,文件中如果有数据,会被清空
        //FileWriter fw1 = new FileWriter("day12\\ddd\\c.txt");
        // 文件不存在,就会自动创建一个空文件
        //FileWriter fw2 = new FileWriter(new File("day12\\ddd\\d.txt"));

        System.out.println("========================================================");
        // true表示追加续写,不清空
        //FileWriter fw3 = new FileWriter("day12\\ddd\\c.txt",true);
        // false会清空文件中的数据
        FileWriter fw4 = new FileWriter("day12\\ddd\\c.txt", false);
    }
}

```

#### FileWriter类写出数据

**写出字符**：`write(int b)` 方法，每次可以写出一个字符数据，代码使用演示：

```java
public class FWWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileWriter fw = new FileWriter("fw.txt");     
      	// 写出数据
      	fw.write(97); // 写出第1个字符
      	fw.write('b'); // 写出第2个字符
      	fw.write('C'); // 写出第3个字符
      	fw.write(30000); // 写出第4个字符，中文编码表中30000对应一个汉字。
      
      	/*
        【注意】关闭资源时,与FileOutputStream不同。
      	 如果不关闭,数据只是保存到缓冲区，并未保存到文件。
        */
        // fw.close();
    }
}
输出结果：
abC田
```

> 小贴士：
>
> 1. 虽然参数为int类型四个字节，但是只会保留一个字符的信息写出。
> 2. 未调用close方法，数据只是保存到了缓冲区，并未写出到文件中。

1. **写出字符数组** ：`write(char[] cbuf)` 和 `write(char[] cbuf, int off, int len)` ，每次可以写出字符数组中的数据，用法类似FileOutputStream，代码使用演示：

```java
public class Test2_写出多个字符 {
    public static void main(String[] args) throws Exception{
        /*
             public void write(char[] cbuf)：将 b.length字符从指定的字符数组写出此输出流。
             public abstract void write(char[] b, int off, int len) ：从指定的字符数组写出 len字符，从偏移量 off开始输出到此输出流。
         */
        // 1.创建字符输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day12\\ddd\\f.txt");

        // 2.创建字符数组,存储要写出的字符数据
        char[] chs = {'a','b','中','国'};

        // 3.把chs字符数组中所有的字符写到目的地文件中
        //fw.write(chs);// 'a','b','中','国'

        // 3.把chs字符数组中的最后2个字符写到目的地文件中
        fw.write(chs,chs.length-2,2);// '中','国'

        // 4.关闭流,释放资源
        fw.close();
    }
}
```

1. **写出字符串**：`write(String str)` 和 `write(String str, int off, int len)` ，每次可以写出字符串中的数据，更为方便，代码使用演示：

```java
public class Test3_写出字符串数据 {
    public static void main(String[] args) throws Exception{
        /*
              public void write(String str) ：写出一个字符串。
              public void write(String str,int off,int len) ：写出一个字符串的一部分。
         */
        // 1.创建字符输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day12\\ddd\\g.txt");

        String str = "黑马程序员";

        // 2.写出"黑马程序员"到文件中
        //fw.write(str);// 黑马程序员

        // 2.写出"黑马"到文件中
        fw.write(str,0,2);

        // 3.关闭流,释放资源
        fw.close();
    }
}
```

1. **续写和换行**：操作类似于FileOutputStream。

```java
public class Test4_续写和换行 {
    public static void main(String[] args) throws Exception{
        /*
            续写:
                public FileWriter(File file, boolean append)：
                public FileWriter(String name, boolean append)
            换行:
                public void write(String str)

               好诗
             看这风景美如画
             吟诗一首赠天下
             奈何自己没文化
             只能卧槽浪好大
         */
        // 1.创建字符输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day12\\ddd\\g.txt",true);

        // 2.写出数据
        fw.write("\r\n");
        fw.write("  好诗  ");
        fw.write("\r\n");

        fw.write("看这风景美如画");
        fw.write("\r\n");

        fw.write("吟诗一首赠天下");
        fw.write("\r\n");

        fw.write("奈何自己没文化");
        fw.write("\r\n");

        fw.write("只能卧槽浪好大");

        // 3.关闭流,释放资源
        fw.close();

    }
}

```

> 小贴士：字符流，只能操作文本文件，不能操作图片，视频等非文本文件。
>
> 当我们单纯读或者写文本文件时  使用字符流 其他情况使用字节流

#### 关闭和刷新

因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据的。如果我们既想写出数据，又想继续使用流，就需要`flush` 方法了。

- `flush` ：刷新缓冲区，流对象可以继续使用。
- `close` ：关闭流，释放系统资源。关闭前会刷新缓冲区,流不能再使用了。

代码使用演示：

```java
public class Test {
    public static void main(String[] args) throws Exception{
        /*
            - public abstract void close() ：关闭此输出流并释放与此流相关联的任何系统资源。
             - public abstract void flush() ：刷新此输出流并强制任何缓冲的输出字符被写出
             
             区别:
                - flush ：刷新缓冲区，流对象可以继续使用。
                - close ：关闭流，释放系统资源。关闭前会刷新缓冲区,流不能再使用了。
         */
        // 创建字符输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day12\\ddd\\h.txt");

        // 写出数据
        fw.write("哈哈哈");
        fw.flush();// 刷新

        fw.write("嘿嘿嘿");
        fw.flush();// 刷新

        fw.write("呵呵呵");
        fw.close();// 关闭流,刷新

        fw.write("嘻嘻嘻");// ---> 报IOException异常,因为流已经关闭了
        fw.close();// 关闭流,刷新
    }
}

```

> 小贴士：即便是flush方法写出了数据，操作的最后还是要调用close方法，释放系统资源。

### 小结

略

# 总结

```java
必须练习:
	1.使用字节流一次读写一个字节拷贝文件
    2.使用字节流一次读写一个字节数组拷贝文件
    3.使用字符流一次读写一个字符拷贝文本文件
    4.使用字符流一次读写一个字符数组拷贝文本文件
    5.搜索一个文件夹下的所有.java文件(包含子子目录,子子子目录...)
    6.字节流追加续写\字符流追加续写
 加强:
     1.删除非空文件夹
     2.统计文件夹的字节大小
     3.预习后面课程(day14难)
        
- 能够说出File对象的创建方式
  public File(String pathname) ：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。
  public File(String parent, String child) ：从父路径名字符串和子路径名字符串创建新的 File实例。
  public File(File parent, String child) ：从父抽象路径名和子路径名字符串创建新的 File实例。
         
- 能够使用File类常用方法
 - public String getAbsolutePath() ：返回此File的绝对路径名字符串。获取绝对路径
 - public String getPath() ：将此File转换为路径名字符串。获取构造路径
 - public String getName()  ：返回由此File表示的文件或目录的名称。
 - public long length()  ：返回由此File表示的文件的字节大小。 不能获取文件夹的字节大小。    
 
 - public boolean exists() ：此File表示的文件或目录是否实际存在。
 - public boolean isDirectory() ：此File表示的是否为目录。
 - public boolean isFile() ：此File表示的是否为文件。  
  
 - public boolean createNewFile() ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
 - public boolean delete() ：删除由此File表示的文件或目录。 只能删除文件或者空文件夹,不走回收站
 - public boolean mkdir() ：创建由此File表示的目录。
 - public boolean mkdirs() ：创建由此File表示的目录，包括任何必需但不存在的父目录。
  
 - public String[] list() ：返回一个String数组，表示该File目录中的所有子文件或子目录。
 - public File[] listFiles() ：返回一个File数组，表示该File目录中的所有的子文件或子目录。 
         
- 能够辨别相对路径和绝对路径
     绝对路径:以盘符开始的完整路径
     相对路径:相对项目路径的路径
         
- 能够遍历文件夹
       - public File[] listFiles() ：返回一个File数组，表示该File目录中的所有的子文件或子目录。 
         
- 能够解释递归的含义
      方法内部自己调用自己
      规律\出口
         
- 能够使用递归的方式计算5的阶乘
      public int jieCheng(int n){
         if(n == 1){return 1;}
     	 return n * jieCheng(n-1);
     }
      
- 能够说出使用递归会内存溢出隐患的原因
    递归没有出口,或者递归的出口太晚了
    
- 能够说出IO流的分类和功能
   字节流:以字节为基本单位,进行读写数据
   字符流:以字符为基本单位,进行读写数据
   
- 能够使用字节输出流写出数据到文件
    1.创建字节输出流对象,关联目的地文件路径
    2.写出数据  write(int b) \  write(byte[] bys,int off,int leng);
    3.关闭流,释放资源
       
- 能够使用字节输入流读取数据到程序
    1.创建字节输如流对象,关联数据源文件路径
    2.读取数据  int read()   \   int read(byte[] bys)
    3.关闭流,释放资源
        
- 能够理解读取数据read(byte[])方法的原理
      把读取到的数据存储到字节数组中,返回读取到的字节个数     
        
- 能够使用字节流完成文件的复制
      1.创建输入流对象,关联数据源文件路径
      2.创建输出流对象,关联目的地文件路径
      3.定义变量,存储读取到的数据
      4.循环读取数据
      5.在循环中,写出数据
      6.关闭流,释放资源
        
- 能够使用FileWriter写数据的5个方法
       write(int c)
       write(char[] chs)
       write(char[] chs,int off,int len)
       write(String str)
       write(String str,int off,int len)
        
- 能够说出FileWriter中关闭和刷新方法的区别
     flush: 刷新数据,流还可以使用
     close: 关闭流,刷新数据,流不可以再使用
         
- 能够使用FileWriter写数据实现换行和追加写
     FileWriter(String path,boolean append)
     FileWriter(File path,boolean append)    
         
- 能够使用FileReader读数据一次一个字符
      int read()
- 能够使用FileReader读数据一次一个字符数组
     int read(char[] chs)
```



