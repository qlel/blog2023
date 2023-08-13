+++
author = "qlel"
title = "golang 早期学习笔记"
date = "2023-08-12"
description = "golang 早期学习笔记"
tags = [
"golang"
]
categories = [
"学习"
]
+++

## 基本结构

Go 的源文件以 `.go` 为后缀名存储在计算机中，这些文件名均由小写字母组成，如 `scanner.go` 。如果文件名由多个部分组成，则使用下划线 `_` 对它们进行分隔，如 `scanner_test.go` 。文件名不包含空格或其他特殊字符。

Go 区分大小写。

Go 的语句结尾使用 `;`，一般不用自己加，编译器会自动加 `;`，除非不换行。

```go
// 所属包为 main
package main

// 导入包 fmt
import "fmt"

func main()  {
	fmt.Println("你好 goland!")
}
```

> 注意：大括号 `{}` 的左边 `{` 必须与函数名在一行，否则报错;
> `if` 之类的语句也一样。

### 注释

单行注释：

```go
//单行注释
```

多行注释：

```go
/*
这是
多行注释
*/
```

### package 包

类似其他编程语言的命名空间或类库。

包名 是已存在的，不是随便定义的。

每个 Go 文件都仅属于一个包。

一个包可以由许多以 `.go` 为扩展名的源文件组成，因此文件名和包名一般来说都是不相同的。

必须在源文件中非注释的第一行指明此文件属于哪个包。

如果对一个包进行更改或重新编译，所有引用了这个包的客户端程序都必须全部重新编译。

### import 导入包

使用 `import` 导入一个或多个包：

```go
// 导入一个fmt包
import "fmt"

// or
import (
    "fmt"
)

//导入多个包
import "fmt";import "os"

// or
import ("fmt";"os")

// or 推荐
import (
    "fmt"
    "os"
)
```

给导入的包起别名：

```go
// 给 fmt 包起别名为 aa
import aa "fmt"

import (
    aa "fmt"
)

func main(){
    aa.Println("hello goland")
}
```

一些特殊用法：
给导入包起别名为 `.`，使用时可以省略包名，如：

```go
import . "fmt"

func main(){
    Println("Hello goland")
}
```

给导入包起别名为 `_`，当我们 `import` 一个包的时候，它里面的所有 `init()` 函数都会被执行，但是有时候我们并不真正需要使用这些包，仅仅是希望它里面的 `init()` 函数被执行，这个时候，就可以使用下划线 `import` 了。

```go
import _ "fmt"
```

> 注意：如果你导入了一个包却没有使用它，则会在构建程序时引发错误

### 打印输出

> `Print()`、`Println()` 和 `Printf()` 的区别

区别在于 Print 函数直接输出内容，Printf 函数支持格式化输出字符串，Println 函数会在输出内容的结尾添加一个换行符。

`Print()` 和 `Println()` 有些类似：
打印多个子项时，`Print()` 不会有空格，`Println()` 会有空格；

```go
fmt.Println("go","python","php","javascript") 
// go python php javascript

fmt.Print("go","python","php","javascript") 
// gopythonphpjavascript
```

使用多个 `Println` 或 `Print` 会发现，`Println` 会自动换行，`Print` 不会换行；

```go
fmt.Println("hello")
fmt.Println("world")

// hello
// world

fmt.Print("hello")
fmt.Print("world")

// helloworld
```

`Printf` 是格式化输出

```go
fmp.Printf("a=%d,b=%s\n",100,"hello")

// a=100,b=hello
```

### 格式化占位符

#### 通用占位符

- `%v` ：值的默认格式表示
- `%+v` ：类似 `%v`，但输出结构体时会添加字段名
- `%#v` ：值的 Go 语法表示
- `%T` ：打印值的类型
- `%%` ：百分号

#### 布尔型

- `%t` ：true 或 false

#### 整型

- `%b` ：表示为二进制
- `%c` ：该值对应的 unicode 码值
- `%d` ：表示为十进制
- `%o` ：表示为八进制
- `%x` ：表示为十六进制，使用 a-f
- `%X` ：表示为十六进制，使用 A-F
- `%U` ：表示为 Unicode 格式：U+1234，等价于”U+%04X”
- `%q` ：该值对应的单引号括起来的 go 语法字符字面值，必要时会采用安全的转义表示

```go
n := 65
fmt.Printf("%b\n", n)
fmt.Printf("%c\n", n)
fmt.Printf("%d\n", n)
fmt.Printf("%o\n", n)
fmt.Printf("%x\n", n)
fmt.Printf("%X\n", n)

// 输出结果如下：

1000001
A
65
101
41
41
```

#### 浮点数与复数

- `%b` ：无小数部分、二进制指数的科学计数法，如-123456p-78
- `%e` ：科学计数法，如-1234.456e+78
- `%E` ：科学计数法，如-1234.456E+78
- `%f` ：有小数部分但无指数部分，如 123.456
- `%F` ：等价于 %f
- `%g` ：根据实际情况采用 %e 或 %f 格式（以获得更简洁、准确的输出）
- `%G` ：根据实际情况采用 %E 或 %F 格式（以获得更简洁、准确的输出）

```go
f := 12.34
fmt.Printf("%b\n", f)
fmt.Printf("%e\n", f)
fmt.Printf("%E\n", f)
fmt.Printf("%f\n", f)
fmt.Printf("%g\n", f)
fmt.Printf("%G\n", f)

// 输出结果如下：

6946802425218990p-49
1.234000e+01
1.234000E+01
12.340000
12.34
12.34
```

#### 字符串和 `[]byte`

- `%s` ：直接输出字符串或者 `[]byte`
- `%q` ：该值对应的双引号括起来的 go 语法字符串字面值，必要时会采用安全的转义表示
- `%x` ：每个字节用两字符十六进制数表示（使用 a-f
- `%X` ：每个字节用两字符十六进制数表示（使用 A-F）

```go
s := "小王子"
fmt.Printf("%s\n", s)
fmt.Printf("%q\n", s)
fmt.Printf("%x\n", s)
fmt.Printf("%X\n", s)

// 输出结果如下：

小王子
"小王子"
e5b08fe78e8be5ad90
E5B08FE78E8BE5AD90
```

#### 指针

- `%p` ：表示为十六进制，并加上前导的 `0x`

```go
a := 10
fmt.Printf("%p\n", &a)
fmt.Printf("%#p\n", &a)

// 输出结果如下：

0xc000094000
c000094000
```

#### 宽度标识符

宽度通过一个紧跟在百分号后面的十进制数指定，如果未指定宽度，则表示值时除必需之外不作填充。

精度通过（可选的）宽度后跟点号后跟的十进制数指定。

如果未指定精度，会使用默认精度；如果点号后没有跟数字，表示精度为 0。

如：

- `%f` ：默认宽度，默认精度
- `%9f` ：宽度 9，默认精度
- `%.2f` ：默认宽度，精度 2
- `%9.2f` ：宽度 9，精度 2
- `%9.f` ：宽度 9，精度 0

```go
n := 12.34
fmt.Printf("%f\n", n)
fmt.Printf("%9f\n", n)
fmt.Printf("%.2f\n", n)
fmt.Printf("%9.2f\n", n)
fmt.Printf("%9.f\n", n)

// 输出结果如下：

12.340000
12.340000
12.34
    12.34
       12
```

#### 其他 falg

- `' +'` ：总是输出数值的正负号；对 %q（%+q）会生成全部是 ASCII 字符的输出（通过转义）；
- `' '` ：对数值，正数前加空格而负数前加负号；对字符串采用 %x 或 %X 时（% x 或 % X）会给各打印的字节之间加空格
- `' -'` ：在输出右边填充空白而不是默认的左边（即从默认的右对齐切换为左对齐）；
- `' #'` ：八进制数前加 0（%#o），十六进制数前加 0x（%#x）或 0X（%#X），指针去掉前面的 0x（%#p）对 %q（%#q），对 %U（%#U）会输出空格和单引号括起来的 go 字面值；
- `'0'` ：使用 0 而不是空格填充，对于数值类型会把填充的 0 放在正负号后面；

### 可见性规则

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个**大写字母开头**，如：`Group1`，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 `public`）；

标识符如果以**小写字母开头**，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 `private` ）。

（大写字母可以使用任何 Unicode 编码的字符，比如希腊文，不仅仅是 ASCII 码中的大写字母）。

因此，在导入一个外部包后，能够且只能够访问该包中导出的对象。

假设在包 `pack1` 中我们有一个变量或函数叫做 `Thing`（以 T 开头，所以它能够被导出），那么在当前包中导入 `pack1` 包，`Thing` 就可以像面向对象语言那样使用点标记来调用：`pack1.Thing`（`pack1` 在这里是不可以省略的）。

因此包也可以作为命名空间使用，帮助避免命名冲突（名称冲突）：两个包中的同名变量的区别在于他们的包名，例如 `pack1.Thing` 和 `pack2.Thing`。

### 常量

常量使用关键字 `const` 定义，用于存储不会改变的数据。

存储在常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

常量的定义格式：`const identifier [type] = value`，例如：

```go
const Pi = 3.141592654
```

在 Go 语言中，你可以省略类型说明符 `[type]`，因为编译器可以根据变量的值来推断其类型。

- 显式类型定义： `const b string = "abc"`
- 隐式类型定义： `const b = "abc"`

一个没有指定类型的常量被使用时，会根据其使用环境而推断出它所需要具备的类型。换句话说，未定义类型的常量会在必要时刻根据上下文来获得相关类型。

你可以在其赋值表达式中涉及计算过程，但是所有用于计算的值必须在编译期间就能获得。

```go
const a1 = 1+2;
const a2 = a1/10;

// 错误的用法
// 因为在编译期间自定义函数均属于未知，因此无法用于常量的赋值
const a3 = getNum()

// 正确用法
// 内置函数可以使用
const a4 = len(a2)

func getNum(){
    return 10;
}

// 数字型的常量是没有大小和符号的，并且可以使用任何精度而不会导致溢出
// 衔接换行使用 \
const a5 = 0.693147180559945309417232121458\
            176568075500134360255254120680009

// 浮点常量
const a6 = 1e9
```

常量还可以**并行赋值**：

