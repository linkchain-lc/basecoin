## 命名规范

#### 基本命名方法
* 做有意义的区分：Product和ProductInfo和ProductData没有区别，NameString和Name没有区别，要区分名称，就要以读者能鉴别不同之处的方式来区分 
* 使用可搜索的名称：单字母名称和数字常量很难从一大堆文字中搜索出来。单字母名称仅适用于短方法中的本地变量，名称长短应与其作用域相对应。若变量或常量可能在代码中多处使用，则应赋其以便于搜索的名称
* 结构体属性或是函数的名字不应该缩写
* 局部变量应该简短。
> 作用域原则：作用域越长，比如函数，全局变量等，其名字描述约详细。作用域短的变量，比如说for, if, 函数局部变量等，其变量应短小简洁。

#### 包名
* 以小写的单个单词来命名,不应使用下划线或驼峰记法
* 包名应简短
```
如src/pkg/encoding/base64 中包名为base64， 而非encoding_base64 或 encodingBase64

另一个简短的例子是 once.Do，once.Do(setup) 表述足够清晰， 使用 once.DoOrWaitUntilDone(setup)完全就是画蛇添足
```
* 包名应该用单数的形式，比如util、model, 而不是utils、models

#### getter/setter
* getter第一个字母大写Var
* setter为SetVar

例有一个owner （小写，未导出）的字段:
* getter应当名为Owner（第一个字母大写），而非 GetOwner
* setter命名为SetOwner 
```
owner := obj.Owner()
if owner != user {
	obj.SetOwner(user)
}
```

#### 接口名
* 包含一个方法的接口应当以该方法的名称加上-er后缀来命名
```
type Reader interface {
	Read(p []byte) (n int, err error)
}
```
* 两个函数的接口名综合两个函数名
```
type WriteFlusher interface {
    Write([]byte) (int, error)
    Flush() error
}
```
* 三个以上函数的接口名，类似于结构体名
```
type Car interface {
    Start([]byte) 
    Stop() error
    Recover()
}
```
#### 驼峰式命名
除包名外，Go中约定使用驼峰记法 
```
MixedCaps 或 mixedCaps。
```

#### 缩写词视为一个整体
url, http, id等为缩写词，其大小写应视为一个整体
比如：
```
定义变量为: urlPony", or "URLPony"
而不是UrlPony

定义变量为appID, 而不是appId

定义变量为xmlHTTPRequest" or "XMLHTTPRequest"
而不是XmlHttpRequest
```

## 注释
* 每个包都应包含一段包注释，对于包含多个文件的包， 包注释只需出现在其中的任一文件中即可。包注释应在整体上对该包进行介绍，并提供包的相关信息。例如
```
/*
	regexp 包为正则表达式实现了一个简单的库。

	该库接受的正则表达式语法为：

	正则表达式:
		串联 { '|' 串联 }
	串联:
		{ 闭包 }
	闭包:
		条目 [ '*' | '+' | '?' ]
	条目:
		'^'
		'$'
		'.'
		字符
		'[' [ '^' ] 字符遍历 ']'
		'(' 正则表达式 ')'
*/
package regexp
```
* 每个可导出（首字母大写）的名称都应该有文档注释
* 注释的第一句应当以被声明的东西开头，并且是单句的摘要
```
// Compile 用于解析正则表达式并返回，如果成功，则 Regexp 对象就可用于匹配所针对的文本。
func Compile(str string) (regexp *Regexp, err error) {

// Request represents a request to run a command.
type Request struct { ...

// Encode writes the JSON encoding of req to w.
func Encode(w io.Writer, req *Request) { ...
```

## 包管理

#### 不要使用相对路径引入包
```
// 这是不好的导入
import “../net”

// 这是正确的做法
import “github.com/repo/proj/src/net”
```

#### 导入包遵循顺序
包的导入遵循顺序，依次为系统库，项目内部库，第三方库，以空格分组
```
//import 
import (
    //系统库
    "fmt"
    "math/rand"
    "os"
    "time"

    //项目内部库
    linchain/xx/
    
    
    //第三方库
    "github.com/spf13/pflag"
)
```

## 变量
#### 变量申明
* 变量名采用驼峰标准，不要使用_来命名变量名
* 多个变量申明放在一起
```
var (
    Found bool
    count int
)
```

