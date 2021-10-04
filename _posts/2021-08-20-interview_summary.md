---
title: "面经汇总"
tags: [基础知识学习]
style: fill
color: success
description: 面经知识整理
---



# C++

* C++四种cast强制类型转换？
  
  + static_cast、const_cast、dynamic_cast、reinterpret_cast
    
    * const_cast: 用于将const变量转换为非const变量；   

    * static_cast: 用于各种隐式转换，比如非const转const、void*转指针、子类对象转父类等；

    * dynamic_cast: 和static_cast效果类似，但增加了类型检查的功能，比static_cast更安全。dynamic_cast常用于带有虚函数的基类与其派生类之间的转换，特点是运行时会检测转换类型是否安全，如果失败返回nullptr，但有额外的开销。
    
    * reinterpret_cast：用于隐式转换无法完成的高风险转换，如将int转为指针。缺点是不进行错误检查，只是强行**逐比特**复制，有安全隐患。

    * 什么是右值应用？右值引用用在哪里？

* 讲一下智能指针，weak和shared使用时有什么区别，什么时候用shared，什么时候用weak？

    + 智能指针是C++11的新特性，他是为了解决动态内存分配中带来的内存泄漏和多次释放同一块内存的问题而提出的。智能指针分为三类：unique_ptr、shared_ptr、weak_ptr。unique_ptr独占资源所有权，不能拷贝构造或赋值，但可以进行移动构造和赋值移动构造(move)将所有权转移给另一个unique_ptr。
    
    + shared_ptr：多个指针可以share同一个资源，通过引入计数机制判断当前资源被多少指针访问，只有use_count = 0时才会释放资源的所有权，从而避免了内存泄漏。
    + weak_ptr：只有资源的观测权，没有所有权，weak_ptr是为了解决shared_ptr中**循环引用**的问题提出的。

* lambda的底层实现？

    + **Step1：** 创建lambda类，实现构造函数，使用lambda表达式的函数体重载operator()(这也是为什么lambda表达式也叫匿名函数对象)
      **Step2：** 实例化lambda对象
      **Step3：** 通过实例化对象调用operator()

* 虚函数？虚函数表？

    + 函数返回值前带有virtual关键字的函数就是虚函数。虚函数配合类的多态特性使用，它允许继承基类的派生类对象对同一方法做出不同响应。

    + 虚函数的地址存放于虚函数表中。一个类的实例对象中存有指向虚函数表的虚表指针。当基类指针指向派生类时，虚函数表指明了实际应该调用的函数。

* map是怎么实现的？ unordered_map？ hashmap比红黑树好在哪？

    + map和set的底层是通过红黑树实现的，unordered_map和unordered_set底层是通过哈希实现的。map的特点在于有序，但是对于插入操作需要调整红黑树的节点，使得调整后的树仍然满足红黑树的特性，从而带来额外的开销。hashmap本身存放的数据是无序的，对于新增的数据按照一定的策略存储至不同的桶。

    + 查找效率方面，map可以达到 $$ logN $$, unordered_map可以达到常数级别的查找。

* C和C++的区别？

* 继承有什么缺点？

* 深拷贝和浅拷贝的区别？

* 内存泄漏？

* 二级指针？函数指针？

* 链表的特点？适合什么场景？

* 快速排序原理？

* map与unordered_map的区别？


* reinterpret_cast和强制类型转换有什么区别？
    
* this指针存放在哪里？32位系统和64位系统的this指针大小是多少？

    + this指针存放在寄存器中。32位系统this指针大小为4字节，64位为8字节。

* C++11新特性？
    + auto类型推导
    + 范围for
    + delete和default关键字
    + lambda表达式
    + 智能指针
    + decltype
    + 右值引用
    + nullptr

* 内存对齐的原理及意义？

    + 作用：减少CPU读的次数
    + 原则：每个变量看后面的变量，如果后面的变量大，就和后面的大小对齐并补充字节。最后一个变量按照结构体成员内最大的对齐值进行对齐并补充字节。

* shared_ptr的实现原理？引用计数在哪里定义？

    + 原理：shared_ptr本质上是一个模板类，表现上与指针相同，其重载了&、*、=运算符。shared_ptr在实现上有两个核心成员，一个是指向资源对象的指针变量，另一个是指向引用计数的指针变量。

    + 引用计数为什么是指针：多个shared_ptr对象指向同一资源时，其中一个对象析构了count = count - 1时不会影响到其它shared_ptr对象。

* new, delete, malloc, free在内存中的位置？

    + malloc、free：申请的内存位于堆中；

    + new、delete：申请的内存位于自由存储区中，new如果是使用malloc实现的，那么申请的内存也位于堆中。

* new expression、operator new和malloc的联系？

    + malloc：从C语言移植过来的语义，申请一定大小的内存并返回void*指针，在堆上申请内存，申请失败会返回null；

    + new：C++中的关键字，针对对象而言，申请一块内存然后在内存上构造相应的对象，最后返回该对象的指针，申请失败会抛出异常。

    + operator new：只申请内存而不构造对象。new操作的第一步就是调用operator new来执行内存的申请。


* RAII是什么？有什么意义？应用场景？
    
    + RAII: Resource Acquisition is Initialization，即“资源获取即初始化”。核心是把资源和对象的生命周期绑定，对象创建时获取资源，对象销毁时释放资源。

    + 场景：智能指针。

* 内存泄漏？如何检测？如何避免？

    + 由于疏忽或错误导致程序未能释放已经不再使用的内存的情况。内存在物理上并未消失，只是程序失去了对该段内存的控制。

    + 检测：1.包含调试堆函数库文件：#include <crtbg.h>，通过包含crtbg.h可以将malloc和free函数映射到其调试版本_malloc_dbg和_free_dbg,，这些函数会跟踪内存分配和释放；2.在需要检测内存泄漏的地方添加_CrtDumpMemoryLeaks()以输出内存泄漏信息。

    + 避免：及时释放、智能指针。

* delete this可以吗？会有什么问题？

    + 可以操作，但是调用后不能再访问涉及这个对象的数据成员和虚函数，否则会导致程序崩溃或内存不确定。如：delete this会调用本对象的析构函数，析构函数又会调用delete this，从而造成无限递归。

* vector的实现？

    + vector是一个模板类，里面存放了三个指针，分别指向数组头、数据尾、数组尾。为了减少频繁的空间申请及释放所带来的程序效率影响，vector在添加元素时会多申请一部分内存作为buffer，即capacity()。相比之下，size()表示实际使用的空间大小。
      如果想主动申请内存，可以使用reserve(n)，超出实际使用的部分作为buffer；如果使用resize(n)，则会严格限制vector的size为n。


2. 一片空间memset清零内容会有什么问题？
3.  
4. 54张扑克牌，怎么洗牌？
5. 几千万条数据，怎么进行管理？
6. Laplace operator在manifold中的表示？mesh Laplacian的weight怎么设定的？
7. 了解过lighting吗？phong是怎么做的？
8. 轴角?四元数？
9. 变换矩阵？[0 -1 ;1 0]是什么变换？
10. blender是左手系还是右手系？
11. A C D继承至B，ABCD都有虚函数，现在实例化A和C，问现在有几张虚表？

# Unity

# Graphics



# Database

    * 数据库ACID？

    * 关系型数据库和非关系型数据库？

    *

# Network

    * TCP和UDP的区别

