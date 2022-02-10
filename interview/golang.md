[toc]

# 1、golang基础

<font color=red>函数体外每个语句应该以关键关键字开始（var、func、type、const、import、package）等</font>

<font color=red>常量：以const定义</font>，常量可以是字符、字符串、布尔值或数值。不能是函数

类型转换：T(v)

关键字：break、default、func、interface、case、defer、go、map、chan、else、goto、package、const、fallthrough、if、range、continue、for、import、return

内建函数：

|        函数名        |                             作用                             |
| :------------------: | :----------------------------------------------------------: |
|        close         |       close（ch）用于channel 通讯。使用它来关闭channel       |
|        delete        |              delete(m,key)用于在map 中删除键。               |
|      len 、cap       | 可用于不同的类型， len 用于返回字符串、slice 和数组的长度，cap用于返回申请空间的长度 |
|         new          |      t := new(T) 用于各种类型的内存分配，返回指针类型。      |
|         make         | s := make([]int),用于内建类型（map、slice 和channel）的内存分配。 |
|         copy         |          copy(dst []Type, src []Type)用于复制slice           |
|        append        |        s :=append(s,ele),用于追加slice,生成新的slice         |
|    panic、recover    |                       用于异常处理机制                       |
|    print、println    |       是底层打印函数，可以在不引入fmt 包的情况下使用。       |
| complex、real 、imag |                       全部用于处理复数                       |

从命令行读取参数 ：os.argv 和 flag 包

打印到io.writer

## 1.1、golang基本类型

基本类型

```go
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
// uintptr is an integer type that is large enough to hold the bit pattern of
// any pointer.
byte // uint8 的别名 0-255 ascll
rune // int32 的别名
    // 表示一个 Unicode 码点 或 utf-8
float32 float64
complex64 complex128
```

零值

```go
int uint //数值类型为 0，
bool //布尔类型为 false，
string //字符串为 ""（空字符串）。
//而其他类型（指针、切片、映射、通道、函数和接口）的零值则是 nil
```

## 1.2、变量初始化

```go
//函数体外 var 关键字定义
var a = 10  //类型可以自动右值推导。
var a int = 10
var a int = a()
//函数体内 
a := 10 //:=精简模式只能在函数体内
a := func a(){}()
```

## 1.3、函数返回值

函数返回值分为<font color=red>匿名放回、具名返回、nil值</font>

```go
func split(sum int) (int,int) {
    return 10,20
}
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return //具名返回，要么不带参数，要么全部带上
    //如果函数没有返回值，默认是nil，return也是nil
    // return 10 , y 会自动把10赋值给x并返回
}
```

## 1.4、for循环

golang 只有for循环，基本的 for 循环由三部分组成，它们用分号隔开：for $1;$2;$3{} <font color="red"> {} 必须有，()只能用在$2上，不能for($1;$2;$3){}</font>

```go
for $1;$2;$3{} //标准情况 $1初始化语句：在第一次迭代前执行；$2条件表达式：在每次迭代前求值，$3后置语句：在每次迭代的结尾执行
for ;a<10;{} //初始化语句和后置语句是可选的。此时可以去掉;
for a<10{} 
for {} //无限循环
```

## 1.5、if条件语句

同 for 一样， if 语句可以在条件表达式前执行一个简单的初始化语句。有条件表达式的时候才能使用（）， {}必须有。

```GO
func pow(x, n, lim float64) float64 {
    // 常用于,ok 模式，或者,err模式
	if v := math.Pow(x, n); v < lim { 
		return v
	}
	return lim
}
```

## 1.6、switch分支选择

switch能将一长串 if-then-else 写得更加清晰。没有条件的 switch 同 switch true 一样。

fallthough 会跳过case 判断，直接执行下一个case的代码语句，<font color="red"> 如果执行到最后case仍然有fallthough ，default也会执行。</font>

```go
a := 10
switch a {
	case 6:
		fmt.Println("6")
	case 10:
		fmt.Println("10")
		fallthrough
	case 12:
		fmt.Println("12")
		fallthrough
	default:
		fmt.Println("13")
}  // 10 12 13

t := time.Now()
switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
}
```

