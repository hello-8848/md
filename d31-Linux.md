---
typora-copy-images-to: img
---

# day31-Linux

# 学习目标

- [ ] 能够独立搭建Linux环境
- [ ] 能够使用Linux进行目录操作的命令
- [ ] 能够使用Linux进行文件操作的命令
- [ ] 能够使用Linux进行目录文件压缩和解压的命令
- [ ] 能够使用Linux进行目录文件权限的命令
- [ ] 能够使用其它常用的Linux命令
- [ ] 掌握Linux网络进行管理和防火墙设置
- [ ] 能够安装JDK
- [ ] 能够安装Tomcat
- [ ] 能够安装MySql

# 第一章-Linux介绍

## 知识点-Linux概述 

### 1.目标

​		我们一般在window系统下开发,  开发完成后部署到Linux系统下. 我们先来了解下什么是Linux

### 2.路径

1. Linux简介
2. 为什么要学习Linux
3. Linux的历史     
4. Linux的系统应用

### 3.讲解

#### 3.1.Linux简介

​	是基于Unix的开源免费，多用户，多任务的操作系统，

​	由于Linux系统的稳定性和安全性几乎成为程序代码运行的最佳系统环境。

#### 3.2为什么要学习Linux

对于windows操作系统而言，大家应该不陌生，这里我列举一些windows的不足：

1. 个人用户正版windows需要收费 
2. 系统长时间运行后，不稳定，变慢，容易死机    
3. 且windows经常招到病毒攻击等 

相反，上述windows的不足，恰好是另一款操作系统Linux的优势所在，这里我也列举一些Linux的优点：

1. 个人用户正版Linux不需要收费

2. 系统长时间运行后，还是比较稳定，比较快，不容易死机

3. 且Linux不常招到病毒攻击等

   

  做为一个后端JavaEE程序员，通常在windows/MAC中开发完程序后，得部署到一个相对比较安全，稳定的服务器中运行，这台服务器上安装的不是windows操作系统，而是Linux操作系统。

   

#### 3.3.Linux的历史              

​	Linux最初是由芬兰赫尔辛基大学学生Linus Torvalds由于自己不满意教学中使用的MINIX操作系统， 所以在1990年底由于个人爱好设计出了LINUX系统核心。后来发布于芬兰最大的ftp服务器上，用户可以免费下载，所以它的周边的程序越来越多，Linux本身也逐渐发展壮大起来，之后Linux在不到三年的时间里成为了一个功能完善，稳定可靠的操作系统.

![img](img/tu_2.png)![img](img/tu_3.png)

#### 3.4.Linux系统的应用

- 服务器系统   

  ​	Web应用服务器(eg: tomcat)、数据库服务器(eg: mysql)、接口服务器、DNS、FTP等等； 

- 嵌入式系统  

  ​	路由器、防火墙、手机(eg: 安卓)、PDA、IP 分享器、交换器、家电用品的微电脑控制器等等，

- 高性能运算、计算密集型应用

  ​	Linux有强大的运算能力。

- 桌面应用系统    

- 移动手持系统  

### 4.小结

1. Linux就是一个操作系统
2. 稳定，不易受到病毒攻击，开源的



## 知识点-Linux的版本

### 1.目标

​	知道内核版本和发行版本区别, 以及常见的发行版本

### 2.路径

1. Linux的版本
2. Linux的主流发行版本

### 3.讲解

#### 3.1Linux的版本

Linux的版本分为两种：内核版本和发行版本；

- 内核版本是指在Linus领导下的内核小组开发维护的系统内核的版本号 ；
- 发行版本是一些组织和公司根据自己发行版的不同而自定的 

#### 3.2Linux常见发行版本



![img](img/tu_4.png)

### 4.小结

1. Linux版本
   1. 内核版本
   2. 发行版本（用户使用的版本）
2. 我们学发行版本：centOS





# 第二章-Linux的安装

## 实操-Linux的安装

### 1.目标

- [ ] 能够独立搭建Linux环境

### 2.路径

1. 虚拟机的安装  
2. CentOS的安装
3. Linux的目录结构

### 3.讲解

#### 3.1虚拟机的安装

​	虚拟机：一台虚拟的电脑.  独立的  

​	虚拟机软件:

​		 VmWare:收费的.

​		 VirtualBox:免费的. Oracle的产品 . 

​	==参考《01.VMware使用》==

==**重点：**==

1. 安装VMware虚拟机教训
	1. 取消勾选【启动时检查产品更新】
	2. 取消勾选【用户体验改进计划】
	3. 输入秘钥： 5G65R-43K8J-8Z658-UHCE2-22M6G

![1597971376791](img/1597971376791.png)

#### 3.2 CentOS的安装

​	参考《02.Linux(CentOS)安装.pdf》

​	密码建议设置为 6个1： 111111。

​	鼠标从虚拟机跳出来快捷键：  Ctrl+Alt。

注意事项：

![1597972594843](img/1597972594843.png)

![1597972648643](img/1597972648643.png)

![1597972697402](img/1597972697402.png)

