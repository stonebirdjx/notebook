# chan

channel是Golang在语言层面提供的goroutine间的通信方式，比Unix管道更易用也更轻便。

> 一个channel同时仅允许被一个goroutine读写

数据结构定义：src/runtime/chan.go:hchan

数据结构可以看出channel由`队列、类型信息、goroutine、锁`等待队列组成

**创建channel**

```go
c := make(chan int,6)

// 底层原理如下
func makechan(t *chantype, size int) *hchan {
    var c *hchan
    c = new(hchan)
    c.buf = malloc(元素类型大小*size)
    c.elemsize = 元素类型大小
    c.elemtype = 元素类型
    c.dataqsiz = size

    return c
}
```

**阻塞**

从channel读数据，如果channel缓冲区为空或者没有缓冲区，当前goroutine会被阻塞

向channel写数据，如果channel缓冲区已满或者没有缓冲区，当前goroutine会被阻塞。

**关闭channel**

> close(c)

关闭channel时会把recvq中的G全部唤醒，本该写入G的数据位置为nil。把sendq中的G全部唤醒，但这些G会panic。

除此之外，panic出现的常见场景还有：

1. 关闭值为nil的channel ，`不要用var来定义一个chan， 如：var c chan int`
2. 关闭已经被关闭的channel
3. 向已经关闭的channel写数据

**单向channel**

- func readChan(chanName <-chan int)： 通过形参限定函数内部只能从channel中读取数据
- func writeChan(chanName chan<- int)： 通过形参限定函数内部只能向channel中写入数据

**select可以监控多channel**

```go
select {
    case e := <- chan1 :
    fmt.Printf("Get element from chan1: %d\n", e)
    case e := <- chan2 :
    fmt.Printf("Get element from chan2: %d\n", e)
    default:
    fmt.Printf("No element in chan1 and chan2.\n")
    time.Sleep(1 * time.Second)
}
```

**range 读取channel**

```go
for e := range chanName {
    fmt.Printf("Get element from chan: %d\n", e)
}
```

通过range可以持续从channel中读出数据，好像在遍历一个数组一样，当channel中没有数据时会阻塞当前goroutine，与读channel时阻塞处理机制一样。

# slice

Slice又称动态数组，依托数组实现，可以方便的进行扩容、传递等，实际使用中比数组更灵活。

`src/runtime/slice.go:slice`定义了Slice的数据结构：

```go
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

array指针指向底层数组，len表示切片长度，cap表示底层数组容量。

**创建Slice**

```go
slice := make([]int, 5, 10) // 创建长度为5.容量为10
slice := array[5:7]
```

**扩容**

使用append向Slice追加元素时，如果Slice空间不足，将会触发Slice扩容，扩容实际上重新一配一块更大的内存，将原Slice数据拷贝进新Slice，然后返回新Slice，扩容后再将数据追加进去。

> 如果原Slice容量小于1024，则新Slice容量将扩大为原来的2倍；
> 如果原Slice容量大于等于1024，则新Slice容量将扩大为原来的1.25倍；

**copy**

使用copy()内置函数拷贝两个切片时，会将源切片的数据逐个拷贝到目的切片指向的数组中，拷贝数量取两个切片长度的`最小值`。

> copy过程中不会发生扩容

**特殊slice**

切片同时也指定容量，即slice[start:en:cap], 其中cap即为新切片的容量，`当然容量不能超过原切片实际值`

```go
sliceA := make([]int, 5, 10)  //length = 5; capacity = 10
sliceB := sliceA[0:5]         //length = 5; capacity = 10
sliceC := sliceA[0:5:5]       //length = 5; capacity = 5
```

**total**

- 创建切片时可`跟据实际需要预分配容量`，尽量避免追加过程中扩容操作，有利于提升性能；尽量使用 s := make([]int,l,c)方式定义切片。
- 切片拷贝时需要判断实际拷贝的元素个数
- 谨慎使用多个切片操作同一个数组，以防读写冲突
- 每个切片都指向一个底层数组
- 每个切片都保存了当前切片的长度、底层数组可用容量
- 使用len()计算切片长度时间复杂度为O(1)，不需要遍历切片
- 使用cap()计算切片容量时间复杂度为O(1)，不需要遍历切片
- 通过函数传递切片时，不会拷贝整个切片，因为切片本身只是个结构体而矣
- 使用append()向切片追加元素时有可能触发扩容，扩容后将会生成新的切片

# map

map使用哈希表作为底层实现，一个哈希表里可以有多个哈希表节点，也即`bucket(哈希桶)`，而每个bucket就保存了map中的一个或一组键值对。

map数据结构由`runtime/map.go/hmap`定义:

bucket数据结构由`runtime/map.go/bmap`定义：

```go
type bmap struct {
    tophash [8]uint8 //存储哈希值的高8位
    data    byte[1]  //key value数据:key/key/key/.../value/value/value...
    overflow *bmap   //溢出bucket的地址
}
```

每个bucket可以存储8个键值对

**哈希冲突**

当有两个或以上数量的键被哈希到了同一个bucket时，我们称这些键发生了冲突。

> 由于每个bucket可以存放8个键值对，所以同一个bucket存放超过8个键值对时就会再创建一个键值对，用类似链表的方式将bucket连接起来。bucket 里面有个指针，地址指向另一个bucket

**负载因子**

负载因子用于衡量一个哈希表冲突情况。

> 负载因子 = 键数量/bucket数量

- 哈希因子过小，说明空间利用率低
- 哈希因子过大，说明冲突严重，存取效率低

**map扩容**

​	扩容前提

1. 负载因子 > 6.5时，也即平均每个bucket存储的键值对达到6.5个。
2. overflow数量 > 2^15时，也即overflow数量超过32768时。

**增量扩容**

> 当负载因子过大时，就新建一个bucket，新的bucket长度是原来的2倍，然后旧bucket数据搬迁到新的bucket。

考虑到如果map存储了数以亿计的key-value，一次性搬迁将会造成比较大的延时，`Go采用逐步搬迁策略，即每次访问map时都会触发一次搬迁，每次搬迁2个键值对。`

**等量扩容**

> 所谓等量扩容，实际上并不是扩大容量，buckets数量不变，重新做一遍类似增量扩容的搬迁动作，把松散的键值对重新排列一次，以使bucket的使用率更高，进而保证更快的存取。

**找查过程**

> 跟据key值算出哈希值 -> 取哈希值低位与hmpa.B取模确定bucket位置 -> 返回对应值或者0值（中间态省略）

**插入过程**

> 跟据key值算出哈希值 -> 取哈希值低位与hmap.B取模确定bucket位置 -> 插入或者更新

# struct

**Tag**

`Tag`本身是一个字符串，但字符串中却是：`以空格分隔的 key:value 对`。

> 注意：冒号前后不能有空格

```go
type Server struct {
    ServerName string `key1:"value1" key11:"value11"`
    ServerIP   string `key2:"value2"`
}
```

常见的tag用法，主要是JSON、xml数据解析、ORM映射等。

```go
type Server struct {
    ServerName string `key1:"value1" key11:"value11"`
    ServerIP   string `key2:"value2"`
}

