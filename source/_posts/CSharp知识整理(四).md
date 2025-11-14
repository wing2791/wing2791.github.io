---
title: CSharp知识整理(四)
math: true
---

# 一、 协变(out)和逆变(in)
协变（covariance）和逆变（contravariance）是泛型类型参数的一种变换方式，主要用于解决泛型类型的继承关系，特别是在处理委托和接口时。这些概念在 C# 中尤其重要，因为它们帮助我们在类型安全的同时实现灵活的类型转换。
- out一般指返回值
- in一般指参数
- 都是里氏转换原则，out是使用父类接收子类，没问题
- in是将参数为父类的赋值为参数为子类的，但是调用的时候还是子类进行调用，传入的参数也是子类。但是由于子类的函数是父类，此时就是父类的参数接收子类，符合里氏转换原则。可以看文后的参考[C# - 协变、逆变 看完这篇就懂了](https://www.cnblogs.com/VVStudy/p/11404300.html "发布于 2019-08-28 12:04")
### 1. 协变（Covariance）
**协变**允许你使用更具体的类型替换泛型类型参数。例如，协变允许你将一个返回类型为派生类的方法赋值给一个返回类型为基类的方法。
#### 示例
```csharp
// 基类
class Animal { }

// 派生类
class Dog : Animal { }

// 协变的委托
delegate TOutput MyDelegate<out TOutput>();

// 使用协变
MyDelegate<Dog> dogDelegate = () => new Dog(); // 可以使用 Dog
MyDelegate<Animal> animalDelegate = dogDelegate; // 可以向上转型到 Animal
```

在上面的示例中，`MyDelegate<out TOutput>` 中的 `out` 关键字指示该类型参数是协变的，这意味着它只会出现在返回类型中。
### 2. 逆变（Contravariance）
**逆变**允许你使用更一般的类型替换泛型类型参数。例如，逆变允许你将一个接受派生类的方法赋值给一个接受基类的方法。
#### 示例
```csharp
// 基类
class Animal { }

// 派生类
class Dog : Animal { }

// 逆变的委托
delegate void MyAction<in TInput>(TInput input);

class Program
{
    static void HandleAnimal(Animal animal)
    {
        Console.WriteLine("Handling animal");
    }

    static void Main()
    {
        // 创建逆变的委托
        MyAction<Animal> animalAction = HandleAnimal;
        MyAction<Dog> dogAction = animalAction; // 使用逆变

        // 这段代码不会报错，因为 HandleAnimal 可以接受 Dog
        dogAction(new Dog()); // 输出: Handling animal
    }
}
```
### 3. 总结
- **协变**（使用 `out`）:
    - 适用于返回类型。
    - 可以将派生类赋值给基类。
- **逆变**（使用 `in`）:
    - 适用于参数类型。
    - 可以将基类赋值给派生类。
### 4. 应用场景
- **协变**通常用于处理返回值的场景，比如泛型集合的查询操作。
- **逆变**通常用于处理接受参数的场景，比如事件处理和回调函数。

# 二、 object和Object的区别
在C#中，`object`和`Object`指向的是同一个类型，但它们的用法和意义略有不同：
### 1. `object`（小写）
- `object` 是 C# 的关键字，表示 .NET 类型系统中的基类 `System.Object`。
- 它是所有类型（包括值类型和引用类型）的基类，意味着任何类型都可以作为 `object` 的实例。
- 在代码中，使用 `object` 关键字是表示类型时的常见做法。
例如：
```csharp
object myObject = "Hello, world!";
```
### 2. `Object`（大写）
- `Object` 是 .NET 框架中的类，完整名称是 `System.Object`。
- 它位于 `System` 命名空间中，是所有类型在 .NET 类型系统中的根类型。
- 可以通过 `System.Object` 直接引用它，但通常情况下使用 `object` 关键字。
例如：
```csharp
System.Object myObject = "Hello, world!";
```
### 3. 主要区别
- `object` 是关键字，作为语法糖使用，更简洁。
- `Object` 是 .NET 类，可以通过完全限定名 `System.Object` 访问。
在大多数场合下，两者是可以互换使用的，但通常推荐使用 `object` 关键字，因为它更符合 C# 的编码风格。

# 三、 匿名函数
匿名函数是指没有名称的函数，通常用于简化代码、实现短小的功能，尤其是在处理回调、事件或作为`参数传递`时。C# 支持两种匿名函数的形式：**匿名方法** 和 **Lambda 表达式**。
- 脱离委托和事件，是不会使用匿名函数的
- 添加到委托或者事件容器后，不记录，无法单独移除，只能Clear()
### 1. 匿名方法
匿名方法使用 `delegate` 关键字来定义一个没有名称的函数。适用于不需要重用的简单逻辑。
示例：
```csharp
// 使用匿名方法定义一个委托
Func<int, int, int> add = delegate (int a, int b)
{
    return a + b;
};

// 调用匿名方法
int result = add(3, 4);
Console.WriteLine(result); // 输出 7
```
### 2. Lambda 表达式
Lambda 表达式是一种更简洁的语法形式，用于编写匿名函数。语法为 `(参数列表) => 表达式或语句块`。
示例：
```csharp
// 使用 Lambda 表达式定义一个委托
// 也可以写(int a,int b)=>a+b;
// 参数类型和委托或事件一致
Func<int, int, int> add = (a, b) => a + b;

// 调用 Lambda 表达式
int result = add(3, 4);
Console.WriteLine(result); // 输出 7
```
#### 带块体的 Lambda 表达式
如果 Lambda 表达式包含多条语句，可以用 `{}` 包围，并显式使用 `return` 返回值。
```csharp
Func<int, int, int> add = (a, b) =>
{
    Console.WriteLine("Adding numbers");
    return a + b;
};

int result = add(3, 4);
Console.WriteLine(result); // 输出 7
```
### 3. 匿名方法与 Lambda 表达式的区别
- **语法**：Lambda 表达式的语法更加简洁，通常更易于阅读。
- **特性**：Lambda 表达式支持类型推断，不需要显式指定参数的类型。
- **捕获外部变量**：匿名方法和 Lambda 表达式都可以捕获外部作用域中的变量。

# 四、 闭包
- 闭包是一个编程概念，指的是一个函数可以捕获并访问其所在作用域（外部函数或环境）中的变量，即使在这些变量的生命周期结束后，该函数依然可以访问这些变量的值。闭包可以让函数“记住”创建它时的环境。
- 在 C# 中，闭包通常与匿名函数（匿名方法或 Lambda 表达式）结合使用。当一个匿名函数捕获了外部作用域中的变量时，就形成了一个闭包。这些捕获的变量会与匿名函数一起存储，即使这些变量在创建闭包的作用域中已经不存在，闭包仍然可以访问它们。
### 闭包的示例
```csharp
Func<int, int> makeAdder(int x)
{
    // 返回一个 Lambda 表达式，捕获外部变量 x
    // 这里y被外部使用，作为了返回值，所以其声明周期和makeAdder一样
    return y => x + y;
}

var addFive = makeAdder(5);//此时addFive中的y为5
Console.WriteLine(addFive(3)); // 输出 8

var addTen = makeAdder(10);//此时addTen中的y为10
Console.WriteLine(addTen(3)); // 输出 13
```
在上面的示例中：
1. `makeAdder` 方法返回一个 `Func<int, int>` 类型的 Lambda 表达式。
2. 这个 Lambda 表达式捕获了 `makeAdder` 方法的参数 `x`，即形成了一个闭包。
3. 当 `addFive` 和 `addTen` 调用时，即使 `makeAdder` 方法已经执行完毕并退出，其返回的 Lambda 表达式依然可以访问被捕获的 `x` 值（分别为 5 和 10）。
### 闭包的特点
1. **捕获变量而非值**：闭包捕获的是外部变量的引用，而不是它的值。这意味着如果外部变量的值发生变化，闭包访问的值也会相应改变。
2. **持久性**：即使捕获变量所在的作用域已经结束，闭包仍然可以访问这些变量。
### 捕获变量的行为
闭包在捕获变量时会捕获其引用，而不是当前值。这可能会导致意料之外的行为：
```csharp
List<Func<int>> actions = new List<Func<int>>();

for (int i = 0; i < 3; i++)
{
    actions.Add(() => i);
}

foreach (var action in actions)
{
    Console.WriteLine(action()); // 输出 3, 3, 3，而不是 0, 1, 2
}
```
在这个示例中，`i` 被捕获为引用，因此当循环结束时，`i` 的最终值是 3，因此所有的闭包都会返回 3。
### 解决上述问题的方式
可以使用一个临时变量来解决这个问题，让闭包捕获不同的值：
```csharp
for (int i = 0; i < 3; i++)
{
    int temp = i; // 临时变量，闭包捕获的是 temp
    actions.Add(() => temp);
}

foreach (var action in actions)
{
    Console.WriteLine(action()); // 输出 0, 1, 2
}
```
### 总结
闭包是一个非常强大的概念，可以使得函数在定义时记住它们的环境。闭包与匿名函数的结合使用为 C# 增添了函数式编程的特性，使得代码更加灵活和简洁。

# 五、  多线程
多线程是指在一个程序中同时执行多个线程的技术。线程是操作系统调度的最小单位，多个线程共享同一进程的内存空间，每个线程都有自己的栈空间和寄存器上下文。多线程可以提高程序的并发性和响应速度，尤其在处理 I/O 操作、长时间运行的任务或用户界面响应时非常有用。
- 默认是前台线程t，主线程会等待t线程结束，如果t线程不结束，主线程就不会结束。t设置为后台线程以后，Main主线程结束以后，t线程也会一起结束。
### 在 C# 中使用多线程
在 C# 中，可以使用多种方法来实现多线程编程。常见的方法包括：
1. **`Thread` 类**
2. **`Task` 并行库**
3. **异步编程 (`async`/`await`)**
4. **线程池**
#### 1. `Thread` 类
`Thread` 类是最基本的创建和管理线程的方法。可以使用 `Thread` 类来创建一个新线程并启动它。
```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // 创建一个线程，执行方法为 PrintNumbers
        Thread thread = new Thread(PrintNumbers);
        thread.IsBackground = true;
        thread.Start(); // 启动线程


        // 主线程继续执行其他任务
        Console.WriteLine("主线程继续运行...");
        // 注释下面这行，并且thread为后台线程，一旦主线程运行完毕，则主线程会自动退出，子线程也会结束
        thread.Join(); // 等待子线程执行完成,此时主线程在这里暂停不动
        Console.WriteLine("主线程继续运行2...");

    }

    static void PrintNumbers()
    {
        for (int i = 1; i <= 5; i++)
        {
            Console.WriteLine($"子线程输出: {i}");
            Thread.Sleep(500); // 让线程休眠500毫秒
        }
    }
}
```
- `thread.Join()` 方法用于阻塞调用线程（通常是主线程），直到指定的线程（子线程）完成执行。这意味着，调用 `Join()` 的线程会等待子线程执行完成后再继续执行后续的代码。
- 运行结果
```csharp
主线程继续运行...
子线程输出: 1
子线程输出: 2
子线程输出: 3
子线程输出: 4
子线程输出: 5
主线程继续运行2...
```
#### 2. `Task` 并行库
`Task` 类提供了更高级的抽象，用于处理并行编程。与 `Thread` 类相比，`Task` 更加灵活，并且能够更好地管理线程的生命周期。
```csharp
using System;
using System.Text;
using System.Threading.Tasks;

class Program
{
    // 定义一个 Program 类，并声明一个 Main 方法，该方法返回 Task。
    // 由于它被标记为 async，表示它可以使用 await 关键字。
    static async Task Main()
    {
        // 启动一个异步任务
        // 使用 Task.Run 方法启动一个新的任务。这个任务会执行 PrintNumbers 方法。
        // Task.Run 方法用于在线程池中异步执行代码，因此 PrintNumbers 方法会在一个独立的线程上运行，
        // 而主线程不会被阻塞。
        Task task = Task.Run(() => PrintNumbers());

        Console.WriteLine("主线程继续运行...");
        // 使用 await 关键字等待 task 完成。此时，主线程会在 await 处暂停，
        // 直到 PrintNumbers 任务执行完毕。此操作确保在程序结束之前，所有的异步操作都已经完成。
        await task; // 等待任务完成,不会继续运行下面的主线程代码
        Console.WriteLine("主线程继续运行2...");

    }

    static void PrintNumbers()
    {
        for (int i = 1; i <= 5; i++)
        {
            Console.WriteLine($"Task 输出: {i}");
            Task.Delay(500).Wait(); // 等待500毫秒
        }
    }
}
```
#### 3. 异步编程 (`async`/`await`)
C# 提供了异步编程的支持，通过 `async` 和 `await` 关键字，可以方便地进行异步操作，特别适合 I/O 密集型任务。
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        await PrintNumbersAsync(); // 阻塞，不会继续运行下面的代码
        Console.WriteLine("主线程继续运行...");
    }

    static async Task PrintNumbersAsync()
    {
        for (int i = 1; i <= 5; i++)
        {
            Console.WriteLine($"异步输出: {i}");
            await Task.Delay(500); // 异步等待500毫秒
        }
    }
}
```
#### 4. 线程池
线程池用于管理一组预先创建的线程，可以减少创建和销毁线程的开销。可以使用 `ThreadPool.QueueUserWorkItem` 方法将工作项排队到线程池。
```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // 将任务排队到线程池
        ThreadPool.QueueUserWorkItem(PrintNumbers);
        // 不会阻塞运行，主线程会继续运行，导致子线程没有运行完就结束了
        Console.WriteLine("主线程继续运行...");
        Thread.Sleep(3000); // 主线程等待一段时间以确保输出
    }

    static void PrintNumbers(object state)
    {
        for (int i = 1; i <= 5; i++)
        {
            Console.WriteLine($"线程池输出: {i}");
            Thread.Sleep(500); // 等待500毫秒
        }
    }
}
```
##### 线程池的线程数量
线程池的线程数量在 .NET 中是动态管理的，通常不会固定在一个特定的数量上。以下是一些关于线程池的关键信息：
###### 1. 动态调整
- 线程池会根据当前的工作负载动态增加或减少线程数量。
- 当工作线程的数量低于线程池的最低数量时，线程池会创建新线程来处理新的任务。
- 当没有任务需要处理时，线程池会减少空闲线程的数量，以释放系统资源。
###### 2. 最大线程数量
- 线程池的最大线程数量可以通过 `ThreadPool.SetMaxThreads` 方法进行设置。默认情况下，这个上限通常是系统可以支持的最大线程数量，但具体的值可能依赖于系统配置和负载。
- `public static void SetMaxThreads(int workerThreads, int completionPortThreads);
	- **`workerThreads`**: 指定工作线程的最大数量。
	- **`completionPortThreads`**: 指定用于处理异步 IO 操作的最大线程数量。`
###### 3. 最小线程数量
- 线程池的最小线程数量也可以通过 `ThreadPool.SetMinThreads` 方法进行设置。这个值定义了线程池在没有任务时保持的最小线程数量。
- `ThreadPool.SetMinThreads(int workerThreads, int completionPortThreads)` 用于设置线程池中工作线程和异步 IO 线程的最小数量。
###### 4. 默认行为
- 在没有特殊配置的情况下，线程池会自动管理线程数量，通常在较低负载时只会保持几个线程。
- 当高负载时，线程池会创建更多线程以满足需求，具体数量会根据需要而变化。
###### 5. 线程池的优点
- 通过重用线程，减少了创建和销毁线程的开销，能够提高性能。
- 适用于处理大量短任务，避免了频繁创建和销毁线程的开销。
##### 示例
使用 `ThreadPool.QueueUserWorkItem` 提交任务的代码示例：
```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // 向线程池队列中添加多个任务
        for (int i = 0; i < 10; i++)
        {
            // i是PrintNumbers()的参数
            // 设置最大工作线程数量为 10，最大异步 IO 线程数量为 5
            // ThreadPool.SetMaxThreads(10, 5);
            // 如果你提交了 15 个计算任务（需要工作线程），
            // 线程池会调度最多 10 个工作线程来执行这些任务。
            // 同时，如果还有 10 个异步文件读取操作（需要异步 IO 线程），
            // 线程池最多会使用 5 个线程来处理这些异步 IO 操作。
            ThreadPool.QueueUserWorkItem(PrintNumbers, i);
        }

        Console.WriteLine("主线程继续运行...");
        // 等待用户输入以防程序提前结束
        Console.ReadLine();
    }

    static void PrintNumbers(object state)
    {
        int num = (int)state;
        for (int i = 1; i <= 5; i++)
        {
            Console.WriteLine($"任务 {num} 输出: {i}");
            Thread.Sleep(500); // 模拟工作负载
        }
    }
}
```
在这个例子中，主线程提交了多个任务到线程池，线程池会处理这些任务，而主线程则继续执行其他操作。具体有多少个线程被创建会根据线程池的管理策略而变化。
### 多线程的常见问题

- **线程安全**：多个线程访问共享资源时，可能会出现竞争条件，导致数据不一致。可以使用锁 (`lock`) 或同步机制解决。
- **死锁**：两个或多个线程互相等待对方释放资源，导致程序无法继续执行。
- **资源管理**：创建太多线程可能会耗尽系统资源，应该合理使用线程池或任务。

### 线程安全的例子

使用 `lock` 关键字防止多个线程同时访问共享资源：
```csharp
private static readonly object _lock = new object();
private static int _counter = 0;

