---
title: "使用 Raycast 打造超級生產力（一）- 為什麼我們需要 Raycast？"
source: "https://vocus.cc/article/661943e1fd897800016eb0b9"
author:
  - "[[FLC]]"
published: 2024-05-13
created: 2026-05-28
description: "Raycast 是在 2020 年推出的一款 Mac 生產力工具，旨在提高使用 Mac 工作的效率，讓使用者可以更專注在核心的工作上。在上手 Raycast 之前，我們首先要了解 Raycast 想解決的問題，也就是在現代化的工作流程裡，有哪些情境會降低我們的生產力？"
tags:
  - "clippings"
---
![zoomable](https://images.vocus.cc/c92953d7-48f2-418f-a14a-f83c765e43e0.png)

Raycast 舊版官網的介紹裡，描述自己是超級生產力（Supercharged Productivity）

## 前言

[Raycast](https://www.raycast.com/) 是在 2020 年推出的一款 Mac 生產力工具，旨在提高使用 Mac 工作的效率，讓使用者可以更專注在核心的工作上。隨著時間發展，除了添加更多實用功能之外， Raycast 也變成一個生產力工具平台，開發者可以開發各種生產力工具的擴充與 Raycast 整合。 2023 年，Raycast 跟上 AI 潮流，整合了多樣的 AI 功能，進一步讓使用者能夠提高更多工作效率。

> 接下來的文章內容，主要說明在我們平常的工作流程中，生產力是如何被不必要的操作給降低的，了解工作效率降低的原因之後，就可以理解 Raycast 想要解決的生產力問題。關於 Raycast 的介紹與詳細體驗，則會在之後文章提到。

---

![vocus｜新世代的創作平台](https://images.unsplash.com/photo-1501250987900-211872d97eaa?crop=entropy&cs=srgb&fm=jpg&ixid=M3wyNTY4N3wwfDF8c2VhcmNofDEyfHxnb29nbGUlMjBzZWFyY2h8ZW58MHx8fHwxNzEzMDI0ODkyfDA&ixlib=rb-4.0.3&q=85)

[Benjamin Dada on Unsplash](https://unsplash.com/@dadaben_?utm_source=vocus&utm_medium=referral)

![zoomable](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.unsplash.com%2Fphoto-1501250987900-211872d97eaa%3Fcrop%3Dentropy%26cs%3Dsrgb%26fm%3Djpg%26ixid%3DM3wyNTY4N3wwfDF8c2VhcmNofDEyfHxnb29nbGUlMjBzZWFyY2h8ZW58MHx8fHwxNzEzMDI0ODkyfDA%26ixlib%3Drb-4.0.3%26q%3D85&width=740&sign=2UEwBiUvRwSxoFYwHgMb42-zlqIrDjCh14uW8uff4hM)

## 哪些情境降低了我們的生產力？

為什麼會看到廣告

<iframe frameborder="0" src="https://74df6ac2a759a604cfaf9207e368283b.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>在介紹如何使用 Raycast 之前，我們得先知道在一般使用電腦工作的流程裡，有哪些降低工作生產力的情境。

想像這個常見的場景，你正在電腦上用 Google Docs 上撰寫工作文件，忽然需要使用 Google 搜尋確認一些細節，所以你做了下面這些動作：

1. 你的手從鍵盤上移到滑鼠；
2. 移動游標到瀏覽器分頁（Tab）位置，新增一個新的分頁；
3. 瀏覽器分頁展開，游標移動到搜尋框，點擊搜尋框；
4. 你的手再次從滑鼠移動回鍵盤，敲擊輸入你要查詢的內容；
5. Google 搜尋頁面出現，你的手又一次移動到滑鼠，瀏覽並點擊了其中一個搜尋結果。

上面這個幾乎每天工作都在進行，看起來微不足道的流程，是怎麼對我們的工作效率產生影響？

- Google Docs 與 Google 搜尋的介面佈局是有差異的，從 Google Docs 的編輯介面切換到 Google 搜尋頁面時，大腦需要花時間重新適應畫面，這是一種 ****「情境切換」（Context Switching）**** ，對大腦來說是一種認知負擔的增加，長時間工作就有可能消耗專注力而導致效率降低。
- Google Docs 與 Google 搜尋的操作方式也有差異，Google Docs 主要是鍵盤打字編輯，而 Google 搜尋則是滑鼠捲動與點擊佔據較多時間。從敲擊鍵盤到滑鼠移動，還有在鍵盤與滑鼠之間來回切換，這種操作工具的轉換與適應也是前面提到的「情境切換」，也會耗損我們的專注力。
- 除此之外，眼睛搜索畫面上瀏覽器頁籤以及搜尋框的位置，以及移動滑鼠游標的過程（我們可能都沒有意識到，移動滑鼠其實是需要計算移動方位與速度的，與鍵盤一致等速的操作並不相同），同樣會對大腦造成認知負擔，降低工作效率。

上述這些過程（適應介面、轉換操作、視覺搜索、游標移動），除了可預期的會增加時間外，更關鍵的是這些操作都有可能 ****額外增加大腦認知負擔，長時間工作後消耗專注力，因此降低工作的生產力**** 。

除此之外， ****從開始到最終工作目標之間進行的操作，都可能有其它干擾打斷原本的工作，阻礙我們進入心流的狀態，操作越多，就越容易分心**** 。試著想想有多少次打開瀏覽器要進行 Google 搜尋時，卻看到之前沒關掉的社群網站分頁而忍不住瀏覽起來，就可以理解我們的大腦有多麼容易受到外在不相干事物干擾。

上述的 Google 搜尋情境較為單純 ，或許還不足以讓人意識到對生產力的消耗，讓我們來看一個較為複雜的情境：

> 你是一位軟體工程師，正在進行程式開發的時候，對於專案的任務需求有點疑慮，於是你到團隊的 **「專案進度管理工具」** 查看對應需求的任務內容，並在裡面找到跟這個任務關聯的專案需求文件連結（專案需求的細節整理在另一個 **「專案文件與資訊平台」** 的服務），平台裡面有更詳細的說明。你確認了自己的疑慮，並覺得有必要與 PM 討論，於是你透過 **「通訊程式」** 傳送了訊息給 PM 表達你的疑慮。

想像一下這一切可能要花多少力氣，你要打開一個新的 **「專案進度管理工具」** 瀏覽器分頁，通常針對不同專案，會有多個不同的任務看板，所以得先找到對應的看板，接著進入看板內搜尋任務名稱，或是透過滑鼠滾輪滾動頁面尋找任務。找到對應的任務之後，檢視任務內容，找到專案需求文件連結，跳轉到 **「專案文件與資訊平台」** 內的需求文件頁面，查看細節確認之後（記得複製需求文件頁面的連結，待會會用上），你切換到 **「通訊程式」** （滑鼠點選，或是快捷鍵切換），搜尋你要傳送訊息的 PM，點選，打上一串訊息說明，然後貼上需求文件連結。如果你也想順便分享任務連結，還得再回到「 **專案進度管理工具」** 頁面複製，然後再回到 **「通訊程式」** 貼上。

這個過程中的步驟都有適應介面、轉換操作、視覺搜索、游標移動等等行為，每切換到不同工具，大腦就得「情境切換」，要花時間適應新的介面與操作方式， **「專案進度管理工具」** 跟 **「通訊程式」** 是截然不同介面與操作方式，每當切換到另一個工具，就是一次的認知負擔，而在一天的工作時間內，你可能要在數十個工具服務間切換上百次甚至上千次。

> 事實上，有 [研究數據指出](https://hbr.org/2022/08/how-much-time-and-energy-do-we-waste-toggling-between-applications) ，平均在不同工具服務切換一次的時間成本大約是 2 秒，而一天會進行將近 1200 次的切換。

在上面描述的情境裡，可以看到使用了至少三種類型的工具： **專案進度管理** ，例如 [Jira](https://www.atlassian.com/software/jira) 、 [Asana](https://asana.com/) 或 [Trello](https://trello.com/) ； **專案文件資訊平台** ，例如較為專業的 [Confluence](https://www.atlassian.com/software/confluence) ，或是使用 [Notion](https://www.notion.so/) 、Google Docs 等來管理專案需求與開發文件等等； **通訊程式** ， [Microsoft Teams](https://www.microsoft.com/microsoft-teams) 、 [Slack](https://slack.com/) 等都是常見的團隊溝通工具。還有其他不在這個情境裡，但也被廣泛使用的各式工具服務，如視訊會議軟體（ [Google Meet](https://workspace.google.com/products/meet/) 、 [Zoom](https://zoom.us/) ）、電子郵件、筆記軟體、簡報程式；各種 **職務專屬的工具** ，像是 UI/UX 設計師的設計軟體（例如 [Figma](https://www.figma.com/) ） 或是工程師的程式編輯器 （例如 [VSCode](https://code.visualstudio.com/) ）與程式碼代管服務（例如 [Github](https://github.com/) ） 等等；當然，還有各種方興未艾的 AI 工具們。

這些工具都是「生產力工具」，越來愈多現代化的公司使用這些工具來推進專案並打造產品。你能想像你究竟用了多少生產力工具嗎？它們確實讓我們在工作上帶來了許多方便，也更有利於團隊協作與管理。然而諷刺的是， ****我們得打開許多瀏覽器頁籤，或是使用不同的應用程式，這些工具彼此切換的過程中，反而不知不覺間降低了我們的工作效率**** 。 ****這些操作不僅消耗時間，降低了你在核心工作上的專注力，更可能讓你感到煩躁挫折，一整天工作下來，覺得自己忙得焦頭爛額，卻仍然一事無成**** 。

![對電腦工作者來說，數量眾多的瀏覽器分頁應該一點都不陌生。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fc0ac6647-174a-4fc9-b964-9061ccffdbde.png&width=740&sign=-trElvu_HrrdZV-XgsMDde6mm3COiaekUTmoqjtiIIo) ![zoomable](https://images.vocus.cc/c0ac6647-174a-4fc9-b964-9061ccffdbde.png)

對電腦工作者來說，數量眾多的瀏覽器分頁應該一點都不陌生。

---

## 如何避免生產力降低的情境？

要解決前述生產力降低的問題，有非常多的面向，如果以操作這些工具服務的角度來看，核心的概念是 ****盡可能地減少不必要的操作**** （例如在使用 Google 搜尋的場景裡，真正核心的行為是輸入關鍵字與瀏覽，而新增瀏覽器分頁與尋找搜尋框或移動游標等都是不必要的操作），也就是要 ****盡可能降低「情境切換」的成本**** 。

一個做法是在同一個軟體工具裡完成類似性質的工作，或是利用這些工具間的整合功能降低切換成本。例如 Microsoft Teams 本身就有傳送訊息、視訊會議與行事曆的功能，你可以在裡面同時安排行程與開會，而不需要再另外尋找視訊會議或行事曆軟體。Jira 跟 Confluence 雖然本質上是不同的服務，但它們隸屬與同一個開發商，並且進行了很好的整合，你可以輕易的在 Jira 的任務裡關聯到 Confluence 的需求文件。這種做法的侷限是：一個服務很難涵蓋所有功能，有些特定需求還是需要使用其他工具來達成；除此之外，也不是所有服務都支援彼此互相整合。

另一個做法是設定鍵盤快捷鍵來快速切換到各種工具，同時有效率的在工具內進行操作，減少鍵盤與滑鼠之間的來回切換。這種做法的優點很直觀，但缺點也同樣明顯，那就是得記上大量的快捷鍵指令，以及每當使用新工具，就得設定並學習新的鍵盤指令，並且需要工具提供一定程度的快捷鍵支援。

我們需要一個更彈性的做法，不需要記憶太多快捷鍵或手勢，也不需要軟體功能全面或彼此深度整合，就可以 ****有效率地讓我們快速「啟動」不同工具進行操作**** ，並且要能夠 ****按照個人需求打造客製化的「啟動」流程**** 。

我們需要一個 ****彈性高效的「啟動器」（Launcher）**** ，就像是 ****Raycast**** ，它正是為了這樣的需求而設計。

---

## 接下來，Raycast 登場 ⋯⋯

我會在下一篇文章簡介 Raycast 的發展，探討其背後的設計原理，並上手體驗 Raycast 的功能使用。