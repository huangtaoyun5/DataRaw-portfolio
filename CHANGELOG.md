# Changelog — dataraw.tech

## 2026-09-05 · 輪播滑鼠移入暫停；滑軌拿掉一張重複的圖

**觸發：** 簡報站是面試現場真的會操作的東西。
面試官（或自己）把滑鼠停在圖上通常就是**想多看兩秒**，
但輪播照舊四秒換一張，等於在跟看的人搶主導權。

### 輪播：兩個旗標，一個閘門

原本只有 `IntersectionObserver` 控制 `start()` / `stop()`。
直覺的做法是 `mouseleave` 就 `start()`——**但那會出事**：
hover 過某一頁的輪播、然後往下捲，指標離開時輪播已經在畫面外，
卻被重新啟動，變成在看不到的地方空轉。

所以改成**兩個狀態、一個同步函式**，只有「在畫面內」且「沒有被 hover」才跑：

```js
var onScreen = false, held = false;
function sync() { (onScreen && !held) ? start() : stop(); }
```

- `mouseenter` / `mouseleave` — 滑鼠暫停
- `focusin` / `focusout` — **鍵盤 Tab 到下面那排圓點時也會停**，
  不然焦點還在按鈕上、圖卻自己跳掉
- ⚠️ **刻意用 `mouseenter` 而不是 `pointerenter`**——
  pointer 事件在觸控裝置上點一下就進入 hover 且不會離開，
  手機上會變成**永久暫停**

**五個輪播全部生效**（認證、電路板、滑軌、天車、LINE OA），共用同一段 `[data-carousel]` 初始化。

### 滑軌 Sheet 02：四張改三張

拿掉 `02-railway/hero.jpg`（`Carriage in travel · S212`）。
**它跟第一張 `stage-alignment.jpg` 講的是同一件事**，
而那張把「螢幕剖面對準後方車體」表達得更清楚。
剩下三張各有分工：**對位 → CAD 機構 → 玻璃上的剖面圖**。

> `hero.jpg` 仍用於索引頁的 `#indexPreview` 縮圖，**沒有刪檔**。

### 驗證

瀏覽器實測：3 張、hover 九秒不換、放開後恢復、
**hover 後捲離畫面再放開滑鼠不會誤啟動**（就是上面防的那個情況）。

> ⚠️ 除錯時一度誤判 hover 失效——**`mechanism.webp` 是動態 WebP，圖本身會轉**，
> 看起來像輪播在換。**之後判斷輪播狀態要讀 `.cslide.on`，不要看畫面。**

## 2026-09-04 · 柴電工場拆成兩個 Sheet（滑台／天車自動化）

**觸發：** 電樞區「把手動天車改成全自動」本身就是一個完整的案子——
自動化改造＋外部閉迴路＋60 點校正基準，
先前被埋在 Sheet 02 的一句 `Also on site` 裡，浪費了。

### 主線從 5 個 Sheet 變 6 個

| Sheet | 內容 |
|---|---|
| 02 | **Diesel-Electric Workshop · Axis** — S212 滑台（20 m／500 kg／EtherCAT 伺服） |
| **03（新）** | **Overhead-Crane Automation** — 電樞區天車（手動改全自動／PLC／三雷射／60 教點） |

**兩頁刻意形成對照：同一個場館、兩種架構。**
滑台要**連續平順的長行程**，所以用伺服＋EtherCAT，軌跡由主站塑形；
天車要**點對點到位判斷與互鎖**，所以用 PLC 邏輯控制。
**不是哪一套比較好，是哪一套適合當下要解的問題**——這個對照本身就是選型判斷的證據。

**連帶改的：**
- `data-sheet` 重編 `01–06`、`data-total` 全部改 `06`、`workCount` → `06 sheets`
- 索引頁新增一列 `#w-crane`，後面三列編號順延
- `#indexPreview` 新增 `w-crane` 的 slot、JS 的 `labels` 補一筆
- **三份 CV 頁**跟著拆：柴電那條 `comm-row` 拿掉 `Also on site: 60 kg armature`，
  另開一條 `Overhead-Crane Automation`
- ⚠️ `cv-general.html` 原本寫 `Same architecture applied to a 60 kg armature`——
  **那是錯的，天車根本是不同架構**，已改成 `A separate bay ... took the opposite approach`

