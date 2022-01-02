---
title: "Hugo x GitHub Pages 架設紀錄"
date: 2022-01-02T15:31:23+08:00
draft: false
tags: ["Hugo","GitHub Pages"]
---

# 前言

終於把 Hugo 跟 GitHub Pages 成功架起來了(就是你現在在看的東西🥳)，途中不知道刪了幾次 repo，也遇到了不少 Hugo 以外的問題，除了學習 Hugo 外也重新把 git 的一些相關知識重新認識了一遍，不過途中還是有遇到目前原因不明但解決的問題，會列在下面的採坑集跟延伸學習裡面，如果有解決方法不吝指教～

# I. 先前準備

## 環境

系統: ubuntu 20.04

## Github Page 建立

#### 1. 建立一個 username.github.io 的 repo

![](https://i.imgur.com/4sRZcwd.png)

#### 2. 從 github **clone** 到本機

```
git clone <username.github.io>
```

---

# II. 本機建立網站

## Hugo 安裝

### brew 安裝

[brew 安裝教學](https://rdpapa.tw/2020/11/30/how-to-install-homebrew-on-ubuntu-20-04-lts/)

#### 1. 透過 brew 安裝 Hugo 

```
brew install hugo 
```

## 建立一個新的網頁

#### 1. 目標資料夾下建立一個新的網頁

```
hugo new site <username.github.io>
hugo new site <username.github.io> --force # 如果資料夾內有其他資料則需要強制建立 (e.g.README)
```

## Hugo 主題安裝

### 1. 從 [官網](https://themes.gohugo.io/) 中找到一個 **適合** 作為 blog 的主題

此 blog 的主題為 [m10c](https://github.com/vaga/hugo-theme-m10c)

### 2. 透過 **submodule** 的方式將主題加入 repo 中的 /themes

```
git sudmodule add <repo-url> /themes/<theme name>
```

### 3. **config.toml** 中增加以下這行
```
theme = "<theme name>"
```

## 文章建立

### 1. 建立一篇新的文章

hugo 預設的文章資料夾為 **/content/posts/**

```
hugo new posts/<filename>.md
```

打開剛剛建立的 md 可以發現以下資訊

```
title: # 文章標題
date: # 文章時間戳
draft: # 是否為草稿(true/false)
```

## 本機 Hugo Server

### 1 啟動本機端的 Hugo Server

```
hugo server -D
```

hugo : 產生靜態網頁

server : 以本機伺服器啟動

-D ： 允許顯示草稿

![](https://i.imgur.com/SE5i8hh.png)

### 2. 訪問 **localhost:1313** ， 可看見剛剛建立的文章以及套用的主題

🏳 這個本機伺服器還能同步 content 內的 md 檔變化，動態顯示變更，可以用來預覽文章喔

---

# III. 丟到 GitHub 做自動化部署

## 建立分支

### 1. 建立一個孤兒分支(gh-pages)

```
git checkout --orphan gh-pages
```

### 2. 將這個目錄內的資料刪除

```
git rm -rf .
```

### 3. 提交這個新的分支

```
git add .
git commit -m "<message>"
git push origin gh-pages
```

![](https://i.imgur.com/TozoSLF.png)

❌ 目前有發現在新的分支刪除檔案時會有把 master 分支的 submodule 一併刪除的問題，詳見問題集

## 建立 Github action

### 1. 進入 GitHub 的 [token](https://github.com/settings/tokens) 頁面，建立一個新的 token

範圍選擇 workflow(如下圖)

![](https://i.imgur.com/2Lqzm9g.png)

![](https://i.imgur.com/zkmUlSp.png)

🏳 這個 token 只會在建立的時候顯示一次，關掉頁面之後只能更新 token 的範圍無法查看，請務必複製

### 2. 進入 repo 的 Settings -> Srcrets 頁面，用剛剛獲得的 token 產生一個新的 repository secret

![](https://i.imgur.com/YmfMTBr.png)

🏳 建立的 secret 名稱也要記住，待會建立的 actions 會使用到

### 3. 建立新的 Actions

New workflow -> set up a workflow yourself

![](https://i.imgur.com/eO0UDb7.png)

![](https://i.imgur.com/lqdX0hD.png)

官方的 [example](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

```
name: github pages # 名稱
on:
  push:
    branches:
      - # 改成自己的 main branch 名稱
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: # 如果 Hugo 是 extend 版本(使用 hugo -v 查看)，務必改為 true，否則註解掉就可以了

      - name: Build
        run: hugo # 如果你想要顯示 draft 文章，在後面加上 -D

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main' # 改成自己的 main branch 名稱
        with:
          github_token: ${{ secrets.<剛剛取的 secret 名稱> }}
          publish_dir: ./public
```

commit 後便會自動 deploy 一次，如果正常的話會在你的 gh-pages 分支只看到原本 /public 下的 html 內容了

## 將目前的預設 branch 改成新的 branch

Settings -> Pages，將 source 改成新的分支，就大公告成了，可以嘗試 username.github.io 觀看網頁是否正常了

![](https://i.imgur.com/K7MARDb.png)

---

## 問題(~~採坑~~)集(待補)

1. server / GitHub Pages 開啟後白畫面
   
2. GitHub Pages 開啟後無 css 渲染
   
3. submodule 消失問題
   
4. 文章無法顯示問題

## 延伸學習(待補)

1. 這個紀錄所用到的 git

2. git submodule
   
3. github ssh key(待補)