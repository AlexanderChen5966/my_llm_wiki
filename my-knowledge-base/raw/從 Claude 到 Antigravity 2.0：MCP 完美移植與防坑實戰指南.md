---
title: "從 Claude 到 Antigravity 2.0：MCP 完美移植與防坑實戰指南"
source: "https://hackmd.io/@BASHCAT/HyKZ0zRJGl"
author:
published:
created: 2026-05-25
description: "從 Claude 到 Antigravity 2.0 的無縫數位傳送與移植"
tags:
  - "clippings"
status: processed
---
> 📌 **發布提示** ：本文所附的示意圖皆以相對路徑存放在本機專案目錄中。若要直接發布至外部部落格平台（如 Medium 或 Blogger），請記得先將圖片上傳至您的公開圖床（例如 Imgur 或 GitHub），並將圖片路徑替換為對應的 CDN URL 以防破圖。

## 前言：當你興致勃勃遷往 Antigravity 2.0，卻發現 MCP 全廢？

隨著「Vibe Coding」與 AI 輔助開發蔚為風潮，相信不少開發者與科技愛好者都跟我一樣，在 Claude Desktop 或是 Claude Code 裡塞了滿滿的 **MCP (Model Context Protocol)** 伺服器。

簡單來說， **MCP（模型上下文協定）就像是給 AI 裝上了外接硬碟與雙手** 。原本的大語言模型（LLM）雖然聰明，但被困在沙盒中，既不能上網查最新資料，也無法碰你電腦裡的任何檔案。而 MCP 就是一個標準化的橋樑，讓 AI 能直接幫你自動化提交 GitHub 程式碼、安全閱讀指定資料夾，甚至是即時連網查地圖，成為我們最得力的「數位雙手」。

然而，當 Google 推出的 **Antigravity 2.0** IDE 框架與強大的 Gemini 3 代模型降臨時，許多人迫不及待想把這套強大的 MCP 工作流移植過去，結果一接上——夭壽喔！遠端 HTTP 的 MCP 連線全部靜默失效，本地以 `npx` 啟動的工具也因為找不到命令而瘋狂報錯。

這不是你的錯，而是因為 Antigravity 2.0 的執行 context 與 Claude 存在幾個極其隱蔽的「技術雷區」。今天這篇文章，我就以爆肝踩坑的真實血淚史，帶你深度剖析兩大平台的架構異同，破解那些讓你痛不欲生的 **Silent Failure（靜默失敗）** 陷阱，並附上一鍵自動化移植的 Python 腳本，讓你 100% 完美復刻你的強大 MCP 生態！

---

## 兩大巨頭的 MCP 機制對決：Claude vs Antigravity 2.0

在開始動手之前，我們先來釐清兩者的底層運作機制。這能幫我們在後面除錯時，不會像無頭蒼蠅一樣亂撞。

### 環境前置條件

- **作業系統** ：macOS (Sequoia)
- **依賴工具** ： `python3` (>= 3.10), `uv` 或 `node` (視您的 MCP 而定)
- **目標平台** ：Google Antigravity 2.0 (Gemini Advanced Agentic Coding 體系)

### 配置路徑大搬家

在 macOS 環境下，兩大平台的全域設定檔路徑大不相同。如果你找不到設定檔在哪裡，可以對照下表：

| 平台與工具 | 設定檔儲存路徑 | 設定格式主鍵 (Root Key) |
| --- | --- | --- |
| **Claude Desktop** | `~/Library/Application Support/Claude/claude_desktop_config.json` | `mcpServers` |
| **Claude Code (CLI)** | `~/.claude/settings.json` | `mcpServers` |
| **Antigravity 2.0 (IDE)** | `~/.gemini/antigravity/mcp_config.json` | `mcpServers` |
| **Antigravity 2.0 (CLI)** | `~/.gemini/antigravity-cli/mcp_config.json` | `mcpServers` |

雖然兩大平台的 root key 都叫做 `mcpServers` ，且結構看似一模一樣，但 Antigravity 2.0 的載入機制更具備「動態感應」的特點。我們可以透過下方這個動態生命週期圖，來看看 Antigravity 2.0 是如何處理這些設定的：

