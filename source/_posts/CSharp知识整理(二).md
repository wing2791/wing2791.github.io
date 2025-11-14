---
title: CSharp知识整理(二)
math: true
---
# 22. 运算符重载
  在C#中，运算符重载允许为用户自定义的类型（如类或结构体）定义或重载运算符的行为。这使得用户可以对自定义类型使用标准的运算符（如 `+`, `-`, `*`, `/`, `相等`, `!=` 等），就像对内置类型操作一样。
### 运算符重载的基本规则
1. **必须是 `static` 方法**：重载的运算符方法必须声明为静态的。
2. **通常为 `public` 访问修饰符**：以便在外部使用。
3. **参数数量**：一元运算符（如 `++`, `--`, `!`）只有一个参数，二元运算符（如 `+`, `-`, `*`, `/`）只有两个参数。
4. **返回类型**：可以是任何类型，不一定要与操作数类型相同。
5. 运算符的参数必须是包含当前类的类型
6. 不能使用ref和out修饰参数，ref和out参数在此上下文中无效

### 重载一元运算符的示例
一元运算符重载允许你为自定义类型定义操作符（如 `++`、`--`、`-`、`!` 等）的行为。与二元运算符重载相似，在C#中，一元运算符的重载也必须是静态方法，且通常只需要一个参数，且该参数通常是包含运算符重载方法的类型。
- C#中的++和--重载部分前缀和后缀，具体看下面的第二个例子
- 不管前置后置运算，程序只执行唯一的重载运算符函数，其实C#不分前缀和后缀的写法，和Cpp不一样。C#对++/--运算符重载的要求：
	- 参数必须是一个，而且只能是包含它的类型，也就是它的所在类类型
	- 返回值，必须是所在类类型或其派生类
	- 所有重载的运算符必须是 public 和 static的，这是编译时要求
#### 示例
以下是一个示例，展示如何重载一元运算符 `++` 和 `--`，以及负号运算符 `-`：
```csharp
using System;

public class Counter
{
    public int Value { get; set; }

	// 不可以用ref或者out修饰int
    public Counter(int initialValue)
    {
        Value = initialValue;
    }

    // 重载前缀 ++ 运算符
    public static Counter operator ++(Counter c)
    {
        c.Value++;
        return c;
    }


    // 重载前缀 -- 运算符
    public static Counter operator --(Counter c)
    {
        c.Value--;
        return c;
    }

    // 重载负号运算符
    public static Counter operator -(Counter c)
    {
        return new Counter(-c.Value);
    }

    // 重写 ToString 方法，以便输出格式化的计数器值
    public override string ToString()
    {
        return Value.ToString();
    }
}

class Program
{
    static void Main()
    {
        Counter counter = new Counter(5);

        // 使用重载的 ++ 运算符
        Console.WriteLine($"Initial Value: {counter}");
        Console.WriteLine($"After Increment: {++counter}");
        Console.WriteLine($"After Decrement: {--counter}");

        // 使用重载的负号运算符
        Counter negativeCounter = -counter;
        Console.WriteLine($"Negative Value: {negativeCounter}");
    }
}
```

- 重载`++`和`--`的另一个例子
```csharp
using System;

public class Counter
{
    public int Value { get; set; }

    public Counter() { }

    public Counter(int initialValue)
    {
        Value = initialValue;
    }

    // 重载前缀和后缀 ++ 运算符
    public static Counter operator ++(Counter c)
    {
        Console.WriteLine("operator++");
        return new Counter(1 + c.Value);
    }

    // 重载前缀和后缀 -- 运算符
    public static Counter operator --(Counter c)
    {
        Console.WriteLine("operator--");

        return new Counter(c.Value - 1);
    }

    // 重载负号运算符
    public static Counter operator -(Counter c)
    {
        return new Counter(-c.Value);
    }

    // 重写 ToString 方法，以便输出格式化的计数器值
    public override string ToString()
    {
        return Value.ToString();
    }
}

class Program
{
    static void Main()
    {
        Counter counter = new Counter(5);

        // 使用重载的 ++ 和 -- 运算符
        Console.WriteLine($"Initial Value: {counter}");
        Console.WriteLine($"After Prefix Increment: {++counter.Value}");
        Console.WriteLine($"After Prefix Value: {counter}");

        Console.WriteLine($"After Postfix Increment: {(counter++).Value}");
        // 上面一行等价于下面两行，不一定是+=1，实际看返回的new Counter(1 + c.Value);是怎么操作的，也可以+2
        // Console.WriteLine($"After Postfix Increment: {counter.Value}"); // 输出当前值
        // counter.Value += 1; // 实际上增1操作是直接对原对象进行修改的
        Console.WriteLine($"After Postfix Value: {counter}");

        Console.WriteLine($"After Prefix Decrement: {--counter.Value}");
        Console.WriteLine($"After Prefix Value: {counter}");

        Console.WriteLine($"After Postfix Decrement: {(counter--).Value}");
        // 上面一行等价于下面两行，不一定是-=1，实际看返回的new Counter(c.Value - 1);是怎么操作的，也可以-2
        // Console.WriteLine($"After Postfix Increment: {counter.Value}"); // 输出当前值
        // counter.Value -= 1; // 实际上增1操作是直接对原对象进行修改的
        // 查看最终计数器值
        Console.WriteLine($"After Postfix Value: {counter}");

        // 使用重载的负号运算符
        Counter negativeCounter = -counter;
        Console.WriteLine($"Negative Value: {negativeCounter}");
    }
}
// 结果
//Initial Value: 5
//After Prefix Increment: 6
//After Prefix Value: 6
//operator ++
//After Postfix Increment: 6
//After Postfix Value: 7
//After Prefix Decrement: 6
//After Prefix Value: 6
//operator--
//After Postfix Decrement: 6
//After Postfix Value: 5
//Negative Value: -5
```

#### 注意事项
1. **访问修饰符**：重载运算符的方法必须是 `public` 和 `static`。
2. **返回类型**：重载运算符的方法返回一个新对象或修改后的对象。
3. 一元运算符的参数必须是包含类型


### 重载二元运算符的示例
以下是一个重载加法运算符的示例：
```csharp
using System;

public class ComplexNumber
{
    public double Real { get; set; }
    public double Imaginary { get; set; }

    public ComplexNumber(double real, double imaginary)
    {
        Real = real;
        Imaginary = imaginary;
    }

    // 重载加法运算符
    public static ComplexNumber operator +(ComplexNumber c1, ComplexNumber c2)
    {
        return new ComplexNumber(c1.Real + c2.Real, c1.Imaginary + c2.Imaginary);
    }

    // 重写 ToString 方法，以便输出格式化的复数
    public override string ToString()
    {
        return $"{Real} + {Imaginary}i";
    }
}

class Program
{
    static void Main()
    {
        ComplexNumber c1 = new ComplexNumber(1.5, 2.5);
        ComplexNumber c2 = new ComplexNumber(3.0, 4.0);
        ComplexNumber sum = c1 + c2; // 使用重载的加法运算符

        Console.WriteLine($"c1 + c2 = {sum}");
    }
}
```

#### 示例说明
- `ComplexNumber` 类表示一个复数，包含两个字段：`Real` 和 `Imaginary`。
- 加法运算符 `+` 被重载，使得可以对两个 `ComplexNumber` 实例进行相加操作。
- `operator +` 方法返回一个新的 `ComplexNumber` 对象，其 `Real` 和 `Imaginary` 分别是两个操作数相加的结果。
- 在 `Main` 方法中，`c1` 和 `c2` 被相加，并输出相加的结果。
### 支持重载的运算符列表
C# 支持重载的运算符包括：
- 算术运算符：`+`, `-`, `*`, `/`, `%`
- 比较运算符：`相等`, `!=`, `<`, `>`, `<=`, `>=`
- 位运算符：`&`, `|`, `^`, `~`, `<<`, `>>`
- 逻辑运算符：`&&`, `||`, `!`
- 自增、自减运算符：`++`, `--`

### 不支持重载的运算符列表
- **成员访问运算符**：`.`（点运算符）
    - 用于访问类型的成员（字段、属性、方法等）。
- **条件运算符**：`?:`（三元运算符）
    - 用于条件表达式。
- **委托运算符**：`delegate`
    - 用于声明委托。
- **指针运算符**：`*`（指针解引用运算符）和`&`（地址运算符）
    - 用于处理指针。
- **类型测试运算符**：`is` 和 `as`
    - 用于检查对象类型和进行类型转换。
- **数组下标运算符**：`[]`
    - 用于数组索引访问。
- **new 关键字**
    - 用于对象的创建。
- **default 关键字**
    - 用于获取类型的默认值。
- **sizeof 关键字**
    - 用于获取类型的大小。
- 赋值符号=
- 强转运算符()


### 重载运算符的注意事项
- 条件运算符需要成对实现
	- 如果重载 `相等` 运算符，通常也需要重载 `!=` 运算符。
	- 如果重载比较运算符（如 `<` 和 `>`），还需要重载其相应的 `<=` 和 `>=` 运算符。
- 二元运算符的参数之一必须是包含类型
- 为了更好地实现运算符的语义，一般还需要重写 `Equals()` 和 `GetHashCode()` 方法。
	- 1. `Equals()` 方法
		`Equals()` 用于比较两个对象是否相等。它通常用于判断两个对象的内容是否相等，而不仅仅是它们是否是同一个对象（即是否是同一个内存地址）。
		- **重写场景：** 当你定义了一个自定义类型并且想要比较两个实例的内容是否相等时，就需要重写 `Equals()` 方法。默认的 `Equals()` 比较的是引用相等性（即对象的地址），但是如果你希望比较的是对象的实际内容（例如 `Counter` 类中的 `Value`），就需要重写它。
		    **重写 `Equals()` 时应遵循以下规则：**
		    - 自反性：`a.Equals(b)` 和 `b.Equals(a)` 都应返回相同的结果。
		    - 对称性：如果 `a.Equals(b)` 返回 `true`，那么 `b.Equals(a)` 也应返回 `true`。
		    - 传递性：如果 `a.Equals(b)` 返回 `true` 且 `b.Equals(c)` 返回 `true`，那么 `a.Equals(c)` 应该返回 `true`。
		    - 一致性：多次调用相同的对象对比应始终返回相同的结果，除非对象的状态改变。
	
	- 2. `GetHashCode()` 方法
		`GetHashCode()` 方法用于为对象提供一个哈希值，哈希值通常用于哈希表（如 `Dictionary` 或 `HashSet`）中，以便在查找、插入或删除时快速定位对象。哈希值应该尽量均匀地分布，以减少冲突。
		- **重写场景：** 当你重写 `Equals()` 方法时，通常也需要重写 `GetHashCode()` 方法，以确保相等的对象有相同的哈希值。这样才能确保在哈希集合（如 `HashSet` 或 `Dictionary`）中，比较相等的对象能够正确地存储和查找。
		    **重写 `GetHashCode()` 时的一些建议：**
		    - 如果两个对象相等（通过 `Equals()` 判断），那么它们的哈希值必须相同。
		    - 如果两个对象不相等，它们的哈希值不必不同，但尽量避免大量哈希冲突，以提高性能。
		    - `GetHashCode()` 生成的哈希值应该基于对象的“值”，而不是其引用地址。
		- 为什么重写这两个方法
			在许多场景中，特别是在使用集合类（如 `Dictionary`、`HashSet`）时，`Equals()` 和 `GetHashCode()` 起着关键作用。集合会利用哈希值来高效地存储和查找对象，因此正确地重写这两个方法能确保自定义类型在这些集合中的正常使用。
		- 示例：
			假设你有一个 `Counter` 类，你想要通过值来判断两个计数器是否相等，并将它们作为键存储在 `Dictionary` 中。你需要重写 `Equals()` 和 `GetHashCode()` 方法。
			```csharp
			public class Counter
			{
				public int Value { get; set; }
			
				public Counter(int initialValue)
				{
					Value = initialValue;
				}
			
				// 重载 Equals 方法
				public override bool Equals(object obj)
				{
					if (obj is Counter otherCounter)
					{
						return this.Value == otherCounter.Value;
					}
					return false;
				}
			
				// 重载 GetHashCode 方法
				public override int GetHashCode()
				{
					return Value.GetHashCode();
				}
			}
			
			class Program
			{
				static void Main()
				{
					var dict = new Dictionary<Counter, string>();
					var counter1 = new Counter(5);
					var counter2 = new Counter(5);
			
					dict[counter1] = "First";
					Console.WriteLine(dict[counter2]); // 输出 "First"，因为 counter1 和 counter2 的值相等，且哈希值相同
				}
			}
			```
			在这个例子中，如果没有重写 `Equals()` 和 `GetHashCode()`，`counter1` 和 `counter2` 将被认为是不同的对象（基于引用），即使它们的 `Value` 相同。但通过重写这两个方法，我们确保了它们在比较时是基于 `Value` 属性的。
			运算符重载使自定义类型在运算时可以更符合直观的数学表示，提高代码的可读性和易用性。


