# Shell语法

## 解释器

最常用的解释器是bash 和sh

~~~shell
#!/bin/bash    #最常用的shell是bash
#!/bin/sh
sh test.sh & #&后台执行shell
~~~

## 多行注释

EOF可以换成其他标志

~~~shell
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
~~~

## 变量

字符串

```shell
name="hjx" #变量名和等号之间不能有空格,不能使用关键字、空格，不能使用数字开头
echo $name 
echo ${name}  #推荐使用${}

string="runoob is a great site"
echo ${#string} #获取字符串长度
echo ${string:1:4} #截取字符串，第一个字符的索引值为 0
FINAL=`echo ${STR: -1}` #取字符串最后一位
```

只读变量

```shell
myUrl="https://www.google.com"
readonly myUrl  #readonly定义只读变量
myUrl="https://www.runoob.com" # 报错，只读变量不能被更改 
unset name  #删除变量
```

局部变量

```shell
[function] hjx(){
	local hjx=99 #local定义局部变量 ，shell 默认是全局变量，没有则为空
}
```

单双引号区别

```shell
#单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
#双引号里可以有变量，双引号里可以出现转义字符
#字符串拼接直接拼接，不需要+
```

数组

```shell
array_name=(value0 value1 value2 value3) #定义shell数组
array_name=(value0 
    value1 
    value2
    value3
    )
valuen=${array_name[n]} #读取下标
echo ${array_name[@]} #获取数组全部元素
# 取得数组元素的个数
length=${#array_name[@]} || length=${#array_name[*]}
lengthn=${#array_name[n]} # 取得数组单个元素的长度
# 遍历数组
for ele in ${array_name[@]}
do
	commond
done
```

## 参数传递

```shell
$0  #当前脚本的文件名
$n(n>=1) #传递给脚本或函数的参数
$# #传递给脚本或函数的参数个数
$* #传递给脚本或函数所有参数 不可迭代
$@ #传递给脚本或函数所有参数 ，当参数有""时 $@和$*有所不同 “$*”会把所以参数当成一个 “$@”会迭代
$? #上一条命令的执行结果，如果是执行的命令是0正常 1异常 ,return 0代表函数正常终止,其他异常终止
$$ #显示当前shell的进程ID
$- #显示Shell使用的当前选项，与set命令功能相同，不常用。
$? #上一条命令的运行状态
```

## 运算符

算术运算符

```shell
#整数使用expr 、let ，小数使用bc
+、-、*、/、%、
=（#赋值，=左右两边不能有空格）
==	#相等。用于比较两个数字，相同则返回 true。	[ $a == $b ] 返回 false。
!=	#不相等。用于比较两个数字，不相同则返回 true。	[ $a != $b ] 返回 true。

expr `1 + 1`
let a++
echo "2.2+2.5"|bc
```

关系运算符

~~~shell
-eq 相等
-ne 不相等
-gt 大于（左大于右）
-ge 大于等于
-lt 小于
-le 小于等于
~~~

布尔运算符

~~~shell
！非运算
-a 与运算
-o 或运算
~~~

逻辑运算符

~~~shell
&& 逻辑与
|| 逻辑或
~~~

字符串运算符

~~~shell
=  相等[$a = $b]  #shell 是一个 =
!=  不等[$a != $b]
-z  检测字符串长度是否为0 ?为0true
-n  检测字符串长度是否为0 ?不为0 ture
$str 检测字符串是否为空?不为空true
~~~

文件测试常用运算符

~~~shell
-d file 检测是否是目录
-s file 检测文件是否为空（文件大小是否大于0），不为空返回 true。
-e file 检测目录或文件是否存在
-r -w -x file 检测文件是否可读写执行
-f file 检测文件是否是普通文件
~~~

## test

~~~shell
#test命令检测条件是否成立
if test $a = $b ;then
	Command
fi
$[a+b]=expr $a + $b
~~~

## 函数

~~~shell
[ function ] funname [()]
{
    action;
    [return int;]
}
#调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数
~~~

## 控制流程

~~~shell
if condition
then
    ....
elif
then
    ....
else
    ....
fi

#for 循环
#break 用于跳出循环
#continue 继续循环，不执行循环体后面语句
for var in item1 item2 item3 ...
do
   ....
done

for((;;))
do
   ....
done
#while
while condition
do
   ....
done

#无限循环
while :
do
    command
done

while true
do
    command
done

for (( ; ; ))
do
	command
done

#until循环
#until 循环执行一系列命令直至条件为 true 时停止。
#until 循环与 while 循环在处理方式上刚好相反。
until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done

#case ... esac
#case ... esac 为多选择语句，与其他语言中的 switch ... case 语句类似，是一种多分枝选择结构，每个 case 分支用右圆括号开始，用两个分号 ;; 表示 break，即执行结束，跳出整个 case ... esac 语句，esac（就是 case 反过来）作为结束标记。
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac

case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
~~~

## 输入输出重定向

~~~shell
command > file #输出重定向（覆盖）
command >> file #将输出以追加的方式重定向到 file。
command1 < file1 #输入重定向

command1 < infile > outfile
#同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

#重定向深入讲解
#一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：
#标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
#标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
#标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

#默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。
command 2> file # stderr 重定向到 file (会打印到屏幕 2> 不能分开写) 

#如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写（ 2>&1 不能分开写 ）：
command > file 2>&1
command >> file 2>&1  

#Here Document
command << delimiter
    document
delimiter

wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
#它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

#/dev/null 文件，/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；起到禁止输出的目的
command > /dev/null 2>&1  #屏蔽 stdout 和 stderr 
~~~

## 文件包含

~~~shell
. filename  # 注意点号(.)和文件名中间有一空格
source filename
~~~

# 常用命令

## chsh

/etc/shells 文件记录当前用户安装了那些shell，可以使用 `chsh -l` 和 `cat /etc/shells`查看当前环境可以使用的shell

```shell
cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/usr/bin/tmux
/usr/bin/screen

# chsh -l 命令有的版本不支持，其本质上也是 cat /etc/shells 文件
# 最常用的shell是bash, zsh它兼容 bash，还有自动补全等好用的功能。zsh 的配置文件\~/.zshrc 
```

使用 chsh 命令的`-s`选项就可以修改登录的 Shell 了。

```shell
chsh -s /bin/zsh
Changing shell for roc.
Password:
Shell changed.

#退出并重新登录后，终于看到了我们想要的 /bin/zsh 了
echo "$SHELL"
/bin/zsh
```

## export

export 可以查看（显示）Shell 环境变量

```shell
export
declare -x DISPLAY="localhost:10.0"
declare -x HISTSIZE="3000"
declare -x HISTTIMEFORMAT="%F %T "
declare -x HOME="/home/ubuntu"
```

> export/set/env/declare 的区别, 不带参数可以直接看，设置的环境变量

- env 和 export 显示的是环境变量。
- set 和 declare 显示的是环境变量和自定义变量。

```shell
export PATH=$PATH:/home/roc/operation_tools
```

## read

读取从键盘或文件输入的数据，常用参数

```shell
-a array 将读取的单词分配给数组的顺序索引变量 ARRAY，从零开始
-d delim 继续直到读到 DELIM 的第一个字符，而不是比换行符
-e 使用 Readline 获取交互式 shell 中的行
-p prompt 输出之前没有尾随换行符的字符串 PROMPT 尝试阅读
-r 不允许反斜杠转义任何字符
-s 不回显来自终端的输入
-t timeout 超时并返回失败
-u 从文件描述符 FD 读取而不是标准输入
```

读取用户输入

```shell
#!/bin/bash
echo -n "please input your name:"
read name
echo "welcome !!! $name"
exit 0
```

读取密码 `-s 不回显`

```shell
#!/bin/bash
read -s -p "please input your code:" password
echo "hehe, your password is $password"
```

读取文件 `-u` 

~~~shell
#! /bin/bash
exec 3< test.txt

while read -u 3 var
do
    echo "$var"
done
echo "finished"
 
exec 3<&-
# 通过“exec 3<test.txt”生成了编号为 3 的文件描述符，接着通过“read-u 3 var”来读取文件内容。最后通过“exec 3<&-”关闭了 3 号文件描述符。
~~~

读取文件管道

```shell
#!/bin/bash
count=1
cat test.txt | while read line
do
echo "Line $count:$line"
let count=$count+1
done
echo "finished"
echo "Line no is $count"
exit 0
```

