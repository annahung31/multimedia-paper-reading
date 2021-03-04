# A survey on Image Data Augmentation for Deep Learning

## Basic information

- Paper link: https://journalofbigdata.springeropen.com/track/pdf/10.1186/s40537-019-0197-0.pdf 
- Conference: J Big Data 2019
- Demo:
- GitHub: 
- work field: Image data processing



## Key takeaway

* Choose augmentation method based on your task.
* The policy to use augmented data needs to be considered.(refer to [Curriculum learning](#policy))
* You can choose online data augmentation(instead of doing augmentation during pre-processing) to save memory.


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
    <img src=img/1-2-3.png width=600x>
    </div>

* Kernel filers
    - type 1: Using Gaussian blur filter to make the image blurrier, or using high contrast edge filter to make the image sharper along edges.
        - Useful for motion blur images.
        - Parameter that effect the result: filter size.
        - Disadvantage: similar to CNN filter
    - Type 2: `PatchShuffle`, randomly swap pixel values in a patch.
    <div align="left">
    <img src=img/1-2-4.png width=600x>
    </div>

* Mixed images

    - Three ways to mix two images:

        1. linearly averaging the pixel values  
        <div align="left">
        <img src=img/1-2-5.png width=600x>
        
        2. Generalized
        <div align="left">
        <img src=img/1-2-6.png width=390x>
        <img src=img/1-2-7.png width=450x>

        3. Using GAN: can reduce the training time and increase the diversity of GAN samples. The disadvantage is hard to explain.
        </div>

* Random erasing
    <div align="left">
    <img src=img/1-2-8.png width=500x>
    </div>

NOTE: Using these augmentation can produce a big amount of data, but not always advantageous. In domain with limited data, these methods will cause over-fitting because of lack of diversity.  

### Methods based on deep learning

* Feature space augmentation
    - Neural networks can map high-dimensional inputs into lower-dimensional representations, and these representations are called feature spaces.
    - Instead of doing data augmentation in the input data, we can also manipulate the feature space.
    - For example, `SMOTE` is a method used for dealing with data imbalance. In the feature space, k nearest neighbors are joined to form a new instance.
    - Apart from SMOTE, `adding noise`, `interpolating`, `extrapolating` are also common ways that doing augmentation in feature space.
    <div align="center">
    <img src=img/1-2-9.png width=500x>
    </div>

* Adversarial training
    - Adversarial training means using two or more networks with contrasting objectives to force they learn from each other and defect each other.
    - Adversarial attack: a model that learns to apply noise to the input and fool its rival classification model, making it to make a wrong classification. This shows that the representation learned by model is vulnerable. Example work: `DeepFool`[77], [78], [79]. 
    <div align="center">
    <img src=img/1-2-10.png width=500x>
    </div>

    - The way to defend the adversarial attack: adversarial training. Example: Li et al.[83]. The result shows that with adversarial training, the model can still have good accuracy when facing adversary examples(FGSM, PGD)
    <div align="center">
    <img src=img/1-2-11.png width=500x>
    </div>

    - Another adversarial attack: `DisturbLabel` randomly replaces labels at each iteration, adding noise to the loss layer.

* GAN-based data augmentation
    - Using GAN or VAE to generate samples.
    - The author thinks that GANs are the most promising generative modeling technique for use in data augmentation.
    - Example: DCGAN, CycleGAN, Conditional GAN
    - GAN samples can be used to generate samples to solve class imbalance.
    - Limitations: 
        - Still have difficulty on generating high-resolution outputs.
        - Training GAN itself also requires a big amount of data.

* Neural style transfer
    - Neural style transfer is somewhat comparable to color space transformation. It can extend lighting variations and generate images with different texture and artistic styles.
    - What we need to think about is what kind of extension we need for our task. for example, for a self-driving task, day-to-night or winter-to-summer transfer might be helpful.
    - **Disadvantage** of using style transfer as augmentation: if the style set is too small, other bias might be introduced into the dataset.
    <div align="center">
    <img src=img/1-2-12.png width=500x>
    </div>


* Meta learning data augmentations
    - Optimizing neural networks with neural networks.
    - Example: NAS, simple random search.
    - Neural augmentation: meta-learn a neural style transfer model to find the weight and content loss. 
    - Smart augmentation: Network A is a augmentation model that take two or more images as inputs and map them into a new image to train Network B. Also another loss is added to Network A to make sure the generated images are still in the same class of the input. Then the change of error rate of Network B will come back to update Network A. It's similar to mixed-examples, but more sophisticated.
    <div align="center">
    <img src=img/1-2-13.png width=500x>
    </div>
    - AutoAugment

NOTE: The disadvantage of these methods is that it's hard to explain why they work, hard to interpret the representation they learned.


## Design considerations
* Test-time augmentation
    - Doing augmentation during test time. 
    - Could be the bottleneck of real time prediction. 
    - A good classifier should have good performance on augmented test set. For example, a prediction image of an image should not be different when the same image is rotated 20 degree.


<a id="policy"></a>

* **Curriculum learning**: how we make use of original data and augmented data?
    - Some research advice that it is better to train with original data first and then finish training with original data and augmented data. For example, 100 epoch for original data, and after adding augmented data, run in cycle between 8:2 epochs.
    - Training accuracy over time across different initial training subsets could help reveal patterns in the data that dramatically speed up training time.

* Determination of the final dataset size

    - What you need to consider: additional memory and compute constraints for the augmented data. There are two options: online data augmentation and offline augmentation.

## Discussions

- Limitation of data augmentation: augmentation methods perform better under the assumption that training set and test set have similar degree of diversity.  For example, in a dog breed classification task, if there is no Border Collie in the training set, no augmentation method can create one.

- Some experiments have shown that the combination of augmentation methods can improve the performance, but there is still no consensus about the best strategy for combining data warping and oversampling techniques.


