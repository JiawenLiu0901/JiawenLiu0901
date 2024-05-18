---
weight: 4
title: "C++面向对象"
date: 2023-03-20T21:57:40+08:00
lastmod: 2023-03-20T16:45:40+08:00
draft: false
author: "Jiawen Liu"
authorLink: "https://jiawenliu0901.github.io"
description: "C++面向对象知识点整理总结"
tags: ["C++"]
categories: ["C++八股"]
lightgallery: true
---
## C++面向对象

### 拷贝控制

- 什么是移动构造函数和移动赋值运算符？

- 什么是拷贝构造函数？

	- 用于通过已经存在的对象创建一个新的对象，新对象是原对象的副本，参数通常是对同类型对象的引用。

- 什么是赋值运算符？

	- =

- 什么是默认构造函数（default constructor)？

	- 默认构造函数指的是一个没有任何参数的构造函数，如果没有一个这样的构造函数存在，或者是private的话，会造成编译错误。如果类里面没有提供任何的构造函数，编译器会提供一个implicit default constructor。如果类里面已经定义过了构造函数，我们就要手动定义默认构造函数，这样的默认构造函数称之为explicit default constructor

- 什么时候会调用拷贝构造函数（copy constructor）？

	- 对象初始化： 当一个对象通过另一个对象进行初始化时，拷贝构造函数会被调用。这包括对象的直接初始化和拷贝初始化。

	- 函数参数传递： 当一个对象作为函数的参数传递给另一个函数时，拷贝构造函数会被调用。

	- 函数返回值： 当一个函数返回一个对象时，拷贝构造函数会被调用，将函数内部的局部对象复制到函数外部。
MyClass createObject() {
    // 拷贝构造函数在这里被调用
    return MyClass();
}

	- 对象赋值： 当一个对象被另一个对象赋值时，拷贝构造函数会被调用。MyClass obj2 = obj1;

- 什么时候会调用赋值运算符？

	- 显示赋值操作

	- 初始化对象

	- 返回值赋值

	- 成员函数返回对象

- 如何实现深拷贝和浅拷贝？
	- 深拷贝：深拷贝是指在拷贝对象时，会同时复制对象所引用的动态分配的内存。这样，两个对象拥有独立的内存空间，互不影响。实现深拷贝通常需要手动编写拷贝构造函数和/或赋值运算符，确保在拷贝对象时分配新的内存并复制数据。
	- 浅拷贝：浅拷贝是指仅复制对象本身，而不复制对象所引用的动态分配的内存。两个对象将共享相同的内存区域，因此修改其中一个对象的数据会影响到另一个对象。

