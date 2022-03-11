---
title: "2. Add Two Numbers"
date: 2022-03-11T13:23:24+08:00
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

給予兩條 linked list，代表從個位數開始的兩組數字，回傳一條代表這兩組數字加總的 linked list

---

## 思路

- 利用自創的一個 Node 當作哨兵節點
- 計算每個回合當下的加總以及進位
- 兩條 linked list 同時遍歷，當 next 等於空指標時代表某一條到底，下次開始把加總的值設為 0 
- 當兩個指標都是空指標值代表加總完畢並退出
- 最後考慮最後一次的進位

---

## 程式碼

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
        int carry = 0;
        int sum = 0;
        int val1,val2;
        ListNode *tmp;
        ListNode *head = new ListNode(10); // 哨兵節點
        tmp = head;
        
        // 當兩條 linked list 都結束時退出
        while(l1 || l2){
            
            if(l1 == NULL) val1 = 0; // 確定 linked list 是否結束
            else val1 = l1 -> val;
            
            if(l2 == NULL) val2 = 0; // 確定 linked list 是否結束
            else val2 = l2 -> val;
            
            sum = val1 + val2 + carry; // 計算當前加總
            carry = sum / 10; // 計算進位
            tmp -> next = new ListNode(sum % 10); // 產生新節點
            tmp = tmp -> next;
            
            if(l1) l1 = l1 -> next; // 避免進入空指標
            if(l2) l2 = l2 -> next; // 避免進入空指標
        }
        
        if(carry == 1) tmp -> next = new ListNode(1); // 最後的進位
        
        return head->next;
    }
};
```
