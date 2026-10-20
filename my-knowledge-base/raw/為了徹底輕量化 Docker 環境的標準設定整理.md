---
title: "為了徹底輕量化 Docker 環境的標準設定整理"
source: "https://codelove.tw/@tony/post/aWKOna"
author:
published:
created: 2026-09-09
description: "### [](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言應該有不少人正為 Docker 環境太過沉重而頭痛吧。  我自己也曾多次接觸過非常卡頓的 Docker 環境，並且有一步一步把原因排除掉的經驗。這次要介紹的是，針對本機的 Docker 環境（Wi..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月09日](https://codelove.tw/@tony/post/aWKOna "2026-09-09 09:15:08")

## 為了徹底輕量化 Docker 環境的標準設定整理

### 前言

應該有不少人正為 Docker 環境太過沉重而頭痛吧。  
我自己也曾多次接觸過非常卡頓的 Docker 環境，並且有一步一步把原因排除掉的經驗。

這次要介紹的是，針對本機的 Docker 環境（Windows）進行輕量化的方法，並搭配我實際執行過的步驟一起說明。

不過，其中包含像是刪除資源、移轉 named volume 等，如果省略確認與備份，可能會影響本機環境的操作。請先確認各步驟的注意事項，並先用工作副本或不重要的資料來測試。

請依照上方順序，從你自己的環境中看起來最可能有效的地方開始嘗試。

**注意** ：.dockerignore、多階段建置、基底映像輕量化、善用建置快取，主要是改善建置、拉取、容器重建與啟動時間。它們不是直接加快運行中頁面回應速度的對策。

### 1\. 清理不必要的資源

Docker 不會自動刪除建置快取、舊映像檔、已停止的容器。這些東西累積起來會壓迫磁碟空間，甚至讓整個主機的運作變慢。

**步驟**

1. 確認目前的磁碟使用狀況
```
docker system df
```
1. 檢查 `RECLAIMABLE` 欄位的數值。如果累積到數十 GB，就要注意了
2. 確認要刪除的對象
```
docker ps -a
docker images
docker volume ls
```
1. 一次刪除未使用的映像檔、容器、網路
```
docker system prune
```

如果你有保留停止中的容器作為復原用，執行前請務必先確認目標。刪除後若要重新建立由 Compose 管理的服務，請執行 `docker compose up -d` 。

### 2\. 用 docker stats 找出瓶頸

如果只是「感覺很慢」就直接挑對策，很容易做白工。先確認到底是哪裡出問題。

**步驟**

1. 確認所有執行中的容器資源狀況
```
docker stats --no-stream
```
1. 看 `CPU %` 和 `MEM USAGE` 欄位，檢查是否只有特定容器特別突出
2. 如果有找到，確認那個容器的應用程式日誌

如果只有某一個容器異常高負載，問題大多不在 Docker 本身，而是應用程式程式碼的可能性較高。

頁面回應時間也要在修改前後進行測量。由於 PowerShell 裡的 `curl` 可能是 `Invoke-WebRequest` 的別名，因此這裡明確使用 `curl.exe` 。

```
1..30 | ForEach-Object {
    curl.exe -s -o NUL -w "TTFB=%{time_starttransfer}s TOTAL=%{time_total}s\`n" http://localhost:8080/
}
```

`TTFB` 是第一個資料回傳前的時間， `TOTAL` 是整體回應完成的時間。

`curl` 能測到的是 HTTP 請求的回應時間。瀏覽器上的完整顯示時間還包含圖片、JavaScript、CSS 的載入、瀏覽器快取、外部服務處理時間等。最後確認時，請搭配瀏覽器開發者工具一起看。

### 3\. 防止日誌檔過度膨脹

Docker 預設的日誌驅動程式 `json-file` 若放著不管，日誌檔會無限制地膨脹。對於流量高的網站來說，這常常就是磁碟空間被吃光的原因。

**步驟**

1. 打開 `docker-compose.yml` ，在變得很重的服務加上以下設定
```
services:
  web:
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "10"
```
1. 套用設定
```
docker compose up -d --force-recreate web
```

由於會重新建立既有容器，服務會暫時停止。 `web` 請替換成實際的服務名稱。

`max-file` 是指定要保留的檔案數。因為這會讓你無法再往前查更早以前的日誌，所以使用這個設定時也要留意這個缺點。

### 4\. 重新檢視 Docker Desktop 的資源配置

很容易被忽略的一點，是分配給 Docker Desktop 本身的 CPU、記憶體設定。

**步驟**

