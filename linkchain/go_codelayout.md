## 代码布局
```
//package
package context

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

//const定义
const (
    MaxActiveDialTasks     = 16
)

//error定义
var (
	ErrInvalidUnreadByte = errors.New("bufio: invalid use of UnreadByte")
	ErrInvalidUnreadRune = errors.New("bufio: invalid use of UnreadRune")
)

//var定义
var (
	blockIndexBucketName = []byte("blockheaderidx")
)

/*
    error struct定义
*/
type PathError struct {
	Op   string
	Path string
	Err  error
}

func (e *PathError) Error() string { return e.Op + " " + e.Path + ": " + e.Err.Error() }

/*
    struct 1
*/
//struct定义
type Conn struct {
	Name  string
}

//new 定义
func NewConn() *Conn {
	return &Conn{}
}

//函数
func (c *Conn) foo() bool {
    ...
}

/*
    struct 2
*/
//struct定义
type Conn2 struct {
	Name  string
}

//new 定义
func NewConn2() *Conn {
	return &Conn2{}
}

//函数
func (c *Conn2) foo() bool {
    ...
}

```
