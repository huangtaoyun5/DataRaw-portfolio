# Changelog — dataraw.tech

## 2026-07-31 (定版) · 投影片化 + 輪播

### 每個 section 就是一張投影片
上下鍵在整段內容之間移動，不是在捲動位置之間移動。
順序：封面 → 01 Profile（宣言 + 完整經歷）→ 02 Capability（能力矩陣 + 證照）
→ 03 Works（作品索引）→ 九張作品圖紙 → 04 Contact。
**全部剛好一個螢幕，零溢出**——包含九個抽屜，打開都不需要再捲。

### 輪播取代堆疊
一個框、多張圖，只有畫面上的那個輪播在跑。用在：
- **每張作品圖紙的左側大圖**——該專案的所有影像都在這裡循環（十三聲部五張、
  柴電四張、文化科技四張、川影四張、闁三張、Blender 八張）。
- **證照**——UWA / UFSP 在同一個位置輪替，尺寸放大到看得清楚為止。

**抽屜改為純文字**（敘述 + 規格表），寬度收窄，開啟時左側大圖仍看得見。

### 素材
- LINE OA 實機操作畫面（動態）、LINE Bot 流程圖、線性軸 CAD 機構模擬、
  Cadence Allegro PCB 佈局與 CAM/Gerber 檢視——共 49 MB 的 GIF/MP4
  轉成 4.5 MB 的 animated WebP。原始檔全部移入 gitignore 的 `_archive/`，一個都沒刪。
- 川影改用實機截圖輪播，不再嵌影片。
- Works 索引恢復舊版的右側大預覽（滑過列表換圖）。
- 移除 Contact 的 Video index 連結與 EU Blue Card / Global Talent 那行。

---

## 2026-07-31 (三次修訂) · 版面系統化 + Work Detail 改為右側抽屜

### Work Detail 改為 hover 抽屜
捲動融合的做法會讓 detail 佔掉一段捲動距離，且長文與圖片在同一張圖紙上很難排。
改為**停在右緣的抽屜**：只露出一條黑色 `DETAIL` 直式標籤，滑鼠移上去滑出、移開自動收合。
不需點按、沒有卡住的狀態、概覽不會被破壞。抽屜內獨立捲動（`data-lenis-prevent`），
文字改單欄由上而下：敘述 → 規格表 → 支援圖。觸控裝置改為點標籤切換。
面板軌道從 230vh 收回 **165vh**（不再需要第二段捲動）。

### 版面系統化
- **所有媒體框統一為 16:9**，不再讓每個專案自帶比例。全景與直幅改用 `contain`
  在框內置中、以網格底襯托，框的尺寸維持一致——每頁自訂圖片尺寸會讓整份簡報看起來像意外而非系統。
- 抽屜內的支援圖同樣統一：單張或首張 16:9 跨欄，其餘 4:3 並排。
- **兩張證照改為並排**（原本直式堆疊各佔半個畫面）。
- **整體字級上調一級**：內文 0.95→1.1rem、標題 3→3.5rem、mono 標籤 0.6→0.68rem。

### 其他
- 封面罐頭往內移，中心落在畫面約四分之三處，不再貼著右緣。
- Profile 的宣言右側原本整片空白，補上 **Experience / Education 完整經歷表**
  （逢甲僅列就讀，不標學位）。
- 製圖框（SHEET / REF / VIEW）在抽屜開啟時自動淡出——sticky 圖紙自成堆疊脈絡，
  抽屜無法蓋過 fixed 元素，所以讓它退開而不是硬拉層級。

---

## 2026-07-31 (二次修訂) · Work Detail 融入捲動 + 補齊素材

### Work Detail 不再需要點按
原本點「Work Detail +」會開一層覆蓋，切換感生硬。改為**同一張圖紙上的第二個狀態**：
面板軌道加長到 230vh，捲到約 40% 時 overview 淡出、detail 淡入，全部由捲動進度驅動。
沒有按鈕、沒有彈層，翻頁節奏連續。右下角製圖框新增 `VIEW` 欄顯示 OVERVIEW / DETAIL。
`↑` `↓` 現在會在「概覽 → 細節 → 下一張概覽」之間停，`1`–`9` 仍直接跳專案。

