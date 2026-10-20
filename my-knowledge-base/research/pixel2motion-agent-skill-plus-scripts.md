---
title: Pixel2Motion：「Agent Skill + 確定性腳本」的分工樣本與可量化 Motion QA
date: 2026-08-27
tags: [ai/claude, ai/agent, type/how-to]
source: https://tools.wingzero.tw/articles/4038
type: note
status: processed
asset_value: 48
---

## 核心論點

Pixel2Motion（nolangz/pixel2motion，MIT）表面是「PNG Logo → SVG → 品牌動畫」的開源 Skill，真正值得記的是它的架構取捨：**AI 只負責需要設計判斷的部分（理解 Logo 結構、規劃動畫個性與編排），能由程式確定執行的 Rendering、Overlay、Frame Capture 全部交給 `scripts/`**——並且把一般靠「播幾次憑感覺」的動畫驗收，做成可重現的量化 QA。

## 重點整理

- **完整鏈路**：`Raster Logo → Vector Fitting → Geometry QA → Motion Brief → SVG Choreography → Motion Frames → Continuity QA → HTML/GIF/Video`，而不是「圖片 → AI → 動畫」
- 🔑 **向量化不是目的**：多數 Raster-to-SVG 工具追求「像不像原圖」，Pixel2Motion 同時要求「這個 SVG 好不好做動畫」。**3 個具語意的部分很容易編排動畫；400 個沿像素邊緣 trace 出來的節點即使靜態接近原圖也做不出乾淨 Motion**
- ⚠️ **IoU 是診斷指標而非門檻**：官方刻意不設固定 IoU 標準，真正的 Hard Gate 是 Smoothness 與 Structure——高 IoU 但滿是鋸齒節點，會輸給較簡潔的曲線結構
- **Final Frame Contract**：動畫過程可以 Reveal / Draw-on / Mask / Scale，但**結束後必須準確回到已通過 QA 的靜態 SVG**，避免為視覺效果無意間改掉 Logo 的最終比例與幾何
- **可量化的 Motion QA**（最可遷移的一點）：用腳本在指定毫秒（0、300、700、1000、1250、1500 ms）擷取畫面排成 `motion_strip.png`；涉及 Draw-on / Path Crossing / Mask 時再用 `probe_motion_continuity.py` 檢查 `stroke-dashoffset`、`offset-distance` 等屬性，抓動畫瞬間跳動或斷線
- **Repo 結構即分工**：`SKILL.md` 定義主流程、`references/` 放動畫原則與 Motion Personality／Reveal Pattern／HTML Template、`scripts/` 放 Tracing／Rendering／Overlay／SVG Path Audit／Frame Capture／Continuity Probe
- **產出物**：`logo.svg`、`motion.css`（依 SVG Semantic ID 編排）、`logo_motion.html`（免套件，含 Replay／Slow Motion／Speed Control／QA Hooks）、`motion_spec.md`（Motion Brief、Timeline、Easing、QA）、`fit_iterations`、`motion_frames`、`motion_strip.png`
- **設計紀律**：把 Disney 12 Principles of Animation 納入 Motion Discipline，要求先寫 Motion Brief 定義品牌個性、使用情境、Logo Parts 與 Choreography，再開始動畫——科技品牌與兒童品牌不該套同一組 Timing
- **環境需求**：Python 3.10+、Pillow、NumPy、Chrome/Chromium（Geometry 與 HTML Rendering）、Playwright（可重現的 Frame Capture）
- **交付形態的差異**：傳統 Motion Designer 交 MP4／GIF，放進網站後失去互動性；這裡交的是具語意結構的 SVG + CSS + HTML，前端可繼續控制播放時機、速度與 Hover

## 可用之處

拿來當**寫 Skill 的架構參考**：哪些步驟該留給模型判斷、哪些該下沉成確定性腳本，這篇給了一個界線清楚的實例——正好呼應 [[research/mcp-server-evaluation-criteria]] 的「能由程式確定完成的事就交給程式，不要每件雜務都丟給模型猜」。「Final Frame Contract」與「固定時間點取樣做視覺 QA」兩個手法也可遷移：前者是「無論中間怎麼跑，結果必須回到已驗收狀態」的通用約束寫法，後者是把主觀驗收轉成可重現檢查的做法。

## 相關連結

- [[research/mcp-server-evaluation-criteria]] — 「確定性的事交給程式」在 MCP 設計上的同一原則
- [[agent-skill-design-patterns]]
- [[claude-code-design-skills-overview]]
- [[research/marketingskills-agent-skills-collection]] — 另一個 Skill 集架構樣本（base skill + 交叉引用）
- 📎 2026-10-19：[[ideas/claude-code-animation-prompt-qa-delivery]] — 🔑 互補：「由時間算畫面」才能逐張重現，本條的 Final Frame Contract 與固定毫秒取樣正是它的驗收手段
- 📎 2026-10-19：[[claude-code-pure-code-animation-practice]] — rar.design 的截圖總表（8～10 個關鍵時間點）＋12 格膠卷自查，是固定時間點取樣 QA 的實作例；matter.js 先算軌跡、反應擴散存快照也屬「AI 判斷、腳本確定執行」
