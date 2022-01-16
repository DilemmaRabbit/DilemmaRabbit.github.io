---
title: "HackTheBox academy : Web Requests"
date: 2022-01-10T19:06:35+08:00
draft: true
---

# 前言

## HTTP

## HTTPS

## Request / Response

## Headers

一般常見的 header 可以分成五類 ：

1. General Headers
    - Date
    - Connection
---

2. Entity Headers
    - Content-Type
    - Media-Type
    - Boundary
    - Content-Length
    - Content-Encoding
---

3. Request Headers
    - Host
    - User-Agent
    - Accept
    - Cookie
    - Referer
    - Authorization
---

4. Response Headers
    - Server
    - Set-Cookie
    - www-authenticate

---

5. Security Headers
    - Content-Security-Policy
    - Strict-Transport-Security
    - Referrer-Policy

## HTTP Methods and Codes

## GET Method

## POST Method

相較於 **GET**，**POST** 擁有以下好處 ：

1. 日誌紀錄的敏感資訊較少：對於一些 log 檔而言，將參數存放在 URL 中的 GET 方法相對危險，無法滿足傳送敏感資訊的要求
  
    **(※因此當發現用 GET 傳送登入資訊時，可以嘗試去挖 log 檔)**

---

2. 較少加密或編碼的需求：由於 URL 被設計用來分享，代表 URL 內所有資料都必須經過一定標準來進行編碼或加密。

    **然而 POST 的資料傳送使用二進位來傳送，只需要紀錄分隔符 ";"**

---

3. 可傳送更多資料：URL長度受到瀏覽器、網頁伺服器、SDN 供應商、URL 縮短器影響，一般來說 URL 建議約 2000 字元以內

---

## PUT / DELETE Methods

## cURL

## 結語
