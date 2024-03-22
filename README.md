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
  width="800"
  height="600"
  frameborder="0"
></iframe>
In this plot, we see the golddiffat10 for red side bot laners. Upon initial inspection, it seems pretty even, maybe leading one to think that there is no inherent disadvantage for red side bot laners.


**Bivariate Analysis**
<iframe
  src="assets/bi.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
In this plot, we see the trend for golddiffat10 and golddiffat15 for red side bot laners. The reason I included this plot was to demonstrate how using data from 10 minutes is alright to help draw conclusions as a disadvantage at 10 minutes also tends to mean you will be disadvantaged at 15 mins, and that this inherent advantage is something that can actually affect matches.

**Interesting Aggregates**
print(grouped_means[['goldat10', 'xpat10', 'csat10', 'golddiffat10', 'xpdiffat10', 'csdiffat10', 'golddiffat15', 'xpdiffat15', 'csdiffat15']].head().to_markdown(index=False))
This table was just a very convenient way to see whether there are inherent differences between red side and blue side bot laners with the averages of the early game stats they have.

## Assessment of Missingness

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis