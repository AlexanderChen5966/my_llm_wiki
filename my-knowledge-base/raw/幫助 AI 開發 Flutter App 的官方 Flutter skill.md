---
title: "幫助 AI 開發 Flutter App 的官方 Flutter skill"
source: "https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-flutter-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%B9%AB%E5%8A%A9-ai-%E9%96%8B%E7%99%BC-flutter-app-%E7%9A%84%E5%AE%98%E6%96%B9-flutter-skill-e8691efdaf55"
author:
  - "[[彼得潘的 iOS App Neverland]]"
published: 2026-05-23
created: 2026-05-25
description: "Flutter 官方推出的 Flutter Agent Skills 提供許多 Flutter 相關的 skill，幫助 AI 寫出更好的 Flutter App。"
tags:
  - "clippings"
status: processed
---
Flutter 官方推出的 Flutter Agent Skills 提供許多 Flutter 相關的 skill，幫助 AI 寫出更好的 Flutter App。

## [GitHub — flutter/skills](https://github.com/flutter/skills?source=post_page-----e8691efdaf55---------------------------------------)

### Contribute to flutter/skills development by creating an account on GitHub.

github.com

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*mVObxcsRb0tnOVv4irahCw.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*GdYt2OsVtzuA92TMGFE3oQ.png)

## 安裝 Flutter skill

使用以下指令可將 flutter 全部的 skill 裝到 `.agents/skills/` ，Codex、Antigravity、GitHub Copilot、Cursor 等常見的 agent 皆可使用。

```c
npx skills add flutter/skills -s '*' -a universal
```

`-s '*'` 表示全部的 skill，-a universal 表示裝到大部分 agent 讀取的 `.agents/skills/` 。

## Get 彼得潘的 iOS App Neverland’s stories in your inbox

Join Medium for free to get updates from this writer.

ps: 以下是更詳細的寫法，將 -s 改成全名 — skill，-a 改成全名 — agent。

```c
npx skills add flutter/skills --skill '*' --agent universal
```
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*gMS6lc0bqJUkx7UN7QoO1Q.png)

選擇將 skill 裝到 Project 或是全部專案皆可讀取的 Global，在此我們選擇 Project。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*FecXMm9h7C8oHM0TR4saBw.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*LPynTCzZ-ZVKF9SLFN_MUQ.png)

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*ceQAR0GT2FankYbbalff8g.png)

安裝完成，現在從.agents/skills/ 讀取 skill 的 agent 都能依據 prompt 判斷是否使用特定的 flutter skill。如果想要強制 agent 使用某個 skill，也可以輸入 / 加名稱手動觸發。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*gNC6T7APPUsKZ7WFVOBFrQ.png)

- Codex
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*qc9IaM-8LoaOd-utglxWig.png)

- GitHub Copilot
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*fYhkeoOfpp4677FOnD9PnQ.png)

- Antigravity
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*S7rV4ZO0ILkHztP0wJyImQ.png)

Claude Code 讀取的 skill 位置在 ` .claude/skills/` ，因此若想從 Claude Code 使用 flutter skill，請改用 -a claude-code 安裝。

```c
npx skills add flutter/skills -s '*' -a claude-code
```
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*yn09FMdFQYXr25fh7EJnvQ.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Es1v9XGECyu1LjT5DeoeIQ.png)

安裝成功，Claude Code 可以正常使用 flutter skill 了。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lOC5Z7iANHt67zCIsZdciw.png)

## 使用 Flutter skill

輸入 prompt `串接 iTunes Search API 的 App，可以輸入歌手名稱顯示歌曲列表` 。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*m6fjJfiAUwIe3CiydFUJNw.png)

AI 自動採用 flutter-use-http-package skill。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lFrbp5akwPKKToviKt9ieA.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ltELpDmloYMRhbhN3dVuxg.png)

AI 成功完成 iTunes 歌曲搜尋 App。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*drMnxomopq027wR9QjR0iw.png)

## 參考連結

## [How to write really good Flutter code](https://io.google/2026/explore/technical-session-27?source=post_page-----e8691efdaf55---------------------------------------)

### Don't miss Google I/O, featuring product launches, innovations, and insights. Tune in for the live keynotes and…

io.google