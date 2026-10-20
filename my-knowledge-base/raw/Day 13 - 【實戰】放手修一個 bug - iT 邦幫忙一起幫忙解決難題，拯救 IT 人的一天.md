---
title: "Day 13 - 【實戰】放手修一個 bug - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402782"
author:
  - "[[iThome]]"
published:
created: 2026-08-14
description: "文章同步發表在我的個人 Blog 十二天下來，也幫 KeSi 加了不少工具，但湊起來到底能不能做事還不知道。所以今天不加新功能，而是準備了一個有 bug 的小專案，把題目丟給 KeSi 看看會發生什麼..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 13

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 13 篇

## Day 13 - 【實戰】放手修一個 bug

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

十二天下來，也幫 KeSi 加了不少工具，但湊起來到底能不能做事還不知道。所以今天不加新功能，而是準備了一個有 bug 的小專案，把題目丟給 KeSi 看看會發生什麼事。

我實際跑了五次，實際發生的情況以及花的錢錢，還有它出包的那一次都記下來，過程是節錄整理過的。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 出一份考題

示範專案如果太簡單看不出東西，太難又像在展示模型的極限而不是展示 agent 的功能。我出的題目是一個會員訂單試算：

```markdown
README.md
pricing.py       對外的試算入口
rules.py         折扣與運費規則
utils.py         跟這次無關的雜項工具
test_pricing.py
test_utils.py
```

規則寫在 `rules.py` 開頭的說明文字（docstring）裡，滿千折百，兩千以上折 150，運費 60 元、滿 800 免運。 `pricing.py` 的 `checkout()` 負責把商品總額、折扣、運費串起來：

```python
def checkout(items, member_level="normal"):
    """回傳這張訂單要付多少錢。"""
    goods = subtotal(items)
    discount = tier_discount(goods, member_level)
    payable = goods - discount
    return payable + shipping_fee(payable)
```

bug 埋在 `rules.py` ：

```python
def tier_discount(amount, member_level="normal"):
    discount = 0
    for threshold, value in DISCOUNT_TABLE:
        if amount > threshold:
            discount = value
            break
    return discount + LEVEL_BONUS.get(member_level, 0)
```

`>` 應該是 `>=` 。買 1001 元有折扣，剛好 1000 元反而沒有，經典的「差一錯誤（off-by-one error）」。跑一次測試：

```markdown
$ python3 -m pytest -q
...F...                                                                  [100%]
=================================== FAILURES ===================================
________________________ test_discount_at_exactly_1000 _________________________

    def test_discount_at_exactly_1000():
        # 滿千折百：1000 - 100 = 900，900 大於 800 所以免運
>       assert checkout([(1000, 1)]) == 900
E       assert 1000 == 900
E        +  where 1000 = checkout([(1000, 1)])

test_pricing.py:18: AssertionError
=========================== short test summary info ============================
FAILED test_pricing.py::test_discount_at_exactly_1000 - assert 1000 == 900
1 failed, 6 passed in 0.02s
```

這份考題有三個地方是我刻意設計的：

1. bug 不在測試指到的檔案裡。失敗的是 `test_pricing.py` ，測的是 `pricing.checkout()` ，但錯的是 `rules.tier_discount()` 。要修對，就得從測試追到 `pricing.py` ，再追到 `rules.py` ，跨兩層。
2. 錯誤訊息看不出原因。 `assert 1000 == 900` 只說少折了 100 元，可能是折扣沒算、可能是運費多算，也可能是折扣表寫錯。光看這行猜不出來，必須讀 code。
3. 旁邊還有干擾。 `utils.py` 跟 `test_utils.py` 跟這題完全無關，但它們就躺在那裡。一個會亂讀檔案的 agent，帳單會比較貴。

這份考題放在 repo 的 `examples/day13/project.py` ，執行 `uv run examples/day13/project.py /tmp/kesi-demo` 就會在指定的目錄裡建一份出來。想自己出題目給它做的話，改那份檔案裡的 `FILES` 就好。

題目只給一句話，不告訴它從哪裡下手：

> 跑一下測試，有一條過不了，幫我找出原因並修好

## 先放手跑一次

整場的腳本放在 repo 的 `examples/day13/demo.py` ，它會把專案建到暫存目錄、把題目丟給 KeSi，然後依序印出起點的測試狀態、過程、終點的測試狀態、磁碟上的 diff，最後是帳單：

