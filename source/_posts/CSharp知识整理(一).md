---
title: CSharp知识整理(一)
math: true
---
# 1.基本数据类型


| 数据类型 | 占用字节数 | 数据范围                                                | 描述                      | 示例                                   |
| -------- | ---------- | ------------------------------------------------------- | ------------------------- | -------------------------------------- |
| bool     | 1          | true 或 false                                           | 布尔类型，表示真或假      | bool isActive = true;                  |
| byte     | 1          | 0 到 255                                                | 无符号 8 位整数           | byte age = 25;                         |
| sbyte    | 1          | -128 到 127                                             | 有符号 8 位整数           | sbyte temperature = -10;               |
| char     | 2          | '\u0000' (0) 到 '\uffff' (65535)                        | Unicode 字符（16 位）     | char letter = 'A';                     |
| short    | 2          | -32,768 到 32,767                                       | 有符号 16 位整数          | short distance = -32000;               |
| ushort   | 2          | 0 到 65,535                                             | 无符号 16 位整数          | ushort width = 60000;                  |
| int      | 4          | -2,147,483,648 到 2,147,483,647                         | 有符号 32 位整数          | int score = 100;                       |
| uint     | 4          | 0 到 4,294,967,295                                      | 无符号 32 位整数          | uint population = 3000000000;          |
| long     | 8          | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 | 有符号 64 位整数          | long distanceToSun = 150000000000L;    |
| ulong    | 8          | 0 到 18,446,744,073,709,551,615                         | 无符号 64 位整数          | ulong starsInGalaxy = 1000000000000UL; |
| float    | 4          | ±1.5 × 10^−45 到 ±3.4 × 10^38                      | 单精度浮点数（32 位）     | float price = 19.99F;                  |
| double   | 8          | ±5.0 × 10^−324 到 ±1.7 × 10^308                    | 双精度浮点数（64 位）     | double pi = 3.14159265359;             |
| decimal  | 16         | ±1.0 × 10^−28 到 ±7.9228 × 10^28                   | 高精度小数（128 位）      | decimal balance = 1000.75M;            |
| string   | 不固定     | 根据字符数量变化                                        | 一组 Unicode 字符（文本） | string name = "John Doe";              |

## char是2字节

在 C# 中，`char` 数据类型使用 2 字节（16 位）是因为它表示一个 Unicode 字符。Unicode 字符集比传统的 ASCII 字符集要大得多，能够表示全球范围内的各种字符和符号。为了支持这一广泛的字符集，C# 采用了 UTF-16 编码格式，其中每个 `char` 类型的字符占用 2 字节。
- 但是如果是某些生僻字，需要先将这两个 char 放到一个 char[] 数组中，然后通过 new string(char[]) 创建一个 string 来输出。
	- `char[] surrogatePair = new char[] { '\uD840', '\uDC8E' }; // "𠜎" 的代理对表示`
	- `string str = new string(surrogatePair);` // 将代理对转化为字符串
	- `Console.WriteLine(str);` // 输出：𠜎

```csharp
using System.Runtime.InteropServices;

class Program
{

    static void Main(string[] args)
    {

        char c = '中';
        Console.WriteLine(c);
        Console.WriteLine($"一个char字符占用字节数:{sizeof(char)}"); // 1
        // 对于更复杂的类型（包括结构和类实例），可以使用
        // System.Runtime.InteropServices.Marshal.SizeOf方法
        int size = Marshal.SizeOf(typeof(char));
        Console.WriteLine($"一个char字符占用字节数:{size}"); // 2
    }
}
```
- 这里控制台还是无法显示，先不管了
```csharp
using System;

class Program
{
    static void Main()
    {
        // 设置控制台输出编码为 UTF-8
        Console.OutputEncoding = System.Text.Encoding.UTF8;

        // 输出包含生僻字的字符串
        string str = "这是一个生僻字: 龘𠜎";
        //"这"（1个字符）
        //"是"（1个字符）
        //"一"（1个字符）
        //"个"（1个字符）
        //"生"（1个字符）
        //"僻"（1个字符）
        //"字"（1个字符）
        //":"（1个字符）
        //"龘"（2个字符，代理对）
        //"𠜎"（2个字符，代理对）
        Console.WriteLine(str.Length);  // 12
        Console.WriteLine(str);  // 在控制台显示，生僻字"𠜎"无法显示

        // 或者通过 Debug 输出
        System.Diagnostics.Debug.WriteLine(str); // 通过 Visual Studio 输出窗口
    }
}
```

