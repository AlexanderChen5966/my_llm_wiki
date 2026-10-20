---
title: 手刻 agent 的 run_command：開 shell 是因為好用，不是因為安全
date: 2026-08-12
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10402531（《為你自己手刻 Claude Code》Day 11，作者高見龍，專案 KeSi）
type: article
status: processed
asset_value: 50
---

同系列 Day 02–04 見 [[claude-code-api-request-reverse-engineering]]、Day 05–10（六個檔案工具）見 [[hand-coding-agent-file-tools]]。本篇是第七個工具。

## T（核心論點）

替自製 coding agent 加上 `run_command` 之後，作者把話講得很白：

> **開 shell 是因為它好用（能用 `&&`、`|`、`2>&1` 串接指令），不是因為它安全。**

而且 argv 陣列白名單**擋不住逃脫**——放行 `python3` 就等於放行 `python3 -c "print(open('.env').read())"`，放行 `sh` 就等於放行任何腳本內容：

> **第一格是乾淨的，不代表它要做的事是乾淨的。**

安全這一關這個工具本身擋不住，得靠後續的授權機制與容器／VM 隔離。本篇做的逾時、截斷、收行程群組，全部屬於「**避免出事**」而不是「**防止有人搞破壞**」。

## B（適用場景）

**適用：**
- 自建 agent 要加執行能力時的完整檢查表
- 對照理解 Claude Code Bash 工具的權限與行為設計（原文有正版對照節）
- 判斷「工具說明書寫規則」到底有多少強制力

**不適用 / 注意：**
- 🔴 **今天做的一切都繞得過前十天所有柵欄**：`IGNORE` 名單、`safe_path()`、`is_ignored()` 對 shell 全部無效，因為它們檢查的是模型填進 `file_path` 的字串，而開檔案的是 `cat` 不是我們的工具
- 原文警告：跟著做的人**只在願意整個弄丟的目錄裡啟動**，千萬不要在家目錄、放著正事的專案或有正式憑證的機器上執行

## R（操作步驟）

### 三行版本缺的四件事

```python
def run_command(command):
    proc = subprocess.run(command, shell=True, capture_output=True, text=True)
    return proc.stdout + proc.stderr, False
```

1. 沒有執行時間上限（`npm install` 要等好幾分鐘，無窮迴圈永遠回不來）
2. 沒有輸出上限（`yes` 會一路印到把記憶體吃光）
3. **stdout/stderr 分開收再前後接起來，原本穿插的順序就沒了**——看不出是跑到哪一步才出事
4. 結束碼被丟掉，模型分不出「測試全過」與「測試掛掉」

### 一、把 shell 寫死

`shell=True` 在 POSIX 用 `/bin/sh`、Windows 看 `COMSPEC`——同一行程式在不同機器可能是不同 shell。

```python
SHELL_PATH = "/bin/sh"
[SHELL_PATH, "-c", command]
```

> Python 文件明說：用 `shell=True` 時，處理空白與 metacharacter 的責任在 **the application**（也就是我們）。作者刻意不跳脫，因為那正是這個工具的功能——**該擋的地方不在這個函式裡，而是往外挪一層，看整道指令想做什麼再決定放不放行**。

### 二、逾時要收整個 process group，不是只收 `sh`

`subprocess.run(timeout=...)` 文件寫的是 **the child process（單數）**——時間到只殺 `sh`，它底下叫起來的管不到。

實驗：`(sleep 20; echo alive > orphan.txt) & echo 背景丟出去了; sleep 30`，上限 2 秒。`sh` 被殺、工具回報逾時，但 20 秒後 `orphan.txt` 照樣出現。

```python
proc = subprocess.Popen([SHELL_PATH, "-c", command], cwd=BASE_DIR, env=env,
    stdin=subprocess.DEVNULL, stdout=subprocess.PIPE, stderr=subprocess.STDOUT,
    start_new_session=True)
```

`start_new_session=True` → `setsid()` → `sh` 成為新 process group 的老大 → 用 `os.killpg` 對整組發訊號。

**先敲門再破門**：先送 `SIGTERM`，寬限 3 秒（`KILL_GRACE_SECONDS`）內清空就收工，還有釘子戶才送 `SIGKILL`。

**逾時要收，正常結束也要收**（`finally: stop_process_group(proc)`）——`sleep 25 & echo 背景啟動` 會讓 `sh` 兩秒內結束、結束碼 0，但 `sleep` 留下來；agent 跑一下午就一隻隻累積。

