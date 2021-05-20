# Adaptive Subspaces for Few-Shot Learning



## Basic information

- Paper link: https://openaccess.thecvf.com/content_CVPR_2020/papers/Simon_Adaptive_Subspaces_for_Few-Shot_Learning_CVPR_2020_paper.pdf 
- Conference: CVPR'20
- Demo: https://www.youtube.com/watch?v=A6dVu5wjSAk
- GitHub: https://github.com/chrysts/dsn_fewshot 
- work field: `few-shot learning`


##  Key contribution

在 object recognition 中，當 data 數量不多，就沒辦法使模型擁有 generalization 的能力。這篇 paper 提出了一個 few-shot learning 的 framework，能夠在少量 data 之下仍然能夠使模型夠 robust，在面對 outlies 時仍有好的表現。

## Method

1. 利用 subspace 建立 classifier：
<div align="center">
<img src=img/13-1-1.png width=1000x>
</div>
(a)-(c) 是幾種常見的 few-shot learning classifier，例如 (a)是 pair-wised 的比較兩點的差異，而 (c)則是求出一條邊界，能夠區分兩個 class。 (d) 是這篇 paper 提出的方法，利用 subspace 來創造出 classifier。


2. 整體 framework: 
<div align="center">
<img src=img/13-1-2.png width=1000x>
</div>
先將 input image 經過feature extractor 投影到subspace，每個 class 都用 一個 vector 表示，再利用 discriminative method 將這些不同 class 的 vector 盡量的分開。


