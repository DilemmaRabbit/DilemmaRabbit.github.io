---
title: "19. Remove Nth Node From End of List"
date: 2022-09-28T12:06:26+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]

---

**難度：🟠 Normal 🟠**

**關鍵字：Linked List, Two Pointer**

<!--more-->
---

## 問題描述

給予一條串列與整數 n，回傳減去倒數第 n 個節點的串列

---

## 思路

- two pointer
  - 利用兩個快慢指標，之間的距離代表了倒數 n 個的狀況，快指標到達節點末端時，慢指標到了目標節點
  - 重新利用一次快指標，讓快指標到達目標節點的前一個節點
  - 重新串接串列

- 如何不讓快指標重新遍歷一次？
  - 預先設定一個只向 head 的哨兵節點，與快慢指標同時移動
  - 會多花時間與空間產生新節點 

---

## 程式碼

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* guard = new ListNode(0, head);
        ListNode* target = head;
        ListNode* tmp = head;
        while(--n) tmp = tmp -> next;
        while(tmp -> next != NULL){
            tmp = tmp -> next;
            target = target -> next;
            guard = guard -> next;
        }
        if(target == head) return head->next;
        guard -> next = target -> next;
        
        return head;
    }
};
```
