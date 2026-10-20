---
title: Docker 本機環境輕量化：先量測再改，並分清「加快建置」與「加快回應」
date: 2026-09-09
tags: [dev/ci-cd, type/how-to]
source: https://codelove.tw/@tony/post/aWKOna（原文 https://qiita.com/nolanlover0527/items/24ee07122dd4d0523069）
type: reference
status: processed
asset_value: 32
---

> **存放理由**：asset_value 32，未達 wiki 完整條目門檻（≥ 50），且主題偏離本庫 dev 主軸
> （Flutter / Spring Boot）。但「先量測再改」與「哪些改動加快建置、哪些加快回應」這組分類判準
> 可轉用，故以精簡筆記保留於 `research/`，不建完整 TBRC 條目、不進 `wiki/INDEX.md`。
> 原文情境為 Windows / WSL2 本機開發環境。

## 可轉用的兩個判準

### 判準一：先量測再改，否則容易做白工

三件套量測工具：

| 指令 | 看什麼 | 判讀 |
| --- | --- | --- |
| `docker system df` | `RECLAIMABLE` 欄位 | 累積到數十 GB 就該 prune |
| `docker stats --no-stream` | `CPU %` / `MEM USAGE` | 🔑 **若只有單一容器爆表，問題大多在應用程式碼而非 Docker 本身** |
| `curl.exe` 跑 30 次 | `TTFB`（首個資料回傳前）與 `TOTAL`（整體完成） | 改動前後都要量，**比較中位數與 P95** |

```powershell
1..30 | ForEach-Object {
    curl.exe -s -o NUL -w "TTFB=%{time_starttransfer}s TOTAL=%{time_total}s`n" http://localhost:8080/
}
```

⚠️ PowerShell 的 `curl` 是 `Invoke-WebRequest` 的別名，**必須明寫 `curl.exe`**。
另外 `curl` 只量到 HTTP 回應時間，瀏覽器的完整顯示時間還含圖片／JS／CSS 載入與快取，
最終確認要搭配開發者工具。

### 判準二：改動要分清它改善的是「建置」還是「回應」

🔑 這是原文最有價值的分類，作者在開頭與結尾都特別標注：

| 只改善**建置／拉取／重建／啟動**時間 | 真正影響**執行中頁面回應**速度 |
| --- | --- |
| `.dockerignore` 縮小 build context | Docker Desktop 資源配額（CPU / Memory，WSL2 後端改 `.wslconfig`） |
| 多階段建置（builder → slim 執行階段） | WSL2 檔案共享位置（專案從 `C:\` 搬到 `\\wsl$\`） |
| slim / alpine 輕量基底映像 | bind mount 改 named volume（DB 等高頻 I/O 目錄） |
| 建置快取（先 COPY 相依定義檔再安裝，最後才 COPY 原始碼；BuildKit `--mount=type=cache`） | — |

把「改善建置時間」的手段當成「加快回應」的對策，是這類調優最常見的白工來源。

## 其他值得記的操作要點

- **日誌上限**：`json-file` 驅動預設**無上限**，是磁碟被吃光的常見主因。在 compose 服務加
  `logging.options.max-size: "10m"` / `max-file: "10"`；代價是查不到更早的日誌，且套用需
  `--force-recreate`（服務會暫時停止）
- **bind mount 跨 OS 邊界**：專案放 Windows 端做 bind mount，Node.js / PHP 大量小檔讀寫時成為瓶頸；
  搬到 WSL2 Linux 端通常明顯改善
- **named volume 遷移必須先備份**：`mysqldump` 備份 → 改 volume 定義 → `down` + `up -d --force-recreate`
  → 還原後**驗證讀寫**。🔴 `docker compose down -v` 會連 volume 一起刪，要保留 DB 資料就不能加 `-v`
- **alpine 用 musl 而非 glibc**：PHP 擴充等相依可能失效，apk 套件名也不同。改 slim/alpine 後
  必須驗證啟動、DB 連線、登入、上傳、圖片處理、主要頁面、批次處理
- **資源配額不是越大越好**：壓迫到主機端記憶體或 CPU，會讓含 IDE 與瀏覽器的整體環境變慢；小幅調整並用同一套量測比較

## 作者對 AI 診斷的態度（可轉用）

建議可把 Docker 相關檔案交給 AI 讀取、問瓶頸在哪，但明確警告
**「通常會列出一長串建議，不小心全部套用環境也可能被破壞」**——
哪些可執行、哪些不行仍需自行判斷並逐項驗證。
這與本庫既有的「AI 產出需要判斷基準」立場一致。
