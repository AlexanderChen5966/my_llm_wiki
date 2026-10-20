---
title: "一套 Claude Code 的 subagent 團隊定義:8 個角色、一份派工協定 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402231"
author:
  - "[[iThome]]"
published:
created: 2026-09-07
description: "去年 11 月我寫了一套 Claude Code 的 subagent 定義，8 個角色、12,069 行。我當時很滿意——每個角色我都把能想到的情境寫進去了。 今年 8 月我把它重寫，同樣 8 個角..."
tags:
  - "clippings"
status: processed
---
1

## 一套 Claude Code 的 subagent 團隊定義:8 個角色、一份派工協定

- 分享至

去年 11 月我寫了一套 Claude Code 的 subagent 定義，8 個角色、12,069 行。我當時很滿意——每個角色我都把能想到的情境寫進去了。

今年 8 月我把它重寫，同樣 8 個角色，1,239 行。 **少了 90%，但它現在才真的能用。**

這篇是那次重構的紀錄，也是我對「寫給 AI 的規則」這件事的認知修正。

---

## 1\. 專案資訊

**名稱**:AI\_Agent\_For\_Full\_Stack\_Engineer(Claude Code Subagent 團隊定義)

**簡介**:一套「主 agent 派工、subagent 執行、PM 決策」的開發團隊定義，供 Claude Code 全域使用、跨專案共用。核心不是 8 個角色檔，而是一份 **派工協定** (`_PROTOCOL.md`)——規定任務書長什麼樣、回報長什麼樣、什麼叫做完、卡住了怎麼辦。

**目前狀態**:v2 已完成並在自己的專案上實際使用中(2026-08 重構完成)。持續依實際踩到的問題修正。

**連結**:

- GitHub:https://github.com/timwei0801/AI\_Agent\_For\_Full\_Stack\_Engineer
- 安裝就是 clone 到 Claude Code 的 agents 目錄，沒有別的步驟:
```bash
git clone https://github.com/timwei0801/AI_Agent_For_Full_Stack_Engineer.git ~/.claude/agents
```

> 圖說:8 個角色檔 + 一份共同協定，就是全部了

---

## 2\. 開發動機:不是「Claude 不會寫 code」，是「它每次都用不同的方式交件」

先講清楚 subagent 是什麼:Claude Code 可以開子代理(subagent)去做事，每個 subagent 有自己獨立的 context，做完把結果回報給主 agent。好處是主 agent 的 context 不會被一堆檔案內容塞爆。

我一開始的用法很單純:寫幾個角色檔，需要時叫它們。問題是實際跑起來，我一直在處理同一批狀況:

- **交件格式每次都不一樣** 。這次回報有貼測試結果，下次沒有；這次有列假設，下次它自己決定了也不說。我沒辦法機械式地讀完回報就知道要不要送審。
- **它會順手多做事** 。叫它加一個 provider，它順便重構了旁邊的模組——出發點是好的，但我沒辦法審一個範圍不明的 diff。
- **它會停在半路等我** 。回報寫「目前進度 60%，請確認方向後我再繼續」。問題是 **subagent 是一次性的，它只有一次最終回覆，沒有中途對話** ——那句「請確認」永遠不會有人回，那次派工就等於作廢。
- **它會告訴我測試過了，但沒有真的跑** 。

這些都不是模型能力問題，是 **規範問題** 。我缺的不是更聰明的 agent，是一份所有角色共同遵守的協定。

---

## 3\. 技術架構:三層決策 + 一份協定 + 8 個角色

### 三層角色

- **PM = 使用者(我)** — 需求方與最終決策者。預算、範圍、方向、不可逆的操作，只有我能拍板。
- **主 agent** — 專案的開發主導人。它讀需求、拆任務、派工、整合回報、決定何時開 PR。
- **subagent** — 一次性的執行者。只跟主 agent 對話，不能聯絡其他 subagent，也不能直接找 PM。

