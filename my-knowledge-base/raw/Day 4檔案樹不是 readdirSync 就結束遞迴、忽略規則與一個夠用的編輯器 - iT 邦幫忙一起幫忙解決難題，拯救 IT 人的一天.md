---
title: "Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401676"
author:
  - "[[iThome]]"
published:
created: 2026-08-06
description: "Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器 昨天把 Sidebar / Editor / Terminal 三欄佈局處理完,今天終於要讓左側的 Sideb..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 2

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### 讓 Claude 學會看圖:30 天打造 LLM 驅動的機器視覺工作站(MCP × OpenCV × Terminal Plugin)》系列 第 4 篇

## Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器

- 分享至

## Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器

昨天把 Sidebar / Editor / Terminal 三欄佈局處理完,今天終於要讓左側的 Sidebar 開始做真正有用的事。

一個看起來很普通的檔案樹,實際上同時牽涉到幾個問題:

- 目錄要怎麼遞迴轉成 React 可以 render 的資料結構?
- `node_modules` 、`.git` 和 Windows junction 要怎麼排除?
- 使用者切換目錄或新增檔案時,畫面要怎麼更新?
- 點開一個程式碼檔案後,怎麼在不引入完整 IDE 的前提下提供編輯體驗?

今天會把這幾個問題串起來,完成 DevTerminal 的第一個 Project Explorer,以及一個「夠用就好」的文字編輯器。

## 先定義資料:檔案樹其實只需要四個欄位

UI 不應該直接操作 `fs.Dirent` 。檔案系統的資料結構是給 Node.js 用的,React 需要的是一個穩定、容易遞迴 render 的 tree。

所以先定義最小資料格式:

```js
{
  name: 'src',
  path: 'C:/project/src',
  type: 'directory',
  children: [
    {
      name: 'index.js',
      path: 'C:/project/src/index.js',
      type: 'file',
      ext: '.js'
    }
  ]
}
```

`directory` 才會有 `children`,`file` 則多一個 `ext` 方便前端決定圖示和開啟方式。這個格式的好處是:不管目錄有幾層,React 都可以用同一個元件遞迴處理。

## 第一關:遞迴讀取目錄

最直覺的版本其實很短:

```js
function buildTree(dirPath, depth = 0, extraIgnore = []) {
  if (depth > MAX_DEPTH) return null;

  const entries = fs.readdirSync(dirPath, { withFileTypes: true });
  const children = [];

  for (const entry of entries) {
    const fullPath = path.join(dirPath, entry.name);

    if (entry.isDirectory()) {
      const child = buildTree(fullPath, depth + 1, extraIgnore);
      if (child) children.push(child);
    } else {
      children.push({
        name: entry.name,
        path: fullPath,
        type: 'file',
        ext: path.extname(entry.name).toLowerCase(),
      });
    }
  }

  return {
    name: path.basename(dirPath),
    path: dirPath,
    type: 'directory',
    children,
  };
}
```

重點不在 `readdirSync` 本身,而在遞迴呼叫時把 `depth + 1` 傳下去。當走到底層目錄時,每一層都會把結果包成一個 node,最後回到 root:

```markdown
project/
├── src/
│   ├── index.js
│   └── components/
│       └── Sidebar.jsx
└── package.json
```

會變成:

```markdown
directory(project)
├── directory(src)
│   ├── file(index.js)
│   └── directory(components)
│       └── file(Sidebar.jsx)
└── file(package.json)
```

## 遞迴最怕兩件事:太深和太大

把整個目錄遞迴掃完,在小型專案沒有問題;但如果使用者不小心把 cwd 設成家目錄,程式就可能開始掃描數十萬個檔案。

因此這一版先設兩道保護:

```js
const MAX_DEPTH = 6;
const MAX_ENTRIES_PER_DIR = 200;

function buildTree(dirPath, depth = 0, extraIgnore = []) {
  if (depth > MAX_DEPTH) return null;
  // 每一層最多處理 200 個項目
}
```

`MAX_DEPTH` 避免遞迴無限向下,`MAX_ENTRIES_PER_DIR` 則避免單一目錄把 renderer 塞爆。這兩個限制不是檔案系統的限制,而是 UI 的保護欄。

這裡還有一個 Windows 特有的坑:symlink 和 junction。

