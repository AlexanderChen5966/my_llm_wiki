---
title: "使用 Claude Code 的免費安全稽核外掛，自動偵測並修復漏洞"
source: "https://codelove.tw/@tony/post/3ojzmx"
author:
published:
created: 2026-07-13
description: "[](#%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88)本文重點-----------------------------------------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月09日](https://codelove.tw/@tony/post/3ojzmx "2026-07-09 17:30:24")

## 使用 Claude Code 的免費安全稽核外掛，自動偵測並修復漏洞

## 本文重點

- Anthropic 於 2026 年 5 月下旬公開了 Claude Code 專用的官方外掛程式 `security-guidance` （各媒體報導時間為 5 月 27 日）。所有方案皆可使用，截至 2026 年 6 月並未標示為 beta。不過，模型進行審查仍會消耗一般 Claude 使用額度
- 特色是在「檔案編輯時／一個回合結束時／提交或 push 時」這 3 層偵測漏洞，並直接讓 AI 依偵測結果修正
- 依據 Anthropic 內部的基準測試（由官方發表、媒體引用，未收錄於官方文件正文），使用此外掛程式後，開啟的 Pull Request 所附的資安相關留言據稱減少了 30～40%，因此很適合用在審查前的早期階段先把問題消掉
- 不過，「AI 寫程式 → AI 修正」的迴圈本身仍然需要被審計，因此實務上較合理的做法，應是搭配 Snyk / CodeQL / SAST，並依照 Anthropic 整理的「封鎖設計（containment）」概念來運作

本文於 2026 年 5 月底發布，並於 2026 年 6 月 30 日在官方文件（ `code.claude.com` ）重新確認各項記述後更新。外掛程式名稱、安裝指令、前提版本與 3 層架構在重新確認時都沒有變更。

科技新聞

## 前言

把這個外掛程式的使用方式只說成「執行 `/plugin install` 就結束」的話，我覺得大概抓不到重點。因為手續本身只要幾行，本篇反而會把篇幅放在「為什麼這類外掛程式在現在這個時間點被需要」「AI 寫出的程式碼漏洞檢測，和傳統資安檢查到底差在哪裡」這些背景上。

當包括 Claude Code 在內的代理式程式開發支援滲入日常開發後，實務上「由人類逐行撰寫、由人類審查」這個前提就開始鬆動。AI 一次生成數百行，另一個 AI 幫忙修，接著又在別的 session 加功能。這時自然會浮現一個疑問：那麼，最後到底是誰（或什麼）來審查這些大量程式碼？我認為 `security-guidance` 可以視為 Anthropic 對這個結構性問題提出的一種回答。

本文會先整理這個外掛程式的定位與用法，再往前追溯脅威模型的變化、3 層分析對應到哪些攻擊面、讓 AI 自己修復本身的風險，以及和既有工具之間的分工，盡量從「設計理念的差異」來整理。希望對關心 AI 與 DevSecOps 交會領域的人有幫助。

## 1\. 為什麼現在需要這個外掛程式

### 1-1. 「誰來審查大量 AI 程式碼」這個結構問題

在傳統軟體開發裡，程式撰寫速度基本上受限於人手。人一天能寫的程式碼量有上限，而審查者大致也能跟上這個速度。就算審查跟不上，也通常只是「人手不足」的數量問題，並不是整個結構失衡。

但代理式程式開發普及後，情況就變了。生成速度開始大幅超過人類審查速度時，「撰寫」與「確認」之間會出現長期性的落差。而且這個落差不是多加幾個人就能輕鬆補上，因為生成端也能一起擴張。這就是我認為「誰來審查大量 AI 寫出的程式碼」這個問題，不是數量問題，而是結構問題的原因。

對這個問題的回應方式有很多，例如「放慢生成速度以配合審查」、「把審查也自動化以追上生成」、「乾脆在生成階段就不要產生危險寫法」等等。 `security-guidance` 看起來主要偏向後兩者，也就是把檢查放到生成附近，直接在當下消滅危險寫法。我的理解是，它想透過縮短生成與檢查之間的距離，在落差擴大前先把它補上。

### 1-2. 官方推出的意義

第三方資安工具其實早就很多了，但由 Anthropic 自己推出外掛程式，還是有幾個意義。我認為其中之一是，最接近 Claude 生成瞬間上下文的位置——例如前一刻的編輯差異、session 內的意圖、工具輸出等——是整合到 Claude Code 本體的機制。外部工具只能在生成完成後回頭看輸出的程式碼；整合式機制則能直接使用「正在寫」時的語境。

