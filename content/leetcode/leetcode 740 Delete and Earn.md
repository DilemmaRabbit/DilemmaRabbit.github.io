---
title: "Leetcode 740 Delete and Earn"
date: 2022-01-10T14:04:12+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Hash Table, Dynamic Programming

### 題目（英文）

```
You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times:

    Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1.

Return the maximum number of points you can earn by applying the above operation some number of times.
```

---

### 測資

```
Example 1:

Input: nums = [3,4,2]
Output: 6
Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.

Example 2:

Input: nums = [2,2,3,3,3,4]
Output: 9
Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.

 

Constraints:
1 <= nums.length <= 2 * 104
1 <= nums[i] <= 104
```

---

### 題目（中文）

```
你有一個整數的陣列，以下列的方式取得點數：
    
選擇一個數字 i，除了刪除這個陣列元素外，會將數值為 i - 1 以及 i + 1 的所有陣列元素刪除，並且獲得點數 i。

藉由重複此行為，回傳最大可獲得的點數值。
```

---

## 思路

1. 既然需要拿取特定數字，就連重複的都一起拿

2. 當紀錄好每個 i 可獲得的 points 時，就變成了 [Robber House](https://dilemmarabbit.github.io/leetcode/leetcode-198-house-robber/) 問題

3. 考慮中間不存在 i 的情況
---

## 程式碼

```c++
class Solution {
public:
    int deleteAndEarn(vector<int>& nums) {
        
        int tmp, take, notake;
        int res;
        int max_num = 0;
        map<int, int> table;
        
        if(nums.size()==1){
            return nums[0];
        }
        
        for(int i=0;i<nums.size();i++){
            if(table[nums[i]] == 0){
                table[nums[i]] = nums[i];
            } 
            else{
                table[nums[i]] += nums[i];
            }
            if(nums[i] > max_num){
                max_num = nums[i];
            }
        }
        
        for(int i=1;i<max_num+1;i++){
            if(i == 1){
                notake = 0;
                take = table[i];
            }
            else{
                tmp = notake;
                notake = max(take, notake);
                take = tmp + table[i];   
            }
            if(res < max(take, notake)){
                res = max(take, notake);
            }
        }
        
        cout << res;
        return res;
    }
};
```

---

## 結果

![](https://i.imgur.com/RwSoB1B.png)
---

## 結語
把每個特定的 i 紀錄合併起來後，發現就變成了中間存在空值的 [Robber House](https://dilemmarabbit.github.io/leetcode/leetcode-198-house-robber/) 問題了

作法是把中間的都設成 0，然後用一般的 dp 方式來紀錄