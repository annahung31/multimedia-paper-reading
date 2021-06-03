
# Fast and Furious: Real Time End-to-End 3D Detection, Tracking and Motion Forecasting with a Single Convolutional Net


## Personal Info

ID: R08922A20  
NAME: 洪筱慈 

## Basic information

- Paper link: https://arxiv.org/abs/2012.12395
- Conference: CVPR'20


## Goal of this Paper

針對 3D 影像（video) 同時做三個 task:
1. 3D detection
2. Tracking
3. Motion forecasting(預測)

舉監視器當例子，就是可以同時做到“偵測出有哪些、哪裡有車子”, "追蹤這些車子往哪裡開“, "接下來會往哪裡開"。

<div align="left">
<img src=img/15-1-4.png width=700x>
</div>



## Key contribution

利用 Convolutional net 對 3D 影像的 space domain 和 time domain 處理，所使用的方法有三大優點：
1. 不僅省 memory，還很快（30ms)
2. Performace 是 state-of-the-art
3. 對於 occlusion 跟 sparse data 是 robust 的


## Method

### Data representation
使用 voxel 來表示 3D 影像。 voxel 就像是玩 minecraft 裡面的一個一個立方體。因為是立方體，所以可以直接 apply convolutions。每一個 voxel 都會被 assign 一個 binary indicator 來表示是否有值，不過要注意這樣的 grid 是很 sparse 的（只有少數 voxel 有值）。
在 voxel 的三個維度中，使用 2D convolution 來處理，而 high dimension 則是當作 channel，這樣就可以使模型學到 high dimension 中的資訊。  

為了處理時序上的資訊，假設現在是處理時間為 t 的 frame，那在 t 前後的 frame 都被轉換座標到 t 的座標上，這樣才能比較車子在不同時間點是在哪個位置上。

### Model design

對於處理時序資訊，採用了兩個方法： Early fusion and late fusion:

1. Early fusion: 在第一個 layer 就 aggregate 時序資訊。
2. Late fusion: 在過程中逐漸 merge 時序資訊。

兩者比較可見下圖：
<div align="left">
<img src=img/15-1-1.png width=800x>
</div>

下一步，會加入兩個 branch 分別做:
1. binary classification: 預測是不是車子
2. 預測在其他時間點的 bounding box

在每一個時間點， model 會預測 n 個 timestamps 的 bounding box。如此以來，只要有過去時間點的 bounding box 收集起來，就可以預測車子的軌跡了。
<div align="left">
<img src=img/15-1-5.png width=500x>
</div>



### Loss function

在上面提到的 branch 中，一個 task 是分類問題，另一個是 regression 問題，因此在 loss 裡面涵蓋了兩者，其中 regression 考慮了所有範圍內的時間點：

<div align="left">
<img src=img/15-1-2.png width=500x>
</div>

其中 t 是目前時間點，w 是模型參數，下標 cla 代表 classification loss， reg 則是 regression。

對於 classification loss，對所有的位置跟 predefined box 計算 binary cross-entropy。 而對於 regression loss ，首先對影像做一些預處理得到  ground truth，再對所有的 target apply weighted smooth L1 loss。 （詳細請參考 session 3.3)   

在訓練的過程中，因為 positive sample 比較多， negative sample 比較少，作者另外取了 classification predict score 比較差的一些 sample 來當作 negative sample 使用。


### Experiment results

1. **Detection** result: 想當然是 outperform 其他方法。值得一提的是，把畫面距離拉長， FaF 的 performance 掉的幅度比其他方法低。我覺得這個 feature 是很有用的，畢竟在 real world 的應用裡面，很難保證畫面都是近拍的。

<div align="left">
<img src=img/15-1-3.png width=500x>
</div>

2. **Tracking** result: 給定 track id, 就能夠 output detection。評斷 tracking 的方法，是計算四種 matrix: MOTA, MOTP, MT, ML。和 Hungarian 方法相比，除了 MOTP 之外，FaF 都 outperform。

3. **Motion forecasting**: 計算車輛中心點的預測結果和真實位置的 L1 和 L2 位置。結果顯示，在未來的 10 的 frame 以內，預測的 l2 distance 可以小於 33 公分！

