---
title: C# 反射
date: 2022-10-7
---



# Reflection

反射提供描述程序集、模块和类型的对象（Type 类型）。 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型，然后调用其方法或访问器字段和属性。 如果代码中使用了特性，可以利用反射来访问它们。

## Implementation principle

反射的工作原理是利用了.NET的**元数据**。.NET框架为每个类型和成员存储了丰富的信息。当程序编译时，所有的类型信息（包括类、接口、字段、方法等）都被嵌入到程序集的元数据部分。反射API提供了查询这些信息的手段。

## Application

- 使用Assembly定义和加载程序集，加载在程序集清单中列出模块，以及从此程序集中查找类型并创建该类型的实例。 
- 使用Module了解包含模块的程序集以及模块中的类等，还可以获取在模块上定义的所有全局方法或其他特定的非全局方法。 
- 使用ConstructorInfo了解构造函数的名称、参数、访问修饰符（如pulic 或private）和实现详细信息（如abstract或virtual）等。 
- 使用MethodInfo了解方法的名称、返回类型、参数、访问修饰符（如pulic 或private）和实现详细信息（如abstract或virtual）等
- 使用FiedInfo了解字段的名称、访问修饰符（如public或private）和实现详细信息（如static）等，并获取或设置字段值。
- 使用EventInfo了解事件的名称、事件处理程序数据类型、自定义属性、声明类型和反射类型等，添加或移除事件处理程序。 
- 使用PropertyInfo了解属性的名称、数据类型、声明类型、反射类型和只读或可写状态等，获取或设置属性值。 
- 使用ParameterInfo了解参数的名称、数据类型、是输入参数还是输出参数，以及参数在方法签名中的位置等。

```csharp
using System.Reflection;
using System.Type;
using System.Reflection.Assembly;
```

### System.Type

System.Type 类对于反射起着核心的作用。但它是一个抽象的基类，Type有与每种数据类型对应的派生类，我们使用这个派生类的对象的方法、字段、属性来查找有关该类型的所有信息。

通过以下方式可以获取给定类型的 Type

```csharp
string name = "";
Type type = typeof(string);
Type type = name.GetType();
Type type = Type.GetType("System.String");
```

注意：只有第一个 `typeof(string)` 是在编译时解析的。它直接使用类型的名称（在这个例子中是`System.String`），编译器在编译过程中就确定了具体的类型。另外两个是在运行时解析的，是实际中反射会用的方式（如果是已知确定的类型，直接用 typeof 甚至用类型名本身去做创建对象之类的事就行了，反射其实和直接把类拿起来用有一定的性能差异）。

#### Type类的属性：

- Name 数据类型名
- FullName 数据类型的完全限定名(包括命名空间名)
- Namespace 定义数据类型的命名空间名
- IsAbstract 指示该类型是否是抽象类型
- IsArray  指示该类型是否是数组
- IsClass  指示该类型是否是类
- IsEnum  指示该类型是否是枚举
- IsInterface  指示该类型是否是接口
- IsPublic 指示该类型是否是公有的
- IsSealed 指示该类型是否是密封类
- IsValueType 指示该类型是否是值类型

#### Type类的方法：

- GetConstructor(), GetConstructors()：返回ConstructorInfo类型，用于取得该类的构造函数的信息
- GetEvent(), GetEvents()：返回EventInfo类型，用于取得该类的事件的信息
- GetField(), GetFields()：返回FieldInfo类型，用于取得该类的字段（成员变量）的信息
- GetInterface(), GetInterfaces()：返回InterfaceInfo类型，用于取得该类实现的接口的信息
- GetMember(), GetMembers()：返回MemberInfo类型，用于取得该类的所有成员的信息
- GetMethod(), GetMethods()：返回MethodInfo类型，用于取得该类的方法的信息
- GetProperty(), GetProperties()：返回PropertyInfo类型，用于取得该类的属性的信息

可以调用这些成员，其方式是调用Type的InvokeMember()方法，或者调用MethodInfo, PropertyInfo和其他类的Invoke()方法。 

#### 查询类型信息

可以使用 `Type` 类来获取一个类型的信息，例如方法、属性、字段和事件。使用 `typeof` 关键字或 `Object.GetType` 方法可以获得 `Type` 对象。

```csharp
NewClassw nc = new NewClassw();
Type t = nc.GetType();

ConstructorInfo[] ci = t.GetConstructors();    //获取类的所有构造函数
foreach (ConstructorInfo c in ci) //遍历每一个构造函数
{
    ParameterInfo[] ps = c.GetParameters();    //取出每个构造函数的所有参数
    foreach (ParameterInfo pi in ps)   //遍历并打印所该构造函数的所有参数
	{
		Console.Write(pi.ParameterType.ToString()+" "+pi.Name+",");
	}
    Console.WriteLine();
}

PropertyInfo[] pis = t.GetProperties();
foreach(PropertyInfo pi in pis)
{
    Console.WriteLine(pi.Name);
}
```

#### 动态创建实例

用构造函数动态生成对象：

```csharp
Type t = typeof(NewClassw);
Type[] pt = new Type[2];
pt[0] = typeof(string);
pt[1] = typeof(string);
//根据参数类型获取构造函数 
ConstructorInfo ci = t.GetConstructor(pt); 
//构造Object数组，作为构造函数的输入参数 
object[] obj = new object[2]{"grayworm","hi.baidu.com/grayworm"};   
//调用构造函数生成对象 
object o = ci.Invoke(obj);    
```

反射可以用来动态地创建类型的实例。 `Activator.CreateInstance` 方法是通常用于这种目的的方法。

```c#
Type t = typeof(NewClassw);
//构造函数的参数 
object[] obj = new object[2] { "grayworm", "hi.baidu.com/grayworm" };   
//用Activator的CreateInstance静态方法，生成新对象 
object o = Activator.CreateInstance(t,"grayworm","hi.baidu.com/grayworm"); 
```

### System.Reflection.Assembly

Assembly类可以获得程序集的信息，也可以动态的加载程序集，以及在程序集中查找类型信息，并创建该类型的实例。
使用Assembly类可以降低程序集之间的耦合，有利于软件结构的合理化。

- 通过程序集名称返回Assembly对象 `Assembly ass = Assembly.Load("ClassLibrary831")`
- 通过DLL文件名称返回Assembly对象 `Assembly ass = Assembly.LoadFrom("ClassLibrary831.dll")`;
- 通过Assembly获取程序集中类 `Type t = ass.GetType("ClassLibrary831.NewClass")`;  //参数必须是类的全名
- 通过Assembly获取程序集中所有的类 `Type[] t = ass.GetTypes()`;

```csharp
//通过程序集的名称反射
Assembly ass = Assembly.Load("ClassLibrary831");
Type t = ass.GetType("ClassLibrary831.NewClass");
object o = Activator.CreateInstance(t, "grayworm", "http://hi.baidu.com/grayworm");
MethodInfo mi = t.GetMethod("show");
mi.Invoke(o, null);

//通过DLL文件全名反射其中的所有类型
Assembly assembly = Assembly.LoadFrom("xxx.dll的路径");
Type[] types = assembly.GetTypes();
foreach(Type t in types)
{
    if(t.FullName == "a.b.c")
    {
    	object o = Activator.CreateInstance(t);
    }
}
```

## flaws

- **性能影响**：反射通常比直接代码访问慢，因为它需要在运行时解析元数据。
- **安全性问题**：动态执行代码可以引入安全漏洞，尤其是在处理不受信任的数据时。
- **维护性**：反射使得代码更难理解和维护，因为某些操作是在运行时而非编译时确定的。



---

## issues



## reference



## appendix



---