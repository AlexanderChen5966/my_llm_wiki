---
title: "Claude Code 社群工作流大整理：跟著 Boris 用一年後，我發現大家最在意的根本不是 sub-agent"
source: "https://hackmd.io/@BASHCAT/BJBh08pgGe"
author:
published:
created: 2026-06-04
description: "整理 2025-2026 社群 Claude Code 工作流共識：Plan-First、客製化六件套、Worktrees 工廠模式、Sub-agents 雙面刃、TDD/SDD，附給硬體韌體工程師的 starter pack。"
tags:
  - "clippings"
status: processed
---
去年這個時候，我刷 Twitter 看到一張截圖：某個 power user 同時開著 17 個 Claude Code sub-agent，每個跑在自己的 git worktree 上，主視窗只負責當「總指揮」。當下我整個傻眼——這真的是日常嗎？還是表演？

後來我認真把社群這一年累積的最佳實踐挖了一遍，包含 Anthropic 官方部落格、Claude Code 創造者 Boris Cherny 本人的 13 步設定、Boris Tane、Shrivu Shankar、Simon Willison 這幾位 power user 的長文，加上 Reddit r/ClaudeAI / r/ClaudeCode 的高讚討論，還有一篇 arxiv 學術解析。整理完發現一件夭壽好笑的事： **Boris Cherny 自己根本不太用 worktree，也不太用 custom sub-agent** ——他用的設定意外地「乾淨」。

社群這一年的真共識在哪？哪些是過度工程？硬體 / 韌體背景的工程師（也就是我）該挑哪幾招用？這篇就是把那份深度研究濃縮成一篇能讓你「明天就改 workflow」的指南。

---

## 全社群唯一共識：Plan-First

跨派系唯一沒人吵的原則： **在 Claude 動手寫 code 之前，先讓它寫出一份你能讀懂、能修改、能 review 的 plan。**