func main() {
    s := Server{}
    st := reflect.TypeOf(s)

    field1 := st.Field(0)
    fmt.Printf("key1:%v\n", field1.Tag.Get("key1"))
    fmt.Printf("key11:%v\n", field1.Tag.Get("key11"))

    filed2 := st.Field(1)
    fmt.Printf("key2:%v\n", filed2.Tag.Get("key2"))
}
```

# iota

```go
const (
    mutexLocked = 1 << iota // mutex is locked
    mutexWoken
    mutexStarving
    mutexWaiterShift = iota
    starvationThresholdNs = 1e6
)
// mutexLocked == 1；mutexWoken == 2；mutexStarving == 4；mutexWaiterShift == 3；starvationThresholdNs == 1000000。
```

> iota代表了const声明块的行索引（下标从0开始）,在第n行就是 n-1

# string

源代码位于`src/builtin/builtin.go`

```go
ype stringStruct struct {
    str unsafe.Pointer
    len int
}
```

**[]byte转string**

string(b) 即可

1. 跟据切片的长度申请内存空间，假设内存地址为p，切片长度为len(b)；
2. 构建string（string.str = p；string.len = len；）
3. 拷贝数据(切片中数据拷贝到新申请的内存空间)

**string转[]byte**

[]byte(str)

- 申请切片内存空间
- 将string拷贝到切片

**字符串不允许修改**

string通常指向字符串字面量，而字符串字面量`存储位置是只读段，而不是堆或栈上`

**string转[]byte场景**

string和[]byte都可以表示字符串，但因数据结构不同，其衍生出来的方法也不同，要跟据实际应用场景来选择。

string 擅长的场景：

- 需要字符串比较的场景；
- 不需要nil字符串的场景；

[]byte擅长的场景：

- 修改字符串的场景，尤其是修改粒度为1个字节；
- 函数返回值，需要用nil表示含义的场景；
- 需要切片操作的场景；

# defer

defer语句用于延迟函数的调用。

延迟函数的参数在defer语句出现时就已经确定下来了

```go
func a() {
    i := 0
    defer fmt.Println(i) // print 0
    i++
    return
}
```

延迟函数执行按后进先出顺序执行，即先出现的defer最后执行

> 设计defer的初衷是简化函数返回时资源清理的动作，资源往往有依赖顺序,所以释放时要反向释放



延迟函数可能操作主函数的具名返回值，匿名返回不影响

```go
func foo() (ret int) {
    defer func() {
        ret++
    }()

    return 0
}

// return 1
```

**total**

- defer定义的延迟函数参数在defer语句出时就已经确定下来了
- defer定义顺序与实际执行顺序相反
- return不是原子操作，执行过程是: 保存返回值(若有)-->执行defer（若有）-->执行ret跳转
- 申请资源后立即使用defer关闭资源是好习惯

# select

select是Golang在语言层面提供的多路IO复用的机制

源码包`src/runtime/select.go:scase`定义了表示case语句的数据结构：

```go
type scase struct {
    c           *hchan         // chan
    kind        uint16
    elem        unsafe.Pointer // data element
}

