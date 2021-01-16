## 	今日内容

- Collections工具类
- Set集合
- Map集合

## 教学目标

- [ ] 能够使用集合工具类
- [ ] 能够使用Comparator比较器进行排序
- [ ] 能够使用可变参数
- [ ] 能够说出Set集合的特点
- [ ] 能够说出哈希表的特点
- [ ] 使用HashSet集合存储自定义元素
- [ ] 能够说出Map集合特点
- [ ] 使用Map集合添加方法保存数据
- [ ] 使用”键找值”的方式遍历Map集合
- [ ] 使用”键值对”的方式遍历Map集合
- [ ] 能够使用HashMap存储自定义键值对的数据
- [ ] 能够完成斗地主洗牌发牌案例

# 第一章  Collections类

## 知识点-- Collections常用功能

### 目标

- 能够使用集合工具类Collections

### 路径

- 代码演示

### 讲解

- `java.utils.Collections`是集合工具类，用来对集合进行操作。

  常用方法如下：


- `public static void shuffle(List<?> list) `:打乱集合顺序。

- `public static <T> void sort(List<T> list)`:将集合中元素按照默认规则排序。

- `public static <T> void sort(List<T> list，Comparator<? super T> comparator)`:将集合中元素按照指定规则排序。

- 打乱集合元素顺序

  ```java
  public class Test1_shuffle方法 {
      public static void main(String[] args) {
          /*
              Collections工具类:
                  public static void shuffle(List<?> list):打乱集合顺序。
           */
          // 创建List集合,限制集合中元素的类型为Integer类型
          List<Integer> list = new ArrayList<>();
  
          // 往集合中添加元素
          list.add(300);
          list.add(100);
          list.add(200);
          list.add(500);
          list.add(400);
  
          System.out.println("打乱顺序之前的集合:"+list);// [300, 100, 200, 500, 400]
          // 打乱集合顺序
          Collections.shuffle(list);
          System.out.println("打乱顺序之后的集合:"+list);
      }
  }
  
  ```

  

- 按照默认规则排序

```java
class Student implements Comparable<Student>{
    public String name;
    public int age;

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

    @Override
    public int compareTo(Student o) {
        // 指定排序规则
        // 前减后  升序
        // 后减前  降序
        // 前: this   后: 参数o
        return this.age - o.age;
    }
}
public class Test2_sort方法 {
    public static void main(String[] args) {
        /*
            Collections工具类:
                public static <T> void sort(List<T> list):将集合中元素按照默认规则排序。
                默认规则: 事先写好的规则
                要求: 要求集合元素所属的类必须实现Comparable接口,重写Comparable接口中的compareTo方法
                      在compareTo方法中指定排序规则
         */
        // 创建List集合,限制集合中元素的类型为Integer类型
        List<Integer> list1 = new ArrayList<>();

        // 往集合中添加元素
        list1.add(300);
        list1.add(100);
        list1.add(200);
        list1.add(500);
        list1.add(400);

        System.out.println("排序之前的集合:"+list1);// [300, 100, 200, 500, 400]

        // 排序
        Collections.sort(list1);

        System.out.println("排序之后的集合:"+list1);// [100, 200, 300, 400, 500]

        System.out.println("===========================================");

        // 创建List集合,限制集合中元素的类型为Student类型
        List<Student> list2 = new ArrayList<>();

        // 往集合中添加元素
        Student stu1 = new Student("张三",18);
        Student stu2 = new Student("李四",19);
        Student stu3 = new Student("王五",17);
        list2.add(stu1);
        list2.add(stu2);
        list2.add(stu3);

        System.out.println("排序之前的集合:"+list2);// 顺序: stu1,stu2,stu3

        // 排序--按照学生对象的年龄升序排序
        Collections.sort(list2);

        System.out.println("排序之后的集合:"+list2);// 顺序: stu3,stu1,stu2

    }
}

```

我们的集合按照默认的自然顺序进行了排列，如果想要指定顺序那该怎么办呢？

### 小结

略

## 知识点-- Comparator比较器

### 目标

- 能够使用Comparator比较器进行排序

### 路径

- 代码演示

### 讲解

- 指定规则排序

```java
public class Test3_sort方法 {
    public static void main(String[] args) {
        /*
            Collections工具类:
                 public static <T> void sort(List<T> list，Comparator<? super T> comparator):将集合中元素按照指定规则排序。
                 指定规则排序: 随时自己指定排序规则
                 传入Comparator接口的实现类对象,该实现类需要重新compare方法,在该方法中指定排序规则
         */
        // 创建List集合,限制集合中元素的类型为Integer类型
        List<Integer> list = new ArrayList<>();

        // 往集合中添加元素
        list.add(300);
        list.add(100);
        list.add(200);
        list.add(500);
        list.add(400);

        System.out.println("排序之前的集合:"+list);// [300, 100, 200, 500, 400]
        // 对集合进行排序,指定排序规则为:降序
       Collections.sort(list, new Comparator<Integer>() {
           @Override
           public int compare(Integer o1, Integer o2) {
               // 指定排序规则
               // 前减后 升序
               // 后减前 降序
               // 前:第一个参数o1 后:第二个参数o2
               return o2 - o1;
           }
       });
        System.out.println("排序之后的集合:"+list);// [500, 400, 300, 200, 100]

        // 对集合进行排序,指定排序规则为:升序
        Collections.sort(list, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        System.out.println("排序之后的集合:"+list);// [100, 200, 300, 400, 500]

        System.out.println("=========================================================================");

        // 创建List集合,限制集合中元素的类型为Student类型
        List<Student> list2 = new ArrayList<>();

        // 往集合中添加元素
        Student stu1 = new Student("张三",18);
        Student stu2 = new Student("李四",19);
        Student stu3 = new Student("王五",17);
        list2.add(stu1);
        list2.add(stu2);
        list2.add(stu3);

        System.out.println("排序之前的集合:"+list2);// 顺序: stu1,stu2,stu3

        // 排序--按照学生对象的年龄降序排序
        Collections.sort(list2, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                return o2.age - o1.age;
            }
        });

        System.out.println("排序之后的集合:"+list2);// 顺序: stu2,stu1,stu3
    }
}

```

### 小结

略

## 知识点-- 可变参数

### 目标

- 能够使用可变参数

### 路径

- 可变参数的使用
- 注意事项
- 应用场景: Collections

### 讲解

#### 可变参数的使用

在**JDK1.5**之后，如果我们定义一个方法需要接受多个参数，并且多个参数类型一致，我们可以对其简化.