### 强转运算符（类型转换运算符）特定的重载方式
- **类型转换运算符**：C# 允许你定义类型转换运算符，但不是通过传统的运算符重载语法。你可以通过定义显式或隐式转换运算符来实现类型转换的行为。
#### 示例
```csharp
public class MyClass
{
    public int Value { get; set; }

    // 显式转换运算符，如果隐式使用，会报错
    public static explicit operator MyClass(int value)
    {
        return new MyClass { Value = value };
    }

    // 隐式转换运算符,如果显式的使用，不会报错
    public static implicit operator int(MyClass myClass)
    {
        return myClass.Value;
    }
}

// 使用示例
class Program
{
    static void Main(string[] args)
    {
        MyClass myObject = (MyClass)5; // 显式转换
        // MyClass myObject2 = 5; // 隐式转换，报错
        int number = myObject;          // 隐式转换
        int number2 = (int)myObject;          // 显式转换
    }
}
```


# 23. explicit implicit
在C#中，`explicit` 和 `implicit` 是用来定义类型转换运算符的关键字，它们决定了类型转换的方式和使用场景。下面是对这两个关键字的详细解释以及它们的区别。

### 1. `implicit`（隐式转换）

- **定义**：隐式转换运算符允许自动转换类型，无需显式地进行类型转换。
- **使用场景**：当你希望一个类型可以被安全地转换为另一个类型，并且这种转换不可能导致数据丢失或异常时，可以使用隐式转换。
#### 示例
```csharp
public class MyClass
{
    public int Value { get; set; }

    // 隐式转换运算符
    public static implicit operator MyClass(int value)
    {
        return new MyClass { Value = value };
    }
}

// 使用示例
class Program
{
    static void Main(string[] args)
    {
        MyClass myObject = 10; // 隐式转换
        Console.WriteLine(myObject.Value); // 输出: 10
    }
}
```
### 2. `explicit`（显式转换）
- **定义**：显式转换运算符需要使用强制类型转换符进行转换，不能隐式转换。
- **使用场景**：当你希望转换可能导致数据丢失或需要显式指明时，可以使用显式转换。这样可以防止意外的类型转换。
#### 示例
```csharp
public class MyClass
{
    public int Value { get; set; }

    // 显式转换运算符
    public static explicit operator int(MyClass myClass)
    {
        return myClass.Value;
    }
}

// 使用示例
class Program
{
    static void Main(string[] args)
    {
        MyClass myObject = new MyClass { Value = 10 };
        int number = (int)myObject; // 显式转换
        Console.WriteLine(number); // 输出: 10
    }
}
```
### 总结

- `implicit`：允许类型自动转换，适用于安全且无损的转换。
- `explicit`：要求使用强制转换，适用于可能导致数据丢失或不安全的转换。