#### 3.3.Linux的目录结构



![img](img/tu_5.png)

​	Linux不像windows那样有盘符的概念，它的最高目录为根目录（用/表示）

​	/:系统根目录  

​		==root目录==：超级管理员root所在的目录，用~表示      

​		==home目录==：普通用户所在的目录 

​		==etc==:配置文件

​		==usr==:存放共享文件的  (装的软件基本都是在usr目录)

### 4.小结

1. 安装时候注意事项
   - 内存    4G(512M)  8G(1G)  16G(1~2G) 
   - 需要配置自动连接

![1559115635987](img/1559115635987.png) 

2. 目录结构
   - 以根目录/作为树形文件系统
     - root目录： root账号所在的家目录
     - home目录：普通用户家目录
     - etc目录： 配置文件
     - usr目录：共享目录，安装我们的软件

## 实操-远程访问的软件 CRT

### 1.目标

- [ ] 能够安装Secure CRT 

### 2.路径

1. CRT 安装
2. CRT 破解
3. 使用CRT连接Linux

### 3.讲解

#### 3.1.CRT 安装

​	参考《03.CRT连接linux.pdf》

#### 3.2.破解CRT软件

​	参考《03.CRT连接linux.pdf》

#### 3.3.使用CRT连接Linux

​	参考《03.CRT连接linux.pdf》

```
ifconfig  #查看ip地址
# 如果ifconfig不可用，则需要安装 net-tools
#  yum install -y net-tools    #安装 net-tools

```



### 4.小结

1. 使用CRT软件连接的时候, 需要输入的是: 服务器(虚拟机)的ip地址, 不是localhost. CentOS的默认端口号是==22==



# 第三章- Linux的常用命令

## 知识点-磁盘管理

### 1.目标

- [ ] 掌握磁盘管理相关命令

### 2.路径

1. 切换目录命令
2. 列出文件列表
3. 创建目录和移除目录

### 3.讲解

#### 3.1切换目录命令

​	**cd app	切换到app目录**

​	cd ..         切换到上一层目录

​	**cd /          切换到系统根目录**     

​	**cd ~    	 切换到用户主目录(回家)  在任何目录都可以,如果当前是root用户, 切换到了root目录**

​	cd -           切换到上一个所在目录(上一个操作的)

==**pwd**==：查看当前处于哪个目录；

#### 3.2 列出文件列表

​	ls(list)是一个非常有用的命令，用来显示当前目录下的内容。配合参数的使用，能以不同的方式显示目录内容。

- 格式

  ```
  ls[参数] [路径或文件名]
  ```

  ls		展示当前目录下资源（不包含隐藏的文件）

  **ls -a** 	显示所有文件或目录（包含隐藏的文件）, 文件带点的是隐藏文件

  ls -l  	展示文件的详细信息,  ==**简写成 ll**==  (1TB=1024G 1G=1024MB  1MB = 1024KB  1KB = 1024Byte)

  **ll -h**		友好显示文件大小

  ls -al	展示文件的详细信息（包含隐藏的文件）

#### 3.3.创建目录和移除目录

##### 3. 3.1 mkdir【掌握】

- 用来创建子目录.

  ```
  mkdir app 在当前目录下创建app目录
  
  mkdir –p app3/test  级联创建app3以及test目录
  ```

##### 3.3.2rmdir【了解】

- 用来删除“空”的子目录

  ```
  rmdir app   删除app目录
  rmdir -p app3/test  可以级联删除test、app3目录
  ```

### 4.小结

1. 切换目录： cd
   1. cd /  切换到根目录
   2. cd /root  切换到/root目录
   3. cd ~  回到当前用户主目录
   4. cd ../  回到上一级
   5. 查看当前所在目录： pwd
2. 文件列出: ls
   1. 查看当前目录的内容： ls
   2. 查看/root的内容：  ls /root
   3. 查看详细信息：  ls -l，简写为ll
   4. 友好查看： ll -h
   5. 查看所有（包含隐藏目录）： ls -a
3. 目录操作
   1. 创建目录： mkdir 目录名
   2. 删除目录： rmdir 目录名





## 知识点-文件浏览

### 1.目标

- [ ] 掌握文件浏览命令

### 2.路径

1. cat【掌握】
2. more【掌握】
3. less
4. tail

### 3.讲解

#### 3.1cat【掌握】

- 用于显示文件的内容, 格式：cat[参数]<文件名>

  ```
  cat 文件名   查看文件的所有的内容	
  ```

#### 3.2more【掌握】

- 分页查看。==回车换行、空格换页==。按 q 键退出查看。(Ctrl+C退出查看)

  ```
  more 文件名 
  示例： cd /root
  more install.log
  q退出
  ```

#### 3.3less

- 用法和more类似，不同的是less可以通过==PgUp、PgDn==键来控制。

  ```
  less 文件名 
  ```

#### 3.4tail

tail命令是在实际使用过程中使用非常多的一个命令，它的功能是：用于显示文件后几行的内容。

- tail -n 文件名:查看文件的末尾几行, n是数字，表示行

  ```
  tail -10 /etc/passwd
  ```

