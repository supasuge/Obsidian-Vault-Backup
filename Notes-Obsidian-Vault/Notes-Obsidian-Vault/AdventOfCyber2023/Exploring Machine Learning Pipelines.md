## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Required Libraries](#Required\Libraries)
    - [Step 1: Data Collection](#Step\1:\Data\Collection)
- [Step 2: Data Preprocessing](#step\2:\data\preprocessing)
    - [**Utilizing CountVectorizer()**](#**Utilizing\CountVectorizer()**)
    - [Step 3: Train/Test Split dataset](#Step\3:\Train/Test\Split\dataset)
- [Step 4:  Model Training](#step\4:\ model\training)
  - [How Naive Bayes Classification Works](#How\Naive\Bayes\Classification\Works)

## Table of Contents

        - [Required Libraries](#Required\Libraries)
    - [Step 1: Data Collection](#Step\1:\Data\Collection)
- [Step 2: Data Preprocessing](#step\2:\data\preprocessing)
    - [**Utilizing CountVectorizer()**](#**Utilizing\CountVectorizer()**)
    - [Step 3: Train/Test Split dataset](#Step\3:\Train/Test\Split\dataset)
- [Step 4:  Model Training](#step\4:\ model\training)
  - [How Naive Bayes Classification Works](#How\Naive\Bayes\Classification\Works)

A machine learning pipeline refers to the series of steps involved invuilding and deploying an ML model.


A typical pipeline would include collecting data from different sources in different forms, preprocessing it and performing feature extraction from the data, splitting the data into testing and training data, and then applying Machine Learning models and predictions.![Shows Machine learning pipeline](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/2f038b15de04a5a21a2cbbed96b164f0.svg)


##### Required Libraries
```python
import numpy as np
import pandas as pd
```
### Step 1: Data Collection
**Data collection** is the process of gathering raw data from various sources to be used for Machine Learning.
- Databases
- Text files
- APIs
- Online repositories
- sensors
- surveys
- web scraping


# Step 2: Data Preprocessing

Data preprocessing refers to the techniques used to convert raw data into a clean, organised, understandable, and structured format suitable for Machine Learning. Given that raw data is often messy, inconsistent, and incomplete, preprocessing is an essential step to ensure that the data feeding into the ML models is relevant and of high quality. Here are some common techniques used in data preprocessing:

|Technique|Description|Use Cases|
|---|---|---|
|**Cleaning**|Correct errors, fill missing values, smooth noise, and handle outliers.|To ensure the quality and consistency of the data.|
|**Normalization**|Scaling numeric data into a uniform range, typically [0, 1] or [-1, 1].|When features have different scales and we want equal contribution from all features.|
|**Standardization**|Rescaling data to have a mean (μ) of 0 and a standard deviation (σ) of 1 (unit variance).|When we want to ensure that the variance is uniform across all features.|
|**Feature Extraction**|Transforming arbitrary data such as text or images into numerical features.|To reduce the dimensionality of data and make patterns more apparent to learning algorithms.|
|**Dimensionality Reduction**|Reducing the number of variables under consideration by obtaining a set of principal variables.|To reduce the computational cost and improve the model's performance by reducing noise.|
|**Discretization**|Transforming continuous variables into discrete ones.|To handle continuous variables and make the model more interpretable.|
|**Text Preprocessing**|Tokenization, stemming, lemmatization, etc., to convert text to a format usable for ML algorithms.|To process and structure text data before feeding it into text analysis models.|
|**Imputation**|Replacing missing values with statistical values such as mean, median, mode, or a constant.|To handle missing data and maintain the dataset’s integrity.|
|**Feature Engineering**|Creating new features or modifying existing ones to improve model performance.|To enhance the predictive power of the learning algorithms by creating features that capture more information.|


### **Utilizing CountVectorizer()**
Machine learning models obviously only understand numbers, so `CountVectorizer`  is a class provided by the `scikit-learn` library in python, this is achieved by converting text into a token (word) count matrix. It is used to prepare the data for the Machine Learning Models to use and predict decisions

Example:
```python
from sklearn.feature_extraction.text import CountVectorizer
vectorizer = CountVectorizer()
x = vectorizeer.fit_transform(df['Message'])
print(x)
```

Here, variable X contains the dataset. We will use the functions from the sklearn library to split the dataset into training data and testing data, as shown below:  
### Step 3: Train/Test Split dataset
By splitting the data, we can train the model on one subset and test its performance on another.
![[Pasted image 20231219050503.png]]
```python
from sklearn.model_selection import train_test_split
y = df['Classification']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

Here, variable X contains the dataset, the functions from the `sklearn` library below split the data into training data and testing data
```python
from sklearn.model_selection import train_test_split
y=df['Classification']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```
- `X`: The first argument to `train_test_split` is the feature matrix `X` which you obtained from the `CountVectorizer`. This matrix contains the token counts for each message in the dataset
- `y`: The second argument is the labels for each instance in your dataset, which indicates whether a message is spam or ham.

- **test_size=0.2**: This argument specifies that 20% of the dataset should be kept as the test set and the rest (80%) should be used for training. It's a common practice to hold out a portion of the dataset for testing to evaluate the performance of the model on unseen data. This is where the actual splitting of data into training and test sets happens.
    

The function then returns four values:

- `X_train`: The subset of the features to be used for training.
- `X_test`: The subset of the features to be used for testing.
- `y_train`: The corresponding labels for the X_train set.
- `y_test`: The corresponding labels for the X_test set


# Step 4:  Model Training  

Now that we have the dataset ready, the next step would be to choose the text classification model and use it to train on the given dataset. Some commonly used text classification models are explained below:  

|Model|Explanation|
|---|---|
|**Naive Bayes Classifier**|A probabilistic classifier based on Bayes’ Theorem with an assumption of independence between features. It’s particularly suited for high-dimensional text data.|
|**Support Vector Machine (SVM)**|A robust classifier that finds the optimal hyperplane to separate different classes in the feature space. Works well with non-linear and high-dimensional data when used with kernel functions.|
|**Logistic Regression**|A statistical model that uses a logistic function to model a binary dependent variable, in this case, spam or ham.|
|**Decision Trees**|A model that uses a tree-like graph of decisions and their possible consequences; it’s simple to understand but can overfit if not pruned properly.|
|**Random Forest**|An ensemble of decision trees, typically trained with the “bagging” method to improve the predictive accuracy and control overfitting.|
|**Gradient Boosting Machines (GBMs)**|An ensemble learning method is building strong predictive models in a stage-wise fashion; known for outperforming random forests if tuned correctly.|
|**K-Nearest Neighbors (KNN)**|A non-parametric method that classifies each data point based on the majority vote of its neighbors, with the data point being assigned to the class most common among its k nearest neighbors.|

## How Naive Bayes Classification Works
- Learn's from pre-labeled/classified data. I.e., it looks at the words in each email and calculates how frequently each word appears in spam or ham emails. For innstance, words like "free"...
- It calculatesd the probabilitiy of the email being spam based on the words it contains.