⚠️ **代價**：`npm run dev &` 這種背景服務會跟著被收掉，模型以為服務起來了其實早死了。**背景任務需要另外設計介面，不是在這個工具裡順便做。**
⚠️ **副作用**：`setsid()` 讓子行程脫離終端機，Ctrl-C 的 SIGINT 只打到 agent 本身；`KeyboardInterrupt` 往上拋沒人接，整個 agent 帶著 traceback 收攤。

### 三、輸出：stderr 併進 stdout，邊跑邊讀

```python
stdout=subprocess.PIPE, stderr=subprocess.STDOUT
```

實驗 `echo 一; echo 二 >&2; echo 三`：終端機是「一二三」，三行版本收到的是「一三二」——因為分兩條通道收完才接。**真正的 traceback 有幾十行，錯誤訊息一被搬走就看不出它原本卡在哪一步。**

代價是模型分不出哪一行來自 stderr，作者認為可接受：**真正的成敗訊號是結束碼，不是輸出走哪一條通道。**

**必須用另一條執行緒邊跑邊讀**：pipe 容量有限（macOS 實測 64 KB），管子一滿子行程卡住不結束，等它結束的主執行緒也一起卡住——死結。

🔴 **`read1()` 而不是 `read(n)`**：`read(n)` 要嘛讀滿 n bytes、要嘛等管子關掉；`read1()` 有多少拿多少。改之前 `sleep 25 & echo 背景啟動` 回給模型的輸出是**空的**，程式不會當掉，模型只會收到缺內容的結果然後開始亂猜。

### 四、兩道上限，守的是不同東西

```python
MAX_CAPTURE_BYTES = 1_000_000   # 守這台機器的記憶體
MAX_OUTPUT_BYTES  = 8000        # 守模型那邊（每輪都跟著日記整包送出去）
```

**截斷方式頭尾都留、只省略中間**——測試報告的重點常在最後幾行，編譯錯誤總結也在尾巴，只留開頭等於把結論丟掉。而且要寫出中間省略了多少 bytes：**資訊可以截斷，但不要隱瞞**。中文一字 3 bytes，`decode` 要帶 `errors="replace"` 免得切在字中間整支程式壞掉。

### 五、結束碼不能丟

```python
code = proc.returncode
if code is not None and code < 0:
    notes.append(f"被訊號 {-code} 終止")
notes.append(f"結束碼 {code}")

is_error = bool(timed_out or state["overflow"] or code)
```

- 好處：模型不用自己從輸出裡猜，測試沒過就是 `is_error=True`
- ⚠️ 壞處：`grep` 沒找到回 1、`diff` 有差回 1、`test` 為假回 1——這三個其實都成功了，只是答案是「沒有」。補償做法是**把結束碼寫進輸出讓模型自己判斷**
- 🔴 **管線會吃掉失敗**：`exit 1` → 結束碼 1；`exit 1 | tail -1` → **結束碼 0**。管線預設回報最後一個指令的結束碼。這在 agent 身上特別容易踩到，因為模型知道輸出會被截斷，很自然就接個 `| tail -20`，接完之後測試掛掉也回報 `is_error: false`
- 沒有輸出要明說 `（指令沒有輸出）（結束碼 0）`，不能回空字串；但空白本身也是輸出，`printf '   '` 的三個空白要原樣留下，不能先 `.strip()`

### 六、說明書要寫清楚的三件事

無互動輸入（`stdin=DEVNULL`，`vim`／`less`／密碼提示都不能用，Anthropic 官方 bash 工具也把這條列第一項）／每次都是全新行程（`cd`、`export` 不留到下一次，需要換目錄請 `cd x && ...`）／讀寫檔案優先用檔案工具。

> 最後一句的理由：`cat` 讀得到 `read_file` 拒絕的檔案，`sed -i` 不會經過 `write_file` 的先讀後寫檢查。**但這只是引導不是攔阻**——它要是不聽，程式裡沒有任何一行會阻止它。

### 🔴 f-string 把常數燒死在工具說明書裡

作者 demo 時把 `COMMAND_TIMEOUT` 從 120 改成 5，工具實際行為跟著變、回報訊息也寫「超過 5 秒」，**只有 description 還停在 120**（f-string 在模組載入時就把值填死了）。模型手上一份說 120、一份說 5，它沒挑一個相信，而是**把兩個都塞進同一句話，讀起來還很通順**。

> 解法二選一：說明書別寫死具體數字（只講「有時間上限，實際秒數看工具回報」），或讓這個數字只有一個來源。
> 📌 這是「文件與行為分岔」的一般性教訓，對 CLAUDE.md／Skill 描述同樣成立。

