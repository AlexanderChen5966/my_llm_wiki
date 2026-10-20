---
title: "使用 Jev 建立判斷特化型 GraphRAG（未使用向量）"
source: "https://codelove.tw/@tony/post/ayoJMx"
author:
published:
created: 2026-10-14
description: "本文的 chunk 資料中登錄了《名偵探柯南》的資料（另外也有少量《ONE PIECE》和《光之美少女！名偵探》）。如果你不認識《名偵探柯南》，建議先看動畫或把單行本全部讀完再來讀這篇文章。大家是不是把 GraphRAG 的所有判斷都交給 LLM 了？如果是不需要寫文章的判斷，那就交給擅..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月10日](https://codelove.tw/@tony/post/ayoJMx "2026-10-10 09:30:10")

## 使用 Jev 建立判斷特化型 GraphRAG（未使用向量）

本文的 chunk 資料中登錄了《名偵探柯南》的資料（另外也有少量《ONE PIECE》和《光之美少女！名偵探》）。如果你不認識《名偵探柯南》，建議先看動畫或把單行本全部讀完再來讀這篇文章。

探索數據管理

大家是不是把 GraphRAG 的所有判斷都交給 LLM 了？如果是不需要寫文章的判斷，那就交給擅長判斷的模型來做就好了！因此，我嘗試做了一個不使用 embedding、向量資料庫、圖資料庫的 GraphRAG。判斷交給 Jev，回答交給 LLM。

## 👀 對象讀者

- 做過 RAG，曾因為「語意相近」和「回答所需」之間的落差而困擾的人
- 曾考慮過 GraphRAG，但卡在建置時 LLM 擷取成本的人
- 想找 Jev 這種判斷專用模型的使用時機的人
- 超喜歡《名偵探柯南》的人

## 🧐 只有「語意相近」還不夠，因為有些問題答不到

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791948730&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639979&amp;bpp=3&amp;bdt=959&amp;idt=3&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791948307%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1519x873&amp;nras=3&amp;correlator=3241191485424&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=109&amp;ady=1067&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1519%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

一般的 RAG 會找出和問題語意相近的文章。麻煩的是，答案可能是用別的名字寫的，或是必須沿著 chunk 一路追到答案。

> 「工藤新一現在住在哪裡？」

答案是毛利小五郎的偵探事務所。新一被藥物變成小孩，並以「江戶川柯南」的身份待在毛利家。不過資料裡寫的是這樣的句子：

閱讀熱門漫畫

> 柯南在隱瞞真實身分的情況下寄住在毛利蘭家，並以蘭的父親毛利小五郎經營的毛利偵探事務所作為據點解決案件。

問題問的是新一，但答案寫在柯南的文章裡，所以只靠語意相近去找，很難找到。GraphRAG 就可以沿著「工藤新一 → 江戶川柯南 → 毛利偵探事務所」一路找到這句話。

[![01-x-vector-vs-graph.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F61e1f6c5-ae11-428e-ad9b-4f2d85d0cab2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3f4891489306aa94baf1478ef065ba48)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F61e1f6c5-ae11-428e-ad9b-4f2d85d0cab2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3f4891489306aa94baf1478ef065ba48)

## 💸 為什麼選用判斷專用型來試：GraphRAG 很貴

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791948730&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639988&amp;bpp=1&amp;bdt=968&amp;idt=1&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791948307%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1519x873%2C952x280&amp;nras=4&amp;correlator=3241191485424&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=109&amp;ady=2359&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1519%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

GraphRAG 雖然方便，但很花錢。因為 GraphRAG（方法很多種）會讓 LLM 去做從文章中擷取人物與關係的工作。

學習AI 課程

- 建置時：每篇文章都用 LLM 擷取人物與關係。
- 回答問題時：若要讓 LLM 判斷「要不要沿著這條線走？」、「已經能回答了嗎？」，就會呼叫很多次 LLM。

但仔細看內容，其實很多都只是可以用是／否解決的判斷而已。  
最近出現了「判斷專用」模型，所以我想：如果交給它們做，會不會便宜一些？於是試了看看。

