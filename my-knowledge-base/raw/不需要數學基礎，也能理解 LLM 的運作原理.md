---
title: "不需要數學基礎，也能理解 LLM 的運作原理"
source: "https://codelove.tw/@tony/post/apXr2a"
author:
published:
created: 2026-06-10
description: "不需要數學基礎，也能理解 LLM 的運作原理=========================    今天剛好看到一篇 [《*How LLMs Actually Work*》](https://link.juejin.cn?target=https%3A%2F%2Fwww.0xkato.xyz..."
tags:
  - "clippings"
status: processed
---

## 不需要數學基礎，也能理解 LLM 的運作原理

```
今天剛好看到一篇 [《*How LLMs Actually Work*》](https://link.juejin.cn?target=https%3A%2F%2Fwww.0xkato.xyz%2Fhow-llms-actually-work%2F) 的內容，可以很形象地解釋 LLM 究竟是怎麼工作的，**特別是作者拋開了各種複雜的數學原理，還能直觀讓你理解 LLM 是怎麼工作的**。
```

> 簡單來說，LLM 大多是把 Transformer block 一層層堆起來，所以只要理解 *token、embedding、位置編碼、attention、FFN、殘差流、歸一化、next-token prediction* ，基本就能看懂很多論文和模型在講什麼，而且可以換個角度理解。

