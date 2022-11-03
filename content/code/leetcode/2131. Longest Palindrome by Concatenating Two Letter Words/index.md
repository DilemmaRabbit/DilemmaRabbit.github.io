---
title: "2131. Longest Palindrome by Concatenating Two Letter Words"
date: 2022-11-03T14:05:07+08:00
draft: false
---

**難度：🟠 Normal 🟠**

**關鍵字：String, Hash Table**

<!--more-->

## 問題描述

- 給予一個包含了兩個字元的陣列，返回可以形成的最長回文字串長度

---

## 思路

- 給予的字串，可以有兩種情況
  1. 同樣的字元(ex. aa, bb, gg)
  2. 不同的字元(ex. ab, ck, kc)
- 要形成回文，會有兩種情況
  1. 字串在中間，分成兩端(只有同樣的字元可以達到這種狀況)
  2. 字串在某一側，需要有相反的字串，相同/不同的字元都可以
- 基於剛剛的想法，可以建立以下的規則
  1. 相同字元的字串出現的情況下，如果是奇數次數，代表可以成為中間值，但後續出現的奇數字串卻不可以
  2. 相同字元的字串把偶數次數 / 2，代表可以形成回文的兩側
  3. 不同字串出現的情況下，取相反字串的次數，把最小的次數作為共同的次數
  4. 將剛剛得到的次數依據相同/不同的狀況加總上去

- unordered_map
  - 如果對正在疊代的 unordered_map 進行存取的話，如果存取到不存在的值，會新增一個值進 table 中，造成疊代的順序出現問題
    - 解決方法
      1. 使用 find() 去從 unordered_map 找值，不用直接存取的方式來尋找值
      2. 使用 map 來實作 hash_map

--- 

## 程式碼

```c++
class Solution {
public:

    string str_reverse(string s){
        reverse(s.begin(), s.end());
        return s;
    }

    int longestPalindrome(vector<string>& words) {
        unordered_map<string, int> table;
        
        int res = 0;

        for(string word: words){
            table[word]++; // 建立對照表
        }
        
        for(auto &[k, v]: table){
            if(k[0] == k[1]){
                if(v%2==1){
                    if((res/2)%2==0) res+=2; // 如果還沒把中間的情況加進去的話
                    res+=(v-1)*2;
                }
                else res+=v*2;
            }
            else{
                string tmp = str_reverse(k); // reverse string
                if(table.find(tmp) != table.end()) res += min(v, table[tmp])*4; // 把比較小的次數 * 4
                v = 0; // 把目前的 value 改成 0，遇到相反的字串時便不會處理
            }
        }

        return res;
    }
};
```
