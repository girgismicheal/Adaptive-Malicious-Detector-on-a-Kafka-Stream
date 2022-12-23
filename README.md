# Adaptive-Malicious-Detector-on-a-Kafka-Stream
## OverView
Soon...

## Dataset
Soon...

## Methodology
Soon...

## Installation instructions:

To achieve the real time data delivery, I used Kafka and Dockers to 
simulate an independent server which will send information.

![im1](Image/Screenshot_1.png)

1. To install the local Docker and Docker Compose programs on our 
computers. Please use the following link to download it just take into consideration your operative 
system. https://docs.docker.com/get-docker
2. Once your installation is complete, you will have the docker’s desktop interface as shown 
in the following image. It is not necessary to create an account or to perform any upgrade, 
with the free version we will be able to complete the assignment
3. Once it is fully installed download and unzip the folder provided that contains the scripts 
and the .csv with the data.

![im2](Image/Screenshot_2.png)

Once unzipped in your computer, run one of the following files depending on your 
operative system:
- If you are using windows then run the docker_script.bat file 
- Otherwise run the docker_script.sh file on Linux/MacOS

This file will create two docker images wurstmeister/kafka and wurstmeister/zookeeper. 
For this activity you don’t have to worry about what they are or how they run, as everything 
is already configured. At the end of the installation

To validate that the container images were created successfully open Dockers and you 
should see two images running like this

![im3](Image/Screenshot_3.png)

Now you are ready to capture the data stream and start the data treatment for your model. 

## EDA
### Validate data imbalanced with justifications
The dataset is slightly imbalanced as there is a difference of 10% between the false and true classes.

### Statistical analysis of data
Feature distributions

| Histograms               | Boxplots (for outliers and skewness) |
|--------------------------|--------------------------------------|
| ![](Image/Picture1.png)  | ![](Image/Picture2.png)              |

Skewness

| Right skew feature using KDE | Right skew feature using KDE | Statical skewness scores |
|------------------------------|------------------------------|--------------------------|
| ![](Image/Picture3.png)      | ![](Image/Picture4.png)      | ![](Image/Picture5.png)  |

- The highly negative score represents the left skew, and the positive represents the right skew. 


### Data Cleansing and Feature creation
I have found 8 missing values in the longest_word, I dropped them as letters so we would realize this feature is not important, so I decided to simplify the final pipeline by dropping the missing ones instead of using imputer.

| Check Nulls             | Check Unique            |
|-------------------------|-------------------------|
| ![](Image/Picture6.png) | ![](Image/Picture7.png) |

We have 3 object features:
- The timestamp would be dropped because by intuition it is not a time series problem
- longest word and sld would be encoded using feature hasher instead of one hot representation due to many unique values in both.

Finally, I applied MinMaxScaler to the dataset to keep the distribution as its but in a unified range, I didn’t use the stander scaler as chi-square requires non-negative ranged variables.




## Work on the static model:

### Feature Filtering

| Chi-square test         | Mutual Info test           |
|-------------------------|----------------------------|
| ![](Image/Picture8.png) | ![](Image/Picture9.png)    |
|Chi-square test scores indicate to have 6 important features|     Mutual Info test scores fail to have a clear indication of the most important feature as the scores are so near                       |

### Feature selection using tree-based models

| Check Nulls              | Check Unique             |
|--------------------------|--------------------------|
| ![](Image/Picture10.png) | ![](Image/Picture11.png) |

-	The final set of features: I would select 6 out of the 20 features, which include the highest significant 6 feature in chi-square, as they include the most important features for both the random forest and XGBoost algorithms feature importance.
Compare the models before and after feature selection
 
-	As shown in the below comparison, the training time and inference time were reduced while the metric are almost the same after the feature selection so I selected the reduced features.

**Compare the models before and after feature selection**

| Classification Metric    | Timing Metric            |
|--------------------------|--------------------------|
| ![](Image/Picture12.png) | ![](Image/Picture15.png) |
| ![](Image/Picture14.png) | ![](Image/Picture16.png) |
| ![](Image/Picture13.png) |                          |


### Data Splitting and justification
I would split the dataset into train, validation, and test by 70%, 15%, and 15% percentage respectively
-	Train: To fit the normalizer (Scaler) and train the models using the cross-validation method on this set to evaluate the model’s stability and robustness accurately and efficiently without data leakage.
-	Test: To evaluate and compare the final models.
-	Validation: To select the features and apply the hyperparameter tuning on the models.

### Choose and justify the correct performance metric
I have chosen the F1-Score as a metric to handle the small imbalance in the data while compromising between precision and recall as both are important in this problem, I don’t need to catch all the hacks while having a lot of false alarms (False Positive), and I don’t need to miss a lot of attacks while trying not to produce false alarms so I have chosen F1-score compromise between them.
Hyperparameter tuning is correct and clear
-	I used the grid search technique to select the best hyperparameters for the models, which are Random Forest and XGBoost to select the best n_estimators and max_depth that achieve the highest f1-score over the validation set to avoid data leakage.

### Compare and describe the two models you will use
From the three graphs below:
-	F1-score, Precision, and Recall score for XGBoost and Random Forest are almost the same
-	The random forest takes less time for training than XGBoost.
-	The XGBoost takes less time for inference than the random forest.

### Plot the models’ results

| Right skew feature using KDE  | Right skew feature using KDE  | Statical skewness scores |
|-------------------------------|-------------------------------|--------------------------|
| ![](Image/Picture17.png)      | ![](Image/Picture18.png)      | ![](Image/Picture19.png) |

### Discuss and analyze the results
So, for static models, it's better to choose the XGBoost as it's faster at inference time and only trains once, but for the dynamic model, it's better to choose the random forest as it has less training time, especially as the training set increases with time and require multiple training iterations.