## 1.7、defer

defer 相当于java、python 中finally，多个defer执行的顺序是先进后出。

defer 只能延迟最外层函数，函数内部的函数会立即执行, defer f4(f3())

```go
// 所有函数在执行return返回指令之前，都会先检查是否存在defer语句，若存在则先逆序调用defer语句进行收尾工作再退出返回；
// 在defer语句中只能访问有名返回值，而不能直接访问匿名返回值；
func increaseA() int {
    var i int
    defer func() {
        i++  //访问不到匿名值，所以最后return 0
    }()
    return i
}

func increaseB() (r int) {
    defer func() {
        r++
    }()
    return r
}

func main() {
    fmt.Println(increaseA()) // 0
    fmt.Println(increaseB()) // 1
}
```

## 1.8、指针

指针是保存了值的内存地址，类型 *T 是指向 T 类型值的指针。其零值为 nil。

<font color="red"> 与 C 不同，Go 没有指针运算。 当然可以用usafe包 操作运算指针</font>

```go
var p *int
i := 42 
p = &i //& 操作符会生成一个指向其操作数的指针。
fmt.Println(*p) // 通过指针 p 读取 i , * 操作符表示指针指向的底层值。
*p = 21         // 通过指针 p 设置 i ,这也就是通常所说的“间接引用”或“重定向”。
// interface{}能接收任何类型， 只有interfave{} 没有 *interface{}  
```

## 1.9、数组与切片

数组：类型 [n]T 表示拥有 n 个 T 类型的值的数组,数组的长度是其类型的一部分，因此<font color="red">数组不能改变大小。</font>

切片：类型 []T 表示一个元素类型为 T 的切片。 切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。

切片 s 的长度和容量可通过表达式 len(s) 和 cap(s) 来获取。nil 切片的长度和容量为 0 且没有底层数组。

```go
//在进行切片时，你可以利用它的默认行为来忽略上下界。切片下界的默认值为 0，上界则是该切片的长度。
var a [10]int //下面情况是等价的
a[0:10]、a[:10]、a[0:]、a[:]

//切片 s 的长度和容量可通过表达式 len(s) 和 cap(s) 来获取。
a := make([]int, 5)  // len(a)=5
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

//append 函数追加切片
s = append(s, 1)
s = append(s, 2, 3, 4) // 可以一次性添加多个元素
s = append(s, []int{1,2,3}...) //追加一个新切片，go语法糖

//可以向字典一样初始化
var a = [4]string{
		1:"hjx",
		2:"nan",
    	"nxlg"  //不指定索引。默认是前面的索引 +1
		0:"18",
	}

//copy
a := []int{2, 3, 5, 7, 11, 13}
c := []int{77,78}
d := make([]int, len(a))
copy(d, a)
fmt.Println(d) //[2 3 5 7 11 13]
copy(c, a)//当复制的元素和被复制的元素个数不一致时 ,不会扩大被复制slice的cap,会在len范围内产生覆盖
fmt.Println(c) //[2,3]

```

## 1.10、range

range，for 循环的 range 形式可遍历数组、切片、映射、字符串、channel。

```go
//除channel外其他都是返回 索引和value
for i,v := range [str|[]|[n]|map ]{
   
} 
//channel 直接返回chnnel成员
for element  := range chan{
    
}
```

## 1.11、map映射（字典）

映射 (`map`),映射将键映射到值。映射的零值为 nil 。nil 映射既没有键，也不能添加键。make 函数会返回给定类型的映射，并将其初始化备用。

```go
var mp map[string]string // nil map 不能添加键
var mp = map[string]string{} // 不是nil，申请了空间，可以添加键 len(mp) == 0
var mp = make(map[string]string)
m[key] = elem //在映射 m 中插入或修改元素：
elem := m[key] //获取元素，最好用if ele，ok := m[key];ok{} 来取字典的值，
// 空key 返回其类型的默认值
delete(m, key) // 删除元素
```

## 1.12、函数值传递

函数也是值。它们可以像其它值一样传递。函数值可以用作函数的参数或返回值。 

