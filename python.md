[toc]

# 1、python基础

## 1.1、关于GIL

GIL 是python的全局解释器锁，同一进程中假如有多个线程运行，一个线程在运行python程序的时候会霸占python解释器（加了一把锁即GIL），使该进程内的其他线程无法运行，等该线程运行完后其他线程才能运行。如果线程运行过程中遇到耗时操作，则解释器锁解开，使其他线程运行。<font color="red">所以在多线程中，线程的运行仍是有先后顺序的，并不是同时进行。</font>

## 1.2、一些小记

```python
#pip 换源安装
pip install scrapy -i http://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com   #阿里云 
https://pypi.mirrors.ustc.edu.cn/simple/ #中国科技大学
http://pypi.douban.com/simple/    #豆瓣(douban) 
https://pypi.tuna.tsinghua.edu.cn/simple/ #清华大学 
  
#!/usr/bin/python3  #解释器路径
# -*- coding: utf-8 -*-   #python 默认编码是utf-8
#coding=utf-8

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
import os
import sys
#获得.py文件所在的绝对路径，包括文件名
py_path = os.path.abspath(__file__)  
#获得.py所在的文件夹的绝对路径
py_file_path = os.path.dirname(os.path.abs(__file__)) 
#获得py_file_path的上一级文件夹的绝对路径
py_pre_file_path = os.path.dirname(os.path.dirname(os.path.abs(__file__))) 
#将路径动态的添加到环境变量中
sys.path.append(py_pre_file_path)

%c	 格式化字符及其ASCII码
%s	 格式化字符串
%d	 格式化整数
%u	 格式化无符号整型
%o	 格式化无符号八进制数
%x	 格式化无符号十六进制数
%X	 格式化无符号十六进制数（大写）
%f	 格式化浮点数字，可指定小数点后的精度
%e	 用科学计数法格式化浮点数
%E	 作用同%e，用科学计数法格式化浮点数
%g	 %f和%e的简写
%G	 %f 和 %E 的简写
%p	 用十六进制数格式化变量的地址

#模块导入
import 
from module import name
import lib as alias  #as 使用别名

#包结构
sound/                          顶层包
      __init__.py               初始化 sound 包
      formats/                  文件格式转换子包
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  声音效果子包
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  filters 子包
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...

# python 输入
string = input("请输入:")

#python单下划线、双下划线、头尾双下划线说明
__foo__: 定义的是特殊方法，一般是系统定义名字 ，类似 __init__() 之类的。
_foo: 以单下划线开头的表示的是 protected 类型的变量，即保护类型只能允许其本身与子类进行访问，不能用于 from module import *
__foo: 双下划线的表示的是私有类型(private)的变量, 只能是允许这个类本身进行访问了。#私有名称 __private 和 __private_method 被重命名为 _C__private 和_C__private_method
p1 = People('苏苏',18)
print(p1._People__tall) #我们应当遵循python规则不应该用c._C__private去访问私有属性
  
#限制class属性，创建大量对象时节省内存方法
__slots__ = ['year', 'month', 'day']  #元组也行 , 超过这些属性定义会报错。

#解压赋值
record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
name, email, *phone_numbers = record


```

## 1.3、python运算符

```python
#算术运算符
+、-、*、/（/返回商的浮点数） 
%	取模 - 返回除法的余数	
**	幂 - 返回x的y次幂	a**b 为10的21次方
//	取整除 - 向下取接近商的整数	 -9//2 = -5

#比较运算符
==、!=、>	、<	、>=	、<=

#复制运算符
=、+=、-=、*=、/=、%=、**=、//=	
:=	#海象运算符，可在表达式内部为变量赋值。Python3.8 版本新增运算符。在这个示例中，赋值表达式可以避免调用 len() 两次:
if (n := len(a)) > 10:
    print(f"List is too long ({n} elements, expected <= 10)")

#位运算符 ，按位运算符是把数字看作二进制来进行计算的。
a = 60 
b = 13
&	按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0	#(a & b) 输出结果 12 ，二进制解释： 0000 1100
|	按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1。	#(a | b) 输出结果 61 ，二进制解释： 0011 1101
^	按位异或运算符：当两对应的二进位相异时，结果为1	#(a ^ b) 输出结果 49 ，二进制解释： 0011 0001
~	按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1。#~x 类似于 -x-1	(~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式。
<<	左移动运算符：运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。	#a << 2 输出结果 240 ，二进制解释： 1111 0000
>>	右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数	# a >> 2 输出结果 15 ，二进制解释： 0000 1111

#逻辑运算符
and or not

#成员运算符
in  、 not in

#身份运算符
is 、 is not
 
```

