---
title: "622. Design Circular Queue"
date: 2022-09-25T16:37:19+08:00
draft: false
categories: ["Learning"]
Tags: ["LeetCode"]

resources:
  - name: "featured-image"
    src: "leetcode.png"
---

難度：🟠 Normal 🟠

---

## 問題描述

完成一個循環佇列(FIFO)的結構，須包含以下功能
1. MyCircularQueue(k)：產生一條長度為 k 的駐列
2. int fornt()：回傳目前 head 的值，如果不存在，回傳 -1
3. int rear()：回傳目前 tail 的值，如果不存在，回傳 -1
4. boolean enQueue(int value)：新增新值到 queue 的尾端，如果操作失敗(佇列滿了)，回傳 false
5. boolean deQueue()：移除目前 head 的值，如果操作失敗(佇列為空)，回傳 false
6. boolean isEmpty()：回傳目前 queue 是否為空的狀況
7. boolean isFull()：回傳目前 queue 是否為滿的狀況

---

## 思路

- 用兩個 pointer (front/rear) 來追蹤目前的頭與尾
- 移除不用真的移除 queue 內原本資料，修改新的 front 就好
- 移除需要考慮移除到空的處理
- full/empty 用 front 與 rear 來判斷

---

## 程式碼

```c++
class MyCircularQueue {
public:
    
    vector<int> queue;
    int end;
    int front;
    int rear;
    
    // 檢查遍歷用
    // void check(string s){
    //     cout << s << endl;
    //     for(int i = 0; i < queue.size(); i++){
    //         cout << queue[i] << " ";
    //     }
    //     cout << endl;
    //     cout << "front: " << front << endl;
    //     cout << "rear: " << rear << endl; 
    //     cout << "=================" << endl;
    // }
    
    // 初始化 queue
    MyCircularQueue(int k) {
        queue.resize(k);
        front = -1;
        rear = -1;
        end = k;
        // check("new");
    }
    
    // 插入 value
    bool enQueue(int value) {
        if(front == -1){ // queue 為空
            queue[0] = value;
            front = 0;
            rear = 0;
            // check("insert " + to_string(value));
            return true;
        }
        else{
            if((rear + 1) % end != front){ // queue 還有空位
                rear = (rear + 1) % end;
                queue[rear] = value;
                // check("insert " + to_string(value));
                return true;
            }
            else{ // queue 還有已滿
                // check("insert " + to_string(value));
                return false;
            }
        }
    }
    
    // 移除 value
    bool deQueue() {
        if(!isEmpty()){
            if(rear == front){ // 只剩下一個元素
                rear = front = -1;
            }
            else{ // 還有多個元素
                front = (front + 1) % end;
            }
            // check("delete");
            return true;
        }else{ // queue 為空
            // check("delete");
            return false;
        }
    }
    
    int Front() {
        // check("front check");
        if(front == -1){
            return front;
        }
        return queue[front];
    }
    
    int Rear() {
        // check("rear check");
        if(rear == -1){
            return rear;
        }
        return queue[rear];
    }
    
    bool isEmpty() {
        // check("empty check");
        if(front == -1) return true;
        else return false;
    }
    
    bool isFull() {
        // check("full check");
        if((rear + 1) % end == front) return true;
        return false;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```
