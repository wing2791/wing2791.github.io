---
title: CSharp知识整理(三)
math: true
---


# 一、 命名空间
> 命名空间（Namespace）是 C# 中用于组织代码的一种方式。它提供了一种机制来将相关的类、结构、接口、枚举和委托分组在一起，从而避免名称冲突，并提供更好的代码管理和结构。

- 命名空间中的类，默认为internal
- internal表示只能在该程序集中使用，也就是只能在当前的项目中使用
- 不同命名空间中允许有同名类
- 命名空间中可以包裹命名空间
### 1. 创建命名空间
可以使用 `namespace` 关键字定义一个命名空间。以下是一个简单的示例：
```csharp
namespace Geometry
{
    public class Circle
    {
        public double Radius { get; set; }

        public Circle(double radius)
        {
            Radius = radius;
        }

        public double Area()
        {
            return Math.PI * Radius * Radius;
        }
    }
    
    public class Rectangle
    {
        public double Width { get; set; }
        public double Height { get; set; }

        public Rectangle(double width, double height)
        {
            Width = width;
            Height = height;
        }

        public double Area()
        {
            return Width * Height;
        }
    }
}
```
### 2. 使用命名空间
在使用命名空间中的类时，您可以使用完整名称，或者使用 `using` 指令来引入命名空间，以便在代码中更方便地使用它们。
#### 完整名称
```csharp
public class Program
{
    public static void Main()
    {
        Geometry.Circle circle = new Geometry.Circle(5);
        Console.WriteLine($"Circle Area: {circle.Area()}");

        Geometry.Rectangle rectangle = new Geometry.Rectangle(4, 6);
        Console.WriteLine($"Rectangle Area: {rectangle.Area()}");
    }
}
```
#### 使用 `using` 指令
```csharp
using System;
using Geometry; // 引入 Geometry 命名空间

public class Program
{
    public static void Main()
    {
        Circle circle = new Circle(5);
        Console.WriteLine($"Circle Area: {circle.Area()}");

        Rectangle rectangle = new Rectangle(4, 6);
        Console.WriteLine($"Rectangle Area: {rectangle.Area()}");
    }
}
```
### 3. 嵌套命名空间
C# 还支持嵌套命名空间，可以通过在命名空间中定义另一个命名空间来实现。
```csharp
namespace Geometry
{
    namespace Shapes
    {
        public class Triangle
        {
            public double Base { get; set; }
            public double Height { get; set; }

            public Triangle(double baseLength, double height)
            {
                Base = baseLength;
                Height = height;
            }

            public double Area()
            {
                return 0.5 * Base * Height;
            }
        }
    }
}
```
使用嵌套命名空间时，可以这样访问：
```csharp
using Geometry.Shapes;

public class Program
{
    public static void Main()
    {
        Triangle triangle = new Triangle(4, 5);
        Console.WriteLine($"Triangle Area: {triangle.Area()}");
    }
}
```
### 4. 在同一文件中定义命名空间
在一个文件中定义多个类，并将它们放在同一个命名空间下。
**文件名：`Geometry.cs`**
```csharp
using System;

namespace Geometry // 命名空间
{
    public class Circle
    {
        public double Radius { get; set; }

        public Circle(double radius)
        {
            Radius = radius;
        }

        public double Area()
        {
            return Math.PI * Radius * Radius;
        }
    }
}
namespace Geometry
{
    public class Rectangle
    {
        public double Width { get; set; }
        public double Height { get; set; }

        public Rectangle(double width, double height)
        {
            Width = width;
            Height = height;
        }

        public double Area()
        {
            return Width * Height;
        }
    }
}

```
**使用示例：**
```csharp
using System;
using Geometry; // 引入 Geometry 命名空间

public class Program
{
    public static void Main()
    {
        Circle circle = new Circle(5);
        Console.WriteLine($"Circle Area: {circle.Area()}");

        Rectangle rectangle = new Rectangle(4, 6);
        Console.WriteLine($"Rectangle Area: {rectangle.Area()}");
    }
}
```
### 5. 将命名空间分散到多个文件中
可以在多个文件中定义属于同一个命名空间的类。这样做有助于代码的模块化和可维护性。
**文件名：`Circle.cs`**
```csharp
using System;

namespace Geometry // 命名空间
{
    public class Circle
    {
        public double Radius { get; set; }

        public Circle(double radius)
        {
            Radius = radius;
        }

        public double Area()
        {
            return Math.PI * Radius * Radius;
        }
    }
}
```
**文件名：`Rectangle.cs`**
```csharp
using System;

namespace Geometry // 命名空间
{
    public class Rectangle
    {
        public double Width { get; set; }
        public double Height { get; set; }

        public Rectangle(double width, double height)
        {
            Width = width;
            Height = height;
        }

        public double Area()
        {
            return Width * Height;
        }
    }
}
```
**使用示例：**
```csharp
using System;
using Geometry; // 引入 Geometry 命名空间

public class Program
{
    public static void Main()
    {
        Circle circle = new Circle(5);
        Console.WriteLine($"Circle Area: {circle.Area()}");

        Rectangle rectangle = new Rectangle(4, 6);
        Console.WriteLine($"Rectangle Area: {rectangle.Area()}");
    }
}
```
### 总结
- **命名空间** 是组织代码的有力工具，帮助管理和避免名称冲突。
- 可以使用 `namespace` 关键字定义命名空间，并通过 `using` 指令引入命名空间。
- 支持嵌套命名空间，允许创建更复杂的结构以组织相关类。
- **同一文件定义**：在一个文件中定义多个类，适合小型项目或紧密相关的类。
- **分散到多个文件**：将类分散到多个文件中，每个文件定义一个类，适合较大项目，能够提高可维护性和可读性。