1. 開啟 Docker Desktop 的「Settings」→「Resources」
2. 確認 CPU 與 Memory 的分配量（若是 WSL2 後端，則在 `C:\Users\使用者名稱\.wslconfig` 中設定）
3. 如果主機端還有餘裕，就逐步增加後重新啟動
4. 用 `docker stats` 確認是否有改善

即使是往增加配置的方向調整，如果壓迫到主機端的記憶體或 CPU，也可能讓包含 IDE 和瀏覽器在內的整體環境變慢。請小幅度調整，並用相同的測量方法比較效果。

我目前的環境是在 `.wslconfig` 裡這樣寫的。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4133387%2Fef58eaa2-62f2-4e36-87c9-dfa30c7fd525.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4813d513e68079bdd5cedbc910fb9cd8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4133387%2Fef58eaa2-62f2-4e36-87c9-dfa30c7fd525.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4813d513e68079bdd5cedbc910fb9cd8)

### 5\. 重新檢視 WSL2 的檔案共享方式

如果把 Windows 主機上的專案直接做 bind mount，PHP、Node.js、Python 等在大量讀寫小檔案時，檔案存取可能會成為瓶頸。

**步驟**

1. 確認專案是放在 Windows 端（ `C:\...`）還是 WSL2 的 Linux 端（ `\\wsl$\...`）
2. 在 Docker Desktop 的「Settings」→「General」確認是否啟用了 WSL2 後端
3. 將現有工作資料夾備份，或把本機的工作副本搬到 WSL2 的 Linux 端
4. 比較搬移前後同一頁面的回應時間與檔案變更偵測

從 Linux 端檔案系統做 bind mount，通常比從 Windows 端檔案系統做 bind mount 更容易得到較好的檔案存取效能。

尤其是大量使用 bind mount 的 Node.js 或 PHP 專案，通常更能感受到差異。

### 6\. 用.dockerignore 縮小建置內容

如果每次建置都變慢，很常見的情況是把 `node_modules` 、`.git` 之類不必要的檔案也一起送進 Docker 了。

**步驟**

1. 在專案根目錄建立 `.dockerignore` 檔案
```
node_modules
.git
*.log
```
1. 執行建置，確認傳送的建置內容大小有沒有變小
```
docker build .
```

如果建置時顯示的 `Sending build context to Docker daemon` 數值變小了，就代表有生效。

### 7\. 用多階段建置讓映像檔變輕量

如果建置所需工具被原封不動地留在執行用映像檔裡，映像檔就會白白變大，容器啟動與重建也會變慢。

依照所使用的框架與語言，Dockerfile 的寫法會差很多。請先確認執行時需要的共用函式庫、設定檔、憑證、檔案權限後再套用。

**步驟**

1. 將 Dockerfile 分成建置階段與執行階段
```
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM node:20-slim
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
CMD ["node", "dist/index.js"]
```

產物類型會依語言與框架而不同。以 PHP 來說，PHP 擴充、Composer 的 `vendor` 、Apache 或 PHP-FPM 設定等，執行時需要的檔案都必須包含到最終階段。

1. 重新建置並比較映像檔大小
```
docker images
```

不過，如果忘了複製產物或共用函式庫，啟動後就會出現錯誤，所以請確認啟動、資料庫連線、檔案上傳、圖片處理與主要頁面都正常。

### 8\. 選擇輕量版基底映像檔

如果 `FROM` 指定的基底映像檔太大，映像檔的 pull 與容器啟動就會花更多時間。

・Node.js： `node:20` → `node:20-slim`  
・PHP： `php:8.2-apache` → `php:8.2-apache-slim` ，如果不需要 Apache，則可用 CLI 版 `php:8.2-cli`  
・Python： `python:3.12` → `python:3.12-slim`

**步驟**

1. 查看 Dockerfile 第一行的基底映像檔
2. 確認是否有輕量版標籤（ `-slim` 或 `-alpine` ）
3. 將標籤改成輕量版後重新建置
```
docker compose build
```

alpine 系列不是基於 glibc，而是 musl，因此有些相依套件可能無法運作。特別是用 apt 前提來建置 PHP 擴充功能時，要注意套件名稱在 alpine（apk）底下會改變。修改後請確認啟動、資料庫連線、登入、檔案上傳、圖片處理、主要頁面、批次處理都正常。

### 9\. 讓建置快取發揮作用

舊版 Docker builder 有時候只要原始碼稍微改一下，快取就不會命中，導致每次都得完整建置。

**重點**

・先只 COPY 相依性定義檔（例如 package.json、composer.json 等），先執行安裝，再 COPY 其餘原始碼；這樣在原始碼變更時，相依性層仍可保留快取並重複使用

