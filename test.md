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