電腦安全性

另一個意義是，AI 寫出來的程式碼有哪些特殊失敗模式，最清楚的往往就是訓練與營運這個模型的供應商。哪些輸入容易導致哪些錯誤，會在模型訓練與實際運作的過程中累積出知識。把這些知識反映到檢查端，是同一供應方才做得到的優勢。不過，這不代表「官方出品就一定安全」，而是應該從兩面看：官方確實更能掌握某些失敗模式，但同時也必然有看漏的範圍。

## 2\. 為什麼 AI 生成程式碼的脅威模型和以往不同

這一節是本文最想多談的部分。我認為 AI 寫出的程式碼風險，不只是「量很多」，而是「危險的性質」和過去不一樣。

### 2-1. 會混入人類不常寫的模式

人類審查仰賴的是長年累積的「人類常犯錯模式」：off-by-one、漏掉 null 檢查、忘記加鎖、典型 SQL 組字錯誤等等。但 AI 會依照和人類不同的統計傾向來產生程式。它會受到訓練資料分布影響，混入一些在語境上看起來合理、但局部卻不一致的寫法。

問題在於，對這類「人類通常不會這樣寫」的錯誤，人類審查者的直覺警報往往不容易響。看起來很自然、變數命名也合理、註解還很完整時，審查者很容易直接把它當作「已經思考過的程式」而略過。也就是說，流暢反而會降低警覺。

### 2-2. 很像樣但其實錯誤的依賴新增

AI 有時會為了實現功能而提議新增套件。大多數情況都合理，但偶爾會自然地推薦「權限要求過高的函式庫」、「已經停止維護的舊套件」，或是「名稱很像但用途不同的套件」。

新增依賴，往往比改程式本體更容易擴大攻擊面。多加一個依賴，就等於把它連帶帶來的遞移依賴也一起納入專案。AI 的提案文字通常很有說服力，所以一旦審查者因為「看起來很合理」而接受，攻擊面就會悄悄擴大。

### 2-3. 因幻覺產生不存在的套件（slopsquatting）

還有一個更具 AI 特性的風險，是 AI 會很有自信地生成根本不存在的套件名稱。這本身只是錯誤，但麻煩的是它可能成為攻擊入口。如果攻擊者搶先把 AI 常幻覺出的名稱註冊成真的套件，開發者若相信並安裝，就可能把惡意程式碼帶進來。

程式設計

這種手法有時被稱為「slopsquatting」。相較於傳統的 typo squatting（利用打錯字），slopsquatting 是建立在 AI 的幻覺之上，算是 AI 時代才會出現的特殊風險。

關於包含 slopsquatting 在內的供應鏈攻擊手法與對策，會因情境而差很多。這裡只是概念整理；若要真的做防護，還是建議查閱你所使用的套件註冊表與資安廠商的最新官方資訊，會比較安全。

### 2-4. 總結來說，變了什麼？

如果說傳統脅威模型主要假設的是「人類犯錯」或「外部攻擊者發動攻擊」，那麼 AI 時代則多了一條新的軸線：那些用「看起來很合理」來降低警戒的生成物。因為流暢、而且有說服力，所以更容易被直接放過。既然如此，檢查端也必須往「不只是明顯可疑的程式碼，而是自然到讓人忽略風險的程式碼」這個方向去抓。

## 3\. 3 層分析與攻擊面對應

`security-guidance` 的特色，在於它不是只審一次，而是在不同開發時機點插入 3 個階段。綜合文章與 Anthropic 的說明，大致可以整理成下面這個結構。

層級發動時機主要處理成本概念1. 模式比對檔案編輯時（ `Edit` / `Write` / `NotebookEdit` 後不久）以決定論、快速的方式偵測危險函式呼叫與已知惡意模式，不呼叫模型輕量、不消耗 token 2. 回合結束審查當一個回合的變更整合完成後在背景用模型審查 git diff，重新檢查漏掉的地方並回饋給 Claude中等3. 提交時審查 `git commit` / `git push` 前後讀入周邊程式碼與呼叫來源做較完整的代理式審查較重官方文件提到，各層在實務上都有上限與前提。第 2 層每次最多 30 個檔案、連續 3 次；第 3 層大約每小時 20 次，以避免成本失控。第 2、3 層只會在 git repository 內運作，離開 repository 就會跳過。

