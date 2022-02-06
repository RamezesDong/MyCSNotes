# Scheme解释器

## 预备知识

REPL: Read-Eval-Print-Loop

REPL指交互式解释器既可以作为一个独立的程序运行，又可以很容易地被包含在其他程序中作为程序的一部分运行。Lisp, Ruby, Python, node.js等都支持REPL。

本项目[Project 4: Scheme Interpreter | CS 61A Fall 2021](https://inst.eecs.berkeley.edu/~cs61a/fa21/proj/scheme/#problem-3-2-pt)实现一个简单的Scheme REPL解释器，能够处理部分Scheme指令。

Lab11 已经完成了对字符串的parse解析，并将结果使用Pair类表示。本次项目继续之后的工作。

## 目的

本笔记更像是一个项目文档的翻译，并且附有个人理解。因为我发现cs61a的内容虽然不算太难，语言间的差异却给我的个人实践带来很大的阻碍。故记录下来方便复习回顾，也希望能和大家一起交流学习。

## Part I: The Evaluator

在part1, 我们将完成解释器的如下特征：

- 符号求值
- 调用内置规程
- 定义

在已经给出的实现里，求值器只能给出自求值表达式的值：数，布尔值，`nil`。

首先阅读相关代码，`scheme_eval_apply.py`的求值/应用部分：

- `scheme_eval`求scheme表达式在给定环境中的值。这个函数除了调用表达式都完成了。
- 当求值特殊表达式时，`scheme_eval`重定向到一个合适的`do_?_form`函数。
- `scheme_apply`实现一个有一些参数的procedures，这个函数有不同情况（内置procedures,用户自定义procedures)

在`scheme_classes.py`环境与过程部分：

- `Frame`类实现一个环境框架。
- `LambdaProcedure`类表示用户自定义过程。

`scheme_forms.py` 定义特殊表达式； `scheme_builtins.py` 定义标准库中的各种函数； `scheme.py` 定义输入与输出。

### Problem 1

 实现`Frame`类中的`define`与`lookup`方法。每个`Frame`类都有如下实例属性：

- `bindings`一个字典表示frame中的绑定关系。
- `parent` 是frame的父环境框架。全局环境的父环境是`None`。

本题比较好理解，在当前环境寻找标志对应值；若未找到递归查找父环境；若都没找到，发出一个`SchemeError`

### Problem 2

调用内置过程，如`+`。需要完成`scheme_apply`中procedure是内置procedure的情况。

`BuiltinProcedure`有两个实例属性：

- `py_func`：Python函数实现内置scheme过程
- `expect_env`：是否期望当前环境被用作最后一个参数。

`args`是一个Scheme列表，被用`Pair`实现。故实现指导如下：

- 将Scheme list 转换为Python list
- 如果`produre.expect_env`为True, 将`env`作为最后一个参数加到Python list之后
- 调用`produce.py_func`参数要使用`*args`
- 如果结果有TypeError被捕获时，触发一个`SchemeError`
- 如果没有错误信息则返回`scheme.py_func`的结果\

本题乍一看不是很好理解，但是如果明白procedure是一个`BuiltinProcedure`类，就可以很轻松的写出代码。

### Problem 3

实现`scheme_eval`函数缺失的部分，即求调用表达式的值：

- 操作符Evaluate, 将其转变为一个`Procedure`实例
- `Evaluate`所有操作数
- 调用操作符对应的运算过程`scheme_apply`，返回结果

前两步我们必须递归调用`scheme_eval`，这是一些建议：

- `Pair`的 `map`方法会返回一个新的scheme list ，它的构造方式是通过对scheme list每一项应用单参数函数
-  The `scheme_apply` function applies a Scheme procedure to arguments represented as a Scheme list (a `Pair` instance). 这句话不是很好理解

- 不要改变`expr`，这将会程序求值时使其被改变，造成奇怪且不正确的影响

### Problem 4

`define`特殊scheme结构，可以将值或者过程与名字绑定。

第一个操作数可以表示什么被定义：

- 若它是一个符号，如a, 那么是表达是用来定义一个名字
- 如果它是一个list, 如`(foo x)` 则表达式用了定义一个过程

`do_define_form`函数评定`define...`表达式。在这个问题里，只需要完成第一部分（将第二个操作数运算值域第一个操作数的符号绑定。`do_define_form`之后返回这个符号。

### Problem 5

引用表达式：实现`do_quote_form`就是简单地返回未求值的操作数表达式。

## Part II: Procedures

第二部分，增加处理用户自定义过程的能力。你将给解释器一下功能：

- Lambda procedure
- Named Lambda procedure, using the `define(..)..`特殊形式
- Mu procedure, with dynamic scope

### User-Defined Procedures

`LambdaProcedure`有三个实例属性：

- `formals` 自定义过程的形参列表，相当于函数变量
- `body` 是一个scheme表达式列表，相当于函数体
- `env`  过程被定义所在的环境

### Problem 6

修改`eval_all`，使之可以处理`begin`特殊结构。

### Problem 7

实现`do_lambda_form`函数，创建并返回一个`LambdaProcedure`实例。

在scheme语言中，将不止一条表达式放在函数（过程）体中是合法的。因此`body`属性是一个scheme list，与`begin` 相似，求值每条语句，并且将最后一句作为返回值。

### Problem 8

实现`make_child_frame`方法：当调用用户定义过程时创建一个新框架。这个方法有两个参数`formals`和`vals`，分别是scheme list 的符号和值。它必须返回一个新的框架，将`formal parameters` 与值绑定。

- 如果参数的个数与标准参数个数不一致，生成一个Scheme Error
- 创建一个新的Frame, 它的父亲是self
- 将形参与实参绑定。
- 返回新Frame

### Problem 9

实现`LambdaProcedure`类型。

首先使用`make_child_frame`创建一个新的frame,将形参与实参绑定。之后对过程体中每个表达式使用`eval_all`求值。 

