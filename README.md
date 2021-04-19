# Credit-Card-Defaulter-Prediction

### Problem Statement 

To build a classification methodology to determine whether a person defaults the credit card payment for the next month.  

#### Language Used - Python
#### Deployed on - Heroku
#### Framework - Flask
#### Tools - PyCharm
#### Algorihtms - K-Mean Clustring, XG Boost Classifer and Navis Bayes Classifer
#### Accuracy Metric - AUC Score

### Architecture 

![Architeture](https://user-images.githubusercontent.com/66250589/115265089-8378f800-a154-11eb-8152-03f88a8e4cd4.PNG)


### Data Description 

The client will send data in multiple sets of files in batches at a given location. The data has been extracted from the census bureau.  

The data contains 32561 instances with the following attributes: 

#### Features: 

 LIMIT_BAL: continuous.Credit Limit of the person. 

SEX: Categorical: 1 = male; 2 = female 

EDUCATION: Categorical: 1 = graduate school; 2 = university; 3 = high school; 4 = others 

MARRIAGE: 1 = married; 2 = single; 3 = others 

AGE-num: continuous.  

PAY_0 to PAY_6: History of past payment. We tracked the past monthly payment records (from April to September, 2005) 

BILL_AMT1 to BILL_AMT6: Amount of bill statements. 

PAY_AMT1 to PAY_AMT6: Amount of previous payments. 

#### Target Label: 

Whether a person shall default in the credit card payment or not. 

default payment next month:  Yes = 1, No = 0.


### Data Validation

In this step, we perform different sets of validation on the given set of training files.

Name Validation- We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

Number of Columns - We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."

Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

The datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder".

Null values in columns - If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

### Data Insertion in Database

Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database.

Table creation in the database - Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training files.

Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".

### Model Training

Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.

#### Data Preprocessing

a) Check for null values in the columns. If present, impute the null values using the KNN imputer. b) Check if any column has zero standard deviation, remove such columns as they don't give any information during model training.

Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction.

#### Model Selection -

After clusters are created, we find the best model for each cluster. We are using two algorithms, "Navis Bayes Classifer" and "XGBoost Classifer". For each cluster, both the algorithms are passed with the best parameters derived from GridSearch. We calculate the AUC scores for both models and select the model with the best score. Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction.

### Prediction

Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.

Data Preprocessing

a) Check for null values in the columns. If present, impute the null values using the KNN imputer. b) Check if any column has zero standard deviation, remove such columns as we did in training.

Clustering - KMeans model created during training is loaded, and clusters for the preprocessed prediction data is predicted.

Prediction - Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.

Once the prediction is made for all the clusters, the predictions along with the Wafer names are saved in a CSV file at a given location and the location is returned to the client.

### Deployment

We will be deploying the model to the Heroku Cloud platform.

### Advantage of Heroku:

1) Beginner and start-up-friendly

2) It allows you to create a new server in just 10 seconds by using the interface of Heroku Command Line.

3) This cloud computing platform takes care of patching systems and keeping everything healthy.

4) A range of automated functionalities including the scaling, configuration, setup, and others

5) Easy integration with other AWS products.

### Output:

After compilation you can see this type of interface. Where you can give Custom files path for prediction or Default File prediction.

![credit card prediction](https://user-images.githubusercontent.com/66250589/115266041-76a8d400-a155-11eb-8d34-19cebe0a3932.PNG)

 ### Happy Learning!!!
