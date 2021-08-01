---
title: 数据结构总结
tags: [基础知识学习]
style: fill
color: primary
description: 基本数据结构的总结.
---

# 链表

**单链表**

优点：添加或删除元素比数组快, $O(1)$
缺点：按照索引访问元素时比数组慢, $O(N)$

```C++
class MyLinkedList {
public:
    struct LinkedNode{
        int val;
        LinkedNode* next;
        LinkedNode(int val) : val(val), next(nullptr) {}
    };

    /** Initialize your data structure here. */
    MyLinkedList() {
        _dummyNode = new LinkedNode(0);
        _size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if(index < 0 || index > _size-1)
            return -1;
        LinkedNode *cur = _dummyNode->next;
        while(index--){
            cur = cur->next;
        }
        return cur->val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        LinkedNode *newNode = new LinkedNode(val);
        newNode->next = _dummyNode->next;
        _dummyNode->next = newNode;
        _size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        LinkedNode *cur = _dummyNode;
        LinkedNode *newNode = new LinkedNode(val);
        while(cur->next != nullptr)
            cur = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if(index > _size || index < 0)
            return;
        LinkedNode *cur = _dummyNode;
        while(index--)
            cur = cur->next;
        LinkedNode *newNode = new LinkedNode(val);
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if(index < 0 || index >= _size)
            return;
        LinkedNode *cur = _dummyNode;
        while(index--)
            cur = cur->next;
        LinkedNode *tmp = cur->next;
        cur->next = tmp->next;
        delete(tmp);
        _size--;
    }
private:
    int _size;
    LinkedNode *_dummyNode;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```