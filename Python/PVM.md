---
title: Python 虚拟机
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

简单来讲就是一个调用栈，用于给 bytecode 提供一个运行的虚拟环境，由 C 语言编写。

## bytecode

```python
import dis
dis.dis(function)
```

字节码可以当成 Python 做的一套类汇编语言，真实的汇编语言是基于硬件的，跨平台无比困难的语言。python 的字节码是是不基于硬件的，类似中间语言。Python 编译成字节码后通常存为 .pyc 的二进制文件，但如果有阅读需要，用 `import dis` 可以显示。

字节码由 C 实现的虚拟机来解释执行，跨平台能力本质还是 C 带来的。



---

## issues



## reference

- [inspect --- 检查对象 — Python 3.12.1 文档](https://docs.python.org/zh-cn/3/library/inspect.html?highlight=inspect#module-inspect)

## appendix

- 

---