// 使用
select {
    case <-chan1:
        fmt.Println("chan1 ready.")
    case <-chan2:
        fmt.Println("chan2 ready.")
    default:
    	 fmt.Println("dadada.")
}
```

**total**

- select语句中除default外，每个case操作一个channel，要么读要么写
- select语句中除default外，各case执行顺序是随机的
- select语句中如果没有default语句，则会阻塞等待任一case
- select语句中读操作要判断是否成功读取，关闭的channel也可以读取

# range

range是Golang提供的一种迭代遍历手段，可操作的类型有string, 数组、切片、Map、channel等，实际使用频率非常高。

**range for string**

> 返回的int32位的值

**range for slice**

> 遍历slice前会先获以slice的长度len_temp作为循环次数,所以循环过程中新添加的元素是没办法遍历到的。

**range for map**

> 遍历map时没有指定循环次数，循环体与遍历slice类似。由于map底层实现与slice不同，map底层使用hash表实现，插入数据位置是随机的，所以遍历过程中新插入的数据不能保证遍历到。

**total**

- 遍历过程中可以适情况放弃接收index或value，可以一定程度上提升性能
- 遍历channel时，如果channel中没有数据，可能会阻塞
- 尽量避免遍历过程中修改原数据

# mutex

互斥锁是并发程序中对共享资源进行访问控制的主要手段，对此Go语言提供了非常简单易用的Mutex，Mutex为一结构体类型，对外暴露两个方法Lock()和Unlock()分别用于加锁和解锁。

源码包`src/sync/mutex.go:Mutex`定义了互斥锁的数据结构：

```go
type Mutex struct {
    state int32 // 状态
    sema  uint32 // 信号量
}
```

> 加锁后立即使用defer对其解锁，可以有效的避免死锁。

> 加锁和解锁最好出现在同一个层次的代码块中，比如同一个函数。
>
> 重复解锁会引起panic，应避免这种操作的可能性。

# RWMutex

读写锁RWMutex，完整的表述应该是读写互斥锁，可以说是Mutex的一个改进版

RWMutex提供4个简单的接口来提供服务：

- RLock()：读锁定
- RUnlock()：解除读锁定
- Lock(): 写锁定，与Mutex完全一致
- Unlock()：解除写锁定，与Mutex完全一致

**total**

​	读锁 -> 可读不可写

​	写锁 -> 不可读不可写

# go程调度

**线程池的缺陷**

高并发应用中频繁创建线程会造成不必要的开销，所以有了线程池。线程池中预先保存一定数量的线程，任务发布到任务队列，线程池中的线程不断的从任务队列中取出任务并执行，可以有效的减少线程创建和销毁所带来的开销。

> 我们把任务队列中的每一个任务称作G，而G往往代表一个函数。 线程池中的线程worker线程不断的从任务队列中取出任务并执行。如果worker线程执行的G任务中发生系统调用，则操作系统会将该线程置为阻塞状态
>
> 如果任务队列中的大部分任务都会进行系统调用，则会让这种状态恶化，大部分worker线程进入阻塞状态，从而任务队列中的任务产生堆积。

`简单的说就是执行过程中阻塞了，就会造成任务大量积压`

**Goroutine调度器**

Go提供一种机制，可以在线程中自己实现调度，上下文切换更轻量，从而达到了线程数少，而并发数并不少的效果。而线程中调度的就是Goroutine.

Goroutine主要概念如下：

- G（Goroutine）: 即Go协程，每个go关键字都会创建一个协程。
- M（Machine）： 工作线程，在Go中称为Machine。
- P(Processor): 处理器（Go中定义的一个摡念，不是指CPU），包含运行Go代码的必要资源，也有调度goroutine的能力。

> M必须拥有P才可以执行G中的代码，P含有一个包含多个G的队列，P可以调度G交由M执行。

**Goroutine调度策略**

**队列轮转**

每个P维护着一个包含G的队列，不考虑G进入系统调用或IO操作的情况下，P周期性的将G调度到M中执行，执行一小段时间，将上下文保存下来，然后将G放到队列尾部，然后从队列中重新取出一个G进行调度。

除了每个P维护的G队列以外，还有一个全局的队列.之所以P会周期性的查看全局队列，也是为了防止全局队列中的G被饿死。

**系统调用**

P的个数默认等于CPU核数，每个M必须持有一个P才可以执行G，一般情况下M的个数会略大于P的个数，这多出来的M将会在G产生系统调用时发挥作用。类似线程池，Go也提供一个M的池子，需要时从池子中获取，用完放回池子，不够用时就再创建一个。

1. 如果有空闲的P，则获取一个P，继续执行Go。
2. 如果没有空闲的P，则将Go放入全局队列，等待被其他的P调度。然后Mo将进入缓存池睡眠。

**GOMAXPROCS设置对性能的影响**

# 内存分配原理

Golang中也实现了内存分配器，原理与tcmalloc类似，

> 简单的说就是维护一块大的全局内存，每个线程(Golang中为P)维护一块小的私有内存，私有内存不足再从全局申请。

为了方便自主管理内存，做法便是先向系统申请一块内存，然后将内存切割成小块，通过一定的内存分配算法管理内存。

预申请的内存划分为spans、bitmap、arena三部分。

> 其中arena即为所谓的堆区，应用中需要的内存从这里分配。
>
> 其中spans和bitmap是为了管理arena区而存在的。

arena的大小为512G，为了方便管理把arena区域划分成一个个的page，`每个page为8KB,一共有512GB/8KB个页`；

spans区域存放span的指针，每个指针对应一个page，所以span区域的大小为(512GB/8KB)*指针大小8byte = 512M

bitmap区域大小也是通过arena计算出来，不过主要用于GC。

**span**

span是用于管理arena页的关键数据结构，每个span中包含1个或多个连续页，为了满足小对象分配，span中的一页会划分更小的粒度，而对于大对象比如超过页大小，则通过多页实现。

**class**

跟据对象大小，划分了一系列class，每个class都代表一个固定大小的对象，以及每个span的大小。

```go
// class  bytes/obj  bytes/span  objects  waste bytes
//     1          8        8192     1024            0
//     2         16        8192      512            0
//     3         32        8192      256            0
//     4         48        8192      170           32
```

- class： class ID，每个span结构中都有一个class ID, 表示该span可处理的对象类型
- bytes/obj：该class代表对象的字节数
- bytes/span：每个span占用堆的字节数，也即页数*页大小
- objects: 每个span可分配的对象个数，也即（bytes/spans）/（bytes/obj）
- waste bytes: 每个span产生的内存碎片，也即（bytes/spans）%（bytes/obj）

**total**

1. Golang程序启动时申请一大块内存，并划分成spans、bitmap、arena区域
2. arena区域按页划分成一个个小块
3. span管理一个或多个页
4. mcentral管理多个span供线程申请使用
5. mcache作为线程私有资源，资源来源于mcentral

# 垃圾回收

所谓垃圾就是不再需要的内存块，这些垃圾如果不清理就没办法再次被分配使用，在不支持垃圾回收的编程语言里，这些垃圾内存就是泄露的内存。

**原理**

> 垃圾回收的核心就是标记出哪些内存还在使用中(即被引用到)，哪些内存不再使用了（即未被引用），把未被引用的内存回收掉，以供后续内存分配时使用。

**内存标记(Mark)**

> span中维护了一个个内存块，并由一个位图allocBits表示每个内存块的分配情况。
>
> 在span数据结构中还有另一个位图gcmarkBits用于标记内存块被引用情况。

allocBits记录了每块内存分配情况，而gcmarkBits记录了每块内存标记情况。标记阶段对每块内存进行标记，有对象引用的的内存标记为1，没有引用到的保持默认为0

标记结束就是内存回收，回收时将allocBits指向gcmarkBits，则代表标记过的才是存活的，gcmarkBits则会在下次标记时重新分配内存，非常的巧妙。

**三色标记法**

- 灰色：对象还在标记队列中等待
- 黑色：对象已被标记，gcmarkBits对应的位为1（该对象不会在本次GC中被清理）
- 白色：对象未被标记，gcmarkBits对应的位为0（该对象将会在本次GC中被清理）

> 初始状态下所有对象都是白色的

根对象引用了对象A、B,那么A、B变为灰色对象，接下来就开始分析灰色对象，

分析A时，A没有引用其他对象很快就转入黑色

B引用了D，则B转入黑色的同时还需要将D转为灰色

> 最终，黑色的对象会被保留下来，白色对象会被回收掉。

**垃圾回收优化（减少STW时间（Stop The World））**

最开始的版本，Golang中的STW（Stop The World）就是停掉所有的goroutine，专心做垃圾回收，待垃圾回收结束后再恢复goroutine。

​	**写屏障(Write Barrier)**

而写屏障就是让goroutine与GC同时运行的手段。虽然写屏障不能完全消除STW，但是可以大大减少STW的时间。

> 写屏障类似一种开关，在GC的特定时机开启，开启后指针传递时会把指针标记，即本轮不回收，下次GC时再确定。

GC过程中新分配的内存会被立即标记，用的并不是写屏障技术，也即GC过程中分配的内存不会在本轮GC中回收。

​	**辅助GC(Mutator Assist)**

> 为了防止内存分配过快，在GC执行过程中，如果goroutine需要分配内存，那么这个goroutine会参与一部分GC的工作，即帮助GC做一部分工作，这个机制叫作Mutator Assist。

**垃圾回收触发时机**

​	**内存分配量达到阀值触发GC**

每次内存分配时都会检查当前内存分配量是否已达到阀值，如果达到阀值则立即启动GC。

```
阀值 = 上次GC内存分配量 * 内存增长率
```

内存增长率由环境变量`GOGC`控制，默认为100，即每当内存扩大一倍时启动GC。

​	**定期触发GC**

默认情况下，最长2分钟触发一次GC，这个间隔在`src/runtime/proc.go:forcegcperiod`变量中被声明

​	**手动触发**

程序代码中也可以使用`runtime.GC()`来手动触发GC。这主要用于GC性能测试和统计。

**GC性能优化**

GC性能与对象数量负相关，对象越多GC性能越差，对程序影响越大。

> 所以GC性能优化的思路之一就是减少对象分配个数，比如对象复用或使用大对象组合多个小对象等等。

# 逃逸分析

所谓逃逸分析（Escape analysis）是指由编译器决定内存分配的位置，不需要程序员指定。 函数中申请一个新的对象

> - 如果分配在栈中，则函数执行结束可自动将内存回收；
> - 如果分配在堆中，则函数执行结束可交给GC（垃圾回收）处理;

**逃逸策略**

每当函数中申请新的对象，编译器会跟据该对象是否被函数外部引用来决定是否逃逸：

> 1. 如果函数外部没有引用，则优先放到栈中；
> 2. 如果函数外部存在引用，则必定放到堆中；

注意，对于函数外部没有引用的对象，也有可能放到堆中，比如内存过大超过栈的存储能力。

**逃逸场景**

​	**指针逃逸**

Go可以返回局部变量指针，这其实是一个典型的变量逃逸案例

```go
package main

type Student struct {
    Name string
    Age  int
}

func StudentRegister(name string, age int) *Student {
    s := new(Student) //局部变量s逃逸到堆

    s.Name = name
    s.Age = age

    return s
}

