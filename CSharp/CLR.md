---
title: CLR！C# 虚拟机？
date: 2023-12-3
---



# CLR

通用语言运行平台(Common Language Runtime，简称CLR)是微软为他们的.Net虚拟机所选用的名称。这是通用语言架构(简称CLI)的微软实现版本，它定义了一个代码运行的环境。CLR运行一种称为“通用中间语言”的字节码，这个是微软的通用中间语言实现版本。

1. CLR是一个类似于JVM的虚拟机，为微软的.Net产品提供运行环境。
2. CLR上实际运行的并不是我们通常所用的编程语言(例如C#、VB等)，而是一种字节码形态的“中间语言”。这意味着只要能将代码编译成这种特定的“中间语言”(MSIL),任何语言的产品都能运行在CLR上。
3. CLR通常被运行在Windows系统上，但是也有一些非Windows的版本。这意味着.Net也很容易实现“跨平台”。(至于为什么大家的印象中.Net的跨平台性不如Java，更多的是微软商业战略导致的)

## CLR basic concepts

### .Net Framework

.NET框架 （.NET Framework） 是由微软开发，一个致力于敏捷软件开发（Agile software development）、快速应用开发（Rapid application development）、平台无关性和网络透明化的软件开发平台。.NET框架是以一种采用系统虚拟机运行的编程平台，以通用语言运行库（Common Language Runtime）为基础，支持多种语言（C#、VB.NET、C++、Python等）的开发。

以上搬运自维基百科，总的来说可以得出这样一个结论，之所以 C# 在 Unity 下开发能跨平台（指的是打包设置里 scripting backend 选择了 Mono 的情况，IL2CPP 是另一种方式）是因为 Mono 是  .NET Framework 的开源实现，而 Unity 使用 Mono 的方式打包，使得 C# 最终跑在 .NET Framework 的 CLR 下。

### languages support CLR

微软已经为多种语言开发了基于CLR的编译器，这些语言包括：C++/CLI、C#、Visual Basic、F#、Iron Python、 Iron Ruby和IL。除此之外，其他的一些公司和大学等机构也位一些语言开发了基于CLR的编译器，例如Ada、APL、Caml、COBOL、Eiffel、Forth、Fortran、Haskell、Lexicon、LISP、LOGO、Lua、Mercury、ML、Mondrian、Oberon、Pascal、Perl、PHP、Prolog、RPG、Scheme、Smaltak、Tcl/Tk。

CLR为不同的编程语言提供了统一的运行平台，在很大程度上对上层开发者屏蔽了语言之间才特性差异。对于CLR来说，不同语言的编译器(Compiler)就相当于一个这种语言的代码审查者(Checker),所做的工作就是检查源码语法是否正确，然后将源码编译成CLR所需要的中间语言(IL)。所以编程语言对于CLR是透明的，也就是说CLR只知道IL的存在，而不知道IL是由哪种语言编译而来。

CLR的这种“公共语言”的特性使得“多语言混合编程”成为可能，让APL开发人员可以使用自己熟悉的语言和语法来开发基于.Net的项目。当然，更重要的是，这种特性允许用不同的语言来开发同一个项目的不同模块，比如在一个项目中用Visual Basic开发UI、用APL开发财务相关的模块，而与数学计算有关的模块使用F#，充分利用这些语言的特性，将会得到意想不到的效果。

### CLS

正如上面所说，CLR集成了这些所有的语言，使得一种编程语言可以使用另一种编程语言创建的对象。语言集成是一个宏伟的目标，然而，一个不得不面对的事实是不同的编程语言之间存在着极大的差异，比如有些语言不支持操作符重载、有些语言不支持可选参数等等。为了保证用一种面向CLR的编程语言创建的对象能够安全的被其他面向CLR的语言来访问，最好的办法就是在用这种语言编程的时候只是用那些所有面向CLR的语言都支持的特性。为此，微软专门定义了一个“公共语言规范(Common Language Specification)”，即CLS。CLS定义了一个最小的功能集，任何面向CLR的编译器生成的类型想要兼容其他“符合CLS，面向CLR”的组件，都必须支持这个最小功能集。

CLR/CTS 支持的功能比 CLS 定义的子集多得多。如果不关心语言之间的互操作性，可以开发一套功能非 常丰富的类型，它们仅受你选用的那种语言的功能集的限制。具体地说，在开发类型和方法的时候，如果 希望它们对外“可见”，能够从符合 CLS 的任何一种编程语言中访问，就必须遵守由 CLS 定义的规则。注意， 假如代码只是从定义（这些代码的）程序集的内部访问，CLS 规则就不适用了。

到目前为止，除了IL汇编语言支持CLR所有的功能特性之外，其他大多数面向CLR的语言都只提供了CLR部分的功能。也就是说这些语言的功能都是CLR功能特性的一个子集，但为了实现与其他语言的互操作性，他们的功能都是CLS的一个超集(但不一定是同一个超集)。

如果想要写出遵循CLS的代码，必须要保证一下几个部分的代码遵循CLS。

- 公共类(public class)的定义
- 公共类中公共成员的定义,以及派生类(family访问)可以访问的成员的定义。
- 公共类中公共方法的参数和返回类型，以及派生类可以访问的方法的参数和返回类型。

### CTS

需要记住的是CLR所有功能的实现都是基于类型的。一个类型将功能提供给一个应用程序或者另一个类型来使用。通过类型，用一种编程语言写的代码能与用另一种语言写的代码沟通。由于类型是CLR的根本，微软专门为如何定义、使用和管理类型定义了一个正式的规范-- 通用类型系统(Common Type System)，即CTS。

CTS对类型的定义和行为特性给出了规范，这些特性包括但不仅限于以下几点：

- 类成员(Field、Method、Property、Event)
- 访问可见性级别(private、family、family and assembly 、assembly、family or assembly 、public)
- 类型的继承
- 接口
- 虚方法
- 对象的生命周期

同时，所有引用类型都必须继承自System.Object的规则也是在CTS中定义的。

一般来说，CLS主要提供了一下功能：

- 建立一个支持跨语言集成、类型安全和高性能代码执行的框架环境。

- 提供一个支持完整实现多种编程语言的面向对象的模型。
- 定义各语言必须遵守的规则，有助于确保用不同语言编写的对象能够交互作用。
- 提供包含应用程序开发中使用的基元数据类型（如Boolean、Byte、Char、Int32 和 UInt64）的库。

### CLR works

![img](https://raw.githubusercontent.com/yoziya/images/main/notes/01193506-fb0c936e13cd4fd8a8bf3fb53a3dabe0.png)

 编译器把不同的高级语言编译为 IL，在运行时，CLR 将 IL 即时编译为 CPU 指令。

总结起来，CLR主要提供了一下功能：

1. 基类库支持 (Base Class Library Support)
2. 内存管理 (Memory Management)
3. 线程管理 (Thread Management)
4. 垃圾回收 (Garbage Collection)
5. 安全性 (Security)
6. 类型检查 (Type Checker)
7. 异常处理 (Exception Manager)
8. 即时编译 (JIT)



---

## issues



## reference

- 《CLR via C#》

## appendix



---