```
COPY composer.json composer.lock ./
RUN composer install
COPY . .
```

在 BuildKit 中，也可以使用 `cache mount` ，讓套件管理器的下載快取能在不同次建置之間重複利用。

```
# syntax=docker/dockerfile:1
RUN --mount=type=cache,target=/root/.npm npm ci
```

**步驟**

1. 確認 BuildKit 與使用中的 builder
```
docker buildx ls
```
1. 在 PowerShell 中啟用 BuildKit 後進行建置
```
$env:DOCKER_BUILDKIT = "1"
docker compose build
```

在 Docker Desktop 裡有時本來就會預設使用 BuildKit，因此就算設定了環境變數，也不一定會有額外效果。

### 10\. 將經常讀寫的目錄切換成 named volume

直接掛載主機目錄的 bind mount，尤其在 Windows 上，每次檔案存取都會牽涉跨越 OS 邊界的處理，因此效能很容易下降。像 DB 資料目錄這種頻繁讀寫的地方，改用 named volume 後，體感通常會有明顯差異。

**步驟**

1. 切換前務必先備份既有資料
```
docker compose exec -T db mysqldump -u root -p mydb > backup.sql
```
1. 修改 `docker-compose.yml` 的 volume 定義
```
services:
  db:
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```
1. 重新建立容器，並從備份還原資料
```
docker compose down
docker compose up -d --force-recreate db
docker compose exec -T db mysql -u root -p mydb < backup.sql
```
1. 確認應用程式是否能正常讀寫資料

`db` 、 `mydb` 、 `/var/lib/mysql` 都只是範例，請替換成實際設定。若執行 `docker compose down -v` ，named volume 也會被刪除，因此若要保留 DB 資料，請不要加 `-v` 。

不只要確認備份有成功，也要確認能還原到另一個本機 volume 或工作副本。

### 總結

・先做資源清理、找出瓶頸、設定日誌上限，絕對不吃虧。不過， `docker system prune` 請先確認刪除對象，並注意不要刪到 DB volume  
・Docker Desktop 的資源配置與 WSL2 檔案共享方式，在資源不足或 bind mount 成為瓶頸時特別有效  
・`.dockerignore` 、多階段建置、基底映像輕量化、善用建置快取，主要是改善建置、拉取、容器重建與啟動時間  
・切換成 named volume 適合 DB、快取等 I/O 成為瓶頸的情況。請務必先備份既有資料，並確認能成功還原  
・請在修改前後測量 `docker system df` 、 `docker stats` 與頁面回應時間，並比較中位數與第 95 百分位

這次我介紹了自己實際做過的 Docker 環境輕量化、加速方法。  
我自己的環境在這樣調整後，運作狀況明顯改善，建置速度與本機環境的頁面顯示也快了很多。

實際能不能感受到效果會依環境而異，因此如果照著這些方法做了還是沒改善，建議可以把 Docker 相關檔案交給 AI 讀取，問問看瓶頸到底在哪裡。

通常它會列出一長串改善建議，但如果不小心全部套用，環境也可能被破壞，所以哪些可以執行、哪些不行，還是得由你自己判斷。

先從安全的方法開始，一個一個試吧！

參考  
・Docker 日誌設定： [https://docs.docker.com/engine/logging/configure/](https://docs.docker.com/engine/logging/configure/)  
・Docker 資源限制： [https://docs.docker.com/engine/containers/resource\_constraints/](https://docs.docker.com/engine/containers/resource_constraints/)  
・Docker WSL2 最佳實務： [https://docs.docker.com/desktop/features/wsl/best-practices/](https://docs.docker.com/desktop/features/wsl/best-practices/)  
・Docker Volume： [https://docs.docker.com/engine/storage/volumes/](https://docs.docker.com/engine/storage/volumes/)

---

原文出處： [https://qiita.com/nolanlover0527/items/24ee07122dd4d0523069](https://qiita.com/nolanlover0527/items/24ee07122dd4d0523069)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aWKOna) [5瀏覽](https://codelove.tw/@tony/post/aWKOna)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aWKOna)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝25

370

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788924689&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaWKOna&amp;pra=5&amp;asro=0&amp;itsi=2&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788924689250&amp;bpp=1&amp;bdt=99&amp;idt=91&amp;shv=r20260908&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788924514%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788924514%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788924514%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;nras=1&amp;correlator=3289333943629&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C122880835&amp;oid=2&amp;pvsid=5484979524659036&amp;tmod=165979714&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=122" title="Advertisement" aria-label="Advertisement"></iframe>