```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}
hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
}
```

## 1.13、golang方法

Go 没有类。不过你可以为结构体类型定义方法。方法就是一类带特殊的 接收者 参数的函数。//方法只是个带接收者参数的函数。

```go
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
//你可以为指针接收者声明方法。指针接收者比值接收者更常用。
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
//而以指针为接收者的方法被调用时，接收者既能为值又能为指针：
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
p := &v
fmt.Println(p.Abs()) // OK这种情况下，方法调用 p.Abs() 会被解释为 (*p).Abs()。
//永远不要使用(*interface{})一个指针指向一个接口类型，因为它已经是一个指针。
```

## 1.14、interface接口

接口(interface)：接口类型 是由一组方法签名定义的集合。接口类型的变量可以保存任何实现了这些方法的值。

类型通过实现一个接口的所有方法来实现该接口。接口也是值。它们可以像其它值一样传递。

接口值可以用作函数的参数或返回值。

```go
//在内部，接口值可以看做包含值和具体类型的元组：
fmt.Printf("(%v, %T)\n", iterface, iterface)//(value, type)

//即便接口内的具体值为 nil，方法仍然会被 nil 接收者调用。
//注意: 保存了 nil 具体值的接口其自身并不为 nil。

//类型断言，提供了访问接口值底层具体值的方式。
t, ok := i.(T)//若 i 保存了一个 T，那么 t 将会是其底层值，而 ok 为 true。否则，ok 将为 false 而 t 将为 T 类型的零值，程序并不会产生恐慌。请注意这种语法和读取一个映射时的相同之处。

switch v := i.(type) {
case T:
    // v 的类型为 T
case S:
    // v 的类型为 S
default:
    // 没有匹配，v 与 i 的类型相同
}
```

## 1.15、error

go 程序使用 error 值来表示错误状态。error 为 nil 时表示成功；非 nil 的 error 表示失败。

```go
err := errors.New("have a err")
err := err := fmt.Errorf("%s a err","have")
//注意：函数不要返回bool值，因为if funca(){} 毫无意义。
```

## 1.16、goroutine

Go 程（goroutine）是由 Go 运行时管理的轻量级线程。信道（chan）是带有类型的管道，你可以通过它用信道操作符 <- 来发送或者接收值。

```go
//带缓冲的信道，信道可以是 带缓冲的。将缓冲长度作为第二个参数提供给 make 来初始化一个带缓冲的信道：
ch := make(chan int, 100)
ch <- v   // 将 v 发送至信道 ch。
v := <-ch  // 从 ch 接收值并赋予 v。
//最好用，ok模式来取值
if v, ok := <-ch ;ok{}
//range 遍历channel
for ele := range ch{
   
}
//for if 遍历channel
for {
    if v, ok := <-ch ;ok{
        
    }else{
        break
    }
}
//close 关闭一个信道来表示没有需要发送的值了。
close(ch) //只有发送者才能关闭信道，而接收者不能。向一个已经关闭的信道发送数据会引发程序恐慌（panic）

// 多路io 复用，select 语句使一个 Go 程可以等待多个通信操作。
select {
case i := <-c1:
    // 使用 i
case j := <-c1:
  	// 使用 j
default:
  // 从 c 中接收会阻塞时执行
}
```

## 1.17、fmt.Printf

```go
%d	十进制整数
%x, %o, %b	十六进制，八进制，二进制整数。
%f, %g, %e 浮点数	3.141593 3.141592653589793 3.141593e+00
%t 	true或false
%c	字符（rune） (Unicode码点)
%s 	字符串
%q	带双引号的字符串"abc"或带单引号的字符'c'
%v 	变量的自然形式（natural format）值
%+v	类似%v，但输出结构体时会添加字段名
%#v	值的Go语法表示
%T 	变量的类型
%% 	字面上的百分号标志（无操作数）
'+'	总是输出数值的正负号；对%q（%+q）会生成全部是ASCII字符的输出（通过转义）；
' '	对数值，正数前加空格而负数前加负号；
'-'	在输出右边填充空白而不是默认的左边（即从默认的右对齐切换为左对齐）；
'#'	切换格式：
  	八进制数前加0（%#o），十六进制数前加0x（%#x）或0X（%#X），指针去掉前面的0x（%#p）；
 	对%q（%#q），如果strconv.CanBackquote返回真会输出反引号括起来的未转义字符串；
 	对%U（%#U），输出Unicode格式后，如字符可打印，还会输出空格和单引号括起来的go字面值；
  	对字符串采用%x或%X时（% x或% X）会给各打印的字节之间加空格；
'0'	使用0而不是空格填充，对于数值类型会把填充的0放在正负号后面；
```

