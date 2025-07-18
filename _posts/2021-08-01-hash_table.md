---
title: 哈希表
tags: [基础知识学习]
style: fill
color: success
description: 哈希表的原理及C++实现.
---

# 哈希表

**概念**：哈希表是一种**数据结构**，其使用**哈希函数**组织数据，以支持**快速插入和搜索**。

**原理**：哈希表通过哈希函数将键映射到存储桶：

   * 对于插入：哈希函数将决定将该键分配到那个存储桶中，并进行存储；
   * 对于搜索：哈希表使用相同的哈希函数查找对应的桶，并在这个桶中进行搜索。

**核心问题**
   * 设计哈希函数：能够将集合中任意可能的元素映射到一个固定范围的整数值，并将该元素存储到整数值对应的地址上；
   * 解决冲突：不同元素可能映射到相同的整数值，需要在发生冲突时进行处理，方法有：链地址法、开放地址法、再哈希法；
   * 扩容：当哈希表中的元素过多时，法生冲突的可能性会增大，从而降低查找效率。需要额外开辟存储空间，以缓解冲突。

## 链地址法实现

首先，设计哈希函数：$$ hash(x) = x \  mod \  base $$

其中，base为哈希表的大小。开辟一个大小为base的数组，其每个位置是一个链表。对于输入$$ x $$，计算出$$ hash(x) $$，然后插入到对应位置的链表中。

```C++
base = 123
hash key       bucket
0         ->   123
1         ->   124 -> 2
2         ->   125 -> 248
...
122       ->   ... -> ...
```

```C++
// implementation of hash table using array + linked list
class MyHashSet{
private:
    static const int base = 123;
    vector<list<int>> data;
    int hash(int x){
        return x % base;
    }
public:

    MyHashSet() : data(base) {}

    void add(int key){
        int bucket = hash(key);
        if(contains(key))
            return;
        data[bucket].push_back(key);
    }

    bool contains(int key){
        int bucket = hash(key);
        for(auto it = data[bucket].begin(); it != data[bucket].end(); ++it){
            if((*it) == key)
                return true;
        }
        return false;
    }

    void remove(int key){
        int bucket = hash(key);
        for(auto it = data[bucket].begin(); it != data[bucket].end(); ++it){
            if((*it) == key)
                data[bucket].erase(it);
                return;
        }
    }
}
```

```C++
// when the contents are paired <key, value>
class MyHashMap {
private:
    static const int base = 123;
    vector<list<pair<int, int>>> data;
    int hash(int x){
        return x % base;
    }
public:
    /** Initialize your data structure here. */
    MyHashMap() : data(base) {}
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int bucket = hash(key);
        for(auto it = data[bucket].begin(); it != data[bucket].end(); ++it){
            if((*it).first == key){
                (*it).second = value;
                return;
            }
        }
        data[bucket].push_back(make_pair(key, value));
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int bucket = hash(key);
        for(auto it = data[bucket].begin(); it != data[bucket].end(); ++it){
            if((*it).first == key){
                return (*it).second;
            }
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int bucket = hash(key);
        for(auto it = data[bucket].begin(); it != data[bucket].end(); ++it){
            if((*it).first == key){
                data[bucket].erase(it);
                return;
            }
        }
    }
};
```


## 复杂度分析

若总共有$$ M $$个键，则空间复杂度为$$ O(M) $$。

通常情况下，插入和搜索的时间复杂度为$$ O(1) $$。

最坏情况下，若桶大小的最大值为$$ N $$，则插入时间复杂度为$$ O(1) $$,搜索时间复杂度为$$ O(N) $$。


## 内置哈希表原理

* 键值是任何可哈希化的类型，其具有哈希码，哈希码用于映射函数以获取存储区索引。

* 每个桶包含一个数组，用于在初始化时将所有值存储在同一个桶中。

* 插入和搜索的平均时间复杂度为$$ O(1) $$，最坏情况下的插入和搜索的时间复杂度是$$ O(logN) $$，使用高度平衡的二叉搜索树。


## STL中的unordered_set

unordered_set是STL中hash set的实现

```C++
int main(){
    unordered_set<int> hashSet;
    hashSet.insert(3);
    hashSet.insert(4);
    hashSet.insert(5);

    hashSet.delete(3);

    if(hashSet.count(7) <= 0>){
        cout << "Key 7 is not in the hash set". << endl;
    }

    cout << "The size of hash set is : " << hashSet.size() << endl;

    for(auto it = hashSet.begin(); it != hashSet.end(); ++it){
        cout << (*it) << " ";
    }

    hashSet.clear(); // clear the hash set

    if(hashSet.empty()){
        cout << "Hash set is empty." << endl;
    }
}
```


## 应用


### 查重

```C++
bool findDuplicates(vector<int>& keys){
    unordered_set<int> hashSet;
    for(const int& key : keys){
        if(hashSet.count(key) > 0){
            return true;
        }
        hashSet.insert(key);
    }
    return false;
}
```
