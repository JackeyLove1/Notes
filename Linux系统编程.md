# Linux系统编程

[TOC]



## 概述

Linux系统： “所见皆文件”


Linux系统目录：

	bin：存放二进制可执行文件
	
	boot：存放开机启动程序
	
	dev：存放设备文件： 字符设备、块设备
	
	home：存放普通用户
	
	etc：用户信息和系统配置文件 passwd、group
	
	lib：库文件：libc.so.6
	
	root：管理员宿主目录（家目录）
	
	usr：用户资源管理目录

Linux系统文件类型： 7/8 种

	普通文件：-
	
	目录文件：d
	
	字符设备文件：c
	
	块设备文件：b
	
	软连接：l
	
	管道文件：p
	
	套接字：s
	
	未知文件。

软连接：快捷方式

	为保证软连接可以任意搬移， 创建时务必对源文件使用 绝对路径。

硬链接：

	ln file  file.hard
	
	操作系统给每一个文件赋予唯一的 inode，当有相同inode的文件存在时，彼此同步。
	
	删除时，只将硬链接计数减一。减为0时，inode 被释放。

创建用户：

	sudo adduser 新用户名		--- useradd

修改文件所属用户：

	sudo chown 新用户名 待修改文件。
	
	sudo chown wangwu a.c

删除用户：

	sudo deluser 用户名

创建用户组：

	sudo addgroup 新组名

修改文件所属用户组：

	sudo chgrp 新用户组名 待修改文件。
	
	sudo chgrp g88 a.c

删除组：

	sudo delgroup 用户组名


使用chown 一次修改所有者和所属组：

	sudo chown 所有者：所属组  待操作文件。


find命令：找文件

	-type 按文件类型搜索  d/p/s/c/b/l/ f:文件
	
	-name 按文件名搜索
	
		find ./ -name "*file*.jpg"
	
	-maxdepth 指定搜索深度。应作为第一个参数出现。
	
		find ./ -maxdepth 1 -name "*file*.jpg"


	-size 按文件大小搜索. 单位：k、M、G
	
		find /home/itcast -size +20M -size -50M
	
	-atime、mtime、ctime 天  amin、mmin、cmin 分钟。
	
	-exec：将find搜索的结果集执行某一指定命令。
	
		find /usr/ -name '*tmp*' -exec ls -ld {} \;
	
	-ok: 以交互式的方式 将find搜索的结果集执行某一指定命令


	-xargs：将find搜索的结果集执行某一指定命令。  当结果集数量过大时，可以分片映射。
	
		find /usr/ -name '*tmp*' | xargs ls -ld 
	
	-print0：
		find /usr/ -name '*tmp*' -print0 | xargs  -0 ls -ld 


grep命令：找文件内容

	grep -r 'copy' ./ -n
	
		-n参数：:显示行号
	
	ps aux | grep 'cupsd'  -- 检索进程结果集。


软件安装：

	1. 联网
	
	2. 更新软件资源列表到本地。  sudo apt-get update
	
	3. 安装 sudo apt-get install 软件名
	
	4. 卸载	sudo apt-get remove 软件名
	
	5. 使用软件包（.deb） 安装：	sudo dpkg -i 安装包名。

tar压缩：

	1. tar -zcvf 要生成的压缩包名	压缩材料。
	
		tar zcvf  test.tar.gz  file1 dir2   使用 gzip方式压缩。
	
		tar jcvf  test.tar.gz  file1 dir2   使用 bzip2方式压缩。

tar解压：

	将 压缩命令中的 c --> x
	
		tar zxvf  test.tar.gz   使用 gzip方式解压缩。
	
		tar jxvf  test.tar.gz   使用 bzip2方式解压缩。

rar压缩：

	rar a -r  压缩包名（带.rar后缀） 压缩材料。
	
		rar a -r testrar.rar	stdio.h test2.mp3

rar解压：

	unrar x 压缩包名（带.rar后缀）

