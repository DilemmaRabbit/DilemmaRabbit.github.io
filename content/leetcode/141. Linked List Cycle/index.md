---
title: "141. Linked List Cycle"
date: 2022-03-08T10:23:50+08:00
draft: false
categories: ["Learning"]
Tags: ["LeetCode"]

resources:
  - name: "featured-image"
    src: "leetcode.png"
---

難度：🟢 Easy 🟢

---

## 問題描述

給予一條可能產生迴圈的 linked list，返回 true or false 表示有無迴圈。

測資：-105 <= Node.val <= 105

![](example.png)

---

## 思路

遍歷節點，並將節點改為範圍外的數字，假設碰到範圍外的數字則代表進入迴圈。

---

## 程式碼

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        
        ListNode *tmp = head;
        
        while(tmp != NULL){
            if(tmp->val != 100001) tmp->val = 100001;
            else return true;
            tmp = tmp->next;
        }
        
        return false;
    }
};
```
