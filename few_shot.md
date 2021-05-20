# Adaptive Subspaces for Few-Shot Learning

## Personal Info
ID: R08922A20  
NAME: 洪筱慈 


## Basic information

- Paper link: https://openaccess.thecvf.com/content_CVPR_2020/papers/Simon_Adaptive_Subspaces_for_Few-Shot_Learning_CVPR_2020_paper.pdf 
- Conference: CVPR'20
- Demo: https://www.youtube.com/watch?v=A6dVu5wjSAk
- GitHub: https://github.com/chrysts/dsn_fewshot 
- work field: `few-shot learning`


##  Key contribution

在 object recognition 中，當 data 數量不多，就沒辦法使模型擁有 generalization 的能力。這篇 paper 提出了一個 few-shot learning 的 framework，能夠在少量 data 之下仍然能夠使模型夠 robust，在面對 outlies 時仍有好的表現。

## Method

### 利用 subspace 建立 classifier：
<div align="center">
<img src=img/13-1-1.png width=1000x>
</div>
(a)-(c) 是幾種常見的 few-shot learning classifier，例如 (a)是 pair-wised 的比較兩點的差異，而 (c)則是求出一條邊界，能夠區分兩個 class。 (d) 是這篇 paper 提出的方法，利用 subspace 來創造出 classifier。


### Pipeline: 
<div align="center">
<img src=img/13-1-2.png width=1000x>
</div>

可以分成三個部分：    

(1) feature extractor：使用了 4-convolutional layers 跟 ResNet-12。     
(2) dynamic classifier     
(3) Discriminative method     
先將 input image 經過feature extractor 投影到subspace，每個 class 都用 一個 vector 表示，再利用 discriminative method 將這些不同 class 的 vector 盡量的分開。  
而因為在分類過程中，每一個 class 計算類別時都和該 class 有關，因此稱為 dynamic classifier。


## Result
1. 實驗使用的 dataset: mini-ImageNet, tiered-ImageNet, CIFAR, OPEN MIC
<div align="center">
<img src=img/13-1-3.png width=1000x>
</div>
上圖中，1, 3 列是使用 prototypes 的方法， 2,4 列則是使用 Subspaces(本篇作法)。前兩欄是做在 2-class 問題，後兩欄則是 3-class 問題。
在 2-class 問題中，可以看到，當沒有加 outliers 時，兩個方法表現得都很好，但是有了 outliers 之後， prototypes 方法裡面的 class boundary 就被嚴重影響，大量的 sample 被歸到錯誤的 class 去了。但是 subspace 的做法中，outliers 的方法影響就比較小，雖然仍受 outlier 影響而偏移 boundary，但是仍然能夠顧及大部分的 data，不至於造成太多的錯誤。
由上圖可以看出，這篇 paper 提出的方法，在有 outlier 存在的狀況下，仍然能夠表現得好。   