[![pAsq64U.png](https://s21.ax1x.com/2024/11/06/pAsq64U.png)](https://imgse.com/i/pAsq64U)

|           | **x64**                        | **x86**                        |
| --------- | ------------------------------ | ------------------------------ |
|           | Marshal.                       | Marshal.                       |
| Primitive | SizeOf<T>()          sizeof(T) | SizeOf<T>()          sizeof(T) |
| Boolean   | 4     <->    1                 | 4    <->    1                  |
| Byte      | 1            1                 | 1            1                 |
| SByte     | 1            1                 | 1           1                  |
| Int16     | 2            2                 | 2           2                  |
| UInt16    | 2            2                 | 2            2                 |
| Int32     | 4            4                 | 4            4                 |
| UInt32    | 4            4                 | 4            4                 |
| Int64     | 8            8                 | 8            8                 |
| UInt64    | 8            8                 | 8            8                 |
| IntPtr    | 8            8                 | 4           4                  |
| UIntPtr   | 8            8                 | 4           4                  |
| Char      | 1     <->    2                 | 1     <->    2                 |
| Double    | 8            8                 | 8            8                 |
| Single    | 4            4                 | 4            4                 |

## 使用char变量，Marshal.SizeOf()和sizeof()不一样

在C#中，`sizeof`和`Marshal.SizeOf`在处理`char`类型时确实会有差异。

1. `sizeof(char)`：
   `sizeof(char)`会返回`2`，因为在.NET中，`char`类型使用UTF-16编码，每个字符占用2个字节。

   ```csharp
   int sizeOfChar = sizeof(char); // 结果是2
   ```
2. `Marshal.SizeOf(typeof(char))`
   `Marshal.SizeOf`在处理`char`类型时会返回`1`，因为它假定在非托管代码中`char`的大小是1个字节，这种情况通常出现在Interop或P/Invoke场景中。因此它会计算为1字节，这也是.NET和非托管环境的一个差异。

   ```csharp
   int marshalSizeOfChar = Marshal.SizeOf(typeof(char)); // 结果是1
   ```

## 非托管代码

在C#和.NET中，**非托管代码（Unmanaged Code）**指的是不受.NET运行时（CLR）管理的代码。这通常包括用C、C++等语言编写的代码或直接与操作系统交互的代码。非托管代码不会自动享受.NET提供的垃圾回收、内存管理和类型安全等功能，开发者需要手动处理内存管理。

### 主要区别

1. **托管代码（Managed Code）**：
   - `由.NET运行时管理`，运行时负责内存分配、垃圾回收和异常处理。
   - 大部分C#代码属于托管代码。
	   - **P/Invoke（平台调用）**: 通过 P/Invoke，C# 代码可以调用 Windows API 或其他操作系统提供的本地库（例如 `.dll` 文件）。这些调用直接与操作系统交互，属于非托管代码。
	    例如，使用 `DllImport` 特性调用本地 C/C++ 库：
		```csharp
		using System.Runtime.InteropServices;
		
		class Program
		{
		    [DllImport("kernel32.dll")]
		    public static extern void Beep(int frequency, int duration);
		
		    static void Main()
		    {
		        Beep(800, 500);  // 调用 Windows API 发出声音
		    }
		}
		```
	- **与 C++ 编写的本地代码交互**: 通过 **C++/CLI** 或其他方法，C# 可以调用直接通过 C++ 编写的本地代码，这些代码通常需要手动进行内存管理，并且不由 CLR 管理。例如，使用 C++ 编写的高性能计算或直接与硬件交互的代码。
	- **Unsafe 代码**: C# 允许通过 **`unsafe`** 关键字编写非托管代码。这些代码不受 CLR 的管理，可以直接操作内存、指针等。这些操作通常用于需要高性能的场景，或者与硬件、操作系统的直接交互。
	    例如，使用指针和内存操作：
		```csharp
		unsafe
		{
		    int* ptr = &myVar;
		    *ptr = 10;
		}
		```
	- **COM Interop**: COM（组件对象模型）是微软的一个底层技术，用于不同编程语言之间的交互。通过 COM Interop，C# 可以与使用 COM 的应用程序或组件进行交互，这些 COM 对象通常是非托管的。
	- **内存映射文件（Memory-mapped files）**: 在某些情况下，C# 代码可能需要与操作系统的内存直接交互，例如使用内存映射文件（`MemoryMappedFile`）。尽管这些代码在托管环境下执行，但它们通常依赖于非托管的操作系统服务来处理大规模数据。
	   - 在托管代码中，不用担心手动释放内存，因为.NET会自动回收不再使用的对象。
1. **非托管代码（Unmanaged Code）**：
   - `不由.NET运行时管理`，通常通过直接访问底层系统资源或外部库实现。
   - 开发者需要手动管理内存分配和释放，否则会导致内存泄漏。
   - 例如通过P/Invoke机制调用Windows API、C++库或与硬件直接交互的代码。

### 非托管代码的典型使用场景

在C#应用中有时需要调用非托管代码，例如：

- **性能需求**：某些情况下，直接调用C或C++代码可以提高性能。
- **平台依赖功能**：如调用操作系统的API或驱动程序，特别是在需要与硬件交互时。
- **第三方库**：如果需要使用某些特定的C/C++库，可以通过非托管代码集成。

### 如何调用非托管代码

在C#中，可以通过**P/Invoke（Platform Invocation Services）**机制来调用非托管代码：

```csharp
using System.Runtime.InteropServices;

class Program
{
    // P/Invoke示例：调用Windows API函数MessageBox
    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern int MessageBox(IntPtr hWnd, string text, string caption, int type);

    static void Main()
    {
        // 调用非托管的MessageBox函数
        MessageBox(IntPtr.Zero, "Hello, world!", "Message", 0);
    }
}
```

在这个例子中，`DllImport`特性用于声明并调用Windows API中的`MessageBox`函数。

# 2. 原始字面量的使用 @

在 C# 中，原始字面量的表示方式使用的是 `@` 符号，而不是 C++ 中的 `R`。这种方式主要用于字符串字面量，使得字符串中的反斜杠 `\` 和双引号 `"` 不会被转义。

```csharp
using System;

class Program
{

    static void Main(string[] args)
    {

        string path = @"C:\Program Files\MyApp\config.json";
        Console.WriteLine(path); // C:\Program Files\MyApp\config.json

    }
}
```

# 3. 枚举

- 声明位置
  - namespace语句块中(常用)
  - class语句块中，struct语句块中
  - 但不能声明在语句块中（Main函数也不行）

```csharp
using System;

class Program
{
    enum E_number
    {
        one = 1, // 默认从0开始
        two = 2, // 后面依次+1
        three, // 3
        four, // 4
    }
    static void Main(string[] args)
    {
        E_number one = E_number.one;
        // 枚举->int
        // Color color = 2;  // int 隐式转换为 Color Cpp
        // int x = Red;  // Color 隐式转换为 int Cpp
        // 不强制转会报错，在Cpp中的C++11之前的弱类型枚举，可以隐式转换
        int oneNum = (int)one;
        Console.WriteLine(oneNum);
        // int->枚举
        E_number two = (E_number)(2);
        Console.WriteLine(two);
        // 枚举->string
        string oneStr = one.ToString();
        Console.WriteLine(oneStr);
        // string->枚举
        one = (E_number)Enum.Parse(typeof(E_number), oneStr);
        Console.WriteLine(one);
    }
}
```

# 4. string

在 C# 中，`string` 是一个用于表示文本的基本数据类型，属于引用类型。它是 .NET 中的 `System.String` 类的别名，并且是不可变（immutable）的。这意味着一旦创建了字符串对象，其内容就不能再修改。

```csharp
class Program
{

    static void Main(string[] args)
    {
	    // `System.String` 和 `string` 是等价的，它们没有本质的区别。`string` 是 `System.String` 的别名
        System.String str1 = "123";
        string str2 = "2343";
        Console.WriteLine("{0} {1}", str1, str2);
    }
}
```

### 特性

1. **不可变性**：每当对字符串进行操作（如拼接、替换、截取等），都会生成一个新的字符串对象，而不是修改原有的字符串。
2. **引用类型**：`string` 是一个引用类型，虽然它在使用上像值类型一样简单。
3. **支持字符串插值和格式化**：C# 支持字符串插值（`$`）和格式化操作。
4. **可以为 `null`** ：`string` 可以被赋值为 `null`，表示没有任何文本。

### 示例

```csharp
using System;

class Program
{
    static void Main()
    {
        // 格式化字符串
        // { 0:D}：将参数作为日期格式化。
        // { 0:N}：将数字格式化为带千位分隔符的格式。
        // { 0:F2}：将浮动小数格式化为带两位小数的固定点表示。

        DateTime date = new DateTime(2024, 11, 6);
        // Tuesday, November 6, 2024
        Console.WriteLine("{0:D}", date);

        int number = 1234567;
        // 1,234,567.00
        // N 格式说明符会在数字中插入千位分隔符，并且默认为显示两位小数。如果希望控制小数的位数，可以使用类似 {0:N3} 来控制显示三位小数。
        Console.WriteLine("{0:N}", number);

        double price = 12345.6789;
        // 12345.68
        // F2 表示将浮动小数格式化为两位小数的固定点表示，四舍五入至两位
        Console.WriteLine("{0:F2}", price);

        // 定义字符串
        string greeting = "Hello, World!";

        // 字符串拼接
        string name = "Alice";
        string message = greeting + " My name is " + name + ".";
        // Hello, World! My name is Alice.
        Console.WriteLine(message);

        // 使用字符串插值
        string interpolatedMessage = $"{greeting} My name is {name}.";
        // Hello, World! My name is Alice.
        Console.WriteLine(interpolatedMessage);

        // 字符串长度
        int length = greeting.Length;
        // Length of greeting: 13
        Console.WriteLine($"Length of greeting: {length:N}");

        // 字符串替换
        string newGreeting = greeting.Replace("World", "C#");
        // Hello, C#!
        Console.WriteLine(newGreeting);

        // 空字符串和 null
        string emptyString = "";
        string? nullString = null;//表示可以是null的string类型
        // True
        Console.WriteLine($"Is empty string empty? {string.IsNullOrEmpty(emptyString)}");
        // True
        Console.WriteLine($"Is null string null? {nullString == null}");
    }
}
```

### 说明

- **定义字符串**：可以使用双引号定义字符串，如 `"Hello, World!"`。
- **拼接字符串**：使用 `+` 运算符或字符串插值（`$`）来拼接字符串。
- **字符串长度**：通过 `Length` 属性可以获取字符串的字符数。
- **字符串替换**：使用 `Replace` 方法替换字符串中的某些内容。
- **空字符串和 `null`** ：C# 提供了 `string.IsNullOrEmpty` 方法来检查字符串是否为空或 `null`。

### 不可变性示例

```csharp
string original = "Hello";
string modified = original.Replace("H", "J");

Console.WriteLine(original); // 输出: Hello
Console.WriteLine(modified); // 输出: Jello
```

在这个例子中，`Replace` 方法不会修改 `original`，而是创建一个新的字符串 `modified`。

# 5. StringBuilder

`StringBuilder` 是 C# 中用于处理可变字符串的类。与 `string` 不同，`StringBuilder` 允许在不创建新对象的情况下修改字符串内容，从而在频繁拼接、追加或插入操作时提高性能。

### 为什么使用 `StringBuilder`

由于 `string` 是不可变的，每次修改字符串都会创建一个新的 `string` 对象。在大量拼接操作时，这种重复的创建和销毁会导致性能下降和内存浪费。而 `StringBuilder` 可以动态地修改字符串内容，不会创建多个字符串对象，能够显著提升性能。

- using System.Text;

### 使用示例

```csharp
using System;
using System.Text;

class Program
{
    static void Main()
    {
        // 创建一个 StringBuilder 对象
        StringBuilder sb = new StringBuilder("Hello");

        // 追加字符串
        sb.Append(", World! World!");
        Console.WriteLine(sb.ToString()); // 输出: Hello, World! World!

        // 插入字符串
        sb.Insert(5, " C#");
        Console.WriteLine(sb.ToString()); // 输出: Hello C#, World! World!

        // 替换所有符合的字符串
        sb.Replace("World", "StringBuilder");
        Console.WriteLine(sb.ToString()); // 输出: Hello C#, StringBuilder! StringBuilder!!

        // 删除字符串
        sb.Remove(5, 3); // 删除从索引5开始的3个字符
        Console.WriteLine(sb.ToString()); // 输出: Hello, StringBuilder! StringBuilder!

        // 清空 StringBuilder
        sb.Clear();
        Console.WriteLine($"Length after clearing: {sb.Length}"); // 输出: Length after clearing: 0
    }
}
```

### 常用方法

`StringBuilder` 是 C# 中用于高效构建和修改字符串的类，因为它能避免频繁创建新的字符串实例。以下是 `StringBuilder` 常用的方法：

#### 1. **Append**

- 将指定的字符串或对象附加到当前 `StringBuilder` 实例的末尾。
- 支持多种重载，可以附加字符串、字符、对象、数字等。

```csharp
using System;
using System.Text;

class Program
{
    class Test
    {
        public int a = 1;
        public void show()
        {
        }

    }
    static void Main()
    {
        StringBuilder sb = new StringBuilder("Hello");
        sb.Append(", World!");
        Console.WriteLine(sb); // 输出: Hello, World!

        sb.Append('+');
        Console.WriteLine(sb); // 输出: Hello, World!+

        sb.Append(new Test());
        Console.WriteLine(sb); // 输出: Hello, World!+Program+Test

        sb.Append(123);
        Console.WriteLine(sb); // 输出: Hello, World!+Program+Test123
    }
}
```

#### 2. **AppendLine**

- 在当前 `StringBuilder` 实例的末尾追加一个字符串，`并添加一个换行符`。
- 可用于追加多行文本。

```csharp
using System.Text;

class Program
{
    static void Main()
    {
        StringBuilder sb = new StringBuilder("Hello");
        sb.Append(", World!\n");
        sb.AppendLine("This is the first line.");
        sb.AppendLine("This is the second line.");
        Console.WriteLine(sb);
        // 输出:
        // Hello, World!
        // This is the first line.
        // This is the second line.
    }
}
```

#### 3. **Insert**

- 在指定位置插入一个字符串或字符。

```csharp
sb.Insert(7, "Beautiful ");
Console.WriteLine(sb); // 输出: Hello, Beautiful World!
```

#### 4. **Remove**

- 从 `StringBuilder` 中删除指定索引处的字符，指定要删除的字符数。

```csharp
sb.Remove(7, 10); // 从索引 7 开始删除 10 个字符
Console.WriteLine(sb); // 输出: Hello, World!
```

#### 5. **Replace**

- 将 `StringBuilder` 实例中的所有匹配的字符串或字符替换为指定的值。

```csharp
sb[0] = 'A';//也可以直接单个字符改
sb.Replace("World", "C#");
Console.WriteLine(sb); // 输出: Hello, C#!
```

#### 6. **Clear**

- 清空 `StringBuilder` 实例中的所有内容。

```csharp
sb.Clear();
Console.WriteLine(sb.Length); // 输出: 0
```

#### 7. **ToString**

- 将 `StringBuilder` 实例转换为 `string`。
- 可以指定一个索引和长度，从而将特定范围内的字符转换为字符串。

```csharp
string str = "Hello, World!";
StringBuilder sb = new StringBuilder(str);
sb.Append("Hello, C#!");
string str = sb.ToString();
Console.WriteLine(str); // 输出: Hello, C#!
```

#### 8. **Length**

- 获取或设置 `StringBuilder` 实例中的字符数。

```csharp
Console.WriteLine(sb.Length); // 输出: 10
sb.Length = 5; // 将长度设为 5，多余的字符将被截断
Console.WriteLine(sb); // 输出: Hello
```

#### 9. **Capacity**

- 获取或设置当前实例能够容纳的字符数。

```csharp
int capacity = sb.Capacity;
sb.Capacity = 50; // 设置容量
```

#### 10. **EnsureCapacity**

- 确保 `StringBuilder` 的容量至少为指定的值，如果小于该值，则会增加容量。

```csharp
sb.EnsureCapacity(100); // 确保容量至少为 100，如果本来就大于100，则不变
```

#### 11. **AppendFormat**

- 该方法用于将格式化字符串追加到 `StringBuilder` 实例的末尾，类似于 `string.Format` 的功能。
- 适用于需要将格式化的内容插入到字符串中的情况。

```csharp
StringBuilder sb = new StringBuilder();
sb.AppendFormat("Name: {0}, Age: {1}", "Alice", 25);
Console.WriteLine(sb); // 输出: Name: Alice, Age: 25
```

- `AppendFormat` 支持标准格式化和自定义格式化，因此非常适合在文本拼接时插入动态数据。

#### 12. **Equals**

- `StringBuilder` 的 `Equals` 方法用于比较当前实例和另一个 `StringBuilder` 实例是否相等。
- 相等的条件是：两个实例的字符内容和长度必须相同。

```csharp
using System.Text;

class Program
{
    static void Main()
    {
        StringBuilder sb1 = new StringBuilder("Hello");
        StringBuilder sb2 = new StringBuilder("Hello");
        StringBuilder sb3 = new StringBuilder("World");
        StringBuilder sb = sb1;

        Console.WriteLine(sb1.Equals(sb2)); // 输出: True，因为内容相同
        Console.WriteLine(sb1.Equals(sb3)); // 输出: False，因为内容不同
        Console.WriteLine(sb1 == sb); // 输出: True，因为引用相同
        Console.WriteLine(sb1 == sb2); // 输出: False，因为引用不同
        Console.WriteLine(ReferenceEquals(sb1, sb));  // 输出 True，因为它们是相同的对象
        Console.WriteLine(ReferenceEquals(sb1, sb2));  // 输出 false，因为它们是不同的对象
    }
}
```

### 性能对比

- **`string` 拼接**:

```csharp
string result = "";
for (int i = 0; i < 1000; i++)
{
    result += "Hello ";
}
```

每次循环都会创建一个新的 `string`，性能较差。

- **使用 `StringBuilder`**:

```csharp
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++)
{
    sb.Append("Hello ");
}
string result = sb.ToString();
```

这种方式性能更好，因为 `StringBuilder` 可以动态地扩展内存，减少不必要的对象创建。

### 初始化容量

可以通过指定初始容量来提高性能：

```csharp
StringBuilder sb = new StringBuilder(100); // 预分配100个字符的空间
```

# 6. ref和out的使用

在 C# 中，`ref` 和 `out` 是两种用于参数传递的关键字，它们允许方法通过引用传递参数，而不是通过值传递。虽然这两者的作用相似，但在使用和语义上有一些重要的区别。

### 1. `ref` 关键字

- **定义**: `ref` 关键字用于将参数作为引用传递给方法。这样在方法内对参数的任何修改都会影响到原始变量。
- **要求**: 在调用方法之前，必须先为参数赋值。
- **用法**:
  - 在方法定义中需要加上 `ref`。
  - 在调用方法时也需要加上 `ref`。

#### 示例

```csharp
using System;

class Program
{
    static void ModifyValue(ref int number)
    {
        number += 10; // 修改原始变量
    }

    static void Main()
    {
        int myNumber = 5;
        ModifyValue(ref myNumber);
        Console.WriteLine(myNumber); // 输出 15
    }
}
```

### 2. `out` 关键字

- **定义**: `out` 关键字也用于将参数作为引用传递，但在方法内部，`out` 参数必须在方法返回之前进行赋值。
- **要求**: 在调用方法之前，无需为参数赋值（即使未赋值，也能正常工作）。但在方法内部，`out` 参数必须在方法返回之前进行赋值
- **用法**:
  - 在方法定义中需要加上 `out`。
  - 在调用方法时也需要加上 `out`。

#### 示例

```csharp
using System;

class Program
{
    static void GetValues(out int number)
    {
        number = 42; // 必须在返回之前赋值
    }

    static void Main()
    {
        int myNumber; // 无需初始化
        GetValues(out myNumber);
        Console.WriteLine(myNumber); // 输出 42
    }
}
```

### 主要区别


| 特性       | `ref`                          | `out`                |
| ---------- | ------------------------------ | -------------------- |
| 初始值要求 | 需要在调用前初始化             | 不需要初始化         |
| 赋值要求   | 可以在方法内修改，也可以不修改 | 必须在方法内赋值     |
| 语义       | 表示方法可能修改参数值         | 表示方法将输出一个值 |

### 总结

- 使用 `ref` 可以在方法中修改传入的参数，并且调用方法之前需要初始化。
- 使用 `out` 适合用于返回多个值的场景，调用前不需要初始化，但必须在方法中赋值。
- ref和out可以作为重载的条件，但是两个不能同时用
  ```csharp
  using System;

  class Program
  {
      static void Main()
      {
          // 调用带有 params 参数的方法
          float f = 1.0f;
          CalcSum(ref f, 2);
      }

      static void CalcSum(ref float f, int a)
      {
          Console.WriteLine(f + a);
          // 确保显示一位小数
          Console.WriteLine((f + a).ToString("F1"));//3.0

      }

      // 两者ref和out不可以同时出现
      //static void CalcSum(out float f, int a)
      //{
      //    Console.WriteLine(f + a);

      //}
  }
  ```

```csharp
using System;

class Program
{
    // 定义方法，使用out参数返回商和余数
    static void Divide(int dividend, int divisor, out int quotient, out int remainder)
    {
        quotient = dividend / divisor;
        remainder = dividend % divisor;
    }

    static void Main()
    {
        int dividend = 10;
        int divisor = 3;
        int quotient;   // 无需初始化
        int remainder;  // 无需初始化

        // 调用方法时使用out参数
        Divide(dividend, divisor, out quotient, out remainder);

        // 输出结果
        Console.WriteLine($"Quotient: {quotient}, Remainder: {remainder}");
    }
}

```

```yaml
Quotient: 3, Remainder: 1
```

在这个例子中，`quotient`和`remainder`作为`out`参数传入方法`Divide`，不需要初始化。调用后，方法将计算结果赋值给这两个参数，供调用者使用。

- C++中&可以作为重载的标准，const int& 指的是常数，int 和int&都可以指变量，用的时候会报错

`ref` 和 `out` 是两个英文单词的缩写，具体含义如下：

1. `ref`：是 **"reference"** 的缩写，表示引用。在参数传递中，`ref` 表示该参数是通过引用传递的，这意味着方法内部对参数的修改会影响到原始变量。
2. `out`：是 **"output"** 的缩写，表示输出。`out` 参数用于方法输出结果，表示该参数用于返回值，方法内部必须对其进行赋值。

### **为什么不需要 `ref` 或 `out`？**

`ref` 和 `out` 主要用于修改值类型或需要替换引用本身的情况，但对于引用类型的内容修改，直接操作即可生效。例如：

#### **示例：引用类型无需 `ref` 的修改**

```csharp
void ModifyList(List<int> list)
{
    list.Clear();        // 修改了 list 的内容
    list.Add(1);         // 修改了 list 的内容
}

List<int> myList = new List<int> { 2, 3, 4 };
// List<T> 是引用类型，在方法内部修改它的内容会直接反映到调用方
ModifyList(myList);      // myList 被直接修改
Console.WriteLine(myList.Count); // 输出 1
```

#### **示例：需要 `ref` 替换引用本身**

```csharp
void ReplaceList(ref List<int> list)
{
    list = new List<int> { 1, 2, 3 }; // 替换了 list 的引用
}

List<int> myList = new List<int> { 2, 3, 4 };
ReplaceList(ref myList);             // 替换了 myList 的引用
Console.WriteLine(myList.Count);     // 输出 3
```

在 `GetComponents<T>(List<T> list)` 中，Unity 不需要替换 `list` 的引用，仅修改其内容，所以无需 `ref` 或 `out`。

# 7. params

在 C# 中，`params` 关键字用于允许方法接受可变数量的参数。这使得可以将多个参数作为数组传递给方法，而无需明确地创建一个数组。这在需要处理不确定数量的参数时非常方便。

### 使用示例

以下是一个使用 `params` 的示例：

```csharp
using System;

class Program
{
    static void Main()
    {
        // 调用带有 params 参数的方法
        PrintNumbers(1, 2, 3, 4, 5);
        PrintNumbers(10, 20);
        // 优先调用非可变参数的函数
        PrintNumbers(); // 空
    }

    static void PrintNumbers()
    {
        Console.WriteLine("空");
    }

    // 这里static不能省略，static的Main不能调用非static函数
    static void PrintNumbers(params int[] numbers)
    {
        Console.WriteLine("Numbers received:");
        foreach (var number in numbers)
        {
            Console.WriteLine(number);
        }
    }
}
```

### 说明

- 在这个示例中，`PrintNumbers` 方法可以接受任意数量的整数作为参数。
- `params` 参数必须是方法参数列表中的最后一个参数。
- 可以在调用 `PrintNumbers` 方法时传入任意数量的整数，包括零个。

### 注意事项

- 使用 `params` 时，方法可以接收一个数组作为参数，或者可以接收单个值而不需要手动创建数组。
- 方法内部会将所有传入的参数封装为一个数组。
- 可变参数可以作为重载的条件，但优先调用非可变参数的函数

# 8. C++的结构体和C#结构体区别

C++ 中的结构体和 C# 中的结构体有许多相似之处，但也有一些重要的区别。以下是对这两种语言中结构体的比较，涵盖定义、特性、构造函数、继承、访问控制等方面。

### C++ 中的结构体

1. **定义**：
   - 使用 `struct` 关键字定义结构体，可以包含成员变量和成员函数。

```cpp
struct Point {
    int x;
    int y;

    void display() {
        std::cout << "Point(" << x << ", " << y << ")" << std::endl;
    }
};
```

2. **访问控制**：
   - 默认情况下，C++ 中结构体的成员是 `public`，可以从外部访问。
3. **构造函数和析构函数**：
   - C++ 结构体可以有构造函数和析构函数，用于初始化和清理资源。

```cpp
struct Point {
    int x;
    int y;

    Point(int xCoord, int yCoord) : x(xCoord), y(yCoord) {} // 构造函数
    ~Point() {} // 析构函数
};
```

4. **继承和多态**：
   - C++ 中的结构体支持继承和多态，结构体可以继承自其他结构体或类。
5. **存储方式**：
   - C++ 中的结构体通常在栈上分配，但也可以通过指针在堆上动态分配。
6. 其他
   - 变量的定义不能是自己的结构体

### C# 中的结构体

- 结构体可以继承接口interface

1. **定义**：
   - 使用 `struct` 关键字定义结构体。结构体是值类型，不能继承自其他结构体或类，但可以实现接口。

```csharp
struct Point
{
    public int x;
    public int y;

    public Point(int xCoord, int yCoord) // 可以有参数化构造函数
    {
        x = xCoord;
        y = yCoord;
    }

    public void Display()
    {
        Console.WriteLine($"Point({x}, {y})");
    }
}
```

2. **访问控制**：
   - C# 中结构体的成员默认是 `private`，需要显式声明为 `public` 才能从外部访问。
3. **构造函数**：
   - C# 结构体可以有参数化构造函数，但不能有无参数的构造函数。
4. **继承**：
   - C# 中的结构体不能继承其他结构体或类，但可以实现接口。
5. **存储方式**：
   - C# 中的结构体是值类型，存储在栈上，而引用类型（如类）则存储在堆上。
6. 其他
   - 变量的定义不能是自己的结构体
   - 结构体中的变量不能直接初始化，否则初始化后，调用默认构造函数，之前的直接初始化还是没用

### 主要区别总结


| 特性           | C++ 结构体                                   | C# 结构体                                                  |
| -------------- | -------------------------------------------- | ---------------------------------------------------------- |
| 定义           | 使用`struct` 关键字                          | 使用`struct` 关键字                                        |
| 默认访问修饰符 | `public`                                     | `private`                                                  |
| 构造函数       | 可以有参数化和无参数构造函数                 | 可以有参数化构造函数，不能有无参数构造函数                 |
| 继承           | 支持继承和多态                               | 不支持继承，但可以实现接口                                 |
| 存储方式       | 通常在栈上，支持动态分配                     | 是值类型，通常在栈上                                       |
| 成员函数       | 可以有成员函数                               | 可以有方法                                                 |
| 其他           | 变量的定义不能是自己的结构体，最后有一个分号 | 变量的定义不能是自己的结构体，结构体中的变量不能直接初始化 |

### 示例代码

#### C++ 结构体示例

```cpp
#include <iostream>

struct Point {
    int x;
    int y;

    Point(int xCoord, int yCoord) : x(xCoord), y(yCoord) {} // 构造函数

    void display() {
        std::cout << "Point(" << x << ", " << y << ")" << std::endl;
    }
};

int main() {
    Point p(1, 2); // 创建结构体实例
    p.display();   // 输出: Point(1, 2)
    return 0;
}
```

#### C# 结构体示例

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public Point(int xCoord, int yCoord) // 构造函数
    {
        x = xCoord;
        y = yCoord;
    }

    public void Display()
    {
        Console.WriteLine($"Point({x}, {y})");
    }
}

class Program
{
    static void Main()
    {
        Point p = new Point(1, 2); // 创建结构体实例
        p.Display(); // 输出: Point(1, 2)
    }
}
```

# 9. 默认值

- 数字类型
  - 默认值为0
- bool
  - 默认值为false
- 引用类型
  - 默认值为null
- 查看默认值
  - default(数据类型)
  - default(int)

# 10. class

在 C# 中，类（`class`）是用于创建对象的模板，它封装了数据和行为。类可以包含字段（成员变量）、属性、方法和事件。以下是对 C# 中类的基本概念、特性和示例的详细介绍：

### 基本概念

- **定义**：类是自定义数据类型的蓝图，可以包含状态（字段）和行为（方法）。
- **实例化**：通过类创建对象实例，每个实例都可以独立地持有状态。

### 特性

1. **封装**：
   - 类可以将数据和方法组合在一起，通过访问修饰符（如 `public`、`private`、`protected`）来控制对类成员的访问。
2. **继承**：
   - 类可以通过继承从其他类派生，复用代码并创建层次结构。
   - 子类可以重写父类的方法，实现多态性。
3. **多态性**：
   - 通过虚方法（`virtual`）和重写方法（`override`），可以实现不同类对同一方法的不同实现。
4. **抽象**：
   - 抽象类（`abstract class`）可以定义一组方法，但不能直接实例化。
5. **接口**：
   - 接口（`interface`）定义了一组方法，但不实现它们。类可以实现多个接口。

- 不能够将方法的声明和定义分离，必须放在一起。当然，抽象类和接口可以单独声明函数，然后继承的时候再重写

### 类的定义和使用示例

```csharp
using System;

// 定义一个简单的类
public class Car
{
    // 字段
    private string make;
    private string model;
    private int year;

    // 属性
    public string Make
    {
        get { return make; }
        set { make = value; }
    }

    public string Model
    {
        get { return model; }
        set { model = value; }
    }

    public int Year
    {
        get { return year; }
        set { year = value; }
    }

    // 构造函数
    public Car(string make, string model, int year)
    {
        this.make = make;
        this.model = model;
        this.year = year;
    }

    // 方法
    public void DisplayInfo()
    {
        Console.WriteLine($"Car Info: {Year} {Make} {Model}");
    }
}

class Program
{
    static void Main()
    {
        // 创建 Car 类的实例
        Car myCar = new Car("Toyota", "Camry", 2021);
        myCar.DisplayInfo(); // 输出: Car Info: 2021 Toyota Camry
    }
}
```

### 重要概念说明

- **构造函数**：
  - 特殊方法，在创建对象时被调用。可以用于初始化对象的状态。
- **析构函数**：
  - 用于清理资源，类在不再需要时被销毁时调用（在 C# 中，通常使用 `IDisposable` 接口和 `using` 语句进行资源管理）。
- **静态成员**：
  - 使用 `static` 修饰符定义的成员属于类本身，而不是类的实例。

### 静态成员示例

```csharp
public class MathUtility
{
    // 静态方法
    public static int Add(int a, int b)
    {
        return a + b;
    }
}

class Program
{
    static void Main()
    {
        // 调用静态方法
        int result = MathUtility.Add(5, 10);
        Console.WriteLine($"Result: {result}"); // 输出: Result: 15
    }
}
```

### 构造函数，析构函数示例

- ~MyClass() 是终结器（finalizer 或 Finalize 方法），不是析构方法。语义上和析构接近的是 Dispose 方法。

```csharp
using System;

class MyClass
{
    // 字段
    private string name;

    // 无参构造函数
    public MyClass()
    {
        name = "Default Name";
        Console.WriteLine("无参构造函数被调用: " + name);
    }

    // 带参数的构造函数
    public MyClass(string newName)
    {
        name = newName;
        Console.WriteLine("带参数的构造函数被调用: " + name);
    }

    // 终结器（finalizer 或 Finalize 方法）
    ~MyClass()
    {
        Console.WriteLine("析构函数被调用，清理资源...");
    }
}

class Program
{
    static void Main()
    {
        // 使用无参构造函数创建对象
        MyClass obj1 = new MyClass();

        // 使用带参数的构造函数创建对象
        MyClass obj2 = new MyClass("Hello, World!");

        // 程序结束时，析构函数会自动被调用
    }
}
// 无参构造函数被调用: Default Name 
// 带参数的构造函数被调用: Hello, World! 
// =====析构函数不一定会调用，不用管下面的
// 析构函数被调用，清理资源... 
// 析构函数被调用，清理资源...
```

### 访问修饰符


| 访问修饰符           | 访问范围                         |
| -------------------- | -------------------------------- |
| `public`             | 可以在任何地方访问               |
| `private`            | 只能在定义它的类内部访问(默认)   |
| `protected`          | 可以在定义它的类及其派生类中访问 |
| `internal`           | 可以在同一程序集内访问           |
| `protected internal` | 可以在同一程序集或派生类中访问   |

### C#构造函数特殊写法，通过this重用构造函数代码

在 C# 中，可以通过使用 `this` 关键字在一个构造函数内部调用另一个构造函数，以重用构造函数的代码。这种写法被称为构造函数重载或构造函数链。它允许你在不同的构造函数中共享初始化逻辑，从而减少代码重复。

### 示例

以下是一个示例，展示了如何通过 `this` 关键字在 C# 中重用构造函数代码：

```csharp
using System;

class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    // 主构造函数
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

    // 另一个构造函数，通过 this 关键字调用主构造函数
    public Person(string name) : this(name, 0) // 默认年龄设为 0
    {
        // 这里可以添加其他初始化逻辑
    }

    // 另一个构造函数，通过 this 关键字调用主构造函数
    public Person() : this("Unknown", 0) // 默认姓名设为 "Unknown"，年龄为 0
    {
        // 这里可以添加其他初始化逻辑
    }
}