Boris Cherny 在 [他自己的 13 步設定](https://www.reddit.com/r/ClaudeAI/comments/1q2c0ne/) 裡這樣寫：「Most sessions start in Plan mode (shift+tab twice). If my goal is to write a Pull Request, I will use Plan mode, and go back and forth with Claude until I like its plan. From there, I switch into auto-accept edits mode and Claude can usually 1-shot it.」

Boris Tane 在 [How I Use Claude Code](https://boristane.com/blog/how-i-use-claude-code) 把流程明確化成一條 pipeline，用了快 9 個月：

```
repeat 1-6xResearchPlanAnnotateTodo ListImplementFeedback / Iterate
```

他下了一句話我覺得寫得很狠：「The single most important thing I do is the separation of planning and execution. It prevents wasted effort, keeps me in control of architecture decisions.」

實務上 Plan mode 用法很簡單——session 開起來就按 `Shift+Tab` 兩次，模型會切到 read-only，只能讀檔不能寫檔。你跟它討論 1 到 6 輪，覺得 plan 夠了就再按 `Shift+Tab` 切換到 auto-accept edits，剩下交給它。

這條原則跨派系成立：極簡派、平行派、SDD 派全都同意。差別只在 plan 是「session-bound」（plan mode）還是「持久化」（spec-driven development）。但 plan-first 這個動作本身，沒人吵。

---

## 客製化六件套：搞清楚誰負責什麼

社群這一年累積的客製化機制收斂到六件套。據 [codewithmukesh 的 Skills 整理](https://codewithmukesh.com/blog/skills-claude-code) 與 [Genaiunplugged 的合併公告](https://genaiunplugged.substack.com/p/claude-code-skills-commands-hooks-agents) ，`.claude/commands/` 已逐步被併入 Skills 系統（舊路徑仍 backward-compatible），這六件套基本就是現在的標準架構。

| 機制 | 路徑 | 性質 | 由誰觸發 |
| --- | --- | --- | --- |
| **[CLAUDE.md](http://claude.md/)** | repo 根 / `~/.claude/` | always-on 脈絡（advisory） | 系統自動 |
| **Slash Commands** | `.claude/commands/*.md` | 你打 `/cmd` 立刻跑 | 使用者 |
| **Skills** | `.claude/skills/<name>/SKILL.md` | 可帶 scripts、templates，被 auto-discover | 模型自動 or 使用者 |
| **Sub-agents** | `.claude/agents/*.md` | 獨立 context、權限、tools | 主 agent spawn |
| **Hooks** | `.claude/settings.json` | **強制** 、deterministic、shell 級 | 系統在 lifecycle 事件 fire |
| **MCP Servers** | `.mcp.json` | 外部系統整合 | 模型呼叫 |

這張表背後有兩條心法，搞懂了你以後挑工具就不會選錯：

**第一條： [CLAUDE.md](http://claude.md/) 是 advisory，hooks 才是 mandatory。** [Genaiunplugged 寫得很到位](https://genaiunplugged.substack.com/p/claude-code-skills-commands-hooks-agents) ：「 [CLAUDE.md](http://claude.md/) 是『should-do』；hooks 是『must-do』。」如果你要求「commit 前必跑測試」，寫進 [CLAUDE.md](http://claude.md/) 是請求，寫成 `PreToolUse` hook 才是命令。Claude 可以「忘記」 [CLAUDE.md](http://claude.md/) 的規矩，但 hook 是 shell 層攔截，模型躲不掉。

**第二條：Action 用 skill，Instruction 用 slash command。** [MindStudio 的判別法](https://www.mindstudio.ai/blog/claude-code-skills-vs-slash-commands-2) 很簡單——這件事是要 Claude「做某個動作」（呼叫 API、跑測試、查資料）還是「按某種方式想」（換個角度 review、用特定格式產文件）？前者是 skill，後者是 slash command。

另外 [CLAUDE.md](http://claude.md/) 不要養成肥豬， [社群多次提醒](https://genaiunplugged.substack.com/p/claude-code-skills-commands-hooks-agents) 建議 **< 200 行** ，列 `file:line` 指標而不是把程式碼整段貼進去——程式碼會 stale， [CLAUDE.md](http://claude.md/) 跟著 stale，模型就被你誤導了。

---

## Worktrees 工廠模式：什麼時候值得開？

2025 年 5 月之後，「parallel coding agents」成了顯學。社群整理出多種 wrapper（Conductor、Termdock、Vibe Kanban）， [Claude Code 官方 common workflows 文件](https://code.claude.com/docs/en/common-workflows) 也把 worktrees 列為支援的工作模式之一。 [dgalarza/claude-code-workflows](https://github.com/dgalarza/claude-code-workflows/blob/main/tips/worktrees-for-parallel-agents.md) 等社群典藏專案則記錄了 `claude --worktree` 一類的常見用法與 `.worktreeinclude` 自動 symlink 設定。

[Cole Medin 的影片](https://www.youtube.com/watch?v=rFGlJ4oIlhw) 講得很有畫面：「I often have 3-10 Claude Code agents running in parallel on isolated worktrees. What makes it work is: planning, implementing, and validating in separate sessions, fresh-context reviews catching the bugs the implementer missed, and adopting a factory mindset, not vibe coding.」

Termdock 整理出的 [典型流程](https://termdock.com/en/blog/parallel-dev-3-agents-3-worktrees-termdock) 大致長這樣：

```
主 agent / 切 issueworktree A: 規格化 → 實作worktree B: 規格化 → 實作worktree C: 規格化 → 實作Fresh-context Reviewer按依賴順序合併
```

關鍵巧思：`.worktreeinclude` 檔可以把 `.env` 、 `config/master.key` 這類 git-ignored 但必要的檔案自動 symlink 到新 worktree——不然 `npm run dev` 一跑就噴錯。

**但這套真的適合所有人嗎？** 反直覺的事實來了： **Claude Code 創造者 Boris Cherny 本人不用 worktree** 。 [InfoQ 訪談](https://www.infoq.com/news/2026/01/claude-code-creator-workflow) 寫得很清楚——他跑 5 個本機 session + 5 到 10 個雲端 session，每個用獨立的 `git checkout` ，並坦承約有 10–20% 的雲端 session 會因為意外情境被放棄（據訪談原文）。

我自己看法是這樣：worktree 是真的好工具，但 **小任務開 worktree 是 over-engineering** 。 [Termdock 那篇](https://termdock.com/en/blog/parallel-dev-3-agents-3-worktrees-termdock) 給的門檻很實用——「每個 feature 至少要 20 分鐘以上，merge overhead 才值得」。如果你只是改一個 bug 或一個檔案，worktree 的設定成本根本回不來。

---

## Sub-agents 是雙面刃，社群正在分裂

Sub-agents 表面上看很香——把專業任務（測試、文件、安全 review）切出去，主 context 保持乾淨。Anthropic 官方也把它列為 5 大 workflow primitive 之一。

但 Shrivu Shankar 在 [How I Use Every Claude Code Feature](https://blog.sshh.io/p/how-i-use-every-claude-code-feature) 提出了一個我覺得非常犀利的反論：

> Custom subagents create two new problems: 1. They Gatekeep Context: If I make a PythonTests subagent, I've now hidden all testing context from my main agent. It can no longer reason holistically about a change. 2. They Force Human Workflows: Worse, they force Claude into a rigid, human-defined workflow.

他的替代方案叫「Master-Clone 架構」——把所有 context 塞 [CLAUDE.md](http://claude.md/) ，讓主 agent 自己決定何時用內建 `Task(...)` clone 自己。這跟 [Cole Medin 那種高度結構化的 sub-agent dream team](https://www.youtube.com/watch?v=HJ9VvIG3Rps) 形成正面對撞。

連 Boris Cherny 自己也滿節制——只用 `code-simplifier` （簡化已完成的程式碼）跟 `verify-app` （跑端到端驗證）這幾個。他的邏輯是：sub-agent 應該自動化「我每個 PR 都會做的」事，不是用來顯擺多 agent 編排。

學術論文 [Dive into Claude Code](https://arxiv.org/html/2604.14228v1) 順便丟一個有意思的數據：Claude Code 整個 codebase 大概只有 **1.6% 是 AI 決策邏輯** ，剩下 98.4% 全是 operational infrastructure。這個比例其實在暗示，「scaffolding 比模型本身重要」——你怎麼包模型，比你選什麼模型還關鍵。

我個人立場偏 Shrivu： **先把 [CLAUDE.md](http://claude.md/) 寫好，再考慮要不要做 sub-agent** 。先確定一個工作流你會做 10 次以上，再去打包成 sub-agent；不然就是過早抽象。

---

## TDD 與 SDD：兩個容易被忽略的細節

### TDD 要明示再明示

Claude 預設行為是先寫 implementation 再補測試。要做 TDD 必須 **顯式 prompt** 。 [Daniel Weinmann 的雙階段 prompt](https://www.linkedin.com/posts/danielweinmann_brad-w-asked-me-how-i-make-my-coding-agents-activity-7356066470478471168-Jfwc) 已經是社群參考：

```
（第一階段）照常用 plan mode 討論到滿意

（第二階段，加碼）
The plan is great! Please add instructions to use TDD
and go through the red-green-refactor cycle in baby steps.
```

最雷的失敗模式來自 [augmentcode.com](https://www.augmentcode.com/guides/claude-code-spec-driven-development) 整理的 **test masking** ：Claude 為了讓測試過，會直接改測試，甚至在 Playwright E2E test 裡面 runtime 注入 JavaScript「修好」瀏覽器 bug 讓測試過關，但 production bug 還在。即使 [CLAUDE.md](http://claude.md/) 寫了 `DO NOT CHANGE THE CODE` 也會被忽略，乾。

防呆的做法：用 sub-agent 分工（一個寫測試、一個寫實作），或加 hook 強制每次 edit 後跑測試套件，讓「測試不可改」這件事走 shell 層強制。

### SDD 是 2026 顯學，但不是萬靈丹

「Vibe coding」失靈引發 spec-driven development 反潮。 [thebcms.com 的整理](https://thebcms.com/blog/spec-driven-development) 指出 2026 年每個主流 AI 工具都有自己的 SDD 變體：GitHub Spec Kit、AWS Kiro、Claude Code 的 `cc-sdd` skills、OpenSpec、BMAD、Tessl。

Claude Code 的 canonical SDD 流程：

```
/constitution    # 寫 memory/constitution.md（不可逾越的原則）
/specify         # 寫 spec
/plan            # 從 spec 生 plan
/tasks           # 拆 phase
implement        # 對著 plan 實作
validate         # 驗證實作對應原 spec
```

Reddit 社群態度兩極化。 [一篇高讚討論](https://www.reddit.com/r/ClaudeCode/comments/1rus6zl/specdriven_development_sdd_frameworks_vs_ai_plan) 直接問：「對小專案是不是 pure ceremony？」

我自己的判讀：SDD 是 plan mode 的 persistent 版本。 **plan 寫一次就丟 vs spec 寫進 git** 的差別，到了大專案、多人協作、多 session 場景才會浮現。單檔修 bug 不需要 SDD，跨服務重構就值得。

---

## 反模式：10 條社群血淚

整理深度研究時，我把社群挖出來的反模式列了一份清單。這比正向最佳實踐還有用，因為錯誤是真的會痛——下面前五條多半有具體來源支撐，後五條則是綜合社群共識與我自己踩坑後的觀察，僅供你避雷時參考：

1. **[CLAUDE.md](http://claude.md/) 肥大化** ：超過 500 行反而降品質。改用 `@docs/...` 引用、 `file:line` 指 code
2. **Custom sub-agent 過度設計** ：強迫主 agent 走 rigid 委派，失去整體推理（Shrivu Shankar）
3. **Test masking** ：Claude 重寫測試或 runtime 注入騙過 E2E
4. **Context exhaustion** ：長 session 不收歛。多用 `/clear` 、 `/compact` 、 `/rewind` 、checkpoint
5. **過早 worktree** ：< 20 分鐘任務的 merge overhead 不值得
6. **過度仰賴第三方包裝** ：Anthropic 持續把社群工具吃進核心（compaction、checkpoint、worktree 都已內建）。買單外掛要看「會不會被官方原生取代」
7. **Habituated approval 風險** ： `--dangerously-skip-permissions` 或養成秒按 yes 的習慣會放過真實風險
8. **Token 燒過頭** ：Boris 風格「百萬級 token」對 Pro 用戶不現實。先確認你的方案吃得起
9. **Mid-edit 阻擋的 hooks** ：在 Edit/Write 期間 block 會打斷 Claude 推理鏈；應在 submit time 才檢查
10. **「17 parallel sub-agent」式表演** ：Boris Cherny 那篇 13 步設定下面的留言區，有不少老用戶直接諷刺「17 個 sub-agent + vibe-coded memory add-on」對普通開發者完全不適用——值得反思你做的工作流到底是給自己用還是給推特看

其中第 6 條我覺得最值得放心上——這一年 Claude Code 已經發了 [176 個版本](https://dev.to/oikon/reflections-of-claude-code-from-changelog-833) ，社群常用的工作流被官方收編的速度比想像快。你買單第三方時，最好評估它的可替代性。

---

## 給硬體 / 韌體工程師的 Starter Pack

社群絕大多數案例都是 Web 開發，硬體 / 韌體工程師看完容易覺得「這跟我有關嗎」。我自己整理出一份最小可行配置，三件式：

**1\. Plan mode + 一份精簡的 [CLAUDE.md](http://claude.md/) （必裝）**

[CLAUDE.md](http://claude.md/) 寫專案脈絡、工具鏈版本（Nordic SDK、Zephyr、KiCAD、編譯器、目標 MCU），列你跟團隊已經吵完的命名 / git / commit 規約。控制在 200 行內。Plan mode 直接讓 `Shift+Tab` 二連發變成肌肉記憶。

**2\. PostToolUse hook 跑 lint / format / 二進位檢查（次裝）**

韌體最常見的痛是「commit 前忘記跑 clang-format 或 binary size check」。把這幾條綁 hook，不要寫進 [CLAUDE.md](http://claude.md/) 。範例：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {"type": "command", "command": "clang-format -i \"$CLAUDE_FILE\""}
        ]
      }
    ]
  }
}
```

**3\. MCP 把你的儀器和工具收編（進階）**

如果你已經在用 BLE sniffer、Wireshark、邏輯分析儀，把它們包成 MCP server 放進 `.mcp.json` 。Claude 可以直接呼叫 `ble_quickscan` 、 `pcap_summary` 、 `dissect_packets` ——抓封包、分析、丟結論一條龍。我自己已經把 `btle-cli` 、 `cynthion` 、 `dsview` 、 `ubertooth` 都掛上去，這是整套配置裡 ROI 最高的一塊。

**先不要做的** ：

- 不要一開始就上 worktree（韌體專案通常一次只開一個分支，merge overhead 不值得）
- 不要急著寫 custom sub-agent（先確定哪個工作流你會跑 10 次以上）
- 不要照搬 Web 那套並行 5 個 agent 的設定——你的編譯時間和燒錄時間不是 Web 級的

---

## 結語：別當別人工作流的拼裝粉

寫完這份整理，我自己最大的收穫不是「該裝什麼」，反而是 **該卸什麼** 。

過去半年我跟風在 `~/.claude/` 底下堆過 30 多個 skill、12 個 sub-agent、4 個 MCP——大部分跑沒幾次就吃灰，剩兩件每週都會打開： `/commit-push-pr` 跟 `verify-fw-binary` 。那段時間我以為自己在「精進工作流」，事實是被推特上的 power user 截圖綁架，做了一堆給別人看的工程。Boris Cherny 是 Anthropic 員工，用 Opus 不心疼 token；Cole Medin 是 YouTuber，並行 5–10 個 agent 拍起來才有畫面；Shrivu Shankar 偏好極簡主義；我是韌體工程師，每天最痛是燒錄等待和 trace 分析——這四個人「最佳」的工作流根本不會長得一樣。

社群這一年累積的真共識其實就一條： **Plan-First** 。其他六件套、worktree、sub-agent、SDD、TDD 全部都是工具櫃裡可選的扳手，不是必裝套件。先把 plan mode 跟 200 行的 [CLAUDE.md](http://claude.md/) 跑熟，每加一件新東西都先問自己「過去兩週這個我做過幾次」——少於 5 次的，先不要做 skill。

下一步我自己要做的事很具體：把現在堆在 `~/.claude/skills/` 的灰塵 skill 砍掉一半，剩下的整合到一個專案模板，目標是「拿到一片 EVB，從 schematic 到 firmware bring-up」這條路能跑進兩小時內。有結果再寫一篇對照。

你呢？你最後留在工作流裡的是哪幾招？哪幾招砍了反而更順？

---

## 延伸閱讀 / 參考資料

- Anthropic Engineering — [Claude Code: Best practices for agentic coding](https://www.anthropic.com/engineering/claude-code-best-practices)
- Anthropic 內部 PDF — [How Anthropic teams use Claude Code](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf)
- Boris Cherny via Reddit — [Claude Code creator Boris shares his 13 steps](https://www.reddit.com/r/ClaudeAI/comments/1q2c0ne/)
- InfoQ — [Inside the Development Workflow of Claude Code's Creator](https://www.infoq.com/news/2026/01/claude-code-creator-workflow)
- Boris Tane — [How I Use Claude Code](https://boristane.com/blog/how-i-use-claude-code)
- Shrivu Shankar — [How I Use Every Claude Code Feature](https://blog.sshh.io/p/how-i-use-every-claude-code-feature)
- Simon Willison — [2025: The year in LLMs](https://simonwillison.net/2025/Dec/31/the-year-in-llms)
- [builder.io](http://builder.io/) — [Claude Code Subagents](https://www.builder.io/blog/claude-code-subagents)
- [augmentcode.com](http://augmentcode.com/) — [Claude Code for Spec-Driven Development](https://www.augmentcode.com/guides/claude-code-spec-driven-development)
- [thebcms.com](http://thebcms.com/) — [Spec-Driven Development (SDD): The Definitive 2026 Guide](https://thebcms.com/blog/spec-driven-development)
- arxiv 2604.14228 — [Dive into Claude Code: The Design Space](https://arxiv.org/html/2604.14228v1)
- 我自己的深度研究報告（vault 內部，未公開）