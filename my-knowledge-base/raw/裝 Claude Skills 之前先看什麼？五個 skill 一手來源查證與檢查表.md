---
title: "裝 Claude Skills 之前先看什麼？五個 skill 一手來源查證與檢查表"
source: "https://www.aiposthub.com/claude-skills-five-check-before-install-claude-mem-skills-sh-audit/"
author:
  - "[[Philo]]"
published: 2026-11-13
created: 2026-11-19
description: "Stop Slop、Claude-Mem、UI/UX Pro Max、Task Observer、Find Skills 對照 README、原始碼與 skills.sh 稽核頁，整理安裝指令與十二項檢查表。"
tags:
  - "clippings"
status: "inbox"
---
Instagram 上有一篇貼文列了五個 Claude Skills：Stop Slop、Claude-Mem、UI/UX Pro Max、Task Observer、Find Skills，說「同樣用 Claude，為什麼別人用得比你好？差別常常在有沒有裝對 Skills」，留言「Claude」就私訊五個安裝指令。我們回到這五個專案自己的 GitHub 頁面、文件和 skills.sh 的稽核頁，逐一查：它是什麼、誰維護、授權、安裝指令寫什麼，以及裝了之後會在你的電腦上做什麼。

先說結論（資料時間：2026 年 11 月 10 日晚上（台北）； **我們沒有安裝任何一個 skill，也沒有執行任何專案的指令** ，只讀公開頁面與原始檔）：

- **貼文的描述大致對得上，差別在沒講的那一半。** Stop Slop 只有 7 個文字檔；UI UX Pro Max 的「80 種風格」其中 51 種是預設會用到的，「119 條準則」在 11 月 7 日的更新後變成 120 條；Find Skills、Task Observer 的「幫你裝好」「持續優化」都有前提（你同意、你審核）。
- **五個專案裝進去的東西輕重差很多。** 從輕到重：只有文字檔 → 本機查詢腳本 → 教 Claude 跑安裝指令 → 八個腳本加排程 → 常駐背景服務加第二個 AI。
- **最重的是 Claude-Mem。** 它的 README 自己寫了：hook 把 Claude 的每個工具呼叫交給本機服務、另一個 AI 把它整理成筆記、做筆記要花誰的額度、安裝時要登入、匿名統計預設開、還有一道會擋下整檔讀取的檔案閘門。這些都是 README 與原始碼寫的設定，不是缺點，是裝之前要先知道的事。
- **skills.sh 對每個 skill 有三項自動稽核。** 五個裡有三個全部通過、Find Skills 有一項警告、Task Observer 有一項標示失敗。這是第三方自動掃描的結果，標記不等於定論；我們只轉述，並寫清楚哪些我們查不到。
- **裝之前先看三件事：會跑什麼腳本、資料存去哪誰付錢、稽核頁怎麼寫。** 文末有一份十二項的檢查表。

連 Find Skills 自己的說明書都這樣寫：第四步「Verify Quality Before Recommending」，要先查安裝數、來源和 GitHub 星數，再推薦給使用者（下面會放原文）。這篇是 AI郵報同主題 Reel 的完整整理，包含貼文留言承諾的「5 個安裝指令」：我們只照各專案 README 或 skills.sh 頁面的原文列出，不加我們自己的指令。

![skills.sh 首頁的 Skills Leaderboard：find-skills 排第一，4.3M 次安裝](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_feature.jpg)

圖：skills.sh（Vercel 的 skill 目錄）首頁排行榜，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，頁面沒有寫再利用條款，AI郵報僅標名引用。

## 貼文寫的，跟一手來源對得上嗎？