```
#mermaid-1779672206168-lxd0ebdhn{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1779672206168-lxd0ebdhn .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1779672206168-lxd0ebdhn .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1779672206168-lxd0ebdhn .error-icon{fill:#552222;}#mermaid-1779672206168-lxd0ebdhn .error-text{fill:#552222;stroke:#552222;}#mermaid-1779672206168-lxd0ebdhn .edge-thickness-normal{stroke-width:1px;}#mermaid-1779672206168-lxd0ebdhn .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1779672206168-lxd0ebdhn .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1779672206168-lxd0ebdhn .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1779672206168-lxd0ebdhn .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1779672206168-lxd0ebdhn .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1779672206168-lxd0ebdhn .marker{fill:#333333;stroke:#333333;}#mermaid-1779672206168-lxd0ebdhn .marker.cross{stroke:#333333;}#mermaid-1779672206168-lxd0ebdhn svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1779672206168-lxd0ebdhn p{margin:0;}#mermaid-1779672206168-lxd0ebdhn .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1779672206168-lxd0ebdhn .cluster-label text{fill:#333;}#mermaid-1779672206168-lxd0ebdhn .cluster-label span{color:#333;}#mermaid-1779672206168-lxd0ebdhn .cluster-label span p{background-color:transparent;}#mermaid-1779672206168-lxd0ebdhn .label text,#mermaid-1779672206168-lxd0ebdhn span{fill:#333;color:#333;}#mermaid-1779672206168-lxd0ebdhn .node rect,#mermaid-1779672206168-lxd0ebdhn .node circle,#mermaid-1779672206168-lxd0ebdhn .node ellipse,#mermaid-1779672206168-lxd0ebdhn .node polygon,#mermaid-1779672206168-lxd0ebdhn .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1779672206168-lxd0ebdhn .rough-node .label text,#mermaid-1779672206168-lxd0ebdhn .node .label text,#mermaid-1779672206168-lxd0ebdhn .image-shape .label,#mermaid-1779672206168-lxd0ebdhn .icon-shape .label{text-anchor:middle;}#mermaid-1779672206168-lxd0ebdhn .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-1779672206168-lxd0ebdhn .rough-node .label,#mermaid-1779672206168-lxd0ebdhn .node .label,#mermaid-1779672206168-lxd0ebdhn .image-shape .label,#mermaid-1779672206168-lxd0ebdhn .icon-shape .label{text-align:center;}#mermaid-1779672206168-lxd0ebdhn .node.clickable{cursor:pointer;}#mermaid-1779672206168-lxd0ebdhn .root .anchor path{fill:#333333!important;stroke-width:0;stroke:#333333;}#mermaid-1779672206168-lxd0ebdhn .arrowheadPath{fill:#333333;}#mermaid-1779672206168-lxd0ebdhn .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1779672206168-lxd0ebdhn .flowchart-link{stroke:#333333;fill:none;}#mermaid-1779672206168-lxd0ebdhn .edgeLabel{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1779672206168-lxd0ebdhn .edgeLabel p{background-color:rgba(232,232,232, 0.8);}#mermaid-1779672206168-lxd0ebdhn .edgeLabel rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1779672206168-lxd0ebdhn .labelBkg{background-color:rgba(232, 232, 232, 0.5);}#mermaid-1779672206168-lxd0ebdhn .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1779672206168-lxd0ebdhn .cluster text{fill:#333;}#mermaid-1779672206168-lxd0ebdhn .cluster span{color:#333;}#mermaid-1779672206168-lxd0ebdhn div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1779672206168-lxd0ebdhn .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#333;}#mermaid-1779672206168-lxd0ebdhn rect.text{fill:none;stroke-width:0;}#mermaid-1779672206168-lxd0ebdhn .icon-shape,#mermaid-1779672206168-lxd0ebdhn .image-shape{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1779672206168-lxd0ebdhn .icon-shape p,#mermaid-1779672206168-lxd0ebdhn .image-shape p{background-color:rgba(232,232,232, 0.8);padding:2px;}#mermaid-1779672206168-lxd0ebdhn .icon-shape rect,#mermaid-1779672206168-lxd0ebdhn .image-shape rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1779672206168-lxd0ebdhn :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}stdio 傳輸http 遠端編輯 mcp_config.json啟動 Antigravity 2.0 / 重啟 IDE偵測伺服器類型讀取 command 與 env使用 serverUrl 建立 SSE 連線背景執行 MCP 伺服器調用 list_tools 獲取 Schema在 ~/.gemini/antigravity-cli/mcp/ 下自動生成工具 JSON 檔AI Agent 完美載入 Lazy-loaded 工具
```