# 24. 内部类
在C#中，内部类是定义在另一个类中的类，也称为嵌套类。内部类可以用来组织代码，使外部类更紧凑，同时为外部类提供额外的功能。以下是关于C#内部类的一些关键点：
### 1. 内部类的定义
内部类可以定义在外部类的任何地方（类体内），并且可以访问外部类的私有成员。内部类本身也可以有自己的成员和方法。
#### 示例
```csharp
public class OuterClass
{
    private int outerValue = 42;

    // 定义一个内部类
    public class InnerClass
    {
        public void Display()
        {
            Console.WriteLine("这是内部类的方法.");
        }
    }

    // 外部类的方法可以创建内部类的实例
    public void CreateInner()
    {
        InnerClass inner = new InnerClass();
        inner.Display();
    }
}
```
### 2. 访问外部类的成员
内部类可以访问外部类的所有成员，包括私有成员。
- C#中的内部类要访问外部类的成员（包括私有成员），需要持有外部类的实例的引用。这是因为内部类与外部类是两个不同的对象，要访问外部类的成员，必须通过外部类的实例进行访问。通常，可以通过构造函数传递外部类的实例，然后使用该实例访问外部类的成员。
#### 示例
```csharp
public class OuterClass
{
    private int outerValue = 42;

    // 定义一个内部类
    public class InnerClass
    {
        private OuterClass outer;
        public int pubInt;

        // 构造函数接收外部类实例
        public InnerClass(OuterClass outer)
        {
            this.outer = outer;
        }

        public void DisplayOuterValue()
        {
            // 访问外部类的私有字段
            // 访问外部类的私有字段,必须使用接收的实例才能够使用其中的变量，不然无法直接使用outerValue
            Console.WriteLine("外部类的值是: " + outer.outerValue);
        }

        // 给外部类提供访问内部类的公共方法
        public void DisplayInnerInfo()
        {
            Console.WriteLine("这是内部类的方法");
        }
    }

    public void CreateInner()
    {
        InnerClass inner = new InnerClass(this);
        inner.DisplayOuterValue();
        inner.DisplayInnerInfo();  // 外部类可以访问内部类的公共方法
        inner.pubInt = 42;
        Console.WriteLine($"内部类的pubInt的值是:{inner.pubInt}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // 内部类可以访问外部类的私有成员，不过也必须传入外部类到构造函数
        // 外部类无法直接访问内部类的私有成员，但是可以通过公共方法或者属性来访问。
        OuterClass outer = new OuterClass();
        outer.CreateInner();
    }
}
```
### 3. 内部类的访问修饰符
内部类可以具有自己的访问修饰符，比如`public`、`private`、`protected`、`internal`等，这决定了内部类在外部类之外的可见性。
- `private`：内部类只能被外部类访问。
- `public`：内部类可以被外部类之外的代码访问。
- `protected`：内部类只能被外部类及其派生类访问。
- `internal`：内部类可以被同一个程序集中的其他代码访问。程序集的理解就是一个解决方案中的项目
	- 在C#中，“程序集”（Assembly）是指一个已编译的代码库，通常是一个 `.exe` 或 `.dll` 文件。它是 .NET 中的基本部署单元，包含可执行代码、资源文件和元数据。
	- 元数据的意思就是描述数据的数据
	- [![pAwb64s.png](https://s21.ax1x.com/2024/10/26/pAwb64s.png)](https://imgse.com/i/pAwb64s)

### 4. 使用场景
- **封装相关功能**：内部类可以作为外部类的辅助类，帮助封装某些复杂的功能。
- **访问控制**：通过嵌套类的访问修饰符，可以严格控制类的访问权限。
- **代码组织**：将相关的类组织在一起，使代码更整洁。
### 5. 示例：使用访问修饰符
```csharp
public class OuterClass
{
    private int outerValue = 42;

    // 私有内部类
    private class InnerClass
    {
        public void Display()
        {
            Console.WriteLine("这是私有的内部类.");
        }
    }

    public void CreateInner()
    {
        InnerClass inner = new InnerClass();
        inner.Display();
    }
}
```
在这个例子中，`InnerClass`是`private`的，因此只能在`OuterClass`中使用，不能在外部类之外的地方访问。
### 总结
- **内部类**用于在类中嵌套另一个类，具有封装性和灵活性。
- 可以通过访问修饰符控制内部类的访问权限。
- 内部类可以访问外部类的所有成员，包括私有成员。


# 25. OuterClass outer = new OuterClass();和OuterClass outer2;区别
```csharp
public class OuterClass
{
    private int outerValue = 42;

    // 定义一个内部类
    public class InnerClass
    {
        private OuterClass outer;

        // 构造函数接收外部类实例
        public InnerClass(OuterClass outer)
        {
            this.outer = outer;
        }

        public void DisplayOuterValue()
        {
            // 访问外部类的私有字段
            Console.WriteLine("外部类的值是: " + outer.outerValue);
        }
    }

    public void CreateInner()
    {
        InnerClass inner = new InnerClass(this);
        inner.DisplayOuterValue();
    }
}


public class Program
{
    public static void Main()
    {
        OuterClass outer = new OuterClass();
        OuterClass outer2;
        outer.CreateInner();
        // outer2.CreateInner(); //CS0165 使用了未赋值的局部变量"outer2"
    }
}
```
在C#中，这两个声明的确有区别：
1. `OuterClass outer = new OuterClass();`
    - 这里声明了一个名为 `outer` 的变量，并且通过 `new OuterClass()` 创建了一个 `OuterClass` 类型的实例，然后将这个实例的引用赋值给变量 `outer`。
    - 这样，`outer` 就指向了一个已经被实例化的对象，可以访问对象的成员。
2. `OuterClass outer2;`
    - 这里声明了一个名为 `outer2` 的变量，但没有初始化它，即没有创建对象实例。
    - 此时，`outer2` 只是一个未赋值的变量，如果尝试使用它（如访问成员），会导致编译错误，因为变量尚未初始化。
### 总结
- `OuterClass outer = new OuterClass();`：创建了一个 `OuterClass` 对象，并将其赋值给 `outer`。
- `OuterClass outer2;`：仅声明了变量 `outer2`，但并未指向任何对象。使用前需要先进行初始化，例如：`outer2 = new OuterClass();`。
```cpp
#include <iostream>
using namespace std;

class Test {
public:
	void show() {
		cout << "test:show" << endl;
	}
};

int main() {
	Test* test = new Test();
	Test test2;
	test->show();
	test2.show();

	return 0;
}
```
在C++中，这两种声明方式有明显的区别：
1. `Test* test = new Test();`
    - 这会创建一个指向`Test`对象的指针，并且该对象是在堆上分配的。使用`new`关键字会动态分配内存，这时需要手动管理内存，在合适的时候使用`delete test`释放内存，否则会导致内存泄漏。
2. `Test test2;`
    - 这会在栈上创建一个`Test`对象实例。当该对象的作用域结束时，会自动销毁，不需要手动管理内存。
第一种方式适用于需要在当前作用域之外持续存在的对象，而第二种方式在对象生命周期较短时效率更高。

# 26. 分部类
在C#中，**分部类（Partial Class）** 允许将一个类的定义分散到多个文件中。这在大型项目中很有用，特别是当多个开发者需要同时对同一个类进行修改，或者自动生成的代码和手写的代码需要分开时。
### 关键点：
1. **使用 `partial` 关键字**：分部类的定义需要使用 `partial` 关键字来标识。每个部分都必须使用 `partial` 声明，并且具有相同的类名。
2. **文件位于同一个命名空间下**：分部类的各部分可以在不同的文件中定义，但它们必须在同一个命名空间下。
3. **编译时合并**：在编译时，所有分部类会被合并为一个完整的类。因此，分部类的所有方法、属性和成员变量在使用时是共享的。
4. **访问修饰符一致性**：分部类的各个部分可以有不同的访问修饰符（如 `public`、`internal`），但最终编译后的类只能具有一种可见性。
5. 分部类中不能有重复成员，可以重载

### 示例：

文件 `Person1.cs`：
```csharp
// Person1.cs
public partial class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    public void PrintName()
    {
        Console.WriteLine($"{FirstName} {LastName}");
    }
}
```

文件 `Person2.cs`：

```csharp
// Person2.cs
public partial class Person
{
    public int Age { get; set; }

    public void PrintAge()
    {
        Console.WriteLine($"Age: {Age}");
    }
}
```

在上述示例中，`Person` 类被拆分到了两个文件 `Person1.cs` 和 `Person2.cs` 中。在编译时，这两个部分会被合并成一个完整的 `Person` 类，拥有所有的属性和方法。
### 分部类的用途：
- **自动生成代码**：比如在一些工具（如Visual Studio设计器或Entity Framework）中，自动生成的代码和用户手写的代码可以放在不同的分部类中，方便管理。
- **模块化开发**：团队开发时，可以将一个类分割成不同部分，方便不同开发人员同时修改。
- **代码组织**：对于大类，可以将不同功能放在不同的文件中，提高代码的可读性和可维护性。
### 注意
在C#的分部类中，**访问修饰符一致性**的意思是：虽然你可以在不同的分部类定义中使用不同的访问修饰符（如 `public` 或 `internal`），但是编译器最终会将所有部分合并为一个完整的类，这个类只能有一个最终的可见性（即访问级别）。

具体来说，编译器会根据以下规则来确定最终的访问修饰符：
1. **如果所有分部类都指定了相同的访问修饰符**，那么最终类的访问修饰符就是这个一致的修饰符。例如，如果所有部分都是 `public`，那么合并后的类也是 `public`。
2. **如果有的部分指定了访问修饰符，而有的没有指定**，编译器会选择访问级别最高的修饰符。例如，如果一个部分是 `public`，而另一个部分没有指定修饰符（默认是 `internal`），那么最终类的可见性将是 `public`。
3. **如果不同部分的修饰符不一致**，并且存在冲突（如一个部分是 `public`，另一个部分是 `internal`），编译器会报错，因为访问修饰符必须在所有部分中一致，或者必须选择可以合并的修饰符。
#### 示例：
文件 `Person1.cs`：
```csharp
// Person1.cs
public partial class Person
{
    public string FirstName { get; set; }
}
```
文件 `Person2.cs`：
```csharp
// Person2.cs
internal partial class Person
{
    public int Age { get; set; }
}
```
在这个示例中，`Person1.cs` 中的 `Person` 类是 `public` 的，而 `Person2.cs` 中的 `Person` 类是 `internal` 的。这会导致编译错误，因为编译器不知道该选择 `public` 还是 `internal` 作为最终类的可见性。
要解决这个问题，你可以确保所有分部类都使用相同的访问修饰符，或者明确指定某一种修饰符，如：
```csharp
// Person2.cs
public partial class Person // 修改为 public
{
    public int Age { get; set; }
}
```
这样就保证了所有部分的访问修饰符一致，编译器就能确定 `Person` 类的可见性是 `public`。

### 分部方法
在C#中，**分部方法（Partial Methods）** 是一种特殊的方法，可以在分部类（`partial`）中使用。分部方法的定义可以分成两个部分**声明**和**实现**，它们可以分别位于分部类的不同部分中。
#### 分部方法的特点
1. **必须定义在分部类或结构中** ： 分部方法只能在使用了`partial`关键字的分部类或结构中定义。
2. **必须是`void`类型** ： 分部方法不能有返回值，必须是`void`类型。
3. **隐式`private`** ： 分部方法默认为私有（`private`），不能使用访问修饰符，因为它只能在当前类中使用。
4. **可选实现** ： 分部方法的实现是可选的。如果没有提供实现，则在编译时，声明将被忽略，不会生成任何代码。因此，调用未实现的分部方法不会产生任何影响。
5. **不支持输出参数（`out`）** ： 分部方法不能使用`out`修饰符，但可以使用`ref`修饰符。
#### 分部方法的语法
1. **声明**： 在一个分部类中声明分部方法，使用`partial`关键字。
2. **实现**： 可以在类的另一个分部文件中实现这个方法，也可以不实现。
#### 示例代码
以下示例演示了分部方法的声明和实现。
文件 `Person1.cs`：
```csharp
// Person1.cs
public partial class Person
{
    private string name;

    // 分部方法的声明
    partial void OnNameChanged();

    public string Name
    {
        get { return name; }
        set
        {
            name = value;
            // 调用分部方法
            OnNameChanged();
        }
    }
}
```
文件 `Person2.cs`：
```csharp
// Person2.cs
public partial class Person
{
    // 分部方法的实现
    partial void OnNameChanged()
    {
        Console.WriteLine($"Name changed to: {name}");
    }
}
```
在这个例子中，`OnNameChanged`方法在`Person1.cs`中进行了声明，并在`Person2.cs`中进行了实现。如果不提供实现，编译器会忽略它的声明。如果调用了声明但没有定义的方法也没事，直接忽略了。
#### 适用场景
- **在生成代码中扩展功能**：分部方法常用于自动生成的代码中，比如工具生成的类中声明分部方法，用户可以选择性地在另外的文件中实现它们。
- **模块化设计**：通过将方法拆分为不同的文件，可以更好地管理和维护大型项目。
分部方法提供了一种灵活的方式来设计和实现方法，同时保持代码的模块化和清晰性。
# 27. 类的继承
在C#中，**类的继承**是一种面向对象编程的机制，它允许一个类（称为子类或派生类）继承另一个类（称为父类或基类）的属性和方法。通过继承，可以实现代码重用、扩展类的功能以及多态性。
### 1. 基本语法
在C#中使用`:`符号来表示继承关系，语法如下：
```csharp
class DerivedClass : BaseClass
{
    // 子类的成员
}
```
- `BaseClass`是基类或父类，提供了一些基本功能。
- `DerivedClass`是子类或派生类，它继承了`BaseClass`的所有公有和受保护的成员（字段、方法、属性等）。
### 2. 继承的特点
- **单继承**：C#只支持单继承，一个类只能有一个直接的父类。但是，一个类可以实现多个接口。
- 传递性：子类可以继承父类的父类
- **成员访问**：子类可以访问父类的`public`和`protected`成员，但不能直接访问父类的`private`成员。
- **构造函数的调用**：子类的构造函数会首先调用父类的构造函数，在调用子类得构造函数。可以使用`base`关键字来指定调用父类的哪个构造函数。
- 类中会默认提供默认构造函数（无参），如果定义了有参构造函数，那么默认构造函数就不会提供
- 子类不会自动继承父类的特性。如果子类需要使用相同的特性，必须显式地在子类上再次应用特性。
	- 有些特性允许被继承，这取决于特性的定义
	- `[AttributeUsage(AttributeTargets.Class, Inherited = true)]` // 表示特性可以被继承
	- **`Inherited = false` 的特性**：如果特性声明时未设置 `Inherited = true`（默认值是 `false`），则子类不会继承该特性。


### 3. 示例代码
以下是一个简单的继承示例：
```csharp
// 父类
public class Animal
{
    public string Name { get; set; }

    public Animal()
    {
        Name = "Animal";
        Console.WriteLine("Animal 默认构造函数");
    }
    public void Eat()
    {
        Console.WriteLine($"{Name} is eating.");
    }
}

// 子类
public class Dog : Animal
{

    public Dog()
    {
        Name = "Dog";
        Console.WriteLine("Dog 默认构造函数");
    }
    public void Bark()
    {
        Console.WriteLine($"{Name} is barking.");
    }
}

class Program
{
    static void Main()
    {
        Dog dog = new Dog(); //Animal 默认构造函数 Dog 默认构造函数
        dog.Name = "Buddy";
        dog.Eat(); // 调用从父类继承的方法
        dog.Bark(); // 调用子类的方法
    }
}
```
输出结果：
```csharp
Animal 默认构造函数
Dog 默认构造函数
Buddy is eating.
Buddy is barking.
```

#### 改变父类的成员的访问修饰符的权限
##### 1. **如何将继承的 `public` 改为 `protected` 或者 `protected` 改为 `public`**
- `public` 成员可以在任何地方访问，包括子类和外部代码。
- `protected` 成员只能在当前类和其子类中访问，外部代码无法访问。
如果你希望将继承的 `public` 改为 `protected`，或者将 `protected` 改为 `public`，你可以在子类中通过访问修饰符的修改来实现。比如：
###### 将 `public` 成员改为 `protected`：
```csharp
// 子类
public class Dog : Animal
{
    // 修改继承的 Name 属性为 protected
    protected new string Name { get; set; }

    public Dog()
    {
        Name = "Dog";
        Console.WriteLine("Dog 默认构造函数");
    }
    
    // 子类独有的方法
    public void Bark()
    {
        Console.WriteLine($"{Name} is barking.");
    }
}
```
###### 将 `protected` 成员改为 `public`：
```csharp
// 父类
public class Animal
{
    // 将 protected 的 Name 改为 public
    public new string Name { get; set; }

    public Animal()
    {
        Name = "Animal";
        Console.WriteLine("Animal 默认构造函数");
    }

    public void Eat()
    {
        Console.WriteLine($"{Name} is eating.");
    }
}

// 子类
public class Dog : Animal
{
    public Dog()
    {
        Name = "Dog"; // 访问父类的公开 Name 属性
        Console.WriteLine("Dog 默认构造函数");
    }

    public void Bark()
    {
        Console.WriteLine($"{Name} is barking.");
    }
}
```
##### 2. **父类有 `private` 成员能继承吗？**
- `private` 成员 **不能** 被子类直接访问或继承。它只能在父类的内部被访问。
- 但是，子类可以继承父类的 `private` 成员 **通过公有或保护的访问方法**，例如父类提供公共的 `getter` 和 `setter` 方法来访问这些私有成员。
###### 示例：父类的 `private` 成员如何通过 `protected` 或 `public` 方法访问
```csharp
// 父类
public class Animal
{
    // private 成员
    private string privateName;

    public Animal()
    {
        privateName = "Animal Private";
        Console.WriteLine("Animal 默认构造函数");
    }

    // 提供公共方法来访问 private 成员
    public string GetPrivateName()
    {
        return privateName;
    }

    // 提供公共方法来修改 private 成员
    public void SetPrivateName(string name)
    {
        privateName = name;
    }

    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

// 子类
public class Dog : Animal
{
    public Dog()
    {
        // 无法直接访问父类的 private 成员
        // privateName = "Dog";  // 编译错误：无法访问
        SetPrivateName("Dog"); // 使用父类提供的公共方法
        Console.WriteLine("Dog 默认构造函数");
    }

    public void Bark()
    {
        Console.WriteLine("Barking...");
    }
}

class Program
{
    static void Main()
    {
        Dog dog = new Dog();
        // 无法直接访问 private 成员
        // Console.WriteLine(dog.privateName);  // 编译错误
        Console.WriteLine(dog.GetPrivateName()); // 通过父类的公共方法访问 private 成员
    }
}
```
##### 总结：
- **`public` 成员**：可以直接在子类和外部访问。
- **`protected` 成员**：只能在子类和父类中访问，外部代码无法访问。
- **`private` 成员**：只能在父类内部访问，不能直接在子类中访问，但可以通过父类提供的公共方法进行访问。
- **改变访问修饰符**：子类可以通过 `new` 关键字隐藏父类的成员，或者通过修改继承的成员的访问修饰符来改变访问权限。

### 4. `base`关键字
- `base`关键字用于在子类中访问父类的成员。
- 它可以用来调用父类的构造函数或方法。
```csharp
public class Animal
{
    public string Name { get; set; }

	// 无默认构造函数，如果new Animal()会报错
    public Animal(string name)
    {
        Name = name;
    }

    public void Speak()
    {
        Console.WriteLine($"{Name} makes a noise.");
    }
}

public class Dog : Animal
{
    // 如果父类中没有默认构造函数，使用下面得构造函数会报错
    //public Dog(string name)
    //{ }
    // 否则需要显示调用父类得构造函数，因为总是会先调用父类得构造函数，然后调用子类得构造函数，如果子类没有写构造函数，会提供一个子类得默认构造函数，而不是使用父类得默认构造函数
    public Dog(string name) : base(name)
    {
    }

    public void Bark()
    {
        base.Speak(); // 调用父类的Speak方法
        Console.WriteLine($"{Name} barks.");
    }
}
```
### 5. 方法重写（`override`）
- 如果子类需要修改父类的方法行为，可以使用`override`关键字重写父类的方法。
- 父类的方法必须使用`virtual`关键字进行标记。
```csharp
public class Animal
{
    //public virtual两种顺序都行
    virtual public void Speak()
    {
        Console.WriteLine("Animal makes a noise.");
    }
}

public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

- 如果不使用virtual关键字
```csharp
public class Animal
{
    public void Speak()
    {
        Console.WriteLine("Animal makes a noise.");
    }

    public void Speak(int i)
    {
        Console.WriteLine("Animal makes a noise=1.");
    }
}

public class Dog : Animal
{
    // 如果是有意隐藏父类中的相同的方法，需要加new，否则会警告
    // 但是和Cpp不一样，父类中其他同名的还是可以用，重载的都可以用，不会全部隐藏。Cpp里面叫名字遮蔽
    public new void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}

public class Program
{
    public static void Main()
    {
        Dog dog = new Dog();
        dog.Speak();
        dog.Speak(1);
    }
}
```
### 6. 继承的限制
- **不能继承`sealed`类**：如果一个类被标记为`sealed`，则不能被继承。
- **`static`类不能被继承**：静态类不能被继承或实例化。
### 7. 里氏替换原则
可以将子类对象赋值给父类引用（基类类型的引用可以指向派生类对象），这样可以利用多态实现灵活的代码设计。
继承提供了一种强大的代码复用和扩展功能的方式，在设计类的层次结构时需要合理使用。
在C#中，可以将子类对象赋值给父类引用，这种行为称为**多态**。通过这种方式，可以利用父类引用来处理不同子类的对象，从而实现灵活的代码设计。下面是一个简单的示例来展示这一点。
#### 示例代码
```csharp
using System;

// 基类
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }
}

