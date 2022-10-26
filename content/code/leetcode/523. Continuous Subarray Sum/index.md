---
title: "523. Continuous Subarray Sum"
date: 2022-10-26T14:39:13+08:00
draft: false
---

**難度：🟠 Normal 🟠**

**關鍵字：Prefix, Hashmap, Math**

## 問題描述

給予一個 array，假設存在連續的 array 使得總和為 k 的倍數，返回 true

---  

## 思路

- Prefix
  - 既然要紀錄的是從 n ~ n+m 個元素累加後的結果，把累加結果先取餘數
  - 當遇到兩個一樣的餘數時，代表這兩個數字中間的結果累加為倍數
    - 在考慮是否為單獨一個的狀況

---

## 程式碼

### 個人寫法
```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        
        unordered_map<int, int> table;
        nums[0] %= k;
        table[nums[0]] = 1;
        for(int i = 1; i < nums.size(); i++){
            nums[i] = (nums[i] + nums[i-1]) % k;
            if(nums[i] == 0) return true;
            if(table[nums[i]] == 0) table[nums[i]] = i+1;
            else{
                if(i - (table[nums[i]]-1) > 1) return true;
            }
        }

        return false;
    }
};
```

### 其他寫法
```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        
        unordered_map<int, int> table;
        table[0] = -1; // 把 0 的倍數設為 -1，即可以達到(1)當 i 為 0 時判斷不存在,(2)當 i > 0 時判斷存在
        int sum = 0;

        for(int i = 0; i < nums.size(); i++){
            
            sum += nums[i];
            sum %= k;    

            if(table.find(sum)!=table.end()){ // 直接用 find 可避免 hash map 初始值為 0 的狀況
                if(i - table[sum] > 1) return true;
            }
            else{
                table[sum] = i;
            }
        }

        return false;
    }
};
```
