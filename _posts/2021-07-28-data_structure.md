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


**双链表**

```C++
struct DoublyListNode{
    int val;
    DoublyListNode *prev, *next;
    DoublyListNode(int x) : val(x), prev(nullptr), next(nullptr) {}
};

class MyLinkedList {
public:
    /** Initialize your data structure here. */
    MyLinkedList() {
        _dummyHead = new DoublyListNode(-1);
        _size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if(index < 0 || index > _size-1) return -1;
        DoublyListNode *curNode = _dummyHead->next;
        while(index--){
            curNode = curNode->next;
        }
        return curNode->val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        DoublyListNode *newNode = new DoublyListNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        newNode->prev = _dummyHead;
        if(newNode->next)
            newNode->next->prev = newNode;
        _size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        DoublyListNode *tailNode = new DoublyListNode(val);
        DoublyListNode *curNode = _dummyHead;
        while(curNode->next){
            curNode = curNode->next;
        }
        curNode->next = tailNode;
        tailNode->prev = curNode;
        _size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if(index < 0 || index > _size) return;
        DoublyListNode *curNode = _dummyHead;
        DoublyListNode *tmp = new DoublyListNode(val);
        while(index--){
            curNode = curNode->next;
        }
        tmp->next = curNode->next;
        curNode->next = tmp;
        tmp->prev = curNode;
        if(tmp->next)
            tmp->next->prev = tmp;
        _size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if(index < 0 || index > _size-1) return;
        DoublyListNode *curNode = _dummyHead;
        while(index--){
            curNode = curNode->next;
        }
        DoublyListNode *tmp = curNode->next;
        if(curNode->next->next){
            curNode->next = curNode->next->next;
            curNode->next->prev = curNode;
        }
        else{
            curNode->next = nullptr;
        }
        delete tmp;
        _size--;
    }
private:
    DoublyListNode *_dummyHead;
    int _size;
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

# 队列

**循环队列**

特点:FIFO

```C++
class MyCircularQueue {
private:
    int *data;
    int front, rear;
    int length;
public:
    MyCircularQueue(int k) {
        data = new int[k];
        front = rear = -1;
        length = k;
    }
    
    bool enQueue(int value) {
        if(isFull()) return false;
        if(isEmpty()) front++;
        rear = (rear + 1) % length;
        data[rear] = value;
        return true;
    }
    
    bool deQueue() {
        if(isEmpty()) return false;
        if(front == rear && front != -1){
            front = rear = -1;
        }
        else{
            front = (front + 1) % length;
        }
        return true;
    }
    
    int Front() {
        if(isEmpty()) return -1;
        return data[front];
    }
    
    int Rear() {
        if(isEmpty()) return -1;
        return data[rear];
    }
    
    bool isEmpty() {
        if(front == rear && front == -1) return true;
        return false;
    }
    
    bool isFull() {
        if(((rear + 1) % length) == front) return true;
        return false;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```

# 栈

特点: LIFO

```C++
#include <iostream>

class MyStack {
    private:
        vector<int> data;               // store elements
    public:
        /** Insert an element into the stack. */
        void push(int x) {
            data.push_back(x);
        }
        /** Checks whether the queue is empty or not. */
        bool isEmpty() {
            return data.empty();
        }
        /** Get the top item from the queue. */
        int top() {
            return data.back();
        }
        /** Delete an element from the queue. Return true if the operation is successful. */
        bool pop() {
            if (isEmpty()) {
                return false;
            }
            data.pop_back();
            return true;
        }
};

int main() {
    MyStack s;
    s.push(1);
    s.push(2);
    s.push(3);
    for (int i = 0; i < 4; ++i) {
        if (!s.isEmpty()) {
            cout << s.top() << endl;
        }
        cout << (s.pop() ? "true" : "false") << endl;
    }
}
```

# 树

**二叉树**

```C++
# Definition of a binary tree node
struct TreeNode{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *treeLeft, TreeNode *treeRight) : val(x), left(treeLeft), right(treeRight) {}
};

# preorder
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> traversed;
        preorder(root, traversed);
        return traversed;
    }

    void preorder(TreeNode* root, vector<int> &traversed){
        if(root == nullptr)
            return;
        traversed.push_back(root->val);
        preorder(root->left, traversed);
        preorder(root->right, traversed);
    }
};

# inorder
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> traversed;
        inorder(root, traversed);
        return traversed;
    }

    void inorder(TreeNode *node, vector<int> &traversed){
        if(node == nullptr) return;
        inorder(node->left, traversed);
        traversed.push_back(node->val);
        inorder(node->right, traversed);
    }
};

# postorder
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> traversed;
        postorder(root, traversed);
        return traversed;
    }
    
    void postorder(TreeNode *cur, vector<int> &traversed){
        if(cur == nullptr)
            return;
        postorder(cur->left, traversed);
        postorder(cur->right, traversed);
        traversed.push_back(cur->val);
    }

};

# max depth
class Solution {
public:
    int max_depth = 0;
    void getDepth(TreeNode *root, int depth){
        if(!root)
            return;
        if(!root->left && !root->right)
            max_depth = max(max_depth, depth);        
        getDepth(root->left, depth+1);
        getDepth(root->right, depth+1);
    }
    int maxDepth(TreeNode* root) {
        if(!root)
            return 0;
        getDepth(root, 1);
        return max_depth;   
    }
};

class Solution {
public:
    void maxDepth(TreeNode* root) {

    }
    int bottom_top_solution(TreeNode *root){
        if(!root)
            return 0;
        int left_depth = bottom_top_solution(root->left);
        int right_depth = bottom_top_solution(root->right);
        return max(left_depth, right_depth) + 1;
    }
}
```
