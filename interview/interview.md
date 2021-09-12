# 1、网络分层模型

```golang
// OSI七层模型
物理层、数据链路层、网络层、传输层、会话层、表示层、应用层
// TCP/IP四层模型
网络接口层（物理层、数据链路层）、网络层、传输层、应用层（会话层、表示层、应用层）
// 五层模型
物理层、数据链路层、网络层、传输层、应用层

// 对应层作用
物理层：定义一些电器，机械，过程和规范，如集线器 （没有寻址的概念）// 单位bit
数据链路层：定义如何格式化数据，支持错误检测；（//交换机通过MAC地址转发数据，逻辑链路控制）// 单位是帧
网络层：定义一个逻辑的寻址，选择最佳路径传输，路由数据包，（//路由器，ip协议，实现寻址）// 单位是数据报
传输层：提供可靠和尽力而为的传输；（TCP,UDP等协议,负责网络传输和会话建立）// 单位是报文
会话层：控制会话，建立管理终止应用程序会话
表示层：格式化数据；（如加密等）
应用层：控制应用程序（telnet, ssh, http, ftp, smtp等协议，为应用程序提供网络服）
```

# 2、grpc为什么高效

```golang
// 跨语言
// 性能高：比如protobuf性能高过json, 比如http2.0性能高过http1.1
// 强类型：编译器就给你解决了很大一部分问题
// 流式处理（基于http2.0）：支持客户端流式，服务端流式，双向流式
```

# 3、对称加密与非对称加密

```golang
// 对称加密
对称加密是指加解密使用的是同样的密钥
优点：简单快速
缺点：容易被拦截

// 非对称加密
非对称加密使用了一对密钥，公钥和私钥。私钥由解密方安全保管，公钥可以发给任何请求它的人。数据使用公钥加密，私钥解密。因为私钥不通过网络发送出去，所以非对称加密的安全性很高。
优点：安全性高
缺点：不是那么高效快速
```

# 4、MYSQL**事务的四大特性**

```golang
原子性(Atomicity)、一致性(Consistency)、隔离性(Isolation)、持久性(Durability)

// Mysql怎么保证持久性的？
将数据存入磁盘上
// mysql 两种引擎的区别
Innodb引擎 事务安全的   表级别
MyIASM引擎 非事务安全的  行级别  快速

// 索引
hash
btree 高效
b+tree 磁盘读写代价更低

// 什么时候不要用索引
经常增删改的列不要建立索引；
有大量重复的列不建立索引；
数据量不大的情况下

// 索引失效的情况
索引中不能有列的值为NULL
在一个SELECT语句中，索引只能使用一次
不要用 in 要用exist
尽量不要or，用union
```

# 5、进程与线程的区别

```golang
// 进程是资源分配的最小单位，线程是CPU调度的最小单位
// 线程在进程下行进
// 一个进程可以包含多个线程
// 进程数据共享难，线程数据共享容易。
类似于火车与车厢的关系
```

# 6、redis是单线程还是多线程，为什么快

```golang
// 在 Redis4.0 之前，Redis 是单线程运行的，redis 6.0 之后可以多线程启动 
// 为什么快？异步io（多路复用io），数据存放在内存中，内存读取熟读大于磁盘读取速度
```

# 7、redis持久化方式与优缺点

```golang
// 默认的RDB(Redis DataBase)。 按照一定的时间将内存的数据以快照的形式存在磁盘中，产生的对应快照文件dump.rdb。通过配置文件中的save参数来定义快照的周期。

// AOF(Append Only File)。将redis每次执行的命令存到单独的日志文件中，当重启redis时会从持久化的日志中恢复数据。

// 优缺点
RDB 只有一个dump.rdb文件，方便持久化。容灾好，数据恢复快，速度快，周期执行
AOF 数据安全，速度慢，单次执行

```

# 8、微服务

```golang
// 它采用 UNIX 设计的哲学，每种服务只做一件事，是一种松耦合的能够被独立开发和部署的无状态化服务（独立扩展、升级和可替换）。
```

# 9、HTTP状态码

```golang
1**	信息，服务器收到请求，需要请求者继续执行操作
2**	成功，操作被成功接收并处理
3**	重定向，需要进一步的操作以完成请求
4**	客户端错误，请求包含语法错误或无法完成请求 //规则权限
5**	服务器错误，服务器在处理请求的过程中发生了错误 //服务器，网关
```

# 10 、TCP三次握手、四次挥手，timewait，closewait状态