读取文件重定向 `个人比较常用`

```shell
#!/bin/bash
count=0
while read line
do
let count=$count+1
echo "Line $count:$line"
done < test.txt
echo "finished"
echo "Line no is $count"
exit 0
```

## expr

用于linux命令下的四则运算和字符串运算

四则运算

```shell
expr 10 + 10
expr 10 - 10
expr 20 / 10
expr 10 \* 10 # * 需要转义
expr \( 10 + 10 \) \* 2 + 100
```

字符串运算

```shell
match	中匹配 REGEXP 字符串并返回匹配字符串的长度
substr	从 POS 位置获取长度为 LENGTH 的字符串
index	杳找子字符串的起始位置
length	计算字符串的长度

expr match "123 456 789" ".*5"  # 6 匹配字符串的长度，若找不到则返回 0
expr substr " this is a test" 3 5 # his i 
expr index "test for the game" "e" # 2 
expr length "this is a test" # 14
```

## tmux

linux 一个终端窗口可以操作多个会话，部分系统需要安装此命令，不常用

## alias

给命令定义别名

~~~shell
# 查看
alias
alias ll='ls -alF --color=auto'

# 设置
alias ll='ls -alF --color=auto'

# 取消
unalias ll
~~~

## history

查看和执行历史命令

`-c` 清理历史命名，比较重要

```shell
history -c
```

## xargs

参数传递过滤器，配合着管道使用

常用参数

~~~shell
-n	多行输出
-d	自定义一个定界符
-I	指定一个替换字符串{}
-t	打印出 xargs 执行的命令
-p	执行每一个命令时弹出确认

cat test.txt | xargs -n3
cat arg.txt | xargs -I {} ./sk.sh -p {} -l
~~~

## time

测量命令的执行时间

```shell
time ls
go  hjx

real    0m0.008s
user    0m0.001s
sys     0m0.000s

(1) real：从进程 ls 开始执行到完成所耗费的 CPU 总时间。该时间包括 ls 进程执行时实际使用的 CPU 时间，ls 进程耗费在阻塞上的时间（如等待完成 I/O 操作）和其他进程所耗费的时间（Linux 是多进程系统，ls 在执行过程中，可能会有别的进程抢占 CPU）。
(2) user：进程 ls 执行用户态代码所耗费的 CPU 时间。该时间仅指 ls 进程执行时实际使用的 CPU 时间，而不包括其他进程所使用的时间和本进程阻塞的时间。
(3) sys：进程 ls 在内核态运行所耗费的 CPU 时间，即执行内核系统调用所耗费的 CPU 时间
```

系统资源的使用情况 `\time -v command`

```shell
\time -v ls
go  hjx
        Command being timed: "ls"
        User time (seconds): 0.00
        System time (seconds): 0.00
        Percent of CPU this job got: ?%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.00
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 2408
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 104
        Voluntary context switches: 1
        Involuntary context switches: 2
        Swaps: 0
        File system inputs: 0
        File system outputs: 0
        Socket messages sent: 0
        Socket messages received: 0
        Signals delivered: 0
        Page size (bytes): 4096
        Exit status: 0
```



## sleep

让shell程序暂停或休眠一段时间长度，后面可接 s、m、h 或 d

```shell
sleep 5m
```

## file

查看文件信息或类型

```shell
-b	列出辨识结果时，不显示文件名称 (简要模式)
-L	直接显示符号连接所指向的文件类别
-i	显示MIME类别

file hurl.go
hurl.go: C source, ASCII text

file -i hurl.go
hurl.go: text/x-c; charset=us-ascii
```

## ln

创建文件链接，连接分为软链接和硬链接，链接文件共有一套底层数据

软连接：L开始，删除源文件，会影响链接文件
硬链接：和源文件一样的基本信息，删除原文件对链接文件没影响

```shell
ln hurl.go hurl.go.link # 硬链接
ln -s hurl.go hurl.go.link # 软链接
ln -F /home/hjx /home/hjxlink # 目录链接
```

## du

查看文件夹和文件的磁盘占用情况

```shell
-a	显示目录中所有文件大小
-k	以KB为单位显示文件大小
-m	以MB为单位显示文件大小
-g	以GB为单位显示文件大小
-h	以易读方式显示文件大小
-s	仅显示总计
--max-depth 输出最大深度

du -hc --max-depth=1 .
du log2012.log 
du -sh *|sort -nr
```

## df

用于显示系统上可使用的磁盘空间

```shell
-a	显示所有系统文件
-B <块大小>	指定显示时的块大小
-h	以容易阅读的方式显示
-H	以1000字节为换算单位来显示
-i	显示索引字节信息
-k	指定块大小为1KB
-l	只显示本地文件系统
-t <文件系统类型>	只显示指定类型的文件系统

df -h
```

## gzip

压缩和解压文件（.gz文件）,gzip 只能针对普通文件（regular file）进行压缩和解压，对于文件夹、符号链接等是不支持的。

```shell
-a	使用ASCII文字模式
-d	解开压缩文件  
-f	强行压缩文件
-l	列出压缩文件的相关信息
-c	把压缩后的文件输出到标准输出设备，不去更动原始文件
-r	递归处理，将指定目录下的所有文件及子目录一并处理 # 对目录下的单个文件
-q	不显示警告信息

gzip hero.avi
gzip -d hero.avi.gz
```

## tar

打包和备份的归档工具 

```shell
-A	新增文件到以存在的备份文件
-B	设置区块大小
-c	建立新的备份文件
-C <目录>	切换工作目录，先进入指定目录再执行压缩/解压缩操作，可用于仅压缩特定目录里的内容或解压缩到特定目录
-d	记录文件的差别
-x	从归档文件中提取文件
-t	列出备份文件的内容
-z	通过gzip指令压缩/解压缩文件，文件名最好为*.tar.gz
-Z	通过compress指令处理备份文件
-f<备份文件>	指定备份文件
-v	显示指令执行过程
-r	添加文件到已经压缩的文件
-u	添加改变了和现有的文件到已经存在的压缩文件
-j	通过bzip2指令压缩/解压缩文件，文件名最好为*.tar.bz2
-v	显示操作过程
-l	文件系统边界设置
-k	保留原有文件不覆盖
-m	保留文件不被覆盖
-w	确认压缩文件的正确性
-p	保留原来的文件权限与属性
-P	使用文件名的绝对路径，不移除文件名称前的“/”号
-N <日期格式>	只将较指定日期更新的文件保存到备份文件里
-- -exclude=<范本样式>	排除符合范本样式的文件
-- -remove-files	归档/压缩之后删除源文件

tar -zcvf mygzipfile.tar.gz curl-7.34.0
tar -zxvf curl-7.34.0.tar.gz
```

## bzip2

压缩和解压文件

由于 bzip2 与 gzip 相比，其压缩稳定性和效果都更好，对应单个文件bzip2 命令和 bunzip2 命令，就可以实现了

```shell
 bzip2 abc1.txt 
 bunzip2 A.txt.bz2
 
 tar -xjvf curl-7.47.1.tar.bz2 /dir
 tar -zvf curl-7.47.1.tar.bz2
```

## zip

压缩和解压文件（.zip文件）, zip 会保留原始文件 gzip 和 bzip2 不会保留

```shell
-r	递归处理，将指定目录下的所有文件和子目录一并处理

zip -r zdata.zip mynote.txt soft/
unzip -d newdir/ zdata.zip
```

## dd

复制（拷贝）文件，并对原文件进行转换，作为了解吧

```shell
dd if=/dev/sda of=/root/sda.img
```

## grep

文本搜索工具（可使用正则表达式）

