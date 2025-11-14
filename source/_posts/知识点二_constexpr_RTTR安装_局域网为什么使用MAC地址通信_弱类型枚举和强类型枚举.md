---
title: 知识点二_constexpr_RTTR安装_局域网为什么使用MAC地址通信_弱类型枚举和强类型枚举
math: true
---
# constexpr
`constexpr` 是 C++11 引入的关键字，主要用于在编译时求值表达式。它允许定义常量表达式，以便在编译阶段进行优化，提高程序的运行效率。随着 C++14 和 C++17 的发展，`constexpr` 的功能进一步增强，使得它能够处理更复杂的编译时计算逻辑。
以下是 `constexpr` 的一些重要特性和应用场景：

### 1. 基本用法和作用
- **定义常量**：`constexpr` 可以定义在编译时确定的常量。例如：
```csharp
constexpr int max_size = 100; // 编译期确定的常量
```
- **用于函数**：`constexpr` 函数会在编译期进行求值，只要所有参数都是编译期常量。这样可以避免运行时计算，提高性能。例如：
```csharp
constexpr int square(int x) {
    return x * x;
}

int main() {
    constexpr int result = square(5); // 在编译期求值
}

```
如果在运行时调用 `constexpr` 函数（如传递非 `constexpr` 参数），它也能正常执行，只是求值会在运行时进行，而不是编译期。
    

### 2. `constexpr` 变量和 `const` 的区别
- **`const`**：用于定义只读的常量，不能在定义后更改，但并不保证值在编译时已知。例如，`const` 可以用于不可变的运行时常量。
```csharp
const int x = std::time(0); // 运行时初始化的常量
```
- **`constexpr`**：要求其初始化表达式在编译期可知。`constexpr` 变量必须在编译时初始化，否则会报错。例如：
```csharp
constexpr int y = 10; // 编译期初始化
```

### 3. `constexpr` 函数的规则
- C++11 要求 `constexpr` 函数必须是单一表达式的返回值函数，不支持循环和分支。
- C++14 允许 `constexpr` 函数包含更复杂的逻辑，例如分支（`if`）和循环（`for`、`while`），进一步增强了其功能。
- C++17 允许在 `constexpr` 函数中定义局部变量和使用 `constexpr` 复合数据结构。
```csharp
constexpr int factorial(int n) {
    int result = 1;
    for (int i = 1; i <= n; ++i) {
        result *= i;
    }
    return result;
}
```

### 4. 编译期计算的应用场景
- **元编程**：通过 `constexpr` 函数实现编译期计算，不需要在运行时执行。
- **数组大小**：编译期常量有助于定义数组大小等。
- **数学常量或计算**：如阶乘、斐波那契数列、平方根等不需要在运行时计算的值。
```csharp
constexpr int fib(int n) {
    return (n <= 1) ? n : fib(n - 1) + fib(n - 2);
}
```

### 5. `constexpr` 与 `consteval` 和 `constinit`
- **`consteval`**：C++20 引入，用于要求表达式必须在编译时求值。所有 `consteval` 函数的结果必须在编译时确定，否则编译报错。
```csharp
consteval int cube(int x) {
    return x * x * x;
}
```
- **`constinit`**：C++20 引入，用于强制静态变量的编译期初始化，但不要求其值是常量（可以在运行时更改）。适用于要求初始化，但不要求保持常量的变量。
### 6. 使用 `constexpr` 的注意事项
- 确保 `constexpr` 表达式在编译期可知。对于复杂表达式，可能需要逐步分解以满足编译期可评估的要求。
- 避免在性能敏感的代码中使用过多递归 `constexpr` 表达式，这可能导致编译时间延长。