- **tail -f 文件名:滚动的查看文件. **【掌握】

  一般用作查看tomcat的日志

  ```
  示例： cd /root
  tail -f install.log
  ```

  

补充：

==clear==：清屏命令

==ctrl+c== ：强制结束命令

### 4.小结

1. cat： cat 文件名 查看所有内容
2. more： more 文件名； 分页查看，回车换行，空格换页。
3. less：less 文件名； 分页查看，pageUP、pageDown
4. tail： 查看文件末尾内容
   1. tail -n 文件名： 查看文件的最后n行内容
   2. tail -f 文件名： 动态输出文件的新增内容（滚动输出）



## 知识点-文件操作

### 1.目标

- [ ] 掌握文件操作命令

### 2.路径

1. touch
2. mv
3. cp
4. rm

### 3.讲解

#### 3.1touch创建一个空文件

- **touch 文件名1 文件名2 文件名3**

  ```
  touch a.txt
  touch b.txt c.txt  #创建2个文件
  ```

#### 3.2mv 移动文件

==mv  源文件(目录) 目标文件(目录)==

- **mv 文件 目录:移动到指定目录**:   mv a.txt /usr  表示将的当前目录的a.txt文件移动到  /usr 目录
- `mv 文件 目录/文件名`:移动到指定目录且重命名
- `mv 目录 指定的目录`:移动一个目录到指定的目录下
- `mv 文件名 新文件名`:重命名

#### 3.3cp 拷贝文件        

- **cp 文件 目录:把一个文件复制到某目录下**
- cp 文件 目录/文件名:复制且重命名
- cp 文件 新文件名 :当前目录下复制一个
- cp -r 目录 新目录:递归复制目录(复制非空目录)

#### 3.4rm删除文件     

- rm  文件;	 询问删除文件

  ```
  rm a.txt  删除a.txt文件
  
  示例：
  [root@sz-local3 ~]# rm d.txt 
  rm：是否删除普通空文件 "d.txt"？y
  ```

- rm -f 文件;不询问，直接删除

  ```
  rm -f a.txt  不询问，直接删除a.txt 
  ```

- rm -r 目录; 删除目录(递归删除)

  ```
  rm -r app3; 递归删除a目录
  
  示例：
  [root@sz-local3 ~]# rm app3
  rm: 无法删除"app3": 是一个目录
  
  [root@sz-local3 ~]# rm -r app3
  rm：是否进入目录"app3"? y
  rm：是否删除目录 "app3/test"？y
  rm：是否删除目录 "app3"？y
  ```

- rm -rf 目录; 不询问递归删除（慎用)  

  ```
  rm -rf  a  不询问递归删除  
  rm -rf *   删除当前目录下所有文件
  rm -rf /*  自杀	*********(不要用)    
  ```

### 4.小结

1. 创建文件 ： touch 文件名
2. 移动： mv
   1. mv 文件  目录：  移动文件到指定目录
   2. mv 文件 目录/文件： 移动文件到指定目录并且重命名
3. 复制 ：  cp
   1. cp 文件 目录： 复制文件到指定目录
   2. cp 文件 目录/文件名：复制文件到指定目录并且重命名
   3. cp 文件 文件名：在当前目录复制并且重命名
4. 删除: rm
   1. rm -rf 文件|目录（谨慎操作）



## 知识点-文件编辑【重点】

### 1.目标

- [ ] 掌握文件编辑命令

### 2.路径

1. vi命令
2. 命令练习

### 3.讲解    

#### 3.1vi编辑   

- 打开文件：vi 文件名  ,处在命令模式  ; 

  ```
  命令模式------(i)----->编辑模式-----(Esc)----->  命令模式-----(:)----->  底行模式
  ```

- 退出：esc->:q

- 修改文件：输入i进入插入模式

  保存并退出：先输入esc(切换到命令模式), 在输入:(切换到底行模式), 最后输入  wq

  不保存退出：先输入esc(切换到命令模式), 在输入:(切换到底行模式), 最后输入  q

  强制保存并退出：  wq!

- vi的模式

  命令模式:对行进行操作 移动光标.  切换到命令行模式：按Esc键

  ​	命令模式常用的快捷键

  ​	yy:复制当前行

  ​	p:粘贴

  ​	dd:删除当前行		

  编辑模式:对具体的字符进行操作. 切换到插入模式：按 i键

  底行模式:退出. 切换到底行模式：按 :（冒号) .  注意:要从命令模式切换,不能从编辑模式切换到底行模式

  ​	:wq  保存并退出

  ​	:q	 退出(不保存)

  ​	:q!  强制退出(不保存)

  ![](img/1575613861166.png)

#### 3.2练习

