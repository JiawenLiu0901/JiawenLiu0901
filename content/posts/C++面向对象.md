---
weight: 4
title: "C++面向对象"
date: 2023-03-20T21:57:40+08:00
lastmod: 2023-05-18T16:45:40+08:00
draft: false
author: "Jiawen Liu"
authorLink: "https://jiawenliu0901.github.io"
description: "C++面向对象知识点整理总结"
tags: ["C++"]
categories: ["C++八股"]
lightgallery: true
---
# C++面向对象

## 拷贝控制

### 什么是移动构造函数(move constructor)和移动赋值运算符(move assignment operator)？

移动构造函数（Move Constructor）和移动赋值运算符（Move Assignment Operator）是C++11引入的两个特殊的成员函数，它们的目的是实现移动语义（Move Semantics），以提高程序的性能。

1. 移动构造函数（Move Constructor）：

移动构造函数是一种特殊的构造函数，它接受一个右值引用作为参数，用于在对象之间转移资源的所有权，而不是拷贝资源。移动构造函数的语法如下：

```cpp
class_name(class_name&& other);
```

示例：
```cpp
class MyString {
public:
    // Move constructor
    MyString(MyString&& other) : data(other.data) {
        other.data = nullptr;
    }

private:
    char* data;
};
```

在这个例子中，移动构造函数接受一个右值引用参数，将`other.data`的所有权转移给新构造的对象，并将`other.data`设置为`nullptr`，避免了不必要的内存拷贝操作。

2. 移动赋值运算符（Move Assignment Operator）：

移动赋值运算符是一种特殊的赋值运算符，它接受一个右值引用作为参数，用于在对象之间转移资源的所有权，而不是拷贝资源。移动赋值运算符的语法如下：

```cpp
class_name& operator=(class_name&& other);
```

示例：
```cpp
class MyString {
public:
    // Move assignment operator
    MyString& operator=(MyString&& other) {
        if (this != &other) {
            delete[] data;
            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }

private:
    char* data;
};
```

在这个例子中，移动赋值运算符首先检查是否为自我赋值，然后释放当前对象的资源，将`other.data`的所有权转移给当前对象，并将`other.data`设置为`nullptr`。最后返回当前对象的引用。

移动构造函数和移动赋值运算符的存在，使得对象可以高效地在不同的上下文之间转移资源的所有权，避免了不必要的拷贝操作，提高了程序的性能。编译器会在适当的情况下自动调用移动构造函数和移动赋值运算符，例如在返回临时对象或将对象存储在容器中时。

需要注意的是，在实现移动构造函数和移动赋值运算符时，要确保转移后的源对象处于一个合法的状态，可以安全地销毁，避免出现悬空引用或资源泄漏等问题。

### 什么是拷贝构造函数？

拷贝构造函数（Copy Constructor）是一种特殊的构造函数，用于创建一个新对象，并将其初始化为另一个同类型对象的副本。当需要复制一个对象时，编译器会自动调用拷贝构造函数。

拷贝构造函数的语法如下：

```cpp
class_name(const class_name& other);
```

其中，`other`是要复制的源对象的常量引用。

示例：
```cpp
class MyClass {
public:
    MyClass(int value) : data(value) {}

    // Copy constructor
    MyClass(const MyClass& other) : data(other.data) {}

private:
    int data;
};
```

在这个例子中，拷贝构造函数接受一个`MyClass`对象的常量引用作为参数，并将`other.data`的值复制给新对象的`data`成员。

**拷贝构造函数的调用时机**：

1. 当一个对象以值传递的方式传递给函数时。
2. 当函数返回一个对象时。
3. 当一个对象被初始化为另一个同类型的对象时。

示例：
```cpp
MyClass obj1(42);
MyClass obj2(obj1);  // 调用拷贝构造函数
MyClass obj3 = obj1; // 调用拷贝构造函数

void func(MyClass obj) {} // 传递对象时调用拷贝构造函数

MyClass createObject() {
    MyClass obj(42);
    return obj; // 返回对象时调用拷贝构造函数
}
```

如果一个类没有显式定义拷贝构造函数，编译器会自动生成一个默认的拷贝构造函数，该函数会执行逐个成员的复制（浅拷贝）。但是，如果类中包含指针成员或动态分配的资源，那么默认的拷贝构造函数可能会导致问题，如多个对象共享相同的资源，导致内存泄漏或悬空指针等。在这种情况下，需要显式定义拷贝构造函数，以实现深拷贝，即复制指针指向的内容，而不仅仅是复制指针本身。

总之，拷贝构造函数是C++中实现对象复制的重要机制，了解其工作原理和正确使用可以帮助我们编写更加健壮和高效的代码。

### 什么是赋值运算符(assignment operator)？

赋值运算符（Assignment Operator）是一种特殊的运算符，用于将一个对象的值赋给另一个已存在的对象。在C++中，赋值运算符是一个成员函数，其语法如下：

```cpp
class_name& operator=(const class_name& other);
```

其中，`other`是要复制的源对象的常量引用，函数返回当前对象的引用，以支持连续赋值的操作。

示例：
```cpp
class MyClass {
public:
    MyClass(int value) : data(value) {}

    // Assignment operator
    MyClass& operator=(const MyClass& other) {
        if (this != &other) {
            data = other.data;
        }
        return *this;
    }

private:
    int data;
};
```