func main() {
    StudentRegister("Jim", 18)
}
```

函数StudentRegister()内部s为局部变量，其值通过函数返回值返回，`s本身为一指针，其指向的内存地址不会是栈而是堆`，这就是典型的逃逸案例。

通过编译参数-gcflags=-m可以查看编译过程中的逃逸分析

```shell
E:\SomeFile\gospace\prac\gcflag>go build  -gcflags=-m
# prac/gcflag
.\gcflag.go:16:6: can inline StudentRegister
.\gcflag.go:25:6: can inline main
.\gcflag.go:26:17: inlining call to StudentRegister
.\gcflag.go:16:22: leaking param: name
.\gcflag.go:17:10: new(Student) escapes to heap #逃逸到了堆
.\gcflag.go:26:17: new(Student) does not escape
```

​	**栈空间不足逃逸**

```go
package main

func Slice() {
    s := make([]int, 1000, 1000)
    // s := make([]int, 10000, 10000) # 此时栈空间不足会逃逸到堆

    for index, _ := range s {
        s[index] = index
    }
}

func main() {
    Slice()
}
```

 	**动态类型逃逸**

很多函数参数为interface类型，比如fmt.Println(a ...interface{})，编译期间很难确定其参数的具体类型，也人产生逃逸。

```go
package main

import "fmt"

func main() {
    s := "Escape"
    fmt.Println(s) //interface 会逃逸到堆
}
```

​	**闭包引用对象逃逸**

```go
func Fibonacci() func() int {
    a, b := 0, 1  // 闭包引用会将这两个值保存到堆上，内存逃逸
    return func() int {
        a, b = b, a+b
        return a
    }
}
```

> 闭包会保存外部函数局部变量值，局部变量的a和b由于闭包的引用，不得不将二者放到堆上，以致产生逃逸

**total**

- 栈上分配内存比在堆中分配内存有更高的效率
- 栈上分配的内存不需要GC处理
- 堆上分配的内存使用完毕会交给GC处理
- 逃逸分析目的是决定内分配地址是栈还是堆
- 逃逸分析在编译阶段完成

**函数传递指针真的比传值效率高吗？** 

我们知道传递指针可以减少底层值的拷贝，可以提高效率，但是如果拷贝的数据量小，由于指针传递会产生逃逸，可能会使用堆，也可能会增加GC的负担，所以传递指针不一定是高效的。

> 结构体方法可用指针，函数参数尽可能用值传递

# channel

channel一般用于协程之间的通信，channel也可以用于并发控制。比如主协程启动N个子协程，主协程等待所有子协程退出后再继续后续流程，这种场景下channel也可轻易实现。

使用channel来控制子协程的优点是实现简单，缺点是当需要大量创建协程时就需要有相同数量的channel，而且对于子协程继续派生出来的协程不方便控制。

WaitGroup、Context看起来比channel优雅一些

# WaitGroup

WaitGroup是Golang应用开发过程中经常使用的并发控制技术。

1. 启动goroutine前将计数器通过Add(2)将计数器设置为待启动的goroutine个数。
2. 启动goroutine后，使用Wait()方法阻塞自己，等待计数器变为0。
3. 每个goroutine执行结束通过Done()方法将计数器减1。
4. 计数器变为0后，阻塞的goroutine被唤醒。

**信号量**

信号量是Unix系统提供的一种保护共享资源的机制，用于防止多个线程同时访问某个资源。

可简单理解为信号量为一个数值：

- 当信号量>0时，表示资源可用，获取信号量时系统自动将信号量减1；
- 当信号量==0时，表示资源暂不可用，获取信号量时，当前线程会进入睡眠，当信号量为正时被唤醒；

> Add()操作必须早于Wait(), 否则会panic
>
> Add()设置的值必须与实际等待的goroutine个数一致，否则会panic

# context

Golang context是Golang应用开发常用的并发控制技术，它与WaitGroup最大的不同点是context对于派生goroutine有更强的控制力，它可以控制多级的goroutine。

- Context仅仅是一个接口定义，跟据实现的不同，可以衍生出不同的context类型；
- cancelCtx实现了Context接口，通过WithCancel()创建cancelCtx实例；
- timerCtx实现了Context接口，通过WithDeadline()和WithTimeout()创建timerCtx实例；
- valueCtx实现了Context接口，通过WithValue()创建valueCtx实例；
- 三种context实例可互为父节点，从而可以组合成不同的应用形式；

# 反射机制

1. 反射提供一种让程序检查自身结构的能力
2. 反射是困惑的源泉

**静态类型**

Go是静态类型语言，比如"int"、"float32"、"[]byte"等等。每个变量都有一个静态类型，且在编译时就确定了

> interface类型是一种特殊的类型，它代表方法集合。

**反射三定律**

Go提供一组方法提取interface的value

提供另一组方法提取interface的type.

**反射第一定律：反射可以将interface类型变量转换成反射对象**

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x float64 = 3.4
    t := reflect.TypeOf(x)  //t is reflext.Type
    fmt.Println("type:", t)

    v := reflect.ValueOf(x) //v is reflext.Value
    fmt.Println("value:", v)
}
```

**反射第二定律：反射可以将反射对象还原成interface对象**

之所以叫'反射'，反射对象与interface对象是可以互相转化的。

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x float64 = 3.4

    v := reflect.ValueOf(x) //v is reflext.Value

    var y float64 = v.Interface().(float64)
    fmt.Println("value:", y)
}
```

对象x转换成反射对象v，v又通过Interface()接口转换成interface对象，interface对象通过.(float64)类型断言获取float64类型的值。

**反射第三定律：反射对象可修改，value值必须是可设置的**

通过反射可以将interface类型变量转换成反射对象，可以使用该反射对象设置其持有的值。

`reflect.Value`提供了`Elem()`方法，可以获得指针向指向的`value`。

```go
package main

import (
	"reflect"
    "fmt"
)

func main() {
    var x float64 = 3.4
    v := reflect.ValueOf(&x)
    v.Elem().SetFloat(7.1)
    fmt.Println("x :", v.Elem().Interface())
}

// x : 7.1
```

# 单元测试

```go
package gotest_test

import (
    "testing"
    "gotest"
)

func TestAdd(t *testing.T) {
    var a = 1
    var b = 2
    var expected = 3

    actual := gotest.Add(a, b)
    if actual != expected {
        t.Errorf("Add(%d, %d) = %d; expected: %d", a, b, actual, expected)
    }
}
```

- 测试文件名必须以"_test.go"结尾；
- 测试函数名必须以“TestXxx”开始；
- 命令行下使用"go test"即可启动测试；

# 性能测试

```go
package gotest_test

import (
    "testing"
    "gotest"
)

func BenchmarkMakeSliceWithoutAlloc(b *testing.B) {
    for i := 0; i < b.N; i++ {
        gotest.MakeSliceWithoutAlloc()
    }
}

func BenchmarkMakeSliceWithPreAlloc(b *testing.B) {
    for i := 0; i < b.N; i++ {
        gotest.MakeSliceWithPreAlloc()
    }
}
```

- 文件名必须以“_test.go”结尾；
- 函数名必须以“BenchmarkXxx”开始；
- 使用命令“go test -bench=.”即可开始性能测试；

# 示例测试（doc）

```go
package gotest_test

import "gotest"

// 检测单行输出
func ExampleSayHello() {
    gotest.SayHello()
    // OutPut: Hello World
}

