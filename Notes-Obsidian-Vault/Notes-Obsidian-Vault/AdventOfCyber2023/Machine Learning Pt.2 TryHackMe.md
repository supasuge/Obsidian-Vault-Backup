## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Libraries](#Libraries)
    - [Step 1: Data Collection](#Step\1:\Data\Collection)
          - [DataFrames](#DataFrames)
          - [Expected Output](#Expected\Output)
- [Step 2: Data Preprocessing](#step\2:\data\preprocessing)
    - [CountVectorizer()](#CountVectorizer())

## Table of Contents

          - [Libraries](#Libraries)
    - [Step 1: Data Collection](#Step\1:\Data\Collection)
          - [DataFrames](#DataFrames)
          - [Expected Output](#Expected\Output)
- [Step 2: Data Preprocessing](#step\2:\data\preprocessing)
    - [CountVectorizer()](#CountVectorizer())


###### Libraries
```python
import numpy as np
import pandas as pd
```

### Step 1: Data Collection
Process of gathering raw data from various sources to be used for machine learning. This data can originate from numerous sources, such as databases, text files, API's, online repositories, sensors, surveys, web scraping, and many others

Here, we are using the *pandas* library to load the data collected from the `csv` file format.
```python
data = pd.read_csv("emails_dataset.csv")
```

###### DataFrames
Provide a structured and tabular representation of data that's intuitive and easy to read. Using the command below, let's use the pandas library to convert the data into a frame. It will make the data easy to analyse and manipulate.

###### Expected Output

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


### CountVectorizer()
Machine Learning models understand numbers.... So test must be transformned into a numerical format. `CountVectorizer`, a class provided from `scikit-learn` ahcieves this  by converting text into a token (word) Count matrix.