---
title: Amper 正式併入 Kotlin Toolchain：Gradle 的統一入口野心
date: 2026-07-09
tags: [dev/architecture, type/concept]
source: https://codelove.tw/@tony/post/qP5DAx
type: article
status: processed
asset_value: 44
---

JetBrains 把實驗性專案 Amper 併入 Kotlin Toolchain，從實驗階段推進到 Alpha（0.11）。目標是給 Kotlin 生態做一個類似 `cargo` / `dotnet` / `go` 的統一官方入口，取代原本 `amper` 命令與外掛。

## T（核心論點）

JetBrains 的目標已從「改善 Gradle 設定體驗」升級為「建立 Kotlin 官方工具鏈入口」。現在建 Kotlin 專案要處理 Kotlin compiler、Gradle、Kotlin/Android Gradle Plugin、Kotlin/Native、Xcode 產物、Maven 發布簽章等一堆碎片，Kotlin Toolchain 把這些收斂成單一 `kotlin` 命令（`kotlin build` / `run` / `test` / `publish`）。**Gradle 腳本本質是可執行程式碼，難被 IDE/CI/AI agent 靜態分析；YAML 宣告式設定則相反**——這是本次轉型少數明確點出的 AI 時代考量。

## B（適用場景）

Kotlin / KMP（Kotlin Multiplatform）專案的建置工具選型；評估是否遷移離開純 Gradle 設定。

## R（0.11 版重點）

- **統一入口**：`amper`/`amper.bat` → `kotlin`/`kotlin.bat`；IDE 外掛也改用 Kotlin Toolchain 外掛；`kotlin` 命令自動找專案內 wrapper 執行相符版本
- **Maven Central 發布**：`module.yaml` 宣告 publishing 設定，`kotlin publish mavenCentral` 自動處理 sources jar、PGP 簽章、POM metadata、checksum；**KMP library publishing 尚未支援**，暫僅 `product: jvm/lib`
- **Cinterop 支援增強**：依 `.def` 檔自動生成 C 函式庫 binding，IDE sync 階段同步生成
- **CLI/IDE 行為一致**：相依性解析從「專案級」改「模組級」，解決 Gradle 長期痛點（命令列過但 IDE 紅字、IDE 不紅但 CI 掛）
- **外掛開發能力**：`checks`（品質檢查，如 lint）、`commands`（自訂命令）、`generated`（統一宣告生成物，取代舊 `markOutputAs`）——**官方特別強調生成物要讓人與 AI agent 都容易識別**

## C（關鍵行動）

- [ ] Kotlin/KMP 新專案評估直接採用 Kotlin Toolchain 而非傳統 Gradle 設定
- [ ] 關注 KMP library publishing 何時補齊（目前限 JVM）
- [ ] 短期 Gradle 仍會共存，但 Kotlin-first 新專案的「直接可見度」會逐步降低

## 相關連結

（暫無相關條目，Kotlin/Gradle 生態首篇記錄）