```go
const beef, two, c = "eat", 2, "veg"
const Monday, Tuesday, Wednesday, Thursday, Friday, Saturday = 1, 2, 3, 4, 5, 6
const (
    Monday, Tuesday, Wednesday = 1, 2, 3
    Thursday, Friday, Saturday = 4, 5, 6
)

// 用作枚举
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

#### 常量计数器 iota

```go
// 第1个常量 a=0,第2个常量 b=a+1,以此类推
const (
    a = iota // 0
    b = iota // 1
    c = iota // 2
)

// 缩写形式
const (
    a = iota // 0
    b        // 1
    c        // 2
)
```

在每遇到一个新的常量块或单个常量声明时， `iota` 都会重置为 `0`（ 简单地讲，每遇到一次 `const` 关键字，`iota` 就重置为 `0` ）。

```go
const a = iota // a=0 

const ( 
  b = iota     //b=0 
  c            //c=1   
)
```

可以使用下划线 `_` 跳过不想要的值：

```go
const (
    a = iota // 0
    b        // 1
    c        // 2
    _
    _
    aa       // 5
)
```

中间有其他常量值的情况：

```go
const (
    a = iota // 0
    b        // 1
    c = 3.14 
    d        // 3.14
    e = iota // 4
    f        // 5
)
```

并行赋值的情况：

```go
const (
	a,b = iota + 1,iota + 2
	c,d // iota + 1,iota + 2
	e,f // iota + 1,iota + 2
)

// 结果
a = 1
b = 2
c = 2
d = 3
e = 3
f = 4
```

### 变量

声明变量的一般形式是使用 `var` 关键字：`var identifier type`。

需要注意的是，Go 和许多编程语言不同，它在声明变量时将变量的类型放在变量的名称之后。Go 为什么要选择这么做呢？

首先，它是为了避免像 C 语言中那样含糊不清的声明形式，例如：`int* a, b;`。在这个例子中，只有 a 是指针而 b 不是。如果你想要这两个变量都是指针，则需要将它们分开书写。

而在 Go 中，则可以很轻松地将它们都声明为指针类型：

```go
var a,b *int

// 其他示例
var c int
var d bool
var e string

// 或者
var (
    c int
    d bool
    e string
)

// 多个同类型的变量声明
var a,b,c int
```

> 注意：变量声明未赋值时必须显示地指定类型，因为对于未赋值的变量编译器不能自动判断类型。

#### 初始化值

当一个变量被声明之后，系统**自动赋予它该类型的零值**：

```go
int 0
float 0.0
bool false
string ''
指针类型 nil
```

记住，所有的内存在 Go 中都是经过初始化的。

#### 命名规则

变量的命名规则遵循驼峰命名法，即首个单词小写，每个新单词的首字母大写，例如：`numShips` 和 `startDate`。

但如果你的全局变量希望能够被外部包所使用，则需要将首个单词的首字母也大写。

#### 作用域

**全局变量：**
如果一个变量在**函数体外声明**，则被认为是**全局变量**，可以在整个包甚至外部包（被导出后）使用，不管你声明在哪个源文件里或在哪个源文件里调用该变量。

**局部变量：**
在**函数体内声明**的变量称之为**局部变量**，它们的作用域只在函数体内，**参数和返回值变量**也是局部变量。

在 `if` 和 `for` 等这些控制结构中声明的变量的作用域只在相应的代码块内。

**一般情况下，局部变量的作用域可以通过代码块（用大括号括起来的部分）判断。**

尽管变量的标识符必须是唯一的，但你可以在某个代码块的内层代码块中使用相同名称的变量，则此时外部的同名变量将会暂时隐藏（结束内部代码块的执行后隐藏的外部同名变量又会出现，而内部同名变量则被释放），你任何的操作都只会影响内部代码块的局部变量。

#### 变量赋值

变量赋值使用等号运算符 `=`：

```go
a = 15
b = false

// 多个变量赋值,并行赋值
a,b,c = 2,"hello",false

// 交换两个变量的值
a,b = b,a
```

变量的声明和赋值(初始化)可以组合使用：

```go
var identifier [type] = value
var a int = 15
var i = 5
var b bool = false
var str string = "Go says hello to the world!"

// 或者，Go编译器根据值来自动判断类型
var a = 15
var b = false
var str = "Go says hello to the world!"

// 或者
var (
    a = 15
    b = false
)
```

> 注意：一般情况下，当变量 `a` 和变量 `b` 之间**类型相同**时，才能进行如 `a = b` 的赋值。

#### 局部变量的声明和赋值

声明和赋值(初始化)局部变量时，可以使用更简短的方式：初始化声明

```go
a := 15
b := "hello"

// 多个变量赋值,并行赋值
a,b,c = 2,"hello",false

// 交换两个变量的值
a,b = b,a
```

局部变量初始化声明后，相同的变量就不能再一次初始化声明了：

```go
a := 15

// 错误
a := 20

// 要改变初始化值，直接使用 = 
a = 20

// 在初始化声明局部变量 b 之前使用 b
b = 333 // undefined:b
```

如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误:

```go
func main(){
    a := "hello"
    fmt.Println("test")
}
// error: a declared and not used
```

> 注意：全局变量是允许声明但不使用

#### 值类型和引用类型

**值类型：**
当使用等号 `=` 将一个变量的值赋值给另一个变量时，如：`j = i`，实际上是在内存中将 `i` 的**值**进行了**拷贝**。

将某一个内存地址的值拷贝到另一个内存地址，指向的不是同一个内存地址。

当 `j` 的值改变时，`i` 的值不会改变；反过来也是如此。

**引用类型：**
要将 `j` 指向 `i` 的内存地址，需要在 `i` 前加取地址符 `&`，即 `j = &i`

此时 `j` 的值如果改变，`i` 的值也会改变；反过来也如此。

#### 空白标识符 `_`

空白标识符 `_` 也被用于抛弃值，如值 `5` 在：`_,b = 5,7` 中被抛弃。

`_` 实际上是一个只写变量，你不能得到它的值。

这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值。

## 基本类型

### 布尔 bool

```go
var a bool = true
var b bool = false
```

布尔型的值只可以是常量 `true` 或者 `false`。

### 整型

与操作系统架构无关的类型都有固定的大小，并在类型的名称中就可以看出来：
**整型：**

- `int8` :[-128, 127]
- `int16` :[-32768, 32767]
- `int32` :[-2147483648, 2147483647]
- `int64` :[-9223372036854775808, 9223372036854775807]

**无符号整型：**

- `uint8` :[0, 255]
- `uint16` :[0, 65535]
- `uint32` :[0, 4294967295]
- `uint64` :[0, 18446744073709551615]

#### 基于架构的类型

这些类型的长度都是根据运行程序所在的**操作系统类型**所决定的:

- `int` ：
  在 32 位操作系统上，使用 32 位（4 个字节）;
  在 64 位操作系统上，使用 64 位（8 个字节）。
- `uint` ：
  在 32 位操作系统上，使用 32 位（4 个字节）;
  在 64 位操作系统上，使用 64 位（8 个字节）。
- `intptr` ： 的长度被设定为足够存放一个指针即可。

### 浮点型

Go 有两种浮点型：

- `float32` :[1.401298464324817e-45, 3.4028234663852886e+38]
- `float64` :[5e-324, 1.7976931348623157e+308]

```go
package main

import "fmt"
import "math"

func main()  {
	fmt.Println("float32最大值",math.MaxFloat32)
	fmt.Println("float32最小值",math.SmallestNonzeroFloat32)
	fmt.Println("float64最大值",math.MaxFloat64)
	fmt.Println("float64最小值",math.SmallestNonzeroFloat64)
}

// 结果
float32最大值 3.4028234663852886e+38
float32最小值 1.401298464324817e-45
float64最大值 1.7976931348623157e+308
float64最小值 5e-324

// 1e3 = 1*10^3 = 1000
```

`float32` 精确到小数点后 `7` 位，`float64` 精确到小数点后 `15` 位。

由于精确度的缘故，你在使用 `==` 或者 `!=` 来比较浮点数时应当非常小心。

### 复数

- `complex64` ：32 位实数和虚数
- `complex128` ：64 位实数和虚数

复数使用 `re+imI` 来表示，其中 `re` 代表实数部分，`im` 代表虚数部分，`I` 代表根号负 1，即： $\sqrt{-1}\quad $ 。

可以自己组装一个复数：`re+imI`，如：`5 + 10i`
也可以通过内建函数 `complex(re, im)` 使两个数组成复数，如：`complex(3,9)`

单独获取**实数部分**使用内建函数 `real(re)`。
单独获取**虚数部分**使用内建函数 `imag(im)`。

```go
package main

import "fmt"

var a1 complex64 = 5 + 10i
var a2 complex64 = complex(3,9)

func main()  {
	fmt.Println(a1)
	fmt.Println(a2)
	
	// 获取实数
	fmt.Println(real(a1))
	// 获取虚数
	fmt.Println(imag(a1))
}

// 结果
(5+10i)
(3+9i)
5
10
```

### 字符类型

严格来说，这并不是 Go 语言的一个类型，字符只是**整型**的特殊用例。
字符使用**单引号**括起来。

- `byte` ：是 `uint8` 的别名，表示只占用 1 个字节的传统 ASCII 编码的字符
- `rune` ：是 `int32` 的别名，表示一个 Unicode(UTF-8)的字符

```go
package main

import "fmt"

var a1 byte = 65
var a2 byte = 'A'

var b1 rune = '我'

func main() {
	fmt.Printf("%v %c %U %T\n",a1,a1,a1,a1)
	fmt.Printf("%v %c %U %T\n",a2,a2,a2,a2)
	fmt.Printf("%v %c %U %T\n",b1,b1,b1,b1)
}