static void IncrementCounter()
{
	// lock(引用类型对象)
    lock (_lock)
    {
        _counter++;
        Console.WriteLine($"计数器值: {_counter}");
    }
}
```

多线程是一个复杂而强大的工具，正确使用可以显著提高程序的性能和响应性。

# 六、 关闭死循环的线程
如果一个线程进入了死循环，需要通过合适的方法来关闭或释放它，以避免资源浪费或程序无法正常退出。可以使用以下几种方法来处理：
### 1. 使用线程间的信号（如 `bool` 标志位）
- 在线程执行的死循环中检查一个 `bool` 标志位，当需要退出时，设置这个标志位来结束循环，从而退出线程。
**示例**：
```csharp
bool keepRunning = true;

void ThreadMethod()
{
    while (keepRunning)
    {
        // 执行任务
        Thread.Sleep(100); // 模拟一些工作
    }
    Console.WriteLine("线程已终止");
}

Thread thread = new Thread(ThreadMethod);
thread.Start();

// 需要终止线程时，将标志位设为 false
keepRunning = false;
```
### 2. 使用 `CancellationToken`
- `CancellationToken` 是专为取消异步任务和线程而设计的机制，可以在需要时向线程发送取消请求。
**示例**：
```csharp
class Program
{
    static void Main()
    {
        CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();

        void ThreadMethod(CancellationToken token)
        {
            while (!token.IsCancellationRequested)
            {
                // 执行任务
                Thread.Sleep(100); // 模拟一些工作
            }
            Console.WriteLine("线程已取消");
        }

        Thread thread = new Thread(() => ThreadMethod(cancellationTokenSource.Token));
        thread.Start();

        // 需要终止线程时，调用 Cancel 方法
        cancellationTokenSource.Cancel();
    }
}
```

### 3. 使用 `Abort()` 方法（不推荐）

- 在非常特殊的情况下，可以调用 `Thread.Abort()` 强制终止线程，不过这种方法不安全，可能会导致资源没有正确释放和程序的不稳定性。
- 在 .NET Core 和 .NET 5 及更高版本中，`Thread.Abort()` 已被移除。
- `System.PlatformNotSupportedException:“Thread abort is not supported on this platform.”`
```csharp
Thread thread = new Thread();
thread.Abort();
thread = null;
```
### 4. 使用 `Join()` 等待线程退出
- 配合上述方法使用 `Join()` 等待线程完成，有助于确保线程在主线程结束前正确退出。
### 5. CancellationToken 感觉和bool标志位一样啊，有什么优势
`CancellationToken` 和 `bool` 标志位在某种程度上都可以用于控制线程的停止，但 `CancellationToken` 提供了一些更强大和灵活的功能。以下是它们之间的主要区别和 `CancellationToken` 的优势：
#### 1. **更强的并发控制**
- **CancellationToken** 可以与多个任务、线程或异步操作一起使用，允许它们共享同一个取消请求。这使得在复杂的并发场景中更容易管理任务的取消。
- **标志位** 通常只在单个线程中使用，不便于在多个线程之间共享。
#### 2. **内置机制**
- `CancellationToken` 是 .NET 中设计的一个专门用于取消操作的机制，提供了一种标准的方式来处理取消请求。它通过 `CancellationTokenSource` 提供了一个清晰的结构。
- 使用 **标志位** 时，开发者需要手动管理状态，容易引入错误，比如忘记检查标志位。
#### 3. **可取消操作的通知**
- 使用 `CancellationToken`，可以很方便地在执行操作时检查取消请求。例如，在异步方法中，可以传递 `CancellationToken`，在特定的点检查是否被取消。
- 通过 `CancellationToken`，还可以在需要的地方抛出 `OperationCanceledException`，提供更明确的取消处理逻辑。
#### 4. **与 Task 并行库的兼容性**
- `CancellationToken` 与 Task 并行库（TPL）紧密集成，可以在使用 `Task` 的场景中直接使用，比如 `Task.Run`、`async/await` 结构中，允许更自然的取消行为。
- **标志位** 需要额外的逻辑来与 `Task` 一起使用，整合不如 `CancellationToken` 流畅。
#### 5. **异步操作的支持**
- 在异步编程中，`CancellationToken` 是非常有用的，可以在执行异步操作时轻松管理取消逻辑。
- 使用 **标志位** 可能会导致代码的可读性下降，因为你需要在每个异步方法中手动传递和检查标志位。
##### 使用 `CancellationToken`
```csharp
CancellationTokenSource cts = new CancellationTokenSource();
CancellationToken token = cts.Token;

