---
title: "Leetcode 997"
date: 2022-01-03T13:14:32+08:00
draft: false
Tags: ["LeetCode"]
---

## 問題描述

### 分類：🟢 easy 🟢
### 相關主題：Graph, Hash_table, Array

### 題目（英文）
```
In a town, there are n people labeled from 1 to n. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

    1. The town judge trusts nobody.
    2. Everybody (except for the town judge) trusts the town judge.
    3. There is exactly one person that satisfies properties 1 and 2.

You are given an array trust where trust[i] = [ai, bi] representing that the person labeled ai trusts the person labeled bi.

Return the label of the town judge if the town judge exists and can be identified, or return -1 otherwise.
```

### 測資

```
Example 1:
Input: n = 2, trust = [[1,2]]
Output: 2

Example 2:
Input: n = 3, trust = [[1,3],[2,3]]
Output: 3

Example 3:
Input: n = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1

Example 4:
Input: n = 3, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: -3

Example 5:
Input: n = 3, trust = [[1,2],[2,3]]
Output: 

```

### 題目（中文）

```
在城鎮中有 n 個人，分別標示 1 ~ n。並且在這 n 個人之中，藏著一位鎮法官

鎮法官及其他人有以下特徵

1. 鎮法官不相信任何人
2. 其他人必定會相信鎮法官(也可能會相信其他正常人)
3. 每個人只會有前者兩種情況的其中一個

現在具有一份每個人相信的名單 trust ，其中 [ai, bi] 代表 ai 相信 bi

請找出可能的鎮法官編號，或者返回 -1
```

## 思路

1. 鎮法官必定被所有人相信，所以如果在鎮法官存在的情況下，bi 出現的次數一定是 n-1
2. 鎮法官一定不會相信任何人，對可能存在的鎮法官編號，不可能在 ai 中出現


## 程式碼

```
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        
        int i, possible=-1;
        map<int, int> table;
        
        for(i=0; i<trust.size(); i++){
            table[trust[i][1]] += 1;
        }
        
        for(i = 1; i <= n; i++){
            if(table[i] == n-1){
                possible = i;
            }
        }
        
        for(i = 0; i<trust.size();i++){
            if(trust[i][0] == possible){
                return -1;
            }
        }
        
        return possible;
    }
};
```

## 結果

![](https://i.imgur.com/LwjDxGm.png)

## 結語
第一篇值得紀念(~~?~~)的刷題文，雖然只是easy(~~然後效率跟記憶體使用率還不怎麼高~~)
原本看到的時候想用 graph 做的，因為既然鎮法官不會相信任何人，代表只要經過鎮法官後的 edge 不會有任何可以連出去的 line，但也要同時確保所有的 edge 都有連到鎮法官的 line，想想還是要確定鎮法官的存在，好像不會比用 hash_table 計算來的方便，就直接放棄用 graph 做了。