# RTTR安装
[官方安装说明](https://www.rttr.org/doc/master/building_install_page.html)
> RTTR (Run-Time Type Reflection) 是一个 C++ 库，提供了运行时类型反射功能，允许在运行时查询和操作类型信息。这使得 C++ 程序可以在运行时更灵活地处理类型，例如进行序列化、反序列化、动态创建对象、调用方法等。

在 Windows 系统上安装 RTTR 库可以通过多种方式完成，包括手动编译、使用包管理器（如 `vcpkg`）或使用预编译的库文件。下面是详细步骤：
### 使用 vcpkg 安装 RTTR
`vcpkg` 是一个 C++ 包管理工具，可以轻松安装和管理库。使用 `vcpkg` 安装 RTTR 是最简单的方式。
1. **安装 vcpkg**（如果未安装）：
    - 打开终端（PowerShell 或命令提示符），克隆 `vcpkg` 仓库并编译：
```powershell
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
```
2. **安装 RTTR**：
    - 使用以下命令安装 RTTR：

```powershell
.\vcpkg install rttr
```

3. **集成到 Visual Studio**（可选）：
    - 若使用 Visual Studio，可以将 `vcpkg` 安装路径集成到 IDE 中，自动包含已安装库：

```powershell
.\vcpkg integrate install
```    
4. **配置项目**：
    - 在项目属性中，确保将 `vcpkg` 安装的 `include` 和 `lib` 路径添加到包含目录中。
	    - `C:\Users\13248\Downloads\vcpkg\installed\x64-windows\include`
	    - `C:\Users\13248\Downloads\vcpkg\installed\x64-windows\lib`
	    - `C:\Users\13248\Downloads\vcpkg\installed\x64-windows\bin\rttr_core.dll`放在目录的sin一起
    - 使用 `#include <rttr/registration>` 包含 RTTR 头文件。

### 配置测试代码（release才行，不然会断点调试）

编译成功后，可以测试 RTTR 是否安装正确。创建一个简单的 C++ 程序来检查 RTTR 功能：

```cpp
#include <rttr/registration>
#include <iostream>

using namespace rttr;

class TestClass {
public:
    TestClass() : value(0) {}
    void setValue(int v) { value = v; }
    int getValue() const { return value; }

private:
    int value;
};

RTTR_REGISTRATION
{
    registration::class_<TestClass>("TestClass")
        .constructor<>()
        .property("value", &TestClass::getValue, &TestClass::setValue);
}

int main() {
    type testType = type::get_by_name("TestClass");

    if (testType.is_valid()) {
        std::cout << "RTTR is working correctly!" << std::endl;
    } else {
        std::cout << "RTTR is not installed/configured correctly." << std::endl;
    }

    return 0;
}
```
编译并运行程序，如果输出 `"RTTR is working correctly!"`，则说明安装和配置成功。

### RTTR 的主要功能

1. **类型注册**：允许开发者注册类、枚举和属性等，以便在运行时进行反射。
2. **对象创建**：可以在运行时通过类型信息动态创建对象。
3. **方法调用**：可以动态调用类的方法，而无需在编译时了解方法的具体签名。
4. **属性访问**：可以动态读取和设置对象的属性。
5. **序列化**：支持将对象序列化为字符串或其他格式，方便存储或传输。

### 使用 RTTR 的基本步骤

1. **安装 RTTR**：可以通过 `vcpkg` 或手动下载源代码进行安装。
2. **包含头文件**：在代码中包含 RTTR 的头文件。
    
```cpp
#include <rttr/registration>
```
    
3. **注册类型**：使用 `RTTR_REGISTRATION` 宏注册类型。
    
```cpp
RTTR_REGISTRATION
{
    rttr::registration::class_<YourClass>("YourClass")
        .constructor<>()
        .property("propertyName", &YourClass::getProperty, &YourClass::setProperty);
}
```
    
4. **使用类型反射**：在程序中获取类型信息，创建对象或调用方法。
    
```cpp
rttr::type myType = rttr::type::get<YourClass>();
```
    

### 示例代码

以下是一个简单的 RTTR 使用示例，定义了一个类并注册它，以演示如何在运行时访问类型信息：

```cpp
#include <rttr/registration> // 包含 RTTR 的注册头文件
#include <iostream> // 包含输入输出流头文件

// 定义 TestClass 类
class TestClass {
public:
	// 默认构造函数，初始化值为 0
	TestClass() : value(0) {}

	// 设置值的方法
	void setValue(int v) { value = v; }

	// 获取值的方法
	int getValue() const { return value; }

	// 打印当前值的方法
	void printValue() const {
		std::cout << "TestClass Current Value: " << value << std::endl;
	}

private:
	int value; // 存储值的私有成员变量
};

// 定义另一个类 AnotherClass
class AnotherClass {
public:
	// 默认构造函数，初始化文本为 "Hello"
	AnotherClass() : text("Hello") {}

	// 设置文本的方法,const std::string&这个类型需要和下面的getText一样
	void setText(const std::string& t) { text = t; }

	// 获取文本的方法，返回值类型需要和const std::string&一样
	const std::string& getText() const { return text; }

	// 打印当前文本的方法
	void printText() const {
		std::cout << "AnotherClass Current Text: " << text << std::endl;
	}

private:
	std::string text; // 存储文本的私有成员变量
};

// 使用 RTTR 进行注册
RTTR_REGISTRATION
{
	// 注册 TestClass
	rttr::registration::class_<TestClass>("TestClass")
		.constructor<>() // 注册无参数构造函数
		.property("value", &TestClass::getValue, &TestClass::setValue) // 注册属性 "value"
		.method("printValue", &TestClass::printValue); // 注册方法 "printValue"

// 注册 AnotherClass
rttr::registration::class_<AnotherClass>("AnotherClass")
	.constructor<>() // 注册无参数构造函数
	.property("text", &AnotherClass::getText, &AnotherClass::setText) // 注册属性 "text"
	.method("printText", &AnotherClass::printText); // 注册方法 "printText"
}

// 主函数
int main() {
	// 创建 TestClass 的实例
	TestClass test;
	test.setValue(42); // 设置值为 42

	// 获取 TestClass 的类型信息
	rttr::type testType = rttr::type::get(test);

	// 获取并调用 printValue 方法
	auto testMethod = testType.get_method("printValue");
	if (testMethod.is_valid()) { // 检查方法是否有效
		testMethod.invoke(test); // 动态调用 printValue 方法
	}

	// 创建 AnotherClass 的实例
	AnotherClass another;
	another.setText("World"); // 设置文本为 "World"

	// 获取 AnotherClass 的类型信息
	rttr::type anotherType = rttr::type::get(another);

	// 获取并调用 printText 方法
	auto anotherMethod = anotherType.get_method("printText");
	if (anotherMethod.is_valid()) { // 检查方法是否有效
		anotherMethod.invoke(another); // 动态调用 printText 方法
	}

	return 0; // 返回 0，表示程序正常结束
}

```
### 配置与编译
确保在编译和链接时正确配置 RTTR，包括：
- 添加 RTTR 的包含目录到项目设置中。
- 添加 RTTR 的库文件到链接器输入中。

### 常见问题与解决方案

1. **找不到头文件**：确保正确设置了包含目录。
2. **链接错误**：确保链接了正确的库文件，例如 `rttr_core.lib`。
3. **运行时错误**：检查类型注册是否正确，确保使用的类型与注册的一致。

# 局域网为什么使用MAC地址通信
> 如果一个局域网内的主机A想要访问很远的一个局域网内的主机B。这两台主机都是接入了Internet互联网，中间会经过Ip地址和多次的MAC地址的转换，最终将主机A的消息发送到了主机B，Ip地址从始至终没有改变，但是MAC一直在变化。
> 而局域网中的通信只使用MAC地址就行，为什么不用Ip了呢？

### ARP协议
- ARP协议，工作在网络层的协议，但是可以产生在数据链路层（网络接口层）传递的帧
	- 作用：`将Ip地址转换为MAC地址`
	- 通过发送MAC地址的广播帧，不断广播，到达目的Ip地址的主机，回复响应的ARP帧，最后返回发送主机，获取到了目标Ip地址的主机的MAC帧。但是实际上并不是一次性获取到目标Ip地址的MAC地址，只是获取到了下一跳主机或者路由器的MAC地址

### 局域网之间用MAC地址通信，为什么不用Ip呢
- 我所理解的局域网，并不是按照地域，距离划分，而是只是工作在物理层，数据链路层的主机间的通信。在局域网中，只是有主机，集线器，二层交换机等只使用到前两层进行消息传递的网络中。但是实际上，有路由器，三层交换机都没事，只要不连接到互联网上，都算是局域网。比如多个局域网通过一个路由器连接等。
	- 因此在局域网中，主机连到交换机上，交换机有一个MAC地址表，能够记录对应的MAC地址在交换机的哪个端口上，如果不知道，则广播除了接收消息的端口的其他所有端口
	- 因此直接使用MAC地址进行通信就可以了
	- 不使用Ip地址，因为`交换机本身是工作在前两层`，Ip地址在网络层，他应该是无法解析对应的数据的，所以Ip地址也用不上
- MAC地址是设备出厂时就固定了的，`每台主机唯一标识了一个MAC地址`，两者一一对应。而局域网中，只是简单的局域网通信，DHCP服务器或者路由器分配Ip地址的话，就只能给局域网内的每个主机分配私有Ip地址了，而具体分配到哪个主机，还是需要MAC地址唯一确定。
	- 所以在一个独立的局域网中，如果你想要给另一台主机发消息，还是需要知道他的地址，然后会使用ARP协议，获取其MAC地址，通过MAC地址来找到对应的交换机的哪个端口，将消息发送出去
- 那为什么局域网内部还是要有Ip地址呢？
	- 我的理解是如果有多个局域网，通过一个或者多个路由器或者三层交换机相连。`不同局域网内的主机通信是需要的`
	- 举例就是有两家网吧，他们各自的网吧中的电脑都组成一个小的局域网。现在举办联谊，两个网吧的游戏爱好者进行比赛，那么消息通信就需要使用Ip地址了
	- 因为Ip地址能够划分不同的区域，假如192.168.1.x是网吧A，192.168.2.x是网吧B，这就涉及到了分割广播域，需要路由器来完成，具体我也不记得了，就不细说了。大概意思就是能够知道192.168.1.x如果发送信息到192.168.2.x，就只是固定的交换机的那几个端口，其他的端口也就不发送消息了，作用也是一定程度上明确了要发送的方向
- MAC地址本身是没有区域划分的，而Ip地址是有区域划分的。类似于分层结构，先Ip地址找到主机大致的是属于哪个局域网，局域网内部再直接使用唯一的MAC地址找到对应的主机
- `历史遗留问题`，刚开始使用MAC进行小范围的信息传递没问题，等接入的设备多了，范围广了，就不好快速准确发送消息了，就出来Ip地址大致确定属于哪个区域，再准确投递消息

# 弱类型枚举和强类型枚举
在 C++ 中，枚举类型通常有两种形式：强类型枚举（`enum class`）和弱类型枚举（传统的 `enum`）。

### 弱类型枚举（传统的 `enum`）

弱类型枚举允许 **隐式转换**，即可以将整数值直接赋给枚举类型，反之亦然。这是因为弱类型枚举实际上与整数类型具有兼容性，枚举的底层类型默认是 `int`，可以直接用整数来赋值。

```cpp
#include <iostream>
using namespace std;

enum Color {
    Red = 1,
    Green = 2,
    Blue = 3
};

int main() {
    Color color = 2;  // int 隐式转换为 Color
    cout << color << endl;  // 输出 2
    
    int x = Red;  // Color 隐式转换为 int
    cout << x << endl;  // 输出 1

    return 0;
}
```

#### 解释：

- 在上述代码中，`Color color = 2;` 允许将一个整数（`2`）赋值给枚举类型 `Color`。这就是因为传统的 `enum` 枚举类型是弱类型枚举，它允许枚举和 `int` 之间进行隐式转换。
- 同样，`int x = Red;` 也可以成功地将枚举值赋值给整数。

### 强类型枚举（`enum class`）

强类型枚举（`enum class`）则不同，它不允许与 `int` 之间进行隐式转换，必须显式进行类型转换。这是为了避免类型混淆和错误。

例如：

```cpp
#include <iostream>
using namespace std;

enum class Color {
    Red = 1,
    Green = 2,
    Blue = 3
};

int main() {
    // Color color = 2;  // 编译错误：不能将 int 隐式转换为 Color
    Color color = static_cast<Color>(2);  // 必须显式转换
    cout << static_cast<int>(color) << endl;  // 输出 2
    
    // int x = Color::Red;  // 编译错误：不能将 Color 转换为 int
    int x = static_cast<int>(Color::Red);  // 显式转换
    cout << x << endl;  // 输出 1

    return 0;
}
```

### 弱类型枚举可能带来的问题
在C++中，enum class 是一种改进的枚举类型，它相对于传统的 enum 具有一些额外的优点。这个语法是C++11引入的，目的是解决传统 enum 存在的一些问题。
**1.** 作用域内枚举（Scoped Enums）
- **解释**：传统的 enum 是**非作用域**的，意味着它的枚举成员（如 Up, Down）会泄露到外部的作用域中，可能导致命名冲突。而 enum class 是作用域内的，枚举成员只能通过枚举类型的名称访问。
- **示例**：
	```cpp
	Direction d = Direction::Up; // 使用了作用域限定符 Direction::
	```
这样，Up 不会与其他枚举中的值或全局变量冲突。

**2.** 强类型（Strongly Typed）
- **解释**：传统的 enum 是**弱类型**的，可以隐式地转换为整数，导致一些潜在的错误。enum class 是**强类型**的，枚举类型不能隐式转换为整数，必须进行显式转换。
- **示例**：
	```cpp
	Direction d = Direction::Up;
	
	int n = d; // 错误，不能隐式转换为int
	
	int n = static_cast<int>(d); // 正确，需要显式转换
	```

**3.** 底层类型指定
- **解释**：在 enum class 中，可以明确指定枚举的底层类型（如 int, unsigned int），默认是 int。这给了开发者更多的控制权，特别是在对内存占用或特定位宽有要求的情况下。
- **示例**：
	```cpp
	enum class Direction : unsigned int
	{
	    None = 0,
	    Up,
	    Down,
	    Left,
	    Right
	
	};
	```

#### 解释：
- 在强类型枚举中，`Color color = 2;` 是不允许的，因为强类型枚举不允许隐式地将 `int` 转换为枚举类型。
- 需要通过 `static_cast<Color>(2)` 显式地进行转换，才能将整数 `2` 转换为 `Color` 枚举类型。
- 同样，若要将枚举值转换为整数，需要使用 `static_cast<int>(color)`。

### 总结：
- **弱类型枚举（传统的 `enum`）**：允许隐式地将整数赋给枚举类型，反之亦然。
- **强类型枚举（`enum class`）**：不允许隐式转换，必须显式地进行类型转换。


# 参考
[RTTR实现C++反射（1）集成rttr库_可以参考doxygen的安装](https://blog.csdn.net/ruisun09/article/details/108535736)
[CMake下载地址](https://cmake.org/download/)
[Windows下Boost库的安装与使用](https://blog.csdn.net/nanke_yh/article/details/124346308)
[局域网内和局域网间的通信（交换机和路由器）](https://blog.csdn.net/ff900709/article/details/82225288)