# Neural Architecture Search: A Survey

## Personal Info
ID: R08922A20  
NAME: 洪筱慈 

## Basic information

- Paper link: https://arxiv.org/pdf/1808.05377.pdf 
- Conference: JMLR'19


## Background
在目前的 CV 領域中，各種 task 都能夠藉由複雜的 model 達到很好的 performance了，然而當模型越複雜，能夠探索的可能性就越多，search space 越大。因此， Neural Architecture Search 目的在於開能夠以有效的方式，自動且有效率的找到好的 model。  

類似這個問題的其他方法還有 meta-learning，或者利用另外一個 model 來學其他 model 的更新，以及使用 RL 的方式。  

## Methods  
要能夠快速且自動的找到好的 model，可以從以下三種角度切入：
1. 縮小範圍：縮小 search space
2. 訂定好的搜尋策略
3. 制定好的性能評估策略

### 縮小 search space 
其實可以簡單理解成在不降低 performance 的情況下把 model 縮小，例如 skip connection, 堆疊 cell 跟 block 的做法。如此一來， model 可能性降低，就可以更快速找到好的 model。

### 訂定好的搜尋策略
目前已存在一些搜尋策略，例如： random search, Bayesian optimization, 或者原本就已熟知的 gradient-based methods 跟 Reinforcement Learning。假如我們把 NAS 想成是一個 RL 問題，state 就是目前的 model architecture，而 reward 就是 model 的 performance， action 則是 application of function-preserving mutations。

### 制定好的性能評估策略
最土法煉鋼的方式就是把 model 訓練在某個 dataset 上，然後看他的 performance。但是複雜的 model 往往需要耗費多個 GPU day。因此，可以改成先 train 較短的時間、在較小的 dataset 上，預先評估他的 performance。只是這樣往往會錯估 model 的 performance，因此需要加入 bias。  
另外一個方法是 one-shot NAS。先把一個架構的所有可能性都組合出來，稱為 superNet。先訓練單一個 model 的 weight，然後接下來其他類似的 model 就可以部分繼承這個 model weight，不需要再全部重新訓練，就可以判斷性能。


## Reference
* [提煉再提煉濃縮再濃縮：Neural Architecture Search 介紹](https://medium.com/ai-academy-taiwan/%E6%8F%90%E7%85%89%E5%86%8D%E6%8F%90%E7%85%89%E6%BF%83%E7%B8%AE%E5%86%8D%E6%BF%83%E7%B8%AE-neural-architecture-search-%E4%BB%8B%E7%B4%B9-ef366ffdc818)