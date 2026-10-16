# Git & GitHub 版本控管規範 (Antigravity Rules)

本規則定義了在目前專案中使用 Antigravity 進行版本控管時應遵循的準則：

## 1. Commit 訊息規範 (Conventional Commits)
每次提交必須使用 Conventional Commits 規範，且使用繁體中文撰寫，格式如下：
- `feat`: 新增功能 (例如: `feat: 新增使用者登入頁面`)
- `fix`: 修復錯誤 (例如: `fix: 修復購物車金額計算異常`)
- `docs`: 文件更動 (例如: `docs: 更新 README 安裝說明`)
- `style`: 程式碼格式調整 (不影響邏輯)
- `refactor`: 程式碼重構 (既非新增功能亦非修復錯誤)
- `test`: 新增或修改測試
- `chore`: 建置流程或輔助工具變動

## 2. 安全守門規範
- 在執行任何提交 (`git commit`) 或推送 (`git push`) 前，務必檢查是否有敏感檔案（如 `.env`、API Key、Token 或憑證檔）。
- 禁止將未經 `.gitignore` 排除的敏感檔案提交至儲存庫。

## 3. 分支策略
- `main` 分支保持隨時可發布的穩定狀態。
- 新功能開發請建立 `feature/<功能名稱>` 分支。
- 錯誤修復請建立 `fix/<問題名稱>` 分支。