```
在root目录下,创建一个app的目录:   
	cd ~	回到root目录
	mkdir app	创建app目录
在app目录下创建一个a.txt
	cd app  进入app目录
	touch a.txt  创建a.txt文件


编辑a.txt, 编写的内容是: hello world... 
	vi a.txt 进入文件，此时处于命令模式
	按住i键	进入编辑模式，输入hello world...
	先保存并退出  按住Esc进入命令模式
	再输入:wq  
	

    

复制2行hello world..
	vi a.txt 进入文件，此时处于命令模式
	在命令模式下 使用 yy进行复制该行
	在使用p进行粘贴
	再按住  :wq
	


再删除最后一行hello world
	vi a.txt  进入文件，光标移动最后一行按dd

再删除app目录
	rm -rf app

```



## 知识点-解压和压缩

### 1.目标

- [ ] 掌握解压和压缩

### 2.路径

1. 打包压缩
2. 解压

### 3.讲解

打包: 将多个文件打包成一个特定文件, 文件扩展名一般是.tar  

压缩: 将多个文件打包成一个特定文件并且做压缩处理, 文件扩展名一般是.gz 

打包并压缩的文件名： .tar.gz文件

#### 3.1打包压缩【tar -zcvf】

- 语法：==tar  -zcvf   打包并压缩后的文件名   要打包压缩的文件/目录==
  - -z调用压缩命令进行压缩, 没有加上-z就是打包（可选项）
  - -c 创建新的文件（必选项）
  - -v 输出文件清单（可选项）
  - -f 文件名由命令台设置（必选项)
- 练习: 把app文件夹进行压缩

```shell
tar -zcvf app.gz app
```



#### 3.2解压【tar -xvf】 【重点】

- 语法

  - tar -xvf 压缩文件名;          									 解压到当前目录
  - ==tar -xvf 压缩文件名 -C /usr/local==                         解压到/usr/local目录
  - 参数含义
    - -x  取出文件中内容
    - -v  输出文件清单
    - -f  文件名由命令台设置

- 练习

  - 解压app.gz

  ```
  tar -xvf app.gz
  ```

  - 解压app.gz 到 /usr/home目录

  ```
  tar -xvf app.gz -C /usr/home
  ```

### 4.小结

1. 压缩命令
   1. tar -zcvf 压缩后的文件名 目标文件
2. 解压命令【重要】
   1. tar -xvf 压缩文件名
   2. tar -xvf 压缩文件名 -C 目标目录





## 知识点-权限命令(chmod 命令)【了解】

### 1.目标

- [ ] 了解权限命令

### 2.路径

1. 权限 命令介绍
2. 修改权限

### 3.讲解      

#### 3.1权限 命令介绍

​	Linux中对每个目录和文件都做了规定，只能由满足条件的用户才能操作，这个规定叫权限。



![img](img/tu_6.png)

==权限对root无效的,这些权限由root授予.==

属主权限：  当前用户（主人）的权限；

属组权限：当前用户所在的同一组的其他成员的权限；

其他用户权限：和当前用户不在同一组的其他用户的权限。

班主任

班主任同一组的同事

其他同事



通过`ll` 命令可以看到文件的详细信息:  

![1575621238766](img/1575621238766.png)

​	第1位:文件类型   - 文件, d是目录,l:快捷方式

==每一个权限操作都可以用一个数字的表示==：

| 权限操作        | 值（数字） |
| --------------- | ---------- |
| r: 可读         | 4          |
| w: 可写         | 2          |
| x: 可操作、执行 | 1          |

​	0  没有权限

​	1	可操作权限

​	2	可写

​	3	可写可操作

​	4	可读

​	5	可读、可操作

​	6	可读、可写

​	7	所有权限

每一个组的权限都可以使用一个数字的值表示，所以三个组可以使用3个数字表示：

比如 777 表示 属主、属组、其他用户组 都可以读、写、执行。

543:  属主可读可操作、属组可读、其他用户可写可操作性

#### 3.2修改权限               

eg: 	==chmod 777 文件名==:让所有的用户对该文件可读可写可操作

​	chmod 000 文件:取消所有用户的所有权限；对root用户不起作用。

​	chmod 456文件: 当前用户可读, 当前组里面其它成员是可读可操作,其它用户可读可写

### 4.小结

1. 修改权限的语法：  chmode 3个数字代表属主、属组、其他用户  文件



## 知识点-其它常用命令

### 1.目标

- [ ] 掌握其它常用命令

### 2.路径

1. 显示当前目录的绝对路径
2. 关机/重启
3. 查看网卡信息
4. 查看进程
5. 杀死进程
6. 管道

### 3.讲解

#### 3.1显示当前目录的绝对路径【重点】

```
pwd 
```

#### 3.2 关机/重启

```
halt:关机
reboot:重启
```

#### 3.3 查看网卡信息【掌握】

```
ifconfig:查看当前网卡信息
```

示例(对于开发人员而言，一般主要是查看一下ip)：