// 结果
65 A U+0041 uint8
65 A U+0041 uint8
25105 我 U+6211 int32
```

### 字符串

```go
var str string = "hello"
```

字符串是 UTF-8 字符的一个序列。

UTF-8 是被广泛使用的编码格式，是文本文件的标准编码，其它包括 XML 和 JSON 在内，也都使用该编码。

由于该编码对占用字节长度的不定性，Go 中的字符串也可能根据需要占用 1 至 4 个字节（示例见第 4.6 节），这与其它语言如 C++、Java 或者 Python 不同（Java 始终使用 2 个字节）。Go 这样做的好处是不仅减少了内存和硬盘空间占用，同时也不用像其它语言那样需要对使用 UTF-8 字符集的文本进行编码和解码。

字符串是一种值类型，且值不可变，即创建某个文本后你无法再次修改这个文本的内容；更深入地讲，字符串是**字节的定长数组**。

**解释字符串：**
使用**双引号** `""` 括起来，其中的相关的转义字符将被替换，这些转义字符包括：

- `\n`：换行符
- `\r`：回车符
- `\t`：tab 键
- `\u` 或 `\U`：Unicode 字符
- `\\`：反斜杠自身

**非解释字符串：**
该类字符串使用**反引号**<kbd>`</kbd>括起来，支持换行(多行)，如：

```go
`
这是多行字符串
不解析换行符\n，\n原样输出
`
```

通过内置函数 `len()` 来获取字符串所占的字节长度，例如：`len(str)`

字符串的内容（纯字节）可以通过**标准索引法**来获取，在中括号 `[]` 内写入索引，索引从 `0` 开始计数，索引不能是负数。

需要注意的是，这种转换方案只对纯 ASCII 码的字符串有效。

如：

- 字符串 str 的第 `1` 个字节：`str[0]`
- 第 `i` 个字节：`str[i - 1]`
- 最后 `1` 个字节：`str[len(str)-1]`

> 注意：获取字符串中某个字节的地址的行为是非法的，例如：`&str[i]`

**字符串拼接：`+`**

```go
s := "hel" + "lo,"
s += "world!"
fmt.Println(s) //输出 “hello, world!”
```

在循环中使用加号 `+` 拼接字符串并不是最高效的做法，更好的办法是使用函数 `strings.Join()` 或者字节缓冲 `bytes.Buffer`。

### 指针类型

程序在内存中存储它的值，每个内存块（或字）有一个地址，通常用十六进制数表示，如：`0x6b0820` 或 `0xf84001d7f0`。

Go 语言的取地址符是 `&`，放到一个变量前使用就会返回相应变量的内存地址。

```go
package main

import "fmt"

func main() {
	a1 := 5
	fmt.Printf("a1的值:%d 内存地址是: %p\n", a1, &a1)
}

/*
a1的值:5 内存地址是: 0xc000010080
*/
```

指针声明格式: 在类型前面加上 `*`

```go
var 变量名 *类型名
```

在指针变量前面加 `*` 获取值.

```go
package main

import "fmt"

func main() {
	a1 := 5
	var p1 *int
	p1 = &a1
	fmt.Printf("a1:%d &a1: %p\n", a1, &a1)
	fmt.Printf("p1:%v *p1: %v\n", p1, *p1)
}

/*
a1:5 &a1: 0xc000010080
p1:0xc000010080 *p1: 5
*/
```

> 注意: 不能得获取常量的地址

### 类型别名

```go
type TZ int

type (
    aaa int
    bbb string
)
```

`TZ` 就是 `int` 类型的新名称

### 类型转换

一般都是范围小的转换为范围大的。

#### 整型与整型

`int8()`、`int16()`、`int32()`、`int64()`、`int()`
`uint8()`、`uint16()`、`uint32()`、`uint64()`、`uint()`

```go
var a1 int8 = 20
var a2 int16 = 40

func main(){
    fmt.Println(int16(a1)+a2)
}

// 60
```

#### 浮点型与浮点型

`float32()`、`float64()`

#### 整型与浮点型

```go
var a1 float64 = 20.999
var a2 int = 40

func main() {
    // 浮点型 -> 整型
    fmt.Printf("%v\n", int(a1) + a2)
    
    // 整型 -> 浮点型
	fmt.Printf("%v\n", a1 + float64(a2))
}

// 结果
60
60.998999999999995
```

#### 数值型转换为 string 型

==第一种方法：==
`fmt.Sprintf()`：把格式化字符串输出到指定的字符串中，可以用一个变量来接受，然后在打印

```go
var a1 float64 = 20.999
var a2 int = 40
var a3 byte = 'A'

func main() {
    // 转换 浮点型为字符串
	str1 := fmt.Sprintf("%f",a1)
	fmt.Printf("值：%v 类型：%T\n", str1, str1)

    // 转换 整型为字符串
	str2 := fmt.Sprintf("%d",a2)
	fmt.Printf("值：%v 类型：%T\n", str2, str2)

    // 转换 字符型为字符串
	str3 := fmt.Sprintf("%c",a3)
	fmt.Printf("值：%v 类型：%T\n", str3, str3)
}

// 结果
值：20.999000 类型：string
值：40 类型：string
值：A 类型：string
```

==第二种方法：==
使用 `strconv` 包里面的方法进行转换。

**将整型转换为 string 类型：**
`strconv.FormatInt(i int64, base int) string`
`strconv.FormatUint(i int64, base int) string`
`strconv.Itoa(i int) string` 等效于 `strconv.FormatInt(int64(i), 10)`
参数：

- `i`：要转换的整型，必须是 `int64` 类型
- `base`：进制

最后返回 string 类型

```go
import (
	"fmt"
	"strconv"
)

func main() {
	var a1 int = 5
	str1 := strconv.FormatInt(int64(a1),10)
	fmt.Printf("值：%v 类型：%T\n", str1, str1)
}
```

**将浮点型转换为 string 类型：**
`strconv.FormatFloat(f float64, fmt byte, prec, bitSize int) string`
参数：

- `f`：要转换的浮点数，必须是 `float64` 类型的
- `fmt`：格式化的类型
  - `'f'`：-ddd.dddd，无指数
  - `'b'`：-ddddp±ddd，指数为二进制
  - `'e'`：-d.dddde±dd，指数为十进制
  - `'E'`：-d.ddddE±dd，指数为十进制
  - `'g'`：指数很大时用 `e` 格式，否则 `f` 格式
  - `'G'`：指数很大时用 `E` 格式，否则 `f` 格式
- `prec`：保留小数点位数，`-1` 表示不对小数点格式化
- `bitSize`：格式化的类型，`64` 或 `32`

```go
import (
	"fmt"
	"strconv"
)

func main() {
	var a1 float64 = 5.999
	str1 := strconv.FormatFloat(a1,'f',-1,32)
	fmt.Printf("值：%v 类型：%T\n", str1, str1)
}

// 结果
值：5.999 类型：string
```

#### string 转换为数值型

**string 转换为整型：**
`strconv.ParseInt(s string, base int, bitSize int) (i int64, err error)`
`strconv.ParseUint(s string, base int, bitSize int) (i int64, err error)`
`strconv.Atoi(s string) (int, error)` 等效与 `strconv.ParseInt(s, 10, 0)`
参数：

- `s`：要转换的字符串
- `base`：进制
- `bitSize`：指定结果必须能无溢出赋值的整数类型，`0`、`8`、`16`、`32`、`64` 分别代表 `int`、`int8`、`int16`、`int32`、`int64`

> 注意：此函数返回 2 个值：转换后的整型和错误信息

```go
import (
	"fmt"
	"strconv"
)

func main() {
	var a1 string = "233"
	num1, err := strconv.ParseInt(a1,10,32)
	fmt.Printf("值：%v 类型：%T 错误：%v\n", num1, num1, err)
}

// 结果
值：233 类型：int64 错误：<nil>
```

**string 转换为浮点型：**
`strconv.ParseFloat(s string, bitSize int) (float64, error)`
参数：

- `s`：要转换的字符串
- `bitSize`：指定了期望的接收类型，32 是 float32，64 是 float64；

> 注意：此函数返回 2 个值：转换后的整型和错误信息

```go
import (
	"fmt"
	"strconv"
)

func main() {
	var a1 string = "233.999"
	num1, err := strconv.ParseFloat(a1,32)
	fmt.Printf("值：%v 类型：%T 错误：%v\n", num1, num1, err)
}

// 结果 ParseFloat(a1,32)
值：233.99899291992188 类型：float64 错误：<nil>

// ParseFloat(a1,64)
值：233.999 类型：float64 错误：<nil>
```

### 类型断言

`x.(T)` 断言 `x` 的动态类型是否是 `T`，其中 `x` 必须是接口类型. 如果不是 `T` 类型, 则断言失败会 panic; 断言成功则返回 `x`.

例如:

```go
var x interface{} = 7 
i := x.(int) // 断言 x 为 int 类型
```

- 如果 T 是**具体类型**
  断言 `x` 的动态类型是否是具体类型 `T`. 如果断言成功, 返回 `x`; 断言失败会触发 panic
- 如果 T 是**接口类型**
  断言 `x` 的动态类型是否满足接口类型 `T`. 如果断言成功，x 的动态值不会被提取，返回值是一个类型为 T 的接口值; 断言失败会触发 panic

如果想知道类型断言是否失败，而不是失败时触发 panic，可以使用返回两个值的版本：

```go
var v, ok = x.(T)
```

还可以配合 switch 使用:

```go
switch x.(type){
case nil: // 如果x是nil
case int, uint: 
case bool:
case string;
default: //没有匹配上
}
//case的顺序是有意义的，因为可能同时满足多个接口，不可以用fallthrough, default的位置无所谓。
```

## 运算符

**不同类型不能进行运算。**

### 算术运算符

| 运算符 | 术语               | 示例          |
| ------ | ------------------ | ------------- |
| `+`    | 加                 | 10+5=15       |
| `-`    | 减                 | 10-5=5        |
| `*`    | 乘                 | 10*5=50       |
| `/`    | 除                 | 10/5=2        |
| `%`    | 取模(取余)         | 10%3=1        |
| `++`   | 后自增，没有前自增 | a=10;a++;a=11 |
| `--`   | 后自减，没有前自减 | a=10;a--;a=9  |

> 注意：在 Go 中，`++` 和 `--` 是作为语句而不是表达式，只能单独使用。

### 关系运算符

| 运算符 | 术语     | 示例   | 结果  |
| ------ | -------- | ------ | ----- |
| `==`   | 相等于   | 4 == 3 | false |
| `!=`   | 不等于   | 4 != 3 | true  |
| `<`    | 小于     | 4 < 3  | false |
| `>`    | 大于     | 4 > 3  | true  |
| `<=`   | 小于等于 | 4 <= 3 | false |
| `>=`   | 大于等于 | 4 >= 3 | true  |

### 逻辑运算符

