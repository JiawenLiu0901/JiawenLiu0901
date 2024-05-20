---
weight: 4
title: "C++知识点（其他）"
date: 2024-5-20T21:57:40+08:00
lastmod: 2024-5-20T16:45:40+08:00
draft: false
author: "Jiawen Liu"
authorLink: "https://jiawenliu0901.github.io"
description: "C++"
tags: ["C++"]
categories: ["C++八股"]
lightgallery: true
---
# C++知识点

## 其他

### string和char数组有什么区别？什么时候应该使用哪个？

使用char数组的情况:

与C语言接口交互:某些C语言库或函数可能要求使用char数组作为参数或返回值。
固定大小的字符串:如果字符串的大小是固定的且较小,使用char数组可以避免string的动态内存分配开销。
性能要求极高:在对性能要求非常严格的场景下,直接使用char数组可能会有一些性能优势。


使用string的情况:

字符串的大小可变:当字符串的大小不固定或可能动态改变时,使用string更加方便。
字符串操作频繁:如果需要频繁进行字符串的追加、插入、删除等操作,string提供了更方便的接口。
代码的可读性和维护性:使用string可以提高代码的可读性和维护性,减少手动内存管理的错误。
与C++标准库交互:C++标准库中的许多函数和类都使用string作为参数或返回值。

在对性能要求非常严格的场景下,直接使用char数组可能比使用std::string有一些性能优势,主要原因如下:

内存分配和释放:

std::string是一个动态分配内存的类,在创建、修改和销毁字符串时,可能会频繁地进行内存分配和释放操作。
char数组通常在栈上分配内存,或者在程序启动时预分配内存,避免了动态内存分配和释放的开销。


字符串长度的计算:

std::string需要维护字符串的长度信息,在某些操作(如获取字符串长度)时,需要进行额外的计算。
char数组可以直接使用字符串的长度信息(如果提前知道),或者使用strlen等函数快速获取字符串长度,减少了长度计算的开销。


字符串拷贝和赋值:

std::string在进行拷贝和赋值操作时,可能会触发内存的重新分配和数据的复制,特别是当字符串较长时,开销较大。
char数组可以使用memcpy等函数进行快速的内存拷贝,避免了额外的内存分配和复制开销。


字符串比较:

std::string的比较操作通常涉及字符串长度的比较和逐字符的比较,有一定的计算开销。
char数组可以使用strcmp等函数进行快速的字符串比较,减少了比较的开销。



需要注意的是,上述性能优势主要体现在对性能要求非常严格的场景下,如高度优化的算法、高并发的网络通信、实时系统等。在一般的应用程序中,使用std::string的性能通常已经足够好,而且std::string提供了更多的功能和便利性。

### C++标准库中的算法有哪些？它们可以用于哪些容器？
C++标准库提供了丰富的算法,可以用于对容器中的元素进行排序、查找、修改、组合等操作。这些算法大多定义在`<algorithm>`头文件中,少部分定义在`<numeric>`头文件中。

以下是一些常用的C++标准库算法及其适用的容器:

1. 非修改序列操作:
   - `find`、`find_if`、`find_if_not`: 在容器中查找满足条件的元素。
   - `count`、`count_if`: 统计容器中满足条件的元素个数。
   - `equal`: 判断两个范围内的元素是否相等。
   - `search`: 在容器中查找子序列。
   - 适用容器:可用于顺序容器(如`vector`、`deque`、`list`)和关联容器(如`set`、`multiset`、`map`、`multimap`)。

2. 修改序列操作:
   - `copy`、`copy_if`、`copy_n`、`copy_backward`: 复制容器中的元素。
   - `move`: 移动容器中的元素。
   - `fill`、`fill_n`: 用指定值填充容器中的元素。
   - `transform`: 对容器中的元素进行转换操作。
   - `replace`、`replace_if`: 替换容器中的元素。
   - `swap`: 交换两个容器中的元素。
   - 适用容器:主要用于顺序容器,部分算法可用于关联容器。

3. 划分操作:
   - `partition`、`stable_partition`: 将容器中的元素按条件划分为两部分。
   - 适用容器:主要用于顺序容器。

4. 排序操作:
   - `sort`、`stable_sort`: 对容器中的元素进行排序。
   - `partial_sort`、`partial_sort_copy`: 对容器中的元素进行部分排序。
   - `nth_element`: 将容器中的第n个元素放在正确的位置上。
   - 适用容器:主要用于顺序容器,关联容器自身已经维护了有序性。

5. 二分查找操作:
   - `binary_search`: 在已排序的容器中进行二分查找。
   - `lower_bound`、`upper_bound`: 在已排序的容器中查找元素的下界和上界。
   - 适用容器:可用于已排序的顺序容器和关联容器。

