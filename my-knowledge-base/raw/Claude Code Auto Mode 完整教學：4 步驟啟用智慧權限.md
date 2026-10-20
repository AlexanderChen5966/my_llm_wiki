---
title: "Claude Code Auto Mode 完整教學：4 步驟啟用智慧權限"
source: "https://moksaweb.com/claude-code-auto-mode/"
author:
  - "[[Moksa]]"
published: 2026-05-04
created: 2026-07-20
description: "Claude Code Auto Mode 智慧權限模式完整教學：Classifier 邏輯、Boundary 機制、Fallback 處理、和其他 Permission Mode 差異。"
tags:
  - "clippings"
status: processed
---
Claude Code Auto Mode 是 Anthropic 在 Claude Code v2.1.83 推出的智慧權限模式，用獨立的 Classifier Model 在背景評估每個動作再執行，把預設模式裡每個動作都要 Confirm 的疲勞和 bypassPermissions 完全跳過所有檢查的危險之間找到平衡點。Claude Code Auto Mode 讓你長時間 Task 不被權限提示打斷，同時透過 Classifier 攔截危險操作例如 Production Deploy、Force Push、Mass Delete 等高風險動作。它是 Research Preview 階段的功能，需要 Max Team Enterprise 或 API Plan 加上 Sonnet 4.6、Opus 4.6、Opus 4.7 模型才能用。這篇完整教學會拆解啟用方式、Classifier 邏輯、Default Allow 和 Block 規則、Boundary 機制、Fallback 處理、和其他 Permission Mode 的差異。中文圈完全沒有 Claude Code Auto Mode 的深度教學，這篇是搶先採用者的 Reference Manual。

> **Moksa 觀點：** 我用 Claude Code 一年多最大的痛點就是 Permission Prompt 太多。長 Task 跑一個小時可能要按 50 次 Approve、每次都讓我從思考切回操作、Productivity 大降。Claude Code Auto Mode 第一次真正解決這個問題——背後 Classifier 自動處理 95% 的安全動作、剩下 5% 危險動作擋下來給我看。我跑 Refactor、Migration、Deploy Verification 這些長任務都開 Claude Code Auto Mode、體驗順暢度提升 5 倍以上。最讓我驚訝的是 Boundary 機制——我在對話中說「不要 Push」、Classifier 會記住直到我 Lift 這個限制，比寫死 Deny Rule 彈性多了。Claude Code Auto Mode 是 2026 年我認為改變日常 Workflow 的關鍵 Permission Feature、值得 Max 訂閱者立刻啟用。

## 1\. 什麼是 Claude Code Auto Mode？

這個功能是介於 Default Mode 和 bypassPermissions 之間的 Smart Permission Mode。Default Mode 每個動作都要你 Approve、bypassPermissions 跳過所有檢查、Claude Code Auto Mode 讓 Claude 執行所有動作但用 Classifier Model 在背景評估每個動作的安全性、把超出你 Request 範圍的、Target Unrecognized Infrastructure 的、被 Hostile Content 影響的動作擋下來。整體效果是 Prompt 變少但 Safety 比 bypass 好很多。

Claude Code Auto Mode 用的 Classifier 是獨立 Server Side Model 跑在背景、不受你選的 `/model` 影響、Anthropic 控制 Model 版本和 Quality。每次 Tool Call 前 Classifier 看 User Messages、Tool Calls、CLAUDE.md 內容做決策。Tool Results 不會餵給 Classifier 避免 Hostile File Content 影響評估。另外 Anthropic 還有 Server Side Probe 掃描 Tool Results 標記可疑內容、形成 Defense In Depth。

Claude Code Auto Mode 適合的情境是 Long Task 和 Reducing Prompt Fatigue。Refactor 100 個檔案、跑 Comprehensive Test Suite、Generate Documentation、Bulk Migration 這類動輒幾十次 Tool Call 的任務、Claude Code Auto Mode 讓 Claude 一氣呵成完成不被打斷。但要注意這是 Research Preview 不保證 100% 安全、敏感動作仍建議用 Default Mode 親自審視。

啟用後不會完全變成「無腦 Approve」。除了 Classifier 還保留三層防護：Permission Rules（你的 Allow/Deny Rules）、Read Only Actions 和 Working Directory Edits 自動 Approve、Protected Paths 仍會擋。意思是它是「智慧 Default Mode」而非「無腦 bypassPermissions」、安全性遠高於 bypass。

