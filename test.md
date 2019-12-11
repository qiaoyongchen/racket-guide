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

## 4.表达式和定义（Expressions and Definitions）

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
    (lambda (given #:;last surname)
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

在某种意义上，make-add-suffix 是一个接受两个参数的函数，但是一次只能接收一个参数。这种接收几个参数并且返回以消耗更多的参数的函数，有时被称为柯里化函数。











