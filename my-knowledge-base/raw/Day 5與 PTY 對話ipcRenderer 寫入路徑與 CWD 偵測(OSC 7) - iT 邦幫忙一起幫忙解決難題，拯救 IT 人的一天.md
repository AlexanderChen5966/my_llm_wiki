---
title: "Day 5|與 PTY 對話:ipcRenderer 寫入路徑與 CWD 偵測(OSC 7) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401796"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "Day 5|與 PTY 對話:ipcRenderer 寫入路徑與 CWD 偵測(OSC 7) 前四天都在「畫」——佈局、檔案樹、編輯器。今天要處理真正讓這個插件變成工作站的部分:側邊欄怎麼跟終端機說話..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 2

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### 讓 Claude 學會看圖:30 天打造 LLM 驅動的機器視覺工作站(MCP × OpenCV × Terminal Plugin)》系列 第 5 篇

## Day 5|與 PTY 對話:ipcRenderer 寫入路徑與 CWD 偵測(OSC 7)

- 分享至

## Day 5|與 PTY 對話:ipcRenderer 寫入路徑與 CWD 偵測(OSC 7)

前四天都在「畫」——佈局、檔案樹、編輯器。今天要處理真正讓這個插件變成工作站的部分:**側邊欄怎麼跟終端機說話,終端機又怎麼告訴我們它走到哪了** 。

這是雙向的兩條路,而且兩條都有坑。

## 去程:把文字送進 PTY

需求很簡單:點檔案樹的資料夾,終端機自動 `cd` 過去。

第一個直覺是去抓 xterm 實例然後 `term.write('cd ...\r')` —— **錯的** 。 `term.write()` 只是把字元「畫」在畫面上,shell 根本不知道有這回事。畫面上會出現一行假的指令,按 Enter 沒有任何反應。

要真的送進 shell,得走使用者敲鍵盤時走的那條路:renderer 把資料透過 IPC 丟給 main process,main 再寫進 node-pty。

