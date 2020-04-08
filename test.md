# Racket 指南

该教程是为 Racket 新手或对 Racket 某些部分不熟悉的人准备的。然而，它假设你已有编程经验，所以如果你是编程新手，可以考虑先看 How to Design Programs。如果你只想快速浏览 Racket 介绍，请点击 Quick: An Introduction to Racket with Pictures。

本指南第2章是 Racket 简单介绍。从第3章开始，将详细介绍Racket工具箱的大部分内容，其他细节部分请参考 The Racket Reference 和其他参考手册。

## 1 欢迎来到 Racket

取决于你如何看待它，**Racket** 可以是
- 一个编程语言 - 一个lisp方言，一个Scheme分支
- 一系列语言 - Racket的变体
- 一套工具 - 用于使用一系列语言

没有特别说明的话，我们只是简单使用 racket 语言本身。

racket的主要工具包括:
- **racket**， 核心编译器，解释器和运行时的系统
- **DrRacket**， 编程环境
- **raco**， 用于执行racket命令来安装第三方包、构建库等操作的命令行工具。

通常，特别是在刚开始的时候，你会通过 DrRacket 学习 racket。如果你愿意，你也可以通过命令行和你熟悉的文本编辑器来运行。详见Command-Line Tools and Your Editor of Choice。本指南其他语言相关的部分将和你选择的编辑器无关。

如果你使用了 DrRacket，需要选择合适的语言，因为 DrRacket 提供了很多不同的 Racket 变体和其他语言。假设你之前从未使用过DrRacket，首先启动它，在文本区的顶部文本区输入

```
#lang racket
```

然后点击文本区上方的运行按钮。DrRacket 会明白你想要在典型的 Racket 变体中运行(这与更小一点的 racket/base 或很多其他选项不同)。

如果你之前使用过 DrRacket，它会记住你上次使用的语言，而不是从 #lang 行推断。选择 **Language|Choose Language** 菜单，将会出现一个对话框，选择第一项，这会告诉 DrRacket 通过 #lang 在源码中选择所选的语言。这仍然会把 #lang 放在文本区的顶部。

### 1.1与 Racket 交互

DrRacket 下方的文本区和 racket 命令行(无参数启动)都在扮演一个计算器的角色。你输入一个 racket 表达式，回车，结果便会被打印出来。在 racket 的术语中被称为 *read-eval-print loop* 或简称为REPL。

数字本身就是表达式，结果也仅仅是这个数字：

```
> 5
5
```

字符串仍然是自运行的表达式。字符串表达式被写为首尾均是双引号的字符串：

```
> "Hello, world!"
"Hello, world!"
```

racket 用圆括号包围长表达式（几乎除了简单的常量以外的任何表达式）。比如，函数调用被写为：左圆括号，函数名，参数表达式，右圆括号。下面的表达式用参数 "the boy out of the cpuntry"， 4， 7 调用内置的方法 substring：

```
> (substring "the boy out of the country" 4 7)
"boy"
```

### 1.2定义和交互

你可以通过 define 定义你自己的类似 substring 的函数，就像这样：

```
（define (extract str)
    (substring str 4 7))

> (extract "the boy out of country")
"boy"
> (extract "the country out of the boy")
"cou"
```

尽管你可以在 REPL 中执行这个 define 形式，但是这些 define 形式通常是程序的一部分，你可能希望保存它们，并在之后使用。所以在 DrRacket 中，你通常都会把这些 define 放在文本区上方的定义区（和 #lang 放在一起）。

```
#lang racket
(define (extract str)
    (substring str 4 7))
```

如果(extract "the boy")是程序的一部分，你也可以在定义区中定义。但是如果你仅仅想运行一下 extract 的例子，那么你可能更偏向于不在定义区运行，而是点击 Run 按钮，并且在REPL中执行(extract "the boy")。

当你用 racket 命令行代替 DrRacket 时，你可以用你喜欢的编辑器并把上面的文本保存进去。例如保存进“extract.rkt”，并在与该文件相同目录执行 racket，执行下面的操作：

```
> (enter! "extract.rkt")
> (extract "the gal out of the city")
"gal"
```

enter! 会加载源码并且在模块中切换运行环境，就像在 DrRacket 中点击 Run 按钮一样。

### 1.3创建可执行程序

如果你的文件（或者DrRacket定义区）包含如下内容

```
#lang racket

(define (extract str)
    (substring str 4 7))

(extract "the cat out of the bag")
```

那么它就是一个在运行是能打印“cat”的完整程序。你可以使用 DrRacket 或者在 racket 中用 enter! 运行这个程序，但是如果这段源码被保存进<源码文件>，也可以在命令行中这样运行它

```
racket <src-filename>
```

这里给出一些把代码打包成可执行程序的建议：

- 在DrRacket中，你可以选择 **Racket|Create Executable**...选项
- 在命令行中，运行raco exe <*src-filename*>。查看 raco exe: Creating Stand-Alone Executables 了解更多信息。
- 在Unix 或 Mac OS中，你可以通过在代码文件开始的地方插入下面的代码来来生成可执行的脚本。

```
#! /usr/bin/env racket
```

然后在命令行中执行 chmod +x <*src-filename*> 改变文件权限。

只要 racket 在用户的可执行程序的搜索路径中，这个脚本就可以执行。或者在 #! 后（#!和路径之间有一个空格）使用 racket 的完整路径，这样racket是否在可执行程序的搜索路径中就无所谓了。

### 1.4对于有 Lisp/Scheme 经验的读者的一些注意事项

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

没错，这可以运行。因为 racket 会模拟一个传统意义上的 lisp 环境，但是我们强烈建议你不要在模块外使用 load 或写程序。

在模块外进行定义将导致错误、性能下降和一个难以维护和运行程序。这些问题并不是 racket 特有的。这是传统顶层环境（top-level environment）的限制，Scheme 和 Lisp 都曾在特殊的命令行标记、编译指令、构建工具上面纠结过。模块系统被设计出来避免这些问题，所以长远地看来，以 #lang 开始你会更加愉快的使用 racket。

## 2 Racket 要点

本章将简单介绍作为本指南其他章节的背景知识。有Racket经验的读者可以放心的跳至 Built-In Datatypes。

### 2.1简单值（Simple Value）

racket的值包括数字，布尔值，字符串，字节串。在 DrRacket 和文档的例子中，值表达式会显示为绿色。

数字以直观的形式被书写，并且包含分数和虚数：

```
1       3.14
1/2     6.02e+23
1+2i    9999999999999999999999
```

布尔值用 #t 代替 true，用 #f 代替 false。然而，一般而言都会把非 #f 的值看成 true。

字符串被写在双引号之间。对于字符串，反斜杠充当转义字符。例如，反斜杠后跟双引号充当字面量的双引号。除了未转义的双引号和反斜杠外，任何其他 Unicode 字符都可以出现在字符串中。

```
"Hello, world!"
"Benjamin \"Bugsy\" Siegel"
"λx:(μα.α→α).xx"
```

当一个常量在REPL中被执行时，通常打印和输入语法相同的内容。在某些情况下，被打印的形式是输入语法的规范化版本。在文档和 DrRacket 的 REPL 中，被打印的结果用蓝色而不是绿色打印，用以区分这是一个结果而不是一个常量表达式。

例如：

```
> 1.0000
1.0
> "Bugs \u0022Figaro\u0022 Bunny"
"Bugs \"Figaro\" Bunny"
```

### 2.2定义和表达式

模块可以这样写：

```
#lang <langname> <topform>*
```

<*topform*>是一个 定义 或者 表达式。REPL会执行 <*topform*>*。

在语法规范中，灰色背景的文本（如 #lang）表示字面量文本。空格必须分割在这些文本和非终结符中，比如 <*id*>。但是空格不需要出现在 ‘(’ , ‘)’ , ‘[’ , ‘]’ 的之前或之后。注释通常以 ; 开始直到本行结束截止，并且会被看成空白。

约定俗称地，* 在语法中指前一个元素的零次或多次重复，+ 指前一个元素的一次或多次重复，{} 将序列中重复元素进行分组。

#### 2.2.1定义

定义的形式如下：

```
( define <id> <expr> )
```

把 <*id*> 绑定到 <*expr*>。

```
( define ( <id> <id>*) <expr>+ )
```

把第一个 <*id*> 绑定到一个函数（也被成为过程（procedure））并命名，该函数以剩余的 <*id*>+ 为参数。在这个例子中，<*expr*>+ 是该函数的函数体。当该函数被调用，返回最后的 <*expr*> 结果。

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

事实上，函数定义和非函数定义一样，并且函数名不一定用于函数调用。函数只是另一种类型的值，尽管打印出来的形式和数字或者字符串比不太完整。

例如:

```
> piece
#<procedure:piece>
> substring
#<procedure:substring>
```

函数定义的函数体可以包含多个表达式。在这种情况下，当函数调用时，只有最后一个表达式的值被返回。其他表达式的执行，仅仅为了它们的副作用，比如打印。

例子

```
(define (bake flavor)
    (pringf "preheating oven...\n")
    (string-append flovr " pie"))
```

racket 程序更倾向于避免副作用，所以函数体中通常只有一个表达式。虽然这很重要，但是我们仍要理解函数体中是允许多表达式的，因为它可以解释下面你的 nobake 函数为什么会失败：

```
(define (nobake flavor)
    string-append flavor "hello")

> (bobake "green")
"jello"
```

函数nobake中，没有用圆括号将 string-append flavor "jello" 扩起来，所以他们是三个独立的表达式而不是一个函数调用。表达式 string-append 和 flavor 被执行了，但是他们的结果未被使用。函数的结果是最后一个表达式的结果：“jello”。

#### 2.2.2代码缩进

换行和缩进对于解析 racket 程序并不重要，但是大部分 racket 程序员会使用一套标准规则使代码更可读。例如，方法体通常在定义的下一行进行缩进。标识符紧跟在没有额外空格的左括号后，右括号永远不在他们自己的那行。