這個「只跟主 agent 對話」的限制很重要:它強迫所有跨角色的協調都走主 agent，資訊流是一棵樹而不是一張網。需要別的角色配合的事、需要 PM 決策的事，一律寫進回報讓主 agent 轉達。

### 檔案結構

```bash
~/.claude/agents/
├── _PROTOCOL.md          ★ 派工協定:所有 agent 共同遵守
│                           (任務書格式、統一回報格式、DoD、阻塞協定、
│                            scope 鎖、一次性回覆、產出路徑、git 規範)
├── product-manager.md    PRD
├── tech-lead.md          架構／技術選型／schema／任務拆解／ADR
├── research-analyst.md   技術、資料源、市場、競品調研
├── backend-engineer.md   所有非 UI 程式碼(服務端、資料管線、腳本、ML)
├── frontend-engineer.md  UI 層(web、儀表板、TUI、通知模板)
├── qa-code-reviewer.md   實跑測試＋逐檔審查，只審不修
├── devops-engineer.md    容器化、CI、部署、排程、密鑰、備份、監控
└── risk-manager.md       專案健康度與風險(讀狀態文件與 issues,不訪談人)
```

配套是每個專案根目錄一份 `CLAUDE.md` (專案憲法)與 `docs/PROJECT_STATE.md` (交接文件)。  
**衝突時專案 `CLAUDE.md` 優先於全域協定** ——這樣同一套 agents 才能跨專案共用。

---

## 4\. 技術難題一:12,069 行的角色定義，讓 Claude 變笨

### 我當初為什麼會寫到 12K 行

v1 的心態是「把所有可能情境都寫進去」。 `tech-lead.md` 2,518 行、 `devops-engineer.md` 2,343 行——裡面有 K8s 部署範例、有電商系統的架構樣板、有完整的 web 全端流程。我以為寫得越完整，它做得越好。

### 實際發生的事

**症狀一:context 被自己的定義吃掉。** subagent 一開工先讀 2,000 行自我介紹，真正該讀的專案設計文件反而排在後面、權重被稀釋。

**症狀二:角色重疊導致派工錯誤。** 8 個檔案各自獨立成長，同一條規則(git 怎麼開分支、commit 訊息怎麼寫、測試要不要真的跑)在 6 個檔案裡各寫了一次——而且 **寫得不完全一樣** 。backend 說 commit 用繁中，devops 那份還留著英文範例。Claude 讀到互相矛盾的規則時不會報錯，它會挑一個。

**症狀三:預設了我沒有的技術棧。** 一堆 web／電商／K8s 的範例，讓它在一個純 Python 資料管線的專案裡建議我加 Ingress。

### 解法:抽出共同規則，角色檔只寫「與眾不同的部分」

重構的核心動作只有一個—— **把所有角色共用的規則抽進 `_PROTOCOL.md` ，角色檔第一句話都改成「先讀協定」**:

> 開工前先完整閱讀 `~/.claude/agents/_PROTOCOL.md` (派工協定)。本檔只描述你這個角色與眾不同的部分;協定寫過的規則不在此重複，兩者衝突以專案 `CLAUDE.md` 為準。

結果:

|  | v1(2025-11) | v2(2026-08) |
| --- | --- | --- |
| 8 個角色檔總行數 | 12,069 | 1,239 |
| 單一角色檔 | 712 ~ 2,518 行 | 150 ~ 169 行 |
| 共同規則 | 散在 8 個檔案，版本不一致 | 集中在 `_PROTOCOL.md` (140 行) |
| 技術棧預設 | web／電商／K8s | 無(由專案 `CLAUDE.md` 帶入) |

