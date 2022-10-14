---
title: "2095. Delete the Middle Node of a Linked List"
date: 2022-10-14T11:40:33+08:00
draft: false
categories: ["leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Linked List, Two Pointer, Sentinel Pointer**

## 問題描述

給予一條 linked list ，刪掉最中間的節點，如果是偶數節點的場合，則刪掉偏向右邊(較大)的節點

---

## 思路

- 雙指標
  - 先建立一個 sentinel 節點，作為慢指標 slow 之前的一個指標，以方便進行新的串接
  - 以 sentinel、slow 前進一步，fast 前進兩部做為遍歷
  - 根據 fast 後是否有節點，判斷為單數或是雙數節點的狀況
  - 如果是雙數節點，則 sentinel、slow 節點需要再前進一步

--- 

## 程式碼

```c++
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {
        
        // record previous node for slow.
        ListNode *sentinel = new ListNode(0,head);
        ListNode *fast, *slow;
        fast = slow = head;

        // only one node
        if(head->next == NULL) return NULL;

        // pointer treversal
        while(fast -> next != NULL && fast -> next -> next != NULL){
            fast = fast -> next -> next;
            slow = slow -> next;
            sentinel = sentinel -> next;
        }

        // even nodes
        if(fast -> next != NULL){ 
            sentinel = sentinel -> next;
            slow = slow -> next;
        }

        // connect new linked list
        sentinel -> next = slow -> next;

        // delete middle node
        delete(slow);

        return head;
    }
};
```
