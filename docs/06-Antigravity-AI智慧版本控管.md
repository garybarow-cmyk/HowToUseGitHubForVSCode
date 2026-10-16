# 06 - Antigravity AI 智慧版本控管指南 (AI-Assisted Git Workflows)

**Google Antigravity** 不僅是一套專為 AI-First 開發打造的先進 IDE / Agent 系統，更將版本控管（Git / GitHub）從「手動執行指令」提升為「AI 協作副駕駛」的全新層次。

本篇章節說明如何利用 Antigravity 的 AI 能力，自動化完成 Commit 訊息撰寫、Pre-commit 代碼審查、語意衝突解決與 PR 產出。

---

## 目錄
1. [Antigravity 在版本控管中的角色定位](#1-antigravity-在版本控管中的角色定位)
2. [實戰工作流 1：AI 一鍵產生 Conventional Commits 訊息](#2-實戰工作流-1ai-一鍵產生-conventional-commits-訊息)
3. [實戰工作流 2：Pre-commit 智慧代碼審查 (Diff Review)](#3-實戰工作流-2pre-commit-智慧代碼審查-diff-review)
4. [實戰工作流 3：用自然語言直接操控 Git](#4-實戰工作流-3用自然語言直接操控-git)
5. [實戰工作流 4：AI 語意分析輔助解決衝突 (Conflict Resolution)](#5-實戰工作流-4ai-語意分析輔助解決衝突-conflict-resolution)
6. [實戰工作流 5：自動生成 Pull Request 說明與 Release Notes](#6-實戰工作流-5自動生成-pull-request-說明與-release-notes)
7. [實戰工作流 6：配置專案安全守門員規則 (.agents/rules)](#7-實戰工作流-6配置專案安全守門員規則-agentsrules)

---

## 1. Antigravity 在版本控管中的角色定位

傳統 Git 工作流往往存在以下痛點：
- 寫完代碼後，不知道怎麼寫出語意精確的 Commit 訊息，最後常常充斥著 `update`、`fix bug` 等低品質紀錄。
- Commit 前沒有仔細檢查 Diff，不小心把測試用的 `console.log`、暫存檔案或敏感金鑰提交出去。
- 合併時遇到複雜的程式碼衝突，因不理解對方的呼叫邏輯而選錯分支，導致功能壞掉。

**Antigravity 透過以下能力解決上述問題：**
- **上下文感知 (Context-Awareness)**：完整理解您當前工作目錄的變更歷史、相關檔案與函式呼叫關聯。
- **Agent 工具呼叫**：Agent 可以直接替您執行終端機指令（檢查狀態、切換分支、建構測試）。
- **可自訂的客製化規則 (Custom Rules)**：設定專屬的資安守則與專案標準。

---

## 2. 實戰工作流 1：AI 一鍵產生 Conventional Commits 訊息

當您將檔案透過 VS Code 或 `git add` 加入暫存區後：

### 操作提示詞 (Prompt)：
> 「請檢查我目前的 Staged 變更，依據 Conventional Commits 規範，幫我產生一則專業、繁體中文的 Commit Message。」

### Antigravity 輸出範例：
```markdown
feat(auth): 實作 Google 第三方登入與 JWT 驗證機制

- 建立 AuthService 處理 OAuth2 回傳之授權碼
- 新增 Token 刷新中介軟體 (TokenRefreshMiddleware)
- 補齊錯誤攔截器以避免憑證過期時頁面崩潰
- 相關 Issue: #42
```

> **規範說明**：
> - `feat:` 新增功能
> - `fix:` 修復問題
> - `docs:` 修改文件
> - `refactor:` 程式碼重構（不影響外部行為）
> - `chore:` 依賴套件更新或建置配置調整

---

## 3. 實戰工作流 2：Pre-commit 智慧代碼審查 (Diff Review)

在執行 `git push` 前，讓 Antigravity 當您的第一個 Reviewer，能把 80% 的疏忽攔截在本機：

### 操作提示詞 (Prompt)：
> 「請幫我檢視這次的 git diff，仔細檢查是否有：
> 1. 潛在的未處理例外或空值錯誤 (Null Pointer)
> 2. 殘留的 console.log / print / debug 測試代碼
> 3. 是否有寫死 (hardcoded) 的連線字串或 API Token」

### Agent 審查範例回饋：
```markdown
⚠️ 審查發現 2 處潛在問題：
1. `src/api/client.js:15`：發現遺留的 `console.log(response.data)`，建議移除。
2. `src/utils/auth.js:42`：`user.profile.name` 未進行可選鏈運算 (?.)，若使用者無 profile 資料可能導致頁面拋出例外崩潰。

是否需要我為您自動修復以上兩點？
```

---

## 4. 實戰工作流 3：用自然語言直接操控 Git

不需要記憶冷門的指令參數，只要在對話視窗清楚表達您的意圖，Antigravity Agent 即可自動轉換並執行：

| 您的需求情境 | 直接對 Antigravity 說 | Agent 在背後執行的動作 |
| :--- | :--- | :--- |
| **開展新任務** | 「幫我開一個 `feature/user-avatar` 分支並切過去」 | `git switch -c feature/user-avatar` |
| **臨時插單處理** | 「我現在要修急件，幫我把目前做到一半的改動暫存起來」 | `git stash save "WIP: avatar upload"` |
| **恢復工作進度** | 「急件修好了，把剛剛暫存的工作還原出來」 | `git stash pop` |
| **捨棄錯誤修改** | 「我不小心改壞了 `config.js`，幫我復原回上次提交」 | `git restore config.js` |
| **檢視專案概況** | 「幫我看目前有哪些檔案還沒追蹤，是否有應該放進 .gitignore 的日誌檔」 | 檢視 `git status` 並建議更新 `.gitignore` |

---

## 5. 實戰工作流 4：AI 語意分析輔助解決衝突 (Conflict Resolution)

面對雙方皆進行了大量重構的複雜衝突，單純靠肉眼選擇 Accept Current 或 Incoming 很容易造成程式破裂。

### 解決步驟：
1. 開啟衝突檔案。
2. 選取帶有 `<<<<<<< HEAD` 到 `>>>>>>>` 的衝突區塊。
3. 對 Antigravity 提問：
   > 「這個檔案在合併時發生了衝突。請幫我分析 Current (HEAD) 與 Incoming 分支各別實作了什麼業務邏輯，並產出正確融合雙方改動的修復代碼。」
4. Antigravity 會理解雙方變更的意圖（例如：A 分支改寫了非同步語法，B 分支新增了欄位驗證），並直接輸出邏輯完整的合併成果。

---

## 6. 實戰工作流 5：自動生成 Pull Request 說明與 Release Notes

當分支開發完畢準備推上 GitHub 提出 PR (Pull Request) 時：

### 操作提示詞 (Prompt)：
> 「比對本分支相較於 `origin/main` 的所有 Commit 與變更，幫我產生一份結構完整的 GitHub PR 說明文案。」

### Antigravity 自動產出的 PR 模板：
```markdown
## 變更概述 (Overview)
本 PR 主要實作了使用者大頭貼上傳與裁切功能，改善使用者設定頁面的互動體驗。

## 重大變更 (Key Changes)
- 導入 ImageCropper 元件，支援前端圖片預覽與等比裁切。
- 建立 `/api/user/avatar` 路由，支援 Multipart 檔案串流上傳至 S3。
- 優化圖檔快取策略，加入 CDN 快取控制標頭。

## 關聯項目 (Related Issues)
- 解決 #58

## 測試與驗證 (Test Plan)
- [x] 上傳超過 5MB 圖檔時正確顯示大小限制警告
- [x] 支援 PNG / JPG 格式，上傳非圖片檔案時阻擋
- [x] 單元測試通過 (`npm run test:unit`)
```

---

## 7. 實戰工作流 6：配置專案安全守門員規則 (.agents/rules)

Antigravity 具備讀取專案專屬規範的能力。透過在專案中建立規則檔，能讓 AI 在協助操作時自動遵循團隊資安準則。

本專案已為您配置好 [.agents/rules/git-workflow.md](file:///c:/CustomApp/HowToUseGitHubForVSCode/.agents/rules/git-workflow.md)：

```markdown
# Git & GitHub 版本控管規範 (Antigravity Rules)

1. Commit 訊息規範 (Conventional Commits)
   每次提交必須使用 Conventional Commits 規範，且使用繁體中文撰寫。

2. 安全守門規範
   - 執行任何 git add 或 git commit 之前，務必檢查是否有敏感檔案（如 .env, API Key 等）。
   - 禁止將未經 .gitignore 排除的敏感檔案提交至儲存庫。

3. 分支策略
   - main 分支保持隨時可發布的穩定狀態。
   - 新功能開發請建立 feature/<功能名稱> 分支。
   - 錯誤修復請建立 fix/<問題名稱> 分支。
```

> 當此規則建立後，只要您要求 Antigravity「幫我提交所有檔案」，Agent 就會主動先檢查暫存清單中是否有機密金鑰，若有會立即警告並阻止危險操作！

---

### 下一步
- 請閱讀 [07 - Git 常用指令速查與常見問題](07-Git常用指令速查與常見問題.md)，掌握日常必備指令清單與急救排除技巧！
