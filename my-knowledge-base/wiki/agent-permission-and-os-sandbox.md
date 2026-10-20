---
title: 手刻 agent 的兩層防線：指令分級詢問與 OS 沙箱
date: 2026-08-17
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10403020 ／ 10403136（《為你自己手刻 Claude Code》Day 14／15，作者高見龍，專案 KeSi）
type: article
status: processed
asset_value: 82
---

兩天合併：**Day 14（把 rm -rf 攔下來：deny/ask/allow 三級）＋ Day 15（把 agent 關在工作目錄：Seatbelt 沙箱）**。同系列 Day 05–10 見 [[hand-coding-agent-file-tools]]、Day 11 見 [[handcraft-agent-shell-tool]]、Day 12 見 [[agent-loop-stop-conditions]]、**Day 13 見 [[agent-bug-fix-eval-green-is-not-correct]]**——那次實戰 KeSi 改掉測試檔卻沒有任何人被問過一句話，這兩天就是把「否決權」拿出來用。

## T（核心論點）

> **字串檢查與 OS 強制是兩層不同的東西，而且第一層必然有洞。**

第一層（Day 14）：七個工具的風險完全不同，不能用同一套規矩——唯讀的放行、會改檔案的問一下、`run_command` 得看它這次要跑什麼。核心原則是 **看不懂就問，不要猜**。

第二層（Day 15）：因為字串檢查只看得到「模型填進參數的那個字串」，而**真正開檔案的不是我們**——`cat ../../.env` 攔得住，`python3 script.py` 攔不住（那個 script 可以 `open("../../.env")` 也可以連網把專案送走）。

> **要看得出來就得實際執行 Python，而會執行 Python 的東西不就是另一個直譯器嗎？何況真的跑完才知道答案，到時候不該發生的也都發生了。**

所以第二層交給作業系統。

## B（適用場景）

**適用：**
- 自建 agent 要加權限層與沙箱的完整檢查表（含大量踩坑實錄）
- 對照理解 Claude Code 的 permission 與 sandboxing 設計為何長那樣（原文逐條引官方文件對照）
- 判斷「我的權限系統是不是假的」——本篇有一個極佳的反例（見下方 `['python']` 事件）

**不適用 / 注意：**
- 🔴 **紅線清單擋不住惡意，只擋手滑**：`rm -rf` 可拆成 `rm -r -f`、繞去 `find . -delete`、或寫成 `python3 -c "import shutil; shutil.rmtree(...)"`——**變形永遠列不完**。Anthropic 的 bash 工具文件建議用**允許白名單而非禁止黑名單**
- 🔴 macOS 的 `sandbox-exec` 官方已標 **DEPRECATED**（建議改用 App Sandbox），但 App Sandbox 是「一支 app 宣告自己需要哪些權限、權限跟簽章一起發布」，**不適用於「臨時關住一條等一下才會冒出來、內容還不知道的指令」**。作者明說就這件事目前找不到官方公開替代品，且 Codex 與 Claude Code 的 macOS 沙箱**也仍以 Seatbelt 為底層**
- ⚠️ 實作為 macOS（Seatbelt）；Linux/Windows 需另尋機制

## R（第一層：deny → ask → allow 三級）

判斷順序是固定的：**先看有沒有踩紅線（deny），再把看不懂或需要人判斷的送去詢問（ask），最後才放行確定安全的唯讀指令（allow）**。

### 四個關鍵判斷

1. **複合指令要逐段檢查** — 用 `[;&|\n]+` 拆段後各自判斷。`ls; sudo rm -rf /` 的開頭是 `ls`，**只看第一個 token 就放行，後面半條就溜進去了**；換行也不能漏
2. **看不懂就問** — 只要出現 `$`、反引號、`(`、`>`、`*` 這些字元就一律回頭問人，因為實際做的事可能超出字面（`cat $(ls)` 的字面是 `cat`）
3. **白名單只放真正唯讀的指令**，且 `git` 要支援「指令＋子指令」寫法（`git status` 唯讀但 `git push` 不是）。⚠️ **即使子指令在白名單，參數也得再看一次**：`git diff --output=report.txt` 會寫檔，`--ext-diff` 與 `--textconv` 可能執行外部程式，一律降級 ask
4. **唯讀指令也不放過** — 白名單裡有 `cat` 就代表 `cat .env` 會自動放行，所以另加 `touches_forbidden()` 檢查禁區。⚠️ 但它回傳 `ask` 不是 `deny`：**你有正當理由要看 `.env` 時說一聲就好，但 agent 不能自己決定要看**