```golang
// TCP提供了一种可靠、面向连接、字节流、传输层的服务，采用三次握手建立一个连接。采用4次挥手来关闭一个连接。

// 三次握手
第一次：客户端发包SYN (SYN=1, seq=x) //这样服务端就能得出结论：客户端的发送能力、服务端的接收能力是正常的。
第二次：服务器发包ACK (SYN=1, ACK=1, seq=y, ACKnum=x+1): //服务端发送和接收能力也是正常的。
第三次：客户端发包ACK (ACK=1，ACKnum=y+1) // 客户端接收能力正常

// 四次挥手 
第一次：客户端发包想要关闭连接FIN (FIN=1，seq=x)
第二次：服务器发送确认包ACK (ACK=1，ACKnum=x+1) //此时进入closewaite 状态
第三次：服务器发送结束连接的请求FIN (FIN=1，seq=y)
第四次：客户发送确认关闭包ACK (ACK=1，ACKnum=y+1) //此时进入timewaite 状态
```

# 11、socket编程

```golang
// socket 是对 TCP/IP 协议族的一种封装,利用三元组（ip地址，协议，端口）
// Socket 起源于 Unix ，Unix/Linux 基本哲学之一就是“一切皆文件”，都可以用“打开(open) –> 读写(write/read) –> 关闭(close)”模式来进行操作。因此 Socket 也被处理为一种特殊的文件。
socket() 创建套接字
bind() 分配套接字地址
listen() 等待连接请求
accept() 允许连接请求
read()/write() 数据交换
close() 关闭连接
```

# 11、HTTPS建立连接详细过程

```golang
// 客户端发起HTTPS连接
// 服务端发送证书
// 客户端验证服务端发来的证书(验证证书、发送随机数、生成握手、服务器检验、客户端确认、完成握手)
```

# 12、常用的设计模式

```golang
// 创建型模式
简单工厂模式（Simple Factory）、工厂方法模式（Factory Method）、抽象工厂模式（Abstract Factory）、创建者模式（Builder）、原型模式（Prototype）、单例模式（Singleton）

//结构型模式
外观模式（Facade）、 适配器模式（Adapter）、 代理模式（Proxy）、组合模式（Composite）、享元模式（Flyweight）、装饰模式（Decorator）、桥模式（Bridge）

// 行为型模式
中介者模式（Mediator）、观察者模式（Observer）、命令模式（Command）、迭代器模式（Iterator）、模板方法模式（Template Method）、策略模式（Strategy）、状态模式（State）、备忘录模式（Memento）、解释器模式（Interpreter）、职责链模式（Chain of Responsibility）、访问者模式（Visitor）
```

# 13、设计原则

```
OCP	开闭原则	对扩展开放，对修改关闭
SRP	单一职责原则	一个类只负责一个功能领域中的相应职责
LSP	里氏代换原则	所有引用基类的地方必须能透明地使用其子类的对象
DIP	依赖倒转原则	依赖于抽象，不能依赖于具体实现
ISP	接口隔离原则	类之间的依赖关系应该建立在最小的接口上
CARP	合成/聚合复用原则	尽量使用合成/聚合，而不是通过继承达到复用的目的
LOD	迪米特法则	一个软件实体应当尽可能少的与其他实体发生相互作用
```





# golang合集

