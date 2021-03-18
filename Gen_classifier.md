# Exploiting Generative Models in Discriminative Classifiers


## Paper basic information
- Paper link: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.44.7709&rep=rep1&type=pdf 
- Conference: NIPS 1998
- First author: Tommi S. Jaakkola (University of California)
- GitHub: https://github.com/wy1iu/sphereface 
- work field: `Face Recognition`


## Why this old paper is a must-read work?

從這篇 paper 後續衍生了許多重要的 work！


## Key contribution

作者提出了拿 generative probability model 的 kernel function 來用在 Descriminative classifier 上面，使得 classification 的 performance 提升。


## Introduciton to kernel function
kernel function 是用來描述兩個 sample 或 feature 之間的相似度，也可以是當作 predict 時各個類別的 weight，根據這些 weight 來決定這個 sample 比較像哪一種類別：
設 kernel function 為 K，要預測 X 的 label，則已知 X_i 的 label 為 label_i，則：  
predict_label = sign(sum( label_i * lambda * K(X, X_i) )   

要能夠有好的 classification performance，除了妥善決定參數 lambda 之外，最關鍵的一點就是如何決定 kernel function。

一般來說，一個 kernel function 最基本的規定是 positive semi-definite[1]，也就是 f(0) = 0, f(x) > 0 for all x ，對任何 input x， output 皆為正（可以想像成距離）。  

## Fishing score and fishing kernel

Fisher score 會將 input X map 成一個 feature vector，在 gradient space 上面是一個點。
fishing kernel 則是將本來困難不可分辨的難題，轉換到更高維度，變得可分。[2]  


## Experiment Result

<div align="center">
<img src=img/3-1-1.png width=1000x>
</div>

使用了 kernel function 的 classification (實線)確實比沒有加(虛線)更好。



## Reference  
[1] [Positive Semidefinite Function](https://math.stackexchange.com/questions/1489670/positive-semidefinite-function)  
[2] [Fisher Vector費舍爾向量and FIsher Kernel費舍爾核](https://www.itread01.com/content/1549136893.html)  
[3] [Fisher kernel on WIKI](https://en.wikipedia.org/wiki/Fisher_kernel)