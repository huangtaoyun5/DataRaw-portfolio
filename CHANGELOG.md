# Changelog — dataraw.tech

## 2026-07-26 · 三軌 CV 體系 + 工程語言改寫 + 學歷更正

### 學歷更正（重要）
- 移除所有頁面上的 `B.Eng. Electrical Engineering` 與 `B.A. Drama & Theatre Arts` 字樣
- 逢甲改列為 `Electrical Engineering`（就讀經歷，不標學位），年份更正 2010–2013 → **2012–2015**
- 唯一取得之學位為陽明交大 M.F.A.（以同等學力入學）
- 影響檔案：`cv-general.html`、`cv-field-service.html`、`cv-experiential.html`
- `Tao_Yun_Huang_CV_Hardware_Engineer.pdf` 為修正前匯出，內含舊學歷 → **保留檔案但解除連結**，待重新匯出

### 新增 cv-field-service.html
- Customer Engineer / Field Service 專用版，對應半導體設備商 CE、FSE 職缺
- Field Readiness 區塊：輪班（含大夜）、on-call、無塵室 PPE、出差 25%+、體能作業
- 經歷條列以維護、排障、安裝試車、客戶對應優先；設計與 ML 內容大幅壓縮
- Hands-On Systems 矩陣：Electrical / Mechanical / Motion & Control / Pneumatic & Gas / Instrumentation / Controls & Network（真空、電漿、液壓未列，無實務經驗）

### 經歷補正（兩份工程 CV）
- 新增 **King One Interactive 王一互動科技 · Technical Project Manager · 2024/01–2024/12**（先前完全遺漏）
- 新增 Earlier 區塊：新竹高中兼任講師 2023/07–2024/06、交大國科會研究助理 2022/04–2023/01
- IF Plus Art 起訖更正為 **Dec 2024 – Apr 2026**，職稱統一 Hardware Engineer
- IF Plus Art 補上 EMC 測試、產線支援與品質控制、技術文件與規格、元件選型與 BOM

### 工程語言改寫
- `index.html`：statement 技能欄 3 類 → 4 類，Motion · Control 置頂；各作品 Stack 與 My role 全面改寫（柴電 PID 與 PM、川影 SLAM 與 6-DoF pose estimation、彌散圈氣體閥控制、13 聲部訊號完整性）
- `cv-general.html`：Profile、Experience 條列化、Selected Engineering Projects、技能欄 6 類

### 修復
- `cv.html` 導覽列指向已不存在的 `#commercial` / `#research` 錨點 → 改為 Works / About / Contact，與首頁一致
- 聯絡資訊 Taipei → Taiwan

### cv.html
- 改為三張卡：A 現場服務（CE）· B 硬體設計 · C 藝術品牌

## 2026-07-26 · Hardware CV PDF 上線

### cv.html
- Version A（Primary）加入 `Tao_Yun_Huang_CV_Hardware_Engineer.pdf` 下載按鈕，並保留線上檢視
- Version B 移除失效連結（`Tao_Yun_Huang_CV_Experiential.pdf` 檔案不存在，原本按下去是 404），改為線上檢視／另存 PDF
- 頁尾更新日期 April 2026 → July 2026

### Assets
- 新增 `Tao_Yun_Huang_CV_Hardware_Engineer.pdf`（由 cv-general.html 匯出，檔名改為 URL 安全格式）

## 2026-07-25 · Reposition site → Systems & Hardware Engineer

### Positioning
- 全站定位由「Creative Technologist & Interactive Developer」轉為「Systems & Hardware Engineer」
- 目標職缺：半導體精密設備硬體工程師（ASML / TSMC / Applied Materials 等）
- 內容依據新版 104 履歷（硬體工程師）調整

### index.html
- `<title>`、cover 職稱、statement 主文案改為機電/硬體工程導向
- statement skill stack 重整為 Hardware·Control / Embedded·Network / Software·ML
- 作品分組標籤 Research & Practice → Engineering & Research

### cv-general.html（重寫）
- 抬頭 Systems & Hardware Engineer；Profile 改為工程導向
- 新增 Professional Experience 區塊，主打 IF Plus Art 硬體工程師（2024–2026，PCB/手工蝕刻、60kg 天車運動控制、物理訊號排障）
- 作品改寫為 Selected Engineering Projects（工程面向優先）
- 新增 Certifications：UniFi Full Stack Professional (UFSP)、UniFi Wireless Admin (UWA) — Ubiquiti
- 學歷：台藝大戲劇濃縮為一行；技能重整為工程導向

### cv.html
- 副標改為 Systems & Hardware Engineer
- Version A 卡片改為「Hardware & Systems Engineer」（Primary），移除過時的 Tao_Yun_Huang_CV.pdf 下載，改為線上檢視/另存 PDF

### Housekeeping
- 新增 .gitignore：排除 104*.pdf（含個資，不上線）

## 2026-04-22 · Initial build & launch

### Deploy
- Netlify 連接 GitHub `huangtaoyun5/DataRaw-portfolio` main branch（auto-deploy）
- 域名 dataraw.tech（Gandi）A record → 75.2.60.5

### Content
- 所有 image-box 佔位符換成實際圖片（12 張，`images/` 資料夾）
- TTXC 新增 STUDIO · IF Plus Art
- Latent Breach Full Document → Google Drive PDF
- LinkedIn 連結修正：`tao-yun-huang-9953b5322`

### Structure
- Commercial 作品排序：Railway Museum → Moss Floating Layer → TTXC → AI Living Lab
- 影片連結加入全部作品：
  - Railway Museum `36CL4jkMAT0`
  - Moss Floating Layer `3qMWraIEPbc`
  - TTXC `KtusLH4ttM0`
  - AI Living Lab `mWuq7uki4DQ`
  - Threshold `YVT2q0_gJyA`
  - Circle of Confusion `YpsZ0EtSozE`
  - River of Shadows `FejVzhDx3Ls`
