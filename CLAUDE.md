# MLTD Idol Ranking Viewer 專案說明

## 專案概述

這是一個用於查詢《偶像大師 百萬人演唱會！ 劇場天堂》(THE IDOLM@STER MILLION LIVE! THEATER DAYS, MLTD) 遊戲活動排名資料的網頁工具。透過 matsurihi.me 提供的 Princess Public REST API 取得資料，並以圖表和表格呈現。

## 檔案結構

```
mltdIR/
├── mltdIR.html   # 主要版本：單一活動、多排名查詢
├── mltdIRY.html  # 年度比較版本：多活動、單一排名比較
└── CLAUDE.md     # 專案說明文件
```

## 使用的 API

### 資料來源
- 網站：https://api.matsurihi.me/docs/
- 基礎路徑：`https://api.matsurihi.me/api/mltd/v2/`

### 主要 Endpoints

| Endpoint | 用途 |
|----------|------|
| `/events?type=anniversary&orderBy=id!` | 取得週年活動清單 |
| `/events/:eventId/rankings/idolPoint/:idolId/logs/:ranks` | 取得特定偶像的排名歷史記錄 |

### API 參數說明

**排名歷史 API 參數：**
- `eventId`: 活動 ID
- `idolId`: 偶像 ID (1-52)
- `ranks`: 排名，支援格式如 `1-10,15,20`
- `since`: ISO 8601 時間格式，從何時開始抓取
- `all`: `true` 為 5 分鐘間隔，`false` 為 30 分鐘間隔
- `prettyPrint`: 設為 `false` 可加速回應

## 頁面功能對比

### mltdIR.html（主要版本）
- 選擇單一活動
- 選擇單一偶像
- 輸入多個排名（如 `1-10,15,20`）
- 設定查詢開始時間
- 選擇資料間隔（30分/5分）
- 產生折線圖（X 軸為時間）
- 產生資料表格
- 下載 CSV
- 計算 PT 差異（不同名次間、同名次時間差）

### mltdIRY.html（年度比較版本）
- 選擇多個活動（可跨年度比較）
- 選擇單一偶像
- 輸入單一排名（1-1000）
- 產生折線圖（X 軸為活動開始後經過的時間）
- 適合比較不同年度同一偶像、同一排名的表現趨勢

## 使用的前端套件

| 套件 | 版本 | 用途 |
|------|------|------|
| jQuery | 3.7.0 | DOM 操作、AJAX |
| Bootstrap | 5.3.0 | UI 框架 |
| Select2 | 4.0.13 | 下拉選單美化 |
| Tabulator | 5.5.0 | 表格元件（mltdIR.html） |
| Highcharts | 11.1.0 | 折線圖繪製 |
| dayjs | 1.11.9 | 日期時間處理 |
| Tempus Dominus | 6.7.13 | 日期時間選擇器（mltdIR.html） |
| Font Awesome | 6.4.2 | 圖示 |
| marked | 5.1.1 | Markdown 解析 |

## 偶像清單

共 52 位偶像，ID 1-52。資料硬編碼於 JavaScript 中，格式為：
```javascript
{id: 1, text: '1.天海 春香／天海 春香／HARUKA AMAMI'}
```
包含繁體中文、日文、英文三種名稱。

## 資料儲存

使用 `localStorage` 儲存：
- `eventList`: 活動清單（含 id、name、beginAt、endAt）
- `eventId`: 上次選擇的活動
- `idolId`: 上次選擇的偶像
- `rank`: 上次輸入的排名
- `time`: 上次選擇的查詢時間

## 開發注意事項

1. **API 請求限制**：短時間大量請求會返回 429 (Too Many Requests)
2. **資料來源標示**：使用公開服務時應標明來源為 matsurihi.me
3. **時區處理**：使用 dayjs 處理時間，顯示使用者本地時區
4. **活動類型**：目前僅抓取 `type=anniversary` 的活動

## 相關連結

- GitHub：https://github.com/katy50306/mltdIR/
- API 文件：https://api.matsurihi.me/docs/
- matsurihi.me：https://www.matsurihi.me/
