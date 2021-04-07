# Graph Embedding and Extensions: A General Framework for Dimensionality Reduction  


## Personal Info
ID: R08922A20
NAME: 洪筱慈

## Paper basic information

- Paper link: https://www.researchgate.net/publication/220181328_Graph_Embedding_and_Extensions_A_General_Framework_for_Dimensionality_Reduction  
- Conference/Journal: IEEE Transactions on Pattern Analysis and Machine Intelligence 29(1):40-51  
- Demo: None
- GitHub: None
- work field: `Dimension Reduction`, `Feature Engineering`  

## Key contribution  
提出一種新的降維方式 Marginal Fisher Analysis (MFA)， 能夠使 intra class 更加群聚， inter class 分得更開。


## Introduction to dimension reduction  
### 什麼時候會用到降維？   
舉影像辨識為例，要判斷一張照片中是貓還是狗，我們其實不需要知道每一個 pixel 的值是多少，只要知道輪廓，就能夠判斷出來。真實的訓練資料通常是很高維度的資料，例如相片 raw 檔，說話音源，但是真實資料中往往包含許多重複不需要的 feature， 假如 feature 數量相較於 training data 的數量大很多，就可能導致 model overfitting。  

![](img/7-1-1.png)  
*The Curse of Dimensionality: As the number of features increases, the model becomes more complex. The more the number of features, the more the chances of overfitting. [2]*


因此減少不必要 feature 的一種方式就是降維。  

### 常見的降維方法 
1. 線性降維：PCA (Principal Component Analysis),Factor Analysis, LDA (Linear Discriminant Analysis)  

![](img/7-1-2.png)  
*PCA orients data along the direction of the component with maximum variance whereas LDA projects the data to signify the class separability [2]*


2. 非線性降維： ISOMAP、LLE、Laplacian Eigenmap, kernel trick 。  

### 傳統降維的缺陷

然而，這些傳統的降維方式，都是建立在 “各個群體的資料本身都是高斯分佈" 假設下做分群的，你可以從上面的 PCA 圖中看見。  真實世界的資料，當然不會那麼完美符合高斯分佈，因此傳統的降維做法存在誤差和缺陷。這篇 paper 中提出的 FMA，就是為了解決這樣的問題。




## Reference
[1] [Why is Dimensionality Reduction so Important?](https://medium.com/@cxu24/why-dimensionality-reduction-is-important-dd60b5611543)  
[2] [A beginner’s guide to dimensionality reduction in Machine Learning](https://towardsdatascience.com/dimensionality-reduction-for-machine-learning-80a46c2ebb7e)


https://hackmd.io/@Ql-hvOksS5KKmIlqAPPrMA/paper_summary/https%3A%2F%2Fhackmd.io%2FtzOBPOFBTom8vAmPery23A%3Fview

