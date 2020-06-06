> [授课网站](https://www.linuxmooc.com/)

# 学习目标

系统编程接口：

文件重定向

进程

线程

# 实验环境

+ OS：[利用实验楼提供的虚拟机](https://www.shiyanlou.com/courses/1)|云主机
+ 工具：vim，GCC（std=99）

# Linux

## 文件系统

Linux系统中，很多基本命令与文件系统相关

+ 使用命令浏览目录
+ 使用命令创建删除文件

Linux的文件系统与Windows文件系统差异很大

+ 路径名分割符不同
+ 路径结构不同

| 比较项目   | Linux                              | Windows                    |
| ---------- | ---------------------------------- | -------------------------- |
| 路径分割符 | `\`                                | `/`                        |
| 路径结构   | 盘符+盘内路径                      | 仅有唯一根目录，无盘符     |
| 特点       | 与编程转义符相同，麻烦             | 不同于编程转转义符，方便   |
| 逻辑实例   | `C:\Windows\notepad.exe`           | `/usr/bin/gcc`             |
| 编程实例   | `fopen("C:\\Windows\\test.c","r")` | `fopen("/usr/test.c","r")` |

## 基础命令

批量改文件名：mv

vi编辑器；多个文件快捷切换？复制一行

内容显示：cat，less，grep过滤，正则查找

目录文件操作

gcc编译链接；makefile

清屏：`clear`，或者快捷键`Ctrl+l`

主目录：用户主目录，`cd ~`快速切换，一般位于`/home/用户名`

mkdir

cat：显示文件内容

touch:创建文件

cp：复制文件或目录，目录用`rm -r`

rm：删除文件或目录，目录用`rm -r`

mv：更改文件或目录名；移动文件

tree：树形展示文件或目录

### 获取帮助

#### man在线手册

Linux命令极多，通过在线手册获取帮助变得极为重要。通过查询格式如下：

```shell
man [手册序号] <命令名称>
// [arg]表示参数arg可选
// ...表示参数可重复
```

+ 可通过`man man`查询命令`man`的使用方式
+ 在线手册内容极多，为了简化查找，将内容进行分册处理，常见8个区段如下

| 区段 | 说明                                     |
| ---- | ---------------------------------------- |
| 1    | 一般命令                                 |
| 2    | 系统调用                                 |
| 3    | 库函数，涵盖C标准函数库                  |
| 4    | 特殊文件（通常为/dev中的设备）和驱动程序 |
| 5    | 文件格式和约定                           |
| 6    | 游戏和屏保                               |
| 7    | 杂项                                     |
| 8    | 系统管理命令和守护进程                   |

+ 想查询相应区段的内容，在man后面加上相应区段数字即可。如`man 1 ls`显示第一区段中的ls命令的man页面。
+ 所有手册页的布局基本一致，结构化展示如下

| 字段名            | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| NAME(名称)        | 该命令或函数的名称，接着是一行简介                           |
| SYNOPSIS(概要)    | 对于命令，正式描述它如何运行，所需命令行参数；对应函数，介绍函数所需参数，定义包含于哪个头文件 |
| DESCRIPTION(说明) | 命令或函数功能的文本描述；功能和参数的详细描述               |
| EXAMPLE(示例)     | 常见使用实例                                                 |
| SEE ALSO(参见)    | 相关命令或函数列表                                           |
| 其它              | OPTIONS(选项)；EXIT STATUS(退出状态);ENVIRONMENT(环境);BUGS(程序漏洞);FILES(文件);AUTHOR(作者);REPORTING BUGS(已知漏洞);HISTORY(历史);COPYRIGHT(版权) |

+ 为了便于搜索，可用`/ <你要搜索的关键字>`进行搜索，查找完毕后可用`n`切换到下一个关键字所在，`shift+n`切换到上一个关键字处。使用`Space`翻页，`Enter`滚动一行。`h`显示帮助(实质是less阅读器的使用帮助)，`q`退出。
+ 若只想快速查看相应命令的参数，使用`命令名称 --help`即可（大部分都有这个）

#### 安装 man

+ CentOS： `yum install man-pages.noarch`，可用 `man fgets` 验证下成功与否

#### 检索技巧

+ 查找文件：find

```
find 路径名 -name "名字特征"
find /etc -name "*.list" // 查找/etc下所有后缀为.list的文件
```

+ 文件内容过滤：grep（可用于查找函数定义，说明，结构体）

```
grep 匹配模式 文件名
// 在/usr/include下所有头文件中查找time_t的定义，或结构体
grep "time_t" /usr/include/*.h | grep "typedef"
```

+ 查找二进制文件/man文件位置：whereis
+ 查询手册：man n 函数名/命令

### 目录管理

目标：创建、复制、删除目录/文件

#### 命令速览表

| 命令    | 功能             | 实例                                                         |
| ------- | ---------------- | ------------------------------------------------------------ |
| `ls`    | 列出目录下的文件 | `ls /`(列出根目录下文件);`ls /bin`(bin目录下文件);`ls`(当前目录下文件) |
| `pwd`   | 显示当前工作目录 | `pwd`                                                        |
| `cd`    | 改变当前工作目录 | `cd /`(切换到根目录);`cd ~`(切换到用户主目录)                |
| `mkdir` | 创建目录         | `mkdir test`(在当前目录下创建test目录);`mkdir test/t2`(在test目录下创建t2目录) |

#### 用户主目录

Linux是一个**多用户**操作系统，系统中存在多个用户。

+ 每个用户都有自己的**专属主目录**
  + 用户主目录位于`/home`下
  + 以用户名作为目录名称
  + 通常情况下，用户只能修改自己主目录下的文件（权限管理）
+ 例如系统中存在2个普通用户`tom`和`jerry`
  + 用户`tom`的主目录为`/home/tom`
  + 用户`jerry`的主目录为`/home/jerry`
+ 在**Shell**中，`~`表示用户主目录
  + 用户tom想切换到主目录，使用`cd ~`和`cd /home/tom`效果等价
+ `id -u -n`：显示当前目录所属的有效用户名

### 文件操作

| 命令格式                        | 功能                 | 实例                                                  |
| ------------------------------- | -------------------- | ----------------------------------------------------- |
| `touch 文件名`                  | 创建空文件           | `touch test`(创建名为test的空文件)                    |
| `cat [选项] [文件名]`           | 打印文件内容到屏幕   | `cat /usr/test.c`(输出test.c的内容到屏幕)             |
| `cp <源文件> <目标文件>`        | 复制文件或目录       | `cp a.c b.c`；`cp -r adir bdir`（目录复制）           |
| `rm [选项] 文件`                | 删除文件或目录       | `rm t.c`；`rm -r adir`（递归删除目录）                |
| `mv <源文件> <目标文件>|<目录>` | 文件更名或移动到目录 | `mv a.c b.c`（更名）；`mv a.c adir`(移动到adir目录下) |

## 用户组和权限管理

### 用户组

保证当前所使用账户为root或者已在`sudo`组中，才有**创建和添加sudo组**操作。

+ **创建用户**：`sudo adduser username`，添加用户名为`username`的用户。(密码自己设置，其余配置默认即可)
+ **切换用户**：存在三种方式
  + `su <username>  `：简单切换到用户，但对应的**工作目录和环境变量**都**不会**切换成user的。
  + `su - <username>`：对应的工作目录(pwd)和环境变量($PATH)**都会**切换成user的。
  + `sudo <cmd>`：以特权级执行cmd命令。
+ **查看用户**：
  + `who am i`
  + `whoami`
+ **修改用户密码**：`passwd <username>`
+ **退出当前用户**
  + `exit`
  + `Ctrl+D`
+ **查看所在组**：`groups <username>`
+ **添加到sudo组**：
  + 普通系统：`sudo usermod -G sudo <username>`
  + centos：`sudo usermod -G wheel <username>`
+ **删除用户**：`sudo deluser <username> --remove-home`（删除用户组用`groupdel`）

### 文件权限

实例表如下：

|              | 读r（4） | 写w（2） | 执行x（1） | 总和 |
| ------------ | -------- | -------- | ---------- | ---- |
| **本用户**   | r        | w        | x          | 7    |
| **用户组**   | r        | w        | -          | 6    |
| **其它用户** | r        | -        | -          | 4    |

+ 查看具体信息：`ls -l`

```
-rwx--x--x 1 wyj wyj 152 May 20 20:55 test.c
```

+ 本次为本用户，用户组，其它用户，`-`表示无对应权限。可用`chmod`更改权限。

```
chmod 777 test.c
// 输出
-rwxrwxrwx 1 wyj wyj 152 May 20 20:55 test.c
```



## vim

强大的文本编辑器

配置参考：

+ [高赞-Basic和Awesome版本](https://github.com/amix/vimrc)
+ [不错的配置](https://github.com/ma6174/vim)
+ [实验楼简单配置-无插件-好看实用](https://github.com/wklken/vim-for-server)

### vim简介

#### vim是什么

目前vim是Unix系统下使用最广泛的文本编辑器。

+ Unix系统存在多种发行版本
  + Linux
  + FreeBSD
  + 各种商业Unix:HPUX、AIX、Solaris
+ vi是Unix系统下标准的文本编辑器
  + vi是 Visual Interface 的缩写
  + 1976年，有伯克利大学Bill Joy开发
+ vim是vi的升级版本，它在vi基础上改进和增加了很多特性
  + vim是vi improved的缩写
  + 80年代末，由Bram Moolenaar开发
+ vim已成为主流
  + Linux发行版自带的编辑器都是vim，而不是原始的vi
  + 在Linux中，可用vi或vim启动vim编辑器

#### vim缺点

学习成本高

+ 使用习惯与常见编辑器差异很大
  + windows下会用记事本，就可以无师自通学会另一款编辑器的基本功能
  + 对vim来世，这点不成立
+ 记忆较多快捷键
  + 对新手很不友好，无菜单和工具条
  + 在熟练记忆20条命令前，会觉得很难使用
  + 对新手来说，前期有一个陡峭的学习曲线
+ 配置繁琐
  + 缺省配置的vim，难用难看，没有**语法高亮、项目文件管理**等功能
  + 但提供各种使用插件，可大幅度提升编辑体验
  + 新手需花大量时间折腾各种插件

#### vim优点

熟练后编辑效率极高，用途广泛

+ 熟练掌握快捷键后，大幅度提升编辑效率
+ vim是通用的编辑器，支持多种编程语言
  + 通用性：可编辑`C/C++/Java/Python/PHP/Shell`等程序源代码
  + 一致性1：只需记忆一套快捷键和命令就可适应多种编程语言
  + 一致性2写任何类型的文本文件都可保持一致的输入习惯
  + 高效率：在vim学习上的投资，会在学习其它编程语言时慢慢收回来

#### 学习vim的必要性

+ Linux下已有成熟的IDE可用于编辑程序
  + 主流编程语言的IDE都有Linux发行版
  + 学习vim仍然有必要，至少掌握最基本的使用
+ **某些场合下，vim是唯一可用的编辑器**
  + 大部分IDE只可工作于GUI环境下，而vim可工作在Terminal环境中
  + 通过远程登录到服务器，通常只有Terminal环境
  + 系统出现问题，无法进入GUI环境，只能在Terminal环境中工作
+ 只需对单文件进行编辑，一个好的文本编辑器足矣
  + 编辑配置文件，Linux中的软件配置信息是存放在文本文件中
  + 编辑脚本文件，脚本程序通常很短，不超过100行

### 编辑模式

#### 引入编辑模式

**效率为王**：双手不离开键盘的主键区，编辑效率最高。

**模式区分**：不同模式下，按键功能不同。

+ 在大部分游戏中，快捷键都是集中在键盘的主键区
  + 使用`w,a,s,d`移动人物，而不是用键盘的上下左右移动人物
  + 玩家单手操作时，只需移动很短距离就可以在不同的快捷键间切换
+ 使用vim也有类似需求
  + **双手不离开键盘的主键区，编辑效率是最高的**
  + 不使用上下左右移动光标，使用`hjkl`移动光标。这样带来一个问题，用户按下`j`键后
    + vim是插入一个字符`j`
    + 还是移动光标到下一行
+ 为了解决上述问题，vim引入了模式的概念
  + 同一个按键，在不同的模式下，含义是不一样的，以`j`键为例
  + 在普通模式下，输入`j`键，移动光标到下一行
  + 在插入模式下，输入`j`键，在光标位置插入字符`j`

#### 三种编辑模式

vim编辑器有三种基本的编辑模式

+ 普通模式（Normal mode）
  + 控制屏幕光标的移动
  + 删除字符或行
  + 复制，粘贴
  + 查找文本
+ 插入模式（Insert mode）
  + 输入字符
  + 在命令模式下，作为普通字符被插入光标的位置
+ 命令模式（Command mode）
  + 替换文本
  + 保存文本
  + 退出程序

#### 三种模式切换

状态转换图如下

![1589597217572](OS-Lab.assets/1589597217572.png)

### 启动、退出和保存文件

#### 启动

vim有两种启动方式，带参数和不带参数

+ 命令行中输入`vi`（vim也行）
  + 不带参数启动vi
  + 启动后，vim进入普通模式
+ 命令行中输入`vi test.c`
  + 带参数启动vi
  + 如果文件test.c存在，则打开文件test.c
  + 否则，创建新文件test.c
  + 启动后，vim进入普通模式

#### 退出&保存文件

在普通模式下，如数以下命令可以退出vim（本质是命令模式）

| 命令                 | 功能                           |
| -------------------- | ------------------------------ |
| `:q`                 | 退出                           |
| `:q!`                | 强制退出，不存盘               |
| `:wq`/`:x`           | 存盘退出                       |
| `:wq file`/`:x file` | 先保存内容与文件file中，再退出 |

### 进入插入模式

存在六种从一般模式进入插入模式的方式，本质是节省了光标移动的操作。

已经在插入模式时，输入的字符都是插到光标所在位置前面。如`1234`,此时光标在`3`，那么我插入字符`5`，变成`12534`，

| 命令 | 进入插入模式后光标位置                 |
| ---- | -------------------------------------- |
| `i`  | 当前光标                               |
| `a`  | 当前光标右侧位置                       |
| `I`  | 当前光标所在行的行首                   |
| `A`  | 当前光标所在行的行尾                   |
| `o`  | 当前光标所在行的下一行行首（插入空行） |
| `O`  | 当前光标所在行的上一行行首（插入空行） |

### 移动光标

#### 基本移动

主要包括字符、单词、句子和段落等单位的移动。

| 命令     | 功能                             |
| -------- | -------------------------------- |
| `h`      | 光标向左移动一个字符             |
| `j`      | 光标向下移动一个字符             |
| `k`      | 光标向上移动一个字符             |
| `l`      | 光标向右移动一个字符             |
| `w`      | 光标向左移动一个**单词**         |
| `b`      | 光标向右移动一个**单词**         |
| `(`，`)` | 光标向**前或后**移动一个**句子** |
| `{`,`}`  | 光标向**前或后**移动一个**段落** |

#### 快速移动

主要包括行内、行间和整个文件内位置切换。

| 命令              | 功能                                                       |
| ----------------- | ---------------------------------------------------------- |
| `0`               | 光标移动到当前行的行首（行内）                             |
| `^`               | 光标移动到当前行的**第一个非空字符**（行内）               |
| `$`               | 光标移动到当前行的行尾（行内）                             |
| `gg`              | 光标移动到当前文件的第一行的**第一个非空字符**（文件内）   |
| `G`               | 光标移动到当前文件的最后一行的**第一个非空字符**（文件内） |
| `Ctrl+d`\|`Ctrl+f` | 光标向下移动**半页/一页**（多行）                          |
| `Ctrl+u`\|`Ctrl+b` | 光标向上移动**半页/一页**（多行）                          |

### 编辑操作

+ 删除

| 命令                   | 功能                                   |
| ---------------------- | -------------------------------------- |
| `x`                    | 删除当前光标处的字符                   |
| `dd`                   | 删除当前光标所在行                     |
| `dw`                   | 删除当前光标所在单词（最后放单词开头） |
| `d(`，`d)`；`d{`，`d}` | **向前/向后**删除一个**句子/段落**     |

+ 撤销

| 命令     | 功能                             |
| -------- | -------------------------------- |
| `u`      | 撤销上一次操作                   |
| `Ctrl+r` | 重新执行被撤销的操作（还原撤销） |

维护两个链表，正常操作链表和撤销操作链表。

+ 复制粘贴

| 命令 | 功能                 |
| ---- | -------------------- |
| `y`  | 复制所有被选中的字符 |
| `yy` | 复制当前行           |
| `p`  | 粘贴到**下一行**     |

+ 上下移动

| 命令         | 功能                                |
| ------------ | ----------------------------------- |
| `:m+lineNum` | 将被选中的行**向上**移动`lineNum`行 |
| `:m-lineNum` | 将被选中的行**向下**移动`lineNum`行 |

### 可视化操作

选中不同字符，不同行，便于进行复制，删除，移动处理。

| 命令     | 功能                   |
| -------- | ---------------------- |
| `v`      | 进入**字符**可视化模式 |
| `V`      | 进入**行**可视化模式   |
| `Ctri+v` | 进入**块**可视化模式   |
| `d`      | 删除选中的所有字符     |

### 重复命令

vim命令支持组合，支持重复。任何命令前加上数字，表示重复次数。（这是一个通用的方法）

`n cmd`表示命令`cmd`重复执行n次，例如

+ `n h/j/k/l`：朝左下上右移动n个字符。
+ `n w/b`:向前/后移动n个单词（句子，段落类似）
+ `n dd`：从当前行开始向下删除n行（包括当前行）
+ `n yy`：从当前行开始复制n行（包括当前行）

### 代码操作

+ **缩进**：用可视化先选中行
  + `<<`：被选中行向左缩进1格（可用重复命令）
  + `>>`：被选中行向右缩进1格（可用重复命令）
  + `=`：自动调整缩进格式

+ **注释**
  + **块注释**
    + 1. 普通模式下，将光标移到行首，然后`Ctrl+v`进入块选择模式。
    + 2. 选择所有需要注释的行的行首
    + 3. `Shift+i`进入插入模式；输入需要的注释符，比如`//`或者`#`
    + 4. 最后按**两下**`Esc`
  + **取消块注释**
    + 前两步和块注释1、2步相同（注意一定要选择全部注释符），第三步只需按`d`，即可删除选中部分

+ **版本对比**：`vimdiff`可实现文件内容比对
  + **差异结果跳转**：`]c`下一个差异；`[c`上一个差异
  + **合并**：（每次合并后都执行`:diff`，显示新的差异）
    + `dp`(diff put)：将光标所在文件的差异复制到另一个文件中
    + `do`（diff get/obtain)：将另一个文件的当前差异复制到当前文件的对于位置。

## GCC

### 理论基础

#### 装入基础

可执行代码被载入内存的过程，存在三种演进方式：

+ **绝对装入**：可执行代码中地址已知，只能装载到指定的指定空间中。单道批处理系统常用。
+ **可重定位装入**：载入时可执行代码会重新计算每个地址的偏移量，解决绝对地址的问题。可以装到任何地址。多道处理系统。但是移动程序时必须重新计算代码中每个地址的位置。
+ **动态运行时装入**：无需更改，直接载入。代码中地址中保存相对地址，寻址时需加上基地址。软硬件相配合，移动程序时仅需改变基地址即可。

#### 链接方式

链接也存在三种方式，各有优缺点。

+ **静态链接**：单纯将不同.o文件的相应段合并，构成exe文件。缺点在于一个.c文件更新，必须重新编译链接所有文件，才能形成exe文件，效率太低，消耗过高。
+ **装入时动态链接**：exe只保存库的头文件，运行时会**加载所有的库**，合并代码（和静态链接效果一致）。升级时无需重新链接。
+ **运行时动态链接**：和装入时动态链接类型，只不过优点在于运行时**只加载用到的库**，没用到不加载。容易升级，但是兼容性不好，需要考虑版本问题。

### 编译&链接&执行文件

假设源文件`hello.c`如下：

```c
#include "stdio.h"
int main() {
	puts("hello world!");
    return 0;
}
```

+ 编译：`gcc -o 可执行文件名 源文件名`。例如`gcc -o test.exe test.c`，将`test.c`编译成可执行文件`test.exe`

+ 执行：`./test.exe`，必须加上`./`告诉系统在当前目录下搜索，否则对于可执行的文件，系统默认会去搜索环境变量`PATH=/bin:/usr/bin`，而不会搜索当前目录。这主要是出于对系统的安全性考虑，涉及用户组权限管理的问题。（管理员容易受到攻击，比如系统文件的恶意删除）

+ 通过`PATH=路径1:路径2:...`设置可执行文件的搜索路径。

### 基本配置

**基本用法**：`cc -std=c99 -o outfile filename`

+ `-std=c99`指定语言标准，之前的语言是无法边声明边使用的，为了便于使用，
  + **增加别名**：`alias cc='cc -std=c99'`
  + **删除别名**：`unalias cc`

## GDB

GDB是Linux下调试的一个利器，用好它大幅度提升效率。

### 内存访问错误调试

段错误`Segmaent fault`恐怕是C语言下见得最多的错误了，因为和指针有关，常见问题如下：

+ 空指针：空指针默认值不确定，是个野指针（无读写权限）
+ 数组访问溢出：下标超过数组边界（无读写权限）
+ 无写权限：改变`const char*`指向的内容。（只读不可写）

下面是一个很简单的空指针访问导致的多错误实例，文件名`mem.c`：

```c
#include <stdio.h>
#include <stdlib.h>

void test()
{
    int *p = NULL;
    *p = 123;
}

int main()
{
    test();
    return 0;
}
```

#### CORE文件简介

Linux程序不正常退出时

+ 内核访问错误
+ 内核会在**当前工作目录**下生成一个 core 文件

core文件是一个**内存映像**

+ 用 gdb 来查看 core 文件
+ 可以查看导致程序**出错的代码**所在**文件**和**行数**

#### CORE文件大小的限制

+ `ulimit -c`
  + 查看允许产生的 core 文件的最大尺寸
  + 若结果为0，则表示不会生成 core 文件
+ `ulimit -c unlimited`：允许产生任意尺寸的 core 文件

#### 编译程序

加上`-g`选项，生成具有符号信息的可执行文件

```
cc -g mem.c
```

#### 执行程序

运行程序，在当前目录下产生 core 文件（core dumped 意为核心转储）

```
$ ./a.out
Segmentation fault (core dumped)
```

#### GDB 和 BACKTRACE

+ GDB 启动调试：`gdb 可执行文件名 core`
+ `BACKTRACE(bt)`：打印堆栈跟踪信息，输出错误代码所在文件、函数和行数。简写为`bt`
+ `quit`：退出 GDB

```
guest@linuxmooc:~/mooc/www/teach/os$ gdb a.out core
GNU gdb (Ubuntu 8.1-0ubuntu3.2) 8.1.0.20180409-git
Core was generated by `./a.out'.
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x000055fef28c560a in test () at mem.c:6
6           *p = 123;
(gdb) bt
#0  0x000055fef28c560a in test () at mem.c:6
#1  0x000055fef28c5621 in main () at mem.c:11
(gdb) quit
```

#### 内存错误调试总结

发现内存错误时，按照如下4步走：

+ **core 大小设置**：`unlimit -c` 查看 core 可用最大尺寸，若为0，`ulimit -c ulimited` 改为无限大；否则进入下一步。
+ **编译程序**：编译程序时加入 `-g` 选项，在生成的可执行文件中加入符号信息。
+ **执行程序**：执行可执行程序，系统在**当前工作目录**下自动生成 core 文件。
+ **GDB 调试**：`gdb 可执行文件 core` 启动调试，`bt` 查看错误代码所在**文件/函数/行数**。

## IO

核心系统调用如下：

+ open
+ read
+ write
+ close

## 进程管理

核心API

+ fork：创建进程
+ exec：执行代码
+ wait
+ exit

### fork

#### 原型

+ 父进程通过fork创建子进程
  + 子进程拷贝父进程的**代码段**、**数据段**和**打开的文件列表**（叉子的主干相同，末梢延展）
  + 子进程的pid是独立生成的，ppid为父进程的pid
+ 子进程从**fork的返回处**处开始执行，而不是从头开始执行代码（意思是子进程不会再次执行fork，造成死循环）。同理，父进程也是从fork返回处开始执行。父子进程对于fork的返回值不同（OS会特殊处理）
  + 父进程的fork返回值为子进程的pid
  + 子进程的fork返回值恒定为0（OS特殊处理）

+ 二者此时是**并发执行**，执行无确定的先后顺序

以下是一个演示实例：

```c
#include <stdio.h>
#include <unistd.h> // 包含fork函数
int main() {
    pid_t pid; // 定义pid类型
    pid = fork(); // fork子进程
    if (pid == 0) printf("child process: pid:%d ppid:%d\n",getpid(),getppid());
    else printf("father process: pid:%d child pid:%d\n", getpid(),pid);
    return 0;
}
```

有可能出现子进程的pid=1，不同于父进程pid的情况

+ Linux下pid=1是超级进程init
+ 当父进程先结束时，子进程成为孤儿进程，自动挂在pid=1下

#### 特性

+ **并发特性**
  + 父子进程并发运行，都处于运行状态
  + 应该是交织执行的
  + 二者代码段相同

```c
#include <stdio.h>
#include <unistd.h>
void child() {
    for (int i=0; i < 3; ++ i) {
        puts("child");
        sleep(1);
    }

}
void parent() {
    for (int i=0; i < 3; ++ i) {
        puts("parent");
        sleep(1);
    }
}
int main() {
    pid_t pid;
    pid = fork();
    if (pid == 0) child();
    else parent();
}
// 交替输出parent和child
```

+ **隔离特性**
  + 父子进程具有独立的地址空间（虽然它们代码段和数据段一样）
  + 进程只能访问属于自己的地址空间。如父进程访问子进程的地址空间会报访问错误
  + 父子进程的全局变量存在于各自的地址空间，无法共享

```c
#include <stdio.h>
#include <unistd.h>

int global = 0;

void child() {
    for (int i=0; i < 3; ++ i) {
        global ++;
        printf("In Child, global = %d\n", global);
        sleep(1);
    }

}
void parent() {
    for (int i=0; i < 3; ++ i) {
        global ++;
        printf("In Parent, global = %d\n", global);
        sleep(1);
    }
}
int main() {
    pid_t pid;
    pid = fork();
    if (pid == 0) child();
    else parent();
}

// 一种可能输出
In Parent, global = 1
In Child, global = 1
In Parent, global = 2
In Child, global = 2
In Parent, global = 3
In Child, global = 3
```



### exec

#### execl

```c
#include <unistd.h>
int execl(const char *path, const char *arg, ...); //可变参数，末尾参数必须为NULL
// 使用示例
execl("/bin/echo","echo","a","b","c",NULL);
// 输出：a, b, c
// 注意参数传递两个要点：
// (1) NULL作为结尾标识
// (2) 第一个参数通常传入命令名称，其实没用，可以为任意字符串，但不能为NULL！（后面参数会被忽略）
```

+ **清空**当前地址空间（不清空也行，用个fork返回值来判断）
+ 将path指定的可执行程序的代码和数据装入当前进程的地址空间

### execl/execlp/execv/execvp

这4个是从两个维度来划分的

+ **参数形式**：l表示`list`，v表示`vector`。（vector比较方便使用）

  + **execl**：函数名execl末尾的l表示list，参数以列表的形式传递给可执行程序

  ```c
  execl("/bin/echo", "echo", "a", "b", "c", NULL); // 输出a b c
  ```

  + **execv**：函数名execv末尾的v表示vector，参数以数组的形式传递给可执行程序

  ```c
  char *argv[] = {"echo", "a", "b", "c", NULL}; // 输出a b c
  execv("/bin/echo", argv);
  ```

+ **路径查找**：p表示会从PATH环境变量指定路径下搜索。没p则表示不会。

  + 无论有没p结尾，其默认搜索路径如下：

    + 绝对路径
    + 相对路径：当前工作目录下的相对路径

  + 有p结尾，增加一条相对搜索路径：环境变量PATH指定的路径下的相对路径

    ```c
    char *argv[] = {"echo", "a", "b", "c", NULL}; // 参数vector形式
    
    // execv  输出：exec: No such file or directory
    execv("echo", argv); 
    
    // execvp 输出：a b c
    execvp("echo", argv); 
    
    // 当前目录下没有echo，所以execv报错；/bin在PATH中，因此execvp可以找到echo
    ```



4个方法总结对比见下表：

| 参数形式\路径查找 | 无PATH | 有PATH |
| ----------------- | ------ | ------ |
| **l**ist          | execl  | execlp |
| **v**ector        | execv  | execvp |

+ 从使用角度来说，有PATH比无PATH更方便；参数形式vector比list更好编程，易于修改。因此，编程推荐使用`execvp`。
+ 从进化历史来说，execl应该最先出现，然后发现每次调用函数要传递参数太多了，于是出现vector传参方式。又发现找不到系统的执行文件，于是增加了PATH查找路径方式。最后`execvp`集大成者。

## 文件描述符

文件描述符(file descriptor)

+ 定义：一个非负整数，应用程序用来访问文件
+ 应用：打开或创建文件时内核会返回 fd，读写文件均需要指定 fd

管道：

+ 用于连接两个进程
+ `ls /bin | wc -l`

## 常用命令

### 下载网络资源

+ centos 使用 wget 下载网络资源

```
wget https://www.linuxmooc.com/tmp/printf.tgz
```

### 解压缩

+ tar 命令可以实现压缩和解压缩功能

```
// 压缩文件或目录
tar -cf 压缩包名 文件名1 文件名2 目录名1 ...
// 解压缩
tar -xvf printf.tgz
```

### 格式化代码

#### 风格标准

+ 命名：**驼峰**或**下划线**
+ 缩进：`tab=4`空格
  + 函数的**左花括号**换行
  + 控制语句 `if while for else`
    +  **左花括号**加1个空格，不换行
    + 语句块需换行，即是仅1句，也要换行
+ 空格
  + 函数声明的参数，函数调用的参数
  + 操作符，运算符之间

#### **Linux**

+ 下载 `indent`
+ `indent -kr -i4 -nut program.c` ：以 `K&R` 风格，缩进4空格，`tab=4`空格来格式化

#### Window10 VSCode

+ 选中代码
+ `shift + alt + F`

# 调试技巧

核心：**缩小错误范围**（排除法-每次排除没有问题的）

缩小范围便于提问，他人很容易回答，自己也容易解决

+ 将代码中最复杂的部分抽取出来，封装为函数，进行测试（linux可直接cp文件，进行删改）
+ 测试完没问题的部分直接删除，进行下一步排除；若执行出现问题，利用报错，gdb等调试工具快速定位
+ 不断缩小范围，迭代排查，对于不确定的代码可写demo进行猜测验证
+ 将代码缩小到一定程度，接下来两种方式
  + 自己深入探究
  + 求助网络/他人：博客，论坛提问(segmentfault，stackoverflow)，老师，同学，师长

# 文件读写编程题目

## myecho

### 需求描述

- myecho.c的功能与系统echo程序相同 

- 接受命令行参数，并将参数打印出来，例子如下： 

  ```
  $ ./myecho x
  x
  $ ./myecho a b c
  a b c
  ```

### 实现思路

从命令行获取参数，从第二个元素开始循环输出即可（第一个为命令名称）

### 实现代码

文件名`myecho.c`

```c
#include <stdio.h>
// argc是agrv长度，第一个字符串为文件名
// 打印从命令行传入的参数
int main(int argc, char* agrv[]) {
    for (int i=1; i < argc; ++ i) {
        printf("%s ", agrv[i]);
    }
    puts("");
}
```

### 编译执行

+ 编译：输入`gcc -o myecho myecho.c`
+ 执行：`./myecho a b c`

## mycat

### 需求描述

- mycat.c的功能与系统cat程序相同 

- mycat将指定的文件内容输出到屏幕，例子如下： 

- 要求使用系统调用open/read/write/close实现 

  ```
  $ cat /etc/passwd 
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  ...
  $ ./mycat /etc/passwd 
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  ...
  ```

### 实现思路

主要使用文件读写接口，Linux 中万物皆文件，标准输出也是一个文件。所以可先从指定的文件读出数据，然后写到屏幕。

### 注意点 

+ **边读边写**：为了提高效率和程序处理大数据文件的能力，采用边读边写的方式，而不是一次性读完再写。
+ 写入文件（屏幕）时



### 实现代码

```c
#include <stdio.h>
#include <unistd.h> 	// read,write
#include <sys/stat.h> 	// open
#include <fcntl.h>
#include <stdlib.h> 	// exit
// error handler
void panic(char *message) {
    perror(message);
    exit(EXIT_FAILURE);
}
int main(int argc, char* argv[]) {
    if (argc < 2) panic("please input cat filename");
    int sourceFileno = open(argv[1],O_RDONLY);
    if (sourceFileno == -1) panic("fail to open file!");
    char buf[512]; 	// read and write buffer
    while (1) { 	// true is not defined !!
        int rnum = read(sourceFileno,buf,sizeof(buf));
        if (rnum == -1) perror("fail to read file!");
        if (rnum == 0) break;
        //write(fileno(stdout),buf,sizeof(buf)); // sizeof(buf)>=real read bytes
        int wnum=write(fileno(stdout),buf,rnum); // write to stdout
        if (wnum == -1) panic("fail to write");
    }
    close(sourceFileno); // close source file
    return 0;
}
```

### 编译执行

- 编译：输入`gcc -o mycat mycat.c`
- 执行：`./mycat /usr/passwd`

#### 遇到问题

> 以上代码运行时，buf取1024时末尾会多出一行乱码；取512时无法读取完所有内容。

尝试：

+ 流程：打开源文件-读取文件内容-写入标准输出-关闭源文件
+ buf大小关系？超出`SSIZE_MAX`?
+ read时不能读刚好sizeof(buf)，要预留\0位置
+ write不能直接写sizeof(buf)，应该写strlen(buf)或者rnum；若要使用`strlen`，必须注意`\0`问题。

## mycp

### 需求描述

- mycp.c的功能与系统cp程序相同 

- 将源文件复制到目标文件，例子如下： 

- 要求使用系统调用open/read/write/close实现 

  ```
  $ cat /etc/passwd
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  ...
  $ ./mycp /etc/passwd passwd.bak 
  $ cat passwd.bak
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  ...
  ```

### 实现代码

```c
#include <stdio.h>
#include <unistd.h> // read,write
#include <sys/stat.h> // open
#include <fcntl.h>
#include <stdlib.h> // exit
// error handler
void panic(char *message) {
    perror(message);
    exit(EXIT_FAILURE);
}
int main(int argc, char* argv[]) {
    if (argc < 2) panic("please input source filename");
    if (argc < 3) panic("please input dest filename");
    int sourceFileno = open(argv[1],O_RDONLY);
    if (sourceFileno == -1) panic("fail to open source file!");
    int destFileno = open(argv[2],O_WRONLY|O_CREAT,0777); // mode=0777
    if (destFileno == -1) panic("fail to open dest file!");
    char buf[512];
    while (1) { // true is not defined !!
        int rnum = read(sourceFileno,buf,sizeof(buf));
        if (rnum == -1) perror("fail to read file!");
        if (rnum == 0) break;
        //write(fileno(stdout),buf,sizeof(buf)); // write to stdout
        int wnum=write(destFileno,buf,rnum); // write to dest file
        if (wnum == -1) panic("fail to write");
    }
    close(sourceFileno); // close source file
    close(destFileno); // close dest file
    return 0;
}
```

### 编译执行

- 编译：输入`gcc -o mycp mycp.c`
- 执行：`./mycp /usr/passwd passwd.bak`

# 多进程题目

## mysys

### 需求描述

 实现函数mysys，用于执行一个系统命令，要求如下

- mysys的功能与系统函数system相同，要求用进程管理相关系统调用自己实现一遍 

- **使用fork/exec/wait系统调用实现mysys** 

- 不能通过调用系统函数system实现mysys     

- 测试程序 

  ```
  #include <stdio.h>
  
  void mysys(char *command)
  {
      实现该函数，该函数执行一条命令，并等待该命令执行结束
  }
  
  int main()
  {
      printf("--------------------------------------------------\n");
      mysys("echo HELLO WORLD");
      printf("--------------------------------------------------\n");
      mysys("ls /");
      printf("--------------------------------------------------\n");
      return 0;
  }
  ```

- 测试程序的输出结果 

  ```
  --------------------------------------------------
  HELLO WORLD
  --------------------------------------------------
  bin    core  home	     lib	 mnt   root  snap  tmp	vmlinuz
  boot   dev   initrd.img      lost+found  opt   run   srv   usr	vmlinuz.old
  cdrom  etc   initrd.img.old  media	 proc  sbin  sys   var
  --------------------------------------------------
  ```

### 实现代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
void mysys(char *command) {
    char cmd_str[strlen(command)+1];
    strcpy(cmd_str,command); // copy const char* -> char*
    char *argv[50]={NULL}; // argv[0] is random,argv.back()=NULL
    const char d[2]=" ";
    argv[0] = strtok(cmd_str,d); // argv[0]=executed file name
    int i=1; // i=0 is random
    while (1) {
        if (i >= 50) perror("i>=50");
        argv[i] = strtok(NULL," "); // first param=NULL
        if (argv[i] == NULL) break;
        ++ i;
    }
    // fork
    // child: exec new program
    // father: block until child is return
    pid_t pid = fork();
    if (pid == 0) { // child
        int error = execvp(argv[0],argv);
        if (error < 0) perror("execvp");
    }
    else { // father
        wait(NULL);
    }
}

int main()
{
    printf("--------------------------------------------------\n");
    mysys("echo HELLO WORLD");
    printf("--------------------------------------------------\n");
    mysys("ls /");
    printf("--------------------------------------------------\n");
    return 0;
}
```

#### C语言实现split

+ 注意不能用双指针返回，局部变量在函数返回后会被释放；要类似引用的方式，传入二维指针
+ 若改变字符串，strlen慎用，每次调用它的时间复杂度为n，最后先用变量保留它的长度
+ bool在C99才支持，需要包含头文件`stdbool.h`

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <stdbool.h>
void split(char s[], char* res[]) {
    char buf[2048];
    strcpy(buf,s);
    bool isHead=true;
    int i=0, j=0, len=strlen(buf);
    while (i < len) {
        if (buf[i] == ' ') {
             buf[i] = '\0';
             isHead = true;
        }
        else {
            if (isHead) {
                res[j++] = buf + i;
                isHead = false;
            }
        }
        ++ i;
    }
}
void print(char* l[]) {
    puts("print==========\n");
    if (l == NULL) perror("l is NULL");
    for (char** p=l; *p; ++ p) printf("%s ", *p);
    puts("");
}
int main() {
    char* s = "   hello   world good   "; // 指向字符串常量
    char* res[100]={NULL};
    split(s,res);
    print(res);
    return 0;
}
```

### 遇见问题

> Segmentation fault (core dumped)：段错误，核心转储

+ `ulimit -a`查看`core file size`是否为`0`，若为0，需使用`unlimit -c 1024`或者`unlimit -c unlimited`将核心转储文件大小设置一下（core file存储错误信息）
+ 编译时加上`-g`参数，`gcc -g -o test test.c`，即加入调试信息。
+ 此时运行可执行文件，会出现`core`文件
+ `gdb ./test core`即可进行调试
+ `backtrace`或者`bt`，堆栈跟踪，会显示错误行数
+ gdb[调试常用命令](https://blog.csdn.net/zdy0_2004/article/details/80102076)

[strtok巨坑](https://stackoverflow.com/questions/8957829/strtok-segmentation-fault?tdsourcetag=s_pctim_aiomsg)

[const char* 和char*转换](https://blog.csdn.net/qq_31893087/article/details/84206166)

问题分析

+ 本质是`const char*`类型和`strtok`会改变第一个参数的问题的矛盾。
  + 字符串常量是`const char*`（不允许被修改）
  + 字符串常量作为实参，传递的仅为它所在地址
  + 函数形参为`char*`，实参地址传来，依旧无法改变该地址是无法改变的事实
  + 然而`strtok`又会改变第一个参数所指内容，引发矛盾，即段错误
+ 修改bug：将传入的字符串拷贝到`char*`类型的空间内。
  + `strcpy(char* dest, const char* src)`：会拷贝最后的`\0`，注意`dest`大小要够。
  + `strncpy(char* dest, const char* src, int size)`：只拷贝指定大小，注意增加末尾`\0`。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void split_bug(char* s) { // bug版
    char *tmp;
    tmp = strtok(s, " "); // 这里的s可能指向const char*，会出现段错误
    if (tmp != NULL) printf("%s\n", tmp);
    while (true) {
        tmp = strtok(NULL, " "); // 第二次传入NULL
        if (tmp == NULL) break;
        printf("%s\n", tmp);
    }
}
void split_fixbug(char* s) { // 修正版，将传入的字符串都拷贝一份到char数组
    char ts[strlen(s)];
    strcpy(ts, s);
    char *tmp;
    tmp = strtok(ts, " "); // 这里的s可能指向const char*，会出现段错误
    if (tmp != NULL) printf("%s\n", tmp);
    while (true) {
        tmp = strtok(NULL, " "); // 第二次传入NULL
        if (tmp == NULL) break;
        printf("%s\n", tmp);
    }
    puts("");
}
int main() {
    char s[] = "char array is all right"; // 字符数组会新分配地址空间
    split_fixbug(s); // 传入char数组没问题
    split_fixbug("fixbug const char* is error"); // 传入const char*会有bug

    split_bug(s); // 传入char数组没问题
    split_bug("bug const char* is error"); // 传入const char*会有bug
    return 0;
}
```



## sh1.c

### 需求描述

- 该程序读取用户输入的命令，调用函数mysys(上一个作业)执行用户的命令，示例如下 

  ```shell
  # 编译sh1.c
  $ cc -o sh1 sh1.c
  
  # 执行sh1
  $ ./sh 
  
  # sh1打印提示符>，同时读取用户输入的命令echo，并执行输出结果
  > echo a b c
  a b c
  
  # sh1打印提示符>，同时读取用户输入的命令cat，并执行输出结果
  > cat /etc/passwd
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  ```

- 请考虑如何实现内置命令cd、pwd、exit （系统中没有对应的可执行文件，需自己实现）

### 实现思路

#### 循环界面

在 main 函数写入死循环，每次等待接受用户输入的一行字符串，然后将字符串传给命令解析函数做相应处理。其伪代码逻辑如下：

```pseudocode
while (true) {
	print("<");
	读入一行字符串到cmd_str
	cmd_handle(cmd_str); // 字符串解析并处理
}
```

**注意点**： `fgets` 读入一行时，会将换行符也存储起来，因此读入的时候做个判断，最后一个字符若为换行符，则改为`\0`，直接作为结尾标识。（`gets` 不安全，已被弃用）

#### 三个内置函数

> 为什么不能像 `ls echo cat` 等命令一样，直接用 `mysys` 去调用，而要自己实现？

主要是因为 Linux 系统中不存在 `cd exit` 的可执行文件，可用 `whereis cd` 进行验证，根本找不到对应的可执行文件。而在实验楼环境下，`pwd` 存在可执行文件，所以他是可以直接被调用的。

##### cd

+ 改变当前进程的工作目录，直接调用 `char* chdir(char *pathname)` 即可。

+ **注意**：若在子进程调用该函数，改变的仅仅是子进程的 `cwd`，结束后父进程 `cwd` 并不会改变。

##### pwd

当前工作目录 `cwd` 作为进程的一个属性，fork 时子进程拷贝父进程的 `cwd`，因此可以通过 mysys 直接调用 pwd 的可执行文件，但为了**提高效率**，还是将其作为系统的内置函数来实现。

+ 调用系统函数 `char* getcwd(char*buf,int size)` ,即可轻松获取当前工作目录的绝对路径
+ `getwd` 已被弃用

##### exit

+ 在字符串分割之后，调用系统函数 `exit` 即可结束当前进程
+ 注意要在进入父进程调用，若进入子进程在调用永远也只是退出子进程，无法退出 `shell`

### 实现代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

// command arguments maximum number
#define ARGNUM 50
// DEBUG switch: If don't want DEBUG, just comment it.
//#define DEBUG

/* Function: Command string parse; remove space, split string, store in argv
*  Return:   argv list
*  Attention: argv must use dynamic memory allocation , otherwise after split_cmd overed, argv is release.
*/
char **split_cmd(char *cmd_str)
{
    char **argv = (char **)malloc(sizeof(char *) * ARGNUM); // dynamic malloc !!!
    const char d[2] = " ";
    argv[0] = strtok(cmd_str, d); // argv[0]=executed file name
    int i = 1;                    // i=0 is random
    while (1) {
        if (i >= ARGNUM)
            perror("Argument num is to much");
        argv[i] = strtok(NULL, " "); // first param=NULL
        if (argv[i] == NULL)
            break;
        ++i;
    }
#ifdef DEBUG
    puts("==DEBUG== split_cmd.argv:");
    char **p = argv;
    while (*p != NULL)
    {
        printf("%s\n", *p);
        ++p;
    }
#endif
    return argv;
}
// PWD: Print calling process's current workplace directory
void mypwd()
{
    char buf[100];
    char *ret = getcwd(buf, 100);
    if (ret == NULL)
        perror("mypwd-getpwd is error");
    else
        puts(buf);
}
void mycd(char *argv[])
{
    if (argv[1] == NULL)
        perror("mycd: pathname is NULL");
    int ret = chdir(argv[1]);
    if (ret < 0)
        perror("cd error");
}
/*
    Function: Fork a child process to exec cmd
    Args:
        argv: Argument list, include cmd name and it's args
    Implementation Approach:
        1. Fork a child process
        2. By calling execvp, child exec new program
        3. Father block until child is return
*/
void mysys(char **argv)
{
    pid_t pid = fork();
    if (pid == 0) { // child
        int error = execvp(argv[0], argv);
        if (error < 0) { // child exec fail
            perror("execvp");
            exit(0);
        }
    }
    else { // father
        wait(NULL);
    }
}
// API: Handle command
void cmd_handle(char *command)
{
    // parse command
    char *cmd_str = (char *)malloc(sizeof(char) * (strlen(command) + 1)); // important!!!
    strcpy(cmd_str, command); // copy const char* -> char*
    char **argv = split_cmd(cmd_str);
    // handle command
    if (strcmp(argv[0], "exit") == 0)
        exit(0);
    else if (strcmp(argv[0], "pwd") == 0)
        mypwd();
    else if (strcmp(argv[0], "cd") == 0)
        mycd(argv);
    else
        mysys(argv); // system call
    // free heap memory
    free(cmd_str);
    free(argv);
}
int main()
{
    char cmd[80];
    while (1) {
        printf(">");
        fgets(cmd, 80, stdin);
        int len = strlen(cmd);
        if (cmd[len - 1] == '\n')
            cmd[len - 1] = '\0'; // delete newline
        cmd_handle(cmd);
    }
    return 0;
}
```

### 遇见问题

#### 问题描述

为了提高程序**可读性**和**复用性**，将 `mysys` 中的字符串解析单独提取出来，封装为一个函数 ，传入参数命令字符串 `command`，返回解析后的命令参数列表 `argv`。

```c
char **split_cmd(char *command); 
```

**测试**时该**函数内部**的参数解析**正常**，但是该**函数返回的参数列表**打印出来的内容却是**乱码**。



#### 问题分析

使用调试技巧大致确定问题范围，`split_cmd` 内部实现正确，问题出在了返回的参数列表上

+ **问题1**：`argv` 是局部开辟的数组，在函数结束后自动释放该数组，因此返回的参数列表地址已经被释放了

+ **解决1**：`argv` 使用 `malloc` 动态申请，只要不手动释放，这个内存一直存在

一执行，发现问题和最初一样，但通过写 demo 方式验证 `malloc` 是可行的

+ **问题2**：`argv` 是 `char **` 类型，突然反应过来我没有对二维数组都开辟内存，但仔细一想又不对，`argv[i]` 中存储的是 `char*` 类型，不需要申请，而 `char *` 来自于 `strtok` 返回的地址。那问题找到了，因为 `strtok` 会改变原字符串，于是我必须使用 `strcpy` 将传入的 `commmand` 拷贝一份为 `cmd_str`，而该 `cmd_str` 是局部变量，和问题1相同
+ **解决2**：保证传入的 `command` 不是常量字符串（在传入之前先 `strcpy`），删除函数内部的 `strcpy`，使得`command` 在该函数内部不是局部变量



#### 问题小结

+ **内存分配**：局部变量在函数结束后自动释放，分配与栈上；全局变量分配在堆上，生存周期与整个程序相同
+ **动态申请**：一维指针只需一次 `malloc` 分配；而二维指针需要对两个分别申请内存，通常会忽略第二层的申请

### 总结心得

+ **调试技巧**：虽然程序小，但是如何写得规范，具有可读性和扩展性，却很考验编程功底。其中调试是一个很重要的部分，写代码70%时间都在调试，常见技巧如下：
  + **缩小范围**：朱老师提供的**缩小范围**调试方法真是受益匪浅，类似二分查找，快速缩小问题范围（即每次排除一定没问题的部分），定位问题所在。
  + **封装简化**：将最复杂的部分抽取出来，封装为函数，进行单独测试。同时将确定没问题的代码删除，简化逻辑，便于定位查找。
  + **实例demo**：对于自己不确定的代码，写几个demo测试验证一下猜想。
  + **寻求帮助**：若以上三个步骤均无法找出bug，那么就可通过**搜索引擎**、著名的问答论坛、老师、同学和朋友寻求帮助。问问题时要用**简化后的代码**，节省双方时间，提高得到答案的效率！

## sh2.c: 实现shell程序

### 需求描述

要求在第1版（sh1.c）的基础上，添加如下功能

- 实现文件重定向 

  ```
  # 执行sh2
  $ ./sh2
  
  # 执行命令echo，并将输出保存到文件log中
  > echo hello >log
  
  # 打印cat命令的输出结果
  > cat log
  hello
  ```

### 思路分析

#### 思路一-仅处理内置程序

只处理外部命令，即通过 `mysys` 执行的命令，内置命令如 `pwd` 先不实现。

主要思路：

+ **字符串处理**：假设 `>outfile` 或 `> outfile` 一定在参数末尾，因此只需参数分割后，判断最后一个参数的首字符是否为 `>` 或者倒数第二个参数是否为 `">"`，取出对应的文件名，并将对应位置置空
+ **子进程重定向**：子进程会拷贝父进程的文件描述符表，因此先打开要输出的文件，然后用 `dup2(fd,1)` 将标准输出重定向到 `fd`，接着关闭 `fd`， 

其核心伪代码如下：

```c
pid_t pid = fork();
if (pid == 0) {
    int fd = open("指定的文件");
    dup2(fd,1); // 标准输出重定向为fd
    close(fd); // 关闭fd
    /* 命令执行操作 */
}
```

#### 思路二-内外部程序统一处理

将内部和外部程序**统一处理**，思路如下：

+ 在 `Shell` 处理程序中，先保存**标准输出**到 `fd1_bak`
+ 打开指定输出文件 `fd`，将**标准输出**重定向为 `fd`，并释放 `fd`
+ 执行完对应命令后，将**标准输出**重定向为 `fd1_bak`，并释放 `fd1_bak`

其核心伪代码如下：

```c
int fd1_bak = dup(1); // 备份标准输出
int fd = open("指定文件"); // 打开指定文件
dup2(fd, 1); // 标准输出重定向为fd
free(fd); // 释放fd的文件描述符
/* 这里是对应的执行命令 */
dup2(fd1_bak, 1); // 文件描述符1重定向为标准输出
free(fd1_bak); // 释放fd1_bak
```

**注意**：系统 `ls` 可区分输出到屏幕还是文件，若是输出到屏幕，需要特殊处理，比如排版和颜色；而输出到文件，直接分行输出。

### 思路一代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
// open, creat
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>

// command arguments maximum number
#define ARGNUM 50
// DEBUG switch: If don't want DEBUG, just comment it.
//#define DEBUG

/* Function: Command string parse; remove space, split string, store in argv
*  Return:   argv list
*  Attention: argv must use dynamic memory allocation , otherwise after split_cmd overed, argv is release.
*/
char **split_cmd(char *cmd_str)
{
    char **argv = (char **)malloc(sizeof(char *) * ARGNUM); // dynamic malloc !!!
    const char d[2] = " ";
    argv[0] = strtok(cmd_str, d); // argv[0]=executed file name
    int i = 1;                    // i=0 is random
    while (1) {
        if (i >= ARGNUM)
            perror("Argument num is to much");
        argv[i] = strtok(NULL, " "); // first param=NULL
        if (argv[i] == NULL)
            break;
        ++i;
    }
#ifdef DEBUG
    puts("==DEBUG== split_cmd.argv:");
    char **p = argv;
    while (*p != NULL)
    {
        printf("%s\n", *p);
        ++p;
    }
#endif
    return argv;
}
// PWD: Print calling process's current workplace directory
void mypwd()
{
    char buf[100];
    char *ret = getcwd(buf, 100);
    if (ret == NULL)
        perror("mypwd-getpwd is error");
    else
        puts(buf);
}
void mycd(char *argv[])
{
    if (argv[1] == NULL)
        perror("mycd: pathname is NULL");
    int ret = chdir(argv[1]);
    if (ret < 0)
        perror("cd error");
}
/*
    Function: Fork a child process to exec cmd
    Args:
        argv: Argument list, include cmd name and it's args
    Implementation Approach:
        1. Fork a child process
        2. By calling execvp, child exec new program
        3. Father block until child is return
*/
void mysys(char **argv)
{
    // redirect
    char outfile[100];
    int len=0;
    for (char **p = argv; *p != NULL; ++ p)
        ++ len;
    if (len >= 2) {
        if (argv[len - 1][0] == '>') { // >outfile
            strcpy(outfile, argv[len - 1] + 1);
            argv[len - 1] = NULL;
        }
        else if (strcmp(argv[len - 2], ">") == 0) { // > outfile
            strcpy(outfile, argv[len - 1]);
            argv[len - 2] = NULL;
        }
    }
    // fork
    pid_t pid = fork();
    if (pid == 0) { // child
        // redirect stdout -> outfile
        if (strlen(outfile) != 0) {
            printf("outfile = [%s]\n", outfile);
            int fd = open(outfile, O_CREAT|O_RDWR|O_TRUNC, 0666);
            dup2(fd, 1);
            close(fd);
        }
        // exec a new program
        int error = execvp(argv[0], argv);
        if (error < 0) { // child exec fail
            perror("execvp");
            exit(0);
        }
    }
    else { // father
        wait(NULL);
    }
}
// API: Handle command
void cmd_handle(char *command)
{
    // parse command
    char *cmd_str = (char *)malloc(sizeof(char) * (strlen(command) + 1)); // important!!!
    strcpy(cmd_str, command);                                             // copy const char* -> char*
    char **argv = split_cmd(cmd_str);
    // handle command
    if (strcmp(argv[0], "exit") == 0)
        exit(0);
    else if (strcmp(argv[0], "pwd") == 0)
        mypwd();
    else if (strcmp(argv[0], "cd") == 0)
        mycd(argv);
    else
        mysys(argv); // system call
    // free heap memory
    free(cmd_str);
    free(argv);
}
int main()
{
    char cmd[80];
    while (1) {
        printf(">");
        fgets(cmd, 80, stdin);
        int len = strlen(cmd);
        if (cmd[len - 1] == '\n')
            cmd[len - 1] = '\0'; // delete newline
        cmd_handle(cmd);
    }
    return 0;
}
```

### 思路二代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
// open, creat
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>

// command arguments maximum number
#define ARGNUM 50
// DEBUG switch: If don't want DEBUG, just comment it.
//#define DEBUG

/* Function: Command string parse; remove space, split string, store in argv
*  Return:   argv list
*  Attention: argv must use dynamic memory allocation , otherwise after split_cmd overed, argv is release.
*/
char **split_cmd(char *cmd_str)
{
    char **argv = (char **)malloc(sizeof(char *) * ARGNUM); // dynamic malloc !!!
    const char d[2] = " ";
    argv[0] = strtok(cmd_str, d); // argv[0]=executed file name
    int i = 1;                    // i=0 is random
    while (1) {
        if (i >= ARGNUM)
            perror("Argument num is to much");
        argv[i] = strtok(NULL, " "); // first param=NULL
        if (argv[i] == NULL)
            break;
        ++i;
    }
#ifdef DEBUG
    puts("==DEBUG== split_cmd.argv:");
    char **p = argv;
    while (*p != NULL)
    {
        printf("%s\n", *p);
        ++p;
    }
#endif
    return argv;
}
// PWD: Print calling process's current workplace directory
void mypwd()
{
    char buf[100];
    char *ret = getcwd(buf, 100);
    if (ret == NULL)
        perror("mypwd-getpwd is error");
    else
        puts(buf);
}
void mycd(char *argv[])
{
    if (argv[1] == NULL)
        perror("mycd: pathname is NULL");
    int ret = chdir(argv[1]);
    if (ret < 0)
        perror("cd error");
}
/*
    Function: Fork a child process to exec cmd
    Args:
        argv: Argument list, include cmd name and it's args
    Implementation Approach:
        1. Fork a child process
        2. By calling execvp, child exec new program
        3. Father block until child is return
*/
void mysys(char **argv)
{
    // fork
    pid_t pid = fork();
    if (pid == 0) { // child
        // exec a new program
        int error = execvp(argv[0], argv);
        if (error < 0) { // child exec fail
            perror("execvp");
            exit(0);
        }
    }
    else { // father
        wait(NULL);
    }
}
// API: Handle command
void cmd_handle(char *command)
{
    // parse command
    char *cmd_str = (char *)malloc(sizeof(char) * (strlen(command) + 1)); // important!!!
    strcpy(cmd_str, command);                                             // copy const char* -> char*
    char **argv = split_cmd(cmd_str);

    // redirect
    char outfile[100]={'\0'}; // must init!!!
    int len=0;
    for (char **p = argv; *p != NULL; ++ p)
        ++ len; // printf("argv[%d] = %s\n", len - 1, *p);
    if (len >= 2) {
        if (argv[len - 1][0] == '>') { // >outfile
            strcpy(outfile, argv[len - 1] + 1);
            argv[len - 1] = NULL;
        }
        else if (strcmp(argv[len - 2], ">") == 0) { // > outfile
            strcpy(outfile, argv[len - 1]);
            argv[len - 2] = NULL;
        }
    }
    int fd1_bak = -1;
    // redirect stdout -> outfile
    if (strlen(outfile) != 0) {
        printf("outfile = [%s]\n", outfile);
        fd1_bak = dup(1); // save stdout
        int fd = open(outfile, O_CREAT|O_RDWR|O_TRUNC, 0666);
        //printf("fd = %d\n", fd);
        dup2(fd, 1);
        close(fd);
    }
    // handle command
    if (strcmp(argv[0], "exit") == 0)
        exit(0);
    else if (strcmp(argv[0], "pwd") == 0)
        mypwd();
    else if (strcmp(argv[0], "cd") == 0)
        mycd(argv);
    else
        mysys(argv); // system call

    if (fd1_bak != -1) {
        dup2(fd1_bak, 1);
        close(fd1_bak);
    }
    //printf("release fd1_bak\n");
    // free heap memory
    free(cmd_str);
    free(argv);
}
int main()
{
    char cmd[80];
    while (1) {
        printf(">");
        fgets(cmd, 80, stdin);
        int len = strlen(cmd);
        if (cmd[len - 1] == '\n')
            cmd[len - 1] = '\0'; // delete newline
        cmd_handle(cmd);
    }
    return 0;
}
```



## sh3.c: 实现shell程序，要求在第2版的基础上，添加如下功能

### 需求描述

- 实现管道

- 只要求连接两个命令，不要求连接多个命令

- 不要求同时处理管道和重定向

  ```
  # 执行sh3
  $ ./sh3
  
  # 执行命令cat和wc，使用管道连接cat和wc
  > cat /etc/passwd | wc -l
  ```

- 考虑如何实现管道和文件重定向，暂不做强制要求

  ```
  $ cat input.txt
  3
  2
  1
  3
  2
  1
  $ cat <input.txt | sort | uniq | cat >output.txt
  $ cat output.txt
  1
  2
  3
  ```

### 遇见问题

> 父进程创建管道，子进程向管道写数据，父进程读数据，若是父进程中没有close(fd[1])，则父进程会卡死在execvp中？

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() 
{
    int fd[2];
    char buf[50];
    pipe(fd);
    int pid = fork();
    if (pid == 0) {
        dup2(fd[1], 1);
        close(fd[0]);
        close(fd[1]);
        char *argv[] = {"cat", "/etc/passwd", NULL};
        execvp(argv[0], argv);
    }
    dup2(fd[0], 0);
    wait(NULL);
    printf("father\n");
//    close(fd[0]);
//    close(fd[1]); // ??注释掉这句程序就无响应了，卡在execvp中
    char *argv[] = {"wc", "-l", NULL};
    execvp(argv[0], argv);
    return 0;
}
```

错误代码二：父进程的写端必须关闭子进程才能不阻塞？

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    int fd[2];
    char buf[50];
    pipe(fd);
    int pid1 = fork();
    if (pid1 == 0) {
        puts("process1 begin");
        dup2(fd[1], 1);
        close(fd[0]);
        close(fd[1]);
        char *argv[] = {"cat", "/etc/passwd", NULL};
        execvp(argv[0], argv);
    }
    wait(NULL);
    puts("process1 end");
    int pid2 = fork();
    if (pid2 == 0) {
        puts("process2 begin");
        dup2(fd[0], 0);
        close(fd[0]);
        close(fd[1]);
        char *argv[] = {"wc", "-l", NULL};
        execvp(argv[0], argv);
    }
    puts("----before wait---- fork2 father begin");
    close(fd[1]); // 这一句注释掉wait陷入忙等，无反应
    wait(NULL);
    puts("process2 end");
    return 0;
}
```



# 常见问题

> 命令行中 int main() 如何传递参数

```c
#include <stdio.h>
// argc是agrv长度，第一个字符串为文件名
int main(int argc, char* agrv[]) {
    for (int i=0; i < argc; ++ i) {
		printf("%d %s", i, argv[i]);
    }
}
```

> open创建文件时，无写权限

调用系统函数创建文件时，加上`mode`来设置访问权限。`ls -l`可看到详细的文件信息，包括访问权限。

```c
open("filename",O_RDWT|O_CREAT, mode); // mode=0777表示访问权限（8进制）
creat("filename",mode); // 创建文件：是对open的封装
```

> 实验楼打印字符串到终端末尾出现% 

末尾加上换行符即可。不同系统处理不同，阿里云就不会。

> 库函数不同于系统调用

系统调用是OS提供的最基础的函数，C语言库函数是对系统调用的封装，提供更加便利丰富的功能。

> mycat无法打印/etc下的文件（**系统文件**）

以读写方式打开/etc下的文件，由于普通用户对该目录只有**只读权限**，因此会打开文件失败。

所以只能以**只读方式**打开对应文件。

> myecho无写入文件权限

打开要写入数据的文件时，可能该文件不存在，必须要创建，需指定`mode=0777`参数，设置权限。

> 开辟静态数组，大小必须为常量

+ C89/C++不支持变量申请数组，而**C99/C11**支持变长定义数组。
+ 有的编译器将其作为扩展功能
+ 结论：静态数组只用常量作为大小。想变量申请有两种方法
  + C：malloc
  + C++：new/malloc；vector

> **const char* 遇上 strtok 的巨坑**

必须把const char*对应的内容复制到char\*，再进行strtok操作（他会改变第一个参数，变态！）

> Linux 下 ls 命令后可以跟文件名，若文件名带有空格如何处理？

用**单引号/双引号括起带空格的文件名**，如`ls 'a b c'`，显示名为 `a b c` 的文件内容。

实现考虑：自己写 shell 时以空格分割字符串，这里需要特殊考虑

+ 思路1：
  + 预处理：先把所有单引号/双引号内部的空格字符替换成非空格字符，如 `$` 
  + `strtok`：以空格作为分割符进行分割
  + 空格还原：对于分割后得到的每个字符串，将其中标记字符改为空格：
  + 优缺点：优点在于简单易实现；缺点在于必须保证文件名不包含**标记字符**。
+ 思路2：手写字符串分割处理函数
  + 单/双引号的处理
    + 单/双引号内部所有的字符都会保持字面值
    + 若存在内部嵌套混用单双引号，必须使用转义字符的格式。如`touch a\'b\" cd\'`
  + 转义符`\`

> printf字符串后无换行，无法输出，而加上换行，即可输出。

printf 的实现本质是调用系统调用 write ，而它并不是 printf 一次， 就调用一次 write， 而是维护一个缓冲区，直到碰见换行符，才使用 write 一次性输出。

关键在于`execl`会清空地址空间，导致buf中的数据全部丢失，所以无法输出

```c
//#include <stdio.h>
#include <unistd.h> 
#include <string.h>

static char buf[12345]; // printf的缓冲区
int idx = 0; // 缓冲区当前可用下标

void printf(char *msg)
{
    for (char *p = msg; *p ; p++) {
        buf[idx] = *p;
        idx++;

        if (*p == '\n') { // 遇到换行，write输出
            write(1, buf, idx);
            idx = 0;
        }
    }
}

int main()
{ 
    printf("before exec");
    int error = execl("/bin/echo", "echo", "a", "b", "c", NULL);
    if (error < 0)
        perror("execl");
    printf("after exec\n");
    return 0;
}
// 输出结果
// a b c
```



# 参考资料

[简明命令参考](http://cyc2018.gitee.io/cs-notes/#/notes/Linux)

《鸟叔的linux私房菜》：命令，使用

《Unix环境高级编程》-APUE：编程接口

[The C Programming Language](https://book.douban.com/subject/1139336/)：C语言程序设计经典，学习其编码风格

xv6，ucore

[课程录播回放](https://www.linuxmooc.com/teach/os/)

[课程地址](https://www.linuxmooc.com/)

[程序装载和链接](https://h5.dingtalk.com/group-live-share/index.htm?encCid=60848862d5c602a4a550b0181a94cdcd&liveUuid=935a8677-b433-4100-88c8-1f5832fcc38a&tdsourcetag=s_pctim_aiomsg#/)

CCF考题

+ 第一题：单循环+分支语句
+ 第二题：多重循环；常需要排序，结构体数组，时序问题
+ 第三题：字符串解析，命令行选项解析，格式文件解析（代码量大，调试能力，分解能力）
+ 第四题：图论算法（最小生成树，最短路，强联通分量，欧拉函数）；动态规划（简单递推，四边形优化）；dfs非递归写法（避免爆栈）
+ 第五题：挺难，动态规划

STL

+ vector
+ list
+ map
+ priority_queue

测试数据（freopen）

+ 手动设计边界样例

+ 随机数据生成
+ 小规模：测试正确性，避免思维定势
+ 大规模：测试时间是否超时/查bug

调试技巧

分段设计算法

+ 不同数据设计不同的算法
  + 贪心，搜索等暴力算法得部分分数
  + 数学规律
  + **分类讨论**（面向测试数据编程）
  + 毫无思路，输出固定值
+ 特例直接判断输出
+ **拿部分分，不追求满分**
+ 仔细简化条件，拿部分分数，考虑特殊输出

理解算法，不要去背代码

