
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
A semi-supervised learning approach. The result is 2% higher than the state-of-the-art model, which is trained on weakly labeled data.


## Key takeaway
Make the student model to be better than teacher, and more robust to different environments by:
1. Add **noise** when training the student.
2. Use **equal-or-larger** student model. (Usually in teacher-student architecture, student is smaller to be faster)


## Detail of the methods
### Training flow
<img src=img/1-1-1.png width=400x>
1. Using labeled data to train the teacher model
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
* 


