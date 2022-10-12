---
title: "838. Push Dominoes"
date: 2022-09-27T14:42:21+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Simulation, Dynamic Programing**

<!--more-->
---

## 問題描述

給予一個字串 dominoes，代表骨牌的方向，"R" , "L" 分別代表往左/右推，"." 則代表不被推動的骨牌，求依據字串推倒後的骨牌呈現狀況

---

## 思路

1. 當 "左" 、 "右" 各有 "右" 、 "左" 推的骨牌時，靠近右推/左推的骨牌會變成右/左，如果剛好位於正中間，則會處於平衡狀態
2. 記錄這個骨牌被左推/右推的比例，決定靠向哪一邊

- two vector
  - 兩個長度為骨牌長度的陣列，分別代表左推/右推的狀況
  - 從左至右遍歷(右推狀況)第一個陣列
    - "R"，將 tmp 設為最大值(100000)，代表開始遞減比重
    - "L"，將 tmp 歸零，代表從頭開始
    - "."，如果左邊不是 "R"(tmp = 0) 的狀況，直接為 0 ，否則加上比重並 tmp--
  - 從右至左遍歷(左推狀況)第二個陣列
    - "R"，將 tmp 歸零，代表從頭開始
    - "L"，將 tmp 設為最小值(-100000)，代表開始遞增比重
    - "."，如果左邊不是 "R"(tmp = 0) 的狀況，直接為 0 ，否則加上比重並 tmp++
  - 相加兩個陣列，如果為零代表平衡、負數代表左推、正數代表右推

- two pointer
  - 與上述作法用兩個不同方面的 pointer 改善，減少一個陣列以及 for 循環

- **是否能在一個方向的遍歷完成?**
- **是否不需要額外的陣列完成?**

---

## 程式碼

```c++
class Solution {
public:
    
    
    // two pointer
    string pushDominoes(string dominoes) {
        
        vector<int> map(dominoes.size());
        
        int tmp = 0, tmp2 = 0;
        int left = 0;
        int right = map.size()-1;
        
        string res="";
        
        for(int i = 0; i < dominoes.size(); i++){
            if(dominoes[left + i] == 'R') tmp = map[left + i] = 100000;
            else if(dominoes[left + i] == '.'){
                if(tmp != 0){
                    tmp--;                    
                    map[left + i] += tmp;
                }
            }
            else tmp = 0;  
            
            if(dominoes[right - i] == 'L')
                tmp2 = map[right - i] = -100000;
            else if(dominoes[right - i] == '.'){
                if(tmp2 != 0){
                    tmp2++;
                    map[right - i] += tmp2;
                }
            }
            else tmp2 = 0;  
        }
                
        for(int i = 0; i < dominoes.size(); i++){
            if(map[i] > 0) res += "R";
            else if(map[i] == 0) res += ".";
            else res += "L";
        }
        
        return res;
        
    }
};

class Solution {
public:

    // two vector
    string pushDominoes(string dominoes) {
        
        vector<int> right(dominoes.size());
        vector<int> left(dominoes.size());
        
        int tmp = 0;
        string res="";
        for(int i = 0; i < dominoes.size(); i++){
            if(dominoes[i] == 'R'){
                tmp = right[i] = 100000;
            }
            else if(dominoes[i] == '.'){
                if(tmp != 0) tmp--;                    
                right[i] = tmp;
            }
            else{
                right[i] = tmp = 0;  
            }
        }
        
        tmp = 0;
        
        for(int i = dominoes.size()-1; i >= 0; i--){
            if(dominoes[i] == 'L'){
                tmp = left[i] = -100000;
            }
            else if(dominoes[i] == '.'){
                if(tmp != 0) tmp++;
                left[i] = tmp;
            }
            else{
                left[i] = tmp = 0;  
            }
        }
        
        for(int i = 0; i < dominoes.size(); i++){
            if(right[i]+left[i] > 0){
                res += "R";
            }
            else if(right[i]+left[i] == 0){
                res += ".";
            }
            else{
                res += "L";
            }
        }
        
        return res;
        
    }
};
```