zip压缩：

	zip -r 压缩包名（带.zip后缀） 压缩材料。
	
		zip -r testzip.zip dir stdio.h test2.mp3

zip解压：

	unzip 压缩包名（带.zip后缀） 
	
		unzip  testzip.zip 

### **七种文件类型**

**普通文件类型**
Linux中最多的一种文件类型, 包括 纯文本文件(ASCII)；二进制文件(binary)；数据格式的文件(data);各种压缩文件.第一个属性为 [-]
**目录文件**
就是目录， 能用 # cd 命令进入的。第一个属性为 [d]，例如 [drwxrwxrwx]
**块设备文件**
块设备文件 ： 就是存储数据以供系统存取的接口设备，简单而言就是硬盘。例如一号硬盘的代码是 /dev/hda1等文件。第一个属性为 [b]
**字符设备**
字符设备文件：即串行端口的接口设备，例如键盘、鼠标等等。第一个属性为 [c]
**套接字文件**
这类文件通常用在网络数据连接。可以启动一个程序来监听客户端的要求，客户端就可以通过套接字来进行数据通信。第一个属性为 [s]，最常在 /var/run目录中看到这种文件类型
**管道文件**
FIFO也是一种特殊的文件类型，它主要的目的是，解决多个程序同时存取一个文件所造成的错误。FIFO是first-in-first-out(先进先出)的缩写。第一个属性为 [p]
**链接文件**
类似Windows下面的快捷方式。第一个属性为 [l]，例如 [lrwxrwxrwx]

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwMjAxMjIwNjAzNTk4?x-oss-process=image/format,png)

544365 -rw-r–r--. 1 root root 3 Jan 28 20:55 a.txt

**inode 索引节点编号**：544365
**文件类型** ：文件类型是’-’,表示这是一个普通文件
**文件权限**：rw-r–r-- 表示文件可读、可写、可执行，文件所归属的用户组可读可执行，其他用户可读可执行
**硬链接个数** 表示a.txt这个文件没有其他的硬链接，因为连接数是1，就是他本身
**文件属主** 表示这个文件所属的用户，这里的意思是a.txt文件被root用户拥有，是第一个root
**文件属组** 表示这个文件所属的用户组，这里表示a.txt文件属于root用户组，是第二个root
**文件大小** 文件大小是3个字节
**文件修改时间** 这里的时间是该文件最后被更新（包括文件创建、内容更新、文件名更新等）的时间可用如下命令查看文件的修改、访问、创建时间

### cp

>  cp **-a** file1 file2 (-a表示全部拷贝)

### 查看文件内容

* cat file
* more file(空格翻页)
* less file



### linux链接

Linux链接分两种，一种被称为硬链接（Hard Link），另一种被称为符号链接（Symbolic Link）。默认情况下，ln命令产生硬链接。

**【硬连接】**
**硬连接指通过索引节点来进行连接。**在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在Linux中，多个文件名指向同一索引节点是存在的。一般这种连接就是硬连接。硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。

**【软连接】**
另外一种连接称之为符号连接（Symbolic Link），也叫软连接。**软链接文件有类似于Windows的快捷方式。**它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。

> [oracle@Linux]$ touch f1     #创建一个测试文件f1
> [oracle@Linux]$ ln f1 f2     #创建f1的一个硬连接文件f2
> [oracle@Linux]$ ln -s f1 f3    #创建f1的一个符号连接文件f3
> [oracle@Linux]$ ls -li      # -i参数显示文件的inode节点信息
> total 0
> 9797648 -rw-r--r-- 2 oracle oinstall 0 Apr 21 08:11 f1
> 9797648 -rw-r--r-- 2 oracle oinstall 0 Apr 21 08:11 f2
> 9797649 lrwxrwxrwx 1 oracle oinstall 2 Apr 21 08:11 f3 -> f1
>
> 从上面的结果中可以看出，硬连接文件f2与原文件f1的inode节点相同，均为9797648，然而符号连接文件的inode节点不同。
>
> [oracle@Linux]$ echo "I am f1 file" >>f1
> [oracle@Linux]$ cat f1
> I am f1 file
> [oracle@Linux]$ cat f2
> I am f1 file
> [oracle@Linux]$ cat f3
> I am f1 file
> [oracle@Linux]$ rm -f f1
> [oracle@Linux]$ cat f2
> I am f1 file
> [oracle@Linux]$ cat f3
> cat: f3: No such file or directory
>
> 通过上面的测试可以看出：当删除原始文件f1后，硬连接f2不受影响，但是符号连接f1文件无效

