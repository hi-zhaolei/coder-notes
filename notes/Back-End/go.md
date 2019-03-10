# Golang

## 基础数据类型

整型 int8 int16 int32 int64 uint8 uint16

浮点型 float32 float64

复数 complex32 complex64

字符串 string utf-8

布尔 bool

类型零值，变量声明后的默认值，值类型默认为 0，布尔类型默认为 false，字符串默认为 ''

## 派生类型

指针 pointer

数组

结构体 struct

channel chan

函数 func

切片 slice

接口 func

Map map

## package

package 是最基本的分发单位，项目中依赖关系的体现

每一个 go 源代码文件必须要有的关键词，必须在，文件代码开头第一行，标识当前文件属于哪个代码包

要生成可执行程序文件，项目必须包含 main 代码包，代码包必须包含 main 函数

同一路径下只能存在一个 package 包，但同一个 package 可以包含多个源文件

## import

导入所依赖的 package

不能导入 package 包文件后但没有引用，编译无法通过

package 会根据代码依次按顺序导入，然后初始化常量和变量，再执行 init 函数；之后继续执行当前文件 main 函数，初始化常量和变量，执行 init 方法。

如果包被多次导入，只会导入一次

```Golang
// import 支持别名
import a 'fmt'

// import 支持包展开引入，去掉命名空间
import . 'fmt'

// import 支持只执行包内 init 方法，用于注册包引擎
import _ 'fmt'
```

## const常量定义

```Golang
const CONSTANT_NAME [constant type] = value
```

常量一般全部大写

## var变量定义

```Golang
var inputCount uint32

var(
    inputCount uint32
    outputCount uint32
    errorCount uint32
)

inputCount = 1024
// 简写
inputCount := 1024
```

## type一般类型声明

```Golang
type varName (type)
```

## 结构体

```Golang
type PersonBase struct{
  Name string
  Age int
}
type Boy struct{
  Person PersonBase
  Sex string
}

var person PersonBase
```

## 接口

```Golang
type apiName interface {

}
```

## 指针

## 条件控制语句

### if语法

```Golang
if i := 10; i < 5 {
  fmt.Print("***********")
}
```

### switch语法

和c/c++不同,go中switch更加现代化,支持string之类的类型.同时需要注意,go中的switch不需要主动写break.

```Golang
var name string
switch name {
  case "Golang":
    fmt.Println("A programming language from Google.")
  case "Rust":
    fmt.Println("A programming language from Mozilla.")
  default:
    fmt.Println("Unknown!")
}
```

### select

### for

go 语言内循环只有 for 一种方式

```Golang
for i:=1;i<10;i++ {

}

for key.value:range=obj {

}
```