Task.Run(() =>
{
    for (int i = 0; i < 100; i++)
    {
        token.ThrowIfCancellationRequested();
        Console.WriteLine(i);
        Thread.Sleep(100);
    }
}, token);

// 取消操作
cts.Cancel();
```
##### 使用 `bool` 标志位
```csharp
bool keepRunning = true;

Task.Run(() =>
{
    for (int i = 0; i < 100; i++)
    {
        if (!keepRunning) return;
        Console.WriteLine(i);
        Thread.Sleep(100);
    }
});

// 取消操作
keepRunning = false;
```
##### 总结
- **CancellationToken** 提供了一个更强大和灵活的方式来管理取消请求，特别是在复杂的并发和异步编程中。
- 虽然 `bool` 标志位可以简单地实现基本的取消逻辑，但在需要更复杂或可重用的取消机制时，`CancellationToken` 是更推荐的选择。
### 总结
- **使用标志位和 `CancellationToken` 是更推荐的做法**，因为它们允许线程在执行任务时检查退出条件，能够安全地终止线程。
- **避免使用 `Thread.Abort()`**，除非没有其他选择。

# 七、 预处理指令
预处理器指令是编程语言中用于在编译期间进行条件编译、宏定义、文件包含等操作的特殊指令。在 C# 中，预处理器指令以 `#` 开头。以下是 C# 中一些常见的预处理器指令及其说明：
### 1. `#define` 和 `#undef`
- `#define`：定义一个符号，之后可以在代码中通过条件编译使用。
- `#undef`：取消定义一个符号。
```csharp
#define DEBUG

#if DEBUG
    Console.WriteLine("Debugging mode");
#endif

#undef DEBUG
```
### 2. `#if`, `#elif`, `#else`, `#endif`
- `#if`：根据条件编译代码块。可以与 `#define` 结合使用。
- `#elif`、`#else` 和 `#endif`：用于条件编译中的其他情况。
```csharp
#define DEBUG

#if DEBUG
    Console.WriteLine("Debugging mode");
#elif RELEASE
    Console.WriteLine("Release mode");
#else
    Console.WriteLine("No mode defined");
#endif
```
### 3. `#region` 和 `#endregion`
- `#region`：用于定义一个代码区域，可以在代码编辑器中折叠和展开。
- `#endregion`：结束一个区域的定义。
```csharp
#region MyRegion
public void MyMethod()
{
    // Method implementation
}
#endregion
```
### 4. `#error` 和 `#warning`
- `#error`：强制编译错误并输出指定的消息。
- `#warning`：发出编译警告并输出指定的消息。
```csharp
#error "This code should not be compiled"
#warning "This code should not be compiled"
```
### 5. `#line`
- `#line`**：用于设置行号和文件名。主要用于代码生成或预处理。
```csharp
#line 100 "MyFile.cs"
```
使用 `#line 100 "MyFile.cs"` 将后续代码的行号视为 100，并且指定了文件名为 `"MyFile.cs"`。因此，如果在这个方法中发生编译错误，编译器会报告这个错误位于 `MyFile.cs` 的第 100 行，而不是实际源文件中的行号。这对于代码生成和预处理非常有用，尤其是在使用代码生成器或动态生成代码的情况下。
### 6. `#pragma`
- `#pragma`：用于启用或禁用编译器特性，常用于控制警告的显示。
```csharp
#pragma warning disable 0168 // 忽略未使用的变量警告
// 0168是代码，具体可以参考编译器给的警告提示，或者文档说明
```

# 八、 反射
反射是 .NET 提供的一种强大功能，允许程序在运行时检查和操作类型信息，包括类、结构、接口、方法、属性等。通过反射，开发者可以动态地创建对象、调用方法、访问字段和属性，甚至可以修改它们的值。这在许多情况下非常有用，例如在插件系统、序列化和调试工具中。
- 在反射中，如果有方法重载（同名但参数不同的方法），`GetMethod` 方法必须精确选择某个重载方法，可以通过指定方法的参数类型来获取它。
### 反射的主要功能
1. **获取类型信息**：
    - 可以使用反射获取类的名称、基类、实现的接口、方法、属性、字段等信息。
2. **创建实例**：
    - 可以通过反射动态创建类型的实例，而不需要在编译时知道其类型。
3. **调用方法**：
    - 可以使用反射调用对象的方法，包括私有和保护方法。
4. **访问和修改字段和属性**：
    - 通过反射可以读取和设置对象的字段和属性值。可以是私有的