# 二、 Object中的方法
[![pA0EdxK.png](https://s21.ax1x.com/2024/10/26/pA0EdxK.png)](https://imgse.com/i/pA0EdxK)
在 C# 中，`Object` 是所有类型的基类。每个类、结构、接口等都隐式地继承自 `Object`，因此它包含了一些通用的方法，这些方法可以在任何对象上调用。以下是 `Object` 类中的一些重要方法：
### 常用方法
#### 1 `ToString()`
- **描述**: 返回对象的字符串表示形式。
- **示例**:
```csharp
public class Person
{
    public string Name { get; set; }
    
    public override string ToString()
    {
        return $"Name: {Name}";
    }
}

Person person = new Person { Name = "Alice" };
Console.WriteLine(person.ToString()); // 输出: Name: Alice
```
#### 2 `Equals(Object obj)`
- **描述**: 确定两个对象是否相等。默认实现比较对象的引用（即内存地址），可以在子类中重写该方法来比较对象的内容。
- 还有一个是静态实现的
- **示例**:
```csharp
public class Person
{
    public string Name { get; set; }

    //public override bool Equals(object? obj)
    //{
    //    if (obj is Person other)
    //    {
    //        return Name == other.Name; // 比较名称
    //    }
    //    return false;
    //}
}
public class Program
{
    public static void Main()
    {
        Person p1 = new Person { Name = "Alice" };
        Person p2 = new Person { Name = "Alice" };
        Console.WriteLine(p1.Equals(p2)); // 输出: True
        // 看了源码，这里如果传入的对象，Person重写了Equals方法
        // 先判断两个对象的地址是否相同，相同返回True
        // 上面已经判断了，两个不可能都为努力了，如果有一个为null，返回True
        // 最后再调用Person重写的Equals方法
        // 如果没有重写，默认判断两者引用是否相同，this == p2
        Console.WriteLine(Object.Equals(p1, p2)); // 输出: True
    }
}
```
#### 3 `GetHashCode()`
- **描述**: 返回对象的哈希代码。通常与 `Equals()` 一起重写，以确保相等的对象具有相同的哈希值。
- **示例**:
```csharp
public class Person
{
    public string Name { get; set; }

    public override bool Equals(object? obj)
    {
        if (obj is Person other)
        {
            return Name == other.Name; // 比较名称
        }
        return false;
    }

    public override int GetHashCode()
    {
        return Name.GetHashCode(); // 根据名称生成哈希代码
    }
}

public class Program
{
    public static void Main()
    {
        Person p1 = new Person { Name = "Alice" };
        Person p2 = new Person { Name = "Alice" };
        Console.WriteLine(p1.Equals(p2)); // 输出: True
        Console.WriteLine(Object.Equals(p1, p2));
    }
}
```
#### 4 `GetType()`
- **描述**: 获取当前实例的 `Type` 对象，该对象表示当前实例的类型。
- **示例**:
```csharp
using PersonSpace;

namespace PersonSpace
{
    public class Person
    {
        public string Name { get; set; }

        public override bool Equals(object? obj)
        {
            if (obj is Person other)
            {
                return Name == other.Name; // 比较名称
            }
            return false;
        }

        public override int GetHashCode()
        {
            return Name.GetHashCode(); // 根据名称生成哈希代码
        }
    }
}
public class Program
{
    public static void Main()
    {
        Person person = new Person();
        Type type = person.GetType();
        Console.WriteLine(type);// 输出: Namespace.Person，如果没有命名空间输出Person
    }
}
```
#### 5 `Finalize()`
- **描述**: 释放对象所占用的资源。在垃圾回收过程中被调用，通常不需要手动调用。可以重写来定义释放非托管资源的逻辑。
- **示例**:
```csharp
public class Person
{
    ~Person()
    {
        // 清理代码
    }
}
```

#### 6 `public static bool ReferenceEquals(object objA, object objB);`
- **描述**: `Object.ReferenceEquals()` 是 C# 中的一个静态方法，用于比较两个对象的引用是否相同。它直接检查两个对象的内存地址，以确定它们是否指向同一个实例。这个方法与 `Equals` 方法不同， `Equals`通常用于检查对象内容是否相等（如果没有重写的话，默认还是比较引用是否相同），而 `ReferenceEquals` 专注于引用的比较。
- 值类型对象返回值始终是false
### 方法签名
```csharp
public static bool ReferenceEquals(object objA, object objB);
```
### 参数
- `objA`：第一个要比较的对象。
- `objB`：第二个要比较的对象。
### 返回值
- 返回 `true` 如果 `objA` 和 `objB` 是同一个引用（即它们指向同一个对象），否则返回 `false`。
### 示例
下面是一个简单的示例，展示了如何使用 `Object.ReferenceEquals` 方法。
```csharp
using System;

public class Program
{
    public static void Main()
    {
        // 创建两个不同的对象
        object obj1 = new object();
        object obj2 = new object();

        // 创建一个指向同一对象的引用
        object obj3 = obj1;

        // 比较对象引用
        Console.WriteLine(Object.ReferenceEquals(obj1, obj2)); // 输出: False
        Console.WriteLine(Object.ReferenceEquals(obj1, obj3)); // 输出: True
    }
}
```
### 详细说明
- **比较两个不同的对象**: 在上面的示例中，`obj1` 和 `obj2` 是两个不同的对象，因此 `Object.ReferenceEquals(obj1, obj2)` 返回 `false`。
- **比较相同的对象引用**: `obj3` 是对 `obj1` 的引用，因此 `Object.ReferenceEquals(obj1, obj3)` 返回 `true`。
### 适用场景
- `Object.ReferenceEquals` 通常用于需要明确区分对象的引用时，例如在性能敏感的代码中。
- 它可以用于确定对象是否为 `null`，因为比较 `null` 和非 `null` 对象时，`ReferenceEquals` 会返回 `false`。
### 注意事项
- `Object.ReferenceEquals` 不会触发任何重写的 `Equals` 方法，所以它的比较仅仅基于对象的引用，而不涉及内容的比较。
- 在比较值类型时，由于值类型通常在栈上分配，它们的比较通常是基于值而不是引用。因此，使用 `ReferenceEquals` 在比较值类型的实例时，效果有限。

# 三、 System.ValueType
在 C# 中，`System.ValueType` 是所有值类型的基类。所有的结构体（`struct`）和枚举（`enum`）都隐式地继承自 `ValueType`。理解 `ValueType` 的作用有助于更好地理解 C# 中的值类型及其行为。
### 1. 值类型与引用类型
- **值类型**：包含数据本身，存储在栈上（或内联分配在堆上）。例如，基本类型如 `int`、`double`、`char`，以及用户定义的结构体和枚举都是值类型。
- **引用类型**：包含指向数据的引用，存储在堆上。例如，类（`class`）、数组（`array`）和字符串（`string`）都是引用类型。

#### 内联分配在堆上
在 C# 中，值类型通常分配在栈上，但在某些情况下，它们也可以内联分配在堆上。这里是一些详细的解释：
##### 1. 值类型的基本特性
- **栈分配**：值类型通常直接存储在栈上。当一个值类型变量被声明时，它的值会被直接分配到栈内存中。例如：
```csharp
int x = 10; // x 存储在栈上
```
- **堆分配**：虽然值类型通常在栈上分配，但在某些情况下，它们会被分配到堆上。这种情况主要发生在以下场景中：
- **作为引用类型的字段**：当值类型作为类的字段时，该值类型的实例会存储在堆上，因为类是引用类型。例如： 
```csharp
class MyClass
{
    public int MyValue; // MyValue 是值类型，但它存储在堆上
}

MyClass obj = new MyClass(); // obj 的地址在堆上
obj.MyValue = 10; // MyValue 存储在 obj 所在的堆内存中
```
- **内联分配**：在某些情况下，值类型可以被 "内联分配" 在堆上，这意味着它们的值会被直接嵌入到包含它们的对象中。这种情况通常发生在使用 **结构体** 作为类字段时。例如：    
```csharp
struct MyStruct
{
    public int Value;
}

class Container
{
    public MyStruct myStruct; // myStruct 是值类型，但内联存储在堆上
}

Container container = new Container(); // container 存储在堆上
container.myStruct.Value = 42; // myStruct 的数据存储在 container 的堆内存中
```
##### 2. 值类型在堆中的存储
- 当值类型作为类的字段或属性时，它们的实例实际上会存储在堆上。这个特性使得值类型可以在类的生命周期内存在，并且可以通过引用访问。例如，当你创建一个类实例并使用该类的字段时，字段值会在堆上分配。
- 在内联分配的情况下，值类型的内存布局会与引用类型相结合，以提高性能。这种内联分配允许在一个对象中组合多个值类型和引用类型，避免了不必要的内存分配。
##### 3. 小结
- **栈分配**：值类型通常直接在栈上分配。
- **堆分配**：当值类型作为类字段时，它们的值在堆上分配。
- **内联分配**：值类型可以内联到对象中，优化内存使用和性能。
### 2. `ValueType` 的主要特性
- **封装**：`ValueType` 提供了一些基本的行为实现，例如 `Equals()`、`GetHashCode()` 和 `ToString()` 方法。这些方法可以在值类型的实例上被调用。
- **值语义**：当值类型被赋值给另一个变量或作为参数传递时，整个对象的值被复制。这与引用类型不同，后者只复制引用。
- **装箱和拆箱**：值类型可以被装箱（将其转换为 `object` 类型）和拆箱（将其从 `object` 转换回值类型），这会引入额外的性能开销。
### 3. 示例代码
以下是一个简单的示例，演示了如何使用值类型和 `ValueType` 的一些特性：
```csharp
using System;

struct MyStruct : IComparable
{
    public int Value;

    public MyStruct(int value)
    {
        Value = value;
    }

    public int CompareTo(object obj)
    {
        if (obj is MyStruct other)
        {
            return Value.CompareTo(other.Value);
        }
        throw new ArgumentException("Object is not a MyStruct");
    }

    public override string ToString()
    {
        return Value.ToString();
    }
}

class Program
{
    static void Main()
    {
        MyStruct struct1 = new MyStruct(10);
        MyStruct struct2 = new MyStruct(20);

        Console.WriteLine(struct1.ToString()); // 输出: 10
        Console.WriteLine(struct2.ToString()); // 输出: 20

        Console.WriteLine(struct1.Equals(struct2)); // 输出: False

        // 装箱
        object boxed = struct1;
        // 拆箱
        MyStruct unboxed = (MyStruct)boxed;

        Console.WriteLine(unboxed.ToString()); // 输出: 10
    }
}
```
### 4. 重要方法
`ValueType` 提供了几个重要的方法，通常被重写以支持特定的值类型：
- `Equals(object obj)`：判断当前实例与给定对象是否相等。默认实现比较引用，通常应被重写以比较值。
- `GetHashCode()`：返回当前对象的哈希代码，通常应根据对象的字段重写。
- `ToString()`：返回当前对象的字符串表示形式，通常应被重写以提供有意义的输出。
### 总结
`System.ValueType` 是 C# 中所有值类型的基类，提供了一些基础方法和值语义的支持。理解值类型的行为及其与引用类型的区别，对于有效地编写 C# 代码至关重要。通过实现自定义结构体和重写 `ValueType` 中的方法，可以创建更丰富的值类型功能。

# 四、 string相关方法和操作
在 C# 中，`string` 是一个常用的引用类型，表示不可变的文本序列。以下是一些常用的 `string` 相关方法和操作：
### 1. 基本操作
- **创建字符串**：
```csharp
string str1 = "Hello, World!";
string str2 = new string('a', 5); // 创建 "aaaaa"
```
- **字符串拼接**：
```csharp
string str3 = "Hello";
string str4 = "World";
string result = str3 + ", " + str4 + "!"; // "Hello, World!"
```
- **字符串格式化**：
```csharp
int age = 25;
string formattedString = string.Format("I am {0} years old.", age); // "I am 25 years old."
string interpolatedString = $"I am {age} years old."; // 使用字符串插值
```
### 2. 常用方法
- `Length`：获取字符串的长度。
```csharp
string str = "Hello";
int length = str.Length; // 5
```
- `ToUpper()` / `ToLower()`：将字符串转换为大写或小写。
- 不会改变原字符串
```csharp
string upper = str.ToUpper(); // "HELLO"
string lower = str.ToLower(); // "hello"
```
- `Trim()` / `TrimStart()` / `TrimEnd()`：去除字符串开头或结尾的空白字符。
```csharp
string strWithSpaces = "  Hello  ";
string trimmed = strWithSpaces.Trim(); // "Hello"
```
- `Substring(int startIndex)`：从指定位置开始截取后续字符串的全部。
- `Substring(int startIndex, int length)`：从指定位置开始截取字符串的一部分。
	- Substring有第二个参数的时候会要求`startIndex+length`小于等于str长度，否则报错`System.ArgumentOutOfRangeException`
- 不会改变原字符串
```csharp
string subAll = str.Substring(1); // "ello"
string sub = str.Substring(1, 3); // "ell"
```
- `Contains(string value)`：检查字符串是否包含指定的子字符串。
```csharp
bool contains = str.Contains("ell"); // true
```
- `StartsWith(string value)` / `EndsWith(string value)`：检查字符串是否以指定的子字符串开头或结尾。
```csharp
bool startsWith = str.StartsWith("He"); // true
bool endsWith = str.EndsWith("lo"); // true
```
- `IndexOf(string value)` / `LastIndexOf(string value)`：返回指定子字符串的第一个或最后一个出现的位置。
- 字符可以是中文
- 没找到返回-1
```csharp
int index = str.IndexOf("l"); // 2
int lastIndex = str.LastIndexOf("l"); // 3
```
- `Replace(string oldValue, string newValue)`：替换字符串中的`所有`匹配子字符串。
```csharp
string replaced = str.Replace("l", "x"); // "Hexxo"
```
### 3. 字符串分割与连接
- `Split(char[] separator)`：根据指定字符分隔符分割字符串。
```csharp
string sentence = "Hello, World!";
string[] words = sentence.Split(new char[] { ' ', ',' }); // {"Hello", "", "World!"}
```
- `Join(string separator, string[] values)`：连接字符串数组中的元素。
```csharp
string[] array = { "Hello", "World" };
string joined = string.Join(", ", array); // "Hello, World"
```
### 4. 字符串比较
- `Equals(string value)`：比较字符串是否相等。
```csharp
bool isEqual = str.Equals("Hello"); // true
```
- `Compare(string strA, string strB)`：比较两个字符串的大小。
- 每个字符的比较是基于其 `Unicode 编码值`的大小。
- A小于B，a小于b
- A小于a
```csharp
int comparisonResult = string.Compare("abc", "xyz"); // 返回负数，表示 "abc" 小于 "xyz"
```
### 5. 其他操作
- `IsNullOrEmpty(string value)`：检查字符串是否为 `null` 或空字符串。
```csharp
bool isNullOrEmpty = string.IsNullOrEmpty(str); // false
```
- `IsNullOrWhiteSpace(string value)`：检查字符串是否为 `null`、空字符串或仅包含空白字符。
```csharp
bool isNullOrWhiteSpace = string.IsNullOrWhiteSpace("   "); // true
```
### 6. 不可变性和性能
由于字符串是不可变的，每次执行类似于 `Replace`、`ToUpper` 等操作时，都会创建一个新的字符串对象。这会带来一些性能开销，因此如果需要大量字符串操作，建议使用 `StringBuilder`：
```csharp
using System.Text;

StringBuilder sb = new StringBuilder();
sb.Append("Hello");
sb.Append(", ");
sb.Append("World");
string finalString = sb.ToString(); // "Hello, World"
```
这些方法和操作涵盖了 C# 中 `string` 类型的常用功能，有助于处理各种字符串操作需求。
### 7. 移除指定位置后的字符
在 C# 中，`string.Remove` 方法用于删除字符串中的某一部分。这个方法会返回一个新的字符串，因为字符串在 C# 中是不可变的（即操作后原字符串不会被改变）。
#### 方法签名
`string.Remove` 有两个重载版本：
1. `Remove(int startIndex)`：删除从指定索引处开始的所有字符。
2. `Remove(int startIndex, int count)`：删除从指定索引处开始的指定数量的字符。
#### 参数说明
- `startIndex`：要开始删除的字符的零基索引位置。
- `count`（可选）：要删除的字符数。不填，则后续的所有字符都删除
#### 示例
1. **删除从指定索引开始的所有字符**：
```csharp
string str = "Hello, World!";
string result = str.Remove(5); // 从索引 5 开始删除，即删除 ", World!"

Console.WriteLine(result); // 输出 "Hello"
```
2. **删除从指定索引开始的指定数量的字符**：
```csharp
string str = "Hello, World!";
string result = str.Remove(7, 5); // 从索引 7 开始删除 5 个字符，即删除 "World"

Console.WriteLine(result); // 输出 "Hello, !"
```
#### 注意事项
- 如果 `startIndex` 超出字符串的范围，会抛出 `ArgumentOutOfRangeException`。
- 如果 `count` 的值导致删除的范围超出字符串的长度，也会抛出 `ArgumentOutOfRangeException`。
- `Remove` 方法不会修改原始字符串，而是返回一个新的字符串。

### 8. 字符串分割
在 C# 中，`string.Split` 方法用于将字符串拆分为子字符串数组，基于一个或多个分隔符。分割后的子字符串不包含分隔符。
#### 方法签名
`string.Split` 有多种重载，常用的有以下几种：
1. `Split(char[] separator)` ：使用一个或多个字符作为分隔符进行分割。
2. `Split(char[] separator, int count)` ：使用分隔符进行分割，并限定返回的子字符串的数量。
3. `Split(char[] separator, StringSplitOptions options)` ：使用分隔符进行分割，并通过 `StringSplitOptions` 控制是否移除空字符串。
4. `Split(string[] separator, StringSplitOptions options)`：使用字符串数组作为分隔符。
#### 参数说明
- `separator`：用于分割的字符数组或字符串数组。如果传入 `null` 或空数组，则使用空白字符（如空格、制表符）作为默认分隔符。
- `count`：限定返回的子字符串数量。
- `options`：`StringSplitOptions` 枚举，用于控制是否移除空字符串。可以为 `StringSplitOptions.None`（不移除空字符串）或 `StringSplitOptions.RemoveEmptyEntries`（移除空字符串）。
- ','也是一个`char[]`
	```csharp
	string str = "apple,banana,orange";
	
	// 使用字符数组作为分隔符
	string[] fruits1 = str.Split(new char[] { ',' });
	
	// 使用单个字符作为分隔符，自动转换为字符数组
	string[] fruits2 = str.Split(',');
	
	// 两种方式的结果是相同的
	foreach (var fruit in fruits1)
	{
	    Console.WriteLine(fruit); // 输出: apple, banana, orange
	}
	```
#### 示例
1. **使用单个字符分隔符**
```csharp
string str = "apple,banana,orange";
string[] fruits = str.Split(','); // 使用逗号分隔

// 输出: apple, banana, orange
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
2. **使用多个字符作为分隔符**
```csharp
using System;

string str = "apple;banana,orange";
string[] fruits = str.Split(new char[] { ',', ';' }); // 使用逗号和分号分隔

// 输出: 
// apple
// banana
// orange
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
3. **限定返回的子字符串数量**
```csharp
string str = "apple,banana,orange";
string[] fruits = str.Split(new char[] { ',' }, 2); // 只分割成两个子字符串

// 输出: apple, banana,orange
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
4. **移除空字符串**
```csharp
string str = "apple,,banana,orange";
string[] fruits = str.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries); // 移除空字符串

// 输出: apple, banana, orange
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
5. **使用字符串作为分隔符**
```csharp
string str = "apple#banana#orange";
// string[] fruits = str.Split(new char[] { '#' }, StringSplitOptions.None); // 使用 '#' 作为分隔符一样的
string[] fruits = str.Split(new string[] { "#b" }, StringSplitOptions.None); // 使用 "#" 作为分隔符，""里面可以更长，比如"#b"

// 输出: apple anana#orange
foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```
#### 注意事项
- `Split` 返回的数组中可能包含空字符串，特别是在分隔符连续出现时。
- 如果需要移除结果中的空字符串，可以使用 `StringSplitOptions.RemoveEmptyEntries`。
	在 C# 中，`StringSplitOptions` 枚举用于控制 `string.Split` 方法的行为，它有三个可选值，分别是 `None`、`RemoveEmptyEntries` 和 `TrimEntries`，每个选项的作用如下：
	##### 1. `None`
	- 默认选项，值为 `0`。
	- 当使用此选项时，`Split` 方法不会忽略空字符串，也不会去除空白字符。
	- 返回的数组中会包含原字符串中所有的子字符串，即使其中有空字符串。
	##### 2. `RemoveEmptyEntries`
	- 值为 `1`。
	- 如果选择此选项，`Split` 方法会从返回的数组中移除任何空字符串。
	- 空字符串通常出现在分隔符连续出现的情况下。
	- 如果与 `TrimEntries` 结合使用，则仅包含空白字符的子字符串也会被移除。
	##### 3. `TrimEntries` (仅在 .NET 5 及更高版本可用)
	- 值为 `2`。
	- 当使用此选项时，`Split` 方法会去除每个子字符串前后的空白字符。
	- 如果与 `RemoveEmptyEntries` 结合使用，那么只包含空白字符的子字符串也会被移除。
	- 组合使用
		`StringSplitOptions` 是一个带有 `[Flags]` 特性的枚举，可以组合多个选项一起使用。例如，使用 `RemoveEmptyEntries | TrimEntries` 可以同时移除空字符串并修剪子字符串中的空白字符。
	- 示例
		```csharp
		string str = " apple , banana , , orange ,  ";
		
		// 使用 StringSplitOptions.None，默认行为
		string[] result1 = str.Split(',', StringSplitOptions.None);
		// 输出: " apple ", " banana ", " ", " orange ", "  "
		
		// 使用 StringSplitOptions.RemoveEmptyEntries，移除空字符串
		string[] result2 = str.Split(',', StringSplitOptions.RemoveEmptyEntries);
		// 输出: " apple ", " banana ", " orange ", "  "
		
		// 使用 StringSplitOptions.TrimEntries，去除每个子字符串的空白
		string[] result3 = str.Split(',', StringSplitOptions.TrimEntries);
		// 输出: "apple", "banana", "", "orange", ""
		
		// 使用 StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries，组合行为
		string[] result4 = str.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
		// 输出: "apple", "banana", "orange"
		```

# 五、 ArrayList
`ArrayList` 是 C# 中的一个非泛型集合类，位于 `System.Collections` 命名空间中。它可以存储任何类型的对象，并且大小是动态调整的。尽管 `ArrayList` 具有灵活性和动态性，但因为它是非泛型的，通常在现代开发中会推荐使用泛型集合（如 `List<T>`）以获得更好的类型安全和性能。

### 1. **ArrayList 的特点**

- **非泛型集合**：`ArrayList` 可以存储不同类型的对象（`object` 类型），但这会导致类型不安全和装箱/拆箱的开销。
- **动态大小**：不像数组那样大小固定，`ArrayList` 的大小可以根据需要自动增加。
- **实现了 `IList`、`ICollection` 和 `IEnumerable` 接口**：因此，它可以与这些接口一起使用。

### 2. **常用方法和属性**

- `Add(object value)`：将对象添加到 `ArrayList` 的末尾。
- `AddRange(ICollection c)` 是 `ArrayList` 类中的一个方法，用于将一个集合中的元素添加到 `ArrayList` 的末尾。与 `Add` 方法不同的是，`AddRange` 可以一次性添加多个元素，而不是一个一个地添加。
	- **支持不同的集合类型**：只要实现了 `ICollection` 接口的集合，都可以使用 `AddRange` 方法添加。
- `Insert(int index, object value)`：在指定索引位置插入一个元素。
- `Remove(object value)`：移除 `ArrayList` 中第一次出现的指定对象。
- `RemoveAt(int index)`：移除指定索引处的元素。
- `Clear()`：移除所有元素。
- `Contains(object value)`：确定某个对象是否在 `ArrayList` 中。
- `IndexOf(object value)`：返回指定对象在 `ArrayList` 中的第一个匹配项的索引。正向查找元素位置，找不到返回-1
- `LastIndexOf(object value)` 是 `ArrayList` 类中的一个方法，用于查找指定元素在 `ArrayList` 中最后一次出现的索引位置。这个方法从 `ArrayList` 的末尾向前搜索，返回匹配的元素的索引。
	- `LastIndexOf(object value, int startIndex)`：从指定的索引位置向前搜索。
	- `LastIndexOf(object value, int startIndex, int count)`：从指定的索引位置开始，向前搜索指定数量的元素。
- `Count`：获取 `ArrayList` 中的元素数量。
- `Capacity`：获取或设置 `ArrayList` 能够包含的元素数量。
- `Sort()`：对 `ArrayList` 中的元素进行排序。
- `Reverse()`：反转 `ArrayList` 中元素的顺序。

### 3. **ArrayList 的使用示例**

```csharp
using System;
using System.Collections;

class Program
{
    static void Main()
    {
        // 创建一个新的 ArrayList
        ArrayList list = new ArrayList();

        // 添加元素
        list.Add(1);
        list.Add("Hello");
        list.Add(3.14);
        list.Add(true);

        // 插入元素
        list.Insert(2, "Inserted Element");

        // 遍历 ArrayList
        Console.WriteLine("ArrayList elements:");
        foreach (var item in list)
        {
            Console.WriteLine(item);
        }

        // 检查是否包含某个元素
        bool containsHello = list.Contains("Hello");
        Console.WriteLine($"\nContains 'Hello': {containsHello}");

        // 获取索引
        int index = list.IndexOf(3.14);
        Console.WriteLine($"Index of 3.14: {index}");

        // 移除元素
        list.Remove(true);
        list.RemoveAt(0);

        // 清空 ArrayList
        list.Clear();
        Console.WriteLine($"\nArrayList count after clearing: {list.Count}");
    }
}
```
### 4. **ArrayList 的缺点**
- **类型安全性差**：因为 `ArrayList` 是非泛型的，它可以存储不同类型的数据，这在运行时可能会导致类型转换异常。
- **装箱和拆箱的性能开销**：对于值类型（如 `int`），在添加到 `ArrayList` 时需要装箱，将其转换为 `object` 类型，而在访问时又需要拆箱，带来性能损耗。
### 5. **替代方案**
在 C# 中，如果需要类型安全的集合，建议使用泛型集合类，如 `List<T>`，它在提供与 `ArrayList` 类似功能的同时还支持类型检查，并`避免装箱/拆箱`的性能开销。
```csharp
List<int> intList = new List<int>();
intList.Add(1);
intList.Add(2);
```
`List<T>` 是 `ArrayList` 更加推荐的替代方案，因为它支持泛型，提供更好的性能和类型安全性。

# 六、 栈
栈（Stack）是一种后进先出（LIFO，Last In, First Out）的数据结构，这意味着最后添加的元素最先被移除。栈具有以下几个主要特点和操作：
### 1. **栈的基本概念**
- **后进先出（LIFO）**：栈的特点是“后进先出”，即最后压入栈的元素最先被弹出。
- **栈的两个主要操作**：
    - **Push()** ：将元素压入栈顶。
    - **Pop()** ：从栈顶移除并返回元素。
- **其他常见操作**：
    - **Peek()** ：查看栈顶的元素但不移除它。
    - **Contains<>()或Contains()** ：是否存在某个元素
	    - **类型安全性**：在编译时检查类型，避免类型转换的错误。例如，如果 `List<int>` 类型的集合调用 `Contains` 方法，就只能传入 `int` 类型的值，否则会出现编译错误。
		- **性能更高**：泛型集合在检查元素时不需要进行装箱和拆箱操作（适用于值类型），直接比较元素的值，因此性能较高。
		- **避免类型转换问题**：由于泛型集合存储的是具体类型的元素，所以 `Contains` 方法可以直接对该类型的值进行比较，而不需要进行类型转换。
    - **Count** ：栈内元素个数
    - **Clear()** ：清空栈内元素
### 2. **栈的实现**
栈可以用数组或链表来实现。数组实现栈时，栈顶的位置随着元素的增加或移除而变化。链表实现栈时，新元素总是插入到链表的头部。
### 3. **栈在编程中的应用**
- **函数调用栈**：程序执行时，每个函数调用都会把当前执行状态（如局部变量、返回地址）压入调用栈，函数返回时，从栈顶弹出恢复状态。
- **表达式求值和语法分析**：在编译器中，栈用于解析表达式的操作顺序和括号匹配。
- **撤销操作**：应用程序（如文本编辑器）中常用栈来记录操作历史，以便进行撤销（Undo）操作。
### 4. **C# 中的栈**
在 C# 中，可以使用 `System.Collections.Generic` 命名空间中的 `Stack<T>` 泛型类来实现栈操作。
#### 示例代码
```csharp
using System;
using System.Collections.Generic;
using System.Collections;

class Program
{
    static void Main()
    {
        //************************************
        // 非泛型 using System.Collections;其他操作和泛型一致
        Stack stackObj = new Stack();


        // using System.Collections.Generic;
        Stack<int> stack = new Stack<int>();

        // Push操作：将元素压入栈
        stack.Push(1);
        stack.Push(2);
        stack.Push(3);

        // 遍历一
        foreach (int item in stack)
        {
            Console.WriteLine(item);
        }

        // 遍历二
        int[] array = stack.ToArray();
        // object[] array = stack.ToArray();
        for (int i = 0; i < array.Length; i++)
        {
            Console.WriteLine(array[i]);
        }

        // Peek操作：查看栈顶元素
        Console.WriteLine("Top element is: " + stack.Peek());

        // Pop操作：弹出栈顶元素
        Console.WriteLine("Popped element is: " + stack.Pop());

        // Contains操作：查看元素是否存在
        if (stack.Contains(1))
        {
            Console.WriteLine("存在1");
        }

        // 再次查看栈顶元素
        Console.WriteLine("Top element after pop is: " + stack.Peek());

        // 检查栈是否为空
        Console.WriteLine("Is stack empty? " + (stack.Count == 0));

        // 循环弹栈
        while (stack.Count > 0)
        {
            Console.WriteLine(stack.Pop());
        }

        // 清空栈
        stack.Clear();
        // 查看栈的元素个数
        Console.WriteLine(stack.Count);
    }
}
```
#### 输出
```csharp
3
2
1
3
2
1
Top element is: 3
Popped element is: 3
存在1
Top element after pop is: 2
Is stack empty? False
2
1
0
```
### 5. **栈在内存管理中的应用**
在程序运行时，栈内存用于存储函数的局部变量和参数。当函数调用时，局部变量会被压入栈，函数返回时会自动从栈中弹出这些变量。
- **栈内存**：栈内存管理快速，但容量有限，适用于生命周期短的对象和变量。
- **堆内存**：用于动态分配的内存，适合存储生命周期较长或大小可变的数据。
### 6. 泛型栈 (`Stack<T>`)
- **定义**：栈是一种后进先出（LIFO，Last In First Out）的数据结构。最后放入栈中的元素最先被取出。
- **常用方法**：和普通栈一样
    - `Push(T item)`：将元素推入栈中。
    - `Pop()`：移除并返回栈顶的元素。
    - `Peek()`：返回栈顶的元素，但不移除它。
    - `Count`：获取栈中元素的数量。
- **示例**：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建一个泛型栈
        Stack<int> stack = new Stack<int>();

        // 压入元素
        stack.Push(1);
        stack.Push(2);
        stack.Push(3);

        // 输出栈顶元素
        Console.WriteLine("栈顶元素: " + stack.Peek()); // 输出 3

        // 弹出元素
        Console.WriteLine("弹出元素: " + stack.Pop()); // 输出 3

        // 栈中剩余元素数量
        Console.WriteLine("栈中元素数量: " + stack.Count); // 输出 2
    }
}
```
# 七、 队列
队列（Queue）是一种先进先出（FIFO，First In, First Out）的数据结构，这意味着第一个加入队列的元素最先被移除。队列的基本操作和特点如下：
### 1. **队列的基本概念**
- **先进先出（FIFO）**：队列的特点是“先进先出”，即最先入队的元素最先被移出队列。
- **队列的主要操作**：
    - **Enqueue()** ：将元素添加到队列的末尾（尾部）。
    - **Dequeue()** ：移除并返回队列的第一个元素（头部）。
- **其他常见操作**：
    - **Peek()** ：查看队列的第一个元素但不移除它。
    - **Count** ：查看队列元素个数,没有IsEmpty()方法
    - **Clear()** ：清空队列元素
    - **Contains<>()或Contains()** ：是否存在某个元素
	    - **类型安全性**：在编译时检查类型，避免类型转换的错误。例如，如果 `List<int>` 类型的集合调用 `Contains` 方法，就只能传入 `int` 类型的值，否则会出现编译错误。
		- **性能更高**：泛型集合在检查元素时不需要进行装箱和拆箱操作（适用于值类型），直接比较元素的值，因此性能较高。
		- **避免类型转换问题**：由于泛型集合存储的是具体类型的元素，所以 `Contains` 方法可以直接对该类型的值进行比较，而不需要进行类型转换。
### 2. **队列的实现**
队列可以通过数组或链表来实现。用数组实现时，需要处理循环队列以节省空间。用链表实现时，新元素添加到链表的尾部，元素移除从链表的头部进行。
### 3. **队列的应用**
- **任务调度**：在操作系统中，队列用于管理任务的调度，保证先到的任务先执行。
- **消息处理**：在网络通信中，队列用于缓冲数据，按顺序处理消息。
- **宽度优先搜索（BFS）**：在图的遍历算法中，队列用于按层次遍历图中的节点。
### 4. **C# 中的队列**
在 C# 中，可以使用 `System.Collections.Generic` 命名空间中的 `Queue<T>` 泛型类来实现队列操作。
#### 示例代码
- 遍历什么的和栈一样，方法也基本一致，只是数据结构不同
```csharp
using System;
using System.Collections.Generic;
using System.Collections;

