
# Self-training with Noisy Student improves ImageNet classification

## Basic information

- Paper link: https://arxiv.org/abs/1911.04252 
- Conference: CVPR'20
- Demo: None
- GitHub: https://github.com/google-research/noisystudent
- work field: `Image Classification`

## What problem does the authors tackled?

To have good performance on image classification when **lack of enough labeled data**.

## How does they solve it? How's the result?

A semi-supervised learning approach. The result is 2% higher than the state-of-the-art model, which is trained on weakly labeled data, and the number of parameters is also twice smaller.


## Key takeaway
Make the student model to be better than teacher, and more robust to different environments by:

1. Add **noise** when training the student.
2. Use **equal-or-larger** student model. (Usually in teacher-student architecture, student is smaller to be faster)


## Detail of the methods

### Basic Training flow

<img src=img/1-1-1.png width=400x>

1. Using labeled data to train the teacher model.  
2. Using the teacher model to generate pseudo label for unlabeled data.
3. Using real label and pseudo label to train the noise-added student model.
4. The student model is viewed as a new teacher model, and go back to 2.

### Noising student

1. input noise: data augmentation(RandAugment):
    - Make the student model to be more **robust** when facing images that are not so clean.
2. model noise: dropout, stochastic depth:
    - These two noises make the teacher model to be like an ensemble model, and so the student model can learn from this powerful ensemble model but act as a single model.


## Experiments
### Datasets

1. Labeled data: ImageNet 2012 ILSVRC challenge prediction task. (good benchmart)
2. Unlabeled data: JFT dataset with 300M images.

### Data filtering and balancing

1. A pre-trained model is used to predict label for each image in JFT dataset.
2. Select images that have confidence of the label higher than 0.3.
3. At most 130k images per class.
4. For those classes that has less than 130k, duplicate some images at random to make it 130k.
5. Result: 130M total, 81M unique.

### Model architecture

* Baseline: EfficientNets.
* Used in experiments: scale up EfficientNet-B7 to EfficientNet-L2 (please refer to [Iterative training](#iter))
    - Reason: EfficientNet-L2 is wider and deeper but use lower resolution, so it has more parameter to fit on more unlabeled data.

### Training Notes

* Use large batch size for unlabeled images to make full use of unlabeled images.
* labeled data and unlabeled data are concatenated to compute average cross entropy loss.
* First train the model with smaller resolution for 350 epochs, and than finetune the model with a larger resolution for 1.5 epochs on unaugmented labeled images.


## Results and discussion
### Classification result
Using ImageNet classification Top-1 accuracy as evaluation matrix, we can see that:
1. From teacher (EfficientNet-B7) to student (EfficientNet-L2), bigger model make a 0.5% improvement.
2. Adding noise to student make 2.9% improvement. This indicate that nosing student is important.

<img src=img/1-1-3.png width=500x>

<a id="iter"></a>

### Iterative training

* Best result is achieved by 3 iterations of training.  
    - iter 1: teacher = EfficientNet-B7, student = EfficientNet-L2(1)
    - iter 2: teacher = EfficientNet-L2(1), student = EfficientNet-L2(2)
    - iter 3: teacher = EfficientNet-L2(2), student = EfficientNet-L2(3)
    
<img src=img/1-1-2.png width=300x>

### Robustness 

* Using three robustness test sets: ImageNet-A, ImageNet-C, ImageNet-P:
    - ImageNet-C, ImageNet-P: blurring, fogging, rotation, scaling.
    - ImageNet-A: difficult images that will cause drops in accuracy.
For all three test sets, the noising model outperformed other baseline and also un-noising model.  

<img src=img/1-1-4.png width=900x>