class Program
{
    static void Main()
    {
        Person p1 = new Person("Alice", 30);
        Console.WriteLine($"{p1.Name}, {p1.Age}"); // 输出: Alice, 30

        Person p2 = new Person("Bob");
        Console.WriteLine($"{p2.Name}, {p2.Age}"); // 输出: Bob, 0

        Person p3 = new Person();
        Console.WriteLine($"{p3.Name}, {p3.Age}"); // 输出: Unknown, 0
    }
}
```
### 解释
1. **主构造函数**：`Person(string name, int age)` 是主要的构造函数，用于初始化 `Name` 和 `Age` 属性。
2. **重载构造函数**：
   - `Person(string name)` 通过 `: this(name, 0)` 调用主构造函数，设定默认的 `Age` 为 `0`。
   - `Person()` 通过 `: this("Unknown", 0)` 调用主构造函数，设定默认的 `Name` 为 `"Unknown"`，`Age` 为 `0`。

# 11. 类与结构体区别（C#）
在 C# 中，类（`class`）和结构体（`struct`）都是用于封装数据和行为的复合数据类型，但它们在多个方面存在显著的区别。以下是它们的主要区别：
### 主要区别

| 特性             | 类 (Class)                   | 结构体 (Struct)                                                                                                                           |
| ---------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **类型**         | 引用类型                     | 值类型                                                                                                                                    |
| **存储方式**     | 存储在堆上                   | 存储在栈上                                                                                                                                |
| **继承**         | 支持继承                     | 不支持继承                                                                                                                                |
| **构造函数**     | 可以有无参数和有参数构造函数 | 只能有参数构造函数，不能有无参数构造函数                                                                                                  |
| **默认构造函数** | 有默认构造函数               | 没有默认构造函数，编译器自动提供一个隐式的默认构造函数，该构造函数会将结构体的所有字段初始化为其类型的默认值（如`0`、`false`、`null` 等） |
| **访问修饰符**   | 默认是`private`              | 默认是`private`                                                                                                                           |
| **接口实现**     | 可以实现接口                 | 可以实现接口                                                                                                                              |
| **实例化**       | 通过`new` 创建               | 通过`new` 创建                                                                                                                            |
| **赋值行为**     | 赋值时复制引用               | 赋值时复制值                                                                                                                              |
| **内存管理**     | 垃圾回收（GC）管理           | 垃圾回收（GC）管理                                                                                                                        |
| **性能**         | 对于小对象，性能开销较大     | 对于小对象，性能开销较小                                                                                                                  |

### 详细说明

1. **类型**：
   - **类**是引用类型，意味着它们的实例在内存中是通过引用来访问的。
   - **结构体**是值类型，意味着它们的实例直接包含数据。
2. **存储方式**：
   - 类的对象存储在堆上，结构体的实例通常存储在栈上（如果是局部变量），因此结构体在内存分配和释放上更加高效。
3. **继承**：
   - 类支持继承，可以派生出子类。结构体不支持继承，因此不能从其他结构体或类继承。
4. **构造函数**：
   - 类可以有无参数构造函数和有参数构造函数，而结构体只能有有参数构造函数，不能有无参数构造函数。结构体会自动提供一个默认构造函数，该构造函数将所有字段设置为其默认值。
   - 类如果声明了有参构造函数，默认的无参构造函数会消失；结构体声明有参构造函数后，无参构造仍然存在
5. **赋值行为**：
   - 当类的对象被赋值给另一个变量时，实际上是复制了引用，而不是对象本身。因此，修改一个对象会影响所有引用它的变量。
   - 结构体的赋值操作会复制整个数据，因此修改一个结构体不会影响另一个结构体的值。
6. 访问修饰符:
   - 结构体成员不可以用protected访问修饰符，类可以
7. 初始值
- 结构体成员变量不能指定初始值，类可以
- 但结构体需要在构造函数中初始化所有成员变量，类随意
8. 析构函数（指终结器）
 - 结构体不能声明析构函数，类可以
9. static修饰
- 结构体不能够被静态static修饰（不存在静态结构体），类可以
10. 嵌套包含
- 结构体不能在自己内部声明和自己一样的结构体变量，类可以

### 示例代码

#### 类的示例

```csharp
using System;

