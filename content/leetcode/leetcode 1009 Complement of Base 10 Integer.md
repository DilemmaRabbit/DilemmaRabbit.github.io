---
title: "Leetcode 1009 Complement of Base 10 Integer"
date: 2022-01-04T10:25:40+08:00
draft: false
Tags: ["LeetCode"]
---

## 問題描述

### 分類：🟢 Easy 🟢
### 相關主題：Bit Manipulation

---

### 題目（英文）

```
The complement of an integer is the integer you get when you flip all the 0's to 1's and all the 1's to 0's in its binary representation.

For example, The integer 5 is "101" in binary and its complement is "010" which is the integer 2.

Given an integer n, return its complement.
```
---

### 測資

```
Example 1:

Input: n = 5
Output: 2
Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.

Example 2:

Input: n = 7
Output: 0
Explanation: 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.

Example 3:

Input: n = 10
Output: 5
Explanation: 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.

Constraints:
0 <= n < 109

```

---

### 題目（中文）

```
補數是由將數字的二進位中的 0 與 1 做翻轉而來

例如5的二進位是 "101"，它的補數就是 "010"，換算為十進位就是 2

給定一個數字 n ，將它的補數以 10 進位表示出來

```

---

## 思路

1. 由於需要計算位數，需要先取得 n 的位數 (5(101) -> 3位數)
2. 透過指數運算，可以獲得與 n 相同位數且全是 1 的數 ( 2 ^ 3 = 8，8 - 1 = 7 (111) )
3. 直接與 n 做減法獲得補數的十位數

---

## 程式碼

```c++
class Solution {
public:
    int bitwiseComplement(int n) {
        
        int cnt=1;
        int tmp = n;
        
        while(tmp > 1){
            tmp /= 2;
            cnt++;
        }
                  
        return pow(2,cnt)- 1 - n;
    }
};
```

改良點的寫法(雖然感覺不出來時間跟空間使用的差別)
```c++
class Solution {
public:
    int bitwiseComplement(int n) {
        
        int cnt=1;
        
        while( n - pow(2,cnt) >= 0){
            cnt++;
        }
                  
        return pow(2,cnt)- 1 - n;
    }
};
```

---

## 結果
![](https://i.imgur.com/O5EsK80.png)

---

## 結語
使用空間有點不甚理想，主要是多在紀錄原本的 n 的 tmp 以及 cnt 上面，目前想到的思路是用 cnt 做指數運算跟 n 做減法來判斷迴圈結束，這樣就可以少一個 tmp 了，但會不會影響運行時間還要在嘗試看看。

比較值得注意的是原本我想用 log2() 來直接判斷位數，不過這麼做會有一些問題，假如出來結果是整數的話會造成判斷錯誤。

例如同樣比較 4,5,6,7(照理來說都要用 2^3(8)) 去做減法，但是代到 log2() 後發現結果是 2, 2.. , 2.. , 2..，如果有小數點就可以直接無條件進入成 3，但對整數而言還要在額外 ＋1，需要額外寫條件。