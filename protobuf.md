[toc]

# 定义消息类型

```protobuf
//可以采用如下的方式来定义消息类型的.proto文件
syntax = "proto3"; //指定语法，默认是proto2

message SearchRequest {
	// 定义SearchRequests的成员变量，需要指定：变量类型、变量名、变量Tag
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}

//定义多个message
message SearchResponse {
    repeated string result = 1;
}

//message 嵌套
message SearchResponse {
    message Result {
        string url = 1;
        string title = 2;
        repeated string snippets = 3;
    }
    repeated Result results = 1;
}

//定义在 message 内部的 message 
message SomeOtherMessage {
    SearchResponse.Result result = 1;
}
```

# 分配标识号

```protobuf
//每个字段都有唯一的一个数字标识符。这些标识符是用来在消息的二进制格式中识别各个字段的
//一旦开始使用就不能够再改变。
//注：[1,15]之内的标识号在编码的时候会占用一个字节。[16,2047]之内的标识号则占用2个字节。
//所以应该为那些频繁出现的消息元素保留 [1,15]之内的标识号。切记：要为将来有可能添加的、频繁出现的标识号预留一些标识号。

//最小的标识号可以从1开始，最大到2^29 - 1, or 536,870,911。
//不可以使用其中的[19000－19999]（ (从FieldDescriptor::kFirstReservedNumber 到 FieldDescriptor::kLastReservedNumber)）的标识号，
```

# 指定字段规则

```protobuf
所指定的消息字段修饰符必须是如下之一：
singular：一个格式良好的消息应该有0个或者1个这种字段（但是不能超过1个）。
repeated：在一个格式良好的消息中，这种字段可以重复任意多次（包括0次）。重复的值的顺序会被保留。
在proto3中，repeated的标量域默认情况虾使用packed
```



# 标量数值类型

```protobuf
.proto	说明	C++	Java	Python	Go	Ruby	C#	PHP
double		double	double	float	float64	Float	double	float
float		float	float	float	float32	Float	float	float
int32	使用变长编码，对负数编码效率低，如果你的变量可能是负数，可以使用sint32	int32	int	int	int32	Fixnum or Bignum (as required)	int	integer
int64	使用变长编码，对负数编码效率低，如果你的变量可能是负数，可以使用sint64	int64	long	int/long	int64	Bignum	long	integer/string
uint32	使用变长编码	uint32	int	int/long	uint32	Fixnum or Bignum (as required)	uint	integer
uint64	使用变长编码	uint64	long	int/long	uint64	Bignum	ulong	integer/string
sint32	使用变长编码，带符号的int类型，对负数编码比int32高效	int32	int	int	int32	Fixnum or Bignum (as required)	int	integer
sint64	使用变长编码，带符号的int类型，对负数编码比int64高效	int64	long	int/long	int64	Bignum	long	integer/string
fixed32	4字节编码， 如果变量经常大于228228 的话，会比uint32高效	uint32	int	int	int32	Fixnum or Bignum (as required)	uint	integer
fixed64	8字节编码， 如果变量经常大于256256 的话，会比uint64高效	uint64	long	int/long	uint64	Bignum	ulong	integer/string
sfixed32	4字节编码	int32	int	int	int32	Fixnum or Bignum (as required)	int	integer
sfixed64	8字节编码	int64	long	int/long	int64	Bignum	long	integer/string
bool		bool	boolean	bool	bool	TrueClass/FalseClass	bool	boolean
string	必须包含utf-8编码或者7-bit ASCII text	string	String	str/unicode	string	String (UTF-8)	string	string
bytes	任意的字节序列	string	ByteString	str	[]byte	String (ASCII-8BIT)	ByteString	string
```

# 默认值

```protobuf
string：默认是空的字符串
byte：默认是空的bytes
bool：默认为false
numeric：默认为0
enums：定义在第一位的枚举值，也就是0
messages：根据生成的不同语言有不同的表现。
```

# 定义枚举Enumerations