默认情况下，C++提供的拷贝构造函数和赋值运算符执行的是浅拷贝。

			-     // 构造函数
    ShallowCopy(const char *str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    // 拷贝构造函数（默认是浅拷贝）
    ShallowCopy(const ShallowCopy &other) {
        data = other.data;
    }

- 什么是 RAII（Resource Acquisition Is Initialization）技术，如何使用它来管理资源？

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

- 什么是智能指针？如何防止内存泄漏？
智能指针是一种在面向对象编程中用于自动管理动态分配内存的工具。它们封装了原生指针，并提供了自动化的内存管理功能，以减少内存泄漏和指针操作错误。智能指针在对象生命周期结束时自动释放内存，因此不需要显式地进行内存释放操作。

在C++中，常见的智能指针包括`std::unique_ptr`、`std::shared_ptr`和`std::weak_ptr`。每种智能指针都有其特定的用途和特性。

### `std::unique_ptr`
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

### `std::shared_ptr`
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

### `std::weak_ptr`
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

### 防止内存泄漏
智能指针通过以下方式防止内存泄漏：
1. **自动内存管理**：智能指针在离开作用域时自动释放内存。
2. **防止多次释放**：智能指针确保一个对象只会被删除一次，避免重复释放导致的错误。
3. **引用计数**：`std::shared_ptr`通过引用计数机制管理共享资源的生命周期。
4. **弱引用**：`std::weak_ptr`避免循环引用导致的内存泄漏。

使用智能指针时，确保合理选择适合的智能指针类型，以有效管理资源并防止内存泄漏。
	

- 如何禁止类的对象进行复制操作？

	- 删除拷贝构造函数和拷贝赋值运算符

### 重载运算与类型转换

- 什么是运算符重载？（operator overloading）

	- 运算符重载是一种编程语言特性，允许程序员重新定义或者扩展已有的运算符的行为。在 C++ 中，运算符重载是通过创建特定的成员函数或全局函数来实现的。通过运算符重载，可以使用户自定义的类或数据类型支持类似内置数据类型的运算和操作。

- 如何重载运算符？

	- 通过成员函数重载

	- 通过全局函数重载

- 重载哪些运算符是合法的？
	- 可以重载的运算符

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

	- 不能重载的运算符

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

- 什么是隐式类型转换？

- 如何防止意外的隐式类型转换？

- 运算符重载和类型转换有什么限制？

### 面向对象程序设计

- 什么是多重继承？它有什么优点和缺点？

	- 多重继承是一个类可以从多个父类继承属性和行为。

	- 优点：

	- 缺点：可能引入菱形继承，比如一个类同时继承了两个相同基类的类，而最终的派生类又同时继承了这两个类，可能导致二义性（最终的派生类无法确定应该调用哪一个基类进行实现）

- 什么是虚继承？它有什么优点和缺点？

	- 通过在继承路径中使用virtual关键字，可以避免在派生类中生成多个基类的实例，从而解决了菱形继承带来的二义性。

- 什么是右值引用？如何使用右值引用？（C++11新特性）

	- https://www.jianshu.com/p/d19fc8447eaa

- 什么是移动构造函数？为什么需要移动构造函数？

	- 移动构造函数通常用于将资源从一个对象“移动”到另一个对象，而不是进行传统的拷贝操作。这对于处理动态分配的资源，如堆上的内存或其他资源，非常有用，因为移动资源比拷贝资源更高效。

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

	- 移动构造函数的参数类型是右值引用 MyClass&&。
移动构造函数通常会在对象的源对象上执行一些操作，例如将指针或资源所有权转移到目标对象，然后将源对象置于一种可析构但不再拥有资源的状态。

	- 避免在资源管理上进行深层次的拷贝，提高程序的性能。例如，在返回局部对象的函数中，使用移动构造函数可以避免拷贝：

- 什么是抽象类？如何定义抽象类？

	- 当我们需要定义某一个成员函数并且希望他被所有的继承类使用，在基类中又没有足够的信息来定义，我们就将这个成员函数定义成一个虚函数，这个基类就变成了一个抽象类。抽象类是一种在面向对象编程中的概念，它不能被实例化，而是被用作其他类的基类。抽象类里面可以包括抽象方法，抽象类只提供这些方法的声明而没有实现。

	- 一个基类通过在类里面定义一个纯虚函数编程抽象类，在基类的成员函数声明前加virtual，函数声明之后加=0。

- 什么是虚函数？为什么需要虚函数？

	- 虚函数让派生类可以重写基类里面成员函数，通过使用虚函数，可以确定调用的是哪个版本的函数，从而实现动态绑定。要在基类里面成员函数之前加virtual声明虚函数，在继承类中重写函数时可以加上override，这样如果函数重写失败编译器会检查并报错。

	- 动态绑定： 使用虚函数可以在运行时（动态时期）确定要调用的函数，而不是在编译时（静态时期）。这被称为动态绑定或运行时多态性。
实现多态性： 虚函数为实现多态性提供了机制，允许不同的派生类对象通过相同的基类接口进行操作。这样，通过基类指针或引用调用虚函数时，实际执行的是具体派生类的版本。

- 什么是纯虚函数？为什么需要纯虚函数？

	- 纯虚函数（Pure Virtual Function）是在C++中定义抽象类（Abstract Class）的一种机制。抽象类是不能被实例化的类，而包含至少一个纯虚函数的类就是抽象类。派生类必须实现抽象类里面的纯虚函数，不然他自己也会变成一个抽象类。

### 模板与泛型编程

- 什么是C++模板？你能给一个例子吗？

- 什么是函数模板？如何使用函数模板？

- 什么是模板类？它有什么作用？

- 什么是模板偏特化（Template Partial Specialization）？它有什么作用？

- 什么是类型推导（Type Deduction）？C++11引入了哪些类型推导方法？