5. **动态加载程序集**：
    - 可以在运行时加载和使用外部程序集。
### 基本示例
以下是一些使用反射的示例代码：
```csharp
using System;
using System.Reflection;

public class MyClass
{
    public int MyProperty { get; set; }

    public void MyMethod(string message)
    {
        Console.WriteLine($"Message: {message}");
    }
}

class Program
{
    static void Main()
    {
        // 获取类型信息
        Type myType = typeof(MyClass);
        Console.WriteLine($"Class Name: {myType.Name}");

        // 创建实例
        object myObject = Activator.CreateInstance(myType);

        // 访问属性
        PropertyInfo propertyInfo = myType.GetProperty("MyProperty");
        propertyInfo.SetValue(myObject, 42);
        Console.WriteLine($"MyProperty Value: {propertyInfo.GetValue(myObject)}");

        // 调用方法
        MethodInfo methodInfo = myType.GetMethod("MyMethod");
        methodInfo.Invoke(myObject, new object[] { "Hello, Reflection!" });
    }
}
```
#### 代码解释
1. **获取类型信息**：
    - 使用 `typeof(MyClass)` 获取 `MyClass` 类型的 `Type` 对象。
2. **创建实例**：
    - 使用 `Activator.CreateInstance(myType)` 动态创建 `MyClass` 的实例。
3. **访问属性**：
    - 使用 `GetProperty` 获取 `MyProperty` 的 `PropertyInfo` 对象，使用 `SetValue` 和 `GetValue` 来设置和读取属性值。
4. **调用方法**：
    - 使用 `GetMethod` 获取 `MyMethod` 的 `MethodInfo` 对象，使用 `Invoke` 方法调用该方法。

### 基本示例2
以下是如何选择特定的重载方法的示例：
```csharp
using System;
using System.Reflection;

public class MyClass
{
    public int variable = 100;
    public int MyProperty { get; set; }

    public MyClass()
    {
        MyProperty = 1;
        Console.WriteLine("(): 构造函数");
    }

    public MyClass(int myProperty)
    {
        MyProperty = myProperty;
        Console.WriteLine("(int myProperty): 构造函数");
    }

    public MyClass(int myProperty, int i) : this(myProperty)
    {
        Console.WriteLine("(int myProperty,int i): 构造函数");
    }

    public void MyMethod(string message, int a)
    {
        Console.WriteLine($"Message: {message}, Number: {a}");
    }

    public void MyMethod(string message)
    {
        Console.WriteLine($"Message: {message}");
    }

    public static void show()
    {
        Console.WriteLine("静态show方法");
    }
}

class Program
{
    static void Main()
    {
        // 获取类型信息
        Type myType = typeof(MyClass);
        Type myType2 = (new MyClass()).GetType();
        // 如果有namespace，需要加上命名空间，如System.Int32
        // 这里的MyClass没有命名空间
        Type myType3 = Type.GetType("MyClass");
        Console.WriteLine($"Class Name: {myType.Name}");
        Console.WriteLine($"Class Name: {myType2.Name}");
        Console.WriteLine($"Class Name: {myType3.Name}");

        // 得到类的程序集信息
        Console.WriteLine($"Assembly Name: {myType.Assembly}");
        Console.WriteLine($"Assembly Name: {myType2.Assembly}");
        Console.WriteLine($"Assembly Name: {myType3.Assembly}");

        // 获取类中的所有公共成员
        MemberInfo[] infos = myType.GetMembers();
        // 这里使用的info变量，后面不允许用了，和C++不一样
        // 局部变量继续使用没问题
        foreach (var info in infos)
        {
            Console.WriteLine($"Member: {info}");
            Console.WriteLine($"Member Name: {info.Name}");
        }

        // 获取所有的构造函数
        ConstructorInfo[] ctors = myType.GetConstructors();
        foreach (var ctor in ctors)
        {
            Console.WriteLine($"Constructor: {ctor}");
            Console.WriteLine($"Constructor Name: {ctor.Name}");
        }
        // new Type[0] 创建了一个长度为 0 的 Type 数组。
        // 这表示我们在查找构造函数时，指定它没有任何参数。
        // 也就是说，我们想获取的是一个无参数的构造函数（也称为默认构造函数）。
        // new Type[] 表示构造函数的参数类型列表
        // new object[] 是用于实际调用方法或构造函数时传递的参数值
        // 获取一个无参构造函数
        ConstructorInfo infoNoneConstruct = myType.GetConstructor(new Type[0]);
        // 执行(int myProperty)构造
        MyClass myClass = infoNoneConstruct.Invoke(null) as MyClass;
        Console.WriteLine($"myClass.MyProperty : {myClass.MyProperty}");

        // 获取一个(int myProperty)构造函数
        ConstructorInfo infoOneConstruct = myType.GetConstructor(new Type[] { typeof(int) });
        // 执行无参构造，无参构造，没有参数，传null
        MyClass myClass1 = infoOneConstruct.Invoke(new object[] { 2 }) as MyClass;
        Console.WriteLine($"myClass1.MyProperty : {myClass1.MyProperty}");

        // 获取一个(int myProperty, int i)构造函数
        ConstructorInfo infoTwoConstruct = myType.GetConstructor(new Type[] { typeof(int), typeof(int) });
        // 执行(int myProperty, int i)构造
        MyClass myClass2 = infoTwoConstruct.Invoke(new object[] { 2, 3 }) as MyClass;
        Console.WriteLine($"myClass2.MyProperty : {myClass2.MyProperty}");

        // 获取类的公共成员变量
        FieldInfo[] fieldInfos = myType.GetFields();
        foreach (var fieldInfo in fieldInfos)
        {
            Console.WriteLine($"fieldInfos: {fieldInfo}");
        }
        // 得到指定名称的公共成员变量
        FieldInfo infoMyProperty = myType.GetField("variable");
        Console.WriteLine($"variable is {infoMyProperty}");

        // 通过反射获取和设置对象的值
        Console.WriteLine($"myClass的variable变量的值为 :{infoMyProperty.GetValue(myClass)}");
        // 通过反射设置指定对象的某个变量的值
        infoMyProperty.SetValue(myClass, 1000);
        Console.WriteLine($"myClass修改后的variable变量的值为 :{infoMyProperty.GetValue(myClass)}");


        // 创建实例
        object myObject = Activator.CreateInstance(myType);

        // 访问属性
        PropertyInfo propertyInfo = myType.GetProperty("MyProperty");
        propertyInfo.SetValue(myObject, 42);
        Console.WriteLine($"MyProperty Value: {propertyInfo.GetValue(myObject)}");

        // 如果方法被重载了，会报错System.Reflection.AmbiguousMatchException
        //MethodInfo methodDefault = myType.GetMethod("MyMethod");
        //methodDefault.Invoke(myObject, new object[] { "Hello, Reflection! Default Method!", 1234 });

        MethodInfo[] methods = myType.GetMethods();
        foreach (var method in methods)
        {
            Console.WriteLine($"method :{method}");
            Console.WriteLine($"method Name:{method.Name}");
        }

        // 调用重载方法MyMethod(string, int)
        MethodInfo methodInfoWithTwoParams = myType.GetMethod("MyMethod", new Type[] { typeof(string), typeof(int) });
        methodInfoWithTwoParams.Invoke(myObject, new object[] { "Hello, Reflection!", 123 });

        // 调用重载方法MyMethod(string)
        MethodInfo methodInfoWithOneParam = myType.GetMethod("MyMethod", new Type[] { typeof(string) });
        methodInfoWithOneParam.Invoke(myObject, new object[] { "Hello, Reflection!" });

        // 调用静态方法show()
        MethodInfo methodShow = myType.GetMethod("show", new Type[0]);
        // 第二个参数填null,new Type[0],new object[0]都可以，保证传入的参数是0个就行
        methodShow.Invoke(null, null);

        // 其他
        // 得枚举 GetEnumName GetEnumNames
        // 得事件 GetEvent GetEvents
        // 得接口 GetInterface GetInterfaces
        // 得属性 GetProperty GetPropertys
    }
}


```
#### 解释：
1. `GetMethod("MyMethod", new Type[] { typeof(string), typeof(int) })` 用于获取接受 `string` 和 `int` 参数的重载方法。
2. `GetMethod("MyMethod", new Type[] { typeof(string) })` 用于获取接受单个 `string` 参数的重载方法。
### 注意事项
- **性能开销**：反射会带来一定的性能开销，因此在性能敏感的代码中应谨慎使用。
- **类型安全**：使用反射时，由于许多操作在运行时进行，因此可能会引入类型安全问题。
### 应用场景
- **序列化/反序列化**：在将对象转换为字节流或从字节流恢复对象时，反射可以用于访问对象的字段和属性。
- **依赖注入**：在一些依赖注入框架中，反射用于创建服务的实例。
- **插件系统**：在插件架构中，反射可以用于加载和实例化插件类。

