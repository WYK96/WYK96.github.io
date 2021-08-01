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
sort(v.begin(), v.end())
sort(v.begin(), v.begin()+5)

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
string str_reverse = std::reverse(str.begin(), str.end());

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
string s = new string("123456");
string sub_s = s.substr(0, 5); // returns s[0]-s[4]

# 计数
string s = "Hello C++";
int space_num = count(s.begin(), s.end(), ' ');

# 替换内容

string s = "Hello C++ and Python";
int space_num = count(s.begin(), s.end(), ' ');
s.replace(s.find(" "), 1, "%123"); // 空格开始的第1个字符替换成%123
// out: Hello%123C++%123and%123Python

```