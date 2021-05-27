
# Importance Estimation for Neural Network Pruning  


## Personal Info
ID: R08922A20  
NAME: 洪筱慈 

## Basic information

- Paper link: https://arxiv.org/pdf/1906.10771.pdf
- Conference: CVPR'19
- Field: `Network pruning`

## Key contribution

用一階和二階泰勒展開來近似 neuron/filter 對 loss 的 contribution，藉此評估 neuron/filter 的重要性，並用來刪減不重要的 neuron/filter。這樣的方法和其他評估的方式有 93% 以上的 correlation，且實驗證明這個方法和暴力 greedy search 的表現差不多，但更加有效率，並且一階展開也比二階更有效率，且 accuracy 也只掉一點點而已。實驗結果顯示，這個方法在 ResNet-101 上，移除了 30% 的模型參數， accuracy 只掉了 0.02%。


## Background

想要減少模型參數，最直覺的做法是在算 loss 的時候，多加一項考慮 minimize 模型參數量。但是這個方法很不實際，因為要 minimize 這一項是 non-convex, NP-hard 的問題。 另一個作法，則是做 greedy search。先 train 一個完整版的 model，再逐次減去一些不重要的參數。

## Method

### 如何評估 neuron 的重要性（importance）？
我們可以看看，把這個 neuron 移掉之後，會增加多少 loss？ 更具體來說，就是計算有這個參數跟沒有，prediction error 的平方差是多少:
    <div align="left">
    <img src=img/14-1-1.png width=300x>
    </div>

但是這樣會耗費大量的運算資源，假如共有 W 個參數，那就需要 train W 個 model，來比較這 W 個參數到底有沒有用。因此，作者提出，將 (3)式做泰勒展開再做近似，得到以下的結果：
    <div align="left">
    <img src=img/14-1-2.png width=300x>
    </div>

其中 g 是 gradient，在 training 的過程中本來就會算了，我們只要把他存起來就好。如此一來就會省下很多時間。  

另外，對於某些模型結構，例如 filter，會需要同時考慮多個 neuron，對此作者定義了 `group contribution` 來衡量一個 filter 的 joint importance。除此之外也設計了加入 `gating layer / gate`，更方便運算 filter 的重要性。  

這個計算方式，透過一些推導，相當於 information theory 中的 variance estimate，以及 Fisher information matrix 的 diagonal。  

### 如何做 pruning?

1. 先 train 一個完整的 model
2. 用小的 learning rate，逐次 fine-tune model。在 fine-tune 的每個 epoch 中：
    a. 在每個 minibatch 中，計算參數的 gradient，並以此 update model。同時計算參數的 importance (根據上一部份提到的方法)。
    b. 在經過幾個 minibatch 之後，計算每個 neuron 的 average importance，並移除 N 個或 n% 個重要性較低的 neuron。（ N 是事先定義的） 
3. 經過數個回合的 fine-tuning 和 pruning，直到 loss 增加到預先設定能忍受的值為止。


### 實驗

在這篇 paper 中，作者先將這個方法實驗在較小的 model (LeNet3) 上，比較 greedy search 和 proposed method ，結果顯示在能忍受的 loss 增加範圍下，proposed 的方法能夠砍掉的 neuron 和 greedy search 差不多。
    <div align="left">
    <img src=img/14-1-3.png width=500x>
    </div>

如圖，綠色的（oracle)是 greedy search，紫色是二階泰勒展開(Taylor SO on gate)，紅色是一階泰勒展開(Taylor FO on gate)。在 loss 增加到 1.0 的時候（圖中紅色虛線）， oracle 丟掉了 145 個 neuron，二階泰勒展開丟了 144 個，一階丟了 140 個。可以看見，泰勒展開能夠丟的數量跟 greedy search 是差不多的，並且一階和二階其實沒有差很多，因此使用一階是更有效率的。  

實作在更大的 model 上（ResNet-101)也可以看到類似的結果：  
    <div align="left">
    <img src=img/14-1-4.png width=500x>
    </div>

並且，透過 Spearman correlation 的分析，可以得到用 Talyer SO/FO 得到的 neuron 重要性排列，和用 oracle 的排列有很高的 correlation。代表用 proposed method 得到的重要性排列用 greedy search 找出來的是很像的。  

### Further study

除了跟 oracle 比較，作者也和其他更多的 pruning method 做了 correlation study，並且比較不同的 pruning 方法（一次 pruning，還是分次 iterate 的 pruning），得到了一些有趣發現，例如：

1. proposed method 和其他 method 有很高的 correlation。
2. ResNet-101 在逐次 pruning 之後， layer 裡面的 neuron importance 彼此間變得比較相近。
3. Iterative 的 pruning ，能夠動態的 evaluate neuron importance，因此有比較好的結果。