貼文網址： [IG 貼文](https://www.instagram.com/p/Dfg9v40j_p7/?ref=aiposthub.com) （約 11 月 9 日發出，我們 11 月 10 日讀；我們沒有留言、沒有追蹤，所以 **沒有收到貼文承諾的五個安裝指令與文章** ，下面的安裝指令都是從各專案自己的 README 或 skills.sh 頁面抄來的）。標籤： **官** ＝一手來源（專案自己的頁面、原始檔、文件）； **三** ＝第三方； **本** ＝本站整理或推論。

| 貼文說 | 判定 | 一手來源怎麼寫 | 標 |
| --- | --- | --- | --- |
| Stop Slop｜自動刪掉 AI 腔，文章讀起來像真人 | 要加註 | SKILL.md 是一份寫作規則（刪填充句、用主動語態、不用破折號、五項評分）。Claude 讀到後照規則寫；README 的說法是「teaches Claude to catch and remove」。「自動」與「像真人」是效果描述，我們沒有測。 | 官 |
| Claude-Mem｜跨對話記憶，開新對話不用重講背景 | 準確，要加註 | README 的 What You Get 第一項：下一次對話從上一次停下來的地方開始。加註：第二次對話才開始注入；做法是 hook＋本機服務＋第二個 AI（後面詳述）。 | 官 |
| UI/UX Pro Max｜80 種 UI 風格 | 準確，要加註 | catalog-summary.json（verifiedAt 11/4）：styles.csv 共 89 列，80 種「可搜尋」（active 51＋supplemental 29），另有 9 種 deprecated；預設推薦只用 51 種。 | 官 |
| UI/UX Pro Max｜119 條 UX 準則 | 更新前的數字 | ux-guidelines.csv 在 11 月 7 日的 commit（feat: add IME-safe Enter shortcut guidance #562）之前是 119 列，之後 120 列；README 與 SKILL.md 現在寫 120。貼文寫的是更新前的數字，不是寫錯。 | 官 |
| Task Observer｜觀察你的工作方式，持續優化你的 Skills | 要加註 | README：「Nothing touches a skill until you approve it」——它記錄觀察、提出修改，你審核並安裝更新版本才會改 skill；還要自己在 CLAUDE.md 加啟動指令，否則常不會觸發（作者自述）。 | 官 |
| Find Skills｜說你想做什麼，它幫你找到並裝好 | 要加註 | SKILL.md 的步驟：了解需求→先看 skills.sh 排行榜→搜尋→Step 4 先驗證品質→Step 5 列選項給使用者→Step 6「If the user wants to proceed」才安裝。「幫你裝好」是在你同意之後。 | 官 |
| 貼文影片第一張卡片「Stop Slop ★18.7k」 | 準確（時間差） | 我們 11/10 晚上讀到 18,803 顆星（18.8k）；貼文發出時更少是合理的。 | 官 |

整體看，貼文沒有「不準」的地方；它用一句話介紹每個 skill，我們做的事是把每一句話背後的另一半補上。這不是在說貼文不好，是因為 skill 和一般的 App 不一樣：裝進去之後，它會被 Claude 讀、有的還會在你的電腦上跑腳本，所以值得多看一眼。

## 貼文留言要送的「5 個安裝指令」是什麼？

我們沒有拿到貼文私訊的內容，所以下面改列各專案自己公開寫的安裝方式， **照原文貼上，標明出處** ；每一條我們都沒有執行過。 **提醒：貼上指令之前，先讀完後面對應的那一節，確認你接受它會做的事。**

### 1\. Stop Slop（README 原文）

README 的 Quick start 寫：「Claude Code: Add this folder as a skill.」（把這個資料夾加成一個 skill；沒有給指令）。skills.sh 頁面顯示的安裝指令是：

```
npx skills add https://github.com/hardikpandya/stop-slop --skill stop-slop
```

### 2\. Claude-Mem（README 原文）

```
npx claude-mem install

# 或在 Claude Code 裡：
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```

README 另外寫：`npm install -g claude-mem` 只會安裝 SDK／函式庫， **不會** 註冊外掛的 hook，要用上面兩種方式之一。

### 3\. UI/UX Pro Max（README 原文）

```
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill

# 或用 CLI（README 標「Recommended」）：
npm install -g ui-ux-pro-max-cli
uipro init --ai claude
```

### 4\. Task Observer（README 原文）

```
npx skills add rebelytics/one-skill-to-rule-them-all --skill task-observer
```

README 接著寫「Activate it」：還要把 `references/environments.md` 裡的啟動指令加到你的 CLAUDE.md，並說「Installing alone is not enough」（只裝不夠）。

### 5\. Find Skills（skills.sh 頁面原文）

```
npx skills add https://github.com/vercel-labs/skills --skill find-skills
```

以上五組都是各專案寫給人類使用者的做法。 **安裝會把檔案寫進你的電腦（專案或全域的 skills 資料夾；外掛還會註冊 hook），所以才要先讀完下面每一節。**

## Stop Slop 裝了會多跑什麼？只有一份文字規則

| 項目 | 內容 | 標 |
| --- | --- | --- |
| 是什麼 | 「A skill file for removing AI tells from prose」：一份教 Claude 去掉 AI 腔的寫作規則 | 官 |
| 誰維護 | Hardik Pandya（GitHub 個人帳號 hardikpandya） | 官 |
| 星數與日期 | 18,803 顆星、1,381 forks；2026-01-11 建立；最後一次推送 2026-03-17（約 8 個月前） | 官 |
| 授權 | MIT | 官 |
| 倉庫裡有什麼 | 7 個檔案：CHANGELOG.md、LICENSE、README.md、SKILL.md，加 references/ 下的 examples.md、phrases.md、structures.md；沒有.sh／.py／.js | 官 |
| 在本機做什麼 | 不讀不寫檔案、不連網、不跑腳本；裝了＝Claude 多讀一份規則 | 本 |

![GitHub stop-slop 倉庫頁：檔案清單只有 references、CHANGELOG.md、LICENSE、README.md、SKILL.md](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_stopslop_repo.jpg)

圖：GitHub hardikpandya/stop-slop 倉庫頁，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（MIT）。

SKILL.md 的核心規則有八條：刪填充句、避開固定句型、用主動語態、說具體的事、把讀者放進場景、變化節奏、相信讀者、刪掉像金句的句子；並附一張五項評分表（directness、rhythm、trust、authenticity、density，各 1–10 分，低於 35 分就重寫）。對使用者來說，它能不能讓文章「像真人」，要靠你自己用過才知道；我們沒有測。

**這一個的稽核最乾淨：** skills.sh 上三項稽核全部是 Pass（見後面「稽核」一節）。這也符合它只有文字檔的結構。

## UI/UX Pro Max 裝了會做什麼？本機資料庫加查詢腳本，貼文的數字要加註

| 項目 | 內容 | 標 |
| --- | --- | --- |
| 是什麼 | 「An AI skill that provides design intelligence for building professional UI/UX across multiple platforms」：一份設計資料庫（風格、色票、字體配對、圖表、UX 準則、22 種技術堆疊的規則）加查詢腳本 | 官 |
| 誰維護 | 組織帳號 nextlevelbuilder | 官 |
| 星數與日期 | 147,635 顆星（五個裡最多）、15,621 forks；2025-11-30 建立；最後 commit 11 月 10 日；版本 v2.15.0 | 官 |
| 授權 | MIT | 官 |
| 在本機做什麼 | README：Python 查詢腳本「standard library only — the scripts install nothing and make no network calls」（只用標準函式庫，不安裝東西、不連網）。我們讀了 core.py、design\_system.py、reasoning\_contract.py、search.py，沒有看到 socket、requests、subprocess、os.system 這類連網或執行外部指令的程式 | 官＋本 |
| 要注意 | 外掛的 plugin.json 把 skills 指向整個.claude/skills/ 資料夾，那裡有 7 個 skill（banner-design、brand、design-system、design、slides、ui-styling、ui-ux-pro-max），其中 ui-styling 的 shadcn\_add.py 會呼叫 npx。這是我們依設定檔推論，沒有實際安裝驗證 | 本 |

### 「80 種風格」與「119 條準則」

![UI UX Pro Max README 的 Style Taxonomy：Active 51、Supplemental 29、Deprecated 9](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_ux_taxonomy.jpg)

圖：GitHub nextlevelbuilder/ui-ux-pro-max-skill README「Style Taxonomy」，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（MIT）。

README 寫的是「80 searchable styles」（80 種可搜尋）：Active 51 種會出現在一般推薦裡，Supplemental 29 種只有明確指定時才會被查到，另有 Deprecated 9 種不參與排名（styles.csv 共 89 列）。所以貼文的「80 種」準確，但「預設會用到」的是 51 種。

![catalog-summary.json 原始檔：styles total 89、searchable 80、active 51、uxGuidelines 120](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_ux_catalog.jpg)

圖：GitHub nextlevelbuilder/ui-ux-pro-max-skill 的 catalog-summary.json 原始檔，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（MIT）。

UX 準則的數字：catalog-summary.json 與 README 現在都寫 120 條。我們比對了 git 紀錄：ux-guidelines.csv 在 11 月 7 日的 commit 之前是 119 列、之後是 120 列。貼文寫 119，是更新前的數字。

## Find Skills 會替你裝什麼？它自己的第四步寫「先驗證」

| 項目 | 內容 | 標 |
| --- | --- | --- |
| 是什麼 | Vercel Labs 的 `skills` 指令（npx skills）的搭配說明書：一份 5.5 KB 的 SKILL.md，教 Claude 在你說「怎麼做 X」時，去搜尋、查品質、列選項、經你同意後安裝 | 官 |
| 誰維護 | 組織帳號 vercel-labs（倉庫 vercel-labs/skills，find-skills 是裡面的 skills/find-skills/SKILL.md） | 官 |
| 星數與日期 | 倉庫 38,828 顆星、3,384 forks；最後推送 11 月 10 日；skills.sh 顯示 find-skills 4.3M 次安裝、排行榜第 1 名（約 130 萬個 skill 裡） | 官＋三 |
| 授權 | MIT | 官 |
| 在本機做什麼 | Claude 會跑 npx skills find／add：下載 npm 套件、讀 GitHub、把別人的 skill 寫進 ~/.claude/skills（-g＝全域）。CLI 有匿名遙測，設 DISABLE\_TELEMETRY=1 或 DO\_NOT\_TRACK=1 可關（README） | 官 |

### Step 4：先驗證品質；Step 6：全域安裝、跳過確認

![find-skills SKILL.md 的 Step 4：Verify Quality Before Recommending，要先查安裝數、來源、GitHub 星數](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_fs_step4.jpg)

圖：GitHub vercel-labs/skills 的 skills/find-skills/SKILL.md，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 Vercel Labs（MIT）。

Step 4 的標題是「Verify Quality Before Recommending」（推薦之前先驗證品質；AI郵報自譯），第一句寫：Do not recommend a skill based solely on search results（不要只靠搜尋結果就推薦一個 skill；AI郵報自譯）。接著列三項：

1. **Install count** （安裝數）：偏好 1K 以上；低於 100 要小心。
2. **Source reputation** （來源）：官方來源（vercel-labs、anthropics、microsoft）比不認識的作者可信。
3. **GitHub stars** （星數）：看來源倉庫；星數少於 100 的 skill 要抱持懷疑（should be treated with skepticism）。
![find-skills SKILL.md 的 Step 6：npx skills add … -g -y，-g 全域安裝、-y 跳過確認](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_fs_step6.jpg)

圖：GitHub vercel-labs/skills 的 skills/find-skills/SKILL.md，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 Vercel Labs（MIT）。

Step 6 的條件是「If the user wants to proceed」，也就是使用者同意之後才安裝；指令是 `npx skills add <owner/repo@skill> -g -y`，說明書寫的是：-g 全域安裝（user-level）、-y 跳過確認提示。 **所以「說你想做什麼，它幫你找到並裝好」成立的前提是你說了好，而且你要知道 -g -y 代表什麼。**

## Task Observer 要在你的電腦上做什麼？八個腳本、要改 CLAUDE.md、建議排程

| 項目 | 內容 | 標 |
| --- | --- | --- |
| 是什麼 | 「The meta-skill that improves your skills」：觀察你的工作，把你的修正和重複的流程記成「觀察」，再整理成 skill 的修改建議；README：Nothing touches a skill until you approve it（你批准之前不會改任何 skill） | 官 |
| 誰維護 | Eoghan Henn（GitHub 帳號 rebelytics；README 與 GitHub 個人頁自述是 Task Observer 與 Augmented Expertise 方法的作者）；README 說有 100 多位貢獻者 | 官 |
| 星數與日期 | 3,834 顆星、383 forks；2026-02-15 建立；最後推送 11 月 9 日；skills.sh 顯示 17.9K 次安裝 | 官＋三 |
| 授權 | **CC BY 4.0** （要署名：Eoghan Henn／rebelytics，並連回 github.com/rebelytics/one-skill-to-rule-them-all；本文已署名） | 官 |
| 裝進去的東西 | SKILL.md（約 50 KB）、references/、scripts/ 資料夾 8 個檔案（6 個.sh：bin-check、diff-staged、new-observation、session-start-scan、set-fields、skill-load；2 個.py：migrate-log、validate-skill-bundle）；另有選用的實驗外掛 task-observer-flush（Stop hook） | 官 |
| 在本機做什麼 | 讀寫你指定的工作資料夾（skill-observations/observation-log/、skill-updates/）；README「Data and network」：平常不連網，只讀寫本機檔案，有三個抓取（回報前檢查、第三方專案上游檢查、版本檢查）都要你主動啟動；validate-skill-bundle.py 用 subprocess 呼叫 sha256sum；README 建議你排程（cron、launchd）定期執行 review | 官 |
| 要你自己做 | 把啟動指令加進 CLAUDE.md；README 說：因為單靠 skill 描述比對常常不觸發（under-triggers），只裝不夠 | 官 |

![task-observer 的 scripts 資料夾：bin-check.sh、diff-staged.sh、migrate-log.py、new-observation.sh、session-start-scan.sh、set-fields.sh、skill-load.sh、validate-skill-bundle.py](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_to_scripts.jpg)

圖：GitHub rebelytics/one-skill-to-rule-them-all 的 scripts 資料夾，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 Eoghan Henn（CC BY 4.0）。

![task-observer README 的 Quick install：安裝後還要把啟動指令加到 CLAUDE.md，Installing alone is not enough](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_to_install.jpg)

圖：GitHub rebelytics/one-skill-to-rule-them-all README「Quick install」，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 Eoghan Henn（CC BY 4.0）。

**這一個「裝了會做什麼」最明確：** 它的設計就是要在你的工作資料夾寫檔案、在每次工作開始時掃描紀錄、並建議你排程。這些在 README 都寫得很清楚；它的稽核結果有一項標示失敗，理由見下面「稽核」一節。

## Claude-Mem 的「跨對話記憶」背後是什麼？README 寫了一長串設定

| 項目 | 內容 | 標 |
| --- | --- | --- |
| 是什麼 | 「Persistent Context Across Sessions for Every Agent」：把 Claude Code 等工具每次工作的紀錄，壓縮成筆記，下一次對話再放回去 | 官 |
| 誰維護 | Alex Newman（GitHub 帳號 thedotmack） | 官 |
| 星數與日期 | 115,658 顆星、10,040 forks；2025-08-31 建立；最新版 v15.15.1（11 月 10 日發佈；11/9–11/10 一天內發了 4 個版本）；skills.sh 上它的 mem-search skill 有 9.3K 次安裝 | 官＋三 |
| 授權 | Apache-2.0 | 官 |

### 它怎麼做到「跨對話記憶」？

![claude-mem README 的 How It Works：Your agent works、The observer takes notes、The next session starts with them](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_how.jpg)

圖：GitHub thedotmack/claude-mem README「How It Works」，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0）。

1. **Your agent works.**（你的 agent 工作）：hook 把 agent 的每個工具呼叫——讀了什麼、改了什麼、跑了什麼——交給一個本機的小服務（README 原話：hand each tool call to a small local worker）。
2. **The observer takes notes.**（觀察員做筆記）：「第二個 agent」把這些活動變成簡短的觀察，並在對話結束時寫摘要。
3. **The next session starts with them.**（下一次對話帶著它們開始）：新對話開始時，agent 會拿到一份精簡的索引，需要細節時再去查。README 補充：記憶要到同一個專案的第二次對話才開始注入。
![claude-mem README 的示範動圖：左邊是 Claude Code 終端機，右邊是記憶清單](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_demo.gif)

圖：claude-mem README 的官方示範動圖 cm-preview.gif（21 秒；左邊是 Claude Code 終端機，右邊是記憶清單），原檔未修改；檔案在 GitHub thedotmack/claude-mem（Apache-2.0）。

### hook 掛在哪些事件上？

外掛的 hooks.json 把 hook 掛在 Setup、SessionStart、UserPromptSubmit、PreToolUse、PostToolUse、PostToolUseFailure、Stop、SubagentStop 和 SessionEnd 這些事件上；其中 PostToolUse 的 matcher 是「\*」，也就是每一個工具呼叫之後都會執行（非同步，timeout 120 秒）。本機服務預設只聽 127.0.0.1。

![claude-mem 的 hooks.json 原始檔：PostToolUse 的 matcher 是 \*，指令是 claude-mem hook claude-code observation](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_hooks.jpg)

圖：GitHub thedotmack/claude-mem 的 plugin/hooks/hooks.json 原始檔，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0）。

### 做筆記的「第二個 AI」是誰、花誰的額度？

這是最容易被一句話帶過、也最值得先看的一點。我們讀到三處， **答案要分情況** ：

- **設定檔的預設值** ：原始碼 SettingsDefaultsManager.ts 裡 CLAUDE\_MEM\_PROVIDER 預設是 'claude'、CLAUDE\_MEM\_MODEL 預設是 'haiku'（官）。
- **安裝程式的互動畫面預選的是另一個** ：install.ts 的提供者選單有兩個選項（CMEM Pro 託管觀察員、Claude），程式碼註解寫「CMEM Pro pre-selected: it is the recommended path」，`initialValues: ['cmem']`（官）。同一份設定檔的註解也寫：互動安裝的提示預設是 'cmem'，沒有互動的無人值守安裝才落在 'claude'。
- **README 的說法** ：登入後你選記憶提供者——claude-mem observer（託管觀察員）、你自己的 OpenRouter 或 Gemini 金鑰、或你的 Anthropic 方案；選觀察員會開始 30 天免費試用，結帳頁會要付款方式；試用結束若沒訂閱，記憶會自動退回用你的 Anthropic 方案。README 還說觀察員讓記憶「runs off-plan」（不佔你的方案額度），反過來說，用 Anthropic 方案做筆記，花的就是你自己方案的用量（官）。
![SettingsDefaultsManager.ts 的註解與設定：互動安裝提示預設 cmem，設定檔預設 provider 是 claude](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_defaults.jpg)

圖：GitHub thedotmack/claude-mem 的 src/shared/SettingsDefaultsManager.ts 原始檔，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0）。

![install.ts 的提供者選單：CMEM Pro pre-selected，initialValues: \['cmem'\]](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_installer.jpg)

圖：GitHub thedotmack/claude-mem 的 src/npx-cli/commands/install.ts 原始檔，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0）。

我們 **沒有安裝、沒有量測** 它實際用掉多少額度，所以不給數字；程式碼註解也寫「Selecting it no longer means 'pay now' — it opens the offer page to read first」（選它不再等於「現在就付款」，而是先打開方案頁給你讀；AI郵報自譯）。要不要登入、選哪個提供者，是你在安裝時要自己決定的事。

### 登入、試用、略過登入、匿名統計

![claude-mem README Quick Start：登入、30 Day Free Trial、a payment method、Prefer to skip the sign-in、Anonymous usage analytics are on by default](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_trial.jpg)

圖：GitHub thedotmack/claude-mem README Quick Start，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0）。

- **登入** ：installer 設定完後，會請你在瀏覽器登入 claude-mem（email 魔法連結，登入不需要信用卡）。（官）
- **付費觀察員** ：選它會開始 30 天免費試用，結帳頁要先填付款方式。（官；我們沒有看價格頁，不寫價格）
- **可以略過登入** ：README 寫「Prefer to skip the sign-in? Pass an explicit --provider flag, set CLAUDE\_MEM\_ONLINE\_OPTIN=false, or run in CI/non-interactive shells」。（官）
- **匿名使用統計預設開** ：README 寫「Anonymous usage analytics are on by default; npx claude-mem telemetry disable turns them off」；文件頁說明用 PostHog、以隨機安裝 UUID 識別、事件欄位走白名單、安裝時會問一次，DO\_NOT\_TRACK 環境變數優先。我們沒有驗證實際送出的內容。（官）
- **資料存哪** ：README 寫記憶放在你電腦的 ~/.claude-mem（SQLite），雲端同步是 opt-in；預設設定檔的雲端同步網址是空的（＝關閉）。螢幕截圖預設會留一份轉換過的副本在 ~/.claude-mem/media-v1/，可在設定關掉。（官）

### 檔案閘門：它不只「記」，也會改 Claude 讀檔的方式

![claude-mem 文件 File Read Gate：stops Claude Code from reading a whole code file that claude-mem already has observations about](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_cm_gate.jpg)

圖：docs.claude-mem.ai 的 File Read Gate 頁，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬原作者（Apache-2.0 專案文件）。

文件說明：File Read Gate 會擋下 Claude Code 讀「claude-mem 已經有觀察紀錄的整個程式檔」，改給 Claude 這個檔案的觀察時間線，再指引它用 smart\_outline、smart\_unfold 等較省的方式讀。條件有七項同時成立：Claude Code 主對話（不是子代理）、檔案是 smart\_outline 能解析的程式檔、在專案資料夾內、 **至少 32 KB** 、有觀察紀錄且之後沒改過、這次讀的是整個檔、tree-sitter 工具已安裝。Markdown、YAML、JSON 等檔案不會被擋。這道閘門 **預設開啟** （The gate is on by default），可以在設定關掉。

README 另有一項保護：把內容包在 `<private>` 標籤裡，就不會被存起來。另外值得知道：README 的開頭有一段寫給 AI agent 的安裝指示（「Installing for someone as an AI agent? Run npx claude-mem install…」）；如果你請 AI 幫你讀這份 README 並安裝，請自己確認它要執行什麼。

**我們的結論只有一句：這些全是 README、文件和預設設定檔自己寫的設定，不是缺點，是裝之前要先知道的事。** 我們沒有評斷它值不值得裝。

## skills.sh 的三項自動稽核，怎麼看？

skills.sh 是 Vercel 的 skill 目錄網站（約 130 萬個 skill）。每個 skill 的頁面右側有「Security Audits」，列出三家的自動稽核：Gen Agent Trust Hub、Socket、Snyk，結果是 Pass、Warn 或 Fail，每一項可以點進去看理由。 **稽核範圍是「那一頁的那一個 skill」，不是整個專案** （所以 Claude-Mem 只有它裡面的 mem-search skill 有頁面）。 **這是第三方自動掃描，標記不等於定論，也可能誤判。**

| skill 頁面（skills.sh） | 安裝次數 | Gen Agent Trust Hub | Socket | Snyk | 標 |
| --- | --- | --- | --- | --- | --- |
| hardikpandya/stop-slop／stop-slop | 19.4K | Pass | Pass | Pass | 三 |
| nextlevelbuilder/ui-ux-pro-max-skill／ui-ux-pro-max | 444.1K | Pass | Pass | Pass | 三 |
| thedotmack/claude-mem／mem-search（只是 claude-mem 裡的一個 skill） | 9.3K | Pass | Pass | Pass | 三 |
| vercel-labs/skills／find-skills | 4.3M | Pass | Pass | **Warn** （Medium） | 三 |
| rebelytics/one-skill-to-rule-them-all／task-observer | 17.9K | **Fail** （Risk CRITICAL） | Pass | Pass | 三 |

![skills.sh 的 stop-slop 頁面：Gen Agent Trust Hub、Socket、Snyk 都是 Pass](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_sh_slop.jpg)

圖：skills.sh 的 stop-slop 頁面，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，頁面沒有寫再利用條款，AI郵報僅標名引用。

### Find Skills：Snyk 的警告是「它的工作本身」

![skills.sh 的 find-skills 頁面：Gen Agent Trust Hub Pass、Socket Pass、Snyk Warn](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_sh_find.jpg)

圖：skills.sh 的 find-skills 頁面，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，頁面沒有寫再利用條款，AI郵報僅標名引用。

Snyk 審查頁（2026 年 9 月 15 日審查，Risk Level MEDIUM，分數 low risk 0.10）寫的是 W011：Third-party content exposure detected (indirect prompt injection risk)（偵測到第三方內容暴露，有間接提示詞注入的風險；AI郵報自譯）。理由是：這個 skill 會透過 CLI 搜尋並讀進開放社群的套件資料與 skill 內容。也就是說，警告來自它「要讀社群發佈的內容」這個工作本身，並不是指它做了什麼壞事。

### Task Observer：一項標示失敗，理由我們只能轉述

![skills.sh 的 task-observer 頁面：Gen Agent Trust Hub Fail、Socket Pass、Snyk Pass](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_sh_to.jpg)

圖：skills.sh 的 task-observer 頁面，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，頁面沒有寫再利用條款，AI郵報僅標名引用。

Gen Agent Trust Hub 的審查頁（2026 年 11 月 9 日審查，Risk Level CRITICAL）列出四個類別，並在「Recommendations」寫：

![task-observer 稽核頁的 Recommendations：Contains 2 malicious URL(s) - DO NOT USE](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_sh_to_reco.jpg)

圖：skills.sh 上 task-observer 的 Gen Agent Trust Hub 審查頁，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，AI郵報僅標名引用。

頁面寫「Contains 2 malicious URL(s) - DO NOT USE」（含有 2 個惡意網址，請勿使用；AI郵報自譯）， **但沒有指出是哪兩個網址** 。我們能確認的是：

- **同一頁的分析本文列了四項** ：COMMAND\_EXECUTION（用 scripts/ 裡的 Bash 與 Python 腳本；validate-skill-bundle.py 用 subprocess.run 呼叫 sha256sum）、PERSISTENCE（weekly-review.md 建議用 cron、launchd 或 Windows 工作排程器註冊定期檢查）、EXTERNAL\_DOWNLOADS（skill-authoring.md 描述用 curl 抓 GitHub 上游檔案比對；weekly-review.md 有選用的上游版本檢查）、INDIRECT\_PROMPT\_INJECTION（會讀 GitHub issue、PR 與個人頁文字，說明書明寫當資料不當指令）。（三）
- **這四項，在 Task Observer 自己的 README 與 SKILL 都有寫** （README「Data and network」：三個抓取都要你主動啟動）。分析本文沒有任何一項描述惡意行為。（本）
- **被標記的兩個網址，我們查不到。** 我們把該倉庫文字檔裡出現的網址網域掃過：大宗是 README「Recommended by／Listed in」清單裡的社群與媒體連結（Instagram、X、TikTok、LinkedIn、YouTube、Medium 等）和各 skill 目錄站，無法判斷是哪兩個被標記。Socket 與 Snyk 兩家都是 Pass。（本）
- **我們沒有驗證那兩個網址，也不下結論。** 稽核可能已被作者處理、可能是誤判，也可能有我們看不出來的問題——所以這個 Fail 的用途是提醒你「自己看過再裝」，而不是替誰定罪。
![task-observer 的 Gen Agent Trust Hub 稽核頁：Risk Level CRITICAL，列出 COMMAND\_EXECUTION、PERSISTENCE、EXTERNAL\_DOWNLOADS、INDIRECT\_PROMPT\_INJECTION 四個類別與分析](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/skills5_sh_to_audit.jpg)

圖：skills.sh 上 task-observer 的 Gen Agent Trust Hub 審查頁，2026 年 11 月 10 日晚上（台北）擷取並裁切；畫面屬 skills.sh，AI郵報僅標名引用。

## 裝第三方 skill 之前，先看哪十二件事？（檢查表）

下面這份是 **本站整理的檢查表，不是專案或 Vercel 的規則** ；最右欄是這五個例子裡我們看到的答案。每一項大約花幾分鐘，多數只要打開倉庫頁面就查得到。

| # | 要看什麼 | 去哪看 | 這五個例子的答案（標） |
| --- | --- | --- | --- |
| 1 | 裝進去的是什麼檔案：只有 markdown，還是有腳本、hooks、外掛設定？ | 倉庫檔案清單；scripts/、hooks/、plugin.json | Stop Slop 全是文字；UI UX Pro Max 有 Python 腳本；Task Observer 有 8 個腳本；Claude-Mem 有 hooks＋常駐服務（官） |
| 2 | 有沒有 hook？掛在哪些事件？ | hooks.json、plugin.json | 只有 Claude-Mem（每個工具呼叫都會執行）；Task Observer 另有選用的實驗外掛（官） |
| 3 | 腳本會連網或執行外部指令嗎？ | README 的聲明＋自己 grep：socket、requests、urllib、subprocess、curl、npx | UI UX Pro Max 查詢腳本沒有（官＋本）；Task Observer README 寫平常不連網（官）；Find Skills 會跑 npx（官） |
| 4 | 有沒有常駐服務、排程或要你設定排程？ | README 的安裝與 Quick start | Claude-Mem 有本機服務；Task Observer 建議排程（官） |
| 5 | 資料存在哪、誰看得到？雲端同步預設開還是關？ | README 的 data／storage 段、預設設定檔 | Claude-Mem：~/.claude-mem，雲端同步預設關（官） |
| 6 | 做事要花誰的額度、要不要登入或付款？ | README 的 provider／pricing／trial 段、安裝程式原始碼 | Claude-Mem：設定檔預設 Claude Haiku；安裝畫面預選託管觀察員；30 天試用要付款方式；沒訂閱就用你的方案（官） |
| 7 | 有匿名統計（遙測）嗎？預設開嗎？怎麼關？ | README、文件的 telemetry 頁 | Claude-Mem 預設開；skills CLI 有，DISABLE\_TELEMETRY=1 可關（官） |
| 8 | 它會改變 Claude 的行為嗎？（擋讀檔、要你改 CLAUDE.md） | README、文件 | Claude-Mem 的檔案閘門預設開；Task Observer 要改 CLAUDE.md（官） |
| 9 | 安裝範圍：專案還是全域？外掛會一次帶幾個 skill？ | 安裝指令的旗標（-g）、plugin.json 的 skills 路徑 | Find Skills 的指令是 -g -y；UI UX Pro Max 外掛指向整個.claude/skills（本，未驗證） |
| 10 | skills.sh 稽核頁怎麼寫？理由是什麼？ | skills.sh 上該 skill 頁面的 Security Audits，點進去看理由 | 三個全 Pass、一個 Snyk Warn、一個 Gen Agent Trust Hub Fail；都是自動掃描（三） |
| 11 | 授權與維護狀態？ | LICENSE、最後 commit、發版紀錄、issues | MIT／Apache-2.0／MIT／CC BY 4.0／MIT；Stop Slop 最後推送約 8 個月前，Claude-Mem 一天發 4 版（官） |
| 12 | 要怎麼移除？ | README 的 uninstall／remove 段 | UI UX Pro Max README 有 `uipro uninstall`；skills CLI 有 `npx skills remove`（官）；其餘請看各 README |

**星數與安裝次數只代表關注度，不代表品質。** Find Skills 自己的說明書把它們當作「要查」的三項之一（安裝數、來源、星數），而不是答案本身。

## 常見問題

### 所以這五個哪個該裝？

這篇不給推薦。它們解決的是不同的問題：寫作規則、設計資料庫、跨對話記憶、改進你的 skill、找別人的 skill。你需要哪一個、願意接受它在電腦上做什麼，是你要自己決定的；我們做的是把「它會做什麼」攤開。

### skills.sh 標 Fail 或 Warn，代表那個 skill 有毒嗎？

不代表。稽核是第三方的自動掃描，頁面會寫理由；Find Skills 的 Warn 來自「要讀社群內容」，Task Observer 的 Fail 欄位寫的網址我們查不到，同頁分析列的四項又都是作者說明書公開的功能。該做的是點進理由、對照 README 與原始碼，而不是只看顏色；反過來，全部 Pass 也不等於保證沒有問題。

### 我沒有安裝這些 skill，你們怎麼知道它們會做什麼？

我們讀的是公開的 README、文件、設定檔和原始碼，並對照 skills.sh 的稽核頁；沒有安裝、沒有執行、沒有量測用量。所以凡是寫「README 說」「原始碼寫」的，都是專案自己寫的；凡是標「本」的，是我們的整理或推論，裡面也註明了哪些沒有驗證。

### 貼文作者提供的五個安裝指令，跟這篇列的一樣嗎？

我們沒有拿到貼文私訊的內容，無法比對。這篇列的是各專案 README 或 skills.sh 頁面公開寫的安裝方式；不論從哪裡拿到指令，裝之前都建議先讀對應那一節。

### Claude-Mem 的設定可以改嗎？

README 寫設定檔在 ~/.claude-mem/settings.json（第一次執行時自動建立），可以調整模型、埠、資料夾、記錄層級與注入內容；匿名統計可用 `npx claude-mem telemetry disable` 關閉；檔案閘門可在設定關掉（CLAUDE\_MEM\_FILE\_READ\_GATE\_ENABLED）。細節請看 [官方文件](https://docs.claude-mem.ai/configuration?ref=aiposthub.com) 。

## 資料來源

資料時間：2026 年 11 月 10 日晚上（台北）前後（11 月 10 日 21:56–23:00）。星數、安裝次數、稽核狀態與觀看數都會變動。圖片皆為上列官方頁面的截圖並裁切，畫面屬各專案作者或網站；各專案授權見表格。

- IG 貼文： [instagram.com/p/Dfg9v40j\_p7](https://www.instagram.com/p/Dfg9v40j_p7/?ref=aiposthub.com) （@your.engineer.ben，約 11/9 發）
- Stop Slop： [github.com/hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop?ref=aiposthub.com) （README、SKILL.md、LICENSE：MIT）
- Claude-Mem： [github.com/thedotmack/claude-mem](https://github.com/thedotmack/claude-mem?ref=aiposthub.com) （README、plugin/hooks/hooks.json、src/shared/SettingsDefaultsManager.ts、src/npx-cli/commands/install.ts；Apache-2.0）；文件： [docs.claude-mem.ai](https://docs.claude-mem.ai/?ref=aiposthub.com) （File Read Gate、Telemetry、Configuration）
- UI/UX Pro Max： [github.com/nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill?ref=aiposthub.com) （README、catalog-summary.json、styles.csv、ux-guidelines.csv 與 git 紀錄、plugin.json、scripts；MIT）
- Task Observer： [github.com/rebelytics/one-skill-to-rule-them-all](https://github.com/rebelytics/one-skill-to-rule-them-all?ref=aiposthub.com) （README、SKILL.md、scripts/、references/；CC BY 4.0，作者 Eoghan Henn／rebelytics）
- Find Skills 與 skills 指令： [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills?ref=aiposthub.com) （README、skills/find-skills/SKILL.md、docs/troubleshooting.md；MIT）
- skills.sh： [skills.sh](https://skills.sh/?ref=aiposthub.com) 與各 skill 頁面及 /security/ 審查頁（stop-slop、ui-ux-pro-max、mem-search、find-skills、task-observer）

**更正與回報：** 如果你是這些專案的作者，認為上面任何一句寫得不準，請告訴我們，我們會查證後更正。