## 1.18、垃圾回收和 SetFinalizer

经典的GC算法有三种： 引用计数(reference counting)、 标记-清扫(mark&sweep)、 复制收集(CopyandCollection)。<font color="red">Golang的GC算法主要是基于标记-清扫(markandsweep)算法，并在此基础上做了改进(三色并发标记法)。</font>

Go 开发者不需要写代码来释放程序中不再使用的变量和结构占用的内存，在 Go 运行时中有一个独立的进程，即垃圾收集器（GC）,通过调用 `runtime.GC()` 函数可以显式的触发 GC，但这只在某些罕见的场景下才有用，比如当内存资源不足时调用 `runtime.GC()`，它会在此函数执行的点上立即释放一大片内存，此时程序可能会有短时的性能下降（因为 `GC` 进程在执行）。

```go
// 查看当前内存状态
var m runtime.MemStats
runtime.ReadMemStats(&m)
fmt.Printf("%d Kb\n", m.Alloc / 1024)
```

## 1.19、位运算

| 符合 | 解释 |                                                              |
| ---- | ---- | ------------------------------------------------------------ |
| &    | 与   | 两个位都为1时，结果才为1                                     |
| \|   | 或   | 两个位都为0时，结果才为0                                     |
| ^    | 异或 | 两个位相同为0，相异为1                                       |
| ~    | 取反 | 0变1，1变0                                                   |
| <<   | 左移 | 各二进位全部左移若干位，高位丢弃，低位补0                    |
| >>   | 右移 | 各二进位全部右移若干位，对无符号数，高位补0，有符号数，各编译器处理方法不一样，有的补符号位（算术右移），有的补0（逻辑右移） |

# 2、golang高级

## 2.1、带标签的结构体

结构体中的字段除了有名字和类型外，还可以有一个可选的标签（tag）：它是一个附属于字段的字符串，可以是文档或其他的重要标记，<font color="red">只有包 reflect 能获取它。</font>

```go
type TagType struct { // tags
	field1 bool   "An important answer"
	field2 string "The name of the thing"
	field3 int    `json:"field_3"`
}
func main() {
	tt := TagType{true, "Barak Obama", 1}
	for i := 0; i < 3; i++ {
		refTag(tt, i)
	}
}
func refTag(tt TagType, ix int) {
	ttType := reflect.TypeOf(tt)
	ixField := ttType.Field(ix)
	fmt.Printf("%v\n", ixField.Tag)
}
```

## 2.2、嵌套结构体和匿名字段

结构体可以包含一个或多个 **匿名（或内嵌）字段**，类似于面向对象的继承概念，在 Go 语言中，相比较于继承，组合更受青睐。

```go
type innerS struct {
    in1 int
    in2 int
}
type outerS struct {
    b    int
    c    float32
    int  // anonymous field
    innerS //anonymous field
}
func main() {
    outer := new(outerS)
    outer.b = 6
    outer.c = 7.5
    outer.int = 60
    outer.in1 = 5
    outer.in2 = 10
    fmt.Println(outer) //&{6 7.5 60 {5 10}}
}

// 注意 二义性，如果两结构体有 共有的字段A ，那么不能用 outerS.A 应该用 outerS.outerA.A ，outerS.outerB.A  
```

## 2.3、接口嵌套、类型断言、type-switch