class Program
{
    static void Main()
    {
        // 非泛型，就是所有的Object
        // using System.Collections;
        Queue queueAll = new Queue();
        Queue<string> queue = new Queue<string>();

        // Enqueue操作：将元素添加到队列的末尾
        queue.Enqueue("First");
        queue.Enqueue("Second");
        queue.Enqueue("Third");

        // Peek操作：查看队列的第一个元素
        Console.WriteLine("Front element is: " + queue.Peek());

        // Contains：查看是否包含某个元素 也可以直接Contains,不用泛型
        Console.WriteLine("First element contains?" + queue.Contains<string>("First"));


        // Dequeue操作：移除并返回队列的第一个元素
        Console.WriteLine("Dequeued element is: " + queue.Dequeue());

        // 再次查看队列的第一个元素
        Console.WriteLine("Front element after dequeue is: " + queue.Peek());


        // 清空队列
        queue.Clear();
        // 检查队列是否为空，队列元素个数
        Console.WriteLine("Is queue empty? " + (queue.Count == 0));
    }
}
```
#### 输出
```csharp
Front element is: First
First element contains?True
Dequeued element is: First
Front element after dequeue is: Second
Is queue empty? True
```
### 5. **双端队列（Deque）**
双端队列是一种特殊的队列，允许从两端进行插入和删除操作。在 C# 中，可以使用 `System.Collections.Generic` 命名空间下的 `LinkedList<T>` 来实现双端队列的功能。
### 6. **队列在内存管理中的应用**
队列通常用作任务队列或事件队列，用于按顺序处理异步事件或任务。
队列是一种简单而灵活的数据结构，在实际开发中广泛应用于各种场景。
### 7. **泛型队列 (`Queue<T>`)**
- **定义**：队列是一种先进先出（FIFO，First In First Out）的数据结构。最先放入队列中的元素最先被取出。
- **常用方法**：和普通队列一样
    - `Enqueue(T item)`：将元素添加到队列的末尾。
    - `Dequeue()`：移除并返回队列头部的元素。
    - `Peek()`：返回队列头部的元素，但不移除它。
    - `Count`：获取队列中元素的数量。
- **示例**：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建一个泛型队列
        Queue<string> queue = new Queue<string>();

        // 入队元素
        queue.Enqueue("Alice");
        queue.Enqueue("Bob");
        queue.Enqueue("Charlie");

        // 输出队头元素
        Console.WriteLine("队头元素: " + queue.Peek()); // 输出 Alice

        // 出队元素
        Console.WriteLine("出队元素: " + queue.Dequeue()); // 输出 Alice

        // 队列中剩余元素数量
        Console.WriteLine("队列中元素数量: " + queue.Count); // 输出 2
    }
}
```
# 八、 Hashtable
在 C# 中，`Hashtable` 是一种以键-值对存储数据的集合，属于非泛型集合类，位于 `System.Collections` 命名空间中。它通过键来快速查找值，使用哈希函数来确定每个键的位置。
### 1. **基本特点**
- **键值对存储**：`Hashtable` 存储数据时使用键-值对的形式，每个键必须是唯一的，但值可以重复。
- **哈希表实现**：通过哈希算法将键映射到特定的存储位置，从而实现快速的数据查找。
- **非泛型集合**：`Hashtable` 存储的键和值都是 `object` 类型，因此需要进行类型转换。
- **自动调整容量**：当元素数量超过某个阈值时，`Hashtable` 会自动扩展其容量，以提高性能。
### 2. **常用操作**
- **添加元素**：使用 `Add()` 方法添加键-值对。
- **访问元素**：使用索引器 `[]` 访问或修改指定键的值。
- **删除元素**：使用 `Remove()` 方法删除指定键的键-值对。
	- 如果删除不存在的键，不报错，只是没反应
- **检查键是否存在**：使用 `ContainsKey()` 方法判断某个键是否存在。
	- 和Contains()一样的，都是判断某个键是否存在
- **检查值是否存在**：使用 `ContainsValue()` 方法判断某个值是否存在。
- **清空集合**：使用 `Clear()` 方法清空所有元素。
- **键值对对数**：使用`Count`字段获取键值对对数。
- **键对**：`Keys`字段获取键对
- **值对**：`Values`字段获取值对
### 3. **示例代码**
下面是一些常用操作的示例：
```csharp
using System;
using System.Collections;

class Program
{
    static void Main()
    {
        // 创建一个 Hashtable
        Hashtable hashtable = new Hashtable();

        // 添加键值对
        hashtable.Add("apple", 5);
        hashtable.Add("banana", 3);
        hashtable.Add("orange", 8);

        // 访问元素
        Console.WriteLine("Apple count: " + hashtable["apple"]);

        // 修改元素
        hashtable["banana"] = 10;
        Console.WriteLine("Banana count (updated): " + hashtable["banana"]);

        // 检查键是否存在
        if (hashtable.ContainsKey("orange"))
        {
            Console.WriteLine("Orange is in the hashtable.");
        }

        // 删除元素
        hashtable.Remove("apple");
        Console.WriteLine("After removing apple, count: " + hashtable.Count);

        // 遍历所有键,这里用var或者object都可以
        foreach (var item in hashtable.Keys)
        {
            Console.WriteLine("key: " + item);
            Console.WriteLine("value: " + hashtable[item]);
        }

        // 遍历所有的值
        foreach (var item in hashtable.Values)
        {
            Console.WriteLine("value: " + item);
        }

        // 键值对一起遍历
        foreach (var item in hashtable)
        {
            Console.WriteLine("key: " + ((DictionaryEntry)item).Key);
            Console.WriteLine("value: " + ((DictionaryEntry)item).Value);
        }

        // 键值对一起遍历
        foreach (var item in hashtable)
        {
            var entry = item as DictionaryEntry?;

            // 判断当前对象是否为一个有效值，如果是，返回true
            // 如果不是，意味着是null，返回false
            // entry必须是一个可以为null的值，所以上面加了?
            if (entry.HasValue)
            {
                Console.WriteLine("key: " + entry.Value.Key);
                Console.WriteLine("value: " + entry.Value.Value);
            }
        }

        // 使用 GetEnumerator 方法遍历 Hashtable
        // 迭代器遍历法
        IDictionaryEnumerator enumerator = hashtable.GetEnumerator();

        while (enumerator.MoveNext())
        {
            // 访问当前键值对
            Console.WriteLine("key: " + enumerator.Key);
            Console.WriteLine("value: " + enumerator.Value);
        }

        // 使用 LINQ 遍历 Hashtable,不建议用这种方法遍历
        /*
        关键点
        类型转换：Cast<DictionaryEntry>() 将 Hashtable 中的每个元素转换为 DictionaryEntry 类型。
        这是因为 Hashtable 中的元素实际上是 DictionaryEntry 对象。
        LINQ 集合操作：通过将 Hashtable 转换为 IEnumerable<DictionaryEntry>，
        你可以使用 LINQ 的其他功能，例如 Where、Select、OrderBy 等，
        对元素进行过滤和操作。
        安全性：使用 Cast<T>() 进行类型转换时，如果集合中的元素不能转换为指定类型，
        将抛出异常。因此，确保 Hashtable 中的元素是 DictionaryEntry 类型是很重要的。
        */
        var entries = hashtable.Cast<DictionaryEntry>();

        foreach (var item in entries)
        {
            // 尝试将 item 转换为可空类型
            var entry = item as DictionaryEntry?;

            // 检查 entry 是否有值
            if (entry.HasValue)
            {
                Console.WriteLine("key: " + entry.Value.Key);
                Console.WriteLine("value: " + entry.Value.Value);
            }
        }


        // 清空 Hashtable
        hashtable.Clear();
        // 得到键值对 对数
        Console.WriteLine("After clearing, count: " + hashtable.Count);

    }
}
```
#### 输出结果
```csharp
Banana count (updated): 10
Orange is in the hashtable.
After removing apple, count: 2
key: orange
value: 8
key: banana
value: 10
value: 8
value: 10
key: orange
value: 8
key: banana
value: 10
key: orange
value: 8
key: banana
value: 10
key: orange
value: 8
key: banana
value: 10
key: orange
value: 8
key: banana
value: 10
After clearing, count: 0
```
### 4. **性能和使用注意事项**
- **性能优势**：`Hashtable` 具有 O(1) 的查找、插入和删除复杂度（在理想情况下），适用于需要快速查找数据的场景。
- **装箱/拆箱开销**：由于 `Hashtable` 是非泛型集合，存储值类型时会发生装箱和拆箱操作，可能导致性能开销。
- **线程安全**：`Hashtable` 本身不是线程安全的，如果需要在多线程环境中使用，可以使用同步方法 `Hashtable.Synchronized` 或其他线程安全的集合。
### 5. **与 `Dictionary<TKey, TValue>` 的比较**
`Hashtable` 是早期的非泛型集合类，现在大部分情况下推荐使用 `Dictionary<TKey, TValue>`，因为：
- **类型安全**：`Dictionary<TKey, TValue>` 是泛型集合，避免了类型转换的错误。
- **性能更高**：对于值类型的数据，`Dictionary<TKey, TValue>` 避免了装箱/拆箱操作。
- **代码可读性**：使用泛型时，类型信息更加明确，提高了代码的可读性和维护性。

