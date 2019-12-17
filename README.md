# tennis

## Question:
### Can we predict who will win the next point in a tennis match? (Player 1 or not Player 1)
### Given the stats of each player so far in the match, and given what we know about where we are in the scheme of the match, before the point begins, will player 1 win the next point?

## Data:
Data was taken from Tennis ATP and Jeff Sackmann's work on accumulating tennis data. The original data consists of all the points played in the 2019 US Open. Each observation represented one point in the tournament. Each point has a match id linking it to its corresponding match, and the point number of the observation within its match. Each point includes information about who won that point, along with descriptive characteristics about that point. However, many of those characteristics indicate who won that point. As formatted, data needed to be reconfigured in order to be able to use non-deterministic variables in the model. 

![Screen Shot 2019-12-16 at 9 17 13 AM](https://user-images.githubusercontent.com/52469561/70913907-eb5d8280-1fe4-11ea-8bbd-b346db929dea.png)

## Feature Engineering:

I transformed the whole dataset so that the information associated with each observation (i.e. point) could be used to predict the outcome of that point. This meant calculating statistics such as the Net Point Rate of each player (how often a player comes up to net), along with the Net Point Win Rate (how often a player wins when she/he comes up to net), considering  the preceding points of that match. Importantly, these statistics did not include the outcome of the point we are trying to predict. 


![Screen Shot 2019-12-16 at 9 17 36 AM](https://user-images.githubusercontent.com/52469561/70914327-c584ad80-1fe5-11ea-94ab-82a625eb631f.png)

![Screen Shot 2019-12-16 at 9 25 51 AM](https://user-images.githubusercontent.com/52469561/70914419-f664e280-1fe5-11ea-83ab-2479cf65c617.png)

## Exploratory Data Analysis
### Checking for Class Imbalance
![Screen Shot 2019-12-16 at 9 48 09 AM](https://user-images.githubusercontent.com/52469561/70916190-41ccc000-1fe9-11ea-9cc4-100687e9df36.png)

### Checking for Differences in Feature Averages and Distributions when Grouped by Player
#### This is important because the assignation of "Player 1" vs. "Player 2" for each match is random. Since each observation represents a point in a match, and since we have different matches represented within the dataframe, we want to ensure that Player 1 is not, already, more likely to win at baseline.

![Screen Shot 2019-12-16 at 12 20 02 PM](https://user-images.githubusercontent.com/52469561/70928155-6da67080-1ffe-11ea-9fe5-86f60c3f12e0.png)
![Screen Shot 2019-12-16 at 12 19 51 PM](https://user-images.githubusercontent.com/52469561/70928156-6da67080-1ffe-11ea-9f4f-76f0a3964500.png)

## Model Building

I am working against a baseline model with an Accuracy Socre of ~50%. I fit models using CV GridSearch Random Forest, AdaBoost, Gradient Boost, and Logistic Regression. Since I was most interested in being able to predict correctly without prioritizing ability to predict postiives or negatives (Player 1 winning or Player 2 winning), I focused on Accuracy and the AUC score for ROC curve. The AUC score for ROC helps to capture the trade-off between false positives and true positives, and so better captures a model's ability to predict correctly overall. Using this criteria, the best model turned out to be Gradient Boost. Tree algorithms work particularly well for datasets with high interaction amongst predictors. Interaction seem likely for tennis data where some variables become more relevant when others are, too, relevant. However, this is just conjuncture. Additionally, the Gradient Boosting method helps to avoid the tendancy to over fit. 

## Gradient Boost -- Final Model
![Screen Shot 2019-12-17 at 7 21 26 AM](https://user-images.githubusercontent.com/52469561/70995128-c24cf880-209d-11ea-991b-80f0958b27c9.png)
### We can see above that while Accuracy score is 75% (well above the baseline model) the AUC score is 84%, the model does better at overpredicting positives (when Player 1 wins) and misses many true negatives (when Player 2 wins). Moving forward, we could try to adjust the threshold between the tradeoff between true positives and false positives to get a more balanced model prediction.

## Conclusion and Next Steps
While the Gradient Boost Model performed well at predicting who would win the next point, I would be interested in doing further feature engineering and model optimization in order to see if we could improve a more interpretable model. This would allow us to get a better sense of what features end up being predictive.

