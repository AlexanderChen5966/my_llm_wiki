---
title: "awesome-gamedev-agent-skills：跨引擎遊戲開發的 AI 代理技能庫，讓你可以更容易開發出優秀的遊戲 | 網路資源"
source: "https://tools.wingzero.tw/articles/4122"
author:
  - "[[赤紅翼零]]"
published: 2026-10-25
created: 2026-10-27
description: "awesome-gamedev-agent-skills 收集跨引擎的遊戲開發技能，並透過路由器依專案與任務載入合用的 AI 程式代理指引。涵蓋 Godot、Unity、Unreal、網頁遊戲等引擎，也包括角色控制、存檔、關卡、美術資產與發布流程，適合想用 AI 加速遊戲製作的開發者。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills)

awesome-gamedev-agent-skills 是一套提供給 AI 程式代理使用的遊戲開發技能庫。它不是新的遊戲引擎，也不是能自己產生完整遊戲的 AI；專案把引擎知識、常見開發任務與工作流程寫成可重複載入的 SKILL.md 指引，讓你用自然語言描述需求時，代理能依專案與任務找到較相關的參考，減少每次都要從頭交代背景的時間。

README 目前列出 76 個技能，涵蓋引擎、跨引擎開發概念、遊戲類型與工作流程。引擎範圍包括 Godot、Unity、Unreal、Ebitengine、Phaser、PixiJS、three.js、Bevy、pygame、LÖVE、Defold 與 Roblox；其他技能則涉及角色移動、輸入、物理、動畫、Shader、存檔、對話、關卡設計、效能調校，以及 Roguelike、RPG、平台跳躍、塔防等遊戲類型，也收錄 Game Jam、快速原型與 itch.io／Steam 發布流程等主題。

專案的路由技能會先從專案檔案辨識引擎，再根據你提出的工作載入適合的技能。例如，要求 Godot 角色加入二段跳時，會搭配角色移動與平台遊戲相關指引；處理 Unity RPG 背包時，則可載入 ScriptableObject、RPG 與存檔等內容。使用者不必記住每個技能名稱，切換到新任務時，路由也會重新挑選相關資料，而不是一次把整套文件都塞給代理。

- **跨多種遊戲引擎：** 從 2D、3D、網頁遊戲到 Roblox 專案，都能找到相對應的開發指引。
- **按工作內容組合知識：** 引擎技能可和 AI 行為、關卡、遊戲手感、UI／UX、效能等概念搭配使用。
- **支援多種程式代理：** 採用通用的 SKILL.md 格式，可供 Codex、Claude Code、Cursor、Windsurf、Cline、Gemini CLI、GitHub Copilot、Kiro 等工具讀取，不必為每個編輯器改寫成不同格式。
- **也涵蓋美術資產流程：** create-game-assets 技能會協助代理先定義視覺方向、建立一致的素材家族、規範輸出並在遊戲中檢查成果；它提供的是工作流程指引，仍需搭配實際的代理、生成工具與人工檢視。

安裝方面，專案提供 skills CLI 的快速安裝方式，也可先列出內容、指定單一代理，或只安裝路由器與特定引擎技能。README 標示此方式需要 Node.js 22.20 以上；實際安裝前可先查看專案的安裝與相容性文件，確認自己使用的代理支援方式和引擎版本。

這套技能可以讓 AI 輔助遊戲開發時更容易套用引擎慣例與專案背景，但輸出仍須由開發者檢查、在目標引擎中執行與測試。技能內容會隨引擎版本更新，適用基準與升級方式應以倉庫文件為準。專案採 Apache License 2.0，並附有 NOTICE；若要複製、修改或散布內容，請閱讀倉庫現行的 [LICENSE](https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/LICENSE) 與 [NOTICE](https://github.com/gamedev-skills/awesome-gamedev-agent-skills/blob/main/NOTICE) 。

<iframe frameborder="0" src="https://89083b235e87d282e32393fcca4c6f1d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>