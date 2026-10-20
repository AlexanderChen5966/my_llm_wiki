---
title: "Claude Code整理Obsidian筆記！Karpathy公開LLM知識庫系統，貼一段指令就能建起來"
source: "https://www.bnext.com.tw/article/90530/llm-knowledge-base-obsidian-claude-code"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-04-07
created: 2026-05-13
description: "用 Claude Code 搭 Obsidian，把雜亂筆記變成可查可問的 AI 知識庫，這篇文章告訴你建構步驟！"
tags:
  - "clippings"
status: processed
---
<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?us_privacy=1---&amp;gpp_sid=-1&amp;client=ca-pub-7689852534818818&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1778643742&amp;plat=9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F90530%2Fllm-knowledge-base-obsidian-claude-code&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjEuMCIsImFybSIsIiIsIjE0Ny4wLjc3MjcuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDcuMC43NzI3LjEzOCJdLFsiTm90LkEvQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE0Ny4wLjc3MjcuMTM4Il1dLDBd&amp;abgtt=10&amp;dt=1778643742385&amp;bpp=2&amp;bdt=34&amp;idt=44&amp;shv=r20260511&amp;mjsv=m202605070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dfc12e3aa82034b13%3AT%3D1763599281%3ART%3D1778642532%3AS%3DALNI_MZqxZynIWJ1XywRGxIpwCjPcmkh5Q&amp;gpic=UID%3D0000130f31c4a4e1%3AT%3D1763599281%3ART%3D1778642532%3AS%3DALNI_MYDZbFuviRCgsGvdADPZ2ghPcyIfw&amp;eo_id_str=ID%3D4193c42b505bcde4%3AT%3D1763599281%3ART%3D1778642532%3AS%3DAA-AfjZf15etmO5N_DfqoeF6Fwtl&amp;nras=1&amp;correlator=6763179552203&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1049&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95387509%2C95390680%2C95373848&amp;oid=2&amp;pvsid=8748099519022665&amp;tmod=1471639880&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C31%2C0%2C0%2C1652%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;plas=516x848_l%7C516x848_r&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=119" title="Advertisement" aria-label="Advertisement"></iframe>

