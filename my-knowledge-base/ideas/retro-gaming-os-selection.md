---
title: 復古遊戲機／主機改造系統選型（Batocera・RetroPie・Bazzite・SteamOS）
date: 2026-08-18
tags: [dev/architecture]
source: 12 篇來源合併（見下方來源對照）
type: idea
status: processed
asset_value: 30
---

⚠️ **非本知識庫關注方向**（AI 工具／PKM／Flutter・Spring Boot／知識資產化），依 2026-08-20 的處理決定，
把 raw/ 積壓的 12 篇壓成這一條選型筆記，**保留比較價值但不擴張知識庫定位**。
⚠️ 多篇來源為機器翻譯品質，術語混亂（例如把 Batocera 譯成「巴托塞拉」），**細節請回查官方文件**。

## 🔑 第一個分流：你要「復古模擬」還是「現代 Steam 遊戲」

這是所有選擇的前提，選錯方向後面都是白工：

| 目標 | 走這條 |
|---|---|
| **玩老遊戲**（紅白機／街機／PS1 等模擬）| **Batocera** / **RetroPie** / **EmuELEC** |
| **玩現代 Steam 遊戲**（把舊 PC 變主機）| **Bazzite** / **SteamOS** / ChimeraOS |

## 復古模擬這一側

| 系統 | 主要硬體 | 定位 |
|---|---|---|
| **RetroPie** | Raspberry Pi（4.6 起支援 Pi 4）；也支援 ODroid C1/C2、PC | 建立在 **Raspbian + EmulationStation + RetroArch + Kodi** 之上的整合專案；**附多種配置工具**，進階使用者可深度客製 |
| **Batocera Linux** | SD 卡即開即用（x86 舊筆電、Raspberry Pi、Steam Deck 皆可）| 專注**模擬器與主機平台的廣度**；⚠️ 特色是**不必動原本的硬碟**，插 SD 卡開機即用 |
| **EmuELEC** | ARM 電視盒（例如 N1）| 把便宜的電視盒變成模擬主機 |

🔑 **RetroPie 與 Batocera 的實務差異**：RetroPie 偏「可深度調整、社群資源多」；Batocera 偏「開箱即用、不動原系統」。
**想先試試看 → Batocera（SD 卡）；打算長期折騰 → RetroPie。**

## 現代 Steam 遊戲這一側

| 系統 | 定位 |
|---|---|
| **Bazzite** | 把**舊 PC 改造成「窮人版 Steam Machine」**（Steam Machine 官方硬體太貴的替代方案）；可裝 KDE 桌面 ＋ 中文輸入法 ＋ Steam 遊戲模式，**兼具桌面與主機兩種用途** |
| **SteamOS** | Valve 官方系統；可與 **Windows 雙系統**並存並共用遊戲磁區 |
| ChimeraOS | 安裝後**直接進 Steam Big Picture**，開箱即為主機體驗（來源文中提及，未深入）|

## 其他

- **Steam Deck 上裝 Batocera**：可單系統或雙系統（讓掌機同時能跑復古模擬）
- **自製大型街機台**：有一篇是「用 GPT 協助設計街機台」的心得（硬體木工＋按鈕配置為主，非系統選型）

## 來源對照（12 篇）

Batocera ×4（介紹／安裝指南／SD 卡舊筆電 Batocera plus 2.0 中文懶人包／Steam Deck 安裝）、
Bazzite ×3（改造舊 PC／遊刃 Z 安裝全紀錄／KDE＋中文輸入法教學）、
RetroPie ×2（Raspberry Pi 介紹／設定篇）、
EmuELEC+N1 ×1、SteamOS+Windows 雙系統 ×1、GPT 協助自製街機台 ×1。

## 相關連結

- [[ideas/home-assistant-pixel-floorplan-dashboard]] — 同批素材裡唯一被判定為技術內容而單獨保留的一篇
