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
* Color space transformations
* Geometric versus photometric transformations
* Kernel filers
    - type 1: Using Gaussian blur filter to make the image blurrier.
    - type 2: Using high contrast edge filter to make the image sharper along edges.
    - Useful for motion blur images.
    - Parameter that effect the result: filter size.
    - Disadvantage: similar to CNN filter

* Mixed images
* Random erasing

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