### `Activator`
`Activator` 是一个用于在运行时动态创建对象的类，通常用于反射或依赖注入的场景。`Activator.CreateInstance` 是其中一个常用的方法，用于创建一个指定类型的实例。下面是一些常见的使用方式和示例。
### 1. 创建无参数构造函数的实例
如果要创建一个使用无参数构造函数的类实例，可以这样做：
```csharp
using System;

public class MyClass
{
    public int Value { get; set; }

    public MyClass()
    {
        Value = 42;
    }
}

class Program
{
    static void Main()
    {
        // 原始对象
        MyClass originalObject = new MyClass();
        originalObject.Value = 100;
        Type targetType = originalObject.GetType();  // 可以是运行时才确定的类型

        // 动态创建实例
        object newObject = Activator.CreateInstance(targetType);

        // 将动态创建的实例转换为 MyClass 类型
        MyClass myClassInstance = newObject as MyClass;

        if (myClassInstance != null)
        {
            Console.WriteLine(myClassInstance.Value); // 输出 42
        }
        else
        {
            Console.WriteLine("创建实例失败");
        }
    }
}

```

在这个例子中，`Activator.CreateInstance` 创建了一个 `MyClass` 的实例，并调用了默认的无参数构造函数。
### 2. 创建带参数的构造函数的实例
如果需要调用带参数的构造函数，可以在 `CreateInstance` 中传递参数值：
```csharp
public class MyClass
{
    public int Value { get; set; }

    public MyClass(int value)
    {
        Value = value;
    }
    public MyClass(int value, int value2)
    {
        Value = value;
        Console.WriteLine($"{value2}");
    }
}

class Program
{
    static void Main()
    {
        // 使用 Activator 创建带参数的实例
        // 两个作用是一样得
        // MyClass myObject = (MyClass)Activator.CreateInstance(typeof(MyClass), new object[] { 99, 1 });
        MyClass myObject = (MyClass)Activator.CreateInstance(typeof(MyClass), 99, 1);
        Console.WriteLine(myObject.Value); // 输出 99
    }
}
```
这里，`new object[] { 99 }` 用于指定构造函数的参数，`Activator.CreateInstance` 会调用匹配的带参数的构造函数。
### 3. 创建泛型类型的实例
使用泛型方法 `Activator.CreateInstance<T>` 可以方便地创建实例：
- 这个只能用无参构造创建
```csharp
class Program
{
    static void Main()
    {
        // 泛型方法创建实例
        MyClass myObject = Activator.CreateInstance<MyClass>();
        Console.WriteLine(myObject.Value); // 输出 42
    }
}
```
这种方式适合无参数构造函数的情况，编译器会根据泛型参数自动推断类型。
### 4. 创建接口或抽象类的实例
`Activator.CreateInstance` 只能用于创建具体类型的实例，不能直接创建接口或抽象类的实例。如果尝试这样做，会抛出 `MissingMethodException`。为了通过接口或抽象类创建实例，可以使用依赖注入或工厂模式来完成。
### 5. 使用 `Activator` 创建私有构造函数的实例
如果要使用 `Activator` 调用私有的构造函数，可以使用反射来设置构造函数的访问权限：
```csharp
public class MyClass
{
    private MyClass()
    {
        Console.WriteLine("Private constructor called.");
    }
}

class Program
{
    static void Main()
    {
        // 使用反射调用私有构造函数
        var constructor = typeof(MyClass).GetConstructor(BindingFlags.Instance | BindingFlags.NonPublic, null, Type.EmptyTypes, null);
        MyClass myObject = (MyClass)constructor.Invoke(null);
    }
}
```
通过这些例子可以看出，`Activator` 在动态创建对象时非常灵活，能够适应各种场景。

# 九、 Assembly
- 程序集类
- 主要用来加载其它程序集，加载后才能用Type来使用其它程序集中的信息
- 如果想要使用不是自己程序集中的内容需要先加载程序集
- 比如dll文件（库文件）
- 简单的把库文件看成一种代码仓库，它提供给使用者一些可以直接拿来用的变量、函数或类//三种加载程序集的函数
- 一般用来加载在同一文件下的其它程序集
	- Assembly assembly2=Assembly.Load（“程序集名称"）；
- 一般用来加载不在同一文件下的其它程序集
	- `Assembly assembly=Assembly.LoadFrom(“包含程序集清单的文件的名称或路径");`
	- `Assemblyasembly3=Assembly.LoadFile(“要加载的文件的完全限定路径");`

### 示例
```csharp
using System.Reflection;

enum Dir
{
    Left,
    Right,
    Up,
    Down
}

class Test
{
    public string name;
    public int Age { get; set; }
    public Test()
    {
        Console.WriteLine("无参构造函数");
    }

    public Test(string _name, int _age)
    {
        name = _name;
        Age = _age;
        Console.WriteLine("(string _name, int _age)构造函数");
    }
}

class Program
{
    static void Main()
    {
        // 路径/项目名 就是程序集
        Assembly assembly = Assembly.LoadFrom(@"F:\C#\projectTest\ConsoleAppTest\bin\Debug\net8.0\ConsoleAppTest");

        Type[] types = assembly.GetTypes();
        for (int i = 0; i < types.Length; i++)
        {
            Console.WriteLine(types[i]);
        }
        // 命名空间.类名 这里没有命名空间，所以就不写命名空间了
        Type test = assembly.GetType("Test");
        MemberInfo[] members = test.GetMembers();
        foreach (var member in members)
        {
            Console.WriteLine($"member: {member}");
            Console.WriteLine($"member Name: {member.Name}");

        }

        Type dir = assembly.GetType("Dir");
        FieldInfo right = dir.GetField("Right");
        Console.WriteLine(right.GetValue(null));

        // 获取 Test 类型
        Type testType = assembly.GetType("Test");

        // 通过无参构造函数创建 Test 实例
        object testObject = Activator.CreateInstance(testType);

        // 访问 Test 对象的字段和属性
        FieldInfo nameField = testType.GetField("name");
        PropertyInfo ageProperty = testType.GetProperty("Age");

        // 设置字段和属性的值
        nameField.SetValue(testObject, "Alice");
        ageProperty.SetValue(testObject, 30);

        // 输出字段和属性的值
        Console.WriteLine($"Name: {nameField.GetValue(testObject)}");
        Console.WriteLine($"Age: {ageProperty.GetValue(testObject)}");

        // 也可以通过反射调用有参构造函数
        object testObjectWithParams = Activator.CreateInstance(testType, "Bob", 25);
    }
}

// F:\C#\projectTest\ConsoleAppTest\bin\Debug\net8.0\ConsoleAppTest
```

# 十、 特性
特性（Attributes）在 C# 中是一种强大的机制，允许开发者向程序的各种元素（如类、方法、属性、字段等）添加元数据。这些元数据可以在运行时或编译时被其他代码或框架读取和使用。特性通常用于配置、描述、验证或提供额外的信息。
- 特性得本质是类
- 可以利用特性类为原数据添加额外信息
- 比如一个类、成员变量、成员方法等等为他们添加更多得额外信息
- 后续通过反射获取这些额外信息
### 特性的基本概念
- **定义**：特性是从 `System.Attribute` 类派生的类，通常通过特性构造函数传递参数。
- **使用**：特性通过方括号 `[]` 应用在类、方法、属性等上。
### 创建自定义特性
要创建一个特性，需要定义一个类并继承自 `System.Attribute`。以下是一个示例：
```csharp
using System;

// 定义一个自定义特性，限制自定义特性得使用范围
// 参数一：AttrivuteTargets 特性能够用在哪些地方
// 参数二：AllowMultiple 是否允许多个特性用在同一个目标上
// 参数三：Inherited 特性是否能够被派生类和重写成员继承
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true, Inherited = true)]
public class DeveloperAttribute : Attribute
{
    public string Name { get; }
    public string Date { get; }

    public DeveloperAttribute(string name, string date)
    {
        Name = name;
        Date = date;
    }
}

// 使用自定义特性
// 在 C# 中，特性类的名称可以省略后缀 "Attribute"，因为编译器会自动为你加上这个后缀。
// 实际上，你可以将 Developer 类的名称改为 DeveloperAttribute，这两者在使用时是等效的。
[Developer("Alice", "2024-10-28")]
public class SampleClass
{
    [Developer("Bob", "2024-10-29")]
    public void SampleMethod()
    {
        Console.WriteLine("Sample Method Executed");
    }
}

class Program
{
    static void Main()
    {
        // 获取类的特性
        // false：仅获取当前类型（SampleClass）的特性，不包括基类的特性。
        // true：不仅获取当前类型的特性，还包括所有基类的特性。
        var classAttributes = typeof(SampleClass).GetCustomAttributes(false);
        foreach (var attr in classAttributes)
        {
            if (attr is DeveloperAttribute developerAttribute)
            {
                Console.WriteLine($"Class Developer: {developerAttribute.Name}, Date: {developerAttribute.Date}");
            }
        }

        // 获取方法
        var method = typeof(SampleClass).GetMethod("SampleMethod");
        // 获取方法的特性
        var methodAttributes = method.GetCustomAttributes(false);
        foreach (var attr in methodAttributes)
        {
            if (attr is DeveloperAttribute developerAttribute)
            {
                Console.WriteLine($"Method Developer: {developerAttribute.Name}, Date: {developerAttribute.Date}");
            }
        }

        // 判断是否使用了某个特性
        // 参数一：特性得类型
        // 参数二：代表是否包含基类得特性，属性和事件忽略此参数
        if (typeof(SampleClass).IsDefined(typeof(DeveloperAttribute), false))
        {
            Console.WriteLine("SampleClass应用了Developer特性");
        }

    }
}
```
### 代码解释
1. **定义特性**：
    - `DeveloperAttribute` 类是一个自定义特性，使用 `[AttributeUsage]` 特性指定它可以应用于类和方法。
