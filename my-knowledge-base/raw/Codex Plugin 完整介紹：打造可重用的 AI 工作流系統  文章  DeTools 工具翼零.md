---
title: "Codex Plugin 完整介紹：打造可重用的 AI 工作流系統 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3847"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-03-27
created: 2026-05-19
description: "Codex Plugin 將 AI 能力模組化為可重用的工作系統，整合 Skills、工具與 MCP，解決產出不穩、無法重用與協作困難問題，讓 AI 從對話工具升級為可部署的開發流程。"
tags:
  - "clippings"
status: processed
---
在 AI 開發工具（如 Codex、Cursor、Claude）快速演進的當下，單純使用 Prompt 已經無法滿足進階開發需求。當你開始需要「穩定產出」、「跨專案重用」、「團隊協作」，就會遇到一個關鍵問題：

如何把 AI 的能力「系統化」？

這正是 **Codex Plugin** 要解決的核心問題。

## 一、什麼是 Codex Plugin？

Codex Plugin 可以理解為：

> **把 AI 的能力（技能、工具、整合）打包成一個可安裝的模組**

它不是單純的一段 Prompt，而是一整套「可重複使用的工作流系統」，可以包含：

- Skills（技能）
- Apps（外部工具整合）
- MCP Servers（遠端能力連接）

簡單來說：

| 類型 | 說明 |
| --- | --- |
| Prompt | 一次性指令 |
| Skill | 可重複使用的任務邏輯 |
| Plugin | 一整套 AI 工作系統 |

## 二、Codex Plugin 解決了什麼問題？

在實務使用 AI 開發時，常見問題包括：

### 1\. 產出不穩定

同樣的需求，每次生成結果風格不同

### 2\. 無法重用

每個專案都要重新寫 Prompt

### 3\. 團隊難以協作

每個人都有自己的 Prompt，沒有統一標準

### 4\. 工具整合混亂

API、CLI、外部工具沒有統一入口

Codex Plugin 的角色就是：

> 把「AI 使用方式」標準化、模組化、產品化

## 三、Plugin 的核心架構

一個 Codex Plugin 的基本結構如下：

my-plugin/  
.codex-plugin/  
plugin.json  
skills/  
.app.json  
.mcp.json  
assets/

### 1\. plugin.json（核心入口）

這是 Plugin 的主設定檔，必須存在：

```js
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "Reusable workflow",
  "skills": "./skills/"
}
```

### 2\. skills/

存放 AI 的「能力模組」

每個 skill 通常是一個資料夾，裡面包含：

\---  
name: hello  
description: Greet user  
\---  
  
Greet the user and ask how you can help.

### 3..app.json（選用）

定義外部工具整合（API、CLI）

### 4..mcp.json（選用）

連接 MCP Server（讓 AI 能操作外部系統）

### 5\. assets/

UI 顯示用（icon、logo、截圖）

## 四、Plugin vs Skill：該怎麼選？

這是文件中非常重要的一個觀念。

### 使用 Skill 就好（簡單情境）

- 單一專案使用
- 個人開發
- 測試用

### 使用 Plugin（進階情境）

- 跨專案重用
- 團隊共享
- 建立標準流程
- 整合外部工具

建議流程：

> **先用 Skill → 確認可行 → 再升級成 Plugin**

## 五、Plugin 如何被載入？

Codex 並不是直接讀你的資料夾，而是透過：

**Marketplace（插件市場）**

### Marketplace 設定位置

可以有三種來源：

1. 官方 Marketplace
2. 專案內
	$REPO\_ROOT/.agents/plugins/marketplace.json
3. 個人設定
	~/.agents/plugins/marketplace.json

marketplace.json 範例

```js
{
  "name": "local-repo",
  "plugins": [
    {
      "name": "my-plugin",
      "source": {
        "source": "local",
        "path": "./plugins/my-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

## 六、Plugin 的實際安裝流程

### Step 1：建立 Plugin

mkdir -p my-plugin/.codex-plugin  
mkdir -p my-plugin/skills/demo

### Step 2：建立 plugin.json

```js
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My AI workflow",
  "skills": "./skills/"
}
```

### Step 3：建立 Skill

\---  
name: review  
description: Review code  
\---  
  
Analyze code and provide improvement suggestions.

### Step 4：加入 Marketplace

mkdir -p.agents/plugins

建立：

```js
{
  "name": "local",
  "plugins": [
    {
      "name": "my-plugin",
      "source": {
        "source": "local",
        "path": "./plugins/my-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

### Step 5：重啟 Codex

這一步非常重要。

Codex 會把 Plugin 複製到快取目錄：

~/.codex/plugins/cache/

如果沒有重啟，可能會讀到舊版本。

## 七、進階 Plugin（完整版本）

當你要正式提供給團隊或產品使用時，可以補上完整設定：

```js
{
  "name": "my-plugin",
  "version": "0.1.0",
  "description": "AI workflow bundle",
  "author": {
    "name": "Your team"
  },
  "skills": "./skills/",
  "interface": {
    "displayName": "My Plugin",
    "category": "Productivity",
    "defaultPrompt": [
      "Use this plugin to analyze data"
    ],
    "brandColor": "#10A37F"
  }
}
```

## 八、實務上的最佳使用策略

### 1\. 先小再大

- 先做 Skill
- 再封裝 Plugin

### 2\. Plugin = AI 工具產品化

你可以把 Plugin 當成：

### 3\. 團隊標準化的關鍵

Plugin 可以讓你：

### 4\. 搭配 MCP / OpenCLI 威力更大

當 Plugin + MCP + CLI 工具整合後：

AI 就不只是「寫程式」  
而是「直接操作系統」

## 九、總結

Codex Plugin 的本質，可以用一句話概括：

> **把 AI 從「對話工具」升級成「可部署的工作系統」**

它讓你能做到：

[完整官方文件介紹](https://developers.openai.com/codex/plugins)