// 检测多行输出
func ExampleSayGoodbye() {
    gotest.SayGoodbye()
    // OutPut:
    // Hello,
    // goodbye
}
```

1. 例子测试函数名需要以"Example"开头；
2. 检测单行输出格式为“// Output: <期望字符串>”；
3. 检测多行输出格式为“// Output: \ <期望字符串> \ <期望字符串>”，每个期望字符串占一行；
4. 检测无序输出格式为"// Unordered output: \ <期望字符串> \ <期望字符串>"，每个期望字符串占一行；
5. 测试字符串时会自动忽略字符串前后的空白字符；
6. 如果测试函数中没有“Output”标识，则该测试函数不会被执行；
7. 执行测试可以使用`go test`，此时该目录下的其他测试文件也会一并执行；
8. 执行测试可以使用`go test <xxx_test.go>`，此时仅执行特定文件中的测试函数；

# 子测试（嵌套测试）

```go
package gotest_test

import (
    "testing"
    "gotest"
)

// sub1 为子测试，只做加法测试
func sub1(t *testing.T) {
    var a = 1
    var b = 2
    var expected = 3

    actual := gotest.Add(a, b)
    if actual != expected {
        t.Errorf("Add(%d, %d) = %d; expected: %d", a, b, actual, expected)
    }
}

// sub2 为子测试，只做加法测试
func sub2(t *testing.T) {
    var a = 1
    var b = 2
    var expected = 3

    actual := gotest.Add(a, b)
    if actual != expected {
        t.Errorf("Add(%d, %d) = %d; expected: %d", a, b, actual, expected)
    }
}

// sub3 为子测试，只做加法测试
func sub3(t *testing.T) {
    var a = 1
    var b = 2
    var expected = 3

    actual := gotest.Add(a, b)
    if actual != expected {
        t.Errorf("Add(%d, %d) = %d; expected: %d", a, b, actual, expected)
    }
}

// TestSub 内部调用sub1、sub2和sub3三个子测试
func TestSub(t *testing.T) {
    // setup code

    t.Run("A=1", sub1)
    t.Run("A=2", sub2)
    t.Run("B=1", sub3)

    // tear-down code
}
//func (t *T) Run(name string, f func(t *T)) bool
```

`Run()`会启动新的协程来执行`f`，并阻塞等待`f`执行结束才返回，除非`f`中使用`t.Parallel()`设置子测试为并发。

命令行下，使用`-v`参数执行测试

```
E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src\gotest>go test subunit_test.go -v
=== RUN   TestSub
=== RUN   TestSub/A=1
=== RUN   TestSub/A=2
=== RUN   TestSub/B=1
--- PASS: TestSub (0.00s)
    --- PASS: TestSub/A=1 (0.00s)
    --- PASS: TestSub/A=2 (0.00s)
    --- PASS: TestSub/B=1 (0.00s)
PASS
ok      command-line-arguments  0.354s
```

**子测试命名规则**

"*<父测试名字>*/*<传递给Run的名字>*"。比如，传递给`Run()`的名字是“A=1”，那么子测试名字为“TestSub/A=1”。这个在上面的命令行输出中也可以看出。

**过滤筛选**

只执行上例中“A=*”的子测试，那么执行时使用`-run Sub/A=`参数即可，

子性能测试则使用`-bench`参数来筛选

**子测试并发**

前面提到的多个子测试共享setup和teardown有一个前提是子测试没有并发，如果子测试使用`t.Parallel()`指定并发，那么就没办法共享teardown了，因为执行顺序很可能是setup->子测试1->teardown->子测试2...。

```go
package gotest_test

import (
    "testing"
    "time"
)

// 并发子测试，无实际测试工作，仅用于演示
func parallelTest1(t *testing.T) {
    t.Parallel()
    time.Sleep(3 * time.Second)
    // do some testing
}

// 并发子测试，无实际测试工作，仅用于演示
func parallelTest2(t *testing.T) {
    t.Parallel()
    time.Sleep(2 * time.Second)
    // do some testing
}

// 并发子测试，无实际测试工作，仅用于演示
func parallelTest3(t *testing.T) {
    t.Parallel()
    time.Sleep(1 * time.Second)
    // do some testing
}

// TestSubParallel 通过把多个子测试放到一个组中并发执行，同时多个子测试可以共享setup和tear-down
func TestSubParallel(t *testing.T) {
    // setup
    t.Logf("Setup")

    t.Run("group", func(t *testing.T) {
        t.Run("Test1", parallelTest1)
        t.Run("Test2", parallelTest2)
        t.Run("Test3", parallelTest3)
    })

    // tear down
    t.Logf("teardown")
}
```

```go
E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src\gotest>go test subparallel_test.go -v -run SubParallel
=== RUN   TestSubParallel
=== RUN   TestSubParallel/group
=== RUN   TestSubParallel/group/Test1
=== RUN   TestSubParallel/group/Test2
=== RUN   TestSubParallel/group/Test3
--- PASS: TestSubParallel (3.01s)
        subparallel_test.go:25: Setup
    --- PASS: TestSubParallel/group (0.00s)
        --- PASS: TestSubParallel/group/Test3 (1.00s)
        --- PASS: TestSubParallel/group/Test2 (2.01s)
        --- PASS: TestSubParallel/group/Test1 (3.01s)
        subparallel_test.go:34: teardown
PASS
ok      command-line-arguments  3.353s
```

- 子测试适用于单元测试和性能测试；
- 子测试可以控制并发；
- 子测试提供一种类似table-driven风格的测试；
- 子测试可以共享setup和tear-down；

# main测试

有时希望在整个测试程序做一些全局的setup和Tear-down，这时就需要Main测试了。

```go
// TestMain 用于主动执行各种测试，可以测试前后做setup和tear-down操作
func TestMain(m *testing.M) {
    println("TestMain setup.")

    retCode := m.Run() // 执行测试，包括单元测试、性能测试和示例测试

    println("TestMain tear-down.")

    os.Exit(retCode)
}
```

# go test 工作机制

go test运行时，跟据是否指定package分为两种模式，即本地目录模式和包列表模式。

**本地目录模式**

当执行测试并没有指定package时，即以本地目录模式运行，例如使用"go test"或者"go test -v"来启动测试。

本地目录模式下，go test编译当前目录的源码文件和测试文件，并生成一个二进制文件，最后执行并打印结果。

**包列表模式**

当执行测试并显式指定package时，即以包列表模式运行，例如使用"go test math"来启动测试。

**缓存机制**

当满足一定的条件，测试的缓存是自动启用的，也可以显式的关闭缓存。

可缓存参数集合如下：

- -cpu
- -list
- -parallel
- -run
- -short
- -v

需要注意的是，测试参数必须全部来自这个集合，其结果才会被缓存，没有参数或包含任一此集合之外的参数，结果都不会缓存。

**使用缓存结果**

使用缓存结果也需要满足一定的条件：

- 本次测试的二进制及测试参数与之前的一次完全一致；
- 本次测试的源文件及环境变量与之前的一次完全一致；
- 之前的一次测试结果是成功的；
- 本次测试运行模式是列表模式

```go
E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src>go test gotest
ok      gotest  3.434s