6. 集合操作:
   - `set_union`: 计算两个集合的并集。
   - `set_intersection`: 计算两个集合的交集。
   - `set_difference`: 计算两个集合的差集。
   - 适用容器:主要用于已排序的顺序容器。

7. 堆操作:
   - `make_heap`、`push_heap`、`pop_heap`: 对容器中的元素建堆、插入元素和删除堆顶元素。
   - `sort_heap`: 对容器中的元素进行堆排序。
   - 适用容器:主要用于顺序容器。

8. 最小/最大操作:
   - `min`、`max`、`minmax`: 返回两个元素中的最小值、最大值或同时返回最小值和最大值。
   - `min_element`、`max_element`、`minmax_element`: 返回容器中的最小元素、最大元素或同时返回最小元素和最大元素。
   - 适用容器:可用于顺序容器和关联容器。

9. 数值操作:
   - `accumulate`: 对容器中的元素进行累加操作。
   - `inner_product`: 计算两个容器中元素的内积。
   - 适用容器:主要用于顺序容器。

这只是C++标准库算法的一部分,还有许多其他算法可供使用。大多数算法都接受迭代器作为参数,因此可以用于各种支持迭代器的容器。

### C++标准库中的异常处理机制是什么？它们是如何工作的？
try, catch, throw

### C++标准库中的文件流有哪些？如何读取和写入文件？

C++ 标准库中的文件流主要涉及 <fstream> 头文件，提供了用于文件输入和输出的类和函数。

主要的文件流类有：

std::ifstream： 用于从文件中读取数据，即输入文件流。
std::ofstream： 用于将数据写入文件，即输出文件流。
std::fstream： 同时具有输入和输出功能的文件流

### C++11 标准库：什么是 C++11 标准库？C++11 标准库中有哪些新增的容器？C++11 标准库中有哪些新增的算法？C++11 标准库中有哪些新增的线程和同步原语？
C++11标准库是C++11标准引入的新版本标准库,提供了许多新的特性、容器、算法和工具,以增强C++的功能和性能。下面详细介绍C++11标准库的一些主要内容:

1. C++11标准库中新增的容器:
   - `std::array`:固定大小的数组容器,提供了类似于内置数组的功能,但具有更好的安全性和接口一致性。
   - `std::forward_list`:单向链表容器,类似于`std::list`,但只支持单向遍历,内存开销更小。
   - `std::unordered_map`:无序关联容器,使用哈希表实现,提供了快速的键值对插入、删除和查找操作。
   - `std::unordered_set`:无序关联容器,使用哈希表实现,提供了快速的元素插入、删除和查找操作。
   - `std::unordered_multimap`:无序关联容器,允许键重复,使用哈希表实现。
   - `std::unordered_multiset`:无序关联容器,允许元素重复,使用哈希表实现。

2. C++11标准库中新增的算法:
   - `std::all_of`、`std::any_of`、`std::none_of`:用于检查范围内的元素是否满足给定的条件。
   - `std::find_if_not`:在范围内查找第一个不满足条件的元素。
   - `std::copy_if`:将范围内满足条件的元素复制到另一个范围。
   - `std::move`、`std::move_backward`:将元素从一个范围移动到另一个范围。
   - `std::shuffle`:随机打乱范围内的元素顺序。
   - `std::is_sorted`、`std::is_sorted_until`:检查范围内的元素是否已排序。

3. C++11标准库中新增的线程和同步原语:
   - `std::thread`:表示一个线程对象,用于创建和管理线程。
   - `std::mutex`、`std::recursive_mutex`:互斥量,用于线程之间的互斥访问。
   - `std::lock_guard`、`std::unique_lock`:互斥量的 RAII 封装,用于自动管理互斥量的锁定和解锁。
   - `std::condition_variable`:条件变量,用于线程之间的同步和等待。
   - `std::future`、`std::promise`:异步结果的占位符和承诺,用于线程之间的数据传递和同步。
   - `std::async`:异步任务的启动器,用于启动异步任务并返回异步结果的`std::future`对象。
   - `std::atomic`:原子类型,用于无锁的原子操作。

除了上述内容,C++11标准库还引入了许多其他的改进和新特性,如:
- `std::tuple`:元组类型,用于表示固定大小的异构值集合。
- `std::chrono`:时间工具库,提供了时间点、时间段和时钟等时间相关的功能。
- `std::initializer_list`:初始化列表,用于支持使用统一的语法初始化容器和其他对象。
- `std::regex`:正则表达式库,提供了正则表达式的匹配、搜索和替换功能。
- 智能指针的改进:引入了`std::unique_ptr`、`std::shared_ptr`和`std::weak_ptr`,提供了更安全和灵活的动态内存管理。
- 随机数生成器的改进:引入了新的随机数引擎和分布,提供了更好的随机数生成功能。

