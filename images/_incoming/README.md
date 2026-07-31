# 待補素材清單

**最後更新：2026-08-01**

新素材丟這個資料夾，我會壓縮並歸位到 `works/NN-*/`。
檔名請標明屬於哪個專案（例如 `05-單車感測器.jpg`），中文檔名可以，我會轉換。

---

## 🔴 還缺的（只剩兩個專案）

這兩張圖紙的左側大圖只有一張,不會輪播——整份簡報只有這兩頁是靜止的。

| 專案 | 需要幾張 | 拍什麼最有用 |
|---|---|---|
| **05 AI Living Lab**（`works/05-ai-living-lab/`） | 2–3 | **自製感測單車的硬體特寫**——感測器裝在哪、怎麼固定、走線。這是「感測器整合」這句話目前唯一缺的證據；另可補民眾騎乘中、印出來的卡帶明信片成品 |
| **08 苔域浮層**（`works/08-moss/`） | 1–2 | **喇叭佈點圖或現場走線**，撐「多聲道系統設計」；或裝置完成照的另一個角度 |

檔名：`detail-1.jpg`、`detail-2.jpg`、`detail-3.jpg`

---

## ✅ 已完成，不用再給

- ~~UWA / UFSP 證照~~ → 官方數位版已上線（`certs/`），在 02 Capability 那張投影片輪播
- ~~Blender MCP 素材~~ → 8 張已歸位，含 `bridge.jpg`（bridge server 原始碼畫面）
- ~~十三聲部電路圖~~ → `layout.jpg`（Cadence Allegro）+ `cam.jpg`（CAM/Gerber）
- ~~柴電機構圖~~ → `mechanism.webp`（線性軸 CAD 動態模擬，由 21 MB mp4 轉檔）
- ~~文化科技交易所 LINE OA~~ → `loop-1/2.webp`（實機操作動畫）+ `flow.jpg`（LINE Bot 流程圖）
- ~~彌散圈 detail~~ → 3 張 VR 擷取

---

## 命名與處理

- 每個專案資料夾：`hero.jpg` 主視覺、`detail-1.jpg` `detail-2.jpg`… 支援圖
- 特殊用途用語意檔名：`layout` `cam` `mechanism` `bridge` `flow` `loop-N`
- 全小寫、連字號、**不要空格 / 括號 / 大寫副檔名**
- 原圖直接丟，不用自己壓縮（會統一縮到長邊 2000–2400px、JPEG q85；
  GIF/MP4 會轉成 animated WebP）
- 原始檔會移到 `_archive/`（已 gitignore，不會上線），**不會刪除**