## 1.4、python数字

```python
var1 = 1
var2 = 10
del var1[,var2[,var3[....,varN]]] #del 删除引用对象
int #整型
float #浮点型
complex #复数 可以用a + bj,或者complex(a,b)表示
number = 0xA0F # 十六进制
number = 0o37  # 八进制
```

## 1.5、python字符串

```python
var1 = 'Hello World!'
var2 = "hjx"
len(var1)
#字符串运算符
+、*、[]、[ : ]、 in、not in	、r/R（r'a'）
%	格式字符串
```


## 1.6、python列表

```python
l = ['red', 'green', 'blue', 'yellow', 'white', 'black']
l[2] = "pink"
del list[2]
len(l)
l + l
l * 4
len(list)、 max(list)、 min(list)
list(iter) #将迭代对象转换为列表

```

## 1.7、python元组

```python
#Python 的元组与列表类似，不同之处在于元组的元素不能修改。
#元组使用小括号 ( )，列表使用方括号 [ ]。
tup1 = ('Google', 'Runoob', 1997, 2000)
tup2 = (1, 2, 3, 4, 5 )
tup3 = "a", "b", "c", "d"   #  不需要括号也可以
type(tup3)
len(tup3)
tp = tup2 + tup3
tp = tup2 * 4 
del tup1
len(list)、 max(list)、 min(list)
tuple(iter) #将迭代对象转换为元组
```

## 1.8、python字典

```python
dict = {} #创建一个空字典
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
dict['Name']
dict['Age'] = 8 #添加或更新
del dict['Name']
del dict
len(dict) 、 str(dict)

a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
a == b == c == d == e #True
```

## 1.9、python集合

```python
#集合（set）是一个无序的不重复元素序列。
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
st = set() //创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。
len(basket)
x in basket
```

## 1.10、python分支与循环

```python
# 分支
if expr-1:
    command
elif expr-2:
    command
else:
    command

#while 循环
while expr：
	command

# while-else ，如果 while 后面的条件语句为 false 时，则执行 else 的语句块。
while <expr>:
    <statement(s)>
else:
    <additional_statement(s)>
    
#for   for完成后执行else
for <variable> in <sequence>:
    <statements>
else:
    <statements>

#range函数 range[start:end:step]
range(10) 0-9

pass #空语句
```

## 1.11、python 迭代器与生成器

```python
#手动创建迭代器
list=[1,2,3,4]
it = iter(list)
print(it.__next__)
print(it.next)

# 以类的方式创建
class People:
    def __init__(self,name,age):
        self.name = name
        self.age =age

    def __iter__(self):
        return self

    def __next__(self):
        self.age += 1
        return self.age
p1 = People('苏苏',18)
print(p1.__next__())
print(p1.__next__())
print(next(p1))
print(next(p1))

#生成器 在 Python 中，使用了 yield 的函数被称为生成器（generator）。
def hjxrange(start, stop, increment):
    x = start
    while x < stop:
    yield x
    x += increment

for i in hjxrange(1,10,2):
    print(i)

# yield - send
def a():
    print('aaa')
    p1 = yield '123'  #p1 用于接收send传的参数
    print('bbb')
    if (p1 == 'hello'):
        print('p1是send传过来的')
    p2= yield '234'
    print(p2)

r = a()
next(r) #第一次执行要么next(r)要么r.send(None)，不能使用r.send('xxxxx')；这会报错的
r.send('hello')
```

