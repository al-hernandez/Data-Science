# **Title: Predicting the Functionality of Water Pumps in Tanzania**
Based on data from the Tanzania Ministry of Water, the objective of this project is to identify the factors that influence water pump functionality, and predict which water-points will fail so that maintenance operations can be improved.

Classification using supervised learning will be used to predict the functionality of water pumps. Additionally, data visualization will be employed to understand the factors that contribute to water pump failure.

<img width="600" alt="Tableau Map" src="https://user-images.githubusercontent.com/112843657/223010365-e55cfd20-5107-41f5-9787-ac150e2411bb.png">


# Dataset
### **Source:** 
Two datasets were obtained from *DrivenData*, an organization that works to engage data scientists with social challenges.
### **Description:** 
The first dataset, *WaterPumpObservations.csv*, contains 59,400 entries and 40 attributes, including a water pump’s funder, installer, subvillage, year of construction, and water quality. The second dataset, *WaterPumps.csv* matches each pump with an ID, and classifies each pump as either functional, functional but needs repair, and non functional.

# Data Cleaning
### **Overview:** 
Most of the variables in the dataset are categorical. Additionally, several features have high cardinality, meaning that a feature can have a wide range of values. The dataset had seven columns with missing values. Only one of these columns had 50% of missing values, while the rest were missing less than 10% of data. The dataset also contained several redundant features.
### **Approach:** 
- To eliminate redundancy, columns containing the same information were reduced to one.
- Since high cardinality makes one hot encoding impractical to use, a CatBoost encoder was used, which encodes categorical features according to a the target value. 
- A KNN imputer was used to address missing values. Prior to imputation, the dataset was scaled by means of a StandardScaler

# Exploratory Data Analysis
### **Notable Relationships between Features and Water Pump Functionality**
1. Payment
<img width="600" alt="Screenshot 2023-02-24 at 10 04 13 PM" src="https://user-images.githubusercontent.com/112843657/221339406-d29bedbc-c5df-4771-a3e1-81e44f2c85c3.png">

Payment for a service (in this case access to water) leads to a more dependable water pump.

2. Extraction Type
<img width="600" alt="Extraction" src="https://user-images.githubusercontent.com/112843657/221339508-61a2eab4-9998-42d6-b9ba-8a722ebd3342.png">

Water pumps that do not extract water via a hand-pump, gravity, or a submersible mechanism tend to fail.


3. Basin
<img width="600" alt="Basin" src="https://user-images.githubusercontent.com/112843657/221339772-b0abe494-aa06-451c-95ef-3957462cefc5.png">

Lake Rukwa and the Southern Coast of Tanzania are non-optimal sources of water since there is a more than half of the water pumps fed by these basins are either non functional, or in need of repair.


4. Installer

*Functional pumps*

<img width="283" alt="finstaller" src="https://user-images.githubusercontent.com/112843657/223009267-f7a6fd69-28ac-42c5-a72e-e5f43ec49c4f.png">

*Non-Functional*

<img width="403" alt="nfinstaller" src="https://user-images.githubusercontent.com/112843657/223009377-9e60921e-6d19-4d45-b3d2-6095f955fad0.png">

Pumps installed by the Tanzanian government are prone to failure.


# Classification Using Supervised Machine Learning
### **Approach:** 
Multi-class classification was accomplished by implementing linear, non-linear, and ensemble methods. A KNN Classifier and a Random Forest Model achieved the highest accuracy score.

**Linear & Non-linear Models**

<img width="421" alt="KNN" src="https://user-images.githubusercontent.com/112843657/221341879-8a8a5350-daea-440a-937d-68cabd61a399.png">

**Ensemble Methods**

<img width="415" alt="RF" src="https://user-images.githubusercontent.com/112843657/221342516-68ee3657-1c4c-4785-aa54-99483720e7c5.png">

### **Balancing the Dataset:**
The original dataset was imbalanced due to a discrepancy in the number of observations in each of the target classes. The "functional" target class had the highest number of observations, while the "functional but needs repair" class had the lowest. Therefore, a balanced dataset was created by taking a sample from each class that was equal to the size of the underrepresented class.

<img width="500" alt="Screenshot 2023-02-24 at 7 40 35 PM" src="https://user-images.githubusercontent.com/112843657/221344577-85123ddd-d219-4cb1-a758-adb04e9ccea7.png">

Before balancing the dataset, the true positive rate for the "functional but needs repair" class was equal to .35

<img width="600" alt="Unbalanced" src="https://user-images.githubusercontent.com/112843657/221343058-23863a68-e97d-418f-9bec-ac91dca7fdee.png">

After balacing the dataset, the true positive rate improved to .71

<img width="600" alt="Balanced" src="https://user-images.githubusercontent.com/112843657/221343086-baf2173d-0c47-45a9-800a-5c8fd22fcb64.png">

# Analysis & Findings
### **Feature Importance:**
<img width="600" alt="Features" src="https://user-images.githubusercontent.com/112843657/221343188-b579f512-7d95-4534-9dfc-2624d2b22f60.png">

Features Sorted by Importance

<img width="393" alt="featureImp" src="https://user-images.githubusercontent.com/112843657/221343313-511d3d54-8542-4014-ad0a-aa889c7d1058.png">

Feature importance was determined by using a Random Forest Regressor and a Correlation Matrix. The results returned by these methods are congruent with the observations obtained from exploratory data analysis: 

- Instances of water pump failure are more concentrated in certain subvillages

  <img width="200" alt="subvillage" src="https://user-images.githubusercontent.com/112843657/221344021-fe3e253e-69ac-4c7e-95a4-972b7bebbf4c.png">
  
- the type of water pump does indeed determine functionality, since those that rely on gravity, hand pumps, or a submersible mechanism tend to be more reliable.

- the year that a water pump was constructed is correlated with functionality. For non-functional pumps, a histogram of their year of construction shows that a great portion of them were built before the year 2000

  <img width="483" alt="nfconstruction" src="https://user-images.githubusercontent.com/112843657/221344423-3b7822f6-eb98-45e7-a3d9-fd37fa26ab25.png">

  On the other hand, functional water pumps were mostly built after the year 2000
  
  <img width="476" alt="fconstruction" src="https://user-images.githubusercontent.com/112843657/221344482-55456da4-48c2-4bf7-9ef0-ea438de38ce2.png">

# Summary:
-Most water pumps in Tanzania are functional, although there are wards and subvillages with a high concentration of non-functional pumps

-Factors that determine functionality include:
  - year of construction
  - basin
  - installer
  - extraction type
  - payment type