### 素材：從館方影片抽幀

`03_ASML\鐵道博物館 柴電工場..._1080p.mp4`，用 `cv2` 依秒數抽 1920×1080 幀：
- **6–46 秒是電樞區** → `images/works/02b-railway-crane/`（hero 25s 天車吊掛電樞、
  detail-1 12s 天車軌道全景、detail-2 40s）
- **52–76 秒是滑軌區** → 補進 `images/works/02-railway/`
  （`stage-alignment.jpg` 74s ＝ **透明螢幕與後方 S212 實車的剖面對齊**，
  這張是「影片索引編碼器位置」最好的視覺證明，已設為該頁首圖）

### 🔴 列印分頁：多一頁之後又溢出，這次的解法值得記

多一個 Sheet 之後，**天車那頁比別頁多約 80 字元就溢出成兩頁**（孤兒頁只有一行）。

**試過但無效的：** 把 `.frame --mh` 從 56mm 縮到 53mm。
**原因：`.p-body` 是左右並排的 grid，圖縮小不會讓文字往上**——
文字欄的高度是由文字本身決定的。**縮圖只會讓左邊留白變多。**

**有效的是收 drawer 的散文行高：**
`.detail-grid p` 由 `.84rem / 1.4` 改為 `.82rem / 1.36`，段距 `.35rem → .3rem`。
**一次就從 12 頁（含孤兒）變成 11 頁，每個 Sheet 剛好一頁。**

💡 **教訓：並排 grid 裡，垂直空間不足時要動的是文字，不是圖。**

## 2026-08-26 · 簡報站可以印成 A4 橫式作品集（26 頁 → 10 頁）

**觸發：** 要附一份 portfolio PDF 給美光。直接列印 `index.html` 出來是 26 頁，
文字重疊、圖片變空框、還有整頁空白。

### 1. 根因：列印時手機版樣式全部生效

Chrome 列印的**媒體查詢與 `vw` 單位是拿 A4「直式」頁寬（約 718px）去比對的**，
即使 `@page` 已經把紙轉成橫式、實際排版寬度是 1046px。
於是 `@media (max-width: 960px)` 整塊照樣命中 —— 封面的姓名與照片被拆成上下兩列、
每張作品的圖與文字也被拆開，一頁的東西變成兩三頁。

修法是在 `@media print` 裡把被蓋掉的桌機格線**明確搶回來**
（`.cover-mid` / `.cover-bot` / `.certs` / `.mrow` / `.reg-row` / `.p-body` / `.thesis-row`）。
**以後在 ≤960px 區塊加任何 `grid-template-columns`，都要順手在列印區塊補一條對應的
`!important`，否則列印版會靜靜地退回單欄。**

### 2. 螢幕幾何攤平

- `.panel`（165vh 捲動軌）與 `.sheet`（sticky + `height:100svh` + `overflow:hidden`）
  在分頁時都沒有意義 → 全部 `static` / `auto` / `visible`
- `.drawer` 從右側抽屜變成同一頁下方的區塊，prose 左、spec 右
- `.frame` 的 `--mh` 給**公釐值**（作品 56mm、證照 42mm）。
  `--mh` 同時決定寬度公式 `min(100%, --mh × --arw)`，一個值就把兩軸都定了。
  ⚠️ 不要改成 `--mh: none`：`calc(none × 1.7778)` 整條宣告失效，
  框會塌成 0 高、圖片（`height:100%`）跟著消失。
- `[data-reveal]` 強制 `opacity:1`（捲動觸發的動畫在列印時永遠不會播，
  不強制就整份空白）

### 3. 分頁點與密度

`.cover` / `.sec` 各 `break-after: page`，`.panel` `break-before: page`。
**`.panel` 不加 `break-inside: avoid`** —— 只超出幾行的整塊會被推到下一頁，
反而在前一頁留一片空白。
行高、內距、字級在列印時整體收緊（`.p-points`、`.spec`、`.detail-grid p`），
`.p-body` 欄寬比例翻轉成 `1fr / 1.15fr`：紙上跑得長的是文字不是圖。

**結果：10 頁** —— 封面 / Profile / Capability / 作品索引 / 五張作品各一頁 / 封底，
沒有空白頁，也沒有只剩兩三行的孤兒頁。