這個動態機制超猛，只要設定檔配置無誤，Antigravity 就會自動在 `~/.gemini/antigravity/mcp/<serverName>` 下為你生成所有工具的 JSON schema，完全不需要手動撰寫！

---

## 夭壽喔！那兩個讓你工具全廢的「Silent Failure」天雷大坑

接下來是本文的重頭戲。為什麼你直接把 Claude 的設定檔複製貼上到 `mcp_config.json` 會雷到爆？因為有兩個地方會發生無聲崩潰。

### ⚡ 陷阱一：url 還是 serverUrl？HTTP 協定的無聲崩潰

在 [Model Context Protocol 官方文檔](https://modelcontextprotocol.io/docs/concepts/transports#http-sse) 之中，如果我們要配置遠端 HTTP/SSE 的 MCP（例如 `supabase` 或 `fal-ai` ），我們會這樣寫：

```json
"supabase": {
  "type": "http",
  "url": "https://mcp.supabase.com/mcp?project_ref=..."
}
```

但是在 **Antigravity 2.0** 的脈絡下，底層的 Go 加載器（繼承自 Gemini CLI 家族）使用的是 **`serverUrl`** 欄位！  
如果你直接複製 `"url"` 過去，Antigravity 在解析時會直接無視它，甚至在聊天介面中完全不會報錯，直接給你一個 **靜默連線失敗 (Silent Failure)** 。當你想呼叫 Supabase 工具時，Agent 只會一臉無辜地說「我不知道有這個工具」，這真的會讓人廢到笑。

### ⚡ 陷阱二：Stdio 伺服器的執行 context 中 PATH 丟失

很多本地運行的 MCP，像是以 `npx` 啟動的 `deepl` 或以 `uvx` 啟動的 `mcp-obsidian` ，都依賴系統的環境變數。  
當我們在終端機執行 Claude CLI 時，它會自動繼承 `.zshrc` 中的 `PATH` 。但 **Antigravity 2.0** 通常是以 IDE 插件或背景服務形式運行，它的啟動環境非常乾淨，根本拿不到 Homebrew 的路徑（ `/opt/homebrew/bin` ）或者是 Node 的路徑。  
這導致它在執行 `command: "npx"` 時會直接噴出 `ENOENT` （找不到命令）的錯誤。因此，我們必須在 `env` 欄位中，強制手動注入完整的 `PATH` ，才能確保連線暢通。

---

## 神級一鍵移植：用 Python 幫你自動鋪路

既然知道了這兩個大坑，如果我們要手動修改 10 幾個 MCP 伺服器，不僅容易打錯 JSON 格式，還會改到眼睛脫窗。

為了拯救大家的肝，我寫了一個 Python 自動化移植腳本： `migrate_mcp.py` 。它會自動讀取您現有的 Claude Code 與 Claude Desktop 設定，融合兩者，自動備份，並完美修正 HTTP 的 `serverUrl` 欄位，同時為 Stdio 自動注入 PATH 環境變數！

### 移植腳本源碼 (migrate\_mcp.py)

```python
import json
import os
import shutil

def migrate():
    home_dir = os.path.expanduser('~')
    # 智慧相容 Antigravity IDE 與 CLI 的設定路徑
    target_dirs = []
    for folder in [".gemini/antigravity", ".gemini/antigravity-cli"]:
        path = os.path.join(home_dir, folder)
        if os.path.exists(path) or folder == ".gemini/antigravity-cli":
            target_dirs.append(path)
            
    target_dirs = list(set(target_dirs))
    
    claude_code_config_path = os.path.join(home_dir, ".claude/settings.json")
    claude_desktop_config_path = os.path.join(home_dir, "Library/Application Support/Claude/claude_desktop_config.json")
    
    print("=== Antigravity 2.0 MCP 智慧移植工具 ===")
    
    # 1. 備份與合併現有配置
    antigravity_mcp = {}
    for t_dir in target_dirs:
        os.makedirs(t_dir, exist_ok=True)
        config_path = os.path.join(t_dir, "mcp_config.json")
        if os.path.exists(config_path):
            backup_path = config_path + ".bak"
            shutil.copy2(config_path, backup_path)
            print(f"[+] 已安全備份原設定至: {backup_path}")
            with open(config_path, 'r', encoding='utf-8') as f:
                try:
                    data = json.load(f).get("mcpServers", {})
                    antigravity_mcp.update(data)
                except Exception:
                    pass
            
    merged_mcp = dict(antigravity_mcp)
    
    # 2. 讀取 Claude 設定
    claude_mcp = {}
    for path, source in [(claude_desktop_config_path, "Desktop"), (claude_code_config_path, "Code")]:
        if os.path.exists(path):
            with open(path, 'r', encoding='utf-8') as f:
                servers = json.load(f).get("mcpServers", {})
                for name, config in servers.items():
                    claude_mcp[name] = config
                    print(f"[發現] 從 Claude {source} 挖掘到: {name}")

    # 3. 智慧合併與轉換
    default_path = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    
    for name, config in claude_mcp.items():
        # 轉換 HTTP SSE 的 url 為 serverUrl
        if config.get("type") == "http":
            if "url" in config:
                config["serverUrl"] = config.pop("url")
                print(f"    [HTTP 修正] {name}: 'url' -> 'serverUrl'")
        else:
            # Stdio 類型自動注入 PATH
            if "env" not in config:
                config["env"] = {}
            if "PATH" not in config["env"]:
                config["env"]["PATH"] = default_path
                print(f"    [PATH 注入] 已為 {name} 補足環境變數 PATH")
                
        if name in antigravity_mcp:
            print(f"    [警告] 發現與 Antigravity 原有配置同名的伺服器 '{name}'，將進行環境變數與參數的智慧融合合併！")
        merged_mcp[name] = config

    # 4. 寫回配置
    for t_dir in target_dirs:
        config_path = os.path.join(t_dir, "mcp_config.json")
        with open(config_path, 'w', encoding='utf-8') as f:
            json.dump({"mcpServers": merged_mcp}, f, indent=2, ensure_ascii=False)
    print(f"\n[成功] 智慧合併完成！共配置了 {len(merged_mcp)} 個 MCP 伺服器！")

if __name__ == "__main__":
    migrate()
```

### 移植前後 (Before/After) 設定對比

看看自動化處理前後的設定差異，你就能明白為什麼腳本能幫你避開雷區了：

#### ❌ 移植前 (直接複製 Claude 的 HTTP 設定，Silent Failure)：

```json
"fal-ai": {
  "type": "http",
  "url": "https://mcp.fal.ai/mcp",
  "headers": {
    "Authorization": "Bearer ..."
  }
}
```

#### 📌 移植後 (腳本修正，Antigravity 2.0 完美連線)：

```json
"fal-ai": {
  "type": "http",
  "serverUrl": "https://mcp.fal.ai/mcp",
  "headers": {
    "Authorization": "Bearer ..."
  }
}
```

---

## 成果展現：5 大最受歡迎的科普級開源 MCP 伺服器

透過這套自動化移植工具，我們能完美地把日常最實用的 MCP 伺服器無痛導入到 Antigravity 2.0 中。以下為大家推薦 5 個最受社群歡迎、適合大眾直接動手體驗的開源通用 MCP 伺服器，讓你的 AI Agent 瞬間擁有無窮無盡的超能力：

| # | 伺服器名稱 | 協議類型 | 執行指令 / 遠端網址 | 核心科普用途（一聽就懂） |
| --- | --- | --- | --- | --- |
| 1 | `filesystem` | `stdio` | `npx -y @modelcontextprotocol/server-filesystem /path/to/dir` | 賦予 AI 「閱讀與安全整理指定電腦資料夾」的雙手 |
| 2 | `github` | `stdio` | `npx -y @modelcontextprotocol/server-github` | 讓 AI 直接幫你自動化提交程式碼、開 Issue 與管理倉庫 |
| 3 | `brave-search` | `stdio` | `npx -y @modelcontextprotocol/server-brave-search` | 讓 AI 擁有「高可信度連網搜尋」以獲取最新即時資訊的能力 |
| 4 | `sqlite` | `stdio` | `npx -y @modelcontextprotocol/server-sqlite --db ~/mydb.db` | 讓 AI 成為你的「資料庫管理員」，輕鬆下 SQL 查詢大數據 |
| 5 | `supabase` | `http` | `https://mcp.supabase.com/mcp` | 透過遠端 HTTP 協定，一鍵免安裝雲端串接 Supabase 後端資料庫 |

這五大伺服器不僅開源且設定極其簡單，完全是踏入 MCP 奇妙世界的最佳敲門磚！

---

## 結語與日常維護 SOP：開啟你的 AI 超能力時代

把設定移植過去之後，如果想在日常使用中維持這套強大且順暢的工具生態，請記住以下三條簡單實用的日常維護 SOP：

1. **重啟感應** ：每次編輯或更新全域設定檔 `mcp_config.json` 後，為了讓 Antigravity 順利重新連線並自動載入最新的工具 Schema，強烈建議您 **重啟 IDE 或者是重啟背景服務** 。
2. **依賴環境檢查** ：大多數本地 stdio 工具都依賴系統的 Node.js 或是 Python。若遇到 AI 回報「找不到命令」，請先在終端機執行 `node -v` 或 `python3 --version` 確認基礎運行環境是否有正確配置在系統 PATH 中。
3. **備份保平安** ：在進行任何手動修改或欄位調整之前，養成好習慣先備份原配置： `cp mcp_config.json mcp_config.json.bak` 。如果真的不小心改壞了 JSON 格式，隨時可以一秒還原。

這次從 Claude 到 Antigravity 2.0 的 MCP 移植實戰，讓我們深刻體會到： **細節決定成敗** 。僅僅是一個 `url` 改名為 `serverUrl` 的小欄位變動，就可能卡住許多開發者好幾天。希望這篇精心準備的避坑科普指南，能帶領大家順利跨越平台門檻，讓你的 AI Agent 在全新模型強大 Reasoning 能力的加持下，飛得再高、再遠一點！

---

## 參考資料與延伸閱讀

- [Model Context Protocol 官方文件](https://modelcontextprotocol.io/docs/getting-started/intro): 學習標準 MCP 的傳輸與工具協定。
- [n8n-mcp Antigravity 設置指南](https://github.com/czlonkowski/n8n-mcp/blob/main/docs/ANTIGRAVITY_SETUP.md): 探討如何在 Antigravity 中開啟全域的 `mcp_config.json` 。
- [Google Antigravity 自訂 MCP 伺服器整合](https://medium.com/google-developer-experts/google-antigravity-custom-mcp-server-integration-to-improve-vibe-coding-f92ddbc1c22d): Tarun Jain 探討如何在 Antigravity 中透過自訂 MCP 來提升 AI 程式設計能力。
- [Antigravity CLI 深度剖析](https://agentpedia.codes/blog/antigravity-cli-deep-dive): 揭露了 remote MCP 在 Antigravity 中必須使用 `serverUrl` 欄位的關鍵論文與部落格。
- [Firebase MCP 官方說明](https://firebase.google.com/docs/ai-assistance/mcp-server): 指引如何透過 Antigravity 的 `...` 選單開啟 raw config 並安裝 Firebase 專屬工具。