// 派生类：Dog
public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}

// 派生类：Cat
public class Cat : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Cat meows.");
    }
}

class Program
{
    static void Main()
    {
        // 创建 Dog 和 Cat 的实例
        Animal myDog = new Dog();
        Animal myCat = new Cat();

        // 调用 Speak 方法，实际调用的是派生类的方法
        myDog.Speak(); // 输出: Dog barks.
        myCat.Speak(); // 输出: Cat meows.

        // 使用多态的集合
        Animal[] animals = { myDog, myCat };

        // 遍历集合并调用 Speak 方法
        foreach (var animal in animals)
        {
            animal.Speak();
        }
    }
}
```
#### 输出结果
```csharp
Dog barks.
Cat meows.
Dog barks.
Cat meows.
```
#### 解释
1. **基类 `Animal`** ：
    - 定义了一个虚方法 `Speak()`，这个方法可以在派生类中被重写。
2. **派生类 `Dog` 和 `Cat`** ：
    - 分别重写了 `Speak()` 方法，提供了特定的实现。
    - `Dog` 类输出 `"Dog barks."`，而 `Cat` 类输出 `"Cat meows."`
3. **多态** ：
    - 在 `Main` 方法中，创建了 `Dog` 和 `Cat` 的实例，并将它们赋值给 `Animal` 类型的引用 `myDog` 和 `myCat`。
    - 通过调用 `Speak()` 方法，尽管引用类型是 `Animal`，实际调用的是子类 `Dog` 和 `Cat` 的实现。
4. **集合与多态** ：
    - 使用 `Animal` 数组存储不同类型的动物，通过遍历数组调用 `Speak()` 方法，展现了多态的灵活性和可扩展性。
### 子类的构造函数会首先调用父类的构造函数。可以使用`base`关键字来指定调用父类的哪个构造函数
在C#中，子类的构造函数在创建对象时会首先调用父类的构造函数。这可以确保父类的初始化逻辑在子类的初始化之前执行。使用`base`关键字可以指定调用父类的特定构造函数。
#### 示例代码
以下是一个简单的示例，展示了如何在子类构造函数中调用父类构造函数。
```csharp
using System;

public class Animal
{
    public string Name { get; set; }

    // 父类的构造函数
    public Animal(string name)
    {
        Name = name;
        Console.WriteLine($"{Name} has been created.");
    }
}

public class Dog : Animal
{
    public string Breed { get; set; }

    // 子类的构造函数，调用父类的构造函数
    public Dog(string name, string breed) : base(name)
    {
        Breed = breed;
        Console.WriteLine($"{Name} is a {Breed}.");
    }
}

class Program
{
    static void Main()
    {
        Dog myDog = new Dog("Buddy", "Golden Retriever");
    }
}
```
#### 输出结果
```csharp
Buddy has been created.
Buddy is a Golden Retriever.
```
#### 解释
1. **父类构造函数**：`Animal`类有一个构造函数，接收一个参数`name`，用于初始化`Name`属性，并打印消息。
2. **子类构造函数**：`Dog`类的构造函数接收两个参数：`name`和`breed`。在构造函数的参数列表中，使用`base(name)`调用父类`Animal`的构造函数，将`name`传递给它。
3. **实例化对象**：在`Main`方法中，创建了一个`Dog`类的实例`myDog`，传递了`"Buddy"`和`"Golden Retriever"`。创建对象时，首先调用`Animal`类的构造函数，然后再执行`Dog`类的构造函数。
这个例子展示了如何在子类中通过`base`关键字调用父类构造函数，确保父类的初始化逻辑被正确执行。

# 28. is和as
> 在C#中，`is` 和 `as` 是两个用于类型检查和类型转换的关键字

### `is` 关键字
`is` 用于检查一个对象是否是某个特定类型或是否实现了某个接口。它返回一个布尔值，指示对象是否可以安全地转换为指定的类型。
#### 示例：
```csharp
using System;

class Program
{
    static void Main()
    {
        object obj = "Hello, World!";

        // 使用 is 检查 obj 是否是字符串类型
        // “模式匹配”（Pattern Matching）
        // obj is string str 的意思是：
        // 检查 obj 是否是 string 类型。
        // 如果 obj 是 string 类型，则将 obj 转换为 string 类型，并赋值给新的局部变量 str。
        // 这个str只能在if的作用域内使用，只是一个局部变量
        // 如果类型检查成功（即 obj 是 string 类型），表达式的值为 true，并且可以在之后的代码中使用 str 变量。
        // 否则表达式的值为false，那么 str 不会被定义，也无法在 if 语句块外使用 str。
        if (obj is string str)
        {
            Console.WriteLine($"The object is a string: {str}");
        }
        else
        {
            Console.WriteLine("The object is not a string.");
        }
    }
}
```
#### 输出：
```csharp
The object is a string: Hello, World!
```

### `as` 关键字
`as` 用于尝试将一个`对象`转换为指定的类型，值类型不可以。如果转换成功，它会返回转换后的对象；如果失败，它会返回 `null`，而不会抛出异常。这在需要进行类型转换时非常有用，可以避免潜在的运行时错误。
#### 示例：
```csharp
using System;