### 4. 學歷字串

三份 CV 頁的 Education 欄拿掉 **Institute of Music**（不精確），
研究助理那列的委託單位也一併統一成校名。學位維持 `M.A. Multimedia & NM`。

---

## 2026-08-24 · 簡報站改為工程主線＋side projects 獨立頁

**觸發：** ASML 一面回饋「學經歷比較特別，工程面要更明顯、藝術成分要降低」，
而且 8/25 美光面談、9/7 ASML 二面都會用這個站當簡報。

### 1. Works 拆成兩層
主線只留 **05 個工程項目**：十三聲部 · 柴電工場 · 文化科技交易所 · AI Living Lab · Threshold。
**川影 · 彌散圈反應 · 苔域浮層 · Blender MCP** 移到新頁 `side-projects.html`
（複製 index 再裁切，CSS 與 GSAP 捲動邏輯完全一致，不另寫樣式）。
清單最後一列改成 `→ Side Projects · Separate page` 連過去；side 頁的導覽列連結
全部改指 `index.html#...`，否則點了沒反應。

- `data-sheet` 主線重編 `01–05`，side 頁 `01–04`
- 新增 `data-total`，HUD 由 `dataset.sheet + ' / 09'` 改為讀 `data-total`
- `workCount` → `05 sheets · 2022 — 2026`
- 彌散圈標籤 `VR · Pneumatics` → **`Gas control · Pneumatics`**
  （即使降為 side project，ASML 問到氣體經驗時它仍是唯一證據，讀起來要是工程不是 VR）

### 2. 封面照換掉罐頭圖
`dataraw-can.webp` → **`cover-engineer.png`**（現場組裝校正照，戴頭燈與手套）。
**理由：** 罐頭圖需要一段口頭解釋才成立；這張不用解釋，一眼就是設備工程師在動手。
美光與 ASML 兩邊都適用。

🔴 **直向照片撐破版面，已修：**
原本 `width:100%` 讓 396×671 的圖被放大到 545×923（138% 上採樣，大螢幕會糊），
封面總高衝到 1249px > 視窗 917px，**職稱與聯絡資訊掉到摺線下**。
改成 `max-height: min(56vh, 671px)` + `width:auto` + `object-fit:contain`，
封面回到剛好一個視窗、且不再上採樣。
⚠️ **1366×768 筆電未實測**（瀏覽器視窗鎖在最大化，resize 無效）——**簡報前用實機開一次**。

### 3. 地點文案（兩份工作地點都已確定在台中）
- 封面 `Taiwan · Open to relocation` → `Taichung, Taiwan · Available immediately`
- 結尾大字 `Open to full relocation worldwide.` → `Based in Taichung. Available immediately.`

### 4. Threshold
輪播拿掉，改為 C-LAB（v2）單張靜態圖；首頁縮圖同步改用 v2。

**備份：** `index.html.bak-20260824`
🔴 **連帶待辦：ASML 逐字稿開場有一整段在講罐頭圖，已與畫面對不上，二面前必須重寫。**

## 2026-08-21 · 移除全站「EMC 測試」宣稱

準備 ASML 二面（Project Lead 劉世宇，機械本科＋台大應用力學碩士）時，
問到「EMC 測試那塊你做了什麼」，用戶回答**不確定**。

**不確定就不能留在對外文件上。** 設備職面試官問「用什麼設備、依哪個規範測的」
是很自然的追問，答不出來的殺傷力遠大於少列一項技能，而且會回頭汙染其他誠實陳述。
全站只有兩處，都已改掉：

| 檔案 | 原本 | 改成 |
|---|---|---|
| `index.html:1355` 技能列 | `… Hand etching · EMC testing · Soldering …` | 直接刪掉 `EMC testing` |
| `cv-field-service.html:372` | `Ran electromagnetic-compatibility (EMC) testing on custom hardware and closed out the interference paths it exposed.` | `Traced interference paths on custom hardware and closed them out through grounding and EMI isolation.` |

`cv-general.html` 只寫 `EMI & Noise Isolation`／`Grounding`，**那是他真的做過的，保留**。
`cv.html`、`cv-experiential.html` 無此宣稱。