## 1.12、python函数

```python
def a(str1,str2="hjx"):    #s1必需参数 , s2 默认参数
def f(a,b,*,c): # 关键字参数，如果单独出现星号 * 后的参数必须用关键字传入。 f(1,2,c=6)
	return a+b+c
def hjx(self,*args,**kwargs): # *args 接受一个元组 ，**kwargs 接受k=v形式参数 

#强制位置参数，Python3.8 新增了一个函数形参语法 / 用来指明函数形参必须使用指定位置参数，不能使用关键字参数的形式。
#在以下的例子中，形参 a 和 b 必须使用指定位置参数，c 或 d 可以是位置形参或关键字形参，而 e 或 f 要求为关键字形参:
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)

f(10, 20, 30, d=40, e=50, f=60)

#global 关键字 
gcount = 0
def global_test():
    global  gcount #global 用于函数修改全局变量
    gcount+=1
    print (gcount)
global_test()

#nonlocal 关键字
def make_counter():
    count = 0
    def counter():
        nonlocal count #nonlocal 用于内容函数修改修改外部函数变量
        count += 1
        return count
return counter

#lambad 匿名函数
fn = lambda a, b: a+b
fn(2,4) #6
def make_incrementor(n):
	return lambda x: x + n
f = make_incrementor(42)
f(0) #42
f(1) #43
还可以把匿名函数用作传递的实参
pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
pairs.sort(key=lambda pair: pair[1])
pairs #[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]

#函数注解
def stra(a:str) -> str:  # stra.__annotations__ 熟悉查看
    pass

#高阶函数 接受函数为参数，或者把函数作为结果返回的函数是高阶函数（higherorder function）。

__annotations__ |dict|参数和返回值的注解
__call__ |methodwrapper|实现 () 运算符；即可调用对象协议
__closure__|tuple|函数闭包，即自由变量的绑定（通常是 None）
__code__|code|编译成字节码的函数元数据和函数定义体
__defaults__|tuple|形式参数的默认值
__get__|methodwrapper|实现只读描述符协议（参见第 20 章）
__globals__|dict|函数所在模块中的全局变量
__kwdefaults__|dict|仅限关键字形式参数的默认值
__name__|str|函数名称
__qualname__|str|函数的限定名称，如 Random.choice（ 参阅PEP3155，https://www.python.org/dev/peps/pep-3155
```

## 1.13、python File

```python
f = open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```

## 1.14、python 错误 与 异常处理

```python
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

#assert 断言
assert 2==1,"2 -ne 0" #assert 表达式 [, 参数] 
#当表达式为真时，程序继续往下执行；
#当表达式为假时，抛出AssertionError错误，并将  参数  输出
   
```

## 1.15、python面向对象

```python
#!/usr/bin/python3
 
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
#另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
 
#多重继承  若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
 
test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
test.__mro__ #类都有一个名为 __mro__ 的属性，它的值是一个元组，按照方法解析顺序列出各个超类，从当前类一直向上，直到object 类。
```

## 1.16、python常用的魔法函数

