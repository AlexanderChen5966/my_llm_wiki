---
title: "那個技能，真的有用嗎？比較 3 種評估 Agent Skills 的工具"
source: "https://codelove.tw/@tony/post/3jAvJx"
author:
published:
created: 2026-07-13
description: "[](#%E5%89%8D%E8%A8%80)前言-----------------------------------自 2025 年 10 月 Anthropic 發表 Agent Skills（以下稱為技能）以來，這已經變成相當熟悉的存在。只要把作業流程明文化到 `SKILL.md`..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月10日](https://codelove.tw/@tony/post/3jAvJx "2026-07-10 17:30:12")

## 那個技能，真的有用嗎？比較 3 種評估 Agent Skills 的工具

## 前言

自 2025 年 10 月 Anthropic 發表 Agent Skills（以下稱為技能）以來，這已經變成相當熟悉的存在。

只要把作業流程明文化到 `SKILL.md` ，再交給 agent，AI 就會照著你寫的內容執行。  
乍看之下非常方便，但實際深入使用後，難免會突然感到不安。

**「這個技能真的有正常運作嗎？」** [![:sweat:](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)

**「到底還有沒有改進空間，或者說該怎麼評估才對？」** [![:unamused:](https://cdn.qiita.com/emoji/twemoji/unicode/1f612.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f612.png)

最近出現了一些用來「評估／改善技能」的工具，  
這次我將實際接觸 **skill-creator（Anthropic 官方）** 、 **darwin-skill** 、 **SkillOpt（Microsoft）** 這 3 個工具，看看它們各自是怎麼評估、評估了什麼 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

## 技能的構成

在進入評估話題之前，先回顧一下技能的構成。  
如果你想先了解技能本身是什麼，可以參考這份投影片。

技能的實體，  
簡單來說就是 **一個 `SKILL.md` Markdown 檔案 + 附屬檔案群** 。

[SKILL.md](http://skill.md/)

```
---
name: pdf-filler
description: Use when the user wants to fill in a PDF form from structured data...
---

# PDF Filler

## Workflow
1. 讀取輸入資料
2. 用 scripts/fill.py 將資料填入表單
3. 驗證輸出
...
```

`SKILL.md` 本身是 **必須** 的，外圍可以掛上各種選配的附屬檔案，  
因此整體會是這樣的目錄結構：

```
skill-name/
├── SKILL.md        … 必要（YAML frontmatter + 內文）
└── （以下皆為選配）
    ├── scripts/    … 執行固定處理的程式碼（可重複使用，不必每次重寫）
    ├── references/ … 只有在需要時才讀取的補充文件
    └── assets/     … 用於輸出的素材（範本、圖示、字型等）
```

`SKILL.md` 的內容大致可分成 2 個部分。

| 組成要素 | 角色 |
| --- | --- |
| YAML frontmatter（ `name` / `description` ） | 讓技能 **被觸發的最重要部分** 。尤其 `description` 不只要寫「做什麼」，還要把「什麼時候用」全部寫清楚。這裡寫得好不好，會直接決定觸發精準度。 |
| 內文（步驟／工作流程） | 技能被觸發後，agent 實際遵循的作業手冊。失敗時的分支、檢查點也寫在這裡。 |

### 逐層讀取的機制

技能之所以設計得好，是因為它採用 **只在需要時，才讀取需要的內容** 的設計。

讀取分成 3 個階段 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

| 階段 | 讀取的內容 | 時機 |
| --- | --- | --- |
| ① 中繼資料 | `name` + `description` | **一直** 都在 context 裡 |
| ② 內文 | `SKILL.md` 的步驟 | 只有技能 **被觸發時** |
| ③ 附屬檔案 | `scripts/` / `references/` / `assets/` | 只有 **需要時** 才讀。 `scripts/` 也可以不讀取而直接執行 |

多虧這個機制，平常只要把很輕量的 metadata 留在 context window 裡，真正要用時才去讀內文和附屬檔案，就能在節省 context 的同時處理大型技能 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

了解構成之後，接著就進入本文重點： **評估** 。

## skill-creator

首先是 Anthropic 官方的 **skill-creator** 。  
它會作為外掛內建在 Claude Code 裡，透過 `/skill-creator` 啟動。

如果找不到指令，或你使用的是其他 coding agent，  
也可以用下列指令安裝：

```
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

顧名思義，它是用來 **從零建立技能** 的工具，  
但更值得一提的是，它還會一路幫你做到 **實際執行技能並進行評估** 。

### 將實際輸出與「baseline」比較評價

skill-creator 的評估會看 **實際執行技能時的輸出** 。

1. 在 `evals.json` 中定義 `prompt` 、 `expected_output` 、 `assertions` （可機械式判定成敗的標準）
2. 用 **有技能** 與 **無技能** 兩種情況，對同一個任務各自執行多次
3. 評估用的 sub-agent 會 **從 transcript 和輸出中引用依據** ，判定 pass / fail
4. 將各設定的結果以 `mean ± stddev` （平均值 ± 變異程度）彙整，並並排比較 **有技能 vs 無技能**

這裡要特別看 `stddev` [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png) `stddev` 很大（也就是結果波動大）的測試會被視為 flaky，代表「這個技能的效果不夠明確／可能很依賴模型」。

如果加入技能之後 pass 率沒有提升，那這個技能就沒有意義，而這件事會直接用數字看出來。結果會在瀏覽器的 Eval Viewer 中，把輸出和數值並列顯示。

實際數值會在後面的「實際嘗試」中，以 `commit-message-writer` 的結果展示。

### 「有沒有確實被調用」也納入評價來修正 description

輸出好不代表沒問題，因為如果在該用的情境下 **技能根本沒被呼叫** ，一樣沒有意義。  
agent 平常只看得到 `name` 和 `description` ，而且只靠這個 `description` 判斷要不要使用。

skill-creator 也會測量這個 **觸發率** ，並進一步改善它。

- 準備約 20 個真實感較高的使用者 prompt，混合 should-trigger（應該觸發）與不該觸發的案例
- 切分成 train / test，並對每個 query 執行多次以量測觸發率
- 把失敗案例交給 Claude 重寫 `description` ，然後反覆再評估
- 最後以 **test 端的分數** 選出最佳版本

可以用數字把「明明希望它觸發卻沒觸發」和「不相關情境卻自己跳出來」這兩種問題壓下去。

### 不是評價完就結束，而是直接循環改善

skill-creator 的核心是，這個評價 **不會只做一次就結束** 。  
每次評估都會產生回饋，接著依據回饋修改 `SKILL.md` ，然後 **用下一輪的同一組測試再跑一次，和前一次比較** ，形成持續改善的迴圈。

例如，如果發現「3 個測試裡每次都在寫差不多的輔助腳本」，那就自然會想到把那段腳本打包進 `scripts/` ，再從本文去呼叫它。

**建立 → 量測 → 修正 → 再量測** ，這就是 `skill-creator` 的工作流程 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

### 實際試看：刻意抄寫得很隨便的技能來改善

前面講了很多抽象說明，現在實際跑一次 `skill-creator` 。  
題材是 **刻意寫得很隨便** 的 commit message 建立技能（commit-message-writer）。

[SKILL.md](http://skill.md/) （改善前・全文）

```
---
name: commit-message-writer
description: 幫忙寫 commit message。請依照情境彈性處理。
---

# Commit Message 作成

針對程式碼變更產生好的 commit message。

## 作法

先確認變更內容，然後把它摘要出來。請盡量寫得容易理解。必要時也可以加上種類（例如 feat 或 fix）。不要太長，盡量簡潔整理會比較好。

如果有多個變更，可以視情況合併著寫。

請根據狀況彈性判斷。
```

如你所見，裡面充滿了「依照情境彈性處理」「視情況」「盡量」這類 **模糊的表達** 。人類讀起來大概知道意思，但對 agent 來說，根本沒有可以遵循的標準 [![:frowning2:](https://cdn.qiita.com/emoji/twemoji/unicode/2639.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2639.png)

把這個技能交給 skill-creator 評估後，會得到像下面這樣的 `evals.json` 。

evals/evals.json（節錄）

```
{
  "skill_name": "commit-message-writer",
  "evals": [
    {
      "id": 1,
      "name": "happy-path-single-feature",
      "prompt": "請針對以下變更撰寫 commit message。已在 src/auth.ts 新增基於 JWT 的登入驗證。access token 的有效期限設定為 1 小時。",
      "expected_output": "Conventional Commits 形式的單行 subject（例如: feat(auth): add JWT-based login）。72 字以內、命令式、結尾沒有句點。",
      "assertions": [
        "subject 行符合 \`type(scope): summary\` 格式（type 為 feat/fix/docs/style/refactor/test/chore 之一）",
        "type 為 feat（因為是新增功能）",
        "subject 行在 72 字以內",
        "subject 行結尾沒有句點"
      ]
    }
  ]
}
```

測試準備了以下 3 個案例：

- **① 單純的功能新增** ：是否能產生 `feat(auth):` 形式的 1 行 subject（72 字以內、命令式、末尾沒有句點）
- **② 錯誤修正** ：是否能產生 `fix(api):` 的 subject ＋ 空行 ＋ 說明原因／處理方式的內文
- **③ 無關的多個變更** ：能否避免硬塞成一個，改為明確建議拆分或用主目的統整為單一項目

#### 首先用較弱的初版與 baseline 比較

「有技能（較弱的初版）」與「無技能（baseline）」的執行結果如下。

| 指標 | 有技能（較弱的初版） | 無技能 | 差分 |
| --- | --- | --- | --- |
| pass 率 | 92% ± 14% | 100% ± 0% | **\-0.08** |
| 執行時間 | 18.4s ± 8.2s | 20.4s ± 10.5s | \-2.0s |
| token | 24,695 | 24,453 | +241 |

[![iter1-benchmark.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F2dc7bc10-aa9b-4906-80d1-f87d063a5256.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7e35efee403326f373bff9994573a307)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F2dc7bc10-aa9b-4906-80d1-f87d063a5256.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7e35efee403326f373bff9994573a307)

[![iter1-outputs.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fa93e2b74-6f34-4b30-a9bf-62b49b5504c4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=446ffcdf5f881e1610a6be5bda31d04f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fa93e2b74-6f34-4b30-a9bf-62b49b5504c4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=446ffcdf5f881e1610a6be5bda31d04f)

結果竟然是， **加入較弱的技能反而比 baseline 更差** [![:sweat:](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)

這是很具體的例子： **模糊的技能，可能不只沒有幫助，甚至會拖累表現** 。也清楚證明了「只要先把流程寫上去就會變好」這件事並不成立 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

#### 打散模糊性後重新打造

根據 Grader 的指摘，將 `SKILL.md` 具體化。主要修改如下：

- **明確寫出 Conventional Commits** （輸出格式圖 + `feat` / `fix` /… 的 type 清單 + scope 原則上必須）
- **規則化 subject / body** （命令式、72 字以內、末尾無句點／本文需空一行，且 bug fix 要明寫原因）
- **加入編號步驟** （掌握 → 判斷多重變更 → 決定 type/scope → 本文）
- **處理多個變更** （明確建議拆分，或以主目的單一化）
- **反例** （禁止像 `chore: update stuff` 這種粗略包成一坨）

`description` 也從觸發角度變得更銳利。

**Before**

```
幫忙寫 commit message。請依照情境彈性處理。
```

**After**

```
從 git 的變更內容（diff・變更說明・已 stage 的檔案）產生 Conventional Commits 形式的 commit message。當使用者說「幫我寫/做 commit message」「幫我 commit 這些」「commit message」「把這些變更提交」等，即使沒有明講格式，也一定要使用這個技能。包含 type(scope) 的判定、subject/內文的區分，以及對無關多重變更的拆分建議。
```

刪除模糊詞（像是「依照情境彈性處理」）後，再加上 **要做什麼 + 具體觸發詞** 。

改善後的完整版本如下。

[SKILL.md](http://skill.md/) （改善後・全文）

```
---
name: commit-message-writer
description: 從 git 的變更內容（diff・變更說明・已 stage 的檔案）產生 Conventional Commits 形式的 commit message。當使用者說「幫我寫/做 commit message」「幫我 commit 這些」「commit message」「把這些變更提交」等，即使沒有明講格式，也一定要使用這個技能。包含 type(scope) 的判定、subject/內文的區分，以及對無關多重變更的拆分建議。
---

# Commit Message 作成

從 git 的變更產生 Conventional Commits 形式的 commit message。
透過機械化統一格式，可以提升歷史紀錄可搜尋性、自動產生 changelog 的便利性，以及 code review 的效率。

## 輸出格式

必須以下列結構輸出:
```

\<type>(\<scope>): \<subject>  
← 如果需要本文，這裡要空一行  
<body: 寫出變更了什麼、為什麼變更。需包含原因與背景>

```
- **type**（必須）: 以下任一種
  - \`feat\`（功能新增） / \`fix\`（錯誤修正） / \`docs\`（文件） / \`style\`（僅格式調整） / \`refactor\`（不改變行為的重構） / \`test\`（測試） / \`chore\`（雜務、依賴更新等）
- **scope**（原則上必須）: 變更的主要對象。從檔案／模組名稱決定（例如: \`auth\`, \`api\`, \`deps\`）。只有在影響整體且無法特定時才可省略。
- **subject**（必須）:
  - 用命令式、現在式書寫（比起「已新增」更應寫成「新增」）
  - **72 字以內**
  - **結尾不要加句點**
- **body**（選用・建議）: 錯誤修正或不直觀的變更，務必寫出**原因與處理方式**。subject 之間必須空一行。

## 步驟

1. **掌握變更內容**：閱讀傳入的 diff、變更說明、檔案清單，列出到底改了什麼。
2. **判斷是否為無關的多重變更**（重要）：如果變更包含彼此在邏輯上獨立的多個目的（例如：文件新增 + 依賴更新 + 檔案刪除），就進入步驟 5。若是單一目的，進入步驟 3。
3. **決定 type 與 scope**：根據主要目的決定 type，根據主要對象決定 scope。
4. **撰寫 subject 與本文**：依照上述格式輸出。若是錯誤修正，本文一定要寫原因。→ 完成。
5. **處理多重變更**：不要硬塞成一個 commit。請明確提出以下其中一種方案：
   - **建議方案：拆分 commit** — 依照目的分成多個 commit message（例如：\`docs: ...\` / \`chore(deps): ...\` / \`refactor: ...\`）
   - **如果一定要合併** — 選主目的的 type，subject 寫主變更，本文用條列列出內部項目

## 範例

**範例 1：單一功能**
輸入：在 src/auth.ts 新增 JWT 登入驗證。token 有效期限 1 小時。
輸出:
```

feat(auth): add JWT-based login authentication

在 src/auth.ts 實作使用 JWT 的驗證。access token 的有效期限為 1 小時。

```
**範例 2：錯誤修正（本文需寫原因）**
輸入：重試進入無限迴圈。原因是沒有設定指數退避上限，修正為最多 3 次後逾時。
輸出:
```

fix(api): cap retry backoff to stop infinite loop

指數退避沒有上限，導致重試無限持續。  
已設定上限，改為最多 3 次後逾時。

```
## 不可做的事（反例）

- ❌ \`chore: update stuff\` / \`fix: bug\` 這種**資訊量為零的摘要** → 必須明確寫出改了什麼
- ❌ 把無關變更硬湊成 \`chore: 什麼都改一點\` → 應該提出拆分建議
- ❌ subject 用過去式長句／尾端加句點 → 必須用命令式、72 字以內、不要句點
- ❌ 沒有依據就省略 scope → 應從檔案／模組推導出來
```

#### 再次回對改善版

改善版（New Skill）與較弱的初版（Old Skill）相比，結果如下。

| 指標 | 改善版 | 較弱的初版 | 差分 |
| --- | --- | --- | --- |
| pass 率 | 100% ± 0% | 92% ± 14% | **+0.08** |
| 執行時間 | 19.3s ± 5.4s | 18.9s ± 7.7s | +0.4s |
| token | 25,986 | 24,723 | +1,263 |

[![iter2-benchmark.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F7d6bdf65-5477-441e-a1fc-814a400cda75.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=79ebb6c2947511fd9faa086c1b8caa4a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F7d6bdf65-5477-441e-a1fc-814a400cda75.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=79ebb6c2947511fd9faa086c1b8caa4a)

[![iter2-outputs.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fa5af36f5-5d22-4211-ab92-38c4f65e46ef.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1af54a280694fca468f65fbb2b613313)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fa5af36f5-5d22-4211-ab92-38c4f65e46ef.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1af54a280694fca468f65fbb2b613313)

老實說，pass 率差距只有 ±8pt，幅度不算大。  
原因是底層模型（Opus）本來就很強，即使什麼都不特別加，也大多能正確完成。這類技能真正的價值，反而在於 **強制一致性** 。  
像是 scope 的補上、命令式、在本文寫出原因，這些都不是靠運氣，而是要 **每次都穩定一致地做出來** ——這次的 scope 問題正好就是代表例子。

## darwin-skill

接著是 **darwin-skill** 。  
它是一種會 **自動評估與改善其他技能的「meta-skill」** 。

安裝也很簡單，只要一個指令。

```
npx skills add alchaincyf/darwin-skill
```

### 9 維度規則評分「寫法的品質」

darwin-skill 的特色在於，它會用 9 個維度、滿分 100 的規則來評分 `SKILL.md` 的 **文字本身品質** （評分基準會明寫在 [SKILL.md](http://skill.md/) 裡）。  
從 9 個維度中挑幾個代表項目，大概像這樣：

| 維度 | 權重 | 觀察重點 |
| --- | --- | --- |
| 可執行的具體性 | 17 | 是否排除了像「建議」「依情況」這種模糊表達 |
| 實測效能 | 23 | 以 test prompt 在有／無技能下執行並比較輸出（和 skill-creator 類似的方法） |
| 失敗模式明示 | 12 | 是否清楚寫出「如果 X 失敗就改成 Y」這類分支 |
| 檢查點設計 | 6 | 是否有像 🔴 這類視覺化、容易辨識的停點 |
| 高風險行為反例清單 | 6 | 是否明確列出像 `git reset --hard` 、force push 這類「不能做」的內容 |

其中「實測效能（dim8）」和 skill-creator 一樣，會實際跑起來比較；而其餘多數維度，則是在檢視 **作為文件時的寫法好不好** ，整體上是個滿平衡的設計。

### 山勢挑高 + git 採納配合的改善循環

它不是只評分就結束，而是內建了 **一次只修一個低分維度，再逐步往上拉高** 的循環。

```
1. 先選出分數最低的維度 1 個
2. 套用改善方案並編輯 SKILL.md → git commit
3. 交由獨立的子 agent 重新評分
4. 分數上升就保留 / 下降就用 git revert 還原
5. 當改善幅度變小時自動停止
```

我覺得這裡做得很漂亮的地方有 2 點 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

- **把評分者和編輯者分開** ：由於自己改完自己評分很容易變得寬鬆，所以重新評分交給獨立的子 agent
- **用 git 保留歷史** ：只有真正變好的修改才會以 commit 累積；如果惡化，就用 `revert` 還原（而不是用 `reset --hard` 把歷史直接清掉）

它是用機制把「自我評分會偏寬鬆」這件事直接壓掉，雖然低調但很有效。

整體來說，它不是完全不需要人介入的全自動流程，而是在關鍵節點保留人類確認。  
文件裡也明寫了：自動評估有其極限，重要判斷仍應由人類確認。這不是把人排除在外，而是設計成不會把所有責任丟給工具。

另外，darwin-skill 的文件中也提到，它是以 Microsoft Research 的 SkillLens / SkillOpt，以及 Karpathy 的 autoresearch 為基礎。  
根據 repository 的 README，最佳化後平均提升約 +13.5 分之類的實績也有寫，但這部分畢竟是 **工具自己宣稱的數字** ，建議還是當作參考即可。

### 實際試看：用 darwin-skill 評分並改善同樣雜的技能

題材和 skill-creator 時 **完全相同** ，都是改善前的 `commit-message-writer` 。

測試 prompt 也維持同樣 3 個案例（①功能新增／②錯誤修正／③無關的多個變更），先放進 `test-prompts.json` ，再切 git branch 跑起來。

#### 基線評價：19.2 分的現實

先看目前狀態的評分。  
這次不是由改善者自己評，而是由 **獨立裁判的子 agent 逐項引用根據** ，對 9 個維度打分。

| dim | 維度 | 分數（滿分 10） | 裁判依據（節錄） |
| --- | --- | --- | --- |
| 1 | Frontmatter 品質 | 2 | 「依照情境彈性處理」是被禁止的空泛結尾 |
| 2 | 工作流程清楚度 | 2 | 沒有編號步驟，也沒有明確的輸入／輸出定義 |
| 3 | 失敗模式明示 | 1 | 完全沒有「如果 X 失敗就改成 Y」這種分支 |
| 4 | 檢查點設計 | 1 | 沒有使用者確認，也沒有 🔴 / STOP 等視覺標記 |
| 5 | 可執行的具體性 | 1 | 「～比較好」「視情況」這類軟化詞超過 5 處，而且沒有格式範例 |
| 6 | 資源一致性 | 5 | 因為沒有附屬檔案、全文都寫在內文中，所以屬中立 |
| 7 | 整體架構 | 2 | 只有一個「作法」見出；「彈性判斷」這段沒有實質內容 |
| 8 | 實測表現 | 3 | **3 題中有 2 題輸給 baseline** （後述） |
| 9 | 反例與黑名單 | 1 | 完全沒有「不可做的事」 |

加權總分是 **19.2 / 100** 。幾乎所有維度都在低標附近，真是非常乾脆的評分結果 [![:sweat:](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f613.png)

有趣的是，dim8（實測表現）的結論，和 skill-creator 那次 **幾乎完全一致** 。  
裁判指出的問題是：「① 在 subject 裡混入檔案路徑」、「③ 將 1 個 commit 塞滿的建議放成主軸，而輸給主打拆分提案的 baseline」——即使是不同工具、不同評分軸，依然得到同樣結論： **模糊的技能會輸給 baseline** [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

#### 3 回合的山勢探升

接下來就是 darwin-skill 的主體： **每次修正最低維度，並交給獨立裁判重新評分** 的迴圈。  
這次跑了 3 回合就停止了（預設 MAX\_ROUNDS）。

| Round | 修正的維度 | 變更內容 | 分數判定 |
| --- | --- | --- | --- |
| 1 | dim5 具體性 | 排除模糊措辭，補上輸出格式、type 清單、subject 規則、編號步驟 | 19.2 → 55.4 keep |
| 2 | dim1 frontmatter | 在 description 中明寫「做什麼 + 什麼時候用 + 觸發詞」 | 55.4 → 63.2 keep |
| 3 | dim3＋dim4 | 加入 if-then 失敗分支表與 🔴 CHECKPOINT | 63.2 → 77.5 keep |

各維度的推移如下。

| dim | 基線 | R1 | R2 | R3 |
| --- | --- | --- | --- | --- |
| 1 Frontmatter | 2 | **9** | 9 | 9 |
| 2 工作流程 | 2 | **7** | 7 | 8 |
| 3 失敗模式 | 1 | **3** | **8** | 8 |
| 4 檢查點 | 1 | **2** | **9** | 5 |
| 5 具體性 | 1 | **7** | 8 | 8 |
| 7 架構 | 2 | **7** | 8 | 8 |
| 8 實測 | 3 | **7** | 7 | 8 |
| 9 反例 | 1 | **4** | 4 | 5 |

`results.tsv` 的記錄如下，三輪都只有 keep，沒有 revert。

results.tsv

```
timestamp   commit  skill   old_score   new_score   status  dimension   note    eval_mode
2026-07-09T10:12    baseline    commit-message-writer   -   19.2    baseline    -   初始評估（dim5/dim3/dim9 最弱） full_test
2026-07-09T10:31    fd706d8 commit-message-writer   19.2    55.4    keep    dim5 具體性    排除模糊措辭 + 明文化格式。dim2/dim7 也連帶上升  full_test
2026-07-09T10:42    183041a commit-message-writer   55.4    63.2    keep    dim1 frontmatter    description 明寫觸發詞。行為不變，因此 dim8 維持前次值    dry_run
2026-07-09T10:55    528c2a6 commit-message-writer   63.2    77.5    keep    dim3+dim4 相關簇   if-then 分支表 + 🔴CHECKPOINT 加入。P3 已確認會等待使用者確認 full_test
```

3 回合後的完整內容如下。

[SKILL.md](http://skill.md/) （darwin 3 回合後・全文）

```
---
name: commit-message-writer
description: 從 git 的變更內容（diff・變更說明・已 stage 的檔案）產生 Conventional Commits 形式的 commit message。當使用者說「幫我寫/做 commit message」「幫我 commit 這些」「commit message」「把這些變更提交」等，即使沒有明講格式，也一定要使用這個技能。包含 type(scope) 的判定、subject/內文的區分，以及對無關多重變更的拆分建議。
---

# Commit Message 作成

從 git 的變更內容產生 Conventional Commits 形式的 commit message。

## 輸出格式
```

\<type>(\<scope>): \<subject>  
← 如果需要本文，這裡要空一行  
\<body>

```
- type: feat / fix / docs / style / refactor / test / chore 的其中一種
- scope: 變更的功能領域（auth, api, web 等）。原則上要加
- subject: 命令式、72 字以內、末尾不要句點、不要放檔案路徑
- body: 錯誤修正時必須寫出原因與處理方式

## 步驟

1. 閱讀變更內容（diff・變更說明・已 stage 的檔案），判斷變更種類
2. 決定 type 與 scope
3. 寫出 subject（例如: \`feat(auth): add JWT-based login\`）
4. 如果需要本文，就寫出變更了什麼、為什麼變更
5. 如果混有彼此無關的多個變更，不要塞成一個 commit，請優先提出 commit 拆分建議
   🔴 CHECKPOINT: 如果已提出拆分方案，在實際執行 commit 前，必須先等待使用者確認

## 失敗時的分支

| 狀況 | 對應方式 |
| --- | --- |
| diff 和變更說明都沒有 | 不要猜測，請求提供 \`git diff --staged\` 的結果或變更內容說明 |
| type 無法唯一決定 | 最多提出 2 個候選，並請使用者確認主目的 |
| 變更太大，無法好好摘要 | 只挑 3 個主要變更組成 subject 和本文，其餘內容改放到本文條列 |
```

另外，正直地補充兩點注意事項。

第一點是 **與大小限制的衝突** 。darwin-skill 有「改善後不得超過原本 150%」的規則，但像這次這種極小的雜技能（737 bytes）一旦套用，第一回合就會超標（最終 2,096 bytes，約 284%）。這裡是靠檢查點，由人類判斷「原文太小，因此不適用」。在關鍵處插入人審，正好幫上忙。

第二點是 **不要太相信分數的絕對值** 。評分本質上是 LLM judge，所以 19.2 或 77.5 這些數字每次跑多少都會有些浮動。即使參考 SkillLens 的實證（LLM 自評準確率 46.4%），使用方式也應該侷限在「抓出哪些維度較弱」以及「只有分數真的上升時才 keep 的 ratchet」這種層次會比較合理。

## Microsoft SkillOpt

最後是 **Microsoft 的 SkillOpt** 。這個最硬派，也最偏研究取向。

### 概念：將技能「近似神經網路的方式學習」

SkillOpt 的概念非常獨特： **把技能文件，當成神經網路那樣來做最佳化** 。  
`epoch` 、 `batch size` 、 `learning rate` 、 `validation gate` 這些深度學習常見名詞，在它的流程裡都會出現。

它把技能文件視為「可學習的狀態（權重）」，而模型本身固定不動，只對 **Markdown 內容** 做最佳化。  
最後產出的 `best_skill.md` 只是一般 Markdown，因此 **部署時不會額外再呼叫模型** 。

### 6 階段的最佳化迴圈

根據官方文件（ `docs/guide/training-loop.md` ），核心是以下 6 個步驟。括號中是對應到神經網路訓練時的類比。

| 步驟 | 做什麼 |
| --- | --- |
| Rollout | 用目前的技能實際執行任務並取得分數（＝前向傳播） |
| Reflect | 分析成功／失敗軌跡，產出編輯建議（patch） |
| Aggregate | 階層式整合多個編輯建議 |
| Select | 依重要度排序編輯建議，並在預算（＝ learning rate）內篩選 |
| Update | 將選出的編輯套用到技能文件 |
| Gate | 用分出的驗證資料評估，只有真的變好才採納 |

特別有效的是最後的 **Gate** ，也就是只有當更新後的技能在驗證資料上確實變好時，才會採納。  
甚至連 `learning rate` 排程（ `constant` / `linear` / `cosine` / `autonomous` ）都實作了，真的很像在做模型訓練。

此外，為了避免遺忘過去的改進，還加入了 Slow Update，以及跨 epoch 累積學習筆記的 Meta Skill，這些都對應到 ML 中對抗「災難性遺忘」的做法。

### 使用時需要資料集與 API 金鑰

相對地，便利性就犧牲不少。它預設需要 Python 環境、用來跑評估的 **資料集** ，以及 LLM 的 **API 金鑰** 。

```
git clone https://github.com/microsoft/SkillOpt.git
cd SkillOpt
pip install -e .
```

.env

```
# 需要 Azure OpenAI / OpenAI / Anthropic / Qwen 其中一種 API 金鑰
ANTHROPIC_API_KEY="sk-ant-..."
```
```
# 指定設定檔來執行最佳化
python scripts/train.py --config configs/searchqa/default.yaml
```

它也附有使用 Gradio 製作的 WebUI 儀表板，可以視覺化觀察訓練過程。

```
python -m skillopt_webui.app
```

SkillOpt 是一套給「準備好基準資料集、並要正式跑最佳化」的研究級工具。  
它沒有那種 `pip install` 後立刻就能在 Claude Code 裡直接用的輕量感，所以我會建議先把目標講清楚，再決定要不要導入。

## 比較與使用順序

把 3 個工具並排來看，關注點和便利性差異其實滿大。

| 工具 | 提供者 | 主要目的 | 評估什麼 | 執行位置 | 便利性 | 觸發（description）最佳化 |
| --- | --- | --- | --- | --- | --- | --- |
| skill-creator | Anthropic 官方 | 建立 + 用實際輸出驗證 | 實際輸出（行為） | Claude Code 內 | ◎ 內建、即開即用 | 有 |
| darwin-skill | 社群 | 磨練既有技能的寫法 | 文件品質（9 維度）＋實測任務分數 | Claude Code 內 | ◯ `npx` 一鍵 | — |
| SkillOpt | Microsoft | 大規模最佳化 | （以學習方式最佳化） | Python + 資料集 | △ 需環境建置與 API 金鑰 | — |

因為評估軸不同，所以不是在比「誰最強」，而是更適合 **分階段使用** 。

1. **先用 skill-creator** 建立技能，透過有／無技能的輸出比較，確認「這東西到底有沒有用」
2. **再用 darwin-skill** ，把模糊表達、失敗時分支、檢查點等「寫法品質」用分數一項一項補強
3. **若要正式大規模最佳化，就用 SkillOpt** 。當你已經有評估用資料集，而且想以 ML 的方式強力調參時，再導入它

如果只是個人想把一到數個技能弄好，多半 `skill-creator` + `darwin-skill` 的組合就很夠用了。SkillOpt 比較像是「已經有資料集，且想反覆跑最佳化」時的選項。

## 總結

技能常常會變成「寫完就算了」的東西，但這次接觸的 3 個工具有一個共同點： **不是看你自己覺得好不好，而是看實際輸出來衡量** 。身為寫技能的人，雖然這有點扎心，但能把成效數字化之後，改善循環確實變得容易許多 [![:relaxed:](https://cdn.qiita.com/emoji/twemoji/unicode/263a-fe0f.png)](https://cdn.qiita.com/emoji/twemoji/unicode/263a-fe0f.png)

那麼，應該從哪裡下手呢？我的建議是循序漸進：

1. 先拿手邊的一個技能，用 `/skill-creator` 做 **有／無技能比較** ，確認它到底有沒有發揮作用
2. 如果確定有效，再用 **darwin-skill** 把模糊表達、失敗分支等「寫法品質」逐項補齊
3. 如果已經有評估用資料集，而且想更正式地做最佳化，就考慮 **SkillOpt**

「我以為它有用」結果被數字直接推翻的瞬間，其實還滿有趣的 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

---

原文出處： [https://qiita.com/Syoitu/items/78d45bee1160d059c972](https://qiita.com/Syoitu/items/78d45bee1160d059c972)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3jAvJx) [19瀏覽](https://codelove.tw/@tony/post/3jAvJx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3jAvJx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️5

735

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

165

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783904881&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3jAvJx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783904881553&amp;bpp=1&amp;bdt=321&amp;idt=297&amp;shv=r20260709&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783904813%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783904813%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783904813%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4193054092318&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=42532523%2C42533293%2C95394427%2C95395869&amp;oid=2&amp;pvsid=4168932593197103&amp;tmod=865723311&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=336" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260709/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>