**格式：**

```
修饰符 返回值类型 方法名(参数类型... 形参名){  }
```

**代码演示:**

```java
 public class Test1_可变参数 {
    public static void main(String[] args) {
        /*
            可变参数:
                概述:在JDK1.5之后，如果我们定义一个方法需要接受多个参数，并且多个参数类型一致，我们可以对其简化.
                格式:
                    修饰符 返回值类型 方法名(数据类型... 变量名){}


         */
        // 调用method1
        method1(10,20,30,40,50);

        // 调用method2
        int[] arr = {10,20,30,40,50};
        method2(arr);

        // 调用method3
        //method3();
        //method3(10);
        //method3(10,20,30,40,50);
        method3(10,20,30,40,50,60,70);// 传单个单个的数据

        System.out.println("======================");

        method3(arr);// 传数组
    }

    // 需求:定义一个方法,可以接收5个int类型的数
    public static void method3(int... nums){
        // 使用可变参数:把可变参数看成数组来使用
        for (int i = 0; i < nums.length; i++) {
            System.out.println(nums[i]);
        }
    }


    public static void method2(int[] arr){// 只能接收int类型的数组

    }

    public static void method1(int num1,int num2,int num3,int num4,int num5){// 只能接收5个int类型的数据

    }
}

```

#### 注意事项

​	1.一个方法只能有一个可变参数

​	2.如果方法中有多个参数，可变参数要放到最后。

#### **应用场景: Collections**

​	在Collections中也提供了添加一些元素方法：

​	`public static <T> boolean addAll(Collection<T> c, T... elements)  `:往集合中添加一些元素。

**代码演示:**

```java
public class Test3_可变参数的使用场景 {
    public static void main(String[] args) {
        /*
            Collections工具类:
                 public static <T> boolean addAll(Collection<T> c, T... elements):往集合中添加一些元素。
         */
        // 创建Collection集合,限制集合中元素的类型为String类型
        Collection<String> colors = new ArrayList<>();
        Collection<String> numbers = new ArrayList<>();

        // 往集合中添加一些元素
        Collections.addAll(numbers,"2","A","K","Q","J","10","9","8","7","6","5","4","3");
        Collections.addAll(colors,"♥","♠","♣","♦");

        // 打印集合
        System.out.println(colors);// [♥, ♠, ♣, ♦]
        System.out.println(numbers);// [2, A, K, Q, J, 10, 9, 8, 7, 6, 5, 4, 3]
    }
}
```

### 小结

略

# 第二章 Set接口

## 知识点--Set接口介绍

### 目标

- Set接口介绍

### 路径

- Set接口

### 讲解

```java
 Set接口:也称Set集合,但凡是实现了Set接口的类都叫做Set集合
	特点: 元素无索引,元素唯一(不重复)
    实现类:
		HashSet类:存取元素无序
		LinkedHashSet类:存取有序
		TreeSet类:对集合元素进行排序
   注意:
	   1.Set集合没有特殊的方法,都是使用Collection集合的方法
       2.Set集合的遍历方式; 增强for循环,迭代器
```

### 小结

略

## 知识点--HashSet集合

### 目标

- 能够说出HashSet集合的特点

### 路径

- HashSet集合的特点

### 讲解

`java.util.HashSet`是`Set`接口的一个实现类，它所存储的元素是不可重复的，并且元素都是无序的(即存取顺序不能保证不一致)。

我们先来使用一下Set集合存储，看下现象，再进行原理的讲解:

```java
public class Test {
    public static void main(String[] args) {
        /*
            HashSet<E>集合:元素存取元素无序,元素无索引,元素唯一(不重复)
         */
        // 创建HashSet集合,限制集合中元素的类型为String类型
        HashSet<String> set = new HashSet<>();

        // 添加元素
        set.add("nba");
        set.add("bac");
        set.add("abc");
        set.add("cba");
        set.add("nba");

        // 打印集合
        System.out.println("set:"+set);// [abc, cba, bac, nba]
    }
}
```

### 小结

略

## 知识点--HashSet集合存储数据的结构（哈希表）

### 目标

- 哈希表底层结构以及HashSet保证元素唯一原理

### 路径

- 哈希表底层结构
- HashSet保证元素唯一原理

### 讲解

##### 哈希表底层结构

在**JDK1.8**之前，哈希表底层采用数组+链表实现，即使用数组处理冲突，同一hash值的链表都存储在一个数组里。但是当位于一个桶中的元素较多，即hash值相等的元素较多时，通过key值依次查找的效率较低。而JDK1.8中，哈希表存储采用数组+链表+红黑树实现，当链表长度超过阈值（8）时，将链表转换为红黑树，这样大大减少了查找时间。

简单的来说，哈希表是由数组+链表+红黑树（JDK1.8增加了红黑树部分）实现的，如下图所示。

![1587659262084](img\1587659262084.png)

##### HashSet保证元素唯一原理

```java
HashSet集合保证元素唯一的原理:
依赖元素的hashCode()和equals()方法
1.当HashSet集合存储某个元素的时候,会调用该元素的hashCode方法计算该元素的哈希值
2.然后判断该哈希值对应的位置上,是否有元素
3.如果该哈希值对应的位置上,没有元素,那么就直接存储该元素
4.如果该哈希值对应的位置上,有元素,那么就产生了哈希冲突
5.如果产生了哈希冲突,就会调用该元素的equals()方法,与该位置上的所有元素进行一一比较:
   如果比较完后,没有一个元素与该元素相等,那么就直接存储
   如果比较的时候,有任意一个元素与该元素相等,那么就不存储

补充:
     Object类: hashCode()和equals()方法;
              hashCode():Object类中的hashCode()方法主要是根据地址值计算哈希值
              equals方法():Object类中的equals()方法是比较地址值
```

```java
public class Demo {
    public static void main(String[] args) {
        // 创建一个HashSet集合,限制集合中元素的类型为String
        HashSet<String> set = new HashSet<>();

        // 往集合中添加一些元素
        set.add("nba");
        set.add("cba");
        set.add("bac");
        set.add("abc");
        set.add("nba");

        // 遍历打印集合
        for (String e : set) {
            System.out.println(e);// cba abc  bac  nba
        }

        System.out.println("nba".hashCode());// nba:108845
        System.out.println("cba".hashCode());// cba:98274
        System.out.println("bac".hashCode());// bac:97284
        System.out.println("abc".hashCode());// abc:96354
    }
}
```