2. **使用特性**：
    - 在 `SampleClass` 和 `SampleMethod` 上使用 `DeveloperAttribute`，传递开发者的名字和日期。
3. **读取特性**：
    - 使用 `GetCustomAttributes` 方法获取应用于类和方法的特性。
    - 检查特性的类型并输出相关信息。
### 特性的使用场景
- **数据验证**：在数据模型中使用特性进行输入验证（如 `Required`、`StringLength` 等）。
- **序列化**：指定序列化行为，例如在 JSON、XML 序列化中使用。
- **框架配置**：为 ASP.NET 等框架提供配置。
- **元数据**：提供有关程序元素的额外信息，以便在运行时读取和处理。
### 访问特性
特性可以通过反射访问。使用 `Type.GetCustomAttributes` 和 `MemberInfo.GetCustomAttributes` 方法获取特性。
### 系统自带得特性
在 .NET 中，有许多系统自带的特性（Attributes），这些特性提供了元数据，用于控制行为、描述特性或提供其他信息。以下是一些常用的系统自带特性及其用途：
#### 1. `ObsoleteAttribute`
表示某个类、方法、属性或字段不再推荐使用。
```csharp
[Obsolete("Use NewMethod instead.")]
public void OldMethod()
{
    // ...
}
```
#### 2. `SerializableAttribute`
指示一个类可以被序列化。
```csharp
[Serializable]
public class MyClass
{
    public int Value;
}
```
#### 3. `NonSerializedAttribute`
指示某个字段在序列化时不应被序列化。
```csharp
[Serializable]
public class MyClass
{
    public int Value;

    [NonSerialized]
    public int TemporaryValue;
}
```
#### 4. `DllImportAttribute`
用于调用非托管代码中的方法，通常用于 P/Invoke。
- 这里DllImport传入的参数是当前的C或C++文件产生的dll文件的路径，这里表示是同一个dll文件下
- 后面定义的MessageBox()就是被这个特性修饰，表示的就是C或C++里面的的那个函数
```csharp
[DllImport("user32.dll")]
public static extern int MessageBox(int hWnd, string text, string caption, uint type);
```
#### 5. `ThreadStaticAttribute`
指示某个字段是线程静态的，每个线程都有自己的独立字段副本。
```csharp
[ThreadStatic]
public static int ThreadLocalValue;
```
#### 6. `AttributeUsageAttribute`
指定自定义特性的使用规则。
```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class MyCustomAttribute : Attribute
{
}
```
#### 7. `DebuggerDisplayAttribute`
用于控制在调试器中显示的类或结构信息。
```csharp
[DebuggerDisplay("Value = {Value}")]
public class MyClass
{
    public int Value { get; set; }
}
```
#### 8. `GuidAttribute`
用于指定接口的全局唯一标识符 (GUID)。
```csharp
[Guid("12345678-1234-1234-1234-1234567890AB")]
public interface IMyInterface
{
}
```
#### 9. `FlagsAttribute`
用于枚举，指示该枚举可以作为位标志进行组合。
```csharp
[Flags]
public enum MyFlags
{
    None = 0,
    Option1 = 1,
    Option2 = 2,
    Option3 = 4
}
```
#### 10. `AssemblyVersionAttribute`
指定程序集的版本。
```csharp
[assembly: AssemblyVersion("1.0.0.0")]
```
在 C# 中，`CallerFilePath`、`CallerLineNumber` 和 `CallerMemberName` 是三个特殊的编译器服务特性（attributes），它们可以用于方法参数，以便在运行时获取调用该方法的文件名、行号和方法名称。这些特性通常用于日志记录、调试或任何需要追踪调用上下文的场景。
#### 11. CallerFilePath
`CallerFilePath` 特性提供了调用方法的源文件的完整路径。
**示例**：
```csharp
using System;
using System.Runtime.CompilerServices;

public class Logger
{
    public void Log(string message, 
                    [CallerFilePath] string filePath = "",
                    [CallerLineNumber] int lineNumber = 0,
                    [CallerMemberName] string memberName = "")
    {
        Console.WriteLine($"[{DateTime.Now}] {message} (File: {filePath}, Line: {lineNumber}, Member: {memberName})");
    }
}

class Program
{
    static void Main()
    {
        Logger logger = new Logger();
        logger.Log("This is a log message.");
    }
}
```
**输出**：
```less
[2023-10-28 12:00:00] This is a log message. (File: C:\path\to\your\file.cs, Line: 14, Member: Main)
```
#### 12. CallerLineNumber
`CallerLineNumber` 特性提供了调用方法的源代码行号。
**示例**：同上面的 `Logger` 类，行号会自动填充。
#### 13. CallerMemberName
`CallerMemberName` 特性提供了调用该方法的成员名称（例如，方法或属性名）。
**示例**：同上面的 `Logger` 类，成员名称会自动填充。
##### 使用场景
这些特性常用于：
- **日志记录**：可以记录出错信息及其来源，以便后续的调试。
- **调试信息**：在调试过程中，可以追踪函数的调用位置。
- **错误处理**：在处理异常时，可以提供更多上下文信息。
#### 14. Conditional
`Conditional` 特性是 C# 中一个非常有用的特性，主要用于控制某个方法是否被编译器调用。通过将 `Conditional` 特性应用于方法，可以根据特定的编译符号（如条件编译符号）决定是否在编译期间包括该方法的调用。这对于调试和日志记录等场景非常有用。
##### 用法
- **特性定义**：`Conditional` 特性位于 `System.Diagnostics` 命名空间中。
- **方法参数**：特性接受一个字符串参数，该参数表示一个编译符号。当定义的符号存在时，标记为 `Conditional` 的方法会被调用；如果该符号不存在，则方法调用会被忽略。和#define配合使用
##### 示例
```csharp
#define Fun
using System;
using System.Diagnostics;

public class Logger
{
    [Conditional("DEBUG")]
    public void Log(string message)
    {
        Console.WriteLine($"Log: {message}");
    }

    // 只有#define Fun采用调用下面得函数
    [Conditional("Fun")]
    public void Log(string message, int i)
    {
        Console.WriteLine($"Log: {message}1");
    }
}

class Program
{
    static void Main()
    {
        Logger logger = new Logger();
        logger.Log("This is a debug message."); // 在DEBUG模式下会输出
        logger.Log("This is a debug message.", 1); // 在#define Fun下会输出
    }
}
```
##### 编译符号
- 在上面的示例中，`Log` 方法使用了 `Conditional("DEBUG")` 特性。当项目在 `DEBUG` 模式下编译时，`Log` 方法的调用会被保留；而在 `RELEASE` 模式下，调用将被忽略，`Log` 方法不会执行。
##### 注意事项
1. **不支持返回值**：被标记为 `Conditional` 的方法必须是 `void` 返回类型，不能返回其他类型。
2. **仅适用于方法**：此特性不能用于属性、字段或其他成员，只能应用于方法。
3. **在调用方法时**：即使在 `RELEASE` 模式下，调用仍会在代码中保留，但不会在运行时执行。
### 注意事项
- 特性是类的实例，它们在运行时被加载，因此会消耗一定的资源。
- 使用特性时要考虑性能，避免在性能敏感的代码中频繁访问特性。
特性为 C# 提供了灵活性和可扩展性，使得代码更具可读性和可维护性。

