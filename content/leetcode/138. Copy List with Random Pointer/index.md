---
title: "138. Copy List With Random Pointer"
date: 2022-03-12T14:44:13+08:00
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

çµ¦äºä¸æ¢ linked listï¼é¤äº next ææ¨ä¹å¤éæä¸æ¢é£æ¥å°å¶ä»ç¯é»ç random ææ¨

è¤è£½åºä¸æ¢ linked list ææç¸åççµæ§

---

## æè·¯

åäººæè·¯
- ç±æ¼å°ä¸æ¢ä¸²åä¾èªªï¼random æ¯æåä¸²åä¸­å¶ä¸­ä¸åç¯é»ï¼ç¨ hash table æ¨¡ä»¿éåè¡çº
- ç¶ç¢çä¸åæ°ç¯é»æï¼å°ç§åæ¬åä¸²åçä½åç¢çä¸å hash table çå°ç§ï¼ä¸¦åå° next é¨ä»½å®æ
- æå¾éæ­·æ¯ååä¸²åç¯é»ç randomï¼å°å°æéå»çæ°ä¸²åçä½ç½®çµ¦æ°ç random å¼

---

## ç¨å¼ç¢¼

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        map<Node*,Node*> table; // å»ºç« hash map
        Node* newList = new Node(0); // å¨åµç¯é»
        Node* cur = newList; // ç´éæ°ä¸²åçç¯é»ä½ç½®
        Node* tmp = head; // ç´éèä¸²åçç¯é»ä½ç½®
        
        while(tmp){
            cur -> next = new Node(tmp->val); // æ ¹æèä¸²åçå¼ç¢çæ°ç¯é»     
            cur = cur -> next;
            table[tmp] = cur;  // ç´éç¶åæ°èä¸²åçå°ç§çæ³
            tmp = tmp -> next;
        }
        
        tmp = head; // åå°åé»
        cur = newList->next; // åå°åé»
        
        while(tmp){
            cur -> random = table[tmp -> random]; // å°æ°ä¸²åç random ä»¥èä¸²åçå°ç§çµ¦äº
            tmp = tmp -> next;
            cur = cur -> next;
        }
        
        return newList->next;
    }
};
```
