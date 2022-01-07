---
title: "Leetcode 382"
date: 2022-01-07T20:21:17+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類： 🟠 medium 🟠
### 相關主題： Linked List, Math, Reservoir Sampling, Randomized

### 題目（英文）
```
Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

Implement the Solution class:

Solution(ListNode head) Initializes the object with the integer array nums.
int getRandom() Chooses a node randomly from the list and returns its value. All the nodes of the list should be equally likely to be choosen.
```

---

### 測資

![](https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg)
```
Input
["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"]
[[[1, 2, 3]], [], [], [], [], []]
Output
[null, 1, 3, 2, 2, 3]
```

---

### 題目（中文）

```
給予一個 Linked List 結構，回傳 Linked List 隨機一個節點的質，每個節點有相同的機率被選擇到

請實作解答的結構

Solution(ListNode head) 初始化 Linked List
int getRandom() 選擇一個隨機的節點並回傳其值，每個節點被選擇的機率應該要一樣
```

---

## 思路

1. 原先想法是從 n 個中選擇一個，在從剩下的 n-1 中選擇 1 個，直到所有都被選擇完，則重新開始
2. 然而原先的方法不夠隨機，只能確保在選擇 n 個時是完全公平的，並不代表相同的被選擇的機率
3. [Reservoir Sampling 水塘抽樣](https://zh.wikipedia.org/wiki/%E6%B0%B4%E5%A1%98%E6%8A%BD%E6%A8%A3)
- 從目標中取 1 個當作目標回傳值
- 對目標外值，每個皆擁有 1 / n 的機會被當成目標回傳值
- 對每一個值都做相同的操作
- 可確保每一個值被當成回傳值得機率為 1 / n

---

## 程式碼

```
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
    
    vector<int> t;
    int cur;
    
    Solution(ListNode* head) {
        ListNode *tmp = head;
        srand(time(NULL));
        while(tmp != NULL){
            t.push_back(tmp->val);
            tmp = tmp -> next;
        }
        
        cur = t.size();
        
    }
    
    int getRandom() {
        
        int val = t.front();
        
        for(int i=1;i<cur;i++){
            int j = rand() % (i+1);
            if(j == 0){
                val = t[i];
            }
        }
            
        return val;
            
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

---

## 結果

![](https://i.imgur.com/qBmNXHO.png)
---

## 結語

原先的想法再取 n 的倍數的情況下才能確保是完全公平的，但是一輪的公平不代表在選擇某個數字時機率是公平的，水塘取樣則可以保證每個值在一次取樣中都有公平的機會被取到。

目前還有的改善地方為可以把原先用陣列的結構改為使用串列的方式，可以節省更多空間。