# 十一、 迭代器
在 C# 中，可以通过实现 `IEnumerable<T>` 和 `IEnumerator<T>` 接口来自定义迭代器。下面是一个简单的示例，展示如何创建一个自定义的迭代器来遍历一个数列。
- using System.Collections;
- 关键接口：IEnumerator，IEnumerable
- 可以通过继承IEnumerator，IEnumerable接口实现其中得方法
### 示例：自定义迭代器
假设我们想要创建一个表示斐波那契数列的类，并提供一个迭代器来遍历该数列。
```csharp
using System;
using System.Collections;
using System.Collections.Generic;

public class FibonacciSequence : IEnumerable<int>
{
    private int _count;

    public FibonacciSequence(int count)
    {
        _count = count;
    }

    // 实现 IEnumerable<int> 接口
    public IEnumerator<int> GetEnumerator()
    {
        return new FibonacciEnumerator(_count);
    }

    // 实现 IEnumerable 接口
    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    private class FibonacciEnumerator : IEnumerator<int>
    {
        private int _count;
        private int _currentIndex = -1;
        private int _previous = 0;
        private int _current = 1;

        public FibonacciEnumerator(int count)
        {
            _count = count;
        }


        // 就是用来返回当前的值的，也就是迭代器的当前值
        // 是一个属性
        public int Current => _current;
        //public int Current
        //{
        //    get
        //    {
        //        Console.WriteLine("Current被调用");
        //        return _current;
        //    }
        //}

        object IEnumerator.Current => Current;
        // 等价于下main得代码，是属性
        //object IEnumerator.Current
        //{
        //    get { return Current; }
        //}

        public bool MoveNext()
        {
            if (_currentIndex >= _count - 1)
                return false;

            if (_currentIndex == -1)
            {
                _currentIndex++;
                return true;
            }

            // 这里实现了斐波拉且数列的相加
            int next = _previous + _current;
            _previous = _current;
            _current = next;
            _currentIndex++;
            return true;
        }

        public void Reset()
        {
            _currentIndex = -1;
            _previous = 0;
            _current = 1;
        }

        public void Dispose() { }
    }
}

class Program
{
    static void Main()
    {
        var fibonacci = new FibonacciSequence(10);

        foreach (var number in fibonacci)
        {
            Console.WriteLine(number);
        }
    }
}
```
#### 代码说明
1. **FibonacciSequence 类**：
    - 实现了 `IEnumerable<int>` 接口，表示可以迭代的集合。
    - `GetEnumerator` 方法返回一个 `IEnumerator<int>` 类型的实例，这里返回的是 `FibonacciEnumerator`。
2. **FibonacciEnumerator 类**：
    - 实现了 `IEnumerator<int>` 接口，用于提供迭代功能。
    - `_currentIndex` 记录当前索引，`_previous` 和 `_current` 分别存储前一个和当前的 Fibonacci 数。
3. **MoveNext 方法**：
    - 计算下一个 Fibonacci 数并更新状态。
    - 如果索引超过范围，返回 `false`。
4. **Reset 方法**：
    - 将迭代器状态重置为初始状态。
5. **Dispose 方法**：
    - 实现 `IDisposable` 接口，虽然在这个例子中没有资源需要释放，但为了完整性仍然实现了这个方法。
6. **Program 类**：
    - 创建一个 `FibonacciSequence` 的实例并使用 `foreach` 循环遍历 Fibonacci 数列。

>这段代码定义了一个自定义的 `IEnumerable` 实现，用于生成指定数量的斐波那契数列。代码通过 `FibonacciSequence` 类生成数列，并通过嵌套的 `FibonacciEnumerator` 类逐步计算和返回斐波那契数。

按调用流程逐步解析这段代码：
1. **创建 `FibonacciSequence` 实例**：
```csharp
var fibonacci = new FibonacciSequence(10);
```
这行代码创建一个 `FibonacciSequence` 实例，用于生成前 10 个斐波那契数。
2. **进入 `foreach` 循环**：
```csharp
foreach (var number in fibonacci)
{
    Console.WriteLine(number);
}
```
这个 `foreach` 循环会隐式调用 `fibonacci.GetEnumerator()` 方法，返回一个 `FibonacciEnumerator` 对象。`foreach` 会在每次迭代时调用 `MoveNext()` 和 `Current`，直到 `MoveNext()` 返回 `false`。
#### 迭代器具体调用流程
以下是每个方法的作用及调用顺序：
1. **调用 `GetEnumerator`**：
    - `fibonacci.GetEnumerator()` 返回一个 `FibonacciEnumerator` 实例，它是实际负责生成斐波那契数的迭代器。
2. **进入迭代 (`MoveNext` and `Current`)**：
    - **首次调用 `MoveNext()`**：
        - `_currentIndex` 初始值为 `-1`，此时更新为 `0`，然后返回 `true`。
        - 此时 `Current` 的值为 `1`，它代表斐波那契数列的第一个值。
    - **获取 `Current` 的值**：
        - `foreach` 调用 `IEnumerator.Current`，返回当前的 `_current` 值（`1`），将其打印。
3. **后续迭代**：
    - 每次调用 `MoveNext()` 依次计算下一个斐波那契数：
        - `next = _previous + _current`：计算下一个斐波那契值。
        - 更新 `_previous` 和 `_current`，并递增 `_currentIndex`。
    - 调用 `Current` 时返回最新的 `_current` 值，逐个输出到控制台。
4. **结束迭代**：
    
    - 当 `_currentIndex >= _count - 1` 时，`MoveNext()` 返回 `false`，`foreach` 循环结束。

#### 流程图示例
以下为每次调用的关键变量变化：

| Step         | `_currentIndex` | `_previous` | `_current` | Output |
| ------------ | --------------- | ----------- | ---------- | ------ |
| Initial      | -1              | 0           | 1          |        |
| 1st MoveNext | 0               | 0           | 1          | 1      |
| 2nd MoveNext | 1               | 1           | 1          | 1      |
| 3rd MoveNext | 2               | 1           | 2          | 2      |
| 4th MoveNext | 3               | 2           | 3          | 3      |
| ...          | ...             | ...         | ...        | ...    |
| Final        | 9               | ...         | ...        | 34     |

当 `MoveNext()` 返回 `false`，表示序列结束。

### `IEnumerator` 接口的实现细节

- **`Current`**：始终返回 `_current` 值，表示当前斐波那契数。
- **`MoveNext`**：更新斐波那契值并判断是否继续。
- **`Reset`**：重置所有值，使迭代器可以重新开始（仅在手动调用时生效）。

通过这个实现，每次调用 `MoveNext()` 都会生成新的斐波那契数，直到达到指定的数量。

### yield return实现迭代器
使用 `yield return` 可以轻松地实现迭代器，提供一种简洁的方式来返回序列中的每个元素。下面是一个示例，演示如何使用 `yield return` 创建一个生成 Fibonacci 数列的迭代器。
- 只需要继承IEnumerable接口
#### 示例代码：
```csharp
using System;
using System.Collections;
using System.Collections.Generic;

public class FibonacciGenerator : IEnumerable<int>
{

    private int _count;

    public FibonacciGenerator(int count)
    {
        _count = count;
    }
    // 使用迭代器生成 Fibonacci 数列
    public IEnumerable<int> Generate(int count)
    {
        int a = 0, b = 1;
        for (int i = 0; i < count; i++)
        {
            yield return a; // 返回当前的 Fibonacci 数
            int temp = a;    // 存储当前值
            a = b;           // 更新 a 为下一个 Fibonacci 数
            b = temp + b;    // 更新 b 为新的 Fibonacci 数
        }
    }

    public IEnumerator<int> GetEnumerator()
    {
        int a = 0, b = 1;
        for (int i = 0; i < _count; i++)
        {
            yield return a; // 返回当前的 Fibonacci 数
            int temp = a;    // 存储当前值
            a = b;           // 更新 a 为下一个 Fibonacci 数
            b = temp + b;    // 更新 b 为新的 Fibonacci 数
        }
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

class Program
{
    static void Main()
    {
        FibonacciGenerator generator = new FibonacciGenerator(10);

        // 生成前 10 个 Fibonacci 数
        foreach (var number in generator.Generate(10))
        {
            Console.WriteLine(number); // 输出 Fibonacci 数
        }

        // 生成前 10 个 Fibonacci 数
        foreach (var number in generator)
        {
            Console.WriteLine(number); // 输出 Fibonacci 数
        }
    }
}
```
#### 解释：
1. **`IEnumerable<int> Generate(int count)`**:
    - 这是一个返回 `IEnumerable<int>` 的方法，意味着它将返回一个可枚举的整数序列。
    - `count` 参数指定要生成的 Fibonacci 数的个数。
2. **`yield return a;`**:
    - 使用 `yield return` 语句来返回当前的 Fibonacci 数。
    - 每次调用 `Generate` 方法时，它都会返回下一个值，而不需要显式维护一个状态。
3. **`for` 循环**:
    - 通过循环生成 Fibonacci 数，更新变量 `a` 和 `b` 来计算下一个 Fibonacci 数。
4. **`Main` 方法**:
    - 创建 `FibonacciGenerator` 实例，并使用 `foreach` 循环遍历生成的 Fibonacci 数列，输出每个值。
#### 使用 `yield return` 的好处：
- **简洁性**: 代码更加简洁，易于理解。
- **状态管理**: 不需要手动管理迭代状态，`yield` 关键字自动处理了。
- **惰性求值**: 值在实际需要时才会被计算和返回，有助于节省内存和计算资源。

