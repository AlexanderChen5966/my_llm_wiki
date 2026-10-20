---
title: "Day 24｜多輪對話的 RAG：如何記住前面的問題？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405955"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "使用者很少在每一輪重新交代完整背景。第一輪問「什麼是 CSRF？」，第二輪通常只會接著說「如果是無狀態 API，還要開著它嗎？」若把第二句單獨拿去搜尋，知識助理不知道「它」指的是 CSRF；若把整段對..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 24

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 24 篇

## Day 24｜多輪對話的 RAG：如何記住前面的問題？

- 分享至

使用者很少在每一輪重新交代完整背景。第一輪問「什麼是 CSRF？」，第二輪通常只會接著說「如果是無狀態 API，還要開著它嗎？」若把第二句單獨拿去搜尋，知識助理不知道「它」指的是 CSRF；若把整段對話全部塞進 Prompt，Context 會越來越長，模型先前說過的話還可能被誤當成可信事實。

今天要完成的是基本多輪 RAG，不是讓模型永久記住一切。設計目標只有三個：用近期歷史解析省略、把追問改寫成可獨立搜尋的問題，以及讓歷史只協助理解，不取代知識庫證據。

## 對話狀態要有限

第一版只保存最近四輪的問題與最終回答：

```python
@dataclass(frozen=True)
class ConversationTurn:
    question: str
    answer: str

@dataclass
class ConversationState:
    max_turns: int = 4
    turns: list[ConversationTurn] = field(default_factory=list)

    def add(self, question: str, answer: str) -> None:
        self.turns.append(ConversationTurn(question, answer))
        self.turns[:] = self.turns[-self.max_turns:]
```

有限視窗同時控制 Token、隱私與錯誤累積。歷史越長，不代表回答越好；遠處的舊主題反而容易干擾目前問題。正式產品還要替不同使用者建立獨立 Session，設定保存期限與刪除機制，絕不能把所有人的 Turn 放進同一個全域串列。

## 先把追問變成獨立查詢

`FollowUpResolver` 接收近期歷史與新問題，只輸出 `search_query` 。Prompt 特別說明：歷史回答只用來解析代名詞與省略，不得視為可信事實，也不能直接拿來回答。

```python
payload = {
    "history": [
        {"question": turn.question, "answer": turn.answer}
        for turn in history
    ],
    "follow_up": question,
}

messages = [
    {
        "role": "system",
        "content": (
            "把 follow_up 改寫成不依賴對話歷史也能搜尋的完整問題。"
            "歷史回答只用來解析代名詞，不得視為可信事實。"
        ),
    },
    {"role": "user", "content": json.dumps(payload, ensure_ascii=False)},
]
```

解析失敗、搜尋式為空或超過 240 字時回退原問題。這和 Day 23 的原則一致：對話解析是增強層，不應讓基本問答一起故障。

## 用完整問題搜尋，回答仍對準原本追問

Resolver 產生的完整句只進 Retrieval；Generation 仍看到使用者原本問的「還要開著它嗎？」以及本次重新檢索到的來源。近期歷史會放在 Payload 的 `conversation_history` ，但 System Prompt 已明確規定它不是事實來源。

```python
results, context, _ = pipeline.retrieve(
    question,
    search_query_override=rewrite.search_query,
)

raw = pipeline.llm_client.generate(
    build_messages(question, context, history=state.turns)
)
answer = finalize_answer(raw, context)
```

這裡沒有「沿用上一輪來源」。每一輪都重新 Retrieval，因為知識庫可能更新，Follow-up 也可能需要不同文件。上一輪回答只能幫助解析主題，不能成為下一輪的證據捷徑。

## 本機兩輪實測

使用 `qwen3.5:9b` 實際執行：

```bash
RAG_LLM_MODE=ollama RAG_MODEL=qwen3.5:9b \
HF_HUB_OFFLINE=1 python scripts/conversation_demo.py
```

第一輪不需要改寫：

```markdown
Q：什麼是 CSRF？
搜尋式：什麼是 CSRF？
A：CSRF 利用瀏覽器自動附帶 Cookie 的行為，誘導已登入使用者送出請求；
   Spring Security 會以 Token 保護會改變狀態的方法。[S1]
```

第二輪則成功補回主題：

```markdown
Q：如果是無狀態 API，還要開著它嗎？
搜尋式：Spring Security 對於無狀態 API（如 RESTful）是否仍需啟用 CSRF 保護
A：若無狀態 API 使用 Bearer Token 且完全不依賴 Cookie，通常可以停用 CSRF；
   若同時提供瀏覽器頁面與 API，建議只對 API 路徑停用。[S1]
```

這次改寫加入了 `RESTful` ，雖然沒有改變檢索結果，仍屬原問題未明說的擴張。它再次提醒我們：解析 Follow-up 也可能漂移，必須保存 `search_query` ，並在評測中檢查實體與意圖是否維持。

## 多輪最危險的是錯誤與主題一起累積

假設第一輪回答錯誤，第二輪又把它當事實，錯誤就會在對話裡自我強化。因此本系列採取兩條限制：只有通過引用驗證的最終回答才加入歷史；即使加入，下一輪仍不能引用歷史作為證據。若第一輪因資料不足而拒答，歷史會記錄拒答文字，但第二輪仍要重新檢索。

另一個風險是主題切換。使用者問完 CSRF，下一句可能直接改問 Chunking。Resolver 不應因為歷史存在，就硬把新問題接在 CSRF 上。完整評測集要同時包含追問、主題切換與矛盾修正；今天的兩輪示範只證明資料流可行，不代表多輪理解已經完成。

## Session 隔離比「記得更多」更重要