另外有一點要特別注意：提交時審查（第 3 層）只會在 Claude 透過 Bash 工具執行 `git commit` / `git push` 時才會觸發。如果是透過 `!` 的 shell escape 直接提交，或使用者在自己的終端機手動提交，就不會進入審查範圍。若誤以為「只要 commit 就一定會有最後檢查」，很容易漏掉；在實際導入時這點一定要記住。

這 3 層其實各自對應不同攻擊面。以下就用前面提到的「依賴、程式碼、設定」三個角度來整理。

### 3-1. 程式碼本身的攻擊面 — 主要由第 1、2 層處理

像是程式碼注入、不安全的反序列化、不安全的 DOM 操作、認證繞過等，這些都是直接出現在原始碼中的漏洞。由於這類問題的「危險寫法樣式」相對固定，因此在檔案編輯時的模式比對（第 1 層）先攔第一波，再由回合結束審查（第 2 層）依文脈補抓漏網之魚，會是比較有效的組合。

程式設計

也就是說，它會在你剛寫完的瞬間就先回饋，讓 2-1 提到的「因為看起來太流暢而被直接略過」這種情況，在人類閱讀前先被機器攔下來一次。

### 3-2. 依賴的攻擊面 — 第 2、3 層需要看上下文

像 2-2、2-3 提到的過度新增依賴或混入不存在套件，光看單行模式往往很難判斷。因為這要看「這個功能是否需要這個依賴」「這個套件是否真的存在、是否還在維護」等問題，必須把整體變更和專案上下文一起看。這種情況，會更適合看完整變更歷程的回合結束審查（第 2 層），以及會讀周邊程式碼的提交時審查（第 3 層）。

不過，依賴的漏洞管理本身如後面所說，還是有很大一部分屬於專用工具（例如 Snyk）的範圍，不是這個外掛程式單獨就能完全包辦。

### 3-3. 設定的攻擊面 — 第 3 層比較能一起看周邊

權限設定、秘密資訊處理、CORS、認證流程等「設定層」問題，單看一個檔案往往不容易察覺，通常得跨檔案讀上下文才看得出危險。會去讀周邊程式碼的提交時審查（第 3 層），相對比較有機會碰到這類問題。

所以，這 3 層不只是「保險起見多檢查幾次」而已；若把它看成從能用簡單模式抓到的攻擊面，一路逐步擴張到需要上下文才能判斷的攻擊面，就會比較合理。文章中列出的偵測對象包含程式碼注入、危險的反序列化、DOM 操作問題、認證繞過等代表性案例，但完整涵蓋範圍還是要以官方文件為準。導入後，最好在自己的技術棧中確認它到底能抓到多少。

## 4\. 為什麼只靠 SAST / DAST 在 AI 時代不夠

這裡先回到傳統檢查方式。SAST（靜態分析）與 DAST（動態分析）長年都是資安檢查主軸。我不認為 AI 時代會讓它們失去價值，但確實存在它們比較難追上的結構性問題。

第一個是速度不匹配。SAST/DAST 通常是在 CI 或定期掃描時執行，回饋往往是程式寫完一段時間之後才回來。當生成速度變快時，檢查結果回來的時候，可能下一波生成又疊上去了，整個回饋迴圈會持續落後。等到寫的人（或 AI）對當時的上下文已經不新鮮時，修改成本通常也更高。

第二個是上下文不足。CI 階段的工具可以看到最後輸出的程式碼，但看不到「為什麼會這樣寫」「為什麼要加這個依賴」這種生成時的語境。若 AI 生成程式的風險在於「看起來很合理」，那麼缺少上下文，只看表面形狀時，就比較難抓到這種自然到有風險的寫法。

程式設計

再來是誤報與漏報的結構問題。規則式 SAST 的誤報太多，維運團隊就會疲乏，最後把警告當空氣；反過來，調得太嚴又容易漏掉真正的問題。這種調校困難本來就存在，但生成量一高，警報總數也會暴增，導致「警報疲勞」更容易發生。

從這些點看來，像 `security-guidance` 這種「在生成附近、保留上下文、即時回饋」的設計，的確是在從另一個角度補足 SAST/DAST 的弱點。不過這不是取代，而是補強。後段的 SAST/DAST 仍然有它的價值，因為它們可以對整個程式庫做高再現性、決定論式的掃描。