![https://ithelp.ithome.com.tw/upload/images/20260831/20182796NQ3nKC0RHJ.png](https://ithelp.ithome.com.tw/upload/images/20260831/20182796NQ3nKC0RHJ.png)

**這裡真正的教訓不是「文件要精簡」，是「規則不能有兩份」。** 12K 行本身不是問題，同一條規則在 6 個地方有 6 個版本才是。抽 `_PROTOCOL.md` 之所以有效，是因為它讓每條規則只有一個定義位置——跟寫程式時把重複邏輯抽成函式是同一件事。

---

## 5\. 技術難題二:我寫了一堆 subagent 永遠讀不到的指令

這是 v1 最致命、也最不明顯的 bug。

v1 的角色檔裡有大量這種模板:

- 「完成第一階段後，向主 agent 回報進度並等待確認」
- 「若需求不明確，請先詢問 PM」
- 「回報進度 60%」

**這些指令在 subagent 身上完全不成立** 。subagent 是一次性的:一次派工、一次最終回覆，中間沒有對話回合。它照著我的模板寫了「請確認後我再繼續」，然後就結束了——那次派工什麼都沒產出，我還要重派一次。

我寫這些模板的時候，腦子裡想的是「跟 Claude 對話」的體驗。但 subagent 不是對話，它比較像 **送出一個 job 然後拿回一份 report** 。

### 解法:把「你是一次性的」寫進協定第 0 節

```markdown
你是**一次性**的：這次任務只有一次最終回覆，沒有中途對話。
不要寫「請確認後我再繼續」「回報進度 60%」「等待指示」
——那些永遠不會有人回。能做的做完，不能做的說清楚，一次交齊。
```

配套是把「不確定怎麼辦」講死，讓它不需要問也能動:

> **不腦補需求。** 任務書不清楚時:先做不依賴那個答案的部分，其餘選一個保守假設完成，把假設寫清楚。只有在「任何假設都會讓工作作廢」時才停下來，交出 partial 產出＋明確問題。

以及一個統一的回報格式，狀態只有三種:`DONE` / `PARTIAL` / `BLOCKED` 。

```markdown
## 回報：<角色> — <任務一句話>

<選填：一段不超過五行的摘要>

### 狀態
DONE

### 產出
- <路徑>：<一句話說明>（commit：<hash> on <branch>）

### 驗證
- 測試：<指令> → <N passed / N failed，或「未執行：原因」>
- lint／型別：<結果>

### 假設
- <未確認就採用的假設，或「無」>

### 待決事項（需主 agent／PM 拍板）
- <問題＋你的建議選項>

### 發現與建議（scope 外，未動手）
- <bug／風險／技術債；建議開 issue 的標明>

### 未完成
- <PARTIAL／BLOCKED 時：缺什麼、為什麼、建議下一步>
```

「用不到的欄位寫『無』，不要刪欄位」——這句也要明寫，否則它會把空欄位整段刪掉，主 agent 就沒辦法機械式地讀。

### 這個格式我改了三版才對——中間那版的失敗最有價值

第一版的狀態長這樣，寫在標題正下方、當作一個裸行:

```markdown
## 回報：<角色> — <任務一句話>

**狀態**：DONE | PARTIAL | BLOCKED
```

它不聽。我用同一份任務書派了兩次，兩次都把那行改寫掉——第一次變成「## 摘要」，第二次變成「## 結論」加一句「完成。」。中間我還特地在協定裡加了三條 **逐字要求** ，明寫「不要寫成『完成。』」「不要變成 `## 狀態` 區塊」。

結果很有意思:**三條裡只有一條生效** 。管欄位層級的那條（一律 `###` 、名稱照抄、額外欄位附加在後面）一次就聽，所有欄位立刻歸位、連空的 `### 未完成` 都會乖乖寫「無」。但管開頭那兩行的兩條，寫得再白都沒用。

想通之後答案就浮出來了:**模型在開口的第一句服從「先講結論」的習慣，那是它最強的行為傾向之一，一段散文規則蓋不過去。** 但只要進到「欄位」這個結構裡，它就照做。

所以我不再對抗它，改成順著它設計:

- 狀態從裸行 **搬進 `### 狀態` 欄位** ，內容限定 `DONE` / `PARTIAL` / `BLOCKED` 一個字
- 它兩次都想在開頭寫摘要——那就 **明文允許** 標題後放一段五行內的摘要，給它一個合法位置
- 主 agent 那端改成 **寬鬆解析**:抓 `### 狀態` 後面第一個出現的那個詞，不要求整份逐字相符

第三次派工，標題、狀態、欄位順序、額外欄位，全數符合。

**這件事的教訓比格式本身重要**:寫給 AI 的規則，不是寫得越嚴越有效。你要先分清楚哪些是它「結構上會遵守」的（欄位、層級、清單），哪些是它「行為傾向壓得過規則」的（開場白、語氣、先講什麼）。前者用規則管，後者要順著設計，或者在讀取端容錯。

![https://ithelp.ithome.com.tw/upload/images/20260831/20182796xfdFXlU8Ov.png](https://ithelp.ithome.com.tw/upload/images/20260831/20182796xfdFXlU8Ov.png)

> 圖說:第三版格式下的真實回報。注意「lint／型別檢查:未執行」那行——  
> 它沒有假裝跑過

---

## 6\. 技術難題三:Claude 會把回報範本裡的 ✅ 當成預設值

這個我 debug 了一陣子才發現。

v1 的回報範本我為了「讓它知道長什麼樣」，直接寫了填好的範例:

```markdown
### 驗證
- 測試：pytest tests/ → ✅ 18 passed
- lint：✅ 通過
```

然後我就開始收到「✅ 18 passed」的回報—— **在一個根本沒有 18 個測試的專案裡** 。它不是在說謊，它是在 **照抄範本** 。我給它看的是一個已填好的表格，它理解成這是輸出的樣子。

同樣的事發生在 PRD 跟調研報告上:範本裡的假數字、假結論，會原封不動出現在產出裡。

### 解法:範本一律留空，加上一條明規則

```markdown
**不偽造結果。** 測試要真的跑、數字要真的量。沒跑就寫「未執行」，
失敗就貼失敗輸出。回報範本裡的 ✅ 是要你填的，不是預設值。
```

文件類產出的完成定義也對應加上一條:**沒有預填的假數字、假結論、佔位符** 。

順帶配上阻塞協定，讓它失敗時有地方去，不用硬掰:

> 同一個問題最多嘗試 **3 次** 不同做法;仍失敗就停，交出目前產出＋失敗紀錄。  
> 阻塞不代表交白卷:完成能完成的部分，把阻塞點、已試過的方法、你建議的解法寫進回報。

---

## 7\. 幾個刻意的設計選擇

**`model: inherit`** — 角色檔不寫死模型。派工時由主 agent 決定用哪個(規劃、審查類的吃重推理，機械性的任務用便宜的)，同一個角色可以逐次覆寫。寫死在角色檔裡會讓你每次改主意都要編輯檔案。

**文件角色給 tools 白名單。** product-manager、research-analyst、risk-manager 的 frontmatter 明確限制成 `Read, Glob, Grep, Write, WebSearch, WebFetch` —— **沒有 Bash、沒有 Edit** 。理由很現實:PM agent 一旦能動程式碼，它就會忍不住去動。權限限制比在文件裡叮嚀「你不要寫程式」有效得多。

**產出路徑總表。** 協定裡有一張表寫死每種產出去哪(PRD → `docs/prd/` 、審查報告 → `docs/reviews/<YYYY-MM-DD>-<feature>.md` ……)，並且明寫「工作項目、bug、風險一律用 **GitHub Issues** ，不要用 markdown 自建追蹤表」。不寫這條的話，每個 agent 都會熱情地幫你生一個 `TODO.md` ，然後你有五份互相矛盾的待辦清單。

**qa-code-reviewer 只審不修。** 審查跟修改分開，是為了讓「修」這件事一定會回到工程師角色、走一樣的測試與 commit 規範。審查者順手修掉的東西不會有測試。

---

## 8\. 現況、限制、還有我不知道的事

先講限制，這些是真的:

- **這是文件工程，不是程式。** 沒有測試套件能驗證一份 prompt 是否「正確」。我唯一的驗證方式是實際派工、看產出、發現問題就改協定。這代表本文所有結論都是 **單一使用者的經驗** ，不是實驗結果。
- **v2 才跑一個多月，樣本不夠。** 12,069 → 1,239 行之後我覺得明顯變好，但我沒辦法排除「我同時也變會派工了」這個混淆因素。
- **效果會隨模型版本變動。** 有些規則(例如那條 ✅ 預設值)是為了繞過特定模型的行為傾向寫的，模型換代後可能就不需要了。
- **它預設你有一套流程。** PRD → 技術設計 → issue → 分支 → 實作 → 審查 → 部署。如果你只是想快速改個 bug，這整套是過重的。

現況:8 個角色檔在我自己的專案上實際使用中，協定 v1(2026-08-19)。  
下一步大概是把「主 agent 側」的東西也整理出來——目前專案 `CLAUDE.md` 跟 `docs/PROJECT_STATE.md` 的寫法還在各專案裡各寫各的，應該要有一份範本。

---

## 9\. 如果你要拿去用

```bash
# 安裝
git clone https://github.com/timwei0801/AI_Agent_For_Full_Stack_Engineer.git ~/.claude/agents

# 之後更新
cd ~/.claude/agents && git pull
```

裝完之後，直接在 Claude Code 裡這樣派工就會生效:

```bash
請用 backend-engineer 實作 issue #12。
目標：新增 FinMind 台股價量 provider
範圍外：不要動既有的資料清洗流程
必讀：docs/architecture/data-pipeline.md
分支：feature/12-finmind-provider
```

**但我的建議是不要照抄。** 這套協定裡有很多是我自己的偏好(繁中 commit、GitHub Issues、產出路徑)。真正值得帶走的是三個結構性決定:

1. **共同規則抽一份，角色檔只寫差異** — 規則不能有兩份。
2. **明寫「你是一次性的」** — 不要讓 subagent 寫出等不到回覆的話。
3. **範本留空，明禁預填** — 你給它看的範例，它會當成答案。

Fork 一份，把裡面的專案規範換成你自己的，比整套照用有用。

![https://ithelp.ithome.com.tw/upload/images/20260831/201827964E0lZVdhbD.png](https://ithelp.ithome.com.tw/upload/images/20260831/201827964E0lZVdhbD.png)

> 圖說:一張任務書長這樣。欄位定義在 `_PROTOCOL.md` 第 2 節

---

如果你是剛開始用 Claude Code、還沒到需要多 agent 協作的階段，我另外有一個從零開始的系列「 **Claude Code 實戰手冊** 」，從安裝、用量計費、 `CLAUDE.md` 一路寫下來，可以先從那邊開始。

有踩到不一樣的坑，歡迎在留言區告訴我——這套協定的每一條都是踩出來的，我很想知道還有哪些我還沒踩到。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

\*提醒邦友，使用第三方服務/API 時，請務必評估資安風險與隱私保護

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/27/58ea9056-7dfe-425b-8d5e-3ed1988ce904_small.jpg)

[TS-1655 採用 Intel Atom C5125 八核心 2.8GHz 處理器，最高支援 256GB 記憶體，大容量混合式儲存架構適合中小企業備份及監控應用，支援 QTS / QuTS hero](https://itplus.ithome.com.tw/vod-page/101?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

QNAP Systems, Inc. |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/368ce414-1126-4859-a8ce-ab679373a83b_small.png)

[容器化實現證券核心轉型](https://itplus.ithome.com.tw/vod-page/1027?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

51 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/f9c76a53-37f0-460b-b00c-8adb84080344_small.png)

[DISCOVER/ ANALYSE and SECURE](https://itplus.ithome.com.tw/vod-page/1152?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

55 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/d73040e3-2358-4699-998b-c1cbd6f59468_small.png)

[從寫到管：打造 GCP DevOps Agent 的實戰紀錄](https://itplus.ithome.com.tw/vod-page/1472?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

30 分