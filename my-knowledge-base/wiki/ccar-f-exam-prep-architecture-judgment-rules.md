---
title: CCAR-F 考試心得：沒有題庫的判斷型考試怎麼準備，以及「錯誤選項四兄弟」架構判斷法則
date: 2026-09-23
tags: [ai/claude, ai/agent, type/how-to]
source: https://yu-wenhao.com/zh-TW/blog/ccar-f-exam-experience/（WenHao Yu，2026-08-29 發布）
type: article
status: processed
asset_value: 64
---

> 作者 WenHao Yu 於 2026-08-24 以 882 分（及格線 720）通過 CCAR-F。本文是其證照總整理（見 [[claude-official-certifications]]）的續篇。考試格式、報名資格、效期與續證等「證照是什麼」的事實，**以 [[claude-official-certifications]] 為主**，本條目只簡列。

## T（核心論點）

**CCAR-F 考的是「症狀相似、機制不同」的架構判斷，用背的沒有用。** 同樣是「規則沒被遵守」，要分辨是規則寫得模糊，還是規則明確但沒有強制手段；同樣是「agent 開始漏東西」，要分辨是 context 快滿、中段內容被稀釋，還是讀進來的資料已經過期。題目的情境外衣一直換，底下的機制就那些。

**最值得重用的法則是「錯誤選項四兄弟」：加更多例子、把描述寫得更精確、換更強的模型、叫它自己檢查——這四種答案幾乎必錯。** 它們都想把 AI 那一步本身變得更可靠，但錯誤率降不到零。正解走另一條路：**接受它會出錯，在它後面加一道攔截**（跑測試、過驗證器、轉給真人）。錯誤還是會發生，但每一次都會被接住，不會靜靜流進下游。

## B（適用場景）

**適用：**
- 準備 CCAR-F，或任何沒有公開題庫、以情境判斷為主的考試
- 🔑 **考試以外**：設計 agent 架構時，判斷一個問題該在 prompt 層修、還是在程式層攔
- 作者一手應試並有模考分數軌跡，來源信任度第 2 級

**不適用 / 注意：**
- 作者建議「先實作，再考」：官方考生畫像是**半年以上實作經驗的 solution architect**，這張證照適合已經在用的人收割，不適合拿來入門
- 🔴 **報名門檻**：原文寫明 CCAR-F 目前只開放 Claude Partner Network 組織成員報名，個人 email 過不了驗證（細節見 [[claude-official-certifications]]）
- 格式簡列：60 題單複選混合、120 分鐘、滿分 1000、720 及格；Pearson VUE 監考（線上 OnVUE 或實體考場），**只有英文**
- 🔴 考試合約（NDA）禁止散布、複製或憑記憶重建任何真題。本條目的兩題示範分別來自**官方公開樣題**與**第三方模考站**，沒有任何真題內容
- 文末作者透過組織協助報名的招攬段落與電子報訂閱，不收錄

## R（操作步驟）

### 一、備考流程

1. **知識來源**：日常實作為底；系統補課用 Anthropic Academy 四門免費課（Claude Code in Action、Introduction to Model Context Protocol、Introduction to Agent Skills、Claude with the Anthropic API）；剩下的洞在檢討錯題時回官方文件補。
2. **自己出題**：官方素材只有 Exam Guide（domain 配分、objective、**12 題附解析的樣題＋4 組 Preparation Exercises**，在認證頁免註冊即可下載）。把 Exam Guide 逐條讀完，用 Claude 依官方樣題的出題邏輯與配分生仿真題；全真模考比照 60 題、120 分鐘、單螢幕、不查資料。
3. **錯題檢討五欄**：這題考哪個機制／我選了什麼／**當時為什麼那樣選**／正解的機制是什麼／我的理解斷在哪裡。問 Claude 時不問「答案是什麼」，而是問「這題在測什麼機制、我的理解哪裡斷了」。
4. **時間差重測**：當天重測不算（那是短期記憶）；隔幾天、選項洗牌後再測還對，才算留下來；隔兩三週還對就可以放心。目標不是錯題清零，是同一型的錯不再犯。
5. **三關準備度**：① 自建的分區練習全過 ② 外部題庫穩定 900 以上 ③ 錯題本每一題都講得出正解機制與每個錯誤選項錯在哪。三關都過才預約，不要拿正式考試當模考。
6. **英文閱讀是隱形的第二考科**：每題約 2 分鐘預算，作者一開始光讀懂一題就要 4–5 分鐘，後來練到考完還剩 15 分鐘。準備時就要用英文練。

### 二、5 個 domain 與各自的判斷原則