## 5\. 導入步驟與檢出範例（假想案例）

前面講了很多背景，這裡就簡單整理實際導入方式。安裝是透過 Claude Code 的 plugin 管理器完成。官方文件寫明，前提是 Claude Code CLI v2.1.144 以上，以及 Python 3.8 以上。（截至 2026 年 6 月，CLI 最新版是 v2.1.195，但最低要求版本仍然是 v2.1.144。）

```
# 在 Claude Code 的命令列中
/plugin install security-guidance@claude-plugins-official
```

如果系統提示找不到 marketplace，可以先註冊 marketplace 再重新安裝。

```
# 先註冊 marketplace，再重新安裝
/plugin marketplace add anthropics/claude-plugins-official
/plugin install security-guidance@claude-plugins-official
/reload-plugins
```

安裝後，hook 會自動啟用，並在檔案編輯、回合結束、提交或 push 時於背景執行。偵測到的問題會直接通知到一般 session 畫面，不需要切換到特別介面。用來做審查的模型預設是 Claude Opus 4.7，也可以透過環境變數改成其他模型（模型審查會消耗一般 Claude 使用額度）。

如果希望整個團隊或雲端 session 都固定啟用，也可以在專案的 `.claude/settings.json` 內設定啟用。

.claude/settings.json

```
{
  "enabledPlugins": {
    "security-guidance@claude-plugins-official": true
  }
}
```

本文內容是以 2026 年 6 月時點的公開資訊（ `code.claude.com` 官方文件）重新確認後撰寫的。由於外掛程式規格與檢測精度未來仍可能改變，實際導入前務必再確認官方文件最新版。

科技新聞

以下開始以讓 AI 撰寫的範例程式為題材，分享一些假想案例的運作感覺。請把它理解成把常見寫法重現出來的示意，而不是實際專案名稱或正式環境設定。

### 5-1. 包含命令注入的 Python 程式

例如，假設 AI 寫出下面這種把使用者輸入直接丟給 shell 的程式。

```
import subprocess

def run_backup(filename: str) -> str:
    # 直接把使用者輸入串進去的例子
    cmd = f"tar czf /tmp/backup.tgz {filename}"
    return subprocess.check_output(cmd, shell=True, text=True)
```

在安裝 `security-guidance` 的情況下，檔案編輯時的模式比對層（第 1 層）會抓到 `shell=True` 搭配外部輸入串接的組合。接著在回合結束時（第 2 層），Claude 會主動提出「這個地方改成用引數分開傳入比較安全，要產生修正建議嗎？」之類的回饋。這可以視為 3-1 所提到的「程式碼本身的攻擊面」在兩層防線下被攔截的典型例子。

修正後的程式大致會變成這樣：

```
import subprocess
from pathlib import Path

def run_backup(filename: str) -> str:
    target = Path(filename).resolve()
    # 不經由 shell，而是用引數陣列傳遞
    return subprocess.check_output(
        ["tar", "czf", "/tmp/backup.tgz", str(target)],
        text=True,
    )
```

### 5-2. 危險的反序列化

另一個常見案例，是把外部傳進來的位元組串直接拿去 `pickle.loads` 還原。

```
import pickle

def load_state(payload: bytes):
    return pickle.loads(payload)  # 很容易直接導致任意程式碼執行
```

提交時的完整審查層（第 3 層）會在讀入周邊程式碼後，找出這種「和可能導致任意程式碼執行的函式組合在一起」的情況。修正方向通常會建議改成 JSON 或其他較安全的表示法，或是改用帶驗證的反序列化方式。

程式設計

```
import json

def load_state(payload: bytes) -> dict:
    # 改成不涉及任意程式碼執行的表示法
    return json.loads(payload.decode("utf-8"))
```

如果要處理無法用 JSON 表示的結構，通常就應該重新檢討「是否真的要對不可信輸入做反序列化」，或至少加上 schema 驗證。這會比較穩妥。

這裡的程式碼範例只是用來理解運作方式而已，實際偵測訊息與修正提案的文字，會因版本與情境而不同。正式上線之前，最好先在自己的 repository 用小型 PoC 試跑，確認實際行為會比較安全。

## 6\. 讓 AI 自己修正的風險與「封鎖設計」

雖然這是個方便的外掛程式，但「AI 寫程式、AI 再修正」這個迴圈本身，有幾個結構上的注意點。這裡也和第 1 章提到的「到底誰來審查」剛好互為表裡。若把檢查與修正都交給 AI，整個迴圈就會封閉起來，讓人類更不容易介入。