### 動效預設開啟
`deck.html` 直接就是完整動效，不再跟隨系統的「減少動態效果」設定。
`?motion=off` 是唯一的降級入口，且**不再記憶**——記住 off 會在最需要的場合把簡報鎖在平面版。

### 素材
- **兩張證照換成官方數位版**（原本是紙本翻拍），並排兩張卡片呈現，尺寸修正。
- **Blender MCP 補完**：不再是 placeholder。以 `blender_bridge_server.py` 為核心——
  Blender 內的 TCP socket server，腳本經 lock 保護的佇列由 `bpy.app.timers` 在主執行緒執行
  （Blender Python API 非執行緒安全）。素材含原始碼畫面、OSM 城市算圖、幾何與位移研究。
- **彌散圈**補 3 張 VR 擷取（取自 `past_works/02_diffuse/`）。
- 9 個專案中 **6 個的 detail 已圖文並茂**；03 文化科技交易所、05 AI Living Lab、
  08 苔域浮層仍缺支援圖，清單見 `images/_incoming/README.md`。

---

## 2026-07-31 · 新增 deck.html（面試簡報站）+ CV 頁轉黑白

為 Applied Materials CE 面試（新竹廠區，視訊螢幕分享）新建的簡報用頁面。
**`index.html` 完全未更動**，新舊兩版並存，隨時可對照或回退。

### 新增 `deck.html`
- **視覺**：工程製圖語彙。嚴格 `#000` / `#fff` 二色（階層以 alpha 表現，不設灰階 token）、
  滿版方格紙網格（40px 細線、200px 粗線）、IBM Plex Sans Condensed + IBM Plex Mono、
  隱藏捲軸、自訂游標（`mix-blend-mode: difference` + 情境提示字）、右下角製圖 title block。
  **配色規則：介面黑白，內容保留原色**——罐頭商標、專案照片、影片一律不改色。
- **捲動**：CSS `position: sticky`。`.panel` 155vh 作為捲動軌道，內層 `.sheet` 100svh 吸附在
  視窗頂端——每張圖紙停住不動、捲到底才換下一張。刻意不用 JS pin：不需測量，
  圖片延遲載入也不會失同步。GSAP 只負責 scrub 特效，Lenis 負責慣性捲動。
- **鍵盤操作**（簡報用）：`1`–`9` 直接跳專案、`↑` `↓` `Space` 上下頁、`Home` / `End`。
- **影片**：進入視野才注入 iframe、離開即卸載，開站時不對 YouTube 發任何請求。
  靜音自動播放（瀏覽器政策），附解除靜音鈕。
  **柴電工場與文化科技交易所兩支影片來源頻道禁止嵌入**，改以本地大圖 + 外連呈現。
- **動效模式**：預設跟隨系統 `prefers-reduced-motion`，可用 `?motion=full` / `?motion=off` 覆寫
  並記憶。簡報請用 `deck.html?motion=full`。
- ≤960px 或 reduced-motion：關閉 sticky 與所有動效，回到單欄自然文件流與原生游標。

### 專案順序（面試敘事）
13-Voice CV Synthesizer → 柴電工場 → 文化科技交易所 → 川影 → AI Living Lab →
彌散圈反應 → 闁 → 苔域浮層 → Blender MCP（素材待補）。LATENT BREACH 不列入本頁。

### 四份 CV 頁轉黑白
移除赭橘 `#B85C38` 與暖紙 `#F7F5F0`，字體改 IBM Plex，與 `deck.html` 一致。

### 素材結構重整
新增 `images/works/NN-*/`、`brand/`、`certs/`、`_incoming/`、`_archive/`（後者 gitignore）。
**現有扁平檔案一律未搬動、未改名、未刪除**（`index.html` 仍在使用）。
新結構圖片壓縮後 47.9MB → 4.4MB。罐頭商標去背（保留原彩色），favicon 由 8MB 換成 49KB / 2KB。

---

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
