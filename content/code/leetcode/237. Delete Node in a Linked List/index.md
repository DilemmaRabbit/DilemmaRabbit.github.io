---
title: "237. Delete Node in a Linked List"
date: 2022-10-13T12:12:46+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Linked List**

<!--more-->
---

## 問題描述

- 給予一條Linked List 中要被刪除的 Node，返回刪除後的結果
- Node 不會位於 Linked List 最末端，因此不會有無法刪除的狀況

---

## 思路

- 由於不會獲得整條 Linked List，因此將所有的 val 往前挪一個位置，最後再刪除多餘的尾端節點


---

## 程式碼

```c++
class Solution {
public:
    void deleteNode(ListNode* node) {

        while(node -> next -> next != NULL){
            node -> val = node -> next -> val;
            node = node -> next;    
        }
        node -> val = node -> next -> val;
        delete(node->next); // 刪掉多出來的節點
        node -> next = NULL; // 新的尾端指向 NULL
        return;
    }
};
```
