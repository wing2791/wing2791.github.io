---
title: 知识点一_inline, const, static
math: true
---

# inline

C++将内联函数的代码组合到程序中，可以提高程序运行的速度。
语法：在函数声明和定义前加上关键字 inline。
`通常的做法是将函数声明和定义都写在头文件（当然都写在源文件也行）。目的是为了避免函数重定义的问题`

### 正确的 inline 例子

- 文件结构如图所示
  [![pAwMuMq.png](https://s21.ax1x.com/2024/10/24/pAwMuMq.png)](https://imgse.com/i/pAwMuMq)

- 代码示例
  my_inline.h

```cpp
#include <iostream>
using namespace std;

inline void sayHello() {
	std::cout << "Hello from version 1!" << std::endl;
}
```

main.cpp

```cpp
#include "my_inline.h"

int main() {
	sayHello();
	return 0;
}

```

my_inline.cpp

```cpp
// 空的
```

### 错误的 inline 例子

> 内联函数就是编译器将函数体（{...}之间的内容）在函数调用处展开，其实可以类比于#define 宏定义那种替换，来免去函数调用的开销。如果普通函数定义在头文件中，所有 include 该头文件的编译单元都可以正确找到函数定义。然而，如果内联函数 fun()定义在某个编译单元 A 中，那么其他编译单元中调用 fun()的地方将无法解析该符号，因为在编译单元 A 生成目标文件 A.obj 后，内联函数 fun()已经被替换掉，A.obj 中不再有 fun 这个符号，链接器自然无法解析。
> 上面的意思我的理解为是，下面的 sayHello 在 my_inline.cpp 中编译后，就不叫 sayHello()了，而 main 函数中仍然需要链接这个 sayHello()函数体，找不到，所以报错。具体不管，反正意思就是不能分开写

代码示例
my_inline.h

```cpp
#include <iostream>
using namespace std;

inline void sayHello();
```

main.cpp

```cpp
#include "my_inline.h"

int main() {
	sayHello();
	return 0;
}

```

my_inline.cpp

```cpp
#include "my_inline.h"

inline void sayHello() {
	std::cout << "Hello from version 1!" << std::endl;
}
```

编译期间报错
[![pAwMKs0.png](https://s21.ax1x.com/2024/10/24/pAwMKs0.png)](https://imgse.com/i/pAwMKs0)

### 正确的普通函数例子

- 代码示例
  my_inline.h

```cpp
#include <iostream>
using namespace std;

void sayHello();
```

main.cpp

```cpp
#include "my_inline.h"

int main() {
	sayHello();
	return 0;
}

```

my_inline.cpp

```cpp
#include "my_inline.h"

void sayHello() {
	std::cout << "Hello from version 1!" << std::endl;
}
```

注意：

- 内联函数节省时间，但消耗内存。
- 如果函数过大，编译器可能不将其作为内联函数。
- 内联函数不能递归。我试了可以写递归，估计被优化成普通函数了

# const

### 1. **修饰普通变量**

- 当`const`修饰一个变量时，该变量在声明时需要初始化，并且在之后的程序执行过程中，其值不能被改变。

```cpp
const int maxValue = 100;
// maxValue = 200; // 错误，无法修改const变量的值
```

- 使用`const`修饰变量可以增强代码的安全性，防止在无意中修改不应该更改的值。

### 2. **修饰指针**

`const`在指针声明中可以有多种组合方式，分别用于不同的用途：

- **指向常量的指针**（`const T*` 或 `T const*`）

```cpp
const int* ptr = &maxValue; // 或 int const* ptr = &maxValue;
// *ptr = 10; // 错误，无法通过ptr修改指向的值
ptr = &anotherValue; // 可以修改指针本身，指向不同地址
```

- **常量指针**（`T* const`）

```cpp
int value = 10;
int* const ptr = &value;
*ptr = 20; // 可以修改指向的值
// ptr = &anotherValue; // 错误，不能修改指针本身
```

- **指向常量的常量指针**（`const T* const` 或 `T const* const`）

```cpp
const int* const ptr = &maxValue;
// *ptr = 20; // 错误，无法修改指向的值
// ptr = &anotherValue; // 错误，无法修改指针本身
```

### 3. **修饰函数参数**

- 当函数参数是一个指针或引用时，`const`可以保证在函数内部不修改该参数的值。

```cpp
void display(const int& value) {
    // value = 10; // 错误，无法修改值
    std::cout << value << std::endl;
}
```

### 4. **修饰函数返回值**

```cpp
#include <iostream>
using namespace std;

class MyClass {
public:
	// 这个返回的是临时变量，函数调用完成后
	// tempValue会被释放，指向的内存行为不定，但是不好测试出来
	//int& getTempValue() {
	//	int tempValue = 1;
	//	return tempValue;
	//}

	// 这个不会报错，返回的是右值，和地址无关
	int getTempValue(int i) {
		int tempValue = 1;
		return tempValue;
	}

	int& getTempValue() {
		int tempValue = 1;
		return tempValue;
	}

	int& getValue() {
		return value; // 返回一个可修改的引用
	}
	const int& getConstValue() const {
		return value; // 返回一个不可修改的引用
	}
private:
	// 生命周期和实例化对象的生命周期一样，所以如果不想要外部修改value
	// 返回value的引用时，需要加const
	int value = 10;
};

int main() {
	MyClass obj;
	obj.getValue() = 20;       // 允许，修改了value
	cout << obj.getValue() << endl; //20
	//obj.getConstValue() = 30;  // 错误，返回的引用是const，无法修改
	// const int 和 int的值可以互相转换，所以返回值是否有const作为等号的右边不会影响等号左边的情况
	// 也就是返回值是const，赋给的变量不是const的，那就不是const的
	// 反之，如果返回值不是const的，赋给的变量是const，那就是const的


	return 0;
}
```

### 5. **修饰类的成员函数**

- 当`const`修饰成员函数时，表示该函数不会修改类的成员变量。

```cpp
#include <iostream>
using namespace std;

class MyClass {
public:
	int getValue() const {
		// value = 190; // 报错，不允许修改，const去掉可以修改
		return value;
	}
private:
	int value = 0;
};

int main() {
	MyClass obj;
	// obj.getValue() = 20;       // 错误，const去掉可以修改，同时需要将上面的int getValue()函数改为int& getValue()
	cout << obj.getValue() << endl;

	return 0;
}
```

### 6. 如果引用的数据对象类型不匹配，当引用为 const 时，C++将创建临时变量，让引用指向临时变量

- 先看最下面的例子
- `int&`定义一个变量 ra，赋值为 8。但是报错。这是因为引用(reference，表现就是&表示引用)必须绑定到一个有效的变量（左值），而不能绑定到右值上。
- 在 C++中，引用是一种**别名**，它需要指向一个**已存在的变量**（左值），而不是一个**临时的或字面值的常量**（右值）。像`8`这样的字面值属于右值，没有实际的内存地址，因此不能为其创建引用【引用本质还就是指针】
- 正确的做法
  引用应该绑定到一个变量，例如：
  ```cpp
  int x = 8;
  int& ra = x; // 正确，引用ra绑定到变量x
  ```
  这样，`ra`成为`x`的别名，通过`ra`可以读取或修改`x`的值。
- 特例：`const`引用
  如果需要引用一个字面值（右值），可以使用`const`引用，因为 C++允许`const`引用绑定到右值。这样做是安全的，因为`const`引用不能修改右值。
  ```cpp
  const int& ra = 8; // 正确，const引用可以绑定到字面值
  ```
  在这种情况下，编译器会创建一个临时对象来存储字面值`8`，然后`const`引用`ra`绑定到这个临时对象。

```cpp
#include <iostream>
using namespace std;

int main() {
	// int& ra = 8;// 报错
	const int& ra = 8; // 正常
	// 等价于下面两行
	int tmp = 8;
	const int& ra = tmp;

	return 0;
}
```

- 来，升级一下
- 原理和上面一样，不过变成了函数的参数

```cpp
#include <iostream>
using namespace std;

void func(const int& no) {
	cout << "no:" << no << endl;
}

void func2(int& no) {
	cout << "no:" << no << endl;
}

int main() {
	func(1);
	// func2(1); 报错

	return 0;
}
```

解释一下 word 中的三句话
什么时候将创建临时变量呢？

- 1. 引用是 const
     就是上面的例子，不再解释啦

- 2. 数据对象的类型是正确的，但不是左值。
     就是上面的例子，不再解释啦
     解释一下什么叫正确的

  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
  	// int& ra = 8;// 报错
  	// const int& ra = "123"; // const char* 类型不能够给const int&赋值
  	const int& ra = 1;// 可以

  	return 0;
  }

  ```

- 3. 数据对象的类型不正确，但可以转换为正确的类型

  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
  	const int& ra = 'X'; // char可以转换为int类型，char可以理解为无符号的1字节整数
  	cout << ra << endl;
  	// 等价于
  	int temp = 'X';
  	const int rs = temp;
  	cout << rs << endl;

  	return 0;
  }
  ```

# static

- 只解释下面两句话，其他的看 word 吧
  - 在静态成员函数中，只能访问静态成员，不能访问非静态成员
  - 静态成员函数中没有 this 指针

> 在 C++中，静态成员函数只能访问静态成员，不能直接访问非静态成员。这是因为静态成员函数属于类本身，而不是类的某个对象。静态成员函数没有 `this` 指针，因此无法访问特定对象的成员变量或调用非静态成员函数。

### 解释原因：

1. **静态成员函数与类关联，而非对象关联**：
   - 静态成员函数是属于类本身的，不需要创建类的对象就可以调用。因为它不属于任何对象实例，所以在静态成员函数中无法访问与对象实例相关的非静态成员。
2. **没有 `this` 指针**：
   - 非静态成员函数隐式地包含了一个指向调用对象的 `this` 指针，可以用来访问该对象的成员。而静态成员函数没有 `this` 指针，因为它与具体的对象实例无关，无法获取或操作非静态成员。

### 示例代码

```cpp
#include <iostream>

class MyClass {
public:
	int nonStaticMember; // 非静态成员变量
	static int staticMember; // 静态成员变量

	MyClass(int val) : nonStaticMember(val) {}

	static void staticFunction() {
		// std::cout << nonStaticMember; // 错误：无法访问非静态成员
		// this只能用于非静态成员函数内部
		std::cout << "Static member: " << staticMember << std::endl;
	}

	void nonStaticFunction() {
		// 可以访问静态和非静态成员
		std::cout << "Non-static member: " << this->nonStaticMember << std::endl;
		std::cout << "Static member: " << staticMember << std::endl;
	}
};

// 初始化静态成员变量
int MyClass::staticMember = 10;

int main() {
	MyClass obj(5);
	MyClass::staticFunction(); // 调用静态成员函数
	obj.nonStaticFunction();   // 调用非静态成员函数

	return 0;
}
```

### 输出结果

```sql
Static member: 10
Non-static member: 5
Static member: 10
```

### 说明

- 在 `staticFunction` 中，尝试访问 `nonStaticMember` 会导致编译错误，因为它是非静态成员。
- 在 `nonStaticFunction` 中，可以同时访问静态成员和非静态成员，因为此函数属于某个对象实例，有 `this` 指针。
  静态成员函数的主要用途是与类相关的操作，而非对象的具体状态

# 补充

### 1. 宏定义的位置

> 宏定义建议写在头文件，源文件也没事，和普通函数一样

### 2. 如何调用不同的重载，const 和非 const

有下面的类，如何调用不同的重载呢

```cpp
class Array {
public:
    int& operator[](size_t index) {
        return data[index]; // 返回可修改的引用
    }
    const int& operator[](size_t index) const {
        return data[index]; // 返回只读引用
    }
private:
    int data[10];
};
```

1. **调用第一个版本（返回可修改引用）**：

   - 当对象是**非`const`类型**时，将调用第一个`operator[]`，即返回一个可修改的引用。
   - 这种情况下，调用者可以通过返回的引用来修改数组中的元素。

```cpp
Array arr;
arr[0] = 10; // 非const对象，调用第一个operator[]，可以修改元素
```

2. **调用第二个版本（返回只读引用）**：

   - 当对象是**`const`类型**时，将调用第二个`operator[]`，即返回一个只读的引用。
   - 在这种情况下，调用者只能读取数组中的元素，而不能对其进行修改。

```cpp
const Array arr;
int value = arr[0]; // const对象，调用第二个operator[]，只能读取元素
arr[0] = 20;        // 错误，无法修改const对象的元素
```

#### 原理解释

- **编译器根据对象的类型（是否为`const`）自动选择合适的`operator[]`重载**：
  - 如果对象是`const`，则只能调用`const`成员函数，因此会选择`const int& operator[](size_t index) const`。
  - 如果对象是非`const`，则会优先选择`int& operator[](size_t index)`，因为它允许修改数据。

#### 例子

下面是完整的示例代码来展示这两种情况：

```cpp
#include <iostream>

class Array {
public:
    int& operator[](size_t index) {
        return data[index]; // 返回可修改的引用
    }

    const int& operator[](size_t index) const {
        return data[index]; // 返回只读引用
    }

private:
    int data[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
};

int main() {
    Array arr;
    arr[0] = 42; // 非const对象，调用第一个operator[]，可以修改元素
    std::cout << "arr[0] = " << arr[0] << std::endl; // 输出：arr[0] = 42

    const Array constArr;
    int value = constArr[0]; // const对象，调用第二个operator[]，只能读取元素
    std::cout << "constArr[0] = " << value << std::endl; // 输出：constArr[0] = 0

    // constArr[0] = 100; // 错误，无法修改const对象的元素

    return 0;
}
```

在这个例子中，`arr[0] = 42`修改了非`const`对象的元素，而`constArr[0]`只读取了`const`对象的元素。

### 3. 左值和右值

左值是可以被引用的数据对象，可以通过地址访问它们，例如：变量、数组元素、结构体成员、引用和解引用的指针。也就是可以出现在等于号左边，或者可以用&取地址

非左值(右值)包括字面常量（用双引号包含的字符串除外）和包含多项的表达式。也就是不可以出现在等于号左边，或者不可以用&取地址

`字符串常量`（例如 `"Hello, world!"`）在 C++中是一个**右值**。它是存储在只读内存中的字符数组的地址，表示一个不可修改的字符序列。

### 4. 为什么`字符串常量`是右值？

- **右值**通常是指无法在程序中获取其内存地址的值或者在求值过程中生成的临时值，而字符串常量就是这样。它是一个临时的、不可修改的对象，其类型是 `const char[N]`，其中 `N` 是字符串的长度加上终止符 `\0`。
- 字符串常量本质上存储在只读内存区域，因此不能被修改，这是右值的典型特征。

#### 示例

```cpp
const char* str = "Hello, world!";
```

这里，字符串常量 `"Hello, world!"` 是一个右值，它的地址被赋值给了指针 `str`。虽然可以通过 `str` 访问字符串数据，但字符串本身是不可修改的。

#### 特例

在某些情况下，字符串常量可以被视为**左值**，例如，当它被作为数组来使用时，字符串常量可以退化为指针（指向它的第一个字符），从而在一些表达式中可以像左值一样使用。但本质上，它仍然是只读的。

##### 举例子

字符串常量在某些表达式中可以表现得像左值，这是因为它会**退化为指针**，指向字符串的第一个字符。尽管如此，字符串常量本身仍然是不可修改的（即指针指向的内容是只读的）。下面是一些可以表现得像左值的例子：

###### 示例 1：作为数组的首地址使用

```cpp
#include <iostream>

int main() {
    const char* p = "Hello, world!"; // "Hello, world!" 退化为指针，指向字符串的首地址
    std::cout << p << std::endl;     // 输出整个字符串
    return 0;
}
```

在这个例子中，字符串常量 `"Hello, world!"` 退化为指针 `const char*`，其值是字符串的首地址，因此可以像左值那样使用 `p`，但字符串的内容是不可修改的。

###### 示例 2：作为数组元素的首地址

```cpp
#include <iostream>

int main() {
    const char* str = "Hello, world!";
    std::cout << str[0] << std::endl; // 输出 'H'
    return 0;
}
```

这里，`str[0]` 表达式访问的是字符串常量中第一个字符 `'H'`，字符串常量退化为指针后表现得像一个数组的首地址，可以使用数组下标访问字符。

###### 示例 3：传递给函数

```cpp
#include <iostream>

void printString(const char* s) {
    std::cout << s << std::endl;
}

int main() {
    printString("Hello, world!"); // 字符串常量退化为指针传递给函数
    return 0;
}
```

在这个例子中，字符串常量 `"Hello, world!"` 退化为指针并作为参数传递给函数 `printString`，在函数内表现得像一个指针的左值。

### 5. 利用 static 的特性，实现单例(多线程需要 C++11)

```cpp
#include <iostream>

class Singleton {
public:
    static Singleton& getInstance() {
    // 由于静态局部变量只会在第一次进入其所在的函数时被初始化一次，后续的函数调用会直接返回已经初始化的 `instance`，这就是为什么 `static Singleton instance` 只会执行一次。这样可以确保类始终只有一个实例，从而实现单例模式。也是延迟加载，因为只会在第一次进入其所在的函数时被初始化一次
        static Singleton instance; // 静态局部变量，只会初始化一次

        return instance;
    }

    void showMessage() {
        std::cout << "Singleton instance is working!" << std::endl;
    }

private:
    Singleton() {} // 构造函数私有化，防止外部创建实例
    Singleton(const Singleton&) = delete; // 删除拷贝构造函数
    Singleton& operator=(const Singleton&) = delete; // 删除赋值运算符
};

int main() {
    Singleton& s1 = Singleton::getInstance();
    Singleton& s2 = Singleton::getInstance();

    s1.showMessage();

    if (&s1 == &s2) {
        std::cout << "s1 and s2 are the same instance." << std::endl;
    } else {
        std::cout << "s1 and s2 are different instances." << std::endl;
    }

    return 0;
}
```

#### C++11 的改进：线程安全的静态局部变量

在 C++11 之前，静态局部变量的初始化在多线程环境下并不保证线程安全，这意味着在多个线程同时访问时，可能会导致静态变量被初始化多次。然而，C++11 对这一特性进行了改进，使得静态局部变量的初始化在多线程环境下变得线程安全，这样可以确保静态局部变量仅被初始化一次。
因此，在 C++11 及以上的版本中，`static`局部变量的单例模式实现无需额外的锁机制即可保证线程安全。

# 参考博客

[inline 函数必须在头文件中定义吗？](https://blog.csdn.net/tonywearme/article/details/7097910)