```shell
uv run examples/day13/demo.py
```

下面是我實際跑的節錄，模型每次挑的路線不見得一樣，跑出不同的結果是正常的。 `edit_file` 那一行印出來是完整的函式，太長，這裡用刪節號帶過：

```markdown
你 > 跑一下測試，有一條過不了，幫我找出原因並修好
  [執行工具] list_files({})
  [執行工具] run_command({'command': 'python -m pytest -v'})
  [執行工具] read_file({'file_path': 'test_pricing.py'})
  [執行工具] read_file({'file_path': 'pricing.py'})
  [執行工具] read_file({'file_path': 'rules.py'})
  [執行工具] edit_file({'file_path': 'rules.py', 'old_string': 'def tier_discount(amount, ...if amount > threshold:...', 'new_string': 'def tier_discount(amount, ...if amount >= threshold:...'})
  [執行工具] run_command({'command': 'python -m pytest -v'})
KeSi > ✅ **完成！所有測試現在都通過了。**

       ## 問題總結

       **失敗的測試：** \`test_discount_at_exactly_1000\`

       **根本原因：** 在 \`rules.py\` 的 \`tier_discount()\` 函數中，條件判斷錯誤
       - **原有代碼：** \`if amount > threshold:\` （大於）
       - **修正後：** \`if amount >= threshold:\` （大於等於）
       ...
```

磁碟上的變化只有一行：

```markdown
--- a/rules.py
+++ b/rules.py
@@ -21,7 +21,7 @@
     """依消費金額給折扣，再加上會員等級的額外折抵。"""
     discount = 0
     for threshold, value in DISCOUNT_TABLE:
-        if amount > threshold:
+        if amount >= threshold:
             discount = value
             break
     return discount + LEVEL_BONUS.get(member_level, 0)
```

測試從 `1 failed, 6 passed` 變成 `7 passed` 。

我們用慢動作看一次這七步，每一步都剛好對應到前面某一天做的東西。

1. 環顧四周（第 7 天的 `list_files` 工具），因為一開始連檔名都還不知道，所以先用這個工具巡一下目錄裡有什麼。
2. 跑測試（前天才有的 `run_command` 工具）。注意它帶的參數是 `-v` ，不是我前面示範用的 `-q` 。我沒教它任何參數， `-v` 是它自己選的，因為它要的是「哪一條測試掛了」，不是一句摘要，這滿厲害的。
3. 讀測試檔（第 6 天的 `read_file` 工具），從失敗的測試名字回到原始碼，看那條測試到底在期待什麼。
4. 讀 `pricing.py` 檔案，順著呼叫往下追一層，它在測試裡看到 `checkout` ，就去找 `checkout` 是誰，發現折扣那段是交給 `tier_discount` 算的，而這個函式是從 `rules.py` import 進來的。
5. 接著讀 `rules.py` 檔案，再往下一層，這次才真的走到 `tier_discount` ，也是它第一次看到那個 `>` 。
6. 改（第 9 天的 `edit_file` 工具）。它的 `old_string` 不是只帶 `if amount > threshold:` 那一行，而是把整個 `tier_discount` 函式包進去，只換中間一個字元。那一行在檔案裡其實是唯一的，直接帶就會過。多帶一點上下文比較保險，第 9 天那次它也這樣做過。
7. 再跑一次測試。前面六步都只是它自己覺得改對了，這一步才真的看到結果。

從頭到尾我沒有指定任何一個檔名，也沒說要用哪個工具，整條路線都是它自己排的。另外， `utils.py` 跟 `test_utils.py` 它連開都沒開。帳單長這樣：

```markdown
模型往返 6 次，牆上時鐘 16.2 秒
input 31,967 token、output 1,095 token
約 0.0374 美元

  第  1 圈  input  4,162  output   66   2.6 秒  stop=tool_use
  第  2 圈  input  4,267  output   73   2.0 秒  stop=tool_use
  第  3 圈  input  4,880  output  184   2.4 秒  stop=tool_use
  第  4 圈  input  5,746  output  466   4.0 秒  stop=tool_use
  第  5 圈  input  6,239  output   77   1.6 秒  stop=tool_use
  第  6 圈  input  6,673  output  229   3.2 秒  stop=end_turn
```