![寫入 PTY 的主要路徑與備援路徑](https://ithelp.ithome.com.tw/upload/images/20260806/20109732ioL1LnRa4h.png)

```js
// src/index.js — sendToTerminal()
function sendToTerminal(text) {
  const state = hyperStore.getState();
  const uid = state.sessions && state.sessions.activeUid;
  if (!uid) return;

  // PRIMARY: window.rpc.emit —— 跟 Hyper 自己的 xterm 鍵盤輸入走同一個 API
  try {
    if (window.rpc) {
      window.rpc.emit('data', { uid, data: text });
      return;
    }
  } catch { /* fall through */ }

  // FALLBACK: 直接打底層 ipcRenderer
  try {
    const { ipcRenderer } = require('electron');
    const channelId = window.__rpcId;
    if (channelId) {
      ipcRenderer.send(channelId, { ev: 'data', data: [{ uid, data: text }] });
      return;
    }
  } catch { /* no method available */ }
}
```

兩件事值得說:

**`uid` 是必要的。** 一個 Hyper 視窗可以開很多分頁,每個分頁是獨立的 PTY。沒帶 uid 的訊息不知道要寫給誰,會被靜靜丟掉——這是最容易 debug 到懷疑人生的一種失敗:沒有錯誤、沒有例外,就是什麼都沒發生。

**備援那條的 payload 形狀不一樣。** `window.rpc.emit` 收物件,底層 `ipcRenderer.send` 收的是 `{ ev, data: [...] }` —— **data 是陣列** 。這層包裝是 Hyper 的 RPC 協定自己的格式,漏掉方括號一樣是靜默失敗。

會保留兩條路,是因為 `window.rpc` 是 Hyper 掛在 window 上的非公開物件,不同版本存在與否沒有保證;`window.__rpcId` 則是更底層、更穩定的東西。這種「在別人的 app 裡動手術」的專案,能多一條退路就多一條。

## 回程:終端機在哪個目錄?

反方向更難。Sidebar 要顯示正確的檔案樹、Git 面板要對正確的 repo 下指令,都得知道 shell 現在的工作目錄。但 PTY 是個黑盒子——它只會吐出「畫面上該顯示什麼」的位元組流,沒有任何「我 cd 了」的結構化事件。

DevTerminal 用兩條管道解決,一準一糙:

![CWD 偵測的兩條管道](https://ithelp.ithome.com.tw/upload/images/20260806/20109732GKpk8YAYw1.png)

### 管道一:OSC 7(準確,但要 shell 配合)

OSC(Operating System Command)是終端機的一組控制序列,其中 **OSC 7 專門用來回報當前目錄** 。格式是:

```perl
ESC ] 7 ; file:///C:/Users/foo/project BEL
```

![OSC 7 序列拆解與注入方式](https://ithelp.ithome.com.tw/upload/images/20260806/20109732OqzTApM127.png)

也就是 `\x1b]7;<file URI>\x07` 。Hyper 認得這個序列,收到後會 dispatch 一個 `SESSION_SET_CWD` action——對插件來說,這就變成一個乾淨的 Redux 事件了。

讓 shell 吐這個序列,只要改寫 `prompt` 函式:

```powershell
# %APPDATA%/devterm-plugin/psreadline-init.ps1
function prompt {
    $p = $PWD.ProviderPath
    Write-Host -NoNewline "$([char]27)]7;file:///$($p.Replace('\','/'))$([char]7)"
    return "PS $p> "
}
```

`[char]27` 是 ESC、 `[char]7` 是 BEL,`-NoNewline` 讓它不換行,反斜線換成正斜線才符合 file URI 規格。因為在 `prompt` 裡,**每次指令跑完重畫提示字元時都會自動送一次**,`cd` 也好、腳本裡切目錄也好,一律涵蓋。

### 這個腳本怎麼被載入?(以及一段自我修復)

早期版本是把 dot-source 寫進使用者的 PowerShell profile。後來拿掉了——理由寫在程式碼註解裡:

> 我們不再碰全域 profile:那會影響這台機器上 **每一個** PowerShell 工作階段(Windows Terminal、VS Code、純主控台),而且會蓋掉使用者自己的 `prompt` / oh-my-posh / starship。

改成只在 Hyper 自己啟動 shell 時注入,靠 Day 3 提過的 `decorateConfig`:

```js
if (process.platform === 'win32' && !config.shell) {
  try {
    execSync('where pwsh.exe', { stdio: 'ignore' });
    result.shell = 'pwsh.exe';        // PS7:PSReadLine 2.3.6 才有預測式自動完成
  } catch {
    result.shell = 'powershell.exe';  // 退回 PS5.1
  }
  result.shellArgs = ['-NoLogo', '-NoExit', '-Command',
    \`. "${path.join(process.env.APPDATA, 'devterm-plugin', 'psreadline-init.ps1')}"\`,
  ];
}
```

同一份設定順手解決兩件事:OSC 7 回報,以及 PSReadLine 的預測式 IntelliSense。

更值得一提的是 `setupPowerShellProfile()` 現在做的事——它不再 **寫入** profile,而是去把舊版本寫進去的那段 **刪掉**:

```js
const cleaned = original.split(/\r?\n/)
  .filter((line) =>
    !line.includes(PROFILE_MARKER) &&
    !/Test-Path.*psreadline-init\.ps1/i.test(line))
  .join('\n')
  .replace(/\n{3,}/g, '\n\n');
```

工具改變主意時,**要順手把自己以前留下的東西收乾淨**,而不是留給使用者自己去發現「為什麼我的 profile 裡有一行不認識的 dot-source」。這是我覺得任何會動使用者系統設定的軟體都該有的自覺。

### 管道二:解析提示字元(不挑 shell,但要小心)

OSC 7 需要 shell 配合。使用者用的是 cmd、bash,或是自己的 profile 蓋掉了我們的 `prompt`,就沒戲唱了。所以還有一條備援:直接從 PTY 輸出裡認提示字元。

```js
const PS_PROMPT_RE  = /^PS\s+([A-Z]:\\[^>]*?)>\s*$/im;   // PS C:\path>
const CMD_PROMPT_RE = /^([A-Z]:\\[^>]*?)>\s*$/im;        // C:\path>

function detectCwdFromPtyData(rawData, sessionUid) {
  const clean = stripAnsi(rawData);
  const lines = clean.split(/\r?\n/).slice(-5);  // 提示字元在輸出尾端
  // ...比對出 detected
}
```

`stripAnsi` 不能省——PTY 流裡到處是顏色控制碼,不洗掉的話 regex 永遠對不上。只看最後 5 行則是因為提示字元一定在尾巴,而且順便避開「使用者 `cat` 了一個內容長得像路徑的檔案」這種誤判。

後面接三層把關:

```js
// ① 黑名單:太廣的目錄不接受
if (/^[a-z]:\/?\s*$/.test(norm)) return;                     // 磁碟根目錄
if (/^[a-z]:\/users\/[^/]+\/?$/i.test(norm)) return;         // 家目錄
if (/^[a-z]:\/(windows|program files)/i.test(norm)) return;  // 系統目錄

// ② 500ms debounce + 真的存在才算數
clearTimeout(cwdDebounceTimer);
cwdDebounceTimer = setTimeout(() => {
  if (!fs.existsSync(detected)) return;
  const { projectRoot } = resolveProjectRoot(detected);
  // ...
}, 500);
```

`resolveProjectRoot()` 是最後一層加工:從偵測到的目錄一路往上找 `.git`,找到就用 git root 當專案根目錄。使用者 `cd src/components` 之後,檔案樹和 Git 面板依然對著整個專案,不會突然只剩一個子目錄。

### 第三層把關:不變就不做

這一段是後來補的,理由很實際:

```js
// 提示字元每按一次 Enter 就會重印,少了這道守門,
// 使用者每下一個指令就會觸發完整刷新管線(recent.json 同步寫檔 +
// 4 個 git 子行程 + 一次 re-render),打字愈快、磁碟和 CPU 燒得愈兇。
if (projectRoot === prevCwd && (!isActive || projectRoot === pluginState.cwd)) {
  return;
}
```

「偵測到路徑」和「路徑真的變了」是兩件事。前者一分鐘可以發生幾十次,後者可能一小時才一次。 **任何從資料流裡推導狀態的設計,都要在最後補一道「值沒變就什麼都別做」**,否則成本會跟使用者的打字速度成正比。

## middleware:五個 action 就是全部的協定

上面兩條管道最後都收斂到 Redux middleware。整個插件跟 Hyper 之間的狀態協定,就這五個 action:

```js
exports.middleware = (store) => (next) => (action) => {
  if (!hyperStore) hyperStore = store;   // 順手偷走 store 給 sendToTerminal 用

  switch (action.type) {
    case 'SESSION_ADD':        // 新分頁:建立 sessions[uid],繼承目前 cwd
    case 'SESSION_SET_ACTIVE': // 切分頁:把該 session 記住的 cwd 換回全域
    case 'SESSION_SET_CWD':    // OSC 7 來的,最準
    case 'SESSION_PTY_DATA':   // 原始輸出:提示字元偵測 + AI token 統計
    case 'SESSION_EXIT':       // 關分頁:delete sessions[uid]
  }
  return next(action);
};
```

注意 `pluginState.sessions` 是「每個分頁各自記住自己的 cwd」。切回舊分頁時 `SESSION_SET_ACTIVE` 會把它的 cwd 換回全域,Sidebar 跟著切——這是分頁式終端機該有的行為,但沒有這張表就做不到。

還有 `SESSION_PTY_DATA` 那行 `tracker.processOutput(action.data)`:同一份輸出流順便餵給 AI 用量統計模組,從 Claude / OpenAI CLI 的 stdout 裡撈 token 數。同一條資料流上掛兩個消費者,零額外成本。

## Bonus:一個讓 UI 凍住好幾秒的 UNC 路徑

終端機輸出裡的路徑會被做成可點連結,用的是 xterm 的 `registerLinkProvider` 。要判斷「這串字是不是真的路徑」,自然是 `fs.existsSync` 。然後就中獎了:

```js
// 略過 UNC 路徑(\\server\share)。link provider 在 hover 時同步執行,
// 對無法連線的主機做 fs.existsSync 會卡住整個 renderer 直到 SMB 逾時
// (好幾秒)——而且滑鼠每掃過那一行就再卡一次。
if (/^\\\\/.test(candidate)) continue;
```

`fs.existsSync` 對本機路徑是微秒級,對 `\\不存在的主機\share` 卻要等完整的 SMB 逾時,而且它是 **同步** 的——整個 renderer 連同終端機一起凍住。滑鼠只要掃過含有 UNC 路徑的那一行就重來一次。

教訓:**在 UI 執行緒上呼叫同步 I/O 之前,先想清楚這個 I/O 最壞情況會等多久** 。「檔案系統很快」這個假設,只在路徑真的指向本機檔案系統時成立。

## 小結

1. 送指令進 shell 要走 IPC(`window.rpc.emit` / `ipcRenderer.send`),`term.write()` 只是畫在畫面上
2. 每則訊息都要帶 session `uid`,漏了是靜默失敗
3. CWD 偵測一準一糙:OSC 7 靠 shell 配合最可靠,提示字元 regex 當備援
4. 注入 shell 設定用 `shellArgs`,別動使用者的全域 profile;改變主意時要把舊的清乾淨
5. 從資料流推導狀態,最後一定要補「值沒變就別動」的守門
6. UI 執行緒上的同步 I/O,先問最壞情況要等多久(UNC 路徑會等到 SMB 逾時)

明天(Day 6)是第一部的收尾,也是我踩過最詭異的一個坑:**明明綁好的 `onClick` 完全沒反應** 。追下去發現是 Hyper 內嵌的 xterm.js 把 React 合成事件吃掉了,解法是回頭用 document 層級的 capture-phase 原生監聽——這也是為什麼前幾天的程式碼裡到處都是 `addEventListener(..., true)` 。

---

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401676)

[

Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器

](https://ithelp.ithome.com.tw/articles/10401676)

系列文

[讓 Claude 學會看圖:30 天打造 LLM 驅動的機器視覺工作站(MCP × OpenCV × Terminal Plugin)》](https://ithelp.ithome.com.tw/users/20109732/ironman/9099) 共 5 篇

目錄

1. 1
	[Day 1|為什麼要讓 LLM 當視覺工程師?——系列開賽與架構總覽](https://ithelp.ithome.com.tw/articles/10400994)
2. 2
	[\# Day 2|Hyper Terminal 插件機制:在 Electron renderer 裡動手術](https://ithelp.ithome.com.tw/articles/10401038)
3. 3
	[Day 3|三欄式佈局:Sidebar / Editor / Terminal,與 inline style 的戰爭](https://ithelp.ithome.com.tw/articles/10401382)
4. 4
	[Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器](https://ithelp.ithome.com.tw/articles/10401676)
5. 5
	[Day 5|與 PTY 對話:ipcRenderer 寫入路徑與 CWD 偵測(OSC 7)](https://ithelp.ithome.com.tw/articles/10401796)
[完整目錄](https://ithelp.ithome.com.tw/users/20109732/ironman/9099)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/cccef095-9dfa-42b9-866d-cee30889b77d_small.jpg)

[笑死，能關防毒幹嘛要做免殺呢？從令牌偽造到把防毒關進沙箱隔離](https://itplus.ithome.com.tw/vod-page/516?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/12/25/a958401f-7e69-4ada-94ac-9f817c0e4485_small.jpg)

[Netflix 如何用機器學習技術加速影片創作](https://itplus.ithome.com.tw/webinar-page/244?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/bfb5bc28-183a-4440-a1ef-aee3c6a7ea5b_small.png)

[Aruba 安全網路架構保護公共資料](https://itplus.ithome.com.tw/vod-page/649?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/ab69ec8c-ae9e-41a7-a787-6a99df4bc15a_small.jpg)

[把 AI 帶入你的資料．啟動完整端到端 GenAI 旅程](https://itplus.ithome.com.tw/vod-page/1231?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

35 分