## 🎯 Jev 只會回傳「是」的可信度

Jev 是 TypeSafe AI 的判斷專用模型。它不會寫文章，只要丟問題給它，就會回傳「是」的可信度，範圍是 0～1。

丟給 Jev 的內容與回傳值

```
丟入內容：問題「工藤新一現在寄住在哪個人的家中，並以哪裡為據點解決案件？」
提問　　：這個問題句中有「工藤新一」嗎？
回傳值　：0.95
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791948730&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639990&amp;bpp=1&amp;bdt=970&amp;idt=1&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791948307%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791948307%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1519x873%2C952x280%2C952x280&amp;nras=5&amp;correlator=3241191485424&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=109&amp;ady=3259&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1519%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

因為只有數字，所以可以像一般 if 句一樣，用「超過 0.5 就往下走」來決定行為。

[![02-x-jev-vs-llm.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fa96fdb73-11cc-4468-b88d-08b4c2923926.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fa10cfd04a4ac4ca05cba15266b0ff5f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fa96fdb73-11cc-4468-b88d-08b4c2923926.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fa10cfd04a4ac4ca05cba15266b0ff5f)

Jev 也不是完全不會飄移的（請把它理解成「比較不容易飄移」的程度）

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791968889&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639993&amp;bpp=1&amp;bdt=973&amp;idt=0&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791962122%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791962122%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791962122%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1519x873%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=3241191485424&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=109&amp;ady=4224&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=742&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGslt5FhP4FaGzMpUBz1nlGJDmKrHF1ekfH79gJuIp0kt-iffKMcoziPkVcM5qV73UFVsegm1YLv1ZdQmflObk4MWCMPr3Hm1Nu17eqyorHBylLBkawJugJaHCOEZR1vyi9Mg%2CAOrYGsn6qnijkS5TSlComNHtkdL6PD-jur3xDpLXafApBL0vTjyvZlReIFWRx2GvXh-eiMIwvGiWNIh2n0V7aWKwqb6uBgfK6Znkg6P4I83uqKkU6A%2CAOrYGskOLmS2cjxTZvyVZwwxvcV7mE5Hr95rxKpH-KUGFdAqHAnkD-FRp9T-iPCB3Xl49tZLcWVWPsPvHeLinA1DEWfeoJIUTncYEQWuD5RTzrHUrw%2CAOrYGsm4rgHRtdjqHez6BmVpTTgRvBfEuZLZhaSoozmWxEGVO8YbibDT0aqiMPvNRiWBkgG63mN7zFRRjqq7hlFBhYD-QqZ4Z1cq7_K27dtTJCE&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1759%2C954%2C1519%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 這次的整體架構

[![architecture-ja.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F03ccf449-0fef-4c3d-9a14-eaf23312901d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2adee708e200ed8496def2631e58a844)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F03ccf449-0fef-4c3d-9a14-eaf23312901d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2adee708e200ed8496def2631e58a844)

實際畫面大概是這樣

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791968890&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639995&amp;bpp=1&amp;bdt=975&amp;idt=1&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791962122%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791962122%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791962122%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1519x873%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=3241191485424&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=109&amp;ady=5301&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=1821&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGslt5FhP4FaGzMpUBz1nlGJDmKrHF1ekfH79gJuIp0kt-iffKMcoziPkVcM5qV73UFVsegm1YLv1ZdQmflObk4MWCMPr3Hm1Nu17eqyorHBylLBkawJugJaHCOEZR1vyi9Mg%2CAOrYGsn6qnijkS5TSlComNHtkdL6PD-jur3xDpLXafApBL0vTjyvZlReIFWRx2GvXh-eiMIwvGiWNIh2n0V7aWKwqb6uBgfK6Znkg6P4I83uqKkU6A%2CAOrYGskOLmS2cjxTZvyVZwwxvcV7mE5Hr95rxKpH-KUGFdAqHAnkD-FRp9T-iPCB3Xl49tZLcWVWPsPvHeLinA1DEWfeoJIUTncYEQWuD5RTzrHUrw%2CAOrYGsm4rgHRtdjqHez6BmVpTTgRvBfEuZLZhaSoozmWxEGVO8YbibDT0aqiMPvNRiWBkgG63mN7zFRRjqq7hlFBhYD-QqZ4Z1cq7_K27dtTJCE%2CAOrYGskXFvFHjurUMI89TcKStq8F1B2Pyd51ECnCwDjjrwZqjdFkco9_qvkyShfAC0Pzv8j7SPvIH0Mauf8THmG95753HilrorTqweeK7H7XNQb6&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1759%2C954%2C1519%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

就是這樣。不過有點雜亂、看起來不太好懂，所以我會按順序說明。

## 🔧 判斷交給 Jev

我選了 6 個「這裡就是判斷」的地方，全部交給 Jev。  
只有最後寫答案時才會用到 LLM（Claude Haiku4.5）。

[![03-fig-judge-points.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F294c202d-8426-4f52-bee9-702018f85d96.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=950ee68e0a324813521c01ae81112591)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F294c202d-8426-4f52-bee9-702018f85d96.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=950ee68e0a324813521c01ae81112591)

- 準備：這篇文章有沒有出現這個人？
- 準備：這篇文章能不能說這兩個人有關係？
- 被提問時：這題是在問誰？
- 被提問時：這篇文章對回答有沒有幫助？
- 被提問時：這個關係看起來能不能當線索？
- 被提問時：蒐集到的文章已經足夠回答了嗎？

## 🗂️ 圖的主更新資料

上面提到的「這個人」「這兩個人有關係」到底對應什麼，是由主資料決定的。

- 種別：9 種（作品、角色、組織、地點、道具等）
- 關係：12 種（不同種別之間用什麼方式連接）
- 實體：212 筆（正式名稱、別名、種別、簡短說明）

> 毛利小五郎／別名：小五郎、沉睡的小五郎、毛利大叔、老爸、毛利先生／種別：角色／說明：蘭的父親。經營毛利偵探事務所

種別與關係部分就是所謂的本體論（ontology）。可以把主資料理解成把這次資料中的實體套進這個本體論後的結果。程式會參照這份主資料來建構圖。

[![04-fig04-master.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F161a3815-f598-43da-8a0e-c49f83e54190.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e255c3f8af388336302020836afb8e3f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F161a3815-f598-43da-8a0e-c49f83e54190.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e255c3f8af388336302020836afb8e3f)

我特別做的設計，是故意把關係做得比較粗。  
角色之間只用「有關係」這一種，至於家人、師徒等關係則作為例子寫進句子裡。

> 毛利蘭和毛利小五郎有關係（家人、青梅竹馬、夥伴、師徒、敵對等。過去或現在都可以）

如果拆得太細，詢問次數會變多，而且分數會被分散，不容易跨過門檻。  
家人或敵對這種細節，最後交給 LLM 看本文就能判斷，所以圖只要知道有連起來就夠了。

## ⛏️ 準備：只靠判斷來建立圖

[![05-fig-ingest-flow.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F33ec23ab-4b5b-4456-90f8-f1617efd6e27.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d4fe1fec802fe6ad73f6df61f2923561)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F33ec23ab-4b5b-4456-90f8-f1617efd6e27.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d4fe1fec802fe6ad73f6df61f2923561)

每篇文章都先用主資料裡的名稱抓候選，再問 Jev「有出現嗎？」、「有關係嗎？」，只把像是「是」的東西存進圖裡。比起用 LLM 擷取，成本低很多（試算大約是 20～30 分之 1）。

## 🔍 被提問時：沿著關係走，資訊夠了就停止

[![06-fig07-search-loop.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faf86f1b2-668d-4025-b3a6-fdb893dbace2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=76918bb242db8d1ab64d49aabfbbc82f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faf86f1b2-668d-4025-b3a6-fdb893dbace2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=76918bb242db8d1ab64d49aabfbbc82f)

先決定起點，然後一路往旁邊走，直到「已經能回答了嗎？」變成 Yes 為止（最多 3 次）。 什麼時候停也是由 Jev 決定，所以如果 1 次就能回答，就只會停在起點。

> 「住在發明柯南常用道具的人家裡的是誰？」

實際畫面如下

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fa0e55e5b-b829-42c1-b59b-88f0f3421922.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d3fc8253b0ba1e7bec8a0a4039dc074f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fa0e55e5b-b829-42c1-b59b-88f0f3421922.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d3fc8253b0ba1e7bec8a0a4039dc074f)

- 起點：江戶川柯南（回答資訊不足）  
	→ 其實這時候已經有「灰原哀和博士一起住」這個可作為答案的 chunk，但因為還沒連到「博士發明了道具」這個事實，所以判斷為資訊不足
- 第 1 次：前進到麻醉槍、蝴蝶結領結、鞋子等實體去找 chunk  
	→ 這時候判明了製作手錶型麻醉槍的是阿笠博士，再加上入口 chunk 中「博士和灰原一起住」這件事，就能判定答案已足夠。

答案：灰原哀住在阿笠博士家，而和發明柯南常用道具的人一起住的人就是灰原哀。

## ✂️ 不讓 Jev 暴力窮舉的技巧

如果對所有文章 × 所有實體都詢問，330 篇 × 212 個就是 69,960 問。實在太多了，所以會先縮小候選範圍。

首先用程式檢查主資料裡的名稱／別名是否出現在本文中（不使用 Jev、免費）。 雖然會統一表記差異，但不會先切成單詞。像「名偵探柯南」會同時成為作品和江戶川柯南的候選，至於是哪一個，交給 Jev 判斷。

- 有沒有出現：只問有找到名稱的項目（69,960 問 → 1,457 問）
- 有沒有關係：如果是人對人，就只問「有關係嗎？」而不問「是否隸屬某組織？」
- 起點：沒有出現名稱的問題，先問「是在問人？還是在問地點？」如果是人，就只和角色比對
- 往旁邊走：只把相連的鄰居當候選，最多取前 3 個

縮小前，某個問題丟給 Jev 的輸入量大約有 7 萬 tokens；縮小後，只要約 6,400 tokens 就夠了。

## 🔢 因為是用數字回傳，所以可以比較

> 「琴酒平常開的車是什麼來著？」

[![07-fig10-entry-chunk-scores.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F19ccfe23-e97c-4b63-8a56-222cf8f278d8.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c18f890b6b4468e20ed3019578ed52a5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F19ccfe23-e97c-4b63-8a56-222cf8f278d8.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c18f890b6b4468e20ed3019578ed52a5)

只有「琴酒的愛車是黑色保時捷 356A」拿到 0.97，其餘都不到 0.1。雖然主資料裡沒有「愛車」這種關係，還是成功找到了。簡單的問題常常可以靠這種模式在 0 hop 直接回答。

> 「偵探大叔的女兒是誰？」

像這種名字沒出現的問題，就會改問「是不是同一個人？」。毛利小五郎以 0.81 被選中，而小五郎的文章裡有「女兒蘭」，所以就能回答毛利蘭。

[![08-x-reask.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F5b6891da-06ab-4556-9e80-5f9b68cfe25d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=61f278ac89f17cb0cd321d704f2d75a0)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F5b6891da-06ab-4556-9e80-5f9b68cfe25d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=61f278ac89f17cb0cd321d704f2d75a0)

## 📊 驗證：簡單問題

[![09-x-results.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F988ace90-8189-4c52-94e5-4e2f0aeda5d5.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=daa678d9b41edaba6bca46e63e866c37)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F988ace90-8189-4c52-94e5-4e2f0aeda5d5.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=daa678d9b41edaba6bca46e63e866c37)

Jev 會把它在何處判斷了什麼全部保留成分數，所以事後也能追蹤理由，這點也很實用。

以下是建立在「主資料正確、分數門檻設定得當」前提下的驗證結果。  
不能保證精度。

## ⚖️ 驗證：和使用 embedding 的 GraphRAG 比比看

在同一個圖、同樣 35 題、同一個回答用的 LLM 下，只改起點尋找方式與判斷工具來比較。

- ① 這次的方法：不用向量、由 Jev 判斷
- ② embedding + LLM：起點用 embedding 找，判斷用 LLM（Haiku4.5）
- ③ 只有 embedding：不判斷停止時機，所以固定走 3 hop，並把蒐集到的文章全部交給 LLM

[![x-compare-graphrag.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fbecf51ae-c892-47aa-b69f-bfe5924ed3da.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c68773afb93410f7748842a4182f793c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fbecf51ae-c892-47aa-b69f-bfe5924ed3da.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c68773afb93410f7748842a4182f793c)

③ 因為無法判斷「已經夠了嗎？」，所以只能把蒐集到的文章全部交給 LLM，導致輸入文字（token）暴增。② 因為每次判斷都要呼叫 LLM，所以成本最高，比①還高出 4 倍以上。

[![x-compare-ingest.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff85c22b5-039f-4913-a36e-146d9b2f3a7a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=02c868bcab96255058b72faadfd98e2e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff85c22b5-039f-4913-a36e-146d9b2f3a7a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=02c868bcab96255058b72faadfd98e2e)

## 驗證：需要 hop 的問題

上面是一般問題的結果比較。接下來是把問題改成「題目裡的名稱和答案不在同一篇文章裡，必須沿著 chunk 走」的情況，再比較一次。

[![x-compare-hop.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff0cd4049-4bf0-4c19-b61e-4c9710b6e155.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=adcedd66a1681d847a031a0110a35ba6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff0cd4049-4bf0-4c19-b61e-4c9710b6e155.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=adcedd66a1681d847a031a0110a35ba6)

在需要 hop 的問題上，把判斷交給 Jev 的①表現最好。② 也會靠推論往下 hop，所以表現接近，但途中誤判稍微明顯。③ 因為不做判斷所以便宜又快，但也因為不推論，所以無法正確 hop。

回頭看，能看出這次的題目類型，剛好對①比較有利！\[:bow\_tone1:\]  
請以這次驗證條件下的參考值來看待。

## ⚠️ 不能說「完全不會幻覺」

Jev 不會寫文章，所以不會硬編造不存在的人物；但如果它判斷錯誤、漏掉重要文章，就可能把本來答得出來的問題拒絕掉。  
所以比較適合把它看成「不太容易產生沒有根據的答案」。另外 Jev 並不是決定性的判斷工具，因此搜尋結果不同時，hop 也可能改變，答案也可能出現波動。

## 🚧 沒成功的地方

### 主資料裡沒有的東西就追不到

> 「山治二年修行的國家的女王是什麼果實能力者？」

中間的「卡瑪巴卡王國」不在主資料裡，所以路線就斷掉了（突然變成 ONE PIECE 抱歉）

[![10-x-gap.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faac509fa-76a8-4af7-9736-d95fee1364e2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6fe1251336ce4cc0cf8f67b9b88816da)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faac509fa-76a8-4af7-9736-d95fee1364e2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6fe1251336ce4cc0cf8f67b9b88816da)

如果不認識「卡瑪巴卡王國」，請先把《ONE PIECE》全卷看完再回來讀這篇文章

- 沒有答案的問題成本很高  
	因為會一路找完才拒答，所以呼叫 Jev 的次數大約是能立刻回答的問題的 2.8 倍
- 成本削太多，準確率會掉  
	我試著再用 Jev 去縮小要交給 LLM 的文章範圍，結果把回答所需的「橋接文章」也一起刪掉，導致拒答題目從 6 題增加到 10 題。所以後來放棄了這個縮法。

## 🌱 還沒做到的事：養成主資料的機制

文章一多，主資料裡沒有的新東西就會一直冒出來。每次都靠人工補上，沒辦法持續，所以需要能自動養成的機制。種別和關係通常不會輕易改變，所以理論上只要增加實體就夠了。

[![11-x-grow-master.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fdbc0dad3-ea66-49aa-898b-aac9a8592f98.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1bb2116151cff1dc9404a48c1e62a04f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fdbc0dad3-ea66-49aa-898b-aac9a8592f98.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1bb2116151cff1dc9404a48c1e62a04f)

1. 把主資料沒有的詞和包含它的文章一起先存起來
2. 由 Jev 判斷它是人物、地點，還是單純的詞
3. 再由 Jev 判斷它是不是現有某人的別名
4. 只有真正全新的東西才交給 LLM 寫說明
5. 更新主資料，並重新登錄只包含那個詞的文章

這裡同樣是大量篩選交給 Jev，寫說明則交給 LLM。

## 🏢 實務上能用嗎？

雖然前面都在講好聽的地方，但...

> 不是不能用，但……操作起來非常敏感！\[:sweat\_smile:\]

老實說，我的感想就是這樣...

### 麻煩的地方

- 先準備實體主資料：需要先準備好圖的型態作為初始資料。以本體論為基礎用 LLM 建立的部分，這裡會大幅影響精度
- 分數門檻的調整：像是 0.5 就往下走、0.7 就停止這類數值，只能一邊看資料與問題傾向一邊抓出微妙的門檻

反覆試、修正門檻、補上缺少的實體，再試一次。  
與其說是方便工具，比較像是需要花心力一起養成的系統。

如果是模糊的問題或摘要，標準 RAG 或 MSGraphRAG 的全域搜尋會更適合。  
我認為這是要看使用場景搭配組合的東西。

## 📈 如果資料變多，成本會怎麼樣？（我的看法）

Jev 是一筆一筆問候選，因此文章越多，詢問次數也會增加。

不過，放進 RAG 的文章通常會集中在某個固定領域。同一個領域裡，就算文章變多，人物與關係也會逐漸補齊，增長幅度應該會趨於平緩。

[![12-x-scale.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ffdfa11ff-3e1d-4c46-b98d-2a0d40f97a65.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6d6bb146086472e6f9a540702a00f190)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ffdfa11ff-3e1d-4c46-b98d-2a0d40f97a65.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6d6bb146086472e6f9a540702a00f190)

所以就算資料稍微變多，應該也不會變成倍數成長，這是我的看法。

這部分我還沒實測，所以仍然只是推測

## 🍜 結尾

它並沒有變成當初想像中的「我心目中最強的 RAG」！\[:sob:\] 前置準備和調校超級麻煩，如果團隊裡沒有 RAG 狂熱者，實務上很難運作！\[:dizzy\_face:\]

實際驗證後，我感受到自己腦中原本有「RAG＝Embedding」這種常識，所以能擺脫向量有點新鮮，哈哈。Jev 以及其他判斷專用模型也陸續登場，而 2026/10 時點也已經有像 haiku5.5 這種高速輕量 LLM 模型出現，因此不再被向量綁住，能從更廣的選項中挑選適合工作負載的方案，是很好的事情。

最後還有一點反省...  
我這個人在做驗證資料（問題）時，總是會忍不住做出符合自己腦中故事走向的資料。（想法太強烈了……）先有結論再去設計驗證資料是沒有意義的，之後想要改善這點！\[:bow\_tone1:\]

之後也想再做更多驗證。

## 驗證資產

---

原文出處： [https://qiita.com/kikuziro/items/6191b5ad83520d181f38](https://qiita.com/kikuziro/items/6191b5ad83520d181f38)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ayoJMx) [24瀏覽](https://codelove.tw/@tony/post/ayoJMx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ayoJMx)

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

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [我用 2006 年的 iMac 寫作，在一台 200 美元的筆電上寫程式](https://codelove.tw/@tony/post/qvd7g3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝46 💬1

711

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1791945639&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FayoJMx&amp;pra=5&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuNDAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuNDAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS40MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1791945639170&amp;bpp=2&amp;bdt=192&amp;idt=48&amp;shv=r20261013&amp;mjsv=m202610080101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791945576%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791945576%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791945576%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=3241191485424&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1519&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C122881580%2C42533293%2C95340253%2C95340255&amp;oid=2&amp;pvsid=3150007624604266&amp;tmod=466374180&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1519%2C873&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=67" title="Advertisement" aria-label="Advertisement"></iframe>