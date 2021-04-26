
# Image Denoising Via Sparse and Redundant Representations Over Learned Dictionaries

## Personal Info
ID: R08922A20  
NAME: 洪筱慈

## Paper basic information

- Paper link: https://www.egr.msu.edu/~aviyente/elad06.pdf
- Conference/Journal:  Image Processing, IEEE Transactions on Image Processing (2006)   
- Author: Elad and Aharon, Department of Computer Science, The Technion—Israel Institute of Technology

## Key contribution

1. 使用 K-SVD algorithm，找到一個可以有效率描述 image content 的 dictionary。這個 dictionary 的特性是 sparse and redundant representations.
2. 嘗試了兩種 dictionary training 作法： a. 用 corrupted image b. 用 high-quality image。
3. 本來的 K-SVD 在處理小 image 的時候有限制，作者突破了這種限制。
4. 綜合以上的技巧，達到了 state-of-the-art 的 denoising performance。


## Sparse