在这个例子中，赋值运算符首先检查是否为自我赋值（即源对象和目标对象是同一个对象），然后将`other.data`的值赋给当前对象的`data`成员，最后返回当前对象的引用。

在定义赋值运算符时，需要注意以下几点：

1. 检查自我赋值，避免不必要的操作和潜在的问题。
2. 在赋值之前，先释放当前对象拥有的资源，避免内存泄漏。
3. 正确处理异常，确保异常发生时对象状态的一致性。

总之，赋值运算符是C++中实现对象赋值的重要机制，了解其工作原理和正确使用可以帮助我们编写更加健壮和高效的代码。

### 什么是默认构造函数（default constructor)？

默认构造函数指的是一个没有任何参数的构造函数，如果没有一个这样的构造函数存在，或者是private的话，会造成编译错误。如果类里面没有提供任何的构造函数，编译器会提供一个implicit default constructor。如果类里面已经定义过了构造函数，我们就要手动定义默认构造函数，使用= default来要求编译器生成。


### 什么时候会调用赋值运算符？

赋值运算符通常在以下情况下被调用：

1. 显式赋值操作：当你使用赋值运算符`=`将一个对象赋值给另一个已存在的对象时，会调用赋值运算符。

```cpp
MyClass obj1(42);
MyClass obj2(0);
obj2 = obj1; // 调用赋值运算符
```

2. 函数参数传递：当一个对象作为函数参数按值传递时，虽然调用的是拷贝构造函数，但如果在函数内部对该对象进行了赋值操作，就会调用赋值运算符。

```cpp
void func(MyClass obj) {
    MyClass obj2(0);
    obj2 = obj; // 调用赋值运算符
}

MyClass obj1(42);
func(obj1);
```

3. 函数返回值：当函数返回一个对象，且该对象是通过赋值操作创建的，会调用赋值运算符。

```cpp
MyClass createObject() {
    MyClass obj(42);
    return obj; // 先调用拷贝构造函数，再调用赋值运算符
}

MyClass obj = createObject();
```

4. 容器元素赋值：当你将一个对象赋值给容器（如`std::vector`、`std::map`等）中的元素时，会调用赋值运算符。

```cpp
std::vector<MyClass> vec(10);
MyClass obj(42);
vec[0] = obj; // 调用赋值运算符
```

5. 其他隐式赋值操作：在某些特定情况下，编译器可能会隐式调用赋值运算符，例如在使用算法库（如`std::fill`、`std::copy`等）时。

需要注意的是，如果你没有显式定义赋值运算符，编译器会自动生成一个默认的赋值运算符，该运算符会执行逐个成员的赋值（浅拷贝）。但是，如果类中包含指针成员或动态分配的资源，那么默认的赋值运算符可能会导致问题，如多个对象共享相同的资源，导致内存泄漏或悬空指针等。因此，在这种情况下，你需要自己定义赋值运算符，以确保正确的行为和资源管理。

### 如何实现深拷贝和浅拷贝？

深拷贝：深拷贝是指在拷贝对象时，会同时复制对象所引用的动态分配的内存。这样，两个对象拥有独立的内存空间，互不影响。实现深拷贝通常需要手动编写拷贝构造函数和/或赋值运算符，确保在拷贝对象时分配新的内存并复制数据。

浅拷贝：浅拷贝是指仅复制对象本身，而不复制对象所引用的动态分配的内存。两个对象将共享相同的内存区域，因此修改其中一个对象的数据会影响到另一个对象。

默认情况下，C++提供的拷贝构造函数和赋值运算符执行的是浅拷贝。

// 构造函数
ShallowCopy(const char *str) {
	data = new char[strlen(str) + 1];
	strcpy(data, str);
}

// 拷贝构造函数（默认是浅拷贝）
ShallowCopy(const ShallowCopy &other) {
	data = other.data;
}

### 什么是 RAII（Resource Acquisition Is Initialization）技术，如何使用它来管理资源？

RAII（Resource Acquisition Is Initialization）是一种管理资源的编程技术，主要用于 C++ 中。该技术的核心思想是：**资源的获取与对象的初始化绑定，资源的释放与对象的销毁绑定**。这样，当对象的生命周期结束时，资源会被自动释放，从而有效地避免资源泄漏问题。

RAII 的核心原理

1. **资源获取与对象初始化绑定**：在对象的构造函数中获取资源（如内存、文件句柄、网络连接等）。
2. **资源释放与对象销毁绑定**：在对象的析构函数中释放资源。当对象超出其作用域或被显式删除时，析构函数会被调用，确保资源被正确释放。

如何使用 RAII 来管理资源

以下是一些常见的 RAII 资源管理示例：

1. 使用智能指针管理动态内存

智能指针是 RAII 的典型应用，用于管理动态分配的内存。`std::unique_ptr` 和 `std::shared_ptr` 是 C++ 标准库中的智能指针。

```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() { std::cout << "MyClass constructor" << std::endl; }
    ~MyClass() { std::cout << "MyClass destructor" << std::endl; }
};

int main() {
    {
	std::unique_ptr<MyClass> ptr = std::make_unique<MyClass>();
	// 当 ptr 超出作用域时，MyClass 对象会自动被销毁，内存被释放
    }
    // 这里 MyClass 对象已经被销毁

    {
	std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>();
	{
	    std::shared_ptr<MyClass> ptr2 = ptr1;  // 共享所有权
	}
	// ptr2 超出作用域，MyClass 对象仍然存在，因为 ptr1 还持有它
    }
    // 这里 MyClass 对象被销毁

    return 0;
}
```

