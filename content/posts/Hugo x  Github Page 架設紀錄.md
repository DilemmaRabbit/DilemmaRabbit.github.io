---
title: "Hugo x GitHub Pages 架設紀錄"
date: 2022-01-02T15:31:23+08:00
draft: false
Tags: ["Hugo","GitHub-Pages","Tutorial"]
---

# 目錄

1. [前言](#前言)
2. [先前準備](#i-先前準備)
3. [本機建立](#ii-本機建立網站)
4. [GitHub自動部署](#iii-丟到-github-做自動化部署)
5. [問題集](#問題採坑集)
6. [延伸學習](#延伸學習)


# 前言

終於把 Hugo 跟 GitHub Pages 成功架起來了(就是你現在在看的東西🥳)，途中不知道刪了幾次 repo，也遇到了不少 Hugo 以外的問題，除了學習 Hugo 外也重新把 git 的一些相關知識重新認識了一遍，不過途中還是有遇到目前原因不明但解決的問題，會列在下面的採坑集跟延伸學習裡面，如果有解決方法不吝指教～

# I. 先前準備

## 環境

系統: ubuntu 20.04

## Github Page 建立

#### 1. 建立一個名稱為 username.github.io 的 repo

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

優點
1. 簡單明瞭，對寫技術文章來說很方便
2. 配置簡單直觀，不用設定太多額外的東西就可以使用

缺點
1. 能客製化部份偏少
2. 右邊有一大塊不明用途的區塊
3. 許多 md 語法的背景沒有顏色，需要自己額外加 scss (有人有發 issue，可以參考去改)
4. 不支援留言功能

### 2. 透過 **submodule** 的方式將主題加入 repo 中的 /themes

```
git sudmodule add <repo-url> /themes/<theme name>
```

### 3. **config.toml** 中增加以下這行
```
theme = "<theme name>"
```

🏳 這邊還可以對主題進行更細部的設定喔，請依每個 theme 的 GitHub 的說明設定

## 文章建立

### 1. 建立一篇新的文章

hugo 預設的文章資料夾為 **/content/posts/** ※你也可以把 post 改成任何想要的路徑或分類

```
hugo new posts/<filename>.md
```

前往 /content/posts 打開剛剛建立的 md 可以發現以下資訊

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
git push -u origin gh-pages
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

Settings -> Pages，將 source 改成新的分支，就可以嘗試 username.github.io 查看網頁是否正常

![](https://i.imgur.com/K7MARDb.png)

## 🎉最後從本機端產生一個 push，查看是否會自動進行更新及部署🎉

---

## 問題(~~踩坑~~)集

1. **server / GitHub Pages 開啟後白畫面**
   
   * **問題**：一進入網頁看不到任何資料
   * **原因**：主題設定錯誤或遺失
   * **解決方法**：通常是 **config** 沒有按照指定格式或是 **typo**(透過使用 **hugo** 通常可以看到錯誤資訊(本機端))，也有可能是 **submodule** 造成的問題，請看下方問題3

2. **GitHub Pages 開啟後無 css 渲染**
   
   * **問題**：看得到內容，但是主題所提供的效果消失
   * **原因**：透過開發人員工具可看見問題，css 只接受 https，但是預設的 ```http://example.com``` 使用的是 http
   * **解決方法**：在 **config** 的 **baseurl** 改成目前所使用的 **Github Page url** (記得 **https**)
   * **參考資料**：[Hugo](https://discourse.gohugo.io/t/css-files-do-not-load-due-to-not-being-https/12627)

3. **submodule 消失問題**
   
   * **問題**：當進入另外一個分支後，假如把 submodule 直接刪除並提交的話，會發現主分支的 submodule 會一併消失
   * **原因**：[stackoverflow](https://stackoverflow.com/questions/52345460/remove-git-submodule-from-a-specific-branch)
   * **解決方法**：把 master 的 sunmodule index 重新洗掉後直接在加入一次 submodule（※暴力解注意，雖然有用但還是建議搞懂 submodule 的運作原理）
        
        ```
        git rm -r --cached themes/<theme directory>
        git submodule add <theme repo> <theme directory>
        ```


4. **文章無法顯示問題**

   * **問題**：明明有文章，但是 GitHub Pages 顯示不出來
   * **原因**：文章可能是草稿狀態，且 action 在執行時沒有允許草稿(~~我就是那個沒注意到的~~)
   * **解決方法**：在 workflow 中的 Hugo 加上 -D 參數，或是乾脆把文章中 draft 改成 false

5. **tags 無法顯示**

   * **問題**：有時候某些 tag 無法正常顯示
   * **原因**：tag 中間不能有空白，會導致無法正常顯示(雖然分類會正常顯示)
   * **解決方法**：用 - 代替空白 (GitHub Pages -> GitHub-Pages) 即可正常顯示空白

## 延伸學習

1. 紀錄所用到的 git 技巧

2. git submodule

- 簡介：當自己的專案使用其他的專案的檔案時，除了把整份資料進行 clone 或是 fork 外，也可以透過 submodule 的方式來進行其他專案的使用，並且在
  
- 新增 submodule

- 移除 submodule

1. github ssh key