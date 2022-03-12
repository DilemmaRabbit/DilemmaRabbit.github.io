---
title: "138. Copy List With Random Pointer"
date: 2022-03-12T14:44:13+08:00
draft: false
categories: ["Learning"]
Tags: ["LeetCode"]

resources:
  - name: "featured-image"
    src: "leetcode.png"
---

難度：🟠 Normal 🟠

---

## 問題描述

給予一條 linked list，除了 next 指標之外還有一條連接到其他節點的 random 指標

複製出一條 linked list 擁有相同的結構

---

## 思路

個人思路
- 由於對一條串列來說，random 是指向串列中其中一個節點，用 hash table 模仿這個行為
- 當產生一個新節點時，對照原本原串列的位址產生一個 hash table 的對照，並先將 next 部份完成
- 最後遍歷每個原串列節點的 random，將對應過去的新串列的位置給新的 random 值

---

## 程式碼

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        map<Node*,Node*> table; // 建立 hash map
        Node* newList = new Node(0); // 哨兵節點
        Node* cur = newList; // 紀錄新串列的節點位置
        Node* tmp = head; // 紀錄舊串列的節點位置
        
        while(tmp){
            cur -> next = new Node(tmp->val); // 根據舊串列的值產生新節點     
            cur = cur -> next;
            table[tmp] = cur;  // 紀錄當前新舊串列的對照狀況
            tmp = tmp -> next;
        }
        
        tmp = head; // 回到原點
        cur = newList->next; // 回到原點
        
        while(tmp){
            cur -> random = table[tmp -> random]; // 將新串列的 random 以舊串列的對照給予
            tmp = tmp -> next;
            cur = cur -> next;
        }
        
        return newList->next;
    }
};
```
