---
title: World Labs Atlas：一張圖生成可運鏡 3D 世界的世界模型
date: 2026-09-02
tags: [ai/agent]
source: https://www.aiposthub.com/world-labs-atlas-world-model/；https://techorange.com/2026/09/03/worldlabs-ai-atlas-fei-fei-li-omni-model/
type: note
status: processed
asset_value: 46
---

李飛飛創辦的 World Labs 於 2026-09-01 發表世界模型 Atlas：從 1-6 張參考圖生成最高 1440p、最長 1 分鐘影片，把相機姿態（3D 位置與朝向）直接當模型輸入而非靠文字描述運鏡，同時支援 3D 重建（點雲、3D Gaussian Splats）與機器人 Real-to-Sim 模擬（手機拍片即可建立機器人訓練模擬場）。核心判準：Atlas 是底層世界模型（少數合作夥伴 Early Access、無公開 API、無公開價格），Marble 才是已對外公開的 3D 世界創作產品，兩者不可混為一談；且「生成合理」不等於「物理正確」，機器人應用需另外驗證碰撞/摩擦/材質。產業脈絡：World Labs 已募得 12 億美元資金（投資者含 Nvidia、AMD、Autodesk），競爭對手版圖為 Odyssey（互動世界模擬）、Yann LeCun 旗下 AMI Labs（實體規劃）、Niantic Spatial（地理空間測繪），評測宣稱優於 Gemini Omni Flash 與 FLUX（但評測與比較設定皆由 World Labs 自行發布，需第三方驗證）。本主題偏離知識庫四大關注方向，僅因與 Agent 訓練場景／Real-to-Sim 有間接關聯而輕量收錄；若要深入查原文，技術解析與限制分析看 aiposthub 版，融資與競爭者名單看 techorange 版。

## 相關連結
- 無既有條目直接相關，暫不強制建立 wikilink