```protobuf
message SearchRequest {
    string query = 1;
    int32 page_number = 2; // Which page number do we want
    int32 result_per_page = 3; // Number of results to return per page
    enum Corpus {
        UNIVERSAL = 0;
        WEB = 1;
        IMAGES = 2;
        LOCAL = 3;
        NEWS = 4;
        PRODUCTS = 5;
        VIDEO = 6;
    }
    Corpus corpus = 4;
}

//如果枚举是 定义在 message 内部，而其他 message 又想使用，那么可以通过 MessageType.EnumType 的方式引用。
//定义枚举的时候，我们要保证第一个枚举值必须是0，枚举值不能重复，除非使用 option allow_alias = true 选项来开启别名。
enum EnumAllowingAlias {
    option allow_alias = true; // STARTED 和 RUNNING的tag 一样 allow_alias 必须位true
    UNKNOWN = 0;  // 必须是0
    STARTED = 1; 
    RUNNING = 1;
}
```

# 引用其他 proto 文件

```protobuf
my.proto ->import-> first.proto ->import-> second.proto   //my.proto 不能使用 second.proto 中定义的内容
my.proto ->import-> first.proto ->import public-> second.proto //my.proto 可以使用 second.proto 中定义的内容

// my.proto
import "first.proto";

// first.proto
//import "second.proto";
import public "second.proto";

```

# Any 的使用

```protobuf
//Any可以让你在 proto 文件中使用未定义的类型，具体里面保存什么数据，是在上层业务代码使用的时候决定的，使用 Any 必须导入 import google/protobuf/any.proto

import "google/protobuf/any.proto";

message ErrorStatus {
    string message = 1;
    repeated google.protobuf.Any details = 2;
}
```

# Oneof 的使用

```protobuf
// 使用oneof特性节省内存
//  至多一个字段会被设置。 设置其中一个字段会清除其它字段。
message SampleMessage {
  oneof test_oneof {
    string name = 4;
    SubMessage sub_message = 9;
  }
  required string status = 1;
  required string token = 2;
}
```

# Map的使用

```protobuf
//其中key_type可以是任意Integer或者string类型（所以，除了floating和bytes的任意标量类型都是可以的）value_type可以是任意类型。
map<key_type, value_type> map_field = N;
map<string, Project> projects = 3;

message MapFieldEntry {
  key_type key = 1;
  value_type value = 2;
}
repeated MapFieldEntry map_field = N;
// map 是无序的
```

# Packages 的使用

````protobuf
//当然可以为.proto文件新增一个可选的package声明符，用来防止不同的消息类型有命名冲突。
package foo.bar;
message Open { ... }

//在其他的消息格式定义中可以使用包名+消息名的方式来定义域的类型
message Foo {
  ...
  required foo.bar.Open open = 1;
  ...
}
//对于Go，包可以被用做Go包名称，除非你显式的提供一个option go_package在你的.proto文件中
````

# Options

```protobuf
//Options 分为 file-level options（只能出现在最顶层，不能在消息、枚举、服务内部使用）、 message-level options（只能在消息内部使用）、field-level options（只能在变量定义时使用）



```



# 定义服务

```protobuf
//如果想要将消息类型用在RPC(远程方法调用)系统中，可以在.proto文件中定义一个RPC服务接口，protocol buffer编译器将会根据所选择的不同语言生成服务接口代码及存根。

service SearchService {
  //简单RPC
  rpc Search (SearchRequest) returns (SearchResponse);
  
  //服务器端流式 RPC,客户端发送请求到服务器，拿到一个流去读取返回的消息序列。客户端读取返回的流，直到里面没有任何消息。从例子中可以看出，通过在响应类型前插入 stream 关键字，可以指定一个服务器端的流方法
  rpc ListFeatures(Rectangle) returns (stream Feature) {}
  
  //客户端流式 RPC,客户端写入一个消息序列并将其发送到服务器，同样也是使用流。一旦客户端完成写入消息，它等待服务器完成读取返回它的响应。通过在请求类型前指定 stream 关键字来指定一个客户端的流方法
  rpc RecordRoute(stream Point) returns (RouteSummary) {}
  
  //双向流式 RPC
  rpc RouteChat(stream RouteNote) returns (stream RouteNote) {}双向流式 RPC
}
```