2. 使用自定义类管理文件句柄

可以定义一个类来封装文件操作，利用 RAII 技术确保文件在使用结束后自动关闭。

```cpp
#include <iostream>
#include <fstream>

class FileRAII {
private:
    std::ofstream file;
public:
    FileRAII(const std::string &filename) {
	file.open(filename);
	if (!file.is_open()) {
	    throw std::runtime_error("Failed to open file");
	}
    }
    
    ~FileRAII() {
	if (file.is_open()) {
	    file.close();
	}
    }

    void write(const std::string &data) {
	if (file.is_open()) {
	    file << data;
	}
    }
};

int main() {
    try {
	FileRAII file("example.txt");
	file.write("Hello, RAII!");
	// 文件在这里被自动关闭
    } catch (const std::exception &e) {
	std::cerr << e.what() << std::endl;
    }

    return 0;
}
```

3. 使用标准库中的锁机制

C++ 标准库提供了 RAII 风格的锁机制，如 `std::lock_guard` 和 `std::unique_lock`，用于确保互斥锁在使用结束时被自动释放。

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;

void print_thread_id(int id) {
    std::lock_guard<std::mutex> lock(mtx);
    // 在锁的作用域结束时，mtx 会自动解锁
    std::cout << "Thread ID: " << id << std::endl;
}

int main() {
    std::thread t1(print_thread_id, 1);
    std::thread t2(print_thread_id, 2);

    t1.join();
    t2.join();

    return 0;
}
```

### 什么是智能指针？如何防止内存泄漏？
智能指针是一种在面向对象编程中用于自动管理动态分配内存的工具。它们封装了原生指针，并提供了自动化的内存管理功能，以减少内存泄漏和指针操作错误。智能指针在对象生命周期结束时自动释放内存，因此不需要显式地进行内存释放操作。

在C++中，常见的智能指针包括`std::unique_ptr`、`std::shared_ptr`和`std::weak_ptr`。每种智能指针都有其特定的用途和特性。

#### `std::unique_ptr`
`std::unique_ptr`表示独占所有权的智能指针。它确保一个指针在同一时间内只有一个所有者，适用于管理独占资源。这里std::move()是一个标准库函数，将vec1转化为右值引用，使ptr2可以接管ptr1的资源，而ptr1变成了一个有效但未指定状态的对象，其size()变为0。

```cpp
#include <memory>
#include <iostream>

int main() {
    std::unique_ptr<int> ptr1(new int(10));
    std::cout << *ptr1 << std::endl;

    // std::unique_ptr<int> ptr2 = ptr1; // 错误：不能复制
    std::unique_ptr<int> ptr2 = std::move(ptr1); // 正确：转移所有权,通过将左值转换为右值引用来实现资源的转移，从而提高程序的效率。
    if (!ptr1) {
        std::cout << "ptr1 is null" << std::endl;
    }
    std::cout << *ptr2 << std::endl;

    return 0;
}
```

#### `std::shared_ptr`
`std::shared_ptr`表示共享所有权的智能指针。它允许多个指针共享同一块内存，当最后一个`std::shared_ptr`被销毁时，内存才会被释放。

```cpp
#include <memory>
#include <iostream>

int main() {
    std::shared_ptr<int> ptr1 = std::make_shared<int>(10);
    std::cout << *ptr1 << std::endl;

    std::shared_ptr<int> ptr2 = ptr1; // 共享所有权
    std::cout << *ptr2 << std::endl;

    std::cout << "Use count: " << ptr1.use_count() << std::endl; // 计数为2

    return 0;
}
```

#### `std::weak_ptr`
`std::weak_ptr`是一个不拥有资源的智能指针，它与`std::shared_ptr`配合使用，解决循环引用的问题。当需要访问共享资源但不影响其生命周期时，使用`std::weak_ptr`。

```cpp
#include <memory>
#include <iostream>

