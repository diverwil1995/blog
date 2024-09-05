---
layout: post
title: "程式碼與牠們的產地(2)：Github Repository"
subtitle: "GitHub 基礎操作概念介紹"
date: 2024-03-15
last_modified_at: 2024-09-03
image: /assets/images/gitrepo-cover.jpg
tags: [Git, GitHub]
---

<!-- # 程式碼與牠們的產地(2)：Github Repository

*作者：diverwil1995*
*發布日期：2024年5月8日*
*最後更新：2024年9月3日* -->

## 目錄
1. [前言](#前言)
2. [版本控制基礎概念](#版本控制基礎概念)
3. [前置準備](#前置準備)
4. [建立遠端通訊位址](#建立遠端通訊位址)
5. [建立連線金鑰](#建立連線金鑰)
6. [上傳程式碼](#上傳程式碼)
7. [忽略不想上傳的檔案](#忽略不想上傳的檔案)
8. [實際操作示例](#實際操作示例)
9. [常見問題 (FAQ)](#常見問題-faq)
10. [安全提醒](#安全提醒)
11. [同場加映 - 如何下載別人的開源專案](#同場加映---如何下載別人的開源專案)
12. [有用資源](#有用資源)
13. [更新日誌](#更新日誌)

## 前言

在軟體開發領域，開源專案扮演著越來越重要的角色。所謂開源專案，是指專案的程式碼是公開可用的，任何人都可以檢視、複製或修改，並由所有參與者共同貢獻和維護。這意味著開源專案的程式碼通常會放在一個對外公開的儲存庫（如 GitHub、GitLab 或 Bitbucket）中進行管理。

本文將重點介紹 GitHub，這是目前最流行的程式碼託管平台之一。我們將討論 GitHub 的基礎操作，包括如何建立 GitHub 連接、上傳程式碼、忽略特定檔案等內容。



![GitHub 儲存庫示意圖](https://media.licdn.com/dms/image/v2/D4D12AQHP7n358_jaTg/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1704361479670?e=2147483647&v=beta&t=yehRTL-qZoQ551wxE19s9z6VeId-RDTMZGLB7l1N2rs)

> 注：本教程基於 2024 年 5 月的 GitHub 介面和 Git 版本 2.35.1。如果您在未來閱讀本文，部分操作可能會有所變化。

## 版本控制基礎概念

在深入 GitHub 操作之前，讓我們先簡單了解一下 Git 的一些基本概念：

1. **儲存庫（Repository）**：存放專案檔案和每個檔案的修訂歷史記錄的地方。

2. **提交（Commit）**：對你的檔案進行的一次修改。Git 保存了這些提交，所以你可以回顧專案的開發歷程。

3. **分支（Branch）**：代表獨立的開發線。主分支通常稱為 "main" 或 "master"。

4. **合併（Merge）**：將一個分支的更改整合到另一個分支中。

5. **推送（Push）**：將本地的提交上傳到遠端儲存庫。

6. **拉取（Pull）**：從遠端儲存庫獲取並整合變更到你的本地儲存庫。

了解這些基本概念將幫助你更好地理解後續的 GitHub 操作。

## 前置準備

在開始之前，請確保您已完成以下步驟：

1. 建立 GitHub 帳號
2. 新增儲存庫（Repository）

如果您還沒有完成這些步驟，請訪問 [GitHub 官網](https://github.com/) 註冊帳號並創建一個新的儲存庫。

## 建立遠端通訊位址

為了讓您的本地儲存庫知道如何連接到 GitHub，需要執行以下命令來建立通訊位址：

```bash
git remote add origin git@github.com:yourusername/your-repo-name.git
```

這個命令不會有任何輸出。要確認是否添加成功，可以使用以下命令查看遠端通訊簿內容：

```bash
git remote -v
```

如果添加成功，您將看到類似下面的輸出：

```
origin  git@github.com:yourusername/your-repo-name.git (fetch)
origin  git@github.com:yourusername/your-repo-name.git (push)
```

## 建立連線金鑰

為了安全地與 GitHub 建立連接，我們需要使用 SSH 金鑰。SSH 金鑰提供了一種安全的方式來驗證您的身份，無需每次都輸入用戶名和密碼。這個過程分為兩個步驟：

1. 在本地生成一對 SSH 金鑰（公鑰和私鑰）
2. 將公鑰部署到 GitHub

### 生成 SSH 金鑰

SSH 金鑰有多種類型，包括 DSA、RSA、ECDSA 和 Ed25519。預設情況下，`ssh-keygen` 會生成 RSA 金鑰，這種金鑰具有最廣泛的系統相容性。但如果您需要更高的安全性，可以使用 Ed25519 類型的金鑰。

> 提示：要生成 Ed25519 類型的金鑰，可以使用命令 `ssh-keygen -t ed25519`。

#### Windows 使用者

Windows 11 及更高版本應該已經內建了 OpenSSH Client。如果您的系統沒有安裝，請參考 [Microsoft 的官方指南](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)。

在命令提示字元中輸入以下命令來生成金鑰對：

```bash
ssh-keygen
```

Windows 預設會在 `%UserProfile%\.ssh` 目錄下生成金鑰。

#### Linux / MacOS 使用者

對於 Linux 和 MacOS 使用者，建議切換到 `~/.ssh` 目錄後再執行命令：

```bash
cd ~/.ssh
ssh-keygen
```

執行後，您將看到類似下面的輸出：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/username/.ssh/id_rsa): demo
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in demo
Your public key has been saved in demo.pub
The key fingerprint is:
SHA256:p+CnqEWcKhql1zGdGuVkxdeIM1Hj41AwkIKXGcsVgA4 username@hostname
The key's randomart image is:
+---[RSA 3072]----+
|   oo=+===+o     |
|E o.=o..=+o..    |
| o .o.+ .+o      |
|  .. B . o .     |
|  . B = S o      |
| o + * . o       |
|+ o + . o        |
|.+ . . o         |
|. ... .          |
+----[SHA256]-----+
```

### 將私鑰添加到 SSH Agent

為了方便使用，我們可以將私鑰添加到 SSH Agent 中。SSH Agent 是一個程序，它可以保存您的私鑰，並在需要時提供給 SSH 客戶端程序。

#### Windows 使用者

```powershell
# 設置 ssh-agent 服務為自動啟動
Get-Service ssh-agent | Set-Service -StartupType Automatic
# 啟動 ssh-agent 服務
Start-Service ssh-agent
# 添加私鑰
ssh-add C:\Users\YourUsername\.ssh\your_private_key
```

#### Linux / MacOS 使用者

```bash
# 啟動 ssh-agent
eval "$(ssh-agent -s)"
# 添加私鑰
ssh-add ~/.ssh/your_private_key
```

### 將公鑰部署到 GitHub

為了確保公鑰的安全性，我們首先要限制其使用權限：

```bash
chmod 400 ~/.ssh/your_public_key.pub
```

然後，複製公鑰的內容。您可以使用以下命令查看公鑰內容：

```bash
cat ~/.ssh/your_public_key.pub
```

複製輸出的內容，然後按照以下步驟將公鑰添加到 GitHub：

1. 登入到您的 GitHub 帳戶
2. 點擊右上角的頭像，選擇 "Settings"
3. 在左側選單中，選擇 "SSH and GPG keys"
4. 點擊 "New SSH key" 或 "Add SSH key"
5. 在 "Title" 欄位中，為您的金鑰添加一個描述性的標籤
6. 將複製的公鑰內容貼到 "Key" 欄位中
7. 點擊 "Add SSH key" 完成添加

![GitHub SSH Key 設置](https://i.imgur.com/example_ssh_key_setup.png)

## 上傳程式碼

在上傳程式碼之前，我們需要注意一個重要的變化。GitHub 現在推薦使用 `main` 作為主分支的名稱，而不是傳統的 `master`。因此，我們需要重新命名本地的主分支：

```bash
git branch -M main
```

這個命令不會有任何輸出。接下來，使用 `push` 命令上傳程式碼：

```bash
git push -u origin main
```

如果一切順利，你會看到類似下面的輸出：

```
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 750 bytes | 750.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:yourusername/your-repo-name.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

完成這步之後，今後只需執行 `git push` 即可上傳程式碼。

## 忽略不想上傳的檔案

在專案開發過程中，有些檔案我們不希望上傳到公開的 GitHub 儲存庫中，比如包含敏感資訊的設定檔、編譯生成的快取檔案等。我們可以透過創建 `.gitignore` 檔案來解決這個問題。

在專案根目錄下創建 `.gitignore` 檔案：

```bash
touch .gitignore
```

然後編輯這個檔案，添加您想忽略的檔案或目錄。例如：

```gitignore
# 忽略 Mac 系統檔案
**/.DS_Store

# 忽略 src 和 output 目錄
**/src/
**/output/

# 忽略所有 Excel 檔案
**/*.xlsx

# 忽略 Python 快取檔案
**/__pycache__
**/.pytest_cache

# 忽略虛擬環境目錄
**/demo-venv
```

- `*.xlsx` 表示忽略所有 Excel 檔案
- `**/` 表示該目錄下的所有檔案都要忽略

## 實際操作示例

讓我們通過一個簡單的實例來演示整個流程，從創建本地專案到推送到 GitHub：

1. 創建一個新的目錄並初始化 Git 儲存庫：

```bash
mkdir my-first-repo
cd my-first-repo
git init
```

2. 創建一個簡單的 README 文件：

```bash
echo "# My First GitHub Project" > README.md
```

3. 將文件添加到 Git 並提交：

```bash
git add README.md
git commit -m "Initial commit: Add README"
```

4. 在 GitHub 上創建一個新的儲存庫（不要初始化 README）

5. 將本地儲存庫連接到 GitHub：

```bash
git remote add origin git@github.com:yourusername/my-first-repo.git
```

6. 推送到 GitHub：

```bash
git branch -M main
git push -u origin main
```

現在，您應該能在 GitHub 上看到您的專案了！

## 常見問題 (FAQ)

1. Q: 為什麼我無法推送到 GitHub？
   A: 確保您已正確設置 SSH 金鑰，並且有該儲存庫的寫入權限。也可能是因為遠端儲存庫有您本地沒有的更改，試試先執行 `git pull` 再推送。

2. Q: 如何解決 "git push" 時的衝突？
   A: 首先使用 `git pull` 獲取最新更改，解決衝突後再次嘗試推送。解決衝突可能需要手動編輯衝突的檔案。

3. Q: 如何撤銷最後一次提交？
   A: 使用 `git reset HEAD~1` 撤銷最後一次提交，但保留更改。如果想完全刪除最後一次提交，使用 `git reset --hard HEAD~1`。

4. Q: 我不小心提交了敏感資訊，如何刪除它？
   A: 使用 `git filter-branch` 或 BFG Repo-Cleaner 工具來從 Git 歷史中移除敏感資訊。然後，強制推送更新後的歷史到 GitHub。但請注意，這會改變 Git 歷史，可能會影響其他協作者。

5. Q: 如何處理 "fatal: refusing to merge unrelated histories" 錯誤？
   A: 這通常發生在嘗試合併兩個沒有共同歷史的儲存庫時。可以使用 `git pull origin main --allow-unrelated-histories` 來強制合併。

6. Q: 如何更改最後一次提交的訊息？
   A: 使用 `git commit --amend` 命令。如果已經推送到 GitHub，則需要使用 `git push --force` 來更新遠端儲存庫。

## 安全提醒

在使用 GitHub 時，請牢記以下安全注意事項：

1. **保護您的私鑰**：您的 SSH 私鑰是非常敏感的資訊。永遠不要分享您的私鑰或將其上傳到公開的地方。確保將私鑰保存在安全的位置，並考慮使用密碼短語來加強保護。

2. **謹慎處理敏感資訊**：避免將密碼、API 金鑰或其他敏感資訊直接寫入代碼中。考慮使用環境變數或專門的密鑰管理服務。

3. **定期更新**：保持您的 Git 客戶端和相關工具的最新版本，以獲得最新的安全修復。

4. **使用雙因素認證**：在您的 GitHub 帳戶上啟用雙因素認證，增加一層額外的安全保護。

5. **審查第三方應用**：在授予第三方應用訪問您 GitHub 帳戶的權限時要小心謹慎。定期審查並撤銷不再需要的訪問權限。

## 同場加映 - 如何下載別人的開源專案？

1. 在 GitHub 上找到您想下載的專案。
2. 點擊 "Code" 按鈕，選擇 HTTPS、SSH 或 GitHub CLI 中的一種連接方式。
3. 複製顯示的 URL。
4. 在本地端新增空的資料夾，並在該資料夾中打開終端機。
5. 執行以下指令：

```bash
git clone <你複製的 URL>
```

這樣就把開源專案複製一份到本地端了！

## 有用資源

以下是一些能夠幫助您深入了解 Git 和 GitHub 的資源：

1. [Git 官方文檔](https://git-scm.com/doc)
2. [GitHub 文檔](https://docs.github.com/en)
3. [Git 教學 - 廖雪峰的官方網站](https://www.liaoxuefeng.com/wiki/896043488029600)
4. [GitHub 技能課程](https://skills.github.com/)
5. [Pro Git 書籍](https://git-scm.com/book/zh/v2)

## 更新日誌

- 2024-05-08: 初始版本發布
- 2024-09-03: 
  - 添加了版本控制基礎概念部分
  - 擴充了常見問題 (FAQ) 部分
  - 新增了安全提醒部分
  - 添加了有用資源列表
  - 改進了整體結構，添加了目錄

## 練習挑戰

為了鞏固所學知識，試試以下的小挑戰：

1. 創建一個新的 GitHub 儲存庫，並將本地的一個專案推送到這個儲存庫。
2. 嘗試創建一個新的分支，在其中進行一些更改，然後將這些更改合併到主分支。
3. 與朋友合作，嘗試克隆對方的儲存庫，進行一些更改，然後創建一個拉取請求（Pull Request）。

完成這些挑戰將幫助您更好地理解 Git 和 GitHub 的工作流程。

希望這個教程對您有所幫助。如果您有任何問題或需要進一步的說明，歡迎隨時提問。祝您在 GitHub 上的開發之旅順利愉快！

[返回首頁]({{ site.baseurl }}/)