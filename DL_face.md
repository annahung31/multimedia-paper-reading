# Deep Learning for Understanding Faces: Machines May Be Just as Good, or Better, than Humans

## Personal Info
ID: R08922A20  
NAME: 洪筱慈 

## Paper basic information

- Paper link: https://ieeexplore.ieee.org/document/8253595
- Conference/Journal: IEEE Signal Processing Magazine 2018
- Demo: None
- GitHub: None
- work field: `Face recognition`


## Introduction
在這篇 paper 中，作者整理了常見的 Face recognition model pipeline 和 dataset。  
1. region-proposal approach v.s sliding window approach
<div align="center">
<img src=img/2-2-1.png width=1000x>
</div> 

2. Crucial facial keypoints  
先抓取臉部特徵，例如 eye center, nose tip, mouth corner 等等的作為區辨。
<div align="center">
<img src=img/2-2-2.png width=1000x>
</div> 

3. Face identification and verification
這個 task 主要包含兩個部分，一個是 feature extractor，另一個是 classifier。training 時學習抽取特徵，並加以辨別， testing 時輸入兩張人臉，抽取特徵之後計算兩個特徵的相似度來辨別是不是同一個人。
<div align="center">
<img src=img/2-2-3.png width=1000x>
</div> 

4. Data sets
作者整理了常見用於 face recognition 的 dataset，其中他提到一篇 paper[1]所做的討論很有趣：
* Q1: Can we train on still images and expect the systems to work on videos?  
實驗結果顯示， train 在 still image + video 的 model 表現得比單獨 train 在 still image 或 video 上的都還好。
* Q2:  Are deeper data sets better than wider data sets? 
    * Deeper data sets: 類別多，同一類別中的 sample 較少
    * Wider data sets: 類別少，每個類別有很多 sample  
假定 sample 總數量相同，對於小一點的 model，用 wider data sets 表現比較好（；反之，比較深的 model 則是用 deeper data sets 比較好。
（這篇 paper 在這裡似乎有筆誤，詳細請見原 paper [1] 的 section 3.2）    
這是一個很有趣的觀察，它告訴我們，當我們在有限的時間下要搜集 data 時，也要把我們預計要用的 model 考量進來，更聰明的選擇收集 data 的策略。



## Reference
[1] [The Do's and Don'ts for CNN-based Face Verification](https://arxiv.org/abs/1705.07426)