原文對照 Claude Code 官方文件的兩處，都很值得記：

> **Commands the analysis can't parse**: when Claude Code can't fully parse a command, it asks for approval… Commands longer than 10,000 characters always prompt.

🔑 **關鍵在 `can't fully parse` 的 fully——沒有完全解析出來就算數，而不是看懂大概就放行。**

而 `bypassPermissions` 模式下仍保留的例外：

> Removals targeting the filesystem root or home directory, such as `rm -rf /` and `rm -rf ~`, still prompt as a **circuit breaker against model error**.

⚠️ 原文注意到官方用的動詞是 **prompt 不是 block**——`rm -rf /` 在正版仍會問你一句，你回 `y` 它就會執行（保留人的最終決定權）。**KeSi 選擇硬一點：這幾條規則沒有 yes 的選項。**

### 問要問清楚（三個細節）

1. **理由要寫出來** — 「pytest 不在自動放行的唯讀清單裡」比「需要授權」有用得多，因為你當下要判斷的是「這次該不該放行」，不是「這系統為什麼這麼囉唆」
2. **`y` 跟 `a` 分開** — 只允許這一次 vs 這個 session 都別問，是兩件不同的事。🔑 **全部當成 `a` 會讓人不敢按 y，全部當成 `y` 又會被問到乾脆把功能關掉——一個煩人到讓人想繞過的安全機制，等於沒有**
3. **沒有人可以問的時候拒絕（fail closed）** — 接進自動化流程沒有終端機可互動時，正確的預設是不做，而不是「反正沒人反對就做吧」。`Ctrl-C`／`Ctrl-D` 也算拒絕

### 記住同意：記檔案不記工具

`GRANTED_WRITES` 記的是**檔案路徑不是工具名字**——`edit_file` 要改 `rules.py` 你回過一次 `a`，之後 `write_file` 寫同一個檔案也不再問；但**同意改 `rules.py` ≠ 同意改 `test_pricing.py`**。

🔑 **這個選擇直接來自 Day 13 那場實戰：如果權限是「同意 `edit_file` 一次就整個 session 放行」，那道關卡等於不存在。改哪個檔案才是重點，不是用哪個工具。**

## 🔴 R（本篇最重要的一課：`已放行的指令：['python']`）

第一次實測流程完全照設計走——三個 `read_file` 沒問、兩個危險動作各問一次。但最後一行印出：

```
已放行的指令：['python']
```

作者同意的明明是 `python -m pytest -v`（一句跑測試），系統記住的卻是「**python 這個指令都可以**」。實測驗證：

```
使用者同意 python -m pytest -v，記住的是： ['python']
  python -m pytest -q          → 自動放行
  python cleanup.py            → 自動放行     ← 🔴
  python -m http.server 8000   → 自動放行     ← 🔴
  pytest -q                    → 還是會問
```

**`python cleanup.py` 自動放行，而那個 `cleanup.py` 可以是模型五秒前才寫出來、內容還沒人看過的檔案。**

> 🔑 **一句「跑個測試」的同意，最後變成了「你想跑什麼都可以」。這比沒有權限系統更刺激——沒有權限系統的時候你至少知道它全開，有了這種權限系統你會以為自己在控制。**

最終採用的規則保守很多：**去掉前後空白後，完整那串指令一字不差才沿用授權**，只要參數或複合指令順序不一樣就重新問（第一版的折衷是把 `python`、`node`、`sh` 這類萬能指令記到前三個 token，變成 `['python -m pytest']`）。

## R（第二層：Seatbelt 沙箱的四塊 profile）

