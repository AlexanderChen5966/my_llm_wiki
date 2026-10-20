---
title: "把 Claude Code 的團隊開發規則從「需要記住的東西」變成「自動遵守的東西」"
source: "https://codelove.tw/@tony/post/a6GDnq"
author:
published:
created: 2026-06-22
description: "[](#%E5%AF%A6%E7%8F%BE%E6%83%B3%E8%A6%81%E7%9A%84%E4%BA%8B%E6%83%85)實現想要的事情--------------------------------------------------------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月21日](https://codelove.tw/@tony/post/a6GDnq "2026-06-21 17:15:09")

## 把 Claude Code 的團隊開發規則從「需要記住的東西」變成「自動遵守的東西」

## 實現想要的事情

這樣對 Claude Code 下指令：

```
用 Rust 做一個顯示 “Hello, World!” 的程式。從建立 Issue 到實作、PR、合併都幫我做完。請依照這個儲存庫的規則來進行
```

我只告訴 Claude 這些就夠了。

- 不需要指示 commit message 的格式
- 也不需要指示 branch 名稱
- 也不用指示 Issue 和 PR 的關聯方式

即使如此，Claude Code 仍然會依照 **開發團隊的規則** 自動執行接下來的流程。

而且一旦嘗試提交像 AWS 金鑰這類祕密資訊，機器就會立刻停下來。

- 不需要記住 branch 的運作方式
- 也不需要背 commit 規範
- 審查者也不必每次都指出同樣的規約違反

不再把規則寫在 README 或 Wiki，讓人讀完後記住並遵守。改成放進 Claude Code 的 Skill 與 Hook，由 AI 與機器每次自動套用。人不必從頭到尾記住那些細碎規則，就算犯錯也會被擋下來。也就是說，從第一天起就能正確開發。

要建立的就是這樣的機制。

## 前言

團隊的開發規則，通常都以文字形式寫在 README 或內部 Wiki 裡。

而且隨著團隊成長，規則會越來越多。規則一多，文件也跟著變多。最後就會累積成沒有人能全部記住的龐大文件量。

像是「commit message 要用這種格式」、「不要直接 push 到 main」、「PR 一定要建立」、「不要提交 API 金鑰」等等。這些確實都有寫，但新進成員真的會全部讀完嗎？就算讀完，內容量也真的能記住嗎？

到最後還是得靠 review 指出「這裡違反規範了」，請對方修改，然後下一位新進成員來時又重複同樣的指正。每次人員更替，都要無止盡地重複同樣的溝通。