`Hashtable` 适用于简单的键值存储场景，但在现代 C# 编程中，通常建议使用泛型的 `Dictionary<TKey, TValue>`。

# 九、 var和object
在 C# 中，`var` 和 `object` 都可以用来声明变量，但它们有不同的用法和意义：
1. **`var` 的特点**：
    - **类型推断**：`var` 是编译时的类型推断，编译器会根据右侧赋值的类型自动推断变量的类型。例如，如果赋值是一个 `int`，`var` 就会被推断为 `int` 类型。
    - **强类型**：使用 `var` 声明的变量在编译时类型是确定的，并且不能更改。虽然看起来像动态类型，但实际上是强类型的变量。
    - **优点**：代码更简洁，特别是当类型名称很长或复杂时；减少显式类型声明带来的冗余。
```csharp
var number = 10; // 推断为 int
var text = "Hello"; // 推断为 string
```
2. **`object` 的特点**：
    - **基础类型**：`object` 是所有类型的基类，意味着任何类型的变量都可以赋值给 `object`。当存储值类型时，会发生装箱操作。
    - **需要类型转换**：如果使用 `object` 进行类型存储，取出时需要进行显式类型转换，并且在编译时不会检查类型安全。
    - **优点**：适用于需要存储不同类型的情况或无法提前确定类型的场景。
```csharp
object number = 10; // 装箱为 object
object text = "Hello"; // 作为 object 类型存储
```
3. **区别和建议**：
    - 使用 `var` 可以保留强类型的优势，同时简化代码，是更推荐的做法，特别是在类型明确的情况下。
    - 使用 `object` 需要在取出时进行类型转换，可能会带来额外的性能开销和类型安全性问题。
    - 在迭代集合时，如果知道集合的元素类型，建议使用 `var`，因为编译器会推断出正确的类型，代码更清晰且类型安全。
在遍历 `Hashtable` 时，`var` 会推断出 `item` 的类型为 `object`，所以在这种情况下两者都能工作。但是使用 `var` 是更好的实践，因为它让代码看起来更简洁。
# 十、 泛型
泛型（Generics）是 C# 中的一种强大功能，允许你定义类、接口和方法时使用占位符（类型参数），而不需要在定义时指定具体的类型。这样，你可以在运行时使用不同的数据类型，提升代码的复用性和类型安全性。以下是关于泛型的一些关键点及示例。
- 如果有多个泛型占位字母，逗号分开即可
- 抽象类和静态类都不能作为泛型
```csharp
//泛型约束一共有6种
// 1.值类型                       where 泛型字母：struct
// 2.引用类型                     where 泛型字母：class
// 3.存在无参公共构造函数          where 泛型字母：new（）
// 4.某个类本身或者其派生类        where 泛型字母：类名
// 5.某个接口的派生类型            where 泛型字母：接口名
// 6.另一个泛型类型本身或者派生类型 where 泛型字母：另一个泛型字母
```
### 1. 泛型类
泛型类允许你创建一个类，其中的成员可以使用类型参数。这使得类可以处理不同的数据类型。
```csharp
public class GenericList<T>
{
    private List<T> items = new List<T>();

    public void Add(T item)
    {
        items.Add(item);
    }

	// 泛型类中的泛型方法
    public T Get(int index)
    {
        return items[index];
    }
}



class Program
{
    static void Main()
    {
        // 使用泛型类
        var intList = new GenericList<int>();
        intList.Add(1);
        intList.Add(2);
        Console.WriteLine(intList.Get(0)); // 输出 1

        var stringList = new GenericList<string>();
        stringList.Add("Hello");
        stringList.Add("World");
        Console.WriteLine(stringList.Get(1)); // 输出 World
    }
}
```
### 2. 泛型方法
你可以在类中定义泛型方法，这些方法可以接受不同的数据类型作为参数。
```csharp
public class Utilities
{
	// 普通类中的泛型方法
    public static T GetMax<T>(T a, T b) where T : IComparable
    {
        return (a.CompareTo(b) > 0) ? a : b;
    }
}

class Program
{
    static void Main()
    {
        // 使用泛型方法
        Console.WriteLine(Utilities.GetMax(5, 10)); // 输出 10
        Console.WriteLine(Utilities.GetMax("apple", "orange")); // 输出 orange
    }
}
```
### 3. 泛型接口
泛型接口允许你定义接口时使用类型参数，这使得实现该接口的类可以指定具体的类型。
```csharp
public interface IRepository<T>
{
    void Add(T item);
    T Get(int id);
}

public class Repository<T> : IRepository<T>
{
    private List<T> items = new List<T>();

    public void Add(T item)
    {
        items.Add(item);
    }

    public T Get(int id)
    {
        return items[id];
    }
}

class Program
{
    static void Main()
    {
        // 使用泛型接口
        var repo = new Repository<string>();
        repo.Add("Item1");
        Console.WriteLine(repo.Get(0)); // 输出 Item1
    }
}
```
### 4. 约束（Constraints）
泛型允许你使用约束来限制类型参数，这样你可以确保类型参数满足特定的条件。
```csharp
public class Example<T> where T : class // T 必须是引用类型
{
    public T Data { get; set; }

    public Example(T data)
    {
        Data = data;
    }
}

public class Person { }



class Program
{
    static void Main()
    {
        var personExample = new Example<Person>(new Person());
    }
}
```
在 C# 中，泛型约束（Generic Constraints）用于限制类型参数的特性。这使得在使用泛型时可以确保某些条件得到满足，从而提供编译时的类型安全性。以下是关于泛型约束的详细说明及示例。
#### 常见的泛型约束类型
##### 1. 引用类型约束
使用 `where T : class` 限制类型参数必须是引用类型。
```csharp
public class Example<T> where T : class
{
    public T Data { get; set; }

    public Example(T data)
    {
        Data = data;
    }
}

// 使用示例
var example = new Example<string>("Hello"); // 合法
// var example2 = new Example<int>(5); // 不合法，int 是值类型
```
##### 2. 值类型约束
使用 `where T : struct` 限制类型参数必须是值类型。
```csharp
public class ValueExample<T> where T : struct
{
    public T Data { get; set; }

    public ValueExample(T data)
    {
        Data = data;
    }
}

// 使用示例
var valueExample = new ValueExample<int>(5); // 合法
// var valueExample2 = new ValueExample<string>("Hello"); // 不合法，string 是引用类型
```
##### 3. `new()` 约束
`new()` 约束用于限制泛型类型参数必须具有一个无参数的构造函数。它允许在泛型类或方法中实例化类型参数。
**示例**：
```csharp
public class Factory<T> where T : new() // T 必须有无参构造函数
{
    public T CreateInstance()
    {
        return new T(); // 使用 new() 创建实例
    }
}

// 使用示例
public class MyClass
{
    public MyClass() { } // 无参构造函数
}

var factory = new Factory<MyClass>();
MyClass instance = factory.CreateInstance(); // 合法，创建 MyClass 实例
```
##### 4. 基类约束
使用 `where T : BaseClass` 限制类型参数必须是指定基类或者其的派生类。
```csharp
public class BaseClass { }
public class DerivedClass : BaseClass { }
public class BaseExample<T> where T : BaseClass
{
    public T Data { get; set; }

    public BaseExample(T data)
    {
        Data = data;
    }
}
class Program
{
    static void Main()
    {
        // 使用示例
        var baseExample = new BaseExample<DerivedClass>(new DerivedClass()); // 合法
        var baseExample2 = new BaseExample<BaseClass>(new DerivedClass());
        // var baseExample2 = new BaseExample<string>("Hello"); // 不合法
    }
}
```
##### 5. 接口约束
使用 `where T : IMyInterface` 限制类型参数必须实现指定的接口或者就是接口本身。
```csharp
public interface IMyInterface
{
    void MyMethod();
}
public class MyClass : IMyInterface
{
    public void MyMethod() { }
}
public class InterfaceExample<T> where T : IMyInterface
{
    public T Data { get; set; }

    public InterfaceExample(T data)
    {
        Data = data;
    }
}
class Program
{
    static void Main()
    {
        // 使用示例
        var interfaceExample = new InterfaceExample<MyClass>(new MyClass()); // 合法
        var interfaceExample2 = new InterfaceExample<IMyInterface>(new MyClass());
        // var interfaceExample2 = new InterfaceExample<string>("Hello"); // 不合法
    }
}
```
##### 6. 类型参数之间的约束
当你想要限制一个泛型类型参数可以是另一个泛型类型参数的类型时，可以使用 `where` 关键字结合多个约束。
**示例**：
```csharp
public class Pair<T1, T2> where T1 : class where T2 : T1 // T2 必须是 T1 的派生类
{
    public T1 First { get; set; }
    public T2 Second { get; set; }

    public Pair(T1 first, T2 second)
    {
        First = first;
        Second = second;
    }
}

// 使用示例
public class BaseClass { }
public class DerivedClass : BaseClass { }

var pair = new Pair<BaseClass, DerivedClass>(new BaseClass(), new DerivedClass()); // 合法
// var pair2 = new Pair<DerivedClass, BaseClass>(new DerivedClass(), new BaseClass()); // 不合法，BaseClass 不是 DerivedClass 的派生类
```
##### 7. 结合 `new()` 约束与其他约束
可以结合 `new()` 约束与其他约束。例如，假设你希望一个类同时支持无参构造函数和基类的约束。
**示例**：
```csharp
public class Example<T> where T : BaseClass, new() // T 必须是 BaseClass 的派生类，并且有无参构造函数
{
    public T CreateInstance()
    {
        return new T(); // 使用 new() 创建实例
    }
}

// 使用示例
public class DerivedClass : BaseClass
{
    public DerivedClass() { } // 无参构造函数
}

var example = new Example<DerivedClass>();
DerivedClass instance = example.CreateInstance(); // 合法
```
可以为一个类型参数指定多个约束，使用 `,` 分隔。
```csharp
public class MultiConstraintExample<T> 
    where T : BaseClass, IMyInterface, new() // 需要是 BaseClass 的派生类，实现 IMyInterface，并且有无参构造函数
{
    public T CreateInstance()
    {
        return new T(); // 使用 new() 约束
    }
}
```

##### 8. 多个泛型有约束
```csharp
using System;

// 定义一个泛型接口，表示实体必须有一个 ID
public interface IEntity<TId>
{
    TId Id { get; set; }
}

// 定义一个具体的实体类，实现 IEntity<int> 接口
public class User : IEntity<int>
{
    public int Id { get; set; }
    public string Name { get; set; }
}

// 定义泛型类 Repository，带有两个泛型参数 TModel 和 TId
public class Repository<TModel, TId>
    where TModel : class, IEntity<TId>, new() // TModel 必须是类，实现 IEntity<TId>，且有无参构造函数
    where TId : struct // TId 必须是值类型
{
    public TModel CreateNew()
    {
        // 可以创建 TModel 的实例，因为有 new() 约束
        return new TModel();
    }

    public TId GetId(TModel model)
    {
        // 可以访问 Id，因为 TModel 实现了 IEntity<TId>
        return model.Id;
    }
}

class Program
{
    static void Main(string[] args)
    {
        // 使用 Repository<User, int> 创建对象
        var userRepository = new Repository<User, int>();

        // 创建一个新的 User 实例
        User newUser = userRepository.CreateNew();
        newUser.Id = 1;
        newUser.Name = "Alice";

        // 获取 User 的 Id
        int userId = userRepository.GetId(newUser);

        Console.WriteLine($"User ID: {userId}, Name: {newUser.Name}");
    }
}
```


### 5. 泛型集合
C# 中有许多内置的泛型集合，如 `List<T>`、`Dictionary<TKey, TValue>` 和 `HashSet<T>`，这些集合类使得存储和操作不同类型的数据变得简单而安全。
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var names = new Dictionary<string, string>
{
    { "Alice", "Developer" },
    { "Bob", "Manager" }
};
```
### 6. 性能优势
使用泛型可以减少装箱和拆箱操作，这对于值类型（如 `int`, `float` 等）特别重要。泛型还可以提高性能，因为它们避免了不必要的类型转换。
### 总结
- **复用性**：泛型提供了一种创建类型安全和可复用的代码的方法。
- **类型安全**：编译器可以在编译时检查类型，从而减少运行时错误。
- **性能**：泛型可以提高性能，特别是在处理值类型时。
泛型是 C# 的一项重要特性，合理使用可以使代码更简洁、高效和可维护。

# 十一、 List
`List<T>` 是 C# 中常用的泛型集合类，它位于 `System.Collections.Generic` 命名空间中，提供了动态数组的功能，可以存储类型为 `T` 的对象。与数组不同，`List<T>` 可以根据需要自动调整大小，并提供了一些常用的方法来操作集合中的元素。
### `List<T>` 的基本用法
1. **创建 `List<T>`**
    可以创建一个空的 `List<T>`，也可以通过指定初始容量或集合来初始化。
```csharp
List<int> numbers = new List<int>(); // 创建一个空的 List<int>
List<string> names = new List<string> { "Alice", "Bob", "Charlie" }; // 使用集合初始化
```
2. **添加元素**
    使用 `Add` 方法可以将元素添加到 `List<T>` 的末尾，使用 `AddRange` 可以添加一个集合的元素。
```csharp
numbers.Add(10); // 添加一个元素
numbers.Add(20);
numbers.Insert(2, 30); // 在索引位置为2的地方插入值30，如果index>numbers.Count会报错：System.ArgumentOutOfRangeException

