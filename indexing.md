
# Efficient Indexing for Large Scale Visual Search  

## Personal Info
ID: R08922A20  
NAME: 洪筱慈

## Paper basic information

- Paper link: http://vigir.missouri.edu/~gdesouza/Research/Conference_CDs/IEEE_ICCV_2009/contents/pdf/iccv2009_142.pdf    
- Conference/Journal: ICCV 2009 
- Author: Xiao Zhang et al., Tsinghua University; Zhiwei Li, Lei Zhang, Wei-Ying Ma, Heung-Yeung Shum, **Microsoft Corporation**  


### Key contribution

1. 把 image representation decompose 成 dimension reduction 跟 residual information representation，則假如要知道兩張 image 的相似度，就只要算這些 component 的相似度。
2. Decompose 的價值在於：
    a. 可以有效率的作 indexing 跟 retrieval
    b. 可以更好的 generalization


### Method

1. 利用 feature decomposition model 幫每張照片抽出 compound representation。比較一般的 feature 跟比較特別的 feature 都可以被抓出來。特別的是，在這個 model 中，我們可以知道圖片裡的哪個部分造成的 representation error 是最大的，因此就可以藉此選出這張照片最關鍵的 representation。最後，這張照片就會被表示成一個 low-dimentional vector，以及一些重要的 key terms。

2. 設計一個適合的 indexing scheme。這個 indexing 會把上一步的重要 key terms 納進 ranking 時考慮。

抽 feature、降維的方法非常多，讓我覺得最厲害的就是他能夠知道影像中的哪一個部分對應到 representation 中的哪裡。他是如何做到這點的呢？


### Decomposition
在 decomposition 中，作者加入了 residual of reconstruction，維度和圖片的高維度 representation 相同。他造成的結果可以從下圖中看到：

<div align="center">
<img src=img/10-1-1.png width=1000x>
</div>

首先，兩張室內照片被投影到比較接近的位置，車子則比較遠，因此可以很容易辨別出室內照片和車子的不同。然而，兩張室內照片就比較難分辨出那一張是 office，哪一張是 kitchen。這時候，照片的 residual，也就是照片 sample point 到subspace 的距離，就可以幫助我們辨別出差別。