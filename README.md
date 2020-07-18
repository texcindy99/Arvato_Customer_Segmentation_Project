### Table of Contents

1. [Installation](#installation)
2. [Project Motivation](#motivation)
3. [File Descriptions](#files)
4. [Methodology](#methodology)
5. [Results](#results)
6. [Discussion](#discussion)
7. [Licensing, Authors, and Acknowledgements](#licensing)

## Installation <a name="installation"></a>

To run this notebook, make sure to install xgboost using "! conda install -c conda-forge xgboost -y" as in the notebook if you don't have it in your working environment. Then the code should run with no issue using Python versions 3.*.

## Project Motivation <a name="motivation"></a>

This is my Udacity Capstone project. In the project, a mail-order sales company in Germany is interested in identifying segments of the general population to target with their marketing in order to grow. Demographics information has been provided for both the general population at large as well as for prior customers of the mail-order company in order to build a model of the customer base of the company. The target dataset contains demographics information for targets of a mailout marketing campaign. The objective is to identify which individuals are most likely to respond to the campaign and become customers of the mail-order company.

As part of the project, half of the mailout data has been provided with included response column. For the Kaggle competition, the remaining half of the mailout data has had its response column withheld; the competition will be scored based on the predictions on that half of the data.

I was interestested in using four data sets to:

1. better understand the characterization of general population and the customers of the mail-order sales company in Germany
2. do supervised learning on both the general population and the customers data using PCA and KMeans clustering
3. create and tune classifier to identify potential customers in the training data
4. predict on the test data

## File Descriptions <a name="files"></a>

There is one notebook containing all the steps described above.

There is one html file which is the html version of the notebook.

There is no data file available in this repo according to the Terms & Conditions associated with the project on Udacity. Below are the description of four data files used in the notebook:

- Udacity_AZDIAS_052018.csv: Demographics data for the general population of Germany; 891 211 persons (rows) x 366 features (columns).
- Udacity_CUSTOMERS_052018.csv: Demographics data for customers of a mail-order company; 191 652 persons (rows) x 369 features (columns).
- Udacity_MAILOUT_052018_TRAIN.csv: Demographics data for individuals who were targets of a marketing campaign; 42 982 persons (rows) x 367 (columns).
- Udacity_MAILOUT_052018_TEST.csv: Demographics data for individuals who were targets of a marketing campaign; 42 833 persons (rows) x 366 (columns).

You are welcome to use your own data to run the notebook.

## Methodology <a name="methodology"></a>

There are four steps to run this projects:

### 1. Explore the general population and customers data

- Check data size and data types
- Check NaN values
- Check numeric columns and category columns
- Check description of data attributes to find the best way to fill NaNs

Four columns are found to have more than 90% of NaNs and they can be dropped.

### 2. Customer Segmentation using unsupervised learning

- Run PCA on general population data to reduce features to half
- Run KMeans clustering on general population data with the optimal cluster number from elbow plot
- Repeat the same steps on customers data
- Compare the cluster count in general population data and customers data

The finding is the customers data are only popular in three clusters while the general population have more even distribution between eight clusters.

### 3. Identify customers in training data using supervised learning

- Run PCA on general population data to reduce features to half
- Do KMeans clustering for class 0 data and downsample in each cluster because the training data is highly imbalanced that size of class 1 is only 1% of class 0. 
- Fit the classifier with the downsampled training data
- Improve the classifier using grid search

### 4. Predict on test data
- Apply the same PCA on test data
- Use the classifier built in step 3 to predict on test data

## Results <a name="results"></a>

Different classifier such as LinearSVC and logistic regression are tested and finally XGBClassifier is chosen becasue it performs better on imbalanced data. Combining with downsampling, the final ROC score exceeds 0.8.

## Discussion <a name="discussion"></a>

Two challenges of this project is large data size and the data imbalance.

1. To deal with the large data which has about 400 features after clearning and digitalization, PCA is used to extract the principal components from the data. Using PCA, 90% of the data characterizations are perserved with only half of the data features. 

2. There are many methods to deal with imbalanced data in classification problem including resampling techniques and algorithmic ensemble techniques. This project chose cluster based downsampling to prepare a more balanced data while perserving as much as possible the distribution of the original data. It also used XGBClassifier to better handle this problem in more efficient way.

3. For improvement, over sampling other than downsampling based on clusters can be tried. Bagging-based & boosting-based techniques on machine learning are also choices to boost the classification accuracy.

## Licensing, Authors, Acknowledgements <a name="licensing"></a>

Must give credit to Arvato Financial Solutions, a Bertelsmann subsidiary and [Udacity data science nano degree course](https://www.udacity.com/course/data-scientist-nanodegree--nd025) for the data. Although data can't be used for other purpose than this project, you can still try your own data following the steps in the notebook. 

For more details, please read my blog post ["Find 1 out of 100?"](https://medium.com/@weixind/find-1-out-of-100-a8eae37794fd).