class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age) // 构造函数
    {
        Name = name;
        Age = age;
    }
}

class Program
{
    static void Main()
    {
        Person person1 = new Person("Alice", 30);
        Person person2 = person1; // 复制引用
        person2.Name = "Bob"; // 修改 person2 的 Name

        Console.WriteLine(person1.Name); // 输出: Bob
    }
}
```

#### 结构体的示例

```csharp
using System;

class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age) // 构造函数
    {
        Name = name;
        Age = age;
    }
}

class Program
{
    static void Main()
    {
        Person person1 = new Person("Alice", 30);
        Person person2 = person1; // 复制引用
        person2.Name = "Bob"; // 修改 person2 的 Name

        Console.WriteLine(person1.Name); // 输出: Bob
    }
}
```

### 如何选择结构体和类

- 想要用继承和多态时，选择类，比如玩家，怪物等
- 对象是数据集合时，优先考虑结构体，比如位置，坐标等
- 从值类型和引用类型赋值时区别上考虑，如果经常被赋值传递的对象，并且改变赋值对象，原对象不想跟着改变时，使用结构体，比如坐标，向量，旋转等

# 12. C#垃圾回收机制

C#的垃圾回收机制（Garbage Collection，简称GC）是一种自动内存管理机制，用于释放不再使用的对象和回收内存资源，从而避免手动管理内存。它在.NET框架中由CLR（Common Language Runtime）负责执行，确保应用程序在运行期间高效地使用内存。

### 主要特点

1. **自动内存管理**：不需要开发人员手动释放对象的内存，GC会自动检测并释放不再使用的对象。
2. **内存压缩**：GC不仅会回收无用的对象，还会对内存进行压缩，把存活的对象移动到内存的开头，减少碎片。
3. **分代回收**：采用分代（Generational）回收机制，根据对象的生命周期将内存划分为不同的代。

### 分代回收机制

GC将托管堆中的对象分为三代：

- **第0代（Gen 0）**：短期对象，如临时变量。GC最频繁地回收这部分对象。
- **第1代（Gen 1）**：较长时间的对象或从Gen 0晋升的对象。适用于中等寿命的对象。
- **第2代（Gen 2）**：长期对象或从Gen 1晋升的对象，如全局静态变量。GC最不频繁地回收这部分对象。

在进行垃圾回收时，GC会优先处理Gen 0，如果清理不够再继续回收Gen 1和Gen 2。这样可以减少不必要的回收操作，提升性能。

### 晋升的规则

对象的晋升主要发生在垃圾回收的过程中，规则如下：

1. **第0代（Gen 0）到第1代（Gen 1）的晋升**：

   - 当第0代进行垃圾回收时，如果某个对象存活（即仍被引用），则这个对象将晋升到第1代。
   - 如果一个对象在Gen 0的垃圾回收后依然存活，它通常被认为是一个较为持久的对象，因此被晋升到Gen 1，减少它在以后的垃圾回收中被频繁扫描的次数。
2. **第1代（Gen 1）到第2代（Gen 2）的晋升**：

   - 当第1代进行垃圾回收时，仍然存活的Gen 1对象将被晋升到Gen 2。
   - Gen 2被认为是“老年代”，也就是说，这些对象的生命周期较长，可能会存活很长时间。因此，GC会对Gen 2对象进行更少的回收操作，以减少垃圾回收的开销。
3. **第2代（Gen 2）对象的处理**：

   - Gen 2的对象在垃圾回收后仍然存活，不会再晋升，因为Gen 2已经是最高代。

### 工作流程

1. **标记阶段**：GC扫描所有的对象，标记出哪些是存活的，哪些是不再使用的。
2. **回收阶段**：对不再使用的对象进行回收，释放它们的内存。
3. **压缩阶段**：把存活的对象移动到内存的开头，减少内存碎片，并更新对象引用。

### 什么时候进行垃圾回收？

- 内存不足时。
- 分配了大量对象，Gen 0空间不足。
- 显式调用`GC.Collect()`方法（不建议频繁使用）。

### 示例

```csharp
using System;

class Program
{
    static void Main()
    {
        // 创建大量对象，强制触发GC
        for (int i = 0; i < 100000; i++)
        {
            string temp = new string('a', 1000);
        }

        // 手动调用GC（不推荐）
        GC.Collect();
        Console.WriteLine("手动垃圾回收完成");
    }
}
```

### 需要注意的事项

1. **GC.Collect()的使用**：尽量避免手动调用GC，因为GC会自动管理内存。手动调用会导致性能下降。
2. **非托管资源的清理**：对于非托管资源（如文件句柄、数据库连接等），需要实现`IDisposable`接口，并在`Dispose`方法中释放资源。
3. **Finalize和Dispose**：实现`Dispose`方法或使用`using`语句来清理资源，避免依赖GC进行非托管资源的回收。[GC终结器性能看这个，](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/classes-and-structs/finalizers)不过Finalize还是可以用来释放非托管资源，如文件句柄、数据库连接等。但还是最好用IDisposable接口来实现。
4. 析构函数（也称为终结器）只有在垃圾回收器（GC）回收对象时才会被调用

### 实现`IDisposable`接口来清理非托管资源的示例

示例将展示如何使用`Dispose`方法和析构函数（`Finalize`）来处理非托管资源的清理

```csharp
using System;

