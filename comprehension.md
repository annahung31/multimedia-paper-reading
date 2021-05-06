# To Test Machine Comprehension, Start by Defining Comprehension 

## Personal Info
ID: R08922A20  
NAME: 洪筱慈 

## Paper basic information

- Paper link: https://arxiv.org/abs/2005.01525
- Conference: ACL 2020
- First author: Jesse Duniietz, Gregory Burnham (Elemental Cognition)
- work field: `Machine comprehension`

## Key takeaway
作者指出，已存在的 machine comprehension 模型，其實並沒有真正解決了這個問題，原因是因為 dataset 存在一些問題，例如所問的問題太過簡單，以及對於模型該理解文章的什麼沒有清楚定義。因此，作者提出一個明確定義“理解”的架構，並提出一個用這個架構建立的 dataset。作者並使用當時存在的各種模型測在這個 dataset，證明 machine comprehension 還有很大的進步空間，以此告訴大家，有明確定義架構的 dataset 對於 comprehension task 是重要的。

## 已存 dataset 的缺陷
已存在的 dataset 有幾種收 data 的方式，各有缺點：
1. 收 data 時，沒有給做 data 的人明確的 guideline，導致標記者總是出很簡單的題目，而缺少真的需要理解文章內容才能回答的題目。
2. 收集那些在論壇上被提出的問題：有時候問題和文章內容是不匹配的。
3. 自動產生的提問：會有 de-emphasized content 的問題。

最好的方式，其實是針對目標的文章，先列出看這篇文章應該理解到什麼內容，再設計問題來測試是否理解到，就像國文老師出考卷一樣。但是已存的dataset 沒有人這樣做。

## 作者提出的 ToU(Templete of Understanding)
1. 針對該篇文章列出應該被理解的（顯而易見的內容，以及延伸的背後的內容）:
    * Spatial: 空間上的內容。故事發生在哪？ 人物從哪裡移動到哪裡？
    * Temporal: 時間上的內容。
    * Causal: 人事物如何影響故事的發展。
    * Motivational: 人物的想法、信念、情緒。

2. 設計問題與答案：提出兩種 approach
    1. Annotating ToU answers
    2. Competing to satisfy judges


## 用現存的 MRC 系統來測試
XLNet 表現得很差，他並沒有學到 ToU 最關鍵的那些部份。在原本的 RACE task 上可以有 81.75%的正確率，測在本 dataset 上只剩 37%，而人類的表現是 96%。當測試在不同的 group 問題時，人類的表現只掉了 3%，而 XLNet 則掉了 17%，顯示出 XLNet 並不是真的理解了文章。

## 結語
作者建議該領域的研究者，應該使用 ToU 來建立 dataset，有更好的定義，才能把這個 task 做得更好。