int main() {
    std::shared_ptr<int> sp = std::make_shared<int>(10);
    std::weak_ptr<int> wp = sp; // 不增加引用计数

    if (auto spt = wp.lock()) { // 尝试获取共享指针
        std::cout << *spt << std::endl;
    } else {
        std::cout << "Pointer is expired" << std::endl;
    }

    sp.reset(); // 释放共享指针

    if (auto spt = wp.lock()) {
        std::cout << *spt << std::endl;
    } else {
        std::cout << "Pointer is expired" << std::endl;
    }

    return 0;
}
```

#### 防止内存泄漏
智能指针通过以下方式防止内存泄漏：
1. **自动内存管理**：智能指针在离开作用域时自动释放内存。
2. **防止多次释放**：智能指针确保一个对象只会被删除一次，避免重复释放导致的错误。
3. **引用计数**：`std::shared_ptr`通过引用计数机制管理共享资源的生命周期。
4. **弱引用**：`std::weak_ptr`避免循环引用导致的内存泄漏。

使用智能指针时，确保合理选择适合的智能指针类型，以有效管理资源并防止内存泄漏。
	

### 如何禁止类的对象进行复制操作？

为了禁止类的对象进行复制操作，你需要禁用类的拷贝构造函数和赋值运算符。可以通过以下方式实现：

1. 将拷贝构造函数和赋值运算符声明为私有成员，不提供实现。

```cpp
class NoCopy {
private:
    NoCopy(const NoCopy&);
    NoCopy& operator=(const NoCopy&);

public:
    NoCopy() {}
    // ...
};
```

通过将拷贝构造函数和赋值运算符声明为私有成员，你可以防止在类外部调用它们。由于这些函数没有实现，如果在类内部尝试调用它们，会导致编译错误。

2. 使用`delete`关键字（C++11及更高版本）。

```cpp
class NoCopy {
public:
    NoCopy() {}
    NoCopy(const NoCopy&) = delete;
    NoCopy& operator=(const NoCopy&) = delete;
    // ...
};
```

通过将拷贝构造函数和赋值运算符标记为`= delete`，你明确告诉编译器不生成这些函数。如果在任何地方尝试调用它们，都会导致编译错误。

示例：

```cpp
NoCopy obj1;
NoCopy obj2(obj1);  // 编译错误，拷贝构造函数被禁用
obj2 = obj1;         // 编译错误，赋值运算符被禁用
```

3. 使用基类（如`boost::noncopyable`）。

```cpp
#include <boost/noncopyable.hpp>

class NoCopy : private boost::noncopyable {
public:
    NoCopy() {}
    // ...
};
```

通过继承`boost::noncopyable`（或类似的实用工具类），你可以快速地禁用类的复制操作，而无需显式声明拷贝构造函数和赋值运算符。`boost::noncopyable`的实现原理与方法1类似，它将拷贝构造函数和赋值运算符声明为私有成员。

禁止对象复制的用途：

1. 当一个类管理唯一的资源（如文件句柄、线程等）时，复制该类的对象可能会导致资源的重复释放或其他问题。
2. 当一个类的设计是基于唯一性的（如单例模式），复制该类的对象会违反其设计原则。
3. 当复制一个对象的开销很大或者没有意义时，禁止复制可以防止意外的性能问题。


## 重载运算与类型转换

### 什么是运算符重载？（operator overloading）

运算符重载（Operator Overloading）是C++的一项特性，它允许你重新定义运算符的行为，使其能够用于用户自定义的类型（类或结构体）。通过运算符重载，你可以使你的自定义类型像内置类型一样支持各种运算符，从而提高代码的可读性和可维护性。

### 如何重载运算符？

要重载运算符，你需要定义一个特殊的函数，称为运算符重载函数。运算符重载函数的名称由关键字`operator`后跟要重载的运算符符号组成。重载函数可以定义为类的成员函数或全局函数。

以下是重载运算符的几种常见方式：

1. 类的成员函数：

```cpp
class MyClass {
public:
    // 重载二元运算符 +
    MyClass operator+(const MyClass& other) {
        // 实现加法操作
    }

    // 重载前置自增运算符 ++
    MyClass& operator++() {
        // 实现自增操作
        return *this;
    }

    // 重载后置自增运算符 ++
    MyClass operator++(int) {
        MyClass temp(*this);
        // 实现自增操作
        return temp;
    }
};
```

2. 全局函数：

```cpp
// 重载二元运算符 +
MyClass operator+(const MyClass& lhs, const MyClass& rhs) {
    // 实现加法操作
}

// 重载输出运算符 <<
std::ostream& operator<<(std::ostream& os, const MyClass& obj) {
    // 实现输出操作
    return os;
}
```

3. 友元函数：

```cpp
class MyClass {
public:
    // 将全局函数声明为友元函数
    friend MyClass operator+(const MyClass& lhs, const MyClass& rhs);
    friend std::ostream& operator<<(std::ostream& os, const MyClass& obj);
};

// 实现友元函数
MyClass operator+(const MyClass& lhs, const MyClass& rhs) {
    // 实现加法操作
}