class Program
{
    static void Main()
    {
        object obj = "Hello, World!";

        // 使用 as 尝试将 obj 转换为字符串类型
        string str = obj as string;

        if (str != null)
        {
            Console.WriteLine($"The object was successfully converted to a string: {str}");
        }
        else
        {
            Console.WriteLine("The object could not be converted to a string.");
        }
    }
}
```
#### 输出：
```css
The object was successfully converted to a string: Hello, World!
```
### 区别总结
1. **用途**：
    - `is`：用于检查对象的类型，返回一个布尔值。
    - `as`：用于进行安全的类型转换，返回转换后的对象或 `null`。
2. **返回值**：
    - `is` 返回 `true` 或 `false`。
    - `as` 返回转换后的对象或 `null`。
3. **异常处理**：
    - 使用 `is` 进行类型检查时，不会抛出异常。
    - 使用 `as` 进行类型转换时，如果转换失败，不会抛出异常，而是返回 `null`。
### 适用场景
- 使用 `is` 当你只需要知道一个对象的类型时。
- 使用 `as` 当你希望将一个对象转换为特定类型，并且希望在失败时得到 `null` 而不是异常时。

# 29. 模式匹配（Pattern Matching）
**模式匹配**（Pattern Matching）是C#中的一种特性，用于检查某个对象是否符合指定的类型或条件，并在匹配成功时将其转换为相应类型或结构。这种特性可以简化类型检查和转换的代码，使代码更加简洁和可读。
### 模式匹配的常见用法
#### 1. **类型模式匹配（Type Pattern Matching）**
- 用于检查一个对象是否属于某种类型，并在成功时将其转换为该类型。这种模式在`is`关键字后面使用。
```csharp
object obj = "Hello, World!";
if (obj is string str)
{
    // str 现在是一个 string 类型
    Console.WriteLine($"The object is a string: {str}");
}
else
{
    Console.WriteLine("The object is not a string.");

```
在这个示例中，如果`obj`是`string`类型，则将其转换为`str`，并可以在`if`语句块中使用。
#### 2. **常量模式匹配（Constant Pattern Matching）**
- 可以用于比较对象是否与某个常量相等。
```csharp
int number = 10;
if (number is 10)
{
    Console.WriteLine("The number is 10.");
}
else
{
    Console.WriteLine("The number is not 10.");
}
```
这里通过`is`来检查`number`是否等于常量10。
- 上面的例子太简单，看这个
```csharp
object obj = 42;

// 使用常量模式匹配
if (obj is 42)
{
    Console.WriteLine("The object is 42");
}

// 使用==操作符
if (obj.Equals(42))
{
    Console.WriteLine("The object is also 42");
}
```

#### 3. **表达式模式匹配（Expression Pattern Matching）**
- 允许在`switch`表达式中使用模式匹配。
```csharp
object obj = 42;

switch (obj)
{
    case int i when i > 0:
        Console.WriteLine("Positive integer.");
        break;
    case int i when i < 0:
        Console.WriteLine("Negative integer.");
        break;
    case string s:
        Console.WriteLine($"String: {s}");
        break;
    default:
        Console.WriteLine("Unknown type or condition.");
        break;
}
```
在这里，`switch`表达式结合`when`条件可以对对象的不同模式进行匹配并处理。

##### 代码解释
这个`switch`语句使用了模式匹配的功能来处理不同类型和条件的`obj`值，`when`关键字用于指定额外的条件约束。具体解释如下：
1. `case int i when i > 0`：
    - 这里的`case`语句匹配`obj`的类型，如果`obj`是一个整数（`int`），定义一个局部变量`i`，`i`就是`obj`转换为`int`的变量，进行下一步判断。
    - `when i > 0` 表示只有当`i`大于0时，这个`case`才会匹配，输出“Positive integer.”。
2. `case int i when i < 0`：
    - 同样，这个分支首先检查`obj`的类型是否为整数（`int`），然后判断这个整数是否小于0。
    - 当`i < 0`时，匹配成功，输出“Negative integer.”。
3. `case string s`：
    - 如果前面的条件都不满足，则检查`obj`是否为字符串（`string`）类型。
    - 如果是，则匹配成功，并输出字符串内容，例如“String: ...”。
4. `default`：
    - 如果`obj`的类型不匹配前面的任何一个`case`，则执行`default`分支，输出“Unknown type or condition.”。
##### `when`关键字的作用
`when`用于在匹配类型的基础上添加条件约束。即使类型匹配成功，也要满足`when`后面的条件，才能执行该分支。它可以用于进一步筛选具体的情况，使模式匹配更灵活和精确。

#### 4. **位置模式匹配（Positional Pattern Matching）**
- 用于解构对象，匹配对象的属性或字段。这在C# 8.0及更高版本中得到支持。
- 位置模式匹配用于检查元组或某些类型的值是否与指定的模式匹配。这里的`(3, 4)`是一个位置模式，它表示检查元组中的每个元素是否与指定的值匹配。
- 例如，`point is (3, 4)`意味着检查`point`是否是一个有两个元素的元组，并且第一个元素的值为`3`，第二个元素的值为`4`。
```csharp
var point = (X: 3, Y: 4);

if (point is (3, 4))
{
    Console.WriteLine("The point is (3, 4).");
}
```
这个例子检查一个元组是否具有指定的值。
#### 解释代码
1. **元组的定义**:
```csharp
var point = (X: 3, Y: 4);
```
这里定义了一个元组`point`，它有两个元素`X`和`Y`，分别赋值为`3`和`4`。这个元组的类型是`(int X, int Y)`，表示一个包含两个整型值的元组。
1. **模式匹配检查**:
```csharp
if (point is (3, 4))
{
    Console.WriteLine("The point is (3, 4).");
}
```
这段代码中的`point is (3, 4)`使用了**位置模式匹配（Positional Pattern Matching）** 来检查`point`的值是否为`(3, 4)`。
- `point is (3, 4)`表示检查`point`是否是一个元组，并且其第一个元素的值为`3`，第二个元素的值为`4`。
- 如果`point`的值确实为`(3, 4)`，则条件成立，执行`Console.WriteLine("The point is (3, 4).");`。
#### 元组和模式匹配
- C#中的元组可以用来存储一组相关的数据，并且可以通过模式匹配来对元组的值进行检查。
- 在`if`语句中使用`is`关键字进行模式匹配时，会将元组的值与指定的常量进行比较。
- 这种方法可以用于简洁地检查多个值是否匹配特定的组合。
#### 代码运行结果
如果`point`的值为`(3, 4)`，那么程序会输出：
```csharp
The point is (3, 4).
```
如果`point`的值不同，比如`(5, 6)`，则不会输出任何内容。

### 模式匹配的优势
- **简洁性**：无需额外的类型转换代码。
- **安全性**：通过类型检查和转换结合，减少显式类型转换带来的潜在错误。
- **灵活性**：可以根据不同的条件和类型编写简洁的控制流。
模式匹配在C# 7.0及更高版本中引入，并在后续版本中不断扩展，使得类型检查、条件分支处理更加直观。

# 30. when的使用场景
`when`关键字主要用于`switch`表达式和`case`语句中，提供对模式匹配的额外条件约束。常见的使用场景包括：
### 1. **精确控制匹配条件**
- 当对类型匹配不够时，`when`可以进一步限定匹配的条件。例如，在类型匹配的基础上，还可以检查数值范围、字符串内容等。
```csharp
switch (obj)
{
    case int i when i > 100:
        Console.WriteLine("Large number.");
        break;
    case int i when i <= 100:
        Console.WriteLine("Small number.");
        break;
    default:
        Console.WriteLine("Not an integer.");
        break;
}
```
### 2. **处理多个条件的组合**
- 适用于多个条件的组合场景。比如同时对类型和某些属性进行匹配时，可以用`when`来增加逻辑判断。
```csharp
switch (person)
{
    case Employee e when e.YearsOfService > 10:
        Console.WriteLine("Senior Employee.");
        break;
    case Employee e when e.YearsOfService <= 10:
        Console.WriteLine("Junior Employee.");
        break;
    default:
        Console.WriteLine("Not an employee.");
        break;
}
```
### 3. **在异常处理中的使用**
- 可以在`catch`语句中结合`when`条件处理特定的异常情况。例如，在捕获异常时，根据异常的某个属性来判断是否执行该捕获块。
```csharp
try
{
    // Some code that might throw exceptions
}
catch (IOException ex) when (ex.Message.Contains("disk"))
{
    Console.WriteLine("Disk-related error.");
}
catch (IOException ex)
{
    Console.WriteLine("General I/O error.");
}
```
### 4. **筛选集合中的元素**
- 在处理集合数据时，使用`when`可以根据元素的条件筛选匹配的元素。
```csharp
var numbers = new int[] { 1, 2, 3, 4, 5, 6 };
foreach (var number in numbers)
{
    switch (number)
    {
        case int n when n % 2 == 0:
            Console.WriteLine($"{n} is even.");
            break;
        case int n:
            Console.WriteLine($"{n} is odd.");
            break;
    }
}
```

# 31. var
`var` 是 C# 中的一种隐式类型声明方式，用于让编译器自动推断变量的类型。在使用 `var` 时，编译器会根据赋值的表达式确定变量的具体类型。
### 使用场景
`var` 可以在以下情况下使用：
1. **局部变量的类型推断**：
```csharp
var number = 10; // 编译器推断为 int 类型
var name = "Hello"; // 编译器推断为 string 类型
var isCompleted = true; // 编译器推断为 bool 类型
```
2. **集合或复杂对象**：
```csharp
var list = new List<int>(); // 编译器推断为 List<int> 类型
var person = new { Name = "Alice", Age = 30 }; // 匿名类型
```
3. **LINQ 查询结果**：
```csharp
var result = from n in numbers
             where n > 5
             select n;
```
### 注意事项
- 使用 `var` 时，必须立即对变量进行初始化，因为编译器需要根据赋值来推断类型。
- 一旦类型推断完成，变量的类型在编译时就已经确定，不能改变。
### `var` 的优缺点
**优点**：
- 代码更简洁，避免重复声明类型。
- 适合处理匿名类型和复杂类型。
**缺点**：
- 可能会降低代码的可读性，尤其是在类型不明显时。
### C#的var和C++的auto
#### 相似之处
1. **类型自动推断**：在两者中，编译器根据右侧的表达式来推断变量的类型。例如：
    - **C#**
	```csharp
	var number = 42; // 编译器推断为 int
	```
    - **C++**
	```cpp
	auto number = 42; // 编译器推断为 int
	```
2. **简化代码**：使用 `var` 或 `auto` 可以减少重复的类型声明，使代码更简洁。
3. **必须初始化**：两者在声明时都必须立即进行初始化，因为编译器需要根据赋值来确定类型。
#### 不同之处
1. **使用场景**：
    - 在 C# 中，`var` 只能用于局部变量的类型推断，而不能用于方法参数、返回类型或字段。
    - 在 C++ 中，`auto` 可以用于函数的返回类型推断（C++14 起支持），也可以在循环中使用 `auto` 来自动推断迭代器的类型。
2. **类型推断的时间点**：
    - C# 的类型推断是在编译时进行的，变量的类型一旦确定，就无法更改。
    - C++ 中的 `auto` 也是在编译时进行类型推断，但由于 C++ 支持模板和类型推断的多样性，`auto` 可以用于更复杂的场景，如模板编程。
3. **类型推断的灵活性**：
    - C++ 中的 `auto` 还可以和 `const`、`&`（引用）等结合使用，来推断更复杂的类型，如常量引用。
    - C# 中的 `var` 不支持这样的组合用法。


# 32. 元组
在 C# 中，元组（Tuple）是一种数据结构，可以存储多个不同类型的值。元组的主要特点是简单、轻量，可以快速创建和传递多个相关的数据。以下是对 C# 中元组的详细介绍：
### 1. **定义和创建元组**
在 C# 中，你可以使用 `Tuple` 类或更简洁的语法来创建元组。C# 7.0 引入了更简化的元组语法，使用小括号和命名元素。
#### **使用 `Tuple` 类**

`Tuple` 类可以包含最多 **8** 个元素。如果需要更多的元素，可以嵌套使用元组。

```csharp
var tuple = new Tuple<int, string, double, char>(1, "Hello", 3.14, 'A');
Console.WriteLine(tuple.Item1); // 输出: 1
Console.WriteLine(tuple.Item2); // 输出: Hello
Console.WriteLine(tuple.Item3); // 输出: 3.14
Console.WriteLine(tuple.Item4); // 输出: A
```
#### **使用 C# 7.0 及以上的元组简化语法**
C# 7.0 引入了更简洁的元组语法，可以轻松创建和使用多个元素的元组：
```csharp
var tuple = (Id: 1, Name: "Alice", Age: 30, Height: 1.75, IsStudent: false);
Console.WriteLine(tuple.Id);      // 输出: 1
Console.WriteLine(tuple.Name);     // 输出: Alice
Console.WriteLine(tuple.Age);      // 输出: 30
Console.WriteLine(tuple.Height);   // 输出: 1.75
Console.WriteLine(tuple.IsStudent); // 输出: False
```
#### **嵌套元组**
如果需要更多的元素，可以通过嵌套元组的方式来实现：
```csharp
using System;

class Program
{
    static void Main()
    {
        var tuple = (Person: (Name: "Alice", Age: 30), Coordinates: (X: 10, Y: 20));
        Console.WriteLine(tuple.Person); // 输出: (Alice, 30)
        Console.WriteLine(tuple.Coordinates.X); // 输出: 10
    }
}
```
### 2. **访问元组元素**
可以通过属性（对于 `Tuple` 类）或直接使用字段（对于简化语法）来访问元组的元素。
```csharp
var tuple = (X: 10, Y: 20);
Console.WriteLine(tuple.X); // 输出: 10
Console.WriteLine(tuple.Y); // 输出: 20
```
### 3. **元组的解构**
C# 支持元组解构，可以将元组的元素赋值给多个变量。
```csharp
var point = (X: 3, Y: 4);
var (x, y) = point;
Console.WriteLine(x); // 输出: 3
Console.WriteLine(y); // 输出: 4
```
### 4. **元组的用途**
元组通常用于以下场景：
- 返回多个值：可以用元组来返回多个相关的值，而不需要创建一个新的类或结构体。
- 临时数据存储：在不需要定义一个完整的类的情况下，快速存储和处理小规模的数据。
### 5. **注意事项**
- 元组的元素类型可以不同，但元组本身是不可变的，这意味着一旦创建，元组的元素值不能被改变。
- 对于需要更多功能和可读性的场景，建议使用自定义类或结构体。
### 示例
下面是一个完整的示例，演示了如何使用元组：
```csharp
using System;

class Program
{
    static void Main()
    {
        var person = (Name: "Alice", Age: 30);

        Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");

        // 解构元组
        var (name, age) = person;
        Console.WriteLine($"Name: {name}, Age: {age}");
    }
}
```
# 33. LINQ
LINQ（Language Integrated Query）是 C# 中的一种用于处理数据的查询语言。它使开发人员能够使用类似于 SQL 的语法来查询和操作数据，而不需要具体了解数据的存储方式。LINQ 可以用于多种数据源，包括数组、集合、XML、数据库等。
- 只是简单记录，以后用到再说
### LINQ 的主要特点
1. **集成查询**：LINQ 提供了一种将查询逻辑直接嵌入 C# 代码中的方式。
2. **强类型**：LINQ 查询是强类型的，编译时会检查类型安全。
3. **可读性**：LINQ 语法简洁易读，类似于 SQL，使得代码更容易理解。
4. **延迟执行**：LINQ 查询支持延迟执行，即查询直到实际需要结果时才会执行。
### LINQ 的基本用法
以下是 LINQ 的一些常见用法示例：
#### 1. **LINQ to Objects**
查询集合中的数据。
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6 };

        // 查询所有大于 3 的数字
        var result = from n in numbers
                     where n > 3
                     select n;

        foreach (var number in result)
        {
            Console.WriteLine(number); // 输出: 4, 5, 6
        }
    }
}
```
#### 2. **LINQ to SQL**
查询数据库中的数据。
```csharp
using (var context = new YourDataContext())
{
    var query = from c in context.Customers
                where c.City == "London"
                select c;

    foreach (var customer in query)
    {
        Console.WriteLine(customer.Name);
    }
}
```
#### 3. **方法语法**
LINQ 支持两种语法：查询语法和方法语法。

```csharp
var result = numbers.Where(n => n > 3); // 方法语法

foreach (var number in result)
{
    Console.WriteLine(number); // 输出: 4, 5, 6
}
```
#### 4. **组合查询**
你可以将多个 LINQ 查询组合在一起。
```csharp
var result = numbers
    .Where(n => n > 2)
    .Select(n => n * 2); // 先过滤，然后乘以 2

foreach (var number in result)
{
    Console.WriteLine(number); // 输出: 6, 8, 10, 12
}
```
#### 5. 进一步处理，不仅仅是LINQ可以用
除了 `ToList()`，LINQ 查询结果还可以通过其他方法进行转换或进一步处理。常用的 LINQ 转换方法包括：
1. **`ToArray()`**  
    将查询结果转换为数组。
```csharp
var evenNumbersArray = numbers.Where(n => n % 2 == 0).ToArray();
```
2. **`ToDictionary()`**  
    将查询结果转换为字典，需要指定键和值的选择器。
```csharp
var evenNumbersDictionary = numbers.Where(n => n % 2 == 0)
                                   .ToDictionary(n => n, n => n.ToString());
// 这里键是数字本身，值是对应的字符串表示
```
3. **`ToHashSet()`**  
    将查询结果转换为 `HashSet`，去除重复项。
```csharp
var evenNumbersSet = numbers.Where(n => n % 2 == 0).ToHashSet();
```
4. **`First()` / `FirstOrDefault()`**  
    返回第一个符合条件的元素，`FirstOrDefault()` 如果没有找到元素会返回类型的默认值。
```csharp
var firstEven = numbers.Where(n => n % 2 == 0).First();
var firstEvenOrDefault = numbers.Where(n => n % 2 == 0).FirstOrDefault();
```
5. **`Last()` / `LastOrDefault()`**  
    返回最后一个符合条件的元素，`LastOrDefault()` 如果没有找到元素会返回类型的默认值。
```csharp
var lastEven = numbers.Where(n => n % 2 == 0).Last();
var lastEvenOrDefault = numbers.Where(n => n % 2 == 0).LastOrDefault();
```
6. **`Single()` / `SingleOrDefault()`**  
    返回唯一一个符合条件的元素，如果有多个或没有找到则抛出异常，`SingleOrDefault()` 在没有找到时返回默认值。
```csharp
var singleEven = numbers.Where(n => n == 2).Single();
var singleEvenOrDefault = numbers.Where(n => n == 2).SingleOrDefault();
```
7. **`Count()`**  
    计算符合条件的元素个数。
```csharp
int evenCount = numbers.Where(n => n % 2 == 0).Count();
```
8. **`Any()`**  
    判断是否有任何元素符合条件。
```csharp
bool hasEven = numbers.Where(n => n % 2 == 0).Any();
```
9. **`All()`**  
    判断是否所有元素都符合条件。
```csharp
bool allEven = numbers.All(n => n % 2 == 0);
```
10. **`Max()` / `Min()`**  
    查找最大或最小值。
```csharp
int maxEven = numbers.Where(n => n % 2 == 0).Max();
int minEven = numbers.Where(n => n % 2 == 0).Min();
```
11. **`Sum()` / `Average()`**  
    计算总和或平均值。
```csharp
int sumEven = numbers.Where(n => n % 2 == 0).Sum();
double avgEven = numbers.Where(n => n % 2 == 0).Average();

```
12. **`ToList()`**  
将查询结果转换为List。
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        // 示例列表
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

        // 使用 LINQ 查询筛选出所有偶数，并转换为 List<int>
        List<int> evenNumbers = numbers.Where(n => n % 2 == 0).ToList();

        // 输出结果
        Console.WriteLine("偶数列表:");
        foreach (var num in evenNumbers)
        {
            Console.WriteLine(num);
        }
    }
}
```

### LINQ 的常用操作
- **Where**：过滤数据。
- **Select**：投影数据。
- **OrderBy** 和 **OrderByDescending**：排序。
- **GroupBy**：分组。
- **Join**：连接多个数据源。
- **Count**、**Sum**、**Average**、**Max**、**Min**：聚合操作。

# 34. C#中的Object，装箱和拆箱
在C#中，**装箱**（Boxing）和**拆箱**（Unboxing）是指值类型和引用类型之间的转换过程。理解这两个概念有助于掌握C#中的内存管理和类型转换。
### 装箱（Boxing）
装箱是将一个值类型转换为对象类型（引用类型）的过程。此过程会在堆上分配内存，并将值类型的值复制到该内存位置。
**示例：**
```csharp
int number = 42; // 值类型
object boxedNumber = number; // 装箱
```
在上述示例中，`number` 是一个值类型的 `int`，当我们将它赋值给 `object` 类型的 `boxedNumber` 时，发生了装箱。`number` 的值被复制到堆上一个新的对象中。
### 拆箱（Unboxing）
拆箱是将一个已装箱的对象转换回值类型的过程。此过程会将对象的值复制回值类型变量。
**示例：**
```csharp
object boxedNumber = 42; // 装箱
int unboxedNumber = (int)boxedNumber; // 拆箱
```
在上述示例中，`boxedNumber` 是一个对象，它包含了一个整型值。通过将其强制转换为 `int`，我们执行了拆箱操作。
### 注意事项
1. **性能开销**：装箱和拆箱会导致额外的性能开销，因为它们涉及内存分配和数据复制。因此，尽量避免频繁的装箱和拆箱。
装箱和拆箱会导致性能开销的原因主要在于它们涉及**内存分配**和**数据复制**。具体来说：
- 装箱的性能开销
	装箱是将一个**值类型**转换为**引用类型**（对象）的过程。这个过程涉及以下几个步骤：
	- **在堆上分配内存**：装箱时，值类型的值需要被封装到一个对象中，而这个对象是在堆上分配的。堆上的内存分配比栈上的内存分配要慢，因为它涉及到更多的管理工作（如内存分配、垃圾回收等）。
	- **复制数据**：在装箱的过程中，需要将值类型的数据复制到堆上分配的对象中。这意味着需要额外的内存操作。
- 拆箱的性能开销
	拆箱是将一个已装箱的对象转换回值类型的过程。拆箱操作也会带来一定的开销，原因如下：
	- **类型检查**：拆箱时，C#运行时会进行类型检查，以确保对象的类型与要转换的值类型匹配。如果类型不匹配，会引发 `InvalidCastException` 异常。这种类型检查会有一定的性能开销。
	- **数据复制**：拆箱时，需要将对象中的数据复制回值类型变量。这也意味着会有额外的内存操作。
- 垃圾回收的影响
	- 由于装箱会在堆上创建新的对象，这些对象可能在将来需要被垃圾回收。当堆上分配了大量装箱产生的对象时，会增加垃圾回收的频率和负担，从而进一步影响性能。
- 为什么栈上的分配更快？
	- 栈上的内存分配只需调整栈指针即可，非常高效。而堆上的内存分配涉及复杂的内存管理，例如查找适合的内存块、记录内存使用状态、处理垃圾回收等，因此要慢得多。
2. **类型安全**：拆箱时，如果对象不包含相应的值类型，程序将抛出 `InvalidCastException`。
3. **值类型与引用类型**：值类型（如 `int`、`float`、`struct`）直接存储在栈上，而引用类型（如 `object`、`string`）则存储在堆上。
### 结论
理解装箱和拆箱是C#中的一个重要概念，特别是在处理集合或需要多态时。尽量减少不必要的装箱和拆箱可以提高程序性能。

# 35. 值类型和引用类型的内存分配
在C#中，**值类型**和**引用类型**的内存分配方式有所不同：
### 值类型的内存分配
- **值类型**（如 `int`、`float`、`struct` 等）通常分配在**栈**上。这意味着它们的内存是在方法调用时分配，并在方法返回时释放。
- 栈上的内存分配非常快，因为它按照顺序管理，只需调整栈指针即可。
- 值类型直接包含其数据，这意味着访问值类型时没有间接性。
- **例外情况**：如果值类型是某个对象的一部分（例如，作为类的字段），那么这个值类型的实例将存储在堆上，因为包含它的对象位于堆上。
### 引用类型的内存分配
- **引用类型**（如 `class`、`array`、`string` 等）通常分配在**堆**上。堆内存适用于动态分配，内存分配和回收由垃圾收集器管理。
- 引用类型的变量实际上是一个指向堆上对象的**引用**，它本身存储在栈上。引用指向的对象实际存储在堆中。
- 当引用类型的对象不再被引用时，垃圾收集器会回收它所占用的堆内存。
### 举个例子
```csharp
int a = 10; // 值类型，分配在栈上