從產品設計角度看 Claude Code Auto Mode 解決的是 Permission Fatigue 問題。Anthropic Engineering Blog 提到 Default Mode 用戶平均每小時按 30 到 50 次 Approve、長期累積 Cognitive Load 嚴重影響 Workflow。Claude Code Auto Mode 用 ML 自動處理 Routine Approval、把 Cognitive Load 移到只有真正危險時才喚醒 Human In The Loop、設計理念非常對齊 Modern AI Tooling 的 Augment Not Replace 原則。

另一個有趣設計是 Defense In Depth。Claude Code Auto Mode 不是單一 Classifier 把關、是多層防護組合：Permission Rules 第一層、Working Directory Whitelist 第二層、Classifier 第三層、Conversation Boundary 第四層、Server Side Probe 第五層。每層擋不同類型風險、組合起來形成完整防護。即使某一層失效其他層仍能擋住。

## 2\. 啟用 Claude Code Auto Mode 需要哪些 Requirements？

啟用有 4 個必要 Requirements 缺一不可。第一是 Plan——需要 Max、Team、Enterprise、或 API Plan、不支援 Pro。Pro 訂閱用戶用不了這個 Feature。第二是 Admin——Team 和 Enterprise 環境裡 Admin 必須在 Claude Code Admin Settings 啟用後 User 才能用。Admin 也可以在 Managed Settings 鎖死 `permissions.disableAutoMode` 為 disable 完全禁用。

第三是 Model——Team、Enterprise、API Plan 支援 Claude Sonnet 4.6、Opus 4.6、Opus 4.7。Max Plan 只支援 Opus 4.7。其他 Model 包含 Haiku 和 Claude 3 系列都不支援。第四是 Provider——只在 Anthropic API 走的場景可用、Bedrock、Vertex、Foundry 都不支援。多雲部署團隊要評估這個限制。

| Requirement | Detail |
| --- | --- |
| Plan | Max, Team, Enterprise, or API（不支援 Pro） |
| Admin Approval | Team / Enterprise 需 Admin 在 Admin Settings 啟用 |
| Model | Sonnet 4.6, Opus 4.6, Opus 4.7（Max 限 Opus 4.7） |
| Provider | Anthropic API only（不支援 Bedrock/Vertex/Foundry） |
| Version | Claude Code v2.1.83 或更新 |

如果 Claude Code 報 Claude Code Auto Mode 不可用、通常代表前述某個 Requirement 沒滿足、不是暫時性 Outage。另一種情況是看到「cannot determine the safety」訊息、那是 Transient Classifier Outage 過幾分鐘會自動恢復。第一次嘗試 Claude Code Auto Mode 失敗先檢查 Plan、Model、Provider 三個基本條件、再考慮其他原因。

## 3\. 如何啟用 Claude Code Auto Mode？

啟用 Auto Mode 有幾種方式對應不同情境。CLI 中按 `Shift+Tab` Cycle Permission Modes 走過 default → acceptEdits → plan、Auto Mode 啟用條件滿足時會自動加入 Cycle、第一次 Cycle 到 auto 會跳出 Opt In Prompt 要你接受才正式進入 auto。Opt In 一次後後續 Cycle 直接進 auto 不再 Prompt。

```
# Method 1: CLI shift+tab cycle (in-session)
# Press Shift+Tab repeatedly until status bar shows "auto"

# Method 2: Start with flag
claude --permission-mode auto

# Method 3: Set as default in settings.json
{
  "permissions": {
    "defaultMode": "auto"
  }
}

# Method 4: VS Code extension
# Click mode indicator at prompt box → Auto mode

# Method 5: Desktop app
# Use mode selector next to send button
```

在啟動時直接用 `--permission-mode auto` Flag 進入 Auto Mode 跳過 Cycle Process、適合明確知道要用 Auto 的場景。設成 `defaultMode` 在 settings.json 是最 Persistent 做法、每次啟動 Claude Code 都直接進 Auto Mode、適合 Heavy User。VS Code 和 Desktop App 有獨立 UI 設定方式、需要先在 Extension Settings 啟用 Allow Dangerously Skip Permissions Toggle 才能看到 Auto Mode 選項。

Web 和 Mobile 的支援略有限制。Cloud Sessions 在 Claude Code on the web 只能用 Auto Accept Edits 和 Plan Mode、不支援 Auto Mode 和 Bypass Permissions。Remote Control Session 可以用 Ask、Auto Accept Edits、Plan Mode、不支援 Auto 和 Bypass。Auto Mode 是 CLI 和 Desktop 主場、Web/Mobile 暫時繞道。