std::ostream& operator<<(std::ostream& os, const MyClass& obj) {
    // 实现输出操作
    return os;
}
```

在重载运算符时，需要注意以下几点：

1. 运算符重载函数的参数数量由运算符的操作数决定。一元运算符接受零个或一个参数，二元运算符接受一个或两个参数。
2. 对于二元运算符，如果重载函数是类的成员函数，那么它只接受一个参数（右操作数），因为左操作数是`this`指针所指向的对象。
3. 某些运算符必须被定义为类的成员函数，如赋值运算符`=`、下标运算符`[]`和函数调用运算符`()`。
4. 重载运算符时，尽量保持运算符的原有语义，以避免混淆和错误。


### 重载哪些运算符是合法的？

可以重载的运算符

| 运算符  | 说明                 | 运算符  | 说明                    |
|---------|----------------------|---------|-------------------------|
| `+`     | 加法                 | `+=`    | 加法赋值                |
| `-`     | 减法                 | `-=`    | 减法赋值                |
| `*`     | 乘法                 | `*=`    | 乘法赋值                |
| `/`     | 除法                 | `/=`    | 除法赋值                |
| `%`     | 取模                 | `%=`    | 取模赋值                |
| `++`    | 自增                 | `--`    | 自减                    |
| `==`    | 等于                 | `!=`    | 不等于                  |
| `<`     | 小于                 | `>`     | 大于                    |
| `<=`    | 小于等于             | `>=`    | 大于等于                |
| `[]`    | 下标访问             | `()`    | 函数调用                |
| `->`    | 成员访问（指针）     | `->*`   | 指针成员访问            |
| `&`     | 按位与、取地址       | `|`     | 按位或                  |
| `^`     | 按位异或             | `~`     | 按位取反                |
| `&=`    | 按位与赋值           | `|=`    | 按位或赋值              |
| `^=`    | 按位异或赋值         | `<<`    | 左移                    |
| `>>`    | 右移                 | `<<=`   | 左移赋值                |
| `>>=`   | 右移赋值             | `!`     | 逻辑非                  |
| `&&`    | 逻辑与               | `||`    | 逻辑或                  |
| `,`     | 逗号                 | `=`     | 赋值                    |
| `*`     | 间接访问（指针解引用）| `&`     | 地址符                  |
| `new`   | 动态分配             | `delete`| 动态释放                |
| `new[]` | 动态数组分配         | `delete[]` | 动态数组释放           |

不能重载的运算符

| 运算符  | 说明                   |
|---------|------------------------|
| `::`    | 作用域解析符            |
| `.`     | 成员访问运算符          |
| `.*`    | 成员指针访问运算符      |
| `?:`    | 三元条件运算符          |
| `sizeof`| 计算对象大小            |
| `typeid`| RTTI 类型信息           |
| `const_cast`| 类型转换             |
| `dynamic_cast`| 动态类型转换      |
| `reinterpret_cast`| 重新解释转换  |
| `static_cast`| 静态类型转换       |

### 什么是隐式类型转换？
隐式类型转换（Implicit Type Conversion），也称为隐式转换或自动类型转换，是指编译器在某些情况下自动将一种数据类型转换为另一种数据类型，而无需显式地使用类型转换运算符。隐式类型转换主要有以下几种情况：

1. 算术转换（Arithmetic Conversion）：
   - 在算术表达式中，较小的整数类型（如`char`、`short`）会被自动转换为较大的整数类型（如`int`）。
   - 在算术表达式中，如果操作数中有一个是浮点类型（`float`、`double`），那么另一个操作数会被自动转换为浮点类型。

```cpp
int a = 10;
double b = 3.14;
double c = a + b; // a 被隐式转换为 double
```

2. 赋值转换（Assignment Conversion）：
   - 当将一个值赋给另一个类型的变量时，如果目标类型能够容纳源类型的值，编译器会自动执行类型转换。

```cpp
int a = 3.14; // 3.14 被隐式转换为 int，a 的值为 3
```

3. 函数参数转换（Function Argument Conversion）：
   - 在函数调用时，如果实参的类型与形参的类型不完全匹配，编译器会自动执行类型转换。

```cpp
void func(double x) { /* ... */ }
int a = 10;
func(a); // a 被隐式转换为 double
```

4. 布尔转换（Boolean Conversion）：
   - 在条件语句（如`if`、`while`）中，非布尔类型的值会被自动转换为布尔类型。
   - 零值（如0、0.0、NULL、nullptr）会被转换为`false`，非零值会被转换为`true`。

```cpp
int a = 0;
if (a) { /* ... */ } // a 被隐式转换为 false
```

5. 指针转换（Pointer Conversion）：
   - `NULL`或者`0`可以被隐式转换为任意类型的指针。
   - `void*`类型的指针可以被隐式转换为其他类型的指针。

```cpp
int* p = NULL; // NULL 被隐式转换为 int*
void* v = p;   // int* 被隐式转换为 void*
```

隐式类型转换可以简化代码，提高代码的可读性。但是，在某些情况下，隐式类型转换也可能导致意外的结果或错误。因此，在编写代码时，应该注意隐式类型转换的发生，必要时使用显式类型转换（如`static_cast`、`reinterpret_cast`等）来避免潜在的问题。

### 如何防止意外的隐式类型转换？

显示类型转换，和explicit关键字

### 运算符重载和类型转换有什么限制？

在C++中，运算符重载和类型转换有一些限制和约定，以确保代码的可读性、可维护性和安全性。以下是一些主要的限制：

1. 不能重载的运算符：
   - 成员访问运算符：`.`
   - 成员指针访问运算符：`.*`
   - 域运算符：`::`
   - 条件运算符：`?:`
   - sizeof运算符
   - typeid运算符

2. 不能改变运算符的优先级和结合性：
   - 重载运算符的优先级和结合性与内置运算符相同，不能被改变。

3. 不能改变运算符的操作数个数：
   - 重载运算符时，不能改变运算符的操作数个数。例如，不能将二元运算符重载为一元运算符。

4. 重载运算符时，至少有一个操作数必须是用户定义的类型：
   - 这是为了防止用户重载内置类型的运算符。

5. 某些运算符必须被定义为成员函数：
   - 赋值运算符`=`
   - 下标运算符`[]`
   - 函数调用运算符`()`
   - 成员访问运算符`->`

6. 重载运算符时，应该保持运算符的常规含义：
   - 重载运算符时，应该遵循运算符的常规语义，以免引起混淆。例如，`+`运算符应该表示加法或连接的含义。

7. 避免隐式类型转换的二义性：
   - 如果一个类同时定义了多个转换函数或转换构造函数，可能导致隐式类型转换的二义性。在这种情况下，编译器将报错。

8. 显式类型转换运算符不能用于隐式类型转换：
   - `explicit`关键字可以用于转换构造函数和类型转换运算符，以防止隐式类型转换。

9. 避免使用C风格的类型转换：
   - 建议使用C++的显式类型转换运算符（`static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`），而不是C风格的类型转换。

10. 谨慎使用用户定义的隐式类型转换：
    - 虽然用户定义的隐式类型转换可以简化代码，但过度使用可能导致代码难以理解和维护。应该根据实际需求，权衡隐式类型转换的利弊。


## 面向对象程序设计

### 什么是多重继承？它有什么优点和缺点？


### 什么是虚继承？它有什么优点和缺点？

通过在继承路径中使用virtual关键字，可以避免在派生类中生成多个基类的实例，从而解决了菱形继承带来的二义性。

### 什么是右值引用？如何使用右值引用？（C++11新特性）

右值引用（Rvalue Reference）是C++11引入的一种新的引用类型，它用于绑定到临时对象或不具名的值（如字面量、表达式等）。右值引用的主要目的是实现移动语义（Move Semantics），以提高程序的性能。

右值引用的语法是在类型名称后面加上两个&符号（&&），例如：

```cpp
int&& rref = 42;
```

右值引用的特点：
1. 绑定到临时对象或不具名的值。
2. 允许从临时对象或不具名的值中"窃取"资源，避免不必要的拷贝操作。
3. 可以用于实现移动构造函数和移动赋值运算符。
4. 具有延长临时对象生命周期的能力。

使用右值引用的几种常见方式：

1. 实现移动构造函数和移动赋值运算符：
```cpp
class MyString {
public:
    // Move constructor
    MyString(MyString&& other) : data(other.data) {
	other.data = nullptr;
    }