var moreNumbers = new List<int> { 30, 40, 50 };
numbers.AddRange(moreNumbers); // 添加多个元素 10 20 30 40 50 
```
3. **访问元素**
    可以使用索引访问 `List<T>` 中的元素。
```csharp
int firstNumber = numbers[0]; // 访问第一个元素
numbers[1] = 25; // 修改第二个元素的值
```
4. **删除元素**
    使用 `Remove` 方法可以删除指定的元素，`RemoveAt` 可以删除指定索引位置的元素，`Clear` 可以清空列表。
```csharp
numbers.Remove(20); // 删除值为 20 的元素
numbers.RemoveAt(0); // 删除索引为 0 的元素
numbers.Clear(); // 清空列表
```
5. **查找元素**
    可以使用 `Contains`、`IndexOf`、`LastIndexOf` 等方法查找元素。
```csharp
bool hasTen = numbers.Contains(10); // 检查列表中是否包含 10
int index = numbers.IndexOf(30); // 正向查找第一个 30 的索引，找不到返回-1
int index2 = numbers.LastIndexOf(30); // 反向查找第一个30的索引，找不到返回-1
```
6. **排序**
    可以使用 `Sort` 方法对列表进行排序，使用 `Reverse` 方法对列表进行反转。
    - 自定义排序要继承IComparable接口，重写CompareTo类
	```csharp
	numbers.Sort(); // 升序排序
	numbers.Reverse(); // 反转列表
	```
	`List<T>.Sort()` 方法有多个重载，允许根据不同的排序需求指定参数。以下是常见的参数选项：
	1. **`Sort()`**  
	    使用默认的比较器对列表中的元素进行升序排序。这要求元素类型实现了 `IComparable<T>` 接口。
	```csharp
	List<int> numbers = new List<int> { 5, 2, 8, 3 };
	numbers.Sort(); // 升序排序
	```
	2. **`Sort(Comparison<T> comparison)`**  
	    通过指定一个比较委托 `Comparison<T>`，根据自定义的比较规则进行排序。
	```csharp
	List<int> numbers = new List<int> { 5, 2, 8, 3 };
	numbers.Sort((a, b) => b.CompareTo(a)); // 降序排序
	```
	3. **`Sort(IComparer<T> comparer)`**  
	    使用指定的比较器 `IComparer<T>` 对列表中的元素进行排序。可以通过实现 `IComparer<T>` 接口来定义自定义排序逻辑。
		```csharp
		List<string> names = new List<string> { "Alice", "Charlie", "Bob" };
		names.Sort(StringComparer.OrdinalIgnoreCase); // 忽略大小写排序,按照Unicode 编码值排序，升序
		// 使用自定义比较器进行降序排序，忽略大小写
		names.Sort((a, b) => StringComparer.OrdinalIgnoreCase.Compare(b, a));
		```
	4. **`Sort(int index, int count, IComparer<T> comparer)`**  
		- 对列表的指定范围进行排序，从 `index` 开始，对接下来的 `count` 个元素进行排序。可以使用 `IComparer<T>` 来指定排序规则。
		```csharp
		List<int> numbers = new List<int> { 5, 2, 8, 3, 1 };
		numbers.Sort(1, 3, Comparer<int>.Default); // 对从索引 1 开始的 3 个元素排序
		```
	5. 自定义类排序
		- 自定义排序要继承IComparable接口，重写CompareTo()方法
		```csharp
		using System;
		using System.Collections.Generic;
		
		// 定义一个 Person 类，实现 IComparable<Person> 接口
		public class Person : IComparable<Person>
		{
		    public string Name { get; set; }
		    public int Age { get; set; }
		
		    // 重写 CompareTo 方法，根据年龄进行比较
		    public int CompareTo(Person other)
		    {
		        if (other == null) return 1;
		        return this.Age.CompareTo(other.Age);
		    }
		
		    // 重写 ToString 方法，方便输出
		    public override string ToString()
		    {
		        return $"{Name}, Age: {Age}";
		    }
		}
		
		class Program
		{
		    static void Main(string[] args)
		    {
		        // 创建一个 Person 类型的 List
		        List<Person> people = new List<Person>
		        {
		            new Person { Name = "Alice", Age = 30 },
		            new Person { Name = "Bob", Age = 25 },
		            new Person { Name = "Charlie", Age = 35 }
		        };
		
		        // 按年龄进行排序
		        people.Sort();
		
		        // 输出排序结果
		        Console.WriteLine("按年龄排序：");
		        foreach (var person in people)
		        {
		            Console.WriteLine(person);
		        }
		    }
		}
		```

	示例代码
	```csharp
	using System;
	using System.Collections.Generic;
	
	class Program
	{
		static void Main()
		{
			List<string> fruits = new List<string> { "Banana", "Apple", "Cherry", "Date" };
	
			// 使用默认排序（升序）
			fruits.Sort();
			Console.WriteLine("默认排序:");
			foreach (var fruit in fruits)
			{
				Console.WriteLine(fruit);
			}
	
			// 使用自定义比较器进行降序排序
			fruits.Sort((a, b) => b.CompareTo(a));
			Console.WriteLine("\n降序排序:");
			foreach (var fruit in fruits)
			{
				Console.WriteLine(fruit);
			}
	
			// 使用部分排序，指定范围
			List<int> numbers = new List<int> { 10, 20, 5, 15, 30 };
			numbers.Sort(1, 3, Comparer<int>.Default); // 对从索引 1 开始的 3 个元素排序
			Console.WriteLine("\n部分排序:");
			foreach (var number in numbers)
			{
				Console.WriteLine(number);
			}
		}
	}
	```


7. **遍历 `List<T>`**
    可以使用 `foreach` 或 `for` 循环遍历列表中的元素。
```csharp
foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```
8. List长度
```csharp
Console.WriteLine(numbers.Count);
```
### 常用方法总结
- `Add(T item)`: 将元素添加到列表的末尾。
- `AddRange(IEnumerable<T> collection)`: 将指定集合的元素添加到列表的末尾。
- `Insert(int index, T item)`: 在指定索引处插入元素。
- `Remove(T item)`: 删除列表中第一个匹配的元素。
- `RemoveAt(int index)`: 删除指定索引处的元素。
- `Contains(T item)`: 判断列表中是否包含指定元素。
- `IndexOf(T item)`: 查找指定元素的索引。
- `Sort()`: 对列表进行排序。
- `Clear()`: 清空列表。
### 示例代码
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建一个 List<int>
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

        // 添加元素
        numbers.Add(6);
        numbers.AddRange(new List<int> { 7, 8, 9 });

        // 访问元素
        Console.WriteLine($"第一个元素: {numbers[0]}");

        // 删除元素
        numbers.Remove(3); // 删除值为 3 的元素
        numbers.RemoveAt(0); // 删除第一个元素

        // 查找元素
        bool containsFive = numbers.Contains(5);
        Console.WriteLine($"包含 5: {containsFive}");

        // 排序并遍历
        numbers.Sort();
        foreach (var number in numbers)
        {
            Console.WriteLine(number);
        }
    }
}
```
`List<T>` 是一种非常灵活且常用的集合类型，适合需要动态调整大小的场景。
# 十二、  (a, b) => b.CompareTo(a)
表达式 `(a, b) => b.CompareTo(a)` 是一个使用 Lambda 表达式定义的比较委托，用于自定义排序。
### 含义
- `(a, b)` 是 Lambda 表达式的参数，表示要比较的两个元素。
- `b.CompareTo(a)` 是 Lambda 表达式的主体，它比较 `b` 和 `a` 的大小。
在 C# 中，`CompareTo` 方法返回一个整数值：
- 如果 `b.CompareTo(a)` 返回正数，表示 `b` 大于 `a`。
- 如果返回 0，表示 `b` 等于 `a`。
- 如果返回负数，表示 `b` 小于 `a`。
### 如何实现降序排序
在 `List<T>.Sort` 方法中，默认的排序顺序是升序（从小到大）。通过 `(a, b) => b.CompareTo(a)`，实际上是将两个元素的比较顺序进行了调换，即比较 `b` 和 `a`，而不是 `a` 和 `b`。这样可以实现降序排序（从大到小）。
### 示例代码
以下代码演示了使用自定义比较委托来对列表进行降序排序：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<int> numbers = new List<int> { 10, 3, 5, 2, 8 };

        // 使用自定义比较器进行降序排序
        numbers.Sort((a, b) => b.CompareTo(a));

        Console.WriteLine("降序排序后的列表:");
        foreach (var number in numbers)
        {
            Console.WriteLine(number);
        }
    }
}
```
运行结果：
```makefile
降序排序后的列表:
10
8
5
3
2
```
在这个例子中，`(a, b) => b.CompareTo(a)` 实现了降序排列，将较大的数排在前面。
# 十三、  Lambda 表达式
Lambda 表达式是一个匿名函数，用于创建委托或表达式树。它可以包含表达式或语句块，并且非常适合用来简化代码、实现委托、LINQ 查询和事件处理。
### 基本语法
Lambda 表达式的基本语法如下：
```csharp
(parameters) => expression //函数体只有一句，可以不用{}大括号
```
- `parameters`：输入参数，可以有一个或多个，用圆括号括起来。一个可以不用圆括号括起来
- $=>$：Lambda 操作符，分隔参数和表达式。
- `expression`：Lambda 表达式的主体，通常是一个单一的表达式。
如果 Lambda 表达式的主体包含多条语句，则需要使用大括号：
```csharp
(parameters) => { statements }
```
### 示例
#### 1. 简单示例
```csharp
Func<int, int> square = x => x * x; // Func是泛型委托，最后一个int表示返回值是int，第一个参数以及后的参数表示输入的参数
Console.WriteLine(square(5)); // 输出 25
```
在这个示例中，`x => x * x` 是一个 Lambda 表达式，它接受一个整数参数 `x`，并返回它的平方。
#### 2. 多参数的 Lambda 表达式
```csharp
Func<int, int, int> add = (a, b) => a + b;
Console.WriteLine(add(3, 4)); // 输出 7
```
此示例中，Lambda 表达式 `(a, b) => a + b` 接受两个整数参数并返回它们的和。
#### 3. 使用语句块的 Lambda 表达式
```csharp
Action<string> greet = name =>
{
    string greeting = "Hello, " + name;
    Console.WriteLine(greeting);
};
greet("Alice"); // 输出 Hello, Alice
```
此示例中的 Lambda 表达式使用了大括号，因为它包含多条语句。
### Lambda 表达式在 LINQ 中的使用
Lambda 表达式经常用于 LINQ 查询。例如：
```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0).ToList();

foreach (var number in evenNumbers)
{
    Console.WriteLine(number); // 输出 2 和 4
}
```
在这个示例中，`n => n % 2 == 0` 是一个 Lambda 表达式，用于过滤出偶数。
### 使用 Lambda 表达式的优势
1. **简洁性**：可以用更少的代码实现功能。
2. **灵活性**：方便地用于创建委托、事件处理程序或表达式树。
3. **可读性**：与 C# 语言的其他特性（如 LINQ）结合使用时，可以提高代码的可读性。

# 十四、 HashSet
`HashSet<T>` 是 C# 中的一种集合类型，它实现了集合的数学概念，特别是无序和唯一性。`HashSet<T>` 适合用于存储不重复的元素，并提供快速的查找、添加和删除操作。
### 特性
1. **无序性**：`HashSet<T>` 中的元素没有特定的顺序。
2. **唯一性**：集合中的每个元素都是唯一的，不能重复添加相同的元素。
3. **快速操作**：由于使用哈希表实现，`HashSet<T>` 提供 O(1) 的平均时间复杂度用于查找、添加和删除操作。
### 常用方法
- `Add(T item)`：向集合中添加元素。如果元素已存在，则不会添加，并返回 `false`。添加成功返回true。（如果用Console.WriteLine()输出变成了True）
- `Remove(T item)`：从集合中移除元素。如果元素不存在，则返回 `false`。
- `Contains(T item)`：检查集合中是否包含指定的元素。
- `Clear()`：移除集合中的所有元素。
- `Count`：获取集合中元素的数量。
- `UnionWith(IEnumerable<T> other)`：将当前集合与指定集合的并集合并。
- `IntersectWith(IEnumerable<T> other)`：将当前集合与指定集合的交集更新为当前集合。
- `ExceptWith(IEnumerable<T> other)`：从当前集合中移除与指定集合中相同的元素。
### 示例代码
以下是一个使用 `HashSet<T>` 的示例，展示了如何添加、查找和移除元素：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建一个 HashSet
        HashSet<int> numbers = new HashSet<int>();

        // 添加元素
        numbers.Add(1);
        numbers.Add(2);
        numbers.Add(3);
        numbers.Add(2); // 不会添加重复的 2

        Console.WriteLine("HashSet 中的元素:");
        foreach (var num in numbers)
        {
            Console.WriteLine(num); // 输出 1, 2, 3
        }

        // 检查是否包含某个元素
        if (numbers.Contains(2))
        {
            Console.WriteLine("集合中包含 2");
        }

        // 移除元素
        numbers.Remove(3);
        Console.WriteLine("移除 3 后的元素:");
        foreach (var num in numbers)
        {
            Console.WriteLine(num); // 输出 1, 2
        }

        // 清空集合
        numbers.Clear();
        Console.WriteLine($"集合是否为空: {numbers.Count == 0}");
    }
}
```
### 使用场景
- **去重**：存储一组唯一元素，避免重复。
- **快速查找**：快速判断某个元素是否存在于集合中。
- **集合运算**：执行并集、交集和差集等集合运算。

