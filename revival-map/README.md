# 復甦島｜作戰地圖系統
## 部署指南 & 使用說明

---

## 📁 檔案結構

```
revival-map/
├── index.html          ← 主程式（協作編輯版）
├── maps/
│   ├── satellite.jpg   ← 衛星圖（3D圖）
│   └── road.png        ← 道路圖（GTA衛星圖）
└── README.md
```

---

## 🚀 Step 1：上傳到 GitHub

1. 前往 [github.com](https://github.com) → 登入 → New Repository
2. Repository name 輸入：`revival-map`（或自訂）
3. 設為 **Public**（GitHub Pages 免費版需 Public）
4. 點 Create repository

### 上傳檔案
```bash
# 如果有安裝 Git：
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/你的帳號/revival-map.git
git push -u origin main
```
**或直接在 GitHub 網頁上傳：**
- 進入 repo → Add file → Upload files
- 把 `index.html` 和整個 `maps/` 資料夾拖進去

---

## 🌐 Step 2：開啟 GitHub Pages

1. Repo 頁面 → **Settings** → **Pages**
2. Source 選 **Deploy from a branch**
3. Branch 選 `main`，folder 選 `/(root)`
4. Save

幾分鐘後網址會出現：
```
https://你的帳號.github.io/revival-map/
```

---

## 🔥 Step 3：設定 Firebase（協作同步）

### 建立 Firebase 專案
1. 前往 [console.firebase.google.com](https://console.firebase.google.com)
2. **新增專案** → 輸入名稱（如 `revival-island`）→ 繼續
3. 不需要 Google Analytics → 建立專案

### 開啟 Realtime Database
1. 左側選單 → **Realtime Database** → 建立資料庫
2. 地區選 **asia-southeast1（Singapore）**（最近台灣）
3. 安全性規則選 **以測試模式啟動**

### 設定安全規則（Rules 頁籤）
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
> ⚠️ 以上是開放規則，適合團隊內部使用。

### 取得設定碼
1. 專案首頁 → 點 `</>` 圖示（Web App）
2. 輸入名稱 → 註冊應用程式
3. 複製 `firebaseConfig` 的 JSON 內容，格式如下：

```json
{
  "apiKey": "AIzaSy...",
  "authDomain": "your-project.firebaseapp.com",
  "databaseURL": "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  "projectId": "your-project",
  "storageBucket": "your-project.appspot.com",
  "messagingSenderId": "123456...",
  "appId": "1:123456:web:..."
}
```

4. 首次開啟網站時，貼入彈出的設定框即可。

---

## 🔗 兩種網址（唯讀 vs 編輯）

| 版本 | 網址 |
|------|------|
| **協作編輯版** | `https://帳號.github.io/revival-map/` |
| **唯讀版**（給玩家看） | `https://帳號.github.io/revival-map/?readonly=true` |

---

## 🎮 操作說明

### 快捷鍵
| 鍵 | 功能 |
|----|------|
| `M` | 標記點模式 |
| `R` | 框選區域模式 |
| `C` | 連線模式 |
| `Esc` | 取消當前操作 |

### 新增標記點
1. 點工具列 📍 圖示（或按 `M`）
2. 點選地圖上的位置
3. 填入名稱、備註、類型、狀態 → 儲存

### 框選區域
1. 點工具列 ⬜ 圖示（或按 `R`）
2. 按住滑鼠左鍵拖曳畫框
3. 放開滑鼠 → 填入資訊 → 儲存

### 連線兩個標記
1. 點工具列 🔗 圖示（或按 `C`）
2. 點選第一個標記
3. 點選第二個標記 → 自動連線

### 搜尋
- 頂部搜尋框輸入文字
- 即時搜尋標記名稱與備註內容

---

## 🎨 標記類型說明

| 類型 | 顏色 | 用途 |
|------|------|------|
| 🔴 主線任務 | 紅色 | 主要劇情任務地點 |
| 🔵 支線任務 | 藍色 | 支線、額外任務 |
| 🟠 觸發任務 | 橙色 | 需特定條件觸發的事件 |
| 🟢 NPC 點位 | 綠色 | 重要 NPC 位置 |
| 🟣 危險區域 | 紫色 | 戰鬥或危險區域 |
| 🟡 資源點 | 黃色 | 物資、裝備、補給 |

---

## ⚙️ 進階設定

### 更換地圖圖片
在 `index.html` 頂部找到：
```javascript
const MAP = {
  sat:  'maps/satellite.jpg',   // 衛星圖路徑
  road: 'maps/road.png',        // 道路圖路徑
  w: 8000, h: 8000              // 圖片尺寸（像素）
};
```

### GTA5 座標校正
如覺得座標顯示不準確，調整：
```javascript
const GTA = { minX:-4150, maxX:4550, minY:-4500, maxY:8500 };
```

---

## 📞 常見問題

**Q：圖片沒有顯示？**
確認 `maps/` 資料夾和圖片有正確上傳到 GitHub。

**Q：Firebase 連線失敗？**
確認 `databaseURL` 包含完整網址，Realtime Database 已啟用。

**Q：多人同時編輯會衝突嗎？**
Firebase 採最後寫入優先（last-write-wins），同時編輯同一個標記時後存者覆蓋前者。建議分工標記不同區域。

**Q：想重設 Firebase 設定？**
開啟瀏覽器 DevTools → Application → LocalStorage → 刪除 `fbConfig`，重新整理即可重新輸入。