### 正版 Claude Code 對照

| 面向 | KeSi | Claude Code |
|---|---|---|
| 行程模型 | 每道指令獨立行程 | 同左 |
| `cd` 保留 | 不保留 | **會延續**（只要沒離開專案目錄）；環境變數同樣不保留 |
| 逾時 | 寫死 120 秒 | 預設 2 分鐘、上限 10 分鐘，**模型可自己傳 `timeout`** |
| 長輸出 | 截到 8,000 bytes 就沒了 | 邊跑邊寫進工作檔，太長只給模型**檔案路徑**讓它自己去讀 |
| 背景任務 | 主動收掉 | 有 `run_in_background` |

Anthropic Platform 的預定義 bash 工具又不同：是**持續存在的 session**（工作目錄、環境變數、產生的檔案都留給下一道指令），多一個 `restart` 參數。

> 官方文件兩句話值得抄下來：
> *Treat every command as untrusted input.*
> *Claude determines which command to run. Your application owns everything else: the shell process, the timeout, and the safety checks.*

## C（關鍵行動）

- [ ] 把 shell 寫死成常數，不要用 `shell=True` 的預設值
- [ ] `start_new_session=True` + `os.killpg`，**逾時與正常結束都要收整個 group**
- [ ] stderr 併進 stdout 同一根 pipe；用**獨立執行緒 + `read1()`** 邊跑邊讀，避免 pipe 滿了死結
- [ ] 兩道上限分開設（記憶體 vs 模型輸入），截斷保留頭尾並標明省略量
- [ ] 🔴 結束碼照實回報，並注意 `| tail` 會吃掉前面指令的失敗
- [ ] 工具說明書寫清楚「無互動輸入／狀態不保留」，且**不要把常數用 f-string 燒進 description**
- [ ] 認清這些都只是「避免出事」——真正的安全要靠授權層與容器隔離（見 [[agent-control-layer-permission-hooks-planning]]）
- [ ] 用 `pgrep` 驗收殘留行程：**工具說它收乾淨了不算數，作業系統說了才算**

## 原始摘要

18 條驗收全部由腳本跑過，其中最後兩條是**刻意造成的失敗**：`read_file` 擋得住而 `run_command` 擋不住、工作目錄柵欄對 shell 無效。作者也順手把 `ANTHROPIC_API_KEY` 從子行程環境變數移除，但明說「這擋得住順手 `echo` 的情況，擋不住 `cat .env`」。結論：**模型只會許願，你的程式握有最後的否決權**——只是目前 `run_command` 對每一張單子都說好，否決權還沒拿出來用。

## 相關連結

- [[agent-loop-stop-conditions]] — **同系列 Day 12（直接續篇）**：本篇給了執行任意指令的能力，那篇補上停止機制——`MAX_TURNS`／連續失敗／Ctrl-C 三個位置／SDK 重試；🔑 本篇的 `start_new_session=True` 正是「工具跑到一半按 Ctrl-C」從罕見變日常的原因
- [[hand-coding-agent-file-tools]] — **同系列 Day 05–10**：六個檔案工具與 write_file 四道防呆；本篇是第七個工具，也是把那些防呆全部繞過去的那一個
- [[claude-code-api-request-reverse-engineering]] — 同系列 Day 02–04：proxy 攔 payload、造假記憶實驗
- [[prompt-injection-vs-jailbreak]] — 🔴 **本篇是 Sink 端的極端案例**：一旦 Agent 有 shell，「不可信任的 Source × 高權限的 Sink」的 Sink 值直接拉滿
- [[agent-tool-runtime-seven-layers]] — Tool Runtime 七層責任的規格層；本篇的 Timeout／輸出上限／結束碼三分法對應第 5 層，「該擋的往外挪一層」對應第 3 層 Permission Gate
- [[agent-control-layer-permission-hooks-planning]] — Capability vs Permission、三道防線；本篇明說安全要靠「後面幾天的授權」與容器隔離
- [[claude-code-harness-internals]] — 官方工具 description 與 `<persisted-output>` 機制；正版「長輸出寫進工作檔只給路徑」在該篇有對應說明
- [[claude-code-auto-mode-permissions]] — Claude Code 權限模式的使用者面
- [[claude-code-codex-sandbox-h5i]] — 沙箱隔離的實作選項
- [[claude-code-tools]] — Claude Code 內建工具總覽（含 Bash）