### 修改权限

> u:user g:group o :others

加权限+ 减权限-

x执行r读w写

> chmod u+x file

**数字表示法x4w2r1求和的方式**

> chmod 471 file

### 添加用户/组

> sudo adduser user1
>
> **修改所有者** (sudo) chown user1 file1
>
> **切换用户** su user1
>
> **创建用户组** sudo addgroup g1
>
> **修改组** sudo chgrp g1 file1
>
> 一句话创建所有 **sudo chown user1:group1 file1**
>
> **删除用户** sudo deluser user1
>
> **删除组** sudo delgroup g1 

### 查找

> find ./ -maxdepth 1 -name '*.jpg'

### 后台进程

> ps aux 查看后台进程

### gcc编译

	4步骤： 预处理、编译、汇编、连接。
	
	-I：	指定头文件所在目录位置。
	
	-c：	只做预处理、编译、汇编。得到 二进制 文件！！！
	
	-g：	编译时添加调试语句。 主要支持 gdb 调试。
	
	-Wall： 显示所有警告信息。
	
	-D：	向程序中“动态”注册宏定义。   #define NAME VALUE

1. **预处理**：gcc -E 展开宏头文件， 删除注释空白行 hello.i

2. **编译** gcc -S 检查语法规范， 消耗时间和系统资源最多 hello.s

3. **汇编** gcc -c 将汇编指令翻译成机器指令 hello.o

4. **链接** 数据段合并，地址回填 a.out

   ### 常用编译命令选项 

   **1. 无选项**

   用法：#gcc test.c

   作用：将test.c预处理、汇编、编译并链接形成可执行文件。

   这里未指定输出文件，默认输出为a.out。

   **2. 选项 -o**

   用法：#gcc test.c -o test

   作用：将test.c预处理、汇编、编译并链接形成可执行文件test。

   -o选项用来指定输出文件的文件名。

   **3. 选项 –E  预处理指定的源文件，不进行编译。将C语言源文件进行预处理，但是并不编译该程序。对于一般的预处理问题，可以使用这个选项进行查看，例如，宏的展开问题、文件的包含问题等。**

   用法：#gcc -E test.c -o test.i

   作用：将test.c预处理输出test.i文件。

   **4. 选项 –S  将C语言源文件编译为汇编语言，但是并不汇编该程序。**

   用法：#gcc -S test.i

   作用：将预处理输出文件test.i汇编成test.s文件。

   **5. 选项 –c  编译、汇编指定的源文件，但是不进行链接**

   用法：#gcc -c test.s

   作用：将汇编输出文件test.s编译输出test.o文件。

   **6. 选项 -o**

   用法：#gcc test.o -o test

   作用：将编译输出文件test.o链接成**最终可执行文件test**。

   **7. 选项-O**

   用法：#gcc -O1 test.c -o test

   作用：使用编译优化级别1编译程序。级别为1~3，级别越大优化效果越好，但编译时间越长。

   **8. 选项-I directory**

   用法：指定 include 包含文件的搜索目录, **指定头文件所在目录的位置**

   **9 选项-g 生成调试信息，该程序可以被调试器调试**

   用法： #gcc hello.c -o hello -g 

   #gdb hello

   **10. 选项 –L directory 指定库文件目录**

### gdb使用

**1.启动gdb**

gdb filename ---- 本例中是gdb test，如下图:

或者

(gdb)file filename，如下图:

**2.退出**