## 错误处理
#### 基本原则
* 大部分函数需要返回错误信息
* 尽量避免使用panic
* 把自定义的Error放在package级别中，统一进行维护,并且变量以Err开头
```
var (
	ErrCacheMiss = errors.New("memcache: cache miss")
	ErrCASConflict = errors.New("memcache: compare-and-swap conflict")
	ErrNotStored = errors.New("memcache: item not stored")
	ErrServerError = errors.New("memcache: server error")
	ErrNoStats = errors.New("memcache: no statistics available")
	ErrMalformedKey = errors.New("malformed: key is too long or contains invalid characters")
	ErrNoServers = errors.New("memcache: no servers configured or available")
)
```

* error作为函数的值返回,必须对error进行处理
```
不要采用下面的处理错误写法

    if err != nil {
        // error handling
    } else {
        // normal code
    }
采用下面的写法

    if err != nil {
        // error handling
        return // or continue, etc.
    }
    // normal code
使用函数的返回值时，则采用下面的方式

x, err := f()
if err != nil {
    // error handling
    return
}
// use x
```
#### 错误字符串
* 错误字符串应尽可能地指明它们的来源，例如产生该错误的包名前缀。例如在 image 包中，由于未知格式导致解码错误的字符串为“image: unknown format”
* 错误字符串以小写开头，不以标点符号结尾。
```
That is, use fmt.Errorf("something bad") not fmt.Errorf("Something bad"), so that log.Printf("Reading %s: %v", filename, err) formats without a spurious capital letter mid-message
```

## 参数传递
* 对于少量数据，不要传递指针
* 对于大量数据的struct可以考虑使用指针
* 传入参数是map，slice，chan不要传递指针，因为map，slice，chan是引用类型，不需要传递指针的指针


## 异步编程
#### goroutine
* 保持goroutine代码简单
* 明确goroutine的生存（结束）周期。如果代码里面并不明确，则通过文档注明其生存周期
* goroutine被channel阻塞的时候，存在内存泄露的可能
```
gc并不会回收被阻塞的goroutine, 即便是引起其阻塞的channel已经被删除
https://github.com/golang/go/wiki/CodeReviewComments#goroutine-lifetimes
```
* 当goroutine不需要的时候，及时退出(确保生产者不再通过channel发送的时候)
> Close only when producing routines can be verified as no longer able to send on the channel being closed
* 再次强调，要明确及追踪goroutine的退出
> Track completion of goroutines. sync.WaitGroup is your friend

#### channel
* consumer和producer分工明确，可能的情况下，使用单工channel。
> recvOnly <-chan Thing are your friends
 

## 单元测试
* 单元测试文件名命名规范为 example_test.go
* 测试用例的函数名称必须以 Test 开头，例如：TestExample


## 工具
* 使用gofmt来格式化代码

## 其他技巧
#### 申明一个空的slice
使用
```
var t []string
```
而不是
```
t := []string{}
```


#### 空字符串检查
不要使用下面的方式检查空字符串:
```
if len(s) == 0 {
	...
}
```
而是使用下面的方式
```
if s == "" {
	...
}
```

#### 非空slice检查
不要使用下面的方式检查空的slice:
```
if s != nil && len(s) > 0 {
    ...
}
```
直接比较长度即可：
```
if len(s) > 0 {
    ...
}
```

#### 直接使用bool值
使用
```
if b {
    ...
}
if !b {
    ...
}
```
而不是
```
if b == true {
    ...
}
if b == false {
    ...
}
```

#### byte/string slice相等性比较
使用
```
var s1 []byte
   var s2 []byte
   ...
bytes.Equal(s1, s2) == 0
bytes.Equal(s1, s2) != 0

```
而不是
```
var s1 []byte
   var s2 []byte
   ...
bytes.Compare(s1, s2) == 0
bytes.Compare(s1, s2) != 0
```

#### 正则表达式中使用raw字符串避免转义字符
使用
```
regexp.MustCompile(`\.`) 
regexp.Compile(`\.`)
```
而不是
```
regexp.MustCompile("\\.") 
regexp.Compile("\\.")
```

#### 尽早return
一旦有错误发生，马上返回
```
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```
参考文件：
* https://go-zh.org/doc/effective_go.html
* https://github.com/golang/go/wiki/CodeReviewComments


