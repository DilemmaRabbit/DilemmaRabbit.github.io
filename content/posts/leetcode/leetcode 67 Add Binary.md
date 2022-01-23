---
title: "Leetcode 67 Add Binary"
date: 2022-01-10T14:04:27+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢

### 相關主題：Math, String, Bit Manipulation, Simulation

### 題目（英文）

```
Given two binary strings a and b, return their sum as a binary string.
```

---

### 測資

```
Example 1:

Input: a = "11", b = "1"
Output: "100"
Example 2:

Input: a = "1010", b = "1011"
Output: "10101"
 

Constraints:

1 <= a.length, b.length <= 104
a and b consist only of '0' or '1' characters.
Each string does not contain leading zeros except for the zero itself.
```

---

### 題目（中文）

```
給予兩字串 a、b，回傳兩者的二進位加法字串
```

---

## 思路

1. 先將兩數翻轉方便運算

2. 將字串較短的部分後面(高位)補0

3.比較每個位數的進位值以及餘數

4.將最後的進位補上
---

## 程式碼

```
class Solution {
public:
    string addBinary(string a, string b) {
        
        string res="";
        int carry = 0;
        int size_a = a.size();
        int size_b = b.size();
        
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        
        for(int i = 0; i < abs(size_a-size_b); i++){
            if(size_a > size_b) b += '0';
            else a += '0';
        }
        
        for(int i = 0; i < a.size(); i++){            
            res += (carry + (a[i] - '0') + (b[i] - '0')) % 2 + '0';
            carry = (carry + (a[i] - '0') + (b[i] - '0')) / 2;
        }
        
        if(carry == 1){
            res += '1';
        }
        
        reverse(res.begin(),res.end());
        
        return res;
    }
};
```

---

## 結果
![](https://i.imgur.com/RwSoB1B.png)

---

## 結語

主要問題在不可將整個字串轉為整數在進行計算，必須單個位數或是直接對字串值做處理

非數字使用 bit operation 需要注意(空字元 != 0)，會影響進位運算