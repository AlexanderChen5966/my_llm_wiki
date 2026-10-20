---
title: "Flutter 又為 AI 時代添磚加瓦：全新 ComponentLibrary 提議"
source: "https://codelove.tw/@tony/post/qBkmPx"
author:
published:
created: 2026-06-15
description: "Flutter 又為 AI 時代添磚加瓦：全新 ComponentLibrary 提議==============================================近日，Flutter 又提出了一個新的提案：**為 Flutter 增加一個標準原語：`ComponentLibra..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月15日](https://codelove.tw/@tony/post/qBkmPx "2026-06-15 12:07:17")


## Flutter 又為 AI 時代添磚加瓦：全新 ComponentLibrary 提議

近日，Flutter 又提出了一個新的提案： **為 Flutter 增加一個標準原語： `ComponentLibrary` 。**

`ComponentLibrary` 的主要想法是： **為 Flutter 提供一個統一的「元件註冊表／元件目錄資料協議」** ，讓 GenUI、Stac、Widgetbook、OpenUI 這類工具可以用同一套方式發現、描述、呼叫元件。

前面我們應該聊過 GenUI，作為一個在執行階段由 Agent 動態生成 UI 的 lib，它支援的 A2UI 協議所生成的是一套與框架無關的 UI 中繼資料，而 Flutter 需要針對這份協議資料做 GenUI 實作。本來其實也沒什麼問題，但架不住現在場景不只 GenUI：

場景它需要什麼  
GenUI  
AI 需要知道有哪些元件、元件 ID 是什麼、怎麼傳參數  
Server-driven UI，例如 Stac  
伺服器端 payload 需要對應到 Flutter widget  
Widgetbook / showcase 工具  
需要枚舉元件，生成元件目錄和 playground  
自動化測試 / storyboarding 工具  
需要知道元件參數、範例、可變狀態

但目前這些工具都需要在專案裡做自己的自訂支援：

- Stac 實作了自己的 custom widget registry
- GenUI catalog items 透過 `json_schema` 解析執行
- Widgetbook v3 叫 knobs，v4 叫 args
- OpenUI Library 也透過 `json_schema` 處理參數

所以如果同一個企業設計系統想同時接 GenUI、Stac、Widgetbook、自動化測試，就得寫多套重複的 wrapper / registry / adapter，這就造成生態割裂：

> 同一批元件，需要不同工具用不同結構描述，但對 AI 時代來說，這種結構化能力本身就是必須的。

不得不說，這段時間在做 AI e2e 專案測試時確實很煩，Compose、iOS 和鴻蒙都沒有標準的 MCP 注入支援，像鴻蒙和 Android 都是 adb 和 hdc 注入加截圖辨識來取得點位，效率和效能都太難受了。Flutter 能走 MCP 執行，相對效率還好一點點，但是對 AI 仍然不夠友善。

而且隨著 Material / Cupertino 逐步解耦落地，Flutter 的整體設計風格會越來越中立，所以 Flutter core 未來應該會更傾向設計系統無關的方向，不把某個具體設計系統深度綁在核心層裡，而是：

> 「這個 App / 公司 / package 有一組元件，它們叫什麼、怎麼建構、怎麼被工具發現。」

這就是 `ComponentLibrary` 提議想要支援的場景，特別是 GenUI / SDUI 也在推動這個需求，因為傳統 Flutter 是開發者手寫 widget tree，像這樣：

```
scss 体验AI代码助手 代码解读复制代码Column(
  children: [
    Text(...),
    ProfileCard(...),
  ],
)
```

但是在 GenUI / SDUI 裡，widget tree 可能來自：

```
json 体验AI代码助手 代码解读复制代码{
  "type": "cards.profile",
  "args": {
    "name": "Asher",
    "avatarUrl": "..."
  }
}
```

也可能來自其他 LLM tool calling 或工具渲染，這時候系統就必須知道：

> `cards.profile` 對應哪個 Flutter widget，參數怎麼傳，建構失敗怎麼報錯。

所以 `ComponentLibrary` 最重要的就是 **提供一個統一的元件庫契約，讓這些工具可以共用** 。

對此，Flutter 官方這次主要設計了兩個類： `Component` 和 `ComponentLibrary` ，其中 `Component` 主要用於描述單個元件，一個 `Component` 大概包含：

```
dart 体验AI代码助手 代码解读复制代码@immutable
class Component {
  final String id;
  final String description;
  final Widget Function(
    BuildContext context,
    Map<String, dynamic> arguments,
  ) builder;
}
```

也就是：

| 字段 | 作用 |
| --- | --- |
| `id` | 元件唯一識別，例如 `text.primary` 、 `cards.profile` |
| `description` | 給 AI / 工具 / 文件看的語意描述 |
| `builder` | 真正把參數轉換成 Flutter Widget 的函式 |

例如下面程式碼， `Component` 就是 widget 的「註冊描述和建構入口」，這一套感覺就是從 GenUI 拆出來的既視感：

```
php 体验AI代码助手 代码解读复制代码Component(
  id: 'cards.profile',
  description: 'Displays user details alongside avatar layouts within standard dashboard feeds.',
  builder: (context, args) {
    return ProfileCard(
      displayName: args['name'] as String,
      avatarUrl: args['avatarUrl'] as String?,
    );
  },
)
```

而 `ComponentLibrary` 主要是管理一套元件的概念，大概如下：