⚠️ **先試過的失敗路線值得記**：從 `(deny default)` 開始逐項放行，結果**連 `echo` 都跑不起來**（`exit=134`＝128+6＝`SIGABRT`，程式自己 abort，且沒留 crash log）。作者的結論是這條路不是不能走，但「沒有任何一份文件會告訴你 shell 啟動到底要讀哪些東西，只能一條一條試出來，而且 macOS 更新換個檔案位置就要再來一輪」。**所以改成 `(allow default)` 逐項收緊——這是黑名單不是白名單。**

四塊規則：

1. **讀取** — 把家目錄整包 deny（含 `/Users`、`/Volumes`），再把工作目錄 allow 回來
2. **metadata 全開** — `(allow file-read-metadata)`。少了它 shell 連啟動都失敗（`getcwd: cannot access parent directories`），因為 `getcwd()` 要一路往上走過每層父目錄，而工作目錄的父目錄正好在剛關掉的家目錄裡。🔑 **知道有這個檔案，跟讀得到裡面寫什麼，是兩回事**
3. **工作目錄內的禁區** — 用 regex 比對**路徑的每一段**（不是只列八個完整路徑），所以巢狀的 `.git`、`.env`、`.env.local` 都會中。🔑 **`run_command("cat .env")` 那個洞今天補在這裡——不是靠檢查指令字串，是靠作業系統拒絕那次 `open()`**
4. **寫入與網路** — 寫入這邊**才是真正的白名單**（預設全禁再開回來）：只開工作目錄、系統暫存（含 macOS 實際放暫存的 `/private/var/folders`）、以及 `/dev`。網路整個關掉（`curl`、`pip install`、`npm install` 一律不通）

⚠️ **`/dev` 那條是跑起來才知道要加的**：`2>/dev/null` 也算一次寫入，拿掉會看到 `/bin/sh: /dev/null: Operation not permitted` 導致整條指令失敗。

⚠️ **所有會變的路徑都用 `sandbox-exec -D NAME=value` 傳入、profile 裡用 `(param "NAME")` 取用**，不把路徑直接插進 Scheme 字串——因為專案名稱若含 `"` 會讓整份 profile 變成語法錯誤。

### 另一條不是 Seatbelt 規則但不能漏的：環境變數

子行程預設會繼承 Python 行程的環境變數，所以**只把 `ANTHROPIC_API_KEY` 拿掉不夠**。現在會移除常見的 `*_API_KEY`／`*_TOKEN`／`*_SECRET`／`*_PASSWORD`／`*_CREDENTIALS`，以及 `SSH_AUTH_SOCK`、`DATABASE_URL` 這類具名變數。

## C（關鍵行動）

- [ ] 檢查自己的權限記憶粒度：同意一次之後，系統到底記住了什麼？（`['python']` 那個坑）
- [ ] 權限詢問一定要印出**理由**，且 `y` 與 `a` 分開；無人可問時 **fail closed**
- [ ] 確認防護是「檢查字串」還是「OS 強制」——前者對自己開檔案的腳本無效

## 補充資料

### 🔴 家目錄一關，python 就不見了

profile 寫好第一個測試就掛：`dyld: Library not loaded: @executable_path/../lib/libpython3.13.dylib`。原因是 `python3` 裝在 `~/.local/`（uv 安裝），**把家目錄整包關掉等於順手把自己的 python 也關掉**。

🔑 **這不是特例**：uv、pyenv、rbenv、nvm、cargo、bun、asdf、mise 全都在 `~` 底下開一個點開頭的資料夾。

> **你想把 agent 關在專案裡，卻發現專案要用的編譯器、直譯器、套件管理器統統住在你剛剛鎖上的那扇門後面。**

所以要為 `TOOLCHAIN_DIRS`（`.local`／`.cargo`／`.rustup`／`.bun`／`.deno`／`.volta`／`.pyenv`／`.rbenv`／`.nodenv`／`.nvm`／`.asdf`／`.mise`／`.sdkman`／`.gem`／`.npm`／`.yarn`／`.pnpm`／`.cache/uv`／`.cache/pip`）開例外，且只把**實際存在的**目錄寫進 profile 免得清單無限長。

