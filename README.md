# The-Red-Side-Experience

Final Project for DSC 80 at UCSD

## Introduction

The League of Legends dataset I have chosen essentially goes over many different points of data from esports games from the year of 2024. It gives a high level statistical analysis of what goes on in a League of Legends esports game. The question I have opted to focus on was whether or not red side bot laner have an inherent advantage in the 2024 season due to map changes. The reason this is important, specifically to me and other enjoyers of the esport, is that there may be a violation of competitive integrity if this were to be the case. This being the biggest esport in the world means that this is an issue that needs to be solved if it is present. There are 26,940 rows of data in the dataset for 2024. The columns that I believe were the most relevant to my dataset were those that have stats relevant to showing whether someone has inherent differences in stats. So the columns I wanted to look into were 'csat10', 'goldat10', 'xpat10', 'csdiffat10', 'golddiffat10', 'xpdiffat10' and those diff rows at 15 minutes. Essentially, the numbers at the end of these columns describe the timing in game time when they were taken, and are a good way to see whether one person has an inherent advantage due to map because the game is isolated to a person's respective side of the map at this point in the game. The reason I used diff as well as the normal values was to get a more compounded look of whether someone was doing bad into their opponent.

## Data Cleaning and Exploratory Data Analysis

**Data Cleaning**
Most of the data cleaning that was neccesary for this dataset was filling in empty columns with NaNs in the relevant columns and getting the relevant columns that I described earlier. In addition, I made seperate dataframes that had both red and blue values, only red values, and only blue side values. Also, I made the filter set to only include values that were in the year 2024. The dataframe did have some values from 2023, and I couldn't include those as the map changes hadn't set into place yet.

**Univariate Analysis**

<iframe
  src="assets/uni.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>
In this plot, we see the golddiffat10 for red side bot laners. Upon initial inspection, it seems pretty even, maybe leading one to think that there is no inherent disadvantage for red side bot laners.

**Bivariate Analysis**

<iframe
  src="assets/bi.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>
In this plot, we see the trend for golddiffat10 and golddiffat15 for red side bot laners. The reason I included this plot was to demonstrate how using data from 10 minutes is alright to help draw conclusions as a disadvantage at 10 minutes also tends to mean you will be disadvantaged at 15 mins, and that this inherent advantage is something that can actually affect matches.

**Interesting Aggregates**

```py
print(grouped_means[['goldat10', 'xpat10', 'csat10', 'golddiffat10', 'xpdiffat10', 'csdiffat10', 'golddiffat15', 'xpdiffat15', 'csdiffat15']].head().to_markdown(index=False))
```

This table was just a very convenient way to see whether there are inherent differences between red side and blue side bot laners with the averages of the early game stats they have.

## Assessment of Missingness

**NMAR Analysis**
None of the data is nmar, from what I can tell from looking at the data. When the datacompleteness columns is partial, you can see why some of the columns have missing data. Essentially, almost all of the points I saw where missing with some information that can be drawn from looking at other columns such as 'league' or 'year' or 'datacompleteness'.

**Missingness Dependency**

<iframe
  src="assets/miss.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>
In the above plot, the p-value is 1.0, but the observed statistic for TVD was 0.849. With this information, we are able to conclude that 'league' is a column that can help give us info for if the data in the goldat10 column would be missing. With further analysis, I was able to conclude that LDL games specifically did not have this data for some reason. With the information that we can figure it out based on information from other columns, the missingness of goldat10 was missing at random. With the other plots I generated, we can also see that another column like 'side' did not really have any affect on whether the goldat10 column was missing or not using the permutation tests.

## Hypothesis Testing

For this section, we were trying to make a valid hypotheses test.
**Hypotheses Tests:**
Null: There is no inherent advantage through gold diff and cs diff for red side and blue side bot laners.
Alternate: There is an inherent advantage for blue side bot laners over red side bot laners.
I did a permutation test with TVD as my test statistic. Essentially, a permutation test makes sense here because we are trying to conclude whether red side or blue side laners were not part of the same distribution. TVD makes sense because they are both categorical distributions. The ideal significance value here would be 0.40. This is because we do not want to see any minute difference in TVD as that would be harmful for the competitive integrity. The p-value I ended up getting was 0.704, and therefore, I have opted to reject the null hypothesis. There is a sizeable advantage towards blue side adc's, or at least enough to where it could be harmful to competitive integrity.

## Framing a Prediciton Problem

The prediction problem I would like to do would be to use the picks for both bot laners and the sides they are on to see if we can predict
a number for what the golddiff at 10 would be?
This is a regression problem as we want to predict a quantitative variable using other features in our dataset.
The response variable I opted for here was golddiffat15 because as I stated earlier, if we saw a continued large difference between blue side and red side from 10 to 15 minutes, there was enough of an issue for us to want to question why if advantage inherently exists for blue side bot laners. I have decided to use rmse and r^2 as a way to validate whether our model was fair, due to wanting a variety of sources and because since we are interested in prediciting future trends with this, rmse is a useful tool.

## Baseline Model

Our model uses goldat10 and xpat10 to predict goldat15. All of these features are quantitative. The only scaling I did was to standardize, so that way it would be easier to interpret the results. Apart from that, there was no necessary scaling required. Currently, our model has an RMSE of 864 and an r^2 of 0.479. At the moment, I would argue that these results are not very ideal, and that we should be looking to improve our scores, as the RMSE is too high and the r^2 is too low.

## Final Model

The features I added for the new final model was 2 new polynomial scaled features with goldat10 and xpat10 as well as a Quantile Transformed goldat10. The reason I added these features was because I was looking to improve the ability of my model to predict future data, or ideally increase the RMSE. I believed that a low degree polynomial scaler would help achieve this, if called on the exact same columns as before. In addition, the quantile scaler was also added because we are not trying to look at the mean of the data, but rather the effects as we go from 10 to 15 minutes. I used grid search to figure out the best hyper parameters and the best hyper parameters I landed on at the end were a max depth of 5 and 100 estimators. I used a random forest model for this one because of the above hyper parameters. For this model, I saw a decrease in the RMSE of above 50 and my r^2 remained almost the same. Because of this, I feel confident that my new model would be better at predicting the gold diff than my old baseline model.

## Fairness Analysis
To evaluate our model, we opted to use the Red Side Bot Laners as Group X and Blue Side Bot Laners as group Y. The evaluation metric in question was RMSE as that made sense for us since we were doing a regression problem.

Null: Our model is fair. The RMSE's for both red and blue side is the same and any differences are due to chance.
Alternate: Our model is not fair. The RMSE for blue is less than red.

The test stat we used was the difference in RMSE and our signifcance level was 0.05. Our p-value was 0.867 which is greater than 0.05, and therefore, we fail to reject the null hypothesis. So we can say with 95% confidence that our model was fair.