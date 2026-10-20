---
title: "Modly：開源 AI 圖片轉 3D 工具，支援多種模型與工作流程 | 網路資源"
source: "https://tools.wingzero.tw/articles/4081"
author:
  - "[[赤紅翼零]]"
published: 2026-09-28
created: 2026-09-29
description: "Modly 是開源本機 AI 3D 生成工具，可將圖片轉成 3D 網格模型，支援 Hunyuan3D、TripoSG、Trellis2 GGUF 與視覺化工作流程。"
tags:
  - "clippings"
status: "inbox"
---
網站連結 [https://github.com/lightningpixel/modly](https://github.com/lightningpixel/modly)

**Modly** 是一款開源桌面應用程式，能利用本機執行的 AI 模型，把圖片轉換成 3D 網格模型。專案主打在使用者自己的 GPU 上執行生成流程，支援 Windows、Linux 與 Apple Silicon Mac；它也提供可視化工作流程，讓使用者連接圖片輸入、網格生成與場景載入等步驟。

Modly 的定位是讓 3D 生成模型更容易安裝與操作。使用者可從模型頁面安裝官方或第三方擴充套件，再下載對應模型，透過工作流程執行生成。官方擴充套件涵蓋 Hunyuan3D 2 Mini、TripoSG、Trellis2 GGUF 等模型；擴充方式採用 GitHub 專案與 `manifest.json` ，也讓社群有機會加入其他模型或處理流程。

Modly 的主要功能包括：

- **圖片轉 3D 網格** ：將輸入圖片送入選定模型，產生可匯入場景的 3D 模型。
- **視覺化工作流程** ：以節點流程串接圖片、模型與場景，例如「Image → Generate Mesh → Add to Scene」。
- **擴充模型與流程** ：可從 GitHub 安裝模型或處理流程擴充套件。
- **網格簡化與平滑** ：匯入的網格可在應用程式內平滑或降低面數，並將最佳化結果存回工作區。
- **命令列與 AI 代理支援** ：內附 CLI，可供腳本或 AI 代理呼叫桌面應用程式、列出模型、執行工作流程及匯出 `.glb` 等模型檔。
- **本機生成** ：專案主打使用本機 GPU 執行開源模型，並將結果留在本機工作流程中。

從使用情境來看，Modly 適合想快速將概念圖、產品照片或其他參考圖片轉成可檢視 3D 模型的人，也適合希望讓 AI 或自動化腳本參與模型生成的開發者。它的工作流程與擴充系統降低了切換不同模型的門檻；但生成品質、速度與可用模型仍會受到 GPU、模型權重與工作流程設定影響。Modly 也不是完整的 3D 建模軟體，生成後的模型若需精細修形、拓樸整理或正式製作，仍可能需要其他 3D 工具協助。

安裝檔可從 GitHub Releases 下載；macOS 版本目前以 Apple Silicon 為目標，Intel Mac 不在專案列出的支援平台內。Modly 本身採 MIT 授權，但各模型與擴充套件可能有不同條款，使用前應分別確認。專案也提醒，若將 Modly 分支改造成自己的應用程式，需依 MIT 授權保留原專案與作者的署名。

整體而言，Modly 將本機 AI 3D 生成、模型擴充、工作流程與命令列自動化整合在同一個桌面工具中。對想嘗試開源圖轉 3D 模型、又希望保留本機生成與後續編輯彈性的使用者來說，它提供了一個方便入門的選擇。