# 十五、  Dictionary
`Dictionary<TKey, TValue>` 是 C# 中的一种集合类型，用于存储键值对（key-value pairs）。它是一个哈希表的实现，提供了快速的查找、添加和删除操作，适合需要快速访问和唯一键的场景。
### 特性
1. **键唯一性**：每个键在字典中都是唯一的，不能重复。
2. **快速访问**：通过键可以快速访问对应的值，查找、添加和删除操作的平均时间复杂度为 O(1)。
3. **动态大小**：字典的大小可以动态调整，可以根据需要增加或减少存储的键值对。
### 常用方法
- `Add(TKey key, TValue value)`：向字典中添加新的键值对。如果键已存在，则抛出异常。
- `Remove(TKey key)`：移除指定键的键值对，并返回是否成功。
- `TryGetValue(TKey key, out TValue value)`：尝试获取指定键的值，返回一个布尔值指示是否成功。
- `ContainsKey(TKey key)`：检查字典中是否包含指定的键。
- `ContainsValue(TValue value)`：检查字典中是否包含指定的值。
- `Count`：获取字典中键值对的数量。
- `Clear()`：移除字典中的所有键值对。
- `Keys`：获取字典中所有的键。
- `Values`：获取字典中所有的值。
### 示例代码
以下是一个使用 `Dictionary<TKey, TValue>` 的示例，展示了如何添加、查找和移除元素：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建一个 Dictionary
        Dictionary<string, int> ages = new Dictionary<string, int>();

        // 添加键值对
        ages.Add("Alice", 30);
        ages.Add("Bob", 25);
        ages.Add("Charlie", 35);

        // 输出所有的键值对
        Console.WriteLine("姓名和年龄:");
        foreach (var kvp in ages)
        {
            Console.WriteLine($"姓名: {kvp.Key}, 年龄: {kvp.Value}");
        }

        // 检查是否包含某个键
        if (ages.ContainsKey("Bob"))
        {
            Console.WriteLine($"Bob 的年龄: {ages["Bob"]}");
        }

        // 检查是否包含某个值
        if (ages.ContainsValue(25))
        {
            Console.WriteLine($"存在值25");
        }

        // 尝试获取值
        if (ages.TryGetValue("Charlie", out int charlieAge))
        {
            Console.WriteLine($"Charlie 的年龄: {charlieAge}");
        }

        // 移除元素
        ages.Remove("Alice");
        Console.WriteLine("移除 Alice 后的键值对:");
        foreach (var kvp in ages)
        {
            Console.WriteLine($"姓名: {kvp.Key}, 年龄: {kvp.Value}");
        }

        // 键值对一起遍历
        // KeyValuePair<string, int>可以改为var
        foreach (KeyValuePair<string, int> kvp in ages)
        {
            Console.WriteLine($"姓名: {kvp.Key}, 年龄: {kvp.Value}");
        }

        // key不存在会报错 System.Collections.Generic.KeyNotFoundException
        Console.WriteLine($"修改前Bob 的年龄: {ages["Bob"]}");
        ages["Bob"] = 100; // key不存在的话，系统就会默认去创建一个
        Console.WriteLine($"修改后Bob 的年龄: {ages["Bob"]}");


        // 清空字典
        ages.Clear();
        Console.WriteLine($"字典是否为空: {ages.Count == 0}");
    }
}
```
### 使用场景
- **快速查找**：通过键快速获取对应的值，适用于存储和查找关联数据。
- **频率统计**：可用于统计某个元素出现的频率，如单词计数等。
- **配置和选项**：存储配置信息或选项，便于通过键快速访问。
`Dictionary<TKey, TValue>` 是处理需要快速查找、存储键值对时非常有用的工具，适合多种数据管理和操作场景。

# 十六、  Dictionary和Hashtable区别
`Dictionary<TKey, TValue>` 和 `Hashtable` 都是用于存储键值对的集合类型，但它们之间有几个重要的区别：
### 1. 类型安全
- `Dictionary<TKey, TValue>`：是泛型集合，提供类型安全，允许在编译时检查键和值的类型。例如，你可以定义 `Dictionary<string, int>`，这确保键是字符串，值是整数。
- `Hashtable`：是非泛型集合，可以存储任何类型的对象，键和值都是 `object` 类型。这意味着在运行时可能会遇到类型不匹配的问题，需要进行显式类型转换。
### 2. 性能
- `Dictionary<TKey, TValue>`：因为它是泛型的，所以在使用时不需要进行装箱和拆箱，通常具有更好的性能。
- `Hashtable`：由于其使用 `object` 类型，可能需要装箱和拆箱，性能上可能会稍差。
### 3. 访问方式
- `Dictionary<TKey, TValue>`：通过键访问值的方式更为简洁和直观，且支持更复杂的键类型（如自定义类型），并且提供方法如 `TryGetValue` 来尝试获取值而不抛出异常。
- `Hashtable`：使用 `ContainsKey` 和 `ContainsValue` 方法来检查键和值，且需要显式类型转换。
### 4. 默认行为
- `Dictionary<TKey, TValue>`：在键不存在时，访问一个不存在的键会抛出 `KeyNotFoundException`。
- `Hashtable`：如果访问一个不存在的键，则返回 `null`。
### 5. 可扩展性
- `Dictionary<TKey, TValue>`：支持并发的集合，例如 `ConcurrentDictionary<TKey, TValue>`，适用于多线程环境。
- `Hashtable`：是线程安全的，但使用的同步方法可能会导致性能问题。
### 示例代码
以下是两个集合的基本用法示例：
```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 使用 Dictionary
        Dictionary<string, int> dictionary = new Dictionary<string, int>
        {
            { "Alice", 30 },
            { "Bob", 25 }
        };

        // 访问值
        Console.WriteLine($"Alice's age: {dictionary["Alice"]}");

        // 使用 Hashtable
        Hashtable hashtable = new Hashtable
        {
            { "Charlie", 35 },
            { "Dave", 40 }
        };

        // 访问值
        Console.WriteLine($"Charlie's age: {hashtable["Charlie"]}");
    }
}
```
### 总结
在大多数情况下，建议使用 `Dictionary<TKey, TValue>` 而不是 `Hashtable`，因为其类型安全性、性能和功能更优越。`Hashtable` 主要是为了向后兼容，现代 C# 开发中更推荐使用泛型集合。

# 十七、 LinkedList
`LinkedList<T>` 是 C# 中的一种双向链表集合，提供了一种高效的方式来存储和操作数据。与数组或列表不同，链表中的元素（称为节点）是动态分配的，因此在插入和删除操作时具有更好的性能，特别是在大数据集的操作中。
### 特点
1. **双向链表**：`LinkedList<T>` 是一个双向链表，节点包含对前一个和后一个节点的引用，因此可以从任何节点轻松遍历链表。
2. **动态大小**：可以在运行时动态添加或删除节点，无需重新分配整个集合的大小。
3. **无序存储**：与数组和列表不同，`LinkedList<T>` 不保持元素的顺序，而是按插入顺序存储节点。
### 主要方法
以下是一些 `LinkedList<T>` 的常用方法：
- **添加元素**：
    - `AddFirst(T value)`：在链表开头添加一个新节点。
    - `AddLast(T value)`：在链表末尾添加一个新节点。
    - `AddBefore(LinkedListNode<T> node, T value)`：在指定节点之前插入一个新节点。
    - `AddAfter(LinkedListNode<T> node, T value)`：在指定节点之后插入一个新节点。
- **删除元素**：
    - `Remove(T value)`：删除第一个匹配的节点。
    - `RemoveFirst()`：删除链表的第一个节点。
    - `RemoveLast()`：删除链表的最后一个节点。
    - `Remove(LinkedListNode<T> node)`：删除指定的节点。
- **查找节点**：
    - `Find(T value)`：查找第一个匹配的节点，未找到时返回 `null`。
    - 没有提供找最后一个匹配的节点的方法，需要自己手动实现
- **遍历**：
    - 可以使用 `foreach` 循环遍历链表中的节点。
-  获取节点：
	- **`First`**: 获取链表的第一个节点。
	- **`Last`**: 获取链表的最后一个节点。
- 清空链表
	- **`Clear()`**: 清空链表中的所有节点。
- 获取元素数量
	- **`Count`**: 获取链表中节点的数量。
- 其他方法
	- **`Contains(T value)`**: 检查链表中是否包含指定的值。

### 示例代码
以下是一个使用 `LinkedList<T>` 的简单示例：
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // 创建 LinkedList
        LinkedList<string> linkedList = new LinkedList<string>();

        // 添加元素
        linkedList.AddLast("Alice");
        linkedList.AddLast("Bob");
        linkedList.AddFirst("Charlie");

        // 遍历链表
        Console.WriteLine("链表中的元素:");
        foreach (var name in linkedList)
        {
            Console.WriteLine(name);
        }

        // 删除元素
        linkedList.Remove("Bob");
        linkedList.RemoveFirst();

        // 遍历链表
        Console.WriteLine("\n删除 Bob 和第一个节点后的链表:");
        foreach (var name in linkedList)
        {
            Console.WriteLine(name);
        }

        // 从头到尾遍历
        LinkedListNode<string> nowNode = linkedList.First;
        while (nowNode != null)
        {
            Console.WriteLine(nowNode.Value);
            nowNode = nowNode.Next;
        }
        // 从尾到头遍历
        nowNode = linkedList.Last;
        while (nowNode != null)
        {
            Console.WriteLine(nowNode.Value);
            nowNode = nowNode.Previous;
        }

        // 查找元素
        var node = linkedList.Find("Alice");
        if (node != null)
        {
            Console.WriteLine($"\n找到节点: {node.Value}");
        }
        else
        {
            Console.WriteLine("\n未找到节点");
        }
        // 添加元素
        linkedList.AddAfter(node, "AddAfter");
        linkedList.AddBefore(node, "AddBefore");

        // 获取第一个和最后一个节点
        Console.WriteLine($"第一个节点: {linkedList.First.Value}");
        Console.WriteLine($"最后一个节点: {linkedList.Last.Value}");

        // 修改
        linkedList.First.Value = "修改了";
        linkedList.Last.Value = "修改了2";
        Console.WriteLine($"第一个节点: {linkedList.First.Value}");
        Console.WriteLine($"最后一个节点: {linkedList.Last.Value}");
        // 清空链表
        linkedList.Clear();
        Console.WriteLine($"链表清空后元素数量: {linkedList.Count}");
    }
}
```
### 输出
运行上述代码，输出将会是：
```makefile
链表中的元素:
Charlie
Alice
Bob

删除 Bob 和第一个节点后的链表:
Alice
Alice
Alice

找到节点: Alice
第一个节点: AddBefore
最后一个节点: AddAfter
第一个节点: 修改了
最后一个节点: 修改了2
链表清空后元素数量: 0

```
### 使用场景
- **频繁插入和删除**：`LinkedList<T>` 特别适合频繁在集合的开头和中间插入和删除元素的场景，例如实现某些数据结构（如队列、栈或图）的底层逻辑。
- **不需要随机访问**：由于不支持索引访问，`LinkedList<T>` 不适合需要频繁随机访问的场景。
### 总结
`LinkedList<T>` 提供了一个灵活且高效的方式来管理动态数据，尤其在插入和删除操作频繁的情况下表现出色。但在需要频繁随机访问的场景中，使用数组或列表可能更为合适。

# 十八、  LinkedList和List区别
### 1. 数据结构
- `List<T>` ： 
    - 基于数组实现，是一个动态数组。
    - 数据存储在连续的内存位置。
- `LinkedList<T>`：
    - 基于双向链表实现，由一系列节点组成。
    - 每个节点包含数据和指向前后节点的引用。
### 2. 存取性能
- `List<T>`：
    - 随机访问性能优越，索引访问（如 `list[0]`）的时间复杂度为 O(1)。
    - 插入和删除元素的时间复杂度为 O(n)，因为可能需要移动大量元素。
- `LinkedList<T>`：
    - 访问性能较差，索引访问的时间复杂度为 O(n)，因为必须从头部或尾部开始遍历。
    - 插入和删除元素的时间复杂度为 O(1)，在已知节点的情况下非常高效。
### 3. 内存使用
- `List<T>`：
    - 内存占用相对较少，因为只需要存储元素本身。
    - 当数组容量不足时，可能会进行扩展（复制到新数组），这会造成性能开销。
- `LinkedList<T>`：
    - 每个节点需要额外的内存来存储前后节点的引用，因此内存使用较高。
### 4. 功能特性
- `List<T>`：
    - 提供了索引、排序、查找等多种方法。
    - 适合需要频繁访问元素的场景。
- `LinkedList<T>`：
    - 支持双向遍历，能够在任意位置高效插入和删除节点。
    - 适合需要频繁添加和删除元素的场景，特别是在中间位置。
### 5. 适用场景
- `List<T>`：
    - 当数据量不频繁变化，且需要随机访问的情况，如实现栈、队列等功能。
- `LinkedList<T>`：
    - 当数据量变化频繁，且需要频繁插入和删除元素的情况，如实现双向队列、最近使用缓存等功能。
### 示例代码
```csharp
// List<T> 示例
List<int> numbersList = new List<int> { 1, 2, 3 };
numbersList.Add(4); // 添加元素
int secondNumber = numbersList[1]; // 随机访问

// LinkedList<T> 示例
LinkedList<int> numbersLinkedList = new LinkedList<int>(new[] { 1, 2, 3 });
numbersLinkedList.AddLast(4); // 添加元素
numbersLinkedList.Remove(2); // 删除元素

```
### 总结
- 选择 `List<T>` 还是 `LinkedList<T>` 取决于具体需求。如果需要快速随机访问，使用 `List<T>` 更加合适；如果需要频繁的插入和删除操作，尤其是在中间位置，使用 `LinkedList<T>` 更加高效。

# 十九、  委托
委托（Delegate）是 C# 中一种类型，代表对方法的引用，可以用于实现事件、回调和异步编程。委托允许将方法作为参数传递，并提供了一种灵活的方式来组织代码和实现解耦。
### 1. 委托的定义
委托定义了一种方法的签名（参数类型和返回类型），任何具有相同签名的方法都可以赋值给这个委托。可以理解为函数的容器，可以添加函数，不止添加一个
- 默认的访问修饰符为public
- 一般写在namespace中，class中也可以写
### 2. 委托的基本用法
#### 2.1 定义委托
使用 `delegate` 关键字定义一个委托类型。
```csharp
// 访问修饰符 delegate 返回值 委托名(参数列表)
// 定义了一个返回值为void，参数为string的委托
public delegate void MyDelegate(string message);
```
#### 2.2 实例化委托
将方法实例化为委托。
```csharp
public class Program
{
    // 访问修饰符 delegate 返回值 委托名(参数列表)
    // 定义了一个返回值为void，参数为string的委托
    public delegate void MyDelegate(string message);
    public static void MyMethod(string message)
    {
        Console.WriteLine(message);
    }

    public static void Main()
    {
	    // 但是并没有使用这个函数
        MyDelegate del = MyMethod; // 创建委托实例
        del("Hello, Delegates!"); // 调用委托
        MyDelegate del2 = new MyDelegate(MyMethod);// 创建委托实例
        del2("你好，委托");// 调用委托
        del2.Invoke("你好委托2");// 调用委托
    }
}
```
### 3. 多播委托
委托可以指向多个方法，这是所谓的多播委托。使用 `+=` 操作符可以将方法添加到委托中，使用 `-=` 操作符可以将方法从委托中移除。
- 按顺序依次执行添加，并以依次执行
- 减的时候先减去后添加的
- 为空能加也能减
- 多减不会报错，无非只是不处理
- 委托 = null,表示清空容器
```csharp
public delegate void MyDelegate(string message);

public class Program
{
    public static void MethodA(string message)
    {
        Console.WriteLine("MethodA: " + message);
    }

    public static void MethodB(string message)
    {
        Console.WriteLine("MethodB: " + message);
    }

    public static void Main()
    {
        MyDelegate del = MethodA; // 创建委托实例，需要先初始化才行或者是del = null;
        // 不可以直接用,比如
        // MyDelegate del;
        del += MethodB; // 添加方法

        del("Hello, Multicast Delegates!"); // 调用所有方法
    }
}
```
### 4. 带参数和返回值的委托
委托不仅可以没有返回值，也可以有返回值。
```csharp
public delegate int MathOperation(int x, int y);

public class Program
{
    public static int Add(int x, int y)
    {
        return x + y;
    }

    public static int Subtract(int x, int y)
    {
        return x - y;
    }

    public static void Main()
    {
        MathOperation operation;

        operation = Add;
        Console.WriteLine(operation(5, 3)); // 输出 8

        operation = Subtract;
        Console.WriteLine(operation(5, 3)); // 输出 2
    }
}
```
### 5. 匿名方法
C# 允许使用匿名方法来创建委托，省略方法名称。
```csharp
public delegate void MyDelegate(string message);

public class Program
{
    public static void Main()
    {
        MyDelegate del = delegate (string message)
        {
            Console.WriteLine("Anonymous: " + message);
        };

        del("Hello, Anonymous Methods!"); // 调用匿名方法
    }
}
```
### 6. Lambda 表达式
从 C# 3.0 开始，支持使用 Lambda 表达式简化委托的创建。
```csharp
public delegate void MyDelegate(string message);

public class Program
{
    public static void Main()
    {
        MyDelegate del = message => Console.WriteLine("Lambda: " + message);
        del("Hello, Lambda Expressions!"); // 调用 Lambda 表达式
    }
}
```

