---
title: Stirling PDF：超過 100 種工具的自架開源 PDF 平台
date: 2026-06-30
tags: [dev/architecture, ai/workflow, type/how-to]
source: https://tools.wingzero.tw/article/sn/3959
type: article
status: processed
asset_value: 52
---

Stirling PDF 是功能完整的開源 PDF 處理平台，提供超過 100 項工具（合併、分割、壓縮、格式轉換、OCR、電子簽名、浮水印、頁面編輯等）。最大特色是**可自行部署（Self-Hosted）**，所有文件都在本機或企業內部伺服器完成處理，不必上傳第三方，因此成為重視隱私者的熱門選擇。

## T（核心論點）

線上 PDF 工具常有檔案大小限制、功能付費、或必須把機密文件上傳雲端。Stirling PDF 反其道而行——免費、開源、可自行掌控，把上百種 PDF 功能整合成一套瀏覽器操作的 Web 應用，讓個人與企業在自己的環境完成所有文件操作。

## B（背景與適用場景）

- 經常處理 PDF 且重視資料隱私（政府、醫療、金融、法律事務所）
- 想降低對商業 PDF 服務的依賴
- 需在企業內網提供團隊共用的文件處理能力
- 個人 NAS / 家庭伺服器想自架

## R（主要內容）

**功能涵蓋**：合併/分割、壓縮、Office↔PDF 互轉、圖片↔PDF、OCR 文字辨識、頁面旋轉/排序/刪除、浮水印增減、加密解密、電子簽名、表單填寫、中繼資料編輯、PDF 差異比較、頁面裁切、QR/條碼工具。

**格式轉換**：除 PDF↔Office，也支援 PNG、JPG、SVG、HTML、Markdown。

**熱門功能**：OCR——把掃描文件/圖片型 PDF 轉成可搜尋可編輯文字。

**部署/架構**：Web 應用設計，官方提供 Docker 映像檔，可快速部署於個人 NAS、家庭伺服器或企業環境；瀏覽器操作免裝大型桌面軟體。專案更新活躍、貢獻者眾，逐步發展成完整文件處理平台。

**安全優勢**：完全自架，所有文件保留在內部環境，不傳外部伺服器，降低機密外洩風險。

## C（對自身的啟發）

- [ ] 可自架作為本知識庫 raw/ PDF 素材的前處理（OCR、分割、轉檔）
- [ ] OCR + 轉 Markdown 能力可與 MarkItDown 互補，建立 PDF 入庫管道
- [ ] 隱私自架特性適合處理含敏感資訊的素材

## 相關連結

- [[markitdown-document-conversion]] — MarkItDown：轉 Markdown 餵 LLM
- [[notebooklm-py-cli-tool]] — NotebookLM CLI
