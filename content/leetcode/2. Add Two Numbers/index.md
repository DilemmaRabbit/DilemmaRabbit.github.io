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

é£åº¦ï¼ð  Normal ð 

---

## åé¡æè¿°

çµ¦äºå©æ¢ linked listï¼ä»£è¡¨å¾åä½æ¸éå§çå©çµæ¸å­ï¼åå³ä¸æ¢ä»£è¡¨éå©çµæ¸å­å ç¸½ç linked list

---

## æè·¯

- å©ç¨èªåµçä¸å Node ç¶ä½å¨åµç¯é»
- è¨ç®æ¯åååç¶ä¸çå ç¸½ä»¥åé²ä½
- å©æ¢ linked list åæéæ­·ï¼ç¶ next ç­æ¼ç©ºææ¨æä»£è¡¨æä¸æ¢å°åºï¼ä¸æ¬¡éå§æå ç¸½çå¼è¨­çº 0 
- ç¶å©åææ¨é½æ¯ç©ºææ¨å¼ä»£è¡¨å ç¸½å®ç¢ä¸¦éåº
- æå¾èæ®æå¾ä¸æ¬¡çé²ä½

---

## ç¨å¼ç¢¼

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
        int carry = 0;
        int sum = 0;
        int val1,val2;
        ListNode *tmp;
        ListNode *head = new ListNode(10); // å¨åµç¯é»
        tmp = head;
        
        // ç¶å©æ¢ linked list é½çµææéåº
        while(l1 || l2){
            
            if(l1 == NULL) val1 = 0; // ç¢ºå® linked list æ¯å¦çµæ
            else val1 = l1 -> val;
            
            if(l2 == NULL) val2 = 0; // ç¢ºå® linked list æ¯å¦çµæ
            else val2 = l2 -> val;
            
            sum = val1 + val2 + carry; // è¨ç®ç¶åå ç¸½
            carry = sum / 10; // è¨ç®é²ä½
            tmp -> next = new ListNode(sum % 10); // ç¢çæ°ç¯é»
            tmp = tmp -> next;
            
            if(l1) l1 = l1 -> next; // é¿åé²å¥ç©ºææ¨
            if(l2) l2 = l2 -> next; // é¿åé²å¥ç©ºææ¨
        }
        
        if(carry == 1) tmp -> next = new ListNode(1); // æå¾çé²ä½
        
        return head->next;
    }
};
```