```shell
-a 或 --text : 不要忽略二进制的数据。
-A<显示行数> 或 --after-context=<显示行数> : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
-b 或 --byte-offset : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
-B<显示行数> 或 --before-context=<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前的内容。
-c 或 --count : 计算符合样式的列数。
-C<显示行数> 或 --context=<显示行数>或-<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
-d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
-e<范本样式> 或 --regexp=<范本样式> : 指定字符串做为查找文件内容的样式。
-E 或 --extended-regexp : 将样式为延伸的正则表达式来使用。
-f<规则文件> 或 --file=<规则文件> : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
-F 或 --fixed-regexp : 将样式视为固定字符串的列表。
-G 或 --basic-regexp : 将样式视为普通的表示法来使用。
-h 或 --no-filename : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
-H 或 --with-filename : 在显示符合样式的那一行之前，表示该行所属的文件名称。
-i 或 --ignore-case : 忽略字符大小写的差别。
-l 或 --file-with-matches : 列出文件内容符合指定的样式的文件名称。
-L 或 --files-without-match : 列出文件内容不符合指定的样式的文件名称。
-n 或 --line-number : 在显示符合样式的那一行之前，标示出该行的列数编号。
-o 或 --only-matching : 只显示匹配PATTERN 部分。
-q 或 --quiet或--silent : 不显示任何信息。
-r 或 --recursive : 此参数的效果和指定"-d recurse"参数相同。
-s 或 --no-messages : 不显示错误信息。
-v 或 --invert-match : 显示不包含匹配文本的所有行。
-V 或 --version : 显示版本信息。
-w 或 --word-regexp : 只显示全字符合的列。
-x --line-regexp : 只显示全列符合的列。
-y : 此参数的效果和指定"-i"参数相同

grep -i "leo" /etc/passwd // 忽略大小写
grep -v "leo" /etc/passwd
```

## cut

剪切文件中的数据,一般用awk 替代

```shell
-b	以字节为单位进行分割 ,仅显示行中指定直接范围的内容
-c	以字符为单位进行分割 , 仅显示行中指定范围的字符
-d	自定义分隔符，默认为制表符”TAB”
-f	显示指定字段的内容 , 与-d一起使用
-n	取消分割多字节字符

cut -f 2 student.txt 
```

## wc

计算单个文件中的字数、单词数和字节数

```shell
-w	统计字数，或--words：只显示字数。一个字被定义为由空白、跳格或换行字符分隔的字符串
-c	统计字节数，或--bytes或--chars：只显示Bytes数
-l	统计行数，或--lines：只显示列数
-m	统计字符数
-L	打印最长行的长度

wc -l hjx.txt
```

## split

切割（拆分）文件

```shell
-<行数> : 指定每多少行切成一个小文件
-b<字节> : 指定每多少字节切成一个小文件
-C<字节> : 与参数"-b"相似，但是在切 割时将尽量维持每行的完整性

split -b 400M king_of_ring.avi
cat xaa xab > king_of_ring_merge.avi # 合并
#合并后记得校验和原文件是否一致 md5sum 或者sha256
```

## paste

合并（拼接）文件，`了解即可`

```shell
$ cat s1.txt
a
b
c
d
e

$ cat s2.txt
1
2
3

paste s1.txt s2.txt
a       1
b       2
c       3
d
e
```

## vi 、 vim

终端文本编辑命令，了解即可

/word	向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！ (常用)
?word	向光标之上寻找一个字符串名称为 word 的字符串。

:1,$s/word1/word2/g 或 :%s/word1/word2/g	从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！(常用)
:1,$s/word1/word2/gc 或 :%s/word1/word2/gc	从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！

dd	删除游标所在的那一整行(常用)
ndd	n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 (常用)

u	复原前一个动作。(常用)
[Ctrl]+r	重做上一个动作。(常用)

## echo

显示文字并给文字添加颜色

```shell
-n	不输出结尾的换行符
-e  允许转义
-E	禁止反斜杠转移，与-e参数功能相反

echo "Hello World\n" # Hello World\n
echo -e "Hello World\n"
echo -e "\033[颜色1;颜色2m 要展示的文字 \033[0m"
```

## sed

替换、删除、更新文件中的内容

```shell
# 参数说明
-n 使用安静silent模式。在一般sed的用法中，所有来自stdin的内容一般都会被列出到屏幕上。但如果加上-n参数后，则只有经过sed特殊处理的那一行(或者动作)才会被列出来
-e 直接在指令列模式上进行 sed 的动作编辑
-f 直接将 sed 的动作写在一个文件内，-f filename则可以执行filename内的sed命令
-r 让sed命令支持扩展的正则表达式(默认是基础正则表达式)
-i 直接修改读取的文件内容，而不是由屏幕输出

# 动作说明
a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！
```

替换

```shell
sed 's/test/trial/g' data4.txt
```

-n 选项会禁止 sed 输出，但 p 标记会输出修改过的行，

```shell
sed -n 's/test/trial/p' data5.txt
This is a trial line.

```

## awk

awk 是一种处理文本文件的语言，是一个强大的文本分析工具。

```shell
awk '{[pattern] action}' {filenames}   # 行匹配语句 awk '' 只能用单引号
awk '{print $1,$4}' log.txt  # 每行按空格或TAB分割，输出文本中的1、4项
awk -F  #-F相当于内置变量FS, 指定分割字符,可以指定多个
awk -F, '{print $1,$2}'   log.txt
awk -F '[ ,]'  '{print $1,$2,$5}' log.txt
awk 'BEGIN{FS=","} {print $1,$2}' log.txt  #可以使用内建变量

awk -v  # 设置变量
awk -va=1 '{print $1,$1+a}' log.txt

awk '$1>2' log.txt    #过滤第一列大于2的行

$n	当前记录的第n个字段，字段间由FS分隔
$0	完整的输入记录)
FILENAME	当前文件名
NF	一条记录的字段的数目 ,#分割后字段总数
NR	已经读出的记录数，就是行号，从1开始

cat label_movie2|grep "hjx"|awk -F "," '{for(i=2;i<NF;i++) print $i}'
```

## cat

在终端设备上显示文件内容

```shell
-n	显示行数（空行也编号）
-b	显示行数（空行不编号）
-E	每行结束处显示$符号
-T	将TAB字符显示为 ^I符号
-v	使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外
-A	等价于 -vET组合
```

## cp

复制文件或目录

```shell
-f	若目标文件已存在，则会直接覆盖原文件
-i	若目标文件已存在，则会询问是否覆盖
-p	保留源文件或目录的所有属性
-r	递归复制文件和目录
-d	当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录
-l	对源文件建立硬连接，而非复制文件
-s	对源文件建立符号连接，而非复制文件
-b	覆盖已存在的文件目标前将目标文件备份
```

## mkdir

创建目录

```shell
-p	递归创建多级目录
-m	建立目录的同时设置目录的权限
-z	设置安全上下文

mkdir -p linuxcool/dir
mkdir -m 700 /usr/linuxcool/dir
```

## mv

移动或改名文件

```shell
-i	若存在同名文件，则向用户询问是否覆盖
-f	覆盖已有文件时，不进行任何提示
-b	当文件存在时，覆盖前为其创建一个备份
-u	当源文件比目标文件新，或者目标文件不存在时，才执行移动此操作

mv /dir1 /dir2 #dir2 不存在时则改名
```

## rm

移除文件或目录

```shell
-f	忽略不存在的文件，不会出现警告信息
-i	删除前会询问用户是否操作
-r/R	递归删除
```

## pwd

 显示当前路径

```shell
pwd
/home/ubuntu/hjx/sftptest
```

## ssh

安全连接客户端

```shell
-1	强制使用ssh协议版本1
-2	强制使用ssh协议版本2
-4	强制使用IPv4地址
-6	强制使用IPv6地址
-A	开启认证代理连接转发功能
-a	关闭认证代理连接转发功能
-b<IP地址>	使用本机指定的地址作为对位连接的源IP地址
-C	请求压缩所有数据
-F<配置文件>	指定ssh指令的配置文件，默认的配置文件为“/etc/ssh/ssh_config”
-f	后台执行ssh指令
-g	允许远程主机连接本机的转发端口
-i<身份文件>	指定身份文件（即私钥文件）
-l<登录名>	指定连接远程服务器的登录用户名
-N	不执行远程指令
-o<选项>	指定配置选项
-p<端口>	指定远程服务器上的端口
-q	静默模式，所有的警告和诊断信息被禁止输出
-X	开启X11转发功能
-x	关闭X11转发功能
-y	开启信任X11转发功能

ssh root@172.16.6.160
ssh -l test 202.102.220.88
```

## rpm

RPM软件包管理器 , 了解即可

## find

查找和搜索文件