    // Move assignment operator
    MyString& operator=(MyString&& other) {
	if (this != &other) {
	    delete[] data;
	    data = other.data;
	    other.data = nullptr;
	}
	return *this;
    }

private:
    char* data;
};
```

2. 使用`std::move`将左值转换为右值，以便调用移动构造函数或移动赋值运算符：
```cpp
MyString str1("Hello");
MyString str2(std::move(str1)); // 调用移动构造函数
str1 = MyString("World"); // 调用移动赋值运算符
```

3. 在模板函数中使用右值引用作为参数，以实现完美转发（Perfect Forwarding）：
```cpp
template <typename T>
void forwardValue(T&& value) {
    processValue(std::forward<T>(value));
}
```

4. 在函数返回值中使用右值引用，以避免不必要的拷贝操作：
```cpp
MyString createString() {
    MyString str("Hello");
    return std::move(str);
}
```

使用右值引用可以显著提高程序的性能，特别是在处理大型对象或需要频繁移动对象时。但是，使用右值引用也需要注意一些细节，如确保移动后的对象处于合法状态、避免悬空引用等。同时，也要平衡可读性和性能，不要过度追求性能而牺牲代码的可读性和维护性。
		

### 什么是移动构造函数(move constructor)？为什么需要移动构造函数？

移动构造函数通常用于将资源从一个对象“移动”到另一个对象，而不是进行传统的拷贝操作。这对于处理动态分配的资源，如堆上的内存或其他资源，非常有用，因为移动资源比拷贝资源更高效。

移动构造函数的语法如下：
class MyClass {
public:
    // 移动构造函数
    MyClass(MyClass&& other) noexcept {
	// 进行资源的移动操作，例如，转移指针、拷贝计数等
	// ...
    }

    // 其他成员函数和构造函数
    // ...
};

移动构造函数的参数类型是右值引用 MyClass&&。移动构造函数通常会在对象的源对象上执行一些操作，例如将指针或资源所有权转移到目标对象，然后将源对象置于一种可析构但不再拥有资源的状态。避免在资源管理上进行深层次的拷贝，提高程序的性能。

### 什么是抽象类？如何定义抽象类？

	- 当我们需要定义某一个成员函数并且希望他被所有的继承类使用，在基类中又没有足够的信息来定义，我们就将这个成员函数定义成一个虚函数，这个基类就变成了一个抽象类。抽象类是一种在面向对象编程中的概念，它不能被实例化，而是被用作其他类的基类。抽象类里面可以包括抽象方法，抽象类只提供这些方法的声明而没有实现。

	- 一个基类通过在类里面定义一个纯虚函数编程抽象类，在基类的成员函数声明前加virtual，函数声明之后加=0。

### 什么是虚函数？为什么需要虚函数？

	- 虚函数让派生类可以重写基类里面成员函数，通过使用虚函数，可以确定调用的是哪个版本的函数，从而实现动态绑定。要在基类里面成员函数之前加virtual声明虚函数，在继承类中重写函数时可以加上override，这样如果函数重写失败编译器会检查并报错。

	- 动态绑定： 使用虚函数可以在运行时（动态时期）确定要调用的函数，而不是在编译时（静态时期）。这被称为动态绑定或运行时多态性。
实现多态性： 虚函数为实现多态性提供了机制，允许不同的派生类对象通过相同的基类接口进行操作。这样，通过基类指针或引用调用虚函数时，实际执行的是具体派生类的版本。

### 什么是纯虚函数？为什么需要纯虚函数？

	- 纯虚函数（Pure Virtual Function）是在C++中定义抽象类（Abstract Class）的一种机制。抽象类是不能被实例化的类，而包含至少一个纯虚函数的类就是抽象类。派生类必须实现抽象类里面的纯虚函数，不然他自己也会变成一个抽象类。

## 模板与泛型编程

### 什么是C++模板？你能给一个例子吗？什么是函数模板？如何使用函数模板？什么是模板类？它有什么作用？
以下是对您提出的三个问题的详细回答：

### 什么是C++模板？你能给一个例子吗？

C++模板是一种编程工具，允许您编写通用的、可重用的代码。模板提供了一种方法，可以将类型作为参数传递给函数或类，从而实现代码的参数化和泛型化。

模板分为两种类型：函数模板和类模板。

函数模板的例子：

```cpp
template <typename T>
T max(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    int maxInt = max(5, 10);             // 使用int类型
    double maxDouble = max(3.14, 2.71);  // 使用double类型
    return 0;
}
```

在上面的例子中，`max`函数是一个函数模板，它接受两个类型为`T`的参数，并返回其中较大的值。通过使用不同的类型调用`max`函数，编译器会自动生成相应类型的函数实例。

### 什么是函数模板？如何使用函数模板？

函数模板是一种通用的函数定义，它允许您使用不同的数据类型来调用相同的函数。函数模板使用模板参数来表示数据类型，编译器会根据实际传递的参数类型自动生成相应的函数实例。

函数模板的定义语法如下：

```cpp
template <typename T>
返回类型 函数名(参数列表) {
    // 函数体
}
```

其中，`typename`关键字用于声明类型参数`T`，您也可以使用`class`关键字。

函数模板的使用示例：

```cpp
template <typename T>
void print(T value) {
    std::cout << value << std::endl;
}

