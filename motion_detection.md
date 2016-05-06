# Motion Detection with SRKM
by _Honghuang Lin_, updated at 05/04/2016

## Data Preprocessing
* Raw data: `action_mosift_500Words_2` (500 features) and `mosift_1000words_1` (1000 features) from Dr. Qiu. Other data sets have not been tested yet.
* Training and testing data separation: randomly select 99 samples out of 199 for training, the rest for testing. The sample number of each motion is either 9 or 10 in the training data set.
* Scaling: each feature in the training data set will be scaled to `[0,1]`. The features in the testing data set will be scaled accordingly.

## Model 1: one-vs-rest classification
* A classifier is trained for each motion to separate it from the others. There are 10 classifiers in total.
* Results(averaged over 10 repetitions):

|Features|Accuracy|
|:---:|:---:|
|500|30.9%|
|1000|34.3%|

* Average number of features selected is about 7.3 out of 500 or 6.6 out of 1000.
* Potential problems: imbalanced training data set.

## Model 2: regression
* A regression model is trained to predict the label directly. Only one model is needed.
* Results(averaged over 10 repetitions):

|Features|Accuracy|
|:---:|:---:|
|500|16.0%|
|1000|15.8%|

* The feature selection results may be more reliable in this case since all motions are modeled in a single learning structure.
* Average number of features selected is about 34.3 out of 500 or 60.5 out of 1000.
* Top 10 features (based on feature weights averaged over 10 repetitions):
  * 500 features: 136, 484, 257, 124, 475, 250, 328, 3, 353, 39;
  * 1000 features: 87, 534, 23, 650, 705, 515, 389, 318, 967, 479.
  
## Model 3: classification based on bit coding
* The 10 motions/lables can be coded as 4-bit binary numbers. A binary classifier can be trained based on the 0/1 value at each bit. 4 classifiers should be sufficient and the training data can be balanced via careful coding.
* Results(averaged over 10 repetitions):

|Features|Accuracy|
|:---:|:---:|
|500|26.3%|
|1000|21.8%|

* Average number of features selected is about 14.1 out of 500 or 13.1 out of 1000.
* Potential problems: there are 6 redundant 4-bit code. Right now they are classified into a target lable with the shortest hamming distance. This may introduce errors.