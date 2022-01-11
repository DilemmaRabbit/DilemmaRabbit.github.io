---
title: "Leetcode 1022 Sum of Root to Leaf Binary Numbers"
date: 2022-01-11T13:35:54+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢

### 相關主題：Tree, Depth-First Search, Binary Tree

### 題目（英文）

```
You are given the root of a binary tree where each node has a value 0 or 1. Each root-to-leaf path represents a binary number starting with the most significant bit.

For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf. Return the sum of these numbers.

The test cases are generated so that the answer fits in a 32-bits integer.
```

---

### 測資


Example 1  Gragh

![](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)
```
Example 1:

Input: root = [1,0,1,0,1,0,1]
Output: 22
Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22

Example 2:

Input: root = [0]
Output: 0

Constraints:

The number of nodes in the tree is in the range [1, 1000].
Node.val is 0 or 1.
```

---

### 題目（中文）

```
給予一棵二元樹，其中的值只會由 0 與 1 組成，從跟節點到葉節點會形成一個二進位數字(高到低位數)

例如：如果節點遍歷後是 0 -> 1 -> 1 -> 0 -> 1，則此路徑的值為 01101(13)

對於上述例子，考慮所有可能產生的值並進行加總

所有測資均會保證答案在 32 bit 之內
```

---

## 思路

1. 由於題目保證不會溢位，所以我們可以直接對目前的值做運算
   
2. 上一個節點相對於下一個節點，相當於 x2，也就是 << 1 的效果

3. 紀錄當回合目前的總和數字
   1. 如果下一個是葉節點：x2 並根據目前節點值決定要不要 +1，並丟到下一個節點
   2. 如果下一個是一般節點：x2 並根據目前節點值決定要不要 +1  
---

## 程式碼

```
class Solution {
public:
    
    int sum = 0;
    
    void dfs(int cur_val, TreeNode *cur){
        if(cur -> left == NULL && cur -> right == NULL){
            cur_val = cur_val << 1;
            if(cur->val == 1){
                cur_val += 1;
            }
            sum += cur_val;
            return;
        }
        
        cur_val = cur_val << 1;
        if(cur->val == 1){
            cur_val += 1;
        }
        
        if(cur->left){
            dfs(cur_val,cur->left);
        }
        if(cur->right){
            dfs(cur_val,cur->right);
        }
    }
    
    int sumRootToLeaf(TreeNode* root) {
        
        if(root){
            dfs(0,root);
        }
        
        return sum;
    }
};
```

---

## 結果

![](https://i.imgur.com/rLPcq2B.png)

---

## 結語

稍微變種的 dfs，主要目的也是算出所有可能的路徑，可以透過 bit wise 預先計算值來減少最後進行的換算