具體來說，我會在意這些點：

- 偵測邏輯本身含有機率性，不保證完全沒有漏網之魚
- 修正提案有時只補表面，真正的設計問題仍需另外處理
- 一個修正可能壞掉別的地方，接著又由 AI 去修，整體變更的脈絡就容易變得混亂
- 為了修正而擴大權限或新增依賴，反而可能把攻擊面弄大
- 「AI 自己抓、AI 自己修」本質上也碰到自我驗證的極限，因為自己犯的錯往往自己不容易察覺
- 「既然修好了就安全」這種心理，可能讓人類審查被輕忽

### 6-1. 為什麼會出現「封鎖」這個發想

這種想法可以參考 Anthropic 公開的 [How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude) （關於封鎖設計的說明）。「containment」這個詞想表達的，不只是提升模型聰明度或檢出精準度來接近安全，而是預先建立一條邊界：就算模型出錯，損害也不會超出某個範圍。

如果只靠機率性的東西來控制機率性的行為，總會碰到極限。所以我認為順序應該反過來：先設好決定論上有效的邊界（也就是環境限制），再讓模型在這個範圍裡運作。該文也介紹了大致 3 層的安全性支撐方式：

電腦安全性

1. **環境層** — 透過沙箱、VM、輸出控制建立決定論邊界
2. **模型層** — 透過系統提示、分類器、訓練修正來調整行為
3. **外部內容層** — 處理 MCP 伺服器與工具輸出等信任邊界外的內容

重點是，不要只依賴會以機率方式行為的模型層，而是先在環境層設好「到這裡為止」的邊界。 `security-guidance` 在這個分類裡比較接近模型層，因此如果沒有另外做好沙箱與權限控制，一旦偵測漏掉，損害仍可能很大。反過來說，也不能因為裝了這個外掛程式，就鬆懈環境層的防護。

Claude Code 本身就有以作業系統層級沙箱運作的設計，例如 macOS 的 Seatbelt、Linux 的 bubblewrap，所以在本地端使用時，維持這條邊界不中斷，看起來是比較實際的做法。

## 7\. 從「設計理念」來看和既有工具如何分工

提到資安審查，很多人會想到 Snyk、CodeQL、Dependabot，或各種 SAST/DAST 工具。我認為 `security-guidance` 比較適合被視為補強，而不是取代它們。這裡不只是列比較表，而是從這些工具各自代表什麼樣的「理念」來看。

- **Dependabot 的理念** — 「依賴會過時，新漏洞會在之後才被發現」，因此它是沿著時間軸做防禦：持續比對已知漏洞資料庫，並更新依賴。它主要關注的是「這個依賴是什麼時候的版本」，而不是程式碼本身。
- **Snyk 的理念** — 把依賴函式庫與其遞移依賴都視為攻擊面，並以已知漏洞的方式系統化管理。我把它理解成，是用資料庫的廣度與可重現性，去回應 2-2 提到的「依賴會擴大攻擊面」問題。
- **CodeQL 的理念** — 把程式碼當作可查詢資料，沿著資料流追蹤「危險輸入是否會到達危險位置」，屬於語意層面的分析。這是針對 3-1 提到的程式碼攻擊面，以高可重現性的決定論方式深入分析。
- **`security-guidance` 的理念** — 前面那些工具多半是針對「寫完之後的程式碼」，而它則是針對「正在寫、且保有上下文的程式碼」。它重視的不是決定論式的大範圍掃描，而是在生成附近快速回饋。

也就是說，決定論且高再現性的掃描工具群（Snyk / CodeQL / Dependabot）與靠近生成、強調速度與上下文的外掛程式（ `security-guidance` ），其實是在不同軸線上發力。要注意的是，SAST 或 CodeQL 即使再可重現，也不能保證涵蓋所有漏洞。與其二選一，我覺得把不同軸線的工具疊在一起，會更合理。

觀點 `security-guidance` Snyk / CodeQL / Dependabot / 一般 SASTDAST運作時機程式撰寫中（Claude Code session 內）PR / CI、定期掃描階段式動態環境主要對象剛寫好的 AI 程式碼片段整個程式庫、依賴函式庫執行中的應用程式優勢即時回饋、帶上下文、可當場修正規則豐富、決定論、可重現以實際行為為基礎偵測弱點偵測範圍依賴 AI 編輯 session回饋迴圈較長環境建置成本較高建議的組合方式可以像下面這樣：