```python
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
	二元操作符：
		+			object.__add__(self, other)
		-			object.__sub__(self, other)
		*			object.__mul__(self, other)
		//			object.__floordiv__(self, other)
		/			object.__div__(self, other)
		%			object.__mod__(self, other)
		**			object.__pow__(self, other[, modulo])
		<<		object.__lshift__(self, other)
		>>		object.__rshift__(self, other)
		&			object.__and__(self, other)
		^			object.__xor__(self, other)
		|			object.__or__(self, other)
	扩展二元操作符：
		+=		object.__iadd__(self, other)
		-=			object.__isub__(self, other)
		*=		object.__imul__(self, other)
		/=			object.__idiv__(self, other)
		//=		object.__ifloordiv__(self, other)
		%=		object.__imod__(self, other)
		**=		object.__ipow__(self, other[, modulo])
		<<=		object.__ilshift__(self, other)
		>>=		object.__irshift__(self, other)
		&=		object.__iand__(self, other)
		^=		object.__ixor__(self, other)
		|=			object.__ior__(self, other)
	一元操作符：
		-			object.__neg__(self)
		+			object.__pos__(self)
		abs()		object.__abs__(self)
		~			object.__invert__(self)
		complex()			object.__complex__(self)
		int()			object.__int__(self)
		long()		object.__long__(self)
		float()		object.__float__(self)
		oct()			object.__oct__(self)
		hex()			object.__hex__(self)
		round()	object.__round__(self, n)
		floor()		object__floor__(self)
		ceil()			object.__ceil__(self)
		trunc()		object.__trunc__(self)
	比较函数：
		<			object.__lt__(self, other)
		<=	    object.__le__(self, other)
		==		object.__eq__(self, other)
		!=			object.__ne__(self, other)
		>=		object.__ge__(self, other)
		>			object.__gt__(self, other)
```



# 2、python高级



## 2.1、python创建可管理的属性(@property)

```python
class Person:
    def __init__(self, first_name):
    	self.first_name = first_name
    # Getter function
    @property
    def first_name(self):
    	return self._first_name
    # Setter function
    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        else:
            self._first_name = value
    # Deleter function (optional)
    @first_name.deleter
    def first_name(self):
    	raise AttributeError("Can't delete attribute")
 
p = Person("hjx")
print(p.first_name)  #hjx
p.first_name = "stb"
print(p.first_name)  #stb
#del p.first_name
print(p.first_name)
```

## 2.2、super的两个用法

```python
#1、为了调用父类 (超类) 的一个方法
#2、super() 的另外一个常见用法 是覆盖 Python 特殊方法
def __setattr__(self, name, value):
    if name.startswith('_'):
        super().__setattr__(name, value) # Call original __setattr__
    else:
        setattr(self._obj, name, value)
```

## 2.3、python抽象基类与多态

```python
#Shapes.py
from abc import ABC, abstractmethod
class Shape(ABC):
    @abstractmethod
    def draw(self):
        pass

    @abstractmethod
    def getSize(self):
        pass

  #Shapes.py
...
class Triangle(Shape):
    def __init__(self):
        self.point0 = (0,0)
        self.point1 = (0,0)
        self.point2 = (0,0)

    def draw(self):
        print("Triangle::draw")

    def getSize(self):
        pass       #detail omitted

    def getArea(self):
        return 0   #it should be w * h / 2
#Shapes.py
...
class Circle(Shape):
    def __init__(self):
        self.ptCenter = (0,0)
        self.iRadius = 0

    def draw(self):
        print("Circle::draw")

    def getSize(self):
        pass

    def getArea(self):
        return 0
#Shapes.py
...
class Paragraph(Shape):
    def __init__(self):
        self.sContent = ""

    def draw(self):
        print("Paragraph::draw")

    def getSize(self):
        pass

    def setFont(self,fontName,fontSize):
        pass
```

## 2.4、python上下文管理器

```python
#实现一个新的上下文管理器，以便使用 with 语句，最简单的方法就是使用 contexlib 模块中的@contextmanager 装饰器
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

with open_func('/Users/MING/mytest.txt') as file_in:
    for line in file_in:
        print(line)

通常情况下，如果要写一个上下文管理器，你需要定义一个类，里面包含一个__enter__() 和一个 __exit__() 方法
class Resource():
    def __enter__(self):
        print('===connect to resource===')
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('===close resource connection===')
        return True
    def operate(self):
        1/0
with Resource() as res:
    res.operate()
```

## 2.5、python多线程

