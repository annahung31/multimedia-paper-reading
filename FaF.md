
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

舉監視器當例子，就是可以同時做到“偵測出有哪些車子”, "追蹤這些車子往哪裡開“, "接下來會往哪裡開"。

## Key contribution

利用 Convolutional net 對 3D 影像的 space domain 和 time domain 處理，所使用的方法有三大優點：
1. 不僅省 memory，還很快（30ms)
2. Performace 是 state-of-the-art
3. 對於 occlusion 跟 sparse data 是 robust 的


## Method

### Data representation
使用 voxel 來表示 3D 影像。 voxel 就像是玩 minecraft 裡面的一個一個立方體。因為是立方體，所以可以直接 apply convolutions。每一個 voxel 都會被 assign 一個 binary indicator 來表示是否有值，不過要注意這樣的 grid 是很 sparse 的（只有少數 voxel 有值）。
在 voxel 的三個維度中，使用 2D convolution 來處理，而 high dimension 則是當作 channel，這樣就可以使模型學到 high dimension 中的資訊。  

為了處理時序上的資訊，假設這在處理以時間為 t 的 frame，那在 t 前後的 frame 都被轉換座標到 t 的座標上，這樣才能比較車子在不同時間點是在哪個位置上。

### Model design

對於處理時序資訊，採用了兩個方法： Early fusion and late fusion:

1. Early fusion: 在第一個 layer 就 aggregate 時序資訊。
2. Late fusion: 在過程中逐漸 merge 時序資訊。

兩者比較可見下圖：
<div align="left">
<img src=img/15-1-1.png width=1000x>
</div>

下一步，會加入兩個 branch 分別做:
1. binary classification: 預測是不是車子
2. 預測在其他時間點的 bounding box

在每一個時間點， model 會預測 n 個 timestamps 的 bounding box。如此以來，只要有過去時間點的 bounding box 收集起來，就可以預測車子的軌跡了。

