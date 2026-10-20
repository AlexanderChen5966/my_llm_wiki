---
title: "⚔️ Go 與 Java：極簡主義者對上企業老將"
source: "https://codelove.tw/@tony/post/qv2M2q"
author:
published:
created: 2026-05-27
description: "**沒有立場，沒有目的。只有兩種語言的人走進酒吧，我們看著他們發生什麼事。**---目錄--- [準備工作](#the-setup)- [競爭者](#the-contenders)- [簡單起源故事](#a-quick-origin-story)- [語言哲學：複雜性與簡..."
tags:
  - "clippings"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月22日](https://codelove.tw/@tony/post/qv2M2q "2026-05-22 09:15:11")

**沒有立場，沒有目的。只有兩種語言的人走進酒吧，我們看著他們發生什麼事。**

---

## 目錄

## ⚔️ 設定

我決定寫這篇文章，因為我最近被各種語言比較的內容淹沒了，這些人就像在比較宗教信仰一樣。 “這個比那個好，那個比這個好”，而他們大多只是為了博取眼球、激起憤怒。

我寫 Java 程式碼已經很久了。很早就愛上了它——它處理線程的方式、並發模型，以及圍繞它建置的整個生態系統，一切都讓我非常 *著迷* 。有一段時間，我真的以為 Java 是執行緒處理終於變得實用，不再是惡夢的語言。

坦白說：我以前想學Java也是因為，那時候懂Java就代表程式設計師會在聚會上誇你。這其中肯定有虛榮心作祟。 （好吧，其實是 *相當* 虛榮。）但我現在還是選擇用Java，其實是出於技術原因，虛榮心只是 *其中一部分* 。

後來 Go 出現了。我大約兩年前開始用它寫程式碼，一直很喜歡。在工作中，我用它搭建了兩個分散式系統。但是它需要你手動 *操作* ，語法第一次看的時候會感覺很奇怪很陌生，而且它的生態系統給人一種「我希望這個庫下個月不會停止維護」的感覺。

我不是討厭 Go 語言。我真心喜歡用它。

所以，這就是我寫這篇文章的原因——（嚴格來說，這是大約一周前我在一堂非常無聊的課上寫的）——目的是真正分析這些語言的優勢所在，這樣你就可以做出比“大家都用 X，所以我們也應該用 X”更明智的選擇。

---

## 🥊 競爭者們

目前，後端開發領域充斥著兩種語言。一種語言誕生於迪斯可時代，挺過了網路泡沫破滅，最後成為全球一半企業軟體的支柱。另一種語言則是由Google工程師們開發，他們厭倦了漫長的 C++ 編譯等待，最終成為了 Docker、Kubernetes 以及現代基礎設施一半的幕後功臣。

Java 和 Go。老手與極簡主義者。大教堂與工具棚。

兩者之間沒有絕對的優劣。它們各自在不同的領域都表現出色。這篇文章的目的並非要評判孰優孰劣，而是幫助你了解它們各自的 *優勢* 所在，以便下次有人問「我們用哪個技術棧？」時，你能做出更明智的選擇。

讓我們開始吧。

---

## 🏁 一個簡短的起源故事

**Java** 誕生於1995年，由 **詹姆斯高斯林** 領導的Sun Microsystems公司開發，其核心理念是： *一次編寫，到處執行* 。 JVM意味著編譯後的字節碼可以在任何機器上執行。這在當時是革命性的。 Java乘著這股浪潮迅速佔領了企業級市場，並一直保持著領先地位。

**Go** （或 Golang）由 **Rob Pike** 、 **Ken Thompson** 和 **Robert Griesemer** 於 2009 年在 Google 建立。這三位擁有的程式語言經驗遠超過我們大多數人一生所能累積。他們苦惱於 C++ 的編譯時間嚴重影響了他們的工作效率。他們的解決方案是什麼？一種編譯速度快、執行速度快，且足夠簡單易用，即使犯錯也不會造成太大問題的語言。

不同的時代，不同的問題，不同的理念。

---

## 🧠 語言哲學：複雜性與簡潔性

這兩種語言最顯著的差異就在於此。不是語法上的差異，而是 *世界觀* 的差異。

**Java** 相信應該提供工具。大量的工具。泛型、繼承、抽象類別、介面、註解、lambda 表達式、流、可選型別、記錄、密封類別。 Java 為每一種模式都提供了解決方案，而每個解決方案都對應著一種模式。它相信你能明智地將它們組合起來。

**Go** 語言秉持著「精簡工具」的理念。它沒有類別（只有結構體），沒有繼承，直到最近（Go 1.18，2022 年）才引入泛型。它沒有異常，只明確地傳回錯誤值。 Go 團隊的理念近乎極簡主義。如果某個功能可能被濫用，他們寧願不將其加入程式碼。

```
// Go error handling: explicit, everywhere, always
file, err := os.Open("data.txt")
if err != nil {
    return fmt.Errorf("failed to open file: %w", err)
}
defer file.Close()
```
```
// Java: exceptions handle the unhappy path separately
try {
    var file = new FileReader("data.txt");
    // do stuff
} catch (IOException e) {
    throw new RuntimeException("failed to open file", e);
}
```

兩種方法都沒錯。 Java 的異常模型保證了正常流程的清晰性。 Go 的明確錯誤處理機制意味著你 *不能* 忘記處理失敗。編譯器不允許你在沒有明確指示的情況下忽略錯誤值。

Go 的程式設計理念是寫出新團隊成員第一天就能讀懂的程式碼。 Java 的程式設計理念是編寫出能夠精確建模真正複雜領域的程式碼。

**如果您對此感興趣，不妨深入了解一下** 。 Go [語言的設計常見問題解答](https://go.dev/doc/faq) 和 [Java 語言的演進歷程](https://openjdk.org/projects/amber/) ，講述了語言發展歷程中截然不同的故事。

---

## ⚡ 效能：JVM 與原生二進位檔案

Go 語言編譯成 **原生二進位** 。執行 `go build` ，就能得到一個獨立的、幾毫秒內即可啟動的可執行檔。這就像是遞給別人一把刀——他們直接就用了。

Java 執行在 **JVM** 上，這就好比直接把一個完整的廚房交給別人。它需要一些設定時間（JVM 初始化），也需要一段預熱期，在此期間 JIT 編譯器會了解你經常執行哪些程式碼（並開始優化它們），但一旦它掌握了情況，就能產生真正具有競爭力的機器碼，在持續的工作負載下，其效能有時甚至優於 Go 語言。

| 場景 | Go | Java |

| ---------------------------------- | --------------- | ----------------------------------- |

| 冷啟動 | 🟢 毫秒 | 🟡 秒（使用 GraalVM 可提升） |

| 峰值吞吐量（預熱後） | 🟡 速度極快 | 🟢 可與圍棋匹敵或超越圍棋 |

| 記憶體佔用 | 🟢 小 | 🟡 更大的基線 |

| 無伺服器/短生命週期進程 | 🟢 自然契合 | 🟡 JVM 開銷過大 |

| 長期執行的服務 | 🟡 很棒 | 🟢 即時優化帶來回報 |

**權衡之下：** Go 可預測的即時啟動特性非常適合需要頻繁啟動和停止的環境。如果進程執行數週，Java 的啟動成本就消失了——JIT 預熱只會進行一次，之後程式碼就會不斷優化。

如果你想要 Java 的生態系統和 Go 的啟動速度，可以使用 GraalVM 原生鏡像，但這會增加建置的複雜度。它只是一個橋樑，而非最終解決方案。

如果您喜歡研究數字 **，不妨深入了解** [TechEmpower 的基準測試](https://www.techempower.com/benchmarks/) 。

---

## 🔀 並發：Goroutine 與虛擬線程

Go 的並發能力曾經是其他開發者遙不可及的夢想。 Goroutine 是 **一種** 輕量級的、類似 GreenThread 的並發機制，由 Go 執行時間自動管理。你可以輕鬆建立數萬個 Goroutine：

```
// Launch 10,000 concurrent tasks. No ceremony.
for i := 0; i < 10_000; i++ {
    go func(id int) {
        doSomethingBlocking(id)
    }(i)
}
```

通道是你的通訊層——它們讓 goroutine 變得真正 *優雅，* 而不僅僅是快速：

```
ch := make(chan string)

go func() {
    ch <- "hello from another goroutine"
}()

msg := <-ch
```

這個思考模型（goroutine + channel）成為了 Go 語言的基石。它使得高並發系統在維運上變得易於上手。正因如此，Docker、Kubernetes、Prometheus——所有需要處理數百萬個 goroutine 的基礎設施——都是用 Go 語言編寫的。

**Java 在這方面遇到了問題。** 多年來，對於“如何處理成千上萬個並發請求？”這個問題，答案要么是“為每個請求建立一個線程”，要么是“使用線程池並祈禱一切順利”。雖然這種方法可行，但總是 *覺得* 不太對勁。你會感覺這門語言在跟你作對。

然後，Java 21 引入了 **虛擬執行緒** 。其理念與 goroutine 相同——輕量級、由 JVM 管理的並發。但關鍵在於：它們看起來與普通的 Java 執行緒完全一樣。沒有新的語法，也沒有新的思考模型：

```
// Java 21: 100,000 virtual threads. Same old executor API.
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 100_000).forEach(i ->
        executor.submit(() -> doSomethingBlocking())
    );
}
```

**真正的差別在於：** Go 要求你用全新的方式思考。 Goroutine 和通道的確是一種非常優雅的範式，但它們與大多數語言處理並發的方式截然不同。 Java 的虛擬執行緒讓你可以繼續沿用舊有的思維方式－提交任務，然後就不用管了，讓執行時間來處理執行緒。

從零開始建置時，Go 的方法能產生更優雅的並發程式碼。而 Java 的方法則較實用，適用於已有阻塞程式碼，或不想為了處理並發請求而學習新的並發理念的情況。

兩者都解決了同一個問題。 Go 語言先解決了這個問題，而且更優雅。 Java 語言後來也解決了這個問題，而且「你不需要做任何修改」。

**深入了解：** [Go Blog 上的 goroutines](https://go.dev/blog/goroutines-and-channels) 或 [JEP 444：虛擬線程](https://openjdk.org/jeps/444) 。

---

## 🌲 生態系與圖書館：森林 vs 工具棚

Java 的生態系非常 *龐大* 。我指的是 Maven Central 上數以百萬計的元件。無論你需要什麼，都能找到。資料庫驅動程式、HTTP 用戶端、支付處理器、機器學習框架——成熟的選擇眾多，可能比你想要的還要多。單單 Spring 生態系統本身就是一個獨立的平台。 Spring Boot、Spring Data、Spring Cloud、Spring Security，這些都足以讓團隊成員終身受用。

**權衡之下：** 資源過多反而會讓人束手無策。你得在 47 個 JSON 函式庫之間做選擇，而且還得不斷猶豫不決。一個「簡單」的 Spring Boot 專案會引入數百個傳遞依賴。你就像在管理一片森林，有時甚至看不到其中的樹木。

Go 的生態系統更年輕，也更完善。標準庫 *確實不錯* ——HTTP 伺服器、JSON 編碼、加密、測試等功能都達到了生產級標準，並且已經內建其中。社區也用一些優秀的包填補了空白，例如 `gin` 、 `echo` 、 `gorm` 和 `cobra` 。但有時你會遇到一些棘手的問題。例如某個小眾領域，那裡什麼都沒有，現在只能自己動手了。

**權衡之下：** 你需要做的決策更少，需要擔心的依賴項也更少，而且你的二進位檔案更小。但有時，你建構的某些功能是現有生態系尚未解決的。

關鍵在於：對於 **小型、邊界明確的服務** （例如 Webhook、速率限制器、健康檢查器、內部工具），Go 的極簡主義設計概念能夠保持程式碼的簡潔易懂。你只需取得標準庫，或許再增加一個特定的包，即可完成開發。而對於 **複雜的企業系統** （例如具有使用者角色、稽核追蹤、合規性日誌記錄和支付整合的多租戶 SaaS），Java 的生態系統可以為你節省數月的開發時間。 Spring Data 可以處理那些原本難以建構的資料庫複雜性。 Spring Security 可以處理那些原本需要耗費大量時間才能正確實現的身份驗證場景。

```
// Go: 8 lines, no dependencies
http.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
    fmt.Fprintln(w, \`{"status": "ok"}\`)
})
http.ListenAndServe(":8080", nil)
```
```
// Spring: more setup, but it's assuming you'll build an actual system on top
@RestController
public class HealthController {
    @GetMapping("/health")
    public Map<String, String> health() {
        return Map.of("status", "ok");
    }
}
```

當你真正需要簡潔性時，Go 版本更簡單。而當複雜性不可避免時，Spring 版本則能帶來顯著優勢。

**如果您想深入了解，請造訪：** [pkg.go.dev](https://pkg.go.dev/) 或 [mvnrepository.com](https://mvnrepository.com/) （警告：您可能會感到不知所措）。

---

## 🛠️ 工具鏈：Go 的嚴謹性 vs Java 的自由發揮

Go 自備一套帶有 *特定規範的* 標準工具鏈：

- `go fmt` ：格式化你的程式碼。這是必須的。所有人的 Go 程式碼看起來都一樣。
- `go test` ：內建測試功能，無需框架
- `go vet` ：發現常見錯誤
- `go mod` ：依賴管理，自 Go 1.11 版本起內建。
- `go build` ：一條指令，一個二進位文件

關於 Go 工具集不存在任何爭議。它就 *在那裡* ，它運作良好，而且整個 Go 社群都在使用相同的工具。

Java 的工具鏈更像是「選擇你自己的冒險」遊戲：

- 建置工具：Maven 或 Gradle（自 2012 年以來一直存在爭議）
- 測試：JUnit + Mockito + AssertJ + 可能還有 Testcontainers + 可能還有 Spock
- 格式：Checkstyle？ Google Java 格式？還是您客戶 2015 年的個人偏好？
- 依賴管理：Maven Central、JitPack，或是你們公司內部運作的、沒人完全懂的 Nexus 系統。

Java 工俱生態系統功能強大、靈活，但至少佔用了新開發人員 30% 的入職培訓時間。

**權衡之下：** Go 語言的剛性工具鏈意味著可以減少在風格和配置上的爭論，但如果遇到特殊需求，靈活性也會降低。 Java 語言的靈活工具鏈意味著你 *可以* 根據具體情況進行最佳化，但你需要預先做出更多決策。

**若出現以下情況，請深入調查：**

- Go 工具：在終端機中 `go help` 真的非常棒。
- Java： [Maven 文件](https://maven.apache.org/guides/) 或 [Gradle 文件，](https://docs.gradle.org/) 取決於你站在歷史的哪一邊。

---

## 👥 團隊學習曲線：第一個月至關重要

語言選擇在這裡展現出 *實際意義* ，而基準測試卻完全忽略了這一點。

### 出發：陡峭短暫

學習 Go 的第一周很艱難。語法看起來很奇怪： `defer` 、 `goroutines` 、 `channels` 、 `interfaces without explicit implementation` 。你寫出的程式碼雖然能編譯，但感覺不太對勁。你會盯著指針接收器發呆，不明白它存在的意義。

但隨後情況發生了變化。到了第三週，你就能有效率地工作了。需要學習的東西已經不多了。 Go 語言的 *設計初衷就是簡潔* ——它沒有太多死胡同，沒有太多模式，也不容易讓你陷入困境。正因為學習曲線 *有* 終點，所以才能更快完成學習。

一個月後，你就能輕鬆上手任何 Go 程式碼庫並理解其內容。由於 `gofmt` 是不可更改的，所以 Go 程式碼風格始終如一。通常情況下，只有一種實現方式，因此任何爭論都由語言本身來解決。

### Java：漸進且無盡

一位新入行的 Java 開發人員 *很快* 就能有效率地工作。 Spring Boot 處理了大量樣板程式碼。 IntelliJ 功能強大，即使你對 Java 的工作原理不太了解，也能寫出可執行的程式碼。第一周，你就能發布一些成果。

但高效不等於勝任。學習曲線不會終結，只會變得平緩。

- 通用詞和通配符：“什麼是`? super T` ？”
- 繼承層次結構：“為什麼這個類別要繼承實作介面Whatever的AbstractSomething？”
- 依賴注入：“這個 bean 是如何實例化的？”
- 流與 for 循環：“我應該使用哪一個？”
- 已檢查異常與未檢查異常：“我應該拋出這個異常還是聲明它？”
- 註：“這是魔法還是顯而易見的？”

一個月後，你發布了一些功能。但它們並不符合慣用語法。你照搬模式卻不懂其原理。你用複雜的方式建立程式碼，因為 Java *可以* 處理複雜的情況，所以你認為它就 *應該如此* 。

六個月後，你開始用Java思考。一年後，你就真的危險了。

### 團隊影響

**Go 團隊可以橫向擴展。** 招三個初級開發人員，到第四週他們都能做出有意義的貢獻。程式碼審查速度很快，因為沒什麼好爭論的。這門語言強制執行一致性。新人不會無意中引入截然不同的模式，因為語言本身就不允許。

**Java 團隊的規模取決於其深度。** 聘請三位精通 Spring 框架的高級工程師，他們就能建立複雜的系統。但如果只聘請三位中級開發人員，就需要花費數月時間建立系統架構。好處在於，一旦大家達成共識，就能建構出 Go 語言難以實現的系統。

### 實踐中

- **Go：** 新開發者 → 第 3 天即可展現價值 → 第 20 天即可寫出可用於生產環境的程式碼
- **Java：** 新開發者 → 第 5 天即可看到成果 → 第 90 天不再讓資深開發者感到尷尬

如果你的團隊以初級成員為主且人員流動頻繁，Go 語言可以減少摩擦。成員在離職前就能快速上手。如果你的團隊經驗豐富且穩定，Java 的強大功能就成為優勢。你可以引導他們理解複雜性，程式碼庫也能表達複雜的需求。

兩者沒有優劣之分。它們的引導曲線不同，最終目標也不同。

---

## 🏢 每個人閃耀之處

### Go 非常適合：

- **雲端原生基礎設施** ：Docker、Kubernetes、Terraform、Prometheus——全部都是用 Go 語言編寫的。它們需要解決並發問題（goroutine 管理數百萬個容器），而 Go 的輕量級並發特性使得大規模基礎設施的運維變得輕鬆易行。當時，很少有主流語言能夠真正實現這種高密度架構。
- **微服務和 API** ：體積小、啟動快、記憶體佔用低。當您將數十個服務部署到不斷啟動和關閉的容器中時，Go 的毫秒級啟動速度在維運上至關重要。在這種情況下，JVM 的秒級啟動時間會成為持續的後台損耗。
- **命令列工具** ：單一二進位文件，無需執行時間環境，開箱即用。只需向用戶提供 Go 可執行文件，他們執行即可。就這麼簡單。
- **網路密集型服務** ：Goroutine 可以有效率地處理數萬個並發連線。如果您建置的是邊緣服務（例如代理程式、負載平衡器、API 閘道），這將帶來維運優勢。
- **人員流動率高或高度重視一致性的團隊** ：語言規範強化了單一的做事方式。新人上手很快。關於風格的爭論消失了，因為 `gofmt` 不容更改。

### Java 非常適合：

- **複雜的企業領域** ：類型系統（泛型、密封類別、記錄）讓您可以精確地對複雜的業務邏輯進行建模。即使三年後需求變化，編譯器也能幫助您找到所有需要更新的內容。 Java 要求您明確定義契約，而這最終會帶來豐厚的回報。
- **高吞吐量、長時間執行的服務** ：JVM 預熱完成後（預熱只需一次，之後可維持數週），JIT 編譯器會產生越來越最佳化的程式碼。對於持續運作並處理數百萬個請求的服務而言，這種最佳化效果會不斷疊加。服務執行時間越長，效能越好，這與微服務模式截然相反。
- **規模龐大且穩定的團隊** ：新成員入職可能需要更長時間，但一旦團隊掌握了程式設計模式，Java 的明確特性就會成為一大優勢。您可以建立複雜的系統，並確保每個人都能理解。
- **資料密集型應用** ：Hibernate、Spring Data、JPA 生態系統——它們解決了許多棘手的資料庫問題，例如複雜的查詢、事務、遷移和關係管理。 Go 的資料庫方案雖然可行，但 Java 的方案更成熟，也經過了實戰檢驗。
- **現有 Java 投資** ：您擁有可執行的程式碼，熟悉這些程式碼的人員也參與其中，而且轉換成本確實存在。現代 Java（21+）確實比以前更容易使用。虛擬線程解決了先前的一個重大缺陷。與其重寫，不如繼續改進。
- **需要長期演進的系統** ：Java 的類型系統有助於你理解多年後的變更。該語言鼓勵你明確定義約束和契約。當需求變得複雜時，這種規範會帶來豐厚的回報。

---

## 🤷 誠實回答

如果你正在建立基礎設施工具、命令列介面 (CLI) 或容器化微服務，並且啟動時間和記憶體佔用對運維至關重要，那麼 Go 就是你的最佳選擇。如果你想快速開發而無需糾結於風格指南，那就選擇 Go。如果你的團隊經驗尚淺，你沒有時間指導他們學習，那麼 Go 也是你的最佳選擇。

如果你要建立一個真正複雜的系統，並且需要一個能夠隨著需求成長而擴展的類型系統，那就選擇 Java。如果你的組織已經在使用 Java，並且切換會是一場惡夢，那就選擇 Java。如果你的團隊經驗豐富且穩定，那就選擇 Java——這門語言獎勵的是專業技能和知識累積。

說實話，語言很少會變成瓶頸。架構、資料庫設計、團隊溝通、部署基礎架構──這些才是更重要的。但是，根據自身限制選擇合適的工具，確實可以避免一些不該存在的摩擦。

兩者都確實很擅長各自的用途。你不是在「好」和「壞」之間做選擇，而是在「適合這個」和「適合那個」之間做選擇。

---

原文出處： [https://dev.to/adamthedeveloper/go-vs-java-the-minimalist-vs-the-enterprise-veteran-1gg3](https://dev.to/adamthedeveloper/go-vs-java-the-minimalist-vs-the-enterprise-veteran-1gg3)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qv2M2q) [115瀏覽](https://codelove.tw/@tony/post/qv2M2q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qv2M2q)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬11 ❤️1

550

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

82

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779847279&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fqv2M2q&amp;pra=5&amp;asro=0&amp;itsi=4&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779847279684&amp;bpp=1&amp;bdt=75&amp;idt=33&amp;shv=r20260521&amp;mjsv=m202605210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779847014%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779847014%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779847014%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3410046088073&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=2074&amp;eid=95390667%2C31098762%2C95391807&amp;oid=2&amp;pvsid=2685227060254622&amp;tmod=941033422&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=37" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260521/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>