# 軌道列車模擬器 Train Track Simulator

https://maxxkao.github.io/train-game/train-game.html

一個完全在瀏覽器裡執行的 3D 列車駕駛 / 軌道編輯模擬器,單一 HTML 檔案、不需要安裝任何東西,用 Three.js 打造。
A fully browser-based 3D train-driving / track-editing simulator — a single self-contained HTML file, no install required, built with Three.js.

![License](https://img.shields.io/badge/license-%C2%A9%202026%20Maxx%20Kao-blue)

---

## 🎮 如何使用 / How to Run

**中文**
1. 下載 `train-game.html`
2. 直接用瀏覽器打開(建議 Chrome / Edge / Firefox 最新版)
3. **重要:請不要透過任何內建的線上 HTML 預覽器開啟**(例如聊天工具內建的預覽視窗),部分沙盒環境會封鎖瀏覽器的 `fetch()` API,導致列車模型無法載入。請直接雙擊檔案,或用瀏覽器「開啟檔案」的方式載入,在你自己電腦上的瀏覽器執行才能看到完整效果(貼圖、環境光遮蔽等)。

**English**
1. Download `train-game.html`
2. Open it directly in a real browser (latest Chrome / Edge / Firefox recommended)
3. **Important: do not open it inside a sandboxed in-app HTML preview** (e.g. a chat tool's built-in preview pane) — some sandboxes block the browser's `fetch()` API, which silently breaks the train model loading. Double-click the file or use your browser's "Open File" to run it locally for the full experience (textures, ambient occlusion, etc.).

No build step, no server, no dependencies to install — it's one `.html` file.

---

## ✨ 功能特色 / Features

### 🛤️ 軌道編輯器 / Track Editor
- **中文**:即時軌道編輯器,支援直線與彎道零件(比照真實 KATO Unitrack 規格的長度/半徑),可調整坡度做出高架橋段。編輯的同時列車照樣能開,不需要切換畫面。
- **English**: Real-time track editor with straight and curve pieces (matching real KATO Unitrack lengths/radii), adjustable grade for elevated sections. You can edit and drive at the same time — no mode switch needed.

- **一鍵對接 / One-click auto-connect**:自動算出一段彎道-直線-彎道組合,把軌道尾端精準接回起點(用數值方法求解 Curve-Straight-Curve 路徑規劃問題)。
  Automatically solves a curve-straight-curve connector (a numerically-solved Dubins-path problem) to snap the open end of a custom track back to the start.

- **匯出 / 匯入軌道 / Export & Import**:把自訂軌道與月台配置存成 JSON 檔案,下次可以重新載入或分享給別人。
  Save your custom track + platform layout to a JSON file, reload it later or share it with someone else.

### 🚆 列車 / Train
- 山手線 E235 系電車車型(4 節編組),車體在 Blender 中建模,含真實比例、車門、扶手、車頭造型等細節,搭配程式生成的刷紋金屬 / 髒污貼圖。
  JR Yamanote Line E235-series EMU (4-car set), modeled in Blender at true scale — doors, grab poles, cab nose detail — with procedurally generated brushed-metal / grime textures.
- 駕駛室視角 + 上帝視角,可即時切換;支援油門/煞車/倒退、鳴笛。
  Cab view + free "god view" camera, switchable anytime; throttle/brake/reverse + horn.
- 自動行駛模式(免持續按鍵,列車自動巡航)。
  Auto-play mode (train cruises automatically, no key-holding required).
- 停車誤差判定 + 月台上的實體停止線標記,練習精準停車。
  Stop-accuracy scoring, plus a physical stop-line marker painted on the platform so you can see the target.

### 🌆 場景 / Scenery
- 城市街區與綠地公園分區自動生成(建築物、樹木皆有安全間距檢查,不會互相穿插)。
  Auto-generated city blocks and parks (buildings and trees are collision-checked against each other and the track, so nothing overlaps).
- 高架橋(橋墩 + 橋面板)、鋼構桁架橋、電車線杆與接觸線、隔音牆。
  Elevated viaduct (piers + deck), a steel truss bridge segment, catenary poles + contact wire, noise barriers.
- 月台雨棚(含支撐柱、售票機、告示牌)、路燈,晨昏光影循環,夜間車窗/路燈自動發光。
  Platform canopy (with support columns, a vending machine, an info sign), lamps, a day/night lighting cycle, windows and lamps glow automatically at night.

### 🎨 美術 / Visuals
- PBR 材質貼圖(車身、地面、道碴、建築牆面、月台),程式生成、不依賴外部圖庫。
  PBR texture maps (train body, ground, ballast, building walls, platform), all procedurally generated — no external asset library needed.
- 動態陰影、色調對映(ACES Filmic)、環境反射貼圖,並跟著晨昏切換。
  Dynamic shadows, ACES Filmic tone mapping, an environment reflection map that re-generates with the day/night toggle.
- 真正的螢幕空間環境光遮蔽(SSAO)後製效果。
  Real screen-space ambient occlusion (SSAO) post-processing.

### 🔊 音效 / Audio
全部即時用 Web Audio API 合成,沒有外部音檔:行駛滾動聲隨速度變化、轉向架式雙輪軸「喀噔」過軌聲、雙音調氣笛、煞車嘶聲。
Everything is synthesized live via the Web Audio API — no sound files: speed-linked rolling rumble, bogie-style paired "clack-clack" rail joints, a two-tone air horn, brake hiss.

---

## 🛠️ 技術細節 / Technical Notes

- **引擎 / Engine**: [Three.js r128](https://threejs.org/)(WebGL)
- **列車模型 / Train model**: 在 Blender 中建模,匯出 glTF (.glb),透過內嵌的 `GLTFLoader` 讀取(原始碼直接內嵌在 HTML 裡,不透過任何第二個外部連結載入,避免某些環境封鎖 CDN 或 `fetch()` 造成模型讀取失敗)。
  Modeled in Blender, exported as glTF (.glb), loaded via an inlined `GLTFLoader` (its source is embedded directly in the HTML — no second external host — to avoid environments that block CDNs or `fetch()`).
- **後製 / Post-processing**: `EffectComposer` + `SSAOPass`,同樣內嵌在檔案裡。
  `EffectComposer` + `SSAOPass`, also inlined.
- **貼圖 / Textures**: 用 Python(NumPy/PIL)生成的程序化紋理,以 base64 內嵌,透過標準 `TextureLoader` 載入(不經過會呼叫 `fetch()` 的圖片解碼路徑)。
  Procedural textures generated with Python (NumPy/PIL), embedded as base64, loaded via the standard `TextureLoader` (a path that doesn't route through `fetch()`).
- 整個專案是**單一 HTML 檔案**(約 4MB,大部分是內嵌的模型與貼圖資料),沒有建置流程、沒有相依套件安裝步驟。
  The whole project is a **single HTML file** (~4MB, mostly embedded model/texture data) — no build step, no dependency installation.

---

## ⌨️ 操作方式 / Controls

| 按鍵 / Key | 功能 / Action |
|---|---|
| ↑ / W | 加速 / Accelerate |
| ↓ / S | 煞車,煞停後持續按著會倒退 / Brake — hold after stopping to reverse |
| H | 鳴笛 / Horn |
| N | 切換晨昏 / Toggle day–night |
| Esc | 駕駛室視角返回上帝視角 / Return to god view from cab view |

畫面上的按鈕也可以切換視角、開關聲音、開關自動行駛。手機/觸控裝置有對應的觸控按鈕。
On-screen buttons also toggle camera view, sound, and auto-play. Touch controls are provided for mobile/touch devices.

---

## ⚠️ 已知限制 / Known Limitations

- **中文**:檔案較大(貼圖與模型皆內嵌),第一次載入需要一點時間;目前貼圖為程式生成的風格化材質,不是真實拍攝或掃描的素材。
- **English**: The file is fairly large (textures and model are embedded), so first load takes a moment; textures are procedurally generated / stylized, not photographed or scanned assets.

---

## 📄 授權 / License

© 2026 Maxx Kao. All rights reserved.
