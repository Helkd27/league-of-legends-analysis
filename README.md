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

To evaluate the baseline and eventually the final model, both *F1-score* and *accuracy* will be used. The *F1-score* is particularly useful for handling imbalanced data, as it balances precision and recall, providing a more nuanced view of model performance. On the other hand, *accuracy* offers a straightforward measure of the proportion of correct predictions, which can still provide valuable insights when the dataset is not heavily imbalanced. By considering both metrics, we can gain a comprehensive understanding of the model's performance.

## Baseline Model

### Baseline Model Description

The baseline model is a **binary classification model** that predicts whether a team will win (`result = 1`) or lose (`result = 0`) based on early game statistics. The features used in the model are those given above (`golddiffat10`, `xpdiffat10`, `csdiffat10`, `killsat10`, `deathsat10`, `assistsat10`). All six features are quantitative, and no ordinal or nominal features are included in this baseline model. A `StandardScaler` was used to standardize the features, ensuring that they are on the same scale for the classifier.

1. **Model**:
  - The classifier used is a `KNeighborsClassifier` with 5 neighbors

2. **Performance**:
  - The model was evaluated using the **F1-score**, which balances precision and recall, making it suitable for imbalanced datasets.
  - After training and testing, the model achieved an F1-score of **0.688** on the test set. This indicates that the model performs reasonably well in predicting game outcomes based on early game statistics.

3. **Model Evaluation**:
  - The current model is a good starting point as it captures key early game metrics that influence game outcomes. However, it may not fully capture the complexity of the game, such as team composition or player skill. Further improvements could include adding more features, tuning hyperparameters, or experimenting with more complex models like ensemble methods.

| Dataset   |   F1 Score |   Accuracy |
|:----------|-----------:|-----------:|
| Train     |   0.761578 |   0.760698 |
| Test      |   0.65492  |   0.654467 |

## Final Model

### Final Model Description

For the final model, additional features were included to better capture the early game dynamics that influence game outcomes. The features added were:
- `firstblood`
- `firsttower`
- `firstdragon`
- `firstherald`
- `firstbaron`
- `firstmidtower`
- `firsttothreetowers`

These features were chosen because they represent key early game objectives and events that can significantly impact a team's momentum and chances of winning. For example, securing the first tower or first dragon provides both strategic and resource advantages, which are critical in the early stages of the game. Including these features allows the model to better understand the relationship between early game objectives and the final result.

### Modeling Algorithm and Hyperparameter Tuning

Two models were evaluated for the final prediction task:
1. **K-Nearest Neighbors (KNN)**:
  - Hyperparameters: The number of neighbors (`n_neighbors`) was tuned using a grid search over the range of 5 to 20.
  - Best Hyperparameter: `n_neighbors = 7`.

2. **Random Forest Classifier**:
  - Hyperparameters: The maximum depth of the trees (`max_depth`) was tuned using a grid search over the range of 1 to 10.
  - Best Hyperparameter: `max_depth = 6`.

The hyperparameters were selected using `GridSearchCV`, which performs an exhaustive search over the specified parameter grid and evaluates each combination using cross-validation. This ensures that the selected hyperparameters generalize well to unseen data.

### Final Model Performance

The final models were evaluated on the test set, and their performance metrics were compared to the baseline model. The results are summarized below:

| Model         |   F1 Score |   Accuracy |
|:--------------|-----------:|-----------:|
| Random Forest |   0.847125 |   0.847785 |
| KNN           |   0.840623 |   0.840653 |

The final models showed a significant improvement over the baseline model. The Random Forest Classifier outperformed KNN in both F1-score and accuracy, making it the preferred model for this task. The inclusion of additional features and the use of hyperparameter tuning contributed to the improved performance, as the model could better capture the complex relationships between early game events and game outcomes.

### Feature Importance Analysis

To better understand the factors influencing the final model's predictions, we analyzed the feature importances from the Random Forest Classifier. The table below highlights the top five features and their respective importance scores:

| Feature            |   Importance |
|:-------------------|-------------:|
| firstbaron         |    0.459617  |
| firsttothreetowers |    0.165464  |
| firstmidtower      |    0.11258   |
| golddiffat10       |    0.0731515 |
| xpdiffat10         |    0.0637435 |

### Explanation of Results

The feature importance analysis reveals that securing the **first baron** is the most critical factor in determining game outcomes, with an importance score of 0.459617. This aligns with the game's mechanics, as the baron buff provides a significant advantage in terms of pushing power and team strength. Following this, **first to three towers** and **first mid tower** also play substantial roles, emphasizing the importance of early map control and objective prioritization.

Interestingly, while **gold difference at 10 minutes** and **XP difference at 10 minutes** are important, their lower scores suggest that early game objectives (like baron and towers) have a more direct impact on the game's result than raw resource advantages.

### Answering the Question from the Introduction

The analysis answers the question posed in the introduction: **"What early game statistic has the highest impact on the result of the game?"** Based on the feature importance scores, the early game statistic with the highest impact is securing the **first baron**. This finding underscores the importance of prioritizing key objectives over purely focusing on resource accumulation, as these objectives provide both immediate and long-term strategic advantages that significantly increase the likelihood of winning the game.