```python
# 使用threading
run(): 用以表示线程活动的方法。
start():启动线程活动。
join([time]): 等待至线程中止。这阻塞调用线程直至线程的join() 方法被调用中止-正常退出或者抛出未处理的异常-或者是可选的超时发生。
isAlive(): 返回线程是否活动的。
getName(): 返回线程名。
setName(): 设置线程名。

#直接启动
t = Thread(target=countdown, args=(10,))
t.start()
t.join()

# 通过继承 Thread，重写run()来启动
import threading
import time
exitFlag = 0

class myThread (threading.Thread):
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter
    def run(self):
        print ("开始线程：" + self.name)
        print_time(self.name, self.counter, 5)
        print ("退出线程：" + self.name)

def print_time(threadName, delay, counter):
    while counter:
        if exitFlag:
            threadName.exit()
        time.sleep(delay)
        print ("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)
thread1.start()
thread2.start()
thread1.join()
thread2.join()
print ("退出主线程")

#线程间通信
from queue import Queue  #安全队列

#线程锁 threading.Lock
#Lock 对象和 with 语句块一起使用可以保证互斥执行，就是每次只有一个线程可以执行 with 语句包含的代码块。with 语句会在这个代码块执行前自动获取锁，在执行结束后自动释放锁。
self._value_lock = threading.Lock()
with self._value_lock:
	self._value -= 1

```

## 2.6、python多进程

```python
#在multiprocessing 中，通过创建一个Process 对象然后调用它的 start() 方法来生成进程。
from multiprocessing import Process

def f(name):
	print('hello', name)
if __name__ == '__main__': # 多进程需要在main下调用
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()

 #多进程通信
from multiprocessing import Process, Queue  #Queue 类是一个近似queue.Queue 的克隆。队列是线程和进程安全的

#pipe() 函数返回一个由管道连接的连接对象，默认情况下是双工（双向)
from multiprocessing import Process, Pipe
def f(conn):
    conn.send([42, None, 'hello'])
    conn.close()
if __name__ == '__main__':
    parent_conn, child_conn = Pipe()
    p = Process(target=f, args=(child_conn,))
    p.start()
    print(parent_conn.recv()) # prints "[42, None, 'hello']"
    p.join()	
  
#Pool 类表示一个工作进程池
pool.apply_async(f, (20,)) # runs in *only* one process
pool.map(f, range(10))

```

## 2.7、python异步I/O

```python
import asyncio
import time

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)
async def main():
    print(f"started at {time.strftime('%X')}")
    await say_after(1, 'hello')
    await say_after(2, 'world')
    print(f"finished at {time.strftime('%X')}")
asyncio.run(main())

#并发执行任务
import asyncio
async def factorial(name, number):
    f = 1
    for i in range(2, number + 1):
        print(f"Task {name}: Compute factorial({i})...")
        await asyncio.sleep(1)
        f *= i
    print(f"Task {name}: factorial({number}) = {f}")
async def main():
# Schedule three calls *concurrently*:
    await asyncio.gather(
        factorial("A", 2),
        factorial("B", 3),
        factorial("C", 4),
    )
asyncio.run(main())
```

## 2.8、数据持久化（shelve、pickle）

```python
#shelve 持久化
import shelve
import datetime
  
info = {'name': 'bigberg', 'age': 22}
name = ['Apoll', 'Zous', 'Luna']
t = datetime.datetime.now()
  
with shelve.open('shelve.txt') as f:
  f['name'] = name  # 持久化列表
  f['info'] = info     # 持久化字典
  f['time'] = t      # 持久化时间类型

#shelve 反序列化
import shelve
with shelve.open('shelve.txt') as f:
  n = f.get('name')
  i = f.get('info')
  now = f.get('time') 

print(n)
print(i)
print(now)

```



# 3、python-cookbook

## 3.1、序列类型

````python
#容器序列
list、 tuple、 collections.deque #能存放不同类型的数据
#扁平序列
str、bytes、bytearray、memoryview 和 array.array，#这类序列只能容纳一种类型。

````

## 3.2、列表推导和生成器表达式