```bash
[root@szlocal1 ~]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.75.130  netmask 255.255.255.0  broadcast 192.168.75.255
        inet6 fe80::669a:2b9e:9d0c:184a  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:c9:b9:8c  txqueuelen 1000  (Ethernet)
        RX packets 34640  bytes 15030781 (14.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8721  bytes 1639356 (1.5 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1  bytes 76 (76.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1  bytes 76 (76.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

- 第一行
	- UP--》网卡开启状态
	- RUNNING--》网线处理连接状态
	- MULTICAST--》支持组播
	- mtu 1500--》最大传输单元1500字节
- 第二行： 该网卡的ip地址、子网掩码、广播地址
- 第三行： ipv6的配置信息
- 第四行： 网卡的MAC地址
	- ether 表示连接类型为以太网
	- txqueuelen 1000 --》传输队列的长度
- 第五六行：网卡接受数据包的统计信息和接受错误的统计信息
- 第七八行：网卡发送数据包的统计信息和发送错误的统计信息

```

==clear== 清除当前屏幕的输出内容

#### 3.4查看进程【掌握】

```
ps -ef :查看所有进程
```

#### 3.5杀死进程【掌握】

```
kill -9 进程号(pid):杀死指定的进程	 
```

#### 3.6 管道【掌握】

```
命令1  | 命令2  解释：一个命令的输出，可以作为另一个命令的输入，至少有二个命令参与执行。 常用的需要使用管道命令有					  more，grep。	

eg: ps -ef | grep vi  #在所有的进程里面筛选出和vi相关的进程

```

注: ==grep 筛选指定的内容==,grep -i:忽略大小写

​	

​		ps -ef | grep vi:  查看所有vi的进程

![img](img/tu_7.png)

### 4.小结

1. 查看当前所属目录：  pwd
2. 查看网卡、ip：  ifconfig
3. 关机、重启： halt、reboot
4. 进程查看： ps -ef
5. 杀死目标进程： kill -9  进程id号pid
6. 管道 过滤：  ps -ef | grep sbin： 查看到包含sbin的所有进程

# 第四章-Linux网络管理和防火墙设置

## 知识点-Linux网络管理

### 1.目标

- [ ] 掌握Linux网络管理

### 2.路径

1. ifconfig命令
2. 启动和停止
3. 网络模型
4. 配置NAT网络模型

### 3.讲解

#### 3.1ifconfig命令

```
ifconfig   #查看与配置网络状态
```

#### 3.2启动和停止

+ 启动 

```
service network start
```

+ 停止

```
 service network stop
```

+ 重启

```
 service network restart
```

#### 3.3网络模型

+ 桥接模式

  ​	物理主机就好像一个交换机，将物理主机和虚拟机连接在一个局域网内。和主机的关系就像局域网中一台独立的主机，和主机同等地位。获取外网ip进行上网。物理机上有一个自身的网卡，虚拟机虚拟一个虚拟网卡，两者可以连接到外网。桥接模式下虚拟机和主机不仅应该处于同一网段，而且相关DNS和网关都应该相同。

+ NAT

  ​	俗称网络地址转换，是将内部私有网络地址翻译成合法网络地址。物理机上有一个自身的网卡，和虚拟NAT设备直连，还有一个虚拟网卡直接连在虚拟交换机上。虚拟NAT设备与虚拟DHCP服务器直接连在虚拟交换机上，虚拟机通过虚拟交换机和NAT设备实现联网，但是和本机的连接是通过物理主机的虚拟网卡vm adapter8来实现的。虚拟机向外部网络发送的请求数据"包裹"，都会交由NAT网络适配器加上"特殊标记"并以主机的名义转发出去，外部网络返回的响应数据"包裹"，也是先由主机接收，然后交由NAT网络适配器根据"特殊标记"进行识别并转发给对应的虚拟机，因此，虚拟机在外部网络中不必具有自己的IP地址。==从外部网络来看，虚拟机和主机在共享一个IP地址==。

+ 仅主机

  仅主机模式即是nat模式去除 了nat设备，虚拟机是一个独立的系统，只能实现虚拟机和主机间的通信，如果虚拟机需要联网的话，还需要主机共享网卡。

#### 3.4配置NAT网络模型（不要去看先！！）

+ 选择编辑>虚拟网络编辑器

![1567923487150](img/1567923487150.png) 

+ 选择NAT 模式的VMnet8

![1567923539652](img/1567923539652.png)  

+ 将其中的“网关IP”设置为“192.168.33.1”

![1567923631614](img/1567923631614.png)  

+ 然后在本机的“网络连接”中对VMnet8进行设置

![1567923666103](img/1567923666103.png) 



![1567923679561](img/1567923679561.png) 

### 4.小结

- 查看网卡信息  ifconfig
- 开启、关闭网络：
  - service network start
  - service network stop
  - service network restart
- NAT： 主机、虚拟机共享主机的ip地址访问公网

## 知识点-Linux防火墙设置

### 1.目标

- [ ] 掌握防火墙设置

### 2.路径

1. 什么是防火墙
2. 查看防火墙状态
3. 关闭防火墙
4. 禁用防火墙
5. 设置端口防火墙放行

### 3.讲解

#### 3.1什么是防火墙

​	防火墙技术是通过有机结合各类用于安全管理与筛选的软件和硬件设备，帮助计算机网络于其内、外网之间构建一道相对隔绝的保护屏障，以保护用户资料与信息安全性的一种技术。

​	在默认情况下，Linux系统的防火墙状态是打开的，已经启动。