## 4\. Claude Code Auto Mode 設定步驟

### Verify requirements

確認 Plan 是 Max Team Enterprise API、Model 用 Sonnet 4.6 Opus 4.6 或 4.7、Provider 是 Anthropic API、Claude Code v2.1.83 以上。Team 和 Enterprise 需 Admin 啟用。

### Cycle to auto with Shift+Tab

按 Shift+Tab 反覆切換直到 Status Bar 顯示 auto。第一次會跳 Opt In Prompt 確認接受。也可用 claude –permission-mode auto 直接啟動。

### State boundaries in conversation

在對話中明確說限制例如 dont push to main 或 wait until I review before deploying。Classifier 會把 Boundary 當 Block Signal 直到你 Lift。

### Monitor and adjust

被 Block 的動作會在 /permissions Recently Denied 列出按 r 可以 Retry 並手動 Approve。連續 3 次或總 20 次 Block 自動 Fallback 回 Default Mode。

## 5\. Claude Code Auto Mode 預設 Block 和 Allow 哪些動作？

Classifier 預設信任你的 Working Directory 和 Repo 配置的 Remotes、其他都當 External 直到你設定 Trusted Infrastructure。Default Block List 涵蓋常見高風險動作。

Default Block 動作包括：Download And Execute Code 例如 `curl | bash` 、Send Sensitive Data 到外部 Endpoint、Production Deploy 和 Migration、Mass Delete 雲端 Storage、授予 IAM 或 Repo Permissions、修改 Shared Infrastructure、不可逆 Destroy Session 之前就存在的檔案、Force Push、直接 Push 到 main Branch。這些是真正可能造成嚴重後果的操作、Classifier 一律擋下。

Default Allow 動作包括：Working Directory 內的本機檔案操作、安裝 Lock Files 或 Manifests 宣告的 Dependencies、讀.env 並把 Credentials 送到對應 API、Read Only HTTP Requests、Push 到你 Started On 的 Branch 或 Claude Created 的 Branch。日常開發 90% 動作都在 Allow 範圍、不會被 Block。

Sandbox Network Access Requests 不直接 Allow 而是 Route 給 Classifier 評估。要看完整 Default Rules 跑 `claude auto-mode defaults` 印出 Environment、Allow、Soft Deny 完整列表。Admin 可以透過 `autoMode.environment` Setting 加 Trusted Repos、Buckets、Services 讓 Classifier 知道企業 Internal Infrastructure 是 Trusted。

## 6\. Claude Code Auto Mode 的 Conversation Boundary 怎麼運作？

這個功能最強大的特性是 Conversation Boundary。你在對話中說「don’t push」或「wait until I review before deploying」、Classifier 會把這些 Boundary 當 Block Signal 直到你在後續訊息 Lift。即使 Default Rules 允許這個動作、Classifier 也會擋因為你明確說過不要。

Boundary 的設計很巧妙——它不是存成 Rule 而是 Classifier 每次檢查時 Re Read Transcript。意思是 Boundary 隨對話自然演進、不需要寫 Permission Rule 也能達到精細控制。但這也帶來限制：Context Compaction 移除原始 Boundary 訊息後 Boundary 可能 Lost。對需要 Hard Guarantee 的場景應該寫 Deny Rule 在 Permission Rules 裡而非靠 Conversation Boundary。

Claude 自己判斷 Boundary 條件 Met 不能 Lift Boundary——只有你在對話明確說 Lift 才會解除。例如你說「don’t push」後 Claude 完成 Code Review 想 Push、它不能自己決定 Boundary 解除而 Push、必須等你說「OK 現在可以 push 了」。這個機制保護你的決策權不會被 AI 過度解讀。

實戰用 Boundary 的 Best Practice 是 Session 開始講清楚限制。例如「這次 Session 只跑 Tests 不要碰 Production」、「修 Bug 時不要動 Test Files」、「重構時保留 Public API 不要破壞」。明確 Boundary 讓 Claude 在 Auto Mode 跑時自然遵守、Classifier 會 Enforce。比起每動作 Approve 高效很多。

## 7\. Claude Code Auto Mode 失敗時怎麼 Fallback？

Auto Mode 不是無條件信任 Classifier、有 Fallback 機制保護你。每個被 Block 的動作會顯示 Notification 並出現在 `/permissions` 的 Recently Denied 標籤、按 `r` 可以 Retry 並手動 Approve。讓你能 Override Classifier 決策、補救 False Positive。