```shell
-name	按名称查找
-size	按大小查找
-user	按属性查找
-type	按类型查找
-iname	忽略大小写
-mount, -xdev : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件
-amin n : 在过去 n 分钟内被读取过
-anewer file : 比文件 file 更晚被读取过的文件
-atime n : 在过去n天内被读取过的文件
-cmin n : 在过去 n 分钟内被修改过
-cnewer file :比文件 file 更新的文件
-ctime n : 在过去n天内被修改过的文件
-empty : 空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name
-ipath p, -path p : 路径名称符合 p 的文件，ipath 会忽略大小写
-name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写
-size n : 文件大小 是 n 单位，b 代表 512 位元组的区块，c 表示字元数，k 表示 kilo bytes，w 是二个位元组。
-type c : 文件类型是 c 的文件。
d: 目录
c: 字型装置文件
b: 区块装置文件
p: 具名贮列
f: 一般文件
l: 符号连结
s: socket
-pid n : process id 是 n 的文件

find /var/log -iname "*.log" #在/var/log目录下忽略大小写查找以.log结尾的文件名
find . -type f -amin +10 # 当前目录访问时间超过10分钟的所有文件
find /home ! -name "*.txt" # /home下不是以.txt结尾的文件
```

## tail命令

查看文件尾部内容

```shell
-f 循环读取
-q 不显示处理信息
-v 显示详细的处理信息
-c<数目> 显示的字节数
-n<行数> 显示文件的尾部 n 行内容
--pid=PID 与-f合用,表示在进程ID,PID死掉之后结束
-q, --quiet, --silent 从不输出给出文件名的首部
-s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒

tail -f notes.log
tail -f -s 8 notes.log
```

## mount

 文件系统挂载

```shell
-t	指定挂载类型
-l	显示已加载的文件系统列表
-n	加载没有写入文件“/etc/mtab”中的文件系统
-r	将文件系统加载为只读模式
-a	加载文件“/etc/fstab”中描述的所有文件系统

mount /dev/cdrom /mnt
mount -t nfs /123 /mnt

# 取消挂载
umount -v /dev/sda1 
```

## rmdir

删除空目录，一般用rm 替代

```shell
-p	用递归的方式删除指定的目录路径中的所有父级目录，非空则报错
--ignore-fail-on-non-empty	忽略由于删除非空目录时导致命令出错而产生的错误信息
```

## netstat

显示网络状态

```shell
-a	显示所有连线中的Socket
-p	显示正在使用Socket的程序识别码和程序名称
-u	显示UDP传输协议的连线状况
-i
显示网络界面信息表单
-n	直接使用IP地址，不通过域名服务器

netstat -anp |grep 6410
```

## fdisk 

磁盘分区

```shell
# 必要参数：
-l 列出素所有分区表
-u 与 -l 搭配使用，显示分区数目

# 选择参数：
-s<分区编号> 指定分区
-v 版本信息

# 菜单操作说明
m ：显示菜单和帮助信息
a ：活动分区标记/引导分区
d ：删除分区
l ：显示分区类型
n ：新建分区
p ：显示分区信息
q ：退出不保存
t ：设置分区号
v ：进行分区检查
w ：保存修改
x ：扩展应用，高级功能

fdisk -l
fdisk /dev/sdb
```

## curl

文件传输工具

```shell
# 调试类
-v, --verbose                          输出信息
-q, --disable                          在第一个参数位置设置后 .curlrc 的设置直接失效，这个参数会影响到 -K, --config -A, --user-agent -e, --referer
-K, --config FILE                      指定配置文件
-L, --location                         跟踪重定向 (H)

# CLI显示设置
-s, --silent                           Silent模式。不输出任务内容
-S, --show-error                       显示错误. 在选项 -s 中，当 curl 出现错误时将显示
-f, --fail                             不显示 连接失败时HTTP错误信息
-i, --include                          显示 response的header (H/F)
-I, --head                             仅显示 响应文档头
-l, --list-only                        只列出FTP目录的名称 (F)
-#, --progress-bar                     以进度条 显示传输进度

# 数据传输类
-X, --request [GET|POST|PUT|DELETE|…]  使用指定的 http method 例如 -X POST
-H, --header <header>                  设定 request里的header 例如 -H "Content-Type: application/json"
-e, --referer                          设定 referer (H)
-d, --data <data>                      设定 http body 默认使用 content-type application/x-www-form-urlencoded (H)
    --data-raw <data>                  ASCII 编码 HTTP POST 数据 (H)
    --data-binary <data>               binary 编码 HTTP POST 数据 (H)
    --data-urlencode <data>            url 编码 HTTP POST 数据 (H)
-G, --get                              使用 HTTP GET 方法发送 -d 数据 (H)
-F, --form <name=string>               模拟 HTTP 表单数据提交 multipart POST (H)
    --form-string <name=string>        模拟 HTTP 表单数据提交 (H)
-u, --user <user:password>             使用帐户，密码 例如 admin:password
-b, --cookie <data>                    cookie 文件 (H)
-j, --junk-session-cookies             读取文件中但忽略会话cookie (H)
-A, --user-agent                       user-agent设置 (H)

# 传输设置
-C, --continue-at OFFSET               断点续转
-x, --proxy [PROTOCOL://]HOST[:PORT]   在指定的端口上使用代理
-U, --proxy-user USER[:PASSWORD]       代理用户名及密码

# 文件操作
-T, --upload-file <file>               上传文件
-a, --append                           添加要上传的文件 (F/SFTP)

# 输出设置
-o, --output <file>                    将输出写入文件，而非 stdout
-O, --remote-name                      将输出写入远程文件
-D, --dump-header <file>               将头信息写入指定的文件
-c, --cookie-jar <file>                操作结束后，要写入 Cookies 的文件位置
```

## ping

测试主机间网络连通性

```shell
-d	使用Socket的SO_DEBUG功能
-c	指定发送报文的次数
-i	指定收发信息的间隔时间
-I	使用指定的网络接口送出数据包
-l	设置在送出要求信息之前，先行发出的数据包
-n	只输出数值
-p	设置填满数据包的范本样式
-q	不显示指令执行过程
-R	记录路由过程
-s	设置数据包的大小

ping -c 4 -i 3 www.linuxcool.com
ping6 2408:877e:31::7

ping -6 # windows
```

## dhclient

动态获取或释放IP地址

```shell
-p	指定dhcp客户端监听的端口号（默认端口号86）
-d	总是以前台方式运行程序
-q	安静模式，不打印任何错误的提示信息
-r	释放ip地址
-n	不配置任何接口
-x	停止正在运行的DHCP客户端，而不释放当前租约，杀死现有的dhclient
-s	在获取ip地址之前指定DHCP服务器
-w	即使没有找到广播接口，也继续运行
```

## ifconfig

显示或设置网络设备

```shell
add<地址>	设置ip地址
del<地址>	删除ip地址
down	关闭指定的网络设备
up	启动指定的网络设备
IP地址	指定网络设备的IP地址

# 启用和关闭网卡
ifconfig eth0 down 
ifconfig eth0 up 

ifconfig eth0 192.168.1.56 netmask 255.255.255.0
```

## uname 

显示系统信息

```shell
-a	显示系统所有相关信息
-m	显示计算机硬件架构
-n	显示主机名称
-r	显示内核发行版本号
-s	显示内核名称
-v	显示内核版本
-p	显示主机处理器类型
-o	显示操作系统名称
-i	显示硬件平台

uname -o
GNU/Linux

uname -r
4.15.0-118-generic
```

## lsblk

 查看系统的磁盘

```shell
-a	显示所有设备
-b	以bytes方式显示设备大小
-d	不显示 slaves 或 holders
-D	print discard capabilities
-e	排除设备
-f	显示文件系统信息
-h	显示帮助信息
-i	use ascii characters only
-m	显示权限信息
-l	使用列表格式显示
-n	不显示标题
-o	输出列
-P	使用key=”value”格式显示
-r	使用原始格式显示
-t	显示拓扑结构信息

lsblk -a
```

## sz

从Linux上下载文件到本地

```shell
-a	以文本方式传输（ascii）
-b	以二进制方式传输（binary）
-e	对控制字符转义（escape），这可以保证文件传输正确
-i	后接命令，在接收端执行命令

# mobaxterm
sz  filename
ctrl + 鼠标右键
Receive file using Z-modem

# rz 上传
rz
ctrl + 鼠标右键
Send file using Z-modem
选择上传文件
```

## mkntfs

创建 NTFS 文件系统 即可

## man

查看命令帮助信息 ，manual 手册

## hexdump

显示文件十六进制格式