![image-20200816103504763](img\image-20200816103504763.png)

### 小结

略

## 知识点--  HashSet存储自定义类型元素

### 1.目标

- 使用HashSet集合存储自定义元素

### 2.路径

- 代码演示

### 3.讲解

给HashSet中存放自定义类型元素时，需要重写对象中的hashCode和equals方法，建立自己的比较方式，才能保证HashSet集合中的对象唯一.

```java
public class Person {
    public String name;// 姓名
    public int age;// 年龄

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

```

创建测试类:

```java
public class Test {
    public static void main(String[] args) {
        /*
            结论:
                如果HashSet集合存储自定义类型的元素,那么该元素所属的类需要重新hashCode和equals方法
         */
        // 创建HashSet集合,限制集合中元素的类型为Person类型
        HashSet<Person> set = new HashSet<>();

        // 创建几个Person对象
        Person p1 = new Person("张三",18);
        Person p2 = new Person("李四",19);
        Person p3 = new Person("王五",17);
        Person p4 = new Person("张三",18);

        // 往集合中存储Person对象
        set.add(p1);
        set.add(p2);
        set.add(p3);
        set.add(p4);

        // 遍历输出集合中的Person对象
        for (Person p : set) {
            System.out.println(p);
        }

        System.out.println(p1.hashCode());
        System.out.println(p2.hashCode());
        System.out.println(p3.hashCode());
        System.out.println(p4.hashCode());
    }
}

```



### 小结

略

## 知识点-- LinkedHashSet

### 目标

- 使用LinkedHashSet保证元素怎么存就怎么取,即存取有序

### 路径

- 代码演示

### 讲解

我们知道HashSet保证元素唯一，可是元素存放进去是没有顺序的，那么我们要保证有序，怎么办呢？

在HashSet下面有一个子类`java.util.LinkedHashSet`，它是链表和哈希表组合的一个数据存储结构。

演示代码如下:

```java
public class Test {
    public static void main(String[] args) {
        /*
            LinkedHashSet集合:链表+哈希表结构
                 特点:元素存取有序,元素无索引,元素唯一(不重复)
                 由链表保证元素存取有序,哈希表保证元素唯一
                 
         */
        // 创建LinkedHashSet集合,限制集合中元素的类型为String类型
        LinkedHashSet<String> set = new LinkedHashSet<>();
        
        // 往集合中存储数据
        set.add("nba");
        set.add("bac");
        set.add("abc");
        set.add("cba");
        set.add("nba");

        // 打印集合
        System.out.println(set);// [nba, bac, abc, cba]
    }
}

```

### 小结

略

## 知识点-- TreeSet集合

### 目标

- 知道使用TreeSet集合的特点并能够使用TreeSet集合

### 路径

- 代码演示

### 讲解

#### 特点

TreeSet集合是Set接口的一个实现类,底层依赖于TreeMap,是一种基于**红黑树**的实现,其特点为：

1. 元素唯一
2. 元素没有索引
3. 使用元素的[自然顺序](../../java/lang/Comparable.html)对元素进行排序，或者根据创建 TreeSet 时提供的 [`Comparator`](../../java/util/Comparator.html) 比较器
   进行排序，具体取决于使用的构造方法：

```java
public TreeSet()：								根据其元素的自然排序进行排序
public TreeSet(Comparator<E> comparator):    根据指定的比较器进行排序
```

#### 演示

案例演示**自然排序**

```java
public class Test {
    /*
        TreeSet集合:  红黑树结构
            特点:可以对元素进行排序,元素无索引,元素唯一(不重复)
            构造方法:
                public TreeSet()：						     根据其元素的默认规则进行排序
                        默认规则:事先写好的规则
                        要求: 要求集合元素所属的类必须实现Comparable接口,重写Comparable接口中的compareTo方法
                                在compareTo方法中指定排序规则

                public TreeSet(Comparator<E> comparator):    根据指定的规则进行排序
     */
    public static void main(String[] args) {
        // 创建TreeSet集合,限制集合中元素的类型为Integer类型
        TreeSet<Integer> set = new TreeSet<>();
        // 往集合中存储数据
        set.add(300);
        set.add(200);
        set.add(100);
        set.add(500);
        set.add(400);
        set.add(400);

        System.out.println(set);// [100, 200, 300, 400, 500]

        System.out.println("==================================");

        // 创建TreeSet集合,限制集合中元素的类型为Student类型
        TreeSet<Student> set2 = new TreeSet<>();

        // 往集合中存储数据
        set2.add(new Student("张三",18));
        set2.add(new Student("李四",19));
        set2.add(new Student("王五",17));
        set2.add(new Student("王五",17));

        System.out.println(set2);// 年龄升序排序   
    }
}

```

案例演示**比较器排序**:

```java
public class Test {
    /*
        TreeSet集合:  红黑树结构
            特点:可以对元素进行排序,元素无索引,元素唯一(不重复)
            构造方法:
                public TreeSet()：						     根据其元素的默认规则进行排序
                        默认规则:事先写好的规则
                        要求: 要求集合元素所属的类必须实现Comparable接口,重写Comparable接口中的compareTo方法
                                在compareTo方法中指定排序规则

                public TreeSet(Comparator<E> comparator):    根据指定的规则进行排序
     */
    public static void main(String[] args) {

        // 创建TreeSet集合,限制集合中元素的类型为Integer类型
        TreeSet<Integer> set3 = new TreeSet<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        // 往集合中存储数据
        set3.add(300);
        set3.add(200);
        set3.add(100);
        set3.add(500);
        set3.add(400);
        set3.add(400);
        System.out.println(set3);// [500, 400, 300, 200, 100]

        System.out.println("==================================");


        // 创建TreeSet集合,限制集合中元素的类型为Student类型
        TreeSet<Student> set4 = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                return o2.age - o1.age;
            }
        });

        // 往集合中存储数据
        set4.add(new Student("张三",18));
        set4.add(new Student("李四",19));
        set4.add(new Student("王五",17));
        set4.add(new Student("王五",17));

        System.out.println(set4);// 年龄降序排序

    }
}

```

### 小结

- ```java
  public TreeSet(); 按照默认规则排序
  public TreeSet(Comparator<E> comparator); 按照指定规则排序  
  ```

  

# 第三章 Map集合

## 知识点-- Map概述

### 目标

- 能够说出Map集合的特点

### 路径

- 图文演示

### 讲解

![](img/Collection与Map.bmp)