```golang
// 指针传值 和 值传值
在 Golang 中所有函数参数传递都是值拷贝，传指针只是拷贝了一份指针副本，同时指向原对象。
一般情况下，需要改变原始对象值、传递大的结构体，传指针是最合适的，因为传一个内存地址的开销很小。 // 指针必须安全使用
反之，如果变量不可变更、map 或 slice 应该选择传值方式。

defer的执行顺序  //panic不能阻止defer的运行
append可以拼接多个元素 // 拼接数组（切片时）要用... 解包，appned会从新分别内存，append不能对指针进行操作
注意函数返回值  // 要么匿名返回、要么都具名返回。
new返回零值指针 // 适用于结构体，数组
make返回引用类型 ，可申请空间大小，非指针 // 适用于 map 、slice、channel
:= 精简命名只适用于函数内部 // 要注意全局变量是否有精简命名，精简命名会重新定义变量
结构体只能比较相等不能比较大小 // 与属性类型顺序都有关系
	//能比较的有  bool、数值、字符串、数组
	//不能比较的有 map slice func （注：不能相互比较但可以和nil比较）
访问指针变量 p.name (*p).name // &取地址，*解引用取值
iota在第n行默认值就为n-1 //就算上面是_ ,也会递归+1
map未申请空间不能存key  // 取key为零值
a[i:j:k] //长度为j-i，容量为k-i
cap适用于array pointer slice channel // cap 不能适用于 string map
len适用于array pointer slice channel string map
delete删除map当key不存在时不会报错
当且仅当动态值和动态类型都为nil时，接口类型值才为nil
对于x=y，必须知道x的地址才能将赋值给x //golang map 的value不可寻找 a := []map[string]string{...} -> a[0].["x"]="y"是错误的
range遍历切边，会使用切片的副本，改变切边的长度，循环次数不会改变 // 但是map 会
	//range 取数组，改变数组的值，取出的还是原值
	//range 取切片，改变切片的值，取出的是新值 //切片共享底层数据
const 常量无法获取地址
去重使用map
排序使用sort
nil 不是关键字 //关键字有：close、delete、len、cap、new、make、copy、append、pannic、recover、print、println、complex、real、imag、fallthought
uintptr 可以任意类型的指针
byte 对应 unit8 0-255 2^8-1 // 注意int8 -128-127 2^7 ~ 2^7-1
rune 对应 int32 长处理Unicode、utf-8编码
零值：数值是0，string是"",其他是nil
for；；{} for可以省略前置和后置语句，可以全部省略
if _,ok; if _, err 模式 // if 前面也可以加一个前置语句
switch v := i.(type) 固定写法
fmt包常用 %q（字符串带""输出） %s （字符串值） %t （ture|false） %T （type） %+v (结构体字段输出) %#v （go语音方式输出结构体） %d （整数） %f （float） // %3.5f 小数点前面三位，后面5位

函数值传递 a :=func(){}
垃圾回收：三色标记法
带tag 的结构体、json元素字段必须大写
没有~取反，没有--i、++i
golang 没有继承，只有组合
读取用户输入 fmt.Scanln 或者os.stdin
处理err ，先type err interface{} 再用err.(type)处理

//常用包
tar,zip
bufio // reader 、writer、 scanner (spiltFunc)
builtin // 系统内建关键字说明
bytes // []byte 数组相关方法、reader、buffer
container/heap (实现堆排序)、 list（双向链表，可以存放任意类型数据）、ring（环形双向链表）
context //上下文处理 WithCannel、WithDeadline、WithTimeout、WithValue
		//context.Background  context.TODO
errors
expvar //http 性能相关
flag  //命令行参数包
index/suffixarray //后缀树，堆子字符串查找
io // Copy ,reader、writer、 Pipe
io/iountil // readall(reader)  readfile ,writefile, readdir ,tmpfile
math/big //对大数子精度计算
mutilpart // mutilpart/form相关的实现
net //socktet 
net/http  rpc.register(注册函数) rpc.client  给予tcp封装更快
net/url
os // 操作系统相关、文件进程
os/exec 
os/user
os/signal

path //路径相关操作
filepath  //walk
runtime //运行时服务 cpu数量、GC、go程退出，内存监控  /pprof /trace
sort //排序
strconv //数据类型转换
strings //字符串操作
sync //同步基元 mutex、Rwmutex、waitgroup、cond(实现条件变量，提供go程宣布某件事情发生)、pool(创建临时对象,可以减少GC压力)
	// Rwmutex.Lock() 写锁，其他go程不可以读，不可以写
	// Rwmutex.Rlock() 读锁的情况，其他go程可以读，不可以写
sync/atomic 原子操作，并发安全 //old = *addr，*addr = new  return old

syscall //系统底层函数
Testing *T (TestFunc) 、 *B (BenchFunc) (主要用性能测试) 、Example（主要给go doc看）
time // 获取时间、比较时间、操作时间、time.After，time.Ticker，time.Sleep
unsafe //跳过go语言类型安全限制的操作 SizeOf Aligof Offset  uintptr
	 // 任意类型的指针可以转换为一个Pointer类型值 unsafe.Pointer(&a)
	//一个Pointer类型值可以转换为任意类型的指针 (*int)(unsafe.Pointer(&a))
	//一个uintptr类型值可以转换为一个Pointer类型值 
	//一个Pointer类型值可以转换为一个uintptr类型值
	对结构体操作必须使用uintptr
(*int16)(unsafe.Pointer(unitptr(unsafe.Pointer(&x)+unsafe. Offsetof(x.b))))
```

# Python合集

