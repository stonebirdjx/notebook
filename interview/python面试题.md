# 1、装饰器

语法糖 @

装饰器的本质是一个以函数作为参数的高阶函数，作用是在不改变函数的情况下，添加额外的功能（如计算执行时间、添加日志等）。

**装饰器顺序**

```python
@a
@b
@c
def f ():
    pass
f = a(b(c(f)))
```

**带参数的装饰器**

```python
def use_logging(level):#@use_logging(level="warn")
    pass 
```

**类装饰器**

必须要有`__call__`方法

`__init__` 里面传入func

```python
class myDecorator(object):
     def __init__(self, f):
         print("inside myDecorator.__init__()")
         f() # Prove that function definition has completed
     def __call__(self):
         print("inside myDecorator.__call__()")
 
 @myDecorator
 def aFunction():
     print("inside aFunction()")
```

**wraps**

```python

# functools.wraps，wraps本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器里面的 func 函数中
from functools import wraps
def logged(func):
    @wraps(func) #可以保留原func的属性
    def with_logging(*args, **kwargs):
        print func.__name__      # 输出 原func
        print func.__doc__       # 输出 原func
        return func(*args, **kwargs)
    return with_logging
```

# 2、迭代器 

迭代器是一个可以记住遍历的位置的对象。
迭代器有两个基本的方法：iter() 和 next() 或者 c.__next__
一个类作为一个迭代器使用需要在类中实现两个方法 __iter__() 与 __next__()

# 3、生成器

使用了 yield 的函数被称为生成器（generator）。
yield本身就是一个迭代器
yield from iter 
yield 可以接收send发送的数据

# 4、lambda 匿名函数及作用
```python
f1 = lambda x,y:x*x+y*y
f1(1,2)
```

语音简洁，提取重复代码块儿，方便使用与维护

# 5、list 和 tuple 底层原理


# 6、set集合 和 dict 底层原理