```java
Map<K,V>集合的特点: K用来限制键的类型,V用来限制值的类型
       1.Map集合是以键值对的形式存储数据,每个键值对都有键和值
       2.Map集合中的键是唯一的,值是可以重复的,如果键重复了,值就会覆盖
       3.根据键取值

Map集合子类:
     HashMap:存储数据采用的结构是哈希表结构,所以不能保证键值对存取有序,可以保证键唯一
         	由哈希表保证键唯一,所以键所属的类需要重写hashCode和equals方法
         
     LinkedHashMap:存储数据采用的结构是链表+哈希表结构,由链表保证键值对存取有序,由哈希表保证键唯一
         	由哈希表保证键唯一,所以键所属的类需要重写hashCode和equals方法
         
     TreeMap:存储数据采用的是红黑树结构,可以保证键唯一,并且可以对键进行排序
         	public TreeMap(); 按照默认规则排序
            public TreeMap(Comparator<? super K> comparator): 按照指定规则排序
```

### 小结

略

## 知识点-- Map的常用方法

### 目标

- 使用Map的常用方法

### 路径

- 代码演示

### 讲解

Map接口中定义了很多方法，常用的如下：

- `public V put(K key, V value)`:  把指定的键与指定的值添加到Map集合中。
- `public V remove(Object key)`: 把指定的键 所对应的键值对元素 在Map集合中删除，返回被删除元素的值。
- `public V get(Object key)` 根据指定的键，在Map集合中获取对应的值。
- `public boolean containsKey(Object key)`:判断该集合中是否有此键
- `public boolean containsKey(Object value)`:判断该集合中是否有此值
- `public Set<K> keySet()`: 获取Map集合中所有的键，存储到Set集合中。
- `public Collection<V> values()`: 获取Map集合中所有的值，存储到Set集合中。
- `public Set<Map.Entry<K,V>> entrySet()`: 获取到Map集合中所有的键值对对象的集合(Set集合)。

Map接口的方法演示

```java
public class Test {
    public static void main(String[] args) {
        /*
            Map<K,V>的常用方法:
                - public V put(K key, V value):  把指定的键与指定的值添加到Map集合中。
                - public V remove(Object key): 把指定的键 所对应的键值对元素 在Map集合中删除，返回被删除元素的值。
                - public V get(Object key) 根据指定的键，在Map集合中获取对应的值。
                - public boolean containsKey(Object key):判断该集合中是否有此键
                - public boolean containsKey(Object value):判断该集合中是否有此值
                - public Set<K> keySet(): 获取Map集合中所有的键，存储到Set集合中。
                - public Collection<V> values(): 获取Map集合中所有的值，存储到Set集合中。
                - public Set<Map.Entry<K,V>> entrySet(): 获取到Map集合中所有的 键值对对象 的集合(Set集合)。
                键值对对象: Entry<K,V>接口类型来表示
                由于Entry<K,V>接口是Map接口的成员内部接口,所以外界要使用的时候,需要这么写:Map.Entry<K,V>

                Set<String>   字符串对象  String
                Set<Map.Entry<K,V>>         键值对对象  Map.Entry<K,V>
         */
        // 创建Map集合,限制键的类型为String,值的类型为String类型
        Map<String, String> map = new HashMap<>();

        // 往map集合中添加键值对
        map.put("黄晓明", "杨颖");
        map.put("文章", "马伊琍");
        map.put("谢霆锋", "王菲");

        System.out.println("map:"+map);// {文章=马伊琍, 谢霆锋=王菲, 黄晓明=杨颖}

        // 验证: 键唯一,如果键重复了,值会覆盖
        map.put("文章", "姚笛");
        System.out.println("map:"+map);// {文章=姚笛, 谢霆锋=王菲, 黄晓明=杨颖}

        // 验证: 值可以重复
        map.put("李亚鹏", "王菲");
        System.out.println("map:"+map);// {文章=姚笛, 谢霆锋=王菲, 李亚鹏=王菲, 黄晓明=杨颖}

        // 需求:删除李亚鹏这个键对应的键值对
        String removeV = map.remove("李亚鹏");
        System.out.println("被删除键的值:"+removeV);// 王菲
        System.out.println("map:"+map);// {文章=姚笛, 谢霆锋=王菲, 黄晓明=杨颖}

        // 需求: 获取文章这个键对应的值
        String value1 = map.get("文章");
        System.out.println("value1:"+value1);// 姚笛

        // 需求:判断是否有谢霆锋这个键
        System.out.println(map.containsKey("谢霆锋"));// true
        // 需求:判断是否有李亚鹏这个键
        System.out.println(map.containsKey("李亚鹏"));// false

        // 需求:判断是否有姚笛这个值
        System.out.println(map.containsValue("姚笛"));// true
        // 需求:判断是否有马伊琍这个值
        System.out.println(map.containsValue("马伊琍"));// false

        // 需求:获取map集合中所有的键
        Set<String> keys = map.keySet();
        System.out.println("所有的键:"+keys);// [文章, 谢霆锋, 黄晓明]

        // 需求:获取map集合中所有的值
        Collection<String> values = map.values();
        System.out.println("所有的值:"+values);// [姚笛, 王菲, 杨颖]

        // 需求:获取map集合中所有 键值对对象
        Set<Map.Entry<String, String>> entrys = map.entrySet();
        System.out.println(entrys);// [文章=姚笛, 谢霆锋=王菲, 黄晓明=杨颖]
    }
}

```

> tips:
>
> 使用put方法时，若指定的键(key)在集合中没有，则没有这个键对应的值，返回null，并把指定的键值添加到集合中； 
>
> 若指定的键(key)在集合中存在，则返回值为集合中键对应的值（该值为替换前的值），并把指定键所对应的值，替换成指定的新值。 

```java
        String putV = map.put("邓超", "孙俪");
        System.out.println("putV:"+putV);// null

        String putValue = map.put("谢霆锋", "张柏芝");
        System.out.println("putValue:"+putValue);// 王菲
```



### 小结

略

## 知识点--Map的遍历

### 目标

- 使用Map的遍历

### 路径

- 方式1:键找值方式
- 方式2:键值对方式

### 讲解

#### 方式1:键找值方式

通过元素中的键，获取键所对应的值

分析步骤：

1. 获取Map中所有的键，由于键是唯一的，所以返回一个Set集合存储所有的键。方法提示:`keyset()`
2. 遍历键的Set集合，得到每一个键。
3. 根据键，获取键所对应的值。方法提示:`get(K key)`