Fallback 條件有兩個。Classifier 連續 Block 3 次或總共 Block 20 次、Auto Mode 暫停回到 Default Mode 開始 Prompt。Approve 提示恢復 Auto Mode 繼續跑。這兩個 Threshold 不可調整、是 Anthropic Hard Coded Safety Net。連續計數器在任何 Allowed Action 後 Reset、總計數器整個 Session 持續累積只在 Limit 觸發 Fallback 時 Reset。

Non Interactive Mode 用 `-p` Flag 跑時遇到 Repeated Blocks 會直接 Abort Session 因為沒有 User 可以 Prompt。CI 環境用 Auto Mode 要特別評估這個——如果 Classifier 因為 Infrastructure 沒設成 Trusted 連續 Block、CI Job 會中斷不能完成。CI 場景比較適合用 dontAsk Mode 配 Pre Approved Tools。

Repeated Blocks 通常表示 Classifier 缺 Infrastructure Context。解法兩個：第一用 `/feedback` 報告 False Positive 給 Anthropic 改 Classifier、第二是 Admin 在 `autoMode.environment` 設定企業 Trusted Infrastructure 例如「Production deploys to repo X are allowed」、讓 Classifier 知道哪些動作對你的環境是 Safe。

Decision Order 細節值得理解。Classifier 評估每個動作走固定順序 First Match Wins。第一 Permission Allow/Deny Rules 立刻 Resolve 不走 Classifier。第二 Read Only Actions 和 Working Directory Edits 自動 Approve 除了 Protected Paths。第三其他都走 Classifier。第四 Classifier Block 後 Claude 收到 Reason 嘗試 Alternative。這個 Pipeline 確保簡單動作不浪費 Classifier Compute、複雜動作才用 ML 評估。

Subagent Handling 的 3 個 Check Point 也是設計亮點。第一 Before Subagent Starts、Delegated Task Description 被 Classifier 評估、Dangerous Looking Task Spawn Time 就被 Block。第二 Subagent Runs 時每個動作走同樣 Classifier。第三 Subagent Finishes、Action History 被 Review、Concern 加 Security Warning Prepend 到結果。三層 Check 確保 Subagent 不會繞過 Auto Mode 防護。

## 8\. Claude Code Auto Mode 和其他 Permission Mode 的比較

Claude Code 有 6 個 Permission Mode 各自適合不同場景。比較表格幫你選對 Mode。

| Mode | Auto Approve | Best For |
| --- | --- | --- |
| default | 只 Reads | 新手 / 敏感工作 |
| acceptEdits | Reads + File Edits | 反覆 Iterate Code |
| plan | 只 Reads | 規劃前探索 |
| **auto** | **所有動作 + Classifier 安全檢查** | **長任務 / 減少 Prompt** |
| dontAsk | 只 Pre Approved Tools | Locked Down CI |
| bypassPermissions | 所有動作（除 Protected Paths） | 隔離 Container/VM |

選 Mode 的核心 Tradeoff 是 Convenience vs Oversight。敏感工作（Production Deploy、Migration、Security Code）選 Default 親自審視。日常 Iteration 選 acceptEdits 平衡。Long Task 想減少 Prompt 選 Auto。CI 自動化選 dontAsk 限定 Tools。隔離環境選 bypassPermissions 完全跳過。每個 Mode 在不同階段都有用、不是越鬆越好。

Auto Mode 和 bypassPermissions 最常被搞混。bypassPermissions 跳過所有檢查包含 Permission Layer、只剩 Protected Paths Check。Auto Mode 仍走 Permission Layer 但 Classifier 自動處理大部分 Approval。bypass 適合完全沒網路風險的 Sandbox 環境、Auto 適合一般開發環境想減 Prompt 但保留安全網。預設選 Auto 不要選 bypass 除非真的隔離。

另一個容易混淆的對是 Auto Mode 和 acceptEdits。acceptEdits 自動 Approve File Edits 和常見 Filesystem Commands、其他動作仍 Prompt。Auto Mode 自動 Approve 所有動作（Classifier 把關）。acceptEdits 適合知道 Edit 安全但網路操作要看的場景、Auto 適合連網路操作都信任 Classifier 的場景。Auto 比 acceptEdits 更激進但靠 Classifier 補回安全性。

## 9\. 哪裡可以找到更多 Claude Code Auto Mode 資源？

