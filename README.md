# Buy-Affinity
## 1. Business problem : 
Given a data point of customer and item related
features predict whether the customer will buy that item or not.
## 2. Datasets : 
Given two .txt files(BuyAffinity_train.txt and BuyAffinity_test.txt), it is the e-commerce dataset.
## 3. Objective : 
Given a datapoint predict if the customer buy a product or not. predict class label(0->Not buy, 1->buy).
## 4. Constraints : 
It is not a low latency problem, we have to predict the class label which tells whether a customer will buy an item or not, if 0 then not buy and if 1 then buy.
## 5. Performance metrics : 
Area under ROC curve. Because the
given dataset is highly imbalanced, so if we choose
accuracy as our metrics then this can be bias towards
majority class, for this problem of imbalanced dataset we
should have to use a metric which can give high score if
accuracy of both the class is high and we want high
accuracy for both the classes, Area under ROC curve
represent accuracy for both the class by a single value.
The AUC score represent the ability to classify +ve point
as +ve and -ve point as -ve. AUC_ROC is the degree or
measure of separability. For example, if my problem is binary classification and model gave AUC=0.75 then its mean
if we fed two query points to the model first is +ve point
and second is -ve point to the trained model then with
0.75 probability that model will classify first point as +ve
point and with 0.75 probability model will predict second
point as -ve point.
## First Cut Approach :
1. First loaded the data and checked the fraction of the datapoints
belonging to class-0 and class-1 i found that 75.5% datapoints belongs
to class-0 and rest 24.5% belongs to class-1 so this dataset is highly
imbalance dataset.
2. Since the feature names does not describe its value and importance
and which feature uses for what so i did feature analysis on each
feature and observe their behaviour.
3. Checked the presence of missing values and i found that there are no
missing values present in any feature.
4. Features F1 to F4 are all very similar in distribution and its range of
values lies within 0.0 to 1.0 and there are no outliers present.
5. Features F5 to F9 are int64 features and its range of values lies from
-10000 to +10000(wide range of values) but the distribution of each
features are very similar to each other, and no outliers are present.
6. Features F10 to F14 are features like userId and itemId because each
row contain unique value for feature F10 to F14 and no relation i found
to create some features from these features so i decided to remove
these features because it would not be capable to separate the
class-0 points from class-1 points.
7. Features F15 and F16 are datetime feature i assume that from
datetime(F15) to datetime(F16) is period from place order and purchase
and some like that thing but it was wrong because the period was to
long so i decided to create day of week feature from feature F15 and
F16 and i found day of week features are very important for
classification.
8. F17 and F18 are int64 features but it looks like categorical features
because these features have only five distinct values and F17 , F18 are
very important for separating class-0 points from class-1 points
because these features have some power of separability.
9. Features F19 and F20 have some crazy behaviour but has good sense
to separate class-0 datapoints from class-1 datapoints.
10.Features F21 and F22 are int64 features but these features have only
21 distinct values so it looks like categorical features after visualizing
its histogram plot it shows it has nice property to separate class-0
points from class-1 points.
11. After plotting the heatmap of correlation between various features i
found 2 features have correlation 0.65 and except these features all
features are correlated in very small amount so i decided to retain all
the features.
12.Since features F5 to F9 have values ranging from -10000 to +10000 so
i decided to create some aggregated engineered features on features
F5 to F9 using categorical features like(F17,F18,and week day name
features), and i found it perform very well for training model.
13. Splitted the train dataset into 70:30 ratio.
14. Normalizing all the numerical features with mean centring and
variance scaling.
15.One hot encoded the categorical features(week day name) because
these features have only seven distinct values.
16.First trained Logistic Regression model it gave train_AUC=0.6875 and
CV_AUC=0.6847 , it did not give very nice auc score but this model is
not overfit and not underfit but the auc score is reasonably good. And i
found that engineered features are very important for logistic
regression. Logistic regression did not perform very well because the
number of features are not very high and LR performs very well when
number of features are very high because high dimensionality leads to
linear separability.
17. Second i trained Random Forest model and and it gave train_AUC
=0.7225 and CV_AUC=0.6935 this model is also not overfit to the train
data because the difference between train auc and test auc is very
less but it perform slightly better that LR model for feature importance
i found that engineered features are again very important for RF
model.
18.Third model that i trained is LGBMClassifier model because it is the
implementation of GBDT and it is superfast than XGBoost model and it
perform better that LR and RF model iot gave train_AUC=0.7164 and
CV_AUC=0.7027 and again the engineered features are very
important for LGBM model, so here the winner is LGBM Classifier
model.
19. The performance of the model can be increased further by applying
some nice feature engineering technique but the model that i got i
reasonably well and the best thing is that these model are not overfit.