E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src>go test gotest
ok      gotest  (cached)
```

**禁用缓存**

测试时使用一个不在“可缓存参数”集合中的参数，就不会使用缓存，比较常用的方法是指定一个参数“-count=1”。

```go
E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src>go test gotest
ok      gotest  3.434s

E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src>go test gotest
ok      gotest  (cached)

E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src>go test gotest -count=1
ok      gotest  3.354s
```

第三次执行使用了参数"-count=1"，所以执行时不会从缓存中获取结果。

# go test 参数

**-args**

指示go test把-args后面的参数带到测试中去。具体的测试函数会跟据此参数来控制测试流程。

-args后面可以附带多个参数，所有参数都将以字符串形式传入，每个参数做为一个string，并存放到字符串切片中。



**-json**

-json 参数用于指示go test将结果输出转换成json格式，以方便自动化测试解析使用。



**-o**

-o 参数指定生成的二进制可执行程序，并执行测试，测试结束不会删除该程序。



**-bench regexp**

> go test默认不执行性能测试，使用-bench参数才可以运行，而且只运行性能测试函数。

其中正则表达式用于筛选所要执行的性能测试。如果要执行所有的性能测试，使用参数"-bench ."或"-bench=."。



**-benchtime s**

-benchtime指定每个性能测试的执行时间，如果不指定，则使用默认时间1s。

例如，执定每个性能测试执行2s，则参数为："go test -bench Sub/A=1 -benchtime 2s"。



**-cpu 1,2,4**

-cpu 参数提供一个CPU个数的列表，提供此列表后，那么测试将按照这个列表指定的CPU数设置GOMAXPROCS并分别测试。

比如“-cpu 1,2”，那么每个测试将执行两次，一次是用1个CPU执行，一次是用2个CPU执行。 例如，使用命令"go test -bench Sub/A=1 -cpu 1,2,3,4" 执行测试：

```
BenchmarkSub/A=1                    1000           1256835 ns/op
BenchmarkSub/A=1-2                  2000            912109 ns/op
BenchmarkSub/A=1-3                  2000            888671 ns/op
BenchmarkSub/A=1-4                  2000            894531 ns/op
```

测试结果中测试名后面的-2、-3、-4分别代表执行时GOMAXPROCS的数值。



**-count n**

-count指定每个测试执行的次数，默认执行一次。



**-failfast**

默认情况下，go test将会执行所有匹配到的测试，并最后打印测试结果，无论成功或失败。

-failfast指定如果有测试出现失败，则立即停止测试。



**-list regexp**

-list 只是列出匹配成功的测试函数，并不真正执行。



**-parallel n**

指定测试的最大并发数。

当测试使用t.Parallel()方法将测试转为并发时，将受到最大并发数的限制，默认情况下最多有GOMAXPROCS个测试并发，其他的测试只能阻塞等待。



**-run regexp**

跟据正则表达式执行单元测试和示例测试。正则匹配规则与-bench 类似。



**-timeout d**

默认情况下，测试执行超过10分钟就会超时而退出。

```
E:\OpenSource\GitHub\RainbowMango\GoExpertProgrammingSourceCode\GoExpert\src\gotest>go test -timeout=1s
TestMain setup.
panic: test timed out after 1s
```

- 按秒设置：-timeout xs或-timeout=xs
- 按分设置：-timeout xm或-timeout=xm
- 按时设置：-timeout xh或-timeout=xh
- 

**-v**

默认情况下，测试结果只打印简单的测试结果，-v 参数可以打印详细的日志。



**-benchmem**

默认情况下，性能测试结果只打印运行次数、每个操作耗时。使用-benchmem则可以打印每个操作分配的字节数、每个操作分配的对象数。

```go
func BenchmarkMakeSliceWithoutAlloc(b *testing.B) {
    for i := 0; i < b.N; i++ {
        gotest.MakeSliceWithoutAlloc() // 一次操作
    }
}

// 没有使用-benchmem
BenchmarkMakeSliceWithoutAlloc-4            2000            971191 ns/op

