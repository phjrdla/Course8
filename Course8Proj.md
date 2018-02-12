---
title: "Course8ML"
author: "P.Briens"
date: "February 11, 2018"
output: 
  html_document:
    keep_md: true
---





# Purpose
Fitness wearables can be used to track physical activity as they provide plenty of data originating from their accelerometers and sensors. 
In this study one is using a dataset (thanks to the HAR project http://groupware.les.inf.puc-rio.br/har ) with 5 classes (sitting-down, standing-up, standing, walking, and sitting) collected from several people working out in a controlled way.
Once an acceptable prediction model is identified/obtained it will be applied to 20 different test cases to figure out which activities were performed.

# Load training and test cases data (from working directory)

```r
training_raw <- read.csv("pml-training.csv", stringsAsFactors=F, na.strings = c("", "NA", "#DIV/0!"))
testcases_raw  <- read.csv("pml-testing.csv", stringsAsFactors=F, na.strings = c("", "NA", "#DIV/0!"))
names(training_raw)
```

```
##   [1] "X"                        "user_name"               
##   [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
##   [5] "cvtd_timestamp"           "new_window"              
##   [7] "num_window"               "roll_belt"               
##   [9] "pitch_belt"               "yaw_belt"                
##  [11] "total_accel_belt"         "kurtosis_roll_belt"      
##  [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
##  [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
##  [17] "skewness_yaw_belt"        "max_roll_belt"           
##  [19] "max_picth_belt"           "max_yaw_belt"            
##  [21] "min_roll_belt"            "min_pitch_belt"          
##  [23] "min_yaw_belt"             "amplitude_roll_belt"     
##  [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
##  [27] "var_total_accel_belt"     "avg_roll_belt"           
##  [29] "stddev_roll_belt"         "var_roll_belt"           
##  [31] "avg_pitch_belt"           "stddev_pitch_belt"       
##  [33] "var_pitch_belt"           "avg_yaw_belt"            
##  [35] "stddev_yaw_belt"          "var_yaw_belt"            
##  [37] "gyros_belt_x"             "gyros_belt_y"            
##  [39] "gyros_belt_z"             "accel_belt_x"            
##  [41] "accel_belt_y"             "accel_belt_z"            
##  [43] "magnet_belt_x"            "magnet_belt_y"           
##  [45] "magnet_belt_z"            "roll_arm"                
##  [47] "pitch_arm"                "yaw_arm"                 
##  [49] "total_accel_arm"          "var_accel_arm"           
##  [51] "avg_roll_arm"             "stddev_roll_arm"         
##  [53] "var_roll_arm"             "avg_pitch_arm"           
##  [55] "stddev_pitch_arm"         "var_pitch_arm"           
##  [57] "avg_yaw_arm"              "stddev_yaw_arm"          
##  [59] "var_yaw_arm"              "gyros_arm_x"             
##  [61] "gyros_arm_y"              "gyros_arm_z"             
##  [63] "accel_arm_x"              "accel_arm_y"             
##  [65] "accel_arm_z"              "magnet_arm_x"            
##  [67] "magnet_arm_y"             "magnet_arm_z"            
##  [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
##  [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
##  [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
##  [75] "max_roll_arm"             "max_picth_arm"           
##  [77] "max_yaw_arm"              "min_roll_arm"            
##  [79] "min_pitch_arm"            "min_yaw_arm"             
##  [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
##  [83] "amplitude_yaw_arm"        "roll_dumbbell"           
##  [85] "pitch_dumbbell"           "yaw_dumbbell"            
##  [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
##  [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
##  [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
##  [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
##  [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
##  [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
##  [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
## [101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
## [103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
## [105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
## [107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
## [109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
## [111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
## [113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
## [115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
## [117] "accel_dumbbell_y"         "accel_dumbbell_z"        
## [119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
## [121] "magnet_dumbbell_z"        "roll_forearm"            
## [123] "pitch_forearm"            "yaw_forearm"             
## [125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
## [127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
## [129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
## [131] "max_roll_forearm"         "max_picth_forearm"       
## [133] "max_yaw_forearm"          "min_roll_forearm"        
## [135] "min_pitch_forearm"        "min_yaw_forearm"         
## [137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
## [139] "amplitude_yaw_forearm"    "total_accel_forearm"     
## [141] "var_accel_forearm"        "avg_roll_forearm"        
## [143] "stddev_roll_forearm"      "var_roll_forearm"        
## [145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
## [147] "var_pitch_forearm"        "avg_yaw_forearm"         
## [149] "stddev_yaw_forearm"       "var_yaw_forearm"         
## [151] "gyros_forearm_x"          "gyros_forearm_y"         
## [153] "gyros_forearm_z"          "accel_forearm_x"         
## [155] "accel_forearm_y"          "accel_forearm_z"         
## [157] "magnet_forearm_x"         "magnet_forearm_y"        
## [159] "magnet_forearm_z"         "classe"
```

```r
names(testcases_raw)
```

```
##   [1] "X"                        "user_name"               
##   [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
##   [5] "cvtd_timestamp"           "new_window"              
##   [7] "num_window"               "roll_belt"               
##   [9] "pitch_belt"               "yaw_belt"                
##  [11] "total_accel_belt"         "kurtosis_roll_belt"      
##  [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
##  [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
##  [17] "skewness_yaw_belt"        "max_roll_belt"           
##  [19] "max_picth_belt"           "max_yaw_belt"            
##  [21] "min_roll_belt"            "min_pitch_belt"          
##  [23] "min_yaw_belt"             "amplitude_roll_belt"     
##  [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
##  [27] "var_total_accel_belt"     "avg_roll_belt"           
##  [29] "stddev_roll_belt"         "var_roll_belt"           
##  [31] "avg_pitch_belt"           "stddev_pitch_belt"       
##  [33] "var_pitch_belt"           "avg_yaw_belt"            
##  [35] "stddev_yaw_belt"          "var_yaw_belt"            
##  [37] "gyros_belt_x"             "gyros_belt_y"            
##  [39] "gyros_belt_z"             "accel_belt_x"            
##  [41] "accel_belt_y"             "accel_belt_z"            
##  [43] "magnet_belt_x"            "magnet_belt_y"           
##  [45] "magnet_belt_z"            "roll_arm"                
##  [47] "pitch_arm"                "yaw_arm"                 
##  [49] "total_accel_arm"          "var_accel_arm"           
##  [51] "avg_roll_arm"             "stddev_roll_arm"         
##  [53] "var_roll_arm"             "avg_pitch_arm"           
##  [55] "stddev_pitch_arm"         "var_pitch_arm"           
##  [57] "avg_yaw_arm"              "stddev_yaw_arm"          
##  [59] "var_yaw_arm"              "gyros_arm_x"             
##  [61] "gyros_arm_y"              "gyros_arm_z"             
##  [63] "accel_arm_x"              "accel_arm_y"             
##  [65] "accel_arm_z"              "magnet_arm_x"            
##  [67] "magnet_arm_y"             "magnet_arm_z"            
##  [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
##  [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
##  [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
##  [75] "max_roll_arm"             "max_picth_arm"           
##  [77] "max_yaw_arm"              "min_roll_arm"            
##  [79] "min_pitch_arm"            "min_yaw_arm"             
##  [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
##  [83] "amplitude_yaw_arm"        "roll_dumbbell"           
##  [85] "pitch_dumbbell"           "yaw_dumbbell"            
##  [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
##  [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
##  [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
##  [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
##  [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
##  [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
##  [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
## [101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
## [103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
## [105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
## [107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
## [109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
## [111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
## [113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
## [115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
## [117] "accel_dumbbell_y"         "accel_dumbbell_z"        
## [119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
## [121] "magnet_dumbbell_z"        "roll_forearm"            
## [123] "pitch_forearm"            "yaw_forearm"             
## [125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
## [127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
## [129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
## [131] "max_roll_forearm"         "max_picth_forearm"       
## [133] "max_yaw_forearm"          "min_roll_forearm"        
## [135] "min_pitch_forearm"        "min_yaw_forearm"         
## [137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
## [139] "amplitude_yaw_forearm"    "total_accel_forearm"     
## [141] "var_accel_forearm"        "avg_roll_forearm"        
## [143] "stddev_roll_forearm"      "var_roll_forearm"        
## [145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
## [147] "var_pitch_forearm"        "avg_yaw_forearm"         
## [149] "stddev_yaw_forearm"       "var_yaw_forearm"         
## [151] "gyros_forearm_x"          "gyros_forearm_y"         
## [153] "gyros_forearm_z"          "accel_forearm_x"         
## [155] "accel_forearm_y"          "accel_forearm_z"         
## [157] "magnet_forearm_x"         "magnet_forearm_y"        
## [159] "magnet_forearm_z"         "problem_id"
```

# Quick summary for training dataset
1. Number of observations and variables
2. Number of observations / classe
3. Propertion of observations / classe
4. Number of observations / person / classe

```r
dim(training_raw)
```

```
## [1] 19622   160
```

```r
table(training_raw$classe)
```

```
## 
##    A    B    C    D    E 
## 5580 3797 3422 3216 3607
```

```r
prop.table(table(training_raw$classe))
```

```
## 
##         A         B         C         D         E 
## 0.2843747 0.1935073 0.1743961 0.1638977 0.1838243
```

```r
table(training_raw$user_name,training_raw$classe )
```

```
##           
##               A    B    C    D    E
##   adelmo   1165  776  750  515  686
##   carlitos  834  690  493  486  609
##   charles   899  745  539  642  711
##   eurico    865  592  489  582  542
##   jeremy   1177  489  652  522  562
##   pedro     640  505  499  469  497
```

# Data pre-processing
All calculated variables such as average, standard deviation, min, max, total etc ... and variables not useful for prediction are deleted.

## remove columns
data frames training_df and testcases_df created for training and testcases data

```r
cols2remove <- "X|user_name|cvtd_timestamp|problem_id|var|raw|window|kurtosis|skewness|stddev|avg|total|max|min|problem_id"
cols2keep <- grep (cols2remove, names(training_raw),invert=TRUE)
training_df <- training_raw[, cols2keep]
cols2keep <- grep (cols2remove, names(testcases_raw),invert=TRUE)
testcases_df <- testcases_raw[, cols2keep]
```

## remove  columns with no values
1. reduced list of variables for training data 
  + classe is our model response variables
  + predictors will be found from all the remaining variables
2. reduced list of variables for test cases data

```r
training_df <- training_df[, colSums(is.na(training_df)) == 0]
names(training_df)
```

```
##  [1] "roll_belt"         "pitch_belt"        "yaw_belt"         
##  [4] "gyros_belt_x"      "gyros_belt_y"      "gyros_belt_z"     
##  [7] "accel_belt_x"      "accel_belt_y"      "accel_belt_z"     
## [10] "magnet_belt_x"     "magnet_belt_y"     "magnet_belt_z"    
## [13] "roll_arm"          "pitch_arm"         "yaw_arm"          
## [16] "gyros_arm_x"       "gyros_arm_y"       "gyros_arm_z"      
## [19] "accel_arm_x"       "accel_arm_y"       "accel_arm_z"      
## [22] "magnet_arm_x"      "magnet_arm_y"      "magnet_arm_z"     
## [25] "roll_dumbbell"     "pitch_dumbbell"    "yaw_dumbbell"     
## [28] "gyros_dumbbell_x"  "gyros_dumbbell_y"  "gyros_dumbbell_z" 
## [31] "accel_dumbbell_x"  "accel_dumbbell_y"  "accel_dumbbell_z" 
## [34] "magnet_dumbbell_x" "magnet_dumbbell_y" "magnet_dumbbell_z"
## [37] "roll_forearm"      "pitch_forearm"     "yaw_forearm"      
## [40] "gyros_forearm_x"   "gyros_forearm_y"   "gyros_forearm_z"  
## [43] "accel_forearm_x"   "accel_forearm_y"   "accel_forearm_z"  
## [46] "magnet_forearm_x"  "magnet_forearm_y"  "magnet_forearm_z" 
## [49] "classe"
```

```r
testcases_df <- testcases_df[, colSums(is.na(testcases_df)) == 0]
```

# Basic exploratory analysis
Scatter and density distribution  plots are created for a subset of predictor variables typical of the training dataset.  
It appears that density distribution are not normal and scatter plots display a mix of complex clusters and others with no evident pattern.
![](Course8Proj_files/figure-html/exploplots-1.png)<!-- -->

# Prediction training and test sets preparation
Data frame training_df is used for model training and testing.  

* 80% of the dataset is allocated to model training, the 20% remaining to model testing.
    + observations for model training
    + observations for model testing


```r
set.seed(12345)
inTrain <- createDataPartition(training_df$classe, p=0.80, list=F)
trainDat <- training_df[inTrain, ]
testDat <- training_df[-inTrain, ]
dim(trainDat)
```

```
## [1] 15699    49
```

```r
dim(testDat)
```

```
## [1] 3923   49
```

# Modelling
"caret" train package is used with random forest classification model limited to 250 trees and variable importance evaluation.  
"train" package is run in parallel.  


```r
cl <- makeCluster(detectCores())
registerDoParallel(cl)
model_rf <- train(classe ~.,data=trainDat,method="rf", importance=TRUE, ntree=250)
stopCluster(cl)
```

## Random forest model details
+ Model details
+ Confusion matrix
+ Variables importance and plot


```r
model_rf
```

```
## Random Forest 
## 
## 15699 samples
##    48 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## Summary of sample sizes: 15699, 15699, 15699, 15699, 15699, 15699, ... 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy   Kappa    
##    2    0.9911335  0.9887805
##   25    0.9903780  0.9878240
##   48    0.9834132  0.9790096
## 
## Accuracy was used to select the optimal model using the largest value.
## The final value used for the model was mtry = 2.
```

```r
rfImp <- varImp(model_rf, scale=F)
rfImp
```

```
## rf variable importance
## 
##   variables are sorted by maximum importance across the classes
##   only 20 most important variables shown (out of 48)
## 
##                       A     B     C     D     E
## yaw_belt          24.61 28.71 23.10 27.59 20.71
## pitch_belt        21.09 28.59 22.09 23.18 19.52
## roll_belt         22.24 25.89 27.30 27.42 21.10
## magnet_dumbbell_z 24.24 21.48 22.53 20.28 21.11
## magnet_dumbbell_y 19.38 21.84 23.94 19.91 18.80
## pitch_forearm     17.43 23.05 21.19 21.55 21.26
## accel_dumbbell_y  19.13 21.52 22.56 22.79 20.34
## roll_arm          19.36 22.61 22.17 21.49 19.63
## magnet_dumbbell_x 16.69 18.19 22.57 18.21 17.05
## yaw_arm           15.00 21.63 17.88 19.64 17.68
## accel_forearm_z   18.15 20.94 21.24 19.98 21.62
## magnet_belt_z     20.46 20.23 21.29 21.31 19.36
## gyros_arm_y       14.78 21.28 19.45 18.71 17.96
## accel_dumbbell_z  17.51 21.01 21.11 19.53 18.52
## gyros_dumbbell_y  16.59 19.83 20.90 17.58 16.09
## gyros_belt_z      17.45 20.82 17.02 18.23 19.63
## roll_dumbbell     15.46 17.98 20.67 17.95 17.23
## magnet_forearm_z  17.22 20.49 19.43 18.65 20.46
## magnet_belt_y     18.47 20.26 19.72 19.06 19.41
## gyros_dumbbell_z  14.87 19.77 17.61 17.46 17.24
```

```r
plot(rfImp,top=15)
```

![](Course8Proj_files/figure-html/details-1.png)<!-- -->

## Testing model accuracy
+ Confusion matrix predictions vs test data
+ Model accuracy is above 99%


```r
pred_rf <- predict(model_rf, testDat)
confusionMatrix(pred_rf, testDat$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1116    5    0    0    0
##          B    0  753    9    0    0
##          C    0    1  675   13    0
##          D    0    0    0  630    2
##          E    0    0    0    0  719
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9924          
##                  95% CI : (0.9891, 0.9948)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9903          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   0.9921   0.9868   0.9798   0.9972
## Specificity            0.9982   0.9972   0.9957   0.9994   1.0000
## Pos Pred Value         0.9955   0.9882   0.9797   0.9968   1.0000
## Neg Pred Value         1.0000   0.9981   0.9972   0.9960   0.9994
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2845   0.1919   0.1721   0.1606   0.1833
## Detection Prevalence   0.2858   0.1942   0.1756   0.1611   0.1833
## Balanced Accuracy      0.9991   0.9946   0.9913   0.9896   0.9986
```

```r
confusionMatrix(pred_rf, testDat$classe)$overall[1]
```

```
##  Accuracy 
## 0.9923528
```

# Application to test cases
The random forest model evaluated in "Testing model accuracy" is applied to the 20 test cases.    
Test cases predicted Classes are listed.  


```r
dim(testcases_df)
```

```
## [1] 20 48
```

```r
pred_20 <- predict(model_rf, testcases_df)
pred_20
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```
