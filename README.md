# 自強大樓 教室課表

國立臺北大學 臺北校區（合江）自強大樓的教室週課表、今日課程、電視看板與後台排課系統。

- 單一檔案 `index.html`，部署在 GitHub Pages
- 資料：Firebase Realtime Database，路徑 `ziqiang-schedule/`（與場地預約系統共用同一個 Firebase 專案，資料分開存放）
- 後台登入：Firebase Authentication（電子郵件／密碼），帳號由管理者在 Firebase 主控台建立

## 網址

| 用途 | 網址 |
|---|---|
| 週課表（預設） | `https://<帳號>.github.io/<repo>/` |
| 今日課程 | `…/#today` |
| 電視看板（全部樓層） | `…/#tv` |
| 電視看板（單一樓層） | `…/#tv-4F`、`…/#tv-B1` |

## 部署步驟

### 1. Firebase 設定檔
將 `firebase-config.example.js` 另存為 `firebase-config.js`，填入 apiKey 與 appId（Firebase 主控台 → 專案設定 → 一般 → 您的應用程式 → SDK 設定）。找不到此檔時系統以示範模式執行。

### 2. Realtime Database 安全規則（已於 2026/10/16 套用）
```json
{
  "rules": {
    "ziqiang-schedule": { ".read": true, ".write": "auth != null" }
  }
}
```
`ziqiang-schedule` 公開讀取、登入後才能寫入；其餘路徑一律拒絕。
原場地預約的 `bookings` 資料仍保留在資料庫中，但已停止讀寫。若需恢復，於 `"rules"` 內加回
`"bookings": { ".read": true, ".write": true }`。

### 3. 後台管理帳號
Authentication → 使用者 → 新增使用者，以電子郵件與密碼建立（「電子郵件／密碼」登入方式已啟用）。

### 4. GitHub Pages
1. 建立新的 repository（例：`NTPU-ziqiang-schedule`），上傳 `index.html` 與本說明檔
2. Settings → Pages → Source 選「Deploy from a branch」，Branch 選 `main`、資料夾 `/ (root)`，儲存
3. 約 1 分鐘後即可由上方網址開啟

### 5. 首次使用
用管理帳號登入後台，系統會自動寫入 15 間預設教室；課程從「課程管理」新增。

## 電視看板建議
- 硬體：Android TV 盒子＋Fully Kiosk Browser，或迷你電腦以 Chrome kiosk 模式開機自動開啟 `…/#tv`
- 每天凌晨 3 點自動重新載入；22:00–07:00 自動調暗；斷線時角落顯示最後更新時間
- 電視開關機請用電視本身的定時功能