class ResourceHolder : IDisposable
{
    // 模拟的非托管资源
    private IntPtr unmanagedResource;
    private bool disposed = false; // 用来跟踪对象是否已被释放

    public ResourceHolder()
    {
        // 分配非托管资源
        unmanagedResource = new IntPtr(123);
        Console.WriteLine("ResourceHolder: 非托管资源已分配.");
    }

    // 实现IDisposable接口的Dispose方法
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this); // 防止析构函数重复释放资源
    }

    // 受保护的Dispose方法，真正的资源释放逻辑在这里执行
    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // 释放托管资源（如果有）
                Console.WriteLine("ResourceHolder: 释放托管资源.");
            }

            // 释放非托管资源
            if (unmanagedResource != IntPtr.Zero)
            {
                Console.WriteLine("ResourceHolder: 释放非托管资源.");
                unmanagedResource = IntPtr.Zero;
            }

            disposed = true;
        }
    }

    // 析构函数（Finalize），在Dispose没有被调用时释放资源，确保我们只释放非托管资源。
    ~ResourceHolder()
    {
        Dispose(false);
    }
}

class Program
{
    static void Main()
    {
        using (ResourceHolder resource = new ResourceHolder())
        {
            Console.WriteLine("使用ResourceHolder对象.");
        } // using语句结束后自动调用Dispose方法

        Console.WriteLine("程序结束.");
    }
}

// ResourceHolder: 非托管资源已分配.
// 使用ResourceHolder对象.
// ResourceHolder: 释放托管资源.
// ResourceHolder: 释放非托管资源.
// 程序结束.
```

### 解释

1. **`IDisposable`接口**：`ResourceHolder`类实现了`IDisposable`接口，提供`Dispose`方法来释放资源。
2. **`Dispose`方法**：实现`Dispose`方法，用于释放托管和非托管资源。调用`GC.SuppressFinalize(this)`避免在垃圾回收时调用析构函数。
3. **析构函数（`Finalize`）**：析构函数用于在`Dispose`方法未被显式调用时释放非托管资源。
4. **`using`语句**：`using`语句会在代码块结束时自动调用`Dispose`方法，确保资源被及时释放。

# 13. 代码解析using (ResourceHolder resource = new ResourceHolder()) { Console.WriteLine("使用ResourceHolder对象."); }

```csharp
using (ResourceHolder resource = new ResourceHolder())
{
    Console.WriteLine("使用ResourceHolder对象.");
} // using语句结束后自动调用Dispose方法
```

#### 1. `ResourceHolder` 类

假设 `ResourceHolder` 是一个实现了 `IDisposable` 接口的类。`IDisposable` 接口定义了一个 `Dispose` 方法，用于显式地释放非托管资源。实现 `IDisposable` 是处理非托管资源或需要手动释放的资源（如文件句柄、数据库连接等）的一种常见做法。

#### 2. `using` 语句的作用

- `using` 语句可以自动管理实现了 `IDisposable` 接口的对象的生命周期。
- 在进入 `using` 块时，会创建一个 `ResourceHolder` 对象并将其分配给 `resource` 变量。
- 当 `using` 语句结束时，无论是否发生异常，都会自动调用 `resource.Dispose()` 方法，以确保资源被正确释放。

#### 3. 代码的执行流程

- 创建 `ResourceHolder` 对象，并分配给 `resource`。
- 执行 `using` 块内的代码，输出 "使用ResourceHolder对象."
- 当 `using` 块结束时，`Dispose` 方法被自动调用，释放资源。

等价于下面的代码：

```csharp
ResourceHolder resource = new ResourceHolder();
try
{
    Console.WriteLine("使用ResourceHolder对象.");
}
finally
{
    // 确保 Dispose 方法在 using 块结束时被调用
    if (resource != null)
    {
        resource.Dispose();
    }
}
```

### `IDisposable` 接口和 `Dispose` 方法的作用

`IDisposable` 接口定义了一个 `Dispose` 方法，用于释放资源。典型的实现方式如下：

```csharp
public class ResourceHolder : IDisposable
{
    // 实现 Dispose 方法
    public void Dispose()
    {
        // 释放资源的代码，如关闭文件句柄或断开数据库连接
        Console.WriteLine("ResourceHolder 的 Dispose 方法被调用.");
    }
}
```

在这个例子中，`Dispose` 方法释放资源，而 `using` 语句确保在使用完对象后自动调用 `Dispose`。

### 总结

- `using` 语句用于自动管理实现了 `IDisposable` 接口的对象的生命周期，确保资源在使用后被正确释放。
- 在 `using` 块结束时，会自动调用 `Dispose` 方法，释放资源，避免内存泄漏或资源占用。

# 14. IntPtr解释

`IntPtr` 是 C# 中表示指针或句柄的平台相关类型。它的主要用途是处理非托管资源、与操作系统交互时的指针或句柄、以及在托管代码和非托管代码之间传递指针。以下是 `IntPtr` 的几个关键点：

1. **平台相关大小**：`IntPtr` 的大小取决于当前运行的平台。在 32 位平台上，`IntPtr` 是 4 字节（32 位），而在 64 位平台上，它是 8 字节（64 位）。这使得它非常适合用来表示指针或句柄的大小。
2. **表示指针或句柄**：`IntPtr` 通常用于存储指针（如内存地址）或操作系统资源的句柄（如窗口句柄、文件句柄）。它可以存储一个整数值，表示内存地址或某种非托管资源的标识符。
3. **用于非托管代码交互**：在与非托管代码交互时（例如，通过 P/Invoke 调用 Win32 API），`IntPtr` 常被用来表示指针参数。它可以与 C/C++ 中的 `void*` 类型类似，适合用来传递内存地址。
4. **转换和比较**：`IntPtr` 支持从整数类型（`int` 或 `long`）进行隐式或显式转换，也可以与整数进行比较或相互赋值。

### 示例

下面是一个使用 `IntPtr` 来表示一个非托管资源句柄的示例：

```csharp
using System;

class Example
{
    static void Main()
    {
        // 模拟一个非托管资源的句柄（假设资源的句柄值为123）
        IntPtr unmanagedResource = new IntPtr(123);

        // 打印 IntPtr 的值
        Console.WriteLine("非托管资源句柄: " + unmanagedResource);

        // 检查 IntPtr 是否为零
        if (unmanagedResource != IntPtr.Zero)
        {
            Console.WriteLine("资源已分配.");
        }
        else
        {
            Console.WriteLine("资源未分配.");
        }

        // 将 IntPtr 转换为 long 类型
        long handleValue = unmanagedResource.ToInt64();
        Console.WriteLine("句柄的长整型值: " + handleValue);
    }
}
```

### 输出

```makefile
非托管资源句柄: 123
资源已分配.
句柄的长整型值: 123
```

在这个例子中，`IntPtr` 被用来模拟表示一个非托管资源的句柄值，并可以进行检查和转换。通过使用 `IntPtr`，代码可以适应不同的平台（32 位或 64 位），确保指针或句柄的大小能够正确匹配。

### IntPtr.Zero的解释

`IntPtr.Zero` 是 .NET 中 `IntPtr` 结构的一个静态只读字段，表示一个空指针或指针值为零的情况。它的作用类似于 C/C++ 中的 `NULL` 或 `nullptr`，用于表示一个未分配的指针或句柄。`IntPtr` 是一种平台相关的类型，用于存储指针或句柄的值。

### `IntPtr` 结构

- `IntPtr` 是一个可以存储内存地址（指针）或操作系统句柄的平台无关类型。
- 它的大小是平台相关的：在 32 位系统上是 4 字节，在 64 位系统上是 8 字节。

### `IntPtr.Zero` 的用途

- `IntPtr.Zero` 可以用来检查指针或句柄是否为空，例如在调用非托管代码或处理操作系统资源时，判断一个指针是否已经被分配。
- 作为默认值，表示未初始化的指针或句柄。

### 示例代码

在使用 P/Invoke 或与非托管代码交互时，可以用 `IntPtr.Zero` 来表示空指针：

```csharp
IntPtr handle = IntPtr.Zero; // 初始化为一个空指针

// 检查指针是否为空
if (handle == IntPtr.Zero)
{
    Console.WriteLine("Handle 未被初始化.");
}
else
{
    Console.WriteLine("Handle 已初始化.");
}

```

在这个例子中，`IntPtr.Zero` 用来判断 `handle` 是否为未分配的空指针。

### 句柄的解释

在计算机编程中，**句柄**（Handle）是用于引用系统资源的一个抽象标识符。它可以看作是一个间接指向资源的指针，通过句柄可以对资源进行操作，而不需要直接访问资源的内存地址。句柄通常用于操作系统提供的资源管理，例如文件、内存、窗口、线程、数据库连接等。

### 句柄的作用

- **间接引用**：句柄为资源提供了一个间接的访问方式，使得程序不需要知道资源的具体内存地址，只需使用句柄进行操作。
- **资源管理**：操作系统或库使用句柄来管理资源的分配和释放。句柄由操作系统分配，当资源不再需要时，程序员可以释放句柄来回收资源。
- **类型安全**：通过句柄访问资源可以防止程序直接操作资源的内存地址，提高了系统的安全性和稳定性。

### 常见的句柄类型

- **文件句柄**：用于打开和操作文件，如读取或写入文件数据。
- **窗口句柄（HWND）**：在图形用户界面编程中，用于标识窗口。
- **进程或线程句柄**：用于操作系统中标识进程或线程的标识符。
- **数据库句柄**：用于连接到数据库或执行查询。

### 示例

在 C# 中，使用句柄来操作系统资源，如文件句柄：

```csharp
using (FileStream fileStream = new FileStream("example.txt", FileMode.Open))
{
    // fileStream.SafeFileHandle 是一个文件句柄
    if (!fileStream.SafeFileHandle.IsInvalid)
    {
        Console.WriteLine("文件句柄有效.");
    }
}
```

在这个例子中，`fileStream.SafeFileHandle` 是文件句柄，用于管理文件资源。句柄让操作系统跟踪文件的状态，并在使用完毕后释放资源。

# 15. 类中的属性

在 C# 中，类的属性是用于封装字段（成员变量）并提供对这些字段的访问的机制。属性使得对类内部数据的访问更加安全和灵活，可以控制对字段的读取和写入。

### 属性的基本定义

属性通常由两个部分组成：

1. **get 访问器**：用于获取属性的值。get必须有返回值
2. **set 访问器**：用于设置属性的值。set可以不设置赋值

以下是一个简单的示例，展示了如何在类中定义属性：

```csharp
public class Person
{
    // 私有字段
    private string name;
    private int age;

    // 公共属性
    public string Name
    {
        get { return name; }   // 获取name字段的值
        set { name = value; }  // 设置name字段的值
    }

    public int Age
    {
        get { return age; }    // 获取age字段的值
        set
        {
            // 可以添加逻辑，例如确保年龄不能为负数
            if (value < 0)
            {
                throw new ArgumentException("年龄不能为负数");
            }
            age = value;         // 设置age字段的值
        }
    }
}
```

### 使用属性

可以通过实例化类并直接访问属性来使用它们：

```csharp
class Program
{
    static void Main()
    {
        Person person = new Person();
        person.Name = "Alice"; // 设置属性值
        person.Age = 30;       // 设置属性值

        Console.WriteLine($"Name: {person.Name}, Age: {person.Age}"); // 获取属性值
    }
}
```

### 自动属性

C# 还提供了自动属性的功能，可以简化属性的定义，编译器会为这些属性创建一个私有的匿名字段。我们只需要使用Name或者Age获取或者修改值即可。下面是一个使用自动属性的例子：

```csharp
public class Student
{
    // 自动属性
    public string Name { get; set; }
    public int Age { get; set; }
}
```

### 只读和只写属性

属性也可以设置为只读或只写：

- **只读属性**：只有 `get` 访问器，没有 `set` 访问器，不能直接修改。

```csharp
public class ReadOnlyPerson
{
    private string name;

    public ReadOnlyPerson(string name)
    {
        this.name = name;
    }

    public string Name
    {
        get { return name; } // 只读属性
    }
}
```

- **只写属性**：只有 `set` 访问器，没有 `get` 访问器，不能直接读取。

```csharp
public class WriteOnlyPerson
{
    private int age;

