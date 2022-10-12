---
title: "258. Add Digits"
date: 2022-02-08T16:04:48+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟢 Easy 🟢**

**關鍵字：Bitwise**

<!--more-->
---

## 問題描述

將一個正整數的所有位數做加總，直到只剩下個位數，並返回該值。

測資 **-1 < num < 2^32** 。

---

## 思路

只計算當次的個位數，如果大於 10 代表只保留個位數並 +1，算至最後 num 為 0 。

---

## 程式碼

```c++
class Solution {
public:
    int addDigits(int num) {
        int cur = 0; // initial answer
        while(num != 0){
            cur += num%10; // add tail number
            if(cur >= 10){ // if cur > 0, remain tail and + 1
                cur = cur % 10 + 1;
            }
            num /= 10;
        }
        return cur;
    }
};
```
