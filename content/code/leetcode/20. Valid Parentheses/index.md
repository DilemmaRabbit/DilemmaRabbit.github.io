---
title: "20. Valid Parentheses"
date: 2022-03-13T12:04:42+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]


---

**難度：🟢 Easy 🟢**

**關鍵字：Stack**

<!--more-->

---

## 問題描述

給予一個只包含了大中小括號的字串，使用以下規則來確定此字串是否合法

1. 每種括號都必須閉合(左右對稱)
2. 括號必須以正確的順序進行閉合
   - "{ } ( ) []"
   - "( [ { } ] )"

---

## 思路

個人思路
- 由於必須進行閉合，因此可以確保一定是存在相對的括號
- 由於需要進行指定順序閉合，當遇到右括號時，代表上一個一定是對應的左括號，否則不合法
- 用 stack 儲存遇到所有的左括號，遇到右括號則比對目前的 stack top 是否正確
- 假如正確則 pop 成對的括號
- 最後 stack 必定為空

---

## 程式碼

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for(auto chr : s){
            if(chr == '}' || chr == ']' || chr == ')'){ // 如果為右括號
                if(st.empty()){ // 確保是否為空
                    return false;
                }
                else{
                    if(chr-1 == st.top() || chr-2 == st.top()){ // 取得目前左括號的數值是否正確
                        st.pop();
                    }
                    else{
                        return false; // 沒有找到對應的左括號，代表不合法的狀態
                    }
                }
            }
            else{
                st.push(chr); // push 左括號的數值
            }
        }
        return st.empty(); // 最後依 stack 狀況決定結果
    }
};
```
