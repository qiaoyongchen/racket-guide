# Racket 指南
该教程是为Racket新手或对Racket某些部分不熟悉的人准备的。然而，它假设你已有编程经验，如果你是编程新手，可以考虑先看[How to Design Programs](http://www.htdp.org/)。如果你只想快速浏览Racket介绍，请点击[Quick: An Introduction to Racket with Pictures](https://docs.racket-lang.org/quick/index.html)。

本指南第二章是Racket简介。从第三章开始，将详细介绍Racket工具箱的大部分内容，其他细节部分请参考[The Racket Reference](https://docs.racket-lang.org/reference/index.html)和其他参考手册。

## 1 欢迎来到Racket
取决于你如何看待它，Racket可以是
- 编程语言 - 一个lisp方言，一个Scheme分支
- 一系列语言 - Racket的变体
- 一套工具 - 用于使用一系列语言

没有特别说明的话，我们只是简单使用Racket语言本身。

Racket的主要工具包括
- Racket - 核心编译器，解释器和一个运行时的系统
- DrRacket - 编程环境
- raco - 用于执行racket命令来安装第三方包、构建库等操作的命令行工具。

通常你会通过DrRacket探索Racket，特别是在刚开始的时候。如果你愿意，你也可以通过你熟悉的文本编辑器和命令行来运行，详见[Command-Line Tools and Your Editor of Choice](https://docs.racket-lang.org/guide/other-editors.html)。本指南的其他部分将和你选择的编辑器无关。

如果你使用DrRacket，你需要选择合适的语言，因为DrRacket提供了很多不同的Racket变体和其他语言。假设你之前从未使用过DrRacket，首先启动它，在文本区的顶部输入
```
#lang racket
```
然后点击文本区上方的运行按钮。DrRacket会明白你的意思并在典型的Racket变体中运行(和更小一点的[racket/base](https://docs.racket-lang.org/reference/index.html)或很多其他选项不同)。如果你之前使用过DrRacket，它会记住你上次使用的语言，而不是从#lang行推断。选择 Language|Choose Language 菜单，将会出现一个对话框，选择第一项，告诉DrRacket通过#lang在源码中选择所选的语言。这仍将会把#lang放在文本区的顶部。

### 1.1与Racket交互
DrRacket文本区的下方和racket命令行程序(无参数启动)都扮演一个计算器。如果你输入一个Racket表达式，回车，结果便会被打印出来。在Racket的术语中，这种类型的计算器被称为read-eval-print loop 或简称为REPL。

数字本身就是表达式，结果是这个数字：
```
> 5
5
```
字符串仍然是一个自运行的表达式。字符串表达式被写为首尾均是双引号的字符串：
```
> "Hello, world!"
"Hello, world!"
```
Racket用圆括号包围长表达式-几乎除了简单的常量以外的任何表达式。比如方法调用被写为:左圆括号，方法名，参数表达式，右圆括号。下面的表达式用参数"the boy out of the cpuntry"， 4， 7调用内置的方法[substring](https://docs.racket-lang.org/reference/strings.html#%28def._%28%28quote._~23~25kernel%29._substring%29%29)：
```
> (substring "the boy out of the country" 4 7)
"boy"
```

### 1.2定义和交互
你可以通过定义的形式定义一个类似substring的函数，就像这样：
```
（define (extract str)
    (substring str 4 7))

> (extract "the boy out of country")
"boy"
> (extract "the country out of the boy")
"cou"
```
尽管你可以在REPL中执行这个定义，但是定义通常是程序的一部分，你可能希望保存它，并在之后使用。所以在DrRacket中，你通常都会把定义放在文本区上方的定义区（和#lang放在一起）。
```
#lang racket
(define (extract str)
    (substring str 4 7))
```
如果(extract "the boy")是你程序的一部分，你可以在定义区中定义。但是如果你仅仅想运行一下extract的例子，那么你可能更偏向于不在定义区运行，而是点击Run按钮，并且在REPL中执行(extract "the boy")。

当你用racket命令行代替DrRacket时，你可以用你习惯的编辑器并把上面的文本保存进去。例如保存进“extract.rkt”，在文件相同目录打开racket，执行下面的操作：
```
> (enter! "extract.rkt")
> (extract "the gal out of the city")
"gal"
```

enter!会加载源码并且在模块中切换运行环境，就像在DrRacket中点击Run按钮一样。

### 1.3创建可执行程序
如果你的文件（或者DrRacket）包含如下内容
```
#lang racket
(define (extract str)
    (substring str 4 7))

(extract "the cat out of the bag")
```
那么这就是一个完整的程序，并会在运行的时候打印出“cat”。你可以使用DrRacket或者在racket中用enter!运行这个程序，但是如果这段源码被保存进<源码文件名>，我也可以在命令行中这样运行它
```
racket <源码文件名>
```
这里给出一些把代码打包成可执行程序的建议：
- 在DrRacket中，你可以选择 Racket|Create Executable...选项
- 命令行中，运行raco exe <源码文件名>。查看[raco exe: Creating Stand-Alone Executables](https://docs.racket-lang.org/raco/exe.html)了解更多信息。
- Unix或Mac OS中，你可以通过在代码文件最开始的地方插入下面的代码来来生成一个可执行的脚本。
```
#! /usr/bin/env racket
```
然后在命令行中执行chmod +x <源码文件名>改变文件权限。

只要racket在用户的可执行程序的搜索路径中，这个脚本就可以执行。或者在#!后（#!和路径之间有一个空格）使用racket的完整路径，这样的话racket是否在可执行程序的搜索路径中就无所谓了。

### 1.4对于有Lisp/Scheme经验读者的一些注意事项