### 7. 委托作为函数参数
在这个例子中，我们定义一个委托类型 `MathOperation`，然后创建一个接受委托作为参数的方法 `PerformOperation`。
```csharp
using System;

public delegate int MathOperation(int x, int y); // 定义委托

public class Calculator
{
    public int PerformOperation(int x, int y, MathOperation operation) // 委托作为参数
    {
        return operation(x, y); // 调用委托
    }
}

public class Program
{
    public static int Add(int x, int y)
    {
        return x + y; // 加法
    }

    public static int Subtract(int x, int y)
    {
        return x - y; // 减法
    }

    public static void Main()
    {
        Calculator calculator = new Calculator();

        // 使用委托调用加法
        int resultAdd = calculator.PerformOperation(5, 3, Add);
        Console.WriteLine($"5 + 3 = {resultAdd}");

        // 使用委托调用减法
        int resultSubtract = calculator.PerformOperation(5, 3, Subtract);
        Console.WriteLine($"5 - 3 = {resultSubtract}");
    }
}
```
### 8. 委托作为类的成员
在这个例子中，我们定义一个类 `Notifier`，其中包含一个委托类型的成员 `Notify`。这个委托可以指向任何符合签名的方法。
```csharp
using System;

public delegate void NotifyDelegate(string message); // 定义委托

public class Notifier
{
    public NotifyDelegate Notify; // 委托作为类的成员

    public void SendNotification(string message)
    {
        Notify?.Invoke(message); // 调用委托
    }
}

public class Program
{
    public static void Main()
    {
        Notifier notifier = new Notifier();

        // 为委托分配方法
        notifier.Notify += message => Console.WriteLine("Notification: " + message);
        
        // 发送通知
        notifier.SendNotification("Hello, this is a notification!");
        
        // 可以添加更多的订阅者
        notifier.Notify += message => Console.WriteLine("Another listener: " + message);
        
        // 再次发送通知
        notifier.SendNotification("Another notification!");
    }
}
```
#### 总结
- **委托作为函数参数**：使得函数可以接受不同的操作，增加了灵活性。
- **委托作为类的成员**：允许类动态地调用不同的方法，方便实现事件处理或通知机制。
### 9. 泛型委托
泛型委托允许在定义委托时指定一个或多个类型参数，使得委托可以支持不同的数据类型。下面是泛型委托的基本定义及其使用的示例：
#### 定义泛型委托
首先，定义一个泛型委托，它接受两个类型参数，并返回一个结果类型。
```csharp
public delegate TOutput GenericDelegate<TInput1, TInput2, TOutput>(TInput1 input1, TInput2 input2);
```
这个泛型委托 `GenericDelegate` 有两个输入参数类型 `TInput1` 和 `TInput2`，返回一个结果类型 `TOutput`。
#### 使用泛型委托的示例
使用上面定义的泛型委托，可以实现不同类型的操作。例如，实现一个函数，将两个数字相加并返回结果。
```csharp
class Program
{
    static void Main(string[] args)
    {
        // 实例化泛型委托，传入两个int类型并返回int类型
        GenericDelegate<int, int, int> add = (x, y) => x + y;
        int sum = add(3, 4);
        Console.WriteLine($"Sum: {sum}"); // 输出：Sum: 7

        // 实例化泛型委托，传入两个字符串并返回字符串的拼接结果
        GenericDelegate<string, string, string> concatenate = (s1, s2) => s1 + s2;
        string result = concatenate("Hello, ", "World!");
        Console.WriteLine(result); // 输出：Hello, World!

        // 实例化泛型委托，传入int和double，返回字符串
        GenericDelegate<int, double, string> combine = (x, y) => $"Combined value: {x + y}";
        string combinedResult = combine(5, 7.5);
        Console.WriteLine(combinedResult); // 输出：Combined value: 12.5
    }
}
```
#### 解释代码
- `add`：这是一个泛型委托的实例，用于将两个整数相加并返回结果。它的类型参数为 `<int, int, int>`，表示输入参数和返回值的类型都是 `int`。
- `concatenate`：用于将两个字符串拼接在一起，并返回拼接结果。其类型参数为 `<string, string, string>`，表示输入参数和返回值的类型都是 `string`。
- `combine`：用于将一个整数和一个双精度浮点数相加，并返回结果字符串。其类型参数为 `<int, double, string>`，表示输入参数分别是 `int` 和 `double`，返回值为 `string`。

### 10. 使用场景
- **事件处理**：委托常用于事件处理机制。
- **回调**：可以用作异步方法的回调。
- **LINQ**：在 LINQ 中广泛使用委托（如 `Func<T>` 和 `Action<T>`）。

# 二十、 系统定义好的委托
C# 提供了一些常用的系统定义的委托，用于简化和通用地表达各种方法签名。这些委托类型定义在 `System` 命名空间中，最常用的系统定义委托包括：
- 委托的名字不能和函数一样，不然还是委托，会重复添加之前委托中所有的函数
1. **Action 系列**  
    `Action` 委托用于定义没有返回值的方法。可以有 0 到 16 个参数。
    - `Action`：没有参数的委托，表示不接受参数且无返回值的方法。
	```csharp
	Action action = () => Console.WriteLine("Hello, Action!");
	action(); // 输出：Hello, Action!
	```
    - `Action<T1>`：一个参数的委托。
	```csharp
	Action<string> greet = name => Console.WriteLine($"Hello, {name}!");
	greet("Alice"); // 输出：Hello, Alice!
	```
    - `Action<T1, T2>`：两个参数的委托。
	```csharp
	Action<int, int> add = (a, b) => Console.WriteLine(a + b);
	add(3, 4); // 输出：7
	```
2. **Func 系列**  
    `Func` 委托用于定义有返回值的方法。它的最后一个类型参数始终表示返回类型，前面的类型参数表示输入参数。
    - `Func<TResult>`：没有参数的方法，返回一个值。
	```csharp
	Func<int> getRandomNumber = () => new Random().Next(1, 100);
	int randomNumber = getRandomNumber();
	Console.WriteLine(randomNumber);
	```
    - `Func<T1, TResult>`：一个参数的方法，返回一个值。
	```csharp
	Func<int, int> square = x => x * x;
	int result = square(5); // 结果为 25
	Console.WriteLine(result); 
	```
    - `Func<T1, T2, TResult>`：两个参数的方法，返回一个值。
	```csharp
	Func<int, int, int> add = (a, b) => a + b;
	int sum = add(3, 4); // 结果为 7
	Console.WriteLine(sum);
	```
3. **Predicate 系列**  
    `Predicate<T>` 委托用于定义返回布尔值的方法，接受一个输入参数。
    - `Predicate<T>`：接受一个参数并返回 `bool` 类型的委托，用于确定某个条件是否满足。
	```csharp
	Predicate<int> isEven = x => x % 2 == 0;
	bool check = isEven(4); // 结果为 true
	Console.WriteLine(check);
	```
### 总结
- `Action` 系列用于不返回值的方法，可以有 0 到 16 个参数。
- `Func` 系列用于有返回值的方法，最后一个泛型参数表示返回值类型，其余参数表示输入参数。
- `Predicate<T>` 专门用于判断某个条件，返回 `bool` 值。

# 二十一、  事件
事件（Event）是 C# 中的一种特殊委托，用于通知订阅者在某个操作发生时执行相应的操作。事件通常用于实现发布/订阅模式，使得对象可以向外界通知状态变化或某些操作的发生。
- 事件是作为成员变量存在于类中
- 委托怎么用，事件怎么用
- 事件相对于委托的区别
	- 不能在类外部赋值
	- 不能在类外部调用
	- 可以在类外部添加或者删除函数
	- 事件只能作为成员存在于类和接口以及结构体中
	- 不能作为临时变量在函数中使用
### 1. 定义事件
事件的定义基于委托类型，事件本质上是一个委托类型的实例，但增加了一些限制，以确保事件只能在事件声明的类中调用。
```csharp
// 定义一个委托
public delegate void NotifyEventHandler(string message);

// 定义一个包含事件的类
public class Publisher
{
    // 声明事件
    // 访问修饰符 event 委托类型 事件名;
    public event NotifyEventHandler Notify;

    // 方法触发事件
    public void TriggerEvent(string message)
    {
        // 触发事件，通知所有订阅者
        Notify?.Invoke(message);
    }
}
```
### 2. 订阅事件
可以通过创建事件处理方法来订阅事件，当事件触发时，会调用这些处理方法。
```csharp
// 定义一个委托
public delegate void NotifyEventHandler(string message);

// 定义一个包含事件的类
public class Publisher
{
    // 声明事件
    // 访问修饰符 event 委托类型 事件名;
    public event NotifyEventHandler Notify;

    // 方法触发事件
    public void TriggerEvent(string message)
    {
        // 触发事件，通知所有订阅者
        Notify?.Invoke(message);
    }
}
public class Subscriber
{
    // 事件处理方法
    public void OnNotifyReceived(string message)
    {
        Console.WriteLine($"Subscriber received: {message}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // 订阅事件,添加了两次
        publisher.Notify += subscriber.OnNotifyReceived;
        publisher.Notify += subscriber.OnNotifyReceived;

        // 触发事件，会调用所有添加的事件
        publisher.TriggerEvent("Hello, Event!");

        // 取消订阅事件
        publisher.Notify -= subscriber.OnNotifyReceived;
    }
}
```
### 3. 代码解释
- **定义委托**：`NotifyEventHandler` 是一个接受 `string` 类型参数并返回 `void` 的委托类型。
- **定义事件**：`Notify` 是一个基于 `NotifyEventHandler` 委托类型的事件，声明在 `Publisher` 类中。
- **触发事件**：通过调用 `TriggerEvent` 方法来触发 `Notify` 事件，并使用 `?.Invoke` 语法安全地调用事件订阅者。
- **订阅和取消订阅事件**：`+=` 操作符用于订阅事件，`-=` 操作符用于取消订阅。
### 4. 多播事件
事件可以有多个订阅者，这些订阅者的方法会按顺序被调用。
```csharp
public class Subscriber2
{
    public void AnotherEventHandler(string message)
    {
        Console.WriteLine($"Subscriber2 received: {message}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber1 = new Subscriber();
        Subscriber2 subscriber2 = new Subscriber2();

        // 订阅多个事件处理器
        publisher.Notify += subscriber1.OnNotifyReceived;
        publisher.Notify += subscriber2.AnotherEventHandler;

        // 触发事件
        publisher.TriggerEvent("Hello, Multicast Event!");

        // 输出两个订阅者的消息
    }
}
```
### 5. 使用内置的 `EventHandler`
C# 提供了一个标准的 `EventHandler` 委托类型以及泛型的 `EventHandler<TEventArgs>`，它们用来更简洁地定义事件。
```csharp
public class CustomEventArgs : EventArgs
{
    public string Message { get; set; }
}

public class PublisherWithEventHandler
{
    // 使用 EventHandler<TEventArgs> 定义事件
    // public delegate void EventHandler(object? sender, EventArgs e);
    // public delegate void EventHandler<TEventArgs>(object? sender, TEventArgs e);
    public event EventHandler<CustomEventArgs> Notify;
    public void TriggerEvent(string message)
    {
        Notify?.Invoke(this, new CustomEventArgs { Message = message });
    }
}

class Program
{
    static void Main(string[] args)
    {
        PublisherWithEventHandler publisher = new PublisherWithEventHandler();
        publisher.Notify += (sender, e) =>
        {
            Console.WriteLine($"Received message: {e.Message}");
        };

        publisher.TriggerEvent("Using EventHandler!");
    }
}
```
- 以下是对代码的详细解释：
#### 1. `CustomEventArgs` 类
```csharp
public class CustomEventArgs : EventArgs
{
    public string Message { get; set; }
}
```
- `CustomEventArgs` 是一个自定义的事件参数类，继承自 `EventArgs`，这符合 .NET 事件处理的惯例。
- 它包含一个 `Message` 属性，用于传递事件的附加信息。
#### 2. `PublisherWithEventHandler` 类
```csharp
public class PublisherWithEventHandler
{
    // 使用 EventHandler<TEventArgs> 定义事件
    public event EventHandler<CustomEventArgs> Notify;

    public void TriggerEvent(string message)
    {
        Notify?.Invoke(this, new CustomEventArgs { Message = message });
    }
}
```

- `PublisherWithEventHandler` 是发布事件的类，包含一个名为 `Notify` 的事件，使用 `EventHandler<CustomEventArgs>` 泛型委托来定义。
    - `EventHandler<TEventArgs>` 是一个内置的事件委托，允许指定一个类型 `TEventArgs` 来传递事件数据。在这里，`TEventArgs` 被替换为自定义的 `CustomEventArgs` 类型。
- `TriggerEvent` 方法用于触发事件，并传递一个消息字符串作为事件数据。
    - `Notify?.Invoke(this, new CustomEventArgs { Message = message })` 这行代码用于触发事件，其中 `this` 表示事件的发送者，`new CustomEventArgs { Message = message }` 创建了一个新的 `CustomEventArgs` 对象，将 `Message` 属性设为传入的 `message` 参数。
#### 3. 订阅和触发事件 (`Main` 方法)
```csharp
class Program
{
    static void Main(string[] args)
    {
        PublisherWithEventHandler publisher = new PublisherWithEventHandler();

        // 订阅事件，使用 Lambda 表达式作为事件处理程序
        publisher.Notify += (sender, e) =>
        {
            Console.WriteLine($"Received message: {e.Message}");
        };

        // 触发事件
        publisher.TriggerEvent("Using EventHandler!");
    }
}
```

- 创建了一个 `PublisherWithEventHandler` 类的实例 `publisher`。
- 使用 `+=` 运算符订阅 `Notify` 事件，将一个 Lambda 表达式作为事件处理程序。
    - 该 Lambda 表达式接受两个参数：`sender`（事件的发送者）和 `e`（事件数据），这里的 `e` 类型为 `CustomEventArgs`。
    - 处理程序中打印了 `e.Message` 的值，从而输出事件传递的消息内容。
- 调用 `TriggerEvent` 方法触发事件，传递字符串 `"Using EventHandler!"`，这会导致事件处理程序被调用，并输出 `Received message: Using EventHandler!`。

#### 4. 代码的工作原理

1. 定义了一个自定义的事件参数类 `CustomEventArgs`，包含一个 `Message` 属性。
2. `PublisherWithEventHandler` 类通过 `EventHandler<CustomEventArgs>` 定义了一个事件 `Notify`，并提供 `TriggerEvent` 方法来触发事件。
3. 在 `Main` 方法中，创建发布者对象并订阅 `Notify` 事件。
4. 当事件被触发时，订阅的处理程序会被调用，并接收到传递的消息。
#### 代码总结
- **事件**用于通知订阅者某个动作发生。
- **事件的定义基于委托类型**，可以有多个订阅者。
- **事件触发时**，会通知所有已订阅的方法。
- **`EventHandler` 和 `EventHandler<TEventArgs>`** 是内置的委托类型，用于更方便地定义事件。
事件机制非常适合处理用户输入、状态变化以及异步通知等场景。

# 二十二、  EventArgs
`EventArgs` 是 .NET 中定义的一个基类，用于传递事件相关的信息。它位于 `System` 命名空间中，并且是所有事件参数类的基类。`EventArgs` 的作用是为事件处理程序提供一个基础类型，可以在事件被触发时传递事件的相关数据。
- 也即是说所有的事件都可以继承EventArgs，类似object
### 1. `EventArgs` 的定义
在 .NET 框架中，`EventArgs` 定义如下：
```csharp
public class EventArgs
{
    public static readonly EventArgs Empty;
}
```
- `EventArgs` 是一个空类，不包含任何属性或方法。它的主要作用是作为事件参数的基类，或在事件不需要传递额外数据时使用。
- `EventArgs.Empty` 是一个静态只读字段，表示没有数据的 `EventArgs` 实例，可以用于不需要传递数据的事件。
- `readonly` 是 C# 中的一个关键字，用于在字段声明中指明字段的值在对象的整个生命周期中只能被赋值一次。`readonly` 关键字可以确保字段一旦被初始化，就不会再被修改，这种特性适用于对数据的安全性和稳定性有要求的场景。
	- `readonly` 字段可以在声明时初始化，也可以在构造函数中赋值。
	- 一旦在构造函数或字段声明中赋值后，`readonly` 字段的值就不能再被更改。
	- `readonly` 字段的值在运行时确定，而不是编译时（与 `const` 相比）。
	- 在静态字段中使用 `readonly`，可以在静态构造函数中赋值。
	- 示例代码
		- 示例1：声明时初始化
		```csharp
		public class Circle
		{
		    // readonly 字段，在声明时赋值
		    public readonly double Pi = 3.14159;
		
		    public void PrintPi()
		    {
		        Console.WriteLine($"Pi: {Pi}");
		    }
		}
		
		class Program
		{
		    static void Main()
		    {
		        Circle circle = new Circle();
		        circle.PrintPi(); // 输出：Pi: 3.14159
		    }
		}
		```
		在这个例子中，`Pi` 是一个 `readonly` 字段，在声明时就被赋值为 `3.14159`，并且在对象生命周期内不会改变。
		示例2：在构造函数中赋值
		```csharp
		public class Person
		{
		    // readonly 字段，未在声明时初始化
		    public readonly string Name;
		
		    // 构造函数中赋值
		    public Person(string name)
		    {
		        Name = name;
		    }
		
		    public void PrintName()
		    {
		        Console.WriteLine($"Name: {Name}");
		    }
		}
		
		class Program
		{
		    static void Main()
		    {
		        Person person = new Person("Alice");
		        person.PrintName(); // 输出：Name: Alice
		
		        // person.Name = "Bob"; // 编译错误：无法为 readonly 字段赋值
		    }
		}
		```
		在这个例子中，`Name` 是一个 `readonly` 字段，通过构造函数进行初始化，一旦对象被创建后，该字段的值就不能被更改。
		- 示例3：静态 `readonly` 字段
		```csharp
		public class Config
		{
		    // 静态 readonly 字段
		    public static readonly string AppName;
		
		    // 静态构造函数中初始化
		    static Config()
		    {
		        AppName = "MyApplication";
		    }
		
		    public static void PrintAppName()
		    {
		        Console.WriteLine($"App Name: {AppName}");
		    }
		}
		
		class Program
		{
		    static void Main()
		    {
		        Config.PrintAppName(); // 输出：App Name: MyApplication
		    }
		}
		```
		在这个示例中，`AppName` 是一个静态 `readonly` 字段，在静态构造函数中初始化，这样在整个程序生命周期中该字段的值都是固定的。
