---
title: "Leetcode 701 Insert Into a Binary Search Tree"
date: 2022-01-12T17:15:17+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：

### 題目（英文）

```
You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.
```

---

### 測資

Example 1 figure

![](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

```
Example 1:

Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]

Example 2:

Input: root = [40,20,60,10,30,50,70], val = 25
Output: [40,20,60,10,30,50,70,null,null,25]

Example 3:

Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
Output: [4,2,7,1,3,5]
```

---

### 題目（中文）

```
給予一個二元樹的 root 節點，並且給予一個 value 做為新的插入值，回傳這個新的二元樹
測資保證新的插入值不會在原本二元樹中

可能具有多種解答，回傳其中一種便可
```

---

## 思路

1. new 一個新的 node 並初始化

2. 用 while 迴圈遍歷二元樹
   1. 比 node 小就向左遍歷
   2. 比 node 大就向右遍歷

3. 當判斷到即將遍歷的點為 NULL 的話，將即將遍歷的方向指向新產生的節點 
---

## 程式碼

```c++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        
        TreeNode *tmp = root;
        TreeNode *node = new TreeNode;
        
        node -> left = NULL;
        node -> right = NULL;
        node -> val = val;
        
        if(!tmp){
            return node;
        }
        
        while(tmp != NULL){
            if(tmp -> val > val){
                if(tmp -> left == NULL){
                    tmp -> left = node;
                    break;
                }
                else{
                    tmp = tmp -> left;
                }
            }
            if(tmp->val < val){
                if(tmp -> right == NULL){
                    tmp->right = node;
                    break;
                }
                else{
                    tmp = tmp -> right;
                }
            }
        }
        
        return root;
    }
};
```

---

## 結果

![](https://i.imgur.com/Rid8FH5.png)

---