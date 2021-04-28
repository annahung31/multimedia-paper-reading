
# Image Denoising Via Sparse and Redundant Representations Over Learned Dictionaries

## Personal Info
ID: R08922A20  
NAME: 洪筱慈

## Paper basic information

- Paper link: https://www.egr.msu.edu/~aviyente/elad06.pdf
- Conference/Journal:  Image Processing, IEEE Transactions on Image Processing (2006)   
- Author: Elad and Aharon, Department of Computer Science, The Technion—Israel Institute of Technology

## Key contribution

1. 使用 K-SVD algorithm，train 出一個可以有效率描述 image content 的 dictionary。這個 dictionary 的特性是 sparse and redundant representations.
2. 嘗試了兩種 dictionary training 作法： a. 用 corrupted image b. 用 high-quality image。
3. 本來的 K-SVD 只能被限制在處理小 image，作者突破了這種限制。
4. 綜合以上的技巧，達到了 state-of-the-art 的 denoising performance。


## 針對兩種 training 方法的討論
### Train on image patches
在 training 的過程，目標是得到一個 dictionary ，使得 samples 的 representation 是 sparse 的，且 representation error 是小的，也就是說 representation 要足以代表原本的 sample，而不同 sample 的 representation 之間又要夠 sparse。 K-SVD 提供了 iterative algorithm 來達到上述要求。   
要將這個方法應用在 denoising 上面，最關鍵的點是在挑選哪些 data 用於 training。真的存在一個 dictionary 能代表所有的 images 嗎？假如存在，要用哪些 images 來找到它？也就是說，什麼樣的 images 是足夠具有代表性的呢？ 文中作者提到，雖然單一的 dictionary 是有可能可以被找到的，但是用多個 dictionary 也是可能的。

### Train on the Corrupted image
除了用完整的 image，我們也可以從 corrupted image 中抓一小塊 patch 來做 training。