### 2. 使用 `EventArgs` 的场景
`EventArgs` 通常在定义事件时作为参数类型。如果一个事件不需要额外的参数，就可以使用 `EventArgs`。如果需要传递额外的信息，可以创建一个自定义的类，继承自 `EventArgs`。
#### 示例1：不传递任何数据的事件
```csharp
public class Publisher
{
    // 定义一个事件，使用 EventArgs 作为参数
    public event EventHandler SimpleEvent;

    public void TriggerEvent()
    {
        // 触发事件时使用 EventArgs.Empty 传递
        SimpleEvent?.Invoke(this, EventArgs.Empty);
    }
}

class Program
{
    static void Main()
    {
        Publisher publisher = new Publisher();

        // 订阅事件
        publisher.SimpleEvent += (sender, e) =>
        {
            Console.WriteLine("SimpleEvent triggered");
        };

        // 触发事件
        publisher.TriggerEvent();
    }
}
```
- 在这个例子中，`SimpleEvent` 使用 `EventArgs` 作为事件参数，因为不需要传递额外的信息。
- 触发事件时使用 `EventArgs.Empty`，表示不需要任何附加数据。
- `EventHandler` 是 C# 中的一个委托类型，用于定义事件处理方法的标准签名。它是事件机制的核心，用来指定事件处理方法的参数格式。`EventHandler` 提供了一种标准的方式来处理事件，使事件的声明和使用变得简单和一致。
- `EventHandler` 是一个预定义的委托类型，表示事件处理方法不需要自定义事件数据时的标准签名。
- 它的标准方法签名是：`void EventHandler(object sender, EventArgs e)`。
    - `sender`：事件的发送者，即引发事件的对象。
    - `e`：事件数据的对象，通常是 `EventArgs` 或其派生类的实例。
### 3. 自定义 `EventArgs` 类
当需要传递附加的事件数据时，可以创建一个继承自 `EventArgs` 的自定义类。
#### 示例2：传递额外数据的事件
```csharp
// 自定义的 EventArgs 类，继承自 EventArgs
public class CustomEventArgs : EventArgs
{
    public string Message { get; set; }
}

public class PublisherWithEventArgs
{
    // 使用自定义的 EventArgs 类
    public event EventHandler<CustomEventArgs> CustomEvent;

    public void TriggerCustomEvent(string message)
    {
        // 触发事件，传递 CustomEventArgs 实例
        CustomEvent?.Invoke(this, new CustomEventArgs { Message = message });
        //new CustomEventArgs { Message = message } 这段代码做了以下工作：
        //new CustomEventArgs：创建了一个新的 CustomEventArgs 类型的实例。
        //{ Message = message }：使用对象初始化器对 Message 属性进行赋值，将传入的 message 值赋给该属性。
    }
}

class Program
{
    static void Main()
    {
        PublisherWithEventArgs publisher = new PublisherWithEventArgs();

        // 订阅事件，处理传递的自定义事件参数
        // 这边的e是自定义的一个EventArgs的事件，可以使用里面的Message参数
        publisher.CustomEvent += (sender, e) =>
        {
            Console.WriteLine($"Custom event triggered with message: {e.Message}");
        };

        // 触发事件，传递消息
        publisher.TriggerCustomEvent("Hello, EventArgs!");
    }
}
```
- 在这个示例中，`CustomEventArgs` 继承自 `EventArgs`，增加了一个 `Message` 属性。
- 当触发事件时，将 `CustomEventArgs` 实例作为事件参数传递，以便处理程序使用。
### 4. `EventArgs` 的作用
- `EventArgs` 提供了一种标准化的方式来定义事件参数。
- 通过继承 `EventArgs`，可以创建自定义事件参数类，添加额外的数据属性，以适应具体的事件需求。
- `EventHandler<TEventArgs>` 泛型委托与自定义的 `EventArgs` 类结合使用，使得事件处理程序可以获得类型安全的事件数据。
总结来说，`EventArgs` 是事件处理系统的一个基础元素，为事件参数提供了一个统一的基类，同时支持自定义扩展，满足不同事件的数据需求。

# 二十三、 对象初始化器
对象初始化器是一种简洁的语法，用于在创建对象的同时设置其公共属性或字段的值。它允许在对象实例化的过程中，使用花括号 `{}` 来指定属性或字段的初始值，而不必通过构造函数逐个进行赋值。
### 使用对象初始化器的语法
基本语法如下：
```csharp
var objectName = new ClassName
{
    Property1 = value1,
    Property2 = value2,
    // ...其他属性
};
```
这种方式在创建对象的同时，通过初始化列表的形式赋值给对象的属性或字段。
### 示例
#### 1. 类的定义
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```
#### 2. 使用对象初始化器来创建对象
```csharp
var person = new Person
{
    Name = "Alice",
    Age = 25
};
```
这段代码做了以下操作：
- 使用 `new Person` 创建了一个新的 `Person` 类型的对象。
- 使用 `{ Name = "Alice", Age = 25 }` 初始化了 `Name` 和 `Age` 两个属性。
这种写法等价于以下代码，但更加简洁：
```csharp
var person = new Person();
person.Name = "Alice";
person.Age = 25;
```
### 对象初始化器的优点
1. **简洁**：减少了代码量，使代码更加简洁易读。
2. **易于维护**：在对象创建时可以同时初始化多个属性的值，避免后续的逐个赋值。
3. **灵活性**：可以用于各种引用类型对象的初始化，包括类、结构、集合等。
### 使用对象初始化器与集合初始化器结合
对象初始化器也可以与集合初始化器结合使用，方便地初始化集合中的对象。
#### 示例
```csharp
var people = new List<Person>
{
    new Person { Name = "Alice", Age = 25 },
    new Person { Name = "Bob", Age = 30 }
};
```
在这个例子中，`people` 是一个 `List<Person>` 类型的集合，通过集合初始化器直接初始化了多个 `Person` 对象。
### 适用范围
对象初始化器不仅适用于类的属性初始化，也可以用于匿名类型、结构、集合等场景。


# 二十四、  委托和事件的区别
委托和事件是 C# 中用于实现事件驱动编程的重要概念。虽然它们有许多相似之处，但它们之间也存在一些关键的区别。下面是委托和事件的定义、用途以及它们之间的区别。
### 1. 委托 (Delegate)
**定义**：委托是一种类型安全的函数指针，可以引用具有特定参数列表和返回类型的方法。委托可以用来定义回调方法。
**用法**：
- 委托可以直接调用。
- 可以通过委托实例添加或移除方法。
- 委托可以被多播，即一个委托可以指向多个方法。
**示例**：
```csharp
public delegate void Notify(string message);

public class Program
{
    public static void Main()
    {
        Notify notifyDelegate = ShowMessage;
        notifyDelegate("Hello, Delegates!"); // 调用委托
    }

    public static void ShowMessage(string message)
    {
        Console.WriteLine(message);
    }
}
```
### 2. 事件 (Event)
**定义**：事件是基于委托的，专门用于处理事件通知的机制。它提供了一种在对象之间发布和订阅消息的方式。
**用法**：
- 事件通常是由类定义的，使用委托作为事件的类型。
- 事件通常只能被类的内部代码触发，外部代码只能订阅或取消订阅。
- 使用事件可以防止外部代码直接调用事件的处理方法，从而保护对象的状态。
**示例**：
```csharp
public delegate void Notify(string message);


public class Publisher
{
    public event Notify NotifyEvent; // 声明事件
    public Notify NotifyDelegate; // 声明事件

    public void TriggerEvent(string message)
    {
        NotifyEvent?.Invoke(message); // 触发事件
    }
}

public class Program
{
    public static void Main()
    {

        Publisher publisher = new Publisher();

        publisher.NotifyEvent += ShowMessage; // 订阅事件
        publisher.NotifyDelegate += ShowMessage; // 订阅事件
        // publisher.NotifyEvent("111"); // 报错,不可以在类外调用
        publisher.NotifyDelegate("111");
        publisher.TriggerEvent("Hello, Events!"); // 触发事件
    }

    public static void ShowMessage(string message)
    {
        Console.WriteLine(message);
    }
}
```
### 3. 委托和事件的区别

| 特征     | 委托                  | 事件                                                  |
| ------ | ------------------- | --------------------------------------------------- |
| 定义     | 委托是一种类型，可以直接调用。     | 事件是基于委托的，专门用于处理事件通知的机制。                             |
| 访问修饰符  | 委托可以具有公共或私有修饰符。     | 事件通常使用 `public` 修饰符，但可以通过 `add` 和 `remove` 访问器限制访问。 |
| 触发     | 可以直接调用委托的方法。        | 事件只能由其定义的类内部触发。                                     |
| 订阅和取消  | 委托没有内置的订阅和取消机制。     | 事件支持订阅和取消机制，使用 `+=` 和 `-=` 操作符。                     |
| 事件处理方法 | 委托可以直接存储任何匹配其签名的方法。 | 事件处理方法不能被外部代码直接调用。                                  |
| 多播支持   | 委托可以多播（即引用多个方法）。    | 事件通常是多播的，但它们的处理方法在内部进行管理。                           |
#### 事件通常使用 `public` 修饰符，但可以通过 `add` 和 `remove` 访问器限制访问。
在 C# 中，事件可以通过 `public` 修饰符声明，但其访问级别可以通过 `add` 和 `remove` 访问器进行限制。这种机制提供了更好的封装性，允许开发者控制谁可以订阅或取消订阅事件。其实就是在添加或者删除事件的时候提前进行处理。
#### 访问器的用法
- **`add` 访问器**：用于添加事件处理程序。
- **`remove` 访问器**：用于移除事件处理程序。
#### 示例
以下是一个使用 `add` 和 `remove` 访问器限制访问的事件示例：
```csharp
using System;

public class Publisher
{
    // 定义一个私有字段来存储事件处理程序
    private event EventHandler _notify;

    // 公开的事件，使用 add 和 remove 访问器
    public event EventHandler Notify
    {

        // 在 C# 中，value 是一个关键字，用于 add 和 remove 访问器中的参数，
        // 它代表即将被添加或移除的事件处理程序（即方法）。
        // 当你使用 += 或 -= 操作符来订阅或取消订阅事件时，value 充当被传递的委托（delegate）。
        add
        {
            Console.WriteLine("Subscribing to the event.");
            _notify += value; // 订阅事件 // 这里的 value 是传入的事件处理程序
        }
        remove
        {
            Console.WriteLine("Unsubscribing from the event.");
            _notify -= value; // 取消订阅事件 // 这里的 value 是要取消订阅的事件处理程序
        }
    }

    public void TriggerEvent()
    {
        // 触发事件
        _notify?.Invoke(this, EventArgs.Empty);
    }
}

public class Subscriber
{
    public void Subscribe(Publisher publisher)
    {
        // 订阅事件,给订阅者订阅，让发布者发布消息的时候给订阅者，每次都调用订阅者里面的方法
        publisher.Notify += OnNotify;
    }

    public void Unsubscribe(Publisher publisher)
    {
        publisher.Notify -= OnNotify; // 取消订阅事件
    }

    private void OnNotify(object sender, EventArgs e)
    {
        Console.WriteLine("Event received!");
    }
}

class Program
{
    static void Main()
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // 订阅事件
        subscriber.Subscribe(publisher);

        // 触发事件
        publisher.TriggerEvent();

        // 取消订阅事件
        subscriber.Unsubscribe(publisher);

        // 再次触发事件（不会输出任何内容，因为已经取消订阅）
        publisher.TriggerEvent();
    }
}
```
#### 代码解析
1. **事件定义**：
    - `private event EventHandler _notify;`：这是一个私有事件字段，用于存储实际的事件处理程序。
    - `public event EventHandler Notify`：公开的事件，使用 `add` 和 `remove` 访问器。
2. **`add` 访问器**：
    - 在订阅事件时，输出 "Subscribing to the event."，并将新的事件处理程序添加到 `_notify` 字段。
3. **`remove` 访问器**：
    - 在取消订阅事件时，输出 "Unsubscribing from the event."，并将事件处理程序从 `_notify` 字段中移除。
4. **触发事件**：
    - 使用 `_notify?.Invoke(this, EventArgs.Empty);` 触发事件，调用所有订阅的事件处理程序。
5. **Subscriber 类**：
    - 提供 `Subscribe` 和 `Unsubscribe` 方法来管理事件的订阅。

### 总结
- **委托** 是一种函数指针，允许方法作为参数传递，并可以被直接调用。
- **事件** 是基于委托的，用于在对象之间发布和订阅消息，通常用于实现事件驱动编程。事件提供了更多的封装和安全性，防止外部代码直接调用事件处理方法。
使用事件的主要原因是它们能够更好地支持事件驱动的设计模式，并提供了更好的封装和可维护性。


# 二十五、  Notify?.Invoke(message); 什么意思
在 C# 中，`Notify?.Invoke(message);` 是一种使用空条件运算符（null-conditional operator）来安全调用委托的方法。这个表达式可以拆分为两部分进行理解：
1. **空条件运算符 (`?.`)** ：这是一个安全访问运算符，允许你在调用成员（如方法、属性）之前检查对象是否为 `null`。如果对象为 `null`，则整个表达式返回 `null`，而不会引发 `NullReferenceException`。
2. ?[ ] 只判断整个数组是否为空,不判断这个位置,所有数组不为空而调用位置太长时还是会报错
3. **`Invoke(message)`** ：这是用于调用委托的方法。通常，调用委托可以直接使用 `Notify(message)`，但是在这里我们使用 `Invoke` 也是有效的。
### 整体理解
- `Notify?.Invoke(message);` 意味着：
    - 如果 `Notify` 委托不为 `null`，则调用 `Notify` 并传递 `message` 参数。
    - 如果 `Notify` 为 `null`，则不执行任何操作。
### 例子
考虑以下代码：
```csharp
public delegate void NotifyDelegate(string message); // 委托定义

public class Notifier
{
    public NotifyDelegate Notify; // 委托作为类的成员

    public void SendNotification(string message)
    {
        Notify?.Invoke(message); // 安全调用委托
    }
}
```
- 在 `SendNotification` 方法中，使用 `Notify?.Invoke(message)` 确保只有在 `Notify` 委托不为 `null` 时才会调用它。这避免了在 `Notify` 为 `null` 时尝试调用会导致程序崩溃。
### 结论
这种写法可以帮助避免潜在的 `NullReferenceException`，使代码更健壮。在处理委托和事件时，常常会使用这种模式，因为委托可以在运行时动态分配或取消，从而可能会为 `null`。

# 参考
[MS-Learning](https://learn.microsoft.com/zh-cn/)