// 使用-benchmem
BenchmarkMakeSliceWithoutAlloc-4            2000            914550 ns/op         4654335 B/op         30 allocs/op
```

# pprof 性能分析

[pprof](https://github.com/google/pprof) 就是用来解决这个问题的。pprof 包含两部分：

- 编译到程序中的 `runtime/pprof` 包

- 性能剖析工具 `go tool pprof`

  

CPU 性能分析

内存性能分析

阻塞性能分析

锁性能分析

**数据分析**

> 生成 profile，`go tool pprof` 分析这份数据，需要安装Graphviz

# 定时器

Go提供了两种定时器，此处分为一次性定时器、周期性定时器。

- 一次性定时器：定时器只计时一次，结束便停止；
- 周期性定时器：定时器周期性进行计时，除非主动停止，否则将永久运行；

# Timer

一定性定时器的使用方法及其使用原理

Timer实际上是一种单一事件的定时器，即经过指定的时间后触发一个事件，这个事件通过其本身提供的channel进行通知。是因为Timer只执行一次就结束，这也是Timer与Ticker的最重要的区别之一。

通过timer.NewTimer(d Duration)可以创建一个timer，参数即等待的时间，时间到来后立即触发一个事件。

源码包`src/time/sleep.go:Timer`定义了Timer数据结构：

```go
type Timer struct { // Timer代表一次定时，时间到来后仅发生一个事件。
    C <-chan Time
    r runtimeTimer
}
```

Timer对外仅暴露一个channel，指定的时间到来时就往该channel中写入系统时间，也即一个事件。

**使用场景**

​	**设定超时时间**

```go
func WaitChannel(conn <-chan string) bool {
    timer := time.NewTimer(1 * time.Second)

    select {
    case <- conn:
        timer.Stop()
        return true
    case <- timer.C: // 超时
        println("WaitChannel timeout!")
        return false
    }
}
```

​	**延迟执行某个方法**

```go
func DelayFunction() {
    timer := time.NewTimer(5 * time.Second)

    select {
    case <- timer.C:
        log.Println("Delayed 5s, start to do something.")
    }
}
```

**简单接口**

**After()**

有时我们就是想等指定的时间，没有需求提前停止定时器，也没有需求复用该定时器，那么可以使用匿名的定时器。

`func After(d Duration) <-chan Time`方法创建一个定时器，并返回定时器的管道，如下代码所示：

```go
func AfterDemo() {
    log.Println(time.Now())
    <- time.After(1 * time.Second)
    log.Println(time.Now())
}
```

AfterDemo()两条打印时间间隔为1s，实际还是一个定时器，但代码变得更简洁。

**AfterFunc()**

前面我们例子中讲到延迟一个方法的调用，实际上通过AfterFunc可以更简洁。AfterFunc的原型为：

```go
func AfterFunc(d Duration, f func()) *Timer
```

该方法在指定时间到来后会执行函数f。例如：

```go
func AfterFuncDemo() {
    log.Println("AfterFuncDemo start: ", time.Now())
    time.AfterFunc(1 * time.Second, func() {
        log.Println("AfterFuncDemo end: ", time.Now())
    })

    time.Sleep(2 * time.Second) // 等待协程退出
}
```

AfterFuncDemo()中先打印一个时间，然后使用AfterFunc启动一个定器，并指定定时器结束时执行一个方法打印结束时间。

**total**

- time.NewTimer(d)创建一个Timer;
- timer.Stop()停掉当前Timer;
- timer.Reset(d)重置当前Timer;

# Ticker

Ticker是周期性定时器

Ticker的数据结构与Timer完全一致：

```go
type Ticker struct {
    C <-chan Time
    r runtimeTimer
}
```

**使用场景**

​	**简单定时任务**

有时，我们希望定时执行一个任务，这时就可以使用ticker来完成。

下面代码演示，每隔1s记录一次日志：

```go
// TickerDemo 用于演示ticker基础用法
func TickerDemo() {
    ticker := time.NewTicker(1 * time.Second)
    defer ticker.Stop()

    for range ticker.C {
        log.Println("Ticker tick.")
    }
}
```

​	**定时聚合任务**

```go
func TickerLaunch() {
    ticker := time.NewTicker(5 * time.Minute)
    maxPassenger := 30                   // 每车最大装载人数
    passengers := make([]string, 0, maxPassenger)

    for {
        passenger := GetNewPassenger() // 获取一个新乘客
        if passenger != "" {
            passengers = append(passengers, passenger)
        } else {
            time.Sleep(1 * time.Second)
        }

        select {
        case <- ticker.C:               // 时间到，发车
            Launch(passengers)
            passengers = []string{}
        default:
            if len(passengers) >= maxPassenger {  // 时间没到，车已座满，发车
                Launch(passengers)
                passengers = []string{}
            }
        }
    }
}
```

> Ticker在使用完后务必要释放，否则会产生资源泄露，进而会持续消耗CPU资源，最后会把CPU耗尽。

**错误示例**

Ticker用于for循环时，很容易出现意想不到的资源泄露问题，下面代码演示了一个泄露问题：

```go
func WrongTicker() {
    for {
        select {
        case <-time.Tick(1 * time.Second):
            log.Printf("Resource leak!")
        }
    }
}
```

上面代码，select每次检测case语句时都会创建一个定时器，for循环又会不断的执行select语句，所以系统里会有越来越多的定时器不断的消耗CPU资源，最终CPU会被耗尽。

**total**

Ticker相关内容总结如下：

- 使用time.NewTicker()来创建一个定时器；
- 使用Stop()来停止一个定时器；
- 定时器使用完毕要释放，否则会产生资源泄露；

# 语法糖

最常用的语法糖莫过于赋值符`:=`，其次，表示函数变参的`...`。

# golang高性能

**字符串高效拼接**

综合易用性和性能，一般推荐使用 `strings.Builder` 来拼接字符串。

`strings.Builder` 和 `+` 性能和内存消耗差距如此巨大，是因为两者的内存分配方式不一样。

`strings.Builder` 和 `bytes.Buffer` 底层都是 `[]byte` 数组，但 `strings.Builder` 性能比 `bytes.Buffer` 略快约 10% 。一个比较重要的区别在于，`bytes.Buffer` 转化为字符串时重新申请了一块空间，存放生成的字符串变量，而 `strings.Builder` 直接将底层的 `[]byte` 转换成了字符串类型返回了回来。



**切片(slice)性能及陷阱**

因此为了减少内存的拷贝次数，容量在比较小的时候，一般是以 2 的倍数扩大的，例如 2 4 8 16 …，当达到 2048 时，会采取新的策略，避免申请内存过大，导致浪费。

**性能陷阱**

在已有切片的基础上进行切片，不会创建新的底层数组

因此很可能出现这么一种情况，原切片由大量的元素构成，但是我们在原切片的基础上切片，虽然只使用了很小一段，但底层数组在内存中仍然占据了大量空间，得不到释放。

```go
func lastNumsBySlice(origin []int) []int {
	return origin[len(origin)-2:]
}

func lastNumsByCopy(origin []int) []int {
	result := make([]int, 2)
	copy(result, origin[len(origin)-2:])
	return result
}
```

> 比较推荐的做法，使用 `copy` 替代 `re-slice`。



**for 和 range 的性能比较**

range 在迭代过程中返回的是迭代值的拷贝，如果每次迭代的元素的内存占用很低，那么 for 和 range 的性能几乎是一样，

例如 `[]int`。但是如果迭代的元素内存占用较高，

例如一个包含很多属性的 struct 结构体，那么 for 的性能将显著地高于 range，有时候甚至会有上千倍的性能差异。

> 对于这种场景，建议使用 for，如果使用 range，建议只迭代下标，通过下标访问迭代值，这种使用方式和 for 就没有区别了。如果想使用 range 同时迭代下标和值，则需要将切片/数组的元素改为指针，才能不影响性能。



**反射(reflect)性能**

使用反射赋值，效率非常低下，如果有替代方案，尽可能避免使用反射，特别是会被反复调用的热点代码。

`FieldByName` 相比于 `Field` 有一个数量级的性能劣化。那在实际的应用中，就要避免直接调用 `FieldByName`。我们可以利用字典将 `Name` 和 `Index` 的映射缓存起来。`先将值存在内存中，免得每次都去查`



**使用空结构体节省内存**

我们可以使用 `unsafe.Sizeof` 计算出一个数据类型实例需要占用的字节数

空结构体 struct{} 实例不占据任何的内存空间。

**空结构体的作用**

因为空结构体不占据内存空间，因此被广泛作为各种场景下的占位符使用。一是节省资源，二是空结构体本身就具备很强的语义，即这里不需要任何值，仅作为占位符。

**实现集合(Set)**

```go
type Set map[string]struct{}

func (s Set) Has(key string) bool {
	_, ok := s[key]
	return ok
}

func (s Set) Add(key string) {
	s[key] = struct{}{}
}

func (s Set) Delete(key string) {
	delete(s, key)
}

func main() {
	s := make(Set)
	s.Add("Tom")
	s.Add("Sam")
	fmt.Println(s.Has("Tom"))
	fmt.Println(s.Has("Jack"))
}
```

**不发送数据的信道(channel)**

```go
func worker(ch chan struct{}) {
	<-ch
	fmt.Println("do something")
	close(ch)
}

func main() {
	ch := make(chan struct{})
	go worker(ch)
	ch <- struct{}{}
}
```

**仅包含方法的结构体**

```go
type Door struct{}

func (d Door) Open() {
	fmt.Println("Open the door")
}

func (d Door) Close() {
	fmt.Println("Close the door")
}
```



**内存对齐对性能的影响**

CPU 访问内存时，并不是逐个字节访问，而是以字长（word size）为单位访问。比如 32 位的 CPU ，字长为 4 字节，那么 CPU 访问内存的单位也是 4 字节。

这么设计的目的，是减少 CPU 访问内存的次数，加大 CPU 访问内存的吞吐量。比如同样读取 8 个字节的数据，一次读取 4 个字节那么只需要读取 2 次。

如果不进行内存对齐，很可能增加 CPU 访问内存的次数

> 简言之：合理的内存对齐可以提高内存读写的性能，并且便于实现变量操作的原子性。

**struct 内存对齐的技巧**

```text
bool size: 1
int32 size: 4
int8 size: 1
int64 size: 8
byte size: 1
string size: 16
```

```go
type demo1 struct {
	a int8
	b int16
	c int32
}