```python
symbols = '$￠￡￥€¤'
codes = [ord(symbol) for symbol in symbols]
#生成器表达式
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes): 
	print(tshirt)
```

## 3.3、列表不是首先时

```python
#要存放 1000 万个浮点数的话，数组（array）的效率要高 得多
#元素操作比较多时 set集合更合适

#数组 ，数组还提供从文件读取和存入文件的更快的方法，pickle.dump 处理浮点数组的速度几乎跟 array.tofile 一样快。
from array import array 
from random import random
floats = array('d', (random() for i in range(10**7))) 
floats[-1]   #0.07802343889111107
fp = open('floats.bin', 'wb')
floats.tofile(fp) 
fp.close()
floats2 = array('d') 
fp = open('floats.bin', 'rb')
floats2.fromfile(fp, 10**7) 
fp.close()
floats2[-1] #0.07802343889111107
floats2 == floats #True

#内存视图 memoryview ，它能让用户在 不复制内容 的情况下操作同一个数组的不同切片。
import array
num = array.array("h", [-2, -1, 0, 1, 2])
memv = memoryview(num) #共用一个内存地址
print(memv)
for i in memv:
    print(i)
memv[2] = 5
print(num)

#双向队列 collections.deque 类（双向队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型
dq = deque(range(10), maxlen=10)
```

## 3.4、字典推导和映射方法

```python
DIAL_CODES = [
    (86, 'China'),
    (91, 'India'),
    (1, 'United States'),
    (62, 'Indonesia'),
    (55, 'Brazil'),
    (92, 'Pakistan'),
    (880, 'Bangladesh'),
    (234, 'Nigeria'),
    (7, 'Russia'),
    (81, 'Japan'),
]
country_code = {country: code for code, country in DIAL_CODES}
country_code = {code: country.upper() for country, code in country_code.items() if code < 66 }

# 用setdefaultkey 处理找不到的键
my_dict.setdefault(key, []).append(new_value)
if key not in my_dict:
    my_dict[key] = []
my_dict[key].append(new_value)
```

## 3.5、字典的变种

```python
collections.OrderedDict  #这个类型在添加键的时候会保持顺序，因此键的迭代次序总是一致的
collections.ChainMap #该类型可以容纳数个不同的映射对象，然后在进行键查找操作的时候，这些对象会被当作一个整体被逐个查找，直到键被找到为止。
collections.Counter #这个映射类型会给键准备一个整数计数器。

#子类化 UserDict
import collections

class StrKeyDict(collections.UserDict):
    def __missing__(self, key):
        if isinstance(key, str):
            raise KeyError(key)
        return self[str(key)]

    def __contains__(self, key):
        return str(key) in self.data

    def __setitem__(self, key, item):
        self.data[str(key)] = item
 
# 不可变映射类型 ，标准库里所有的映射类型都是可变的，但有时候你会有这样的需求，比如不能让用户错误地修改某个映射。
from types import MappingProxyType
d = {1:'A'}
d_proxy = MappingProxyType(d) #只读 ，修改报错
d_proxy[1] 
'A'
d_proxy[2] = 'x' 

```

## 3.6、集合推导

```python
# frozenset 不可变集合
frozenset(range(10))
frozenset({0, 1, 2, 3, 4, 5, 6, 7, 8, 9})

#集合推导
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
{x for x in 'abracadabra' if x not in 'abc'} #集合式推导，类似列表
```

## 3.7、参数化装饰器

```python
registry = set() 
def register(active=True): 
    def decorate(func): 
        print('running register(active=%s)->decorate(%s)'% (active, func))
        if active: 
        	registry.add(func)
        else:
        	registry.discard(func) 
        return func 
    return decorate 
@register(active=False) 
def f1():
	print('running f1()')
@register() 
def f2():
	print('running f2()')
def f3():
	print('running f3()')
```

## 3.8、浅拷贝和深拷贝