```go
type ReadWrite interface {
    Read(b Buffer) bool
    Write(b Buffer) bool
}
type Lock interface {
    Lock()
    Unlock()
}
type File interface {
    ReadWrite
    Lock
    Close() //嵌套结构体 可以有自己的方法
}

//类型断言，varI 必须是一个接口变量
if v, ok := varI.(T); ok {  // checked type assertion
    Process(v)
    return
}

//type-switch
switch t := areaIntf.(type) {
case *Square:
    fmt.Printf("Type Square %T with value %v\n", t, t)
case *Circle:
    fmt.Printf("Type Circle %T with value %v\n", t, t)
case nil:
    fmt.Printf("nil value: nothing to check?\n")
default:
    fmt.Printf("Unexpected type %T\n", t)
}
```

## 2.4 读取用户输入

```go
//读取用户输入fmt.Scanln os.stdin：
//明确变量可以用fmt.Scanln，指向其地址即可。如果对象中有空格“hjx nan”采用标准输入
var (
        name string
        age  int
)
fmt.Print("输入姓名和年龄，使用空格分隔：")
fmt.Scanln(&name, &age)
fmt.Scanf("%s : %d",&name,&age)

//fmt的scanln需要先声明变量，指向地址
//明确变量时使用fmt，不明确变量时使用bufio.NewReader
inputReader := bufio.NewReader(os.Stdin)
input, err := inputReader.ReadString('\n')
if err != nil{
       panic(err)
}
fmt.Println(input)
```

## 2.5、读写文件与文件拷贝

文件使用指向 os.File 类型的指针（*os.File）来表示的，也叫做文件句柄。

```go
inputFile, _ := os.Open("input.dat")
inputReader := bufio.NewReader(inputFile)
for {
	inputString, readerError := inputReader.ReadString('\n') //按行读取
    if readerError == io.EOF {
        return
    }
    fmt.Printf("The input was: %s", inputString)
}
buf, err := ioutil.ReadFile(inputFile) //读取整个文件
//带缓冲的读取 ，文件的内容是不按行划分的，或者干脆就是一个二进制文件。在这种情况下
buf := make([]byte, 1024)
n, err := inputReader.Read(buf)
if (n == 0) { break}
//案列读取 一行一行拼起来的
_, err := fmt.Fscanln(file, &v1, &v2, &v3)
//写文件 bufio.NewWriter

文件拷贝io.copy
src, err := os.Open(srcName)
dst, err := os.OpenFile(dstName, os.O_WRONLY|os.O_CREATE, 0644)
io.Copy(dst, src)
```

## 2.6、json

```go

json.Marshal() //func Marshal(v interface{}) ([]byte, error)，下面是数据编码后的 JSON 文本（实际上是一个 []byte）
var f interface{} //json 包使用 map[string]interface{} 和 interface{} 储存任意的 JSON 对象和数组
err := json.Unmarshal(b, &f)
//编码和解码流
//json 包提供 Decoder 和 Encoder 类型来支持常用 JSON 数据流读写。NewDecoder 和 NewEncoder 函数分别封装了 io.Reader 和 io.Writer 接口。

//要想把 JSON 直接写入文件，可以使用 json.NewEncoder 初始化文件（或者任何实现 io.Writer 的类型），并调用 Encode()；反过来与其对应的是使用 json.Decoder （从文件读取json） 和 Decode() 函数：
func NewDecoder(r io.Reader) *Decoder
func NewEncoder(w io.Writer) *Encoder
func (dec *Decoder) Decode(v interface{}) error
func (enc *Encoder) Encode(v interface{}) error
```

## 2.7、gob数据传输

​	用Gob传输数据：类似于 Python 的 "pickle" ，Gob 通常用于远程方法调用（RPCs，参见 15.9 的 rpc 包）参数和结果的传输，以及应用程序和机器之间的数据传输。存入.gob文件，自身语言读取更加高效。

## 2.8、错误处理

用switch-type方式来处理错误。

```go
type error interface {
    Error() string
}
type PathError struct {
    Op string    // “open”, “unlink”, etc.
    Path string  // The associated file.
    Err error  // Returned by the system call.
}

func (e *PathError) String() string {
    return e.Op + “ ” + e.Path + “: “+ e.Err.Error()
}
errors.New("xxx")
switch err := err.(type) {
    case ParseError:
        PrintParseError(err)
    case PathError:
        PrintPathError(err)
    ... 
    default:
        fmt.Printf(“Not a special error, just %s\n”, err)
}
```