| 运算符 | 术语 | 示例 | 结果                                           |
| ------ | ---- | ---- | ---------------------------------------------- |
| `&&`   | 与   | a&&b | 如果 a 和 b 都为真，则结果为真，否则为假       |
| `      |      | `    | 或                                             |
| `!`    | 非   | !a   | 如果 a 为假，则!a 为真；如果 a 为真，则!a 为假 |

### 位运算符

| 运算符 | 术语   | 说明                                                                     | 示例                               |
| ------ | ------ | ------------------------------------------------------------------------ | ---------------------------------- |
| `&`    | 按位与 | 参与运算的两数各自对应的二进制相与                                       | 5&3 == 101 & 011 = 001 = 1         |
| `      | `      | 按位或                                                                   | 参与运算的两数各自对应的二进制相或 |
| `^`    | 异或   | 参与运算的两数各自对应的二进制异或，两数对应二进制位不同时，结果为 1(真) | 5^3 == 101 ^ 011 = 110 = 6         |
| `>>`   | 左移   | 左移 n 位就是乘以 2 的 n 次方。左边丢弃，右边补 0                        | 5<<2 == 10100 == 5*2^2 = 20        |
| `<<`   | 右移   | 右移 n 位就是除以 2 的 n 次方。左边补位，右边丢弃                        | 5>>2 == 001 == 5/2^2 = 1           |

### 赋值运算符

| 运算符 | 说明             | 示例                                  |
| ------ | ---------------- | ------------------------------------- |
| `=`    | 简单赋值         | C = A + B 将 A + B 表达式结果赋值给 C |
| `+=`   | 相加后再赋值     | C += A 等价于 C = C + A               |
| `-=`   | 相减后再赋值     | C -= A 等价于 C = C - A               |
| `*=`   | 相乘后再赋值     | C *= A 等价于 C = C * A               |
| `/=`   | 相除后再赋值     | C /= A 等价于 C = C / A               |
| `%=`   | 求余后再赋值     | C %= A 等价于 C = C % A               |
| `<<=`  | 左移后再赋值     | C <<= A 等价于 C = C << A             |
| `>>=`  | 右移后再赋值     | C >>= A 等价于 C = C >> A             |
| `&=`   | 按位与后再赋值   | C &= A 等价于 C = C & A               |
| `^=`   | 按位异或后再赋值 | C ^= A 等价于 C = C ^ A               |
| `      | =`               | 按位或后再赋值                        |

### 其他运算符

| 运算符 | 术语         | 示例 | 说明                      |
| ------ | ------------ | ---- | ------------------------- |
| `&`    | 取地址运算符 | &a   | 变量 a 的地址             |
| `*`    | 取值运算符   | *a   | 指针变量 a 所指向内存的值 |

### 运算符的优先级

在 Go 语言中，一元运算符拥有最高的优先级，二元运算符的运算方向均是从左至右。

由于 `++` 和 `--` 运算符形成语句，而不是表达式，因此它们不属于运算符层次结构。

由上至下代表优先级由高到低：

| 优先级 | 运算符                               |
| ------ | ------------------------------------ |
| 5      | `*`  `/`  `%`  `<<`  `>>`  `&`  `&^` |
| 4      | `+`  `-`  `                          |
| 3      | `==`  `!=`  `<`  `<=`  `>`  `>=`     |
| 2      | `&&`                                 |
| 1      | `                                    |

可以通过使用括号来临时提升某个表达式的整体运算优先级。

## 控制结构

### if-else 结构

基本语法：

```go
if 条件1 {
    // ...
} else if 条件2 {
    // ...
} else {
    // ...
}
```

和前面函数一样，左大括号 `{` 也必须与条件语句在同一行。`else` 也要和 `}` 同一行.

if-else 的分支数量是没有限制的，尽可能把先满足的条件放在前面。

if 可以包含一个初始化语句。(如:给一个变量赋值)

```go
if val := 10; val > max {}
```

注意：使用 `:=` 方式赋值，表示只在 if 作用域访问内变量才有效。

### switch 结构

基本语法：