# JSON 映射

```protobuf
//Proto3 支持JSON的编码规范，使他更容易在不同系统之间共享数据，在下表中逐个描述类型。
//如果JSON编码的数据丢失或者其本身就是null，这个数据会在解析成protocol buffer的时候被表示成默认值。


```

| proto3                 | JSON          | JSON示例                                | 注意                                                         |
| :--------------------- | :------------ | :-------------------------------------- | :----------------------------------------------------------- |
| message                | object        | {“fBar”: v, “g”: null, …}               | 产生JSON对象，消息字段名可以被映射成lowerCamelCase形式，并且成为JSON对象键，null被接受并成为对应字段的默认值 |
| enum                   | string        | “FOO_BAR”                               | 枚举值的名字在proto文件中被指定                              |
| map                    | object        | {“k”: v, …}                             | 所有的键都被转换成string                                     |
| repeated V             | array         | [v, …]                                  | null被视为空列表                                             |
| bool                   | true, false   | true, false                             |                                                              |
| string                 | string        | “Hello World!”                          |                                                              |
| bytes                  | base64 string | “YWJjMTIzIT8kKiYoKSctPUB+”              |                                                              |
| int32, fixed32, uint32 | number        | 1, -10, 0                               | JSON值会是一个十进制数，数值型或者string类型都会接受         |
| int64, fixed64, uint64 | string        | “1”, “-10”                              | JSON值会是一个十进制数，数值型或者string类型都会接受         |
| float, double          | number        | 1.1, -10.0, 0, “NaN”, “Infinity”        | JSON值会是一个数字或者一个指定的字符串如”NaN”,”infinity”或者”-Infinity”，数值型或者字符串都是可接受的，指数符号也可以接受 |
| Any                    | object        | {“@type”: “url”, “f”: v, … }            | 如果一个Any保留一个特上述的JSON映射，则它会转换成一个如下形式：`{"@type": xxx, "value": yyy}`否则，该值会被转换成一个JSON对象，`@type`字段会被插入所指定的确定的值 |
| Timestamp              | string        | “1972-01-01T10:00:20.021Z”              | 使用RFC 339，其中生成的输出将始终是Z-归一化啊的，并且使用0，3，6或者9位小数 |
| Duration               | string        | “1.000340012s”, “1s”                    | 生成的输出总是0，3，6或者9位小数，具体依赖于所需要的精度，接受所有可以转换为纳秒级的精度 |
| Struct                 | object        | { … }                                   | 任意的JSON对象，见struct.proto                               |
| Wrapper types          | various types | 2, “2”, “foo”, true, “true”, null, 0, … | 包装器在JSON中的表示方式类似于基本类型，但是允许nulll，并且在转换的过程中保留null |
| FieldMask              | string        | “f.fooBar,h”                            | 见fieldmask.proto                                            |
| ListValue              | array         | [foo, bar, …]                           |                                                              |
| Value                  | value         |                                         | 任意JSON值                                                   |
| NullValue              | null          |                                         | JSON null                                                    |

# 代码生成

```protobuf
//使用 protoc 工具可以把编写好的 proto 文件“编译”为Java, Python, C++, Go, Ruby, JavaNano, Objective-C,或C#代码
protoc --proto_path=IMPORT_PATH --cpp_out=DST_DIR --java_out=DST_DIR --python_out=DST_DIR --go_out=DST_DIR --ruby_out=DST_DIR --javanano_out=DST_DIR --objc_out=DST_DIR --csharp_out=DST_DIR path/to/file.proto

–cpp_out：生成c++代码
java_out ：生成java代码
python_out ：生成python代码
go_out ：生成go代码
ruby_out ：生成ruby代码
javanano_out ：适合运行在有资源限制的平台（如Android）的java代码
objc_out ：生成 Objective-C代码
csharp_out ：生成C#代码
php_out ：生成PHP代码


protoc --go_out=plugins=grpc:. login.proto

```