    public int Age
    {
        set { age = value; } // 只写属性
    }
}
```

### get，set的访问修饰符的规定

在 C# 中，属性的 `get` 和 `set` 访问修饰符可以单独定义，允许你对属性的读取和写入操作分别控制访问权限。这种灵活性使得你能够根据需要来限制对类成员的访问。

### 访问修饰符

C# 提供了以下几种常用的访问修饰符：

1. **public**：可以被任何其他代码访问。
2. **private**：只能在定义该成员的类内部访问。
3. **protected**：只能在定义该成员的类及其子类中访问。
4. **internal**：只能在同一程序集（项目）中访问。
5. **protected internal**：可以在同一程序集或从其子类中访问。

### 示例

以下是一些示例，展示了如何为 `get` 和 `set` 分别指定不同的访问修饰符：

```csharp
public class Person
{
    private string name; // 私有字段
    private int age;

    // 只读属性
    public string Name
    {
        get { return name; }  // 公开的get方法
        private set { name = value; }  // 仅在类内部可设置
    }

    // 只写属性
    public int Age
    {
        private get { return age; } // 仅在类内部可读取
        // 注释掉下面一行，会报错
        set { age = value; }  // 公开的set方法
    }

    public Person(string name, int age)
    {
        Name = name;  // 通过公共set方法设置
        Age = age;    // 通过公共set方法设置
    }
}
```

### 使用示例

```csharp
class Program
{
    static void Main()
    {
        Person person = new Person("Alice", 30);
    
        // 访问Name属性
        Console.WriteLine(person.Name); // 公开的get方法可以访问

        // 不能直接设置Name属性，因为set是私有的
        // person.Name = "Bob"; // 这行代码会产生编译错误

        // 访问和修改Age属性
        // Console.WriteLine(person.Age); // 编译错误，因为get是私有的
        person.Age = 35; // 公开的set方法可以访问
    }
}
```

### 访问修饰符的规则

- **访问修饰符可以单独定义**：`get` 和 `set` 访问修饰符可以不同。例如，`get` 可以是 `public`，而 `set` 可以是 `private`。
- **当未指定访问修饰符时**：如果没有明确指定 `get` 和 `set` 的访问修饰符，则默认情况下：

  - `get` 默认为 属性声明时的访问权限。
  - `set` 默认为  属性声明时的访问权限（如果是自动属性，或类内部没有定义）。
- **构造函数和方法**：构造函数和其他方法可以访问私有属性的 `set` 访问器，从而在对象创建时初始化值。
- 仅当属性或索引器同时具有 `set` 和 `get` 访问器时，才能使用访问器修饰符。 这种情况下，只允许对其中一个访问器使用修饰符。
- 访问器的可访问性级别必须比属性或索引器本身的可访问性级别具有更严格的限制。如果属性本身就是private，get和set均不可以使用访问修饰符。如果是public的属性，不可以对get或者set使用public，只能更严格

# 16. 索引器

C#中的索引器是一种特殊的属性，允许对象像数组一样通过索引访问其元素。索引器使得类的实例能够使用“[]”语法来访问其内部数据，使得代码更加简洁和直观。索引器的定义通常涉及到 `this` 关键字，后面跟随一个或多个参数。

- 索引器可以重载，中括号里面的参数不一样就可以
- 结构体也支持索引器

### 基本语法

索引器的基本语法如下：

```csharp
using System;

public class MyClass
{
    private int[] data = new int[10]; // 内部数组

    // 定义索引器
    // 访问修饰符 返回值 this[参数类型 参数名, 参数类型 参数名, ...]
    public int this[int index]
    {
	    // 内部书写和属性相同
        get { return data[index]; } // 获取元素
        set { data[index] = value; } // 设置元素
    }
}

class Program
{
    static void Main()
    {
        MyClass myClass = new MyClass();
    
        // 使用索引器设置元素
        myClass[0] = 10;
        myClass[1] = 20;

        // 使用索引器获取元素
        Console.WriteLine(myClass[0]); // 输出: 10
        Console.WriteLine(myClass[1]); // 输出: 20
    }
}
```

### 使用示例

以下是一个使用索引器的完整示例：

```csharp
using System;

public class MyClass
{
    private int[] data = new int[10]; // 内部数组

    // 定义索引器
    public int this[int index]
    {
        get { return data[index]; } // 获取元素
        set { data[index] = value; } // 设置元素
    }
}

class Program
{
    static void Main()
    {
        MyClass myClass = new MyClass();
    
        // 使用索引器设置元素
        myClass[0] = 10;
        myClass[1] = 20;

        // 使用索引器获取元素
        Console.WriteLine(myClass[0]); // 输出: 10
        Console.WriteLine(myClass[1]); // 输出: 20
    }
}
```

### 特点

1. **简洁性**：索引器提供了一种方便的方式来访问对象的内部数据，语法类似于数组的访问方式。
2. **可以重载**：索引器可以有多个重载形式，以支持不同类型和数量的参数。
3. **访问修饰符**：索引器的 `get` 和 `set` 访问修饰符可以独立设置，可以是 `public`、`private`、`protected` 等。
4. **类型安全**：索引器可以返回任何类型，而不仅限于基本数据类型。

### 访问修饰符示例

下面的示例演示了如何设置不同的访问修饰符：

```csharp
using System;

public class MyCollection
{
    private int[] data = new int[10];

    // 只允许内部访问的索引器
    private int this[int index]
    {
        get { return data[index]; }
        set { data[index] = value; }
    }

    // 公开的索引器
    public int this[string index]
    {
        get { return data[int.Parse(index)]; }
        set { data[int.Parse(index)] = value; }
    }

    // 公共方法可以通过私有索引器来间接访问数据
    public void SetDataAt(int index, int value)
    {
        if (index >= 0 && index < data.Length)
        {
            this[index] = value;  // 使用私有索引器设置值
        }
        else
        {
            Console.WriteLine("Index out of bounds");
        }
    }

    public int GetDataAt(int index)
    {
        if (index >= 0 && index < data.Length)
        {
            return this[index];  // 使用私有索引器获取值
        }
        else
        {
            Console.WriteLine("Index out of bounds");
            return -1;
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        MyCollection collection = new MyCollection();

        // 通过公共方法间接访问私有索引器
        collection.SetDataAt(0, 100);
        collection.SetDataAt(1, 200);

        Console.WriteLine(collection.GetDataAt(0));  // 输出 100
        Console.WriteLine(collection.GetDataAt(1));  // 输出 200
        Console.WriteLine(collection["0"]);  // 输出 100
        Console.WriteLine(collection["1"]);  // 输出 200
    }
}

```

### 使用注意事项

- **越界检查**：在 `get` 和 `set` 访问器中可以添加越界检查，以避免访问数组外的元素。
- **性能**：索引器与普通方法的性能基本相同，但使用索引器的代码通常更加简洁。
- **适用性**：索引器适用于需要通过索引访问集合或数组的场景，例如自定义集合类。

索引器为C#提供了一种直观且强大的方式来操作类的内部数据，使得编写清晰易读的代码变得更加容易。

# 17. static的使用

在C#中，`static`关键字用于定义静态成员或静态类。静态成员属于类本身，而不是属于类的某个实例，因此可以在不创建类对象的情况下访问。`static`的主要用途包括定义静态字段、方法、属性、构造函数、类和操作符等。以下是C#中`static`的具体使用情况：

### 1. 静态字段（Static Fields）

静态字段在类级别定义，对于该类的所有实例共享同一个字段。可以通过类名来访问静态字段，而不需要创建对象实例。

```csharp
public class Counter
{
    public static int Count = 0; // 静态字段

    public Counter()
    {
        Count++; // 每创建一个实例，Count加1
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine(Counter.Count); // 输出: 0

        Counter c1 = new Counter();
        Counter c2 = new Counter();

        Console.WriteLine(Counter.Count); // 输出: 2，因为创建了两个实例
    }
}
```

### 2. 静态方法（Static Methods）

- 静态方法可以在没有创建对象实例的情况下被调用。静态方法中不能访问非静态成员，因为它们没有绑定到任何实例。静态方法属于类本身，而不是某个具体的实例对象。我记得C++中，还有一个角度是static是在编译阶段确定，而非static是在运行阶段确定，所以无法访问未定义的变量，C#也可以这么理解。
- 在 C# 中，静态方法无法访问非静态成员的原因与 C++ 类似。静态方法属于类本身，而非某个具体的对象，因此它没有 `this` 引用，因为 `this` 只在实例方法中才有意义。非静态成员需要特定的对象实例来调用，而静态方法在类的上下文中调用，无法使用 `this` 来访问非静态成员。
- 而非静态方法，是可以使用静态成员的

```csharp
using System.Collections.Specialized;
using System.Runtime.CompilerServices;

public class MathUtils
{
    int c = 0;

    public static int Add(int a, int b) // 静态方法
    {
        // this.c // 报错
        return a + b;
    }

    public void show()
    {
        Console.WriteLine(this.c);
    }
}

class Program
{
    static void Main()
    {
        int result = MathUtils.Add(3, 5); // 调用静态方法
        Console.WriteLine(result); // 输出: 8
    }
}
```

### 3. 静态属性（Static Properties）

静态属性用于封装静态字段的访问，类似于静态字段，但通过`get`和`set`访问器来访问或修改值。

```csharp
public class Settings
{
    private static int _volume = 5; // 静态字段

    public static int Volume // 静态属性
    {
        get { return _volume; }
        set { _volume = value; }
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine(Settings.Volume); // 输出: 5
        Settings.Volume = 10;
        Console.WriteLine(Settings.Volume); // 输出: 10
    }
}
```

### 4. 静态构造函数（Static Constructors）

静态构造函数用于初始化静态字段或执行只需要运行一次的操作。它在类的第一次使用之前自动调用。

```csharp
public class Logger
{
    public static string LogFilePath;

    static Logger() // 静态构造函数，C++不允许静态构造函数，会报错
    {
        LogFilePath = "log.txt";
        Console.WriteLine("静态构造函数被调用");
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine(Logger.LogFilePath); // 输出: log.txt
    }
}
```

静态构造函数的主要作用是在类的第一次使用之前自动执行一次，无论是否创建了该类的实例。它通常用于初始化静态字段或执行只需运行一次的操作。

#### 关键点

1. **只执行一次**：静态构造函数只会在第一次访问类时自动调用一次，无论你创建了多少个实例。
2. **自动调用**：你不需要显式调用静态构造函数。它会在类的静态成员或方法首次被访问时自动执行。
3. **实例化无关**：即使没有实例化类，静态构造函数也会在类的静态成员被访问时调用。

#### 例子

以下是一个 C# 中静态构造函数的示例：

```csharp
using System;

class Example
{
    public static int StaticValue;

    // 静态构造函数
    static Example()
    {
        StaticValue = 42; // 初始化静态字段，但只会被调用一次
        Console.WriteLine("静态构造函数被调用。");
    }

    public Example()
    {
        Console.WriteLine("实例构造函数被调用。");
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine("访问静态成员之前。");
        Console.WriteLine("StaticValue: " + Example.StaticValue); // 触发静态构造函数
        Console.WriteLine("StaticValue: " + Example.StaticValue); // 触发静态构造函数
        Console.WriteLine("访问静态成员之后。");

        Example obj = new Example(); // 触发实例构造函数
    }
}
```

#### 输出

```makefile
访问静态成员之前。
静态构造函数被调用。
StaticValue: 42
StaticValue: 42
访问静态成员之后。
实例构造函数被调用。
```

#### 总结

在这个例子中，静态构造函数在访问 `StaticValue` 之前自动调用，而实例构造函数在创建 `Example` 类的实例时调用。这表明静态构造函数确实在类的实例化之前运行，即使没有创建实例。

### 5. 静态类（Static Classes）

静态类只能包含静态成员，不能创建实例，也就是不能被new。它适用于只包含工具方法的类（如数学运算类）。

```csharp
public static class Utility
{
    public static void PrintMessage(string message)
    {
        Console.WriteLine(message);
    }
}

class Program
{
    static void Main()
    {
        Utility.PrintMessage("Hello, world!"); // 调用静态类的方法
    }
}
```

### 6. 静态操作符（Static Operators）

可以定义静态操作符重载，如重载`+`、`-`等操作符。它们也是类级别的操作，不需要类实例。其实和static没关系，就是需要这样用

```csharp
public class Vector
{
    public int X { get; set; }
    public int Y { get; set; }

