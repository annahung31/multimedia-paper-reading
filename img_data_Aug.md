# A survey on Image Data Augmentation for Deep Learning

## Basic information

- Paper link: https://journalofbigdata.springeropen.com/track/pdf/10.1186/s40537-019-0197-0.pdf 
- Conference: J Big Data 2019
- Demo:
- GitHub: 
- work field: Image data processing

## Data augmentation methods included in this survey

### Methods Based on Basic image manipulations

* Geometric transformations
* Flipping
* Color space
* Cropping
* Rotation
* Translation
* Noise injection
    <div align="left">
    <img src=img/1-2-1.png width=500x>
    </div>

* Color space transformations
    <div align="left">
    <img src=img/1-2-2.png width=400x>
    </div>
* Geometric v.s photometric transformations:  
    A study from Taylor and Nitschke shows the performance of different augmentation methods:
    <div align="left">
    <img src=img/1-2-3.png width=400x>
    </div>

* Kernel filers
    - type 1: Using Gaussian blur filter to make the image blurrier, or using high contrast edge filter to make the image sharper along edges.
        - Useful for motion blur images.
        - Parameter that effect the result: filter size.
        - Disadvantage: similar to CNN filter
    - Type 2: `PatchShuffle`, randomly swap pixel values in a patch.
    <div align="left">
    <img src=img/1-2-4.png width=400x>
    </div>

* Mixed images

    - Three ways to mix two images:

        1. linearly adding the pixel values  
        <div align="left">
        <img src=img/1-2-5.png width=400x>
        
        2. Generalized
        <div align="left">
        <img src=img/1-2-6.png width=260x>
        <img src=img/1-2-7.png width=300x>

        3. Using GAN: can reduce the training time and increase the diversity of GAN samples. The disadvantage is hard to explain.
        </div>

* Random erasing
    <div align="left">
    <img src=img/1-2-8.png width=400x>
    </div>

NOTE: Using these augmentation can produce a big amount of data, but not always advantageous. In domain with limited data, these methods will cause over-fitting because of lack of diversity.  

### Methods based on deep learning

* Feature space augmentation
* Adversarial training
* GAN-based data augmentation
* Neural style transfer
* Meta learning data augmentations

NOTE: The disadvantage of these methods is that it's hard to explain why they work, hard to interpret the representation they learned.




## Discussions

- Limitation of data augmentation: augmentation methods perform better under the assumption that training set and test set have similar degree of diversity.  For example, in a dog breed classification task, if there is no Border Collie in the training set, no augmentation method can create one.

- Some experiments have shown that the combination of augmentation methods can improve the performance, but there is still no consensus about the best strategy for combining data warping and oversampling techniques.
