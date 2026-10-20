---
title: "別吝嗇 Token，要好好設計：善用 GitHub Copilot 的脈絡策略"
source: "https://codelove.tw/@tony/post/3V2n43"
author:
published:
created: 2026-07-07
description: "※如果有幫助，請記得收藏、按讚！！**＜📝本篇文章的目標讀者＞**- 剛開始在工作中使用 GitHub Copilot Chat 或 Agent mode 的人- 已導入或正在評估導入 MCP 與 Agent Skills 的人- 正在煩惱 `AGENTS.md` 與設計文件該放..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月04日](https://codelove.tw/@tony/post/3V2n43 "2026-07-04 17:06:06")

## 別吝嗇 Token，要好好設計：善用 GitHub Copilot 的脈絡策略

※如果有幫助，請記得收藏、按讚！！

**＜📝本篇文章的目標讀者＞**

- 剛開始在工作中使用 GitHub Copilot Chat 或 Agent mode 的人
- 已導入或正在評估導入 MCP 與 Agent Skills 的人
- 正在煩惱 `AGENTS.md` 與設計文件該放哪裡的人
- 想管理 AI 開發速度、品質與成本的技術負責人
- 想設計 AI 自動化與人工審核邊界的人

---

[.NET Lab](https://dotnetlab.connpass.com/) 於 2026 年 6 月讀書會的演講資料在 [這裡](https://speakerdeck.com/ochtum/tokunwoketiruna-she-ji-siro-github-copilotwoxian-kushi-ukontekisutozhan-lue)

資料管理

---

剛開始使用 GitHub Copilot 時，常會在意這些事：

- 要怎麼提問，才會寫出正確的程式碼
- 指示要具體到什麼程度才夠
- 提示詞是不是越短越好
- 反覆重做，是不是代表提問方式不好

當然，提問方式很重要。

但當你越來越熟悉 Copilot Chat、Agent mode、MCP、Agent Skills、客製化指示之後，問題會逐漸改變。

不只是「怎麼問」，而是 **要讓 AI 讀到什麼** 更重要。

例如，AI 寫出了和意圖不同的實作。這個原因不一定是提示詞寫得不好。

- 沒有讀到必要的設計文件
- 提示了大量無關的工具
- 搜尋結果或命令輸出太長
- 被舊的對話歷史帶偏
- 重要限制被埋在巨大的指示檔中
- 把本來不該交給 AI 判斷的事情也自動化了

這類問題，單靠提示詞的措辭無法解決。

必須設計 AI 在工作時使用的整體資訊，也就是 **上下文** 。

本篇最想傳達的一句話是：

> 目標不是減少 token。目標是把能幫助 AI 做出好判斷的資訊，以適當的時機與粒度交給它。

單純刪減 token，可能會把必要的設計資訊與審核依據一起弄丟。相反地，為了保險把所有資料都丟給 AI，又會增加成本與判斷雜訊。

設計

因此，本篇會把傳給 GitHub Copilot 的資訊拆成以下幾類：

- 永遠要遵守的固定指示
- 特定工作會用到的流程
- 需要時才讀取的設計系統與設計知識
- 可用工具的定義
- 工具執行結果
- 對話歷史
- 可透過快取或壓縮重用的資訊
- 依任務選擇的模型

接著會用初學者也能理解的方式，整理 MCP、Skills、 `AGENTS.md` 、 `DESIGN.md` 、CLI 與模型選擇該怎麼分工。

## 🔷1. Token 不只會因為提問文字而增加

先整理 token 與上下文的基本概念。

送進生成式 AI 的文字，會被切成小單位來處理。這個單位就是 **token** 。

在日文裡，不一定是「1 個字 = 1 個 token」。英文單字、符號、程式碼、JSON、檔案路徑等，也會依模型方式切成多個 token。

不過，實務上不需要每次都自己手算「這段字串有幾個 token」。

重點是，除了提問文字之外，還有很多資訊也會傳給模型。

## 🔹上下文視窗是 AI 的工作桌

上下文視窗是 AI 在這次處理中能參照的資訊範圍。

對初學者來說，可以把它想成 **AI 的工作桌** 。

工作桌上不只有使用者輸入的問題，還會放上像這些內容：

1. 系統層指示
2. `AGENTS.md` 等客製化指示
3. 可用工具的名稱、說明、輸入 schema
4. 使用者的提問
5. AI 過去的回答
6. 載入的原始碼或設計文件
7. 工具呼叫的參數
8. 搜尋結果、log、JSON 等工具執行結果
9. 最終要產出的回答

下圖說明這些資訊如何共享同一個上下文視窗。

程式設計

[![GitHub Copilot 的上下文視窗中，固定指示、工具定義、對話歷史、檔案與工具結果共同共享的示意圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2F212aeacc-1b2c-470e-a04d-088e9fbcddf7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a55b50f6c0ad7fd8fd5f87c480644d50)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2F212aeacc-1b2c-470e-a04d-088e9fbcddf7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a55b50f6c0ad7fd8fd5f87c480644d50)

*GitHub Copilot 的上下文消耗，不只是提問文字，還包含指示、工具、執行結果與對話歷史的總和。*

這裡特別值得注意的是， **工具執行結果** 。

工具呼叫本身可能只需要工具名稱與簡短參數，但如果結果回傳以下內容，就會一下子消耗很多上下文：

- 500 則 Slack 訊息
- 200 個 GitHub Issue
- 數萬行建置紀錄
- 所有原始碼檔案
- 1000 行資料庫查詢結果
- 巨大的 JSON 回應

有時候，比起把提問文字縮短 20 個字，把不必要的搜尋結果從 500 筆減成 20 筆，效果大得多。

也就是說，思考 token 時，不能只想「把提示詞縮短」。

> 要同時看 AI 平常就持有的資訊，以及工作過程中新增的資訊。

## 🔹輸入 token、輸出 token、快取 token 的差異

「token」這個詞有好幾種意思，容易混淆，所以先分開。

| 用語 | 意義 |
| --- | --- |
| 上下文視窗 | 模型一次能參照的資訊範圍 |
| 輸入 token | 傳給模型的指示、問題、歷史、工具定義、工具結果等 |
| 輸出 token | 模型產生的說明、程式碼、摘要等 |
| 快取 token | 與先前請求共通、被重用的輸入部分 |
| 費用 / AI Credits | 依產品、模型、合約、快取單價等計算出的使用量 |

這裡最重要的是， **上下文量與費用不是完全相同的概念** 。

程式設計

例如，即使每次都把很長的固定指示送給模型，若 Prompt Caching 生效，可能會用不同於一般輸入的單價或計算方式。

但即便被快取，不必要的資訊對 AI 判斷造成的影響並不會因此消失。

「在計費上能便宜重用」與「值得讓 AI 讀」是兩回事。

另外，GitHub Copilot 的計費方式可能變動。2026 年 6 月 1 日之後的說明中，已有 AI Credits 與 token 用量的對應關係，但你閱讀本文時的最新資訊，請以 [GitHub Copilot 的模型與價格](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing) 為準。

把固定單價直接寫進文章裡，將來一旦調價內容就會過時，因此本文以機制與判斷方式為主。

## 🔹MCP 通信發生，不代表就是 LLM token 消耗

還有一件事也要先區分。

MCP 的通訊或本地命令的執行本身，和 LLM 的 token 消耗不是同一件事。

例如，MCP client 對 MCP server 發送 `tools/list` ，取得工具清單。

```
MCP 客戶端
    ↓ tools/list
MCP 伺服器
    ↓ 工具清單
MCP 客戶端
```

這是 MCP 的 RPC 通訊。

取得的工具清單中，哪些資訊會傳給模型，取決於 client 或 agent runtime 的實作。

若有傳給模型，工具名稱、description、input schema 等就會影響輸入上下文。反之，如果只保留在 client 內部而沒有送進模型，就不一定會直接變成 LLM 輸入 token。

因此，這樣理解比較清楚：

```
發生通訊
    ↓
client 取得資訊
    ↓
將資訊傳給模型
    ↓
影響模型的輸入上下文
```

「一連接 MCP，所有定義與資源本文每次都會直接送進模型」這種說法並不精確。

開放原始碼

不過，AI 要能選工具，就必須某種程度知道有哪些可用工具。因此，工具數量與 schema 大小，是上下文設計上很重要的觀點。

## 🔹先觀測，再做最佳化

想減少 token 時，不建議一開始就直接把指示檔縮短。

先觀察是哪裡膨脹了。

GitHub Copilot CLI 提供了查看上下文與使用量的指令。

```
/context
```

用來確認目前上下文視窗的使用狀況。

```
/usage
```

用來確認 session 使用量與各模型 token 資訊等。

```
/compact
```

用來摘要變長的對話歷史，並壓縮上下文。

由於指令支援與顯示內容可能隨產品更新而改變，也請一併參考 [GitHub Copilot CLI 的上下文管理](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/context-management) 。

### ▸觀測時的重點

把同一個工作在以下條件下比較，較容易找出原因。

| 比較 | 要確認的事 |
| --- | --- |
| MCP 開啟／關閉 | 加入工具定義後，固定區域增加多少 |
| 搜尋結果 500 筆／20 筆 | 工具結果對上下文的影響 |
| 檔案全文／相關範圍 | 載入檔案量的影響 |
| 長對話／新 session | 對話歷史累積的影響 |
| 壓縮前／ `/compact` 後 | 歷史摘要帶來的變化 |
| Auto／手動指定模型 | 模型選擇如何影響品質、速度與使用量 |

重點是要讓模型、設定、儲存庫、提問內容保持一致再比較。

如果一次改太多條件，就不知道到底是哪個因素發揮作用。

💡 **第一個結論**

不要只削減提問文字，而是要確認固定指示、工具定義、載入檔案、工具結果、對話歷史中，究竟是哪裡膨脹了。

## 🔷2. 用資訊壽命區分 AGENTS.md、Skills、DESIGN.md

整理 AI 所需資訊時，常會想把所有內容都寫進同一個巨大指示檔。

也就是「只要每次都讀，應該就不會漏掉吧」的想法。

但如果換成人類工作來比喻，就像每天上班前都要先把所有工作規則、作業手冊、產品設計文件、過去會議紀錄全部讀完一遍。

設計

需要的資訊，應依 **資訊壽命** 與 **適用範圍** 來分。

- 幾乎所有工作都要遵守的資訊
- 只有特定工作才需要的資訊
- 只有設計變更時才需要的資訊
- 會依目標檔案或目錄而變動的資訊
- 只針對這次需求才需要的資訊

## 🔹AGENTS.md 不是「全知識倉庫」，而是入口

在 GitHub Copilot 中， `AGENTS.md` 可以作為給 agent 的指示。

請注意檔名：

```
AGENTS.md
```

是複數，不是 `AGENT.md` 。

放在 `AGENTS.md` 裡的，應該是幾乎所有任務都要遵守的短規則，以及指向相關資料的入口。

例如：

```
# Repository rules

- 回答與程式碼註解請用日文撰寫。
- 修改既有公開 API 前，先列出影響範圍。
- 作業完成後，執行對應變更的測試。
- 不要修改無關檔案。

# Architecture and design rules

- 修改公開 API 前，請先閱讀 \`docs/adr/0002-api-versioning.md\`。
- 更改 UI 外觀前，請先閱讀 \`DESIGN.md\`。
- 變更 DB schema 時，請確認 \`docs/database/\` 中對應的 ADR。
- 只有 UI 變更時，不要去讀後端設計文件。
```

重點不是把 `DESIGN.md` 的內容全部複製進來。

設計

`AGENTS.md` 應該扮演一個 **路由器** ：

```
這次任務是什麼
    ↓
需要遵守哪些固定規則
    ↓
還要讀哪些額外資料
```

這樣一般的小改動只需要短的共通規則，只有在設計變更時才讀取詳細文件。

## 🔹Agent Skills 是需要時才打開的作業手冊

Agent Skills 是把特定作業所需的流程、知識、腳本、參考資料等集中起來的機制。

對初學者來說，可以把它想成 **專業作業的操作手冊** 。

例如，以下工作可以拆成 Skill：

- 撰寫技術文章
- 進行安全性審查
- 撰寫 Release Note
- 驗證資料庫遷移
- 產生圖解
- 部署到 Azure

Skill 不一定要一開始就把全文都讀進來，而是先根據 name、description 等資訊，判斷是否適用於這個任務。

選中之後，再讀 `SKILL.md` 本文與必要的參考檔。

### ▸Skill 的 description 要寫觸發條件

反例：

```
description: 支援開發
```

這樣無法知道這個 Skill 什麼時候該用。

好的寫法應該具體說明對象工作與使用條件：

```
description: >
  當要根據以 GitHub Copilot 為主題的技術文章章節草稿，
  產生適合 Qiita 的 Markdown 文章本文時使用。
  會參考原始素材、圖解生成紀錄與文風規則。
```

如果 description 太模糊，可能會導致需要的 Skill 沒有被選中，或在不相關的任務中被誤觸發。

這不只是 token 的問題，也會造成 AI 套用錯誤的作業流程。

### ▸不要把 Skill 本文做得太大

如果把範例程式碼、完整 API 規格、過去案例、長清單全部塞進 `SKILL.md` ，一旦觸發，context 會變得很大。

程式設計

比較好整理的方法是這樣：

```
my-skill/
├─ SKILL.md
├─ scripts/
│  └─ validate.ps1
├─ references/
│  ├─ api-rules.md
│  └─ review-checklist.md
└─ assets/
   └─ template.md
```

`SKILL.md` 只寫作業入口與判斷流程。

當需要詳細 API 規範時，再讀 `references/api-rules.md` ；驗證流程則執行既有的 `scripts/validate.ps1` 。

這樣就能避免每次都讀全部內容，而是逐步載入必要資訊。

## 🔹DESIGN.md 在需要時參考

`DESIGN.md` 在 [Google Labs 官方儲存庫](https://github.com/google-labs-code/design.md) 中，是作為「說明 coding agents 的 visual identity 的格式」公開的。

也就是說，它不是架構設計書或 DB 設計書，而是讓 AI 一致處理品牌與產品外觀的 **設計系統文件** 。

[官方規格](https://github.com/google-labs-code/design.md/blob/main/docs/spec.md) 指出， `DESIGN.md` 主要由兩層構成：

- YAML front matter：機器可讀的 design token，必要時可加上
- Markdown body：人類也能閱讀的設計理由與指引

YAML front matter 可定義這類 token：

- `version` / `name` / `description`
- `colors`
- `typography`
- `rounded`
- `spacing`
- `components`

顏色可以寫成 CSS 顏色值，尺寸可以寫成帶單位的 Dimension，例如 `px` 、 `em` 、 `rem` 。也可以使用像 `{colors.primary}` 這樣的 token 參照。

Markdown 本文可以只寫必要的章節。官方規格建議存在的章節順序如下：

1. Overview
2. Colors
3. Typography
4. Layout
5. Elevation & Depth
6. Shapes
7. Components
8. Do's and Don'ts

重點是，不需要所有任務都一直載入。

即使檔名叫 `DESIGN.md` ，也不代表 GitHub Copilot 在所有任務中都會自動讀取。相反地，如果從提示詞或 `AGENTS.md` 明確引用，也能在 GitHub Copilot 中作為 UI 生成或設計判斷的上下文。

設計

因此，應從 `AGENTS.md` 或 Skill 以條件式參照它。

```
# AGENTS.md

- 新增 UI，或修改外觀時，請閱讀 \`DESIGN.md\`。
- 決定顏色、字體、間距、圓角、元件狀態時，請確認 \`DESIGN.md\` 的對應章節。
- 修改 API、DB、認證授權結構時，請確認 ADR 或 \`docs/architecture.md\` 等其他設計文件。
```

這樣有兩個好處：

1. 小變更時，不必每次都讀完整份設計系統
2. 在 UI 或外觀判斷時，可以確實讀到所需的 token 與指引

也就是說，不是單純「不要讀」，而是要 **設計閱讀條件** 。

## 🔹用圖整理三種角色

下圖顯示使用者任務如何以 `AGENTS.md` 為入口，分流到一般任務、Skill、 `DESIGN.md` 等參考資料。

[![以 AGENTS.md 為入口，依任務分流到 Agent Skill 或 DESIGN.md 的資訊設計圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2Fb1b20d71-d0a4-4a9f-a8c6-cbb3d3ca2fca.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7998cf173e8d83024edc40e068d90ef)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2Fb1b20d71-d0a4-4a9f-a8c6-cbb3d3ca2fca.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7998cf173e8d83024edc40e068d90ef)

*常駐規則要保持精簡，專業流程與設計系統只在需要的任務中載入。*

設計

簡單整理如下：

| 資訊 | 角色 | 何時讀取 |
| --- | --- | --- |
| `AGENTS.md` | 常駐規則與參考入口 | 任務入口 |
| `SKILL.md` | 特定作業流程 | 對應作業觸發時 |
| `DESIGN.md` | 設計系統 | 需要做 UI 或外觀判斷時 |
| `*.instructions.md` | 路徑專屬規則 | 處理特定檔案或目錄時 |
| 使用者提示詞 | 這次特定條件 | 執行這次需求時 |

## 🔹不知道該寫到哪裡時的判斷方式

請依序使用以下問題。

### ▸問題 1：幾乎所有任務都需要嗎

如果是，優先考慮放進 `AGENTS.md` 或儲存庫共通的 Copilot instructions。

例如：

- 使用語言
- 測試執行方針
- 不要修改無關檔案
- 不輸出機密資訊

### ▸問題 2：只有特定類型的工作才需要嗎

如果是，優先考慮做成 Agent Skill。

例如：

- 撰寫文章流程
- 部署流程
- 安全性掃描流程
- 發版作業

### ▸問題 3：是詳細背景或設計理由嗎

如果是，依內容分類放置。若是 UI 或外觀判斷，就放 `DESIGN.md` ；若是架構或過去的技術決策，就放 ADR 或 `docs/` 下的設計文件。

設計

### ▸問題 4：只適用於特定路徑嗎

如果是，優先考慮路徑專屬 instructions 或接近目標檔案的 `AGENTS.md` 。

### ▸問題 5：只針對這次需求嗎

如果是，就寫在使用者提示詞或任務文件中。

💡 **配置原則**

不要看檔名來決定，而是根據資訊壽命與適用範圍決定放哪裡。

## 🔷3. MCP 不看連線數，而是設計定義與執行結果

MCP 是讓 AI 應用程式連接外部工具與資料的共通協定。

MCP 伺服器主要公開以下能力：

| 功能 | 內容 |
| --- | --- |
| Tools | 模型可執行的操作 |
| Resources | 檔案、DB schema、文件等上下文 |
| Prompts | 固定訊息或工作流程 |

使用 MCP 後，GitHub、資料庫、公司內部系統、瀏覽器等功能更容易被 AI 使用。

但如果可用的 MCP 伺服器或工具無限制增加，模型需要用來選擇的資訊，以及工具執行結果，可能也會變大。

這裡要思考的不是「要不要接 MCP」，而是 **要讓模型看到什麼、回傳什麼** 。

## 🔹區分 MCP 通訊與模型輸入

MCP server 與 client 之間會進行工具列表取得、資源列表取得、工具執行、資源讀取等通訊。

網路連線

概念上像這樣：

```
MCP 伺服器
  ├─ Tools
  ├─ Resources
  └─ Prompts
        ↓ MCP 通訊
MCP 客戶端 / Agent runtime
        ↓ 選擇必要的定義、本文、結果
LLM 的上下文
```

MCP 伺服器回傳的資訊，client 要傳給模型多少，會依實作而不同。

因此要分成兩件事：

- 透過 MCP 通訊取得的資訊
- 實際傳給 LLM 的資訊

影響 LLM 輸入的代表例子如下：

- tool name
- tool description
- input schema
- prompt 本文
- resource 本文
- tool call 參數
- tool result

## 🔹MCP 中容易變重的三個地方

### ▸1. 大量且龐大的 tool schema

AI 要選工具，就必須理解工具的用途與參數。

例如這樣的定義：

```
{
  "name": "search_issues",
  "description": "Search issues in the selected repository.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "repository": {
        "type": "string"
      },
      "query": {
        "type": "string"
      },
      "limit": {
        "type": "integer",
        "minimum": 1,
        "maximum": 100
      }
    },
    "required": ["repository", "query"]
  }
}
```

如果只有一個工具，通常問題不大；但如果有數十個、數百個類似工具，提示給模型的定義量就會增加。

另外，如果 description 很長、input schema 很複雜、還包含大量範例與限制，即使只有一個工具也會變大。

可行的 client 端做法包括：

網路連線

- 只提示與這次任務相關的 namespace
- 動態挑選必要工具
- 整理相似工具
- 讓 description 變短且具體
- 把用途不同的大型單一工具拆開

但實際可用哪種方式，取決於所使用的 client 或 agent runtime。

### ▸2. 很長的 tool result

在實務上，tool result 往往比 tool schema 更容易變大。

反例：

```
{
  "issues": [
    {
      "number": 1,
      "title": "...",
      "body": "長文內容...",
      "comments": ["所有留言..."],
      "events": ["所有事件..."]
    }
  ]
}
```

如果只是要列表，卻把本文、全部留言與全部事件都回傳，模型要讀的資訊就會增加。

列表型工具只應回傳必要欄位：

```
{
  "issues": [
    {
      "number": 1,
      "title": "登入流程會逾時",
      "state": "open",
      "updatedAt": "2026-06-15"
    }
  ],
  "nextCursor": "..."
}
```

需要詳細內容時，再用另一個工具取回 Issue 本文或留言。

```
search_issues
    ↓ 回傳 20 筆列表
AI 選定目標
    ↓
get_issue
    ↓ 回傳選定 1 筆的詳細資料
```

這種把「列表」與「詳細」分開的設計，不只對 MCP 有效，對一般 API 設計也一樣有效。

設計

### ▸3. 很長的 resource 本文

`resources/list` 取得資源清單，與 `resources/read` 讀取本文，要分開看。

即使清單很短，若本文很大，讀取時仍然會消耗上下文。

例如，不要每次都把 5000 行的 `DESIGN.md` 全文讀進來，可以考慮：

- 先看標題清單
- 用關鍵字搜尋
- 只讀相關章節
- 把文件按功能拆分
- 區分摘要版與詳細版

## 🔹在 MCP 伺服器端控制結果

要讓回傳結果變小，MCP 伺服器端的工具設計也很重要。

像這樣的參數，能讓 client 或 AI 更容易控制輸出量：

```
{
  "name": "search_issues",
  "description": "Search issues and return a limited summary.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "query": {
        "type": "string"
      },
      "limit": {
        "type": "integer",
        "minimum": 1,
        "maximum": 20,
        "default": 10
      },
      "fields": {
        "type": "array",
        "items": {
          "type": "string",
          "enum": ["number", "title", "state", "updatedAt"]
        }
      }
    },
    "required": ["query"]
  }
}
```

實務上常見且好用的控制項有：

- `limit` ：最大筆數
- `cursor` ：分頁
- `fields` ：要回傳哪些欄位
- `top_k` ：只回傳相關度前幾名
- `include_body` ：是否包含本文
- `max_chars` ：最大字數
- `summary` ：回傳摘要而非全文

重點不只是叫 AI「把結果縮短」，而是 **從工具介面本身就能限制** 。

## 🔹有時候 CLI 比 MCP 更合適

MCP 很方便，但不需要把所有處理都做成 MCP 工具。

開放原始碼

特別是當 Skill 或固定提示詞中，使用的工具與執行步驟已完全確定時，固定 CLI 指令反而更簡單。

例如，Skill 中寫了這樣的流程：

1. 取得變更檔案清單
2. 執行目標專案的測試
3. 只抽出失敗的測試名稱

如果要使用的命令是唯一且明確的，就不必讓 AI 從多個 MCP 工具中選擇，直接執行以下命令即可：

```
git diff --name-only
dotnet test --logger "console;verbosity=minimal"
```

固定 CLI 的好處包括：

- 減少 AI 從多個工具中選擇的步驟
- 不必把 MCP 工具的 description 或 schema 全部提示給 AI
- 可以把 Skill 內的執行步驟固定成可重現流程
- 可以直接使用開發者平常就在用的命令
- 人類與 CI 也更容易執行相同命令

可簡化成一句話：

> 如果要讓 AI 選擇要用哪個工具，就用 MCP。若 Skill 已經決定「就用這個命令」，那就可以考慮 CLI。

下面這張圖比較 MCP 與 CLI 的路徑。

[![比較 AI 自行選擇操作的 MCP 路徑，以及從 Skill 執行固定 CLI 命令的路徑的示意圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2F207300e6-54e9-44a6-9df6-7bd460389731.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7fd2fa603b2d89e51c45dc2d84b6290)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2F207300e6-54e9-44a6-9df6-7bd460389731.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7fd2fa603b2d89e51c45dc2d84b6290)

*如果要讓 AI 選擇操作，就用 MCP；若 Skill 已經決定流程，CLI 也可以是候選。兩者都能控制回傳給模型的資訊量。*

開放原始碼

### ▸CLI 輸出太大一樣會很重

這是容易誤解的地方。

使用 CLI 不代表自動省 token。

像這些命令都可能產生大量輸出：

```
git log
dotnet test --verbosity diagnostic
Get-ChildItem -Recurse
kubectl logs deployment/my-app
```

如果把大量輸出原封不動傳給模型，和 MCP 的 tool result 一樣，仍然會吃掉上下文。

CLI 也應該像這樣盡量縮小輸出：

```
# 只列出變更的檔名
git diff --name-only

# 只看最近 20 筆
git log -n 20 --oneline

# 只留下包含錯誤的行
dotnet test --logger "console;verbosity=minimal" |
  Select-String -Pattern 'Failed|Error'

# 將 JSON 限縮到必要欄位
gh issue list --limit 20 --json number,title,state
```

比起 CLI 與 MCP 的差異，最後實際傳給模型的資訊量更重要。

## 🔹適合選 MCP 的場景

以下情況下，MCP 比較合適：

開放原始碼

- 任務中需要 AI 自己從多種操作中做選擇
- 需要用型別化 schema 驗證參數
- 要共用認證、權限、稽核、核准流程
- 處理無法在本地安裝 CLI 的外部服務
- 想讓 IDE、CLI、雲端 agent 都用同一套功能
- 想在伺服器端統一搜尋、分頁、摘要
- 想在不暴露內部 API 實作的情況下公開功能

整理成目的別表格如下：

| 條件 | MCP | CLI |
| --- | --- | --- |
| AI 動態選擇操作 | 適合 | 不適合固定流程 |
| 型別化參數 schema | 適合 | 可作為命令參數管理 |
| 認證 / 權限共用 | 伺服器端較容易實作 | 每個 CLI 可能不同 |
| 重用既有開發命令 | 需要 MCP 化 | 直接可用 |
| 供多個 client 使用 | 適合 | 環境差異較大 |
| 固定的一連串流程 | 可能過度設計 | 適合 |
| 輸出控制 | 透過工具設計控制 | 透過選項或後處理控制 |

不要問「MCP 與 CLI 誰比較好」，而是根據 **是否需要選擇、權限管理、重用性、結果量** 來決定。

## 🔷4. 把快取、壓縮、自動化與品質一起看

處理長指示、工具定義與對話歷史時，常會提到 Prompt Caching。

Prompt Caching 很有用，但不能取代上下文設計。

程式設計

這裡先分成三件事：

1. 上下文設計
2. Prompt Caching
3. 上下文壓縮

## 🔹上下文設計是不要塞入無關資訊

在上下文設計中，最重要的是一開始就不要放入這次判斷不需要的資訊。

例如：

- 這次不會用到的工具不要提示
- 不讀不相干的設計文件
- 搜尋結果限制在前 20 筆
- 不要回傳完整 log，只回傳錯誤附近
- 把列表取得與詳細取得分開

這不只是減少輸入 token，也能減少 AI 判斷上的雜訊。

## 🔹Prompt Caching 是重用相同前綴

Prompt Caching 是指在多個請求之間，重用共通輸入前半段，也就是 prefix 的計算。

概念上如下：

```
第 1 次
[固定 instructions][固定 tools][固定文件][問題 A]
 └──────── 建立快取 ────────┘

第 2 次
[固定 instructions][固定 tools][固定文件][問題 B]
 └──────── 讀取快取 ────────┘
```

如果固定部分相同，後續請求就可以重用。

可能成為快取對象的固定資訊包括：

- system instructions
- 相同的工具定義
- 相同的 Skill 本文
- 相同的長文件
- 對話歷史中的共通部分
- 位置相同的 tool result

但如果工具順序、schema、時間戳、前半段訊息等改變，共通 prefix 可能就會被破壞。

另外，MCP 伺服器儲存結果，和 Prompt Caching 是不同機制。

開放原始碼

```
MCP 伺服器端快取
  → 重用 API 呼叫或 DB 搜尋結果

Prompt Caching
  → 重用傳給模型的共通輸入計算
```

即使某個 MCP 工具回傳了相同內容，如果它沒有被作為共通 prefix 再送進下一次模型輸入，也不一定算是 Prompt Caching 命中。

## 🔹上下文壓縮是把長對話摘要化

上下文壓縮是把變長的對話歷史，替換成較短摘要的想法。

例如有以下歷史：

```
User: 說明需求
Assistant: 調查結果
Tool: 檔案列表
Assistant: 設計方案
User: 修改需求
Tool: 測試結果
Assistant: 修正版
```

可壓縮成這樣的摘要：

```
目前為止的決定：
- 不修改公開 API
- 只修改 OrderService
- 已新增 3 個測試

未解決：
- timeout 值尚未決定

下一步：
- 確認設定值並完成實作
```

這樣即使不保留全部舊對話，也能延續重要決策。

不過，壓縮時有可能遺失細節。之後可能還需要的原文，最好另外保存在檔案或外部紀錄中。

## 🔹用圖比較三者差異

下圖呈現設計、快取與壓縮的差異。

設計

[![以三欄比較上下文設計、Prompt Caching 與上下文壓縮的處理與目的之示意圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2Fab623d71-6da3-4370-8ced-9fdc81221b4b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b6ac4e5a42d9723e6b0065442d6be5cd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F755626%2Fab623d71-6da3-4370-8ced-9fdc81221b4b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b6ac4e5a42d9723e6b0065442d6be5cd)

*上下文削減、快取、壓縮是不同對策，解決的問題也不同。*

實作順序可以這樣想：

1. 不放不必要的資訊
2. 快取需要但反覆出現的固定資訊
3. 壓縮變長的對話歷史

不要一開始就塞入大量不必要的資訊，然後覺得「反正可以快取就沒問題」。

即使計費變便宜了，AI 讀到不必要資訊後仍會產生判斷問題。

## 🔹用 Claude API 確認快取

在 Claude API 中，可以透過 usage 資訊確認 Prompt Caching 的使用狀況。

程式設計

常見值如下：

```
{
  "usage": {
    "cache_creation_input_tokens": 5000,
    "cache_read_input_tokens": 0,
    "input_tokens": 80
  }
}
```

如果 `cache_creation_input_tokens` 增加，表示有輸入被寫入快取。

後續請求若出現 `cache_read_input_tokens` 增加，表示實際從快取讀取了資料：

```
{
  "usage": {
    "cache_creation_input_tokens": 0,
    "cache_read_input_tokens": 5000,
    "input_tokens": 60
  }
}
```

也就是說，確認時可以這樣理解：

- cache creation：為下次預先建立快取
- cache read：這次請求實際使用了快取

詳細內容請參考 [Anthropic 的 Prompt Caching 官方文件](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 。

## 🔹用 OpenAI API 確認快取

在 OpenAI API 中，針對較長的 prompt，Prompt Caching 會自動套用。

可透過回應中的 usage 裡的 `cached_tokens` 查看使用狀況。

```
{
  "usage": {
    "prompt_tokens": 4200,
    "prompt_tokens_details": {
      "cached_tokens": 3072
    }
  }
}
```

把固定的 instructions、工具定義等共通資訊放在前面，把每次不同的使用者輸入放在後面，較容易形成共通 prefix。

詳細內容請參考 [OpenAI 的 Prompt Caching 官方文件](https://developers.openai.com/api/docs/guides/prompt-caching) 。

## 🔹不要混淆 GitHub Copilot 與底層 API

GitHub Copilot、Claude API、OpenAI API 即使使用到相同模型，使用者能操作的設定也不完全一樣。

像 Claude API 的 `cache_control` 這類設定，並不表示你可以直接把同樣 JSON 寫進一般的 GitHub Copilot prompt 就生效。

程式設計

在 GitHub Copilot 中，是由 Copilot 服務端或所使用的模型側來管理快取。

因此要分開說明：

| 使用方式 | 快取處理 |
| --- | --- |
| 直接使用 Claude API | 依 API 規格設定並查看 usage |
| 直接使用 OpenAI API | 查看自動快取與 usage |
| 使用 GitHub Copilot | 依 Copilot 的功能、顯示與計費規格 |

「Claude 可以這樣設定」不代表「Copilot 也能指定同樣的 JSON」。

※產品規格、支援模型、價格與快取保留時間都可能變動。實作時請確認各服務最新的官方文件。

## 🔷5. 模型選擇要依任務難度與風險來決定

在上下文設計裡，不只有「讓它讀什麼」很重要， **讓哪個模型來讀** 也很重要。

即使提示詞、檔案、工具結果都相同，模型不同，仍可能有這些差異：

- 回應速度
- 推理深度
- 對長上下文的能力
- 幻覺多少
- 程式修改穩定性
- 每個輸入 / 輸出 token 的 AI Credits

在 GitHub Copilot 中，Chat 有時可以選模型。可用模型會依方案、組織政策、使用位置（例如 IDE 或 [GitHub.com](http://github.com/) ）而不同。

因此，比起固定「永遠都用某個模型」，更實務的做法是依任務類型選擇。

GitHub Docs 也說明了不同模型各有強項，AI Credits 的消耗也會依模型 token 價格而異。最新支援狀況請參考 [GitHub Copilot 的模型比較](https://docs.github.com/en/copilot/reference/ai-models/model-comparison) 與 [更換模型的步驟](https://docs.github.com/en/copilot/how-tos/use-ai-models/change-the-chat-model) 。

開放原始碼

## 🔹先以 Auto 為基準

初學者可以先以 Auto 為基準。

在支援的環境中，Auto 會根據可用性等條件選擇模型。當你還不確定為什麼需要手動選模型時，使用 Auto 可以減少糾結在模型選擇上的時間。

不過，把選擇交給 Auto，與放棄設計是兩回事。

以下情況值得手動選擇模型：

- 需要重要的設計判斷
- 要深入調查不明原因的 bug
- 要規劃跨多個檔案的修改
- 涉及安全性或認證授權
- 產出品質多次不穩定
- 想優先處理大量輕量工作並顧及成本或速度

做法上可以是：平常預設 Auto，需要時再明確切換。

## 🔹模型可從三個層次思考

模型名稱會變。新模型會加入，preview 會結束，組織政策可用的模型也會改變。

因此，在文章或團隊規則中，比起只寫特定模型名稱，更適合用以下角色來思考：

| 層級 | 適合的工作 | 注意事項 |
| --- | --- | --- |
| 高推理模型 | 設計、規劃、困難除錯、審查、安全性判斷 | 品質高，但成本與等待時間通常較高 |
| 標準模型 | 一般實作、補測試、跨檔案修改、文件更新 | 猶豫時的中心選項 |
| 輕量模型 | 小型重構、格式轉換、摘要、簡單文案修改 | 不要把複雜設計判斷全丟給它 |

例如可這樣分工：

```
調查・規劃：高推理模型或 Auto
一般實作：標準模型或 Auto
簡單修正：輕量模型
審查：高推理模型
大量定型處理：輕量模型 + 測試 / linter
```

重點不是「要用貴模型還是便宜模型」。

而是要 **選擇具備足夠分析能力的模型，再用上下文、測試與人工審核補足品質** 。

## 🔹在用高效能模型前，先檢查資訊怎麼交付

遇到不順利時，很容易馬上切到更高階模型。

但失敗原因有時不是模型能力，而是上下文設計。

- 沒有讓它讀到必要的設計文件
- 一次丟了太多無關 log
- 任務目標不清楚
- 沒有指定變更檔案
- 沒有寫清楚測試條件
- 還留著舊對話歷史

這種情況下切換到高效能模型，確實有時會改善。

但那可能只是「模型夠強，勉強撐住了雜亂輸入」。若要讓運用可重現，還是得先整理輸入。

建議順序如下：

1. 用 `/context` 或 `/usage` 確認哪裡膨脹了
2. 減少無關檔案、log 與工具結果
3. 補上必要的設計文件與限制
4. 把任務拆成調查、規劃、實作、審查
5. 只有真的困難的判斷，再交給高推理模型

高效能模型不是拿來處理大量不必要資訊的，而是拿來專注處理困難判斷的。

## 🔹Chat 與 Inline suggestion 的模型要分開看

還有一個常被混淆的地方。

GitHub Copilot Chat 選用的模型，和編輯器中的 Inline suggestion 使用的模型，不一定是同一設定。

GitHub Docs 也說明，Chat 的模型變更不會影響 Inline suggestion 的模型，且 Inline suggestion 端還會受到可用替代模型與擴充功能版本的條件限制。

因此要分開思考：

| 場景 | 主要用途 | 看模型選擇的方式 |
| --- | --- | --- |
| Copilot Chat / Agent mode | 調查、說明、規劃、多檔案修改 | 依任務難度與審查風險選擇 |
| Inline suggestion | 輸入中程式碼補全 | 依速度、手感與可用模型選擇 |
| Cloud agent / CLI | 非同步工作、長時間作業、工具執行 | 同時考慮成本、執行時間與審核邊界 |

所以，「Chat 用高推理模型，但補全用輕快模型」這樣的搭配是可行的。

## 🔹模型選擇也能寫進 AGENTS.md 或 Skill

如果團隊要運作模型選擇規則，與其每次口頭交代，不如寫進 `AGENTS.md` 或 Skill。

例如：

```
# Model selection

- 一般實作與補測試，使用 Auto 或標準模型。
- 架構變更、認證授權、安全性判斷時，考慮使用高推理模型。
- 單純文案修改、格式轉換、輕微重構時，優先使用輕量模型。
- 如果切換模型，請在工作備註中留下原因。
```

這裡同樣不建議只固定某個模型名稱。

組織可用的模型、base model、LTS model、費用與政策都可能變動。重點不是名稱，而是 **針對哪種判斷使用哪種推理能力** 。

💡 **模型選擇的結論**

猶豫時先以 Auto 為基準；在困難的設計、除錯、審查時，考慮高推理模型。輕量的定型工作則偏向輕量模型或 CLI，品質則由測試與審查支撐。

## 🔷6. 用 CDQR 判斷自動化

前面主要是在講 token 與上下文。

但實務上真正要最佳化的，不只是 token。

AI 活用時，要一起考量以下四件事：

- Cost：成本
- Delivery：速度
- Quality：品質
- Review：人工確認

本文將其整理為 **CDQR** 。

## 🔹Cost：減少不必要的輸入與輸出

Cost 不是只看提示詞字數，而是看整個處理流程。

- 常駐指示有沒有變得過大
- 有沒有每次都提示用不到的工具定義
- 搜尋結果是不是全件回傳
- log 有沒有無限制輸出
- 同一個檔案是不是重複讀取
- 長 session 是否一直放著不整理
- 反覆重做是不是因為說明不夠
- 輕量工作是不是一直用高推理模型

有時候，稍微增加適當的 instructions，反而能減少重做次數，讓總 token 降低。

Cost 改善不只是「讓輸入更短」，也包括「減少來回次數」與「切換到適合任務的模型」。

## 🔹Delivery：交給 AI 真的會更快嗎

AI 自動化很適合以下工作：

- 在 repository 內探索
- 列出可能的修改項目
- 產生定型程式碼
- 進行格式轉換
- 執行測試
- 摘要差異
- 撰寫文件草稿

但有時候，把作業流程固定成 Skill 或 CLI，反而比每次都讓 AI 思考流程更快。

例如，如果每次都用同一個測試指令，那就不需要讓 AI 從一堆工具裡找測試工具。

```
# SKILL.md

驗證時依下列順序執行：

1. \`dotnet format --verify-no-changes\`
2. \`dotnet test --logger "console;verbosity=minimal"\`
3. 若失敗，只回報錯誤周邊內容
```

把不需要判斷的部分固定起來，讓 AI 專注在需要判斷的部分，效率會更高。

## 🔹Quality：交付必要的設計意圖與驗證條件

為了減少 token 而不讓它讀設計文件，短期內輸入可能變少。

但可能會造成這些品質問題：

- 違反現有架構
- 破壞公開 API
- 跨越安全邊界
- 損害資料一致性
- 違反測試方針
- 重複實作相似功能

要維持品質，就不能把選擇變成「要嘛全部讀，要嘛都不讀」。

應在 `AGENTS.md` 中寫清楚何時要讀設計文件：

```
- 變更公開 API 時，請閱讀 \`docs/api-design.md\`。
- 變更認證授權時，請確認 \`docs/security/\`。
- 變更 DB 時，請確認 migration 與 rollback 流程。
```

只在必要任務中加入適當資訊，就能兼顧上下文量與品質。

## 🔹Review：保留人類應該批准的邊界

即使 AI agent 可以自主作業，也不代表所有事情都應該無條件批准。

應保留人類審核的代表情況包括：

- 公開 API 的破壞性變更
- 資料刪除
- 上線部署
- 認證 / 授權變更
- 機密資訊或個資處理
- 架構變更
- 對成本有重大影響的變更
- 需求優先順序調整

AI 可以協助調查候選方案、產生差異、執行測試、整理審查材料。

但「是否接受這個風險」以及「是否正式採納這個規格」，仍然是組織或負責人的判斷。

### ▸適合交給 AI 的部分

- 找出需要的檔案
- 說明既有實作
- 提出多個修改方案
- 新增測試
- 執行靜態分析
- 列出影響範圍
- 撰寫 Pull Request 說明

### ▸要保留人工批准的部分

- 最終方案決定
- 正式上線
- 資料破壞操作
- 安全性例外
- 預算與時程的優先順序
- 法務與合規判斷

## 🔹CDQR 判斷表

| 軸 | 要確認的問題 |
| --- | --- |
| Cost | 是否有不必要的輸入、輸出或重試 |
| Delivery | 交給 AI 後，整體作業是否真的更快 |
| Quality | 是否在必要範圍內讓它讀到設計意圖、限制與驗證條件 |
| Review | 是否保留人類應該批准的邊界 |

模型選擇也可以放進這張表裡一起判斷。

- Cost：是否過度使用高推理模型處理輕量工作
- Delivery：是否在需要速度的工作上選了過重模型
- Quality：是否用輕量模型硬做複雜設計判斷
- Review：是否因為模型變強就省略了人工審核

token 最佳化並不只是改善 CDQR 裡的 Cost。

透過上下文設計，也能一起改善 Delivery、Quality、Review。

## 🔷7. 實作範例：為 repository 設計上下文

接著把以上內容套用到一個有管理介面的假想 Web API 專案。

## 🔹專案結構

```
shop-api/
├─ AGENTS.md
├─ DESIGN.md
├─ README.md
├─ src/
│  ├─ Api/
│  ├─ Application/
│  └─ Infrastructure/
├─ tests/
├─ docs/
│  ├─ architecture.md

---

原文出處：https://qiita.com/ochtum/items/d442ed23d24245b789a0
```

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3V2n43) [22瀏覽](https://codelove.tw/@tony/post/3V2n43)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3V2n43)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385984&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2n43&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385984426&amp;bpp=1&amp;bdt=557&amp;idt=70&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3296781613571&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395661%2C95393485%2C95395500&amp;oid=2&amp;pvsid=4504386202289458&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=91" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

設計