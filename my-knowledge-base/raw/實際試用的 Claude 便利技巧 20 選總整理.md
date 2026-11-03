---
title: "實際試用的 Claude 便利技巧 20 選總整理"
source: "https://codelove.tw/@tony/post/3jAopx"
author:
published:
created: 2026-10-30
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------我開始使用 Claude 之後，整理了 20 個實際試過、看起來能提升工作效率的功能。  這篇文章的特色是，我是透過..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月28日](https://codelove.tw/@tony/post/3jAopx "2026-10-28 17:03:30")

## 前言

我開始使用 Claude 之後，整理了 20 個實際試過、看起來能提升工作效率的功能。  
這篇文章的特色是，我是透過同一份 Terraform 程式碼一路測試下來的。

學習AI 課程

> 貼上架構圖圖片 → Terraform 產生 9 個檔案、573 行 → 用 `/goal` 把 trivy 的 HIGH/CRITICAL 從 6 件降到 0 件 → 再讓 3 個子代理平行審查 → 結果還是冒出 3 件架構上的 High 指摘

我把這個流程連同截圖一起整理出來了。

**摘要**

- [CLAUDE.md](http://claude.md/) 是 **「請求」** ，所以不一定會被遵守；permissions 和 Hooks 是 **「強制」** ，所以一定生效。會導致事故的規則（ `terraform apply` / `destroy` 等）要放在強制那邊
- 一開始只要做 3 件事就夠了： **① 用訪談方式建立 [CLAUDE.md](http://claude.md/) ② 用 permissions 禁止危險操作 ③ 平常用自動模式**
- 就算把靜態分析（trivy）降到 0 件，AI 審查還是可能會抓到設計問題。工具和 AI 審查最好併用

**適合這樣的人**

- 已經開始用 Claude Code，但不知道除了 [CLAUDE.md](http://claude.md/) 之外還要設定什麼
- 對於要把 AI 代理用在基礎架構（AWS / Terraform）上感到害怕
- 不只想看功能介紹，也想知道「實際用了之後變成怎樣」

本文資訊以 2026 年 10 月為準。Claude Code 更新很快，指令與費用請也一併確認官方文件。

學習AI 課程

## 全貌

#功能一句話類別 [1CLAUDE.md](http://1claude.md/) 每次一開始都會讀的前提說明書基礎2 **permissions / Hooks\*\*** 一定會執行／阻擋的規則 **\*\*基礎（最重要）** 3`.claude` 資料夾skills / agents / rules・設定放置處基礎4權限模式安全與速度兼顧的使用方式基礎5架構圖 → Terraform一張圖片產生 573 行程式碼實作6 `/goal` 讓它持續工作到符合完成條件實作7讓 3 位子代理平行審查實作8 `/ultrareview` 多個 AI 審查者的檢驗實作9建立技能把重複作業存成步驟手冊效率化10Superpowers 工作術把技能整合導入的外掛效率化11清理對話 `/context` `/compact` `/clear` 維護12 `/doctor` 環境健康檢查維護13小技巧 `/rewind` `/btw` `/fast` `ultrathink` 小技巧14定期執行在固定時間自動工作進階15Claude in Chrome讓 Claude 操作瀏覽器進階16Remote Control用手機操作 Claude Code進階17跨會話訊息在不同對話間接手進度進階18Dynamic Workflow整個 AI 組織一起運作進階19與 NotebookLM 分工把大量閱讀交給 NotebookLM外部工具整合20與 Codex 連動製作設計感較高的簡報外部工具整合---

學習AI 課程

## 第 1 部：先建立基礎

### 1\. 建立 CLAUDE.md

#### 是什麼

這是 Claude Code 每次開始對話時一定會先讀取的檔案。把工作內容、格式、語氣等每次都要說明的「前提」先寫進去，新對話就能從一開始就理解你的情況。

學習AI 課程

不過內容本質上只是「請求」，沒有強制力。想要絕對遵守的內容，請放到下一節的「2. permissions / Hooks」。

#### 放置位置

檔案效力範圍Git 管理 `~/.claude/CLAUDE.md` 你所有專案不管理 `<repository>/CLAUDE.md` 該儲存庫（團隊共享）管理#### 建立方式：比起 `/init` 更推薦訪談式

執行 `/init` 之後，Claude 會看資料夾內容並推測產生 [CLAUDE.md](http://claude.md/) 。雖然方便，但本質上還是 AI 的推測。

比較推薦的方式是 **讓 Claude 來訪談你** 。

```
請先針對我的工作提問，並根據我的回答幫我建立 CLAUDE.md
```

它會像「你的主要工作角色最接近哪一種？」這樣搭配選項問你幾題，你只要回答就好。因為能用自己的話細部設定，所以準確度更高。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fd9d8a62f-4cdc-4381-be67-e076c0a4252d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4bf06f385178f6e21f5d83bee9fe8aee)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fd9d8a62f-4cdc-4381-be67-e076c0a4252d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4bf06f385178f6e21f5d83bee9fe8aee)

#### 實際做出來的 CLAUDE.md

我目前還在受訓，所以為了避免之後派到單位後卡關，有加入 **學習支援** 和 **安全規則** 。

- 回答時附上 1～3 行「為什麼要這樣做」
- AWS / Terraform 的術語第一次出現時要解釋
- `terraform apply` / `destroy` / `import` / state 操作，在執行前一定要先確認。apply 前要先摘要 plan 結果
- IAM policy 使用 `*` 時，要說明理由與風險

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Ffd8f3a74-9fe4-404d-af3f-4e707e4c6af0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a51a79eabb013e1d9c9d3a1f45069398)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Ffd8f3a74-9fe4-404d-af3f-4e707e4c6af0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a51a79eabb013e1d9c9d3a1f45069398)

#### 養成方式與重點

- 對話中只要說「把剛剛那個加到 [CLAUDE.md](http://claude.md/) 裡」，它就會補上
- 內容只是純文字檔，隨時都能查看、修改、刪除
- 郵件文風、文件格式、使用技術等「每次都在講的事」都放這裡
- 但不要寫太長。因為就算跟這次工作無關，也會每次都讀，會拖慢效率

---

### 2\. permissions / Hooks（本文最想傳達的重點）

#### 是什麼

可以設定「絕對不要做」的操作，或是在作業節點上「一定要執行」的規則。  
[CLAUDE.md](http://claude.md/) 是 Claude 讀了之後遵守的「請求」，但這個機制是 Claude Code 本身去執行，所以一定會生效。

學習AI 課程

機制能做的事強制力 [CLAUDE.md](http://claude.md/) 傳達前提與規則沒有permissions允許／禁止特定命令有Hooks在固定時機自動執行命令有不是「小心不要犯錯」，而是能做出「一定要確認有沒有犯錯」或「根本不讓它犯錯」的機制。

#### Hooks 的啟動時機

時機何時 `PreToolUse` Claude 要執行命令或編輯檔案之前 `PostToolUse` 剛執行完之後 `SessionStart` 開始對話時 `Stop` Claude 回覆結束時#### 設定範例

寫在 `.claude/settings.json` 。

.claude/settings.json

```
{
  "permissions": {
    "deny": ["Bash(terraform apply:*)", "Bash(terraform destroy:*)"]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "f=$(jq -r '.tool_input.file_path'); case \"$f\" in *.tf) terraform fmt \"$f\";; esac"
          }
        ]
      }
    ]
  }
}
```
- `permissions.deny` ：禁止 `terraform apply` / `destroy`
- `PostToolUse` ：在編輯／儲存.tf 檔案後，自動執行 `terraform fmt`

#### 實際試看看

**① 儲存.tf 檔時，會不會自動幫我格式化**

我故意讓 Claude 儲存一個縮排亂掉的.tf 檔案。

學習AI 課程

```
# 儲存前（亂掉的）
locals {
project="hooks-demo"
    env =     "test"
  owner= "your-name"
}

# Hook 自動執行 terraform fmt 之後
locals {
  project = "hooks-demo"
  env     = "test"
  owner   = "your-name"
}
```

→ 儲存後立刻就被格式化了，完全不用另外說。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793347754&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3jAopx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347388766&amp;bpp=1&amp;bdt=1075&amp;idt=0&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347388%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1654x898%2C952x280%2C952x280%2C952x280%2C728x90%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=252847437314&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=7865&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=4273&amp;eid=95404663%2C95405492%2C95405931%2C95406273%2C95408372%2C95405499%2C122881957&amp;oid=2&amp;psts=AOrYGsmWApBlASyqWR2-_nvQuWnN4WTJqxKRVGl5ns2I00raSrS0HdrUURa0kYaYMXeorRjdJwC2b04z_xYn8t_cv1kelhhVn5wvcMtFXyKwMbOfPMpML9JSEeYG1pUEhC6hR8U&amp;pvsid=1906629352141593&amp;tmod=1339698072&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1946%2C30%2C1946%2C30%2C1920%2C30%2C1894%2C979%2C1654%2C898&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**② 能不能阻擋 terraform apply**

試用開發工具

我試著讓 Claude 執行 `terraform apply` 。

→ 因為 deny 設定而被擋下，沒有執行。

我測試的資料夾裡沒有 AWS 資源或連線設定，所以就算沒擋住也不會真的建出任何東西，確認完安全狀態才去測試。

#### 只用 deny 還是有漏洞

`Bash(terraform apply:*)` 會按照字面去比對 `*` 前面的部分（ `terraform apply` ）。所以像下面這些寫法，就不會被擋住。

```
terraform -chdir=env/prod apply   # apply 前面先加了選項
bash -c 'terraform apply'         # 包在另一層 shell 裡
/usr/local/bin/terraform apply     # 用絕對路徑呼叫
```

另一方面，像 `cd env/prod && terraform apply` 這種用 `&&` 或 `;` 串起來的命令，Claude Code 會拆成一條一條判斷，所以還是會擋住。

試用開發工具

官方文件也明確寫著，deny 規則是用來阻擋 Claude 平常會寫出的那種命令，並不是 **「安全邊界」** （ [Configure permissions › What a Bash rule doesn't match](https://code.claude.com/docs/en/permissions) ）。

[CLAUDE.md](http://claude.md/) 是「請求」，permissions 是「強制」。但這個強制本身還是有漏洞，所以要雙重阻擋。

因此我會再加上 `PreToolUse` Hook，檢查整段命令字串並阻擋。若 Hook 以返回碼 `2` 結束，會比 permissions 規則更早把操作擋下來。

.claude/settings.json（新增到 hooks）

```
"PreToolUse": [
  {
    "matcher": "Bash",
    "hooks": [
      {
        "type": "command",
        "command": "cmd=$(jq -r '.tool_input.command'); if echo \"$cmd\" | grep -Eq 'terraform.*(apply|destroy)'; then echo '禁止 terraform apply / destroy' >&2; exit 2; fi"
      }
    ]
  }
]
```

只要 `terraform` 和 `apply` / `destroy` 同時出現在同一行，不管中間夾了什麼都會被擋住。  
因為這個設定是偏寬鬆地擋，所以像 `terraform plan -destroy` 這種命令也會被擋。

試用開發工具

#### 重點

- 就算自己不會寫，也可以直接請 Claude 幫你寫這種 Hook
- 先用 permissions 的 deny，再加上 PreToolUse Hook 做保險，是比較安心的雙層保護
- Hooks 是能用自己的權限執行任何命令的；如果要套用別人寫的 Hooks，先看過內容再裝

---

### 3..claude 資料夾的設計

如果規則越加越多， [CLAUDE.md](http://claude.md/) 會塞得滿滿的。  
`.claude` 資料夾就是收納這些內容的地方，讓規則與流程依照職責分開放。

資料夾／檔案放什麼 `skills/` 步驟手冊（→ 第 9 章） `agents/` 負責人（子代理 → 第 7 章） `rules/` 細部規則 `settings.json` 權限與 Hooks 設定（團隊共享 → 第 2 章） `settings.local.json` 只有自己用的設定（不要提交到 Git）只要把 repository 裡的 `.claude` 資料夾提交到 Git，整個團隊就能共享規則和技能。

預約職涯諮詢

建立這些也只要請它做就好。

```
請在這個工作資料夾建立 .claude 資料夾。裡面幫我建 skills、agents、rules 這 3 個資料夾，分別放步驟手冊、負責人和細部規則
```

[![スクリーンショット 2026-10-26 19.02.15.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fe28e85a8-49aa-4f18-a031-29852b1dcae4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=de588e750283223320ba0994a2393ad1)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fe28e85a8-49aa-4f18-a031-29852b1dcae4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=de588e750283223320ba0994a2393ad1)

在 Mac 的 Finder 裡，`.claude` 是隱藏資料夾，要按 `Command` + `Shift` + `.`（句點）才會顯示。

---

### 4\. 權限模式的設定

輸入框下方會顯示目前的權限模式，點一下就能切換。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F56ed720c-6fdd-4313-a924-b18cc63bf775.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fe09ef7d896f804b3c26473d7e037e88)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F56ed720c-6fdd-4313-a924-b18cc63bf775.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fe09ef7d896f804b3c26473d7e037e88)

模式行為評價手動只看內容就好時可自動；編輯或執行命令都要每次確認安全但確認很多接受編輯檔案編輯自動，其餘命令確認適合寫程式時 **自動** 另一個 AI 會逐條檢查操作。只要在請求範圍內就會自動進行，危險操作會被擋下來 **平常使用的推薦** 計畫先調查並提出計畫，得到同意前不會改檔案適合大工作開始前使用完全繞過所有確認直接執行。刪除類操作也不檢查很快但危險，平常不建議

學習AI 課程

可以把它想成：把簡單工作整包交給新人，只在牽涉到錢或正式環境時才先擋下來。  
最近的版本裡，自動模式已經成為預設，畫面上也會顯示。

不過官方也有說，自動模式不能保證安全。 **`terraform apply` 之類的操作還是要靠第 2 章的 permissions / Hooks 一起擋住。**

#### 計畫模式的使用時機

第一次做大型工作、或是會影響範圍很廣的變更前，切換到這個模式。

```
我想用 Terraform 建立新的 VPC 和子網路。請先不要直接開始做，先提出一份架構方案。CIDR、AZ 數量這些需要判斷的地方，不要自行決定，請先問我。不要執行 apply
```

→ 它會像「CIDR 要怎麼設？」「要用幾個 AZ？」這樣附選項詢問，能減少做完之後才發現要重來的情況。

---

## 第 2 部：用同一份 Terraform 程式碼探索實作

接下來會用 **同一份 Terraform 程式碼** ，依序測試第 5～7 章。

試用開發工具

### 5\. 從架構圖圖片生成 Terraform

#### 做了什麼

我貼上一張 AWS 架構圖（VPC、ALB、EC2、Aurora 的一般 Web 三層架構），只說了一句話。

```
請根據這個架構寫 Terraform
```

#### 結果

- 產生了 **9 個檔案、573 行** 的程式碼
- Claude 還自己執行了 `terraform fmt` 和 `terraform validate` ，並確認都通過
- 沒有執行 `plan` / `apply` （第 1 章 [CLAUDE.md](http://claude.md/) 的安全規則有生效）

檔案內容 `network.tf` VPC、IGW、6 個子網路、NAT、3 種路由表 `security_groups.tf` bastion / alb / web / db 用的安全群組 `ec2.tf` / `alb.tf` / `rds.tf` bastion 與 2 台 web、ALB、Aurora MySQL（2 台） `versions.tf` / `variables.tf` / `outputs.tf` / `terraform.tfvars.example` 版本、變數、輸出、範例輸入值 [![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F83ca0174-3bd1-43ee-9d04-b34dd993f47b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8f1ea249c12458e933ebab44055e6551)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F83ca0174-3bd1-43ee-9d04-b34dd993f47b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8f1ea249c12458e933ebab44055e6551)

試用開發工具

#### 但「validate 通過」不等於「設計很好」

validate 只能確認語法正不正確，無法判斷安全性或設計優劣。所以接下來用第 6、7 章來檢查。

在貼截圖之前，也要先確認裡面沒有 AWS 帳號 ID、access key、客戶名稱等資訊。

**其他很適合用圖片丟給它的東西** ：AWS Console 的錯誤畫面（問原因與處理方式）、白板照片（請它整理成文件）、看不懂的申請畫面（問你要做什麼）。

---

### 6\. 用 /goal 修正到「0 件為止」

#### 是什麼

只要先給它完成條件，Claude 就會持續工作到符合條件為止，不需要你一直下新指示。可以避免長任務中途停住或偏題。

學習AI 課程

- 只輸入 `/goal` 可以確認目前目標
- `/goal clear` 可以中途取消

#### 做了什麼

針對第 5 章產生的程式碼，我這樣請它：

```
/goal 執行 trivy config .，持續修正到 HIGH 和 CRITICAL 的指摘為 0 件為止。但如果必須改變架構圖本身的設計才修得好（例如 NAT 只有一台），就不要改，請把理由整理到 README 並排除
```

#### 結果

- 因為 trivy 沒裝，Claude 先自己用 Homebrew 安裝後再執行
- **HIGH / CRITICAL 從 6 件 → 0 件**
	- 修掉的 4 件：例如 ALB 丟棄不合法標頭的設定、RDS 使用客戶管理金鑰（KMS）加密、安全群組的對外連線縮到只允許 S3 等
		- 排除的 2 件：ALB 對網際網路公開與使用 HTTP。這些是架構圖本身決定的，所以把理由寫進 README 後排除
- 修正後也確認 `terraform validate` 通過

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F51c8ab27-661e-4344-a57d-434e1e6e3e45.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0f4d96af58c911e776c1b387c9305508)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F51c8ab27-661e-4344-a57d-434e1e6e3e45.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0f4d96af58c911e776c1b387c9305508)

#### 小技巧

- 條件要寫成像「錯誤 0 件」這種 **能清楚判斷是否達成** 的形式
- **也要一起寫清楚不能做什麼** 。這次我寫了「必須改變設計才修得好的就排除」。如果沒寫，為了把數字變成 0，可能會把 ALB 改成不公開，做出跟架構圖不同的東西
- 像安裝工具這種你沒要求的事，它也可能自動做掉。若你不想要，就用第 2 章的 permissions 擋住

---

### 7\. 讓子代理並行審查

#### 是什麼

在 Claude Code 裡建立多個負責人，把工作分派出去的機制。

學習AI 課程

- 以前：我 ↔ Claude
- 導入子代理後：我 ↔ Claude（負責拆分與整理）↔ 子代理（實作負責人）
- 用基礎架構來比喻，就是「一台伺服器全包」→「負載平衡器 + 多個 worker 平行處理」

每個負責人都在自己專屬的工作區裡運作，只回傳結果，不會把主對話塞滿中間過程。

#### 做了什麼

對 trivy 已經清到 0 件的程式碼，這樣請它：

```
請讓子代理從安全性、成本、可用性 3 個角度平行審查這份 Terraform 程式碼，並把指摘按重要度整理成一份報告。只做審查，不要修改程式碼
```

#### 結果

- Critical 為 0 件， **High 為 3 件**
- **3 位都提到應該新增 S3 Gateway Endpoint** 。這是一個同時對安全性、可用性、成本都有幫助的對策
- 預估每月約 $300（假設 24 小時持續運作），其中約 6 成來自 Aurora 2 台
- 3 件 High 如下：
	1. 可用性：EC2 啟動腳本中的 dnf 沒有重試，失敗一次就可能讓 httpd 沒裝好而回傳失敗
		2. 可用性：如果更換 instance type 等設定，2 台 web 可能會同時重建，導致 ALB 後面一度沒有任何主機
		3. 成本：以長時間持續運作為前提。若在受訓時間外 `destroy` ，每月可省約 $200

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fd97e2872-1cd7-49c3-a601-6f95e0d94fb8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=14d2a764515679ae32757d548a37583f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fd97e2872-1cd7-49c3-a601-6f95e0d94fb8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=14d2a764515679ae32757d548a37583f)

**即使把 trivy 清成 0 件，還是會出現工具抓不到的設計指摘。**  
看來工具檢查與 AI 審查不是二選一，而是應該一起用。

學習AI 課程

#### 重點

- 適合：彼此不依賴的任務（不同觀點審查、不同環境差異調查、技術選型比較等）
- 不適合：有先後順序的任務，或多個人改同一個檔案的任務（容易衝突）
- 負責人越多，消耗的 token 越多，所以小工作不要硬拆，直接交給一個人做就好
- 目前啟用中的負責人，以及各自使用的模型，可以用 `/tasks` 查看

---

### 8\. /code-review ultra（著手前的檢查）

#### 是什麼

Anthropic 雲端上會平行跑多個審查代理來找 bug 的功能（通稱 ultrareview）。可以把它想成第 7 章的子代理，做成專門給審查用的大規模版本。  
它會先把找到的 bug 一件一件重現確認後再回報，所以比較少出現像格式風格這類噪音。一次大概要 15～25 分鐘，而且可以在背景執行，你仍然可以繼續做別的事。

選擇雲端硬碟

這是研究預覽（試用）功能，所以規格與費用都可能改變。正式指令是 `/code-review ultra` ，在支援的帳號上也可以用 `/ultrareview` 呼叫。

#### 使用方法

在 Git 管理的資料夾中執行。

```
/code-review ultra                 # 目前分支與預設分支的差異（包含未提交變更）
/code-review ultra develop         # 與 develop 分支的差異
/code-review ultra 1234            # GitHub 的 PR #1234
/code-review ultra 優先檢查認證相關變更   # 附上一句備註（目標是目前分支）
```

它只會檢查 **分支差異** 或 **GitHub PR** ，不能指定任意檔案或文件叫它審查（如果要那樣做，請用平常的 `/code-review` ）。執行前會顯示審查範圍、剩餘免費次數與預估費用的確認畫面。

#### 與平常的 /code-review 差別

/code-review/code-review ultra執行位置本機雲端對象作業中的差異、PR、分支、路徑作業中的差異、PR時間幾秒～幾分鐘15～25 分鐘費用一般額度內免費前 3 次免費，之後每次約 5～25 美元用途作業中的輕量檢查合併前的大變更最終確認#### 費用

選擇雲端硬碟

- Pro / Max 方案每個帳號前 3 次免費，不是每個月重置，而是每個帳號只有 3 次
- 第 4 次起，會從 **額外使用額度（usage credits）** 扣除，不是從方案額度扣。每次約 5～25 美元，取決於變更大小；如果沒有開啟 usage credits，根本無法執行
- 就算中途停止或失敗，免費次數還是會算 1 次（若是付費則依實際跑到的部分計費）
- 如果是審查分支，儲存庫內容會上傳到雲端；若指定 PR 編號，則不會從本機上傳，而是直接從 GitHub 取回
- Terraform 的話要特別注意 \* *\`*.tfvars `**。官方文件也寫到，像`.env `或` \*.tfvars\` 這種容易放憑證的檔名，在上傳時會特別處理。總之敏感資訊不要放在 repository 內
- 需要使用 [claude.ai](http://claude.ai/) 帳號登入。只有 API key、或透過 Amazon Bedrock 等方式使用的情況不能用
- 費率表只列 Pro / Max。公司用的 Team / Enterprise 方案是否能用、費用如何，請向管理員確認

---

## 第 3 部：效率化

### 9\. 建立技能

#### 是什麼

把做過一次的工作存成步驟手冊，變成可以隨時叫出來的業務 SOP。輸入 `/` 就能從清單中叫出。

#### 與 CLAUDE.md 的差異

讀取時機適合內容 [CLAUDE.md](http://claude.md/) 每次一定全部讀短規則技能平常只讀名稱和說明，需要時才讀內容長步驟因為技能只在需要時才讀內容，所以就算越做越多，平常對話也不會變重。「不是每次都要講，但很重要」的內容適合放進技能。

學習AI 課程

#### 建立方法

只要在作業結束後直接說就可以。

```
請把我剛剛做的工作整理成之後可以重現的技能
```

→ 它會建立包含使用情境、注意事項、完成條件等內容的步驟手冊檔案。

Anthropic 官方的 **skill-creator** （用來建立技能的技能）也可以幫你持續優化已建立的技能。Claude Code 應用程式裡預設就有，所以只要說「用 skill-creator 幫我做成技能」就能用。

以這次來說，「架構圖 → Terraform → trivy → 3 個角度審查」這整套流程就可以直接做成技能。  
像週報、會議紀錄潤稿、Terraform plan 結果整理這種每週都在做的事，只要有一項固定工作，就值得試著技能化。

---

### 10\. Superpowers（技能外掛）的導入

#### 是什麼

**外掛** 是可以把技能、代理等內容一次安裝進來的 Claude Code 擴充包。  
**Superpowers** 是 Jesse Vincent（obra）公開的外掛，裡面塞滿了工作流程技能。它的核心流程是「發想 → 規劃 → 實作 → 審查 → 確認完成」。

預約職涯諮詢

技能用途 `brainstorming` 製作前的腦力激盪與需求整理 `writing-plans` 擬定工作計畫 `systematic-debugging` 逐一拆解原因來排查 `verification-before-completion` 在說「完成了」之前先確認真的做完#### 導入方式

從左側選單進入自訂 → 外掛 → 搜尋欄輸入「Superpowers」→ 加入來源是 **obra/superpowers** 的那個。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F53bb7382-91a8-4e24-9906-9b496be1977f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=278d05c27c4acf728f1d6f0d026db60c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F53bb7382-91a8-4e24-9906-9b496be1977f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=278d05c27c4acf728f1d6f0d026db60c)

導入後，輸入 `/super` 就會出現技能清單。最常用的是 `brainstorming` 。

```
/brainstorming 我想做一個能改善公司內部困擾的應用程式。請告訴我有哪些點子
```

→ Claude 會變成腦力激盪夥伴，幫你把點子往下挖深。

學習AI 課程

它有一個機制會在每次開啟對話時自動載入，所以會多消耗一點上下文。

---

## 第 4 部：管理

### 11\. 清理對話（/context /compact /clear）

對話一長，上下文（AI 一次能記住的量）就會被塞滿，輸出的準確度也會下降。

指令用途建議時機 `/context` 顯示目前對話塞了多少定期確認 `/compact` 只保留重要部分並壓縮對話變長時 `/clear` 重置對話重新開始話題改變時，或 compact 也不夠時在我的環境裡， `/context` 顯示 121.6k / 1M（12%）的對話，經過 `/compact` 後，大約壓縮掉 109k tokens。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fa0c57372-e51b-4ace-89d9-fc29e9928697.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8eabf6008d2f114246938a44d6f08d01)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2Fa0c57372-e51b-4ace-89d9-fc29e9928697.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8eabf6008d2f114246938a44d6f08d01)  塞滿時也會自動 compact，但因為只是摘要，所以細節可能會消失。  
自己選擇時機並指定要保留的內容，保留得會更漂亮。

```
/compact 只保留已確定的內容和目前工作，刪掉已被否決的部分
```

**製作交接文件並 clear 的流程**

1. 請它「上下文快滿了，要切到另一個 session，請幫我做交接文件」
2. 複製產出的交接文件
3. 執行 `/clear`
4. 貼上交接文件，開始新對話

---

### 12\. /doctor（健康診斷）

#### 是什麼

讓 Claude Code 自己檢查環境的指令（別名 `/checkup` ）。它會先回報結果，若要更動什麼也會先詢問，所以可以放心使用。

學習AI 課程

主要會檢查：

- **安裝狀態** ：重複安裝、 `PATH` 問題、無法讀取的設定檔、有沒有新版本
- **沒在用的技能／MCP 伺服器／外掛** ：也會顯示各自消耗多少上下文，並建議停用
- **速度慢的 Hooks** ：如果第 2 章有加 Hooks，這裡可以檢查是不是拖慢了
- **[CLAUDE.md](http://claude.md/) 減肥** ：會建議把每次都要讀的 [CLAUDE.md](http://claude.md/) 中像目錄結構說明這種多餘內容刪掉，偶爾才需要的內容移到技能等地方
- **權限相關** ：會建議把自動模式設成預設，或是先允許常被拒絕的唯讀命令  
	在更新後怪怪的，或是設定好像沒生效時也很適合用。  
	在終端機輸入 `claude doctor` ，就能不進入對話、直接看安裝診斷。

#### 沒在用的外掛其實每次都在花成本

外掛和技能就算沒用到，名稱與說明也會每次都被讀進對話，所以還是會計入用量（第 10 章的 Superpowers 也是一樣）。  
第 11 章實測顯示，就算是新對話，光技能就用了 5.8k。  
如果用了 3 個月，很可能會累積一些當初試裝、但只用過一次的外掛或技能，所以大概每月用 `/doctor` 檢查一次比較好。

預約職涯諮詢

另外也可以用以下方式確認。

方法可知道什麼 `/plugin` 的 Installed 分頁14 天內、10 個 session 都沒用過的外掛，會顯示為「Not used recently」 `/skill-doctor` 各技能的成本與使用頻率。沒被呼叫過的技能會被標記 `/usage` 最近用量中，技能、子代理、外掛、MCP 伺服器各佔多少 `claude plugin details <名稱>` （在終端機執行）該外掛每次加入對話的 token 數量估算

---

### 13\. 小技巧

不用全記，只要知道「原來有這種功能」就夠了。

小技巧用途補充 `Esc` 兩次 / `/rewind` 把對話往前回退一小段輸入框是空的時候連按 `Esc` 兩次，會出現選擇回退點的選單（輸入中文字時，按 `Esc` 只會先清掉文字）。能回退的只有 Claude 編輯過的檔案與對話。像 `rm` 指令或 `terraform apply` 、AWS CLI 改掉的檔案， **回不來** `/btw` 插入與主題無關的問題問題與答案不會留在對話歷史裡，所以不會污染主對話的上下文。即使 Claude 正在工作中也能用。它不能使用工具，只會根據對話中的資訊回答（不會讀檔或查詢） `/fast` 維持聰明度但加速（最高 2.5 倍）下面詳述 `ultrathink` 讓它只對那個指示深度思考寫在指令最後即可，不用改設定#### `/fast` 是「快但貴」

預約職涯諮詢

在輸入框打 `/fast` 就能切換 ON / OFF（桌面 App 也可用）。

`/fast` 不是切換到別的模型，而是讓同樣的 Opus 以速度優先的設定運作。品質不變，但回應速度最高可快 2.5 倍；代價是費用上升。

輸入（每 100 萬 tokens）輸出（每 100 萬 tokens）Opus 5.5（一般）$4$20Opus 5.5（fast 模式）$8$40Opus 5.5 的話，單價是一般模式的 **2 倍** 。

- 在 Pro / Max / Team / Enterprise 方案裡，不是吃方案額度，而是 **直接從額外使用額度（usage credits）扣除** 。就算方案額度還有，也是一樣。沒開啟 usage credits 就不能用
- **Team / Enterprise 一開始是關閉的** 。除非組織 Owner 開啟，否則不能用，所以公司要用的話請先確認管理員
- 如果在對話中途才打開，等於要把整段對話都重新用 fast 模式計費一次。要用的話， **最好在對話一開始就打開**
- 只能用在 Opus，Sonnet 或 Haiku 不能用
- 這是研究預覽（試用）功能，所以規格與費用都可能變動

**適合用在** ：需要快速看結果、反覆修正的工作，或像故障排除這種很吃時間的情境。  
長時間的自動作業，或很在意成本的工作，通常還是一般模式比較好。

---

## 第 5 部：進階業務自動化

### 14\. 定期執行

#### 是什麼

讓 Claude 按固定排程接手工作的功能。到時間時會自動開啟新 session，開始處理並把結果留下來。

學習AI 課程

種類執行位置條件本機自己的 PC PC 必須開著，而且 Claude App 也要開著。可使用本機檔案雲端（Routines）Anthropic 雲端即使 PC 關機也能執行。不能使用本機檔案#### 設定方法

直接請它做就好。建立好的任務會出現在側邊欄的「Scheduled」。

```
請每天早上 9 點把前一天的 AWS 成本按服務整理，並列出比前一天增加的 3 項。請幫我設成定期執行
```

#### 應用例

- 每天早上報告 AWS 成本與前一天的差異
- 每週跑一次 terraform plan，檢查是不是有人手動改了設定
- 每天早上把 AWS 新功能／故障資訊整理成 3 行
- 如果是本機執行，電腦睡眠期間會跳過，等下次啟動時再一次補跑；所以有時候會發生「早上 9 點的任務晚上才跑」。如果時間很重要，可以在 prompt 裡加上「超過 17 點就不要執行」之類的條件
- 如果會用到 AWS 憑證，請先縮到只剩唯讀權限
	選擇雲端硬碟

---

### 15\. Claude in Chrome

#### 是什麼

讓 Claude 看著 Chrome 畫面，自行點擊與輸入來完成工作的功能。  
概念上就是「旁邊那位 Claude 幫你直接操作瀏覽器」。

#### 如何開始

1. 到 Chrome Web Store 搜尋「Claude」
2. 選擇名稱是「Claude」、提供者是 **[claude.com](http://claude.com/) （有驗證標記）** 的擴充功能，按「加到 Chrome」

搜尋結果裡會有很多名稱含有「Claude」的其他第三方擴充功能，所以要看提供者辨識。若要裝在公司電腦上，請先確認內部規範。

#### 使用範例

```
在 Chrome 打開 AWS 文件，查出 ALB idle timeout 的預設值與修改方法並整理給我
```

→ 它會自動開頁面，並把查到的內容整理出來。適合拿來查文件、內部 Wiki、或確認管理介面的設定值（只看不動）。

- 它是直接操作你登入中的瀏覽器，所以 **不要把已登入正式環境的 AWS Console 直接交給它**
- 網頁裡可能藏有誘導 AI 的指令。對陌生網站操作時，最好有人盯著

---

### 16\. Remote Control（手機遠端操作）

可以用手機操作 Claude Code 的功能。通勤路上或休息時間也能做事，讓你有機會「回到家時已經完成一件事」。

學習AI 課程

1. 在 Claude Code 執行 `/remote-control` （簡寫 `/rc` 也可以）
2. 會顯示 QR Code，用手機相機掃描
3. 手機上的 Claude App 會打開，顯示 PC 上正在運作的工作畫面
4. 第二次以後，直接從 Claude App 的「Code」分頁選 session 一覽即可
- 手機操作時，PC 端的 Claude Code 不要關掉。關掉或進入睡眠就會中斷連線
- 工作是在 PC 上執行，所以 PC 裡的 AWS 憑證等也會照樣被使用

---

### 17\. Session 間訊息

#### 是什麼

當你同時開多個對話（session）來工作時，它們彼此不知道對方在做什麼。人類就會變成在不同 session 之間來回複製貼上的「搬運工」。  
這個功能可以讓 Claude 直接把訊息送到另一個 session。

學習AI 課程

傳送的只是 **Claude 寫的一小段文字訊息** ，不會把整段對話歷史或檔案整包移過去。  
如果你要整個對話一起接手，請用第 11 章「製作交接文件並 clear」的流程。

#### 使用方法

直接請它傳就好。收件人會由 Claude 自己找（底層是用 `ListAgents` 找對象，再用 `SendMessage` 傳送）。

```
把我剛剛查到的 ALB timeout 結論傳給正在寫 Terraform 的那個 session
```

如果想自己指定收件人，可以在 `@` 後面輸入 session 名稱前幾個字，它會跳出候選項讓你選。

```
@terraform-worker 請告訴它，我們已經決定要加上 S3 Gateway Endpoint
```
- 收件那邊的對話會顯示一行 `Message from @寄件者名稱: …` ，Claude 讀到後會把內容納入工作
- Session 名稱可用 `/rename 名稱` 命名。取個好懂的名字會比較方便指定
- 想確認有哪些 session 可送，可以執行 `/list-agents` （或 `/peers` ）
- 如果你說「等那個 session 的工作完成後告訴我」，當對方有空時會通知你一次

#### 安全性

- 其他 session 傳來的訊息不會被當成「你的指令」。收到訊息不會自動通過權限確認，也不會因此改寫設定或 [CLAUDE.md](http://claude.md/)
- 如果是同一台電腦裡的 session 互傳，訊息不會經過 Anthropic 伺服器  
	**應用例** ：把「查資料的對話」和「寫 Terraform 的對話」分開，資料交接交給 AI 自己做。

這是 v2.1.224 之後的功能（Windows 為 v2.1.234 之後）。如果 `/list-agents` 不能用，就把 Claude Code 更新一下。

學習AI 課程

---

### 18\. Dynamic Workflow（AI 組織運作）

#### 是什麼

把複雜工作交給它後，Claude Code 會當場為該工作製作流程表，動用許多分身分工與互相檢查，最後還會整理成報告。  
可以把它想成第 7 章的子代理，依照流程表用大量人力自動運作。

#### 事前準備

到 App 設定 →「Session」→ 確認「Dynamic Workflow」已開啟。官方說明中，Pro 方案預設是關閉的。公司方案（如 Team）有時一開始就是開啟的（我的環境也是預設開啟）。

#### 使用方法

**① 在指令中明確要求**

```
ultracode 請全面檢查這個 repository 的 Terraform 程式碼，從安全性、成本、可用性三個面向都要涵蓋，並且每一條指摘都先確認過再整理成報告
```
- 只要包含「ultracode」這個字，或是請它「使用 workflow」就可以
- 它會先提出流程表，確認 OK 後開始工作。進度會顯示在畫面側邊的背景任務中

**② 使用內建 workflow**

用 `/deep-research （想查的內容）` ，它會從多個角度蒐集網路資料、交叉比對內容，最後附上引用來源的報告。

#### ultracode 與用量上限不是同一件事

很多人會搞混，但它們改變的是不同東西。

改變的東西用量上限（低・中・高・超高・最大）一次回覆裡 **思考多深入** ultracode **要不要啟用大量分身的 workflow** - App 的用量上限選單裡沒有 ultracode；在 App 裡要像上面那樣用指令要求

- 終端機版可以用 `/effort ultracode` 讓整個對話持續開啟（用 `/effort ultracode off` 關閉）
- 以前的版本曾經是「開啟 ultracode 也會提高 effort」，所以有時會被介紹成 effort 的一種
- effort 的「最大」在 App 裡也會顯示消耗量很大，例如「6 倍以上的使用量」。平常用「中（推薦）」就很夠

因為會大量動用分身，token 消耗非常大。  
Pro 方案很容易就碰到上限。建議先從一個小範圍，例如單一資料夾開始試，確認用量後再擴大。至於同時動用多少人，大致可以在設定中的「Dynamic workflow size」調整。

---

## 第 6 部：外部工具整合

以下不是 Claude Code 本身的功能，而是和其他公司的工具搭配使用。

學習AI 課程

### 19\. NotebookLM 的分工

如果讓 Claude Code 讀太長的資料，token 會大量消耗，很快就碰到使用上限。  
所以要分工。

負責項目NotebookLM（Google）讀入大量資訊並整理Claude Code讀整理後的內容、思考與判斷 **使用範例**

1. 在 NotebookLM 加入 AWS 官方文件與解說影片的 URL
2. 問 NotebookLM：「請把對基礎架構維運可能有影響的變更，用引用來源整理出來」
3. 把回答貼到 Claude Code，並請它：「根據這些內容，幫我找出目前 Terraform 程式碼中應該重新檢查的地方」

NotebookLM 端也可以把研究結果做成簡報、圖解、音訊。

- 雖然有非官方工具可以直接從 Claude Code 操作 NotebookLM，但它們用的是 Google 沒有公開的機制，可能不符合使用條款。公司內部不建議使用
- 放進 NotebookLM 的資料會送到 Google，所以不要放公司文件或客戶資訊

---

### 20\. Codex 的連動（資料製作）

#### 是什麼・為什麼要連動

**Codex** 是 OpenAI 做的 AI 代理，和 Claude Code 類似。做簡報時，它在「效率」和「設計感」之間的平衡很好，產出的簡報比較不會有明顯 AI 味。

學習AI 課程

#### 連接步驟

1. 在 Claude Code 輸入以下提示
```
我想使用 Codex CLI，請幫我準備。

1. 檢查 Node 和 npm 是否已安裝，若沒有就安裝。
2. 檢查 codex 是否已安裝，若沒有就用 npm install -g @openai/codex 安裝。
3. 用 codex login status 確認是否已登入。
   若尚未登入，請執行 codex login。
   如果瀏覽器開啟，之後我會用 ChatGPT 帳號登入。

※ 不使用 API key。請用 ChatGPT 帳號登入。
```
1. 中途會打開瀏覽器登入畫面，請登入 ChatGPT
2. 再告訴 Claude Code「我已登入，請幫我確認」，如果顯示 `Logged in using ChatGPT` 就完成了

之所以不用 API key、而是用 ChatGPT 帳號登入，是因為這樣可以在 ChatGPT 方案範圍內使用，也不用擔心用量另計或 key 外洩。

體驗企業簡訊

#### 使用方法

只要請 Claude Code 幫你做就行。

```
請讓 Codex 依照參考資料夾圖片的風格，製作這份內容的投影片，完成後幫我確認結果
```

背後其實是 Claude Code 透過 `codex exec` 這個命令把工作交給 Codex。

```
codex exec -C ./slides --skip-git-repo-check -s workspace-write "（要它做的事）"
```
- `-C ./slides` ：工作資料夾，只在這裡面作業
- `-s workspace-write` ：只允許在這個資料夾內寫入（只讀時可用 `read-only` ）
- `--skip-git-repo-check` ：即使不是 Git 管理的資料夾也能執行

#### 實際製作的簡報

我請它「做一張 Claude Code 介紹簡報」，最後輸出成 PowerPoint 格式（.pptx）。如果你想指定格式，可以直接說「PPTX」或「PNG」。

學習AI 課程

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F5ee4d45f-36e4-4d65-b2a3-14124bc77c86.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b87d0a399db331d3e2a3e70cf79b3191)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3192867%2F5ee4d45f-36e4-4d65-b2a3-14124bc77c86.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b87d0a399db331d3e2a3e70cf79b3191)

不過一開始 Codex 說「做好了」的檔案是壞掉的，根本打不開。後來我讓 Codex 修正，再讓 Claude Code 確認畫面才完成。右上角換行等細節還有些小瑕疵，最後我自己手動修掉。

- **不要只相信「做好了」這句話，最後一定要自己打開確認**
- 丟給 Codex 的內容會送到 OpenAI；如果要用在公司資料上，請先確認公司規範

---

## 資安注意事項

要注意的事怎麼做秘密資訊不要把存取金鑰或密碼放進程式碼或 commit。貼截圖前也要先檢查送到外部服務的資料把公司資料丟給 NotebookLM（Google）、Codex（OpenAI）、ultrareview（Anthropic 雲端）前先確認規範不可回復的操作 `terraform apply` / `destroy` 要用 permissions 與 Hooks 擋掉，或至少一定要先確認。 `/rewind` 無法救回瀏覽器操作不要把已登入正式環境的狀態交給 Claude in Chrome定期執行使用的 AWS 憑證請設成唯讀人寫的 Hooks／外掛先看內容再裝

## 總結

- 需要強制遵守的規則，不要只放在 [CLAUDE.md](http://claude.md/) （請求），而要放在 **permissions / Hooks（強制）**
- 從架構圖 → Terraform → trivy → AI 審查一路做下來，你會發現： **就算 validate 通過、trivy 也變成 0 件，還是會冒出要修的地方** 。工具與 AI 審查應該一起用
- 每個功能都是「用得越多，累積越多」。 [CLAUDE.md](http://claude.md/) 整理得越好，技能做得越多，子代理角色分工越清楚，工作速度就會越快

### 推薦的導入順序

1. 用訪談方式建立 [CLAUDE.md](http://claude.md/) （第 1 章）
2. 用 permissions 和 Hooks 擋住 `terraform apply` / `destroy` 等危險操作（第 2 章）
3. 把平常使用切到自動模式（第 4 章）
4. 把重複作業做成技能（第 9 章）
5. 養成用 `/context` → `/compact` → `/clear` 清理對話的習慣（第 11 章）
6. 熟悉後，再往定期執行、子代理、Dynamic Workflow 擴充

不需要一次全部上。先從一兩個功能開始試就好。  
如果你也發現了「這個也很好用」的功能，歡迎留言告訴我。

## 參考

- Claude Code 官方文件： [https://code.claude.com/docs](https://code.claude.com/docs)
- 設定（permissions）： [https://code.claude.com/docs/en/settings](https://code.claude.com/docs/en/settings)
- Hooks： [https://code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks)
- ultrareview： [https://code.claude.com/docs/en/ultrareview](https://code.claude.com/docs/en/ultrareview)
- Dynamic Workflow： [https://code.claude.com/docs/en/workflows](https://code.claude.com/docs/en/workflows)
- Superpowers： [https://github.com/obra/superpowers](https://github.com/obra/superpowers)
- Codex CLI： [https://github.com/openai/codex](https://github.com/openai/codex)
- permissions 規則寫法： [https://code.claude.com/docs/en/permissions](https://code.claude.com/docs/en/permissions)
- 子代理： [https://code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents)
- 指令列表： [https://code.claude.com/docs/en/commands](https://code.claude.com/docs/en/commands)
- 互動模式操作： [https://code.claude.com/docs/en/interactive-mode](https://code.claude.com/docs/en/interactive-mode)
- fast 模式： [https://code.claude.com/docs/en/fast-mode](https://code.claude.com/docs/en/fast-mode)
- 跨 session 訊息： [https://code.claude.com/docs/en/cross-session-messaging](https://code.claude.com/docs/en/cross-session-messaging)
- 價格表： [https://platform.claude.com/docs/en/about-claude/pricing](https://platform.claude.com/docs/en/about-claude/pricing)

---

原文出處： [https://qiita.com/N-Kyo/items/bf4a3a2185db2d28eee5](https://qiita.com/N-Kyo/items/bf4a3a2185db2d28eee5)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3jAopx) [123瀏覽](https://codelove.tw/@tony/post/3jAopx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3jAopx)

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

🔥 [REA：讓你的 AI 代理反向工程你希望擁有的應用程式功能](https://codelove.tw/@tony/post/qenz7x)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [PHP脆弱性php2shell應對：在不停止7.4正式環境下守住系統的那個夜晚與實際攻擊](https://codelove.tw/@tony/post/qOZgJa)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7 💬1

273

🥈

[boluoji](https://codelove.tw/@boluoji123)

📝5 ❤️3

97

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [川川｜全客製電商：開店平台限制多？我們幫你寫一套自己的。➡️ 了解更多](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1793347387&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3jAopx&amp;pra=5&amp;aiof=14&amp;asro=0&amp;itsi=4&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347387921&amp;bpp=2&amp;bdt=229&amp;idt=55&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347075%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=252847437314&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=0&amp;eid=95404663%2C95405492%2C95405931%2C95406273%2C95408372%2C95405499%2C122881957&amp;oid=2&amp;pvsid=1906629352141593&amp;tmod=1339698072&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1654%2C898&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=73" title="Advertisement" aria-label="Advertisement"></iframe>