    public static Vector operator +(Vector a, Vector b) // 静态操作符
    // 运算符重载必须是静态方法，因为运算符通常在两个操作数之间操作，不依赖于单个实例的状态
    // 并且必须是public的
    {
        return new Vector { X = a.X + b.X, Y = a.Y + b.Y };
    }
}

class Program
{
    static void Main()
    {
        Vector v1 = new Vector { X = 1, Y = 2 };
        Vector v2 = new Vector { X = 3, Y = 4 };
        Vector result = v1 + v2;
        Console.WriteLine($"X: {result.X}, Y: {result.Y}"); // 输出: X: 4, Y: 6
    }
}
```

### 7. 静态成员的特点

- 静态成员只能通过类名访问，不能通过对象实例访问。
- 静态方法和字段在程序的整个生命周期中只会存在一个实例。
- 静态构造函数没有访问修饰符，不能有参数，只能定义一个。

# 18. C#中const和static的区别

在 C# 中，`const` 和 `static` 都与类的成员有关，但它们有不同的用途和特点。以下是它们的主要区别：

### 1. 定义与初始化

- **const**:
  - 用于定义常量，值在编译时确定，并且不可更改。（C++const也是编译期间确定）
  - 必须在声明时初始化。
    ```csharp
    public class Example
    {
        public const int MaxValue = 100; // 必须初始化
    }
    ```
- **static**:
  - 用于定义静态成员，属于类本身而不是类的实例。
  - 可以在定义时或在类的静态构造函数中初始化。
    ```csharp
    public class Example
    {
        public static int Count; // 可以在其他地方初始化

        static Example()
        {
            Count = 0; // 静态构造函数中初始化
        }
    }
    ```

### 2. 值的可变性

- **const**:
  - 一旦定义，值无法改变。
- **static**:
  - 可以在程序运行期间修改静态字段的值。
    ```csharp
    public class Counter
    {
        // 定义静态字段
        public static int Count = 0;

        // 增加计数的方法
        public static void Increment()
        {
            Count++;
        }
    }

    class Program
    {
        static void Main()
        {
            // 输出初始值
            Console.WriteLine("初始计数: " + Counter.Count); // 输出：0

            // 调用静态方法增加计数
            Counter.Increment();
            Counter.Increment();

            // 输出修改后的值
            Console.WriteLine("修改后的计数: " + Counter.Count); // 输出：2
        }
    }
    ```

### 3. 作用域

- **const**:

  - 常量可以是类级别的（static）或实例级别的，通常与类一起使用时，会被视为静态成员。但是C++还是static方法不能访问const常量

    ```csharp
    public class MathConstants
    {
        // 定义一个静态常量
        public const double Pi = 3.14159;

        // 定义一个实例常量
        public const double E = 2.71828;

        // 静态方法访问静态常量
        public static void DisplayConstants()
        {
            Console.WriteLine("Pi: " + Pi); // 访问静态常量
        }

        // 实例方法访问实例常量
        public void DisplayE()
        {
            Console.WriteLine("E: " + E); // 访问实例常量
        }
    }

    class Program
    {
        static void Main()
        {
            // 调用静态方法
            MathConstants.DisplayConstants();
            Console.WriteLine("MathConstants.E: " + MathConstants.E); // 访问实例常量


            // 创建实例并调用实例方法
            MathConstants mathConstants = new MathConstants();
            mathConstants.DisplayE();
        }
    }
    ```

    ```cpp
    #include <iostream>

    class MathConstants {
    public:
    	// 定义静态常量
    	static const double Pi;
    	// 定义实例常量
    	const double E;

    	// 构造函数初始化实例常量
    	MathConstants() : E(2.71828) {}

    	// 静态方法访问静态常量
    	static void DisplayConstants() {
    		std::cout << "Pi: " << Pi << std::endl; // 访问静态常量
    		// std::cout << "E: " << E << std::endl; // 报错，无法访问实例常量
    	}

    	// 实例方法访问实例常量
    	void DisplayE() {
    		std::cout << "E: " << E << std::endl; // 访问实例常量
    	}
    };

    // 静态常量的定义
    const double MathConstants::Pi = 3.14159;

    int main() {
    	// 调用静态方法
    	MathConstants::DisplayConstants();

    	// 创建实例并调用实例方法
    	MathConstants mathConstants;
    	mathConstants.DisplayE();

    	return 0;
    }
    ```
- **static**:

  - 仅与类相关，所有实例共享同一份数据。

### 4. 性能

- **const**:
  - 编译时常量，直接替换为其值，因此在性能上通常更高效。
- **static**:
  - 访问静态成员需要查找类的静态表，相比之下可能稍慢。

### 5. 书写顺序

- **const**:
  - public const
- **static**:
  - public static
  - static public

### 示例

```csharp
public class Example
{
    public const double Pi = 3.14; // 常量
    public static int InstanceCount; // 静态字段

    public Example()
    {
        InstanceCount++;
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine(Example.Pi); // 输出：3.14
        Console.WriteLine(Example.InstanceCount); // 输出：0

        Example ex1 = new Example();
        Example ex2 = new Example();
    
        Console.WriteLine(Example.InstanceCount); // 输出：2
    }
}
```

### `const`和`static readonly`的区别

`const`和`static`不能一起使用，但如果你需要一个可以在运行时初始化并且不允许更改的静态值，可以使用`static readonly`，它允许在静态构造函数中初始化值：

```csharp
public class Example
{
    public static readonly int StaticReadOnlyValue;

    static Example()
    {
        StaticReadOnlyValue = 100; // 可以在静态构造函数中初始化
    }
}
```

### readonly

在C#中，`readonly`关键字用于定义字段，这些字段在运行时初始化后，其值不能被修改。与`const`和`static`结合使用时，`readonly`可以帮助开发者更好地控制数据的不可变性。以下是`readonly`的详细说明及其用法：

#### `readonly`的特点

1. **初始化**：

   - `readonly`字段可以在声明时初始化，或者在类的构造函数中初始化。这意味着你可以在对象创建时设定字段的值。
   - 一旦被赋值（无论是在声明时还是在构造函数中），`readonly`字段就不能再被修改。
2. **运行时可变性**：

   - 与`const`不同，`readonly`字段的值可以在运行时决定（如通过构造函数），而`const`字段的值在编译时确定。
3. **与`static`结合使用**：

   - `readonly`字段可以与`static`结合使用，表示该字段是类级别的，并且在所有实例之间共享，但仍然具有只读特性。

#### 示例

#### 1. 使用`readonly`字段

```csharp
public class Example
{
    // 只读字段
    public readonly int ReadOnlyValue;

    // 构造函数
    public Example(int value)
    {
        ReadOnlyValue = value; // 在构造函数中初始化
    }
}
```

#### 2. 与`static`结合使用

```csharp
public class Example
{
    // 静态只读字段
    public static readonly int StaticReadOnlyValue;

    // 静态构造函数
    static Example()
    {
        StaticReadOnlyValue = 100; // 只在类加载时初始化一次
    }
}

// 使用
public class Program
{
    static void Main()
    {
        Example ex = new Example(10);
        Console.WriteLine(ex.ReadOnlyValue); // 输出: 10
        Console.WriteLine(Example.StaticReadOnlyValue); // 输出: 100
    }
}
```

#### 主要区别

- `const` vs `readonly`：
  - `const`是编译时常量，值在编译时确定并且无法修改。
  - `readonly`允许在运行时根据逻辑进行赋值（如通过构造函数），但赋值后不可修改。
- `static readonly` vs `static`：
  - `static`字段可以被修改，而`static readonly`字段在类的构造完成后就不能被更改。

### 总结

- `const`：编译时常量，隐含静态，值不可更改。用于定义不可更改的常量。
- `readonly`允许在运行时根据逻辑进行赋值（如通过构造函数），但赋值后不可修改。
- `static`：类级成员，运行时初始化，可更改。用于定义与类相关的共享成员，值可以在运行时改变。
- `static readonly`：运行时初始化（可在静态构造函数中设置），但一旦初始化，值不可更改。

# 19. 拓展方法

> 扩展方法（Extension Method）是C#中的一种功能，允许为现有类型（`非静态`）添加新的方法，而无需修改类型本身的源代码或创建子类。扩展方法是通过定义`一个静态类`，其中包含`静态方法`，并在方法的第一个参数前加上 `this` 关键字来实现的。这个第一个参数指定了要扩展的类型。

**示例**：

```csharp
public static class StringExtensions
{
    // 扩展方法: 为string类型添加一个ToWordCount方法，统计单词数量
    // 访问修饰符 static 返回值 函数名(this 扩展类名 参数名, 参数类型 参数名, 参数类型 参数名...)
    public static int ToWordCount(this string str)
    {
        if (string.IsNullOrEmpty(str))
        {
            return 0;
        }
        return str.Split(new char[] { ' ', '.', '?' }, StringSplitOptions.RemoveEmptyEntries).Length;
    }
}

class Program
{
    static void Main()
    {
        string sentence = "Hello, how many words are in this sentence?";
        // 使用扩展方法
        int wordCount = sentence.ToWordCount();
        // The sentence has 8 words.
        Console.WriteLine($"The sentence has {wordCount} words.");
    }
}
```

在这个示例中，`ToWordCount` 方法被定义为 `string` 类型的扩展方法，能够像调用实例方法一样使用扩展方法。扩展方法通常用于增强第三方库或框架中的类，或者在不修改原始类型的情况下添加辅助功能。

扩展方法可以为以下类型添加新方法：

1. **普通类和结构体**：可以为任何已存在的类或结构体（包括 .NET 框架中的类型或自定义的类型）添加扩展方法。例如，为 `string`、`int`、`List<T>` 等常见类型添加扩展方法。类只可以是非静态。
2. **接口**：可以为接口添加扩展方法，这样接口的所有实现类都可以使用该扩展方法。例如，为 `IEnumerable<T>` 添加扩展方法来简化 LINQ 查询。
3. **泛型类型**：可以为泛型类型添加扩展方法。例如，为 `List<T>` 或 `Dictionary<TKey, TValue>` 添加方法来处理泛型数据。
4. **枚举**：可以为枚举类型添加扩展方法，以提供一些便捷的功能，如转换或格式化枚举值。
   **注意**：

- 扩展方法必须定义在一个静态类中。
- 扩展方法不能为静态类添加方法
- 扩展方法本质上是静态方法，但可以像实例方法一样调用。
- 在C#中，如果扩展方法的名字和已有的方法名字一样，原有方法会优先被调用。这是因为扩展方法的解析规则优先级较低，只有当编译器找不到匹配的实例方法时，才会尝试使用扩展方法。
  ```csharp
using System;

namespace ExtensionMethodDemo
{
    // 定义一个普通类
    public class MyClass
    {
        public void PrintMessage()
        {
            Console.WriteLine("这是原始类中的方法");
        }
    }

