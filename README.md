# Stress Level Detection using IoT amd Machine Learning

## Modules of Stress Detector
1. Data acquistion
2. Data pre-processing
3. Training ML model
4. Testing Model
5. Model deployment

## Abstract

* Predicts stress level on a scale of 0 – 4

* Stress Levels 
    - 0 - No Stress
    - 1 - Slightly stressed
    - 2 - Stressed
    - 3 - Highly stressed

![image](https://user-images.githubusercontent.com/83827603/237031486-690073e5-b2c0-43d0-badd-3319bc68193e.png)

## Requirements

1. Arduino Uno board
2. ESP8266
3. MAX3010 Pulse and SPo2 sensor
4. SW-420 Vibration sensor module
5. puTTY
6. ThingSpeak
7. Jupyter Notebook

## Circuit Connections

* Connections for SPo2 sensor

    ![image](https://user-images.githubusercontent.com/83827603/237032133-802601e3-1bdd-4ca3-ad0a-df165405814a.png)

* Connections for Vibration sensor SW-420 Module

    ![image](https://user-images.githubusercontent.com/83827603/237032497-a02d79ea-8eaf-4b88-b518-6921370f6eb1.png)

## Data Acquisition

* MAX3010 sensor is connected to the Arduino to record the Heart rate (in bpm and the spO2 levels.
* Vibration sensor is used to record the limb movement, the process we employed to calculate the limb movement is that we recorded the number of vibrations in a certain interval of time ( for our case 10s ).
* We used ESP8266 to send limb movement data to ThingSpeak cloud.

## Preparing the Dataset

* We employed two different methods for loading sensor values in a csv.
* In case of vibration sensor module, we uploaded the data to ThingSpeak and imported the csv from there and hence we recorded the limb movement data.
* We used putty to load MAX3010 sensor values using serial communication.
* PuTTY is a tool for logging sensor values from a specified serial port at a particular baud rate and stores it in a printable output say csv file.

## Data Preprocessing

* Missing Values: No missing value was detected.
* Duplicates: No duplicate was detected
* Feature Extraction: Correlation among different features were studied for the same. The following have been concluded on visualizing the data and finding 
the correlation matrix.
* Stress Levels and Sleeping Hours are strongly negatively correlated. The more people sleep, the less they are likely to be stressed. negative correlation between Stress (categorical variable) and Number of Hours Slept (continuous variable).
    - People who sleep 8 hours on average in a single day report no or very low stress levels. It is detected that people with high limb movement have more stress.
    - Body temperature and stress level are highly negatively correlated.
    - Stressed people have higher heart rate, people with low or no stress have heart rate between 50-60
* Outliers detection: 
    - The seaborn plot for each of the feature has been visualized to detect any outlier present in each of the feature – heart rate and limb movement.
    - IQR method was used for limb movement feature
    - Outlier detection using box method for heart rate, body temperature, blood oxygen and sleeping hours.
    - No outlier was actually detected
* Data standardization: Standard Scaler is applied to the independent features of the dataset the bring normalize the feature values

## Training the Machine Learning Model

Different Supervised Classification Algorithms were tried:

* Support Vector Machine
* Decision Tree Regression
* Random Forest Regression
* K-Nearest Neighbors Algorithm
* Naive Bayes
* Logistic Regression

The algorithm Random Forest is used to predict the Stress Levels which gave the best accuracy of 99%.

## Imputation of Testing Data and Prediction

Algorithms tried for Imputation:

* Mice Algorithm
* Mean/Median
* Random Forest Regression
* Decision Tree Regression
* Polynomial Regression
* Support Vector Regression

The best final accuracy of Stress levels was provided by Decision Tree Regression Algorithm. Other methods gave accuracy of only about 24%. Mean/Median methods are only suitable when there are only a few missing values in a particular feature and not when all the values of the feature are supposed to be imputed
 
 ## Model Deployment
 
* Ml model is dumped into sav file using pickle library
* Web app using python library called streamlit which use sav file to predict the stress level on the basis of features entered by the user.
 
 ![image](https://user-images.githubusercontent.com/83827603/237034457-b5d18244-a8a5-4678-b916-f767743cad73.png)

 
