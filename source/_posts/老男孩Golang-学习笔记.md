---
title: 老男孩Golang 学习笔记
date: 2018-05-09 13:35:00
---
# 老男孩Golang-学习笔记第一期

## 第1章-简介和配置

 2018-02-19

# 一、介绍和安装

## 1.介绍

### 1.1 什么是Golang

Go也被称为Golang，它是由谷歌创建的一种开源、编译和静态类型的编程语言。

Golang的主要目标是使高可用性和可伸缩的web应用程序的开发变得简单易行。

### 1.2 为什么选择Golang

当有很多其他语言(如python、ruby、node.js)时，为什么选择Golang作为服务端编程语言呢?

- 并发是语言的一个固有部分。因此编写多线程程序是小菜一碟。这是通过Goroutines和channel实现的，我们将在后面的教程中讨论。
- Golang是一种编译语言。源代码编译成原生二进制。这在解释语言(如nodejs中使用的JavaScript)中丢失了。
- 语言规范非常简单。整个规范适合于一个页面，您甚至可以使用它来编写自己的编译器:)
- go编译器支持静态链接。所有的go代码都可以静态地链接到一个大的fat二进制文件中，并且可以轻松地部署到云服务器中，而不用担心依赖关系。

## 2.安装

### 2.1下载

在Mac、Windows和Linux三个平台上都支持Golang。您可以从https://golang.org/dl/下载相应平台的二进制文件。