```js
const stat = fs.lstatSync(fullPath);
if (stat.isSymbolicLink()) continue;
```

Windows 的某些系統資料夾會透過 junction 指向別的地方。若直接跟著連結走,你以為正在掃一個專案,實際上可能掃到整個使用者目錄,甚至遇到 `EPERM` 。

所以策略是:

1. 先用 `lstatSync` 判斷是不是連結。
2. 是連結就略過,不追蹤 target。
3. 讀取每個檔案時都放在 `try/catch` 裡,權限不足就跳過。

檔案樹的任務是幫使用者快速找到檔案,不是挑戰作業系統的所有權限邊界。

## 預設忽略清單與.gitignore

有些目錄不管專案有沒有寫進 `.gitignore`,都不應該出現在 Explorer 裡:

```js
const DEFAULT_IGNORE = [
  'node_modules',
  '.git',
  'dist',
  'build',
  '__pycache__',
  '.next',
  '.nuxt',
  'coverage',
  '.cache',
  'Thumbs.db',
  '.DS_Store',
];
```

這不是因為這些資料夾不重要,而是因為它們通常不適合被當成使用者要瀏覽的 source tree。尤其 `node_modules` 一旦展開,檔案樹立刻失去可讀性。

接著讀取專案根目錄的 `.gitignore`:

```js
function readGitignore(dirPath) {
  const gitignorePath = path.join(dirPath, '.gitignore');

  try {
    const content = fs.readFileSync(gitignorePath, 'utf-8');
    return content
      .split('\\n')
      .map((line) => line.trim())
      .filter((line) => line && !line.startsWith('#'));
  } catch {
    return [];
  }
}
```

Sidebar 在 cwd 改變時,先讀規則,再建立 tree:

```js
const extraIgnore = readGitignore(cwd);
const result = buildTree(cwd, 0, extraIgnore);
setTree(result);
```

`buildTree` 內部把預設清單和專案規則合併:

```js
const ignoreSet = new Set([...DEFAULT_IGNORE, ...extraIgnore]);

if (ignoreSet.has(entry.name)) continue;
```

這是目前的第一版,處理的是「名稱式」忽略,例如 `logs` 或 `temp` 。它還不是完整的 Git glob parser,所以 `*.log` 、 `**/cache` 這類規則後續仍可再補上。

這個取捨是刻意的:先把檔案樹的生命週期和 UI 跑通,再把 `.gitignore` 的規則相容性逐步補齊。與其一開始就複製完整 Git 行為,不如先讓核心資料流保持簡單可除錯。

## React 端:一個元件處理所有層級

有了 tree 資料後,`FileTreeItem` 不需要知道自己是第幾層的特殊情況。它只要收到 `node` 和 `depth`:

```jsx
function FileTreeItem({ node, depth }) {
  const [expanded, setExpanded] = useState(depth < 1);
  const paddingLeft = depth * 12 + 8;

  if (node.type === 'file') {
    return (
      <li>
        <div className="file-tree-item" style={{ paddingLeft }}>
          <span className="file-tree-icon">📄</span>
          <span>{node.name}</span>
        </div>
      </li>
    );
  }

  return (
    <li>
      <div className="file-tree-item" style={{ paddingLeft }}>
        <span>{expanded ? '▼' : '▶'}</span>
        <span>{node.name}</span>
      </div>

      {expanded && (
        <ul className="file-tree-children">
          {node.children.map((child) => (
            <FileTreeItem
              key={child.path}
              node={child}
              depth={depth + 1}
            />
          ))}
        </ul>
      )}
    </li>
  );
}
```

這裡有三個小決策:

- `depth < 1` 讓 root 的第一層預設展開,再深的目錄保持收合。
- `paddingLeft` 只由 depth 推導,不用為每層寫一組 CSS。
- React `key` 使用完整 path,避免不同目錄下同名檔案互相衝突。

最外層只需要把 root 的 children 丟進去:

```jsx
function FileTree({ tree }) {
  if (!tree || !tree.children || tree.children.length === 0) {
    return <div className="file-tree-empty">此目錄為空</div>;
  }

  return (
    <ul className="file-tree">
      {tree.children.map((child) => (
        <FileTreeItem key={child.path} node={child} depth={0} />
      ))}
    </ul>
  );
}
```

