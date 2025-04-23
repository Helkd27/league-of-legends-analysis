# League of Legends Early Game Win Rate Prediction

## Introduciton
League of Legends (aka LoL or League) is a popular multiplayer online battle arena (MOBA) game where two teams of five players compete to destroy the opposing team's base. Each match involves strategic decision-making, teamwork, and individual skill. 

An early game advantage, such as securing kills, objectives, or gold, can often lead to a "snowball effect." This occurs when the team with the advantage continues to grow stronger, making it increasingly difficult for the opposing team to recover. Learning more about the outcome of the game give early game choices can show which where the focus of the players should be to help maximize the chances that they win the game. In this analysis I hope to answer what should the focus of the team be to maximize the chance that they will win the game. In other words, **what early game statistic has the highest impact on the result of game.**

To answer this, we will use games played from 2022, which the complete data set contains 150588 rows and 161 columns. The columns that will be focused on the following columns

- `position` **String** The position that the player played (Jungler, Bot, Mid, Top, Support, or Team)
- `datacompleteness`**String** Whether the data from the given row comes from a complete game of partial (forfit)
- `firstblood` **True/False** Whether the team was the first to kill another player
- `firstbaron` **True/False** Whether the team was the first to kill a baron
- `firstdragon` **True/False** Whether the team was the first to kill a dragon
- `firstherald` **True/False** Whether the team was the first to kill the Rift Herald
- `firstmidtower` **True/False** Whether the team was the first to destroy the middle tower
- `firsttothreetowers` **True/False** Whether the team was the first to destroy three towers
- `firsttower` **True/False** Whether the team was the first to destroy any tower
- `result` **True/False** Whether the team won the game
- `goldat10` **Integer** The total gold earned by the team at 10 minutes
- `xpat10` **Integer** The total experience points earned by the team at 10 minutes
- `csat10` **Integer** The total creep score (minion kills) by the team at 10 minutes
- `golddiffat10` **Integer** The difference in gold between the team and their opponents at 10 minutes
- `golddiffat15` **Integer** The difference in gold between the team and their opponents at 15 minutes
- `xpdiffat10` **Integer** The difference in experience points between the team and their opponents at 10 minutes
- `csdiffat10` **Integer** The difference in creep score (minion kills) between the team and their opponents at 10 minutes
- `killsat10` **Integer** The total number of kills by the team at 10 minutes
- `assistsat10` **Integer** The total number of assists by the team at 10 minutes
- `deathsat10` **Integer** The total number of deaths by the team at 10 minutes

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning

To continue with the rest of the analysis, we have to clean and organize the vast amount of data that is given to us. First, we will select only rows with both position equal to team and datacompleteness to complete. This is to ensure that we are looking at rows with a completed game and overall team data. 

The only column with missing values is `firstmidtower`. If this column is empty, it can be assumed that neither team took a tower, so we can fill these values with 0.

Below is the first 5 rows of with the selected columns