🔴 **判準**：**EMI／雜訊隔離、接地** 是實作經驗，可以講；
**EMC 測試** 是有規範、有設備、有報告的正式驗證，**沒做過就不要寫**。

## 2026-08-07 · 簡報站在 1600px 以下改為長頁

簡報站的網址已經寄給應材主管做面試後評估，所以它現在是被「讀」的材料，
不只是簡報時由本人操作的東西。這次針對讀者的螢幕做修正。

### 問題：窄視窗會吃掉最有力的四列規格

實機 1366×768 的筆電，CSS 視窗只剩 **1350×589**——瀏覽器介面吃掉 179px。
而最高的作品詳述需要約 780px。Sheet 02 柴電工場（面試主場）因此有 **193px 在摺線下，
六列 spec 有四列看不到**：`DRIVE`（`AZXD-SED` over EtherCAT）、`FEEDBACK`
（免電池絕對編碼器、斷電煞車）、`ALSO ON SITE`、`SERVICE`（scheduled PM、remote monitoring）。

**掉的正好是全站對客戶工程師職務最有力的技術證據**，而 `.drawer-body` 的捲軸
被 `::-webkit-scrollbar { display: none }` 藏起來，讀者連「下面還有字」都不會知道。

### 做法：1600px 以下解除投影片模型

新增一個 `@media (max-width: 1600px)` 區塊，`.sheet` 不再是固定高度的一頁，
詳述區塊改為跟在概覽下面流動：`.panel` 高度 auto、`.sheet` 改 `static`／`auto`／
`overflow: visible`、`.drawer` 改 `static` 全寬、`.drawer-tab` 隱藏、
`.drawer-body` 改 `overflow: visible`。**這是沿用 `≤960px` 手機版早就在走的路徑，
只是把適用範圍往上延**，不是新寫一套。

不能直接把既有的 `960` 改成 `1600`——那個區塊裡混了純手機規則（隱藏作品索引、
證照單欄、`.mrow` 窄格、`.p-body` 單欄），套到 1400px 會變成手機版面。
所以另開區塊、只放解除簡報模式那幾條，並保留雙欄的左圖右文。

### 斷點取 1600 而不是內容剛好放不下的寬度

因為**餘裕比內容更早崩**。最高的抽屜在 1584 只剩 **+15px**、在 1536
（1920 螢幕開 125% 縮放，商用筆電常見設定）**+18px**——一條書籤列就吃光；
而 1920 未縮放是 **+63～+119px**。分界線落在 +18 與 +63 之間，
所以投影片模式只留在真正有餘裕的寬度。本機 1920 與 F11 全螢幕都不受影響。

### 兩個攤平後才浮現的配套修正

- **`.tblock` 隱藏。** 製圖 title block 是 `position: fixed` 釘在右下角，
  只有在「一張圖紙＝一個視窗、右下角本來就空著」時才成立。內容一旦流動，
  它就會壓在正在經過的內文上。`≤960` 早就這樣處理。
- **`.detail-grid p` 加 `max-width: 66ch`。** 文案是為 40rem 抽屜寫的，
  放到滿版 1229px 會變成**一行 141 字元**。加上限後九張圖紙全部落在 619px／約 71 字元。
  `.spec` 維持全寬——值不折行才是它省高度的原因。

### 抽屜捲軸還原

`.drawer-body` 改用 `scrollbar-width: thin` ＋ 6px 的 `::-webkit-scrollbar`，
拇指用 `--ink-45`。因為容器是 `overflow-y: auto`，**內容放得下時捲軸根本不出現**，
1920 的觀感完全不變；只有在放不下的機器上才現身，也正是需要它的時候。

### 驗證

| CSS 視窗 | 模式 | 裁切 | 橫向溢出 | 每行字元 |
|---|---|---|---|---|
| 1920×861／917（本機實測） | 投影片 | 0 | 0 | 64 |
| 1601×900 | 投影片 | 0 | 0 | 73 |
| 1600×900 · 1536×724 · 1453×721 · 1350×589 | 長頁 | 0 | 0 | 71 |
| 810×1010 · 390×750 · 360×720 | 長頁 | 0 | 0 | — |

九張圖紙 55 列 spec 全數可見。量測工具與真實 Chrome 交叉校準過：
同一個抽屜兩邊都量到 798px。

## 2026-08-04 · 履歷頁補上 8/1 的改寫，舊 PDF 下架