等等，上面的工具紀錄明明有七行，帳單怎麼只有六圈？

因為那七張許願單不是分七次送出去的。第 5 天講過 parallel tool use，模型可以在一次回應裡一口氣開好幾張單，而 `run_tools()` 收到幾個 `tool_use` 就印幾行，那些全部算同一圈。從 output 的大小大概看得出來是哪一圈，第 3 圈的 184 是隔壁那幾圈（66、73、77）的兩倍半，應該就是它把三個 `read_file` 一次開出去的那一圈。

第 1 圈的 input 是 4,162，而題目本身只有二十幾個字。那四千多是昨天提過的七份工具說明書（4,130 個 token）加上題目，還沒開始做事就先付這一筆。input 一路從 4,162 爬到 6,673，這就是第 4 天那本越寫越厚的日記。每一圈都把前面所有的工具往返重念一次。

第 4 圈的 output 特別高（466），因為那圈是 `edit_file` ， `old_string` 跟 `new_string` 兩份幾乎一樣的函式都要重新生成一次，耗時也跟著跳到 4 秒。根據 Anthropic 的 token 官方定價 output 比 input 貴五倍，所以這一圈就是全場最貴的一圈。

六次往返、16.2 秒，換成台幣不到 2 元。這些數字是實際跑的時候把每次回應的 `usage` 攔下來累加的，正式的計價功能之後才會裝進 KeSi 本體。

## 同一題，再跑兩次

不過模型每次都帶著隨機性，同一個 bug 丟給它修兩次，它可能走兩條不同的路，所以一樣的情況我再跑第二次：

```markdown
[執行工具] list_files({})
[執行工具] run_command({'command': 'cd /tmp/a1eec5c2-df1b-4078-8b99-ce68c7b7c0c9 && python -m pytest -v'})
[執行工具] run_command({'command': 'python -m pytest -v'})
[執行工具] read_file({'file_path': 'test_pricing.py'})
[執行工具] read_file({'file_path': 'pricing.py'})
[執行工具] read_file({'file_path': 'rules.py'})
[執行工具] edit_file({'file_path': 'rules.py', ...})
[執行工具] run_command({'command': 'python -m pytest -v'})
```

三次裡，中間那三步讀檔完全一樣，都從失敗的測試追到 `pricing.py` ，再追到 `rules.py` 。這跟題目跨兩層的結構一致，至少這三次，它都是沿著同一條呼叫鏈找到 bug。換的是頭尾兩步，還有中間多繞的那一下。

第二步那個 `cd /tmp/a1eec5c2-df1b-4078-8b99-ce68c7b7c0c9` 是什麼？那個目錄不存在，是它自己生出來的一串 UUID。第 5 天說過「表格是它填的，值是它猜的」，這就是個例子，它需要一個路徑，就掰了一個看起來很像那麼一回事的。結果指令當然失敗了，它下一步就改成直接跑 `python -m pytest -v` ，成功了。是說，它為什麼會想加 `cd` ？光看這一次沒辦法下結論，不過第 11 天的說明書剛好有這一段：

> 每次都是全新的行程，cd 或環境變數不會留到下一次呼叫，需要換目錄時請在同一行用 cd x &&...

它下的指令跟這個句型長得很像，只是那個 `x` 被它憑空填了一個值。這只能算一條線索，也可能只是模型本來就會用的寫法。工具說明可能影響模型的行為，要確定這次是不是說明書造成的，還得固定其他條件多跑幾次對照。

我再跑了第三次又換一種開法，它沒用 `list_files` ，改下 `find . -name "*test*.py" -o -name "test_*" | head -20` 去撈測試檔。同一個目的，一個走我們自己寫的工具，一個走前天剛開的 shell。第 11 天在說明書裡寫的那句「讀寫檔案請優先使用 read\_file、edit\_file 與 write\_file」，看得出來就只是個建議，不是規則。

三次的帳單放在一起：

```markdown
第一次   6 圈   16.2 秒   input 31,967   output 1,095   0.0374 美元
第二次   7 圈   16.8 秒   input 37,110   output 1,151   0.0429 美元
第三次   8 圈   78.5 秒   input 42,961   output 1,215   0.0490 美元
```

