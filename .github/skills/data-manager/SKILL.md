---
name: data-manager
description: 提供通用的 JSON 資料集管理（CRUD）工具，支援任意遵循專案目錄結構的資料集（例如 news.json、restaurants.json 等）；會自動辨識專案名稱、定位檔案、提示欄位、驗證格式，並協助建立或檢查關聯的 data/ 與 image/ 資源。
metadata:
  author: OpenData
  version: "1.1"
  triggers: "新增資料到, 修改 的 id, 刪除 的 id"
  supported_patterns: "**/*.json"
---

# Data Manager Skill

遵循 .github/copilot-instructions.md 的規範，本 skill 適用於工作區內任何專案下的 JSON 資料集。

## Summary
- 自動辨識專案名稱與目標 `*.json` 檔案
- 智慧欄位提示與 JSON schema 推斷
- 資料驗證（id 唯一性、日期格式、布林值、關聯檔案存在性）
- 可選自動建立關聯的 `data/` HTML 檔案

## Examples
- 幫我在 KKHCleanBus 的 news.json 新增一筆公告
- 幫我在 TainanFood 的 restaurants.json 新增一筆餐廳資料
- 把 KKHCleanBus/news.json 的 id 2 的 enabled 改成 false

## Usage
1. 確保 `SKILL.md` 位於 `.github/skills/data-manager/SKILL.md`。
2. 使用 `skills-ref validate .github/skills/data-manager` 驗證 frontmatter。
3. 在 VS Code 中重新載入視窗（Command Palette -> Developer: Reload Window）。
4. 在 Copilot Chat 中貼上 Examples 中的句子以驗證是否觸發。

## 注意（依 .github/copilot-instructions.md）
- 根據規範，新增/修改 JSON 時應先辨識專案名稱，並將 JSON 放在專案根目錄（`{ProjectName}/`），詳細說明內容應放在 `{ProjectName}/data/` 中的 HTML 片段。
- 建立 HTML 時請只輸出片段，不要包含 `<html>`、`<head>` 或 `<body>`，也不要在 HTML 片段中包含標題或發布時間（這些由 JSON 提供）。

## Test Sentences
- 請幫我在 KKHCleanBus 的 news.json 新增一筆公告
- 幫我在 TainanFood 的 restaurants.json 新增一筆餐廳資料
- 把 KKHCleanBus/news.json 的 id 2 的 enabled 改成 false

## Timezone requirement

- 所有時間類欄位（例如 `createdDate`、`startDate`、`endDate` 等）**必須使用 ISO 8601 格式並且時區為 +08:00**。
- 範例格式：`2026-01-20T08:00:00.0000000+08:00`
- 在執行新增或修改時，skill 會自動驗證並提示轉換或修正時區；若使用者提供無時區的日期，skill 將建議並自動補上 `+08:00`（需使用者確認）。