```python
#copy 模块提供的 deepcopy 和 copy 函数能为任意对象做深复制和浅复制。
import copy
bus1 = Bus(['Alice', 'Bill', 'Claire', 'David'])
bus2 = copy.copy(bus1)
bus3 = copy.deepcopy(bus1)
```

## 3.9、del和垃圾回收

```python
#对象绝不会自行销毁；然而，无法得到对象时，可能会被当作垃圾回收
import weakref
s1 = {1, 2, 3}
s2 = s1 
def bye(): 
	print('Gone with the wind...')
ender = weakref.finalize(s1, bye) 
ender.alive #True
del s1 
ender.alive  #True ,s2 还在使用s1的地址
s2 = 'spam' #Gone with the wind...
ender.alive #False

#弱引用，正是因为有引用，对象才会在内存中存在。当对象的引用数量归零后，垃圾回收程序会把对象销毁。
import weakref
a_set = {0, 1}
wref = weakref.ref(a_set) 
wref #<weakref at 0x100637598; to 'set' at 0x100636748>
wref() #{0, 1}
a_set = {2, 3, 4} 
wref()  #{0, 1}
wref() is None #False
wref() is None #True
```

## 3.10、抽象基类语法详解

```python
#声明抽象基类最简单的方式是继承 abc.ABC
class Tombola(metaclass=abc.ABCMeta): #python 3
	pass
class Tombola(object): # 这是Python 2！！！
	__metaclass__ = abc.ABCMeta
 
#例如，声明抽象类方法的推荐方式是
class MyABC(abc.ABC):
    @classmethod
    @abc.abstractmethod
    def an_abstract_classmethod(cls, ...):
    	pass	
```

## 3.11、使用yield from

```python
def gen():
yield from 'AB'
yield from range(1, 3)
list(gen()) #['A', 'B', 1, 2]
```

## 3.12、使用asyncio处理并发(了解)

## 3.13、元编程

```python
#动态属性和特征  --元编程

#使用 __new__ 方法以灵活的方式创建对象
# 构建对象的伪代码
def object_maker(the_class, some_arg):
    new_object = the_class.__new__(some_arg)
    if isinstance(new_object, the_class):
        the_class.__init__(new_object, some_arg)
    return new_object
# 下述两个语句的作用基本等效
x = Foo('bar')
x = object_maker(Foo, 'bar')
```

## 3.14、创建缓存实例

```python
import logging
a = logging.getLogger('foo')
b = logging.getLogger('bar')
a is b  #False
c = logging.getLogger('foo')
a is c  #True

#可以用weakref 达到这样的效果
# The class in question
class Spam:
def __init__(self, name):
self.name = name
# Caching support
import weakref
_spam_cache = weakref.WeakValueDictionary()
def get_spam(name):
	if name not in _spam_cache:
		s = Spam(name)
		_spam_cache[name] = s
	else:
		s = _spam_cache[name]
	return s	
```

## 3.15、在函数上加包装器

```python
import time
from functools import wraps
def timethis(func):
    '''
    Decorator that reports the execution time.
    '''
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(func.__name__, end-start)
        return result		
    return wrapper

# 删除一个装饰器，调用__wrapped__
@somedecorator
def add(x, y):
	return x + y
orig_add = add.__wrapped__
orig_add(3, 4)
```

## 3.16、在不同python解释器之间交互

```python
#通过使用 multiprocessing.connection 模块可以很容易的实现解释器之间的通信。
from multiprocessing.connection import Listener
import traceback
def echo_client(conn):
    try:
        while True:
            msg = conn.recv()
            conn.send(msg)
    except EOFError:
    	print('Connection closed')
def echo_server(address, authkey):
	serv = Listener(address, authkey=authkey)
	while True:
		try:
			client = serv.accept()
			echo_client(client)
		except Exception:
			traceback.print_exc()
echo_server(('', 25000), authkey=b'peekaboo')	

#客户端交互
from multiprocessing.connection import Client
c = Client(('localhost', 25000), authkey=b'peekaboo')
c.send('hello')
c.recv() #'hello'
```