int main() {
    print(42);        // 输出整数
    print(3.14);      // 输出浮点数
    print("Hello");   // 输出字符串
    return 0;
}
```

在上面的例子中，`print`函数是一个函数模板，它接受一个类型为`T`的参数，并将其输出到标准输出流。通过使用不同类型的参数调用`print`函数，编译器会自动生成相应类型的函数实例。

### 什么是模板类？它有什么作用？

模板类是一种通用的类定义，它允许您使用不同的数据类型来创建类的实例。模板类使用模板参数来表示数据类型，编译器会根据实际指定的类型参数生成相应的类实例。

模板类的作用包括：

1. 代码重用：模板类可以用于不同的数据类型，避免了为每种类型编写重复的代码。
2. 泛型编程：模板类提供了一种编写通用算法和数据结构的方法，提高了代码的灵活性和可扩展性。
3. 性能优化：模板类在编译时生成具体的类实例，可以进行编译器优化，提高代码的执行效率。

模板类的定义语法如下：

```cpp
template <typename T>
class 类名 {
    // 类的成员和方法
};
```

模板类的使用示例：

```cpp
template <typename T>
class Stack {
private:
    T* data;
    int size;
    int capacity;

public:
    Stack(int cap = 10) {
        data = new T[cap];
        size = 0;
        capacity = cap;
    }

    ~Stack() {
        delete[] data;
    }

    void push(const T& value) {
        // 实现压栈操作
    }

    T pop() {
        // 实现弹栈操作
    }

    bool isEmpty() const {
        return size == 0;
    }
};

int main() {
    Stack<int> intStack;       // 创建整数类型的栈
    Stack<std::string> strStack;  // 创建字符串类型的栈
    return 0;
}
```

在上面的例子中，`Stack`类是一个模板类，它使用模板参数`T`来表示栈中存储的数据类型。通过指定不同的类型参数，可以创建不同类型的栈实例，如整数栈和字符串栈。

总之，C++模板是一种强大的编程工具，它允许您编写通用的、可重用的代码。函数模板和类模板都使用模板参数来实现代码的参数化和泛型化，提高了代码的灵活性、可扩展性和性能。合理使用模板可以简化代码编写，提高开发效率。

### 什么是模板偏特化（Template Partial Specialization）？它有什么作用？

模板偏特化(Template Partial Specialization)是C++模板的一种高级特性,它允许对模板参数进行部分特化,提供更灵活和特定的实现。模板偏特化是在模板参数列表中指定一部分参数,而将其他参数保持通用性。

模板偏特化的语法形式如下:

```cpp
template <模板参数列表>
class 类名<特化参数列表> {
    // 偏特化类的成员变量和成员函数
};
```

其中,`模板参数列表`是通用的模板参数列表,可以包含类型参数和非类型参数。`类名`是要偏特化的模板类的名称。`特化参数列表`是偏特化的参数列表,可以指定一部分参数的具体类型或值,而将其他参数保持通用性。

模板偏特化的作用包括:

1. 针对特定类型提供特定实现: 模板偏特化允许对特定的类型组合提供专门的实现,可以针对这些特定类型进行优化和定制。

2. 解决模板参数的限制: 有时候,通用的模板实现可能不适用于某些特定的类型组合。通过模板偏特化,可以为这些特定类型组合提供合适的实现。

3. 改变模板的行为: 模板偏特化可以改变模板的行为,例如提供不同的成员函数、不同的内存布局等,以满足特定类型的需求。

4. 提高代码的可读性和可维护性: 通过模板偏特化,可以将特定类型的实现与通用实现分离,使代码更加清晰和易于维护。

下面是一个模板偏特化的例子,用于实现一个简单的数组容器:

```cpp
template <typename T, int Size>
class MyArray {
    // 通用实现
};