MyClass obj = new MyClass(); // 引用类型，obj的引用存储在栈上，对象本身分配在堆上

struct MyStruct
{
    public int x;
}

class MyClass
{
    public int y;
    public MyStruct myStruct; // myStruct存储在堆上，因为它是对象的一部分
}
```

在这个例子中，`a` 是一个值类型变量，分配在栈上。`obj` 是一个引用类型变量，指向堆上分配的 `MyClass` 实例。`MyClass` 的实例包含 `MyStruct` 类型的字段 `myStruct`，由于它是对象的一部分，因此也存储在堆上。

### 值类型和引用类型
#### 值类型
值类型直接包含数据，存储在栈上（或在某些情况下存储在堆上，比如作为类的字段）。常见的值类型包括：
1. **基本数据类型**：
    - `int`
    - `float`
    - `double`
    - `char`
    - `bool`
    - `byte`, `sbyte`
    - `short`, `ushort`
    - `long`, `ulong`
    - `decimal`
2. **结构体**（`struct`）：C#中的结构体是值类型。
    - 自定义的`struct`
    - 预定义的结构体，如 `DateTime`, `TimeSpan`, `Guid`
3. **枚举**（`enum`）：在C#中，枚举也是值类型。
#### 引用类型
引用类型的变量存储在栈上，但它们指向存储在堆上的实际数据。常见的引用类型包括：
1. **类**（`class`）
    - 自定义的类
    - 预定义的类，如 `string`, `ArrayList`, `List<T>`
2. **接口**（`interface`）：实现接口的类型也是引用类型。
3. **数组**：无论是 `int[]`, `string[]`，还是自定义类型的数组，都是引用类型。
4. **委托**（`delegate`）：委托类型也是引用类型。
5. **字符串**（`string`）：尽管字符串表现得像值类型（不可变），但它是引用类型。
#### 值类型与引用类型的主要区别
- **内存分配**：值类型通常在栈上分配，而引用类型在堆上分配。
- **赋值操作**：值类型赋值时会复制值，引用类型赋值时会复制引用（地址）。
- **默认值**：值类型的默认值是类型本身的默认值（如 `int` 的默认值为 `0`），而引用类型的默认值是 `null`。

# 36. 类型转换
在C#中，**类型转换**是将一个数据类型的值转换为另一个数据类型的过程。根据转换的方式和适用的场景，类型转换可以分为几种类型：**隐式转换**、**显式转换**、**装箱和拆箱**、以及**类型转换方法**。
### 1. 隐式转换
- 隐式转换是一种**自动进行**的转换，不需要显式地使用类型转换运算符。
- 这种转换通常在**不会丢失数据**的情况下进行，例如从较小范围的数值类型转换为较大范围的数值类型。
- 常见的隐式转换示例：
```csharp
int a = 10;
double b = a; // int 隐式转换为 double
```   
在上述示例中，`int` 转换为 `double` 是安全的，因为 `double` 的范围比 `int` 大，不会丢失精度。
### 2. 显式转换（强制转换）
- 显式转换要求使用**强制转换运算符**，即 `(目标类型)`，因为这种转换可能会**丢失数据**或导致异常。
- 这种转换用于从大范围的数值类型转换为小范围的数值类型，或者从一种兼容的类型转换为另一种。
- 示例：
```csharp
double a = 9.78;
int b = (int)a; // 显式转换，结果为 9
```
在这个例子中，将 `double` 显式转换为 `int` 会导致小数部分丢失。
### 3. 装箱和拆箱
- **装箱（Boxing）**：将**值类型**转换为**引用类型**（对象）的过程。会在堆上分配一个新的对象来存储该值类型的值。
```csharp
int num = 123;
object obj = num; // 装箱
```
- **拆箱（Unboxing）**：将已装箱的**对象**转换回**值类型**的过程。这需要显式地进行转换。
```csharp
object obj = 123;
int num = (int)obj; // 拆箱
```
### 4. 使用类型转换方法
C#提供了一些用于类型转换的内置方法和类，如 `Convert` 类、`Parse` 方法和 `TryParse` 方法。
- **Convert类**：可用于将基本数据类型之间进行转换。如果转换失败会抛出异常。
```csharp
string str = "123";
int num = Convert.ToInt32(str); // 将字符串转换为整数
```
- **Parse方法**：用于将字符串转换为对应的数值类型。如果转换失败会抛出异常。
```csharp
string str = "123.45";
double num = double.Parse(str); // 将字符串转换为 double
```
- **TryParse方法**：与 `Parse` 类似，但在转换失败时不会抛出异常，而是返回 `false`。
- 为什么 `TryParse` 使用 `out`
	`TryParse` 方法的设计初衷是尝试将字符串解析为数值类型（例如 `double`、`int` 等），并返回一个布尔值，表示转换是否成功。`out` 参数用于接收解析后的数值。
	- **如果解析成功**：`TryParse` 返回 `true`，并且通过 `out` 参数将解析后的结果赋值给调用者定义的变量。
	- **如果解析失败**：`TryParse` 返回 `false`，`out` 参数的值不会被赋给任何有效的数值（可能是类型的默认值，如 `0.0`）。
```csharp
string str = "123.45";
if (double.TryParse(str, out double result))
{
    Console.WriteLine($"转换成功，值为 {result}");
}
else
{
    Console.WriteLine("转换失败");
}
```
### 5. 类型转换的注意事项
- **兼容性**：转换前要确保类型兼容，不兼容的类型转换会导致编译错误或运行时异常。
- **数据丢失**：从大范围类型转换为小范围类型可能会丢失数据（例如，浮点数转换为整数会丢失小数部分）。
- **异常处理**：使用显式转换时，建议进行异常处理以防止转换失败导致程序崩溃。
### as呢
`as` 关键字在C#中也用于类型转换，但它的用法和普通的显式类型转换有一些区别。虽然它也可以实现类型转换的功能，但它的工作方式与传统的强制转换有所不同。
#### `as` 关键字的特点
1. **用于引用类型和可空类型的转换**：`as` 关键字只能用于将一个**引用类型**或**可空类型**转换为另一种引用类型或可空类型。对于值类型的转换，不能使用 `as`。
2. **安全的类型转换**：`as` 是一种**安全的类型转换**方法，如果转换失败，它会返回 `null`，而不是抛出异常。传统的强制转换在转换失败时会抛出 `InvalidCastException` 异常。
3. **转换失败返回 null**：当类型转换无法进行时，`as` 会返回 `null`，这意味着你可以通过检查转换结果是否为 `null` 来判断转换是否成功。
#### 示例
```csharp
object obj = "Hello, World!";
string str = obj as string; // 成功转换，str 现在是 "Hello, World!"

