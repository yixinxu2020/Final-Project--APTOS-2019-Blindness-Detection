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
2015 kaggle Diabetic Retinopathy (resized)
> https://www.kaggle.com/tanlikesmath/diabetic-retinopathy-resized

Pseudo Labeling

Pseudo labeling is the process of adding confident predicted test data to your training data. Pseudo labeling is a 5 step process. (1) Build a model using training data. (2) Predict labels for an unseen test dataset. (3) Add confident predicted test observations to our training data (4) Build a new model using combined data. And (5) use your new model to predict the test data and submit to Kaggle. Here is a pictorial explanation using sythetic 2D data.
> https://www.kaggle.com/cdeotte/pseudo-labeling-qda-0-969
### Model Ensemble


