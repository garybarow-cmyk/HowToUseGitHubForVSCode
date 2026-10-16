# 05 - VS Code 視覺化操作指南 (Source Control GUI)

Visual Studio Code 內建了業界最優秀的圖形化 Git 控制介面，讓開發者不用在終端機輸入繁複指令，即可透過滑鼠與直覺的視覺化視窗完成版本控管。

---

## 目錄
1. [開啟「原始檔控制 (Source Control)」面板](#1-開啟原始檔控制-source-control-面板)
2. [檔案狀態代號一覽 (U, M, D, A, C)](#2-檔案狀態代號一覽-u-m-d-a-c)
3. [視覺化 Diff 檢視與局部暫存 (Stage Selected Ranges)](#3-視覺化-diff-檢視與局部暫存-stage-selected-ranges)
4. [日常五大高頻操作：暫存、放棄、提交、還原、同步](#4-日常五大高頻操作暫存放棄提交還原同步)
5. [神級 VS Code Git 擴充套件推薦](#5-神級-vs-code-git-擴充套件推薦)

---

## 1. 開啟「原始檔控制 (Source Control)」面板

- **快捷鍵**：按下鍵盤上的 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>G</kbd>（macOS 為 <kbd>⌃</kbd> + <kbd>⇧</kbd> + <kbd>G</kbd>）。
- **側邊欄圖示**：點擊左側活動列的「分支節點」圖示（三顆圓點連成樹狀分支）。

![VS Code 原始檔控制面板架構]
- **頂部工具列**：檢視檢視模式、重新整理、Commit、更多動作 (`...`)。
- **Commit Message 輸入框**：輸入提交訊息。
- **Staged Changes (暫存的變更)**：等同於已執行 `git add` 的檔案。
- **Changes (變更)**：已修改但尚未暫存的檔案。

---

## 2. 檔案狀態代號一覽 (U, M, D, A, C)

在檔案清單旁，VS Code 會用不同顏色與單一英文字母標記檔案的 Git 狀態：

| 字母代號 | 英文全名 | 意義說明 | 代表顏色 |
| :---: | :--- | :--- | :--- |
| **U** | Untracked | 未追蹤的新檔案（Git 尚未記錄過） | 綠色 |
| **M** | Modified | 已修改的既有檔案 | 黃色 / 橘色 |
| **A** | Added / Staged | 已加入暫存區的新增檔案 | 綠色 |
| **D** | Deleted | 檔案已被刪除 | 紅色（且有刪除線） |
| **C** | Conflict | 合併衝突待解決 | 紅色（通常帶驚嘆號） |
| **R** | Renamed | 檔案被重新命名或移動位置 | 藍色 |

---

## 3. 視覺化 Diff 檢視與局部暫存 (Stage Selected Ranges)

點擊「Changes」中的任一檔案，編輯器會開啟強大的 **對比檢視 (Diff View)**：

- **左側欄 (紅色背景)**：修改前（上一版）的代碼。
- **右側欄 (綠色背景)**：目前修改後的代碼。
- **迷你地圖 (Minimap)**：右側捲軸會用色塊標示變更發生的確切位置。

### 局部暫存技巧 (Stage Selected Ranges)
您是否曾遇到「在同一個檔案裡改了兩件事，但只想先提交其中一件事」的情況？
1. 在 Diff 視窗中，用滑鼠反白選取您想先提交的特定幾行代碼。
2. 按下滑鼠右鍵，選擇 **Stage Selected Ranges (暫存選取的範圍)**。
3. 這樣就只會把選取的行數移至暫存區，剩下的修改依然保留在 Changes 中，無需動用複雜的命令列！

---

## 4. 日常五大高頻操作：暫存、放棄、提交、還原、同步

### 操作 1：暫存變更 (Stage)
- **單一檔案暫存**：滑鼠懸停在檔案名稱旁，點擊 **`+` (Stage Changes)**。
- **全部暫存**：點擊「Changes」分類標題右側的 **`+`** 圖示（等同 `git add .`）。

### 操作 2：取消暫存 (Unstage)
- 若不小心把不想提交的檔案加進了暫存區，懸停在該檔案旁點擊 **`-` (Unstage Changes)** 即可（等同 `git restore --staged`）。

### 操作 3：放棄未儲存修改 (Discard Changes)
- 若寫壞了想完全捨棄工作目錄的修改，滑鼠懸停在檔案旁，點擊**迴旋箭頭圖示 (Discard Changes)**（等同 `git restore <file>`）。
- ⚠️ **注意**：這會永久清除該檔案未提交的改動，請確認無誤再點擊。

### 操作 4：提交變更 (Commit)
1. 在文字框中輸入簡潔明瞭的提交訊息（例如：`fix: 修復登入按鈕點擊無反應問題`）。
2. 點擊頂部的藍色 **Commit** 按鈕，或按下快捷鍵 <kbd>Ctrl</kbd> + <kbd>Enter</kbd>。

### 操作 5：同步至 GitHub (Sync / Push / Pull)
- 提交完成後，按鈕會變成 **Sync Changes (同步變更)**，旁邊會標註要推 (Push) 幾筆或要拉 (Pull) 幾筆。
- 點擊按鈕，VS Code 會在背景完成安全推播至 GitHub 遠端儲存庫。

---

## 5. 神級 VS Code Git 擴充套件推薦

按 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd> 開啟擴充套件市集，強烈建議安裝以下三個神器：

### 5.1 GitHub Pull Requests and Issues (官方必裝)
- **功能**：
  - 直接在 VS Code 側邊欄建立、審核與合併 GitHub Pull Request (PR)。
  - 在代碼中直接檢視團隊夥伴留下的 Review 評論，不必在網頁與編輯器間切換。
  - 在 commit 訊息中直接以 `#issue_number` 關聯並自動關閉 GitHub Issue。

### 5.2 GitLens — Git supercharged
- **功能**：
  - **行內責難 (Current Line Blame)**：游標停留在任何一行代碼，行尾會淡淡顯示「這行是誰在幾個月前寫的、commit 訊息是什麼」。
  - **歷史時光機 (File History)**：點擊即可逐筆檢視該檔案在歷史上的所有變遷。
  - **視覺化分支比較**：輕鬆比對兩個分支之間的差異。

### 5.3 Git Graph
- **功能**：
  - 以色彩斑斕的樹狀地圖呈現所有 Commit、Branch 與 Merge 軌跡。
  - 支援點擊任一節點檢視當時的代碼快照與比對。
  - 右鍵點擊分支節點即可快速 Checkout、Merge、Rebase 或 Tag。

---

### 下一步
- 熟悉圖形化操作後，請閱讀 [06 - Antigravity AI 智慧版本控管指南](06-Antigravity-AI智慧版本控管.md)，探索如何運用 AI Agent 全面升級您的 Git 工作流！
