# ML-Capstone-Project
Capstone Project: Gender Classification Using Dentistry Data
Table of Contents
Introduction
Data Preprocessing
Exploratory Data Analysis (EDA)
Model Building
Model Evaluation
Conclusion
Future Work
1. Introduction
This capstone project aims to build and evaluate machine learning models to classify the gender of individuals based on various dentistry measurements. The dataset contains columns such as Sample ID, Age, Gender, intercanine distance intraoral, intercanine distance casts, right canine width intraoral, right canine width casts, left canine width intraoral, left canine width casts, right canine index intraoral, right canine index casts, left canine index intraoral, and left canine index casts.

2. Data Preprocessing
Step 1: Install Required Packages
First, ensure that all necessary packages are installed. Specifically, we need to install xgboost if it is not already installed.

python
Copy code
!pip install xgboost
Step 2: Import Necessary Packages
We import all the required packages for data manipulation, visualization, and modeling.

python
Copy code
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import LabelEncoder, Normalizer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from xgboost import XGBClassifier
from sklearn.metrics import confusion_matrix, roc_curve, auc, classification_report
Step 3: Load Dataset
We load the dataset from a CSV file into a pandas DataFrame.

python
Copy code
# Replace 'Dentistry.csv' with the path to your dataset file
dataset = pd.read_csv('Dentistry.csv')
Step 4: Handle Missing Values
We handle any missing values in the dataset by using forward fill.

python
Copy code
dataset = dataset.ffill()
Step 5: Encode Categorical Data
We encode the categorical variable 'Gender' using LabelEncoder.

python
Copy code
le = LabelEncoder()
dataset['Gender'] = le.fit_transform(dataset['Gender'])
Step 6: Split Independent and Dependent Variables
We split the dataset into independent variables (X) and the dependent variable (y).

python
Copy code
X = dataset.drop(['Gender', 'Sample ID', 'Sl No'], axis=1)
y = dataset['Gender']
Step 7: Normalize Data
We normalize the independent variables using the Normalizer.

python
Copy code
normalizer = Normalizer()
X = normalizer.fit_transform(X)
3. Exploratory Data Analysis (EDA)
Correlation Heatmap
We visualize the correlation between different features in the dataset using a heatmap.

python
Copy code
plt.figure(figsize=(12, 10))
sns.heatmap(dataset.corr(), annot=True, cmap='coolwarm')
plt.show()
4. Model Building
Split Data into Training and Testing Sets
We split the data into training and testing sets.

python
Copy code
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
Initialize Models
We initialize various machine learning models for classification.

python
Copy code
log_reg = LogisticRegression()
dt_clf = DecisionTreeClassifier()
rf_clf = RandomForestClassifier()
xgb_clf = XGBClassifier()
Train Models
We train each model on the training data.

python
Copy code
log_reg.fit(X_train, y_train)
dt_clf.fit(X_train, y_train)
rf_clf.fit(X_train, y_train)
xgb_clf.fit(X_train, y_train)
5. Model Evaluation
Evaluate Models
We evaluate the performance of each model using classification metrics, confusion matrix, and ROC curves.

python
Copy code
models = {'Logistic Regression': log_reg, 'Decision Tree': dt_clf, 'Random Forest': rf_clf, 'XGBoost': xgb_clf}

for model_name, model in models.items():
    print(f"Evaluating {model_name}")
    y_pred = model.predict(X_test)
    print(classification_report(y_test, y_pred))
    cm = confusion_matrix(y_test, y_pred)
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
    plt.title(f"{model_name} Confusion Matrix")
    plt.show()
    
    # ROC and AUC
    y_prob = model.predict_proba(X_test)[:, 1]
    fpr, tpr, thresholds = roc_curve(y_test, y_prob)
    roc_auc = auc(fpr, tpr)
    plt.plot(fpr, tpr, label=f'{model_name} (AUC = {roc_auc:.2f})')
    
plt.plot([0, 1], [0, 1], 'k--')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
plt.legend(loc='lower right')
plt.show()
6. Conclusion
We summarize the findings of our model evaluations and compare the performance of different models.

7. Future Work
We suggest potential improvements and future directions for this project, such as:

Collecting more data to improve model accuracy.
Exploring other machine learning algorithms.
Fine-tuning hyperparameters for better model performance.
This documentation covers the entire process of loading, preprocessing, analyzing, building, and evaluating models for your dataset. You can expand on each section with more details, observations, and insights specific to your project.