```java
public class Test1_方式一 {
    public static void main(String[] args) {
        /*
            根据键找值:
                1.获取map集合所有的键  keySet()方法
                2.遍历所有的键
                3.根据键找值          get(K k)方法

         */
        // 创建Map集合,限制键的类型为String,值的类型为String类型
        Map<String, String> map = new HashMap<>();

        // 往map集合中添加键值对
        map.put("黄晓明", "杨颖");
        map.put("文章", "马伊琍");
        map.put("谢霆锋", "王菲");

        // 1.获取map集合所有的键  keySet()方法
        Set<String> keys = map.keySet();

        // 2.遍历所有的键
        for (String key : keys) {
            // 3.根据键找值          get(K k)方法
            String value = map.get(key);
            System.out.println("key:"+key+",value:"+value);
        }
    }
}

```

#### 方式2:键值对方式

```
Entry<K,V>接口:简称Entry项,表示键值对对象,用来封装Map集合中的键值对
Entry<K,V>接口:是Map接口中的内部接口,在外部使用的时候是这样表示: Map.Entry<K,V>

Map集合中提供了一个方法来获取所有键值对对象:
            public Set<Map.Entry<K,V>> entrySet()

根据键值对对对象获取键和值:
            - public K getKey()：获取Entry对象中的键。
            - public V getValue()：获取Entry对象中的值。

Map遍历方式二:根据键值对对象的方式
            1.获取集合中所有键值对对象，以Set集合形式返回。  Set<Map.Entry<K,V>> entrySet()
            2.遍历所有键值对对象的集合，得到每一个键值对(Entry)对象。
            3.在循环中,可以使用键值对对对象获取键和值   getKey()和getValue()
```

```java
public class Test2_方式二 {
    public static void main(String[] args) {
        /*
            键值对方式:
                1.获取map集合的所有键值对对象   entrySet()方法
                2.循环遍历所有的键值对对象
                3.根据键值对对象去获取键和值

            键值对对象: Entry<K,V>接口类型来表示
            由于Entry<K,V>接口是Map接口的成员内部接口,所以外界要使用的时候,需要这么写:Map.Entry<K,V>
            Entry<K,V>接口中的方法:
                    K getKey();  获取键值对对象的键
                    V getValue();获取键值对对象的值
         */
        // 创建Map集合,限制键的类型为String,值的类型为String类型
        Map<String, String> map = new HashMap<>();

        // 往map集合中添加键值对
        map.put("黄晓明", "杨颖");
        map.put("文章", "马伊琍");
        map.put("谢霆锋", "王菲");

        // 1.获取map集合的所有键值对对象   entrySet()方法
        Set<Map.Entry<String, String>> entrys = map.entrySet();

        // 2.循环遍历所有的键值对对象
        for (Map.Entry<String, String> entry : entrys) {
            // 3.根据键值对对象去获取键和值
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println("key:"+key+",value:"+value);
        }

    }
}
```

### 小结

略

## 知识点--  HashMap存储自定义类型

### 目标

- 使用HashMap存储自定义类型

### 路径

- 代码演示

### 讲解

练习：每位学生（姓名，年龄）都有自己的家庭住址。那么，既然有对应关系，则将学生对象和家庭住址存储到map集合中。学生作为键, 家庭住址作为值。

> 注意，学生姓名相同并且年龄相同视为同一名学生。

编写学生类：

```java
public class Student {
    public String name;// 姓名
    public int age;// 年龄

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

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age &&
                Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

```

编写测试类：

```java 
public class Test {
    public static void main(String[] args) {
        /*
            练习：每位学生（姓名，年龄）都有自己的家庭住址。那么，既然有对应关系，则将学生对象和家庭住址存储到map集合中。
                 学生作为键, 家庭住址作为值。
                 注意，学生姓名相同并且年龄相同视为同一名学生。
         */
        // 创建HashMap集合,限制键的类型为Student,值的类型为String
        HashMap<Student, String> map = new HashMap<>();

        // 创建学生对象
        Student stu1 = new Student("张三",18);
        Student stu2 = new Student("李四",19);
        Student stu3 = new Student("王五",17);
        Student stu4 = new Student("张三",18);

        // 往集合中添加键值对
        map.put(stu1, "北京");
        map.put(stu2, "上海");
        map.put(stu3, "深圳");
        map.put(stu4, "广州");

        // 循环遍历集合
        Set<Student> keys = map.keySet();
        for (Student key : keys) {
            String value = map.get(key);
            System.out.println("key:"+key+",value:"+value);
        }
    }
}

```

- 当给HashMap中存放自定义对象时，如果自定义对象作为key存在，这时要保证对象唯一，必须复写对象的hashCode和equals方法(如果忘记，请回顾HashSet存放自定义对象)。
- 如果要保证map中存放的key和取出的顺序一致，可以使用`java.util.LinkedHashMap`集合来存放。

### 小结

略

## 知识点--LinkedHashMap介绍

### 目标

- 我们知道HashMap保证成对元素唯一，并且查询速度很快，可是成对元素存放进去是没有顺序的，那么我们要保证有序，还要速度快怎么办呢？

### 路径

- LinkedHashMap

### 讲解

- 通过链表结构可以保证元素的存取顺序一致；
- 通过哈希表结构可以保证的键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。

```java
public class Test {
    public static void main(String[] args) {
        /*
            LinkedHashMap集合特点:键值对存取有序,键唯一,值可以重复,如果键重复了,值就会覆盖
                由于是由哈希表保证键唯一,所以如果键是自定义类型的类,那么必须重写hashCode和equals方法
         */
        // 创建LinkedHashMap集合,限制键的类型为String,值的类型为String
        LinkedHashMap<String, String> map = new LinkedHashMap<>();

        // 往集合中添加键值对
        map.put("黄晓明", "杨颖");
        map.put("文章", "马伊琍");
        map.put("谢霆锋", "王菲");
        map.put("李亚鹏", "王菲");
        map.put("文章", "姚笛");


        // 打印集合
        System.out.println(map);// {黄晓明=杨颖, 文章=姚笛, 谢霆锋=王菲, 李亚鹏=王菲}
    }
}

```

### 小结

略

## 知识点--TreeMap集合

### 目标

- 使用TreeMap集合

### 路径

- TreeMap介绍
- 构造方法

### 讲解

#### TreeMap介绍

TreeMap集合和Map相比没有特有的功能，底层的数据结构是红黑树；可以对元素的***键***进行排序，排序方式有两种:**自然排序**和**比较器排序**；到时使用的是哪种排序，取决于我们在创建对象的时候所使用的构造方法；

