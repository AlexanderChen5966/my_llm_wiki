---
title: "Claude Cowork內建瀏覽器怎麼用？免裝外掛，自動查資料、點擊與下載發票"
source: "https://www.bnext.com.tw/article/92044/claude-cowork-built-in-browser"
author:
  - "[[李先泰]]"
published: 2026-08-28
created: 2026-09-01
description: "Claude Cowork 內建瀏覽器上線！免裝外掛即可讓 AI 自動開網頁、填表單與蒐集資料。採獨立運行機制，保障個人隱私，大幅簡化繁瑣操作。"
tags:
  - "clippings"
status: "processed"
---
Anthropic 於美國時間 8 月 26 日宣布，桌面版 Claude Cowork 正式內建瀏覽器。當使用者交辦的任務涉及網站操作時，該瀏覽器會在側邊欄自動開啟，由 Claude 負責導航網頁、閱讀內容、點擊與輸入。

這意味著使用者無須安裝任何外掛或進行額外設定，也不必提供個人瀏覽器的隱私數據。

此功能自 8 月 26 日起，於一週內分批推送至 Pro、Max 與 Team 方案的桌面版用戶；Enterprise 方案則由管理員決定是否啟用。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 獨立運行：Claude 使用專屬瀏覽器，保障個人隱私

Claude 的內建瀏覽器與使用者日常使用的瀏覽器完全獨立，因此它無法讀取你的分頁、書籤或密碼。若需在要求登入的網站上執行任務，系統會採用「逐站轉移登入狀態」的機制：macOS 用戶可從 Chrome、Edge 或 Firefox 轉移狀態；Windows 與 Linux 則僅支援自 Firefox 匯入。

值得注意的是，系統預設會排除銀行、電子郵件及單一登入（SSO）等敏感性網站，除非使用者主動設定授權，否則相關資料不會被轉移。

Anthropic 列舉了幾項典型應用場景：為報告蒐集研究資料、登入供應商後台下載當月發票，或是操作尚未具備 API 串接功能的內部系統。這些以往需要使用者親自切換網頁、頻繁點擊的繁瑣操作，如今都能交由 Claude 自動代勞。

## 與「Claude in Chrome」的分工定位

針對既有的「Claude in Chrome」擴充功能，官方給出了明確的功能界線：內建瀏覽器適合將整個網頁任務「外包」，讓 AI 背景執行資料蒐集或操作外部網站，使用者可專心處理其他事務；而 Claude in Chrome 則專注於處理「當前開啟的網頁」，利用你已登入的帳號協助更新 CRM 系統、清理收件匣或修改文件。

預設情況下，已安裝外掛的用戶將維持以 Claude in Chrome 為預設值；未使用者則自動套用內建瀏覽器。若需更改，可至「Settings → Cowork → Preferred browser」進行切換。

在裝置支援方面，內建瀏覽器目前僅存在於桌面應用程式中，網頁版與手機版並未直接搭載。不過，只要桌面版保持開啟並連線，使用者仍可透過網頁或手機端遠端下達指令。若桌面版未啟動，網頁端則只能依賴 Claude in Chrome 執行任務。

## 提示詞注入風險降低但未完全消除

任何具備自動操作能力的 AI 代理程式（AI Agent），都面臨著「提示詞注入（Prompt Injection）」的資安威脅——即惡意指令被隱藏於網頁內容中，誘導 AI 偏離原始任務。

Anthropic 坦言內建瀏覽器亦有此風險。在防護機制上，系統沿用了 Claude in Chrome 的安全標準，透過交叉比對「Claude 的實際動作」與「使用者的原始指令」來進行審查。雖然這能大幅降低風險，但無法完全杜絕，因此官方建議初期僅在受信任的網站上使用此功能。

內建瀏覽器目前支援 macOS、Windows 與 Linux 桌面版（Linux 標註為 Beta 版）。該功能推播後將預設為開啟狀態，一旦任務涉及網站操作，瀏覽器即會自動啟動。Enterprise 企業版自 8 月 26 日起同步開放，管理員可至「Organization settings → Cowork → Built-in browser」中進行權限管控。

> 延伸閱讀： [運動30分鐘就能領雞胸肉、豆漿！「揮汗有禮」9月開跑，誰能領、去哪換一次看](https://www.bnext.com.tw/article/92045/sweat-rewards-2026)

資料來源： [Anthropic 官方公告](https://claude.com/blog/cowork-built-in-browser/)

本文初稿為AI編撰，整理．編輯/ 李先泰