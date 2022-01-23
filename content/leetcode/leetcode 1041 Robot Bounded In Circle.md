---
title: "Leetcode 1041 Robot Bounded In Circle"
date: 2022-01-09T13:04:57+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Math, String, Simulation

### 題目（英文）

```
On an infinite plane, a robot initially stands at (0, 0) and faces north. The robot can receive one of three instructions:

"G": go straight 1 unit;
"L": turn 90 degrees to the left;
"R": turn 90 degrees to the right.

The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.
```

---

### 測資

```
Example 1:

Input: instructions = "GGLLGG"
Output: true
Explanation: The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.

Example 2:

Input: instructions = "GG"
Output: false
Explanation: The robot moves north indefinitely.

Example 3:

Input: instructions = "GL"
Output: true
Explanation: The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...

 

Constraints:

1 <= instructions.length <= 100
instructions[i] is 'G', 'L' or, 'R'.
```

---

### 題目（中文）

```
在一個無限大的平面上，一個機器人在原點(0,0)的位置並且朝北的方式接受指令

具有以下三種指令

'D' : 機器人向目前方向前進一步
'L' : 機器人向左轉 90 度
'R' : 機器人向右轉 90 度

機器人的行為由以上三種字元組成的字串而成，並且不斷地重複執行

回傳 true 代表機器人的行為會有繞圈的行為，機器人永遠都會在固定的路徑上行動
```

---

## 思路

1. 先判斷出一次指令執行完後，最終機器人的位置以及目前面對方向

2. 如果在 0,0 代表機器人剛好回到原點，必定有繞圈

3. 如果機器人的面向位置沒有改變，代表如果機器人一旦移動，必定不會繞圈(※需要比原點還晚判斷，因為有可能機器人只是純粹轉方向)

4. 如果機器人面向位置沒有改變，代表機器人在4次輪替後必定會回到北面，而行動也會各個方向都跑一次，代表一定有繞圈行為

---

## 程式碼

```c++
class Solution {
public:
    bool isRobotBounded(string instructions) {
        
        vector<int> position{0,0}; // 初始化位置
        vector<vector<int>> direction{{0,1},{1,0},{0,-1},{-1,0}}; // 建立四個方位的移動值
        int cur_direction = 0; // 初始化方位
        
        for(auto instruction:instructions){
            if(instruction == 'G'){ // 前進
                position[0] = position[0] + direction[cur_direction][0];
                position[1] = position[1] + direction[cur_direction][1];
            }
            else if(instruction == 'L'){ // 改變左向
                cur_direction = (cur_direction-1 % 4 + 4) % 4;
            }
            else{ // 改變右向
                cur_direction = (cur_direction+1) % 4;
            }
        }
        
        if(position[0] == 0 && position[1] == 0){
            return true; // 判斷是否在原點
        }
        
        if(!(cur_direction == 0)){
            return true; // 判斷是否面向北方
        }
        
        return false;
    }
};
```

---

## 結果

![](https://i.imgur.com/FpI3cVs.png)
---

## 結語
原本想要直接用 mod 的方式控制轉向的方向，但是必須考慮是從 L 或是 R 轉向而來的，會導致加總結果不一樣，最後索性直接設定固定值(因為才四個方向)，透過 cur_direction來判斷目前要使用的方向。