    // 定义一个扩展类
    public static class MyClassExtensions
    {
        // 定义一个扩展方法，与原有方法同名
        public static void PrintMessage(this MyClass myClass)
        {
            Console.WriteLine("这是扩展方法");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            MyClass myClass = new MyClass();
            myClass.PrintMessage(); // 调用原有方法
            MyClassExtensions.PrintMessage(myClass); // 调用扩展方法
        }
    }
}
  ```

在这个例子中，`MyClass`的实例方法`PrintMessage`优先于扩展方法，因此会输出“这是原始类中的方法”。如果要调用扩展方法，就需要给扩展方法取一个不同的名字，或者避免在类中定义相同名称的方法。- 解决方法

1. **使用不同的名字**：避免扩展方法与原有方法同名。
2. **显式调用扩展方法**：如果确实需要调用扩展方法，可以通过反射或直接使用静态方法调用的方式：
   ```csharp
   MyClassExtensions.PrintMessage(myClass);
   ```

   这样可以绕过实例方法的优先级限制，直接调用扩展方法。

### 扩展方法的第一个参数干嘛的，后面的是参数，那第一个干嘛的

扩展方法的

第一个参数通过`this`关键字来指定要扩展的类型。它定义了该扩展方法应用于哪个类型的对象，`this`后面跟着类型名称，表示这个方法可以像该类型的实例方法一样被调用。

#### 解释

- 第一个参数使用`this`关键字来表示该方法是一个扩展方法，`this`后面的类型是被扩展的类型。
- 扩展方法的第一个参数（`this`后面的参数）是指向被扩展对象的引用。在调用扩展方法时，这个对象将作为参数传递给方法。
- 而更后面的参数是该方法的其他输入参数，类似于普通方法的参数。

#### 举例说明

```csharp
public static class StringExtensions
{
    // 扩展方法，第一个参数是要扩展的类型（string），表示这个方法可以用于string类型的对象
    public static int WordCount(this string str)
    {
        // 使用扩展的字符串对象
        return str.Split(new char[] { ' ', '.', '?' }, StringSplitOptions.RemoveEmptyEntries).Length;
    }
}

// 使用扩展方法
string sentence = "Hello world, this is an extension method example.";
int count = sentence.WordCount();  // 相当于 StringExtensions.WordCount(sentence)
Console.WriteLine(count);  // 输出: 7
```

在这个例子中：

1. `WordCount`方法的第一个参数是`this string str`，表示这个方法是用来扩展`string`类型的对象的。
2. 当`sentence.WordCount()`被调用时，`sentence`对象被传递给`str`参数，相当于执行了`StringExtensions.WordCount(sentence)`。

因此，扩展方法的第一个参数`this`后面的类型就是要扩展的类型，指定了这个方法适用于哪些对象，并使其能够像实例方法一样调用。而str则表示什么样的对象调用了这个方法，可以对调用的对象进行操作

### 为普通类和结构体、接口、泛型类型、枚举类型编写扩展方法的示例

### 1. 普通类和结构体的扩展方法

#### 扩展普通类

```csharp
using System;

// 普通类
public class Person
{
    public string Name { get; set; }
}

// 扩展方法类
public static class PersonExtensions
{
    public static void Greet(this Person person)
    {
        Console.WriteLine($"Hello, {person.Name}!");
    }
}

class Program
{
    static void Main()
    {
        // 使用扩展方法
        var person = new Person { Name = "Alice" };
        person.Greet();  // 输出: Hello, Alice!
    }
}

```

#### 扩展结构体

```csharp
// 结构体
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }
}

// 扩展方法类
public static class PointExtensions
{
    public static double DistanceToOrigin(this Point point)
    {
        return Math.Sqrt(point.X * point.X + point.Y * point.Y);
    }
}

class Program
{
    static void Main()
    {
        // 使用扩展方法
        var point = new Point { X = 3, Y = 4 };
        Console.WriteLine(point.DistanceToOrigin());  // 输出: 5
    }
}
```

### 2. 接口的扩展方法

```csharp
// 接口
public interface IShape
{
    double GetArea();
}

// 扩展方法类
public static class ShapeExtensions
{
    public static void PrintArea(this IShape shape)
    {
        Console.WriteLine($"The area is {shape.GetArea()}");
    }
}

// 实现接口的类，这里扩展方法可以不用重写了
public class Circle : IShape
{
    public double Radius { get; set; }
    public double GetArea()
    {
        return Math.PI * Radius * Radius;
    }
}

class Program
{
    static void Main()
    {
        // 使用扩展方法
        IShape circle = new Circle { Radius = 5 };
        circle.PrintArea();  // 输出: The area is 78.5398163397448
    }
}
```

### 3. 泛型类型的扩展方法

- 感觉这个泛型很好用了啊

```csharp
// 泛型类型的扩展方法
public static class GenericExtensions
{
    public static void PrintTypeName<T>(this T obj)
    {
        Console.WriteLine($"The type of the object is {typeof(T).Name}");
    }
}

class Program
{
    static void Main()
    {
        // 使用扩展方法
        int number = 42;
        number.PrintTypeName();  // 输出: The type of the object is Int32

        string text = "Hello";
        text.PrintTypeName();    // 输出: The type of the object is String
    }
}
```

### 4. 枚举的扩展方法

```csharp
// 枚举
public enum DayOfWeek
{
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}

// 扩展方法类
public static class DayOfWeekExtensions
{
    public static bool IsWeekend(this DayOfWeek day)
    {
        return day == DayOfWeek.Saturday || day == DayOfWeek.Sunday;
    }
}

class Program
{
    static void Main()
    {
        // 使用扩展方法
        DayOfWeek today = DayOfWeek.Saturday;
        Console.WriteLine(today.IsWeekend());  // 输出: True
    }
}
```

这些示例展示了如何为普通类、结构体、接口、泛型类型和枚举编写扩展方法。每种类型的扩展方法通过`this`关键字为目标类型增加新功能，方便开发和代码的可读性。

# 20. C#的泛型和C++的模板有什么区别呢

C#的泛型和C++的模板虽然看起来有些相似，但它们的工作方式和设计初衷存在一些关键区别。

### 1. **编译机制**

- **C++ 模板**
  - C++的模板是在编译期间进行代码的生成，称为模板实例化。模板是基于文本替换的宏机制，编译器会根据使用模板时传递的类型生成特定类型的代码。这种方式导致代码膨胀（代码冗余）。
  - 模板允许类型参数进行特殊化（模板特化），可以为某些特定类型实现不同的模板行为。
- **C# 泛型**
  - C#的泛型在编译期间会被编译成中间语言（IL），并在运行时通过JIT（即时编译器）生成类型安全的代码。编译生成的泛型代码只需要一份，所有的具体类型共享相同的代码，这样可以减少代码的膨胀。
  - C# 泛型没有模板特化的概念。泛型的类型参数只能在运行时提供，所有类型参数在使用时都必须满足编译时的类型约束。

### 2. **类型安全**

- **C++ 模板**
  - 由于C++模板是基于代码替换的宏机制，编译器会根据模板的具体使用来生成代码，在编译期间进行类型检查。因此，如果模板代码不符合类型要求，会在模板实例化时出现编译错误。
- **C# 泛型**
  - C#的泛型提供类型安全的机制，允许在定义泛型时指定类型约束（如`where T : class`）。编译器在编译期间会检查这些约束，确保类型的安全性。

### 3. **运行时与编译时的行为**

- **C++ 模板**
  - 模板实例化是在编译时完成的。编译器为每种使用的模板参数生成独立的代码，因此没有额外的运行时开销。
  - 模板的元编程能力强大，可以实现复杂的编译时计算和类型变换。
- **C# 泛型**
  - C#的泛型是在运行时通过JIT进行类型参数化，允许在运行时决定使用的类型。
  - 泛型无法在编译时进行元编程计算，只能使用运行时的反射等方式来进行类似的操作。

### 4. **模板特化与泛型约束**

- **C++ 模板**
  - 支持模板特化，可以为某些特定类型定义不同的实现。
  - 模板特化分为全特化和偏特化，灵活性更高。
- **C# 泛型**
  - 不支持模板特化，但支持泛型约束，可以用`where`关键字为类型参数指定一定的约束条件，如必须实现某接口或继承某基类。

### 5. **编译器错误信息**

- **C++ 模板**
  - 由于模板的复杂性和模板元编程的灵活性，模板相关的编译器错误通常非常冗长且难以理解。
- **C# 泛型**
  - 由于C#的泛型设计简单，错误信息相对直观。泛型约束的限制也使得类型安全检查更加明确。

### 总结

- C++模板是基于编译期的宏替换机制，支持编译期的元编程和模板特化，灵活性高，但可能会带来代码膨胀。
- C#的泛型是类型安全的运行时机制，支持类型约束，减少了代码冗余，更易于使用，但不具备C++模板的编译期元编程能力。

两者的设计目的是不同的：C++模板主要为了性能和灵活性，而C#泛型更注重类型安全和代码重用性。

# 21. 什么叫元编程

元编程（Metaprogramming）是一种编程技术，在这种技术中，程序可以将其他程序作为数据进行操作、分析或生成。换句话说，`元编程的目标是编写能够生成、修改或编译代码的代码`。它能够提升代码的灵活性、可重用性和性能。元编程主要体现在以下几个方面：

### 1. **代码生成**

- 通过编写元程序来自动生成某些重复性较强或复杂的代码，从而减少手工编写的工作量。常见的例子有代码生成器和宏。

### 2. **编译期计算**

- 元编程允许在编译时执行计算。这使得可以利用编译器在编译阶段执行某些逻辑，从而优化程序运行时的性能。例如，C++的模板元编程可以在编译期计算出一些结果，而不是在运行时执行。

### 3. **反射和动态类型**

- 一些编程语言（如Python、Java、C#）支持在运行时检查和操作类型信息，这种机制称为反射。通过反射，程序可以动态地创建和修改对象、调用方法或访问属性。

### 4. **宏和预处理器**

- 在C和C++等语言中，预处理器宏是一种简单的元编程工具。它们在编译前处理源代码，用于定义常量、条件编译、代码替换等。

### 5. **泛型编程**

- 泛型编程是一种特殊的元编程形式，允许编写适用于多种数据类型的代码。例如，C++中的模板和C#中的泛型使得函数和类可以针对任意类型编写。

### 6. **模板元编程（Template Metaprogramming）**

- C++中广泛应用的元编程技术，允许在编译期通过模板实现复杂的逻辑推导和代码生成。例如，可以利用模板递归计算阶乘或实现编译期类型检查。

### 7. **编译器插件和自定义编译器**

- 一些语言和编译器允许开发者编写插件或自定义编译器来进行元编程，从而可以在编译过程中自动进行代码优化、生成和修改。

### 8. **领域特定语言（DSL）**

- 元编程有时还用于创建领域特定语言，这些语言是针对某个特定领域设计的，可以通过扩展已有的语言或创建新的语法来简化编程。

### 元编程的优缺点

**优点：**

- **减少代码重复性**：通过生成代码或编写通用的模板，可以大大减少重复性代码。
- **提升性能**：利用编译期计算和优化，可以生成更高效的代码。
- **增强灵活性**：程序可以根据不同的条件动态生成或调整代码，增加程序的适应性。
  **缺点：**
- **复杂性**：元编程的代码往往较为复杂，难以理解和维护。
- **编译时间**：编译期元编程可能会显著增加编译时间，尤其是在复杂的模板元编程中。
- **调试困难**：由于元编程代码是在编译期生成的，调试错误会更加困难。

### 举例：C++模板元编程示例

在C++中，可以使用模板元编程来计算编译期的阶乘：

```cpp
#include <iostream>

// 模板结构体，用于计算阶乘
template <int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value; // 递归计算阶乘
};

// 模板特化，用于终止递归
template <>
struct Factorial<0> {
    static const int value = 1; // 0! = 1
};

int main() {
    // 输出5的阶乘
    std::cout << "Factorial of 5: " << Factorial<5>::value << std::endl;
    return 0;
}

```

在这个例子中，`Factorial<5>::value`在编译期被计算为120，不需要运行时进行计算。

#### 解释

1. **模板递归定义**：
   - `template <int N> struct Factorial` 定义了一个模板结构体，用来计算整数 `N` 的阶乘。
   - `static const int value = N * Factorial<N - 1>::value;` 这一行表示 `Factorial<N>::value` 等于 `N` 乘以 `Factorial<N - 1>::value`。这是一种递归定义，类似于数学上的阶乘定义：`N! = N * (N-1)!`。
2. **模板特化**：
   - `template <> struct Factorial<0>` 是一个模板特化，用来终止递归计算。它指定当 `N` 为 0 时，`Factorial<0>::value` 等于 1。
3. **使用 `Factorial<5>::value` 进行取值**：
   - `Factorial<5>::value` 的含义是通过模板递归计算 5 的阶乘，即 `5 * 4 * 3 * 2 * 1 = 120`。
   - `::value` 是用来访问 `Factorial` 结构体中的 `value` 成员，它保存了计算结果。

#### 运行结果

输出为：

```mathematica
Factorial of 5: 120
```

这段代码使用模板元编程在编译时计算阶乘，`Factorial<5>::value` 中的 `::value` 是访问结构体 `Factorial` 的静态成员变量，用于获取计算结果。

#### 总结

元编程可以提高代码的灵活性和效率，通过编写“生成代码的代码”，解决某些问题变得更加优雅和高效。然而，由于它增加了程序的复杂性，使用时需要权衡和谨慎。

[MS-Learning](https://learn.microsoft.com/zh-cn/)