```shell
-n length	只格式化输入文件的前length个字节
-C	输出规范的十六进制和ASCII码
-b	单字节八进制显示
-c	单字节字符显示
-d	双字节十进制显示
-o	双字节八进制显示
-x	双字节十六进制显示
-s	从偏移量开始输出
-e	指定格式字符串，格式字符串包含在一对单引号中
-v	显示所有输入数据

hexdump testfile
```

## readonly

标记shell变量或函数为只读

## la

显示当前目录下的所有文间，包括隐藏文件, 需要安装

## dirname

去除文件名中的非目录部分

```shell
dirname /a/b/
/a 

dirname a
.
```

## strings

在对象文件或二进制文件中查找可打印的字符串

```
ls |strings
```

## source

在当前Shell环境中从指定文件读取和执行命令

## unlink

删除指定文件,通过link删除，通常用rm替代

## chown

改变文件或目录用户和用户组

```shell
-R	对目前目录下的所有文件与子目录进行相同的拥有者变更
-c	若该文件拥有者确实已经更改，才显示其更改动作
-f	若该文件拥有者无法被更改也不要显示错误讯息
-h	只对于连结(link)进行变更，而非该 link 真正指向的文件
-v	显示拥有者变更的详细资料

chown root:root -R  /home/hjx
```

## stat

显示文件或文件系统的详细信息

```shell
-L	支持符号链接
-f	显示文件系统的信息
-t	以简洁的方式输出

stat hurl.go
  File: hurl.go
  Size: 938             Blocks: 8          IO Block: 4096   regular file
Device: fc01h/64513d    Inode: 929074      Links: 1
Access: (0664/-rw-rw-r--)  Uid: (  500/  ubuntu)   Gid: (  500/  ubuntu)
Access: 2021-12-05 12:05:42.491584506 +0800
Modify: 2021-12-05 10:50:02.472892197 +0800
Change: 2021-12-05 12:05:41.559580684 +0800
 Birth: -
```

## diff3

比较3个文件的不同之处

~~~shell
-A	全部显示，有冲突内容用括号括起来
-a	将所有文件视为文本
-T	使制表符对齐

diff3 file1 file2 file3
~~~

## vimdiff

同时编辑多个文件

```shell
vimdiff file1 file2 
```

## chroot

改变根目录,慎用

```shell
chroot /mnt/ls 
```

## rename

批量改变文件名,慎用

~~~shell
rename main1.c main.c main1.c #main1.c重命名为main.c
rename .jpg .png *.jpg # 将所有以jpg结尾的文件重命名为以png结尾的文件
~~~

## su

切换用户

```shell
-c或--command	执行完指定的指令后，即恢复原来的身份
-f或--fast	适用于csh与tsch，使shell不用去读取启动文件
-l或--login	改变身份时，也同时变更工作目录，以及HOME,SHELL,USER,logname,此外，也会变更PATH变量
-m,-p或--preserve-environment	变更身份时，不要变更环境变量
-s或--shell	指定要执行的shell

su linuxcool #切换到linuxcool用户，但环境变量仍然是root用户
su - linuxcool  #切换到linuxcool用户，并改变为linuxcool用户环境变量
su - root -c "sh test.sh"
```

## nl

添加行号

~~~shell
nl hurl.go
~~~

## ethtool

查询与设置网卡参数

```shell
-i	显示网卡驱动的信息
-E	修改网卡只读存储器字节
-K	修改网卡 Offload 的状态
ethx	查询ethx网口基本设置，其中 x 是对应网卡的编号，如eth0、eth1等
-s	修改网卡的部分配置
-t	让网卡执行自我检测

ethtool eth0
ethtool -s eth0 autoneg off speed 100 duplex full 
```

## dos2unix 

将DOS格式的文本文件转换成UNIX格式

```shell
dos2unix hjx.sh
```

## diff

比较文件的差异

```shell
-a	diff预设只会逐行比较文本文件
-b	不检查空格字符的不同
-W	在使用-y参数时，指定栏宽
-x	不比较选项中所指定的文件或目录
-X	您可以将文件或目录类型存成文本文件，然后在=<文件>中指定此文本文件
-y	以并列的方式显示文件的异同之处

diff log2014.log log2013.log -y

# diffstat命令用于读取diff的输出结果，然后统计各文件的插入，删除，修改等差异计量 
diff test1 test2 | diffstat
```

## od

8进制输出文件内容

~~~shell
od hurl.go
~~~

## zcat 

查看压缩文件的内容

~~~shell
-S	当后缀不是标准压缩包后缀时使用此选项
-c	将文件内容写到标注输出
-d	执行解压缩操作
-l	显示压缩包中文件的列表
-L	显示软件许可信息
-q	禁用警告信息
-r	在目录上执行递归操作

zcat file1.gz 
~~~

## resize

设置终端机视窗的大小 ，了解即可

## sum

计算文件的校验码和显示块数,了解即可

```shell
sum insert.sql 
00827    12 
```

## unset

删除指定的shell变量或函数

```shell
-f	仅删除函数
-v	仅删除变量

unset -v hjx
```

## tree

以树状图列出目录内容

```
-a	显示所有文件和目录
-d	显示目录名称而非内容
-D	列出文件或目录的更改时间
-f	在每个文件或目录之前，显示完整的相对路径名称
-L	层级显示
```

## last

显示用户或终端的登录情况

```
last
ubuntu   pts/0        163.125.203.196  Sun Dec  5 10:25   still logged in
ubuntu   pts/2        tmux(8048).%1    Sun Dec  5 10:25   still logged in
ubuntu   pts/1        tmux(8048).%0    Sun Dec  5 10:24   still logged in
ubuntu   pts/0        163.125.203.196  Sun Dec  5 09:38 - 10:25  (00:47)
ubuntu   pts/0        27.38.44.38      Thu Dec  2 12:04 - 23:31  (11:27)
```

## chmod 

改变文件或目录权限

~~~shell
-c	若该文件权限确实已经更改，才显示其更改动作
-f	若该文件权限无法被更改也不显示错误讯息
-v	显示权限变更的详细资料
-R	对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更)

chmod 777 hurl
chmod +x hurl
~~~

## scp

远程拷贝文件

~~~shell
-1	使用ssh协议版本1-2
-2	使用ssh协议版本2
-4	使用ipv4
-6	使用ipv6
-B	以批处理模式运行
-C	使用压缩
-F	指定ssh配置文件
-l	指定宽带限制
-o	指定使用的ssh选项
-P	指定远程主机的端口号
-p	保留文件的最后修改时间，最后访问时间和权限模式
-q	不显示复制进度
-r	以递归方式复制

scp root@192.168.10.10:/opt/soft/rhel-server-7.3-x86_64.tar.gz /opt/soft/
scp -r root@10.10.10.10:/opt/soft/mysql /opt/soft/

scp /opt/soft/rhel-server-7.3-x86_64.tar.gz root@192.168.10.10:/opt/soft/scptest
scp -r /opt/soft/mysql root@192.168.10.10:/opt/soft/scptest
~~~

## md5sum

校验文件 md5 值

~~~shell
-b	以二进制模式读取文件
-t	以文本模式读入文件内容
-c	根据已生成的md5值，对现存文件进行校验

md5sum file  
~~~

## more

显示文本文件内容

```shell
# 参数说明
-num	指定每屏显示的行数
-l	more在通常情况下把 ^L 当作特殊字符, 遇到这个字符就会暂停,-l选项可以阻止这种特性
-f 	计算实际的行数，而非自动换行的行数
-p	先清除屏幕再显示文本文件的剩余内容
-c	与-p相似，不滚屏，先显示内容再清除旧内容
-s	多个空行压缩成一行显示
-u	禁止下划线
+/pattern	在每个文档显示前搜寻该字(pattern)，然后从该字串之后开始显示
+num 	从第 num 行开始显示

# 操作键说明
Space键：显示文本的下一屏内容
Enter键：向下n行，需要定义，默认为1行
斜线符\：接着输入一个模式，可以在文本中寻找下一个相匹配的模式
H键：显示帮助屏
B键：显示上一屏内容
Q键：退出more命令
Ctrl+F、空格键：向下滚动一屏
Ctrl+B：返回上一屏
=： 输出当前的行号
：f：输出文件名和当前的行号
V：调用vi编辑器
!：调用Shell，并执行命令

more hurl.go
```

## rcp

远程文件复制, 一般用scp代替