#### 3.2查看防火墙状态

+ 语法：

```
service  服务     status
```

+ 命令：

```
service  iptables   status
```

#### 3.3关闭防火墙

+ 语法：

```
service  服务     stop
```

+  命令:

```
service  iptables   stop
```

#### 3.4禁用防火墙自启

+ 语法

```
chkconfig    服务     off
```

+ 命令

```
chkconfig    iptables   off
```

#### 3.5设置端口防火墙放行【重要！！！】

+ 修改配置文件

```
vi /etc/sysconfig/iptables
```

+ 添加放行的端口

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 端口号 -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```

> 备注: 默认情况下22端口号是放行的

### 4.小结

- 端口放行设置：
  - vi /etc/sysconfig/iptables
  - -A INPUT -m state --state NEW -m tcp -p tcp --dport 端口号 -j ACCEPT

### 扩展（不做要求）： 高版本centos的防火墙放行设置

```shell
[学一招]CentOS7系统防火墙设置:
	firewall-cmd --zone=public --add-port=8080/tcp --permanent
	systemctl restart firewalld.service
	

说明：	
	--zone=public：表示作用域为公共的；

	--add-port=8080/tcp：添加tcp协议的端口8080；

	--permanent：永久生效，如果没有此参数，则只能维持当前服务生命周期内，重新启动后失效；
```



​	

# 第五章-软件安装

## 知识点-rpm软件包管理器

### 1.目标

- [ ] 掌握rpm常见的命令

### 2.路径

1. rpm介绍
2. rpm常见的命令

### 3.讲解

#### 3.1rpm介绍

​	一种用于互联网下载包的打包及安装工具，它包含在某些Linux(CentOs)分发版中。

#### 3.2rpm常见的命令

- rpm -qa : 查询所有安装过的软件包
- rpm -e --nodeps  ：安装过的软件包名: 删除指定的安装包 ； 加上 --nodeps 表示强制删除，忽略被其他包依赖的关系。
- rpm -ivh  包名 : 安装rpm包；  
  - i 表示安装；  
  - v  表示输出信息； 
  - h 表示hash校验即会在安装过程中输出 一串的符号:     `##########`

### 4.小结

rpm ：  就是一个包管理器，安装。

### 扩展： `!$`命令

`!$` 表示获取==上一条命令的最后一个参数==

- 示例：

  ```shell
  #前一条命令
  mv /root/apache-tomcat-8.5.32.tar.gz /usr/local/tomcat
  # 使用 !$   ，则等效于  cd /usr/local/tomcat
  cd !$
  ```

  - 场景： 如果用于前面命令的参数表较长的时候，后面的命令又要用到时，则可以很方便



## 实操-安装JDK

### 1.目标

- [ ] 掌握jdk的安装

### 2.讲解

1. 下载jdk 

   http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2. 从windows上传到linux

   在工具Secure CRT下, 快捷键==Alt+P 会打开一个sftp传输窗口，直接将windows的文件拖拽进去即可完成上传了。==

   - sftp一些基本语法【有兴趣也可以了解下，重点不在这里~】：

     ```
     sftp一些基本语法：
     登录远程主机：  sftp username@remote_hostname_or_IP
     查询帮助手册：	 help
     在命令前面加一个！表示命令在本地主机上执行：   
                     //在远程主机上执行
                     vim test.sh
                     //在本地主机上执行
                     !vim test.sh
     从远程主机下载文件:
     				//下载到本机主机当前目录，并且文件名与remoteFile相同
                     get remoteFile
     
                     //下载到本机主机当前目录，并且文件名改为localFile
                     get remoteFile localFile
     从远程主机下载一个目录及其内容:
     				get -r someDirectory
     				
     				
     上传文件到远程主机的当前目录：
     				put localFile
     上传目录到远程主机的当前目录：
     				put -r localDirectory
     退出sftp：
   				exit
     ```
     
     

3. 检查系统上是否安装了jdk(若安装了就需要先卸载再使用我们自己的)

   ```
   java -version 
   ```

4. 查看出安装的java的软件包

   ```
   rpm -qa | grep java
   ```

5. 卸载linux自带的jdk

   ```
   rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.i686
   rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.i686 tzdata-java-2013g-1.el6.noarch
   ```

6. 在 /usr/local 新建一个文件夹 java

   ```
   mkdir /usr/local/java
   ```

7. 移动 jdk.....gz 到 /usr/local/java下（切换到/root目录执行该命令）

   ```
   mv jdk-8u181-linux-i586.tar.gz /usr/local/java
   ```

8. 进入 /usr/local/java 目录,解压jdk

   ```
   cd /usr/local/java 
   tar -xvf  jdk-8u181-linux-i586.tar.gz
   ```

9. 配置==环境变量==

   ```
   vi /etc/profile
   
   
   export JAVA_HOME=/usr/local/java/jdk1.8.0_181   #填你的目录（你下载的的jdk版本号的目录）
     # Linux环境变量冒号:分隔开
   export PATH=$JAVA_HOME/bin:$PATH  
   ```

