---
title: "21. Merge Two Sorted Lists"
date: 2022-03-07T21:13:08+08:00
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

給定兩條已排序的 linked list，合併為一條排序的 linked list

測資
  - 每條 node 數量 < 50
  - -100 <= Node.val <= 100

---

## 思路

產生一個新的list head，用兩個 pointer 紀錄目前 list1 與 list2 位置，並比較大小決定下一個節點，直到其中一條結束，再由另一條補上

---

## 程式碼

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        
        ListNode *list=NULL;
        ListNode *head=NULL;
        ListNode *tmp=NULL;
        
        if(list1==NULL){
            return list2;
        }
        
        if(list2==NULL){
            return list1;
        }
        
        while(list1!=NULL && list2!=NULL){

            if(list1 -> val >= list2 -> val){
                tmp = list2;
                list2 = list2 -> next;
                tmp->next = NULL;
            }
            else{
                tmp = list1;
                list1 = list1 -> next;
                tmp->next = NULL;
            }
            
            if(head==NULL){
                head = tmp;
                list = tmp;
            }
            else{
                list -> next = tmp;
                list = list -> next;
            }
            if(list1 == NULL || list2 == NULL){
                break;
            }
        }
        
        if(list1){
            list->next=list1;
        }
        
        if(list2){
            list->next=list2;
        }
        
        return head;
    }
};
```

---
