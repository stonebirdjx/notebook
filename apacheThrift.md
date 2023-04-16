<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [**结构体类型**](#%E7%BB%93%E6%9E%84%E4%BD%93%E7%B1%BB%E5%9E%8B)
- [枚举类型](#%E6%9E%9A%E4%B8%BE%E7%B1%BB%E5%9E%8B)
- [常量类型](#%E5%B8%B8%E9%87%8F%E7%B1%BB%E5%9E%8B)
- [类型定义](#%E7%B1%BB%E5%9E%8B%E5%AE%9A%E4%B9%89)
- [异常类型](#%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%9E%8B)
- [服务类型](#%E6%9C%8D%E5%8A%A1%E7%B1%BB%E5%9E%8B)
- [namespace 关键字](#namespace-%E5%85%B3%E9%94%AE%E5%AD%97)
- [include 关键字](#include-%E5%85%B3%E9%94%AE%E5%AD%97)
- [注释](#%E6%B3%A8%E9%87%8A)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

Thrift 脚本可定义的数据类型包括以下几种类型：

- ## **基本类型**

- - **bool**: 布尔值
  - **byte**: 8位有符号整数
  - **i16**: 16位有符号整数
  - **i32**: 32位有符号整数
  - **i64**: 64位有符号整数
  - **double**: 64位浮点数
  - **string**: UTF-8编码的字符串
  - **binary**: 二进制串

## **结构体类型**

- **struct**: 定义的结构体对象

首先编写一个简单的`thrift`文件`base.thrift`

```Thrift
include "base.thrift"

namespace go example.hello

struct User{
  1: required string name, //改字段必须填写
  2: optional i32 age = 0; //默认值
  3: bool gender //默认字段类型为optional
}

service ExampleService {
       ExampleResp CheckService(1:ExampleReq req)
}
```

这里标识了`required`的字段，要求在使用时必须正确赋值，否则运行时会抛出`TProtocolException`异常。

缺省和指定为`optional`时，则运行时不做字段非空校验。

- ## **容器类型**

- - **list**: 有序元素列表
  - **set**: 无序无重复元素集合
  - **map**: 有序的key/value集合

```Thrift
struct Test {
  1: map<string, User> usermap,
  2: set<i32> intset,
  3: list<double> doublelist
}
```

## 枚举类型

1. 编译器默认从0开始赋值
2. 可以赋予某个常量某个整数
3. 允许常量是十六进制整数
4. 末尾没有分号
5. 给常量赋缺省值时，使用常量的全称

```Thrift
enum HttpStatus {
  OK = 200,
  NOTFOUND=404
}
```

## 常量类型

只需要在常量前面加上关键字 const 即可

```Thrift
const i32 const_int = 1
```

## 类型定义

thrift 支持自定义类型

```Thrift
typedef i32 myint
typedef i64 usernumber
```

## 异常类型

异常类型在结构上类似于 struct , 差异是需要使用关键字 exception

```Thrift
exception MyException {
    1: i32 errorCode,
    2: string message
}
```

## 服务类型 

服务类型的定义方法在语意上等同于面向对象中的接口

```Thrift
service HelloService {
    i32 sayInt(1:i32 param)
    string sayString(1:string param)
    bool sayBoolean(1:bool param)
    void sayVoid()
}
```

## namespace 关键字

>  Thrift中的命名空间类似于C++中的namespace和java中的package，它们提供了一种组织（隔离）代码的简便方式。名字空间也可以用于解决类型定义中的名字冲突。由于每种语言均有自己的命名空间定义方式（如python中有module）, thrift允许开发者针对特定语言定义namespace

```Thrift
namespace java com.example.test
namespace go api
```

## include 关键字 

thrift 允许 文件包含其他 thrift 文件， 用户可以通过前缀访问被包含的对象。

```Plain
include "test.thrift"   

struct StSearchResult {
    1: in32 uid;
     ...
}
```

## 注释

Thrift 支持单行或者多行风格, 和 Java 风格类似

```Plain
/** 
 * This is a multi-line comment. 
 * Just like in C. 
 */// C++/Java style single-line comments work just as well.
```