```shell
-p	保留源文件或目录的属性，包括拥有者，所属群组，权限与时间
-r	递归处理，将指定目录下的文件与子目录一并处理
-x	加密两台Linux主机间传送的所有信息
-D	指定远程服务器的端口号

rcp test1 192.168.10.10:/home/test1
rcp root@192.168.10.10:./test2 test2 
```

## which

查找文件，一般用于找查命令的执行文件

~~~
which go
/usr/local/go/bin/go
~~~

## less

分页显示工具，less的作用与more十分相似，不同点为less命令允许用户向前或向后浏览文件，而more命令只能向前浏览 。

```
ps -ef |less 
```

## sort 

排序文件并输出

```shell
-b	忽略每行前面开始出的空格字符
-c	检查文件是否已经按照顺序排序
-d	排序时，处理英文字母、数字及空格字符外，忽略其他的字符
-f	排序时，将小写字母视为大写字母
-i	排序时，除了040至176之间的ASCII字符外，忽略其他的字符
-m	将几个排序号的文件进行合并
-M	将前面3个字母依照月份的缩写进行排序
-n	依照数值的大小排序
-o <输出文件>	将排序后的结果存入制定的文件
-r	以相反的顺序来排序
-t <分隔字符>	指定排序时所用的栏位分隔字符
-k	指定需要排序的栏位
```

## whereis 

显示命令及相关文件的路径, 和which命令一致

~~~shell
whereis go
go: /usr/local/go /usr/local/go/bin/go
~~~

## chattr

改变文件隐藏属性

```shell
a：让文件或目录仅供附加用途。
b：不更新文件或目录的最后存取时间。
c：将文件或目录压缩后存放。
d：将文件或目录排除在倾倒操作之外。
i：不得任意更动文件或目录。
s：保密性删除文件或目录。
S：即时更新文件或目录。
u：预防意外删除。

chattr +i hurl.go
```

## lsattr

显示文件隐藏属性

```shell
lsattr hurl.go
--------------e--- hurl.go
```

## locate

快速查找文件或目录，比find 要快

```shell
locate hurl.go
/home/ubuntu/hjx/program/hurl/hurl.go
/home/ubuntu/hjx/sftptest/hurl.go
```

## cksum

校验文件,了解即可

```shell
cksum tempfile
4294967295 0 tempfile #“4294967295”表示校验码，“0”表示字节数。
```

## cd

切换目录

```shell
-P	如果切换的目标目录是一个符号链接，则直接切换到符号链接指向的目标目录
-L	如果切换的目标目录是一个符号链接，则直接切换到符号链接名所在的目录
--	仅使用”-“选项时，当前目录将被切换到环境变量”OLDPWD”对应值的目录
~	切换至当前用户目录
..	切换至当前目录位置的上一级目录
```

## userdel

 删除用户

```shell
-f	强制删除用户账号
-r	删除用户主目录及其中的任何文件

userdel -rf hjx
```

## touch

创建文件,是创建新的空文件，二是改变已有文件的时间戳属性

~~~shell
a	改变档案的读取时间记录
-m	改变档案的修改时间记录
-r	使用参考档的时间记录，与 --file 的效果一样
-c	不创建新文件
-d	设定时间与日期，可以使用各种不同的格式
-t	设定档案的时间记录，格式与 date 命令相同
--no-create	不创建新文件

touch hurl.go
~~~

## ls

显示指定工作目录下的内容及属性信息

```shell
-a	显示所有文件及目录 (包括以“.”开头的隐藏文件)
-l	使用长格式列出文件及目录信息
-r	将文件以相反次序显示(默认依英文字母次序)
-t	根据最后的修改时间排序
-A	同 -a ，但不列出 “.” (当前目录) 及 “..” (父目录)
-S	根据文件大小排序
-R	递归列出所有子目录
```

## make

make命令是GNU的工程化编译工具，用于编译众多相互关联的源代码文件，还可以编辑内核或模块，以实现工程化的管理，提高开发效率。

~~~
make 
~~~

## nano

字符终端文本编辑器, 用vi代替

## head

显示文件开头内容

~~~shell
-n	后面接数字，代表显示几行的意思
-c	指定显示头部内容的字符数
-v	总是显示文件名的头信息
-q	不显示文件名的头信息
~~~

## tac

反向列示文件内容，从下往上打印

## let

执行一个或多个表达式

```shell
a=1
let a++ # a=`expr $a + 1`
```

## fmt

编排文本文件

~~~shell
fmt -w 20 hurl.go #-w 设置行宽
~~~



## comm

比较两个已排过序的文件

~~~shell
-1 	不显示只在第1个文件里出现过的列
-2	不显示只在第2个文件里出现过的列
-3	不显示只在第1和第2个文件里出现过的列
~~~

## tr

字符转换,transform 

```shell
-c	选定字符串1中字符集的补集，即反选字符串1的补集
-d	删除字符串1中出现的所有字符
-s	删除所有重复出现的字符序列，只保留一个

tr "[a-z]" "[A-Z]" <file_1
tr -d "[a-z]" <file_1
```

## uniq

去除文件中的重复行

~~~shell
-c	打印每行在文本中重复出现的次数
-d	只显示有重复的纪录，每个重复纪录只出现一次
-u	只显示没有重复的纪录

cat hurl |uniq -c
~~~

## timeout

 在指定的时间应在运行则杀死该进程

```shell
-s<信号>	指定在超时时发送的信号，信号可以是类似“HUP”的信号名或是信号数
-k<时间>	达到给定的时间限制后会强制结

timeout -k 10s 1m sh linuxcool.sh # 运行命令一分钟，如果命令没有结束，将在10秒后终止命令
imeout -s SIGKILL 5s ping www.linuxprobe.com # 发送SIGKILL信号给ping命令，5秒钟后终止
```

## virsh

KVM虚拟机相关命令， 了解即可

## logger

系统日志记录，了解即可，在/var/log/syslog中查看

## fuser

使用文件或文件结构识别进程，fuser命令作用是报告进程使用的文件和网络套接字

~~~shell
fuser /etc/passwd  #列出使用/etc/passwd文件的本地进程的进程号
~~~

## dstat

全能系统信息统计工具

```shell
-c	显示CPU系统占用，用户占用，空闲，等待，中断，软件中断等信息
-d	显示磁盘读写数据大小
-n	显示网络状态
-l	显示系统负载情况
-m	显示内存使用情况
-g	显示页面使用情况
-p	显示进程状态
-s	显示交换分区使用情况
-r	I/O请求情况
-y	系统状态
--ipc	显示ipc消息队列，信号等信息
--socket	用来显示tcp udp端口状态

dstat
```

## sh

shell命令语言解释器

~~~shell
-c	命令从-c后的字符串读取
-i	实现脚本交互
-n	进行shell脚本的语法检查
-x	实现shell脚本逐条语句的跟踪

sh -x tesh.sh
~~~

## pkill

按照进程名杀死进程

~~~shell
-o	仅向找到的最小（起始）进程号发送信号
-n	仅向找到的最大（结束）进程号发送信号
-P	指定父进程号发送信号
-g	指定进程组
-t	指定开启进程的终端

pkill nginx
~~~

## pgrep

用于检索当前正在运行的进程,打印的pid

```shell
-d	设置一个字符串，用于分隔输出的每个进程ID
-f	模式参数仅用于匹配进程名
-I	列出进程名及进程ID
-P	仅选择匹配指定父进程ID的进程
-u	选择仅匹配指定有效用户ID进程
-U	选择仅匹配指定真实用户ID的进程

pgrep  -u root |head -4
1
2
4
6
```

## iostat

监视系统输入输出设备和CPU的使用情况，用dstat 代替

~~~shell
-c	仅显示CPU使用情况
-d	仅显示设备利用率
-k	显示状态以千字节每秒为单位，而不使用块每秒
-m	显示状态以兆字节每秒为单位
-p	仅显示块设备和所有被使用的其他分区的状态
-t	显示每个报告产生时的时间

iostat -d 2
~~~

## enable

启动或关闭 shell 内建指令，危险，慎用

## syslog

系统默认的日志守护进程，了解即可

## type

 显示指定命令的类型

```shell
type ls
ls is aliased to `ls --color=auto'

type cd
cd is a shell builtin
```

## service

控制系统服务

```
service httpd start
```

## iotop

监视磁盘I/O状态，iotop命令是一个用来监视磁盘I/O使用状况的top类工具。iotop具有与top相似的UI，其中包括PID、用户、I/O、进程等相关信息。

```
iotop

