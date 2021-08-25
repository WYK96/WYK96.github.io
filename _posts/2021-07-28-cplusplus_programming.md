---
title: C++复习笔记
tags: [编程语言, 基础知识学习]
style: fill
color: danger
description: C++要点知识整理
---

# vector

```C++
vector<string> nums;

string tmp = "123";
nums.push_back(tmp); // 这里不能直接用字面值

nums.emplace_back("123"); // 可以直接用字面值，隐式构造，和上面的效果一样

// set 与 vector 转换
unordered_set<int> hashSet{1,2,4,5,6};
vector<int> vec;
vec.assign(hashSet.begin(), hashSet.end());

```


# pair用法
```C++
// pair将两个数据组成一对
pair<int, float> p(2, 4.5);
auto p1 = p.first;
auto p2 = p.second;

// make_pair的用法
int key = 2;
float value = 4.6;
vector<pair<int, float>> data;
data.emplace_back(make_pair(key, value));
```


# algorithm库

```C++
#include <algorithm>

//count
count(InputIterator first, InputIterator last, const T& val);

//sort
vector<int> v;
sort(v.begin(), v.end());
sort(v.begin(), v.begin()+5);
sort(first_pointer, first_pointer+n, cmp);

bool compare(int a, int b){
    return a > b;
}
sort(v.begin(), v.end(), compare);

```


# std

```C++

// 字符串反转
int x = 123;
string str = std::to_string(x);
std::reverse(str.begin(), str.end());

// 交换值
float a = 1.23, b = 4.25;
std::swap(a, b);

//绝对值
# include <cstdlib>
int abs(int n);
long int abs(long int n);

# include <cmath>
double fabs(double x);
float fabs(float x);
long double fabs(long double x);

// max, min
int a = 3, b = 5;
int c = max(a, b);
c = min(a, b);


```


# string

```C++
#include <string>

# 子串
string substr(size_t pos = 0, size_t len = npos) const;

string s = new string("123456");
string sub_s = s.substr(1, 5); // returns s[1]-s[5]

# 计数
string s = "Hello C++";
int space_num = count(s.begin(), s.end(), ' ');

# 替换内容
string s = "Hello C++ and Python";
int space_num = count(s.begin(), s.end(), ' ');
s.replace(s.find(" "), 1, "%123"); // 空格开始的第1个字符替换成%123
// out: Hello%123C++%123and%123Python

# 比较，相同输出0，不同输出1
string s1 = "Hello world";
string s2 = s1 + "123";
cout << s1.compare(s2) << endl;

# 调整大小
string s = "Computer Graphics";
s.resize(10);
s.resize(20, '!');

```


# numeric

```C++
# 累加
int sum = accumulate(v.begin(), v.end(), 0);
```

# 右值引用

左值：等号左边的值，可以取地址；
右值：等号右边的值，不能取地址；

左值引用：能指向左值但不能指向右值的引用，以&进行标识。

（例外：const左值引用可以指向右值）

右值引用：可以指向右值但不能指向左值的引用，以&&进行标识

```C++
int &&ref_a_right = 5;  // ok

int a = 4;
int &&ref_a_left = a; // 错误，右值引用不能指向左值

ref_a_right = 6; // 可以修改右值
```

右值可以通过std::move指向左值

```C++
int a = 5;
int &ref_a_left = a;
int &&ref_a_right = std::move(a);
```

被声明出来的左右值引用都是左值，因为它们都有地址，且位于等号左边。

右值引用既可以是左值，也能是右值，如果有名称则为左值，否则是右值。


# set

set/map的底层实现是**平衡二叉搜索树**(通过红黑树)，unordered_set/unordered_map的底层实现是哈希。哈希集和树集的本质区别在于：树集中的键是有序的。

# 红黑树

红黑树的规则：
 * 根节点是黑色；
 * 节点是红色或黑色；
 * 每个叶子节点都是黑色节点（NULL节点）
 * 每个红色节点的子节点都是黑色；
 * 从任一节点到其每个子节点的所有路径都包含相同数目的黑色节点。

调整方式：
  * 变色
  * 左旋
  * 右旋

应用：存储有序的数据

时间复杂度： $$ O(longN) $$