template <typename T>
class MyArray<T, 0> {
    // 针对Size为0的特化实现
};

template <int Size>
class MyArray<bool, Size> {
    // 针对T为bool的特化实现
};
```

在这个例子中,`MyArray`是一个包含两个模板参数`T`和`Size`的模板类。第一个偏特化`MyArray<T, 0>`针对`Size`为0的情况提供了特化实现。第二个偏特化`MyArray<bool, Size>`针对`T`为`bool`类型的情况提供了特化实现。

使用模板偏特化的示例:

```cpp
MyArray<int, 5> intArray;     // 使用通用实现
MyArray<double, 0> emptyArray;  // 使用针对Size为0的特化实现
MyArray<bool, 10> boolArray;    // 使用针对T为bool的特化实现
```

在这个示例中,根据实例化时提供的模板参数,编译器会选择适当的实现:通用实现、针对`Size`为0的特化实现或针对`T`为`bool`的特化实现。

模板偏特化的一些注意事项:

1. 偏特化与通用实现的匹配: 当存在多个偏特化和通用实现时,编译器会根据实例化时提供的模板参数,选择最特定的匹配。偏特化的匹配优先级高于通用实现。

2. 偏特化的顺序: 偏特化的定义顺序不影响匹配结果,编译器会根据特化程度来选择最匹配的实现。

3. 偏特化与全特化: 除了偏特化,还有全特化(Template Full Specialization),即为所有模板参数提供具体的类型或值。全特化的匹配优先级高于偏特化。

4. 函数模板的偏特化: 与类模板不同,函数模板不支持偏特化。但是,可以通过函数重载和启用If-Constexpr（从 C++17 开始）来实现类似的效果。

总的来说，模板偏特化是 C++ 模板的一个强大特性，它允许对模板参数进行部分特化，提供更灵活和特定的实现。通过模板偏特化，可以针对特定类型提供优化和定制，解决通用实现的限制，改变模板的行为，提高代码的可读性和可维护性。合理使用模板偏特化可以增强模板的适用性和表现力，是高级 C++ 编程中的重要工具。

### 什么是类型推导（Type Deduction）？C++11引入了哪些类型推导方法？

类型推导(Type Deduction)是C++11引入的一个重要特性,它允许编译器根据上下文信息自动推断变量、函数参数或函数返回值的类型,而无需显式指定类型。类型推导可以简化代码,提高编程效率,并增强代码的可读性。

C++11引入了几种类型推导方法:

1. auto关键字:
   - `auto`关键字可以用于变量声明和初始化,编译器会根据初始值的类型自动推断变量的类型。
   - 例如:`auto x = 10;`  // x的类型为int
   - `auto`也可以用于函数返回值类型的推导,例如:`auto func() { return 42; }`  // 返回值类型为int

2. decltype关键字:
   - `decltype`关键字可以用于获取表达式的类型,而无需计算表达式的值。
   - 例如:`int x = 10; decltype(x) y = 20;`  // y的类型为int
   - `decltype`常用于推断函数返回值类型,特别是在模板函数中,例如:
     ```cpp
     template <typename T, typename U>
     auto add(T x, U y) -> decltype(x + y) {
         return x + y;
     }
     ```

3. 函数模板参数推导:
   - 对于函数模板,编译器可以根据实参的类型自动推断模板参数的类型。
   - 例如:
     ```cpp
     template <typename T>
     void print(T value) {
         std::cout << value << std::endl;
     }
     print(42);  // T被推导为int
     print("Hello");  // T被推导为const char*
     ```

4. lambda表达式参数推导:
   - 对于lambda表达式,编译器可以根据传递给lambda的实参自动推断参数的类型。
   - 例如:
     ```cpp
     auto lambda = [](auto x, auto y) {
         return x + y;
     };
     int result1 = lambda(10, 20);  // x和y被推导为int
     double result2 = lambda(3.14, 2.5);  // x和y被推导为double
     ```

5. 类模板参数推导(C++17):
   - 从C++17开始,类模板也支持参数推导,可以根据构造函数的实参自动推断模板参数的类型。
   - 例如:
     ```cpp
     template <typename T>
     class Vector {
     public:
         Vector(T x, T y) : x(x), y(y) {}
     private:
         T x;
         T y;
     };
     Vector v(1, 2);  // T被推导为int
     ```

类型推导的一些注意事项:
1. 类型推导是在编译时进行的,不会影响运行时性能。
2. 类型推导并不总是最佳选择,有时显式指定类型可以提高代码的可读性和维护性。
3. 类型推导可能会导致意外的类型转换或精度损失,需要注意推导的类型是否符合预期。
4. 在某些情况下,类型推导可能会导致编译错误,特别是当推导的类型不明确或存在二义性时。

总的来说,类型推导是C++11引入的一个方便且强大的特性,可以简化代码,提高编程效率。通过`auto`、`decltype`、函数模板参数推导、lambda表达式参数推导等方法,编译器可以自动推断变量、函数参数和返回值的类型。合理使用类型推导可以使代码更加简洁和易读,但也需要注意推导的类型是否符合预期,避免意外的类型转换或编译错误。