```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

变量 var1 可以是任何类型，而 val1 和 val2 则可以是同类型的任意值。类型不被局限于常量或整数，但必须是相同的类型；或者最终结果为相同类型的表达式。前花括号 `{` 必须和 switch 关键字在同一行。

可以同时测试多个可能符合条件的值，使用逗号分割它们，例如：`case val1, val2, val3`。

每执行完一个 `case` 块会自动跳出整个 `switch` 语句 , 不需要特别使用 `break` 语句来表示结束。

如果在执行完每个分支的代码后，还希望继续执行后续分支的代码，可以使用 `fallthrough` 关键字来达到目的。

示例 1:

```go
package main

import "fmt"

func main() {
	num1 := 96
	switch num1 {
	case 99:
		fmt.Println("this is 99")
	case 97, 96, 95:
		fmt.Println("this is 97 or 96 or 95")
		fallthrough
	case 123:
		fmt.Println("this is 123")
	default:
		fmt.Println("this is default")
	}
}

/* 结果
this is 97 or 96 or 95
this is 123
*/
```

示例 2: 可以使用一个初始化语句

```go
package main

import "fmt"

func main() {
	switch a, b := 9, 3; {
	case a > b:
		fmt.Println("this is a>b")
	case a < b:
		fmt.Println("this is a<b")
	case a == b:
		fmt.Println("this is a==b")
	}
}

/*
this is a>b
*/
```

### for 与 range 结构

`for` 基本语法:

```go
for 初始化语句; 条件语句; 修饰语句 {}
```

如果只有条件语句, 就相当于其它语言的 `while`:

```go
for 条件语句 {}
```

如果什么都没有, 直接写 `for` 就会无线循环:

```go
for {}
```

示例 1:

```go
package main

import "fmt"

func main() {
	for i := 0; i < 5; i++ {
		fmt.Printf("计数器: %d\n", i)
	}
}

/*
计数器: 0
计数器: 1
计数器: 2
计数器: 3
计数器: 4
*/
```

示例 2:

```go
package main

import "fmt"

func main() {
	for i, j := 0, 10; i < 5; i, j = i+1, j+10 {
		fmt.Printf("计数器: i: %d j: %d\n", i, j)
	}
}

/*
计数器: i: 0 j: 10
计数器: i: 1 j: 20
计数器: i: 2 j: 30
计数器: i: 3 j: 40
计数器: i: 4 j: 50
*/
```

示例 3: 相当于其它语言的 `while`

```go
package main

import "fmt"

func main() {
	i := 5
	for i > 0 {
		i = i - 1
		fmt.Printf("计数器: %d\n", i)
	}
}

/*
计数器: 4
计数器: 3
计数器: 2
计数器: 1
计数器: 0
*/
```

`range` 关键字用于 `for` 循环中迭代数组(`array`)、切片(`slice`)、通道(`channel`)或集合(`map`)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 `key-value` 对。

一般形式为: `for ix, val := range coll { }`

> 要注意的是，`val` 始终为集合中对应索引的值拷贝，因此它一般只具有只读性质，对它所做的任何修改都不会影响到集合中原有的值.

示例 1:

```go
package main

import "fmt"

func main() {
	str1 := "Hello!你好"

	for ix, val := range str1 {
		fmt.Printf("索引:%v 默认格式值: %v unicode值: %c\n", ix, val, val)
	}
}

/*
索引:0 默认格式值: 72 unicode值: H
索引:1 默认格式值: 101 unicode值: e
索引:2 默认格式值: 108 unicode值: l
索引:3 默认格式值: 108 unicode值: l
索引:4 默认格式值: 111 unicode值: o
索引:5 默认格式值: 33 unicode值: !
索引:6 默认格式值: 20320 unicode值: 你
索引:9 默认格式值: 22909 unicode值: 好
*/
```

打印中文会跨 3 个索引.

### break 与 continue

`break` 结束当前循环块

`continue` 结束当前次循环, 继续下一次循环, 只能用于 `for` 语句中

示例 1:

```go
package main

import "fmt"

func main() {
	for i := 0; i < 3; i++ {
		for j := 0; j < 10; j++ {
			if j > 5 {
				break
			}
			fmt.Print(j)
		}
		fmt.Print("\n")
	}
}

/*
012345
012345
012345
*/
```

上例中 `break` 只结束了里面的 `for` 语句, 外层的 `for` 还是循环了 3 次.

示例 2:

```go
package main

import "fmt"

func main() {
	for i := 0; i < 5; i++ {
		if i == 3 {
			continue
		}
		fmt.Print(i)
	}
}

/*
0124
*/
```

上例中 `continue` 只结束了第 3 次循环, 还会继续循环下去.

### goto 与 标签

`goto` 配合标签使用, 当程序执行到 `goto` 语句时会跳转到 `goto` 指定的标签位置, 标签位置可以在 `goto` 语句之前或之后.

> 注意标签和 `goto` 语句之间不能出现定义新变量的语句，否则会导致编译失败。

标签的定义使用字符串加冒号: `MYLABEL1:`

> 标签的名称是大小写敏感的，为了提升可读性，一般建议使用全部大写字母

示例:

```go
package main

import "fmt"

func main() {
	fmt.Println("111")
    goto LABEL1
	fmt.Println("222")
	fmt.Println("333")
LABEL1:
	fmt.Println("444")
}

/*
111
444
*/
```

## 数组

数组是具有相同**唯一类型**的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整型、字符串或者自定义类型。

数组的长度是不可变的, 切片 Slice 的长度可变.

把一个大数组传递给函数会消耗很多内存。有两种方法可以避免这种现象：

- 传递数组的指针
- 使用切片

数组元素可以通过索引（位置）来读取（或者修改），索引从 0 开始，第一个元素索引为 0，第二个索引为 1，以此类推。

索引长度为 `len(数组变量名)-1`

==声明数组:==

```go
var 变量名 [长度]类型

// 如:
var arr1 [5]int

// [0 0 0 0 0]
```

当声明的数组是整型/字符型/浮点型时, 所有的元素都会被自动初始化为默认值 `0`。
当声明的数组是字符串型时, 所有的元素都会被自动初始化为默认值空。
当声明的数组是布尔型时, 所有的元素都会被自动初始化为默认值 `false`。
当声明的数组是复数型时, 所有的元素都会被自动初始化为默认值 `(0+0i)`。

==初始化数组:==

```go
package main

import "fmt"

func main() {
	// 正常初始化, 可以少元素, 不能超过数组长度
	var arr1 = [3]int{33, 76}

	// 指定索引初始化
	var arr2 = [5]string{2: "hello", 4: "你好"}

	// 自动计算数组元素, 相当于切片
	arr3 := []int{2: 123, 678, 999}
	arr4 := [...]int{3: 123, 678, 999}
	fmt.Println(arr1)
	fmt.Println(arr2, len(arr2))
	fmt.Println(arr3, len(arr3))
	fmt.Println(arr4, len(arr4))
}

/*
[33 76 0]
[  hello  你好] 5
[0 0 123 678 999] 5
[0 0 0 123 678 999] 6
*/
```

==访问与修改数组元素:==

```go
var arr1 [5]int

// 访问数组第一个元素
arr1[0]

// 访问数组最后一个元素
arr1[len(arr1)-1]

// 修改数组第3个元素的值
arr1[2]=233
```

==遍历数组:==

```go
package main

import "fmt"

func main() {
	var arr1 [3]int

	// 直接打印数组
	fmt.Println(arr1)

	// 直接遍历数组
	for i := 0; i < len(arr1); i++ {
		fmt.Printf("直接遍历 索引: %d 值: %d\n", i, arr1[i])
	}

	// 使用 range 遍历数组
	for i, v := range arr1 {
		fmt.Printf("range遍历 索引: %d 值: %d\n", i, v)
	}
}


/*
[0 0 0]
直接遍历 索引: 0 值: 0
直接遍历 索引: 1 值: 0
直接遍历 索引: 2 值: 0
range遍历 索引: 0 值: 0
range遍历 索引: 1 值: 0
range遍历 索引: 2 值: 0
*/
```

==多维数组:==
数组通常是一维的，但是可以用来组装成多维数组，例如：`[3][5]int`，`[2][2][2]float64`

数组长度由最外层数组决定.

```go
package main

import "fmt"

func main() {
	var arr1 [3][5]int
	var arr2 [5][3]int
	var arr3 [2][3][1]int
	fmt.Println(arr1, len(arr1))
	fmt.Println(arr2, len(arr2))
	fmt.Println(arr3, len(arr3))
}

/*
[[0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0]] 3
[[0 0 0] [0 0 0] [0 0 0] [0 0 0] [0 0 0]] 5
[[[0] [0] [0]] [[0] [0] [0]]] 2
*/
```

## 切片 Slice

切片（slice）是对数组一个连续片段的引用（该数组我们称之为相关数组，通常是匿名的），所以切片是一个引用类型(类似 python 的列表 list 类型).

切片是一个长度可变的数组。

计算切片的长度也可以使用 `len()`
切片提供了计算容量的方法 `cap()` 可以测量切片最长可以达到多少。

==声明切片:==
`var 变量名 []类型`
不需要说明长度.

也可以使用内置函数 `make()`:
`var 变量名 []类型 = make([]类型, 长度)`

一个切片在未初始化之前默认为 `nil` 表示空，长度为 0。

==初始化切片:==

```go
// 直接初始化
var s1 []int{22, 67, 33}

// 初始化切片s1,是数组arr1的引用
s1 := arr1[:] 

// 将arr中从索引 startIndex 到 endIndex-1 下的元素创建为一个新的切片
s := arr[startIndex:endIndex] 

// 取arr数组从索引 startIndex 开始到最后索引作为切片s
s := arr[startIndex:] 

// 取arr数组从索引 0 开始到 endIndex 结束作为切片s
s := arr[:endIndex] 

// 通过切片s初始化切片s1
s1 := s[startIndex:endIndex]
```

示例:

```go
package main

import "fmt"

func main() {
	// 数组
	var arr1 = [5]int{33, 55, 12, 66, 99}

	// 切片
	sl1 := arr1[2:]
	fmt.Println(sl1, len(sl1), cap(sl1))
}

/*
[12 66 99] 3 3
*/
```

==append() 和 copy() 函数==
如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

`copy(目的切片, 被拷贝的切片)`
将一个切片的意元素拷贝到另一个切片中, 并覆盖目的切片中原来的元素, 返回被拷贝元素的个数.

必须是**同一类型**切片.

```go
package main

import "fmt"

func main() {
	s1 := []int{0, 0}
	s2 := []int{1, 2, 3, 4, 5}
	s3 := []int{55, 66, 77}

	// 从 s2 拷贝到 s1
	n1 := copy(s1, s2)

	// 从 s3 拷贝到 s2
	n2 := copy(s2, s3)

	fmt.Println("s1: ", s1)
	fmt.Println("s2: ", s2)
	fmt.Println("s3: ", s3)
	fmt.Println(n1)
	fmt.Println(n2)
}

/*
s1:  [1 2]
s2:  [55 66 77 4 5]
s3:  [55 66 77]
2
3
*/
```

字符类型也能拷贝:

```go
package main

import "fmt"

func main() {
	s1 := []byte{'a', 'b'}

	// 将字符串当作 []byte 类型拷贝到s1
	n1 := copy(s1, "hello")

	fmt.Printf("s1: %c %d %T", s1, n1, s1)
}

/*
s1: [h e] 2 []uint8
*/
```

`append(目的切片, 同类型元素或者切片...)`
用来追加元素到数组、slice 中,返回修改后的数组、slice
原切片 slice 不变

```go
package main

import "fmt"

func main() {
	s0 := []int{0, 0}

	// 添加单个元素
	s1 := append(s0, 2)

	// 添加多个元素
	s2 := append(s1, 3, 5, 7)

	// 添加切片
	s3 := append(s2, s0...)
	s4 := append(s3[3:6], s3[2:]...)

	// 添加不同类型元素到接口类型
	var t1 []interface{}
	t1 = append(t1, 42, 3.1415, "hello")

	// 添加字符串型到 []byte
	var b1 []byte
	b1 = append(b1, "world"...)

	fmt.Printf(" s0: %v\n s1: %v\n s2: %v\n", s0, s1, s2)
	fmt.Printf(" s3: %v\n s4: %v\n", s3, s4)
	fmt.Printf(" t1: %v\n b1: %c\n", t1, b1)
}

/*
 s0: [0 0]
 s1: [0 0 2]
 s2: [0 0 2 3 5 7]
 s3: [0 0 2 3 5 7 0 0]
 s4: [3 5 7 2 3 5 7 0 0]
 t1: [42 3.1415 hello]
 b1: [w o r l d]
*/
```

## Map

Map 是一种特殊的数据结构, 由键值对组成的无序集合, 类似 python 中的字典, 所以这个结构也称为关联数组或字典。

Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

map 默认是无序的，不管是按照 key 还是按照 value 默认都不排序.

### 声明和初始化

map 是引用类型，可以使用如下声明：

```go
var map1 map[keytype]valuetype

// 如
var map1 map[string]int

// 使用make创建map
ages := make(map[string]int)
```

在声明的时候不需要知道 map 的长度，map 是可以动态增长的。

未初始化的 map 的值是 `nil`。

初始化:

```go
ages := map[string]int{
    "alice":   31,
    "charlie": 34,
}

// 相当于
ages := make(map[string]int)
ages["alice"] = 31
ages["charlie"] = 34
```

### 相关操作

判断元素是否存在, 可以使用 `val1, isPresent = map1[key1]`

如果元素存在, `val1` 返回元素值, `isPresent` 返回布尔值 `true`

删除元素使用 `delete(map1, key1)`

如果 key1 不存在，该操作不会产生错误。

示例:

```go
package main

import "fmt"

func main() {
	m1 := map[string]string{"aa": "", "bb": "hello"}
	val1, isP1 := m1["aa"]
	val2, isP2 := m1["cc"]
	fmt.Printf("是否存在: %v 值: %v\n", isP1, val1)
	fmt.Printf("是否存在: %v 值: %v\n", isP2, val2)
	delete(m1, "aa")
	val3, isP3 := m1["aa"]
	fmt.Printf("是否存在: %v 值: %v m1字典: %v\n", isP3, val3, m1)
}

/*
是否存在: true 值:
是否存在: false 值:
是否存在: false 值:  m1字典: map[bb:hello]
*/
```

也可以使用 `for-range` 遍历 Map:

```go
package main

import "fmt"

func main() {
	m1 := map[string]string{"aa": "one", "bb": "hello"}
	for key, value := range m1 {
		fmt.Printf("键: %v 值: %v\n", key, value)
	}
}

/*
键: aa 值: one
键: bb 值: hello
*/
```

## 结构体

结构体是复合数据类型, 当需要定义某个事物，它由一系列属性组成，每个属性都有自己的类型和值的时候，就应该使用结构体，它把数据聚集在一起。

Go 语言中数组可以存储同一类型的数据，但在结构体中我们可以为不同项定义不同的数据类型。

结构体也是值类型，因此可以通过 `new` 函数来创建。

组成结构体类型的那些数据称为 **字段**（fields）。每个字段都有一个类型和一个名字；在一个结构体中，字段名字必须是唯一的。

### 定义结构体

```go
type 结构体名 struct {
    字段1 类型
    字段2 类型
    ...
}

// 如
type st1 struct {
	i1, i2   int
	f1   float32
    str1 string
}
```

### 实例化结构体

可以声明一个变量, 类型为定义的结构体: `var 变量 结构体`, 此时变量被称做类型的一个实例（instance）或对象（object）。

实例化后的变量结构体内所有字段会自动初始化为各自类型的零值.

也可以使用 `new` 函数给一个新的结构体变量分配内存，它返回指向已分配内存的指针：`var t *T = new(T)`

```go
package main

import "fmt"

type st1 struct {
	i1 int
	f1 float32
	b1 bool
}

func main() {
	var t1 st1
	t2 := new(st1)
	fmt.Println(t1)
	fmt.Println(t2)
}

/*
{0 0 false}
&{0 0 false}
*/
```

可以使用点 `.` 访问结构体实例的字段值, 也可以在实例化时直接修改字段值:

```go
package main

import "fmt"

type st1 struct {
	i1 int
	f1 float32
	b1 bool
}

func main() {
	var t1 st1 = st1{f1: 3.14}
	t1.i1 = 666
	t2 := new(st1)
	fmt.Println(t1)
	fmt.Println(t2)
}

/*
{666 3.14 false}
&{0 0 false}
*/
```

如果不使用字段名修改值, 则需要按照结构体内的顺序赋值:
`var t1 st1 = st1{666, 3.14, true}`

### 带标签的结构体

结构体中的字段除了有名字和类型外，还可以有一个可选的标签（tag）：它是一个附属于字段的字符串，可以是文档或其他的重要标记。标签的内容不可以在一般的编程中使用，只有包 `reflect` 能获取它。

```go
package main

import (
	"fmt"
	"reflect"
)

type st1 struct {
	i1 int     "i1的标签"
	f1 float32 "f1 浮点类型"
	b1 bool    "b1 布尔类型"
}

func main() {
	var t1 st1 = st1{f1: 3.14}
	for i := 0; i < 3; i++ {
		refTag(t1, i)
	}
}

func refTag(tt st1, ix int) {
	ttType := reflect.TypeOf(tt)
	ixField := ttType.Field(ix)
	fmt.Println(ixField.Tag)
}

/*
i1的标签
f1 浮点类型
b1 布尔类型
*/
```

### 匿名字段和内嵌结构体

结构体可以包含一个或多个 **匿名（或内嵌）字段**，即这些字段没有显式的名字，只有字段的类型是必须的，此时类型就是字段的名字。匿名字段本身可以是一个结构体类型，即 **结构体可以包含内嵌结构体**。

类似面向对象语言中的对象继承或组合.

> 注意:在一个结构体中对于每一种数据类型只能有一个匿名字段。

```go
package main

import "fmt"

type innerS struct {
	i1 int
	i2 int
}

type outerS struct {
	b      int
	c      float32
	int    // 匿名字段
	innerS // 匿名字段, 内嵌结构体
}

func main() {
	outer1 := new(outerS)
	outer1.b = 6
	outer1.c = 3.555
	outer1.int = 99
	outer1.i1 = 233
	outer1.i2 = 999

	fmt.Printf("outer1: %v\n", outer1)

	// 使用结构体字面量
	outer2 := outerS{11, 2.33, 55, innerS{21, 65}}
	fmt.Printf("outer2: %v\n", outer2)
}

/*
outer1: &{6 3.555 99 {233 999}}
outer2: {11 2.33 55 {21 65}}
*/
```

==命名冲突==
当两个字段拥有相同的名字（可能是继承来的名字）时该怎么办呢？

1. 外层名字会覆盖内层名字（但是两者的内存空间都保留），这提供了一种重载字段或方法的方式；
2. 如果相同的名字在同一级别出现了两次，如果这个名字被程序使用了，将会引发一个错误（不使用没关系）。没有办法来解决这种问题引起的二义性，必须由程序员自己修正。

## 函数

函数是基本的代码块。

Go 语言最少有个 `main()` 函数。

Go 是编译型语言，所以函数编写的顺序是无关紧要的.

Go 里面有 3 种类型的函数:

- 普通带有名字的函数
- 匿名函数或 lambda 函数
- 方法(Methods)

Go 的函数名不允许重复, 也就是不允许函数重载.

函数定义:

```go
func 函数名([参数]) [返回类型] {
   函数体
}

// 如果需要声明一个在外部定义的函数, 则不需要给出函数体
func 函数名([参数]) [返回类型]

// 或者
type 函数名 func(int, int) int
```

### 参数

Go 默认使用按**值传递**来传递参数，也就是传递参数的副本。函数接收参数副本之后，在使用变量的过程中可能对副本的值进行更改，但不会影响到原来的变量.

如:

```go
func f1(x int, y int) int {
	return x + y
}
```

如果希望函数可以直接修改参数的值, 可以在形参类型前面加 `*`, 实参前面加上取地址符 `&`, 以便传递参数的地址给函数处理, 此时传递给函数的是一个指针。这种叫做**引用传递**.

如:

```go
package main

import "fmt"

func main() {
	a := 1
	b := 2
	sum1 := f1(&a, &b)
	fmt.Println(sum1)
}

func f1(x *int, y *int) int {
	return *x + *y
}

```

在函数调用时，像切片（slice）、字典（map）、接口（interface）、通道（channel）这样的引用类型都是默认使用引用传递.

Go 函数参数没有默认值; 形参定义了可以不用.

参数的几种形式:

```go
// 没有参数
func f1(){
    函数体
}

// 多个参数
func f1(a, b int, c, d string){
    函数体
}

// 以上等价于
func f1(a int, b int, c string, d string){
    函数体
}

// 可变参数, 应该放在形参后面
func f1(a, b int, arg ...string){
    函数体
}
```

可变参数接收类似于切片 `slice` 类型.

如:

```go
package main

import "fmt"

func main() {
	f1(2, 3, "你好", "hello")

	sl1 := []string{"切片", "火星", "world"}

	// 也可以使用解构后的切片
	f1(78, 99, sl1...)
}

func f1(x, y int, args ...string) {
	fmt.Printf("%d %d\n%v\n%T\n", x, y, args, args)
}

/*
2 3
[你好 hello]
[]string
78 99
[切片 火星 world]
[]string
*/
```

### 返回值

Go 返回值的几种情况:

```go
// 没有返回值
func f1(){
    函数体
}

// 一个返回值
func f1(x int) int {
    return x
}

// 多个返回值, 要括起来
func f1(x1 int) (int, string) {
    return x,"你好"
}

// 命名返回值, 要括起来
// 命名返回值在函数内可当作变量用, 并且 return 后面可省略
// 命名返回值作为结果形参被初始化为相应类型的零值
func f1(x int) (a int, b string) {
    a = x+x
    b = "hello"
    return
}
```

### defer 语句

`defer` 语句调用一个函数，该函数的执行被推迟到相关函数返回(`return`)的那一刻.

`defer` 处理的必须是函数或方法调用； 不能用括号括起来。 内置函数的调用与表达式语句一样受到限制。

关键字 `defer` 的用法类似于面向对象编程语言 Java 和 C# 的 `finally` 语句块，它一般用于释放某些已分配的资源。

示例:

```go
package main

import "fmt"

func main() {
	fmt.Println("开始")
	defer fmt.Println("defer执行")
	fmt.Println("结束")
}

/*
开始
结束
defer执行
*/
```

当有多个 `defer` 行为被注册时，它们会以逆序执行（类似栈，即后进先出）：

```go
package main

import "fmt"

func main() {
	for i := 0; i < 5; i++ {
		defer fmt.Printf("%d\n", i)
	}
}

/*
4
3
2
1
0
*/
```

示例: 使用 `defer` 语句实现代码追踪

```go
package main

import "fmt"

func main() {
	b()
}

func trace(s string) {
	fmt.Println("进入", s)
}

func untrace(s string) {
	fmt.Println("离开", s)
}

func a() {
	trace("a")
	defer untrace("a")
	fmt.Println("in a func")
}

func b() {
	trace("b")
	defer untrace("b")
	fmt.Println("in b func")
	a()
}

/*
进入 b
in b func
进入 a
in a func
离开 a
离开 b
*/
```

### 内置函数

| 函数名    | 说明                                                                     |
| --------- | ------------------------------------------------------------------------ |
| `close`   | 主要用来关闭 channel                                                     |
| `len`     | 求长度，比如 string、array、slice、map、channel ，返回长度               |
| `cap`     | capacity 是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）  |
| `make`    | 用来分配内存，返回 Type 本身(只能应用于 slice, map, channel)             |
| `new`     | 用来分配内存，主要用来分配值类型，比如 int、struct。返回指向 Type 的指针 |
| `copy`    | 用于复制和连接 slice，返回复制的数目                                     |
| `append`  | 用来追加元素到数组、slice 中,返回修改后的数组、slice                     |
| `panic`   | 用来做错误处理, 停止常规的 goroutine                                     |
| `recover` | 用来做错误处理, 允许程序定义 goroutine 的 panic 动作                     |
| `print`   | 底层打印函数，在部署环境中建议使用 fmt 包                                |
| `println` | 底层打印函数，在部署环境中建议使用 fmt 包                                |
| `complex` | 创建复数                                                                 |
| `real`    | 返回 complex 的实部                                                      |
| `imag`    | 返回 complex 的虚部                                                      |

### 匿名函数

当我们不希望给函数起名字的时候，可以使用匿名函数, 如:

```go
func(x, y int) int {return x+y}

// 直接对匿名函数进行调用
func(x, y int) int {return x+y}(3, 4)

// 匿名函数赋值给变量
var fn1 = func(x, y int) int {return x+y}
fn1(3, 4) // 调用
```

可以将函数作为返回值:

```go
func Add2() (func(b int) int)
func Adder(a int) (func(b int) int)
```

示例:

```go
package main

import "fmt"

func main() {
	fmt.Printf("传入3给Add2的返回函数: %d\n", Add2()(3))
	fmt.Printf("传入2给Addr函数, 3给其返回函数: %d\n", Adder(2)(3))
}

func Add2() func(b int) int {
	return func(b int) int {
		return b + 2
	}
}

func Adder(a int) func(b int) int {
	return func(b int) int {
		return a + b
	}
}

/*
传入3给Add2的返回函数: 5
传入2给Addr函数, 3给其返回函数: 5
*/
```

## 方法

方法 Method 是一种特殊的函数, 它作用在某个**接收者**上面.

如: 当它作用在接收者为结构体上时, 类似于面向对象语言中类的方法, 其结构体的字段类似于类的属性.

方法的定义:

```go
func (接收者 类型) methodName(参数) (返回值) {
   ...
}
```

示例 1:
在接收者结构体 `student` 上面添加上方法 `study`, 在定义 `study` 方法的第一个括号中指定接收者变量.

```go
package main

import "fmt"

type student struct {
	id   int
	name string
}

func main() {
	s1 := student{3, "宇宙"}
	s1.study()
}

// 定义方法
func (s student) study() {
	fmt.Printf("id: %d name:%v\n", s.id, s.name)
}

/*
id: 3 name:宇宙
*/
```

示例 2: 结合类型别名对其它任意类型来添加方法

```go
package main

import "fmt"

type AA int

func main() {
	var a1 AA
	a1.f1()
}

func (a AA) f1() {
	fmt.Println("类型int别名AA方法")
}

/*
类型int别名AA方法
*/
```

在以上例子中, 通过变量调用类型的方法叫做 `Method Value`, 还可以通过类型传入定义的变量作为参数调用方法叫做 `Method Expression`:

```go
package main

import "fmt"

type AA int

func main() {
	var a1 AA
	AA.f1(a1) //类型调用方法, 传入变量作为参数
}

func (a AA) f1() {
	fmt.Println("类型int别名AA方法")
}

/*
类型int别名AA方法
*/
```

==总结==
方法的一些特性:

- Go 的方法是一种特殊的函数，与普通函数的区别在于，方法需要一个接收者（receiver），它是作用在接收者上的函数，接收者是某种类型的变量
- 接收者几乎可以是任何的类型，不仅仅是结构体 struct，还可以是整型、浮点型、布尔型、数组等等，但是不能是接口。因为接口是一个抽象定义，但是方法却是具体实现；使用接口作接收者会引发一个编译错误：invalid receiver type
- 使用结构体加上它的方法其实就类似实现了面向语言中的类，但不同的是，Go 拓展了“类”的范围，Go 的方法可以在任何类型上面添加，不局限于结构体
- 在 Go 中，类型的代码和绑定在它上面的代码可以分开存在于不同的源文件中，不一定要放置在一起。但至少，它们**必须是同一个包**的（如果方法和接收者类型在不同包，方法会找不到他的接收者）。在同一个包中，方法可以访问到类型中的属性字段（无论是否大写）
- 方法是一种函数，因此不支持重载，即对于某个类型，不能有多个相同名字的方法。但是，如果接收者类型不同，那么允许相同名字的方法存在，即相同名字的方法可以在多个不同的类型接收者上存在。

### 方法接收者的值传递与指针传递

方法中对接收者默认也是值传递，即传入方法的接收者为原始接收者的拷贝，在方法中无法直接改变真正的接收者变量。

如果确实需要在方法中改变接收者，可以使用接收者的指针来传递.

示例:

```go
package main

import "fmt"

type st1 struct {
	i1 int
}

func main() {
	var s1 st1
	s1.i1 = 11
	s1.f1(233)
	fmt.Printf("接收者真正的i1字段: %v\n", s1)
	s1.f2(456)
	fmt.Printf("接收者真正的i1字段: %v\n", s1)
}

func (s st1) f1(a int) {
	s.i1 = a
	fmt.Printf("f1方法改变i1后: %v\n", s)
}

func (s *st1) f2(a int) {
	s.i1 = a
	fmt.Printf("f2方法改变i1后: %v\n", s)
}

/*
f1方法改变i1后: {233}
接收者真正的i1字段: {11}
f2方法改变i1后: &{456}
接收者真正的i1字段: {456}
*/
```

从性能方面考虑，传递一个地址比起拷贝一个类型变量更加具有优势，因此更加推荐这种传递接收者类型的指针。而且我们也可以看到在访问结构体字段时用法和非指针的结构体的用法是相同的，Go 在内部帮我们自动作了转换.

### 内嵌方法的继承

如果内嵌结构体拥有一个方法，那么外层的结构体同样继承了内嵌结构体的方法，可以直接通过外层结构体直接访问到该方法。

而如果外层结构体定义了一个同名的方法，那么外层的方法将覆盖掉内嵌方法，直接通过外层结构体访问该方法将访问到外层结构体关联的方法，而如果要使用内嵌方法，则需要一层一层嵌套访问。

## 接口

接口也是一种类型, 一种抽象的类型.

接口定义了一组方法（方法集），但是这些方法不包含（实现）代码：它们没有被实现（它们是抽象的）。接口里也不能包含变量。

定义接口:

```go
type 接口名 interface {
    Method1(param_list) return_type
    Method2(param_list) return_type
    ...
}
```

一个实现了接口中这些方法的具体类型是这个接口类型的实例。

在 Go 语言中接口可以有值，一个接口类型的变量: `var s1 Shape`, `s1` 的值和类型都是 `<nil>`, 因为接口是动态类型, `s1` 只能赋值给实现了该接口的某一类型, 此处没有实现该接口的类型, 也没有赋值:

```go
package main

import "fmt"

type Shape interface {
	Area() float32
	Perimeter() float32
}

func main() {
	var s1 Shape
	fmt.Printf("s1 值: %v 类型: %T\n", s1, s1)
}

/*
s1 值: <nil> 类型: <nil>
*/
```

实现接口:

```go
package main

import "fmt"

type Shape interface {
	Area() float32
	Perimeter() float32
}

type Rect struct {
	w1 float32
	h1 float32
}

func (r Rect) Area() float32 {
	return r.w1 * r.h1
}

func (r Rect) Perimeter() float32 {
	return 2 * (r.w1 + r.h1)
}

func main() {
	var s1 Shape
	s1 = Rect{}
	r1 := Rect{2.33, 10.55}
	fmt.Printf("s1 值: %v 类型: %T\n", s1, s1)
	fmt.Printf("r1 面积: %v 周长: %v\n", r1.Area(), r1.Perimeter())
}

/*
s1 值: {0 0} 类型: main.Rect
r1 面积: 24.5815 周长: 25.76
*/
```

`Rect` 结构体类型实现了 `Shape` 接口, 所以 `s1` 可以赋值 `Rect` 结构体类型.
如果 `s1` 赋值给其它实现了此接口的类型, `s1` 就可以是其它实现了此接口的类型.

总结:

1. 类型不需要显式声明它实现了某个接口：接口被隐式地实现。
2. 多个类型可以实现同一个接口。
3. 实现某个接口的类型（除了实现接口方法外）可以有其他的方法。
4. 一个类型可以实现多个接口。
5. 接口类型可以包含一个实例的引用，该实例的类型实现了此接口（接口是动态类型）。

### 接口嵌套

一个接口可以包含一个或多个其他的接口，这相当于直接将这些内嵌接口的方法列举在外层接口中一样。

如:

```go
type ii1 interface {
    a1() int
}

type ii2 interface {
    ii1  // 嵌套 ii1 接口
    b1() string
}
```

### 类型断言

一个接口类型的变量, 可以包含任何类型的值, 也就是动态类型, 必须有一种方式来检测它的 **动态** 类型，即运行时在变量中存储的值的实际类型。

可以使用**类型断言**来测试在某个时刻接口类型的变量包含哪种类型的值.
`varI.(T)`
`varI` 必须是接口类型的变量, 否则报错.
`T` 是某一类型.

类型断言可能是无效的，虽然编译器会尽力检查转换是否有效，但是它不可能预见所有的可能性。如果转换在程序运行时失败会导致错误发生。

更安全的方式是使用以下形式来进行类型断言：

```go
if v, ok := varI.(T); ok {  // checked type assertion
    ...
}
```

如果转换合法，v 是 varI 转换到类型 T 的值，ok 会是 true；否则 v 是类型 T 的零值，ok 是 false，也没有运行时错误发生。

示例:

```go
package main

import "fmt"

type Shape interface {
	Area() float32
	Perimeter() float32
}

type Rect struct {
	w1 float32
	h1 float32
}

func (r Rect) Area() float32 {
	return r.w1 * r.h1
}

func (r Rect) Perimeter() float32 {
	return 2 * (r.w1 + r.h1)
}

func main() {
	var s1 Shape
	s1 = Rect{}
	r1 := Rect{2.33, 10.55}
	if v, ok := s1.(Rect); ok {
		fmt.Printf("s1的值: %v 是否合法: %v\n", v, ok)
	}
	fmt.Printf("r1 面积: %v 周长: %v\n", r1.Area(), r1.Perimeter())
}

/*
s1的值: {0 0} 是否合法: true
r1 面积: 24.5815 周长: 25.76
*/
```

### 空接口

空接口不包含任何方法, 对实现没有要求:

```go
type Any interface {}
```

可以给一个空接口类型的变量 `var val interface {}` 赋任何类型的值。

## 反射

Go 语言提供了一种机制，能够在**运行时**更新变量和检查它们的值、调用它们的方法和它们支持的内在操作，而不需要在编译时就知道这些变量的具体类型。这种机制被称为反射。

反射机制允许我们在程序运行时检查变量的类型结构、值、方法等，同时还能动态修改变量值、调用方法等。

使用反射需要导入反射包:

```go
import "reflect"
```

对于一个变量来说，最基本的信息就是它的类型和值。在 Go 的反射包中定义了两个类型: `reflect.Type` 和 `reflect.Value`, 分别表示变量的类型信息和变量的值。

同时, 这两种类型下面定义了一些函数:

```go
// 分别返回被检查对象的类型和值
func TypeOf(i interface{}) Type
func ValueOf(i interface{}) Value
```

示例:

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	i1 := 233
	v1 := reflect.ValueOf(i1)
	t1 := reflect.TypeOf(i1)
	fmt.Printf("v1: %v t1: %v\n", v1, t1)
}

/*
v1: 233 t1: int
*/

```

以上函数返回的类型和值也有相应的方法:

```go
// 类型方法
type Type interface {
    // 返回类型名称
    Name() string

    // 返回底层类型名称
    Kind() Kind

    // 返回结构体的第 i 个字段(结构体形式)
    Field(i int) StructField

    // 返回名为 name 的结构体字段和是否找到该字段的布尔值
    FieldByName(name string) (StructField, bool)

    // 返回结构体中的字段数量
    NumField() int
}

// 值方法
type Value interface {
    // 返回 Value 的布尔值, 如果 Value不是布尔值会出错
    // 以下为同类型方法
    func (v Value) Bool() bool
    func (v Value) Bytes() []byte
    func (v Value) Float() float64
    func (v Value) Int() int64
    func (v Value) String() string

    // 返回结构体中的字段数量
    func (v Value) NumField() int

    // 返回方法集中导出的方法的数量。
    func (v Value) NumMethod() int

    // 返回与 v 的第 i 个方法相对应的函数值。
    func (v Value) Method(i int) Value

    // 返回结构体的第 i 个字段。
    func (v Value) Field(i int) Value

    // 返回名为 name 的结构体字段
    func (v Value) FieldByName(name string) Value

    // 返回 v 的第 i 个元素。
    func (v Value) Index(i int) Value

    // 返回接口 v 包含的值或指针 v 指向的值。
    func (v Value) Elem() Value

    // 检测 值 是否能被修改
    func (v Value) CanSet() bool

    // 将v的基础值设置为x。
    func (v Value) SetInt(x int64)
}
```

### 获取结构体相应的属性

```go
package main

import (
	"fmt"
	"reflect"
)

type Dog struct {
	name string
	age  int
}

func (d Dog) Say(c1 string) string {
	return c1
}

func main() {
	d1 := Dog{"dog1", 3}
	t1 := reflect.TypeOf(d1)
	v1 := reflect.ValueOf(d1)

	for i := 0; i < t1.NumField(); i++ {
		tf := t1.Field(i)
		vf := v1.Field(i)
		fmt.Printf("t1: %v %v %v\n", tf.Name, tf.Type, tf)
		fmt.Printf("v1: %v\n", vf)
	}

	for i := 0; i < t1.NumMethod(); i++ {
		tm := t1.Method(i)
		fmt.Printf("tMethod: %v %v %v\n", tm.Name, tm.Type, tm)
		fmt.Printf("vMethod: %v\n", v1.Method(i))
	}
}

/*
t1: name string {name main string  0 [0] false}
v1: dog1
t1: age int {age main int  16 [1] false}
v1: 3
tMethod: Say func(main.Dog, string) string {Say  func(main.Dog, string) string <func(main.Dog, string) string Value> 0}
vMethod: 0xa53600
*/

```

### 通过反射修改值

要修改值, 在通过函数获取值的时候应该传入值的地址: `v = reflect.ValueOf(&x)`

检查 `v.CanSet()` 是否为 `true`, 表示能修改值.

使用 `v.Elem()` 间接使用指针, 再配合相应的方法修改同类型的值.

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x1 := 123
	v1 := reflect.ValueOf(&x1)
	v1.Elem().SetInt(999)
	fmt.Printf("x1: %v", x1)
}