type demo2 struct {
	a int8
	c int32
	b int16
}

func main() {
	fmt.Println(unsafe.Sizeof(demo1{})) // 8
	fmt.Println(unsafe.Sizeof(demo2{})) // 12
}
```

> 在对内存特别敏感的结构体的设计上，我们可以通过调整字段的顺序，减少内存的占用。

对齐系数

- 32 位：4
- 64 位：8

**空 struct{} 的对齐**

空 `struct{}` 大小为 0，作为其他 struct 的字段时，一般不需要内存对齐。但是有一种情况除外：即当 `struct{}` 作为结构体最后一个字段时，需要内存对齐。因为如果有指针指向该字段, 返回的地址将在结构体之外，如果此指针一直存活不释放对应的内存，就会有内存泄露的问题（该内存不因结构体释放而释放）。

```
type demo3 struct {
	c int32
	a struct{}
}

type demo4 struct {
	a struct{}
	c int32
}

func main() {
	fmt.Println(unsafe.Sizeof(demo3{})) // 8
	fmt.Println(unsafe.Sizeof(demo4{})) // 4
}
```



**读写锁和互斥锁的性能比较**

- 读写比为 9:1 时，读写锁的性能约为互斥锁的 8 倍
- 读写比为 1:9 时，读写锁性能相当
- 读写比为 5:5 时，读写锁的性能约为互斥锁的 2 倍

> 读写锁，读得时候没有锁比较快，写得时间与互斥锁无异



**如何退出协程(超时场景)**

父go程退出，子go程不会退出

**如何避免**

创建有缓冲区的 channel

即创建channel `done` 时，即使没有接收方，发送方也不会发生阻塞。

使用select

```go
func do2phases(phase1, done chan bool) {
	time.Sleep(time.Second) // 第 1 段
	select {
	case phase1 <- true:
	default:
		return
	}
	time.Sleep(time.Second) // 第 2 段
	done <- true
}

func timeoutFirstPhase() error {
	phase1 := make(chan bool)
	done := make(chan bool)
	go do2phases(phase1, done)
	select {
	case <-phase1:
		<-done
		fmt.Println("done")
		return nil
	case <-time.After(time.Millisecond):
		return fmt.Errorf("timeout")
	}
}

func Test2phasesTimeout(t *testing.T) {
	for i := 0; i < 1000; i++ {
		timeoutFirstPhase()
	}
	time.Sleep(time.Second * 3)
	t.Log(runtime.NumGoroutine())
}
```

> 不能强制 kill goroutine ，每一个 goroutine 都需要承担自己退出的责任。



**如何退出协程(其他场景)**

channel 接收操作 v, beforeClosed := <-ch

- `t, beforeClosed := <-taskCh` 判断 channel 是否已经关闭，beforeClosed 为 false 表示信道已被关闭。若关闭，则不再阻塞等待，直接返回，对应的协程随之退出。
- `sendTasks` 函数中，任务发送结束之后，使用 `close(taskCh)` 将 channel taskCh 关闭。

> 一个常用的使用Go通道的原则是不要在数据接收方或者在有多个发送者的情况下关闭通道。换句话说，我们只应该让一个通道唯一的发送者关闭此通道。

礼貌的方式，用 sync.Once 或互斥锁(sync.Mutex)确保 channel 只被关闭一次。

```go
type MyChannel struct {
	C    chan T
	once sync.Once
}

func NewMyChannel() *MyChannel {
	return &MyChannel{C: make(chan T)}
}

func (mc *MyChannel) SafeClose() {
	mc.once.Do(func() {
		close(mc.C)
	})
}
```



**控制协程(goroutine)的并发数量**

使用channel控制

利用第三方库

目前有很多第三方库实现了协程池，可以很方便地用来控制协程的并发数量，比较受欢迎的有：

- [Jeffail/tunny](https://github.com/Jeffail/tunny)
- [panjf2000/ants](https://github.com/panjf2000/ants)

调整系统资源的上限（不推荐）

ulimit

虚拟内存

# sync.Pool 复用临时对象

> 一句话总结：保存和复用临时对象，减少内存分配，降低 GC 压力。

sync.Pool 的大小是可伸缩的，高负载时会动态扩容，存放在池中的对象如果不活跃了会被自动清理。

sync.Pool 的使用方式非常简单：

```go
var studentPool = sync.Pool{
    New: func() interface{} { 
        return new(Student) 
    },
}

// get put
stu := studentPool.Get().(*Student)
json.Unmarshal(buf, stu)
studentPool.Put(stu)
```

存粹的内存搬运工，耗时几乎可以忽略。

# sync.Once 的使用场景

`sync.Once` 是 Go 标准库提供的使函数只执行一次的实现，常应用于单例模式，例如初始化配置、保持数据库连接等。作用与 `init` 函数类似，但有区别。

sync.Once 可以在代码的任意位置初始化和调用，因此可以延迟到使用时再执行，并发场景下是线程安全的。

```go
func (o *Once) Do(f func())
```

#  sync.Cond 的使用场景

场景

协程在异步地接收数据，剩下的多个协程必须等待这个协程接收完数据，才能读取到正确的数据。

每个 Cond 实例都会关联一个锁 L（互斥锁 *Mutex，或读写锁 *RWMutex），当修改条件或者调用 Wait 方法时，必须加锁。

```go
func NewCond(l Locker) *Cond
func (c *Cond) Broadcast()
func (c *Cond) Signal()
func (c *Cond) Wait()
```

```go
var done = false

func read(name string, c *sync.Cond) {
	c.L.Lock()
	for !done {
		c.Wait()
	}
	log.Println(name, "starts reading")
	c.L.Unlock()
}

func write(name string, c *sync.Cond) {
	log.Println(name, "starts writing")
	time.Sleep(time.Second)
	c.L.Lock()
	done = true
	c.L.Unlock()
	log.Println(name, "wakes all")
	c.Broadcast()
}

func main() {
	cond := sync.NewCond(&sync.Mutex{})

	go read("reader1", cond)
	go read("reader2", cond)
	go read("reader3", cond)
	write("writer", cond)

	time.Sleep(time.Second * 3)
}
```

- `done` 即互斥锁需要保护的条件变量。
- `read()` 调用 `Wait()` 等待通知，直到 done 为 true。
- `write()` 接收数据，接收完成后，将 done 置为 true，调用 `Broadcast()` 通知所有等待的协程。
- `write()` 中的暂停了 1s，一方面是模拟耗时，另一方面是确保前面的 3 个 read 协程都执行到 `Wait()`，处于等待状态。main 函数最后暂停了 3s，确保所有操作执行完毕。

# 减少编译体积

-ldflags="-s -w" 可以减少20%

```go
go build -ldflags="-s -w" -o server main.go
```

- -s：忽略符号表和调试信息。
- -w：忽略DWARFv3调试信息，使用该选项后将无法使用gdb进行调试。

使用 upx 减小体积 linux需要安装，[upx](https://github.com/upx/upx) 是一个常用的压缩动态库和可执行文件的工具，通常可减少 50-70% 的体积。

# 死码消除

死码消除(dead code elimination, DCE)是一种编译器优化技术，用处是在编译阶段去掉对程序运行结果没有任何影响的代码。

