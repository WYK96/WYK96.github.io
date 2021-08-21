---
title: "面经汇总"
tags: [基础知识学习]
style: fill
color: success
description: 面经知识整理
---



目录



[toc]



# C++

* C++四种cast强制类型转换？
  
  + static_cast、const_cast、dynamic_cast、reinterpret_cast
    
    * const_cast: 用于将const变量转换为非const变量；   

    * static_cast: 用于各种隐式转换，比如非const转const、void*转指针、子类对象转父类等；

    * dynamic_cast: 和static_cast效果类似，但增加了类型检查的功能，比static_cast更安全。dynamic_cast常用于带有虚函数的基类与其派生类之间的转换，特点是运行时会检测转换类型是否安全，如果失败返回nullptr，但有额外的开销。
    
    * reinterpret_cast：用于隐式转换无法完成的高风险转换，如将int转为指针。缺点是不进行错误检查，只是强行复制，有安全隐患。

    * 什么是右值应用？右值引用用在哪里？

    * 讲一下智能指针，weak和shared使用时有什么区别，什么时候用shared，什么时候用weak？

    * lambda的底层实现？

    * 虚函数？虚函数表？

    * map是怎么实现的？ tmap？ unordered_map？ hashmap比红黑树好在哪？

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