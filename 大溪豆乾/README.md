# 大溪豆乾風味散策

以手機優先的 Vite 靜態網站，呈現大溪豆乾文化、互動名店地圖、味蕾測驗與主題散步路線。

## 本機執行

需求：Node.js 18 或更新版本。

```bash
npm install
npm run dev
```

Vite 會顯示本機網址。正式打包：

```bash
npm run build
npm run preview
```

## 功能

- Leaflet + OpenStreetMap 地圖、店家分類篩選、瀏覽器定位與直線距離估算。
- 10 家示意店家資料、Google Maps 導航連結、3 題味蕾配對測驗。
- 文化工藝切換比較、三條主題散策路線、手機底部導覽。
- 繁體中文 SEO、JSON-LD 結構化資料、`utm_source` QR 導流提示。

## 店家資料與地圖金鑰

店家資料集中在 `src/main.js` 的 `shops` 陣列。示範座標是大溪散策區域內的約略位置，並非經查證的店家地址；營業時間亦為提示資訊。正式上線前請向各店確認名稱、地址、座標、營業時間、品項及路線順序，再更新資料。Google Maps 導航會以座標開啟外部路線規劃。

目前使用 OpenStreetMap 圖磚，不需要 API Key。若日後改接 Google Maps，建立專案及限制金鑰後，可新增 Vite 環境變數 `VITE_GOOGLE_MAPS_API_KEY`，在地圖初始化程式讀取 `import.meta.env.VITE_GOOGLE_MAPS_API_KEY`。金鑰限制應設定允許的網站網域；不要把未限制的金鑰提交到公開 repository。Leaflet/OSM 無法載入時會顯示靜態散策示意畫布，Google Maps 導航連結仍可用。

## GitHub 上傳

1. 在 GitHub 建立空 repository（例如 `daxi-tofu-guide`），先不要新增 README 或 `.gitignore`。
2. 在專案資料夾終端機執行以下指令，將 `YOUR_GITHUB_NAME` 換成你的帳號或組織名稱：

```bash
git init
git add .
git commit -m "Build Daxi tofu travel guide"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_NAME/daxi-tofu-guide.git
git push -u origin main
```

若 Git 提示設定身份，先執行 `git config --global user.name "你的名稱"` 與 `git config --global user.email "你的 GitHub 電子郵件"`，再重新 commit。

## Render 靜態網站部署

1. 登入 Render，選 **New → Static Site**，連結 GitHub 並選取 repository。
2. 設定：
   - **Build Command:** `npm install && npm run build`
   - **Publish Directory:** `dist`
3. 在 **Redirects/Rewrites** 新增 rewrite：**Source** `/*`、**Destination** `/index.html`、**Action** `Rewrite`。本網站目前是單頁錨點導覽，此規則也方便未來加入前端路由。
4. 按 **Create Static Site**。Render 完成建置後會提供 `onrender.com` 網址；每次推送至 `main` 會自動重新部署。

## QR 導流網址範例

- `https://你的網站.onrender.com/?utm_source=qr_parking`
- `https://你的網站.onrender.com/?utm_source=lbs_daxi`

頁面會顯示對應來源提示。此版本僅在前端呈現提示，不會將事件送往分析平台；若需成效統計，可再串接 GA4 或自有分析服務。
