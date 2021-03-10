# SphereFace: Deep Hypersphere Embedding for Face Recognition


## Personal Info
ID: R08922A20  
NAME: 洪筱慈 


## Paper basic information

- Paper link: https://arxiv.org/abs/1704.08063
- Conference: CVPR 2017
- First author: Weiyang Liu (Georgia Institute of Technology)
- Demo: https://www.youtube.com/watch?v=P6jEzzwoYWs&ab_channel=WeiyangLiu 
- GitHub: https://github.com/wy1iu/sphereface 
- work field: `Face Recognition`

## Key takeaway

* A new loss function called `A-softmax` is proposed to make CNN-based model learn better on angularly discriminative features.  
* This is the first paper that transfer the feature space to hypersphere!

## Introduction
### Open set and close set
In Face Recognition, we can divide the problem into two kinds. If the testing face is existed in training data, we call the problem `close set`. On the other hand, we call it `open set`.    

想像德田館採用了人臉辨識系統，則

* Close set: 每個新生入學時都要先建置自己的臉到系統內，一張臉對應一個 ID，系統會用來 training。當學生 A 掃臉時，系統根據他的臉得到一個預測 ID ，說明這個人是誰（Face identification）。 而若要確認兩張人臉是不是同一個人（Face verification），則系統會根據這兩張人臉各自預測出一個 ID，再藉由比對 ID 知道是不是同一個人。因此， close set 其實是一個 **分類** 問題，已知共有 n 類（也就是學生總數）。 input 一張人臉，預測他是誰（屬於哪一類）。

* Open set: 給定一張人臉，系統不是預測類別，而是當作 feature extractor，output 屬於這張人臉的 embedding。因此當要做 Face verification 時，計算兩張臉的 embedding 距離，距離越小則代表這兩個人越像、是同一個人的機率越高。

### Similarity
如上所述，我們藉由計算 embedding 的距離來判斷兩張人臉是不是同一個人，傳統做法便是計算 cosine similarity 作為距離的評估， cosine 大，代表兩個 embedding 的夾角小，也就是這兩個 embedding 接近、很像。而最理想的 embedding 便是不要模棱兩可，不同人就分得越開越好、夾角越大越好。因此，這篇 paper 的貢獻便在於提出一個 loss ，能讓模型學出的 embedding 明顯分開，有明顯的 `Decision boundary`，大大提升辨識的表現。  


## Loss design

### From softmax loss to M-softmax loss

原始 softmax loss 的 Decision boundary 並非只由角度決定，因此我們沒辦法單靠角度將不同類別分開。經過一番數學上的調整之後，讓 Decision boundary 完全只由角度決定，變成 M-softmax loss。  
當<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dpi{100}&space;cos(\theta_{1})&space;>&space;cos(\theta_{2})" target="_blank"><img src="https://latex.codecogs.com/png.latex?\inline&space;\dpi{100}&space;cos(\theta_{1})&space;>&space;cos(\theta_{2})" title="cos(\theta_{1}) > cos(\theta_{2})" /></a>  ，則屬於第一類，反之則是第二類。

### From M-softmax loss to A-softmax loss

但作者不滿足於此，要分開就把它分超開！   
A-softmax loss 則是進一步擴大差異，加入一個參數 ｍ，使得   <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dpi{100}&space;cos(m&space;\theta_{1})&space;>&space;cos(\theta_{2})" target="_blank"><img src="https://latex.codecogs.com/png.latex?\inline&space;\dpi{100}&space;cos(m&space;\theta_{1})&space;>&space;cos(\theta_{2})" title="cos(m \theta_{1}) > cos(\theta_{2})" /></a>   
 
## Experiments



## Discussion


## Extended summary
這一篇 paper 開啟了一系列 angular margin 的應用，包含後來的 CosFace(CVPR 2018)[2], ArcFace(CVPR 2019)[3]，可以說是一篇啟蒙 paper!  
<a href="https://openaccess.thecvf.com/content_CVPR_2019/papers/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.pdf"><img src=img/2-1-1.png  width=600x></a>



### Reference
[1] [SphereFace Paper Study and Implementation Notes](https://bobondemon.github.io/2019/06/18/SphereFace-paper-study-and-implementation-notes/)  
[2] [CosFace: Large Margin Cosine Loss for Deep Face Recognition](https://arxiv.org/abs/1801.09414)  
[3] [ArcFace: Additive Angular Margin Loss for Deep Face Recognition, CVPR_2019](https://openaccess.thecvf.com/content_CVPR_2019/papers/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.pdf)