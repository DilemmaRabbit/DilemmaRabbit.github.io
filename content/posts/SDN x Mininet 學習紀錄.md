---
title: "SDN X Mininet 學習紀錄"
date: 2022-09-28T13:37:46+08:00
draft: true
---

# SDN
- SDN(soft define network)是一種網路架構，主要的用途為將 **資料平面(Data Plane)** 與 **控制平面(Control Plane)** 進行分離，達到更加靈活的網路架構
  - Control Plane：指的是 **如何決定資料處理** 的過程，包含封包路線、黑/白名單等決定封包流向規則
  - Data Plane：指的是 **將封包依據決定好的路線進行傳送** 的過程
  - SDN 提出將 Control Plane 提出為 controller 的方法，將網路中設備僅處理 Data Plane 的資訊
- 為什麼需要進行分離？傳統的做法是什麼？
  - 傳統是網路中的各個節點(應該是指 router)，依據內部韌體制訂好的規則、硬體本身寫死的規則來進行路由，缺點為被設備商綁定，無法實現靈活的路由規則。
- OpenvSwitch
  - 一種以軟體模擬 layer 3 switch
  - 實現了部分 router 的功能，但無法完全取代 router
  - 透過 openflow 協定與 controller 進行溝通
- Controller
  - 透過 openflow 協定與 layer 3 switch 溝通
  - ex. Ryu,Onos

# Mininet