資料遞迴和元件遞迴剛好一一對應。這是檔案樹最舒服的地方:資料結構怎麼長,UI 就怎麼長。

## 不要每次按鍵都重新掃整棵樹

檔案樹不是建立一次就結束。使用者可能在 Terminal 裡執行:

```powershell
New-Item src\\new-file.js
Remove-Item old-file.js
```

Sidebar 需要知道檔案系統變了,但也不能每個事件都立刻 `buildTree` 。大量檔案複製時,chokidar 可能在很短時間內送出一串事件,所以要 debounce:

```js
const refresh = () => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(() => {
    const extraIgnore = readGitignore(cwd);
    setTree(buildTree(cwd, 0, extraIgnore));
  }, 300);
};

watcher
  .on('add', refresh)
  .on('unlink', refresh)
  .on('addDir', refresh)
  .on('unlinkDir', refresh);
```

這樣一批連續變更只會在最後一次事件後重新建構一次。

而且 `useEffect` 的 cleanup 很重要:

```js
return () => {
  clearTimeout(debounceTimer);
  if (watcher) watcher.close();
};
```

如果切換 cwd 時沒有清掉舊 watcher,會發生兩種問題:

- 舊目錄仍然持續觸發 refresh,造成不必要的 IO。
- 舊 timer 在新元件狀態上呼叫 `setTree`,畫面偶爾跳回上一個目錄。

另外,對 `C:\\` 、使用者家目錄或 `C:\\Windows` 這些過於寬廣的目錄,目前直接不啟用 watcher。這是一個看似保守,但實際上很重要的 renderer 保護。

## 第二個問題:我們需要一個 IDE 嗎?

當檔案樹可以點開檔案後,下一個問題就是編輯器。

完整 IDE 會帶來非常多功能:語法樹、游標系統、選取區塊、搜尋取代、多游標、語言伺服器、外掛系統。這些都很好,但目前 DevTerminal 的目標只是「在 Terminal 旁邊快速看和改檔案」。

所以先採用一個很樸素的方案:

```markdown
┌──────────────────────────────┐
│ <pre>  語法上色的背景層         │
│ <textarea> 透明文字輸入層      │
└──────────────────────────────┘
```

使用者實際輸入的是透明的 `textarea`,使用者看到的顏色則來自下面的 `<pre>` 。兩個元素的字型、行高、padding 和捲動位置必須完全一致。

## textarea + highlight.js 疊層編輯器

先用 `highlight.js` 的 core 版本,只註冊需要的語言:

```js
const hljs = require('highlight.js/lib/core');

hljs.registerLanguage('javascript', require('highlight.js/lib/languages/javascript'));
hljs.registerLanguage('typescript', require('highlight.js/lib/languages/typescript'));
hljs.registerLanguage('python', require('highlight.js/lib/languages/python'));
hljs.registerLanguage('csharp', require('highlight.js/lib/languages/csharp'));
hljs.registerLanguage('json', require('highlight.js/lib/languages/json'));
hljs.registerLanguage('css', require('highlight.js/lib/languages/css'));
```

不用 `require('highlight.js')` 的原因很單純:完整語言包會讓 bundle 變大。DevTerminal 是跑在 Hyper renderer 裡的插件,每一個不必要的 KB 都會影響啟動和更新。

接著把副檔名對應到語言:

```js
const EXT_LANG = {
  '.js': 'javascript',
  '.jsx': 'javascript',
  '.ts': 'typescript',
  '.tsx': 'typescript',
  '.py': 'python',
  '.cs': 'csharp',
  '.json': 'json',
  '.css': 'css',
  '.html': 'xml',
  '.xml': 'xml',
};

function getLang(filePath) {
  const ext = path.extname(filePath).toLowerCase();
  return EXT_LANG[ext] || null;
}
```

真正做 highlight 時,有指定語言就使用指定語言,沒有就退回自動偵測:

```js
function highlightCode(code, lang) {
  try {
    if (!code) return '';
    if (lang && hljs.getLanguage(lang)) {
      return hljs.highlight(code, { language: lang }).value;
    }
    return hljs.highlightAuto(code).value;
  } catch {
    return escapeHtml(code);
  }
}
```

最後把結果放進 `<pre>`,再把文字內容放進透明 `textarea`:

```jsx
const highlightedHtml = useMemo(
  () => (lang && !isMd ? highlightCode(content, lang) : escapeHtml(content)),
  [content, lang, isMd]
);

return (
  <div className="editor-code-container">
    <pre
      ref={preRef}
      className="editor-highlight-pre hljs"
      dangerouslySetInnerHTML={{ __html: highlightedHtml + '\\n' }}
      aria-hidden="true"
    />
    <textarea
      ref={textareaRef}
      className="editor-textarea editor-textarea--overlay"
      value={content}
      onChange={(e) => setContent(e.target.value)}
      onScroll={handleScroll}
      spellCheck={false}
    />
  </div>
);
```

這裡的 `dangerouslySetInnerHTML` 不是把使用者輸入直接塞進 DOM。 `highlight.js` 會將程式碼轉成帶有 token class 的 HTML,而沒有語言或 highlight 失敗時則先呼叫 `escapeHtml` 。編輯器顯示的是程式碼,不是讓它被當成 HTML 執行。

## 為什麼一定要同步 scroll?

疊層編輯器最容易出現的 bug 就是:文字輸入層已經捲到第 200 行,底下的彩色程式碼還停在第 1 行。

因此 `textarea` 捲動時,同步 `<pre>`:

```js
const handleScroll = useCallback(() => {
  if (textareaRef.current && preRef.current) {
    preRef.current.scrollTop = textareaRef.current.scrollTop;
    preRef.current.scrollLeft = textareaRef.current.scrollLeft;
  }
}, []);
```

兩層的 CSS 也要完全對齊:

```markdown
.editor-code-container {
  position: relative;
  flex: 1;
  overflow: hidden;
}

.editor-highlight-pre,
.editor-textarea--overlay {
  position: absolute;
  inset: 0;
  padding: 8px 12px;
  font-family: 'Consolas', 'Fira Code', monospace;
  font-size: 13px;
  line-height: 1.6;
  white-space: pre;
  tab-size: 2;
}

.editor-textarea--overlay {
  color: transparent;
  caret-color: #f5e0dc;
  background: transparent;
  z-index: 2;
}
```

`textarea` 的文字設成透明,但 caret 保持可見。使用者看到的是下層 highlight,游標則由上層 textarea 繪製。

## 每次 render 都 highlight 會卡頓

編輯器每輸入一個字就會 render 一次。如果每次 render 都對整個檔案執行 `hljs.highlight`,大檔案很快就會感覺到延遲,而且這個 renderer 同時還在跑 Terminal。

所以把 highlight 放進 `useMemo`:

```js
const highlightedHtml = useMemo(
  () => highlightCode(content, lang),
  [content, lang]
);
```

這不會讓每次輸入都不 highlight,因為 `content` 確實會改變;它避免的是其他 state 改變時重複做相同工作。像是切換預覽按鈕、錯誤訊息更新,都不需要重新 tokenize 整份檔案。

## 儲存、Tab 和 Escape:小功能也有 Electron 的坑

編輯器最基本的互動只有三個:

- `Ctrl+S` 儲存。
- `Tab` 插入兩個空白,而不是跳到瀏覽器下一個控制項。
- `Escape` 關閉編輯器。

`Tab` 的處理可以直接使用 selection range:

```js
if (e.key === 'Tab') {
  e.preventDefault();

  const start = textarea.selectionStart;
  const end = textarea.selectionEnd;
  const next = content.substring(0, start) + '  ' + content.substring(end);

  setContent(next);
  requestAnimationFrame(() => {
    textarea.selectionStart = textarea.selectionEnd = start + 2;
  });
}
```

`requestAnimationFrame` 是因為 React 更新 value 後,selection 可能還沒落到新的 DOM 上。等下一幀再恢復游標,位置才穩定。

儲存時則直接寫回原始檔案:

```js
fs.writeFileSync(filePath, contentRef.current, 'utf-8');
setSavedContent(contentRef.current);
```

這裡使用 `contentRef` 而不是只依賴 effect closure,因為事件 listener 只註冊一次。若直接捕捉 render 當下的 `content`,按下 Ctrl+S 時可能拿到舊內容,這就是典型的 stale closure。

另外,Hyper 的 xterm.js 會攔截 React synthetic event,所以儲存和關閉按鈕使用 document capture-phase listener:

```js
document.addEventListener('mousedown', captureHandler, true);
document.addEventListener('keydown', saveKeyHandler, true);
```

第三個參數的 `true` 代表 capture phase。事件先經過 document,再往下走到目標,因此可以在 xterm.js 有機會攔截之前處理快捷鍵和按鈕。

## 今天完成的資料流

把今天的功能串起來,流程大概是這樣:

```markdown
cwd 改變
  ↓
readGitignore(cwd)
  ↓
buildTree(cwd, 0, extraIgnore)
  ↓
Sidebar state
  ↓
FileTreeItem 遞迴 render
  ↓
點擊檔案
  ↓
EditorPane 讀取檔案
  ↓
textarea 編輯 + pre highlight
  ↓
Ctrl+S 寫回磁碟
```

檔案樹和編輯器看起來是兩個 UI 元件,中間其實只需要一個事件:

```js
onFileSelect(filePath)
```

這也是我很喜歡 React component 的地方:檔案系統、tree rendering 和編輯狀態可以各自處理,只透過明確的 callback 接在一起。

## 小結

- 檔案樹先轉成穩定的 `{ name, path, type, children }` 資料結構,再交給 React。
- `MAX_DEPTH` 、每層項目上限和 symlink 檢查,是避免一次掃爆 renderer 的基本防線。
- 預設忽略清單先處理高風險和高雜訊目錄,`.gitignore` 則以名稱式規則接入第一版。
- `FileTreeItem` 用遞迴元件對應遞迴資料,每一層只需要管理自己的 expanded state。
- chokidar 負責感知檔案變更,debounce 避免短時間內重建整棵樹。
- `textarea + pre` 可以用很少的程式碼做出可編輯、可上色的輕量編輯器。
- `useMemo` 、scroll sync、 `contentRef` 和 capture-phase listener,分別處理效能、視覺同步、事件閉包和 Hyper 事件攔截問題。

明天(Day 5)會把視線移回 Terminal:如何從 shell prompt 和 OSC 7 取得目前 cwd,再把 PowerShell / PSReadLine 的能力接進 DevTerminal。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401382)

[

Day 3|三欄式佈局:Sidebar / Editor / Terminal,與 inline style 的戰爭

](https://ithelp.ithome.com.tw/articles/10401382)

系列文

[讓 Claude 學會看圖:30 天打造 LLM 驅動的機器視覺工作站(MCP × OpenCV × Terminal Plugin)》](https://ithelp.ithome.com.tw/users/20109732/ironman/9099) 共 4 篇

目錄

1. 1
	[Day 1|為什麼要讓 LLM 當視覺工程師?——系列開賽與架構總覽](https://ithelp.ithome.com.tw/articles/10400994)
2. 2
	[\# Day 2|Hyper Terminal 插件機制:在 Electron renderer 裡動手術](https://ithelp.ithome.com.tw/articles/10401038)
3. 3
	[Day 3|三欄式佈局:Sidebar / Editor / Terminal,與 inline style 的戰爭](https://ithelp.ithome.com.tw/articles/10401382)
4. 4
	[Day 4|檔案樹不是 readdirSync 就結束:遞迴、忽略規則與一個夠用的編輯器](https://ithelp.ithome.com.tw/articles/10401676)
[完整目錄](https://ithelp.ithome.com.tw/users/20109732/ironman/9099)

[![.](https://itadstatic.ithome.com.tw/B9/1784885238_6a632ff62c5c0.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/21/70be29c3-8b84-47dd-b8d2-9b03050862b7_small.png)

[FHIR：一段旅程 – 技術、藍圖與應用實例](https://itplus.ithome.com.tw/vod-page/690?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/709f477b-3e59-49d0-9c99-80cdb72c0d67_small.png)

[運用多維策略，打造企業級設計系統](https://itplus.ithome.com.tw/vod-page/1339?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

46 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/ad05ddde-b04a-4120-b9ea-bb0de19d77a6_small.png)

[以 Kotlin 打造 AI Agent：Koog AI Agent 框架實戰入門](https://itplus.ithome.com.tw/vod-page/1349?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

36 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/a09fa9f5-f703-4d2f-9bbf-4a1144fa59c3_small.png)

[DataOps + ChatOps: 打造跨團隊協作的控制面](https://itplus.ithome.com.tw/vod-page/1473?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

25 分