#### 构造方法

```java
public TreeMap()									使用自然排序
public TreeMap(Comparator<? super K> comparator) 	   通过比较器指定规则排序
```

#### 案例演示

```java
public class Test {
    public static void main(String[] args) {
        /*
            TreeMap集合的特点:可以对键值对进行排序,键唯一,值可以重复,如果键重复了,值就会覆盖
            TreeMap集合的构造方法:
                public TreeMap()									使用默认规则排序
                如果键是自定义类型的类,那么该类需要实现Comparable接口,重写compareTo方法,指定排序规则
                public TreeMap(Comparator<? super K> comparator) 	通过比较器指定规则排序
         */
        // 默认规则排序: 升序
        // 创建TreeMap集合,限制键的类型为Integer,值的类型为String
        TreeMap<Integer, String> map = new TreeMap<>();

        // 往集合中添加键值对
        map.put(300, "刘德华");
        map.put(100, "张学友");
        map.put(200, "郭富城");
        map.put(500, "黎明");
        map.put(400, "梁朝伟");
        map.put(400, "古天乐");
        map.put(600, "古天乐");

        // 打印集合
        System.out.println(map);


        System.out.println("=======================================");

        // 指定规则排序: 降序
        // 创建TreeMap集合,限制键的类型为Integer,值的类型为String
        TreeMap<Integer, String> map2 = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });

        // 往集合中添加键值对
        map2.put(300, "刘德华");
        map2.put(100, "张学友");
        map2.put(200, "郭富城");
        map2.put(500, "黎明");
        map2.put(400, "梁朝伟");
        map2.put(400, "古天乐");
        map2.put(600, "古天乐");

        // 打印集合
        System.out.println(map2);

    }
}

```

### 小结

略

## 案例-- Map集合练习

### 需求

- 输入一个字符串中每个字符出现次数。

### 分析

- 获取一个字符串对象
- 创建一个Map集合，键代表字符，值代表次数。
- 遍历字符串得到每个字符。
- 判断Map中是否有该键。
- 如果没有，第一次出现，存储次数为1；如果有，则说明已经出现过，获取到对应的值进行++，再次存储。     
- 打印最终结果

### 实现

**方法介绍**

`public boolean containKey(Object key)`:判断该集合中是否有此键。

**代码：**

```java
public class Test {
    public static void main(String[] args) {
        // 1.键盘录入一个字符串
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串:");
        String str = sc.nextLine();

        // 2.创建Map集合,限制键的类型为Character,值的类型为Integer
        Map<Character, Integer> map = new HashMap<>();

        // 3.循环遍历该字符串
        for (int i = 0; i < str.length(); i++) {
            // 4.在循环中,获取遍历出来的字符
            char c = str.charAt(i);

            // 5.在循环中,判断该字符在Map集合中是否已经作为了键
            if (map.containsKey(c)) {
                // 6.如果已经作为了键,那么就取出该键对应的值,然后让该值+1,作为新的值,存储到Map集合中
                Integer value = map.get(c);
                value++;
                map.put(c,value);

            } else {
                // 6.如果没有作为键,那么该字符作为键,值为1,存储到Map集合中
                map.put(c,1);
            }
        }
        // 7.打印map集合的键值对
        System.out.println(map);
    }
}
```

### 小结

略

# 第四章 集合的嵌套

- **总述：任何集合内部都可以存储其它任何集合**

## 知识点--集合的嵌套

### 目标

- 理解集合的嵌套

### 路径

- List嵌套List
- List嵌套Map
- Map嵌套Map

### 讲解

#### List嵌套List

~~~java
public class Test1 {
    public static void main(String[] args) {
        /*
            集合的嵌套:
                - List嵌套List
                - List嵌套Map
                - Map嵌套Map
            结论:任何集合内部都可以存储其它任何集合
         */
        //  List嵌套List
        // 创建一个List集合,限制元素类型为String
        List<String> list1 = new ArrayList<>();

        // 往集合中添加元素
        list1.add("王宝强");
        list1.add("贾乃亮");
        list1.add("陈羽凡");

        // 创建一个List集合,限制元素类型为String
        List<String> list2 = new ArrayList<>();

        // 往集合中添加元素
        list2.add("马蓉");
        list2.add("李小璐");
        list2.add("白百何");

        // 创建一个List集合,限制元素类型为List集合 (List集合中的元素是List集合)
        List<List<String>> list = new ArrayList<>();
        list.add(list1);
        list.add(list2);

        // 遍历
        for (List<String> e : list) {
            for (String name : e) {
                System.out.println(name);
            }
            System.out.println("=============");
        }

        System.out.println(list);
    }
}


~~~

#### List嵌套Map

~~~java
public class Test2 {
    public static void main(String[] args) {
        /*
            List嵌套Map:

         */
        // 创建Map集合对象
        Map<String,String> map1 = new HashMap<>();
        map1.put("it001","迪丽热巴");
        map1.put("it002","古力娜扎");

        // 创建Map集合对象
        Map<String,String> map2 = new HashMap<>();
        map2.put("heima001","蔡徐坤");
        map2.put("heima002","李易峰");

        // 创建List集合,用来存储以上2个map集合
        List<Map<String,String>> list = new ArrayList<>();
        list.add(map1);
        list.add(map2);

        System.out.println(list.size()); // 2

        for (Map<String, String> map : list) {
            // 遍历获取出来的map集合对象
            Set<String> keys = map.keySet();// 获取map集合所有的键
            // 根据键找值
            for (String key : keys) {
                System.out.println(key + ","+ map.get(key));
            }
        }

    }
}

~~~

#### Map嵌套Map

~~~java
public class Test3_Map嵌套Map {
    public static void main(String[] args) {
        /*
            Map嵌套Map:

         */
        // 创建Map集合对象
        Map<String, String> map1 = new HashMap<>();
        map1.put("it001", "迪丽热巴");
        map1.put("it002", "古力娜扎");

        // 创建Map集合对象
        Map<String, String> map2 = new HashMap<>();
        map2.put("heima001", "蔡徐坤");
        map2.put("heima002", "李易峰");

        // 创建Map集合,把以上2个Map集合作为值存储到这个map集合中
        Map<String, Map<String, String>> map = new HashMap<>();

        // 往map集合中添加键值对
        map.put("itheima", map1);
        map.put("itcast", map2);

        // 获取map集合所有的键
        Set<String> keys = map.keySet();

        // 循环遍历所有的键
        for (String key : keys) {
            // 根据键找值
            Map<String, String> stringMap = map.get(key);
            //System.out.println(key + ":" + stringMap);
            // 获取stringMap集合所有的键
            Set<String> keySet = stringMap.keySet();
            // 循环遍历所有的键
            for (String k : keySet) {
                // 根据键找值
                String v = stringMap.get(k);
                System.out.println(k+"="+v);
            }

            System.out.println("===================");
        }


    }
}