8/1 那次「轉為 Field Service 語言」只改了簡報站，四個 CV 頁全部停在 7/31。
同一個網站因此有兩套說法：簡報站的柴電工場主敘事是 20 m／500 kg 透明屏滑台，
履歷上卻只有 60 kg 電樞。這次把履歷補齊。

### 三個 CV 頁補上滑台
內容源頭是 `index.html` Sheet 02，改寫成履歷語氣：
20 m 單軸 · 500 kg 移動負載 · 六片透明螢幕 · 600 W AZX 伺服 + 25:1 行星 +
`AZXD-SED` over EtherCAT · 免電池絕對編碼器 · 斷電動作型煞車。
核心論述**播放索引絕對編碼器位置而非時鐘、誤差不累積**三個版本都寫進去了。
60 kg 電樞降為 `Also on site`。A 版側重維護與試車，B 版側重驅動選型與迴路設計，
C 版維持藝術軌語氣——三軌的差異化沒有被這次補寫抹平。

### Version A 其他對齊
- `Hands-On Systems Experience` 對齊簡報站 Capability Matrix：補
  `long-travel high-mass axes`、`EtherCAT`、`absolute encoder feedback`、
  `component-level root-cause analysis`、`leak checking by soap test and pressure decay`。
- **新增 `Not claimed — no vacuum, plasma, or hydraulic process experience` 一列。**
  簡報站早就有，履歷一直沒有。主動承認對設備商是加分。
- 出差／異動改折衷寫法：主線 `relocation within Taiwan — Hsinchu · Taoyuan ·
  Taichung · Tainan · Kaohsiung`（涵蓋 AMAT／LAM／KLA／Micron 台灣廠區），
  海外收成最後一句 `Open to overseas assignment if required`。原本的
  `anywhere in Taiwan or abroad` 與 `Travel 25%+, including overseas` 太滿。
  **定位是通用設備商 CE/FSE 履歷，不對單一 JD 專用化。**

### 學歷字串四頁統一
`M.F.A. Multimedia & NM`——與簡報站完全一致。原本三個 CV 頁三種寫法
（`M.F.A. — Institute of Music, NYCU`／`M.F.A. Multimedia Music`／
`MFA Multimedia Music (New Music Theatre)`）。系所名一律 `Institute of Multimedia NM`。

### 列印版面：修掉吃掉整頁的 page-break 規則
`.cv-section { page-break-inside: avoid }` 套在整段工作經歷上，
塞不下就把整塊推到下一頁，第 1 頁下半整片空白。改成**有列狀子元素的長區塊可以跨頁**
（每一列自己仍受保護），短區塊（學歷／證照／技能／Availability）維持整塊不拆：

```css
.cv-section { page-break-inside: avoid; }
.cv-section:has(.exp-bullets), .cv-section:has(.comm-row),
.cv-section:has(.work-row), .cv-section:has(.proj),
.cv-section:has(.res-row) { page-break-inside: auto; }
```

**CE 版 4 頁 → 3 頁，一個字都沒砍。** B 版仍 4 頁（末頁只有 Availability，
要進 3 頁得砍內容，用戶裁示不砍）；C 版 2 頁。

### CE 版 PDF 重匯
線上那份還是 7/26 匯出的**舊赭橘 `#B85C38` 配色**，網頁早已全黑白，下載版與網站不一致。
用 Edge headless + CDP `Page.printToPDF`（`preferCSSPageSize`，吃頁面自己的
`@page { size: A4; margin: 15mm 18mm }`）重匯。