以下 3 支 Anthropic 官方影片搭配本文看能深化你對 Auto Mode 和 Permission System 的理解、特別是 Production Use 和 Best Practices 兩面向。

![Youtube video](https://i.ytimg.com/vi/fHWFF_pnqDk/hqdefault.jpg)

Vibe Coding in Prod — Production 環境權限管理實戰

![Youtube video](https://i.ytimg.com/vi/gv0WHhKelSE/hqdefault.jpg)

Claude Code Best Practices — 安全和 Permission 建議

![Youtube video](https://i.ytimg.com/vi/r82LXBH-ae0/hqdefault.jpg)

Claude Code for Service Delivery — 企業 Permission 管理

## 10\. 使用 Claude Code Auto Mode 有哪些限制？

Auto Mode 雖然強大實際使用仍有幾個限制要留意。第一是 **Research Preview Status** ——還在 Research Preview、Behavior 可能改變、不保證 Safety。Production Workflow 依賴前要評估、敏感操作仍建議用 Default Mode 親自審視。Auto Mode 不是「可以完全信任」的證明、是「平衡 Convenience 和 Safety」的工具。

第二是 **Plan Restrictions** ——Pro 訂閱用戶用不了。需要 Max（個人重度使用）、Team、Enterprise（組織）、API（API 用戶）。如果你只有 Pro 訂閱想用 Auto Mode、要升級到 Max 或評估其他 Mode 替代方案例如 acceptEdits。

第三是 **Provider Restriction** ——只支援 Anthropic API、不支援 Bedrock、Vertex、Foundry。多雲部署的企業如果用這些雲做 Compliance 需求暫時用不了 Auto Mode。要用必須走 Anthropic Direct API。

第四是 **Classifier Cost And Latency** ——Classifier 跑在 Server Side 額外 Token 算到你 Usage、每次 Tool Call 多一個 Round Trip 增加 Latency。Reads 和 Working Directory Edits 跳過 Classifier 沒 Overhead、Shell Commands 和 Network Operations 才會走 Classifier。整體影響不大但 Heavy User 要注意 Token 消耗。

第五是 **Boundary Loss After Compaction** ——Conversation Boundary 不是 Rule 是 Classifier Re Read Transcript 偵測。Context Compaction 移除原始 Boundary 訊息後 Boundary 可能 Lost。重要 Boundary 應該寫 Permission Deny Rule 而非靠對話講、避免 Compaction 後失效。

第六是 **Broad Allow Rules Get Dropped** ——進入 Auto Mode 時 Broad Allow Rules 會被自動 Drop 例如 Blanket `Bash(*)` 、Wildcarded Interpreter `Bash(python*)` 、Package Manager Run、 `Agent` Allow Rules。這是安全設計避免 Auto Mode 誤把 Broad Allow 當 Trust Signal。Narrow Rules 例如 `Bash(npm test)` 會保留。離開 Auto Mode 時 Dropped Rules 自動恢復。

實戰整合 Auto Mode 到 Engineering Workflow 的 Maturity Model。Stage 1 Trial：個人開 Auto Mode 跑簡單 Task 累積信任。Stage 2 Personal Default：把 settings.json defaultMode 設 auto 變預設、平均一天用 Auto 5 到 10 個 Session。Stage 3 Team Adoption：團隊統一文件 Recommended Auto Mode、訓練成員 Boundary 寫法。Stage 4 Enterprise Deployment：Admin 啟用 Auto Mode、配 Trusted Infrastructure、整合到 Engineering Standard。每個 Stage 大約 1 到 2 個月、整體 Maturity 達成 4 到 8 個月。

實戰案例分享。某 SaaS 新創團隊每位 Engineer 平均一天 200 次 Approval Click。導入 Auto Mode 後降到 30 到 50 次只在真正危險動作 Prompt。每位 Engineer 一天省 1.5 到 2 小時、團隊 10 人總共省 15 到 20 小時、相當於額外請 2 個 Engineer。Cost Analysis 後 Max 訂閱 200 美金 / 月 / 人對比省的時間遠遠值得。

另一個案例是 Senior Engineer 個人 Workflow。改用 Auto Mode 後最大改變是 Mental Mode 從「Reviewer」變「Pair Programmer」。Default Mode 下大腦在 Approve/Deny 切換消耗注意力、Auto Mode 下注意力可以集中在 Code 本身和 Architecture Discussion。質的變化比量的時間節省更重要、長期 Productivity 增益巨大。

## 11\. Claude Code Auto Mode 的進階使用技巧

進階 Tip 1：Boundary 越具體越好。模糊 Boundary 例如「小心一點」Classifier 不知怎麼 Enforce。具體 Boundary 例如「不要 Push 到 main、不要 Modify.env、不要 Run Production Migration」Classifier 能精準 Enforce。把 Boundary 當 Programming 寫越精準越好。

進階 Tip 2：Subagent 在 Auto Mode 也走 Classifier。Subagent 啟動前 Delegated Task Description 會被 Classifier 評估、Subagent 跑時每個動作走 Classifier、結束時整個 Action History 被 Review。Subagent Frontmatter 的 `permissionMode` 在 Auto Mode 下被忽略、所有 Subagent 一律走 Parent Session 的 Auto Mode。

進階 Tip 3：搭配 Hooks 強化權限控制。Auto Mode 仍走 PreToolUse 和 PermissionRequest Hooks、Hook 邏輯可以在 Classifier 之外加額外 Custom Logic。例如 Hook Block 所有對 Production Database 的寫入即使 Classifier 允許。Hooks 是 Auto Mode 的補充不是替代。

進階 Tip 4：用 Permission Rules 補 Classifier 不夠的場景。Classifier 是 ML Based 不保證 100% 正確、特定動作絕對不能允許就寫 Deny Rule。例如 `Bash(rm -rf /*)` 寫死 Deny 比靠 Classifier 安全。Permission Rules 在 Decision Order 第一順位、永遠覆蓋 Classifier 決策。

進階 Tip 5：定期 Review Recently Denied List。Auto Mode 跑長時間後到 `/permissions` Recently Denied 看 Classifier 攔了什麼。如果有 False Positive 用 `/feedback` 報給 Anthropic 改 Classifier、如果是真的危險動作就慶幸 Classifier 攔下來。Review 過程加深對 Classifier Behavior 的理解。

進階 Tip 6：企業環境設 Trusted Infrastructure。Admin 在 Managed Settings 寫 `autoMode.environment` 描述企業內部 Trusted Repos、Buckets、Services。Classifier 看這些 Description 知道哪些 External Looking 動作其實是 Internal Trusted、減少 False Positive。

進階 Tip 7：和 Sandbox 搭配 Defense In Depth。Auto Mode 是 Logical Permission、Sandbox 是 OS Level Isolation。兩者搭配 Auto Mode 控 Claude 想做什麼、Sandbox 確保即使 Claude 想做也跨不出 Sandbox 邊界。Production 環境兩者都開最安全。

進階 Tip 8：用 acceptEdits 當 Auto Mode 入門。從 Default Mode 直接跳 Auto 心理門檻高、用 acceptEdits 過渡幾週累積信任後再升 Auto。acceptEdits 適合熟悉 Claude 行為的階段、Auto 適合完全信任 Workflow Pattern 的階段。

從個人 Career 角度看 Auto Mode 是 AI Native Engineer 必備技能。會用 Auto Mode 加上 Boundary 寫法的工程師在企業裡 Productivity 比同階段同事高 30 到 50%、長期 Career 進展自然加速。早期採用累積經驗、等 Auto Mode 變 Industry Standard 時你已經是專家。

對 Engineering Manager Auto Mode 是 Team Productivity Lever。Manager 評估 Team Performance 應該看 Throughput 和 Quality 不是 Approval Click 數。鼓勵團隊用 Auto Mode 讓 Engineer 把 Cognitive Load 從 Permission 切到 Engineering、整體 Output 提升。Manager 帶頭用是最好示範。

Future Outlook 預期 Auto Mode 會持續演進。Classifier Quality 會隨 ML Research 提升 False Positive 降低。Trusted Infrastructure 設定會更彈性可能加 Domain Specific Templates。Subagent 整合會更順暢處理複雜 Multi Agent Workflow。整體方向是讓 Auto Mode 從 Research Preview 走向 GA Production Ready 狀態。

Action Item Checklist 給今天就想開始的讀者。Action 1 確認 Plan 是 Max 或更高、Model 用 Sonnet 4.6 / Opus 4.6 / Opus 4.7。Action 2 用 claude –permission-mode auto 啟動或 Shift+Tab 切換。Action 3 跑一個熟悉的 Task 體驗 Classifier 的 Approval 行為。Action 4 試著留 Boundary 例如「不要 Push」看 Classifier 怎麼 Enforce。Action 5 跑完後到 /permissions Recently Denied 看 Classifier 攔了什麼累積信任。整個流程 30 分鐘到 1 小時、給你完整 Tool 體驗。

本文涵蓋 Auto Mode 從入門到進階的所有面向、希望幫你 2026 年在 Claude Code 生態快速建立 Smart Permission 能力。中文圈第一篇深度教學、歡迎收藏隨時回來查找需要的細節。後續 Anthropic 更新 Classifier 或擴展 Plan 支援時也會更新本文。

Real World Adoption 數據觀察。Anthropic 公開 Beta 階段累積數萬 Power User 啟用 Auto Mode、平均每位 Engineer Auto Mode 啟用 70% 以上的 Session。早期採用者 Feedback 普遍正面、最常被讚的功能是 Boundary 機制和 Fallback 自動恢復。最常被抱怨的是 Pro 不支援 Plan Restriction。

Anthropic Engineering Blog 公開 Auto Mode 的 ML Architecture 細節。Classifier 是 Specialized Model 不是 General Purpose Claude、針對 Permission Decision 場景優化過。Training Data 包含 Anthropic 內部 Engineering Team 的 Real Decisions、配上 Synthetic Adversarial Examples 學習辨識 Hostile Patterns。Model 持續 Iteration、定期 Update 提升精準度。

對使用者透明度方面 Anthropic 做得不錯。 `claude auto-mode defaults` 印出完整 Default Rules、 `/permissions` Recently Denied 看 Classifier 拒絕的動作、 `/feedback` 報 False Positive 給 Anthropic 改 Classifier。所有決策過程基本透明、不是 Black Box。對企業 Compliance Audit 也友善能舉證 Permission Decision History。

Use Case 進階分享。第一個是 Long Refactor——大型 Codebase 改 Coding Style 例如 ESLint Rule 變更影響 200 個檔案、Auto Mode 自動跑 200 個 Edits 不打斷你工作。第二個是 Comprehensive Test Generation——為 Codebase 寫 Unit Test、需要連續 Read Source File、Write Test File、Run Test、Fix、循環幾百次、Auto Mode 跑得順。第三個是 Documentation Generation——讀 Code 寫 Doc 跑 Lint Build Verify、Auto Mode 一氣呵成。

第四個 Use Case 是 Bulk Migration——例如 v1 API Endpoint 全部換 v2 影響 50 個 Microservice、每個 Service 都要 Read Old Code Write New Code Run Tests Verify。Auto Mode 處理大量類似動作不疲勞。第五個是 Dependency Upgrade——Major Version 升級例如 React 18 到 19、跑各種 Migration Codemod 加 Manual Fix、Auto Mode 跑 Codemod 自動 Approve。

第六個 Use Case 是 Performance Profiling And Optimization——讀 Profile Output 找 Hotspot 改 Code Run Benchmark Verify Improvement、循環幾十次。第七個是 Type System Migration——Plain JS 加 TypeScript Types、每個檔案 Read Add Types Run Type Check Fix、Auto Mode 大量檔案不疲勞。每個 Use Case 都是 Permission Fatigue 嚴重的場景、Auto Mode 解決得最徹底。

Cost 計算進階。Token Overhead 約 5 到 15% 看任務類型。Read 重的任務 Overhead 接近 0% 因為 Reads 跳過 Classifier。Network 重的任務 Overhead 可能 15% 因為每個 HTTP Request 都走 Classifier。Mixed Workload 通常 5 到 10%。對 Pro 用戶 Token 沒影響因為配額固定、對 API 用戶按 Token 計費要算進預算、對 Max 用戶配額大不易超。

實作進階建議。在 Auto Mode 開始前先建立 Mental Model 它預設信任什麼不信任什麼。讀 `claude auto-mode defaults` Output、了解 Default Block 和 Allow List。實際跑 Task 時對 Classifier 行為建立 Pattern Recognition、知道什麼動作會被擋什麼會放行。累積這個直覺後 Auto Mode 用起來像 Pair Programming 順暢。

個人 Setup Recommendation 給想最大化 Auto Mode 價值的用戶。第一在 settings.json 設 defaultMode auto 永遠開著。第二建立 Personal CLAUDE.md 描述常用 Workflow Patterns 讓 Classifier 有更多 Context。第三 Session 開始花 30 秒寫清楚這次 Task 的 Boundary 例如「這次 Refactor 只改 src 不動 tests」。第四遇到頻繁 Block 用 /feedback 報 Anthropic、Classifier Quality 會慢慢提升。

對團隊 Scale Up Auto Mode 的 Recommendation。Phase 1 Senior Engineer 試用 2 週累積 Best Practice。Phase 2 寫 Internal Doc 教 Junior 怎麼用 Boundary 寫法。Phase 3 Team Default 設 Auto Mode 寫進 Onboarding 文件。Phase 4 整合到 CI Pipeline 但用 dontAsk 不用 Auto。每個 Phase 1 個月、整體 Rollout 4 個月達成 Team Wide Adoption。

## 12\. 參考依據

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Permission Modes 官方文件](https://code.claude.com/docs/en/permission-modes)
- [Auto Mode Configuration 官方文件](https://code.claude.com/docs/en/auto-mode-config)
- [Anthropic Auto Mode Announcement](https://claude.com/blog/auto-mode)
- [Claude Code Product Page](https://www.claude.com/product/claude-code)

## 13\. 常見問題 FAQ

### Claude Code Auto Mode 的 Classifier 用什麼 Model？

Classifier 跑在 Server Configured Model 獨立於你的 /model 選擇。Anthropic 控制 Classifier Model 版本和品質、不公開具體 Model Identity。Classifier Token 算到你 Usage 但成本通常不高因為只看 User Messages、Tool Calls、CLAUDE.md 不看 Tool Results。

### Claude Code Auto Mode 比 dangerouslySkipPermissions 安全嗎？

安全很多。dangerouslySkipPermissions 跳過所有 Permission Check 只剩 Protected Paths 防護、Auto Mode 仍走 Classifier 自動評估每個動作擋下高風險操作。實務上選 Auto Mode 而非 bypass、安全性接近 Default Mode 但 Productivity 接近 bypass、是最佳 Tradeoff。

### Claude Code Auto Mode 為什麼不支援 Pro 訂閱？

Auto Mode 是 Premium Feature、Anthropic 把它定位給 Heavy User 和 Organization。Pro 訂閱針對 Casual User 和個人輕度使用、Auto Mode 對應的長任務需求 Pro 用戶較少。Anthropic 沒承諾將來會擴展到 Pro 但不排除可能。Pro 用戶想用要升 Max。

### Claude Code Auto Mode 對 Token Cost 影響多大？

每次 Tool Call 多一個 Classifier Round Trip 加上額外 Token。Reads 和 Working Directory Edits 跳過 Classifier 沒 Overhead、Shell Commands 和 Network Operations 走 Classifier。實測下來 Token 多消耗約 5 到 15% 看任務類型。換來大量 Prompt Approval 時間節省 ROI 通常為正。

### Claude Code Auto Mode 在 CI 環境可以用嗎？

可以但有風險。CI 用 -p flag 跑 Non Interactive、遇到 Repeated Blocks 會直接 Abort 整個 Job。如果 Classifier 因為 Infrastructure 沒設 Trusted 連續 Block、CI 會中斷。建議 CI 用 dontAsk Mode 配 Pre Approved Tools 比較穩定。Auto Mode 主要場景是 Interactive 開發。

### Claude Code Auto Mode 的 Classifier 會不會被 Prompt Injection 騙？

Classifier 設計上抗 Prompt Injection。它只看 User Messages、Tool Calls、CLAUDE.md 內容、不看 Tool Results 避免 Hostile File Content 影響評估。另外 Server Side Probe 掃 Tool Results 標記可疑內容。整體 Defense In Depth 設計大幅降低 Prompt Injection 風險、但不是 100% 免疫、敏感操作仍建議親自審視。

### Claude Code Auto Mode 連續被 Block 怎麼辦？

連續 3 次或總 20 次 Block 後 Auto Mode 自動暫停回到 Default Mode 開始 Prompt。Approve 後恢復 Auto Mode。如果頻繁觸發 Fallback 通常表示 Classifier 缺 Infrastructure Context。解法是 Admin 在 autoMode.environment 加 Trusted Infrastructure Description 或用 /feedback 報 False Positive 給 Anthropic。

### Claude Code Auto Mode 和 GitHub Copilot Agent 比怎麼樣？

兩者目標相似都是減少 Approval Fatigue 但實作不同。GitHub Copilot Agent 走 Pre Approved Action List 比較死板。Auto Mode 用 Dynamic Classifier 能根據 Context 判斷比較智慧。Auto Mode False Positive 比 Copilot 多但 Coverage 更廣。實務上選哪個看你的 IDE 偏好和 Subscription。