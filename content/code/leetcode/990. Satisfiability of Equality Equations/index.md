---
title: "990. Satisfiability of Equality Equations"
date: 2022-09-26T17:06:04+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Union Find**

<!--more-->

---

## 問題描述

給予一個包含兩種狀況的判斷式，"a==b"、"a!=b"，判斷所有的判斷式是否合理，回傳 true/false

---

## 思路

- Union find
1. 把每個字母進行編號(分別代表不同的群組)
2. 當 "=="，把右邊字母的群組改成左邊字母的群組
3. 假如右邊字母的群組不是原本的群組(被歸類過)，則在找到源頭為止，把所有的源頭都改成左邊字母的群組
4. 完成分群後，掃過每一個 "!="，當兩邊字母在同群組時，判斷為失敗

---

## 程式碼

```c++
class Solution {
public:
    std::vector<int> group;
    
    int find_leader(int cur){
        if(group[cur] == cur) return cur;
        else{
            group[cur] = find_leader(group[cur]);
            return group[cur];
        }
    }
    
    void merge(int prev, int after){
        
        int g1 = find_leader(prev-97);      
        int g2 = find_leader(after-97);
        
        if(g1 != g2){
            group[g2] = g1;
        }
    }
    
    bool equationsPossible(vector<string>& equations) {
        for(int i = 0; i < 26; i++) group.push_back(i);
        for(string equal: equations){
            if(equal[1] == '='){
                merge(equal[0], equal[3]);
            }
        }
        
        for(string equal: equations){
            if(equal[1] == '!'){
                if(find_leader(equal[0]-97) == find_leader(equal[3]-97)) return false;
            }
        }
        
        return true;
    }
};
```
