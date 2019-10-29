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
如果你已经有一些Scheme或者Lisp的经验，你可能会把
```
(define (extract str)
    (substring str 4 7))
```
写到“extract.rktl”,然后这样运行racket
```
> (load "extrack.rktl")
> (extract "the dog out")
"dog"
```
没错，这可以运行，因为racket会模拟一个传统意义上的lisp环境。
但是我们强烈建议你不要在模块外加载([load](https://docs.racket-lang.org/reference/eval.html#%28def._%28%28quote._~23~25kernel%29._load%29%29))或写程序。

在模块外写定义将导致错误、性能下降和一个难以维护和运行程序。这些问题并不是racket特有的。这是传统顶层环境（top-level environment）的
限制，Scheme和Lisp都曾在特殊的命令行标记、编译指令、构建工具上面纠结过。模块系统被设计用来避免这些问题，所以从长远看来，以#lang开始你会更加喜欢racket。

## Racket要素
本章将简单介绍作为本指南其他章节的背景知识。有Racket经验的读者可以放心的跳至[Built-In Datatypes](https://docs.racket-lang.org/guide/datatypes.html)。

### 2.1简单值
Racket的值包括数字值，布尔值，字符串值，字节串值。在DrRacket和文档的例子中，值表达式会显示为绿色。

数字以通常的形式被书写，并包含分数和虚数：
```
1       3.14
1/2     6.02e+23
1+2i    9999999999999999999999
```
布尔值用#t代替true，用#f代替false。然而，一般而言都会把非#f的值看成true。

字符串被写在双引号之间。当写一个字符串时，反斜杠充当转义字符。例如，反斜杠后面跟双引号在字符串中充当字面量的双引号。除了非转义的双引号和反斜杠外，任何其他Unicode字符都可以出现在字符串常量中。

```
"Hello, world!"
"Benjamin \"Bugsy\" Siegel"
"λx:(μα.α→α).xx"
```
当一个常量在REPL中被执行时，通常打印和输入语法相同的内容。在某些情况下，打印的形式是输入语法的规范化版本。在文档和DrRacket的REPL中，执行结果用蓝色而不是绿色打印，用以区分这是一个结果而不是一个常量表达式。
```
> 1.0000
1.0
> "Bugs \u0022Figaro\u0022 Bunny"
"Bugs \"Figaro\" Bunny"
```

### 2.2定义和表达式
程序模块会这样写
```
#lang <langname> <topform>*
```

\<topform\>是一个<定义>或者<表达式>。REPL会执行 \<topform\>s。

在语法规范中文本会是灰色背景表示（如#lang）文本。空格必须分割在这些文本和非终结符中，比如<标识符>。但是空格不需要出现在‘(’ , ‘)’ , ‘[’ , ‘]’的之前或之后。注释通常以;开始直到本行结束截止，并会被看成空白。

遵循惯例，* 在语法中指前一个元素的零次或多次重复，+ 指前一个元素的一次或多次重复，{} 将序列作为重复元素分组。

#### 2.2.1定义
定义的形式如下
```
( define <标识符> <表达式> )
```
把<标识符>绑定到<表达式>，而
```
( define (<标识符> <标识符>*) <表达式>+ )
```
把地一个<标识符>绑定到一个函数，该函数以<标识符>*为参数。在这个例子中，<表达式>+是该函数的函数体。当该函数被调用，返回最后一个表达式的结果。

例如:
```
(define pie 3)              ; defines pie to be 3
(define (piece str)         ; defines piece as a function
    (substring str 0 pie))  ;   of one argument

> pie
3
> (piece "key lime")
"key"
```
实际上，方法定义和非方法定义一样，并且方法名不一定得用于方法调用。方法只是另一种形式的值，尽管打印出来的形式和数字或者字符串比不太完整。

例如:
```
> piece
#<procedure:piece>
> substring
#<procedure:substring>
```

#### 2.2.2代码缩进
换行和缩进对于解析Racket程序并不重要，但是大部分Racket程序员使用一套标准的规则是代码更可读。例如，方法体通常在定义的下面一行进行缩进。标识符紧跟在左括号后（中间没有空格），右括号永远不在他们自己的那行。