| teamid                          | gameid                |   result |   firstblood |   firstbaron |   firstdragon |   firstherald |   firstmidtower |   firsttothreetowers |   firsttower |   goldat10 |   xpat10 |   csat10 |   golddiffat10 |   golddiffat15 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |
|:--------------------------------|:----------------------|---------:|-------------:|-------------:|--------------:|--------------:|----------------:|---------------------:|-------------:|-----------:|---------:|---------:|---------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|
| 733ebb9dbf22a401c0127a0c80193ca | ESPORTSTMNT01_2690210 |        0 |            1 |            0 |             0 |             1 |               1 |                    1 |            1 |      16218 |    18213 |      322 |           1523 |            107 |          137 |           -8 |           3 |             5 |            0 |
| 7c64febcd5ccff13dcd035dc6867a00 | ESPORTSTMNT01_2690210 |        1 |            0 |            0 |             1 |             0 |               0 |                    0 |            0 |      14695 |    18076 |      330 |          -1523 |           -107 |         -137 |            8 |           0 |             0 |            3 |
| 731b7a9fd004cdbe2bcb3da795bce47 | ESPORTSTMNT01_2690219 |        0 |            0 |            0 |             0 |             1 |               0 |                    0 |            0 |      14939 |    17462 |      317 |          -1619 |          -1763 |        -1586 |          -27 |           1 |             1 |            3 |
| e7a7c6bf58eb268ed3f13aac4158aa8 | ESPORTSTMNT01_2690219 |        1 |            1 |            1 |             1 |             0 |               1 |                    1 |            1 |      16558 |    19048 |      344 |           1619 |           1763 |         1586 |           27 |           3 |             3 |            1 |
| b9733b8e8aa341319bbaf1035198a28 | ESPORTSTMNT01_2690227 |        1 |            0 |            1 |             1 |             0 |               1 |                    1 |            1 |      15466 |    19600 |      368 |           -103 |           1191 |          813 |           13 |           0 |             0 |            1 |


### Univariate Analysis

Starting this analysis, we can look at the percentages of games with a positive win rate looking at the gold difference 10 minutes into the game. The pie plot below shows this, by showing the games with a postive gold difference, and the number of games that were won and lost. We can see that over two thirds of the teams that had a postive gold difference 10 minutes into the game ended up winning the game!
<iframe
 src="assets/win_rate_pie_chart.html"
 width="600"
 height="400"
 frameborder="0"
></iframe>


### Bivariate Analysis

Looking at the distribution of the games that were won or lost based on the gold difference 10 minutes into the game, we can see that farther into the game, at 15 minutes, the gold difference is larger between teams that won and lost

| gameid                | result   |   time |   golddiff |
|:----------------------|:---------|-------:|-----------:|
| ESPORTSTMNT01_2690210 | Loss     |     10 |       1523 |
| ESPORTSTMNT01_2690210 | Win      |     10 |      -1523 |
| ESPORTSTMNT01_2690219 | Loss     |     10 |      -1619 |
| ESPORTSTMNT01_2690219 | Win      |     10 |       1619 |
| ESPORTSTMNT01_2690227 | Win      |     10 |       -103 |

<iframe
 src="assets/golddiff.html"
 width="600"
 height="400"
 frameborder="0"
></iframe>

### Interesting Aggregates

Grouping by teams, I was curious as to how the first tower affected the mean win rate. The following graph shows that the average of wins is higher per team for the games that they got the first tower. This is another example on how an early game advantage can lead winning the game, i.e. the "snowball" affect.

| teamid                          |      False |      True |
|:--------------------------------|---------:|---------:|
| ff07fcf769a41fa17ded4746368d6c7 | 0.5      | 0.757576 |
| fe59c993d0bda004e54eaecdd957f54 | 0.166667 | 0.333333 |
| fe409cbd7c72eb621d9c4e7eac75936 | 0.321429 | 0.666667 |
| fcec508e780bbd1ad493852640f5b36 | 0.333333 | 0.666667 |
| fca935f82fd01de843aa2799eb575ea | 0.206897 | 0.272727 |

## Framing a Prediction Model

Looking at the primlimiary analysis from the above section, we can see that early game advantage shown by positive Gold and XP difference more often then not causes the team with these advantages to win. 

Going back to the origional question stated in the introduction, we can solve this problem using a **binary classification model**. This model will be predicting the `result` column, indicating the whether the team won the game or not. The baseline model will be trained on all columns with distinct data points from 10 minutes into the game. These include: 
- `golddiffat10`
-  `xpdiffat10`
- `csdiffat10` 
- `killsat10`
- `deathsat10`
- `assistsat10`

To evaulate the baseline and eventually the final model, *F1-score* will be used. This is because it accounts for imbalanced data and it avoids a false preception that *accuracy* can lead to.

## Baseline Model

## Final Model