/*
x1: 999
*/

```

### 动态调用方法

通过 `MethodByName()` 方法来返回指定方法名的函数值类型，然后使用它的 `Call()` 方法去动态调用方法，当需要传参时，传入的参数必须时 `reflect.Value` 类型组成的一个切片，可以使用 `reflect.ValueOf()` 方法将任意类型的值转换成一个 `relfect.Value` 类型.

```go
package main

import (
	"fmt"
	"reflect"
)

type Dog struct {
	Name string
	Age  int
}

func (d Dog) Say(c1 string) {
	fmt.Printf("%v %v", d.Name, c1)
}

func main() {
	d1 := Dog{"dog1", 2}
	v1 := reflect.ValueOf(d1)
	vm := v1.MethodByName("Say")
	args := []reflect.Value{reflect.ValueOf("汪汪汪")}
	vm.Call(args)
}

/*
dog1 汪汪汪
*/

```

## 协程 (goroutine) 和 通道 (channel)

进程、线程 和 协程 之间概念的区别:

- 对于 进程、线程，都是有内核进行调度，有 CPU 时间片的概念，进行 **抢占式调度**（有多种调度算法）
- 对于 协程(用户级线程)，这是对内核透明的，也就是系统并不知道有协程的存在，是完全由用户自己的程序进行调度的，因为是由用户程序自己控制，那么就很难像抢占式调度那样做到强制的 CPU 控制权切换到其他进程/线程，通常只能进行 **协作式调度**，需要协程自己主动把控制权转让出去之后，其他协程才能被执行到。

本质上，goroutine 就是协程。 不同的是，Golang 在 runtime、系统调用等多方面对 goroutine 调度进行了封装和处理，当遇到长时间执行或者进行系统调用时，会主动把当前 goroutine 的 CPU (P) 转让出去，让其他 goroutine 能被调度并执行，也就是 Golang 从语言层面支持了协程。

Golang 的一大特色就是从语言层面原生支持协程，在函数或者方法前面加 `go` 关键字就可创建一个协程。

协程的基本特点归纳为：

- 协程调度机制无法实现公平调度
- 协程的资源开销是非常低的，一台普通的服务器就可以支持百万协程。

协程（coroutine）是 Go 语言中的轻量级线程实现，由 Go 运行时（runtime）管理。

在一个**函数**调用前加上 `go` 关键字，这次调用就会在一个新的 goroutine 中并发执行。当被调用的函数返回时，这个 goroutine 也自动结束。需要注意的是，如果这个函数有返回值，那么这个返回值会被丢弃。

```go
package main

