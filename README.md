# 每日工作記錄表 v2.6：PWA 與 Android APK 打包說明

## 資料夾內容

| 檔案 | 用途 |
|---|---|
| `index.html` | App 本體（v2.6，已加上 manifest 連結與 Service Worker 註冊） |
| `manifest.json` | PWA 設定：名稱、圖示、主題色、截圖、捷徑 |
| `sw.js` | Service Worker：讓 App 可安裝、可離線開啟 |
| `icons/` | 一般圖示 192／512、可裁切圖示（maskable）192／512、iPhone 圖示、網頁小圖示 |
| `screenshots/` | 商店與安裝畫面用截圖（手機直式 2 張、電腦橫式 1 張） |
| `.well-known/assetlinks.json` | 網站與 APK 的驗證檔。拿到 PWABuilder 的指紋後要填寫，見第 4 步 |

> 這個資料夾裡的所有檔案（包含以「.」開頭的 `.well-known` 資料夾）都要一起上傳。

---

## 第 1 步：放到 https 網站（以 GitHub Pages 為例）

1. 在 GitHub 新增一個儲存庫，例如 `worklog`。
2. 把本資料夾**裡面的所有檔案**上傳到儲存庫的根目錄。
3. 到 **Settings → Pages**，Source 選 `Deploy from a branch`，Branch 選 `main`／`/(root)`，按 Save。
4. 約 1 分鐘後，網址會是：`https://您的帳號.github.io/worklog/`
5. 用 Android 的 Chrome 打開，確認 App 正常、語音可以用。

> **重要：`.well-known` 資料夾的位置**
> Android 只會去「網域根目錄」找驗證檔：`https://您的帳號.github.io/.well-known/assetlinks.json`
> 如果 App 放在子路徑（例如 `/worklog/`），還要另外建一個名為 `您的帳號.github.io` 的儲存庫，把 `.well-known/assetlinks.json` 放在那裡。
> 少了這一步 APK 仍然可以用，只是畫面最上方會多出一條網址列。

## 第 2 步：用 PWABuilder 檢查

1. 打開 https://www.pwabuilder.com
2. 貼上您的網址，按 **Start**。
3. 確認 Manifest 和 Service Worker 都顯示通過（綠色）。

## 第 3 步：產生 Android APK

1. 按 **Package For Stores**，選 **Android → Generate Package**。
2. 建議設定：
   - **Package ID**：`io.github.您的帳號.worklog`（英文小寫，之後不能更改）
   - **App name**：每日工作記錄表
   - **Launcher name**：工作記錄
   - **Version**：`1.0.0`，Version code：`1`
   - **Signing key**：選 **Create new**（第一次）
3. 下載 zip，裡面會有：
   - `*.apk`：可以直接傳到手機安裝測試
   - `*.aab`：上架 Google Play 時使用
   - `signing.keystore` 與 `signing-key-info.txt`：**務必妥善保存**，之後更新 App 都要用同一把金鑰
   - `assetlinks.json`：PWABuilder 已經幫您填好指紋的版本

## 第 4 步：填寫 assetlinks.json（去掉網址列）

用 PWABuilder 下載的 `assetlinks.json` 取代本資料夾的 `.well-known/assetlinks.json`，再上傳到網域根目錄（見第 1 步的「重要」說明）。
上傳後可以在這裡檢查：
`https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://您的帳號.github.io&relation=delegate_permission/common.handle_all_urls`

## 第 5 步：安裝到手機

1. 把 `.apk` 傳到手機（LINE、Email 或 Google 雲端硬碟都可以）。
2. 點開安裝，手機提示「允許安裝不明來源的應用程式」時，選擇允許。
3. 第一次按 🎤 或 ➕ 時，請允許麥克風權限。

---

## 備份與還原（v2.4 起）

- **☁️ 備份到 Google Drive**：使用手機的「分享」功能，在分享選單選「**雲端硬碟**」，再選擇 Google 帳號和資料夾，就能直接存進 Google Drive，不需要任何設定。備份檔是 `.txt`，內容是 JSON；Android 的分享功能不接受 `.json` 檔，所以改用 `.txt`。電腦或不支援分享的瀏覽器會自動改為下載。
- **📥 下載備份檔到手機**：存成 `.json` 檔，放在手機的「下載」資料夾。
- **📂 從備份檔還原**：在檔案選擇畫面點左上角 ☰，選「**雲端硬碟**」，就能直接選 Google Drive 裡的備份檔，`.txt` 或 `.json` 都可以。
- **📋 複製備份文字／📝 貼上文字還原**：以上方式都不能用時的備用方案。
- 超過 7 天沒備份，畫面上方會出現提醒，按「立即備份」會優先使用 Google Drive 分享。

## 完成的任務自動排到最下方（v2.5 起）

- 主任務、次任務、孫任務只要**打勾完成**，就會自動移到**同一層的最下方**。
- **取消打勾**時，會移回未完成任務的最後面，也就是已完成任務的上方。
- 還是可以用 ☰ 手動拖曳調整順序。

## 子項目全部完成，上層自動完成（v2.6 起）

- 某個次任務底下的**孫任務全部打勾**，這個次任務會自動標示完成。
- 某個主任務底下的**次任務全部打勾**，這個主任務會自動標示完成，而且會一路往上連動：勾完最後一個孫任務時，次任務和主任務可能一起完成。
- 自動完成的項目會依 v2.5 的規則移到同一層最下方，畫面下方也會顯示提示。
- 自動完成之後，如果又**取消某個子項目的打勾**或**新增未完成的子項目**，上層會自動恢復成未完成。
- 手動打勾的上層項目（子項目還沒全部完成時就先打勾）不會被自動取消。

## 注意事項

- **資料存在手機裡**：APK 透過手機上的 Chrome 執行，通常會和 Chrome 開同一個網址時共用資料。但如果清除 Chrome 資料、換了網址或手機沒有 Chrome，資料就會不同。安裝 APK 前，建議先用「下載備份檔」或「複製備份文字」備份一份，必要時再還原。
- **語音輸入**：APK 實際上是由手機上的 Chrome 執行，所以語音辨識跟 Chrome 一樣需要網路；離線時會自動改用鍵盤輸入。
- **更新 App**：修改 `index.html` 後，請把 `sw.js` 第 3 行的 `worklog-v2.6` 改成新的版本號（例如 `worklog-v2.7`）再上傳，手機下次開啟時就會更新。只改網站內容不需要重新產生 APK。
- **拖曳套件**：SortableJS 從 CDN 載入，第一次連網開啟時會自動存進快取，之後離線也能拖曳。
