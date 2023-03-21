# Predicting Money Loss From Outages
by Junyi Xu and Sean Chan

## Framing the Problem
The problem we are looking at for our outages dataset is how can we predict the money loss from outages (regression).

We chose to predict money loss because it would be useful for electric companies to have an estimate of how much money 
they would lose right after an outage is resolved. Thus, we would have information available when the outage happens 
such as the outage start date, start time, cause category, duration, and any info related to prices, sales, and 
customers separated by sector (Residential, Commercial, Industrial). 

For our metric, we are using root mean squared 
error (RMSE) to evaluate our model's ability to predict money loss.

---

## Baseline Model
In our baseline model, we use 6 features: 4 quantitative, 0 ordinal, and 2 nominal. Our quantitative features are 
average electricity consumption (sales), outage duration, price, and customers served. Our nominal features are the 
cause category of the outage and the sector.

We one hot encoded our two nominal categories, 'CAUSE.CATEGORY' and 'Sector', in order to use them as features for our
model.

For our model, we calculated

---
## Final Model
To improve from our baseline model, we added three new features: the time of day an outage occurs 
(Morning, Afternoon, Evening, or Night), whether the day is a weekday or weekend, and the U.S region an outage occurs in.
Having the time of day of an outage can influence the price of electricity during the time frame or how long it takes
for the outage to be resolved. Whether an outage was on a weekday or weekend can influence electricity prices as well 
in certain sectors. The U.S region can influence the cause of the outage and different regions have different 
populations as well. All these features would improve our model's performance as it would account for regional
differences and time differences which play a role in customers served, outage duration, and overall money lost.

---

## Fairness Analysis