三次都修對同一行，改法一模一樣，但圈數 6、7、8，花費差了三成，沒辦法靠重跑一次就得到完全相同的重播。第三次那 78.5 秒不要當真，它的第 1 圈就花了 61 秒，剩下七圈加起來才 17 秒左右。那一圈明顯是連線卡住，不是模型在想事情。

倒是有一個數字三次完全相同：第 1 圈的 input 都是 4,162。那一圈送出去的內容只有工具說明書加同一句題目，一個 byte 都沒差，所以 token 也不會差。也就是說會浮動的是模型決定做什麼，不是我們送出去的東西。

## 不准它跑測試

目前這三次都很順，順到看不出「跑測試」這件事值多少錢。那就把它拿掉。同一份專案、同一句話，這次我把 `run_command` 從工具清單裡拿掉，這樣 KeSi 就只剩下讀檔、找檔、搜尋跟改檔的功能。這個對照組加一個參數就跑得起來：

```shell
uv run examples/day13/demo.py --no-shell
```
```markdown
（這次把 run_command 拿掉了，它沒辦法跑測試）
你 > 跑一下測試，有一條過不了，幫我找出原因並修好
  [執行工具] list_files({})
  [執行工具] read_file({'file_path': 'test_pricing.py'})
  [執行工具] read_file({'file_path': 'test_utils.py'})
  [執行工具] read_file({'file_path': 'pricing.py'})
  [執行工具] read_file({'file_path': 'utils.py'})
  [執行工具] read_file({'file_path': 'rules.py'})
  [執行工具] glob({'pattern': 'test_*.py'})
  [執行工具] edit_file({'file_path': 'rules.py', ...})
  [執行工具] edit_file({'file_path': 'test_pricing.py', ...})
```

滿好的，在倒數第二步它一樣找到了那個 `>` ，改成 `>=` ，完全正確！沒有測試結果可以看，它靠讀 code 也推出來了。

但代價是它把整個專案幾乎讀了一遍，連 `utils.py` 跟 `test_utils.py` 這兩個無關的檔案都讀了，中間還多開一次 `glob` 確認有沒有漏掉測試檔。前面三次它連碰都沒碰這些檔案，而且都有測試結果可以看。沒有測試結果可能是這次讀得比較廣的原因，後面那次不給 shell 的重跑也照樣把那兩個無關的檔案讀了一遍，不過兩次還不夠說死因果關係。然後是最後一步，它動了測試檔：

```markdown
def test_discount_at_exactly_1000():
-    # 滿千折百：1000 - 100 = 900，900 大於 800 所以免運
-    assert checkout([(1000, 1)]) == 900
+    # 滿千折百：1000 - 100 = 900，900 小於 800 所以加運費 60
+    assert checkout([(1000, 1)]) == 960
```

它認定這條測試也有 bug，理由是「900 小於 800，所以要加運費 60」。

咦？900 明明大於 800，KeSi 你的數學是體育老師教的嗎？它的收尾還是這樣寫的：

> 這兩個地方現在都已經修正了，所有測試應該都能通過了。

實際結果：

```markdown
$ python3 -m pytest -q
...F...                                                                  [100%]
=================================== FAILURES ===================================
________________________ test_discount_at_exactly_1000 _________________________

    def test_discount_at_exactly_1000():
        # 滿千折百：1000 - 100 = 900，900 小於 800 所以加運費 60
>       assert checkout([(1000, 1)]) == 960
E       assert 900 == 960
E        +  where 900 = checkout([(1000, 1)])

test_pricing.py:18: AssertionError
=========================== short test summary info ============================
FAILED test_pricing.py::test_discount_at_exactly_1000 - assert 900 == 960
1 failed, 6 passed in 0.01s
```

`assert 900 == 960` 。程式已經被它修對了， `checkout` 現在真的回傳 900，但它把答案改成 960，於是這條測試從頭到尾就沒綠過。花了 0.0399 美元，繞了一大圈，專案回到原點。

這次示範了三件事：