import "fmt"

func main() {
	for i := 0; i < 10; i++ {
		go Counter(i)
	}
}

func Counter(x int) {
	fmt.Println(x)
}
```

执行上面的程序, 发现什么也没有打印出来, 程序就退出了。
对于以上例子, `main` 函数执行的时候不会等待 `f1` 函数执行完就返回了.

**`main` 协程结束之后, 所有协程都会销毁.**

想要让 `main` 函数等待所有 goroutine 退出后再返回，但如何知道 goroutine 都退出了呢？这就引出了多个 goroutine 之间通信的问题。

在工程上，有两种最常见的线程通信模型：**共享内存** 和 **消息传递**。

而 golang 中的 channel(通道)是使用消息传递机制线程通信模型.

### 通道 (channel)

消息机制认为每个并发单元是自包含的、独立的个体，并且都有自己的变量，但在不同并发单元间这些变量不共享。每个并发单元的输入和输出只有一种，那就是消息。

channel 是 Go 语言在语言级别提供的 goroutine 间的通信方式，我们可以使用 channel 在多个 goroutine 之间传递消息。channel 是进程内的通信方式，因此通过 channel 传递对象的过程和调用函数时的参数传递行为比较一致，比如也可以传递指针等。

channel 是类型相关的，一个 channel 只能传递一种类型的值，这个类型需要在声明 channel 时指定。

channel 的声明形式为:

```go
var 通道名 chan 类型

