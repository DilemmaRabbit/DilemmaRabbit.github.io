---
title: "61. Rotate List"
date: 2022-03-11T13:31:35+08:00
draft: false
categories: ["leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Linked List, Two Pointer**

<!--more-->
---

## 問題描述

給予一條 linked list 以及 k，代表這個 linked list 向右位移 k 個單位(最末尾會變成新的 head)，回傳最後的 linked list

![](rotate1.jpg)

---

## 思路

個人思路(雙指標)

- 讓快慢指標先距離 k 個單位(需要用一個 cnt 預先計算好最小的距離，否則會 TLE)
- 快慢指標同時移動直到快指標移動到尾部
- 將快指標的尾部接上原本的頭，慢指標 next 當作新的頭，並將慢指標當作尾部(指向空指標)


---

解答思路(單指標)

- 一樣先紀錄 cnt 數量，並讓指標移動到 k 個單位的位置
- 將指標的尾部直接接上頭，形成一條循環 linked list
- 移動到目標位置 cnt - (k % cnt)，並設定新的頭以及尾

---

## 程式碼

```c++
// 個人方法
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        
        if(!head) return head;
        
        int cnt = 0;
        ListNode *fast, *slow;
        fast = head;
        slow = head;
        
        while(slow){
            slow = slow->next;
            cnt++; // 紀錄總共的節點數量
        }
        
        slow = head; // 慢指標回復到原本位置
        
        for(int i = 0; i < k % cnt; i++){
            if(fast -> next) fast = fast->next; // 快指標前進到目標位置
        }
        
        
        while(fast -> next != NULL){ // 快慢指標同時移動
            slow = slow->next;
            fast = fast->next;
        }
        
        fast -> next = head; // 將舊的尾接到頭
        head = slow -> next; // 設定新的頭
        slow -> next = NULL; // 設定新的尾
        
        return head;
    }
};
```

```c++
// 解答方法
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        
        if(!head) return head;
        
        int cnt = 1;
        ListNode *tail = head;
        
        while(tail -> next != NULL){
            tail = tail -> next;
            cnt++; // 紀錄總共的節點數量
        }
    
        tail -> next = head; // 將串列形成迴圈
        
        for(int i = 0; i < cnt - (k % cnt); i++) tail = tail -> next; // 指標移動到目標位置
        
        head = tail -> next; // 產生新的頭
        tail -> next = NULL; // 產生新的尾
        
        return head;
    }
};
```
