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

* C和C++的区别？

* 继承有什么缺点？

* 深拷贝和浅拷贝的区别？

* 内存泄漏？

* 二级指针？函数指针？

* 链表的特点？适合什么场景？

* 快速排序原理？

* map与unordered_map的区别？


* reinterpret_cast和强制类型转换有什么区别？
    
    + 

# Unity

# Graphics


# Database

    * 数据库ACID？

    * 关系型数据库和非关系型数据库？

    *

# Network

    * TCP和UDP的区别