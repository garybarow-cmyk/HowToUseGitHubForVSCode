# 07 - Git 常用指令速查與常見問題 (Cheat Sheet & FAQ)

本篇章節整理了開發者在日常工作中最頻繁使用的 Git 指令分類速查表，以及新手與資深工程師在協同開發時最常碰到的疑難排解 (FAQ)。

---

## 目錄
1. [Git 常用指令分類速查表 (Cheat Sheet)](#1-git-常用指令分類速查表-cheat-sheet)
   - [儲存庫初始化與狀態檢視](#11-儲存庫初始化與狀態檢視)
   - [暫存與提交操作](#12-暫存與提交操作)
   - [分支與合併操作](#13-分支與合併操作)
   - [遠端同步操作 (GitHub)](#14-遠端同步操作-github)
   - [暫存區暫存 (Stash) 與復原操作](#15-暫存區暫存-stash-與復原操作)
2. [常見問題與疑難排解 (FAQ)](#2-常見問題與疑難排解-faq)
   - [Q1: 推送時出現 `error: failed to push some refs (fetch first)`？](#q1-推送時出現-error-failed-to-push-some-refs-fetch-first)
   - [Q2: 不小心把含有 API Key / 密碼的檔案 Commit 甚至 Push 了怎麼辦？](#q2-不小心把含有-api-key--密碼的檔案-commit-甚至-push-了怎麼辦)
   - [Q3: 換行符號一直跳警告 `warning: LF will be replaced by CRLF`？](#q3-換行符號一直跳警告-warning-lf-will-be-replaced-by-crlf)
   - [Q4: 出現 `Permission to user/repo denied` 權限錯誤？](#q4-出現-permission-to-userrepo-denied-權限錯誤)
   - [Q5: 上一次的 Commit 訊息打錯字了，如何修改？](#q5-上一次的-commit-訊息打錯字了如何修改)
   - [Q6: 終端機顯示 `You are in 'detached HEAD' state` 是什麼意思？](#q6-終端機顯示-you-are-in-detached-head-state-是什麼意思)

---

## 1. Git 常用指令分類速查表 (Cheat Sheet)

### 1.1 儲存庫初始化與狀態檢視
```bash
git init                         # 在當前資料夾初始化新的 Git 儲存庫
git status                       # 查看目前檔案變更狀態（紅字：未暫存；綠字：已暫存）
git log --oneline --graph --all  # 以視覺化單行樹狀圖檢視所有分支提交歷史
git diff                         # 檢視工作目錄中「尚未暫存」的檔案具體修改內容
git diff --staged                # 檢視「已暫存」準備提交的具體修改內容
```

### 1.2 暫存與提交操作
```bash
git add <檔案名稱>               # 將指定檔案加入暫存區
git add .                        # 將當前目錄下所有新增與變更加入暫存區
git commit -m "feat: 提交訊息"   # 提交暫存區檔案並附上說明
git commit -am "fix: 提交訊息"   # 快捷指令：針對已追蹤檔案同時完成 add 與 commit
git commit --amend               # 修改上一次提交的內容或訊息（僅限尚未 push 前使用）
```

### 1.3 分支與合併操作
```bash
git branch                       # 列出所有本機分支（目前所在分支前有 * 號）
git branch -r                    # 列出所有遠端 GitHub 分支
git branch <新分支名>            # 建立新分支
git switch <分支名>              # 切換分支（等同於 git checkout <分支名>）
git switch -c <新分支名>         # 建立並直接切換至新分支（等同於 git checkout -b）
git merge <目標分支名>           # 將目標分支的修改合併進當前所在分支
git branch -d <分支名>           # 刪除已合併的本機分支
git branch -D <分支名>           # 強制刪除尚未合併的本機分支（謹慎使用）
```

### 1.4 遠端同步操作 (GitHub)
```bash
git remote -v                    # 檢視目前綁定的遠端倉庫清單與 URL
git remote add origin <URL>      # 新增遠端倉庫並命名為 origin
git fetch origin                 # 下載遠端最新進度但不自動合併到當前檔案
git pull origin <分支名>         # 下載遠端進度並直接與本地分支合併 (fetch + merge)
git pull --rebase origin <分支名># 採用 rebase 方式拉取，保持線性的乾淨提交紀錄
git push origin <分支名>         # 將本機分支推送到 GitHub
git push -u origin <分支名>      # 首次推送並綁定為預設追蹤分支
git push origin --delete <分支名># 刪除 GitHub 上的遠端分支
```

### 1.5 暫存區暫存 (Stash) 與復原操作
```bash
# 暫存臨時工作
git stash save "說明文字"        # 把尚未 commit 的修改打包暫存起來，還原乾淨工作目錄
git stash list                   # 查看目前儲存了哪些 stash
git stash pop                    # 取出最近一次暫存的工作內容並繼續開發
git stash drop                   # 捨棄最近一次的暫存

# 放棄與復原 (請小心使用)
git restore <檔案名稱>           # 放棄工作目錄中單一檔案的修改（回復成上次 commit 狀態）
git restore .                    # 放棄當前目錄下所有未暫存的修改
git restore --staged <檔案名稱>  # 將已暫存檔案移回未暫存 (Unstage)
git reset --soft HEAD~1          # 撤銷最後一次 commit，但保留修改內容於暫存區
git reset --hard HEAD~1          # ⚠️ 危險：完全刪除最後一次 commit 及其所有代碼
```

---

## 2. 常見問題與疑難排解 (FAQ)

### Q1: 推送時出現 `error: failed to push some refs (fetch first)`？
- **原因**：GitHub 遠端儲存庫包含了您本機尚未同步的新提交（例如：曾直接在 GitHub 網頁上修改了 README、或是同事已推送新代碼）。
- **解法**：先拉取遠端最新變更再推送：
  ```bash
  # 方式 1（推薦）：使用 rebase 保持乾淨歷史
  git pull --rebase origin main
  git push origin main

  # 方式 2：使用一般 merge
  git pull origin main
  git push origin main
  ```

---

### Q2: 不小心把含有 API Key / 密碼的檔案 Commit 甚至 Push 了怎麼辦？

#### 情境 A：尚未 push 到 GitHub（僅在本機 commit）
1. 立即從程式碼中移除敏感金鑰。
2. 將該檔案路徑加入 `.gitignore`。
3. 執行軟重置復原最後一次 commit：
   ```bash
   git reset --soft HEAD~1
   git add .
   git commit -m "feat: 重新提交並排除敏感檔案"
   ```

#### 情境 B：已經 push 到公開 GitHub 儲存庫
> [!CAUTION]
> **最高層級資安警報**：
> 1. **第一優先動作**：立即前往服務後台（例如 Google Cloud / AWS / OpenAI / Stripe）**將外洩的金鑰廢除 (Revoke) 並重新生成**！
> 2. **切勿心存僥倖**：公開倉庫推播後，公網上的自動化爬蟲機器人會在 **數秒內** 抓取歷史記錄並拿去盜刷。即使刪除該 commit 或檔案，歷史快照依然可能已被鏡像備份。
> 3. 清理歷史紀錄可使用專門工具（如 `git-filter-repo` 或 BFG Repo-Cleaner）。

---

### Q3: 換行符號一直跳警告 `warning: LF will be replaced by CRLF`？
- **原因**：Windows 系統預設換行字元為 Carriage Return + Line Feed (`\r\n`)，而 Linux / macOS 為 Line Feed (`\n`)。
- **解法**：在終端機執行全域設定，讓 Git 自動進行轉換：
  ```bash
  git config --global core.autocrlf true
  ```
  設定完成後，Git 就會在檢出檔案時轉為 Windows CRLF，在提交時轉為 Linux LF，警告便不會影響開發。

---

### Q4: 出現 `Permission to user/repo denied` 權限錯誤？
- **原因**：目前電腦記錄的 GitHub 憑證沒有該儲存庫的寫入權限，或登入成了另一個 GitHub 帳號。
- **解法**：
  1. **Windows 憑證管理員重置**：
     - 按鍵盤 <kbd>Win</kbd> 鍵，搜尋並開啟 **認證管理員 (Credential Manager)**。
     - 點選 **Windows 認證 (Windows Credentials)**。
     - 在「一般認證」清單中找到 `git:https://github.com`。
     - 點擊展開並選擇 **移除 (Remove)**。
  2. 再次於終端機執行 `git push`，系統便會重新跳出瀏覽器登入視窗，此時登入具有該專案權限的正確認證帳號即可。

---

### Q5: 上一次的 Commit 訊息打錯字了，如何修改？
- **若尚未 push 到遠端**：
  直接執行：
  ```bash
  git commit --amend -m "fix: 正確的提交說明"
  ```
- **若已經 push 到遠端**：
  修改後需要強制推送 (`git push --force-with-lease`)。**注意**：若為多人協作分支，修改已公開的歷史會影響團隊，請盡量避免。

---

### Q6: 終端機顯示 `You are in 'detached HEAD' state` 是什麼意思？
- **原因**：您使用 `git checkout <commit_id>` 切換到某個具體的提交節點，而不是在某個分支名稱上。此時所做的任何提交都不屬於任何分支，切換分支後容易遺失。
- **解法**：
  若想以該節點為基礎繼續開發，請直接在此狀態下建立新分支：
  ```bash
  git switch -c new-feature-branch
  ```
  若只是想回去主要分支，直接切換回去即可：
  ```bash
  git switch main
  ```