object num = 123;
string str2 = num as string; // 转换失败，str2 为 null
```
在这个例子中，`obj` 是一个字符串，因此可以使用 `as` 转换为 `string` 类型。而 `num` 是一个整数，不能转换为 `string`，所以 `as` 返回 `null`。
#### 与强制转换的对比
- **强制转换**（`(目标类型)obj`）在转换失败时会抛出异常：
```csharp
object num = 123;
string str = (string)num; // 运行时会抛出 InvalidCastException
```
- **`as` 转换**则会返回 `null`，而不会抛出异常：
```csharp
object num = 123;
string str = num as string; // 转换失败，str 为 null
```
#### 使用场景
- 当你不确定转换是否会成功时，使用 `as` 可以避免异常，并通过 `null` 检查来判断转换的结果。
- `as` 只能用于引用类型的转换，如果需要对值类型进行安全转换，可以结合 `Nullable<T>` 或使用其他方法（如 `TryParse`）。
#### 总结
`as` 可以看作是一种特殊的类型转换方式，主要用于引用类型的安全转换。它的主要特点是在转换失败时返回 `null` 而不是抛出异常，因此适合在不确定转换是否能成功的情况下使用。

# 37. 密封类（sealed class）
在C#中，**密封类**（`sealed class`）是一种不允许被继承的类。通过使用 `sealed` 关键字修饰类，可以防止其他类从该类派生。这样做的目的是增强安全性、提高性能或限制类的使用方式。
### 密封类的特点
1. **不能被继承**：密封类禁止其他类从它派生。这意味着你无法创建该类的子类。
2. **可以实例化**：虽然密封类不能被继承，但它仍然可以像普通类一样被实例化并使用。
3. **提高性能**：对于某些虚方法，JIT编译器可以针对密封类进行优化，因为它知道该方法不会被重写，从而减少方法调用的开销。
### 密封类的定义示例
```csharp
sealed class MySealedClass
{
    public void Display()
    {
        Console.WriteLine("This is a sealed class.");
    }
}

// 下面的代码会导致编译错误，因为 MySealedClass 是密封的，不能被继承
// class DerivedClass : MySealedClass
// {
// }
```
在这个例子中，`MySealedClass` 是一个密封类，禁止其他类从它派生。如果尝试继承这个类，会导致编译错误。
### 使用密封类的场景
1. **安全性需求**：当你希望某个类的功能不被扩展或修改时，可以将其声明为密封类。这样可以确保类的行为在使用时不会被子类改变。
2. **性能优化**：密封类在某些情况下可以提高性能，因为编译器可以针对它们进行优化，特别是方法调用。
3. **设计限制**：有时，设计一个不可扩展的类是必要的，例如一些工具类或帮助类，这些类的行为应该是固定的。
### 密封方法
除了密封类，C#还允许在继承类中将**方法**标记为密封。密封方法使用 `sealed` 关键字，并且必须是**重写**的（即继承自父类的虚方法或抽象方法）。

```csharp
class BaseClass
{
    public virtual void Display()
    {
        Console.WriteLine("Base class display method.");
    }
}

class DerivedClass : BaseClass
{
    public sealed override void Display()
    {
        Console.WriteLine("Derived class sealed display method.");
    }
}

// 不能进一步重写 Display 方法
// class AnotherDerivedClass : DerivedClass
// {
		// 如果关键字override变成new或者删除override，不会编译报错，这个是会隐藏DerivedClass的Display()方法
//     public override void Display() // 这会导致编译错误
//     {
//         Console.WriteLine("Another derived class display method.");
//     }
// }

```
在这个例子中，`DerivedClass` 重写了 `BaseClass` 的 `Display` 方法并将其密封，防止进一步的重写。
### 总结
- **密封类**：通过 `sealed` 关键字修饰的类，不能被继承，用于提高安全性和性能。
- **密封方法**：用 `sealed` 修饰的重写方法，防止其在子类中被进一步重写。
- 使用密封类和方法有助于控制类层次结构和优化性能。

# 38. 多态
**多态**（Polymorphism）是面向对象编程中的一个重要概念，它指的是**同一个方法或属性在不同的对象中可以有不同的实现**。多态性允许程序在不同的类中定义相同的接口或方法名称，并根据具体对象的类型来调用适当的方法。这种特性增强了代码的灵活性和可维护性。

### 多态的类型
在C#中，多态可以通过**编译时多态（静态多态）** 和**运行时多态（动态多态）** 来实现。
#### 1. 编译时多态（静态多态）
编译时多态通常通过**方法重载**和**运算符重载**来实现。
- **方法重载（Method Overloading）**：同一个类中可以定义多个具有相同名称但参数不同的方法。这些方法会根据传入的参数来决定调用哪个具体的实现。
```csharp
class MathOperations
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }
}

// 使用示例
MathOperations math = new MathOperations();
Console.WriteLine(math.Add(2, 3));       // 调用 Add(int, int)
Console.WriteLine(math.Add(2.5, 3.5));   // 调用 Add(double, double)
```
#### 2. 运行时多态（动态多态）
运行时多态通过**继承**和**虚方法**、**抽象类**或**接口**来实现。最常见的实现方式是**方法重写（Method Overriding）**。
- **虚方法和重写**：在基类中定义一个方法为 `virtual`，然后在派生类中使用 `override` 关键字重写这个方法。调用时根据对象的实际类型来决定执行哪个版本的方法。
- 当然，virtual虚函数也可以不重写，这样调用的就是父类的虚函数了
```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Some generic animal sound");
    }
}

class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Bark");
    }
}

class Cat : Animal
{
	// 不重写父类的MakeSound()虚函数
    //public override void MakeSound()
    //{
    //    Console.WriteLine("Meow");
    //}
}



class Program
{
    static void Main()
    {
        // 使用示例
        Animal myAnimal;
        myAnimal = new Dog();
        myAnimal.MakeSound(); // 输出 "Bark"

        myAnimal = new Cat();
        myAnimal.MakeSound(); // 输出 "Some generic animal sound"
    }
}
```
在这个例子中，`MakeSound` 方法在基类 `Animal` 中是虚方法，允许派生类 `Dog`  重写它。当我们调用 `MakeSound` 时，实际执行的是对象所属类型（`Dog`）的具体实现。
- **接口**与重写：在C#中，**接口**（Interface）是一种定义类和结构应遵循的契约。接口可以包含方法、属性、事件和索引器，但不能包含字段或实现。任何实现接口的类或结构都必须提供接口中定义的所有成员的实现。
- 接口不包含成员变量
- 接口包含的成员不能被实现
- 接口包含的成员访问修饰符可以不写，默认是public，但是不能写成私有
- 接口不能继承类，但是可以继承另一个接口
- 接口不能被实例化，但是可以作为容器存储对象
- 继承了接口，必须是public方式实现
- 接口也遵循历史转换原则
	- 接口的基本定义
		```csharp
		public interface IShape
		{
		    double Area();      // 定义一个方法，计算面积
		    void Draw();       // 定义一个方法，绘制形状
		}
		```
		在这个例子中，我们定义了一个名为 `IShape` 的接口，它包含两个方法：`Area` 和 `Draw`。继承结构的类，必须实现这两个方法，否则报错
	- 实现接口的类
		多个类可以实现同一个接口，并提供各自的实现。
		```csharp
		public class Circle : IShape
		{
		    public double Radius { get; set; }
		
		    public Circle(double radius)
		    {
		        Radius = radius;
		    }
		
		    public double Area()
		    {
		        return Math.PI * Radius * Radius; // 圆的面积公式
		    }
		
		    public void Draw()
		    {
		        Console.WriteLine("Drawing a Circle");
		    }
		}
		
		public class Rectangle : IShape
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
		        return Width * Height; // 矩形的面积公式
		    }
		
		    public void Draw()
		    {
		        Console.WriteLine("Drawing a Rectangle");
		    }
		}
		```
		在这个例子中，`Circle` 和 `Rectangle` 类实现了 `IShape` 接口。它们都提供了 `Area` 和 `Draw` 方法的具体实现。
	- 使用接口的示例
		通过接口类型，可以编写更灵活的代码，处理不同的实现。
		```csharp
		public class Program
		{
		    public static void Main(string[] args)
		    {
		        IShape circle = new Circle(5);
		        IShape rectangle = new Rectangle(4, 6);
		
		        Console.WriteLine($"Circle Area: {circle.Area()}"); // 调用圆的面积方法
		        circle.Draw(); // 绘制圆
		
		        Console.WriteLine($"Rectangle Area: {rectangle.Area()}"); // 调用矩形的面积方法
		        rectangle.Draw(); // 绘制矩形
		    }
		}
		```
	- 输出结果
		```mathematica
		Circle Area: 78.53981633974483
		Drawing a Circle
		Rectangle Area: 24
		Drawing a Rectangle
		```
	- 其他示例
		```csharp
		using System;

// 定义接口 IShape
public interface IShape
{
    // 方法声明
    void Draw();

    // 属性声明
    double Area { get; }

    // 事件声明
    event EventHandler ShapeChanged;

    // 索引器声明
    double this[int index] { get; set; }
}

// 实现接口的类 Circle
public class Circle : IShape
{
    public double Radius { get; set; }

    public Circle(double radius)
    {
        Radius = radius;
    }

    // 实现 Draw 方法
    public void Draw()
    {
        Console.WriteLine($"Drawing a circle with radius: {Radius}");
    }

    // 实现 Area 属性
    public double Area => Math.PI * Radius * Radius;

    // 事件的实现
    public event EventHandler ShapeChanged;

    // 索引器实现
    public double this[int index]
    {
        get
        {
            if (index == 0)
            {
                return Radius;
            }
            throw new IndexOutOfRangeException("Index out of range");
        }
        set
        {
            if (index == 0)
            {
                Radius = value;
                OnShapeChanged(); // 触发事件
            }
            else
            {
                throw new IndexOutOfRangeException("Index out of range");
            }
        }
    }

    // 触发 ShapeChanged 事件的方法
    protected virtual void OnShapeChanged()
    {
        ShapeChanged?.Invoke(this, EventArgs.Empty);
    }
}

// 实现接口的类 Rectangle
public class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public Rectangle(double width, double height)
    {
        Width = width;
        Height = height;
    }

    // 实现 Draw 方法
    public void Draw()
    {
        Console.WriteLine($"Drawing a rectangle with width: {Width} and height: {Height}");
    }

    // 实现 Area 属性
    public double Area => Width * Height;

    // 事件的实现 
    // EventHandler是自带的事件
    // public delegate void EventHandler(object? sender, EventArgs e);
    public event EventHandler ShapeChanged;

    // 索引器实现
    public double this[int index]
    {
        get
        {
            if (index == 0)
            {
                return Width;
            }
            else if (index == 1)
            {
                return Height;
            }
            throw new IndexOutOfRangeException("Index out of range");
        }
        set
        {
            if (index == 0)
            {
                Width = value;
                OnShapeChanged(); // 触发事件
            }
            else if (index == 1)
            {
                Height = value;
                OnShapeChanged(); // 触发事件
            }
            else
            {
                throw new IndexOutOfRangeException("Index out of range");
            }
        }
    }

    // 触发 ShapeChanged 事件的方法
    protected virtual void OnShapeChanged()
    {
        ShapeChanged?.Invoke(this, EventArgs.Empty);
    }
}

