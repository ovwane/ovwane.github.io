---
title: Google资深工程师深度讲解Go语言
date: 2018-05-11 13:05:42
---

# Google资深工程师深度讲解Go语言-笔记

**课程结构**

语法部分

综合部分

实战项目部分

**课程目标**

会一门编程语言

最好有项目经验



**基本语法**

变量

选择，循环

指针，数组，容器

**面向接口**

结构体

duck typing的概念

组合的思想

**函数式编程**

闭包的概念

多样的例题

**工程化**

资源管理，错误处理

测试和文档

性能调优

**并发编程**

goroutine和channel

理解调度器

多样的例题



**Go语言的安装与开发环境**

[下载Go](http://studygolang.com/dl) go1.9.2

IED: GoLand, IntelliJ IDEA + Go插件



**变量**

GOPATH

GOROOT

**变量定义**

```go
//使用var关键字
/*
var a, b, c bool
var s1, s2 string = "hello","world"
可放在函数内，或直接放在包内
使用var()集中定义变量
 */

//让编译器自动决定类型
/*
var a, b, i, s1, s2 = true, false, 3, "hello", "world"
 */

//使用:=定义变量
/*
a, b, i, s1, s2 := true, false, 3, "hello", "world"
只能在函数内使用
 */
```

**内建变量类型**

```go
bool, string
(u)int, (u)int8, (u)int16, (u)int32, (u)int64, uintptr
byte, rune
float32, float64, complex64, complex128
```

**复数**

3 + 4i

3实部，4i虚部

欧拉公式(euler)

**强制类型转换**

类型转换是强制的

```go
var a, b int = 3, 4
var c int

c = int(math.Sqrt(float64(a * a + b * b)))
//浮点数精度问题？
```

**常量的定义**

```go
const filename = "abc.txt"
//const 数值可作为各种类型使用
const a, b = 3, 4
var c int = int(math.Sqrt(a * a + b * b))
```

**使用常量定义枚举类型**

```go
func enums() {
    //普通枚举类型
	const(
		cpp = iota
		_
		python
		golang
		javascript
	)
	
    //自增值枚举类型
	// b, kb, mb, gb, tb, pb
	const (
		b = 1 << (10 * iota)
		kb
		mb
		gb
		tb
		pb
	)

	fmt.Println(cpp, javascript, python, golang)
	fmt.Println(b, kb, mb, gb, tb, pb)
}
```

**变量定义要点回顾**

变量类型写在变量名之后

编译器可推测变量类型

没有char，只有rune

原生支持复数类型

**条件语句**

if

```go
/*
if的条件里可以赋值
if的条件里赋值的变量作用域就在这个if语句里
 */
if contents, err := ioutil.ReadFile(filename); err != nil {
    fmt.Println(string(contents))
} else {
    fmt.Println("cannot print file contents:", err)
}
```

switch

```go
//switch会自动break，除非使用fallthrough
func grade(score int) string {
    g := ""
    switch {
        case score < 0 || score > 100:
        	panic(fmt.Sprintf("Wrong score: %d", score))
        case score < 60:
        	g = "F"
        case score < 80:
         	g = "C"
        case score < 90:
         	g = "B"
        default:
        	g = "A"
    }
    return g
}

//switch后可以没有表达式
func grade(score int) string {
    switch {
        case score < 60:
        	return "F"
        case score < 80:
        	return "C"
        case score < 90:
        	return "B"
        default:
        	return "A"
    }
}
```

for

```go
//for的条件里不需要括号
//for的条件里可以省略初始条件，结束条件，递增表达式

//省略初始条件，相当于while
func convertToBin(v int) string {
    result := ""
    for ; v > 0; v /= 2 {
        result = strconv.Itoa(v%2) + result
    }
    return result
}

//省略初始条件，相当于while
for scanner.Scan() {
    fmt.Println(scanner.Text())
}

//无限循环
for {
    fmt.Println("abc")
}
```

**基本语法要点回顾**

for, if 后面的条件没有括号

if 条件里也可定义变量

没有while

switch不需要break，也可以直接switch多个条件

**函数**

```go
//函数可返回多个值
func div(a,b int) (int, int) {
    return a / b, a % b
}

//函数返回多个值时可以起名字
//仅用于非常简单的函数
//对于调用者而言没有区别
func div(a, b int) (q, r int) {
    q = a / b
    r = a % b
    return
}

//函数作为参数
func apply(op func(int, int) int, a, b int) int {
    fmt.Printf("Calling %s with %d, %d\n",
        runtime.FuncForPC(reflect.ValueOf(op).Pointer()).Name(),
        a, b)
    return op(a, b)
}

//可变参数列表
func sumArgs(values ...int) int {
    sum := 0
    for i := range values {
        sum += values[i]
    }
    return sum
}
```

**函数语法要点回顾**

返回值类型写在最后面

可返回多个值

函数作为参数

没有默认参数，可选参数

**指针**

```go
//指针并不能运算

```