~~~

### 小结

- 略

# 第五章 模拟斗地主洗牌发牌

## 需求

按照斗地主的规则，完成洗牌发牌的动作。

![](img/斗地主.png)

具体规则：

1. 组装54张扑克牌
2. 54张牌顺序打乱
3. 三个玩家参与游戏，**三人交替摸牌，每人17张牌**，最后三张留作底牌。
4. 查看三人各自手中的牌（按照牌的大小排序）、底牌

> 规则：手中扑克牌从大到小的摆放顺序：大王,小王,2,A,K,Q,J,10,9,8,7,6,5,4,3

## 分析

1.准备牌：

完成数字与纸牌的映射关系：

使用双列Map(HashMap)集合，完成一个数字与字符串纸牌的对应关系(相当于一个字典)。

2.洗牌：

通过数字完成洗牌发牌

3.发牌：

将每个人以及底牌设计为ArrayList<String>,将最后3张牌直接存放于底牌，剩余牌通过对3取模依次发牌。

存放的过程中要求数字大小与斗地主规则的大小对应。

将代表不同纸牌的数字分配给不同的玩家与底牌。

4.看牌：

通过Map集合找到对应字符展示。

通过查询纸牌与数字的对应关系，由数字转成纸牌字符串再进行展示。

![image-20200816163457597](img\image-20200816163457597.png)

## 实现

```java
package com.itheima04;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;

public class Test {
    public static void main(String[] args) {
        /*
            模拟斗地主洗牌发牌:
            需求
                按照斗地主的规则，完成洗牌发牌的动作。
                具体规则：
                    1. 组装54张扑克牌
                    2. 54张牌顺序打乱
                    3. 三个玩家参与游戏，三人交替摸牌，每人17张牌，最后三张留作底牌。
                    4. 查看三人各自手中的牌（按照牌的大小排序）、底牌
                    规则：手中扑克牌从大到小的摆放顺序：大王,小王,2,A,K,Q,J,10,9,8,7,6,5,4,3
         */
        // 造牌
        // 1.创建Map集合对象,限制键的类型为Integer,值的类型为String
        HashMap<Integer, String> pokeBox = new HashMap<>();
        // 2.创建一个List集合,表示花色集合,
        ArrayList<String> colors = new ArrayList<>();
        // 3.创建一个List集合,表示牌面值集合
        ArrayList<String> numbers = new ArrayList<>();

        // 4.往花色集合中存储4个花色
        Collections.addAll(colors, "♥", "♦", "♠", "♣");
        // 5.往牌面值集合中存储13个牌面值
        Collections.addAll(numbers, "2", "A", "K", "Q", "J", "10", "9", "8", "7", "6", "5", "4", "3");

        // 6.定义一个int类型的变量,表示牌的编号,初始值为0
        int id = 0;
        // 7.往map集合中添加大王,编号为0,添加完后编号自增1
        pokeBox.put(id++, "大王");

        // 8.往map集合中添加小王,编号为1,添加完后编号自增1
        pokeBox.put(id++, "小王");

        // 9.牌面值的集合和花色集合循环嵌套遍历,注意牌面值集合作为外层循环,花色集合作为内层循环
        for (String number : numbers) {
            for (String color : colors) {
                // 10.在循环中,遍历出来的牌面值和花色组成一张扑克牌
                String pai = color + number;
                // 11.在循环中,编号作为键,扑克牌作为值存储到map集合中,每存储一张牌后,编号自增1
                pokeBox.put(id++,pai);
            }
        }

        System.out.println(pokeBox.size());
        System.out.println(pokeBox);

        //2.洗牌 :--->洗牌的编号
        //2.1 获取所有牌的编号,返回的是所有编号的Set集合
        Set<Integer> keySet = pokeBox.keySet();

        //2.2 创建ArrayList集合,用来存储所有的牌编号
        ArrayList<Integer> idList = new ArrayList<>();

        //2.3 把keySet集合中存储的所有牌编号,存储到这个新的ArrayList集合中
        idList.addAll(keySet);

        //2.4 使用Collections.shuffle方法对新的ArrayList集合中的元素打乱顺序
        Collections.shuffle(idList);
        System.out.println("打乱顺序后的牌编号:"+idList.size());// 54
        System.out.println("打乱顺序后的牌编号:"+idList);


        // 3.发牌-->发牌的编号--->对牌的编号进行从小到大排序---->再根据排好序的编号去map集合中获取牌
        // 3.1 创建4个List集合,分别用来存储玩家一,玩家二,玩家三,底牌得到的牌编号
        ArrayList<Integer> play1Id = new ArrayList<>();
        ArrayList<Integer> play2Id = new ArrayList<>();
        ArrayList<Integer> play3Id = new ArrayList<>();
        ArrayList<Integer> diPaiId = new ArrayList<>();

        // 3.2 循环把打乱顺序的牌编号,按照规律依次发给玩家一,玩家二,玩家三,底牌
        for (int i = 0; i < idList.size(); i++) {
            // 获取牌编号
            Integer paiId = idList.get(i);
            // 三人交替摸牌
            if (i >= 51){
                diPaiId.add(paiId);
            }else if (i%3==0){
                play1Id.add(paiId);
            }else if (i%3==1){
                play2Id.add(paiId);
            }else if (i%3==2){
                play3Id.add(paiId);
            }
        }

        // 3.3 对获取到的牌编号进行从小到大排序
        Collections.sort(play1Id);
        Collections.sort(play2Id);
        Collections.sort(play3Id);
        Collections.sort(diPaiId);

        // 3.4 根据排好序的牌编号去map集合中获取牌
        // 遍历玩家一的牌编号
        System.out.print("玩家一的牌:");
        for (Integer paiId : play1Id) {// 1,2,3,4,5
            String pai = pokeBox.get(paiId);
            System.out.print(pai+" ");
        }

        System.out.println();

        // 遍历玩家二的牌编号
        System.out.print("玩家二的牌:");
        for (Integer paiId : play2Id) {
            String pai = pokeBox.get(paiId);
            System.out.print(pai+" ");
        }

        System.out.println();

        // 遍历玩家三的牌编号
        System.out.print("玩家三的牌:");
        for (Integer paiId : play3Id) {
            String pai = pokeBox.get(paiId);
            System.out.print(pai+" ");
        }

        System.out.println();

        // 遍历底牌的牌编号
        System.out.print("底牌的牌:");
        for (Integer paiId : diPaiId) {
            String pai = pokeBox.get(paiId);
            System.out.print(pai+" ");
        }
    }
}

```

