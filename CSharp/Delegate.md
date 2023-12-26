---
title: Delegate
date: 2022-8-26
---



# Delegate

委托类似于 C或 C++中的函数指针，允许将方法作为参数进行传递。但委托是面向对象、类型安全的。

## delegate

简单的总结不如Microsoft文档：[System.Delegate 和“delegate”关键字 - C# | Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/csharp/delegate-class)

### 定义委托类型

声明 delegate 至少 0 个参数，至多 32 个参数，可以无返回值，也可以指定返回值类型。语法像是声明抽象函数，但 delegate 是类型。

```csharp
public delegate int Comparison<in T>(T left, T right);
```

编译器会生成一个 `Comparison<T>` 类，它派生自与使用的签名匹配的 `System.Delegate`（这个套路和 `string` 关键字会生成一个 `System.String` 有点像，只不过这个不是在生成继承某某的类，而是生成实例）。 

对于这种有返回值的情况，详情见 `System.Delegate` 源码中的 `public object DynamicInvoke(params object[] args) => this.DynamicInvokeImpl(args);`，其中的 `object` 即是正在传递的返回值。

### 声明委托的实例

```csharp
public Comparison<int> comparator;
```

### 调用委托

```csharp
int result = comparator(left, right);
```

在上面的行中，代码会调用附加到委托的方法。 可将变量视为方法名称，并使用普通方法调用语法调用它。

该代码行进行了不安全假设：不保证目标已添加到委托。 如果未附加目标，则上面的行会导致引发 `NullReferenceException`（`?.` 解决）。

### 分配、添加和删除调用目标

```csharp
private static int CompareLength(string left, string right) =>2    left.Length.CompareTo(right.Length);

phrases.Sort(CompareLength);

Comparison<string> comparer = CompareLength;
phrases.Sort(comparer);
```

在用作委托目标的方法是小型方法的用法中，经常使用 [lambda 表达式](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/lambda-expressions)语法来执行分配：

```csharp
Comparison<string> comparer = (left, right) => left.Length.CompareTo(right.Length);
phrases.Sort(comparer);
```

Sort() 示例通常将单个目标方法附加到委托。 但是，委托对象支持将多个目标方法附加到委托对象的调用列表（支持 `+=`）。

### Delegate 和 MulticastDelegate 类

上面介绍的语言支持可提供在使用委托时通常需要的功能和支持。 这些功能采用 .NET Core Framework 中的两个类进行构建：[Delegate](https://learn.microsoft.com/zh-cn/dotnet/api/system.delegate) 和 [MulticastDelegate](https://learn.microsoft.com/zh-cn/dotnet/api/system.multicastdelegate)。

`System.Delegate` 类及其单个直接子类 `System.MulticastDelegate` 可提供框架支持，以便创建委托、将方法注册为委托目标以及调用注册为委托目标的所有方法。

要求不能声明派生自 `Delegate` 或 `MulticastDelegate` 的类。 C# 语言规则禁止这样做。相反，C# 编译器会在你使用 C# 语言关键字声明委托类型时，创建派生自 `MulticastDelegate` 的类的实例。确保在使用委托时，语言强制实施类型安全。 

即使不能直接创建派生类，也会使用对这些类定义的方法。 

对委托最常使用的方法是 `Invoke()` 和 `BeginInvoke()` / `EndInvoke()`。 `Invoke()` 会调用已附加到特定委托实例的所有方法。

### Action

Action 至少 0 个参数，至多 16 个参数，无返回值。

```csharp
public delegate void Action();
```

### Func

Func 至少 0 个参数，至多 16 个参数，根据返回值泛型返回。必须有返回值，不可 void。

```csharp
public delegate TResult Func<in T1, out TResult>(T1 arg);
```

### Predicate

Predicate 有且只有一个参数，返回值固定为 bool

```csharp
public delegate bool Predicate<in T>(T obj);
```

## event

### 事件委托签名

```csharp
void EventRaised(object sender, EventArgs args);
```

返回类型为 void。 事件基于委托，而且是多播委托。 参数列表包含两种参数：发件人和事件参数。 `sender` 的编译时类型为 `System.Object`，即使有一个始终正确的更底层派生的类型亦是如此。 按照惯例使用 `object`。

### 选择 delegate | event

-  如果代码必须调用订阅服务器提供的代码，则在需要实现回调时，应使用基于委托的设计。 
-  需要返回值时使用 delegate
-  事件通常是公共类成员。 相比之下，委托通常作为参数传递，并存储为私有类成员（如果它们全部存储）。
-  事件通常应用在生命周期较长的情况

---

## issues





## reference

- [C# 中的委托和事件简介 - C# | Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/csharp/delegates-overview)

## appendix

- 

---