軟體

1. 本地端 / Claude Code session 內：用 `security-guidance` 邊寫邊修，保留上下文
2. PR / CI：用 CodeQL、Snyk、Dependabot 做涵蓋依賴與資料流的決定論式靜態分析
3. Stage 環境：用 DAST 與 fuzzing 驗證實際行為
4. Production：監控日誌、WAF、執行階段防護

這只是其中一種例子，實際順序仍要依語言、法遵要求而定。比較好的方式，應該是依照團隊狀況重新排列。

名稱很像、容易搞混的是，Anthropic 另外還有一個 `claude-code-security-review` 的獨立 repository（GitHub Action）。它是在 PR 開啟時於 CI 上執行安全審查，和本文講的 session 內外掛程式 `security-guidance` 不是同一個東西。外掛程式本體的原始碼則放在 `claude-plugins-official` repository 的 `plugins/security-guidance` 底下。前面「2. PR / CI」那一層，也可以把這個 GitHub Action 一併納入考慮。

## 總結

- `security-guidance` 可以視為對代理式程式開發普及後所出現的「大量 AI 程式碼誰來審查」這個結構問題，從生成端提出的一種回應
- AI 生成程式碼的威脅與以往不同之處，在於它會用「看起來很合理」來降低警戒；依賴新增與 slopsquatting 也屬於 AI 時代特有的風險
- 3 層分析不是單純多做幾次檢查，而是分別對應程式碼、依賴、設定等不同攻擊面，逐步擴大涵蓋範圍
- AI 偵測、AI 修正的迴圈有修正連鎖與自我驗證極限等風險；不能只靠機率式檢測，還是要透過環境層固化邊界，也就是「封鎖設計」
- 和 Snyk / CodeQL / Dependabot / DAST 相比，它們與 `security-guidance` 追求的軸線不同；現階段比較實際的做法是把它們疊加使用，而不是互相取代

「AI 與資安如何結合」這件事，目前仍然是各家公司持續嘗試的領域，最佳實踐尚未完全定型。本文也建議把它當成導入評估的起點，而不是最終答案；若要正式套用到生產環境，還是應該對照官方文件與公司內部的資安政策來決定。

## 參考連結

- [security-guidance 外掛程式官方文件 - Claude Code Docs](https://code.claude.com/docs/en/security-guidance) （規格的一手資訊。3 層的觸發條件、上限與前提版本以此為準）
- [外掛程式的尋找方式與安裝步驟 - Claude Code Docs](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code 官方文件（首頁）](https://code.claude.com/docs/en/overview) （舊的 `docs.anthropic.com` Claude Code 資訊已移到這裡）
- [claude-plugins-official - GitHub](https://github.com/anthropics/claude-plugins-official) （包含 `security-guidance` 的官方外掛程式 marketplace repository）
- [claude-code-security-review - GitHub](https://github.com/anthropics/claude-code-security-review) （在 PR 時進行審查的 GitHub Action，和 session 內外掛程式不同）
- [How we contain Claude - Anthropic Engineering](https://www.anthropic.com/engineering/how-we-contain-claude) （封鎖設計說明）
- [Anthropic releases Claude Code security guidance plugin - Help Net Security](https://www.helpnetsecurity.com/2026/05/27/anthropic-claude-code-security-guidance-plugin/) （30～40% 減少這個內部驗證數值的來源。官方文件正文未收錄）
- [Slopsquatting: AI 供應鏈攻擊研究筆記 - Cloud Security Alliance](https://labs.cloudsecurityalliance.org/research/csa-research-note-slopsquatting-ai-supply-chain-20260419-csa/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Anthropic News](https://www.anthropic.com/news)

---

原文出處： [https://qiita.com/nogataka/items/4d2a551f89f6b4f94b01](https://qiita.com/nogataka/items/4d2a551f89f6b4f94b01)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3ojzmx) [30瀏覽](https://codelove.tw/@tony/post/3ojzmx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3ojzmx)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783905001&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3ojzmx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783905001698&amp;bpp=1&amp;bdt=112&amp;idt=66&amp;shv=r20260709&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783904813%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783904813%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783904813%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5602917705568&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396104%2C95394425%2C95395869&amp;oid=2&amp;pvsid=2741007372050077&amp;tmod=865723311&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=79" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260709/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

開發工具