总之,C++11标准库在原有标准库的基础上,引入了大量的新特性和改进,使得C++的标准库更加强大、灵活和易用。这些新增的容器、算法和工具可以帮助开发者编写更高效、更可靠的C++代码。

### 并发编程：什么是并发编程？为什么需要并发编程？如何使用 C++11 中的 std::thread、std::mutex、std::condition_variable 实现并发编程？
并发编程(Concurrent Programming)是一种编程范式,它允许多个任务或程序在同一时间内同时执行,以提高程序的性能和响应性。在并发编程中,多个线程或进程可以并行地执行不同的任务,充分利用计算机的多核处理能力。

需要并发编程的原因包括:
1. 提高性能:通过并发执行多个任务,可以充分利用计算机的多核处理器,加快程序的执行速度。
2. 提高响应性:并发编程可以使程序在执行长时间的任务时,仍然能够响应其他的请求或事件,提高程序的交互性和用户体验。
3. 简化编程:并发编程可以将复杂的任务分解为多个独立的子任务,每个子任务由不同的线程或进程来执行,使得程序的结构更加清晰和模块化。
4. 解决阻塞问题:并发编程可以避免某个任务的阻塞影响整个程序的执行,提高程序的并发性和可伸缩性。

C++11标准库提供了多个工具和类来支持并发编程,下面介绍如何使用`std::thread`、`std::mutex`、`std::condition_variable`实现并发编程:

1. `std::thread`:
   - `std::thread`类表示一个线程对象,可以用来创建和管理线程。
   - 通过将函数或函数对象传递给`std::thread`的构造函数,可以创建一个新的线程来执行该函数。
   - 可以使用`join()`函数等待线程执行完毕,或者使用`detach()`函数将线程与主线程分离,使其独立执行。
   - 示例:
     ```cpp
     #include <iostream>
     #include <thread>
     
     void threadFunction(int id) {
         std::cout << "Thread " << id << " is running." << std::endl;
     }
     
     int main() {
         std::thread t1(threadFunction, 1);
         std::thread t2(threadFunction, 2);
         
         t1.join();
         t2.join();
         
         return 0;
     }
     ```

2. `std::mutex`:
   - `std::mutex`类是一个互斥量,用于在多个线程之间保护共享资源,防止数据竞争和不一致性。
   - 通过调用`lock()`函数获取互斥量的所有权,调用`unlock()`函数释放互斥量的所有权。
   - 可以使用`std::lock_guard`或`std::unique_lock`来自动管理互斥量的锁定和解锁操作,避免手动管理的繁琐和错误。
   - 示例:
     ```cpp
     #include <iostream>
     #include <thread>
     #include <mutex>
     
     std::mutex mtx;
     int sharedData = 0;
     
     void incrementData() {
         std::lock_guard<std::mutex> lock(mtx);
         ++sharedData;
     }
     
     int main() {
         std::thread t1(incrementData);
         std::thread t2(incrementData);
         
         t1.join();
         t2.join();
         
         std::cout << "Shared data: " << sharedData << std::endl;
         
         return 0;
     }
     ```

3. `std::condition_variable`:
   - `std::condition_variable`类是一个条件变量,用于在多个线程之间进行同步和等待。
   - 通过调用`wait()`函数让线程进入等待状态,直到另一个线程调用`notify_one()`或`notify_all()`函数通知条件变量。
   - 条件变量通常与互斥量一起使用,以保护共享数据和避免竞争条件。
   - 示例:
     ```cpp
     #include <iostream>
     #include <thread>
     #include <mutex>
     #include <condition_variable>
     
     std::mutex mtx;
     std::condition_variable cv; 
     bool dataReady = false;
     
     void producerThread() {
         std::unique_lock<std::mutex> lock(mtx);
         dataReady = true;
         std::cout << "Producer: Data is ready." << std::endl;
         cv.notify_one();
     }
     
     void consumerThread() {
         std::unique_lock<std::mutex> lock(mtx);
         cv.wait(lock, []{ return dataReady; });
         std::cout << "Consumer: Data received." << std::endl;
     }
     
     int main() {
         std::thread producer(producerThread);
         std::thread consumer(consumerThread);
         
         producer.join();
         consumer.join();
         
         return 0;
     }
     ```

以上示例展示了如何使用`std::thread`创建线程,使用`std::mutex`保护共享资源,使用`std::condition_variable`进行线程间的同步和通信。

并发编程还涉及其他的概念和工具,如原子操作(`std::atomic`)、异步任务(`std::async`)、期物(`std::future`)等,可以根据具体的需求选择合适的并发编程技术。
