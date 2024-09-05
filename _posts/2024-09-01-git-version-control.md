---
layout: post
title: "程式碼與牠們的產地(1)：Git 版本控制"
subtitle: "Git 版本控制基礎概念介紹"
date: 2024-09-01
last_modified_at: 2024-09-05
image: /assets/images/git-cover.jpg
tags: [Git, GitHub]
---
## 前言

相信某些人對於右邊那張圖片感到心有戚戚焉，其實呢，這個就是版本控制的雛形啊！

本篇就不談論什麼是 Git 了，會著重在 Git 基礎操作介紹上，如何初始化版本控制、加入檔案追蹤、提交變更等等。

## 如何安裝

請參考 [Git 官方教學](https://git-scm.com/book/zh-tw/v2/%E9%96%8B%E5%A7%8B-Git-%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8)，不論是 Linux, Windows 都有詳細步驟說明。

*建議使用套件管理器（例如：yum, apt-get, homebrew 等等）進行安裝，未來比較方便管理。*

## 初始化專案版本控制

承接上篇，我們已經完成撰寫程式碼了，接下來請在專案目錄下執行此指令，來初始化 Git 版本控制：

*這指令會將所有版本控制的訊息保留在 `.git` 這個隱藏的資料夾內。*

```bash
git init

# 輸出結果
提示：將「master」設定為初始分支的名稱。這個預設分支名稱可以變更。
提示：如果要設定所有新版本庫要使用的初始分支名稱，
提示：請呼叫（會隱藏這個警告）：
提示：
提示： git config --global init.defaultBranch <name>
提示：
提示：除了 "master" 外，常用的分支名稱有 "main", "trunk" 以及
提示："development"。剛建立的分支可以用這個命令重新命名：
提示：
提示： git branch -m <name>
已初始化空的 Git 版本庫於 /Users/wilson/Downloads/project/demo/.git/
```

## 狀態檢查

透過 `git status` 指令來檢查當前 git 追蹤了哪些檔案，哪些檔案尚未追蹤：

```bash
git status

# 輸出結果
位於分支 master

尚無提交

未追蹤的檔案:
  （使用 "git add <檔案>..." 以包含要提交的內容）
 __pycache__/
 demo/
 main.py

提交為空，但是存在尚未追蹤的檔案（使用 "git add" 建立追蹤）
```

## 暫存變更

假設我今天編輯了 main.py 的內容，需要透過版本控制來追蹤檔案，就可以執行 `git add main.py` 指令來保存變更，執行此指令不會回傳任何東西，故需要再執行 `git status` 來確認狀態：

*從 **當前目錄** 複製一份 main.py 到 **暫存區域**。*

```bash
git add main.py
git status

# 輸出結果
位於分支 master

尚無提交

要提交的變更：
  （使用 "git rm --cached <檔案>..." 以取消暫存）
 新檔案：   main.py

未追蹤的檔案:
  （使用 "git add <檔案>..." 以包含要提交的內容）
 __pycache__/
 demo/
```

## 提交變更

將暫存區域中的所有變更提交，透過 `git commit -m "<狀態描述>"` 指令來完成：

*複製一份 **暫存區域** 的內容，存放至 **git 儲存庫** 內。*
*相當於玩遊戲時建立存檔點的概念，當未來需要狀態回朔可隨時切換回來。*

```bash
git commit -m "初始化專案，建立 main.py。"

# 輸出結果
[master (根提交) 57fe6d2] 初始化專案，建立 main.py。
 Committer: wilson <wilson@admindeMacBook-Air.local>
您的姓名和信件位址皆根據您的使用者名稱和主機名稱自動設定。
請檢查是否正確。您可以自行設定，這樣便不會再出現這個提示訊息：

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

設定完畢後，您可以使用下述指令，修正這個提交使用的提交者身份：

    git commit --amend --reset-author

 1 file changed, 13 insertions(+)
 create mode 100644 main.py
```

透過回傳結果可得知，我們在 `master` 成功建立了 `57fe6d2` 節點，提交了 1 個檔案變更、新增 13 行內容。

## 檢視歷史紀錄

若我接著新增或是修改了一段程式碼，並且執行上述流程後，在 `master` 建立了第二個儲存點 `68f4803`，我們可以透過此指令來檢視歷史紀錄：

```bash
git log
```

*此時會進入 (less) 檢視模式，若要離開按下 q 即可。*

```
commit 68f48035a1a5119969085dcaa0519e01c2496ab5 (HEAD -> master)
Author: wilson <wilson@admindeMacBook-Air.local>
Date:   Wed May 8 15:27:02 2024 +0800

    test: 新增 test_main.py。

commit 57fe6d2365cd653dd43ef9b2a9e954f1483677fb
Author: wilson <wilson@admindeMacBook-Air.local>
Date:   Wed May 8 14:54:57 2024 +0800

    初始化專案，建立 main.py。
```

上面記錄了提交編號、提交者、時間以及描述。

## 切換儲存點

假設我發現剛剛新增的程式碼不能執行，想要回朔到前一個儲存點時，我們就會利用 `git reset` 指令，有兩種方式可以達成：

### 相對位置

帶入當前分支名稱 `master`，後面加上 `^` 表示返回前一次儲存點；若要返回前五次則是加上 `^^^^^`。

```bash
git reset master^
```

### 絕對位置

由於我們已經瀏覽過歷史紀錄了，因此也可以直接帶入欲切換的儲存點編號：

```bash
git reset 57fe6d2
```

## 關於 reset 你必須知道的兩件事

1. 在 git 裡面 reset 並不是重置的意思，比較像是 go to 的概念，因此即便你返回前一個儲存點，也不會破壞掉新的那個儲存點。
2. 根據帶入參數的不同，會有不同行為模式，以下圖表擷取自高見龍老師的書籍 [為你自己學 Git](https://gitbook.tw/)。

## 小結

目前為止我們學會了基礎版控流程 - **初始化版本控制、狀態檢查、暫存變更、提交變更、檢視歷史紀錄、切換儲存點**，分別為下列六個指令：

* `git init`
* `git status`
* `git add <file>`
* `git commit -m "<description>"`
* `git log`
* `git reset <commit>`

## 參考文章

* [Git 教學 - Git 書 - 為你自己學 Git | 高見龍](https://gitbook.tw/)