当我们在 DrRacket 中输入代码或者在REPL表达式时，DrRacket 会根据标准格式自动进行缩进。例如，当你在输入(define (greet name)后敲回车，DrRacket 会自动在下一行插入两个空格。如果更改了代码的位置，可以在 DrRacket 选中代码并按 Tab 键，那样DrRacket 会重新缩进代码（不插入任何换行符）。像Emacs这样的编辑器提供 Racket 或者 Scheme 模式来智能缩进。

重新缩进不仅使得代码易于阅读，还可以提示括号是否按想要的方式匹配。例如，如果你在函数的最后一个参数后少写了一个右圆括号，自动缩进会在第一个参数的下一行开始，而不是在 define 关键字的下面：

```
(define (halbake flavor
                 (string-append flavor " creme brulee")))
```

对于这个例子，缩进会使错误更加明显。在其他例子中，当一个左圆括号没有一个与之匹配的右圆括号时，缩进会正常。当圆括号缺失时，racket 和 DrRacket 都会通过源码缩进显示。

#### 2.2.3标识符

racket 的标识符语法很自由。除了特殊字符

```
( ) [ ] { } " , ' ` ; # | \
```

和构成数字常量字符序列，几乎任何不含有空白字符的形式都可以是一个 <*id*> （标识符）。例如， subtring 是一个标识符。同样 string-append 和 a+b 也是标识符，而不是算术表达式。下面还有几个例子：

```
+
Hfuhruhurr
integer?
pass/fail
john-jacob-jingleheimer-schmidt
a-b-c+1-2-3
```

#### 2.2.4 函数调用（过程应用（Procedure Applications））

我们已经见过很多函数调用了，在更传统一点的术语中，被称作过程应用。函数调用的语法是：

```
( <id> <expr>* )
```

<*expr*>* 的数量决定应用到名为 <*id*> 的函数的参数数量。

racket内置了很多函数标识符，如 substring、string-append。后面我们会见到很多的例子。

文档中的 racket 代码示例中，使用的内置名称会链接到参考手册。你可以点击并查看它的使用细节。

```
> (string-append "rope" "twine" "yarn")  ; append strings
"ropetwineyarn"
> (substring "corduroys" 0 4)            ; extract a substring
"cord"
> (string-length "shoelace")             ; get a string's length
8
> (string? "Ceci n'est pas une string.") ; recognize strings
#t
> (string? 1)
#f
> (sqrt 16)                              ; find a square root
4
> (sqrt -16)
0+4i
> (+ 1 2)                                ; add numbers
3
> (- 2 1)                                ; subtract numbers
1
> (< 2 1)                                ; compare numbers
#f
> (>= 2 1)
#t
> (number? "c'est une number")           ; recognize numbers
#f
> (number? 1)
#t
> (equal? 6 "half dozen")                ; compare anything
#f
> (equal? 6 6)
#t
> (equal? "half dozen" "half dozen")
#t
```

#### 2.2.5条件判断(if、and、or和cond)

最简单的一种条件判断是if

```
( if <expr> <expr> <expr> )
```

第一个<*expr*>总是会被执行。如果它执行出一个非 #f 值，第二个<*expr*>将被执行，并把该值作为整个 if 表达式的值返回，否则第三个<*expr*>执行并返回结果值。

例如：

```
(if (> 2 3)
    "bigger"
    "smaller")
"smaller"

(define (reply s)
    (if (equal? "hello" (substring s 0 5))
    "hi!"
    "huh?"))

> (reply "hello racket")
"hi!"
> (reply "λx:(μα.α→α).xx")
"huh?"
```

复杂的条件判断可以通过嵌套if表达式来实现。例如，可以使reply函数处理非字符串的输入：

```
(define (reply s)
    (if (string? s)
        (if (equal? "hello" (substring s 0 5))
            "hi!"
            "huh?)
        "huh?"))
```

避免出现两次“huh？”，更好的书写方式为

```
(define (reply s)
    (if (if (string? s)
            (equal? "hello" (substring s 0 5))
            #f)
        "hi!"
        "huh?"))
```

不过这种嵌套if的方式难以阅读。racket 提供可用于任意表达式数量的and和or形式使代码更可读：

```
(and <expr>* )
(or <expr>* )
```

and 截断：当有表达式返回#f时，and 会停止并返回 #f，否则它会继续执行。or 会在碰到一个真值时出现截断。

例如：

```
(define (reply s)
    (if (and (string? s)
             (>= (string-length s) 5
             (equal? "hello" (substring s 0 5)))
        "hi"
        "huh?"))

> (reply "hello racket")
"hi!"
> (reply 17)
"huh?"
```

另一个常见的 if 嵌套是提供一系列测试，每个都对应一个结果：

```
(define (reply-more s)
    (if (equal? "hello" (substring s 0 5))
        "hi!"
        (if (equal? "goodbye" (substring s 0 7))
            "bye"
            (if (equal? "?" (substring s (- (string-length s) 1)))
                "I don't know"
                "huh?"))))
```

一系列测试的简化版是 cond 形式：

```
( cond {[ <expr> <expr>* ]}* )
```

cond 形式使用方括号包裹一些列子句。每个子句中的第一个 <*expr*> 是测试表达式。如果返回 true，那么剩下的 <*expr*>* 将被执行，其中最后一个表达式执行结果将作为整个 cond 表达式的结果；并且剩下的子句将会被忽略。如果 <*expr*> 返回#f，那么这个表达式剩下的 <*expr*>* 将会被忽略，继续执行下一条子句。最后一个子句可以使用 else (它的作用和#t一样)作为测试表达式。

如果使用 cond，上面的 reply-more 函数可以被更简洁的写成下面的形式：

```
(define (reply-more s)
    (cond
        [(equal? "hello" (substring s 0 5)) "hi!"]
        [(equal? "goodbye" (substring s 0 7)) "bye!"]
        [(equal? "?" (substring s (- (string-length s) 1))) "I don't know"]
        [else "huh?"]))

> (reply-more "hello racket")
"hi!"
> (reply-more "goodbye cruel world")
"bye!"
> (reply-more "what is your favorite color?")
"I don't know"
> (reply-more "mine is lime green")
"huh?"
```

在 cond 中使用方括号只是一种惯例。在racket中方括号和圆括号实际上是可互换的，只要 ( 匹配 ) ， [ 匹配 ] 就可以。在一些关键地方使用方括号会使得代码更可读一点。

#### 2.2.6又是函数调用

在我们之前函数调用的程序中，我们介绍得过于简单了。函数调用实际上可以允许任意表达式，而不只是一个<*id*>：

```
( <expr> <expr>* )
```

第一个 <*expr*> 通常是 <*id*>，比如 string-append 或者 +，但是它可以是任何一个可执行的函数。例如，可以是一个条件表达式：

```
(define (bouble v)
    ((if (string? v) string-append +) v v))

> (double "mnah")
"mnahmnah"

> (double 5)
10
```

从语法上看，第一个表达式甚至可以是一个数字，但是这个会出错，因为数字不是一个函数。

```
> (1 2 3 4)
application: not a procedure;
 expected a procedure that can be applied to arguments
  given: 1
  arguments...:
   2
   3
   4
```

当你省略了函数名或者执行表达式使用额外的圆括号时，通常你会碰到一个 “expected a procedure” 错误

#### 2.2.7 lambda匿名函数

如果为每个数字都起一个名字，racket程序将会特别枯燥。比如一个一个(+ 1 2)，你得这样写：

```
> (define a 1)
> (define b 2)
> (+ a b)
3
```

同理，给所有函数命名通常也会非常枯燥。比如，定义一个函数twice，它接受一个函数和一个参数。如果你已经有一个函数，执行twice会很方便，比如 sqrt:

```
(define (twice f v)
    (f (f v)))

> (twice sqrt 16)
2
```

如果你想调用一个还未定义的函数，你也可以定义它，然后传给twice：

```
(define (louder s)
    (string-append s "!"))

> (twice louder "hello")
"hello!!"
```

但是，如果 louder 只在 twice 这一个地方调用，一个完整的定义会使人十分难受。在 racket 中，你可以使用一个 lambda 表达式直接提供一个函数。lambda 表达式使用标识符作为函数的参数，使用表达式作为函数体：

```
(lambda ( <id>* ) <expr>+ )
```

lambda 自身作为一个函数执行：

```
> (lambda (s) (string-append s "!"))
#<procedure>
```

使用 lambda 重写上面的 twice 可以这样：

```
> (twice (lambda (s) (string-append s "!"))
         "hello")
"hello!!"
> (twice (lambda (s) (string-append s "?!"))
         "hello")
"hello?!?!"
```

另一个使用 lambda 的方式是作为函数的结果来生成函数：

```
(define (make-add-suffix s2)
    (lambda (s) (string-append s s2)))

> (twice (make-add-suffix "!") "hello")
"hello!!"
> (twice (make-add-suffix "?!") "hello")
"hello?!?!"
> (twice (make-add-suffix "...") "hello")
"hello......"
```

racket 是一个基于词法作用域的语言，这表示 make-add-suffix 返回的函数中的s2，在调用这个函数时，总是引用该参数。换句话说，lambda生成的函数记住了 s2：

```
> (define louder (make-add-suffix "!"))
> (define less-sure (make-add-suffix "?"))
> (stice less-sure "really")
"really??"
>> (twice louder "really")
"really!!"
```

到目前为止，我们已经在非函数定义中提到过这种定义 (define <*id*> <*expr*>)。这种描述具有误导性，因为 <*expr*> 可以是一个lambda 形式，这种情况下，这种定义相当于是一这个 “function” 定义的形式。例如，下面两种形式是一样的：

```
(define (louder s)
    (string-append s "!"))

(define louder
    (lambda (s)
        (string-append s "!")))

> louder
#<procedure:louder>
```

请注意，第二个 louder 函数是一个用 lambda 写的匿名函数，但是，编译器会尽可能推断一个名称使打印和错误报告更有意义和更具可读性。

#### 2.2.8局部绑定 define， let， let*

是时候介绍racket语法的另一个简化了。在函数体中，可以在函数体表达式之前进行定义:

```
(define ( <id> <id>* ) <definition>* <expr>+ )
(lambda ( <id>* ) <definition>* <expr>+ )
```

函数体开始处的定义，只对该函数体生效。

例如：

```
(define (converse s)
    (define (starts? s2) ; local to converse
        (define len2 (string-length s2)) ; local to starts?
        (and (>= (string-length s) len2)
             (equal? s2 (substring s 0 len2))))
    (cond 
     [(starts? "hello") "hi!"]
     [(starts? "goodbye") "bye!"]
     [else "huh?"]))

> (converse "hello!")
"hi!"
> (converse "urp")
"huh?"

> starts? ; outside of converse, so...
starts?: undefined;
 cannot reference an identifier before its definition
  in module: top-level
```

另一个创建本地绑定的方式是 let。let 的好处是它可以被使用在任何表达式使用的地方。另外，let 可以一次性绑定多个标识符，而不必像 define 一样为每个标识符单独绑定。

```
( let ([ <id> <expr>]* <expr>+) )
```

每一个绑定子句是一个被放括号包围的 <*id*> 和 <*expr*>，子句后面的那些表达式是 let 体。在 let 体里，每一个子句中的 <*id*> 会被被绑定成<*expr*>的结果。

```
> (let ([x (random 4)]
        [o (random 4)])
    (cond
        [(> x o) "X wins"]
        [(> o x) "O wins"]
        [else "cat's game"]))
 "cat's game"
```

let 形式的绑定只能在它的执行体中其作用，所以它的子句中的绑定不能相互引用。然后，let* 形式允许后面的子句引用前面子句的绑定：

```
> (let* ([x (random 4)]
         [o (random 4)]
         [diff (number->string (abs (- x o)))])
    (cond 
     [(> x o) (string-append "X wins by " diff)]
     [(> o x) (string-append "O wins by " diff)]
     [else "cat's game"]))
 "O wins by 1"
```

### 2.3列表，迭代和递归

racket 是 lisp （LISt Processor 列表处理器）语言的一个方言。所以内置的列表数据结构仍然是该语言的突出特性。

函数 list 可以传入任何数量的值，返回一个包含这些值的列表：

```
> (list "red" "green" "blue")
'("red" "green" "blue")
> (list 1 2 3 4 5)
'(1 2 3 4 5)
```

正如你看到的，列表的结果在 REPL 中以 ' 的方式被打印出来，并且用一对圆括号包围元素列表。这可能是个让人迷惑的地方，因为圆括号被同时用于表达式，比如 (list "red" "green" "blue") 和被打印的结果 比如'("red" "green" "blue")。除了单引号外，表示结果的圆括号在文档和DrRacket中都用蓝色显示，而用于表达式的圆括号用棕色显示。

许多预定义的函数用来操作列表。下面举一些列子：

```
> (length (list "hop" "skip" "jump")) ; count the elements
3
> (list-ref (list "hop" "skip" "jump") 0) ;extract by position
"hop"
> (list-ref (list "hop" "skip" "jump") 1)
"skip"
> (append (list "hop" "skip") (list "jump")) ;combine lists
'("hop" "skip" "jump")
> (reverse (list "hop" "skip" "jump")) ;reverse order
'("jump" "skip" "hop")
> (member "fall" (list "hop" "skip" "jump")) ; check for an element
#f
```

#### 2.3.1内置的列表迭代（Predefined List Loops）

除了像 append 这样的简单操作，racket还提供了许多方法用来迭代列表中的元素。这些迭代方法和 java、racket 或者其他语言中的 for 一样。racket 的迭代体被封装进函数用来依次应用到每个元素，所以 lambda 和迭代函数结合使用将十分方便。

不同的迭代函数以不同的方式组合迭代结果。函数 map 使用每个元素的结果产生一个新的列表：

```
> (map sqrt (list 1 4 9 16))
'(1 2 3 4)
> (map (lambda (i)
            (string-append i "!"))
        (list "peanuts" "popcorn" "crackerjack"))
'("peanuts" "popcorn" "crackerjack")
```

andmap 和 ormap 会调用 and 或 or 来组合产生的结果。

```
> (andmap string? (list "a" "b" "c"))
#t
> (andmap string? (list "a" "b" 6))
#f
> (ormap number? (list "a" "b" 6))
#t
```

函数 map、andmap 和 ormap 不仅可以处理单个列表也可以同时处理多个列表。被处理的表达式必须长度相同，接受的函数从每个列表中取一个元素作为参数：

```
(map (lambda (s n) (substring s 0 n))
     (list "peanuts" "popcorn" "crackerjack")
     (list 6 3 7))
'("peanut" "pop" "cracker")
```

函数 filter 会保留迭代体执行后返回真的元素，丢弃返回 #f 的元素:

```
> (filter string? (list "a" "b" 6))
'("a" "b")
> (filter positive? (list 1 -2 6 7 0))
'(1 6 7)
```

函数 foldl 扩充了一些迭代函数。它使用一个函数处理一个元素并把该元素和当前值组合，所以这个函数需要有一个额外的初始值。此外初始值必须在列表之前就被提供：

```
> (foldl (lambda (elem v)
            (+ v (* elem elem)))
         0
         '(1 2 3))
14
```

尽管它的场景很普遍，但是依然没有像其他函数那样被频繁使用。其中一个原因是 map、ormap、andmap、filter涵盖了列表处理的大部分场景。

racket还提供了一个通用的列表处理形式 for/list，它通过遍历一个序列来构建一个列表。列表和相关迭代形式将会在 Iterations and Comprehensions 中介绍。

#### 2.3.2从头开始进行列表迭代（List Iteration from Scratch）

尽管 map 和其他迭代方法都预先内置了，但是它们都不是原始的方式。你也可以通过使用列表原语来生成等效的迭代。

因为 racket 的列表是一个链表，所以在一个非空的列表中，两个核心的是

- first：获取列表中的第一个元素
- rest：获取列表中剩余的元素

比如：

```
> (first (list 1 2 3))
1
> (rest (list 1 2 3))
'(2 3)
```

 在链表添加一个元素，可以使用 cons （“construct”的缩写）把元素添加到列表的前面。如果是空列表，可以使用常量 empty。

```
> empty
'()
> (cons "head" empty)
'("head")
> (cons "dead" (cons "head" empty))
'("dead" "head")
```

因为 first 和  rest 只能处理非空列表，为了处理列表，我们需要能区分空列表和非空列表。函数 empty? 用来区分空列表，cons? 用来区分非空列表。

```
> (empty? empty)
#t
> (empty? (cons "head" empty))
#f
> (cons? empty)
#f
> (cons? (cons "head" empty))
#t
```

有了以上这些，你可以写出自己的 length、map 函数，以及更多的其他函数。

比如：

```
(define (my-length lst)
    (cond
        [(empty? lst) 0]
        [else (+ 1 (my-length (rest lst)))]))

> (my-length empty)
0
> (my-length (list "a" "b" "c"))
3
```

```
(define (my-map f lst)
    (cond
        [(empty? lst) empty]
        [else (cons (f (first lst))
                    (my-map (rest lst)))]))

> (my-map string-upcase (list "ready" "set" "go"))
'("READY" "SET" "GO")
```

如果上面的定义对你来说比较难懂，考虑阅读 How to Design Programs。但如果你仅仅疑惑用递归代替迭代的用法，请继续阅读。

#### 2.3.3尾递归

my-length 和 my-map 都运行在O(n)的算法空间上（n是列表长度）。想象一下(my-length (list "a" "b" "c"))怎样运行就很容易知道。

```
(my-length (list "a" "b" "c"))
= (+ 1 (my-length (list "b" "c")))
= (+ 1 (+ 1 (my-length (list "c"))))
= (+ 1 (+ 1 (+ 1 (my-length (list)))))
= (+ 1 (+ 1 (+ 1 0)))
= (+ 1 (+ 1 1))
= (+ 1 2)
= 3
```

对于有n个元素的列表，运行时会堆叠n个(+ 1 ...)，最后当列表迭代完再把他们累加起来。

你可以通过一路累加结果来避免堆叠。我们需要一个函数（它接受一个列表和一个当前积累的长度）来积累长度。下面的代码使用一个局部函数，它用一个参数len表示当前积累的长度：

```
(define (my-length lst)
    ; local function iter
    (define (iter lst len)
        (cond
            [(empty? lst) len]
            [else (iter (rest lst) (+ lenn 1))]))
    ; body of my-length calls iter
    (iter lst 0))
```

运行起来就像这样：

```
(my-length (list "a" "b" "c"))
= (iter (list "a" "b" "c") 0)
= (iter (list "b" "c") 1)
= (iter (list "c") 2)
= (iter (list) 3)
3
```

按照我们建议的运行步骤，改进过的 my-length 将在常量级的空间中运行。这是因为，当一个函数运行的结果（比如 (iter (list "b" "c") 1)）正好是其他函数结果中所需要的（比如(iter (list "c") 2)），第一个函数不必等待第二个函数执行完，因为那样占用空间毫无意义。

这样的执行行为被成为尾调用优化。但是在 racket 中它不仅仅是优化，它是代码运行方式的保障。更准确地说，相对于其他表达式，尾递归的表达式不会占用额外的空间。

对于my-map，O(n)空间复杂度是可以接受的，因为它生成一个O(n)长度的列表。尽管如此，你依然可以通过累积结果列表来减少常量因子。唯一的问题是累计的结果列表是向后添加的，所以在最后你需要反转它。

```
(define (my-map f lst)
    (define (iter lst backward-result)
        (cond
            [(empty? lst) (reverse backward-result)]
            [else (iter (rest lst)
                        (cons (f (first lst))
                        backward-result))]))
    (iter lst empty))
```

如果你这样写

```
(define (my-map f lst)
    (for/list ([i lst])
        (f i)))
```

for/list 会被扩展成和 iter 基本一样的定义和使用。区别仅仅是语法上的便利。

#### 2.3.4递归vs迭代

my-length 和 my-map 表明迭代只是一种特殊形式的递归。在许多语言中，在迭代形式里进行尽可能多的计算很重要。然而，性能会很差，并且导致栈溢出。类似地，在racket中，当在常量空间中很容易运行的计算，应当使用尾递归避免O(n)的空间占用是很重要的。

同时，在racket中递归不会导致特别糟糕的性能问题，也没有像栈溢出一样的情况；如果计算包含特别多的上下文，你可以用尽所有的内存，但是耗尽内存的递归级数通常要比其他语言触发栈溢出时多。这些事项，以及尾递归程序实际上自动以循环的方式运行，使得racket程序员接受递归的形式，而不是避免递归。

例如，你想要移除列表中的连续重复项。这样的函数，可以被写成按循环的方式，每次迭代时记住前一次的元素，racket程序员会喜欢写成如下的形式：

```
(define (remove-dups l)
    (cond
        [(empty? l) empty]
        [(empty? (rest l)) l]
        [else
            (let ([i (first l)])
                (if (equal? i (first (rest l)))
                    (remove-dups (rest l))
                    (cons i (remove-dups (rest l)))))]))

> (remove-dups (list "a" "b" "b" "b" "c" "c"))
'("a" "b" "c")
```

一般来说，用一个长度为n的列表输入，这个方法消耗O(n)空间的内存，但是这没问题，因为它生成一个O(n)的结果。如果输入列表碰巧大部分是连续的重复项，那么结果列表远小于O(n)，remove-dups也将会使用比O(n)小得多的空间！原因在于，当函数丢弃重复项时，它直接返回remove-dup调用的结果，因此为递归优化生效了：

```
(remove-dups (list "a" "b" "b" "b" "b" "b"))
= (cons "a" (remove-dups (list "b" "b" "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b")))
= (cons "a" (remove-dups (list "b")))
= (cons "a" (list "b"))
= (list "a" "b")
```

### 2.4 序对、列表和racket语法

cons 函数实际上接收两个值，而第二个参数并不仅仅是列表。当第二个参数不是empty或者不是 cons 生成的值，结果会以特殊的方式打印。用 cons 连接的两个值被打印成在括号之间，但是之间有一个点（被空格包围的句号）：

```
> (cons 1 2)
'(1 . 2)
> (cons "banana" "split")
'("banana" . "split")
```

因此，被 cons 生成的值并非总是列表。一般来说，cons 的结果是一个序对。对 cons? 来说，更传统的函数名是 pair?，从现在开始我们将使用传统的名字。

rest 对非列表序对也没什么作用；对于 first 和 rest 更传统的名字分别是 car 和 cdr。（当然，传统的名字也是胡说八道，只需要知道 “a” 在 “d” 前面，cdr的发音为“could-er”）

比如

```
> (car (cons 1 2))
1
> (cdr (cons 1 2))
2
> (pairs? empty)
#f
> (pairs? (cons 1 2))
#t
> (pairs? (list 1 2 3))
#t
```

racket 的序对数据类型，以及它和列表的关系，包括点的打印和有趣的名字（car 和 cdr）是一个历史问题。然而序对和 racket 的文化、规范、实现紧密相连，这是它们得以保存的原因。

当你出错时，大部分原因可能是你遇到了非列表序对的问题，比如颠倒了 cons 的参数：

```
> (cons (list 2 3) 1)
'((2 3) . 1)
> (cons 1 (list 2 3))
'(1 2 3)
```

非列表序对有时故意被使用。比如，函数 make-hash 使用一个序对的列表，car 对于每个序对来是是key，cdr 则是value。

对于racket新手来说，唯一一件比非列表序对更加困惑的事是：一个序对（但是它的第二个参数是序对而不是列表）的打印方式。

```
> (cons 0 (cons 1 2))
'(0 1 . 2)
```

通常，打印序对的方式是这样的：用点表示，除非点后面紧跟一个左圆括号。如果是这种情况，移除点、左圆括号然后匹配右圆括号。因此'(0 . (1 . 2))变成'(0 1 . 2)，'(1 . (2 . (3 . ()))变成'(1 2 3)。

#### 2.4.1引用序对和带引号的符号（Quoting Pairs and Symbols with quote）

列表被打印出来会带有引号，但是如果列表中的一个元素是列表，内部的列表打出出来不带引号：

```
> (list (list 1) (list 2 3) (list 4))
'((1) (2 3) (4))
```

对于嵌套列表，quote 形式使得你将列表作为表达式书写本质上和打印这个列表一致：

```
> (quote ("red" "green" "blue"))
'("red" "green" "blue")
> (quote ((1) (2 3) (4)))
'((1) (2 3) (4))
> (quote ())
'()
```

quote 也可用使用点表示法，不管要表达的形式是不是点-圆括号消除规则：

```
> (quote (1 . 2))
'(1 . 2)
> (quote (0 . (1 . 2)))
'(0 1 . 2)
```

任何类型的列表都可以嵌套：

```
> (list (list 1 2 3) 5 (list "a" "b" "c"))
'((1 2 3) 5 ("a" "b" "c"))
> (quote ((1 2 3) 5 ("a" "b" "c")))
'((1 2 3) 5 ("a" "b" "c"))
```

如果你对一个标识符使用 quote，那么你得到一个类似标识符的输出，但是有一个 ' 前缀：

```
> (quote jane-doe)
'jane-doe
```

一个像这样被引用的标识符是一个符号。和带圆括号的输出不应该和表达式混淆一样，符号不应该和标识符混淆。特别是符号与标识符映射和预定义的函数映射无关，符号只是碰巧和标识符同名而已。

的确，符号的值除了字面外没有任何意义。这样的话，符号和字符串几乎一样，最大的区别只是打印的不同。函数 symbol->string 和 string->symbol 用来相互转换它们。

比如:

```
> map
#<procedure:map>
> (quote map)
'map
> (symbol? (quote map))
#t
> (symbol? (map))
#f
> (procedure? map)
#t
> (string->symbol "map")
'map
> (symbol->string (quote map))
"map"
```

和作用于列表的 quote 自动转换为嵌套列表一样，quote 应用于圆括号中的标识符自动生成符号列表：

```
> (car (quote (road map)))
'road
> (symbol? (car (quote (road map))))
#t
```

当一个符号在用 ' 表示的被打印列表中时，' 会被省略，因为 ' 已经做了这些工作了：

```
> (quote (road map))
'(road map)
```

quote 对字面量表达式无效，比如一个数字或字符串：

```
> (quote 42)
42
> (quote "on the record")
"on the record"
```

#### 2.4.2quote的缩写，'

可能你已经猜到了，你可以在quote形式的前面，用缩写 ' 代替 quote ：

```
> '(1 2 3)
'(1 2 3)
> 'road
'road
> '((1 2 3) road ("a" "b" "c"))
'((1 2 3) road ("a" "b" "c"))
```

在文档中，'以及它后面的表达式将一起被打印成绿色，因为这种组合方式是一个常量表达式。在DrRacket中，只有'会被打印成绿色。准确地说，DrRacket更正确，因为quote 的含义会根据表达式的上下文变化。然而，在本文档中，我们通常假设标准都绑定在范围内，所以为了额外的清晰性，我们把 quote 形式打印为绿色。

quote 形式的 ' 扩展以字面量的形式表示。如果你在一个 ' 形式前面加 '，你可以看到：

```
> (car ''road)
'quote
> (car '(quote road))
'quote
```

' 缩写在输出时看起来和输入一样。当用 ' 打印输出时，REPL 打印端把 'quote 当成第一个元素是两个元素的列表：

```
> (quote (quote road))
''road
> '(quote road)
''road
> ''road
''road
```

#### 2.4.3列表和racket语法

现在你知道了序对和列表的真相，并且你已经了解 quote，你即将了解我们已经简化过的racket真正语法。

racket的语法不是直接用字符定义，而是用两条规则定义的：

- 读取器层，把字符序列转换成列表，符号和其他常量
- 扩展层(an expander layer)，处理列表，符号和其他常量并转换为表达式

打印和读取的规则是一致的。比如，用括号打印列表，读入一对括号生成列表。同样地，非列表序对用点表示法打印，输入点时用点表示法获得序对。

表达式读取层的一个结果是，在表达式不是quote形式时，你可以用点表示法。

```
> (+ 1 . (2))
```

这可以运行是因为，(+ 1 . (2))只是另一种形式的(+ 1 2)。用这种点表示法写程序实际不是一个好主意。这仅仅是racket语法定义方式的造成的。

通常，读取器只允许 . 用于带括号的序列，而且只能在最后一个元素之前。然而一对 . 也可以在圆括号对中包裹一个单一直出现，只要这个值不是第一个也不是最后一个。这样一对 . 会转换成把在这对 . 中间的元素移到列表的前面。这种转换方式可用于通用的中缀表示法：

```
> (1 . < . 2)
#t
> '(1 . < . 2)
'(< 1 2)
```

这种两个点的表示法在传统lisp中是没有的，它本质上和非列表的序对的点表示法无关。racket程序员很少使用中缀表达式，主要也只是用于非对称二元运算符，比如 < 和 is-a?。

## 3内置数据类型

前面的章节介绍了一些 racket 的内置数据结构：数值，布尔，字符串和函数。本节将更全面的介绍简单数据形式的内置数据类型。

### 3.1布尔值

racket 有两个有特色的常量来表达布尔值：#t 表示真，#f 表示假。大写的 #T 和 #F 也会被解析为一样的值，但是小写的形式是首选。

函数 boolean? 用来识别这两个布尔常量。然而 if、cond、and、or...中的测试表达式的结果会把非 #f 视为真。

例如：
```
> (= 2 (+ 1 1))
#t
> (boolean? #t)
#t
> (boolean? #f)
#t
> (boolean? "no")
#f
> (if "no" 1 0)
1
```

### 3.2数值

racket数值分为精确类型和非精确类型：

+ 精确类型包括
    + 一个任意长度的整数，比如5, 99999999999999999 或者 -17
    + 一个由两个任意长度整数之比组成的有理数，比如1/2，99999999999999999/2，或者-3/4
    + 一个由实部和虚部组成的复数(虚部部分不能为零)，比如1+2i 或者1/2+3/4i
  
+ 非精确类型包括
    + IEEE浮点表示的数,比如 2.0 或者 3.14e+87,其中 IEEE 中无穷大和非数字表示为 +inf.0, -inf.0 和 +nan.0(或者 -nan.0)
    一个用 IEEE 浮点表示实部和虚部的复数，比如 2.0+3.0i 或者-inf.0+nan.0i；有一个例外就是，非精确的复数可以有精确的0实部和一个非精确的虚部。

非精确数值以小数点或者指数说明符打印，精确数值以整型和分数打印。这样的规定同样适用于阅读数字常量时，但是 #e 或者 #i 可以作为数字前缀来强制按精确或者不精确转换数字。#b , #o 和 #x 指定以二进制，八进制和十六进制解释数字。

比如:

```
> 0.5
0.5
> #e0.5
1/2
> #x03BB
955
```

涉及非精确数值的计算会产生非精确数值，所以非精确性就像是数字的污点。
但是请注意，racket 不提供“不精确的布尔值”，因此，不精确数字的比较仍然产生精确的结果。函数 exact->inexact 和 ineaact->exact 用来在这两种类型之间转换：

```
> (/ 1 2)
1/2
> (/ 1 2.0)
0.5
> (if (= 3.0 2.999) 1 2)
2
> (inexact->exact 0.1)
3602879701896397/36028797018963968
```

当精确数值需要表示为实数而不是有理数时，sqrt、log 和 sin 等函数也会产生非精确数值。racket 只表示有理数和复数的有理数部分。

```
> (sin 0)    ; rational...
0
> (sin 1/2)  ; not rational...
0.479425538604203
```

在性能方面，小数值整型的计算是最快的，这里的“小数值”表示，这个数字比机器对有符号数字定义的长度少一位。对于非常大的整形或者非整型的精确数值的计算可能比非精确数值的计算昂贵的多。

```
（define (sigma f a b)
    (if (= a b)
        0
        (+ (f a) (sigma f (+ a 1) b))))

> (time (round (sigma (lamdba (x) (/ 1 x)) 1 2000)))
cpu time: 206 real time: 205 gc time: 0
8
> (time (round (sigma (lambda (x) (/ 1.0 x)) 1 2000)))
cpu time: 0 real time: 0 gc time: 0
8.0
```

数值的这些类型，整形，有理数，实数（也是有理数）和复数都是按通常的方式定义的，它们可以用 integer?，rational?，real?，和 complex? 识别，除了 number? 。一些数学方法只接受实数，但是大部分都实现了对复数的标准的扩展。

```
> (integer? 5)
#t
> (complex? 5)
#t
> (integer? 5.0)
#t
> (integer? 1+2i)
#f
> (complex? 1+2i)
#t
> (complex? 1.0+2.0i)
#t
> (abs -5)
5
> (abs -5+2i)
abs: contract violation
  expected: real?
  given: -5+2i
> (sin -5+2i)
3.6076607742131563+1.0288031496599335i
```

函数 = 用于比较数字是否相等。如果同时给它精确数值和非精确数值进行比较，在比较前通常会把非精确数值转换为精确数值。函数 eqv? (还有 equal? )正好相反，会在同时考虑到精确性和数值的情况下进行比较。

比如：

```
> (= 1 1.0)
#t
> (eqv? 1 1.0)
#f
```

当心非精确数值的计算，它们的性质可能有令人意想不到的行为。即使表面上简单的非精确数值也可能不是你想的那样。比如，当二进制 IEEE 浮点数值可以表示 1/2,但是只能表示近似 1/10。

```
> (= 1/2 0.5)
#t
> (= 1/10 0.1)
#f
>(inexact->exact 0.1)
3602879701896397/36028797018963968
```

### 3.3字符

每一个 racket 字符都对应一个 Unicode 标量值。一般地讲，这个标量值是一个21位的无符号整型，并且映射为一个自然语言的字符或者字符片段。从技术上讲，虽然这个标量是一个比Unicode标准中“字符”概念更简单的概念，但是这是一个有很多用途的近似值。比如任何一个带重音的罗马字母或普通汉字可以被表示为一个标量值。

尽管每个 racket 字符对应一个整型，但是字符的数据类型是区别于数字的。函数 char->integer 和 integer->char 用于转换标量数字和对应字符。

可打印的字符通常打印为 #\ 后跟所表示的字符。不能打印的字符通常打印为 #\u 后跟十六进制的标量值。有一些字符打印得很特殊。比如，空格和换行分别打印为 #\space 和 #\newline。

例子:

```
> (integer->char 65)
#\A
> (char->integer #\A)
65
> #\λ
#\λ
> #\u03BB
#\λ
> (integer->char 17)
#\u0011
> (char->integer #\space)
32
```

函数display直接把字符写到当前的输出接口（详情 Input and Output），这与用于打印字符结果的字符常量语法不一样。

例子：

```
> #\A
#\A
> (display #\A)
A
```

racket为字符提供了一些分类和转换函数。但是请注意，有些字符的转换只有在字符串中才能像人们期望的那用工作。（比如，大写 “ß” 或者小写 “Σ”）

例子：

```
> (char-alphabetic? #\A)
#t
> (char-numeric? #\0)
#t
> (char->whitespace? #\newline)
#t
> (char-downcase #\A)
#\a
> (char-upcase #\ß)
#\ß
```

函数 char=? 用于比较至少两个字符，char-ci=? 用于比较忽略大小写的情况。函数eqv? 和 equal? 作用于字符时的行为和 char=? 一样。当你更明确要比较的是字符时，使用char=?。

例子：

```
> (char=? #\a #\A)
#f
> (char-ci=? #\a #\A)
#t
> (qev? #\a #\A)
#f
```

### 3.4 字符串（Unicode）

字符串是一个固定长度的字符数组。它用双引号打印，双引号和反斜杠在字符串中用反斜杠转义。其他常见的转义也支持，比如 \n 表示换行，\r 表示回车，八进制转义使用 \ 后跟最多3个八进制数字，十六进制转义使用 \u （最多跟4个数字）。字符串中不能打印的字符，在字符串打印时，通常用 \u 表示。

区别于字符串常量语法的打印结果，display 函数直接把字符串中的字符写入到当前输出端口。

例子：

```
> "Apple"
"Apple"
> "\u03BB"
"λ"
> (display "Apple")
Apple
> (display "a \"quoted\" thing")
a "quoted" thing
> (display "two \nlines")
two
lines
> (display "\u03bb")
λ
```

字符串可以是可修改或不可修改的。直接用表达式写出来的字符串是不可修改的，但是大部分其他字符串是可修改的。函数 make-string 在给定长度和可选填的字符后，创建一个可变字符串。函数 string-ref 从字符串中访问字符（从零开始的下标）。函数string-set! 用于改变一个可变字符串的字符。

```
> (string-ref "Apple" 0)
#\A
> (define s (make-string 5 #\.))
> s
"....."
> (string-set! s 2 #\λ)
> s
"..λ.."
```

字符串排序和大小写操作通常是与地区无关的，他们对任何用户都一样。一些地区相关的操作被用来允许最终用户根据语言环境对字符串进行大小写转换和排序。如果你在排序字符串，请使用 string<? 或者 string-ci<?，如果排序结果应在计算和和用户之间保持一致，请使用 string-locale<? 或者 string-locale-ci<?。

例子：

```
> (string<? "apple" "Banana")
#f
> (string-ci<? "apple" "Banana")
#t
> (string-upcase "Straße")
"STRASSE"
> (parameterize ([current-locale "C"])
    (string-locale-upcase "Straße"))
"STRAßE"
```

对用纯 ASCII，使用原始字节或将 Unicode 编码/解码为字节，请使用字节串。

### 3.5字节和字节串（Bytes and Byte Strings）

字节是介于0到255的精确整型。谓词 byte? 用于识别表示字节的数字。

例如：

```
> (byte? 0)
#t
> (bytes? 256)
#f
```

字节串类似字符串，只是包含的是字节序列而不是字符序列。字节串可被用于处理纯 ASCII 码而不是 Unicode 文本。字节串的打印方式非常适用这种方法，因为字节串的打印类似于 ASCII 码的字节串解码，只是前缀是 #。不能打印的 ASCII 字符或者非ASCII字符用八进制写入。

```
> #"Apple"
#"Apple"
> (bytes-ref #"Apple" 0)
65
> (make-bytes 3 65)
#"AAA"
> (define b (make-bytes 2 0))
> b
#"\0\0"
> (bytes-set! b 0 1)
> (bytes-set! b 255)
> b
#"\1\377"
```

字节串的 display 形式将会写入到当前输出端口（详见 see Input and Output）。从技术上讲，用于字符串的 display 打印字符串的 UTF-8 编码到输出端口，因为输出最终是以字节定义的；然而，用于字节串的 display 会把原始字节而非编码过的写入。同样，当本文档显示输出时，在技术上显示了输出的 UTF-8 解码形式。

例子：

```
> (display #"Apple")
Apple
> (display "\316\273")  ; same as "Î»"
Î»
> (display #"\316\273") ; UTF-8 encoding of λ
λ
```

为了在字符串和字节串之间转换，racket 直接支持三种形式的直接编码：UTF-8,Latin-1,和当前本地的编码。字节转字节（特别是到 UTF-8 和从 UTF-8 ）的通用工具填补了支持任意字符串编码的空白。

例子：

```
> (bytes->string/utf-8 #"\316\273")
"λ"
> (bytes->string/latin-1 #"\316\273")
"Î»"
> (parameterize ([current-locale "C"])  ; C locale supports ASCII,
    (bytes->string/locale #"\316\273")) ; only, so...
bytes->string/locale: byte string is not a valid encoding
for the current locale
  byte string: #"\316\273"
> (let ([cvt (bytes-open-converter "cp1253" ; Greek code page
                                   "UTF-8")]
        [dest (make-bytes 2)])
    (bytes-convert cvt #"\353" 0 1 dest)
    (bytes-close-converter cvt)
    (bytes->string/utf-8 dest))
"λ"
```

### 3.6符号

符号是一个原子值，打印方式类似于以 ' 开头的标识符。以 ' 开始后面跟标识符的表达式生成一个符号值。

例子：

```
> 'a
'a
> (symbol? 'a)
#t
```

对于任何字符序列，只有一个对应的符号。调用函数 string->symbol 或者读取语法标识符将会生成一个临时符号。因为符号可以方便的使用 eq? （还有 eqv? 和 equal?）进行比较，所以被用作方便的标记值和枚举值。

符号是大小写敏感的。通过使用 #ci 前缀或者其他方式，将使得读取器使用一个大小写转换过的符号，但是读取器默认保留大小写。

例子：

```
> (eq? 'a 'a)
#t
> (eq? 'a (string->symbol "a"))
#t
> (eq? 'a 'b)
#f
> (eq? 'a 'A)
#f
> #ci'A
'a
```

任何字符串（任何字节数组）都支持用 string->symbol 生成对应的符号。对于读取器，除了空白字符和以下特殊字符，任何字符都可以直接出现在标识符中。

```
( ) [ ] { } " , ' ` ; # | \ 
```

实际上， # 只有在位于符号开头和后面紧跟 % 的情况下才被禁止。其他情况 # 也是可以允许的。 . 自身不能做为一个符号。

空格或者特殊字符可以被包围进一个标识符，通过 | 或 \ 来引用他们。这些引用机制通过包含特殊字符或看起来像数字标识符的形式引用。

例子：

```
> (string->symbol "one, two")
'|one, two|
> (string->symbol "6")
'|6|
```

函数 write 打印一个无 ' 前缀的符号。display 打印形式的符号和其对应字符串一样。

例子：

```
> (write 'Apple)
Apple
> (write 'Apple)
Apple
> (write '|6|)
|6|
> (display '|6|)
6
```

函数 gensym 和 string->uninterned-symbol 生成新的不被理解的符号，这和任何之前被识别或者不被理解的符号都不同。不被理解的符号作为新标记非常有用，它不能与任何其他值混淆。

例子：

```
> (define s (gensym))
> s
'g42
> (eq? s 'g42)
#f
> (eq? 'a (string->uninterned-symbol "a"))
#f
```

### 3.7关键字

关键字类似符号，但是它的打印形式会被加上 #: 前缀。

例如：

```
> (string->keyword "apple")
'#:apple
>'#:apple
'#:apple
> (eq? '#:apple (string->keyword "apple"))
#t
```

更准确的说，关键字和标识符相似。和标识符前面加 ' 生成一个符号一样，一个关键字前面加 '#: 生成一个值。在这两种情况下都使用属于“关键字”，但是有时我们使用关键字值更具体指引用关键字表达式或 string-keyword 函数的结果。没被引用的关键字不是表达式，就像没被引用的标识符不是一个符号一样：

例子：

```
> not-a-symbol-expression
not-a-symbol-expression: undefined;
 cannot reference an identifier before its definition
  in module: top-level
> #:not-a-keyword-expression
eval:2:0: #%datum: keyword misused as an expression
  at: #:not-a-keyword-expression
```

虽然关键字和符号很相似，但是他们的使用方法却不同。关键字是为了在参数列表和某些语法形式中用作特殊标记。对于运行时的标志和枚举，请使用符号而不是关键字。下面的例子说明了关键字和符号的不同作用。

```
> (define dir (find-system-path 'temp-dir)) ; not '#:temp-dir
> (with-output-to-file (build-path dir "stuff.txt")
    (lambda () (printf "example\n"))
    ; optional #:mode argument can be 'text or 'binary
    #:mode 'text
    ; optional #:exists argument can be 'replace, 'truncate, ...
    #:exists 'replace
```

### 3.8序对和列表（Pairs and Lists）

序对连接起两个任意的值。函数 cons 用来创建序对，函数 car 和 cdr 分别提取序对中第一个和第二个元素。谓词 pair? 用来识别序对。

一些序对被打印为用圆括号包围两个打印元素，以 ' 开头并用 . 间隔。

例子：


```
> (cons 1 2)
'(1 . 2)
> (cons (cons 1 2) 3)
'((1 . 2) . 3)
> (car (cons 1 2))
1
> (cdr (cons 1 2))
2
> (pair? (cons 1 2))
#t
```

列表是链表式序对的组合。更准确地说，列表要么是空列表 null，要么是一个列表，这个列表的第一个元素是列表的元素，第二个元素是列表。谓词 list? 用来识别列表。谓词 null? 用来识别空列表。

列表通常打印为 ' 后跟用圆括号包围起来的列表元素。

例子：
```
> null
'()
> (cons 0 (cons 1 (cons 2 null)))
'(0 1 2)
> (list? null)
#t
> (list? (cons 1 (cons 2 null)))
#t
> (list? (cons 1 2))
#f
```

当列表或序对中有元素不能用引号输出时，将会使用 list 和 cons 打印。比如，存在一个值用 srcloc 创建，不能用引号输出（它用srcloc打印）：

```
> (srcloc "file.rkt" 1 0 1 (+ 4 4))
(srcloc "file.rkt" 1 0 1 8)
> (list 'here (srcloc "file.rkt" 1 0 1 8) 'there)
(list 'here (srcloc "file.rkt" 1 0 1 8) 'there)
> (cons 1 (srcloc "file.rkt" 1 0 1 8))
(cons 1 (srcloc "file.rkt" 1 0 1 8))
> (cons 1 (cons 2 (src "file.rkt" 1 0 1 8)))
(list* 1 2 (srcloc "file.rkt" 1 0 1 8))
```

如上面最后一个例子，list* 用于缩写一些列不能使用 list 的 cons。

函数 write 和 display 打印序对和列表时不使用 ' , cons, list 和 list*。除了他们使用列表元素外， write 和 display 对于序对和列表没有区别。

```
> (write (cons 1 2))
(1 . 2)
> (display (cons 1 2))
(1 . 2)
> (write null)
()
> (display null)
()
> (write (list 1 2 "3"))
(1 2 "3")
> (display (list 1 2 "3"))
(1 2 3)
```

迭代列表元素函数是最重要的列表内置函数：

```
> (map (lambda (i) (/ 1 i))
       '(1 2 3))
'(1 1/2 1/3)
> (andmap (lambda (i) (i . < . 3))
          '(1 2 3))
#f
> (ormap (lambda (i) (i . < . 3))
          '(1 2 3))
#t
> (filter (lambda(i) (i . < . 3))
          '(1 2 3))
'(1 2)
> (foldl (lambda (v i) (+ v i))
         10
         '(1 2 3))
16
> (for-each (lambda (i) (display))
            '(1 2 3))
123
> (member "keys"
          '("Florida" "keys" "U.S.A"))
'("Keys" "U.S.A")
> (assoc 'where
         '((when "3:30") (where "Florida" (who "Mickey"))))
'(where "Florida")
```

序对是不可变的（与 lisp 的传统相反），函数 pair? 和 list? 只用来识别不可变的序对和列表。函数 mcons 用来创建可变的序对，它与 set-mcar!、 set-mcdr!、mcar、mcdr 一起使用。可变的序对使用 mcons 打印，当用 write 和 display 打印可变序对时会使用 { 和 }。

例子：

```
> (define p (mcons 1 2))
> p
(mcons 1 2)
> (pair? p)
#f
> (mpair? p)
#t
> (set-mcar! p 0)
> p
(mcons 0 2)
> (write p)
{0 . 2}
```

### 3.9向量(vectors)

向量是一个固定长度的、包含任意值的数组。与列表不同，向量支持对其元素的常量时间内的访问和更新。

向量和列表类似，用圆括号括起其元素序列，但是向量前缀会在 ' 后面加上 #，或者当其中一个元素不能使用 quote 时，使用 vecotr 表示。

向量作为表达式，提供了一个可选长度。此外向量作为表达式隐式引用其内容，这表示向量常量中的标识符和带圆括号的形式表示符号和列表。

```
> #("a" "b" "c")
'#("a" "b" "c")
> #(name (that tune))
'#(name (that tune))
> #4(baldwin bruce)
'#(baldwin bruce bruce bruce)
> (vector-ref #("a" "b" "c") 1)
"b"
> (vector-ref #(name (that tune)) 1)
'(that tune)
```

和字符串一样，向量分为可修改和不可修改的，直接以表达式的方式写出的向量是不可修改的。

向量可以通过 vector->list 和 list->vector 与列表相互转换；这些转换与列表中内置的函数结合时特别有用。当分配额外的列表看起来非常昂贵时，可以考虑使用 for/fold 形式的循环，它可以识别向量和列表。

```
> (list->vector (map string-titlecase
                     (vector->list #("three" "blind" "mice"))))
'#("Three" "Blind" "Mice")
```

### 3.10哈希表（Hash Table）

哈希表实现了一个用含有键和值的映射（键和值可以是任意的racket值，并且访问和更新这个表通常是常数时间的操作）。取决于哈希表是否是用 make-hash，make-hasheqv 还是make-hasheq 创建的，键可以被 equal?， eqv?， 或者 eq?来比较。

```
> (define ht (make-hash))
> (hash-set! ht "apple" '(red round))
> (hash-set! ht "banana" '(yellow long))
> (hash-ref ht "apple")
'(red round)
> (hash-ref ht "coconut")
hash-ref: no value found for key
    key: "coconut"
> (hash-ref ht "coconut" "not there")
"not there"
```

函数hash，hasheqv 和 hasheq 从一个字面量的键值集合创建不可变的哈希表，在这种情况下值会在键之后以一个参数提供。

不可变哈希表可以用 hash-set 扩充，这样会在常量的时间内创建一个新的不可变哈希表。

```
> (define ht (hash "apple" 'red "banana" 'yellow))
> (hash-ref ht "apple")
'red
> (define ht2 (hash-set ht "coconut" 'brown))
> (hash-ref ht "coconut")
hash-ref: no value found for key
    key: "coconut"
> (hash-ref ht2 "coconut")
'brown
```

一个字面量不可变哈希表会被写成 #hash （基于 equal? 的表）、 #hasheqv （基于 eqv？ 的表）或者 #hasheq （基于 eq? 的表） 形式的表达式。圆括号对必须紧跟在 #hash、#hasheq 或者 #hasheqv，并且每个元素必须是一个键-值点对。#hash等形式立即引用他们的键值。

例子：

```
> (define ht #hash(("apple" . red)
                   ("banana" . yellow)))
> (hash-ref ht "apple")
'red
```

可变和不可变哈希表都会像不可变哈希表一样打印，如果所有键值可以用引号表示，则使用带引号的 hash、hasheqv 或者 hasheqv 形式，否则使用 hahs、hasheq 或者 hasheqv：

例子：

```
> #hash(("apple" . red)
        ("banana" . yellow))
'#hash(("apple" . red) ("banana" . yellow))
> (hash 1 (srcloc "file.rkt" 1 0 1 (+ 4 4)))
(hash 1 (srcloc "file.rkt" 1 0 1 8))
```

可变哈希表可以选择性的弱保留它的键，也就是说每个映射只有在键保留在其他地方时才会保留。

例子：

```
> (define ht (make-weak-hasheq))
> (hash-set! ht (gensym) "can you see me?")
> (collect-garbage)
> (hash-count ht)
0
```

请注意，即使是弱哈希表也会一直保留其值，只要相应的键是可访问的。

例子：

```
> (define ht (make-weak-hasheq))
> (let ([g (gensym)])
    (hash-set! ht g (list g)))
> (collect-garbage)
> (hash-count ht)
1

> (define ht (make-weak-hasheq))
> (let ([g (gensym)])
    (hash-set! ht g (make-ephemeron g (list g))))
> (collect-garbage)
> (hash-count ht)
0
```

### 3.11盒子（Boxes）

盒子类似于单元素的向量。被打印为一个 #& 的引用后面跟着盒子值的打印形式。#& 形式也可以当成表达式使用，但是由于结果的盒子是常量，所以没什么用处。

例子：

```
> (define b (box "apple"))
> b
'#&"apple"
> (unbox b)
"apple"
> (set-box! b '(banana boat))
> b
'#&(banana boat)
```

### 3.12void 和 undefined

一些函数和表达式不需要返回值。比如，display 函数被调用只是为了写入到输出端的副作用。在这些情况下，返回值通常是一个被打印为 <*viod*> 的特殊常量。当一个表达式的结果仅仅是一个 #<*void*>，REPL不会打印任何东西。

void 函数使用任意数量的参数，并且返回 #<*void*>。（那是因为，标识符 void 被绑定为一个返回 #<*void*> 的函数，而不是直接绑定为 #<*void*>。）

例子：

```
> (void)
> (void 1 2 3)
> (list (void))
'(#<void>)
```

常量 undefined 被打印为 #<*undefined*>，有时被用于一个其值尚不可用的引用的结果。在之前的 racket 版本中（6.1版本之前）,过早地引用一个绑定会产生 #<*undefined*>；而现在，过早的引用会抛出异常。

```
(define (fails)
    (define x x)
    x)

> (fails)
x: undefined;
  cannot use before initialization
```

## 4 表达式和定义（Expressions and Definitions）

在 racket 要素一章介绍过一些 racket 的语法形式：定义、函数调用、条件句等等。这一节会展示这些形式的更多细节，和一些新增的基础形式。

### 4.1表示法（Notation）

本章节（以及文档的剩余部分）使用一个与《racket要素》一章那种基于字符的语法略有不同的表示法。这种语法形式如下所示：

```
(something [id ...+] an-expr ...)
```

本规范中的元变量(meta-variable)（比如 id 和 an-expr）使用 racket 标识符的语法，因此 an-expr 是一个元变量。命名规范隐式地定义了许多元变量的含义：

- 元变量以 *id* 结尾，表示标识符，比如 x 或者 my-favorite-martian
- 元标识符以 *keyword* 结尾，表示关键字，比如 #:tag.
- 元标识符以 *expr* 结尾，表示任何子形式，并且会被解析为表达式
- 元标识符以 *body* 结尾，表示任何子形式；会被解析为一个局部的定义或表达式。*body* 只有在前面没有任何表达式、并且结尾必须为表达式的时候会被解析为一个定义。

通常，根据惯例，语法中的方括号表示一系列的圆括号形式。这表示，方括号不表示为语法形式的可选部分。

... 表示前一个形式的另个或多个重复。...+ 表示前一个形式的一个或多个重复。除此之外，非斜体的标识符只代表他们自己。

根据上面的语法，这里给出一些用法：

```
(something [x])
(something [x] (+ 1 2))
(something [x my-favorite-martian x] (+ 1 2) #f)
```

### 4.2标识符和绑定

表达式上下文决定了出现在表达式中标识符的含义。特别是以racket语言启动一个模块：

```
#lang racket
```

这表示，在模块中，本指南中描述的标识符从此处描述的含义开始： cons 指向创建序对的函数、 car 指向提取需对地一个元素的函数，等等。

像 define, lambda 和 let 形式将含义与一个或多个相关联，这表示他们绑定了标识符。对于这些绑定，代码部分是他们的绑定范围。一个给定的表达式的有效的绑定集合是这个表达式的环境。

比如：

```
#lang racket
(define f
    (lambda (x)
        (let ([y 5])
            (+ x y))))

(f 10)
```

define 是 f 的绑定，lambda 是 x 的一个绑定， let 是 y 的一个绑定。f 的绑定范围是整个模块； x 的绑定范围是 (let ([y 5] (+ x y)))； y 的绑定范围仅仅是 (+ x y)。(+ x y)的环境包括 y、x 和 f 的绑定，当然也包括 racket 中的所有东西。

模块层的 define 只能绑定模块中未被定义或未被引用的标识符。然而，局部 define 或者其他绑定形式在标识符已经存在绑定的时候仍然可以进行绑定；新的绑定遮挡了(shadows)已经存在的绑定。

例如：

```
(define f
    (lambda (append)
        (define cons (append "ugly" "confusing"))
        (let ([append 'this-was])
            (list append cons))))

> (f list)
'(this-was ("ugly" "confusing"))
```

类似的，模块层的 define 可以遮挡模块语言的绑定。例如：racket模块中的 (define cons 1) 遮挡了 racket 提供的 cons。故意遮挡语言层的绑定并不是个好主意（特别是像 cons 一样被广泛使用的绑定），但是遮挡使程序员不必避免语言提供的每一个模糊绑定。

甚至像 define 和 lambda 这样的标识符也可以绑定，尽管它们有 *transformer* 绑定而不是值绑定。因为 define 有 *transformer* 绑定，所以标识符 define 不能被自身使用来获得值。然而 define 的正常绑定也可以被遮挡。

例子：
```
> define
eval:1:0: define: bad syntax
    in: define
> (let ([define 5]) define)
5
```

同样，以这种方式遮挡标准绑定很少是一个好主意，但是这是racket的固有的灵活性。

### 4.3函数调用

当 *porc-expr* 不是语法转换器（syntax transformer）的标识符时（比如 if 和 define），这种形式的表达式

```
（proc-expr arg-expr ...）
```

就是一个函数调用（也被成为过程应用）。

#### 4.3.1执行顺序和数量

函数调用按顺序（从左至右）执行 proc-expr 和 arg-exprs。如果 proc-expr 生成一个接受的参数和 arg-exprs 一样多的函数，那么这个函数就会被调用。否则会抛出一个异常。

例如：

```
> (cons 1 null)
'(1)
> (+ 1 2 3)
6
> (cons 1 2 3)
cons: arity mismatch;
 the expected number of arguments does not match the given
number
  expected: 2
  given: 3
  arguments...:
   1
   2
   3
> (1 2 3)
application: not a procedure;
 expected a procedure that can be applied to arguments
  given: 1
  arguments...:
   2
   3
```

一些函数，比如 cons，接受固定长度参数。一些函数，比如 + 或者 list，接受任意数量的参数。一些函数接受的参数的个数是一个区间，比如 substring 接受2至2个参数。

#### 4.3.2关键字参数

一些函数接受关键字参数，而不是接受按位置定义的参数。这种情况下，arg 可以表达为 arg-keyword arg-expr 而不仅仅是 arg-expr： 

```
(proc-expr arg ...)

    arg = arg-expr
        | arg-keyword arg-expr
```

例如：

```
(go "super.rkt" #:mode 'fast)
```

用 "super.rkt" 按位置传惨调用函数 go，用 'fast 作为参数传递给 #:mode 关键字。关键字会隐士匹配跟在它后面的表达式。

因为关键字自身不是表达式，所以 

```
(go "super.rkt" #:mode #:fast)
```

是一个语法错误。#:mode 关键字必须被跟在一个表达式后面才会生成一个参数值，而 #:fast 不是一个表达式。

关键字参数的顺序取决与 arg-exprs 被执行时的顺序，但是函数接受关键字参数不关心他们在参数列表的位置。上述的调用等价于：

```
(go #:mode 'fast "super.rkt")
```

#### 4.3.3 apply 函数

函数调用的语法支持任意数量的参数，但是某一特定的函数通常明确接受固定数量的参数。因此，接受参数列表的函数不能将列表中的项直接应用到类似于 '+' 这样的函数。

```
(define (avg lst) ; doesn't work...
    (/ (+ lst) (length lst)))

> (avg '(1 2 3))
+: contract violation
  expected: number?
  given: '(1 2 3)

(define (avg lst) ; doesn’t always work...
  (/ (+ (list-ref lst 0) (list-ref lst 1) (list-ref lst 2))
     (length lst)))

> (avg '(1 2 3))
2
> (avg '(1 2))
list-ref: index too large for list
  index: 2
  in: '(1 2)
```

apply 函数给这种显示提供一种解决方式。它使用一个函数和一个参数列表，将函数应用到参数列表里的值。

```
(define (avg lst)
    (/ (apply + lst) (length lst)))

> (avg '(1 2 3))
2
> (avg '(1 2))
3/2
> (avg '(1 2 3 4))
5/2
```

方便起见，函数 apply 还可以接受函数和参数列表之间的其他参数。而外的参数会被 cons 到参数列表：

```
(define (anti-sum lst)
    (apply - 0 lst))

> (anti-sum '(1 2 3))
-6
```

函数 apply 也可以接受关键字参数，并把这些参数全部传递给被调用函数：

```
(apply go #:mode 'fast '("super.rkt"))
(apply go '("super.rkt") #:mode 'fast)
```

apply列表参数中包含的关键字不算作被调用函数的关键字参数；相反，此列表中的所有参数都被视为位置参数。为了传关键字参数给函数，使用函数 keyword-apply，它接受一个函数和三个列表。前两个列表是对应的：地一个列表包含关键字（按 keyword<?排序），第二个列表包含相应关键字的没一个参数。第三个列表包含按位置传递的参数。

```
(keyword-apply go
               '(#:mode)
               '(fast)
               '("super.rkt"))
```

### 4.4匿名函数 （Functions (Procedures): lambda）

lambda 表达式可以创建函数。最简单的情况，lambda 表达式的形式为：

```
(lambda (arg-id ...)
    body ...+)
```

n 个 arg-id 的 lambda 表达式表示接受 n 个参数：

```
> ((lambda (x) x)
   1)
1
> (lambda (x y) (+ x y)
   1 2)
3
> ((lambda (x y) (+ x y))
   1)
#<procedure>: arity mismatch;
 the expected number of arguments does not match the given
number
  expected: 2
  given: 1
  arguments...:
   1
```

#### 4.4.1声明剩余参数（Declaring a Rest Argument）

lambda 表达式也可以是这种形式：

```
(lambda rest-id
    body ...+)
```

lambda 可以有一个单一的 *rest-id*,而不必用圆括号包围。
生成的函数接受任何数量的参数，并且这些参数被放进一个绑定为 *rest-id* 的列表。

例如：

```
> ((lambda x x)
    1 2 3)
'(1 2 3)
> ((lambda x x))
'()
> ((lambda x (car x))
    1 2 3)
1
```

带有 *rest-id* 的函数经常使用 apply 函数调用另一个接受任意数量参数的函数。

例如：

```
(define max-mag
    (lambda num
        (apply max (map magnitude nums))))

> (max 1 -2 0)
1
> (max-mag 1 -2 0)
2
```

lambda形式还支持结合 *rest-id* 的必需参数：

```
(lambda (arg-id ...+ . rest-id)
    body ...+)
```

这种形式的结果是一个函数，它至少需要和 *arg-ids* 一样多的参数，同时也可以接受任何数量的额外参数。

例如：

```
(define max-mag
    (lambda (num . nums)
        (apply max (map magnitude (cons num nums)))))

> (max-mag 1 -2 0)
2
> (max-mag)
max-mag: arity mismatch;
 the expected number of arguments does not match the given
number
  expected: at least 1
  given: 0
```

*rest-id* 有时也被成为剩余参数，因为它接受函数参数的剩余部分。

#### 4.4.2声明可选参数（Declaring Optional Arguments）

不仅仅是标识符，lambda 形式的参数（除了剩余参数）可以用标识符和默认值指定：

```
(lambda gen-formals
    body ...+)

    gen-formals = (arg ...)
                | rest-id
                | (arg ...+ . rest-id)

            arg = arg-id
                | [arg-id default-expr]
```

\[arg-id default-expr\]形式的参数是可选的。当这个参数在程序中没有提供， *default-expr* 产生一个默认值。*default-expr* 可以指向前面的 *arg-id*,并且每一个 *arg-id* 也必须有一个默认值。

例如：

```
(define greet
    (lambda (given [surname "Smith"])
        (string-append "Hello, " given " " surname)))

> (greet "John")
"Hello, John Smith"
> (greet "John" "Doe")
"Hello, John Doe"

(define greet
    (lambda (given [surname (if (equal? given "John")
                                 "Doe"
                                 "Smith")])
        (string-append "Hello, " given " " surname)))
> (greet "John")
"Hello, Jojn Doe"
> (greet "Adam")
"Hello, Adam Smith"
```

#### 4.4.3声明关键字参数

lambda 形式可以定义按关键字（而不是按位置）传入的参数。关键字参数可以和按位置参数混合使用，默认值表达式可以应用到任意类型的参数。

```
(lambda gen-formals
  body ...+)
 
  gen-formals = (arg ...)
              | rest-id
              | (arg ...+ . rest-id)

          arg = arg-id
              | [arg-id default-expr]
              | arg-keyword arg-id
              | arg-keyword [arg-id default-expr]
```

形如 arg-keyword arg-id 的参数被程序通过 arg-word 使用。keyword-identigfer 在参数列中的位置对于程序匹配参数不重要，因为它会被按关键字匹配成为参数，而不是按位置。

```
(define greet
    (lambda (given #:last surname)
        (string-append "Hello, " given " " surname)))

> (greet "John" #:last "Smith")
"Hello, John Smith"
> (greet #:last "Doe" "John")
"Hello, John Doe"
```

arg-keyword [arg-id default-expr] 指定一个基于关键字并带有默认值的参数。

例如:

```
(define greet
    (lambda (#:hi [hi "Hello"] given #:last [surname "smith"])
        (string-append hi ", " given " " surname)))

> (greet "John")
"Hello, John Smith"
> (greet "karl" #:last "Marx")
"Hello, Karl Marx"
> (greet "John" #:hi "Howdy")
"Howdy, John Smith"
> (greet "karl" #:last "marx" #:hi "Guten Tag")
"Guten tag, Karl Marx"
```

lambda 形式不支持直接创建可接受“剩余”关键字参数的函数。为了构造能接受所有关键字参数的函数，可以使用 make-keyword-procedure。被提供了 make-keyword-procedure 的函数通过两个参数列表接受所有关键字参数，然后所有应用中按位置的参数作为作为剩余参数。

例如：

```
(define (trace-wrap f)
  (make-keyword-procedure
   (lambda (kws kw-args . rest)
     (printf "Called with ~s ~s ~s\n" kws kw-args rest)
     (keyword-apply f kws kw-args rest))))
 
> ((trace-wrap greet) "John" #:hi "Howdy")
Called with (#:hi) ("Howdy") ("John")

"Howdy, John Smith"
```

#### 4.4.4数量敏感函数:case-lambda （Arity-Sensitive Functions: case-lambda）

case-lambda 形式根据被提供参数的数量的不同，产生函数的行为完全不同。case-lambda 表达式具有如下形式：

```
(case-lambda
  [formals body ...+]
  ...)
 
formals = (arg-id ...)
        | rest-id
        | (arg-id ...+ . rest-id)
```

每一个[formals body ...+]都类似(lambda formals body ...+)。使用一个 case-lambda 生成的函数类似于应用 lambda 到第一个参数数量匹配的情况。

例如：

```
(define greet
    (case-lambda
        [(name) (string-append "Hello, " name)]
        [(given surname) (string-append "Hello, " given " " surname)]))

> (greet "John")
"Hello, John"
> (greet "John" "Smith")
"Hello, John Smith"
> (greet)
greet: arity mismatch;
 the expected number of arguments does not match the given
number
  given: 0
```

case-lambda 函数不能直接支持可选参数和关键字参数。

### 4.5定义：define

定义的基本形式如下

```
(define id expr)
```

在这种情况下 *id* 被绑定到 *expr* 的结果。

例如：

```
(define salutation (list-ref '("Hi" "Hello") (random 2)))
> salutation
"Hi"
```

#### 4.5.1函数简写

define 形式通常支持函数定义的简写：

```
(define (id arg ...) body ...+)
```

是下面形式的简写

```
(define id (lambda (arg ...) body ...+))
```

例如：

```
(define (greet name)
    (string-append salutation ", " name))

> (greet "John")
"Hi, John"

(define (greet first [surname "Smith"] #:hi [hi salutation])
    (string-append hi ", " first " " surname))

> (greet "John")
"Hi, John Smith"
> (greet "John" "Doe")
"Hi, john Doe"
```

函数间歇通过 define 也支持 剩余参数（rest argument）。

```
(deifne id (lambda (arg ... . rest-id) body ...+))
```

例如：

```
(define (avg . l)
    (/ (apply + l) (length l)))

> (avg 1 2 3)
2
```

#### 4.5.2柯里化函数简写

考虑下面的 make-add-suffix 函数，它接受一个字符串并返回另一个接受字符串的函数：

```
(define make-add-suffix
    (lambda (s2)
        (lambda (s) (string-append s s2))))
```

尽管不是很普遍，make-add-suffix 的结果可以直接被调用，就像这样：

```
> ((make-add-suffix "!") "Hello")
"Hello!"
```

在某种意义上，make-add-suffix 是一个接受两个参数的函数，但是一次只能接收一个参数。这种接收几个参数并且返回消耗更多的参数的函数，有时被称为柯里化函数。

使用 define 函数简写，make-add-suffix 可以被等价写成：

```
(define (make-add-suffix s2)
    (lambda (s) (string-append s s2)))
```

函数简写反映了 (make-add-suffix "!") 函数调用的形态。define 形式还支持定义嵌套函数调用的柯里化函数的简写：

```
(define ((make-add-suffix s2) s)
    (string-append s s2))

> ((make-add-suffix "!") "Hello")
"Hello!"

(define louder (make-add-suffix "!"))
(define less-sure (make-add-suffix "?"))

> (less-sure "really")
"really?"
> (louder "really")
"really!"
```

define 函数简写的完整语法如下：

```
(define (heads args) body ...+)

    head = id
         | (head args)
    
    args = arg ...
         | arg ... rest-id
```

这个简写的扩展对于每个 *head* 定义都有一层 lambda 嵌套（从最里层 lambda 到最外层 lambda）。

#### 4.5.3多值和 define-values （Multiple Values and define-values）

racket 表达式通常生成单一值，但是有些表达式可以生成多个结果。比如 quotient 和 remainder 各自生成一个值，但是quotient/remainder 一次生成同样的两个值：

```
> (quotient 13 3)
4
> (remainder 13 3)
1
> (quotient/remainder 13 3)
4
1
```

如上所示， REPL 会在每一行打印各自的结果。

多返回值函数可以通过函数 values 来实现，它会接受任意数量的值，并作为结果返回它们。

```
> (values 1 2 3)
1
2
3

(define (split-name name)
    (let ([parts (reg-split " " name)])
        (if (= (length parts) 2)
            (values (list-ref parts 0) (list-ref parts 1))
            (error "not a <first> <last> name"))))

> (split-name "Adam Smith")
"Adam"
"Smith"
```

define-values 形式通过一个单一表达式，一次性绑定多个标识符到多个结果。

```
(define-values (id ...) expr)
```

*expr* 生成参数的数量必须和 *id* 的数量一致。

例如：

```
(define-values (given surname) (split-name "Adam Smith"))

> given
"Adam"
> surname
"Smith"
```

define 形式（不是函数简写）等价于 define-values 形式的单值情况。

#### 4.5.4内部定义（Internal Definitions）

当句法形式指定了 *body*，那么对应的形式可以是定义或者是表达式。*body* 的定义就是内部定义。

*body* 里的表达式和内部定义可以混合，只要最后一个是表达式就可以。

例如，lambda 语法是这样

```
(lambda gen-formals
    body ...+)
```

所以，如下显示的是一个合法的句法实例：

```
(lambda (f)                     ; no definitions
    (printf "running\n")
    (f 0))

(lambda (f)                     ; one definitions
    (define (log-it what)
        (printf "~a\n" what))
    (log-it "running")
    (f 0)
    (log-it "done"))

(lambda (f n)                  ; two definitions
    (define (call n)
        (if (zero? n)
            (log-it "done")
            (begin
                (log-it "running")
                (fn)
                (call (- n 1)))))
    (define (log-it what)
        (printf "~a\n" what))
    (call n))
```

*body* 序列中的内部定义是相互递归的。这表示，任何定义可以引用任何其他的定义，只要引用在定义前没有被实际求值。如果定义太早被引用，会导致一个错误。

例如：

```
(define (weird)
    (define x x)
    x)

> (weird)
x: undefined;
 cannot use before initialization
```

使用 define 的内部定义序列，可以很容易转成等价的 letrec 形式。然而，其他定义形式也可以出现在 *body* 中，包括 define-values，struct或者 define-syntax。

### 4.6本地绑定

尽管内部定义可以被用作本地绑定（local binding），然而racket 还提供给程序员比绑定更多控制的三种形式：let，let*，和 letrec。

#### 4.6.1 平行绑定：let（Parallel Binding: let）

let 形式使用 let body 绑定标识符集，其中每一个都是表达式的结果。

```
(let ([id expr] ...) body ...+)
```

这些 *id* 是被平行绑定的。也就是说，对于任何 *id*，右侧的 *expr* 都没有绑定 *id*，但是在 *body* 中都是可用的。这些 *id* 之间都是不同的。

例如：

```
> (let ([me "Bob"])
    me)
"Bob"
> (let ([me "Bob"]
        [myself "Robert"]
        [I "bobby"])
    (list me myself I))
'("Bob", "Robert" "Bobby")
> (let ([me "Bob"]
        [me "Robert"])
    me)
eval:3:0: let: duplicate identifier
  at: me
  in: (let ((me "Bob") (me "Robert")) me)
```

事实上，对于必须引用旧值的包装器，*id* 的 *expr* 不意识到自己的绑定通常很有用。

```
> (let ([+ (lambda (x y)
                (if (string? x)
                    (string-append x y)
                    (+ x y)))]) ; use original +
    (list (+ 1 2)
          (+ "see" "saw")))
'(3 "seesaw")
```

有时，let 绑定需要方便的交换或重新排列：

```
> (let ([me "Tarzan"]
        [you "Jane"])
    (let ([me you]
         [you me])
        (list me you)))
'("Jane" "Tarzan")
```

将 let 绑定描述为“并行”并不意味着要进行并发计算。表达式是按顺序求值的，即使绑定延迟到所有表达式求值为止。

#### 4.6.2顺序绑定：let*（Sequential Binding: let*）

let* 的语法和 let一样：

```
(let* ([id expr] ...) body ...+)
```

不同点在于每一个 *id* 都可以被后面的 *expr* 使用，当然也包括 *body*。另外，这些 *id* 可以不是唯一的，最新的绑定是可见的。

例如：

```
> (let* ([x (list "Burroughs")]
         [y (cons "Rice" x)]
         [z (cons "Edgar" y)])
    (list x y z))
'(("Burroughs"), ("Rice" "Burroughs") ("Edgar" "Rice" "Burroughs"))
> (let* ([name (list "Burroughs")]
         [name (cons "Rice" name)]
         [name (cons "Edgar" name)])
    name)
'("Edgar" "Rice" "Burroughs")
```

换句话说，let* 形式等价于嵌套的 let 形式，每一个都是单值绑定：

```
> (let ([name (list "Burroughs")])
    (let ([name (cons "Rice" name)])
        (let ([name (cons "Edgar" name)])
            name)))
'("Edgar" "Rice" "Burroughs")
```

#### 4.6.3递归绑定：letrec（Recursive Binding: letrec）

letrec 语法同样和 let 一样：

```
(letrec ([id expr] ...) body ...+)
```

let 使绑定只能在 body 中使用，let* 使绑定可以在其后绑定的任何 expr 中可用，letrec 是绑定在其它所有 expr 中都可用，甚至它之前的expr。换句话说，letrec 绑定是递归的。

letrec形式的表达式通常是递归和相互递归函数的lambda形式：

```
> (letrec ([swing
            (lambda (t)
                (if (eq? (car t) 'tarzan)
                    (cons 'vine
                          (cons 'tarzan (cddr t)))
                    (cons (car t)
                          (swing (cdr t)))))])
    (swing '(vine tarzan vine vine)))
'(vine vine tarzan vine)

> (letrec ([tarzan-near-top-of-tree?
            (lambda (name path depth)
              (or (equal? name "tarzan")
                  (and (directory-exists? path)
                       (tarzan-in-directory? path depth))))]
           [tarzan-in-directory?
            (lambda (dir depth)
              (cond
                [(zero? depth) #f]
                [else
                 (ormap
                  (λ (elem)
                    (tarzan-near-top-of-tree? (path-element->string elem)
                                              (build-path dir elem)
                                              (- depth 1)))
                  (directory-list dir))]))])
    (tarzan-near-top-of-tree? "tmp"
                              (find-system-path 'temp-dir)
                              4))
directory-list: could not open directory
  path: /var/tmp/systemd-private-2664ffead56f4c109ad4eb0808e
8919f-chronyd.service-5qoHCC
  system error: Permission denied; errno=13
```

其实 letrec 形式的 expr 通常是 lambda 表达式，他们可以是任何表达式。这些表达式按顺序执行，虽然他们的值被获得，并立即关联到对应的 id。在内部定义中，当一个 id 在其值未生成时就被引用，就会抛出一个错误。

```
> (letrec ([quicksand quicksand])
    quicksand)
quicksand: undefined;
 cannot use before initialization
```

#### 4.6.4let命名（Named let）

let 命名是迭代和递归的。它使用的同本地绑定一样的语法关键字 let，但是 let 后面跟一个标识符（而不是直接的左圆括号），用来触发不同的解析。

```
(let proc-id ([arg-id init-expr] ...)
    body ...+)
```

这个 let 命名形式等价于

```
(letrec ([proc-id (lambda (arg-id ...)
                    body ...+)])
    (proc-id init-expr ...))
```

这表示，let 命名绑定了一个只在函数体中可见的函数标识符，
并且它通过一些初始表达式的值隐式的调用函数。

```
(define (duplicate pos lst)
    (let dup ([i 0]
              [lst lst])
        (cond
            [(= i pos) (cons (car lst) lst)]
            [else (cons (car lst) (dup (+ i 1) (cdr lst)))])))
> (duplicate 1 (list "apple" "cheese burger!" "banana"))
'("apple" "cheese burger!" "cheese burger!" "banana")
```

#### 4.6.5多值绑定（Multiple Values: let-values, let*-values, letrec-values）

同样的，define-values 用来在定义中绑定多值，let-values，let*-values 和 letrec-values 在局部绑定多个结果。

```
(let-values ([(id ...) expr] ...)
    body ...+)
```

```
(let*-value ([(id ...) expr] ...)
    body ...+)
```

```
(letrec-values ([(id ...) expr] ...)
    body ...+)
```

每一个 expr 生成的多值必须对应 id。绑定规则相同：let-values 的 id 只能在 let 体中被绑定， let*-values 只能在其后的子句中被绑定，letrec-values 可以在所有的 expr 中被绑定。

例如：

```
> (let-values ([q r (quotient/remainder 14 3)])
    (list q r))
'(4 2)
```

### 4.7条件句

大多数用于分支的函数，比如 < 和 string?，产生 #t 或 #f。然而racket 的分支形式把所有非 #f 的值作为 #t 对待。可以这样说真值表示任何非 #f 的值。

“真值”的这个约定与协议很好地结合在一起，其中#f可以用作失败或指示未提供可选值。（注意不要过度使用这个技巧，异常通常是报告失败的更好机制。）

比如，函数 member 具有双重职责。它可以用来查找以特殊的项开始的列表的尾部，或者可以用来简单检查这个项在列表中是否被提供。

```
> (member "Groucho" '("harpo" "Zeppo"))
#f
> (member "Groucho" '("Harpo" "Groucho" "Zeppo") )
'("Groucho" "Zeppo")
> (if (member "Groucho" '("Harpo" "Zeppo"))
    'yep
    'nope)
'nope
> (if (member "Groucho" '("Harpo" "Groucho" "Zeppo"))
    'yep
    'nope)
'yep
```

#### 4.7.1 简单分支：if

if 形式

```
(if test-expr then-expr else-expr)
```

text-expr 总是会被执行。如果它生成一个非 #f 值，那么 then-expr 执行，否则 else-expr 执行。

if 形式必须同时拥有 then-expr 和 else-expr。后者（else-expr）不是可选的。基于 test-expr 执行（或跳过）副作用，请使用 when 或 unless，我们将在后面的序列中描述。

#### 4.7.2合并测试：and 和 or

racket 的 and 和 or 是语法形式，而不是函数。和函数不同，and 和 or 形式当在前面的表达式决定了结果时，会跳过执行后面的表达式。

```
(and expr ...)
```

对于 and 形式，如果任何一个 expr 产生 #f，它就会返回 #f。否则，它返回最后一个 exp 的值。作为一个特殊的例子，(and) 返回 #t。

```
(or expr ...)
```

对于 or 形式，如果它的表达式都返回 #f，它就返回 #f。否则它从它的表达式中返回地一个非 #f 值。作为一个特殊的例子，(or) 返回 #f。

例如：

```
> (define (got-milk? lst)
    (and (not (null? lst))
         (or (eq? 'milk (car lst))
             (got-milk? (cdr lst))))) ; recurs only if needed
> (got-milk? '(apple banana))
#f
```

如果执行到 and 或 or 形式的最后一个表达式，这个表达式的值直接决定了 and 或 or 的值。因此，位于尾部的最后一个表达式，表明了上述的函数 got-milk? 在常数空间内运行。

#### 4.7.3链式测试：cond
cond 形式链起一系列测试并选择一个结果表达式。cond 的语法如下：

```
(cond [test-expr body ...+]
      ...)
```

每一个 test-expr 按顺序被执行。如果它生成 #f，对应的 body 就被忽略，执行继续转到下个 test-expr。一旦某个 test-expr 生成真值，那么他的 body 将被执行并生成 cond 形式的结果，并且其余的 test-expr 不会再被执行。

cond 中最后的 test-expr 可以用 else 代替。在执行的条目中，else 作为 #t 的同义词，它表示最后的子句的意思是捕获所有剩下的情况。如果没有使用 else，那么可能会没有 test-expr 生成 真值。在这种情况下，cond 表达式的结果是 #\<void\>。

例如：
```
> (cond
    [(= 2 3) (error "wrong!")]
    [(= 2 2) 'ok])
'ok
> (cond
    [(= 2 3) (error "wrong!")])
> (cond 
    [(= 2 3) (error "wrong!")]
    [else 'ok])
'ok

(define (got-milk? lst)
    (cond
        [(null? lst) #f]
        [(eq? 'milk (car lst)) #t]
        [else (got-milk? (cdr lst))]))

> (got-milk? '(apple banana))
#f
> (got-milk? '(apple milk banana))
#t
```

cond 的完整语法还包括两个两条子句：

```
(cond cond-clause ...)
 
cond-clause = [test-expr then-body ...+]
            | [else then-body ...+]
            | [test-expr => proc-expr]
            | [test-expr]
```

变体 => 捕获 test-expr 的真值结果，并把结果传递给 proc-expr（必须是接受一个参数的函数）。

例如：

```
> (define (after-groucho lst)
    (cond
      [(member "Groucho" lst) => cdr]
      [else (error "not there")]))
> (after-groucho '("Harpo" "Groucho" "Zeppo"))
'("Zeppo")
> (after-groucho '("Harpo" "Zeppo"))
not there
```

只包含 test-expr 的子句很少被使用。它捕获 test-expr 的真值结果，并把该结果作为整个 cond 表达式的结果返回。

### 4.8顺序执行

racket 程序员喜欢写尽可能少副作用的代码，因为纯函数式代码更容易被测试和被组织进更大的程序。然而，和外部环境交互需要顺序执行，比如写入到显示（writing to a play）、打开图形化窗口或者操纵文件到磁盘。

#### 4.8.1效果在前：begin(Effects Before: begin)

begin 表达式串联一组表达式：

```
(begin expr ...+)
```

这些 expr 按顺序被执行，并且除了最后一个表达式的其他执行结果都会被忽略。最后一个表达式的结果是 begin 形式的结果，它相对 begin 形式处于末尾处。

例如：

```
(deifne (print-triangle height)
    (if (zero? height)
        (void)
        (begin
            (display (make-string height #\*))
            (newline)
            (print-triangle (sub1 height)))))
> (print-triangle 4)
****
***
**
*
```

很多形式，比如 lambda 或者 cond 支持不借助 begin 就可以实现表达式序列。这些形式有时被认为是一个隐式的 begin。

例如：

```
(define (print-triangle height)
    (cond
        [(positive? height)
         (display (make-string height #\*)
         (newline)
         (print-triangle) (sub1 height))]))

> (print-triangle 4)
****
***
**
*
```

begin 形式在 REPL中、在模块级和在其执行体后只有局部定义时有特殊。在这些地方，begin 的内容不是形成表达式而是拼接到上下文环境中。

例如：

```
> (let ([curly 0])
    (begin
        (define moe (+ 1 curly))
        (define larry (+ 1 moe)))
    (list larry curly moe))
'(2 0 1)
```

这种拼接行为主要用于宏，我们稍后会讨论。

#### 4.8.2 效果在后:begin0 （Effects After: begin0）

begin0 表达式语法和 begin 表达式语法一样：

```
(begin0 expr ...+)
```

不同点在于，begin0 返回地一个表达式的结果，而不是返回最后一个表达式的结果。begin0 形式适用于在计算后产生副作用（特别是在计算产生未知数量结果的情况下）。

例如：

```
(define (log-times thunk)
    (printf "Start: ~s\n" (current-inexact-milliseconds))
    (begin0
        (thunk)
        (printf "End..: ~s\n" (current-inexact-milliseconds))))

> (log-times (lambda () (values 1 2)))
Start: 1574107139348.75
End..: 1574107139348.887
1
2
```

#### 4.8.3 效果选择（Effects If...: when and unless）

when 形式结合了 if 风格的条件和 then 子句的序列，并且没有 else 子句。

```
(when test-expr then-body ...+)
```

如果 test-expr 生成一个真值，那么所有 then-body 将被执行。最后一个 then-body 的结果是 when 形式的结果。否则，then-body 不会被执行，结果为 #\<viod\>。

unless 形式类似：

```
(unless test-expr then-body ...+)
```

不同点在于 text-expr 的结果是反的：只有在 test-expr 的结果是 #f 的时候 then-body 才会被执行。

例如：

```
(define (enumerate lst)
  (if (null? (cdr lst))
      (printf "~a.\n" (car lst))
      (begin
        (printf "~a, " (car lst))
        (when (null? (cdr (cdr lst)))
          (printf "and "))
        (enumerate (cdr lst)))))
 
> (enumerate '("Larry" "Curly" "Moe"))
Larry, Curly, and Moe.

(define (print-triangle height)
  (unless (zero? height)
    (display (make-string height #\*))
    (newline)
    (print-triangle (sub1 height))))

> (print-triangle 4)
****
***
**
*
```

### 4.9 赋值：set!（Assignment: set!）

给变量赋值使用 set!：

```
(set! id expr)
```

set! 表达式执行 expr 并且改变 id (id 必须在封闭环境中绑定过)的值为 expr 的结果值。set!表达式的结果为 #\<void\>。

例如：

```
(define greeted null)

(define (greet name)
    (set! greeted (cons name greeted))
    (string-append "Hello, " name))
> (greet "Athos")
"Hello, Athos"
> (greet "Porthos")
"Hello, Porthos"
> (greet "Aramis")
"Hello, Aramis"
> greeted
'("Aramis" "Porthos" "Athos")

(define (make-running-total)
    (let ([n 0])
        (lambda ()
            (set! n (+ n 1))
        n)))
(define win (make-running-total))
(define lose (make-running-total))

> (win)
1
> (win)
2
> (lose)
1
> (win)
3
```

#### 4.9.1 使用赋值的参考指南 （Guidelines for Using Assignment）

尽管使用 set! 有时是适当的，但是 racket 风格通常不鼓励使用 set!。下面的一些参考或许会告诉你何时使用 set! 是合适的。

- 与其他现代语言一样，分配共享标识符不能代替将参数传递给过程或获取过程结果。

**非常糟糕的**例子：

```
(define name "unknown")
(define result "unknown")
(define (greet)
    (set! result (string-append "Hello, " name)))

> (set! name "John")
> (greet)
> result
"Hello, John"
```

合适的例子：

```
(define (greet name)
    (string-append "Hello, " name))

> (greet "John")
"Hello, John"
> (greet "Anna")
"Hello, Anna"
```

- 一系列局部变量的赋值序列远不如嵌套绑定

坏的例子：

```
> (let ([tree 0])
    (set! tree (list tree 1 tree))
    (set! tree (list tree 2 tree))
    (set! tree (list tree 3 tree))
    tree)
'(((0 1 0) 2 (0 1 0)) 3 ((0 1 0) 2 (0 1 0)))
```

好的例子：

```
> (let* ([tree 0]
         [tree (list tree 1 tree)]
         [tree (list tree 2 tree)]
         [tree (list tree 3 tree)])
    tree)
'(((0 1 0) 2 (0 1 0)) 3 ((0 1 0) 2 (0 1 0)))
```

- 使用赋值从迭代中累积结果是错误的方式。通过循环变量来累计更好一些。

有点糟糕的例子：

```
(define (sum lst)
    (let ([s 0])
        (for-each (lambda (i) (set! s (+ i s)))
                  lst)
        s))

> (sum '(1 2 3))
```

好一点的例子：

```
(define (sum lst)
    (let loop ([lst lst] [s 0])
        (if (null? lst)
            s
            (loop (cdr lst) (+ s (car lst))))))

> (sum '(1 2 3))
6
```

好的例子（一般方法）：

```
(define (sum list)
    (for/fold ([s 0])
              ([i (in-list lst)])
        (+ s i)))

> (sum '(1 2 3))
6
```

- 若对于一个对象，状态是必须的而且适当的，那么使用 set! 实现对象的状态是和是的。

合适的例子：

```
(define next-number!
    (let ([n 0])
        (lambda ()
            (set! n (add1 n))
            n)))

> (next-number!)
1
> (next-number!)
2
> (next-number!)
3
```

在其他条件相同的情况下，不使用赋值或可变的程序比使用赋值和可变的程序好。然而，这样虽然避免了副作用，但是如果代码的可读性更高，或者实现了一个更好的算法，他们还是应该被使用的。

可变值的使用，比如数组和哈希表，这种程序的风格比直接使用 set! 减少了疑虑。尽管如此，简单地在程序中使用 vector-set! 替换 set! 并没有改善程序的风格。

#### 4.9.2 多值：set!-values

set!-values 形式一次性赋值多个变量（通过一个产生适当数量值的表达式）：

```
(set!-values (id ...) expr)
```

这种形式等价于使用 let-values 从 expr 接受多个结果，然后使用 set! 将结果逐个赋值个每个 id。

例子：

```
(define game
    (let ([w 0]
          [l 0])
        (lambda (win?)
            (if win?
                (set! w (+ w 1))
                (set! l (+ l 1)))
            (begin0
                (values w l)
                ; swap sides
                (set!-values (w l) (values l w))))))

> (game #t)
1
0
> (game #t)
1
1
> (game #f)
1
2
```

### 4.10引用：quote 和 '

quote 形式产生一个常量：

```
(quote datum)
```

datum 的语法是在技术上被指定为可以被 read 函数解析为单一值的内容。 quote 形式的值和给定 datum 后 read 函数的产生的值一样。

datum 可以是 符号、布尔值、数字、字符/字节串、字符、关键字、空列表、包含更多这样值的序对/列表、包含更过这样值的数组、包含更多这样值的哈希表、包含另一个这样值的 box。

例子：

```
> (quote apple)
'apple
> (quote #t)
#t
> (quote 42)
42
> (quote "hello")
"hello"
> (quote ())
'()
> (quote ((1 2 3) #("z" x) . the-end))
'((1 2 3) #("z" x) . the-end)
> (quote (1 2 . (3)))
'(1 2 3)
```

上面显示的最后一个例子中，datum 不必和标准化打印形式一样。datum 不能是打印形式是以 #< 开头的内容，所以不能是 #\<void\>、#\<undefined\>或这一个过程。

quote 形式很少用于本身就是布尔值、数字或字符串的数据，因为这些值的打印形式已经被用作为常量。quote 形式通常用于符号和列表，这些值在未被引用时有其他意义（标识符，函数调用等）。

表达式

```
'datum
```

是下列形式的缩写

```
(quote dautm)
```

并且这个缩写通常用于取代 quote。这个简写甚至可以应用在 datum 中，所以它可以带引号的列表。

例子：

```
> 'apple
'apple
> '"hello"
"hello"
> '(1 2 3)
'(1 2 3)
> (display '(you can 'me))
(you can (quote me))
```

### 4.11 准引用：quasiquote and ‘（Quasiquoting: quasiquote and ‘）

quasiquote 形式和 quote 类似：

```
(quasiquote datum)
```

然而，对于没一个出现在 datum 里的 (unquote expr)，expr 被执行并生成一个替代 unquote 子形式的值。

例子：

```
> (quasiquote (1 2 (unquote (+ 1 2)) (unquote ()- 5 1)))
'(1 2 3 4)
```

这个形式可以用于写根据某些形式建立列表的函数。

例子：

```
> (define (deep n)
    (cond
        [(zero? n) 0]
        [else 
         (quasiquote ((unquote n) (unquote (deep (- n 1)))))]))
> (deep 8)
'(8 (7 (6 (5 (4 (3 (2 (1 0))))))))
```

或者甚至以编程的方式廉价的创建列表。（当然，十有八九，你会使用 macro 来做这些。）

例子：

```
> (define (build-exp n)
    (add-lets n (make-sum n)))
> (define (add-lets n body)
    (cond
        [(zero? n) body]
        [else 
         (quasiquote
            (let ([(unquote (n-var n)) (unquote n)])
                (unquote (add-let (-n 1) body))))]))
> (define (make-sum n)
    (cond
        [(= n 1) (n->var 1)]
        [else 
            (quasiquote (+ (unquote (n->var n))
                           (unquote (make-sum (- n 1)))))]))
> (define (n->var n) (string->symbol (format "x~a" n)))
> (build-exp 3)
'(let ((x3 3)) (let ((x2 2)) (let ((x1 1)) (+ x3 (+ x2 x1)))))
```

unquote-splicing 形式和 unquote 类似，但是它的 expr
必须生成列表，并且 unquote-splicing 形式必须出现在产生在列表或数组的上下文中。顾名思义，结果列表被拼接进这个上下文中。

例子：
```
> (quasiquote (1 2 (unquote-splicing (list (+ 1 2) (- 5 1))) 5))
'(1 2 3 4 5)
```


使用拼接，我们可以修改上述示例的表达式的结构，是它只需要一个 let 表达式 和一个 + 表达式。

例子：

```
> (define (build-exp n)
    (add-lets
     n
     (quasiquote (+ (unquote-splicing
                     (build-list
                      n
                      (λ (x) (n->var (+ x 1)))))))))
> (define (add-lets n nody)
    (quasiquote
     (let (unquote
           (build-list
            n
            (λ (n)
              (quasiquote
               [(unquote (n->var (+ n 1))) (unquote (+ n 1))]))))
      (unquote body))))
> (define (n->var n) (string->symbol (format "x~a" n)))
> (build-exp 3)
'(let ((x1 1) (x2 2) (x3 3)) (+ x1 x2 x3))
```

如果一个 quasiquote 形式出现在一个封闭的 quasiquote 形式中，那么内层 quasiquote 会有效的消除一层 unquote 和 unquote-splicing 形式，所以另一个 unquote 或者 unquote-splicing 是必需的。

例子：

```
> (quasiquote (1 2 (quasiquote (unquote (+ 1 2)))))
'(1 2 (quasiquote (unquote (+ 1 2))))
> (quasiquote (1 2 (quasiquote (unquote (unquote (+ 1 2))))))
'(1 2 (quasiquote (unquote 3)))
> (quasiquote (1 2 (quasiquote ((unquote (+ 1 2)) (unquote (unquote (- 5 1)))))))
'(1 2 (quasiquote ((unquote (+ 1 2)) (unquote 4))))
```

上面的执行不会如打印一样显示。而是使用 quasiquote 和 unquote 的简写 ` 和 ，。这些简写同样可以用在表达式中：

例子

```
> `(1 2 `(,(+ 1 2) ,,(- 5 1)))
'(1 2 `(,(+ 1 2) ,4))
```

unquote-splicing 的简写是 ,@ 

例子

```
> `(1 2 ,@(list (+ 1 2) (- 5 1)))
'(1 2 3 4)
```

### 4.12 单值分发：case （Simple Dispatch: case）

case 形式分发到一个子句（通过匹配表达式的结果和值）：

```
(case expr
    [(datum ...+) bpdy ...+]
    ...)
```

每一个 datum 都会使用 equal? 来和 expr 的结果进行比较，然后相应的 body 将会被执行。case 形式会在 N 条分支中，在 O(logN) 的时间分发到正确的子句。

多个 datum 每个都可以提供一个子句，如果任何一个 datum 匹配，那么对应的 body 将会被执行。

例子：

```
> (let ([v (radom 6)])
    (printf "~a\n" v)
    (case v
      [(0) 'zero]
      [(1) 'one]
      [(2) 'two]
      [(3 4 5) 'many]))
5
'many
```

case 形式的最后一个子句可以是 else （和 cond 一样）：

例子：

```
> (case (random 6)
    [(0) 'zero]
    [(1) 'one]
    [(2) 'two]
    [else 'many])
```

对于更一般的模式匹配(但是没有调度时间保证)，使用match。这会在模式匹配中进行介绍。

### 4.13动态绑定：parameterize

parameterize 形式在表达式体的计算期间，将一个新值和一个参数绑定。

```
(parameterize ([parameter-expr value-expr] ...)
    body ...+)
```

比如，参数 error-print-width 控制一个值的多少字符被打印在错误信息中：

```
> (parameterize ([error-print-width 5])
    (car (expt 10 1024)))
car: contract violation
    expected: pair?
    given: 10...
> (parameterize ([error-print-width 10])
    (car (expt 10 1024)))
car: contract violation
    expected: pair?
    given: 1000000...
```

更一般的讲，参数实现了一种动态绑定。函数 make-parameter 接受任何值并返回一个新的初始值为该值的变量。将参数作为函数使用并返回当前值：

```
> (define location (make-parameter "here"))
> (location)
"here"
```

在 parameterize 形式中，每个 parameter-expr 必须生成一个参数。在 body 执行期间，每个明确的参数的结果给定为对应的 value-expr。当执行代码离开 parameterize 形式时（不管是正常退出，异常退出，还是其他推出）,这个参数会还原到它之前的值。

```
> (parameterize ([location "there"])
    (location))
"there"
> (location)
"here"
> (parameterize ([location "in a house"])
    (list (location)
          (parameterize ([location "with a mouse"])
            (location))
          (location)))
'("in a house" "with a mouse" "in a house")
> (parameterize ([location "in a box"])
    (car (location)))
car: contract violation
    expected: pair?
    given: "in a box"
> (location)
"here"
```

parameterize 形式不像 let 绑定一样，上面的每次 location 的使用都会直接引用原定义。在parameterize 表达式执行体被执行期间，parameterize 形式会修改参数的值，甚至即便是在 parameterize 执行体外部定义的参数也是一样。

```
> (define (would-you-could-you?)
    (and (not (equal? (location) "here"))
         (not (equal? (location) "there"))))
> (would-you-could-you?)
#f
> (parameterize ([location "on a bus"])
    (would-you-could-you?))
#t
```

如果一个在生成值之前还未执行的 parameterize 执行体内部对一个变量进行使用，那么这次使用将不会看到 parameterize 形式对这个变量的修改：

```
> (let ([get (parameterize ([location "with a fox"])
                (lambda () (location)))])
    (get))
"here"
```

通过把参数作为函数调用并获取值，当前变量的绑定可以被命令式的修改。如果一个 parameterize 已经修改了参数的值，那么直接应用参数仅影响与当前 parameterize 关联的值。

```
> (define (try-again! where)
    (location where))
> (location)
"here"
> (parameterize ([location "on a train"])
    (list (location)
          (begin (try-again! "in a boat")
                 (location))))
'("on a train" "in a boat")
> (location)
"here"
```

parameterize 的使用更适用于命令式的修改参数值，出于同样的原因，对于用 let 绑定的新值，使用 set! 更合适。

可以看出，变量 和 set! 可以解决很多与参数相同的问题。比如, lokation 可以被定义为一个字符串，set! 可以用来修改它的值：

```
> (define lokation "here")
> (define (would-ya-could-ya?)
    (and (not (equal? lokation "here"))
         (not (equal? lokation "there"))))
> (set! lokation "on a bus")
> (would-ya-could-ya?)
#t
```

然而，parameterize 形式提供几个超过 set! 的优势:

- 当执行由于异常推出时，parameterize 形式帮助自动重置参数的值。添加异常处理和其他形式的 set! 恢复是很乏味的。
- 参数可以与尾递归漂亮的工作。parameterize 形式的最后一个执行体相对于 parameterize 形式处于尾部位置。
- 参数对多线程工作的好。parameterize 形式只在当前线程的执行中修改参数的值，这样避免了和其他线程的竞争。

## 5 程序员定义的数据类型（Programmer-Defined Datatypes）

新数据类型通常用 struct 形式创建，这便是这节的主题。基于类型的对象系统（参考 Classes and Objects）提供了创建新数据类型的代替方案，但是即便是类型和对象也遵循数据结构的规则。

### 5.1 简单结构类型：struct

第一个相似之处是，struct 的语法是

```
(struct struct-id (field-id ...))
```

例子：

```
(struct posn (x y))
```

struct 形式绑定 struct-id 和一些 struct-id 创建的标识符以及 field-id：

- struct-id:一个构造函数，它接受和 field-id 数量一致的参数，并返回结构类型的实例。

例子：

```
> (posn 1 2)
#<posn>
```

- struct-id?:一个判断函数，它接受一个单一的参数，并且返回 #t（如果是该结构类型的实例），#f（如果不是该结构类型的实例）。

例子：

```
> (posn? 3)
#f
> (posn? (posn 1 2))
#t
```

- struct-id-field-id:对于每个 field-id，是一个从结构类型实例中提取对应字段值的访问器。

例子：

```
> (posn-x (posn 1 2))
1
> (posn-y (posn 1 2))
2
```

- struct:struct-id : 是一个结构类型的描述器，它是一个值，这个值将结构类型表示为第一类值。

struct 形式对结构类型实例中的字段可能出现的值类型不进行约束。例如，尽管 "apple" 和 #f 明显不符合 posn 实例的用法，(posn "apple" #f) 仍然产生一个 posn 的实例。强制约束字段的值，比如要求它们是数字，这是 contract 的工作，后面我们会讨论 Contract。

### 5.2 复制和更新

struct-copy 形式在克隆时，克隆一个结构并选择性的更新规定字段。这个操作有时被称为功能更新（functional update），以为产生的结果是一个被更新过字段的结构。但是原结构并没有被修改。

```
(struct-copy strucr-id struct-expr [field-id expr] ...)
```

出现在 struct-id 后面的 struct-copy 必须是名称已绑定为 struct 的结构类型（名称不能直接被用作表达式）。struct-expr 必须生成一个该结构类型的实例。结构是和旧值类似的一个新的该类型的实例（除非每个 filed-id 指定的字段都获取了对应 expr 的值）。

```
> (define p1 (posn 1 2))
> (define p2 (struct-copy posn p1 [x 3]))
> (list (posn-x p2) (posn-y p2))
'(3 2)
> (list (posn-x p1) (posn-y p1))
'(1 2)
```

### 5.3结构子类型（Structure Subtypes）

struct 的扩展形式可以用于定义结构子类型（基于现有类型扩展而来的类型）。

```
(struct struct-id super-id (field-id ...))
```

super-id 必须是已绑定为 struct 的类型名称。（名称不能直接作为表达式使用）

例子：

```
(struct posn (x y))
(struct 3d-posn posn (z))
```

结构子类型集成了父类型的字段，并且构造器在接受父类型字段值之后接受子类型字段值。子类型的实例可以用于父类型的判断函数和接收器。

例子：

```
> (define p (3d-posn 1 2 3))
> p
#<3d-posn>
> (posn? p)
#t
> (3d-posn-z p)
3
; a 3d-posn has an x field, but there is no 3d-posn-x selector:
> (3d-posn-x p)
3d-posn-x: undefined;
 cannot reference an identifier before its definition
  in module: top-level
; use the supertype's posn-x selector to access the x field:
> (posn-x p)
1
```

### 5.4 不透明与透明结构类型

当一个结构类型被定义这样

```
(struct posn (x y))
```

这类结构类型的实例的打印方式不会显示字段的任何信息。那是因为结构类型模式是不透明的。如果一个结构类型的访问器和修改器对模块保持私有，那么没有其它模块可以依赖该类型实例的表示。

为了使得结构类型透明，在参数名序列后使用 #:transparent 关键字：

```
(struct posn (x y)
        #:transparent)

> (posn 1 2)
(posn 1 2)
```

一个透明的结构类型实例打印起来就像构造函数的一次调用，即它会显示该结构字段的值。透明结构类型同样允许常规操作它的实例，比如 struct? 和 struct-info。

结构类型默认是不透明的，因为不透明的结构实例提供了更多的封装保证。也就是说，一个库可以使用不透明的结构来封装数据，除非该库允许，否则该库的调用方不能操纵这个结构的数据。

### 5.5 结构比较

通用的 equal? 比较自动在透明结构类型的字段上递归，但是对于非透明的结构类型仅仅比较实例的标识符：

```
(struct glass (width height) #:transparent)

> (equal? (glass 1 2) (glass 1 2))
#t
```

```
(struct lead (width height))

> (define slab (lead 1 2))
> (equal? slab slab)
#t
> (equal? slab (lead 1 2))
#f
```

为了支持实例不使用透明结构就可以通过 equal? 比较，你可以使用 #:method 关键字、gen:equal+hash、并实现三个方法:

```
(struct lead (width height)
    #:methods
    gen:equal+hash
    [(define (equal-proc a b equal?recur)
        ; compare a and b
        (and (equal?-recur (lead-with a) (lead-with b))
             (equal?-recur (lead-height a) (lead-height b))))
     (define (hash-proc a hash-recur)
        ; compare primary hash code of a
        (+ (hash-recur (lead-width a))
            (* 3 (hash-recur (lead-height a)))))
     (define (hash2-proc a hash2-recur)
        ; compute secondary hash code of a
        (+ (hash2-recur (lead-width a))
           (hash2-recur (lead-height a))))])

> (equal? (lead 1 2) (lead 1 2))
#t
```

列表中的第一个函数作用在两个 lead 上，实现了 equal? 测试：函数的第三个参数在递归情况用来取代 equal? 进行相等性测试，以便可以正确的处理循环数据。另外两个方法分别计算用于哈希表的主、次哈希码：

```
> (define h (make-hash))
(hash-set! h (lead 1 2) 3)
(hash-ref h (lead 1 2))
3
> (hash-ref h (lead 2 1))
hash-ref: no value found for key
  key: #<lead>
```

提供 gen:equal+hash 的第一个函数不必递归比较结构字段。比如，表示集合的结构类型可以通过检查集合的成员是否相等（与内部表示的元素的顺序无关）来实现相等。仅仅需要担心的是，对于任何两个应该是等价的结构类型，hash 函数应该生成相同的值。

### 5.6 结构类型生成（Structure Type Generativity）

每当一个 struct 形式被执行，就会生成一个和已有所有结构类型都不同的结构类型（即使一些其他的结构类型有相同的名字和字段）。

这种生成方式对执行抽象和实现诸如解释器之类的程序很有用，
但是要注意不要将 struct 形式放在进行多次求值的地方。

例子:

```
(define (add-bigger-fish lst)
    (struct fish (size) #:transparent) ; new every time
    (cond
        [(null? lst) (list (fish 1))]
        [else (cons (fish (* 2 (fish-size (car lst))))
                    lst)]))

> (add-bigger-fish null)
(list (fish 1))
> (add-bigger-fish (add-bigger-fish null))
fish-size: contract violation;
 given value instantiates a different structure type with
the same name
  expected: fish?
  given: (fish 1)
```

```
(struct fish (size) #:transparent)
(define (add-bigger-fish lst)
    (cond
        [(null? lst) (list (fish 1))]
        [else (cons (fish (* 2 (fish-size (car lst))))
                    lst)]))
> (add-bigger-fish (add-bigger-fish null))
(list (fish 2) (fish 1))
```

### 5.7 预置结构类型（Prefab Structure Types）

尽管透明(transparent)结构类型通过显示自身内容的方式进行打印，但是在表达式中，结构的这种打印形式不能用来获取结构(不像 number、string、symbol、和 list 一样)。

预置("previously fabricated")结构类型是一个已经被 racket 打印器和表达式读取器知晓的内置类型。存在无限多的这种类型，并且按名称、字段数量、父类型和其他这类的细节进行索引。预置结构的打印形式类似于数组，但是以 #s 而不只是 # 开始，并且打印形式的第一个元素是预置结构类型的名字。

下面的例子显示含有一个字段的 sprout 预置结构类型的实例。第一个实例包涵字段值 'bean，第二个包含字段值 'alfalfa：

```
> '#s(sprout bean)
'#s(sprout bean)
> '#s(sprout alfalfa)
'#s(sprout alfalfa)
```

像 number 和 string，预置结构是“自引用”，所以引号是可选的：

```
> #s(sprout bean)
'#s(sprout bean)
```

当你在 struct 时使用 #:prefab 关键字，不是生成一个新类型，而是获取了已存在预置结构类型的绑定：

```
> (define lunch '#s(sprout bean))
> (struct sprout (kind) #:prefab)
> (sprout? lunch)
#t
>(sprout-kind lunch)
'bean
>(sprout 'garlic)
'#s(sprout garlic)
```

上述 kind 字段名对于找到预置结构类型不重要（只要 sprout 名称和字段数量匹配）。同时，有三个字段的预置结构类型 sprout 和有一个字段的不一样。

```
> (sprout? #s(sprout bean #f 17))
#f
> (struct sprout (kind yummy? count) #:prelab) ;redefine
> (sprout? #s(sprout bean #f 17))
#t
> (sprout? lunch)
#f
```

预置结构类型可以有另一个预置结构类型作为它的父类型，它可以有可变字段，也可以有自动字段。这些变化对应不同预置结构类型，结构类型名称的打印形式封装了所有相关细节。

```
> (struct building (rooms [location #:mutable]) #:prelab)
> (struct house building ([occupied #:auto]) #:prelab
    #:auto-value 'no)
> (house 5 'factory)
'#s((house (1 no) building 2 #(1)) 5 factory no)
```

每个预置类型都是透明的，但是没有透明类型抽象，因为可以不需要对特定结构类型的声明或现有示例进行访问就可以创建实例。总的来说，结构类型的不同选项提供了从更抽象到更方便的各种可能性：

- **不透明(Opaque)**(默认):在不访问结构类型申明的情况下，不能检查和伪造。如下一节讨论的，构造函数保护和属性可以添加到结构类型以进一步保护或专门化其实例的行为。

- **透明(Transparent)**:在不访问结构类型声明的情况下，任何人都可以检查或创建一个实例，这表示值打印器可以显示实例的内容。然而所有实例的创建都要经过构造函数保护，以便控制实例的内容，并且可以通过属性专门化实例的行为。由于结构内行是由其定义生成的，因此不能简单地通过结构类型的名称来生成实例，因此不能由表达式读取器自动生成实例。

- **预置(Prefab)**:在没有预先访问结构类型的定义或者实例的情况下，任何人都可以在任何时候检查或创建实例。因此，表达式读取器可以直接生成实例。这样的实例不能有构造函数保护或者属性。

由于表达式读取器可以生成预置实例，所以当方便的序列化比抽象更重要的时候，这是很有用处的。然而，如果不透明和透明的结构被如 Datatypes and Serialization 中所述的 serializable-struct 定义的话也可以被序列话。

### 5.8更多结构类型的选项

struct 的全语法支持很多选项，包括结构类型级别和单独的字段级别：

```
(struct struct-id maybe-super (field ...)
        struct-option)
maybe-super = 
            | super-id
      field = field-id
            | [field-id field-option ...]
```

struct-option 总是以一个关键字开始：

```
#:mutable
```

使结构的全部字段可变，并对每个 field-id 使用可变器 set-struct-id-field-id!(这会设置结构类型实例的对应字段值)。

例子:

```
> (struct dot (x y) #:mutable)
(define d (dot 1 2))

> (dot-x d)
1
> (set-dot-x! d 10)
> (dot-x d)
10
```

#:mutable 选项同样可以用于 field-option，在这种情况下使得单独的字段可变。

例子:

```
> (struct person (name [age #:mutable]))
(define friend (person "Barney" 5))
> (set-person-age! friend 6)
> (set-person-name! friend "Mary")
set-person-name!: undefined;
 cannot reference an identifier before its definition in module: top-level
```

```
#:transparent
```

通过结构实例控制反射，如前一节讨论过的，Opaque versus Transparent Structure Types。

```
#:inspector inspector-expr
```

泛化 #:tarasparent 以支持对反射操作更可控的访问。


```
#:prefab
```

访问一个内置的结构类型，如前一节所述，Prefab Structure Types。

```
#:auto-value auto-expr
```

指定要用于结构类型中所有自动字段的值，其中自动字段由：#auto 字段选项指示。构造函数不接受自动字段参数。自动字段是隐式可变的(通过反射操作)，但是可变函数只在 #:mutable 被指定时才被绑定。

例子:

```
> (struct posn (x y [z #:auto])
               #:transparent
               #:auto-value 0)
> (posn 1 2)
(posn 1 2 0)
```

```
#:guard guard-expr
```

当结构类型的实例被创建时，指定构造函数保护过程被执行。保护函数接受和该结构类型非自动字段个数一样多的参数，再加上实例化类型的名字。保护函数需要返回的参数数量和给定一样，再减去实例化类型的名字，或者它可以覆盖一个参数。

例子：

```
> (struct thing (name)
          #:transparent
          #:guard (lambda (name type-name)
                    (cond
                        [(string? name) name]
                        [(symbol? name) (symbol->string name)]
                        [else (error type-name
                                     "bad name: ~e"
                                     name)])))
> (thing "apple")
(thing "apple")
> (thing 'apple)
(thing "apple")
> (thing 1/2)
thing: bad name: 1/2
```

甚至在子类型实力被创建时，保护函数也会被执行。在那种情况下，只有被构造函数结构的字段才被提供给保护函数（但是子函数的保护函数获取原始字段和子类型新增的字段）。

例子:

```
> (struct person thing (age)
          #:transparent
          #:guard (lambda (name age type-name)
                    (if (negative? age)
                        (error type-name "bad age: ~e" age)
                        (values name age))))
> (person "Jhon" 10)
(person "Jhon" 10)
> (person "Mary" -1)
person: bad age: -1
> (person 10 10)
person: bad name: 10
```

```
#:methods interface-expr [body ...]
```

关联对应于一个通用接口的结构类型的方法定义。例如，为 gen:dict 实现方法允许结构类型的实例用于字典。为 gen:custom-write 实现方法允许定制一个结构类型的实例怎样被显示。

例子:

```
> (struct cake (candles)
          #:methods gen:custom-write
          [(define (write-proc cake port mode)
             (define n (cake-candles cake))
             (show "   ~a   ~n" n #\. port)
             (show " .-~a-. ~n" n #\| port)
             (show " | ~a | ~n" n #\space port)
             (show "---~a---~n" n #\- port))
            (define (show fmt n ch port)
             (fprintf port fmt (make-string n ch)))])
> (display (cake 5))
   .....   
 .-|||||-. 
 |       | 
-----------
```

```
#:property prop-expr val-expr
```

对结构类型关联一个属性和值。比如，prop:procedure 属性允许结构类型实例可以作为函数被使用；属性值决定当把结构作为函数使用时，调用如何实现。

例子：

```
> (struct greeter (name)
          #:property prop:procedure
                     (lambda (self other)
                       (string -ppend
                        "Hi " other
                        ", I'm " (greeter-name self))))
(define joe-greet (greeter "Joe"))

> (greeter-name joe-greet)
"Joe"
> (joe-greet "Mary")
"Hi mary, I'm Joe"
> (joe-greet "John")
"Hi John, I'm Joe"
```

```
#:super super-expr
```

给 struct-id 提供 super-id 的一种替代方案。区别于结构类型的名字(不是表达式)，super-expr 可以生成结构类型描述器的值。#:super 更好的一点是，结构类型描述器是值，所以可以被传递给函数。

例如:

```
(define (raven-constructor super-type)
    (struct raven ()
            #:super super-type
            #:tanransparent
            #:property prop:procedure (lambda (self)
                                        'nevermore))
    raven)

> (let ([r ((raven-constructor struct:posn) 1 2)])
    (list r (r)))
(list (raven 1 2) 'nevermore)
> (let ([r ((raven-constructor struct:thing) "apple")])
    (list r (r)))
(list (raven "apple") 'nevermore)
```

## 6 模块

模块使你在多个文件和可复用的库中组织 racket 代码。

### 6.1 模块的基本介绍

每个 racket 模块基本都位于它所在的文件。例如，假设 ”cake.rkt“ 文件包含下面的模块:

```
“cake.rkt“
```
```
#lang racket

(provide print-cake)

; draws a cake with n candles
(define (print-cake n)
    (show "   ~a   " n #\.)
    (show " .-~a-. " n #\|)
    (show " | ~a | " n #\space)
    (show "    a   " n #\-))

(define (show fmt n ch)
    (printf fmt (make-string n ch))
    (newline))
```

那么，其他模块可以导入“cake.rkt”使用 print-cake 函数，因为“cake.rkt”中provide 那行明确地导出了 print-cake 的定义。show 函数是”cake.rkt“私有的(即它不能被其他模块使用)，因为 show 函数没有被导出。

下面的”random-cake.rkt“模块引入了”cake.rkt“：

```
”random-cake.rkt“
```
```
#lang racket

(require "cake.rkt")

(print-cake (random 30))
```

如果”cake.rkt“和”random-cake.rkt“模块位于同一目录，那么引入的(require "cake.rkt")可以使用。unix 风格的相对路径作用于所有平台上的模块引用，非常类似HTML页面中的相对路径。

#### 6.1.1 组织模块

”cake.rkt“和”random-cake.rkt“例子最常用的方式来组织模块中的程序：把所有模块文件放在单一目录（可能有子目录），然后通过相对路径持有彼此的模块引用。因为该单一目录可以在文件系统上移动或复制到其他机器上，相对路径保存模块间的联系，所以这些模块的目录可以当成一个项目。

另一个程序，假如你正在创建糖果分类的程序，你可能需要一个使用其他模块访问数据库并且控制分类程序的”sort.rkt“模块。如果这个糖果数据库模块本身已被组织进处理条形码和制造商信息的子模块，那么数据库模块可以是“db/lookup.rkt”，它使用帮助模块“db/barcodes.rkt”和“db/makers.rkt”。类似地，分类程序驱动”machine/control.rkt“可以使用帮助模块”machine/sensors.rkt“和”machine/actuators.rkt“。

![xxx](https://docs.racket-lang.org/guide/pict.png "optional title")

"sort.rkt"模块使用相对路径”db/lookup.rkt“和”machine/control.rkt“来从数据库和控制程序导入：

```
”sort.rkt“
```
```
#lang racket
(require ”db/lookup.txt“ ”machine/control.rkt“)
....
```

”db/lookup.rkt“模块同样使用相对它自身的路径访问”db/barcodes.rkt“和”db/makers.rkt“模块：

```
”db/lookup.rkt“
```
```
#lang racket
(require "barcode.rkt" "makers.rkt")
....
```

"machine/control.rkt"同上：

```
”machine/control.rkt“
```
```
#lang racket
(require "sensors.rkt" "actuators.rkt")
....
```

racket 工具自动使用相对路径运行。例如

```
racket sort.rkt
```

在命令行运行”sort.rkt“程序，自动加载和编译引入的模块。在一个足够大的程序中，从源码编译时间非常长，所以使用

```
raco make sort.rkt
```

来进行编译”sort.rkt“和所有它的依赖为字节码文件。当运行 *racket sort.rkt* 时，字节码存在会自动使用字节码。

#### 6.1.2 库集合（Library Collections）

集合是已安装库模块的分层分组形式。集合中的模块通过非引用、无后缀的路径被引用。例如，如下模块指向"racket"集合中的”date.rkt“库。

```
#lang racket

(require racket/date)

(printf "Today is ~s\n"
        (date->string (seconds->date (current-seconds))))
```

当你搜索在线 racket 文档，搜索结果指明提供每个绑定的模块。或者，如果你通过点击超链接到达绑定的文档，你可以将鼠标悬停在绑定名称上，找出哪些模块提供了绑定名陈。

像 racket/date 这样的引用看起来想一个标识符，但是对待方式和 printf 或者 date->string 不一样。取而代之的是，当 require 看到一个未用引号的模块引用，它会把这个模块引用转换为基于集合（collection-based）的模块路径。

- 首先，如果这个未使用引号的路径不包含 /，那么 require 自动 添加”/main“到这个模块引用。例如，(require slideshow) 等价于 (require slideshow/main)。

- 其次，require 隐式地添加".rkt"后缀到该路径。

- 最后，require 通过搜索已安装的库的集合来解析出路径，而不是把路径看成相对于当前模块路径。

对于上述第一点，模块集合是当作文件系统目录实现的。例如，”racket“ 集合通常位于 racket 安装目录的 ”collect“目录里的 ”racket“目录，用如下方式显示

```
#lang racket

(require setup/dirs)

(build-path (find-collects-dir) ; main collection directory
            "racket")
```

然而，racket 安装目录的 ”collect“ 目录只是唯一的可供 require 查找的地方。其他地方包括通过 (find-user-collects-dir) 报告出的用户制定的目录和通过 PLTCOLLECTS 搜索路径配置的目录。最后，也是最经典的，集合通过安装过的包来查找。

#### 6.1.3 包和集合（Packages and Collections）

包是一组通过包管理器安装过的库的集合（或作为预装在Racket发行版中的库）。例如, racket/gui 库由”gui“包提供，而 parser-tools/lex 由”parser-tools“库提供。

racket 程序不直接引用包。取而代之的是，程序通过集合引用库，并且添加和移除一个包来使得基于集合的一组库可用。单独的包可以在多个集合中提供库，两个不同的包可以在同一个集合中提供库（但不是同一个库，并且包管理器确保不能和已安装的包冲突）。

#### 6.1.4 添加集合（Adding Collections）

回看组织模块那节的糖果分类的例子，假设”db/“和”machine/“模块需要共同的帮助函数的集合。帮助函数可以放进”utils/“目录，那么”db/“和”machine/“中的模块可以可以通过以”../utils/“开头的相对路径访问工具模块。只要一组模块在单个目录中协同工作，最好坚持使用相对路径。程序员可以在不知道 racket 配置的情况下跟踪相对路径的引用。

有些库需要跨多个项目被使用，因此将库源码保存在它使用的目录不符合场景。在这种情况下，最好的建议是添加一个新的集合。当该库在一个集合了之后，它可以使用不带引号的地址来引用，就像那些 racket 发行版中包含的库一样。

你可以通过把文件放到 racket 安装目录或者通过(get-collects-search-dirs)报告出来的目录出来新增一个集合。或者你添加到通过设置 PLTCOLLECTS 环境变量的目录列表。然而，最好的建议是添加一个包。

创建一个包不是意味着你必须通过包服务器注册或者执行一个复制你的源码到文档格式的捆绑步骤。创建包可以简单的理解为使用包管理器来使你的库作为集合（基于源码位置），从而本地可访问，

例如，假设你有一个包含”cake.rkt“模块和其他关联模块的目录”/usr/molly/bakery“。为了使这些模块当作”bakery“集合可用，你可以

- 使用 raco pkg 命令行工具:

```
raco pkg install --link /usr/molly/bakery
```

当提供的路径包含文件分割符，这里的 --link 标识实际上是不需要的。

- 使用 DrRacket 中 File 菜单的 Package Manager。在 Do What I Mean 面板，点击 Browse...，选择”/usr/molly/bakery“目录，然后点击 Install。

之后，从任何模块(require bakery/cake)都会从”/usr/molly/bakery/cake.rkt“导入 print-cake 函数。

默认情况下，安装的目录名同时用于用于包名和集合名。同时，包管理器通常默认为当前用户安装，而不是 racket 安装的所有用户。

如果你准备分发你的库给其他人，慎重选择集合和包的名字。集合的命名空间是分层次的，但是顶层的集合名字是全局的，包的命名空间是扁平的。

当你的库被放进集合后，你可以一直使用 raco 编译库源码，但是更好和更方便的是使用 raco setup。raco setup 命令行接受集合名（不是文件名）并编译集合内的所有库。此外，raco 可以建立集合的文档并添加到文档索引，如集合中的“info.rkt”模块所指定的。

### 6.2 模块语法

模块文件开始处的 #lang 开启一个模块形式的缩写，非常类似 quote 形式的缩写。不像 ' ，#lang 缩写在 REPL 中不能很好工作，一个原因是它必须在文件结尾处终止，另一个原因是 #lang 的长扩展表达式依赖封闭的文件名称。

#### 6.2.1 module形式（The module Form）

模块的非缩写形式在 REPL 和文件中都工作的很好

```
(module name-id initial-module-path
    decl ...)
```

这里的 name-id 是模块名，initial-module-path 是初始化导入，每个decl都是一个导入、导出、定义或者表达式。在一个文件里，name-id通常匹配的是文件名出去目录路径和后缀，但是当模块通过文件路径引入，name-id 被忽略。

initial-module-path 是必须的，因为即使是 require 形式也必须导入，以便在模块体中进一步使用。换句话说，initial-module-path 导入引导了模块体中的语法。最通常被用的 initial-module-path 是 racket，它提供了本指南中描述的大多数绑定，包括 require、define 和 provide。另一个常用的 initial-module-path 是 racket/base，它提供的功能稍少一些，但是仍然提供了许多最常用的功能和语法。

例如，上一节的”cake.rkt“可以被写为

```
(module cake racket
    (provide print-cake)
    
    (define (print-cake n)
        (show "   ~a   " n #\.)
        (show " .-~a-. " n #\|)
        (show " | ~a | " n #\space)
        (show "---~a---" n #\-))
    
    (define (show fmt n ch)
        (printf fmt (make-string n ch))
        (newline)))
```

此外，这个 module 形式可以在 REPL 中执行来声明一个和任何文件无关的 cake 模块。quote 这个模块名，以引用这个无关联的模块，

例子：

```
> (require 'cake)
> (print-cake 3)
   ...   
 .-|||-. 
 |     | 
---------
```

声明一个模块不会立即执行模块体的定义和表达式。模块必须在顶层导入来触发执行。当执行被触发一次后，之后的 require 不会再次执行模块体。

例如：

```
> (module hi racket
    (printf "Hello\n"))
> (require 'hi)
Hello
> (require 'hi)
```

### 6.2.2 #lang 缩写

#lang 缩写的模块体没有特别的语法，因为语法决定于 #lang 后面的语言。

以 #lang racket 为例，语法为

```
#lang racket
decl ...
```

它被翻译为

```
(module name racket
    decl ...)
```

这里的 name 从包含 #lang 形式的文件名生成。

#lang racket/base 形式与 #lang racket 有相同的语法，除了展开后使用 racket/base 代替 racket。相反，#lang scribble/manual 形式，有一个完全不同的语法，甚至看起来不像 racket，我们不打算在本指南中描述。

除非另有规定，一个使用 #lang 表示法以”language“标明的模块将会和 #lang racket 一样展开模块。标明的语言名也可以直接和 module 或者 require 使用。

### 6.2.3 子模块（Submodules）

一个模块可以被嵌套进另一个模块，这样的话，被嵌套的模块声明了一个子模块。子模块可以被包围的模块以”quote 模块名“的方式直接引用。下面的例子从 zoo 子模块导入 tiger 并打印 ”Tony“： 

```
"park.rkt"
```
```
#lang racket
(module zoo racket
    (provide tiger)
    (define tiger "Tony"))

(require 'zoo)

tiger
```

运行一个模块不必运行它的子模块。上面的例子中，运行”park.rkt“就运行它的子模块”zoo“只是因为”park.rkt“模块
导入了 zoo 子模块。否则，一个模块和它的每个子模块可以独立运行。此外，如果”park.rkt“被编译成字节码文件（通过 raco make），那么”park.rkt“的字节码或者 zoo 的字节码将被独立加载。

子模块可以被嵌套进子模块，并且子模块可以被模块直接引用，而不是像外层模块一样使用子模块路径。

module* 形式类似嵌套模块的形式：

```
(module* name-id initial-module-path-or-#f
    decl ...)
```

module* 形式和 module 不同的是，它反转了子模块和外层模块的引用。

- module 形式的子模块可以被外层模块导入，但是子模块不能导入外层模块或从外部引用外层模块的绑定。

- module* 形式的子模块可以导入外层模块，但是外层模块不能导入子模块。

此外，module* 形式可以指定 #f 替换 initial-module-path，在这种情况下，子模块可以看到所有外层模块的绑定（包括没有被 provide 导出的绑定）。

module* 形式的子模块和 #f 的一个用处是通过从外层模块没有正常导出的子模块来导出额外的绑定。

```
”cake.rkt“
```
```
#lang racket

(provide print-cake)

(define (print-cake n)
    (show "   ~a   " n #\.)
    (show " .-~a-. " n #\|)
    (show " | ~a | " n #\space)
    (show "---~a---" n #\-))

(define (show fmt n ch)
    (printf fmt (make-string n ch))
    (newline))

(module* extras #f
    (provide show))
```

在这个修改过的”cake.rkt“模块中，show 函数被有被使用 (require "cake.rkt") 的模块导入，因为大部分”cake.rkt“的调用端不想要这个额外的方法。模块可以使用 (require (submod "cake.rkt" extras)) 来导入额外的 子模块来访问隐藏的 show 方法。

#### 6.2.4 main 和测试子模块（Main and Test Submodules）

下面的”cake.rkt“的变体包含一个 调用 print-cake 的 main 子模块:

```
"cake.rkt"
```
```
#lang racket

(define (print-cake n)
    (show "   ~a   " n #\.)
    (show " .-~a-. " n #\|)
    (show " | ~a | " n #\space)
    (show "---~a---" n #\-))

(define (show fmt n ch)
    (printf fmt (make-string n ch))
    (newline))

(module* main #f
    (print-cake 10))
```

运行一个模块不会运行它 module* 定义的子模块。尽管如此，通过 racket 和 DrRacket 运行上面的模块会打印带用10根蜡烛的蛋糕，因为 main 子模块是一个特例。

当模块当作程序名提供给 racket 执行器或直接在 DrRacket 内运行，如果该模块有一个 main 子模块，在外围模块加载后子模块会被执行。因此当想直接执行一个模块时，定义一个指定额外执行动作的 main 子模块，而不是作为库导入进一个更大的程序。

main 子模块不必非得用 module* 声明。如果 main 模块不需要使用外围模块的绑定，它可以用 module 声明。更常见的是，main 用 module+ 声明：

```
(module+ name-id
    decl ...)
```

module+ 定义的子模块就像用 #f 作为 initial-module-path 的 module* 定义的子模块。另外，多个 module+ 可以指定同一个子模块名，这种情况下，module+ 形式的模块体被合并起来创建一个单独的子模块。

module+ 这种合并的行为对定义 test 子模块特别有用，这样可以方便地被 *raco test* 以同 main 一样的方式运行。比如，下面的"physics.rkt" 模块导出了函数 drop 和 to-energy，并且定义了一个 test 模块来进行测试:

```
"physics.rkt"
```
```
#lang racket
(module+ test
    (require rackunit)
    (define ε 1e-10))

(provide drop
         to-energy)

(define (drop t)
    (* 1/2 9.8 t t))

(module+ test
    (check-= (drop 0) 0 ε)
    (check-= (drop 10) 490 ε))

(define (to-energy m)
    (* m (expt 299792458.0 2)))

(module+ test
    (check-= (to-energy 0) 0 ε)
    (check-= (to-energy 1) 9e+16 1e+15))
```

如果该模块被编译，导入"physics.rkt"到一个更大的程序不执行 drop 和 to-energy 的测试，甚至不出发加载测试代码。但是在命令行执行 *raco test physics.rkt* 将会执行测试。

上面的"physics.rkt"等价于使用 module*：

```
"physics.rkt"
```
```
#lang racket
(provide drop
         to-energy)

(define (drop t)
    (* 1/2 49/5 t t))

(define (to-energy m)
    (* m (expt 299792458 2)))

(module* test #f
    (require rackunit)
    (define ε 1e-10)
    (check-= (drop 0) 0 ε)
    (check-= (drop 10) 490 ε)
    (check-= (to-energy 0) 0 ε)
    (check-= (to-energy 1) 9e+16 1e+15))
```

使用 module+ 代替 module* 允许 test 分散在函数定义处。

module+ 的合并行为有时对 main 模块同样有用。就算不必合并时，凭(module+ main .... )比(module* main #f)更可读，它应该也是首选写法。

### 6.3 模块路径

模块路径，被作为 require 和 initial-module-path 处使用，是一个模块的参考。它是下面集中形式中的一种：

- \(quote id\)

引号标识符的模块路径使用标识符指向一个非文件模块。这种模块形式的用法大多在 REPL 使用。

例如:

```
> (module m racket
    (provide color)
    (define color "blue"))
> (module n racket
    (require 'm)
    (printf "my favorite color is ~a\n" color))
> (require 'n)
my favorite color is blue
```

- rel-string

模块地址字符串是一个 unix 风格的相对地址(/ 是路径分割符、.. 指向父级目录、. 指向当前目录)。rel-string 不能是以路径分割符开头或结尾。如果路径没有后缀，会自动加上".rkt"。

如果有外层模块的话，路径指向外层模块，否则指向当前目录。（更准确地说，路径指向(current-load-relative-directory)的值，它指明了从哪加载文件。）

Module Basics 展示了使用相对路径的例子。

如果相对路径以".ss"后缀结尾，它会被装换为".rkt"。如果实现这个模块的文件实际确实以".ss"结尾，当尝试加载文件时，后缀将会被改回来（但是以".rkt"后缀为准）。这两种转换提供了与旧版本 racket 的兼容性。

- id

未被引用的标识符模块路径，指向一个已安装的库。id 被约束为只能是 ASCII 字符、ASCII 数字、+、-、_、和 /，这里的 / 用来分割标识符中的路径元素。这些元素指向集合和子集合，而不是目录和子目录。

这种形式的一个例子是 racket/date。它指向一个在”racket.rkt“集合中，源码地址为”date.rkt“的文件，它作为 racket 的一部分被安装。".rkt"后缀会被自动加上。

这种形式的另一个例子是 racket，它通常被用于初始导入。racket 是 racket/main 的缩写。当 id 没有 /，那么 /main 会自动被加到结尾。因此，racket 和 racket/main 都指向在”racket“集合中，源码为”main.rkt“的文件。

例如：

```
> (module m racket
    (require racket/date)
    
    (printf "Today is ~s\n"
        (date->string (seconds->date (current-seconds)))))
> (require 'm)
Today is "Monday, November 18th, 2019"
```

当有以”.rkt“结尾的模块的路径，如果没有这个文件，但是存在”.ss“后缀的文件，那么会自动换为".ss"后缀。这种转换提供了与旧版本 racket 的兼容。

- \(lib rel-string\)

就像一个未加引号的标识符路径，但是以字符串而非标识符表示。此外，rel-string 可以以文件后缀结尾，在这种情况下，”.rkt“不会被自动添加。

包含(lib "racket/date.rkt")和(lib "racket/date")的这种形式的例子，等价于 racket/date。其它包含(lib "racket")、(lib "racket/main")和(lib "racket/main.rkt")的例子，等价于 racket。

例子：

```
> (module m (lib "racket")
    (require (lib "racket/date.rkt"))
    
    (printf "Today is ~s\n"
            (date->string (seconds->date (current-seconds)))))
> (require 'm)
Today is "Monday, November 18th, 2019"
```

- (plant id)

访问部署在 PlaneT server 上的第三方库。该库在背需要时第一时间下载，之后便使用本地拷贝。

该 id 编码由 / 分割的几条信息：包所有者，带可选版本信息的包名和包指定库的可选路径。就像像 id 作为一个库路径的缩写，”.rkt“后缀会被自动加上，并且没有子路径元素 /main 会自动加上。

例子:

```
> (module m (lib "racket")
    ; Use "schematics"'s "random.plt" 1.0, file "random.rkt":
    (require (planet schematics/ramdom:1/random))
    (display (random-gaussian)))
> (require 'm)
0.9050686838895684
```

与其他形式一样，如果没有用“.rkt”结尾的实现文件，则可以自动替换以“.ss”结尾的实现文件。

- (planet package-string)

就像 planet 的符号形式，但是使用字符串代替标识符。此外，当”.rkt“没有添加的时候，package-string 可以带文件后缀。

与其他形式一样，如果没有用“.rkt”结尾的实现文件，则可以自动替换以“.ss”结尾的实现文件。

```
(planet rel-string (user-string pkg-string vers ...))

  vers = nat
       | (nat nat)
       | (= nat)
       | (+ nat)
       | (- nat)
```

一个更通用的形式来访问来自PLaneT server上的库。在这个通用形式，PLaneT 引用像基于相对地址的库的引用一样，但是地址后面跟了生产者，包，版本的信息。这个制定的包在需要时下载和安装。

当版本号是一个非负的整数序列，vers 指定该包可使用版本的约束。如果没有提供约束，则任何版本都被允许。特别地，省略所有版本意味着任何版本你都可以。强烈建议至少指定一个版本。

对于版本约束，纯 nat 和 (+ nat) 一个意思，这匹配 nat 或高于对应版本号。(start-nat end-nat) 匹配他们之间的数字。(= nat) 严格匹配 nat。(- nat) 匹配 nat 或更低。

```
> (module m (lib "racket")
    (require (planet "random.rkt" ("schenatics" "random.plt" 1 0)))
    (display (random-gaussian)))
> (require 'm)
0.9050686838895684
```

".ss" 和 ".rkt" 会像其他形式一样自动转换。

- \(file string\)

引用一个文件，其中string是使用当前平台惯例的相对或绝对路径。这种形式不可移植，当可移植的 rel-string 可以使用时，不应该使用它。

".ss" 和 ".rkt" 会像其他形式一样自动转换。

```
(submod base element ...+)
    base = module-path
         | "."
         | ".."
 element = id
         | ".."
```

只想 base 的子模块。submod 中的 elements 序列指定到达最终子模块的子模块名的路径。

例如：

```
> (module zoo racket
    (module monkey-house racket
        (provide monkey)
        (define monkey "Curious George")))
> (require (submod 'zoo mokey-house))
> monkey
"Curious George"
```

在 submod 中使用"."作为 base 代表外层模块。使用 ".."作为 base 等价于使用"."后跟额外的".."。当这个形式的路径指向子模块时，等价于 (submod "." id)。

使用”..“作为元素取消一个子模块步骤，能有效的引用外层模块。例如，(submod "..")指向当前模块的外层模块。

```
> (module zoo racket
    (module monkey-house racket
      (provide monkey)
      (define monkey "Curious George"))
    (module crocodile-house racket
      (require (submod ".." monkey-house))
      (provide dinner)
      (define dinner monkey)))
> (require (submod 'zoo crocodile-house))
> dinner
"Curious George"
```

### 6.4 导入:require

导入形式用来从其他模块导入。require 可以出现在模块内部，在这种情况下，它从指定模块引进绑定到导入模块。require 形式同样可以出现在顶层，在这个情况下，它既引入绑定也实例化指定模块；这表示，如果被引入模块还没执行，reqiure会执行指定模块体的定义和表达式。

一个 require 一次可以指定多个导入：

```
(require require-spec ...)
```

在一个 require 中指定多个 require-spec 基本和多次 require 每次 require 一个一样。 两种 require 区别很小，仅限于顶层(top-level)不同:单一的 require 可以导入一个给定的标识符最多一次，而分割的 require 可以替换前一个 require 的绑定（必须都在顶层中(top-level)并且在模块外）。

被允许的 require-spec 形式是递归定义的：

```
module-path
```

最简单的形式，一个 require-spec 是一个 module-path(就像前一章定义的那样)。在这种情况下，被 require 引入的绑定决定于 module-path 指向的模块的 provide 声明。

例子：

```
> (module m racket
    (provide color)
    (define color "blue"))
> (module n racket
    (provide size)
    (define size 17))
> (require 'm 'n)
> (list color size)
'("blue" 17)
```

```
(only-in require-spec id-may-be-renamed ...)

  id-may-be-renamed = id
                    | [orig-in bind-id]
```

only-in 形式限制将要被基础 require-spec 引入的绑定的集合。此外，only-in 可选择重命名每一个绑定：在 \[orig-id bind-id\]形式中，orig-id 指向被 require-spec 导入的绑定，bind-id 是在导入上下文中取代 orig-id 将要被导入的名字。

例子:

```
> (module m (lib "racket")
    (provide tastes-great?
             less-filling?)
    (define tastes-great? #t)
    (define less-filling? #t))
> (require (only-in 'm tastes-great?))
> tastes-great?
#t
> less-filling?
less-filling?: undefined;
 cannot reference an identifier before its definition
  in module: top-level
> (require (only-in [less-filling? lite?]))
> lite?
#t
```

```
(except-in require-spec id ...)
```

这种形式是 only-in 的补充：它从指定集合中排查特定的绑定。

```
(rename-in require-spec [orig-id bind-id] ...)
```

这个形式向 only-in 一样支持重命名，但是保留 require-spec 中未作为 orig-id 提到的标识符。

```
(prefix-in prefix-id require-spec)
```

这是重命名的简写，prefix-id 被添加到 require-spec 指定的标识符的前面。

only-in、except-in、rename-in 和 prefix-in 形式可以嵌套以实现被导入的绑定更复杂的操作。比如：

```
(require (prefix-in m: (except-in 'm ghost)))
```

导入 m 导出的所有绑定，除了 ghost 绑定，并且本地的名字都会加上 m: 前缀。

等价地，prefix-in 可以被应用到 except-in 之前：

```
(require (except-in (prefix-in m: 'm) m:ghost))
```

### 6.5 导出:provide

默认情况下，模块所有的定义都是私有的。provide 形式指定哪些模块可被导入。

```
(provide provide-spec ...)
```

provide 只可能出现在模块一级（模块体中）。在一个 provide 中指定多个 provide-spec 和 使用多个 provide 每次指定一个 provide-spec 一样。

一个模块中所有的 provide中，每个标识符最多能被导出一次。更准确地说，每个 export 的外部名必须是不同的。同一个内部名可以以不同的外部名被导出多次。

被允许的 provide-spec 形式如下:

```
identifier
```

在这个最简单的形式中，provide-spec 表明模块中的一个绑定会被导出。这个绑定可以是一个本地定义，也可以是一个导入。

```
(rename-out [orig-id export-id] ...)
```

rename-out 形式类似于仅仅指定标识符，但是对于导入模块，导出的绑定 orig-id 会给出另外一个名称，export-id。

```
(struct-out struct-id)
```

struct-out 形式导出被 (struct struct-id ...) 创建的绑定。

```
(all-defined-out)
```

all-defined-out 缩写导出被模块定义的所有绑定。（不包括被导入的）

all-defined-out 缩写形式通常不被建议，因为它使得模块的导出模糊，并且 racket 程序员习惯于认为定义可以自由地添加到模块中，而不影响其公共接口（当使用 all-defined-out 时，情况并非如此）。

```
(all-from-out module-path)
```

all-from-out 缩写导出模块中所有用基于 module-path 使用 require-spec 导入的绑定。

虽然不同的 module-apth 会指向相同的基于文件的模块，但是使用 all-from-out 重复导入基于指定 module-path 的引用，而不是实际的引用。

```
(except-out provide-spec id ...)
```

像 provide-spec 一样，但是如果 id 是要忽略的外部绑定名称，不会导出 id。

```
(prefix-out prefix-id provide-spec)
```

像 provide-spec 一样，但是添加 prefix-id 到每个要导出的绑定的外部名称的前面。

### 6.6 分配和重定义（Assignment and Redefinition）

定义在模块中的变量的 set! 用法受限于定义的方法体。也就是说，模块被允许在自己的定义中修改值，并且这些修改对导入模块可见。然而导入模块不允许修改导入绑定的值。

例如:

```
> (module m racket
    (provide counter increment!)
    (define counter 0)
    (define (increment!)
        (set! counter (add1 counter))))
> (require 'm)
> counter
0
> (increment!)
> counter
1
> (set! counter -1)
set!: cannot mutate module-required identifier
    at: counter
    in: (set! counter -1)
```

正如上面例子表明，模块总是可以授予其他模块通过函数修改其导出的值的能力，例如 increment!。

禁止分配被导入的变量有利于支持程序的模块化推导。比如，这个模块，

```
(module m racket
    (provide rx:fish fishy-string?)
    (define rx:fish #rx"fish")
    (define (fishy-string? s)
        (regexp-match? rx:fish s)))
```

函数 fishy-string? 总是匹配字符串包含"fish"，不管其他模块怎么使用 rx:fish 绑定。基本上这也帮助了程序员，禁止导入的绑定进行分配值同样使得很多程序运行的更有效。

同样的道理，当模块不包含在模块中定义的特殊标识符的 set!，那么标识符被认为是一个常量，即使重新声明后也不能更改。

因此，通常不允许重新声明模块。对于基于文件的模块，简单的更改文件在任何情况下都不会导致重新声明，因为基于文件的模块按需加载，并且之前加载的声明满足将来的需求。用 racket 的反射重新生命一个模块是可以的，然而，只有非文件的模块在 REPL 中可被重新声明。在这种情况下，如果该模块涉及之前常量绑定的重新声明，将会出错。

```
> (module m racket
    (define pie 3.141597))
> (require 'm)
> (module m racket
    (define pie 3))
define-values: assignment disallowed;
 cannot re-define a constant
  constant: pie
  in module: 'm
```

出于探索和调试的目的，racket 的反射层提供一个 compile-enforce-module-constants 参数用来禁用常量的强制。

```
> (compile-enforce-module-constants #f)
> (module m2 racket
    (provide pie)
    (define pie 3.141597))
> (require 'm2)
> (module m2 racket
    (provide pie)
    (define pie 3))
> (compile-enforce-module-constants #t)
> pie
3
```

### 6.7 模块和宏（Modules and Macros）

racket 的模块系统与 racket 的宏紧密合作，为 racket 增加了新的语法形式。比如，以导入 racket/base 为 require 和 lambda 引入语法相同，导入其他模块可以引入新的语法形式（除了更传统的导入类型，比如函数和常量）。

我们之后会介绍更多宏的细节，但是这里有一个简单的例子，有关定义一个基于模式（pattern-based）的宏的模块：

```
(module noisy racket
    (provide define-noisy)
    
    (define-syntax-rule (define-noisy (id arg ...) body)
        (define (id arg ...)
            (show-argments 'id (list arg ...))
            body))
    
    (define (show-argments name args)
        (printf "call ~s with argments ~e" name args)))
```

被模块提供的 define-noisy 绑定是一个宏，它扮演的角色类似于 define 之于 函数,但是它造成函数的每次调用都打印函数的参数：

```
> (require 'noisy)
> (define-noisy (f x y)
    (+ x y))
> (f 1 2)
calling f with argments '(1 2)
3
```

大致上，define-noisy 形式就是把

```
(define-noisy (f x y)
    (+ x y))
```

替换为

```
(define (f x y)
    (show-argments 'f (list x y))
    (+ x y))
```

由于 show-argments 没有被 noisy 模块提供，因此这里的文本替换并不完全正确。实际的替换会正确地跟踪标识符（如show-argments）的来源，因此它们可以在定义宏的位置引用其他定义，即使这些标识符在使用宏的位置不可用。

宏和模块i的交互远不止标识符绑定。defined-syntax 形式本身就是一个宏，它展开成编译时代码(实现从 define-noisy 到 define 的转换)。模块系统跟踪哪些代码需要在编译时运行，那些代码正常运行。

## 7 合约(Contracts)

本章对 racket 的合约系统做简单介绍。

### 7.1 合约和边界（Contracts and Boundaries）

就像两个商业伙伴之间的合同一样，软件合约就是双方之间的协议。这个协议制定了从一方移交到另一方时的每个产品（值）的义务和保证。

合同从而在双方之间确定了一个边界。无论何时，当一个值越界，合约监控系统执行检查，确保对方遵守已确定的合约。

本着这种精神，racket 鼓励在模块边界签订合约。具体来说，程序员可以通过合约来提供条款，从而对导出值的使用添加约束和承诺。比如，导出规范

```
#lang racket

(provide (contract-out [amount positive?]))

(define amount ...)
```

对上述模块的所有调用端许诺 amount 的值总是一个正数。合约系统会仔细监控模块。每当一个调用方指向amount，监视器都会检查 amount 的值的确是一个正数。

合约库已经被内置到 rackt 语言中，但是如果你想使用 racket/base 你像这样可以明确导入合约库:

```
#lang racket/base
(require racket/contract) ; now we can write contracts

(provide (contract-out [amount positive?]))

(define amount ...)
```

#### 7.1.1 Contract Violations (合约违规)

如果我们绑定 amount 到一个非正数，

```
#lang racket

(provide (contract-out [amount positive?]))

(define amount 0)
```

那么当这个模块被导入时，监控系统会发出合同的违规信号，并责怪模块违背了约定。

一个更大的错误，绑定 amount 到一个非数值值：

```
#lang racket

(provide (contract-out [amount positive?]))

(define amount 'amount)
```

在这种情况下，监视系统会对一个符号使用 positive?,但是 positive? 报告一个错误，因为它的域只是数字。为了使合约捕获我们所有 racket 的值的意图，我们可以使用 and/c 组合两个合约来确保这个值既是数字也是正的。

```
(provide (contract-out [amount (and/c number? positive?)]))
```

#### 7.1.2 试验合约和模块（Experimenting with Contracts and Modules）

这章中的合约和模块（不包括下面那些）都是使用标准 #lang 语法来描述模块的。由于模块是合同双方之间的边界，因此例子涉及多个模块。

为了实验单个模块中的多个模块或者 DrRacket 的定义区，我们使用 racket 的子模块。例如，尝试本章前面的示例，就像这样：

```
#lang racket

(module+ server
    (provide (contract-out [amount (and/c number? positive?)]))
    (define amount 150))

(module+ main
    (require (submod ".." server))
    (+ amount 10))
```

这里的每个模块和合约都使用 module+ 包围。module 后的第一个形式是要在随后的 require 语句中使用的名称（通过 reuire 的每个引用都以 ".." 作为前缀）。

#### 7.1.3 试验嵌套合约边界（Experimenting with Nested Contract Boundaries）

在许多情况下，在模块边界附加合约是有意义的。这十分方便，然而，能够比模块更细力度的使用合约。define/contract 形式可以这样使用：

```
#lang racket

(define/contract amount
    (and/c number? positive?)
    150)

(+ amount 10)
```

在这个例子中，define/contract 形式在 amount 的定义和它所在的上下文中建立合约。换句话说，这里合约的双方是定义和包含定义的模块。

创建这些嵌套合约边界的形式有时很难使用，因为可能会产生意想不到的性能影响。

### 7.2 函数的简单合约（Simple Contracts on Functions）

数学函数有域和范围。域指定函数可接受为参数的值的类型，范围指定它生成的值的类型。带域和范围的描述函数的传统的符号是这样

```
f : a -> B
```

这里的 A 是函数的域，B 是范围。

编程语言的汗是同样有域和范围，并且合约可以确保函数接受只在其域之内的值，只生成在其范围之内的值。A -> 为函数创建这样一个合约。-> 后面的形式指定这个域的合约和最终范围的合约。

这里有一个表示银行账户的模块：

```
#lang racket

(provide (constract-out
          [deposit (-> number? any)]
          [balance (-> number?)]))

(define amount 0)
(define (desposit a) (set! amount (+ amount a)))
(define (balance) amount)
```

该模块导出两个函数：

- deposit, 接受一个数字并返回一些在合约中未被指定的值

- balance, 返回一个数字指定账户的当前余额

当模块到处函数时，它在它自己作为服务器和导入函数作为客户端之间建立两个通信通道。如果客户端模块调用该函数，它就会发送值到服务端模块。相对的，如果这个函数调用完，该函数会返回值，服务端模块发送值返回到客户端模块。这个客户端-服务端区别是重要的，因为当发生错误时，一方或另一方会被阻挠。

如果客户端使用 'millions 调用 deposit，它就会违反合约。合约监控系统将会捕捉这次违例并且责备客户端打破了关于上述模块的合约。相反的，如果 balance 返回 'broke，合约监控系统将会责备服务端模块。

-> 自身不是合约，它是合约连接符，它组合其它合同为一个合同。

#### 7.2.1 -> 的形式

如果你习惯于数学函数，你可能喜欢箭头出现在域和范围之间，而不是在开头。如果你已经阅读 How to Design Programs，你已经看到很多次了。的确，你可能看过许多其他人的代码里的这样的合同。

```
(provide (contract-out
          [desposit (number? . -> . any)]))
```

如果 racket 的 S-表达式 包含两个点之间带一个符号，读取器会重新组合这个 s-表达式，把符号放在开头。

```
(number? . -> . any)
```

只是另一种书写方式的

(-> number? any)

#### 7.2.2 使用 define/contract 和 ->

在 Experimenting with Nested Contract Boundaries 中介绍的 define/contract 同样可以用于定义来自合同的函数。比如，

```
(define/contract (desposit amount)
    (-> number? any)
    ; implementation goes here
    ....)
```

这里用之前你的合约定义 desposit 函数。需要注意的是，使用 desposit 有两个潜在重要的影响：

1.任何在 desposit 定义之外处调用时，这个合约将会被检查（即便调用函数是在该模块内定义的）。因为可能有许多模块内的调用，这项检查可能引起该合约被检查的太频繁，这可能会引起性能问题。如果在循环中重复调用，尤其如此。

2.在某些情况下，当函数在被其模块中的其他代码调用时，可以被写成接受更宽泛的输入。这些情况下，使用 define/contract 建立合约边界就太严格了。

#### 7.2.3 any 和 any/c

用于 desposit 的 any 合约匹配任何种类的结果，并且它只可以用于函数合约的范围处。我们可以使用更具体的合约 void? 来取代上面的 any，这表明函数总是返回 (void) 值。然而，void? 合约会请求合约监控系统每当函数被调用时检查返回值，虽然调用端模块并不能对该值做什么处理。相反，any 告诉监控系统不要检查返回值，它告诉调用端：服务端模块对函数的返回值不做一丁点的承诺，甚至不保证是单一值还是多值。

any/c 合约和 any 类似，因此它对值没有要求。但是不同于 any，any/c 表示一个单一值，并且适合当作参数合约使用。当作范围合约使用 any/c 会强加一个函数产生单一值的检查。

```
(-> integer? any)
```

描述了函数接受一个整数并且返回任何数量的值，而

```
(-> integer? any/c)
```

描述了函数接受一个整数并且返回一个单一值(但是没有说有关这个值的任何事)。函数

```
(define (f x) (values (+ x 1) (- x 1)))
```

可以匹配(-> integer? any)，但是不能匹配(-> integer? any/c)。

当从函数许诺一个单一结果特别重要时，使用 any/c。当你希望尽可能少的许诺函数结果（尽可能少的检查）时,使用 any。

#### 7.2.4 定义你自己的合约（Rolling Your Own Contracts）

函数 deposit 把给定的数值加到 amount 值上。虽然函数合约防止调用端使用非数值调用该函数，但是合约依然允许调用端使用复数、负数或者不精确数值调用，他们都不能合理的代表钱的数额。

合约系统允许程序员通过函数定义自己的合约：

```
#lang racket

(define (amount? a)
    (and (number? a) (integer? a) (exact? a) (>= a 0)))

(provide (contract-out
          ; an amount is a natural number of cents
          ; is the given number an amount?
          [deposit (-> amount? any)]
          [amount? (-> any/c boolean?)]
          [balance (-> amount?)]))

(define amount 0)
(define (despost a) (set! amount (+ amount a)))
(define (balance) amount)
```

该模块定义了一个 amount? 函数并且用 -> 将它作为一个合约使用。当调用端调用与合约 (-> amount? any) 一起导出的 deposit 函数，它必须提供一个精确的，非负数的整数，否则 amount? 则会被应用到该参数并返回 f#，这会出发合约监控系统组织该客户端。类似地，服务端也会提供一个准确的，非负整数作为 balance 的值，以保持正确无误。

当然，将通信通道限制为调用端不理解的值是没有意义的。因此，服务方还应该到处 amount? 谓词本身，该合约表示它接收任意值并返回一个 boolean 值。

这种情形下，我们也可以使用 natural-number/c 代替 amount?，因为它表示完全一样的检查：

```
(provide (contract-out
          [deposit (-> natural-number/c any)]
          [balance (-> natural-number/c)]))
```

每个接受单一参数的函数都可以被看出一个谓词并且作为一个合约使用。然而，组合已存在的谓词成一个新谓词，比如 and/c 或 or/c 通常很有用，下面就是生成上面合约的另一种方式：

```
(define amount/c
    (and/c number? integer? exact? (or/c positive? zero?)))

(provice (contract-out
          [deposit (-> amount/c any)]
          [balance (-> amount/c)]))
```

值也可以作为合约提供双重职责。例如，如果函数接受一个数字或 #f，那么 (or/c number? #f) 就可以了。类似地，amount/c 合约可以被写为用 0 代替 zero?。如果使用正则表达式作为合约，那么该合约接受匹配该正则表达式的字符串和字节串。

自然，你可以通过组合，组合你拥有的实现合约的函数，就像 and/c。下面是一个从银行记录创建字符串的模块：

```
#lang racket

(define (has-decimal? str)
    (define L (string-length str))
    (and (>= L 3)
        (char=? #\. (string-ref str (- L 3)))))

(provide (contract-out
          ; covert a random number to a string
          [format-number (-> number? string?)]
          
          ; convert an amount into a string with a decimal
          ; point, as in an amount of US currrency
          [format-nat (-> natural-number/c
                          (and/c string? has-decimal?))]))
```

被导出的函数 format-number 的合约说明了，它处理一个数字并返回一个字符串。被导出函数 format-nat 的合约比 format-number 更有趣。它只消耗一个自然数。它的范围合约许诺是一个从右起第三个位置处带有 . 的字符串。

如果我们希望加强 format-nat 范围合约的许诺，以致成为只允许带数字和点的字符串，我们可以这样写：

```
#lang racket

(define (digit-char? x)
    (number x '(#\1 #\2 #\3 #\4 #\5 #\6 #\7 #\8 #\9 #\0)))

(define (has-decimal? str)
    (define L (string-length str))
    (and (>= L 3)
        (char=? #\. (string-ref str (- L 3)))))

(define (is-decimal-string? str)
    (define L (string-length str))
    (and (has-decimal? str)
        (andmap digit-char?
            (string->list (substring str 0 (- L 3))))
        (andmap digit-char?
            (string->list (substring str (- L 2) L)))))

...

(provide (contract-out
          ....
          ; convert an amount (natural number) of cents
          ; into a dollor-base string
          [format (-> natural-number/c
                      (and/c string?
                             is-decimal-string?))]))
```

或者，在这种情况下，我们可以使用正则表达式作为合约:

```
#lang racket

(provide
    (contract-out
     ...
     ; convert an amount (natural number) of cents
     ; into a dollar-based string
     [format-nat (-> natural-number/c
                     (and/c string? #rx"[0-9]*\\.[0-9][0-9]"))]))
```

#### 7.2.5 关于高阶函数的合约 (Contracts on Higher-order Functions)

函数合约不受限于仅仅在他们的域或范围上只能有简单的谓词。这里讨论的任何一个合约组合，包括函数合约本身，都可以被用于参数和返回值是函数的合约。
比如:

```
(-> integer? (-> integer? integer?))
```

在合约中描述了一个柯里化的函数。它匹配了一个函数，该函数接受一个参数，并返回一个接受一个参数在结束前返回一个整形的函数。如果服务端在该合约中导出一个 make-adderr 函数，如果 make-adder 返回值而不是函数，那么服务端将会阻止，而如果 make-adder 确实返回了一个函数，但是结果函数被应用于一个值而不是整数，那么调用方会阻止。

类似的，合约

```
(-> (-> integer? integer?) integer?)
```

描述了函数接受另一个函数作为它的输入。如果服务方使用了合约导出了函数 twice，这个 twice 被用于值而非一个参数的函数，那么调用方会组织。 如果 twice 被用于一个参数的函数，并且 twice 调用的这个给定的函数接受的值不是整数，那么服务方会阻止。

#### 7.2.6带"???"的合约消息 (Contract Messages with “???”)
假如你已经写完了你的模块，并添加了合约，你把他们放入接口，以便客户端程序员从这个接口获得所有信息。这简直是一件艺术品：

```
> (module bank-server racket
    (provide
     (contract-out
      [deposit (-> (λ (x)
                     (and (number? x) (integer? x) (>= x 0)))
                    any)]))
    (define total 0)
    (define (deposit a) (set! total (+ a total))))
```

一些客户端会使用你的模块,而其他客户端会依次使用它们。如果其中一个模块突然看到了这条错误信息：

```
> (require 'bank-server)
> (deposit -10)
deposit: contract violation
  expected: ???
  given: -10
  in: the 1st argument of
      (-> ??? any)
  contract from: bank-server
  blaming: top-level
   (assuming the contract is correct)
  at: eval:2.0
```

那么这里的???到底是干嘛的呢？如果我们对这类数据进行命名会不会好看点呢,就像我们有字符串、数字等等。

对于这类情况，Rackrt 提供了统一命名的合约。在这类术语中的"contract"用法表明合约是第一类值。“统一”表示这些数据的集合是内置原子类型数据的子集。它们被一个谓词描述，该谓词接受所有 Racket 值并生成一个布尔值。"命名"部分表示我们想做的事，即命名这个合约以便错误消息变得可理解:

```
> (module improved-bank-server racket
    (provide
     (contract-out
      [deposit (-> (flat-named-contract
                    'amount
                    (λ (x)
                      (and (number? x) (integer? x) (>= x 0))))
                   any)]))
    (define total 0)
    (define (depsoit a) (set! total (+ a total))))
```

通过这些小小的改变，错误信息变得特别可读：

```
> (require 'improved-bank-server)
> (deposit -10)
deposit: contract violation
  expected: amount
  given: -10
  in: the 1st argument of
      (-> amount any)
  contract from: improved-bank-server
  blaming: top-level
   (assuming the contract is correct)
  at: eval:5.0
```

#### 7.2.7 剖析合约错误信息

通常，合约错误信息由六个部分组成:

- 合约相关连的方法或函数，以及取决于服务端还是客户端“合同违约”的短语，例如，上面的示例:

```
deposit: contract violation
```

- 被违约合约的确切描述

```
expected: amount
given: -10
```

- 完整的合约及其路径，显示哪一方面违约，

```
in: the 1st argment of
(-> amount any)
```

- 合约放置的模块（或者，更一般的说，合约调用的边界）

```
contract from: improved-bank-server
```

- 阻止方

```
blaming: top-level
(assuming the contract is correct)
```

- 合约发生时的源码定位

```
at: eval:5.0
```

### 7.3 通用函数合约

-> 合约构造器服务于接受固定数量参数函数，并且生成的合约不依赖输入参数。为了支持其他形式的函数，Racket 提供了额外的合约构造器，尤其是 ->* 和 ->i。

#### 7.3.1 可选参数

看一下这个摘选自字符串处理模块的片段：

```
# lang racket

(provide
    (contract-out
     ; pad the given str left and right with
     ; the (optional) char so that it is centered
     [string-pad-center (->* (string? natural-number/c)
                             (char?)
                             string?)]))
(define (string-pad-center str width [pad #\space])
    (define field-width (min width (string-length str)))
    (define rmargin (ceiling (/ (- widht field-width) 2)))
    (define lmargin (floor (/ (- width field-width) 2)))
    (string-append (build-string lmargin (λ (x) pad))
                   str
                   (build-string rmargin (λ (x) pad))))
```

该模块导出 string-pad-center,这是通过给定宽度和给定字符串生成字符串的函数。默认填充的是 #\space，如果客户端希望使用不同的字符，可以在调用 string-pad-center 传入第三个参数，一个字符，用来覆盖默认字符。

使用可选参数定义的函数，对此类功能很合适。这里有趣的是对于 string-pad-center 合约的制定。

合约组合器 ->*，需要几组合约：

- 首先是括号扩起来的一组所有必须参数。在这个例子中，我们看到的是这两个：string? 和 natural-number/c。 

- 第二组是括号扩起来的一组所有可选参数：char?。

- 最后是一个单独的合约：函数的返回值。

值得注意的是如果默认值不满足合约，你不会获取到合约的错误。如果你不相信你获取的是正确的初始化值，你需要跨边界传递初始值。

#### 7.3.2 剩余参数（rest arguments）

max 操作处理至少一个实数，但是它接口任何数量的额外参数。你也可以通过 rest argument 写出其他诸如此类的函数，比如 max-abs：

```
(define (max-abs n . rst)
    (foldr (lambda (n m) (max (abs n) m)) (abs n) rst))
```

在合约中通过 ->* 的进一步扩展来描述这个函数： #:rest 关键字在必须参数和可选参数后指定一个参数列表。

```
(provide
    (contract-out
     [max-abs (->* (real?) () #:rest (listof real?) real?)]))
```

对于 ->*，必选参数总是在第一个括号对中，对于这个例子是一个单独的实数。空括号对表示没有可选参数（不算剩余参数）。合约的剩余参数跟在 #:rest 后面，因为所有的剩余参数必须是实数，所以，剩余参数列表必须满足合约 (listof real?)。

#### 7.3.3 关键字参数

-> 合约构造器总也支持关键字参数。比如，考虑这个函数，它创建一个简单的 GUI 并且询问用户 yes-or-no 问题：

```
#lang racket/gui
(define (ask-yes-or-no-question question
                                #:default answer
                                #:title title
                                #:width w
                                #:height h)
    (define d (new dialog% [label title] [widht w] [height h]))
    (define msg (new message% [label question] [parent d]))
    (define (yes) (set! answer #t) (send d show #f))
    (define (no) (set! answer #f) (send d show #f))
    (define yes-b (new button% 
                       [label "yes"] [parent d]
                       [callback (λ (x y) (yes))]
                       [style (if answer '(border) '())]))
    (define no-b (new button%
                      [label "No"] [parent d]
                      [callback (λ (x y) (no))]
                      [style (if answer '() '(border))]))
    (send d show #t)
    answer)
(provide (contract-out
          [ask-yes-or-no-question
           (-> string?
               #:default boolean?
               #:title string?
               #:width exact-interger?
               #:height exact-interger?
               boolean?)]))
```

ask-yes-or-no-question 合约使用了 ->，lambda （或者基于define定义的函数）允许关键字位于形式参数之前，同样的，-> 允许关键字位于函数合约的参数合约之前。在这里，合约表示 ask-yes-or-no-question 必须接受四个关键字参数，分别是 #:default，#:title，#:width 和 #:height。在函数定义中，-> 中参数之间的相对顺序对于调用端没什么关系，只有在没有关键字的时候相对顺序才有意义。

#### 7.3.4 可选的关键字参数 （Optional Keyword Arguments）

当然，ask-yes-or-no-question 中的许多参数（就是前一个问题）有必要拥有默认值，并使其可选:

```
(define (ask-yes-or-no-question question
                                #:default answer
                                #:title [title "Yes or No?"]
                                #:width [w 400]
                                #:height [h 200])
 ...)
```

为了指明这个函数的合约，我们需要再次使用 ->*。它像你在可选参数和必需参数章节中希望的那样支持关键字参数。在这个例子中，我们有必须关键字参数 #:default 和可选关键字参数 #:title, #:width 和 #:height。所以我们像这样定义合约：

```
(provide (contract-out
          [ask-yes-or-no-question
           (->* (string?
                 #:default boolean?)
                 (#:title string?
                  #:width exact-integer?
                  #:height exact-integer?)
                
                 boolean?)]))
```

如上，我们把必须关键字放在第一部分，把可选关键字放在了第二部分。

#### 7.3.5 case-lambda 合约

使用 case-lambda 定义的函数可能会根据参数的数量，对其参数使用不同的合约。比如 report-cost 函数可能由一对数字或者字符串生成一个新字符串：

```
(define report-cost
  (case-lambda
    [(lo hi) (format "between $~a and $~a" lo li)]
    [(desc) (format "~a of dollars" desc)]))

> (report-cost 5 8)
"between $5 and $8"
> (report-cost "millions")
"millions of dollars"
```

这样的函数的合约用 case-> 组合器形成，它组合所需的多个合约：

```
(privode (contract-out
          [report-cost
           (case->
            (integer? integer? . -> . string?)
            (string? . -> . string))]))
```

正如你所见，report-cost 合约组合两个函数合约，它正好列符合实际描述所需的子句。

#### 7.3.6 参数和结果依赖 （Argument and Result Dependencies）

如下是虚拟数字模块的一个节选:

```
(provide
 (contract-out
  [real-sqrt (->i ([argment (>=/c 1)])
                  [result (argment) (<=/c argment)])]))
```

被导出函数 real-sqrt 的合约使用 ->i 而不是 ->* 函数合约。“i” 代表非独立合约，表示该函数的合约的范围取决于参数值。result合约那行的参数意味着该结果取决于参数。这个特别的例子中，real-sqrt 的参数大于等于1,所以基本正确的检查就是结果会小于参数。

通常，依赖函数合约看起来像是更一般的 ->* 合约，只是带了可以在该合约其他地方被使用的名字。

回顾下 bank-account 的例子，设想，我们组织模块以支持多用户，并且还包含了提现操作。改良版的 bank-account 模块包含 account 结构类型和以下函数：

```
(provide (contract-out
          [balance (-> account? amount/c)]
          [withdraw (-> account? amount/c account?)]
          [deposit (-> account? amount/c account?)]))
```

然而，除了要求调用端提供合法的金额给提现，金额还应该小于等于指定账户的余额，并且账户的结果将比初始金额的钱要少。类似地，该模块应该保证 deposit 会生成一个加上金额的账户。下面的实现强制对合约强制实行这些限制和保证。

```
#lanng racket
; section 1: the contract definitions
(struct account (balance))
(define amount/c natural-number/c)

; section 2: the exports
(provide
 (contract-out
  [create (amount/c . -> . account?)]
  [balance (account? . -> . amount/c)]
  [withdraw (->i ([acc account?]
                  [amt (acc) (and/c amount/c (<=/c (balance acc)))])
                 [result (acc amt)
                         (and/c account?
                                (lambda (res)
                                  (>= (balance res)
                                      (- (balance acc) amt))))])]
  [deposit (->i ([acc account?]
                 [amt amount/c])
                [result (acc amt)
                        (and/c account?
                               (lambda (res)
                                 (>= (balance res)
                                     (+ (balance acc) amt))))])]))
; section 3: the functionn definitions
(define balance account-balaance)

(define (create amt) (account amt))

(define (withdraw a amt)
  (account (- (account-balance a) amt)))

(define (deposit a amt)
  (account (+ (account-balance a) amt)))
```

第二部分的合约给 create 和 balance 提供了典型的 type-like 保护。然而，对于 withdraw 和 deposit，合约检查并提供更复杂的限制。withdraw的第二参数的合约使用 (balance acc)来检查提供的金额是否足够小，这里 acc 是 ->i 内给定函数的第一个参数的名字。withdraw 结果的合约使用 acc 和 amt 来保证提取金额不超过请求金额。deposit 合约类似的使用 acc 和 amount 在结果中保证被提供的金额被存进账户。

如上所述，当合约检查失败，错误消息并不好。下面的版本使用 flat-named-contract 和帮助函数 mk-account-contract 来提供更好的错误信息。

```
#lang racket
; section 1: the contract definitions
(struct account (balance))
(define amount/c natural-number/c)

(define msg> "account a with balance larger than ~a expected")
(define msg< "account a with balance less than ~a expected")

(define (mk-account-contract acc amt op msg)
  (define balance0 (balance acc))
  (define (ctr a)
    (and (account? a) (op balance0 (balance a))))
  (flat-named-contract (format msg balance0) ctr))

; section 2: the exports
(provide
 (contract-out
  [create (amount/c . -> . account?)]
  [balance (account? . -> . account/c)]
  [withdraw (->i ([acc account?]
                  [amt (acc) (and/c amount/c (<=/c (balance acc)))])
                 [result (acc amt) (mk-account-contract acc amt >= msg>)])]
  [deposit (->i ([acc account?]
                 [amt amount/c])
                [result (acc amt)
                        (mk-account-contract acc amt <= msg<)])]))

; section 3: the function definitions
(define balance account-balance)

(define (create amt) (account amt))

(define (withdraw a amt)
  (account (- (account-balance a) amt)))

(define (deposit a amt)
  (account (+ (account-balance a) amt)))
```

#### 7.3.7 检查状态改变（Checking State Changes）

->i 合约组合器总是可以确保某个函数只能通过某些限制来修改状态。比如，思考下这个合约(来自 preferences:add-panel 函数)：


```
(->i ([parent (is-a?/c area-container-window<%>)])
     [_ (parent)
      (let ([old-children (send parent get-children)])
           (λ (child)
                (andmap eq?
                    (append old-children (list child))
                    (send parent get-children))))])
```

上述代码表示，函数接受一个参数，名叫 parent,并且 parent 必须是一个符合接口 area-container-window<%> 的对象。合约的范围确保函数只通过添加一个 child 到 children 列表的前面以进行修改。函数通过使用 _ 而不是正常的标识符来完成这些，他告诉合约库：合约范围不依赖任何结果的值，然后当函数被调用时（而不是在返回时），合约库执行 _ 之后的表达式。因此对于 get-children 方法的调用先于合约中函数的调用。当合约中函数返回时，它的结果作为 child 传递，合约就会确保，函数返回的 children 和 被调用之前的 children 只是在列表前面多了一个 child，其他都是相同的。

看一下简单例子的关于这点的区别，思考下这个程序：

```
#lang racket
(define x '())
(define (get-x) x)
(define (f) (set! x (cons 'f x)))
(provide
 (contract-out
  [f (->i () [_ (begin (set! x (cons 'ctc x)) any/c)])]
  [get-x (-> (listof symbol?))]))
```

如果我们请求这个模块，调用 f, get-x 的结果将会是 '(f ctc)。相反，如果 f 的合约是

```
(-> () [res (begin (set! x (cons 'ctc x)) any/c)])
```

（只是改为申明了 res），get-x 的结果将会是 '(ctc f)。

#### 7.3.8 多结果值（Multuple Result Values）

split 函数使用字节列表并返回遇到的第一个 #\newline 之前的字符串和该列表的其余部分：

```
(define (split l)
  (define (split l w)
    (cond
      [(null? l) (values (list->string (reverse w)) '())]
      [(char=? #\newline (car l))
       (values (list->string (reverse w)) (cdr l))]
      [else (split (cdr l) (cons (car l) w))]))
  (split l '()))
```

这是一个典型的多返回值函数，它拆分单个列表返回两个值。

对于这种函数的合约，可以使用普通函数箭头 ->，因为当 values 出现在最后， -> 会特殊对待：

```
(provide (contract-out
          [split (-> (listof char?)
                     (values string? (listof char?)))]))
```

这类函数的合约也可以使用 ->*:

```
(provide (contract-out
          [split (->* ((listof char?))
                      ()
                      (value string? (listof char?)))]))
```

就像之前一样，使用 ->* 的参数的合约被一对额外的括号包围（必须总是像这样包围），空括号对表示这里没有可选参数。结果合约里有 values:一个字符串和一个字节数组。







