![img](http://om1c35wrq.bkt.clouddn.com/001_%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85%E5%8C%85.jpg)

Mac OS 从<https://golang.org/dl/>下载`osx`安装程序。双击启动安装。按照提示，这应该在`/usr/local/go`中安装了`Golang`，并且还会将文件夹`/usr/local/go/bin`添加到您的`PATH`环境变量中。

Windows 从<https://golang.org/dl/>下载`MSI`安装程序。双击启动安装并遵循提示。这将在位置`C`中安装`Golang:\Go`，并且还将添加目录`C:\Go\bin`到您的`path`环境变量。

Linux 从<https://golang.org/dl/>下载 `tar `文件，并将其解压到`/usr/local`。

将`/usr/local/go/bin`添加到`PATH`环境变量中。这应该安装在`linux`中。

### 2.2 windows下安装并配置环境变量

安装步骤就不在多说什么了，一路到底

**A、配置环境变量**

注意：如果是 `msi` 安装文件，`Go` 语言的环境变量会自动设置好。

我的电脑——右键“属性”——“高级系统设置”——“环境变量”——“系统变量”

​	假设GO安装于C盘根目录

**新建：**

- `GOROOT：Go` 安装路径（例：`C:\Go`）
- `GOBIN：Go`安装路径的bin路径（例：`C:\Go\bin`）
- `GOPATH：Go`工程的路径（例：`E:\go`）。如果有多个，就以分号分隔添加

**修改：**

- `Path：` 添加上`Go`安装路径，以分号结尾

> 工作目录就是我们用来存放开发的源代码的地方，对应的也是Go里的GOPATH这个环境变量。这个环境变量指定之后，我们编译源代码等生成的文件都会放到这个目录下，GOPATH环境变量的配置参考上面的安装Go，配置到Windows下的系统变量里。

**B、查看是否安装配置成功**

使用快捷键win+R键，输入cmd，打开命令行提示符，在命令行中输入

```
go env  # 查看得到go的配置信息
go version  # 查看go的版本号
```

### 2.3 mac系统安装并配置

**安装**

双击 pkg 包，顺着指引，即可安装成功。 在命令行输入 go version，获取到 go 的version，则代表安装成功。

**配置环境变量**

1、打开终端输入cd ~进入用户主目录; 2、输入ls -all命令查看是否存在.bash_profile; 3、存在既使用vim .bash_profile 打开文件; 4、输入 i 进入vim编辑模式； 5、输入下面代码， 其中 GOPATH: 日常开发的根目录。GOBIN:是GOPATH下的bin目录。

> `export GOPATH=/Users/yztc/go`
>
> `export GOBIN=$GOPATH/bin`
>
> `export PATH=$PATH:$GOBIN`

6、点击ESC，并输入 :wq 保存并退出编辑。可输入vim .bash_profile 查看是否保存成功。

7、输入source ~/.bash_profile 完成对golang环境变量的配置，配置成功没有提示。 8、输入go env 查看配置结果

# 二、搭建开发工具

安装好atom工具，然后安装go-plus插件和atom-terminal-panel插件。

1.安装go-plus插件

![img](http://om1c35wrq.bkt.clouddn.com/002_%E6%8F%92%E4%BB%B6.bmp)

2.安装atom-terminal-panel插件

![img](http://om1c35wrq.bkt.clouddn.com/001_%E6%8F%92%E4%BB%B6.bmp)

# 三、第一个程序：HelloWorld

### 3.1 编写第一个程序

1.打开编辑器创建一个新的helloworld.go文件，并输入以下内容：

```
package main

import "fmt"

func main() {
   /* 输出 */
   fmt.Println("Hello, World!")
}
```

2.执行go程序

执行go程序由几种方式

方式一：使用go run命令

​	step1：使用快捷键win+R，输入cmd打开命令行提示符

​	step2：进入helloworld.go所在的目录

​	step3：输入go run helloworld.go命令并观察运行结果。

方式二：使用go build命令

​	step1：使用快捷键win+R，输入cmd打开命令行提示符

​	step2：进入helloworld.go所在的目录

​	step3：输入go build helloworld.go命令进行编译，产生同名的helloworld.exe文件

​	step4：输入helloworld.exe，执行

方式三：使用 go playground

​	step1：打开一下网址https://play.golang.org/

### 3.2 第一个程序的解释说明

#### 3.2.1 package

- 在同一个包下面的文件属于同一个工程文件，不用`import`包，可以直接使用
- 在同一个包下面的所有文件的package名，都是一样的
- 在同一个包下面的文件`package`名都建议设为是该目录名，但也可以不是

#### 3.2.2 import

import “fmt” 告诉 Go 编译器这个程序需要使用 fmt 包的函数，fmt 包实现了格式化 IO（输入/输出）的函数

可以是相对路径也可以是绝对路径，推荐使用绝对路径（起始于工程根目录）

1. 点操作 我们有时候会看到如下的方式导入包

   ```
   import(
   	. "fmt"
   )
   ```

   这个点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名，也就是前面你调

   用的`fmt.Println("hello world")`可以省略的写成`Println("hello world")`

2. 别名操作 别名操作顾名思义我们可以把包命名成另一个我们用起来容易记忆的名字

   ```
   import(
   	f "fmt"
   )
   ```

   别名操作的话调用包函数时前缀变成了我们的前缀，即`f.Println("hello world")`

3. _操作 这个操作经常是让很多人费解的一个操作符，请看下面这个import

   ```
   import (
     "database/sql"
     _ "github.com/ziutek/mymysql/godrv"
   )
   ```

   _操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数

#### 3.3.3 main与init

- 这两个函数在定义时不能有任何的参数和返回值
- 虽然一个package里面可以写任意多个init函数，但推荐只用一个
- Go程序会自动调用init()和main()
- 每个package中的init函数都是可选的，但package main就必须包含一个main函数
- 先调用init函数，再调用main函数
- 运行程序，必须要运行存在main函数的go文件

`初始化顺序：`

程序的初始化和执行都起始于main包。如果main包还导入了其它的包，那么就会在编译时将它们依次导入。有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。当一个包被导入时，如果该包还导入了其它的包，那么会先将其它包导入进来，然后再对这些包中的包级常量和变量进行初始化，接着执行init函数（如果有的话），依次类推。等所有被导入的包都加载完毕了，就会开始对main包中的包级常量和变量进行初始化，然后执行main包中的init函数（如果存在的话），最后执行main函数。

# 四、编码规范

### 4.1 编码规范

### 4.2 注释

- 单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释
- 多行注释也叫块注释，均已以 /* 开头，并以 */ 结尾，且不可以嵌套使用，多行注释一般用于包的文档描述或注释成块的代码片段

### 4.3 标识符

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就**可以被外部包的代码所使用**（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；**标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的**（像面向对象语言中的 private ）

### 4.4 语句的结尾

Go语言中是不需要类似于Java需要冒号结尾，默认一行就是一条数据

如果你打算将多个语句写在同一行，它们则必须使用 **;**



### 参考

[第1章-简介和配置](http://liyuechun.org/2018/02/19/golang-introduce/)