| Domain | 配分 | 原則 |
|---|---|---|
| Agentic Architecture & Orchestration | 27% | 要確定性就用 hook 在程式層擋，不要靠 prompt |
| Tool Design & MCP Integration | 18% | 工具描述是 agent 選工具的主要依據，描述不清事後用 prompt 救不回來 |
| Claude Code Configuration & Workflows | 20% | 規範放 CLAUDE.md、路徑條件 rules、skill 還是 hook，看「它該在什麼時候、對誰生效」 |
| Prompt Engineering & Structured Output | 20% | 要鎖死格式用 `tool_use`＋JSON schema；要一致性給 few-shot |
| Context Management & Reliability | 15% | 升級轉真人看明確訊號（客戶明說、政策有缺口），不看 agent 對複雜度的自我感覺 |

### 三、另外兩條刷選項法則

- **題幹寫「A team member proposes X」，X 幾乎必錯**：提議如果就是正解，這題沒有出題價值。
- **看似多餘的限定條件是拿來刪選項的**：例如「context window 還沒滿」，唯一目的是刪掉所有衝著容量問題去的選項（壓縮、摘要類）。

### 四、兩題解析

1. **官方樣題 Q6**（不同區域有不同 coding convention，測試檔散落各處）：正解是在 `.claude/rules/` 建規則檔、用 YAML frontmatter 的 glob pattern 依檔案路徑條件套用。考點是「一條規則何時該用路徑條件自動套用、何時該放進永遠載入的設定檔」。其餘選項：全塞根目錄 CLAUDE.md 靠模型自己猜（不保證）、做成 skill（要手動叫，與「自動套用」矛盾）、用資料夾綁規則（處理不了散落多目錄的檔案）。
2. **CCG 模考題**（session 已跑 45 分鐘，早期讀過的檔案被同事改了）：正解是**開新 session、帶著關鍵發現與決定的摘要，再重讀變更的檔案**。作者兩次都選「在原 session 叫它重讀」，錯在機制：**context 只能追加，不能改也不能刪**，重讀是把新版疊上去，新舊兩份並存，agent 之後可能又引用過期那份。開全新空白 session 會丟掉 45 分鐘的有效理解；`fork_session` 是從已經髒掉的狀態分岔，設計給「同一起點平行試兩種做法」，不是拿來清過期資料。作者的結論：「考試要的是機制上最乾淨的答案，不是實務上混得過去的答案。」

### 五、作者經驗（個人自述，非官方）

- 在 Claude Certification Guide（CCG，claudecertificationguide.com）考了五場全真模考：766 → 846 → 901 → 903 → 915；實考 882（2026-08-24）。作者自述**與該站沒有合作關係**，推薦理由是模考與實考落點對得上。
- 作者提醒：刷題刷到後來容易變成死記，題庫的角色是驗證和補洞。

## C（關鍵行動）

- [ ] 遇到「AI 那一步不夠可靠」時，先檢查自己的解法是否屬於四兄弟（加例子／寫更細／換大模型／自我檢查）；是的話改想「後面要加哪一道攔截」
- [ ] 對「規則沒被遵守」類問題，先分辨是規則模糊還是缺強制手段，再決定改文字還是改用 hook
- [ ] context 出現過期資料時，開新 session 帶摘要，不要在原 session 叫它重讀
- [ ] 若要報考，先確認所屬組織是否為 Claude Partner Network 成員（見 [[claude-official-certifications]]）
- [ ] 準備判斷型考試時，錯題紀錄一定要寫「當時為什麼那樣選」，並用時間差重測驗收

## 原始摘要

WenHao Yu 記錄自己準備與應考 CCAR-F（Claude Certified Architect – Foundations）的完整過程。考試沒有考古題，官方只給一份 Exam Guide，所以他用 Claude 依官方權重出仿真題，搭配固定格式的錯題檢討、時間差重測與三關準備度來驗收。從錯題中歸納出三條刷選項法則，其中「錯誤選項四兄弟」最有遷移價值。文章也記錄了 5 個 domain 的判斷原則、兩題示範解析、線上與實體考場的規定，以及效期與續證機制。考完後作者最大的收穫是：以前靠手感的架構判斷，現在說得出機制。

## 相關連結

- [[claude-official-certifications]] — 🔑 **證照事實的主條目**（格式、報名限制、效期、續證）；本篇是同作者續篇
- [[wenhao-yu-ai-workflow]] — 同一作者的個人 AI 工作流系統
- [[anthropic-academy-courses]] — 作者實際採用的四門免費課
- [[agent-control-layer-permission-hooks-planning]] — 「要確定性就放程式層」：Permission／Hook 控制層
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — Prompt 是期待，不是保證
- [[steering-claude-seven-carriers]] — 規範放哪一層（對應 Domain 3）
- [[tool-description-and-error-message-as-prompt]] — 工具描述是 agent 選工具的依據（對應 Domain 2）
- [[agent-context-management]] / [[context-isolation-and-compaction]] — context 只能追加、過期資料的處理
- [[boris-cherny-prompt-to-verification]] / [[ai-era-quality-assurance-test-oracle]] — 「在後面加一道攔截」的驗證思路
