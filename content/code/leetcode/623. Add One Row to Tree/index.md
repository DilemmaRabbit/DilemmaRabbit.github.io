---
title: "623. Add One Row to Tree"
date: 2022-10-05T10:35:07+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Binary Tree, DFS, BFS**

<!--more-->
---

## 問題描述

- 給予一個 binary tree 的 root, 插入值 val, 深度 depth，代表要在第 depth 層插入 val 值
  - 如果 depth 為 1，則將 root 插入新節點的左子樹
  - 如果 depth 不為 1，則在 depth - 1 的左右節點插入新值
    - 原本的左右子樹則根據原本的方向成為新節點的左右子樹

---

## 思路

- DFS
  - 遍歷所有節點，直到 depth - 1 時，並新增 node 
- BFS
  - 將所有 depth - 1 的節點儲存於節點中，並進行逐個新增 node
---

## 程式碼

DFS code
```c++
class Solution {
public:

    int val;
    int target_depth;

    void dfs(TreeNode* cur, int depth){
        if(depth == target_depth - 1){ // reach the parent node
            TreeNode* tmp;
            
            // create left node
            tmp = cur -> left; 
            cur -> left = new TreeNode(val);
            cur -> left -> left = tmp;
            
            // create right node
            tmp = cur -> right;
            cur -> right = new TreeNode(val);
            cur -> right -> right = tmp;
        }
        else{
            if(cur -> left) dfs(cur -> left, depth + 1); // enter next depth
            if(cur -> right) dfs(cur -> right, depth + 1); // enter next depth
        }
    }

    TreeNode* addOneRow(TreeNode* root, int val, int depth) {
        this -> val = val;
        this -> target_depth = depth;
        
        if(depth != 1) dfs(root, 1);
        else{ // if depth is 1
            TreeNode* tmp = new TreeNode(val);
            tmp -> left = root;
            root = tmp;
        }
        return root;
    }
};
```


BFS 
```c++
class Solution {
public:

    TreeNode* addOneRow(TreeNode* root, int val, int depth) {
        
        if(depth == 1){ // if depth is 1
            TreeNode* tmp = new TreeNode(val);
            tmp -> left = root;
            root = tmp;
        }
        else{
            // use queue to store node and depth
            queue<pair<TreeNode*, int>> bfs;
            bfs.push({root, 1});
            
            // push node until reach the depth - 1 level 
            while(bfs.front().second != depth - 1){
                TreeNode* cur = bfs.front().first;
                int dep = bfs.front().second;
                if(cur -> left) bfs.push({cur -> left, dep + 1});
                if(cur -> right) bfs.push({cur -> right, dep + 1});
                bfs.pop();
            }

            // pop all nodes with depth - 1 and create next node
            while(!bfs.empty()){
                TreeNode* cur = bfs.front().first;
                TreeNode* tmp;
                
                tmp = cur -> left;
                cur -> left = new TreeNode(val);
                cur -> left -> left = tmp;
                
                tmp = cur -> right;
                cur -> right = new TreeNode(val);
                cur -> right -> right = tmp;
                bfs.pop();
            }
        }
        

        return root;
    }
};
```