// 主程序
public class Program
{
    public static void Main()
    {
        IShape circle = new Circle(5);
        circle.Draw(); // 输出 "Drawing a circle with radius: 5"
        Console.WriteLine($"Circle Area: {circle.Area}"); // 输出圆的面积

        IShape rectangle = new Rectangle(4, 6);
        rectangle.Draw(); // 输出 "Drawing a rectangle with width: 4 and height: 6"
        Console.WriteLine($"Rectangle Area: {rectangle.Area}"); // 输出矩形的面积

        // 使用索引器
        circle[0] = 7; // 修改半径
        Console.WriteLine($"Updated Circle Area: {circle.Area}"); // 输出更新后的面积

        // 订阅事件
        rectangle.ShapeChanged += (sender, e) => Console.WriteLine("Rectangle shape changed!");
        rectangle[0] = 5; // 修改宽度，触发事件
    }
}
		```
	- 如果两个接口中存在同名方法时，需要显式实现接口
	- 显式实现接口时，不能写访问修饰符
		```csharp
		using System;
		
		// 定义接口 IShape
		public interface IShape
		{
		    void Draw(); // 方法声明
		}
		
		// 定义接口 IFigure
		public interface IFigure
		{
		    void Draw(); // 方法声明
		}
		
		// 实现接口的类 Circle
		public class Circle : IShape, IFigure
		{
		    // 显式实现 IShape 的 Draw 方法
		    void IShape.Draw()
		    {
		        Console.WriteLine("Drawing a shape as IShape.");
		    }
		
		    // 显式实现 IFigure 的 Draw 方法
		    void IFigure.Draw()
		    {
		        Console.WriteLine("Drawing a shape as IFigure.");
		    }
		}
		
		public class Program
		{
		    public static void Main()
		    {
		        // 创建 Circle 的实例
		        Circle circle = new Circle();
		
		        // 使用 IShape 接口调用 Draw 方法
		        IShape shape = circle;
		        shape.Draw(); // 输出 "Drawing a shape as IShape."
		
		        // 使用 IFigure 接口调用 Draw 方法
		        IFigure figure = circle;
		        figure.Draw(); // 输出 "Drawing a shape as IFigure."
		    }
		}
		```
	- 接口的优点
		1. **松耦合**：接口允许类之间松散耦合，方便替换和扩展。你可以在不修改现有代码的情况下，引入新的实现。
		2. **多态**：通过接口可以实现多态性，允许不同的对象以相同的方式进行处理。
		3. **强制实现**：接口强制实现类遵循特定的协议，从而确保一致性。
	- 继承多个接口
		C#允许一个类实现多个接口，从而支持多重继承的特性。
		```csharp
		public interface IDrawable
		{
		    void Draw();
		}
		
		public interface IColorable
		{
		    void Color(string color);
		}
		
		public class ColoredCircle : IShape, IDrawable, IColorable
		{
		    public double Radius { get; set; }
		
		    public ColoredCircle(double radius)
		    {
		        Radius = radius;
		    }
		
		    public double Area()
		    {
		        return Math.PI * Radius * Radius;
		    }
		
		    public void Draw()
		    {
		        Console.WriteLine("Drawing a Colored Circle");
		    }
		
		    public void Color(string color)
		    {
		        Console.WriteLine($"Coloring the circle with {color}");
		    }
		}
		```


- **抽象类和抽象方法**：如果一个类中有抽象方法（没有方法体的方法），该类必须被声明为抽象类，不能被直接实例化，必须由派生类实现抽象方法。
- 抽象类不可以被实例化，但是遵循里氏转换原则，可以作为父类接受子类
- 抽象类需要用abstract关键字声明，放在class前面
- 抽象类可以没有抽象方法，但是抽象方法必须在抽象类里面
- 抽象方法不能是私有的，不然无法继承后重写。抽象方法一定是没有函数体的。且必须在抽象类中
- 如果继承了抽象类，子类不是抽象类，则必须实现抽象类，否则可以不实现
- 如果某个子类，继承的父类已经实现了抽象函数，那么可以直接继承，不继续实现。如果实现，用的是override，和普通的实现抽象函数一样，多态。如果使用的是new，则是和普通的重载规则一样，具体下面注释写了
```csharp
abstract class Shape
{
    public abstract void Draw();
}

abstract class Shape2
{
    public void Draw()
    {
        Console.WriteLine("Shape2");
    }
}

class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}

// 如果不实现抽象类Shape中的Draw方法，也必须声明为abstract类
abstract class Rectangle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a rectangle");
    }
}

// 如果这里继承实现了Draw，使用new关键字，下面是用Shape接受，那么会使用Rectangle的方法，输出"Drawing a rectangle"
class Rectangle2 : Rectangle
{ }

class Rectangle3 : Shape2
{ }

class Program
{
    static void Main()
    {
        // 使用示例
        Shape shape;
        shape = new Circle();
        shape.Draw(); // 输出 "Drawing a circle"

        shape = new Rectangle2();
        shape.Draw(); // 输出 "Drawing a rectangle"

        Shape2 shape2;
        shape2 = new Rectangle3();
        shape2.Draw(); //输出 "Shape2"
    }
}
```
### 多态的优点
1. **代码可维护性高**：多态可以让程序在不修改现有代码的情况下，方便地扩展新的功能。例如，新增一种 `Shape` 类型的子类只需实现 `Draw` 方法，不影响其他代码。
2. **提高代码的灵活性和可扩展性**：通过接口或基类编程，可以编写通用的代码，这些代码可以应用于各种具体的对象。
3. **面向接口编程**：多态性鼓励面向接口编程，而不是面向实现编程，从而提高系统的可扩展性。
### 多态的实现注意事项
- 当使用虚方法或抽象方法时，子类应使用 `override` 关键字重写基类方法。
- 接口方法在实现时也类似于虚方法，可以实现多态。

### 抽象类和接口的区别
抽象类和接口在面向对象编程中用于定义抽象的类型成员，帮助实现多态性，但它们有不同的特点和使用场景。以下是抽象类和接口之间的主要区别：
#### 1. **定义方式**
- **抽象类**：使用 `abstract` 关键字定义，允许包含抽象方法（没有实现）和非抽象方法（有实现）。
- **接口**：使用 `interface` 关键字定义，不能包含任何方法的实现（从 C# 8.0 开始，接口可以包含默认实现）。
	- 如果接口是类的唯一父类，如果用这个父类接收，可以调用其中原有的方法
	- 接口中的方法，只能是public，不能是private，protected的话不可以在类外使用，也不可以用virtual
	```csharp
	interface Itest
	{
	    public void show()
	    {
	        Console.WriteLine("interface Itest 的默认实现");
	    }
	}
	
	public class Test : Itest
	{
	}
	
	public class Program
	{
	    public static void Main()
	    {
	        Test test = new Test();
	        //test.show();// 报错
	
	        // 通过接口引用来调用默认实现的方法
	        Itest itest = test;
	        itest.show(); // 输出: interface Itest 的默认实现
	    }
	}
	```
#### 2. **继承关系**
- **抽象类**：支持单继承，即一个类只能继承一个抽象类。
- **接口**：支持多继承，一个类可以实现多个接口。
- 如果是抽象类和接口都继承，也就是一个类继承了单个类和多个接口，那么其中的父类，只能调用自己相关的方法，而不能调用其他接口或者类的方法
	```csharp
	public class BaseClass
	{
	    public void BaseMethod()
	    {
	        Console.WriteLine("Method from BaseClass");
	    }
	}
	
	interface IInterface1
	{
	    void InterfaceMethod();
	}
	
	public class DerivedClass : BaseClass, IInterface1
	{
	    public void InterfaceMethod()
	    {
	        Console.WriteLine("Method from IInterface1");
	    }
	}
	
	public class Program
	{
	    public static void Main()
	    {
	        DerivedClass derivedClass = new DerivedClass();
	        derivedClass.BaseMethod();        // 输出: Method from BaseClass
	        derivedClass.InterfaceMethod();   // 输出: Method from IInterface1
	
	
	        BaseClass derivedClass2 = new DerivedClass();
	        derivedClass2.BaseMethod();        // 输出: Method from BaseClass
	        // derivedClass2.InterfaceMethod();   // 报错
	
	        IInterface1 derivedClass3 = new DerivedClass();
	        // derivedClass3.BaseMethod();        // 报错
	        derivedClass3.InterfaceMethod();   // 输出: Method from IInterface1
	    }
	}
	```
#### 3. **成员类型**
- **抽象类**：可以包含字段、属性、方法、构造函数、事件等成员，可以有访问修饰符（如 `public`、`protected`）。
- **接口**：不能包含字段或构造函数，成员默认是 `public`，且只能包含方法、属性、事件和索引器的声明（在 C# 8.0 之前）。从 C# 8.0 开始，接口可以包含静态方法、默认实现、属性和事件。(这个8.0之前和之后的属性和事件没区别，只是允许有默认实现了)
#### 4. **实现方式**
- **抽象类**：子类继承抽象类并实现其抽象方法，可以使用 `override` 关键字来实现父类的抽象方法。
- **接口**：实现接口的类必须实现接口中定义的所有成员，不能使用 `override` 关键字。
#### 5. **构造函数**
- **抽象类**：可以有构造函数，用于初始化抽象类的字段。不能创建抽象类的实例。
- **接口**：不能有构造函数，不能创建接口的实例。
#### 6. **使用场景**
- **抽象类**：适用于描述具有相似属性和行为的类，并且需要在基类中提供一些默认实现时。
- **接口**：适用于定义一组不相关类的行为约定，或者需要多重继承时。
#### 7. **字段支持**
- **抽象类**：可以定义字段。
- **接口**：不能定义字段，接口的成员只能是方法、属性、事件或索引器的签名。
#### 8. **访问修饰符**
- **抽象类**：可以为成员指定不同的访问修饰符（`public`、`protected`、`private`）。
- **接口**：接口成员默认是 `public`，不能使用其他访问修饰符。
#### 例子
##### 抽象类
```csharp
public abstract class Animal
{
    public string Name { get; set; }
    public abstract void Speak(); // 抽象方法，没有实现

    public void Eat() // 非抽象方法，有实现
    {
        Console.WriteLine($"{Name} is eating.");
    }
}

public class Dog : Animal
{
    public override void Speak() // 必须实现抽象方法
    {
        Console.WriteLine("Woof!");
    }
}
```
##### 接口
```csharp
public interface IMovable
{
    void Move(); // 接口成员没有实现
}

public interface ISpeakable
{
    void Speak();
}

public class Robot : IMovable, ISpeakable
{
    public void Move() // 实现接口方法
    {
        Console.WriteLine("Robot is moving.");
    }

    public void Speak() // 实现接口方法
    {
        Console.WriteLine("Beep beep!");
    }
}
```
#### 总结
- 抽象类适合有默认行为的基类，而接口适合定义无关类的契约。
- 使用抽象类可以提供部分实现，而接口更注重行为规范的定义和多继承的支持。

### 总结
多态是面向对象编程的核心概念之一，它可以通过方法重载、方法重写、抽象类和接口等多种方式实现。编译时多态主要通过方法重载和运算符重载实现，而运行时多态通过继承、虚方法、抽象类和接口实现。它使得代码更加灵活、易于扩展和维护。
# 参考
[MS-Learning](https://learn.microsoft.com/zh-cn/)
[解决方案、项目、程序集、命名空间之间的联系与区别](https://blog.csdn.net/zjzzjzzjzzjzzjz/article/details/8984057) 
[c# 重载++（前后）](https://zhidao.baidu.com/question/1446226042843310700.html?qbl=relate_question_0)