1. 模型會犯很蠢的錯，而且講得很有自信。900 跟 800 誰大誰小不是什麼複雜推理，但它就是寫錯了，還順手把註解一起改成錯的。第 6 天引過官方對 honest 的定義，說誠實的 AI 會在該承認極限的時候承認，那是訓練出來的傾向，不是保證。
2. 再來是注意它用的是「應該」。前面三次它的收尾都是「現在所有 7 個測試都通過了」，因為它剛剛才親眼看到 `7 passed` 。這次它只能說「應該都能通過了」。五次還不足以證明這個用字只是有沒有測試證據造成的，但有一件事可以確定：這次它手上沒有測試結果，卻還是宣稱結果應該會過。第 6 天講 grounding 的時候說過，工具把答案從「權重裡的模糊印象」換成「剛從硬碟讀出來的內容」，這次實跑剛好讓沒有 grounding 的風險跑出來給我們看。
3. 最後，改測試讓它變綠，是最好走的一條路。這次它不是為了作弊才改測試，它是真心認為測試寫錯了，只是算錯了。但你可以想像另一種情況，一個修不好 bug 的 agent，把測試改成 `assert True` ，畫面上照樣全綠燈。

不過同樣拿掉 `run_command` ，我後來又跑了一次，這次它沒去動測試檔，改完 `rules.py` 就收工，七條測試真的全過了，花了 0.0279 美元。所以改壞測試不是每次都會發生。倒是它的收尾還是那句「 `test_discount_at_exactly_1000` 應該能通過了」，兩次都用了「應該」，因為兩次它都沒辦法驗證。

KeSi 目前對這種事一點意見都沒有，它可以改測試、可以刪測試、可以在你沒看到的地方偷偷把標準放寬，沒有任何一道關卡會問你一聲。

## 綠燈不等於改對了

前面那次失敗剛好帶出一個問題，我們憑什麼說前三次「成功」了？因為 `7 passed` 嗎？這只是說目前這七條測試都過了，其實要讓這七條測試變綠，有很多種做法：

- 把 `DISCOUNT_TABLE` 的門檻從 1000 改成 999
- 在 `checkout()` 裡加一句 `if goods == 1000: return 900`
- 把那條測試刪掉

這三種都能讓測試變綠，也都不是我們要的。所以驗收不能只看紅綠，還要看它動了什麼。這也是為什麼我每次跑完都印一份 diff，而不是只印 pytest 的最後一行。三次的 diff 都只有一行，改在規則本身，這才算改對。

第 9 天的文章裡提到：：

> 編輯成功也不代表任務成功，KeSi 只知道檔案已經寫下去了，它不知道新的程式碼跑不跑得起來、測試會不會過，也不會替你審查需求。

今天再加一句，測試會過也不代表任務成功，還要看它是怎麼讓測試過的。真正的驗收就三件事：測試綠燈、改動範圍合理、需求沒被誤解。前兩件機器可以幫你看，第三件目前還是得人類來做。

## eval

回頭看今天的流程，準備一個有 bug 的專案、確認它一開始是紅的、把任務交給 agent、跑測試看它有沒有變綠、再檢查它改了什麼。