[![2026 未來經理人年會](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-05/img-1777689571-11505.gif)](https://edm.managertoday.com.tw/future-manager/?utm_campaign=2026FM&utm_source=web_bn&utm_medium=logo_banner&utm_content=164733&utm_term=channel_all)

2026.04.07 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

## 用Claude Code管理100篇研究筆記！OpenAI共同創辦人公開LLM知識庫系統，貼一段指令就能建起來

用 Claude Code 搭 Obsidian，把雜亂筆記變成可查可問的 AI 知識庫，這篇文章告訴你建構步驟！

[＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude)

---

你存了幾百篇文章，打了幾十份筆記，但要用的時候怎麼也找不到。

這不是你的問題。知識管理最耗人的部分，從來不是「讀」和「想」，而是 **整理** ：把資訊分類、建立連結、讓舊筆記和新資料串起來。

OpenAI 共同創辦人安德烈·卡帕西 (Andrej Karpathy) 最近公開了他的解法： **讓 AI 代替你做這些整理工作** 。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

你只需要把資料丟進去，剩下的摘要、分類、交叉連結、維護——全部交給 AI。他自己用這個方法，在某個研究主題上累積了將近 100 篇筆記、40 萬字，而且查得動。

這篇文章說明這套系統怎麼運作，以及怎麼用最快的方式把它建起來。

## 你需要準備什麼？

**1\. Obsidian** （免費）  
存放筆記的桌面應用程式。所有筆記存在你自己的電腦上，不上傳雲端。到 [obsidian.md](https://obsidian.md/) 下載，安裝後建立一個新的「保險庫」（Vault），這就是你的知識庫所在地。

如果你是第一次使用 Obsidian，可以先參考 [這篇入門教學](https://www.bnext.com.tw/article/84880/obsidian-tutorial-2025) 。

**2\. [Obsidian Web Clipper](https://obsidian.md/clipper)** （免費）  
瀏覽器擴充功能，讓你在看網頁文章時，一鍵把整篇文章存進 Obsidian，格式乾淨不跑版。

在瀏覽器的擴充功能商店搜尋「Obsidian Web Clipper」安裝即可。

**3\. Claude Code** （需要 Anthropic 帳號）  
這是整套系統的核心，也是和一般 AI 工具最不一樣的地方。

一般的 ChatGPT 或 Claude 網頁版只能在對話框裡交換文字。 **Claude Code 則可以直接進入你的資料夾** ，讀取筆記、寫入新內容、更新目錄，就像一個能操作你電腦的 AI 助理，不需要人工複製貼上。

前往 [claude.ai/code](https://claude.ai/code) 下載 Mac 或 Windows 的桌面應用程式。

關鍵是需要Anthropic 帳號，Claude Pro 訂閱方案（每月約 20 美元）包含 Claude Code 的使用權限。如果你對 Claude Code 還不熟悉，可先參考 [這篇入門教學](https://www.bnext.com.tw/article/90451/claude-code) 。

## 最快的起步方式？

準備好之後，打開 Claude Code，把它指向你的 Obsidian 保險庫資料夾。有兩種方式可以讓它幫你把系統建起來：

### 方法一：餵入卡帕西的原始文件（英文）

把以下指令貼給 Claude Code：

```
請根據以下卡帕西的 GitHub Gist，在這個資料夾裡幫我建立 LLM 知識庫系統，並說明我接下來要怎麼使用：
> [https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
```

### 方法二：使用完整架構說明文件

AI 研究者 Elvis Saravia 根據卡帕西的系統整理了一份完整的架構說明文件，並發布在 [dair.ai](https://academy.dair.ai/blog/llm-knowledge-bases-karpathy) 。

把這份文件直接貼給 Claude Code，它就能根據完整的架構說明建立系統，細節比方法一更豐富一些，整體架構如下：

現在把以下指令全部複製，貼給 Claude Code，並在最後加上一句：「請根據以上架構，在這個資料夾裡建立系統，所有筆記使用繁體中文，完成後告訴我怎麼開始使用。」

```
# LLM Knowledge Bases - Architecture (Karpathy)

## Overview
A personal knowledge base system where an LLM acts as a compiler that reads raw source documents and produces a structured, interlinked markdown wiki. No vector databases or embeddings needed at personal scale.

## Phase 1: Ingest
- Obsidian Web Clipper: Browser extension converts web articles into clean .md files with locally downloaded images
- Papers & Repos: arXiv papers, GitHub repos, datasets collected into raw/ staging directory
- raw/ directory: All source documents land here first - the LLM reads from this staging area

## Phase 2: Compile (LLM Compiler)
The LLM incrementally reads raw/ and builds a structured wiki:
- Index & Summaries: Auto-maintained index files with brief summaries of all documents - entry point for queries
- Concept Articles: ~100 articles, ~400K words, organized by topic with backlinks and cross-references
- Derived Outputs: Marp slide decks, matplotlib charts, filed-back query answers
- Backlinks & Cross-links: Auto-generated link graph between concepts, finding connections for new article candidates

## Phase 3: Query & Enhance
- Obsidian IDE: Frontend for browsing the wiki and visualizations
- Q&A Agent: Complex research questions across articles - answers rendered as markdown, slides, or charts
- Search Engine: Vibe-coded naive search over the wiki, usable via web UI or as a CLI tool for the LLM
- Key insight: Outputs from queries get filed back into the wiki - every exploration adds up

## Phase 4: Lint & Maintain
- Scan for inconsistent data
- Impute missing information via web search
- Find connections between concepts for new articles
- Suggest further questions to explore
- After linting, cycle returns to Phase 2 - wiki keeps growing

## Feedback Loops
- Q&A Agent outputs -> filed back into wiki (Derived Outputs)
- Linting results -> enhance the wiki
- Phase 4 cycles back to Phase 2 continuously

## Future Direction
Synthetic data generation from the wiki to fine-tune an LLM so it "knows" the data in its weights rather than just through context windows.

## Tools Used
- Obsidian (IDE + file viewer)
- Obsidian Web Clipper (article ingestion)
- LLM with large context window (compilation)
- Markdown directory structure (wiki storage)
```

兩個方法的效果一樣：Claude Code 會照著說明把整個系統建好，並告訴你後續怎麼操作。不需要手動設定任何東西。

## 這套系統怎麼運作？

整套系統由三個區域和四個動作組成：

三個區域：

- **`raw/` 原始資料夾** ：你蒐集的所有原始素材，只進不改
- **`wiki/` 知識庫** ：AI 整理好的筆記，每個概念一篇，互相連結
- **`index.md` ＋ `log.md`** ：目錄與整理記錄，由 AI 自動維護

四個動作：

1. **蒐集** ：用 Obsidian Web Clipper 把網頁文章存進 `raw/`
2. **整理** ：讓 Claude Code 把原始資料編譯成知識庫筆記
3. **提問** ：對知識庫問問題，把回答也存回去累積
4. **清理** ：定期讓 AI 掃描知識庫，找矛盾、補缺漏

### 蒐集資料

看到一篇好文章，點一下瀏覽器上的 Obsidian Web Clipper，文章就自動存進原始資料夾（ `raw/` ）。PDF、截圖、逐字稿，直接拖進去就好。這個資料夾只進不改，是你所有知識的原始存檔。

### 讓 AI 整理

每次累積了幾篇新資料，在 Claude Code 下一個指令，它會自動讀取原始資料、為每個概念建立或更新筆記、在相關筆記之間加上連結，並更新整座知識庫的目錄。 **整個過程你不需要複製貼上任何東西** ——Claude Code 直接在你的 Obsidian 資料夾裡寫入修改。

### 問問題，把答案存回去

知識庫累積到一定規模，就可以開始對它提問。問題可以很廣：「幫我整理 XX 主題的重點」、「我在這個領域還有哪些盲點」、「把這些筆記的矛盾找出來」。

卡帕西的做法是讓 AI 把回答也整理成新筆記，直接存回知識庫——讓每一次提問都成為知識的一部分，而不是問完就消失。目前他在某個研究主題上的知識庫已達約 100 篇筆記、40 萬字，在這個規模下可以提出相當複雜的問題。

### 定期清理

每隔一段時間，讓 Claude Code 掃描整座知識庫，找出頁面之間的矛盾、過時的資訊、以及值得深入的新方向。AI 在這個環節也很擅長主動建議下一步值得追問的問題。

## 為什麼這樣做有意義？

傳統知識管理工具，無論是 Notion、Roam 還是 Obsidian，都把整理的苦工甩給人做。連結要人建、標籤要人貼、矛盾要人找。大多數人最終放棄，不是因為沒有毅力，而是維護成本超過了回報。

LLM 知識庫把這個成本轉移給 AI：人負責找素材、定方向、問好問題；AI 負責摘要、交叉連結、一致性維護。

卡帕西在公開這套方法時留下一句話：

> 「我認為這裡有空間誕生一個了不起的產品，而不只是一堆雜七雜八的腳本。」

目前這套系統還需要自己動手搭建，但它的邏輯已經完整。選一個方法，把文件餵給 Claude Code，就是開始的第一步。

> 延伸閱讀： [Claude送免費額度！這三種方案都能領，怎麼領？要注意什麼？教學一次看](https://www.bnext.com.tw/article/90528/claude-free-credit-claim-guide)

資料來源： [Andrej Karpathy GitHub Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 、 [Karpathy X 貼文](https://x.com/karpathy/status/2039805659525644595) 、 [Elvis Saravia / dair.ai](https://academy.dair.ai/blog/llm-knowledge-bases-karpathy)

本文初稿為AI編撰，整理．編輯/ 李先泰

[![](https://cdn.bnextmedia.com.tw/img/16x9.png "【謀略應用】商業談判的謀略與藝術│2026劉必榮談判攻略")](https://eventgo.bnextmedia.com.tw/event/detail/e41365u692072ac0add1?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "【謀略應用】商業談判的謀略與藝術│2026劉必榮談判攻略")

[【謀略應用】商業談判的謀略與藝術│2026劉必榮談判攻略](https://eventgo.bnextmedia.com.tw/event/detail/e41365u692072ac0add1?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "【謀略應用】商業談判的謀略與藝術│2026劉必榮談判攻略")

[

活動詳情

](https://eventgo.bnextmedia.com.tw/event/detail/e41365u692072ac0add1?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "【謀略應用】商業談判的謀略與藝術│2026劉必榮談判攻略")

關鍵字： [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude)

往下滑看下一篇文章

tw\_bnext \[Dynamic Article\]-20260508-08:04

arrow\_forward\_ios

閱讀文章

00:00

00:00

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1778227498.324214.mp4?verify=1778642962-txrWgdFnifk8Q25oBHqxviQPJPb2%2Bh1zDThlGzKWdIs%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1778227498.324214.mp4?token=CL_OQdXhO9Y1s6KMvWLWtHZU8hVqb6VJeGtWbMJ5lBc&amp;expires=1778729362"></video>

即時熱門文章

[1 光進銅退！輝達重金押寶兩巨頭，下一代全光網路商機爆發，哪些台廠先布局？](https://www.bnext.com.tw/article/90887/all-photonics-network) [2 ChatGPT Image 2.0指令大全！40組提示詞一次整理，LINE貼圖、簡報封面、插畫菜單全搞定](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [3 Meet大南方2026徵展啟動，免費早鳥席次6/8截止！6大解方區直球接傳產、ESG、醫療照護痛點](https://www.bnext.com.tw/article/90895/meet-greater-south-2026) [4 觀點｜台電Logo在吵什麼？96萬換標爭議背後，解析Gap、英航也曾踩過的「設計紅線」](https://www.bnext.com.tw/article/90898/taipower-logo-design) [5 做會議紀錄好痛苦？教你用「NotebookLM＋一組提示詞」，散會3分鐘就能發信！](https://www.bnext.com.tw/article/90875/notebooklm-3-steps-ai-meeting-minutes-and-email-notes) [6 00403A掛牌首日成交逾449億！這檔台股主動ETF在紅什麼？00403A成分股、配息懶人包](https://www.bnext.com.tw/article/90914/united-etf-00403a-taiwan-upgrade-50-active)

![Meet大南方2026徵展啟動，免費早鳥席次6/8截止！6大解方區直球接傳產、ESG、醫療照護痛點](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/5fqj-1778214500.png?w=900&output=webp)

2026.05.11 | [創新創業](https://www.bnext.com.tw/categories/entrepreneurship)

Meet大南方2026徵展啟動，免費早鳥席次6/8截止！6大解方區直球接傳產、ESG、醫療照護痛點

Meet大南方今年不談空泛願景、不畫技術大餅，而是把AI應用、智慧製造、ESG碳管理這些詞彙，翻譯成每一位企業主聽得懂的語言、用得上的解方。

[＃創新創業](https://www.bnext.com.tw/tags/%E5%89%B5%E6%96%B0%E5%89%B5%E6%A5%AD)

---

分享

> Meet大南方2026不是一場讓你「露個臉、拿名片」的展會。  
> **Meet大南方2026是一場讓你「找到真正客戶」的展會。**

2025年，我們用一組數字證明了這件事：雙日觀展近1.3萬人次、290+組團隊參展、促成352組商機媒合。這個媒合數字是2024年的2.7倍。同一批展商、同一個場地，媒合效率在一年內跳了一個量級。

這代表什麼？意味著來到Meet大南方的人， **越來越不是來「逛展」，而是來「談生意」。**

過去5年的經驗，我們有了一個心得，南台灣的企業主不是來聽矽谷故事的，他們是帶著明確的問題走進展場。

![#0 2026Meet大南方徵展](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/mtpc-1778214430.jpg?w=1200&output=webp)

2025 Meet大南方吸引近300家新創參展，雙日觀展近1.3萬人次。

圖／ Meet創業小聚

所以今年，我們不再單純以「趨勢」和「創業」為號召，正式把展會定位為「 **Meet Your Best Solution** 」。不談空泛願景、不畫技術大餅，而是把AI應用、智慧製造、ESG碳管理這些詞彙，翻譯成每一位企業主聽得懂的語言、用得上的解方。

對解方提供商意味著什麼？代表來到你攤位前的人並不是路人，極有可能就是來找答案的老闆。

## 為什麼你應該來？因為這是「南台灣市場的稀缺入口」

2026年8月28日（五）至29日（六），Meet大南方將於高雄展覽館展開第6屆展會，並於即日起正式啟動招商。

對正在開發南部市場的解方商來說，Meet大南方 **不只是「一次曝光機會」** ，還代表以下4件事：

**第一，精準接觸南部企業決策者**  
傳產老闆、二代接班人、廠長、中小企業主……這些人平常不會出現在台北的科技活動，也很難透過線上管道觸及。為了把這群人真正拉到展會現場，過去幾個月《創業小聚》每月固定南下舉辦實體小聚，攜手高雄市經濟發展協會、高雄市建築經營協會、高雄市青年企業家協會與中山EMBA等組織，一場一場把在地企業主凝聚成一個社群。這群人不是展會當天才第一次聽到Meet大南方，他們早已是Meet大南方的一份子。

**第二，從cold mail到warm lead的捷徑**  
兩天展期裡，除了攤位對話，我們會透過企業媒合會、投資人媒合會、新創交流之夜等機制，主動把你和潛在客戶、投資人湊到同一張桌子上。2025年這套機制為展商促成352組商機媒合，是前一年的2.7倍。現場示範、現場對話、現場加LINE，一次抵過3個月的線上開發。

![#2 2026Meet大南方徵展](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/a519-1778214315.jpg?w=1200&output=webp)

南台灣的企業主帶著明確的問題走進展場，找他們最迫切需要的解方。

圖／ Meet創業小聚

**第三，解方區分類帶來精準人流**  
6大解方區依照企業痛點分類，觀展者按需求找到對應展區。來到你攤位的人，不會只有過路客，也有正在找你這類解方的決策者。

**第四，南部市場的最低成本試水溫**  
對想評估是否投入南部市場的團隊，到高雄準備一個攤位的成本，遠低於派業務長駐南下3個月。2天內，你會得到足以判斷市場值不值得投入的第一手資訊。

值得一提的是，2025年Meet大南方的媒體曝光總效益超過新台幣3,000萬元，涵蓋《工商時報》、《經濟日報》、《數位時代》等91家媒體、共323則網路新聞露出。展商的品牌能量會自然搭上這波媒體浪潮。雖然這不該是你來的主要理由，但它確實是附贈的。

## 六大解方區：每一區都是一組企業痛點

今年我們把展區濃縮為6大「解方區」，每一區都直球對應一組具體的企業痛點：

**智慧製造與產線升級 解方區** ——給自動化設備、AOI/AI視覺檢測、MES、工業物聯網、系統整合的團隊。對應痛點：缺工、良率不穩、設備老舊、排程沒效率。

**數位管理與企業效率 解方區** ——給ERP/CRM/HRM、AI Agent、AI辦公工具、RPA、SaaS、FinTech支付、資安的團隊。這是最跨產業的一區，涵蓋所有企業的效率需求。

**醫療健康與高齡照護 解方區** ——給遠距醫療、AI診斷、長照科技、健康數據、醫療管理系統的團隊。對應高齡化社會下的照護人力缺口與醫療數位化缺口。

![#1 2026Meet大南方徵展](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/fc44-1778214315.jpg?w=1200&output=webp)

今年策劃6大「解方展區」，直接對應企業經營現場最常見的問題情境。

圖／ Meet創業小聚

**淨零碳排與綠能永續 解方區** ——給碳盤查SaaS、ESG顧問工具、綠能設備、能源管理系統、循環經濟的團隊。供應鏈碳足跡要求已經壓到南部製造業頭上，這一區的需求只會愈來愈急。

**品牌轉型與跨境行銷 解方區** ——給電商平台、跨境物流、MarTech、AI行銷工具、品牌顧問的團隊。南部有太多做代工做到品牌老化、想做電商卻不知從何開始的業者。

**未來零售與餐飲科技 解方區** ——給POS、餐飲SaaS、無人商店、會員CRM、供應鏈方案的團隊。搭配「大南方餐飲創業沙龍」同步導流。

## 方案與招商資訊

**2026 Meet Greater South亞灣新創大南方**  
時間：8/28（五）、8/29（六）  
地點：高雄展覽館北館  
官網： [https://meetgreatersouth.tw/](https://meetgreatersouth.tw/?utm_campaign=2026mgs&utm_source=pr&utm_medium=02)

> **徵展正式起跑，新創享專屬免費方案！**  
> **早鳥優惠至6/8，報名收件至7/3** 　>> [瞭解詳情](https://meetstartup.pse.is/92hpcl)  
>   
> 報名採審核制。請至Meet Online更新公司資料及填寫參展報名表單，主辦單位將以Email通知審核結果。若您的解方尚在評估是否合適，歡迎先聯繫我們，一起確認哪一個解方區最貼近你的目標客戶。

**企業帶著問題來，我們希望你帶著解方來。  
8月28-29日，高雄展覽館見。**

今年Meet大南方將於8/28、8/29在高雄展覽館盛大舉辦，現已開放參展報名。

圖／ Meet創業小聚

關鍵字： [＃創新創業](https://www.bnext.com.tw/tags/%E5%89%B5%E6%96%B0%E5%89%B5%E6%A5%AD)

即時熱門文章

[1 光進銅退！輝達重金押寶兩巨頭，下一代全光網路商機爆發，哪些台廠先布局？](https://www.bnext.com.tw/article/90887/all-photonics-network) [2 ChatGPT Image 2.0指令大全！40組提示詞一次整理，LINE貼圖、簡報封面、插畫菜單全搞定](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [3 Meet大南方2026徵展啟動，免費早鳥席次6/8截止！6大解方區直球接傳產、ESG、醫療照護痛點](https://www.bnext.com.tw/article/90895/meet-greater-south-2026) [4 觀點｜台電Logo在吵什麼？96萬換標爭議背後，解析Gap、英航也曾踩過的「設計紅線」](https://www.bnext.com.tw/article/90898/taipower-logo-design) [5 做會議紀錄好痛苦？教你用「NotebookLM＋一組提示詞」，散會3分鐘就能發信！](https://www.bnext.com.tw/article/90875/notebooklm-3-steps-ai-meeting-minutes-and-email-notes) [6 00403A掛牌首日成交逾449億！這檔台股主動ETF在紅什麼？00403A成分股、配息懶人包](https://www.bnext.com.tw/article/90914/united-etf-00403a-taiwan-upgrade-50-active)

[![AI全球100+台灣20](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-04/47ru-1775031962.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130126)