10. 保存退出

11. 重新加载配置文件; 否则需要重新连接才生效。

```
    source /etc/profile
```

### 3.小结

老老实实抄

## 实操-安装Tomcat

### 1.目标

- [ ] 掌握Tomcat的安装

### 2.讲解

1. 下载tomcat

2. 上传到linux

   ```shell
   多学一招:
   	在crt上 使用 alt+p 
   	将windows上的软件拖进去即可(此方法会拖到用户的家目录如果是root账户则会到 /root 目录)
   ```

3. 在 /usr/local 新建一个文件夹tomcat

   ```shell
   mkdir /usr/local/tomcat
   ```

4. 移动 tomcat...tar.gz 到 /usr/local/tomcat

   ```shell
   mv apache-tomcat-8.5.32.tar.gz /usr/local/tomcat/
   ```

5. 进入/usr/local/tomcat目录,解压Tomcat

   ```shell
   cd /usr/local/tomcat
   tar -xvf apache-tomcat-8.5.32.tar.gz
   ```

6. 进入 /usr/local/tomcat/apache-tomcat-8.5.32/bin

   ```shell
   cd /usr/local/tomcat/apache-tomcat-8.5.32/bin
   ```

7. 启动tomcat

   ```shell
   方式1:
   	sh startup.sh
   方式2:
   	./startup.sh
   ```

8. 修改防火墙的规则 

   ```bash
   方式1:service iptables stop  关闭防火墙(不建议); 用到哪一个端口号就放行哪一个(80,8080,3306...)
   
   方式2:放行8080 端口
   	修改配置文件
   		vi /etc/sysconfig/iptables
   			复制(yy , p)	
   				-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
   			改成
   				-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
   	重启加载防火墙或者重启防火墙
   			service iptables reload  
   			或者
   			service iptables restart
   
   ```



9. 讲解乱码

   jdbc连接的时候可能出现乱码, 可以在连接的路径后面拼接字符集解决 

```
jdbc:mysql://ip地址:端口/数据库名?characterEncoding=utf-8
```



### 3.小结

套路，老老实实按部就班版，不要出轨。

## 知识点-安装MySql

### 1.目标

- [ ] 掌握MySql的安装

### 2.讲解

1. 下载mysql

2. 上传到linux  ==在CRT下,按Alt+P==：会打开一个sftp传输窗口, ==拖拽进去。==

   1. 或者使用命令，输入 ==`put`== 表示将本地文件上传到远程机器；

   ```shell
   sftp> put D:\softwares\01_linux-softwares\MySQL-5.5.49-1.linux2.6.i386.rpm-bundle.tar
   Uploading MySQL-5.5.49-1.linux2.6.i386.rpm-bundle.tar to /root/MySQL-5.5.49-1.linux2.6.i386.rpm-bundle.tar
     100% 971KB    971KB/s 00:00:00     
   
   ```

   

3. 检查系统上是否安装了mysql( 若安装了就需要先卸载再使用我们自己的)

   ```
   rpm -qa |grep -i mysql                          #忽略大小写查看
   rpm -e --nodeps mysql-libs-5.1.71-1.el6.i686    #卸载
   ```

4. 在 /usr/local 新建一个文件夹mysql

   ```
   mkdir /usr/local/mysql
   ```

5. 把mysql压缩包移动 到/usr/local/mysql

   ```
   mv MySQL-5.5.49-1.linux2.6.i386.rpm-bundle.tar /usr/local/mysql/
   ```

6. 进入 /usr/local/mysql,解包mysql

   ```
   cd /usr/local/mysql
   tar -xvf MySQL-5.5.49-1.linux2.6.i386.rpm-bundle.tar
   ```

7. 安装 服务器端

   ```
   rpm -ivh MySQL-server-5.5.49-1.linux2.6.i386.rpm 
   ```

8. 安装 客户端 

   ```
   rpm -ivh MySQL-client-5.5.49-1.linux2.6.i386.rpm 
   ```

9. 启动Mysql

   ```
   service mysql start  #启动mysql (注意:只启动一次)  
   ```

10. 修改密码   (把用户root的密码改为root)

```
  /usr/bin/mysqladmin -u root password 'root'
```

11. 登录mysql

    ```
     mysql -uroot -proot
    ```

12. 放行3306端口号

```shell
   1.修改配置文件
      	cd /etc/sysconfig
      	vi iptables
      	或者  vi /etc/sysconfig/iptables
      复制(yy  p)	
      	-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
      改成
      	-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
    2.重启加载防火墙或者重启防火墙
      	service iptables reload  
      	或者
      	service iptables restart

```

13. ==允许外部通过远程连接 mysql==,需要进入MySQL进行设置。

```
    在linux上 先登录mysql	
      	cd /usr/local/mysql   #进入mysql目录
      	mysql -uroot -proot    #登录
      	
  1.    创建远程账号(账号为root、密码也被identified设置为root)
      	create user 'root'@'%' identified by 'root';  
  2.    授权
      	grant all on *.* to 'root'@'%' with grant option;
  3.    刷新权限（使得权限及时生效）
      	flush privileges;
      	
      	
  ========================================    
  创建一个zs密码为zs的账户，外界访问：
      	create user 'zs'@'%' identified by 'zs';  
      	grant all on *.* to 'zs'@'%' with grant option;
      	flush privileges;
```