## 2.9、pannic、recover

```go
//panic 可以直接从代码初始化：当错误条件（我们所测试的代码）很严苛且不可恢复，程序不能继续运行时，可以使用panic 函数产生一个中止程序的运行时错误。
//recover 只能在 defer 修饰的函数中使用：用于取得 panic 调用中传递过来的错误值，如果是正常执行，调用 recover 会返回 nil，且没有其它效果。
panic(err)
defer func() {
        if e := recover(); e != nil {
            fmt.Printf("Panicing %s\r\n", e)
        }
}()
```

## 2.10、逗号ok模式

，ok，第一个参数是一个值或者nil，第二个参数是true/false或者一个错误error。在一个需要赋值的if条件语句中，使用这种模式去检测第二个参数值会让代码显得优雅简洁。

```go
if value, err := pack1.Func1(param1); err != nil {
        …
        return err
}
if value, isPresent = map1[key1]; isPresent {
        Process(value)
}
// key1不存在
if value, ok := varI.(T); ok {
    Process(value)
}
if input, open := <-ch; !open {
    break // 通道是关闭的
}
Process(input)
```

# 3、高级整理

1、<font color="red">defer 的执行顺序是先进后出。出现panic语句的时候，会先按照 defer 的后进先出顺序执行，最后才会执行panic。</font>

2、在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名。如果有多个返回值必须加上括号();如果只有一个返回值且命名也需要加上括号()。

```go
func funcMui(x, y int) (sum int, error) {
	//返回值应该是(sum int ,err error)
    return x + y, nil
}
```

3、make 和new的区别：

```go
//new(T)  和 make(T, args)  是Go语言内建函数，用来分配内存，但适用的类型不用。
//new(T) 会为了 T 类型的新值分配已置零的内存空间，并返回地址（指针），即类型为 *T 的值。换句话说就是，返回一个指针，该指针指向新分配的、类型为 T 的零值。new()适用于值类型，如 数组 、 结构体 等。
//make(T, args) 返回初始化之后的T类型的值，也不是指针 *T ，是经过初始化之后的T的引用。 make() 适用于 slice 、 map 和 channel 。
```

4、不能对指针执行append操作

```go
func main() {
    list := new([]int)
    list = append(list, 1)
    fmt.Println(list)
}
//new([]int) 之后的 list 是一个 *int[] 类型的指针，不能对指针执行 append 操作。可以使用 make() 初始化之后再用。同样的， map 和 channel 建议使用 make() 或字面量的方式初始化，不要用 new 。
```

5、append第二个参数不能是slice

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{4, 5}
    s1 = append(s1, s2) // 编译不过 ，append() 的第二个参数不能直接使用 slice ，需使用 ... 操作符，将一个切片追加到另一个切片上： append(s1, s2...) 。或者直接跟上元素，形如： append(s1, 1, 2, 3)  。
    fmt.Println(s1)
}
```

6、结构体比较

```go
func main() {
    sn1 := struct {
        age  int
        name string
	}{age: 11, name: "qq"}
	sn2 := struct {
        age  int
        name string
	}{age: 11, name: "11"}

    if sn1 == sn2 {
        fmt.Println("sn1 == sn2")
    }

    sm1 := struct {
        age int
        m   map[string]string
    }{age: 11, m: map[string]string{"a": "1"}}
    sm2 := struct {
        age int
        m   map[string]string
    }{age: 11, m: map[string]string{"a": "1"}}

    if sm1 == sm2 {
        fmt.Println("sm1 == sm2")
    }
}
考点是结构体的比较，有几个需要注意的地方：
//结构体只能比较是否相等，但是不能比较大小；
//想同类型的结构体才能进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关；
//如果struct的所有成员都可以比较，则该struct就可以通过==或!=进行比较是否相同，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等；