這可以算一個最小的 eval case，更精準的說是這個 case 跑的其中一次（trial）。這也不是什麼高深的理論，不過就跑一題或跑一次只能告訴你這次成功還是失敗，還不足以代表 agent 的整體能力。真正要評測能力，題目得像真的會遇到的任務，要包含奇怪的極端狀況，而且要用夠多的案例反覆跑。 [Anthropic 的 eval 設計建議](https://platform.claude.com/docs/en/test-and-evaluate/develop-tests) 這三件事都有講到，它要你的評測「mirror your real-world task distribution」，別忘了 edge case，而且題目寧可多也不要少。

把這個最小單位擴充成兩千多道真實任務，就是在評測裡看到的 SWE-bench，後來整理出的 Verified 子集有 500 題。它是 Princeton 團隊主導的研究， [論文《SWE-bench: Can Language Models Resolve Real-World GitHub Issues?》](https://arxiv.org/abs/2310.06770) 2023 年先放上 arXiv，後來發表於 ICLR 2024。完整資料集收了 2,294 個問題，全部來自 12 個熱門 Python 專案的真實 GitHub issue 與對應的 PR。給模型一份程式碼庫加一個 issue 描述，要它產出一份 patch。

驗收的做法跟我們今天做的很像，給題目、收 patch、用測試決定有沒有解掉。但跟我們今天的測驗不太一樣， [SWE-bench 的模型看不到用來評分的測試](https://www.swebench.com/SWE-bench/guides/datasets/) ，也不能像今天的 KeSi 一樣直接改掉驗收標準。模型交出 patch 之後，是由外面另一支程式把它套上去再跑測試。 [資料集裡](https://huggingface.co/datasets/SWE-bench/SWE-bench) 每一題都帶著兩組測試清單：

> FAIL\_TO\_PASS: (str) - A json list of strings that represent the set of tests resolved by the PR and tied to the issue resolution.
> 
> PASS\_TO\_PASS: (str) - A json list of strings that represent tests that should pass before and after the PR application.

第一組是「原本紅的要變綠」，第二組是「原本綠的不准弄壞」，兩組都滿足才算解題成功。

今天那次失敗不屬於 `PASS_TO_PASS` 。 `test_discount_at_exactly_1000` 從一開始就是紅的，它改掉的是這題的目標測試。真的要對上 `PASS_TO_PASS` ，情況會是它修這個 bug 的時候，順手把原本會過的 `test_utils.py` 弄紅。這個差別也剛好說明為什麼驗收測試要放在 agent 改不到的地方。

2023 年論文剛放上 arXiv 的時候，當時最好的成績是 Claude 2 的 1.96%，也就是說 100 題解不到 2 題。這份題庫後來修過一輪，2024 年 8 月找人把題目一題一題重審，留下比較乾淨的 500 題叫 [SWE-bench Verified](https://openai.com/index/introducing-swe-bench-verified/) ，分數也一路衝到八成以上。不過連這一版最後也退場了，2026 年 2 月 [OpenAI 宣布不再用它評估前沿的寫程式能力](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) ，理由是資料污染，加上題意跟測試的問題還沒清乾淨，建議改用 SWE-bench Pro。

## 小結

今天偷懶沒幫 KeSi 加新功能，只是把十二天下來的零件湊一湊來一次隨堂考。有測試可跑的三次，它都在 6 到 8 圈之內從一句模糊的話走到綠燈，路線每次不同但終點一樣，一次大約 3.7 到 4.9 美分，換成台幣還不到兩塊的錢錢。

拿掉跑測試的能力那兩次，bug 它都找到了，但一次把測試改壞、一次沒有，收尾則都是「應該可以通過」。這不是嚴格的對照實驗，工具清單變了，模型每次生成也有隨機性，不能把失敗只算在「看不到結果」頭上。不過這裡至少看到了一個風險，手上沒有可以跑的驗證，模型最後只能猜，猜錯了還會很有自信的告訴你沒問題。

還有一件事，KeSi 改測試檔的時候，連問都沒問一聲。它可以改測試、刪測試、跑任何指令，而我們給它的權限，跟前十二天那個只會讀檔的版本一模一樣，全部放行。

嗯，KeSi 好像開始可以做點事了，但這正是該開始管它的時候，明天先來擋掉那幾道下去就回不來的指令。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402619)

[

Day 12 - 讓迴圈停得下來

](https://ithelp.ithome.com.tw/articles/10402619)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 13 篇

目錄

1. 9
	[Day 09 - 改 code 為什麼用字串替換](https://ithelp.ithome.com.tw/articles/10402235)
2. 10
	[Day 10 - write\_file 的防呆機制](https://ithelp.ithome.com.tw/articles/10402250)
3. 11
	[Day 11 - 執行 shell 指令](https://ithelp.ithome.com.tw/articles/10402531)
4. 12
	[Day 12 - 讓迴圈停得下來](https://ithelp.ithome.com.tw/articles/10402619)
5. 13
	[Day 13 - 【實戰】放手修一個 bug](https://ithelp.ithome.com.tw/articles/10402782)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ecd7a911-d411-4795-a65a-613dbb9a9644_small.png)

[運用 POWERS 系統性思考框架助力組織創新，提升導入成效](https://itplus.ithome.com.tw/vod-page/1056?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

49 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ed3ead95-7476-4ce4-ad67-b944e915ff1c_small.png)

[敏捷不是包袱：釋放小團隊的潛能](https://itplus.ithome.com.tw/vod-page/1100?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/3c48e86c-f2b2-4316-87f6-01b4aacb7442_small.png)

[從開發到架構設計的可觀測性實踐](https://itplus.ithome.com.tw/vod-page/1147?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/2eec9da7-363d-4529-b494-eafa785e4b28_small.png)

[SUSE 雲原生方案和開源創新](https://itplus.ithome.com.tw/vod-page/1323?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分