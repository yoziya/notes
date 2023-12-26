---
title: Python 虚拟机
date: 2023-12-19
---



# PVM



## CodeObject

```python
def function(arg, constarg=3, *args, **kwargs):
    print("Hello")
    return

class obj(object):
    def __init__(self):
        return

code = function.__code__
print(code.co_code)

# number of arguments
print(code.co_argcount)

# number of positional only arguments
print(code.co_posonlyargcount)

# number of keyword only arguments
print(code.co_kwonlyargcount)

import dis
dis.dis(function)
```

### `co_code`

二进制字节码，如`co_code = {str} "'d\\x01\\x00GHd\\x00\\x00S'"`

### `co_flags`

特殊行为标志

### `co_stacksize`

所需的栈空间

### `co_argcount`

### `co_posonlyargcount`(Python3.8+)

Python 的函数参数可以按顺序传入，也可以指定关键字传入，如 `func(1,1)` 和 `func(arg = 1)`。

使用 `def function(arg, constarg=3, /, *args, **kwargs):` 语法，`/` 前的参数将只能省略关键字按顺序传入。

### `co_kwonlyargcount`

使用 `def function(arg, *, constarg=3, *args, **kwargs):` 语法，`*` 后的参数将只能使用关键字传入。

### `co_argcount`

### `co_nlocals`

`co_nlocals = {int} 4`

函数体内定义的局部变量的数量（包括定义的参数）

### `co_varnames`

`co_varnames = {tuple: 4} ('arg', 'constarg', 'args', 'kwargs')`

函数体内所有局部变量的名称，按照它们在源代码中出现的顺序排列。

### `co_names`

函数中引用的所有全局变量的名称的元组。这些变量可能不在函数内定义，但在函数中被使用。

### `co_cellvars`

### `co_freevars`

### `co_const`

函数中用到的所有常量的元组，包括字面量和编译时生成的对象，依然按照出现的顺序排列。

## frame

简单来讲就是一个调用栈，每当一个函数被调用时，一个新的 frame 就被推入调用栈；当函数执行完毕时，其对应的 frame 就被弹出。frame 用于给 bytecode 提供一个运行的虚拟环境，由 C 语言编写。bytecode 发出一系列指令对调用栈中的 frame 内容进行操作。

在 Python 中，每当一个函数被调用时，就会创建一个新的 frame。frame 是一个包含了所有执行状态信息的对象，这些信息包括局部变量、全局变量、指向当前执行指令的指针等。

内容:

- 局部变量: 函数内定义的变量。
- 全局变量: 在函数外定义的变量。
- 指令指针: 指向当前正在执行的字节码指令。
- 返回地址: 当前函数调用结束后，控制流应该返回到的位置。
- 数据栈: 用于存储操作数的栈结构。

（PS：.Net 反射能做的我也能做。反射做不到的我还能做）

## bytecode

```python
import dis
dis.dis(function)
```

字节码可以当成 Python 做的一套类汇编语言，真实的汇编语言是基于硬件的，跨平台无比困难的语言。python 的字节码是是不基于硬件的，类似中间语言。Python 编译成字节码后通常存为 .pyc 的二进制文件，但如果有阅读需要，用 `import dis` 可以显示（多看一眼就会爆炸）。

字节码由 C 实现的虚拟机来解释执行，跨平台能力本质还是 C 带来的。



---

## issues



## reference

- [inspect --- 检查对象 — Python 3.12.1 文档](https://docs.python.org/zh-cn/3/library/inspect.html?highlight=inspect#module-inspect)

## appendix

- 

---