### 舊 PDF 下架
`Tao_Yun_Huang_CV.pdf` 與 `Tao_Yun_Huang_CV_Hardware_Engineer.pdf` 內含
**更正前的 `B.Eng. Electrical Engineering 2010–2013` 與 `B.A. Drama & Theatre Arts`**
——兩個從未取得的學位，而且一直是 git 追蹤中、網址可直接開啟。
`git rm --cached` 後搬到 `job_application\archive\`（檔案保留不刪），
`.gitignore` 補上防止再被加回。

---

## 2026-08-03 · 學歷字串縮短

Profile 頁的 Education 欄 `M.F.A. Multimedia & New Music` → `M.F.A. Multimedia & NM`。
（`cv-experiential.html` 的 `MFA Multimedia Music (New Music Theatre)` 未同步，那是藝術／品牌軌的 CV。）

---

## 2026-08-01 · 轉為 Field Service 語言

面向 8/3 的 Customer Engineer 面試，把簡報站的文案從「作品說明」改寫為「現場工程語言」。
版面與互動一律未動，只改內容。

### Sheet 02 柴電工場：其實是兩件作品
站上原本只寫了 60 kg 電樞，但圖全是透明屏滑台——文圖不符。改為**滑台為主敘事**：
20 m 單軸、承載 500 kg、六片透明螢幕，`AZXM1260MC-PS25`（600 W／25:1 行星減速）
透過 `AZXD-SED` 走 EtherCAT。

核心論述是**影像索引的是絕對編碼器位置，不是時鐘**——玻璃上永遠對得上後方實車的剖面，
誤差不會累積。配兩個決策說明：免電池絕對編碼器＝通電即知位置、20 m 載半噸不必歸原點；
斷電動作型煞車＝E-stop 是夾住而不是滑行。

「變慣量、單組增益跨全行程」的調校經驗歸屬滑台（原本錯放在電樞）。
電樞降為第二件，抽屜裡只留一列 `Also on site`。

### Section 02 能力矩陣：診斷動詞在前
元件級 RCA 提到第一項；`handover` → `handover sign-off`、
`fault diagnosis` → `corrective maintenance & on-site fault isolation`、加 `escalation`。
補進 `EtherCAT`、`Absolute encoder feedback`、`Long-travel high-mass axes`。
刪 `3-D printed`（對 CE 是雜訊），保留 mechanism design。**真空／電漿／液壓仍然不列。**

### 其他
- **Sheet 06 彌散圈**新增測漏（皂液測試＋壓降測試）bullet 與 `Leak check` 規格列；刪 `Audio`。
  原有的 `Not claimed: No vacuum, plasma or hydraulic process experience` 保留。
- **Sheet 01 十三聲部**只改 tag 為 `Analog hardware · Component-level diagnosis · Solo build`，
  內文本來就已是元件級 RCA 語言。
- **Sheet 07 闁**刪 `Scale` 列（與段落重複）、段落縮一行。純減重。
  這一刀讓全站最壞的抽屜從 `w-threshold` 854px 降到 `w-circle` 798px。
- `images/_incoming/README.md` 更新待補清單：schematic、mechanism、LINE OA、兩張證照都已到齊，
  只剩 05 AI Living Lab 與 08 苔域浮層。

### 量測（Edge headless + CDP，動效全開）
1920 寬時全站零溢出，九個抽屜同時打開也不用捲；最壞是 `w-circle` 798px，
**視窗高 ≥ 798px 就保證零捲軸**。

⚠️ 新發現：**抽屜內容高度會隨視窗「寬度」變動**（抽屜寬 `min(49%, 40rem)`，窄視窗多折行）。
1366 寬時 `w-circle` 長到 779px、1280 寬時 804px，在 768/800 高的視窗會溢出 11px／4px。
改寫前更糟（854px，溢出 86px），但仍未歸零。

用戶主螢幕是 1920×1080 無 DPI 縮放，瀏覽器最大化 CSS 高約 880–940（餘裕 82–142px），
**面試按 F11 全螢幕最保險**（餘裕 282px）。窄視窗的溢出留待日後處理。

---

## 2026-07-31 (上線) · 新版成為網站首頁

先前新版一直放在 `deck.html`，網站根目錄仍是舊版，所以 dataraw.tech 打開還是舊的。這次交換：

| 檔案 | 內容 |
|---|---|
| **`index.html`** | **新版簡報站**（原 `deck.html`）— 現在就是 dataraw.tech 的首頁 |
| **`index-classic.html`** | **舊版作品集 Issue 02** — 原封不動保留上線，檔頭有註記說明它是封存版本 |

- `_redirects`：`/deck.html` → `/`（301），先前分享出去的連結不會斷。
- `cv.html` 的「About」原本指向 `index.html#about`，新版的區塊叫 `#profile`，已修正。
- CV 頁 favicon 從 8 MB 的原圖換成 32px 版本。
- 用 `git mv` 換名，兩個檔案的歷史都保留，沒有任何刪除。

---

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