```

## sysctl（配置内核参数）

配置内核参数，sysctl命令被用于在内核运行时动态地修改内核的运行参数，可用的内核参数在目录“/proc/sys”中。

sysctl命令对内核参数的修改仅在当前生效，重启系统后参数丢失。如果希望参数永久生效可以修改配置文件“/etc/sysctl.conf”。

~~~shell
-n	打印值时不打印关键字
-e	忽略未知关键字错误
-N	仅打印名称
-w	当改变sysctl设置时使用此项
-p	从配置文件“/etc/sysctl.conf”加载内核参数设置
-a	打印当前所有可用的内核参数变量和值
-A	以表格方式打印当前所有可用的内核参数变量和值

## 读取
sysctl net.ipv6.neigh.lo.locktime
net.ipv6.neigh.lo.locktime = 0

## 修改
sysctl net.ipv6.neigh.lo.locktime=1
net.ipv6.neigh.lo.locktime = 1 
~~~

## lsmod（加载模块管理）

显示已加载模块状态，用于显示已经加载到内核中的模块的状态信息。

lsmod命令的指定结果共有三列。
Module：模块名。
Size：模块大小。
Used by：模块是否被其他模块调用。

## insmod （用于载入模块）

Linux insmod（英文全拼：install module）命令用于载入模块。

Linux有许多功能是通过模块的方式，在需要时才载入kernel。如此可使kernel较为精简，进而提高效率，以及保有较大的弹性。这类可载入的模块，通常是设备驱动程序。

```shell
-f 　不检查目前kernel版本与模块编译时的kernel版本是否一致，强制将模块载入。
-k 　将模块设置为自动卸除。
-m 　输出模块的载入信息。
-o<模块名称> 　指定模块的名称，可使用模块文件的文件名。
-p 　测试模块是否能正确地载入kernel。
-s 　将所有信息记录在系统记录文件中。
-v 　执行时显示详细的信息。
-x 　不要汇出模块的外部符号。
-X 　汇出模块所有的外部符号，此为预设置
```

## rmmod (用于删除模块)

Linux rmmod（英文全拼：remove module）命令用于删除模块。

```shell
rmmod [-as][模块名称...]
```

## modprobe（内核模块智能加载工具）

modprobe命令用于智能地向内核中加载模块或者从内核中移除模块。

```shell
-a	加载命令行给出的全部的模块
-c	显示所有模块的设置信息
-d	使用排错模式
-l	显示可用的模块
-r	从内核中移除模块
-t	指定模块类型
-s	记录错误信息到系统日志中
-- -show-depends	显示模块依赖关系
-v	执行时显示详细的信息
-V	显示版本信息
-help	显示帮助
```





## systemctl（系统管理服务）

管理系统服务，和service 类似，推荐使用systemctl

```shell
-start	启动服务
-stop	停止服务
-restart	重启服务
-enable	使某服务开机自启
-disable	关闭某服务开机自启
-status	查看服务状态
-list -units --type=service	列举所有已启动服务

systemctl start httpd.service 
systemctl stop httpd.service 
```

## passwd

修改用户账户密码

```shell
-d	删除密码
-l	锁定用户密码，无法被用户自行修改
-u	解开已锁定用户密码，允许用户自行修改
-e	密码立即过期，下次登陆强制修改密码
-k	保留即将过期的用户在期满后能仍能使用
-S	查询密码状态

passwd # 修改当前用户
passwd hjx # 修改指定用户
```

## vmstat

显示虚拟内存状态，了解即可

## lsof

查看文件的进程信息

```shell
-a	列出打开文件存在的进程
-c <进程名>	列出指定进程所打开的文件
-g	列出GID号进程详情
-d <文件号>	列出占用该文件号的进程
+d <目录>	列出目录下被打开的文件
+D <目录>	递归列出目录下被打开的文件
-n <目录>	列出使用NFS的文件
-i <条件>	列出符合条件的进程
-p <进程号>	列出指定进程号所打开的文件
-u	列出UID号进程详情

lsof |grep filename
```

## exec

调用并执行指定的命令,exec命令通常用在shell脚本程序中，可以调用其他的命令。如果在当前终端中使用命令，则当指定的命令执行完毕后会立即退出终端。

```shell
exec -c echo Welcome to use Linux!
```

## crontab

定时执行任务

```shell
-e	编辑该用户的计时器设置
-l	列出该用户的计时器设置
-r	删除该用户的计时器设置
-u	指定要设定计时器的用户名称

minute   hour   day   month   week   command     顺序：分 时 日 月 周 命令
* * * * * /bin/bash /home/ubuntu/hjx/program/mycron/crotab.sh >/dev/null 2>&1
```

## reboot

重新启动计算机

```shell
-n	在重开机前不做将记忆体资料写回硬盘的动作 帮助
-w 	并不会真的重开机，只是把记录写到 /var/log/wtmp 档案里
-d	不把记录写到 /var/log/wtmp 档案里（-n 这个参数包含了 -d）
-f	强迫重开机，不呼叫 shutdown 这个指令
-i	在重开机之前先把所有网络相关的装置先停止
```

## adduser

创建用户

```shell
-c	加上备注文件，备注文字会存储在 passwd 的备注参数中
-d	指定用户登录时的起始目录
-D	变更默认值
-e	设定此帐号的使用期限（格式为 YYYY-MM-DD），预设值为永久有效
-f <缓冲天数>	指定在密码过期后多少天即关闭该帐号
-g <群组>	指定用户所属的群组
-G <群组>	指定用户所属的附加群组
-m	自动建立用户的登入目录
-M	不要自动建立用户的登入目录
-n	取消建立以用户名称为名的群组
-r	建立系统帐号
-s <shell>	指定用户登入后所使用的shell
-u <uid>	指定用户ID

```

## groupadd

 新建工作组

```shell
-g	指定新建工作组的id
-r	创建系统工作组，系统工作组的组ID小于500
-K	覆盖配置文件“/ect/login.defs”
-o	允许添加组ID号不唯一的工作组

```

## hostnamectl

修改主机名称

~~~shell
hostnamectl set-hostname linuxprobe
~~~

## finger

查询其他使用者,finger命令会去寻找并显示指定账号的用户相关信息，包括本地与远程主机的用户皆可，账号名称名优大小写的差别。

```
finger -l
finger root
```

## logname

显示用户名称

## logout

用户退出系统

## clear

清除屏幕

## whoami 

打印当前登录用户

## killall

使用进程名称来杀死一组进程，

~~~shell
-e	对长名称进行精确匹配
-l	打印所有已知信号列表
-p	杀死进程所属的进程组
-i	交互式杀死进程，杀死进程前需要进行确认
-r	使用正规表达式匹配要杀死的进程名称
-s	用指定的进程号代替默认信号“SIGTERM”
-u	杀死指定用户的进程

killall nginx
~~~

## bash

bash 是一个为GNU计划编写的Unix shell。

## nohup

后端运行程序

~~~
nohup command > linuxcool.com 2>&1 
~~~

## halt 

关机

```
-w	并不是真正的重启或关机，只是写wtmp(/var/log/wtmp)纪录
-d	不写wtmp纪录(已包含在选项[-n]中)
-f	没有调用shutdown而强制关机或重启
-i	关机(或重启)前关掉所有的网络接口
-p	该选项为缺省选项，就是关机时调用poweroff
```

## chgrp

更改文件用户组, 一般用chown替代

```shell
-c	效果类似”-v”参数，但仅回报更改的部分
-f	不显示错误信息
-h	对符号连接的文件作修改，而不更动其他任何相关文件
-R	递归处理，将指定目录下的所有文件及子目录一并处理
```

## top

实时显示进程动态

```shell
d : 改变显示的更新速度，或是在交谈式指令列( interactive command)按 s
q : 没有任何延迟的显示速度，如果使用者是有 superuser 的权限，则 top 将会以最高的优先序执行
c : 切换显示模式，共有两种模式，一是只显示执行档的名称，另一种是显示完整的路径与名称
S : 累积模式，会将己完成或消失的子进程 ( dead child process ) 的 CPU time 累积起来
s : 安全模式，将交谈式指令取消, 避免潜在的危机
i : 不显示任何闲置 (idle) 或无用 (zombie) 的进程
n : 更新的次数，完成后将会退出 top


