---
title: LLM 從零開始的模型訓練：神經可塑性 Transformer 實驗思路
source: 社群貼文
created: 2026-06-01
tags:
  - clippings
status: processed
---
最近我在玩什麼呢？兩個：  
1. 在 RTX 5080 16GB 利用技術的方式，生成連續且穩定的長影片（目前做到 120 秒，目標 5 分鐘。）  
2. LLM 從零開始的模型訓練。  
  
先來說說第二個，第一個我不知道有沒有人有興趣，但我也還在實驗。  
  
說真的，像我這種窮人，說真的不太可能走大公司那種路線：先訓練一個 100B+ 的大模型，再蒸餾成一個比較小、比較好部署的模型。  
  
這條路很正規，也很有效，但問題很簡單：我沒那個錢，也沒那個算力。  
  
所以我最近在想另一條比較窮人但有趣的路線：  
  
不是一開始就把模型做很大，  
而是先從小模型開始，例如 0.5B / 0.6B 左右，  
然後讓模型像神經一樣，在訓練過程中慢慢「長大」或「修剪」。  
  
大概概念是這樣：  
  
1. 如果某些 layer 長期出現 capacity pressure  
  例如 training loss / validation loss 都 plateau，  
  而且不是單純 overfitting，  
  那就讓那一層新增一些 MLP neuron 或 attention head。  
  
2. 如果某些 neuron / head 長期低激活、低梯度貢獻、ablation 後 loss 幾乎不變，  
  那就把它先 mask 掉，再觀察一段時間，最後才真的 prune 掉。  
  
3. 新增參數時不直接亂加，  
  而是用 zero-output growth 或 duplicate/split 的方式，  
  讓新增的 neuron/head 一開始不要破壞原本模型輸出。  
  
4. 整個模型不直接在每個 training step 改 tensor shape，  
  而是用 active mask / gate 控制哪些節點活著，  
  到 checkpoint boundary 再做 physical compaction。  
  
說白一點，就是想做一個比較像「神經可塑性」的 Transformer：  
  
* 神經新生：新增 MLP neuron / attention head  
* 神經修剪：移除長期低貢獻的 neuron / head  
* 神經分裂：把高壓力、高重要性的 neuron/head duplicate 出一份  
* 恆定調節：限制 active parameters，不讓模型無限長大  
* 凋亡機制：先休眠，再經過驗證，最後才物理刪除  
  
當然啦，這東西我不會說它現在很穩，也不會說它一定實用。  
  
看了一些論文後，我自己的感覺是：  
這方向有研究基礎，但距離「可以穩定用在 LLM pretraining」還有一段距離。  
  
相關概念大概來自幾條線：  
  
* Net2Net：function-preserving expansion，讓小網路變寬或變深時盡量不破壞原本函數。  
* DEN：Dynamically Expandable Networks，根據任務需要動態增加必要 units。  
* NeST / Grow-and-Prune：用 grow-and-prune 的方式讓網路拓撲自己演化。  
* RigL / Dynamic Sparse Training：根據 gradient / magnitude 動態更新稀疏連線。  
* Attention Head Pruning：研究 Transformer 裡哪些 attention head 其實可以被移除。  
  
我目前比較想做的不是「整層增長」，而是更細的結構化單元：  
  
* MLP intermediate neuron  
* attention head  
* attention head dimension  
* 之後才考慮 layer-level growth  
  
原因也很簡單：  
整層增加太粗，而且工程上容易把 residual stream、optimizer state、checkpoint、torch.compile 全部搞炸。  
先從 neuron/head 級別做，至少比較可控，也比較符合「哪裡不夠補哪裡」的想法。  
  
這篇不是要說我做出什麼很猛的東西。  
比較像是我因為資源不夠，但手癢，所以只能走這種奇怪的窮人研究路線。  
  
如果有興趣討論，我比較想聽大家對這幾個點的看法：  
  
1. grow trigger 應該怎麼定義？只看 loss plateau 夠不夠？感覺上節點的評判又會跟任務有關。  
2. neuron / head importance 用 activation、gradient、Fisher、還是 ablation delta 比較合理？  
3. 在 Transformer 裡做節點級 growth/pruning，怎麼處理 optimizer state 最乾淨？  
4. active mask + checkpoint compaction 這種做法，工程上會不會比直接重建模型更實際？  
5. 這種方法對小模型 pretraining 到底是有幫助，還是只是增加不穩定因素？  
  
目前我自己的判斷是：  
  
它不是成熟工程方案，  
但很值得實驗。  
  
如果真的能讓模型像神經一樣，自動增長、休眠、修剪、壓縮……一定很好玩XD  
  
好拉，這個也沒什麼技術難度，基本上就是想法跟 GPT 討論，然後看一下相關論文，接著規劃完請 GPT 把 spec 寫完，剩下請 codex 幫我實做。  
  
說真的，我真的覺得技術的落地變得越來越簡單了，門檻有夠低，低到我覺得這東西就向是我高中獨自做學校內科展的感覺。  
  
小弟能力不強，沒什麼技術，好玩就好。