```
dart 体验AI代码助手 代码解读复制代码@immutable
class ComponentLibrary {
  final String name;
  final Map<String, Component> components;
​
  const ComponentLibrary({
    required this.name,
    required this.components,
  });
​
  factory ComponentLibrary.compose(
    String name,
    List<ComponentLibrary> libraries,
  ) {
    final aggregated = <String, Component>{};
    for (final lib in libraries) {
      aggregated.addAll(lib.components);
    }
    return ComponentLibrary(name: name, components: aggregated);
  }
​
  Widget buildComponent(
    BuildContext context,
    String componentId,
    Map<String, dynamic> arguments,
  ) {
    final component = components[componentId];
    if (component == null) {
      throw FlutterError.fromParts(<DiagnosticsNode>[
        ErrorSummary('Component resolution failed within library: $name'),
        ErrorDescription('The componentId "$componentId" was not registered.'),
      ]);
    }
    return component.builder(context, arguments);
  }
}
```

`ComponentLibrary` 主要做三件事：

- 保存一套元件
- 支援多個 library 組合 `ComponentLibrary.compose(...)` ，例如可以把 `coreDesignSystem` 、 `adminDashboardComponents` 、 `marketingComponents` 合成一個總庫
- 根據 `componentId` 建構 widget，如果找不到元件，就拋出 Flutter 風格的 `FlutterError`
```
dart 体验AI代码助手 代码解读复制代码library.buildComponent(
  context,
  'cards.profile',
  {
    'name': 'Asher',
    'avatarUrl': '...',
  },
)
```

例如對應 App 企業的設計系統可以統一註冊，例如：

```
php 体验AI代码助手 代码解读复制代码final acmeDesignSystem = ComponentLibrary(
  name: 'AcmeCorporate',
  components: [
    Component(
      id: 'text.primary',
      description: 'Primary body text used when text is information or descriptive...',
      builder: (context, args) {
        return AcmeText(
          content: args['content'] as String,
        );
      },
    ),
    Component(
      id: 'cards.profile',
      description: 'Displays user details alongside avatar layouts within standard dashboard feeds.',
      builder: (context, args) {
        return ProfileCard(
          displayName: args['name'] as String,
          avatarUrl: args['avatarUrl'] as String?,
        );
      },
    ),
  ],
);
```

這個方向就是，設計系統的維護者把元件集中宣告出來，形成一個標準 library。

另一個方向就是給 GenUI / Server-driven UI 用，例如這種場景通常需要：

```
javascript 体验AI代码助手 代码解读复制代码Widget buildDynamicNode(
  BuildContext context,
  Map<String, dynamic> remotePayload,
) {
  return Renderer(
    context,
    library: acmeDesignSystem,
    payload: remotePayload,
  );
}
```

也就是伺服器或 AI 回傳 payload，Renderer 根據 payload 裡的 component id 去 `ComponentLibrary` 裡找對應元件，不過 Flutter 不負責做這個 `Renderer` ，也不負責定義 JSON 協議，Flutter 只提供元件庫結構。

另一個場景就是給 Widgetbook 之類的專案使用，例如：

```
csharp 体验AI代码助手 代码解读复制代码Widget buildDevelopmentPlayground() {
  return Widgetbook.fromLibrary(
    library: acmeDesignSystem,
  );
}
```

也就是說，Widgetbook 這類工具可以直接讀取 `ComponentLibrary` ，自動生成側邊欄、元件目錄、參數面板、預覽頁面。以前每個工具都要自己定義，以後理論上可以統一消費 `ComponentLibrary` 。

當然， `ComponentLibrary` 的目標是抽象出一套 Flutter 統一的控制項描述協議，但它不做完整的鏈路，因為這個設計本身不是 Remote UI Parsing Engine：

- 不做 JSON-to-Widget parser
- 不做 AI SDK，它只是一個 layout container primitive
- 不接管 accessibility， `Component` 不改變 rendering tree、layout behavior 和 semantics nodes
- 不接管國際化， `Component.description` 和 `ComponentLibrary.name` 主要是開發期、除錯、工具索引用的，不要求 runtime localization，真正使用者可見的多語文案，還是要在 builder 裡實作

當然，目前這個設計還處於草稿階段，一些問題還需要後續解決，例如：

- 多個庫合併時，ID 衝突策略還需要考慮怎麼處理
- 是否需要 `InheritedComponentLibrary` ，提供類似 `Theme.of(context)` 的能力還沒定
- 它應該進 Flutter framework，還是應該先做 package？
- 安全邊界也還沒選好，畢竟雖然不做 JSON parser，但典型使用場景就是遠端 payload / LLM payload 決定元件建構，這會帶來一些安全風險
- ·····

所以 `ComponentLibrary` 的核心是暴露一個乾淨、獨立的註冊協議，第三方庫可以漸進式採用，不會破壞既有設計，同時又把元件設計支援提升成 Flutter framework 的統一實作，這在 AI 時代來說還蠻有用的。

就是不知道下一個版本能不能落地。

## 連結

[flutter.dev/go/introduce-component-library](http://flutter.dev/go/introduce-component-library)

---

原文出處： [https://juejin.cn/post/7649666537947807780](https://juejin.cn/post/7649666537947807780)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qBkmPx) [2瀏覽](https://codelove.tw/@tony/post/qBkmPx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qBkmPx)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝15 ❤️1

455

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1781501457&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqBkmPx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1781501457175&amp;bpp=2&amp;bdt=65&amp;idt=30&amp;shv=r20260611&amp;mjsv=m202606100101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781493177%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6846665622046&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=10&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390667%2C31099095%2C95390277%2C95393280&amp;oid=2&amp;pvsid=4813129704290428&amp;tmod=1688975609&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1993%2C33%2C1993%2C33%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=41" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260611/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>