# 十二、 空合并操作符
空合并操作符（Null Coalescing Operator）在 C# 中用来处理可能为 `null` 的情况，提供一个简洁的语法来返回第一个非 `null` 的值。空合并操作符是 `??`，可以用于简化条件判断和默认值的赋值。
### 基本用法
语法如下：
```csharp
var result = value1 ?? value2;
```
这里，如果 `value1` 不为 `null`，`result` 将等于 `value1`；否则，`result` 将等于 `value2`。
### 示例代码
```csharp
using System;

class Program
{
    static void Main()
    {
        string input = null;

        // 使用空合并操作符
        string output = input ?? "默认值";

        Console.WriteLine(output); // 输出 "默认值"
    }
}
```
### 使用场景
1. **设置默认值**: 当你想要为可能为 `null` 的变量设置默认值时，使用空合并操作符是非常方便的。
```csharp
string userInput = GetUserInput(); // 可能返回 null
string displayInput = userInput ?? "无输入";
```
2. **简化条件判断**: 可以减少需要写的条件语句数量，使代码更简洁。
```csharp
string firstName = GetFirstName() ?? "John";
```
### 结合其他操作符
- **空合并赋值操作符 (`??=`)**: 从 C# 8.0 开始，你还可以使用空合并赋值操作符来为 `null` 的变量赋值。
```csharp
string username = null;
username ??= "Guest"; // 如果 username 为 null，则赋值为 "Guest"
```
在这个例子中，`username` 仅在其为 `null` 时被赋值。
### 其他示例
```csharp
class Program
{
    static void Main()
    {
        int? intV = 1;
        // 这里是因为intV是int?类型，不能赋值给int，所以需要用intV.Value进行转换
        // int intI = intV == null ? 100 : intV.Value;
        // 这里的int intI = intV ?? 100;的intV不需要.Value，是因为？？进行了处理
        int intI = intV ?? 100;
        Console.WriteLine(intI);
        string str = null;
        str = str ?? "hahah";
        Console.WriteLine(str);
    }
}
```

# 十三、 结构体和类中的值和引用
### 结构体中的值和引用
- 结构体本身是值类型
- 前提：该结构体没有作为其他类的成员
- 在结构体中的值，占用存储值具体得内容
- 在结构体中的引用，堆中存储引用具体的内容

### 类中的值和引用
- 类本身是引用类型
- 在类中的值，堆中存储具体的值
- 在类中的引用，堆中存储具体的值

# 十四、 this
在 C# 中，`this` 是一个特殊关键字，用于引用当前对象的实例。以下是 `this` 的具体用法和规则。

---
### **主要用途**
#### 1. **引用当前实例对象：**
    - `this` 通常用于区分实例字段、方法或属性与局部变量或参数名称冲突的情况。
```csharp
public class Example
{
    private int value;

    public void SetValue(int value)
    {
        this.value = value; // 使用 this 区分实例字段和方法参数
    }
}
```
#### 2. **调用类中的其他构造函数：**
    - 使用 `this` 可以在一个构造函数中调用另一个构造函数。
```csharp
public class Example
{
    private int value;

    public Example() : this(0) // 调用另一个构造函数
    {
    }

    public Example(int value)
    {
        this.value = value;
    }
}
```
#### 3. **作为方法或属性的返回值：**
    - `this` 可以用作返回值，通常在链式调用中使用。
```csharp
public class Example
{
    private int value;

    public Example SetValue(int value)
    {
        this.value = value;
        return this; // 返回当前实例
    }
}
```
#### 4. **在扩展方法中显式引用调用者对象：**
    - 扩展方法的第一个参数通常用 `this` 表示扩展的类型。
```csharp
public static class Extensions
{
    public static void Print(this string str)
    {
        Console.WriteLine(str); // str 是调用该方法的实例
    }
}
```
#### 5. **在事件或委托中作为参数传递：**
    - 使用 `this` 表示当前对象。
```csharp
public class Example
{
    public event EventHandler OnClick;

    public void Click()
    {
        OnClick?.Invoke(this, EventArgs.Empty); // 传递当前对象作为 sender
    }
}
```
---

### **使用限制**
#### 1. **静态成员中不能使用 `this`：**
    - 静态成员（字段、方法、属性）属于类本身，而不是实例。
```csharp
public class Example
{
    private static int value;

    public static void StaticMethod()
    {
        // this.value = 10; // 错误：静态成员不能访问实例成员
    }
}
```
#### 2. **字段或属性的初始化中不能使用 `this`：**
    - 在字段或属性的初始化阶段，实例尚未完全构造，因此 `this` 不可用。
```csharp
public class Example
{
    private int value = this.SomeMethod(); // 错误：实例成员尚未初始化

    private int SomeMethod()
    {
        return 42;
    }
}
```
#### 3. **匿名方法或 lambda 表达式中的捕获：**
- 如果匿名方法或 lambda 表达式被封装到静态上下文中，则不能使用 `this`。
```csharp
public class Example
{
    public void Method()
    {
        Action action = () =>
        {
            // this.value = 10; // 错误：如果 lambda 表达式捕获的上下文是静态的
        };
    }
}
```

>**lambda 表达式或匿名方法是否能使用 `this`，取决于它的上下文是否是实例上下文**。下面分几种情况来详细说明：

##### **1. 实例上下文中可以使用 `this`**
如果 lambda 表达式定义在实例方法中，它会捕获周围实例的上下文，因此可以使用 `this`。
```csharp
public class Example
{
    private int value;

    public void Method()
    {
        Action action = () =>
        {
            this.value = 10; // 合法，this 是当前实例
        };

        action.Invoke(); // 执行 lambda 表达式
    }
}
```
在这个例子中，`this` 是指当前 `Example` 类的实例。因为 `Method` 是一个实例方法，lambda 表达式也在实例上下文中运行。

---
##### **2. 静态上下文中不能使用 `this`**
如果 lambda 表达式定义在静态方法中，`this` 是无效的，因为静态方法不属于类的实例，而是直接属于类本身。
```csharp
public class Example
{
    private static int value;

    public static void Method()
    {
        Action action = () =>
        {
            // this.value = 10; // 错误：静态方法中没有 this
        };

        action.Invoke();
    }
}
```
在这个例子中，`this` 是非法的，因为 `Method` 是静态方法，没有与任何类实例绑定。静态方法中只能访问静态成员，不能访问实例成员，也没有 `this`。

---

##### **3. 捕获上下文的行为**
当 lambda 表达式或匿名方法捕获上下文时，它会捕获当前作用域中的变量（包括实例变量和局部变量）
```csharp
public class Example
{
    public void Method()
    {
        int localVar = 5;

        Action action = () =>
        {
            Console.WriteLine(localVar); // 捕获局部变量
        };

        action.Invoke(); // 输出：5
    }
}
```
在这个例子中，`localVar` 是一个局部变量，lambda 表达式会捕获它，并在运行时使用。

---

##### **4. 静态上下文和捕获的结合**
如果 lambda 表达式在静态上下文中，不能捕获实例上下文（即不能使用 `this`），但仍然可以捕获静态成员或局部变量。
```csharp
public class Example
{
    private static int staticValue;

    public static void Method()
    {
        int localValue = 5;

        Action action = () =>
        {
            staticValue = 10; // 可以访问静态成员
            Console.WriteLine(localValue); // 捕获局部变量
        };

        action.Invoke(); // 输出：5
    }
}
```

这里的 lambda 表达式虽然不能访问 `this`，但可以访问静态成员 `staticValue` 和局部变量 `localValue`。

---

### **特殊情况**
1. **在 `struct` 中的 `this`：**
    - 在结构体（`struct`）中，`this` 是当前实例的副本，因此不能直接修改 `struct` 的成员。
    - 可以通过调用方法或使用 `ref` 修饰的结构体参数修改实例。
```csharp
public struct Example
{
    public int Value;

    public void ModifyValue()
    {
        this.Value = 10; // 合法，但修改的是副本
    }

    public void ModifyWithRef(ref Example instance)
    {
        instance.Value = 10; // 正确修改
    }
}
```
2. **在扩展方法中：**
    - 扩展方法中的 `this` 参数是调用者实例的引用，因此 `this` 可以用来操作调用者对象，但不能为其重新赋值。

---

### 注意
- **字段初始化顺序问题：** Unity 或 C# 中的类字段初始化发生在构造函数运行之前，在这个阶段 `this` 尚未完全初始化。
- `this` 只能在类的方法（包括构造函数）或属性的 getter/setter 中使用，而不能在字段的声明或初始化部分中使用。






# 参考

[MS-Learning](https://learn.microsoft.com/zh-cn/)
[C# - 协变、逆变 看完这篇就懂了](https://www.cnblogs.com/VVStudy/p/11404300.html "发布于 2019-08-28 12:04")

