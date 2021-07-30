---
title: "Practical Machine Learning Project"
author: "Lee Knupp"
output: html_document
---
## Overview  
In the following report, we attempt to predict the manner in which participants in a study did exercise (details below). After some preprocessing, we fit 3 different prediction models: decision tree, random forest, and generalized boosted model. The accuracy of these are explored, and the best fit is used to predict the test dataset.  

## Background  
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).  

## Data and Package Loading  
First, we load the data and look at the initial dimensions of our training csv. The required packages are also loaded.

```
## [1] 19622   160
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```
## Loading required package: tibble
```

```
## Loading required package: bitops
```

```
## Rattle: A free graphical interface for data science with R.
## Version 5.4.0 Copyright (c) 2006-2020 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
```

```
## Loading required package: foreach
```

```
## Loading required package: iterators
```

```
## randomForest 4.6-14
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:rattle':
## 
##     importance
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

## Exploration and Preprocessing  
Next, we create training and test subsets from the training data. The metadata (first seven columns) are removed from both sets. Columns with mostly NAs as well as ones with near zero variance are also removed. We also set the seed to make the work reproducible. Below we see the new dimensions of out training and test subsets respectively.  

```
## [1] 11776    53
```

```
## [1] 7846   53
```

## Classification Tree Model  
The first prediction model we will use is a decision/classification tree. Before we create our model, we set up a control using 3 fold cross-validation. This will be used in all of our models. Below, you can see a plot of our model. Then we predict the outcome for our test subset and create a confusion matrix, also seen below. This tells us that the accuracy rate is only 0.49, so out-of-sample error is about 51% (yikes).  
![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1997  635  642  585  192
##          B   38  503   44  244  180
##          C  163  380  682  457  396
##          D    0    0    0    0    0
##          E   34    0    0    0  674
## 
## Overall Statistics
##                                           
##                Accuracy : 0.4915          
##                  95% CI : (0.4803, 0.5026)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.3357          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.8947  0.33136  0.49854   0.0000  0.46741
## Specificity            0.6341  0.92004  0.78450   1.0000  0.99469
## Pos Pred Value         0.4930  0.49851  0.32820      NaN  0.95198
## Neg Pred Value         0.9381  0.85154  0.88107   0.8361  0.89241
## Prevalence             0.2845  0.19347  0.17436   0.1639  0.18379
## Detection Rate         0.2545  0.06411  0.08692   0.0000  0.08590
## Detection Prevalence   0.5163  0.12860  0.26485   0.0000  0.09024
## Balanced Accuracy      0.7644  0.62570  0.64152   0.5000  0.73105
```

## Random Forest Model  
Our previous model does not seem to be a very good fit. Let's now try a random forest. You can see from our confusion matrix of predicted values vs. the test data that the accuracy of this model is about 0.99 and out-of-sample error is around 1%, a large improvement on the classification tree model.  

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2232   12    0    0    1
##          B    0 1505    8    0    0
##          C    0    1 1359   20    5
##          D    0    0    1 1264    5
##          E    0    0    0    2 1431
## 
## Overall Statistics
##                                           
##                Accuracy : 0.993           
##                  95% CI : (0.9909, 0.9947)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9911          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   0.9914   0.9934   0.9829   0.9924
## Specificity            0.9977   0.9987   0.9960   0.9991   0.9997
## Pos Pred Value         0.9942   0.9947   0.9812   0.9953   0.9986
## Neg Pred Value         1.0000   0.9979   0.9986   0.9967   0.9983
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2845   0.1918   0.1732   0.1611   0.1824
## Detection Prevalence   0.2861   0.1928   0.1765   0.1619   0.1826
## Balanced Accuracy      0.9988   0.9951   0.9947   0.9910   0.9960
```

## Generalized Boosted Regression Model  
We'll fit one last model to see if we can improve our accuracy. From our gbm fit, we get the confusion matrix below. The test set accuracy is approximately 0.96 and out-of-sample error is about 4%, which is alright but not as good as our random forest.  

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2199   39    0    0    3
##          B   21 1442   44    3   24
##          C    8   32 1302   38   16
##          D    3    2   22 1227   22
##          E    1    3    0   18 1377
## 
## Overall Statistics
##                                          
##                Accuracy : 0.9619         
##                  95% CI : (0.9574, 0.966)
##     No Information Rate : 0.2845         
##     P-Value [Acc > NIR] : < 2.2e-16      
##                                          
##                   Kappa : 0.9518         
##                                          
##  Mcnemar's Test P-Value : 1.658e-08      
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9852   0.9499   0.9518   0.9541   0.9549
## Specificity            0.9925   0.9855   0.9855   0.9925   0.9966
## Pos Pred Value         0.9813   0.9400   0.9327   0.9616   0.9843
## Neg Pred Value         0.9941   0.9880   0.9898   0.9910   0.9899
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2803   0.1838   0.1659   0.1564   0.1755
## Detection Prevalence   0.2856   0.1955   0.1779   0.1626   0.1783
## Balanced Accuracy      0.9889   0.9677   0.9686   0.9733   0.9757
```

## Applying Most Accurate to Testing Set  
Now we predict the outcomes for the testing dataset we initially loaded using the random forest model. Below you will see the predictions for these twenty rows.    

```
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
```
