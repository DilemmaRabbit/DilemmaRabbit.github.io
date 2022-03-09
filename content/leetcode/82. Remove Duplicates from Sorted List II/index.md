---
title: "82. Remove Duplicates From Sorted List II"
date: 2022-03-09T11:05:07+08:00
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

給予一條已排列的 linked list，刪除所有不唯一的節點，返回一條排序後的 linked list

---

## 思路

個人思路
- 先將重複的 node 刪除，並將重複的值紀錄在 hash map 中
- 根據 hash map 元素刪除表中的元素(不包含頭)
- 根據 hash map 元素刪除頭的重複元素

解答思路
- 產生一個哨兵(sentinel)節點接上 head，就不用在考慮單獨處理 head 的情況
- 用一個 prev 節點當作新的定位點(不重複的元素/第一個重複的元素)
- 用 head 去遍歷重複的狀況，視情況用 prev 去指到新的不重複
- 回傳 sentinel 節點的 next

---

## 程式碼

```c++
# 個人思路
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL || head->next == NULL) return head;
        
        ListNode *slow,*fast;
        slow = head;
        fast = head -> next;
        map<int, bool> table;
        
        while(fast != NULL){
            if(slow -> val == fast -> val){
                table[slow->val] = true;
                slow -> next = fast -> next;
                fast = fast -> next;
            }
            else{
                slow = slow -> next;
                fast = fast -> next;
            }
        }
        
        slow = head;
        fast = head -> next;
        
        while(fast != NULL){
            if(table[fast -> val]){
                slow -> next = fast -> next;
                fast = fast -> next;
            }
            else{
                slow = slow -> next;
                fast = fast -> next;
            }
        }
        
        slow = head;
        while(slow != NULL){
            if(table[slow -> val]){
                head = slow -> next; 
                slow = slow -> next;
            }
            else break;
        }
        
        
        return head;
    }
};
```

```c++
# 解答思路
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        
        ListNode *sentinel = new ListNode(0, head);
        ListNode *prev = sentinel;
        
        while(head != NULL){
            if(head -> next != NULL && head -> val == head -> next -> val){
                while(head -> next != NULL && head -> val == head -> next -> val){
                    head = head -> next;
                }
                prev->next = head->next;
            }
            else{
                prev = prev -> next;
            }
            head = head -> next;
        }
        
        return sentinel -> next;
    }
};
```