top  -H -p 28189
```

## init

切换系统运行级别

```shell
0	关机
1	单用户
2	多用户
3	完全多用户模式
5	图形界面
6	重启
```

## shutdown

 关闭服务器

```shell
-c	当执行“shutdown -h 11:50”指令时，只要按+键就可以中断关机的指令
-f	重新启动时不执行fsck
-F	重新启动时执行fsck
-h	将系统关机
-k	只是送出信息给所有用户，但不会实际关机
-n	不调用init程序进行关机，而由shutdown自己进行
-r	shutdown之后重新启动
-t	送出警告信息和删除信息之间要延迟多少秒
```

## sudo

以系统管理者的身份执行指令

## date

显示日期与时间

~~~shell
-d datestr	显示 datestr 中所设定的时间 (非系统时间)
-s datestr	将系统时间设为 datestr 中所设定的时间
-u	显示目前的格林威治时间
~~~

## kill

杀死进程

```shell
-l	列出系统支持的信号
-s	指定向进程发送的信号
-a	处理当前进程时不限制命令名和进程号的对应关系
-p	指定kill命令只打印相关进程的进程号，而不发送任何信号

kill -9 pid
```

##  watch

周期性执行命令

~~~shell
-n/--interval	watch默认每2秒运行一下程序，可以用-n或-interval来指定间隔的时间
-d/--differences	用-d或--differences 选项watch 会高亮显示变化的区域。 而-d=cumulative选项会把变动过的地方(不管最近的那次有没有变动)都高亮显示出来
-t/--no-title	关闭watch命令在顶部的时间间隔、命令、当前时间的输出
-h/--help	查看帮助文档

watch -n 10 'cat /proc/loadavg'
~~~

## w 

显示已登录用户 whoami  代替

## ps

显示进程状态

```
-ef 
-aux 
-w
```

## cal

显示日历

```shell
-l	单月分输出日历
-3	显示最近三个月的日历
-s	将星期天作为月的第一天
-m	将星期一作为月的第一天
-j	显示在当年中的第几天（儒略日）
-y	显示当年的日历
```

## exit

退出shell

```shell
0	执行成功
1	执行失败
$?	参照上一个状态值
```

## who

 打印当前登录用户  与 w ，whoami 功能类似

## mkfs.ext4  

创建ext4文件系统，了解即可

~~~shell
-c	格式化前检查分区是否有坏块
-q	执行时不显示任何信息
-b block-size	指定block size大小，默认配置文件在/etc/mke2fs.conf，blocksize = 4096
-F	强制格式化

mkfs.ext4 -c /dev/sdb
~~~

## free

显示系统内存情况

```shell 
-b	以Byte显示内存使用情况
-k	以kb为单位显示内存使用情况
-m	以mb为单位显示内存使用情况
-g	以gb为单位显示内存使用情况
-s	持续显示内存
-t	显示内存使用总合
```

## ftp

文件传输协议客户端

```shell
help或?- 列出所有可用的FTP命令。
cd - 切除远程计算机上的目录。
lcd - 切换本地计算机上的目录。
ls - 列出当前远程目录中的文件和目录的名称。
mkdir - 在当前远程目录中创建一个新目录。
pwd - 打印远程计算机上的当前工作目录。
delete - 删除当前远程目录中的文件。
rmdir- 删除当前远程目录中的目录。
get - 将一个文件从远程复制到本地计算机。
mget - 将多个文件从远程复制到本地计算机。
put - 将一个文件从本地复制到远程计算机。
mput - 将一个文件从本地复制到远程计算机
```

## tcpdump

 监听网络流量

```shell
-a	尝试将网络和广播地址转换成名称
-c<数据包数目>	收到指定的数据包数目后，就停止进行倾倒操作
-d	把编译过的数据包编码转换成可阅读的格式，并倾倒到标准输出
-dd	把编译过的数据包编码转换成C语言的格式，并倾倒到标准输出
-ddd	把编译过的数据包编码转换成十进制数字的格式，并倾倒到标准输出
-e	在每列倾倒资料上显示连接层级的文件头
-f	用数字显示网际网络地址
-F<表达文件>	指定内含表达方式的文件
-i<网络界面>	使用指定的网络截面送出数据包
-l	使用标准输出列的缓冲区
-n	不把主机的网络地址转换成名字
-N	不列出域名
-O	不将数据包编码最佳化
-p	不让网络界面进入混杂模式
-q	快速输出，仅列出少数的传输协议信息
-r<数据包文件>	从指定的文件读取数据包数据
-s<数据包大小>	设置每个数据包的大小
-S	用绝对而非相对数值列出TCP关联数
-t	在每列倾倒资料上不显示时间戳记
-tt	在每列倾倒资料上显示未经格式化的时间戳记
-T<数据包类型>	强制将表达方式所指定的数据包转译成设置的数据包类型
-v	详细显示指令执行过程
-vv	更详细显示指令执行过程
-x	用十六进制字码列出数据包资料
-w<数据包文件>	把数据包数据写入指定的文件

```

## ip 

显示与操作路由

```
ip addr
```

## host

域名查询

```
host linuxcool.com #查询域名对应的ip
```

## dig

查询域名DNS信息

~~~shell
@	指定进行域名解析的域名服务器
-b	使用指定的本机ip地址向域名服务器发送域名查询请求
-f	指定dig以批处理的方式运行，指定的文件中保存着需要批处理查询的DNS任务信息
-p	指定域名服务器所使用端口号
-t	指定要查询的DNS数据类型(默认为A)
-x	执行逆向域名查询
-4	使用ipv4（默认）
-6	使用ipv6

dig www.linuxcool.com
~~~

## nslookup

域名查询

~~~shell
-sil	不显示任何警告信息
exit	退出命令
server	指定解析域名的服务器地址
set type=soa	设置查询域名授权起始信息
set type=a	设置查询域名A记录
set type=mx	设置查询域名邮件交换记录

nslookup linuxcool.com
~~~

## ss 

显示活动套接字信息,ss命令用来显示处于活动状态的套接字信息。它可以显示和netstat类似的内容。但ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。

```shell
-n	不解析服务名称，已数字方式显示
-a	显示所有套接字
-l	显示处于监听状态的套接字
-o	显示计时器信息
-e	显示详细的套接字信息
-m	显示套接字的内存使用情况
-p	显示使用套接字的进程
-i	显示内部的TCP信息
-s	显示套接字使用概况
-4	仅显示ipv4的套接字
-6	仅显示ipv6的套接字
-0	显示PACKET套接字
-t	只显示TCP套接字
-u	只显示UDP套接字
-d	只显示DCCP套接字
-w	只显示RAW套接字
-x	只显示 Unix套接字
-D
将原始TCP套接字信息转储到文件

ss -t -a 
```

## lsscsi

列出SCSI设备及属性 ，了解即可

## lscpu

显示CPU架构的有关信息，了解即可

## shuf命令

产生随机的排列

```
cat hjx.txt |shuf
```

## seq

打印数字序列

```
-f	格式
-s	字符串
-w	在列前添加0 使得宽度相同
```

## wget

文件下载，建议用curl hurl 代替

## wall

输出信息 wall命令用于向系统当前所有打开的终端上输出信息

```shell
wall this is a test line
```

## test

检查条件是否成立

```shell
-eq （=）	等于则为真
-ne （！=）	不等于则为真
-gt （>）	大于则为真
-ge （>=）	大于等于则为真
-lt （<）	小于则为真
-le （<=）	小于等于则为真


num1=100 
num2=100 
if test $[num1] -eq $[num2] then     
echo '两个数相等！' 
else     
echo '两个数不相等！' 
fi 
```

## printf 

shell 格式化打印输出

```shell
printf '%d %s\n' 1 "abc" 
```

## declare

声明shell变量, 建议使用export 代替

~~~
-a	声明数组变量
-f	仅显示函数
-F	不显示函数定义
-i	先计算表达式，把结果赋给所声明变量
-p	显示给定变量的定义的方法和值，当使用此选项时，其他的选项将被忽略
-r	定义只读变量
-x	将指定的Shell变量转换成环境变量
~~~

## jobs

显示shell的作业信息

~~~shell
-l	显示作业列表时包括进程号
-n	显示上次使用jobs后状态发生变化的作业
-p	显示作业列表时仅显示其对应的进程号
-r	仅显示运行的（running）作业
-s	仅显示暂停的（stopped）作业
~~~

## bc

 浮点运算

```shell
echo "1.212*3" | bc 
```