## 小结

略

# 总结

```java
- 能够使用集合工具类
    Collections工具类:
	- public static void shuffle(List<?> list):打乱集合顺序。
	- public static <T> void sort(List<T> list):将集合中元素按照默认规则排序。
	- public static <T> void sort(List<T> list，Comparator<? super T> comparator):将集合中元素按照指定规则排序。
	- public static <T> boolean addAll(Collection<T> c, T... elements):往集合中添加一些元素。
        
- 能够使用Comparator比较器进行排序
      重新compare(T o1,T o2)方法,在compare方法中指定排序规则
     前减后: 升序
     后减前: 降序
     前: 第一个参数    后:第二个参数
         
- 能够使用可变参数
   格式:
		修饰符  返回值类型 方法名(数据类型... 变量名{}
   使用:
       1.传参数,可以传多个该数据类型的数据,或者该数据类型的数组
       2.在方法中,使用的时候,就把可变参数当成数组使用
       3.一个方法中只能有一个可变参数
       4.可变参数只能放在最后
                       
- 能够说出Set集合的特点
     元素唯一(不重复),元素无索引
                       
- 能够说出哈希表的特点
      jdk8以前: 哈希表就是由数组+链表组成
      jdk8以后: 
             元素的个数没有超过8:哈希表就是由数组+链表
             元素的个数超过8:哈希表就是由数组+链表+红黑树组成
      哈希表保证元素唯一: 依赖的是元素的hashCode()和equals()方法
                       
- 使用HashSet集合存储自定义元素
      要求自定义的类需要重写hashCode()和equals()方法 --->alt+insert-->equals and hashCode
                       
- 能够说出Map集合特点
    1.以键值对的形式存储数据,每个键值对都有键和值
    2.键唯一,值可以重复,如果键重复了,值就会覆盖
    3.根据键找值
                       
- 使用Map集合添加方法保存数据
  - public V put(K key, V value):  把指定的键与指定的值添加到Map集合中。
  - public V remove(Object key): 把指定的键 所对应的键值对元素 在Map集合中删除，返回被删除元素的值。
  - public V get(Object key) 根据指定的键，在Map集合中获取对应的值。
  - public boolean containsKey(Object key):判断该集合中是否有此键
  - public boolean containsKey(Object value):判断该集合中是否有此值
  - public Set<K> keySet(): 获取Map集合中所有的键，存储到Set集合中。
  - public Collection<V> values(): 获取Map集合中所有的值，存储到Set集合中。
  - public Set<Map.Entry<K,V>> entrySet(): 获取到Map集合中所有的键值对对象的集合(Set集合)。

                       
- 使用”键找值”的方式遍历Map集合
     1.获取map集合所有的键
     2.循环遍历所有的键
     3.在循环中,根据键找值
                       
- 使用”键值对”的方式遍历Map集合
	 1.获取map集合所有的键值对对象
     2.循环遍历所有的键值对对象
     3.在循环中,使用键值对对象获取键和值  getKey()  getValue()
                        
- 能够使用HashMap存储自定义键值对的数据
     键所属的类需要重写hashCode()和equals()方法
                       
- 能够完成斗地主洗牌发牌案例
     造牌--洗牌--发牌
```



# 扩展

## 3.4 HashSet的源码分析

### 3.4.1 HashSet的成员属性及构造方法

~~~java
public class HashSet<E> extends AbstractSet<E>
    					implements Set<E>, Cloneable, java.io.Serializable{
    
	//内部一个HashMap——HashSet内部实际上是用HashMap实现的
    private transient HashMap<E,Object> map;
    // 用于做map的值
    private static final Object PRESENT = new Object();
    /**
     * 构造一个新的HashSet，
     * 内部实际上是构造了一个HashMap
     */
    public HashSet() {
        map = new HashMap<>();
    }
    
}
~~~

- 通过构造方法可以看出，HashSet构造时，实际上是构造一个HashMap

### 3.4.2 HashSet的add方法源码解析

~~~java
public class HashSet{
    //......
    public boolean add(E e) {
       return map.put(e, PRESENT)==null;//内部实际上添加到map中，键：要添加的对象，值：Object对象
    }
    //......
}
~~~

### 3.4.3 HashMap的put方法源码解析

~~~java
public class HashMap{
    //......
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    //......
    static final int hash(Object key) {//根据参数，产生一个哈希值
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    //......
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; //临时变量，存储"哈希表"——由此可见，哈希表是一个Node[]数组
        Node<K,V> p;//临时变量，用于存储从"哈希表"中获取的Node
        int n, i;//n存储哈希表长度；i存储哈希表索引
        
        if ((tab = table) == null || (n = tab.length) == 0)//判断当前是否还没有生成哈希表
            n = (tab = resize()).length;//resize()方法用于生成一个哈希表，默认长度：16，赋给n
        if ((p = tab[i = (n - 1) & hash]) == null)//(n-1)&hash等效于hash % n，转换为数组索引
            tab[i] = newNode(hash, key, value, null);//此位置没有元素，直接存储
        else {//否则此位置已经有元素了
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))//判断哈希值和equals
                e = p;//将哈希表中的元素存储为e
            else if (p instanceof TreeNode)//判断是否为"树"结构
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {//排除以上两种情况，将其存为新的Node节点
                for (int binCount = 0; ; ++binCount) {//遍历链表
                    if ((e = p.next) == null) {//找到最后一个节点
                        p.next = newNode(hash, key, value, null);//产生一个新节点，赋值到链表
                        if (binCount >= TREEIFY_THRESHOLD - 1) //判断链表长度是否大于了8
                            treeifyBin(tab, hash);//树形化
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))//跟当前变量的元素比较，如果hashCode相同，equals也相同
                        break;//结束循环
                    p = e;//将p设为当前遍历的Node节点
                }
            }
            if (e != null) { // 如果存在此键
                V oldValue = e.value;//取出value
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;//设置为新value
                afterNodeAccess(e);//空方法，什么都不做
                return oldValue;//返回旧值
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
}
~~~



## 3.5 LinkedHashSet