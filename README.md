# Predicting Money Loss From Outages
by Junyi Xu and Sean Chan

## Framing the Problem
The problem we are looking at for our outages dataset is how can we predict the money loss from outages (regression).

We chose to predict money loss because it would be useful for electric companies to have an estimate of how much money 
they would lose right after an outage is resolved. Thus, the information we would have available include 
the outage start date, start time, cause category, duration, and any info related to prices, sales, and 
customers served separated by sector (Residential, Commercial, Industrial). 

For our metric, we are using the score (R^2) to evaluate our model's ability to predict money loss.

---

## Baseline Model
For our baseline model, we one hot encode our categorical columns and fit our data as a linear regression.
In our baseline model, we use 6 features: 4 quantitative, 0 ordinal, and 2 nominal. Our quantitative features are 
average electricity consumption (sales), outage duration, price, and customers served. Our nominal features are the 
cause category of the outage and the sector.

We one hot encoded our two nominal categories, 'CAUSE.CATEGORY' and 'Sector', in order to use them as features for our
model. We did this through a sklearn Pipeline which used ColumnTransformer() and OneHotEncoder().

For our model, we calculated the training score and the test score. We got 0.17180298336664357 and 0.20517124605525375
respectively.

The current model is not "good" as the scores we got for the training and test model are low. This indicates that linear
regression does not fit well for predicting money loss with our current features.

---
## Final Model
To improve from our baseline model, we added three new features: the time of day an outage occurs 
(Morning, Afternoon, Evening, or Night), whether the day is a weekday or weekend, and the U.S region an outage occurs in.
Having the time of day of an outage can influence the price of electricity during the time frame or how long it takes
for the outage to be resolved. Whether an outage was on a weekday or weekend can influence electricity prices as well 
in certain sectors. The U.S region can influence the cause of the outage and different regions have different 
populations as well. All these features would improve our model's performance as it would account for regional
differences and time differences which play a role in customers served, outage duration, and overall money lost.

For our final model, we decided to use ElasticNet regression, which is a similar linear regression model that combines 
L1 and L2 norm in loss function. Since the calculation of mean square error requires for loop whereas the calculation 
of L2 norm relies on matrix multiplication, ElasticNet regression can achieve higher computational efficiency.

Moreover, as we incorporate more features from the outage dataset to build our final model, ElasticNet has the advantage
of using L1 and L2 regularization to prevent our model from over-fitting. L1 regularization also helps reduce 
multicollinearity by forcing the coefficient of the feature that do not contribute much towards zero. L2 regularization 
can reduce the impact of outliers, which is desirable to fit our model to the highly skewed outage dataset.

The hyperparameters that ended up performing the best were {'alpha': 0.3, 'l1_ratio': 0.6, 'max_iter': 300}. 

We selected the best hyperparameters for our ElasticNet() model by using GridSearchCV() on ElasticNet() with the 
dictionary: hyperparameters = {"max_iter": [300, 500, 600, 700, 800],"alpha": [0.3, 0.4, 0.5, 0.6, 0.8, 0.9, 1], 
"l1_ratio": [0.1, 0.2, 0.3, 0.4, 0.5, 0.6]}. The GridSearchCV() used 3-fold cross-validation and was scored based on R^2

Our Final Model's performance improved over our Baseline Model's performance as the training score of both model's 
stayed the same, while the test score improved when going from the Baseline Model to the Final model. Improving from 
0.20517124605525375 to 0.22195186620978952 respectively.
 

---

## Fairness Analysis
The two groups we observed for our fairness analysis are the residential sector and the other sectors (commercial and 
industrial).

Our evaluation metric is R^2 or the score of our model.

**Null hypothesis:** The closeness of the model's prediction to actual money lost for the residential sector is the same 
as the closeness of the model's prediction to actual money lost for other sectors.

**Alternative hypothesis:** The closeness of the model's prediction to actual money lost for the residential sector is 
different from the closeness of the model's prediction to actual money lost for other sectors.

The test statistic for our fairness analysis is the absolute difference between the R^2 value of the residential sector
and the other sectors.

For the significance level, we decided to use 0.05

Our resulting p-value was 1.0. As our p-value is greater than our significance level, we reject the null hypothesis. 
This suggests that our model is fair when predicting between the residential sector and other sectors. 

