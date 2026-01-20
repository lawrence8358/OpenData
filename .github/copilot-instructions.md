---
applyTo: '**/*.json, **/*.html'
description: 規範公開服務專案的結構、HTML 視覺風格(Inline CSS)、語氣，並整合 Skill 功能調用策略。
---

# 公開服務數據與說明內容規範

## 1. 核心原則：動態專案結構
**僅在**處理本儲存庫的公開服務數據（JSON/HTML）時適用此結構。
請先辨識當前上下文的**「專案名稱」**（例如：`KKHCleanBus`、`MyWeatherApp`），並依此決定根目錄。

**⚠️ 注意：不要預設使用 `KKHCleanBus`，請依據使用者指令動態調整。**

## 2. 檔案與目錄結構規範 (In-Scope)
假設當前專案為 `{ProjectName}`，檔案結構必須嚴格遵守：

1.  **API 資料 (`*.json`)**：
    * **位置**：`{ProjectName}/*.json`
    * **職責**：定義元數據（標題、日期、ID 等）。HTML 檔名應記錄於此。
2.  **說明頁面 (`data/*.html`)**：
    * **位置**：`{ProjectName}/data/*.html`
    * **職責**：僅包含內文與排版。
3.  **圖片資源 (`image/*`)**：
    * **位置**：`{ProjectName}/image/*`

## 3. HTML 內容與風格規範 (Visual & Tone)
生成的 HTML 必須是 **HTML 片段 (Fragment)**，並具備**高度視覺質感**與**人性化語氣**。

### A. 結構限制
* **❌ 禁止**：`<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`, `<h1>`(標題由 JSON 控制)。
* **✅ 允許**：`<div>`, `<p>`, `<img>`, `<hr>`, `<strong>`, `<ul>`, `<style>`, `<span>`。

### B. 視覺樣式 (Inline CSS)
請**適度且精準**地使用 Inline CSS，不要過度渲染：
* **圖片 (Image)**：
    * **路徑**：必須使用相對路徑 `../image/`。
    * **樣式**：維持高質感，強制加入圓角與陰影：`border-radius: 12px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); max-width: 100%;`。
    * **風格參考**：若上下文包含現有圖片資源，請參考既有圖片的風格進行生成或挑選。
* **重點強調 (Emphasis)**：
    * **僅在必要時**使用顏色標記重點，例如 `<span style="color: #c0392b;">` (警示) 或 `<span style="color: #2980b9;">` (連結/資訊)。
    * 一般內文請保持乾淨，不要對每個標籤都加上 style。

### C. 文案語氣 (Tone)
* **拒絕機器人**：不要寫成生硬的說明書。
* **充滿人味**：語氣需誠懇、具同理心（如：「這也是我最掛心的」、「拼命趕工」）。
* **活潑**：在不失專業的前提下，使用更貼近使用者的口語。

## 4. Skill 與工具整合策略 (Skill Integration)
在回應前，請先評估是否符合特定 **Skill (擴充功能/工具)** 的功能範圍：

1.  **情境感知 (Context Awareness)**：
    * 若請求涉及修改現有資料（如新增一筆 news），請先透過 **Workspace Search** 或讀取當前 JSON 檔案，確保新資料的 Schema 與既有格式完全一致。
2.  **Skill 優先 (Skill First)**：
    * 若使用者的指令符合已安裝 Skill 的觸發條件（例如：特定指令、部署動作、資料庫查詢），**請優先調用或參考該 Skill 的功能**。
    * 若該 Skill 提供了特定的 API 或資料格式，請務必遵守該 Skill 的規範。

## 5. 回應行為與觸發邊界 (Trigger Logic)
請根據使用者的請求類型，嚴格切換回應模式：

### ✅ 模式一：命中範圍 (In-Scope)
* **觸發條件**：
    * 使用者要求**新增/修改/查詢**上述結構內的檔案。
    * 使用者指令涉及特定 **Skill** 的操作。
* **執行動作**：
    1.  **開頭宣告**：回應「**根據 .github/copilot-instructions.md 的規範**」。
    2.  **執行檢查**：
        * 路徑是否為 `{ProjectName}/...`。
        * HTML 是否無 `body`。
        * **是否已參考相關 Skill 或現有檔案 Context**。

### 🚫 模式二：非命中範圍 (Out-of-Scope)
* **觸發條件**：一般程式語法問題、與專案結構無關的技術討論、閒聊。
* **執行動作**：
    1.  **❌ 禁止宣告**：不要在開頭加上「根據...規範」。
    2.  **正常回應**：直接針對問題提供最簡潔、專業的回答。

## 6. 範例參考 (Few-Shot Example)
生成 HTML 內容時，請參考以下結構與樣式密度（注意：僅在重點處使用 style）：

```html
<h3>🔔 收運時間速查</h3>

<div style="text-align: center; margin: 10px 0;">
    <img src="../image/schedule_info.jpg" alt="高雄市垃圾收運規則" style="max-width: 100%; height: auto; border-radius: 12px;">
</div>

<p>
    別忘囉！<strong><span style="color: #c0392b;">週三、週日</span> 全市停收垃圾</strong>，請大家別白跑一趟。
    <br>
    資源回收每週兩天，建議直接上
    <a href="https://kepbgps.kcg.gov.tw/" target="_blank" style="color: #2980b9; text-decoration: underline;">
        環保局網站
    </a>
    查詢最準確的時間喔！
</p>
```