⚠️ **這份清單就是這套做法的代價**：每開一個例外，擋住的範圍就小一點（`~/.local` 底下若有你放的其他東西，agent 現在讀得到了）。🔑 **換成容器或虛擬機的話，工具鏈可以自己重裝一套，不必替宿主機上每一種安裝習慣開例外。**

### 在家目錄啟動 = 邊界失效（而且要明講）

profile 的規則是「deny 家目錄 → allow 工作目錄」，後面的規則會蓋掉前面的。**所以在家目錄裡啟動時，工作目錄就是家目錄，整個家目錄都落進讀寫範圍。**

⚠️ 但這不等於整份沙箱都沒擋：**網路禁令還在、工作目錄內的禁區規則也還在**，失效的只是「只能讀寫工作目錄」這條。

作者的處理是啟動時把狀態印出來（`sandbox_would_be_useless()`），並提出跟 `/permissions` 同一個原則：

> **你看不見的防護，不能算防護。**

### 拒絕之後要回一則結果給模型

「執行失敗」與「我拒絕執行」走同一個管道回覆（`is_error: true`）。模型收到後可以回頭問你、也可以說做不到，**但它必須知道這件事發生了**。

⚠️ 這也是 Day 12 的規矩（見 [[agent-loop-stop-conditions]]）：**日記裡每張許願單後面都要有一則結果緊跟著，被拒絕的操作也是一張許願單，漏掉它整本日記就送不出去了**。驗收有一條專門測「同一批三張單：第一張踩紅線、第二張被拒絕、第三張唯讀」——**擋下兩張，第三張照常執行；拒絕一個操作不等於中止整批**。

### 官方沙箱的邊界（Claude Code 文件）

> Read and Edit deny rules apply to Claude's built-in file tools and to file commands Claude Code **recognizes in Bash**, such as `cat`, `head`, `tail`, and `sed`. They don't apply to arbitrary subprocesses that read or write files indirectly… **For OS-level enforcement that blocks all processes from accessing a path, enable the sandbox.**

🔑 關鍵在 `recognizes in Bash`——擋得住的是它認得出來的那幾個檔案指令；**要每個行程都涵蓋到就得靠 OS 層級 sandbox**。這正是本篇兩天分成兩層的理由。

正版內建的唯讀指令清單（文件載明**不能自己改**）：`ls`／`cat`／`echo`／`pwd`／`head`／`tail`／`grep`／`find`／`wc`／`which`／`diff`／`stat`／`du`／`cd` 加唯讀形式的 `git`。想讓其中某個改成要問，得自己在設定檔的 `ask` 清單多寫一條（例如 `Bash(cat *)`）。

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[agent-bug-fix-eval-green-is-not-correct]] — **同系列 Day 13**：那次它改掉測試檔沒被問過一句話，正是本篇的動機
- [[agent-loop-stop-conditions]] — 同系列 Day 12：許願單必須有結果、停在日記完整處
- [[handcraft-agent-shell-tool]] — 同系列 Day 11：🔑「開 shell 是因為好用，不是因為安全」＋白名單擋不住逃脫
- [[hand-coding-agent-file-tools]] — 同系列 Day 05–10：路徑柵欄與 write_file 四道防呆
- [[agent-control-layer-permission-hooks-planning]] — Capability vs Permission 的規格層；🔑 本篇是「Prompt 不是安全邊界」的動手實證
- [[claude-code-auto-mode-permissions]] — Claude Code 權限模式的使用者面
- [[claude-code-codex-sandbox-h5i]] — 沙箱隔離的實作選項比較
- [[agent-error-recovery-taxonomy]] — Permission Denied 為何不該 Retry
- [[github-skill-security-audit]] — 「跟 AI 說別回傳」無效，要靠機制而非提示
- [[agent-api-key-handoff-six-methods-risk]] — 同一觀察（子行程繼承環境變數、點名指令攔得住但腳本攔不住）擴展到 Claude Code、Codex、Gemini CLI、Copilot、Cursor，附各家設定名稱