範例使用記憶體內的 `ConversationState` ，適合展示資料流，卻不能直接搬到多人服務。正式 API 至少要以不可猜測的 `session_id` 區隔狀態，並再次檢查 Session 是否屬於目前登入者。只靠前端傳來一串 ID 就讀取歷史，會形成跨使用者資料外洩。

狀態也需要生命週期：多久未使用就過期、使用者能否主動清除、服務重啟後要不要保留，以及稽核紀錄保存多久。知識助理若處理內部文件，對話歷史往往比單一問題更敏感，因為連續問題可能暴露使用者正在處理的案件與決策方向。最小保存、明確刪除與權限檢查應先於長期記憶功能。

同一個 Session 若同時送出兩個請求，還有順序競爭：後送出的問題可能先回答，狀態卻依完成順序寫入。第一版可對單一 Session 加鎖或序號，拒絕過期寫入；流量提高後再評估事件序列或外部狀態儲存。這是服務工程問題，LLM 不會自動替我們解決。

「只留四輪」也只是容易理解的起點，沒有真正控制 Token。某輪回答很長，四輪仍可能塞滿 Prompt；四個短問答則可能只占少量空間。較完整的 Context Budget 應分配給系統規則、知識來源、目前問題與對話歷史，歷史超過預算時由最舊輪開始移除。

另一種方法是把舊對話摘要成記憶，但摘要本身也可能失真。若採用摘要，必須保留它是「對話摘要」而非「知識來源」的標記，並在使用者修正前述內容時更新或失效。對高風險問答，與其讓模型長期累積模糊摘要，不如要求使用者在新 Session 重新確認核心條件。

為了處理主題切換，Resolver 可以輸出除了 `search_query` 以外的決策，例如 `follow_up` 、 `new_topic` 、 `clarification` 。當新問題包含完整主詞與新的技術實體時，應偏向 `new_topic` ，不把舊歷史帶進搜尋；若只說「那 403 呢？」才使用近期主題補全。

使用者也可能主動糾正前文：「我剛才說的是 Session Cookie，不是 Bearer Token。」這不是一般 Follow-up，而是對上下文條件的修正。系統應以新訊息為準，重新檢索，不得把舊回答中已被否定的 Bearer Token 繼續當條件。歷史的時間順序與否定關係，也是多輪評測不可缺少的案例。

## 多輪評測要看整條對話

單題評測無法發現「第一輪正確、第二輪指涉錯、第三輪又被錯誤帶走」的累積問題。多輪資料應以 Conversation 為單位，保存每一輪的獨立搜尋式、相關文件、預期狀態與不可帶入的舊實體。至少涵蓋代名詞、省略、主題切換、使用者修正、第一輪拒答後補充資訊，以及不同 Session 使用相同代名詞等情境。

指標除了每輪 Hit@K 與回答正確性，也要回報整段對話全數成功的比例。三輪各自 90% 看似不錯，但若錯誤互相獨立，三輪都成功只有約 73%；真實錯誤還可能連鎖，結果更差。因此今天的兩輪 Demo 是功能證據，不是可靠性證明。

## 結語

今天完成了有限狀態的多輪 RAG：最近四輪只用來解析省略，追問會轉成可獨立搜尋的問題，每一輪都重新檢索，最終回答仍只能引用本次 Context。兩輪本機實測成功把「它」解析成 CSRF，並回答無狀態 API 的設定取捨。

到這裡，第四階段完成：系統已能組 Context、遵守回答契約、串接 LLM、驗證引用、拒答、改寫問題與處理基本多輪。下一篇進入第五階段，回到本系列最重要的紀律——評測。Hit@K 只是起點，我們會正式計算 Recall@K 與 MRR，檢查正確文件是否被找回，以及它到底排在多前面。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405503)

[

Day 23｜問題改寫：提升使用者問題的搜尋效果

](https://ithelp.ithome.com.tw/articles/10405503)[下一篇](https://ithelp.ithome.com.tw/articles/10406131)

[

Day 25｜評估搜尋品質：Recall@K、MRR 與命中率

](https://ithelp.ithome.com.tw/articles/10406131)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 29 篇

目錄

1. 25
	[Day 25｜評估搜尋品質：Recall@K、MRR 與命中率](https://ithelp.ithome.com.tw/articles/10406131)
2. 26
	[Day 26｜評估回答品質：正確性、引用與幻覺分析](https://ithelp.ithome.com.tw/articles/10406229)
3. 27
	[Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響](https://ithelp.ithome.com.tw/articles/10406516)
4. 28
	[Day 28｜RAG 安全性：Prompt Injection 與惡意文件](https://ithelp.ithome.com.tw/articles/10406994)
5. 29
	[Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務](https://ithelp.ithome.com.tw/articles/10407057)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/254a0234-400f-4b72-9159-6b2a4d66c2ea_small.png)

[2021 Q4 - Progress MFT 安全檔案傳輸管理軟體 - MOVEit Automation 培訓課程 (2)](https://itplus.ithome.com.tw/vod-page/510?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

EC NETWORKER |

113 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/2cc4afe4-e117-4311-85f9-42961aa95bc5_small.png)

[Three.js實現ROS虛實環境監控系統](https://itplus.ithome.com.tw/vod-page/757?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/92ffb6f1-e85f-4c4e-8d38-7bb1a646f783_small.png)

[【DevOps入門班】DevOps 實務中不可或缺的產品思維](https://itplus.ithome.com.tw/vod-page/970?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/cfd207ee-1d8f-4fc6-99c4-8eefec2553f3_small.png)

[AI 驅動的敏捷產品開發之旅 - 從靈感到交付](https://itplus.ithome.com.tw/vod-page/1379?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

39 分