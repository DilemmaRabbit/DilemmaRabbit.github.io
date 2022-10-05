---
title: "HTTPS 101"
date: 2022-10-03T16:51:54+08:00
draft: false
---

## 前言

> 一直以來對 HTTPS 並沒有到很熟悉，只知道是透過加密的方式來保證傳輸的安全性，然後途中有用到 SSL / TLS 之類的方式，沒有一個很系統性的概念，因此寫了這篇文章來記錄一下這幾天

## HTTPS

- 傳統 HTTP 在傳送資料時，內容不會進行加密
  - 因此有潛在的 MITM 攻擊
- HTTPS 本身便是透過非對稱式加密的方式來達到安全的效果
  - **私鑰(private key)**：只有創造公鑰的人才會知道，用來解密用密鑰
  - **公鑰(public key)**：任何人都有可能知道，用來加密用的密鑰
- 因此 HTTPS 達到防止被竊聽原理為
  - 傳送給對方 request 時，用對方的 **public key** 進行加密
  - 接收到對方 request 時，用自己的 **private key** 進行解密

> 所以理應來說，雙方都會互相拿到彼此的 **public key**，才能進行相互的通訊

- 這邊就會有一些問題
  - **如何獲得？**
  - **這個過程如何確保不會被竊聽？**
  - **如何確保拿到的是正確的 public key？**

## TLS(Transport Layer Security)

- TLS 前身為 SSL(Secure Sockets Layer)，經過 IETF 的標準化後，目前發展到 1.3版本
- HTTPS 與 HTTP 相同，都是透過 **TCP(layer 4)** 來進行包裝的，但區別為 HTTPS 使用了 **TLS(layer 4)** 來對內部的資料進行加密與解密
- 被 TLS 加密的封包，只會看到 Request URL