### 3.小结

老实，一步一步走。



# 附录-彩蛋

## 常用快捷键

linux常用操作快捷键：
Tab： 命令补全/路径补全/文件名补全，一次tab是补全，两次tab，列出相关信息。
Ctrl+C： 强制结束当前的进程。干了一半不想干了想反悔就Ctrl+C。
Ctrl+D： 发送一个exit信号，每次当我们有“退出”的意思的时候，就可以使用这个。比如SSH登录到另一个机器，想退出就Ctrl+D。
Ctrl+A： 移动到命令行首。
Ctrl+E： 移动到命令行尾。
Ctrl+U ：从当前光标所在位置向前清除命令。
Ctrl+W： 从当前光标所在位置向前清除一个单词。
上下箭头： 上下翻看命令的输入记录，如果历史记录太多翻起来太慢，就用history显示出来然后再复制粘贴。





## 系统监控命令

### **查看端口占用情况**

查看80端口是否已经打开

```shell
netstat -an | grep -w 80 其中的-w表示按单词匹配，以免匹配到8080端口
```

![1577759738938](img/1577759738938.png)

查看80端口被什么进程占用

```shell
netstat -anp | grep -w 80
```

![1577759758795](img/1577759758795.png)



root用户也无法删除的文件

```shell
[root@byron4j-os6 app]# touch a.txt
[root@byron4j-os6 app]# ll
总用量 16
-rw-r--r--  1 root root     0 1月   2 21:28 a.txt
-rw-r--r--  1 root root 12035 1月   2 18:15 demo.txt
drwxr-xr-x. 3 root root  4096 12月 10 14:18 HMLY

# 给文件添加隐藏属性
[root@byron4j-os6 app]# chattr +a a.txt     
[root@byron4j-os6 app]# rm a.txt 
rm：是否删除普通空文件 "a.txt"？y
rm: 无法删除"a.txt": 不允许的操作
```





# 回顾要点

看视频

- vmware
- linux-centos
- secureCRT



- 目录操作命令
  - 创建： mkdir 目录
  - 删除： rmdir 目录
- 文件操作命令
  - 创建： touch 文件名
  - 移动： mv
    - mv 文件 目录：  把文件移动到指定目录
    - mv 文件 目录/文件名：  把文件移动到指定目录，并且重命名
  - 复制： cp
    - cp 文件 目录：把文件复制到指定目录
    - cp 文件 目录/文件名：把文件复制到指定目录，并且重命名
  - 删除： rm
    - rm -rf 文件/目录：  删除并且不提示
- 压缩、解压缩
  - 压缩：tar -zcvf 压缩后的文件名.gz 原始文件目录
  - ==解压：tar -xvf 压缩文件名 -C  目录==
- 权限操作：  chmod 777 文件目录
- 其他常用命令
  - pwd
  - ifconfig
  - 进程： ps -ef
  - 杀死： kill -9 进程id
  - 管道： pe -ef | grep  -i java
  - 关闭： halt
- cd命令



命令： 每一个敲一下





安装jdk、tomcat、mysql----老老实实



# 总结

- 切换目录： cd
  - cd / : 到根目录
  - cd  ../ ： 上一级目录
  - cd ~  ： 家目录
  - cd - ： 回到上一个操作所在的那个目录
- 创建目录： mkdir 目录名
- 文件操作
  - 创建： touch 文件名
  - 移动： mv
    - mv 文件  目录： 文件移动到指定目录
    - mv 文件  目录/文件： 移动且重命名
  - 复制： cp
    - cp 文件 目录： 文件复制到指定目录
    - cp 文件 目录/文件： 复制且重命名
    - cp 文件  文件名： 在当前目录复制且重命名
  - 删除： rm -rf 文件/目录： 强制删除目录或者文件
- 文件编辑（5遍）
  - vi 文件（命令模式）------》i键（编辑模式）-----》ESC（命令模式）---》:号（底部命令）---》wq
- 解压缩： 
  - 解压： tar -xvf 压缩文件名
  - 压缩： tar -zcvf 压缩后的文件名XX.gz   源文件/目录
- 权限： chmod    777 文件名： 可读可写可执行
- 其他命令：
  - pwd： 查看当前处于哪个目录
  - ifconfig： 查看ip
  - 关机halt、重启reboot
  - 查看所有进程： ps -ef
  - 杀死进程： kill -9 进程id号
  - 管道：   ps -ef | grep  -i  java： 查看和包含ava字眼的一些进程
- 防火墙
  - vi /etc/sysconfig/iptables
  - 找到22，拷贝
  - 生效： service iptables reload

安装jdk、tomcat、mysql----老老实实



# 预习要点

- 安装redis要掌握
- 常用数据类型的操作
- 理解2中持久化的比较
- 在java里面使用redis---Jedis

