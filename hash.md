# Learning to Hash for Indexing Big Data – A Survey  

## Paper basic information
- Paper link: https://arxiv.org/abs/1509.05472  
- Conference: Proceedings of the IEEE, 2016    
- First author: Jun Wang (the Institute of Data Science and Technology at
Alibaba Group)  
- work field: `Big Data`  



## Key takeway  
傳統的 hash 方式，例如 locality-sensitive Hashing，雖然比 random hashing 更有效率一些，但是對於真實 task 仍然不夠好用。在這篇 survey 中作者整理了一些 data-driven 的 hash method，經由學習得到的 hashing，可以更有效保留 feature 之間的遠近關係。


## Problem

當檢索相關技術進到企業之後，面對的是大量的 data，大量的類別，以及 request 之後 feedback 的時間差。因此，需要有一個好的 hash 方式，能夠更有效率的找到想要的 data。

一般來說，我們可以把搜尋的過程簡單分成四個步驟：

1. 設計一個 hashing function，可以想像成 mapping 的機制，如何將 quary mapping 到 target。
2. 生成 hash code 供對照用。
3. 針對 database 裡面的所有 data，根據上述的 hash code 建表。
4. 當有一個 quary 進來時，先透過 hash function 得到 hash code，再根據相似程度來查找。


## hashing 的分類

在這篇 survey 中，介紹了 learning-based hashing 的方法以及分類。  

### 根據是否需要對 data 分析分類

1. Data-Independent: 最常見的是 random projection，例如 LSH, SIKH。他們不太有效率，因為不是對 dataset 量身訂造的。
2. Data-Dependent: 利用 dataset 本身，以及 dataset 的 label 來做 hashing。


### 根據對 label 的需求分類

1. unsupervised
2. supervised  
    * Pointwise  
    * Pairwise  
    * Triplet-wise  
    * Listwise  

<div align="left">
<img src=img/5-1-1.png width=500x>
</div>

3. semi-supervised

### 根據方法特性分類

另外，也可以分成 linear 和 nonlinear learning，其中 nonlinear 的方法表現是比 linear 好的，可以想像 linear hashing 可能不足以代表複雜的 mapping 關係，沒辦法作出準確的判斷。  

### 依照 optimize 的方法分類 

1. Single-shot learning: 每一個 hash function 自己獨立 optimize objective function 來得到最佳解。
2. Multi-shot learning: 會考慮 global objective function，以及前面以產生的 hash functions 不斷的做 optimization。

### 根據對維度的處理分類  

1. Nonweighted hashing: 不考慮各種為度之間的差異
2. Weighted hashing: 有考慮不同維度的貢獻差異，做了 weighted sum，且 weight 是 learnable 的。

## Hashing 方法介紹

在這篇 paper 中，共 review 了 9 種 hashing 方法：

<div align="left">
<img src=img/5-1-2.png width=1000x>
</div>

在 unsupervised 的方法中， Spectral Hashing 目標是希望使 input space 的距離關係跟 hamming space 中的距離關係相同，並且希望 hash codes 是 balanced 且 uncorrelated 的。   

但是要達成這件事，需要計算 pairwise similarity matrix A ，這是一件 cost 很大的事，因此 Anchor Graph Hashing 提出使用一個 subset M 當作 anchor points，來近似 A 所代表的 graph 結構。  

至於 supervised learning 的方法，則包括這學期常提到的 Metric Learning Hashing，以及目標是最小化 empirical loss 同時保持最大 entropy 的 Semi-Supervised Hashing。

大部分的方法，都是考慮 2~3 個 sample point 彼此之間的關係，而 Ranking Supervised Hashing 則是企圖保存 quary set 對應到的一組 data points 彼此間的順序：

<div align="left">
<img src=img/5-1-3.png width=500x>
</div>


## Open issues of Deep learning-based hashing method

雖然說 deep learning hashing 已經有很多的 approach 了，但是目前(2016)仍然沒有理論支持這些 hashing 真的會 return 正確的 neighbor。另外，我們真的可以不使用真實數據，而是直接用 compact code 來做 learning，而且不影響 accuracy 嗎？

未來的研究方向可能是：
1. deep learning-based 的方法跟 binary code learning 結合
2. 透過設計有效的 hashing，妥善利用 heterogeneous features 以及 multi-modal data 的特性，增加 overall 的 indexing quality。