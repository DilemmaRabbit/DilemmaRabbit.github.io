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

é£åº¦ï¼ð  Normal ð 

---

## åé¡æè¿°

çµ¦äºä¸æ¢å·²æåç linked listï¼åªé¤ææä¸å¯ä¸çç¯é»ï¼è¿åä¸æ¢æåºå¾ç linked list

---

## æè·¯

åäººæè·¯
- åå°éè¤ç node åªé¤ï¼ä¸¦å°éè¤çå¼ç´éå¨ hash map ä¸­
- æ ¹æ hash map åç´ åªé¤è¡¨ä¸­çåç´ (ä¸åå«é ­)
- æ ¹æ hash map åç´ åªé¤é ­çéè¤åç´ 

è§£ç­æè·¯
- ç¢çä¸åå¨åµ(sentinel)ç¯é»æ¥ä¸ headï¼å°±ä¸ç¨å¨èæ®å®ç¨èç head çææ³
- ç¨ä¸å prev ç¯é»ç¶ä½æ°çå®ä½é»(ä¸éè¤çåç´ /ç¬¬ä¸åéè¤çåç´ )
- ç¨ head å»éæ­·éè¤ççæ³ï¼è¦ææ³ç¨ prev å»æå°æ°çä¸éè¤
- åå³ sentinel ç¯é»ç next

---

## ç¨å¼ç¢¼

```c++
# åäººæè·¯
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
# è§£ç­æè·¯
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
