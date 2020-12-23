# Final-Project--APTOS-2019-Blindness-Detection
## Introduction
This task is a kaggle competation-APTOS-2019-Blindness-Detection, the details is under

https://www.kaggle.com/c/aptos2019-blindness-detection/overview/description

This is our team member:

name | student id
------------ | ------------- |
易新旭 | 0856152
左笑寒 |  0616109
蘇信恩 |  0616215
## Proposed Approach
### Data Proprecess
The pretreatment program of Diabetic Retinopathy Detection, the first prize in a similar competition in 2015, was Ben.In simple terms, it is to improve the inconsistent brightness of pictures, and make center crop and remove black edges on the pictures. The details can be referred to as follows:
> Ben Graham's preprocessing method(scale radius) 

> https://www.kaggle.com/ratthachat/aptos-eye-preprocessing-in-diabetic-retinopathy#2.-Try-Ben-Graham's-preprocessing-method.
### Additional Training Datasets
- 2015 kaggle Diabetic Retinopathy (resized)
> https://www.kaggle.com/tanlikesmath/diabetic-retinopathy-resized

- Pseudo Labeling

Pseudo labeling is the process of adding confident predicted test data to your training data. Pseudo labeling is a 5 step process. (1) Build a model using training data. (2) Predict labels for an unseen test dataset. (3) Add confident predicted test observations to our training data (4) Build a new model using combined data. And (5) use your new model to predict the test data and submit to Kaggle. Here is a pictorial explanation using sythetic 2D data.
> https://www.kaggle.com/cdeotte/pseudo-labeling-qda-0-969
### Model Ensemble
## Training
The training process can be divided into two stages. In the first stage, use 2019 train dataset (5-folds cv) + 2015 dataset as training datasets, devide the datasets into 5 folder to train 5 basic on the local, and get the 1st-level model. Then use the 1st-level model with Pseudo Labeling to label the test dataset as addtional training datasets, and run the 2en-level model on the kernel. Fanally ensamble the models get the final result.
### 1st-level models (run on local)
To train 1st-level models, run:

```
python train.py --arch se_resnext50_32x4d
python train.py --arch se_resnext101_32x4d --batch_size 24
python train.py --arch senet154 --batch_size 16
```
- Models: SE-ResNeXt50\_32x4d, SE-ResNeXt101\_32x4d, SENet154
- Loss: MSE
- Optimizer: SGD (momentum=0.9)
- LR scheduler: CosineAnnealingLR (lr=1e-3 -> 1e-5)
- 30 epochs
- Dataset: 2019 train dataset (5-folds cv) + 2015 dataset (https://www.kaggle.com/tanlikesmath/diabetic-retinopathy-resized)

### 2nd-level models (run on [kernel](https://www.kaggle.com/stormdiv/nctu-cs-t0828-final-aptos-2019-0856152?scriptVersionId=49287316))
- Models: SE-ResNeXt50\_32x4d, SE-ResNeXt101\_32x4d (1st-level models' weights)
- Loss: MSE
- Optimizer: RAdam
- LR scheduler: CosineAnnealingLR (lr=1e-3 -> 1e-5)
- 10 epochs
- Dataset: 2019 train dataset (5-folds cv) + 2019 test dataset (**public + private**,  divided into 5 and used different data each fold. )
- Pseudo labels: weighted average of 1st-level models

### Ensemble
Finally, averaged 2nd-level models' predictions.

- PublicLB: 0.826
- PrivateLB: 0.930
## Reference 
**Unsupervised Clustering Using Pseudo-semisupervised Learning**
> https://openreview.net/pdf?id=rJlnxkSYPS

**Pseudo-Label : The Simple and Efficient Semi-Supervised Learning Method for Deep Neural Networks**
> https://www.researchgate.net/publication/280581078_Pseudo-Label_The_Simple_and_Efficient_Semi-Supervised_Learning_Method_for_Deep_Neural_Networks