本文介紹的 [team-dev-kit](https://github.com/aRaikoFunakami/team-dev-kit) ，就是為了終結這種重複而建立的工具。它把規則從「讓人閱讀、記住、遵守的文件」，改成「Claude Code 每次自動套用的 Skill」與「在 commit 當下由機器攔截的 Hook」。從 clone 的那一刻起，就讓團隊規則立即生效。

實現「不用記、犯錯會自動被擋下來，所以從第一天就能正確開發」。

### 誰適用這個工具

如果只是自己一個人玩票性質地改自己的 repo，這個東西意義不大。

真正有感的是，在多個人、多个 repository 中反覆執行同一套規範的團隊。特別是有新成員進出，每次都要重講同樣 onboarding 說明的團隊；每次 review 都會出現規約違反指摘的團隊；以及不想只靠「小心一點」來避免誤把機密資訊提交上去，而是想用機制直接擋下來的團隊。

前提是，團隊已經在使用 Claude Code，或者準備開始使用。因為負責套用這些規則的，就是 Claude Code。

### 從「文件」到「Skill / Hook」

把規則寫成文件的做法，是建立在「人會閱讀、記住、遵守」的前提上。

這個前提的弱點在於，遵守規則的成本幾乎全都落在人的身上。閱讀的時間、記住的負擔、每次重新想起來並套用的注意力，最後都還是要由人來付出。新人一多，所有人就得重新付出同樣的成本。而且人會忘記，趕時間時也容易跳過步驟。規則寫得再漂亮，和實際有沒有被遵守，是兩回事。

改成用 Skill 與 Hook 運作之後，就不再是「Claude 來呼叫、記住、遵守」以外的事。

需要記住並套用規則的，不再是人，而是 Claude Code；而真正看守那條絕不能跨越的底線的，也不是人的注意力，而是機器。新進成員不需要先背規範才能開始開發。只要說一句「我想在登入畫面加上輸入檢查，請先建立 Issue 再開始」，Claude Code 就會自動做好 branch 切法、commit message 格式、PR 關聯等細節。

| 以前（文件運作） | 之後（Skill 運作） |  |
| --- | --- | --- |
| 規則放置位置 | 散落在 README / Wiki | 集中到 Skill / Hook |
| 誰負責遵守 | 人（記憶與注意力） | AI（每次自動套用）與機器（commit 時檢查） |
| 事前需要的知識 | 讀完並記住全部規則 | 幾乎不需要（只要告訴目的） |
| 出錯時 | review 指出 → 重新修改 | 當場由 Hook 擋下／Skill 修正 |

### Skill 只是以前的「自然語言文件」

聽到「移到 Skill」時，或許會以為是要把規則重寫成程式，聽起來很難。其實不是。

Skill 的內容就是 Markdown 文字。以前寫在 README 裡的「commit message 要用 `type(scope): subject` 格式」這句話，幾乎原封不動就會變成 Skill 裡的一句話。它沒有執行程式碼。Claude Code 讀了這段文字之後，會改變自己的行為。所以與其說是移轉，不如說只是寫的地方和閱讀的人改變了。以前是寫給人看，現在是寫給 AI 看。

維護方式也一樣。想改規則，就改 Markdown。git 會留下歷史、PR 會做 review，這些都和原本的文件管理方式完全相同。實際上，team-dev-kit 的 Skill 也把移轉前那份「給人閱讀並遵守的規則文件」保存在 [`docs/legacy-manual-rules/`](https://github.com/aRaikoFunakami/team-dev-kit/tree/main/docs/legacy-manual-rules) 。可以直接對照看出哪些內容被移到 Skill 了。

具體來說，哪些文件變成哪些 Skill / Hook，大致如下：

| 舊文件（給人看） | 取代後的 Skill / Hook（由 AI 運作） | 遵守方式 |
| --- | --- | --- |
| commit 規範 | `git-commit` skill | 在你說「提交」時，自動產生正確格式 |
| branch 運作、Issue/PR 運作 | `github-workflow` skill | 自動建立 branch、附加 `Closes #N` 、禁止直接 push |
| Issue 建立指南／模板 | `ticket-*` skill 群 | 從對話與規劃生成草稿 → 發行 |
| 文件撰寫規則 | `doc-writing` skill | 自動套用設計文件的文體與結構 |
| 機密資訊處理規則 | `pre-commit hook` + `PreToolUse hook` （gitleaks） | 在 commit 前、發行前由機器檢查，危險時直接 **阻擋** |

### Skill 與 Hook 怎麼分工

team-dev-kit 並不是只靠 Skill 來實作所有規則，而是有一部分放在 Hook。這樣切分是有原因的。

Skill 是自然語言的流程指示。Claude Code 會讀它，然後提出「這樣寫如何？」並執行。它適合會隨脈絡改變、需要判斷的事情。例如 commit message 裡的 `scope` 要寫什麼，會依修改內容而不同。如果把這件事用機器固定成單一格式，反而會產生不自然的訊息。所以這類規則適合放在 Skill，讓 AI 理解脈絡後靈活決定。不過，Skill 是否被遵守仍然是機率性的，AI 不一定每次都會完全照到指示。

Hook 則不同。它是在 commit 或發行那一瞬間執行的決定性程式，只要條件符合就會毫不含糊地擋下來。像機密資訊外洩這種「只要放過一次就無法挽回」的底線，就適合放在這裡。如果只是用 Skill 寫「提交時要小心」，AI 一旦漏看就可能外洩。而且人直接 `git commit` 的流程裡，根本沒有 AI 介入。所以機密資訊檢查不是放在 Skill，而是放在 Hook，同時用機器封住人類的 commit 路徑（pre-commit）與 Claude 的外部發行（PreToolUse）。

一句話來說：引導你走向正確做法的是 Skill，絕對不能放行的是 Hook。能接受機率性生效的放 Skill，必須確定攔下來的放 Hook。

## 實例：只要告訴目的，就能跑完整個開發流程

與其一直講理論，不如直接看實際怎麼運作更快。以下是在已導入 team-dev-kit 的測試用 repository [`sandbox-team-dev-kit`](https://github.com/aRaikoFunakami/sandbox-team-dev-kit) 中，實際對 Claude Code 發出請求時的紀錄。branch 運作、commit 規範、Issue 與 PR 的關聯，我這邊完全沒有指示。傳達的只有「想做什麼」。

我給的 prompt 是這樣：

> 我想做一個用 Rust 把 "Hello, World!" 印到標準輸出的簡單程式。請從建立 Issue、實作、建立 PR 到合併全部幫我處理。細節做法（branch 運作、commit 規範、Issue 與 PR 的關聯）請依照這個 repository 的規則。

只丟這一句話，Claude Code 就自動幫我做了下面這些事。

| 階段 | 自動完成的內容 | 實際成果 |
| --- | --- | --- |
| Issue 建立 | 依 feature 模板建立 | [#5](https://github.com/aRaikoFunakami/sandbox-team-dev-kit/issues/5) |
| branch | 建立 `feature/5-rust-hello-world` （沒有直接 commit 到 `main` ） | — |
| 實作 | 產生 `Cargo.toml` / `src/main.rs` / `.gitignore` ，並用 `cargo run` 驗證 | — |
| commit | `feat(hello-rs): Rust で "Hello, World!" を標準出力するサンプルを追加` | — |
| PR 發行 | 內文加入 `Closes #5` ，讓 Issue 與 PR 雙向連結 | [#6](https://github.com/aRaikoFunakami/sandbox-team-dev-kit/pull/6) |
| 合併 | squash merge → Issue #5 自動關閉，branch 也自動刪除 | — |

`type(scope): subject` 這種 commit 格式、切 feature branch 的做法、透過 `Closes #N` 將 Issue 與 PR 雙向連結、以及 merge 時自動關閉 Issue，全部都是 Skill 在背後自動套用的結果。原本如果要靠人記住，就得每次手動確認的步驟，在你說出目的的那一刻就已經確定好了。

順便也測了機密資訊的防護。我故意嘗試 commit 一段看起來很像真實 AWS key 的隨機字串，結果 pre-commit hook 把它擋下來。實際輸出如下：

```
WRN leaks found: 1

✋ 已偵測到疑似機密資訊或個人資訊，已停止 commit。
   處理方式：請刪除、遮蔽或匿名化相關內容後再重新 commit。
   若為誤判：請在 .gitleaks.toml 的 [allowlist] 中加入示例值。
```

這裡要特別提醒一件事：像 `AKIAIOSFODNN7EXAMPLE` 這種文件裡常見的示例 key，我們刻意讓它通過。因為如果誤判一直擋下來，反而會造成干擾。也就是說，「沒有被阻擋」不代表壞掉了。要測試真的有沒有在運作，請用更接近真實的隨機值。

## 快速開始

導入的成本，會依照你的角色差很多。若你只是加入一個已經安裝好 team-dev-kit 的 repository，幾乎不用做任何事。

```
git clone <repository URL>
cd <repository>
```

接著啟動 Claude Code，並允許 Trust（信任），就完成了。到了這一步，團隊規則已經開始生效。如果你不確定有沒有真的啟用，可以用 `git config --local core.hooksPath` （如果顯示 `.githooks` 就代表 pre-commit 已啟用）以及 `ls .claude/skills` （如果看到 `git-commit` 等 skill 就代表已載入）來確認。

如果是第一次把它導入自己的團隊 repository，只需要做一次性作業。這件事只要團隊裡有一個人做過，之後其他人只要執行前面的 clone 就可以了。前提是已安裝 `git` / `python3` / `gitleaks` ，而且可以使用 Claude Code。導入方式就是在 repository root **只執行一次 bootstrap** 。

```
cd <repository>
curl -fsSL https://raw.githubusercontent.com/aRaikoFunakami/team-dev-kit/main/bootstrap.sh | sh
```

這樣所有必要的檔案就會自動配置到 **這個專案底下** （不會放到 `$HOME` ，也不會影響其他專案）。

```
.claude/skills/       ← 業務用 skill（只在這個專案啟用）
.claude/settings.json ← PreToolUse 機密資訊 hook
.team-dev-kit/        ← 規則本體 + egress script（禁止編輯）
.githooks/            ← commit 時的自動檢查
AGENTS.md             ← 撰寫專案專屬規則的地方（可編輯）
.gitleaks.toml        ← 機密資訊偵測規則（可編輯）
.github/              ← Issue / PR 模板（可編輯）
```

如果你已經有自己的 `AGENTS.md` 或 `.gitleaks.toml` ，原本的內容會被保留下來。系統只會幫你補上共通契約的引用 `@.team-dev-kit/contract.md` （ [AGENTS.md](http://agents.md/) ）以及繼承基礎偵測規則的 `[extend]` （.gitleaks.toml）這種 **連接設定，而且是冪等地追加** 的（重跑也不會重複）。如果你要套用到所有專案，才使用 `--global` 。

配置好之後，做一下動作確認。如果出現下面的結果，就代表導入完成：

```
$ git config --local core.hooksPath
.githooks

$ ls .claude/skills
doc-writing  git-commit  github-workflow  ticket-draft  ticket-publish  ...
```

最後，故意 commit 一段更接近真實的隨機機密資訊，若 pre-commit 真的會擋下來，就代表防護機制也正常運作（像 `AKIAIOSFODNN7EXAMPLE` 這種示例 key 會因為避免誤判而放行）。配置好的檔案需要分享給團隊；而那次 commit 與 PR，正好就是接下來要介紹的「只要傳達目的」流程的最佳第一個練習。

## 實作細節

到這裡，應該已經清楚「能做什麼」了。至於為什麼會設計成這樣、配發的東西如何分層、為什麼機密資訊檢查要放兩個地方，這些設計細節都寫在 [`docs/architecture.md`](https://github.com/aRaikoFunakami/team-dev-kit/blob/main/docs/architecture.md) 裡。

先摘一句最核心的：這個 kit 發放的內容分成兩層——「給 Claude Code 用的工具（A 層）」與「放在 repository 裡，讓人的 commit 和 GitHub 生效的檔案（B 層）」。A 層不想留在歷史裡；B 層則必須留在每個 clone 下來的 repository 裡才有意義。由於需求完全相反，所以從一開始就分開發放，而不是混在一起。這個判斷的經過與取捨，都可以在 [architecture.md](http://architecture.md/) 裡看到。

## 客製化範例：讓誤判安靜下來、加上自訂規則

把機密資訊掃描帶進實際運作後，最先遇到的通常就是誤判。team-dev-kit 讓各專案能自行調整這件事。做法如下：共通的偵測規則放在 `.team-dev-kit/base.gitleaks.toml` ，這個檔案禁止編輯（更新時會被替換）。各專案再建立一份繼承它的 `.gitleaks.toml` ，只補上自己專案需要的內容。也就是說，不碰 base，只在上面薄薄疊一層。

這些設定到底有沒有真的生效，我實際用 `gitleaks` 8.30.1 測過。題材是 base 規則會把私人 IP 位址視為個人資訊並偵測出來這件事。

### 允許本來正確卻被擋下來的情況

假設你想在公司內部工具的 README 裡寫一個參照用的 NTP server 固定 IP `192.168.10.50` 。這明明不是什麼機密，但 base 規則會把它當成私人 IP 抓出來。若直接用 starter 狀態（沒有 allowlist）去掃描，結果會像這樣：

```
$ gitleaks detect --no-git --source README.md -c .gitleaks.toml --redact
INF scanned ~80 bytes (80 bytes) in 22.5ms
WRN leaks found: 1
```

這時候 commit 會被擋下來。於是就把這個專案專屬的允許規則加到 `.gitleaks.toml` 裡。

```
title = "secret & PII scan (project overlay)"

[extend]
path = ".team-dev-kit/base.gitleaks.toml"

[allowlist]
description = "公司內部文件中記載的固定內部主機"
regexTarget = "match"
regexes = [
  '''\b192\.168\.10\.50\b''',
]
```

再掃描一次同一個檔案，就會通過。

```
$ gitleaks detect --no-git --source README.md -c .gitleaks.toml --redact
INF scanned ~80 bytes (80 bytes) in 17.8ms
INF no leaks found
```

`leaks found: 1` 變成了 `no leaks found` （exit code 0）。base 規則維持不變，只精準放行了這個 IP。要注意的是，像 `192.168.10.50` 這樣應該盡可能寫得很具體。如果像 `192.168.` 這樣放得太寬，真正想隱藏的內部位址也會一起被放行。每次消除誤判時，都要盡量把洞縮到最小，這就是重點。

### 只檢出這個專案才有的機密資訊

也可以做反向客製化。假設要把自家特有的 token 格式加進偵測條件中。比如想抓出以 `ACME-` 開頭、後面接 12 碼的內部服務 token，就在 overlay 裡新增一條規則。

```
title = "secret & PII scan (project overlay)"

[extend]
path = ".team-dev-kit/base.gitleaks.toml"

[[rules]]
id = "acme-internal-token"
description = "ACME 公司內部服務 token"
regex = '''ACME-[A-Z0-9]{12}'''
```

如果掃描含有 `service_token = ACME-7H3K9Q2M5P8W` 的檔案，就會正確被抓到。

```
$ gitleaks detect --no-git --source config.txt -c .gitleaks.toml --redact
INF scanned ~34 bytes (34 bytes) in 19.7ms
WRN leaks found: 1
```

base 的標準規則（API 金鑰、私密金鑰等）會照常存在，上面再疊上專案自己的規則。這樣就能在所有 repository 中維持一致的共通基礎，同時讓各專案加入自己的需求。因為有這種分離，所以即使更新共通規則（重新執行 bootstrap 並加上 `--force` ），專案自己的調整也不會消失。

```
cd <repository>
curl -fsSL https://raw.githubusercontent.com/aRaikoFunakami/team-dev-kit/main/bootstrap.sh | sh -s -- --force
```

`--force` 會用最新版替換 skill / framework / egress，但像 `AGENTS.md` 、`.gitleaks.toml` 這些屬於專案擁有的設定會保留原本內容。

如果想補上專案專屬的規範本身（例如「這個 repository 一定要更新 API 文件」），就把內容寫到 `AGENTS.md` 的「專案專屬規範」段落中。這個區域 team-dev-kit 不會碰，所以更新時不會被覆蓋。若想修改共通規則，就不要直接在本地改，而是回到本體 repository 提 Issue / PR 提案。等合併後，各專案只要重新執行 bootstrap，就會套用到所有 repository。

## 讓規則成長：在最少之後加什麼

team-dev-kit 透過 Skill / Hook 實作的，是團隊開發中「最少但必要」的規則。

在實際案件裡，通常會以這個基礎為核心，依照各專案的情況再往上加規則。那麼，該從哪裡開始加？加的時候要放進 Skill 還是 Hook？

### 新增規則時的判斷，和前面完全一樣

新增規則時，判斷標準也不會變。 **會隨脈絡改變、需要判斷的，放 Skill；絕對不能放行、必須確定擋下來的，放 Hook** 。新增規則，其實就是再做一次這個切分。

### 先加什麼，效果最容易看出來

以一般軟體開發來說，最值得先加的規則可以這樣排：

| 順序 | 要加的規則 | Skill / Hook | 例子 |
| --- | --- | --- | --- |
| 1 | **lint / format** | Skill（引導）＋ Hook（強制） | commit 前執行 `eslint` / `ruff` / `gofmt` 。能自動修正的內容，就用 Hook 確定性強制 |
| 2 | **測試通過** | Hook（偏向 pre-push）＋ Skill | push 前執行 `cargo test` 等。因為比較重，所以比起 pre-commit，更適合放在 pre-push 或 CI |
| 3 | **程式風格／命名規範** | Skill | 像「公開函式要寫 docstring」這種需要判斷的規則，適合放 Skill |
| 4 | **文件更新義務** | Skill | 例如「API 有變更時也要更新 docs」 |
| 5 | **依賴與授權審查** | Hook | 用機器檢查是否混入禁止授權，型態和機密掃描相同 |
| 6 | **必須 review、需要幾個批准** | kit 之外（GitHub branch protection） | 這不是 Skill / Hook 的範圍，而是靠 GitHub 設定來限制 |

如果要先加， **lint** 通常是最標準的起點。因為效果很明顯，而且多數情況都能自動修正，所以新增規則時帶來的摩擦最小。

### 規則的追加方式

- **新增 Skill** ：只要用自然語言寫一頁規則即可，不需要執行程式碼。格式和現有的 `git-commit` skill 相同。如果是專案專屬的規則，只要寫在 `AGENTS.md` 的「專案專屬規範」中，Claude Code 就會讀並套用。
- **新增 Hook** ：像 commit 或 push 當下要由機器確實攔下來的東西，就放進 Hook。不過 team-dev-kit 發放的 `.githooks/pre-commit` 是共通 framework（禁止編輯、更新時會替換），所以基本上不要直接去改它，而是 **由專案自己帶 lint 或 test 的 hook** 。team-dev-kit 只提供基礎，實際的程式風格規則則由各專案自行客製化。

### 具體例：在 commit 前通過 lint（Skill ＋ Hook 兩段式）

以最可能先加的 lint 為例，示範同時使用 Skill 與 Hook 的做法。這和機密資訊防護一樣，也是兩段式。

**Skill 端（引導）** —— 只要在 `AGENTS.md` 補上一句即可。

```
## 專案專屬規範
- 修改程式碼後，在 commit 前先跑 lint。若失敗就先修正再 commit。
```

**Hook 端（確定性阻擋）** —— 在專案的 pre-commit 中加入 lint 步驟。只要還有錯誤，就停止 commit。

```
#!/bin/sh
# 專案獨有的 lint gate（例如：JS/TS 就用 eslint）
if ! npx eslint --max-warnings=0 $(git diff --cached --name-only --diff-filter=ACM | grep -E '\.[jt]sx?$'); then
  echo "✋ 還有 lint 錯誤，請修正後再 commit。"
  exit 1
fi
```

為什麼兩邊都要做？因為 Skill 會引導你「去做 lint」，但還是有可能漏掉（機率性的）。Hook 則會對人類直接執行 `git commit` 的路徑做確定性阻擋。這和用 Skill 與 Hook 雙重保護機密資訊的邏輯完全相同。Skill 負責引導，最後由 Hook 來擋下來。

### 不要把 Hook 加過頭

最後提醒一點。Hook 加太多，commit 速度會變慢、提示會變吵，最後大家就會用 `--no-verify` 把它關掉，結果沒人真的使用。 **只有能自動修正、而且是確定性的規則才放 Hook；需要判斷的放 Skill** —— 這才是安全的界線。盡量降低誤判、不要增加摩擦，這個態度其實和把秘密掃描的 allowlist 寫得很精準是同一件事。team-dev-kit 只保留最低限度規則，也是基於同樣的理由。

## 結語

team-dev-kit 做的，不是刪掉規則，也不是增加規則。它只是把「誰在承擔遵守規則的成本」從人轉移給 AI 與機器。以前是新人背規則、reviewer 每次指正、所有人靠注意力在付出的成本，現在由 Claude Code 與 pre-commit hook 分擔了。

- 會因脈絡而改變正解的東西（commit 規範、PR 流程、文件文體）由 **Skill** 引導
- 不能跨越的底線（機密資訊、個人資訊混入）由 **Hook** 確定性擋下來
- 新人只要說明「想做什麼」就夠了。branch 運作、 `Closes #N` 的關聯、規範格式都會自動生效

反過來說，只有前提吻合的團隊才會真的受益。若你們團隊已經在用 Claude Code、規範也有明文化、而且是多個人、多個 repository 在共同維運同一套規則——如果你們現在每次都在重複 onboarding 說明與規約違反指正，那效果會非常明顯。若只是單人玩的興趣專案，坦白說幫助不大。

試用其實不難。先在一個 repository 的根目錄執行 bootstrap，然後把那個「已導入 team-dev-kit 的變更」本身交給 Claude Code，從 Issue → PR → merge 走完一次。你會在第一個 commit 就實際感受到這套機制是有效的。

- repository: [https://github.com/aRaikoFunakami/team-dev-kit](https://github.com/aRaikoFunakami/team-dev-kit)
- 測試用沙盒: [https://github.com/aRaikoFunakami/sandbox-team-dev-kit](https://github.com/aRaikoFunakami/sandbox-team-dev-kit)

---

原文出處： [https://qiita.com/rairaii/items/a1b622ff1faf4ca34231](https://qiita.com/rairaii/items/a1b622ff1faf4ca34231)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/a6GDnq) [38瀏覽](https://codelove.tw/@tony/post/a6GDnq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/a6GDnq)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬1 ❤️1

507

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

30

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782108389&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa6GDnq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782108389180&amp;bpp=1&amp;bdt=103&amp;idt=58&amp;shv=r20260617&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782108370%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4032499931034&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099218&amp;oid=2&amp;pvsid=986492694282107&amp;tmod=1256732551&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=80" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260617/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>