// 如: 声明一个传递 int 类型的 channel
var ch chan int

// 使用内置函数 make() 
ch := make(chan int)

// 带缓冲的 channel
ch := make(chan int, 10)
```

channel 是使用通信操作符 `<-` 进行发送和接收, 这个操作符直观的标示了数据的传输：信息按照箭头的方向流动。

```go
// 发送
// 将一个数据value发送至channel，这会导致阻塞，直到有其他goroutine从这个channel中接收数据
ch <- value

// 接收
// 从channel中接收数据，如果channel之前没有发送数据，也会导致阻塞，直到channel中被发送数据为止
value := <-ch

// 无变量接收, 发送过来的值会被丢弃
<-ch
```

默认情况下，channel 的接收和发送都是阻塞的，除非另一端已准备好。

我们还可以创建一个带缓冲的 channel：

```go
c := make(chan int, 1024)

// 从带缓冲的channel中读数据
for i:=range c {
　　...
}
```

此时，创建一个大小为 1024 的 int 类型的 channel，即使没有读取方，写入方也可以一直往 channel 里写入，在缓冲区被填完之前都不会阻塞。

可以关闭不再使用的 channel：

```go
close(ch)
```

上面的计时器使用 channel 重写:

```go
package main

import "fmt"

func main() {
	ch := make(chan int)
	for i := 0; i < 10; i++ {
		go Counter(i, ch) // 子协程1
		<-ch // main 协程接收通道 ch 的值
	}
}

func Counter(x int, ch chan int) {
	fmt.Println(x)
	ch <- 23 // 子协程发送值到通道 ch
}

/*
0
1
2
3
4
5
6
7
8
9
*/
```

### select 语句

select 是 Go 中的一个控制结构，类似于用于通信的 switch 语句。每个 case 必须是一个通信操作，要么是发送要么是接收。

语法:

```go
select {
    case <-ch1:
        // 如果从 ch1 信道成功接收数据，则执行该分支代码
    case ch2 <- 1:
        // 如果成功向 ch2 信道成功发送数据，则执行该分支代码
    default:
        // 如果上面都没有成功，则进入 default 分支处理流程
}
```

select 默认是阻塞的，只有当监听的 channel 中有发送或接收可以进行时才会运行，当多个 channel 都准备好的时候，select 是随机的选择一个执行。

> 注意：如果 ch1 或者 ch2 信道都阻塞的话，就会立即进入 default 分支，并不会阻塞。但是如果没有 default 语句，则会阻塞直到某个信道操作成功为止。

知识点:

1. select 语句只能用于信道的读写操作
2. select 中的 case 条件(非阻塞)是并发执行的，select 会选择先操作成功的那个 case 条件去执行，如果多个同时返回，则随机选择一个执行，此时将无法保证执行顺序。对于阻塞的 case 语句会直到其中有信道可以操作，如果有多个信道可操作，会随机选择其中一个 case 执行
3. 对于 case 条件语句中，如果存在信道值为 nil 的读写操作，则该分支将被忽略，可以理解为从 select 语句中删除了这个 case 语句
4. 对于空的 select{}，会引起死锁
5. 对于 for 中的 select{}, 也有可能会引起 cpu 占用过高的问题

示例 1: case 语句是随机执行的

```go
package main

import "fmt"

func main() {
	size := 10
	ch1 := make(chan int, size)
	ch2 := make(chan int, size)
	ch3 := make(chan int, 1)

	for i := 0; i < size; i++ {
		ch1 <- 1
	}

	for i := 0; i < size; i++ {
		ch2 <- 2
	}

	select {
	case v1 := <-ch1:
		fmt.Printf("ch1: %v\n", v1)
	case v2 := <-ch2:
		fmt.Printf("ch2: %v\n", v2)
	case ch3 <- 10:
		fmt.Printf("ch3: %v\n", <-ch3)
	default:
		fmt.Println("default")
	}
}

/*
执行1次: ch3: 10
执行2次: ch1: 1
执行3次: ch1: 1
执行4次: ch2: 2
...
*/

```

多次执行，会随机输出不同的值, 永远不会执行 default 语句，因为上面的三个 case 都是可以操作的信道。

示例 2: 超时用法

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan int)
	go func(ch chan int) {
		time.Sleep(1 * time.Second) // 等待1s, 会打印 1
		// time.Sleep(5 * time.Second) // 等待5s, 会打印 超时
		ch <- 1
	}(ch1)

	select {
	case v1 := <-ch1:
		fmt.Printf("<-ch1: %v\n", v1)
	case <-time.After(3 * time.Second): // 3s后超时
		fmt.Println("超时")
	}

}

/*
等待1s, 会打印 1
等待5s, 会打印 超时
*/

```