```python
GIL  #python 解释器每次之后运行一个线程
'' #普通字符串，不能转义
"" #可转义的字符串
""" """ #可以多行输入的字符串， 三引号注释内容可以通过 mydef.__doc__ 查看
\  #代码太长可以用 \ 换行 ,Python 会忽略代码里 []、{} 和 () 中的换行
r'str'   -> 代表普通字符
b'str'  -> byte类型
u'str'  -> unicode 编码
f'Results of the {year} {event}' -> 格式化输出
*args,**kwargs  #*代表是元组tuple、**代表字典dict
''' ''' or """""" #多行注释
s[start:end:step]
#下标复数方向取值 str[-1] list[-1]
#在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。
print(sys.path) #查看python路径

#python单下划线、双下划线、头尾双下划线说明
__foo__: 定义的是特殊方法，一般是系统定义名字 ，类似 __init__() 之类的。
_foo: 以单下划线开头的表示的是 protected 类型的变量，即保护类型只能允许其本身与子类进行访问，不能用于 from module import *
__foo: 双下划线的表示的是私有类型(private)的变量, 只能是允许这个类本身进行访问了。#私有名称 __private 和 __private_method 被重命名为 _C__private 和_C__private_method

#限制class属性，创建大量对象时节省内存方法
__slots__ = ['year', 'month', 'day']  #元组也行 , 超过这些属性定义会报错。

#解压赋值
record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
name, email, *phone_numbers = record 

#运算符
and 、or 、in 、not in 、is、 is not

del 删除引用对象

#Python 的元组与列表类似，不同之处在于元组的元素不能修改。
#元组使用小括号 ( )，列表使用方括号 [ ]。

dict = {} #创建一个空字典

#集合（set）是一个无序的不重复元素序列。
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
st = set() //创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。

# while-else ，如果 while 后面的条件语句为 false 时，则执行 else 的语句块。和if 差不多
while <expr>:
    <statement(s)>
else:
    <additional_statement(s)>
#for   for完成后执行else
for <variable> in <sequence>:
    <statements>
else:
    <statements>

#迭代器 与 生成器
it = iter(list)
it.__next__
yield  # p = yield 123 yield可以接收send传值，必须先next()迭代一次 
yield from iter

#global 关键字  修改全局变量
#nonlocal 关键字 闭包函数修改外层函数变量

#lambad 匿名函数
fn = lambda a, b: a+b

# 函数注解
def stra(a:str) -> str:  # stra.__annotations__ 熟悉查看
    pass

# 异常
try:
    执行代码
except：
	异常时执行
else：
	没有异常时执行
finally：
	最后都会执行
    
#使用raise 触发异常
raise Exception('x 不能大于 5。x 的值为: {}'.format(x))

#自定义异常 需要继承Exception 类
>>> class MyError(Exception):
        def __init__(self, value):
            self.value = value
        def __str__(self):
            return repr(self.value)
>>> try:
        raise MyError(2*2)
    except MyError as e:
        print('My exception occurred, value:', e.value)

类的构造、删除：
		object.__new__(self, ...)
		object.__init__(self, ...)
		object.__del__(self)
	类的表示、输出：
		str()								object.__str__(self) 
		repr()						    	object.__repr__(self)
		len()								object.__len__(self)
		hash()							    object.__hash__(self) 
		bool()							    object.__nonzero__(self) 
		dir()								object.__dir__(self)
		sys.getsizeof()				          object.__sizeof__(self)
	类容器：
		len()								object.__len__(self)
        self.key							 object.__getattr__(self, key)
        self.key = value 					  object.__setattr__(self, key, value)
		self[key]						     object.__getitem__(self, key)
		self[key] = value			          object.__setitem__(self, key, value)
		del[key] 						     object.__delitem__(self, key)
		iter()								object.__iter__(self)
		reversed()					         object.__reversed__(self)
		in操作							  object.__contains__(self, item)
		字典key不存在时			             object.__missing__(self, key)

# @property  //setter getter deleter

# 抽象基类
from abc import ABC, abstractmethod
class Shape(ABC):
    @abstractmethod
    def draw(self):
        pass

    @abstractmethod
    def getSize(self):
        pass

# 上下文管理
实现一个新的上下文管理器，以便使用 with 语句，最简单的方法就是使用 contexlib 模块中的@contextmanager 装饰器
import contextlib
@contextlib.contextmanager
def open_func(file_name):
    # __enter__方法
    print('open file:', file_name, 'in __enter__')
    file_handler = open(file_name, 'r')
    # 【重点】：yield
    yield file_handler
    # __exit__方法
    print('close file:', file_name, 'in __exit__')
    file_handler.close()
    return

或者class 实现__enter__ 和 __exit__ 魔法函数

# threading multiprocessing

# asyncio
# 数据持久化 shelve、pickle

#容器系列：能存放不同类型的数据
#扁平序列:这类序列只能容纳一种类型。

#列表推导、字典推导、集合推导
#collections

#装饰器 @func  f = func(f1())
# 删除一个装饰器，调用__wrapped__


# 深拷贝 与 浅拷贝
copy模块提供的 deepcopy 和 copy 函数能为任意对象做深复制和浅复制。#浅拷贝共享底层数据，python 默认都是浅拷贝

#弱引用 weakref

#元编程 动态属性编程就叫元编程

#通过使用 multiprocessing.connection 模块可以很容易的实现解释器之间的通信。

```

