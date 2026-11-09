---
title: AI 吉祥物動畫：角色表＋狀態表提示詞，做出會回應操作的品牌角色
date: 2026-11-05
tags: [ai/prompt-engineering, ai/claude]
source: https://www.aiposthub.com/ai-animated-mascot-character-states-prompt-guide/（César Álvarez 的 X 教學，經 aiposthub 二手整理，2026-10-23；未驗證）
type: idea
status: processed
asset_value: 12
---

> ⚠️ 單一原作者、二手轉述未驗證；站方明言無法宣稱提升留存或轉換；範本為站方原創；參考知名吉祥物須注意版權。

## T（核心論點）

吉祥物動畫是 UI 狀態的視覺回饋，不是裝飾：先固定角色外觀，再讓每個狀態只做一件事，並把動畫綁在真實事件上。

## B（適用場景）

想讓產品裡的角色（ChatGPT 出形象、Claude 寫成可放進產品的程式）回應使用者操作。

## R（要點）

- 角色先定義單一「情緒工作」，個性寫成可觀察行為
- **角色表**（正面中性／開心／思考／困惑）固定外觀；**狀態表**（待機、處理中、成功、錯誤）各做一件事
- prompt 要寫起點、過程、結束位置；SVG／圖層利於分部位動作
- 驗收：中斷與連點的處理規則、共用基準點、`prefers-reduced-motion`、小螢幕不擋按鈕、隱藏角色仍能看懂結果
- **成功動畫只綁真正收到的成功事件**，不要用計時器假裝成功

## C（關鍵行動）

- [ ] 先寫角色表與狀態表，再寫動畫 prompt
- [ ] 驗收時固定檢查中斷、連點、減少動態偏好三項

## 附帶一行

同批另有「15 秒動態作品集提示詞（Stephan 示範）」：同樣是先給關鍵影格與時間表、確認後才做完整版的模板，與 [[ideas/claude-code-animation-prompt-qa-delivery]] 重疊，不另建條目。

## 相關連結

- [[claude-code-pure-code-animation-practice]] — 含角色動作狀態的 prompt 範本與實測
- [[claude-motion-beta-2026-10]] — Claude Motion 產品面
- [[ideas/claude-code-animation-prompt-qa-delivery]] — 狀態清單＋時間表＋驗收