[![](https://i.imgur.com/1IUacNF.jpeg)](https://i.imgur.com/1IUacNF.jpeg)

這個圖說的就是，LLM 要理解一句話並給出回答，正常來說它需要：

- 先把文字切成 token ID
- 再變成高維向量
- 加入位置信息
- 通過多層 Transformer 讓 token 彼此交換資訊
- 做局部和全局變換
- 把最後一個 token 的向量映射成下一個 token 的機率分布，然後一個 token 一個 token 地採樣生成完整回答

看不懂？沒事，後面看完就理解了，簡單來說就是：

> **LLM 的一次生成，是每一步都在問：基於目前所有上下文，下一個最合理的 token 是什麼？**

1. ## Tokenization：模型不讀文字，只讀整數

這個其實很重要， **LLM 不直接處理自然語言字串** ，而是先透過 tokenizer 把文字變成一串整數 ID，每個整數對應詞表裡的一個 token。

所以 **token 不一定是單字，也不一定是字元，更多時候是 subword** ，比如：

```
css 體驗AI代碼助手 代碼解讀複製代碼tokenization -> ["token", "ization"]
running -> ["run", "ning"]
```

因為如果用整個單字作表，詞表會太大，而且遇到新詞泛化會變差，如果字元級別詞表太小，就會導致序列太長，所以：

> **subword 是折衷方案，常見片段作為單獨 token，罕見詞或新詞透過更小片段拼出來** 。

[![a328a3b6-5530-4970-9cb0-f0a07df4a1df](https://i.imgur.com/FOS1Dh3.jpeg)](https://i.imgur.com/FOS1Dh3.jpeg)

這就可以解釋了 LLM 裡一個經典現象：

> **LLM 有時數不清「strawberry」裡有幾個 r，不是因為模型完全不會數數，而是因為它不是天然按字母處理文字，而是按 token ID 處理** 。

另外，不同模型家族 tokenizer 不同，一般來說，GPT 系列常用 BPE 變體，LLaMA 風格模型常見 SentencePiece，所以簡單來說， **tokenizer 類似 LLM 世界裡的「輸入法 + 壓縮器 + 詞表協議」** 。

> **同一句話，在不同 tokenizer 下可能變成不同 token 數** ，這會直接影響上下文長度、價格、速度和模型對某些語言/符號的處理能力。

2. ## Embeddings：整數本身沒意義，要變成向量

前面說的 token ID 只是一個索引，比如 `1024` 本身不代表任何語義，模型真正處理的是 embedding，也就是從 embedding matrix 裡查出來的一行高維向量。

> 比如 7B 級別模型的 hidden size 可能是 4096，也就是每個 token 會被映射成一個 4096 維向量，更大的模型通常會有更寬的向量。

*是不是看著又開始懵了* ？簡單來說，可以把 **hidden size = 4096** 理解成：

> 模型不是用一個詞來理解一個 token，而是給每個 token 做了一張「4096 項特徵表」。

比如 token 是 `蘋果` ，人看到「蘋果」可能會想到：

```
水果
甜的
紅色/綠色
公司 Apple
手機
賈伯斯
中文詞
名詞
可以吃
科技品牌
……
```

但模型不會用中文標籤寫這些特徵，它會把 `蘋果` 變成一串數字，大概像：

```
csharp 體驗AI代碼助手 代碼解讀複製代碼[0.12, -0.48, 0.03, 1.27, ...]  共 4096 個數字
```

這 4096 個數字不是隨機的，而是模型訓練出來的「含義座標」，你可以粗暴理解成：

> 每個 token 都被放進一個 4096 維的語義空間裡，4096 個維度一起描述它的意思、語法、上下文潛力和各種隱含特徵。

比如只用一個數字描述 `蘋果` ，大概可能是：

```
體驗AI代碼助手 代碼解讀複製代碼蘋果 = 8
香蕉 = 7
手機 = 3
```

如果是二維就是：

```
體驗AI代碼助手 代碼解讀複製代碼甜度、科技感
```

蘋果水果可能是：

```
體驗AI代碼助手 代碼解讀複製代碼甜度 9，科技感 1
```

Apple 公司可能是：

```
體驗AI代碼助手 代碼解讀複製代碼甜度 0，科技感 10
```

而 4096 維就是超級複雜的「含義雷達圖」，它不是只看甜度、科技感，而是同時看幾千個隱含方向：

```
體驗AI代碼助手 代碼解讀複製代碼是不是名詞
是不是品牌
是不是食物
是不是中文
和水果相關程度
和手機相關程度
和公司相關程度
和顏色相關程度
和代碼上下文相關程度
和句子主語相關程度
……
```

當然真實維度不是這麼人工命名的，但是大概理解就是這樣，所以：

> **hidden size 4096 = 模型用 4096 個數字來表示每個 token 當前的「狀態」** 。

而且需要注意的是，不只是 token 原始含義，隨著 Transformer 一層層處理，這個 4096 維向量也會不斷變化，所以關鍵點是：

> **embedding 不是人工寫死的語義表，而是在訓練中學出來的** ，語義相近的 token 會在向量空間中靠得更近，比如 “king” 和 “queen”，“Paris” 和 “France”。

經典的 embedding 例子就是： `king - man + woman ≈ queen`

[![](https://i.imgur.com/PUI8c4Q.jpeg)](https://i.imgur.com/PUI8c4Q.jpeg)

不過 embedding 也有一個問題： **它只表示 token 的語義，不表示位置** ，也就是說 “dog” 出現在句首還是句尾，查出來的基礎 embedding 是一樣的，所以才會有後續的「位置編碼」。

到這裡就可以理解： **tokenization 把文字變成編號，embedding 把編號變成模型能計算的「語義座標」** 。

3. ## Positional Encoding：模型怎麼知道順序？

純 self-attention 本身不天然知道詞序，如果不給它位置信息，它就無法直接區分 “dog bites man” 和 “man bites dog”， **所以模型必須把 token 的「位置」注入進去** 。

> 原始 Transformer 使用正弦/餘弦位置編碼，把位置信息加到 token embedding 上，這樣 “dog at position 1” 和 “dog at position 5” 會變成不同向量。

不需要理解太高深， **就是粗暴認為有一組代表順序的數字就行** ，不過早期這種 additive positional encoding 有兩個問題：

- 語義和位置被塞進同一組數字裡，容量有限
- 絕對位置 embedding 對長上下文泛化不好，如果模型訓練時只見過 2048 token 長度，那麼位置 5000 可能沒有被同樣充分學習

**現在 LLM 大多使用 RoPE，也就是 Rotary Position Embeddings** ，它不是把位置向量加到 embedding 上，而是在 attention 裡旋轉 Query 和 Key 向量， **旋轉角度取決於 token 的位置** 。

> 兩個 token 做 attention 比較時，Query 和 Key 的相對旋轉差異就編碼了它們之間的相對距離。

[![](https://i.imgur.com/c5zZk70.jpeg)](https://i.imgur.com/c5zZk70.jpeg)

> RoPE 的好處就是天然表達相對位置，更適合 attention 的需求，而且長上下文泛化更好，不增加額外參數。

不過就算使用了好的位置編碼，LLM 還是會有 “lost in the middle” 問題，也就是長 prompt 中開頭和結尾的資訊更容易被利用，中間資訊經常被忽略，所以：

> **「把重要資訊放前面，或者在結尾重複關鍵約束」，這種 prompt 技巧是有道理的** ～～

所以， **長上下文不等於模型平均理解所有上下文，能塞進去，不代表能同等權重地用好。**

---

4. ## Attention：token 之間怎麼交換資訊？

一般來說，每個 token 會被投影成三種向量：

- Query：我在找什麼？
- Key：我能被什麼匹配？
- Value：如果我被匹配上，我傳遞什麼資訊？

Query 和 Key 做點積，得到匹配分數，再透過 softmax 變成權重，最後用這些權重對 Value 做加權平均，高權重 token 的 Value 會更強地影響當前 token 的新表示。

說人話就是： **每個 token 都會去上下文裡「找相關資訊」，先判斷誰和我最相關，再按相關程度把它們的資訊混合進自己** 。

或者說： **Query 負責提問，Key 負責被匹配，Value 負責提供內容，匹配越高，內容影響越大** 。

[![](https://i.imgur.com/wjhA3qN.jpeg)](https://i.imgur.com/wjhA3qN.jpeg)

比如 `The cat that I saw yesterday was sleeping.`：

- 當模型處理 “was” 時，它需要知道誰 “was sleeping”
- “was” 的 Query 會和前面 token 的 Key 做比較
- 和 “cat” 的匹配較高，和 “yesterday” 的匹配較低
- softmax 後，“cat” 得到更高權重，於是 “cat” 的 Value 對 “was” 的新表示影響更大
- 這樣模型就能把幾個位置之前的主語關聯過來

對於 GPT-style decoder-only 模型還有一個限制：生成是從左到右的，所以當前位置不能看未來 token，這叫 causal masking，實作上就是把未來 token 的注意力分數設得極低，softmax 後權重幾乎為 0。

> 作者還提到 Anthropic 2022 年發現的 induction heads，這類 attention head 會識別類似 `A B ... A` 的模式，然後當第二次看到 A 時，回頭找第一次 A 後面跟著什麼 B，從而預測 B，這是目前解釋 `in-context learning` 的一個機制。

另外 attention 的代價很高，full attention 裡每個 token 要和所有可見 token 比較，所以 prompt 長度會翻倍，計算量大致會變成四倍， **這就是長上下文貴、慢、占資源的底層原因之一** ，也是 FlashAttention、sparse attention、linear attention 等研究方向存在的原因。

說人話就是：

> **Attention 是每個 token 都在問：上下文裡哪些 token 的資訊最該被我吸收？**

---

5. ## Multi-head Attention

單個 attention head 只能學一種關係，但語言裡同時存在很多關係： *主謂一致、代詞指涉、局部片語、長距離引用、位置模式* 等，所以 Transformer 會並行運行多個 attention head。

當然， **每個 head 不是簡單拿原始 token 向量的一小段切片** ，比如：

> hidden size 是 4096，有 32 個 head，每個 head 可能工作在 128 維空間，但這 128 維是從完整 4096 維學出來的投影，不是原向量的固定切塊。

每個 attention head 不是把 4096 維向量切一塊拿走，而是用自己的一套「濾鏡」，從完整的 4096 維裡提取它關心的 128 維資訊。

> **所以多頭注意力不是「把腦子切成 32 塊」，其實是「32 個不同視角同時看同一個 token」** 。

[![](https://i.imgur.com/n3PBZOU.jpeg)](https://i.imgur.com/n3PBZOU.jpeg)

**每個 head 獨立做 attention，輸出再拼接起來，經過一個最終線性層混合回完整向量** 。

> 不同 head 會自然分工，有的關注語法，有的關注代詞，有的關注位置模式，有的形成 induction head，模型沒有被人工規定哪個 head 做什麼，這些都是在訓練中湧現出來的。

特別是 KV cache 生成時，模型不希望每生成一個 token 就重算整個 prefix，所以會快取過去 token 的 Key 和 Value，這樣新 token 到來時，可以直接重用舊 K/V， **但 KV cache 是長上下文推理的主要記憶體成本之一** 。

因此現代 decoder-only LLM 常用 GQA，也就是 Grouped-Query Attention， 多個 query head 共享較少數量的 key/value head，從而減少 KV cache 記憶體壓力，說人話就是：

> **GQA 負責「提問」的頭很多，但負責「存檔和回答」的頭少一點，多個提問頭共用同一套 Key/Value，從而省記憶體。**

正常多頭注意力可以理解成：

```
32 個 head
= 32 套 Query
+ 32 套 Key
+ 32 套 Value
```

問題是生成時要快取歷史 token 的 Key/Value，也就是 KV cache，上下文越長，層數越多，Key/Value 頭越多，占用記憶體越大，所以 GQA 改成：

```
32 個 Query head
但只有 8 個 Key/Value head
```

也就是每 4 個 Query head 共用 1 組 Key/Value，這就像公司裡：

> 32 個員工都可以提問題，但不用配 32 個資料庫，只需要 8 個共享資料庫

所以 **保留多個 Query head 的不同觀察角度，同時大幅減少要快取的 Key/Value 數量，這樣長上下文推理更省顯存，也更快** ，比如：

- LLaMA-2 70B 有 64 個 query heads，但只有 8 個 key/value heads
- Mistral 7B 有 32 個 query heads 和 8 個 key/value heads

**所以長上下文不只是輸入 token 多，還意味著每一層、每個生成步驟都要維護大量歷史 K/V 狀態，KV cache 是推理記憶體成本的核心之一** 。

---

6. ## Feed-Forward Network

Feed-Forward Network 是模型「記知識」的大倉庫之一，Attention 負責 token 之間的資訊交換，但每個 Transformer layer 還有一個很重要的模組：Feed-Forward Network，也就是 FFN。

attention 是 token 之間「互相看」，而 FFN 是每個 token 獨立做進一步加工，不混合其他 token，它通常有三步：

> 擴展向量維度 -> 經過非線性函數 -> 壓回原始維度，說人話就是： **先把 token 的資訊攤開看得更細，再做一次複雜判斷，最後濃縮回原來的大小** 。

[![](https://i.imgur.com/lpYLEVd.jpeg)](https://i.imgur.com/lpYLEVd.jpeg)

簡單說就是， **Attention 負責「從上下文裡找資訊」，FFN 負責「把找到的資訊在自己腦子裡再加工一遍」** ，粗暴點理解：

- Attention：
	- 這個 token 去看別的 token
		- 「我該參考誰？」
		- 「前面哪個詞和我有關？」
		- 「這個代詞指的是誰？」

FFN：

- 這個 token 不再看別人，而是在自己內部加工：
	- 「結合剛才拿到的資訊，我現在應該更像什麼？」
		- 「我是水果蘋果，還是 Apple 公司？」
		- 「我是變數名，還是普通英文單字？」

再簡單點理解就是：

- Attention：資訊搬運 / 上下文關聯
- FFN：資訊加工 / 知識變換

原始 Transformer 用 ReLU，GPT/BERT 常用 GELU，現代 LLaMA、Mistral、PaLM 等模型常用 SwiGLU，核心結構還是 expand-transform-compress，變化主要在非線性函數。

所以 Attention 讓 token 從上下文裡拿資訊，而 FFN 就是讓每個 token 在自己的向量空間裡做深加工，FFN 先把向量擴展到更大的空間，讓隱藏特徵充分展開，再透過 ReLU/GELU/SwiGLU 這類非線性函數做選擇和變換，最後壓回原來的 hidden size。

> 非線性很關鍵，因為沒有它，多層線性網路本質上仍然只是一個大矩陣，模型就很難表達複雜語義和知識。

而且， **dense transformer 裡大量參數其實在 FFN，而不是 attention** ，因為 FFN 裡存著很多模型的事實和語義結構，一些 neuron 會對特定概念強激活，比如 Eiffel Tower、程式語言、過去式動詞等。

> 模型知道「巴黎是法國首都」這類事實，這並不是存在一個顯式資料庫裡，而是分布在 FFN 權重和激活中。

而 MoE 不是每層只有一個 FFN，而是有多個 expert FFN，再由 router 為每個 token 選擇少數幾個 expert，比如：

> Mixtral 8x7B 每層有 8 個 expert，每個 token 只激活 2 個，這樣總參數量可以變大，但每個 token 的實際計算量增長沒那麼快，Mixtral 8x7B 總參數 46.7B，但每個 token 大約只用 12.9B 參數。

---

7. ## Residual Stream 和 LayerNorm

而 Transformer 裡不是每層都把舊表示完全替換掉，而是把 attention 或 FFN 的輸出加回原向量，也就是：

```
new vector = old vector + block output
```

[![](https://i.imgur.com/ArsxHol.jpeg)](https://i.imgur.com/ArsxHol.jpeg)

這叫殘差連接（residual connection），多層堆疊後，每層貢獻都會累積在一個持續流動的表示裡，這個運行中的總和就是殘差流（residual stream）。

> **說人話就是，Transformer 每一層不是把前一層的理解推翻重寫，而是在原來的理解上「補一筆」** 。

比如一個 token 一開始只是 `蘋果` ，經過 attention 之後，它可能從上下文裡拿到資訊：

> 前面說「發布了新晶片」。

那這一層不會把「蘋果」原來的表示整個刪掉，而是把新資訊加進去：

> 蘋果 + 發布新晶片相關資訊。

所以它越來越像 `Apple 公司` ，再經過下一層 FFN，它又補上一些更深的知識：

> Apple 公司 + 科技公司 + 晶片 + 發表會 + 產品資訊。

所以殘差連接可以理解成：

> **每一層都在給 token 的表示做增量更新，而不是全量重寫。**

作者在這裡主要強調了殘差連接讓深層網路可以訓練，這個技巧來自 ResNet，最早是為了解決深層影像網路訓練困難的問題：

> 梯度在很多層中傳播時會變弱或變強，導致訓練失敗，shortcut path 讓訓練信號更容易從輸出傳回輸入，Transformer 繼承了這個技巧。

殘差連接解決的是「深層網路怎麼把資訊和訓練信號一路傳下去」，然後 LayerNorm/RMSNorm 解決的是「傳下去的時候，數值不會出問題」。

簡單來說就是：

- 殘差連接讓 Transformer 每層只做「增量修改」，舊資訊可以沿著 residual stream 一路傳下去，所以深層網路更容易訓練
- 殘差流就像模型內部的公共黑板，attention、FFN 和最終輸出層都在上面讀
- LayerNorm/RMSNorm 則是數值穩定器，防止這塊黑板上的向量經過幾十層相加後爆炸或塌縮

簡單來說就是：

> 殘差連接讓資訊和梯度有高速路，LayerNorm/RMSNorm 讓這條高速路上的數值別失控，沒有它們堆幾十層、上百層 Transformer 會困難很多。

---

8. ## Next-token Prediction

那最後到底輸出什麼？所有 Transformer 層處理完後，模型會得到每個 token 的最終向量。

生成下一個 token 時，模型通常只取最後一個 token 的最終向量，把它映射成詞表大小的一組分數，比如詞表有 100,000 個 token，就得到 100,000 個 raw scores，這些叫 logits。

這裡 logits 還不是機率，經過 softmax 後，才變成「下一個 token 是每個候選 token 的機率分布」，而模型通常不會永遠選機率最高的 token，temperature、top-k、top-p 等 decoding 參數會影響採樣行為：

- 低 temperature 更保守、更確定
- 高 temperature 更隨機、更發散
- top-k/top-p 會限制候選 token 集合，只從比較合理的範圍裡採樣

一旦選出一個 token，它就被追加到輸入後面。模型再基於更長的序列預測下一個 token，這個循環一直持續，直到生成 EOS token 或達到長度限制，整段回答就是這個循環反覆運行的結果。

說人話其實就是：

> **Transformer 最後不是直接輸出一句話，而是輸出一張「下一個 token 候選榜」。**

比如目前輸入是 `蘋果發布了新` ，那麼模型處理完所有 Transformer 層後，會拿最後一個 token，也就是「新」的最終向量，去預測後面最可能接什麼，這時候它不會直接說 `晶片` ，而是先給整個詞表裡的所有 token 打分：

```
芯片：12.8
手機：10.4
產品：9.7
系統：8.9
電影：-3.2
香蕉：-6.5
……
```

這些原始分數就是 **logits** ，然後 softmax 把這些分數變成機率：

```
芯片：45%
手機：18%
產品：12%
系統：8%
……
```

這時模型才開始「選下一個 token」，然後 temperature、top-k、top-p 可以理解成「選詞風格控制器」：

- **temperature** 控制模型敢不敢冒險
- **top-k** 是只看前 k 個候選
- **top-p** 是只看累計機率達到 p 的候選集合

> 所以 top-k 是固定人數入圍，top-p 是按機率動態入圍。

而基礎 LLM 的核心訓練目標就是 next-token prediction， **基礎並不是直接以「事實正確」「會聊天」「會推理」「會寫程式」為目標訓練的，而是在海量文字上學習預測下一個 token** 。

> 之後的 instruction tuning、preference tuning、safety tuning 這些 post-training，才讓它更會遵循指令、對齊偏好和適應對話。

另外小模型會先快速草擬多個 token，大模型並行驗證，如果草擬 token 在大模型機率下可接受，就直接採用，否則回退到大模型生成，做得好的時候，輸出分布可以等價於單獨運行大模型，但速度更快。

---

9. ## Architecture vs Weights

最後作者還分析了 GPT、Claude、Gemini、LLaMA 到底差在哪？

他認為 GPT、Claude、Gemini、LLaMA 等模型的公開細節不同，閉源模型也不會公布所有結構選擇，但在這篇文章討論的層面，它們大多都處於 Transformer-family 設計空間中。

現代 Transformer LLM 的共同骨架通常包括前面我們所說的這些東西：

- tokenization
- embedding
- positional encoding
- stacked transformer layers
- multi-head attention
- feed-forward network
- residual stream
- normalization
- next-token prediction

而真正讓模型不同的主要是三類東西：

- 第一，訓練出來的 weights，也就是不同資料、規模、訓練流程學出來的參數
- 第二，配置差異，比如層數、詞表大小、head 數、參數量、dense 還是 MoE
- 第三，post-training，比如指令微調、人類偏好學習、安全控制、對話行為塑造

> 所以現在不同 LLM 的差距， **不只是「架構誰更神秘」，更多來自訓練資料、規模、訓練 recipe、後訓練、推理系統、工具鏈和產品化策略** 。

## 最後

最後簡單總結一下：

- 第一， **token 是一切的入口** ，模型不是按人類自然理解的字、詞、句直接思考，而是處理 tokenizer 給出的 ID 序列，所以 tokenization 會影響成本、長上下文、多語言表現、字元級任務表現
- 第二， **embedding 是語義空間** ，token ID 透過 embedding matrix 變成向量，語義關係是訓練中學出來的幾何結構
- 第三， **RoPE 不是簡單「告訴模型第幾個詞」，而是在 attention 的 Q/K 比較中編碼相對位置** ，這也是為什麼它比早期絕對位置 embedding 更適合現代長上下文模型
- 第四， **attention 是資訊路由，不是知識本體** ，它決定當前 token 應該從哪些上下文 token 吸收資訊，但 dense 模型中大量參數和事實結構其實在 FFN 裡
- 第五， **KV cache 是推理成本的核心之一** ，很多人只知道「上下文越長越貴」，但原因不只是輸入 token 變多，還包括每層要保存歷史 K/V，生成時持續重用
- 第六， **FFN 很可能是「模型知識倉庫」的重要部分** ，事實、概念、語義結構大量分布在 FFN 權重和激活中，MoE 本質上也是圍繞 FFN 容量和計算成本做稀疏擴展
- 第七， **殘差流是現代 interpretability 的核心對象** ，每個模組都在 residual stream 上讀寫，模型不是一層層覆蓋舊資訊，而是在不斷累積和修改表示
- 第八， **base model 的訓練目標不是「正確回答」，而是 next-token prediction** ，後訓練讓模型變得像助手，但底層生成機制仍然是基於機率分布逐 token 採樣

[![](https://i.imgur.com/alE8c1w.jpeg)](https://i.imgur.com/alE8c1w.jpeg)

## 連結

[www.0xkato.xyz/how-llms-ac…](https://link.juejin.cn/?target=https%3A%2F%2Fwww.0xkato.xyz%2Fhow-llms-actually-work)

---

原文出處： [https://juejin.cn/post/7649105672894464006](https://juejin.cn/post/7649105672894464006)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/apXr2a) [2瀏覽](https://codelove.tw/@tony/post/apXr2a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/apXr2a)

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

📝7 💬2

353

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1781082424&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapXr2a&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1781082424459&amp;bpp=2&amp;bdt=361&amp;idt=504&amp;shv=r20260604&amp;mjsv=m202606040101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781082421%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781082421%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781082421%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5186650169344&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668&amp;oid=2&amp;pvsid=3996211724468309&amp;tmod=1205431642&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=537" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>