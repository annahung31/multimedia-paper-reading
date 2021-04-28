
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
     