//可以比较的有bool、数值型、字符、指针、数组等
//不能比较的有slice、map、函数
```

7、& 取址运算符， * 指针解引用，过指针变量p访问其成员变量name的方式有 p.name  或者 (*p).name

8、关于 init 函数 ：`一个包可以出现多个init()函数，一个源文件也可以包含多个init()函数`；同一个包中多个init()函数的`执行顺序没有明确的定义`，但是不同包的init函数是`根据包导入的依赖关系决定init执行顺序`的；init函数在代码中不能被显示调用、不能被引用（赋值给函数变量），否则出现编译失败；`引入包，不可出现死循环。即A import B，B import A，这种情况下编译失败；`

9、i.(type)，`其中i是接口，type是固定关键字`，需要注意的是，只有接口类型才可以使用类型选择。

10、`s[starte:end:length]`,截取操作符还可以有第三个参数，形如 [i,j,k]，第三个参数 k 用来限制新切片的容量，但不能超过原数组（切片）的底层数组大小。<font color="red">截取获得的切片的长度和容量分别是：j-i、k-i。</font>

11、cap() 函数不适用 map

12、fmt.Printf，%d表示输出十进制数字，+表示输出数值的符号。这里不表示取反。常用 %s ，%v ，%f，%T

13、关于iota

```go
const (
    x = iota
    _
    y
    z = "zz"
    k
    p = iota
)
const(
   h = iota  //h 为0 会被重新定义
   j
)
func main() {
    fmt.Println(x, y, z, k, p) //0 2 zz zz 5
}
//iota 在 const 关键字出现时将被重置为0，const中每新增一行常量声明将使 iota 计数一次。

```

14、在执行 defer 语句的时候会保存一份副本,  defer hello(i) //会先用i当前值存一份副本。

15、var m map[string]int 声明map 没有申请空间不能直接赋值，最好使用make来声明map。

16、defer 与返回值

```go
//多个defer的执行顺序为“后进先出”；
//所有函数在执行return返回指令之前，都会先检查是否存在defer语句，若存在则先逆序调用defer语句进行收尾工作再退出返回；
//匿名返回值是在return执行时被声明，有名返回值则是在函数声明的同时被声明，因此在defer语句中只能访问有名返回值，而不能直接访问匿名返回值；
//因此defer、return、返回值三者的执行顺序应该是：return最先给返回值赋值；接着defer开始执行一些收尾工作；最后RET指令携带返回值退出函数。
```

17、函数参数为 interface{} 时可以接收任何类型的参数，包括用户自定义类型等，即使是接收指针类型也用 interface{}，而不是使用 *interface{}（接收参数没有*interface{}一说）。

18、golang 的字符串类型是不能赋值 nil 的，也不能跟 nil 比较。

19、切片与append

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := s1[1:]
    s2[1] = 4
    fmt.Println(s1)  //[1,2,4]  当使用 s1[1:] 获得切片 s2，和 s1 共享同一个底层数组，这会导致 s2[1] = 4 语句影响 s1。
    s2 = append(s2, 5, 6, 7)  append 操作会导致底层数组扩容，生成新的数组，因此追加数据后的 s2 不会影响 s1。
    fmt.Println(s1)  //[1,2,4] 
}
```

20、类型和方法必须定义在同一代码块内

```go
func (i int) PrintInt ()  {
    fmt.Println(i)
}
func main() {
    var i int = 1
    i.PrintInt()  //int 类型创建了 PrintInt() 方法，由于 int 类型和方法 PrintInt() 定义在不同的包内，所以编译出错。
}
```

21、map value 寻址问题

```go
type Math struct {
	x, y int
}
var m = map[string]Math{
	"foo": Math{2, 3},
}
func main() {
	m["foo"].x = 4 //对于类似 X = Y的赋值操作，必须知道 X 的地址，才能够将 Y 的值赋给 X，但 go 中的 map 的 value 本身是不可寻址的。
	// 可以增加临时变量 mh := m["foo"] ; mh.x = 4
	// 或者使用指针 var m = map[string]&Math{"foo": &Math{2, 3},}
	fmt.Println(m["foo"].x)
}
```