(gdb)quit

**3.基本操作**

列出源代码list，在提示符下打入list，会出现一部分源代码，接着按回车会重复上一次命令

可以利用help list查询list的使用方法

* list 10 -- 以第10行为中心显示

* 显示compute函数 list compute

* 列出10-15行的源代码 list 10,15

* 列出其他文件的相应行或函数

  list gdbinc.h:1

  list gdbinc.h:max

**4.运行程序run**

如果需要参数可以在run后面跟上参数

**5.设置断点break**

* 在某行设置断点 break 7

* 在某函数设置断点 break compute

* 在其他文件设置断点(行或函数名) break gdbinc.h:2 break gdbinc.h:max

* 在某个地址设置断点 break *address (当你调试的程序没有源程序时使用)

* 查询断点信息info break

* 条件断点 break or if condition

  如:break 8 if a == 10

* 开启和关闭断点

  disable 断点号 (关闭)

  enable 断点号 (开启)

  enable once 断点号 (开启一次)

  enable delete 断点号(开启一次后删除)

* 删除断点

  delete 断点号

  clear 清除当前行的断点

* 继续执行continue，当执行到某处中断时，使其继续执行

**单步执行不进入函数next**

**单步执行进入函数step**

**终止正在调试的程序kill**

**监视值变动watch expression(当你运行run后，你想知道哪些值在运行中被改变了，可以设置此)**

**监视值被读rwatch expression(基本同上)**

**在运行时打印变量的值print expression**

​	print/F expression,其中F为格式(x--16进制,d--有符号十进制,u--无符号十进制,f--浮点格式)



#### 静态库制作及使用步骤：

	1. 将 .c 生成 .o 文件
	
		gcc -c add.c -o add.o
	
	2. 使用 ar 工具制作静态库
	
		ar rcs  lib库名.a  add.o sub.o div.o
	
	3. 编译静态库到可执行文件中：
	
		gcc test.c lib库名.a -o a.out

#### 头文件守卫：防止头文件被重复包含

	#ifndef _HEAD_H_
	
	#define _HEAD_H_
	
	......
	
	#endif

#### 动态库制作及使用：

	1.  将 .c 生成 .o 文件， （生成与位置无关的代码 -fPIC）
	
		gcc -c add.c -o add.o -fPIC
	
	2. 使用 gcc -shared 制作动态库
	
		gcc -shared -o lib库名.so	add.o sub.o div.o
	
	3. 编译可执行程序时，指定所使用的动态库。  -l：指定库名(去掉lib前缀和.so后缀)  -L：指定库路径。
	
		gcc test.c -o a.out -lmymath -L./lib
	
	4. 运行可以执行程序 ./a.out 出错！！！！ --- ldd a.out --> "not found"
	
		error while loading shared libraries: libxxx.so: cannot open shared object file: No such file or directory
	
		原因：
			链接器：	工作于链接阶段， 工作时需要 -l 和 -L
	
			动态链接器：	工作于程序运行阶段，工作时需要提供动态库所在目录位置。
	
		解决方式：				
	
			【1】 通过环境变量：  export LD_LIBRARY_PATH=动态库路径
	
				./a.out 成功！！！  （临时生效， 终端重启环境变量失效）
	
			【2】 永久生效： 写入 终端配置文件。  .bashrc  建议使用绝对路径。
	
				1) vi ~/.bashrc
	
				2) 写入 export LD_LIBRARY_PATH=动态库路径  保存
	
				3）. .bashrc/  source .bashrc / 重启 终端  ---> 让修改后的.bashrc生效
	
				4）./a.out 成功！！！ 
	
			【3】 拷贝自定义动态库 到 /lib (标准C库所在目录位置)
	
			【4】 配置文件法
	
				1）sudo vi /etc/ld.so.conf
	
				2) 写入 动态库绝对路径  保存
	
				3）sudo ldconfig -v  使配置文件生效。
	
				4）./a.out 成功！！！--- 使用 ldd  a.out 查看