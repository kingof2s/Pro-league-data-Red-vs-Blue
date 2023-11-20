# Pro-league-data-Red-vs-Blue
Used in dsc80 project 3 
# Introduction
Our dataset for this project is the set of all professional League of Legends games played, beginning in 2014. 
Each game of league of legends is played on a diagonal asymmetrical map, which roughly looks like 

<iframe src="assets/summoners_rift.jpg" width=800 height=600 frameBorder=0></iframe>.


While the map layout has changed slightly over the years (something we will discuss later), it has generally remained consistent. There are 2 teams of 5 players each, the blue team which starts on the bottom left of the map, and the red team which starts on the top right of the map. As we can see, this map is asymmetrical, meaning that there are differences between playing on the red and blue side.

We wanted to take a look at how those differences affect pro matches by asking the question:

  **Does the side that a professional team is assigned influence the outcome of the game?**

## Cleaning 
Here are some steps we took to gather and clean the data 
1. Download all the data from years 2014-2023 from oracle as csv files, open them and combine into 1 dataset
2. Convert the date to datatime format, making it easier to look at data within a certain time interval
3. Remove all data after 10-01-2023, since it is ongoing at the time of our project

After completing these steps, here is the dataset we worked with.

|       | gameid               | league   |   game |   patch | side   | position   |   playername | teamname        |   champion |   gamelength |   result |   dragons |   elders |   opp_elders |   heralds |   barons |   opp_barons |   towers |   inhibitors |   vspm | Win?   |
|------:|:---------------------|:---------|-------:|--------:|:-------|:-----------|-------------:|:----------------|-----------:|-------------:|---------:|----------:|---------:|-------------:|----------:|---------:|-------------:|---------:|-------------:|-------:|:-------|
| 32650 | ESPORTSTMNT02/180233 | EU CS    |      1 |     7.1 | Blue   | team       |          nan | MNM Gaming      |        nan |         2387 |        1 |         2 |        0 |            0 |         0 |        2 |            0 |        7 |            1 | 7.6414 | Win    |
| 32651 | ESPORTSTMNT02/180233 | EU CS    |      1 |     7.1 | Red    | team       |          nan | Millenium       |        nan |         2387 |        0 |         2 |        0 |            0 |         1 |        0 |            2 |        6 |            1 | 9.0239 | Loss   |
| 32662 | ESPORTSTMNT02/180242 | EU CS    |      1 |     7.1 | Blue   | team       |          nan | EURONICS Gaming |        nan |         1762 |        0 |         0 |        0 |            0 |         0 |        0 |            2 |        0 |            0 | 6.4018 | Loss   |
| 32663 | ESPORTSTMNT02/180242 | EU CS    |      1 |     7.1 | Red    | team       |          nan | Red Bulls       |        nan |         1762 |        1 |         3 |        0 |            0 |         0 |        2 |            0 |       11 |            2 | 8.6493 | Win    |
| 32674 | ESPORTSTMNT02/190231 | EU CS    |      1 |     7.1 | Blue   | team       |          nan | Reign           |        nan |         2644 |        0 |         2 |        0 |            1 |         0 |        0 |            3 |        5 |            1 | 7.1029 | Loss   |
{:.mbtablestyle}

## Univariate Analysis: 

<iframe src="assets/univariate.html" width=800 height=600 frameBorder=0></iframe>

This pie chart shows that the `blue` side tends to win more games than the `red` side, suggesting that starting on the `blue` side might have provided certain advantages over the `red` side.


## Bivariate Analysis:

For our bivariate analysis, we included several plots. One of the ones we tracked is the `win percentage` on each `side` by `gamelength`, where we split up our win rate by side into several different `gamelength` buckets. We found that the win rate advantage for blue side in shorter games (15-30) min was very high, above a 10% difference, while in longer games it seems that the `red` team tend to have a higher win rate.   

<iframe src="assets/bivariate.html" width=800 height=600 frameBorder=0></iframe>

This vertical bar chart shows the `Blue Side Winrate Advantage` (y-axis), defined as `Blue side winrate - Red side winrate`, as grouped by "buckets" of `gamelength` (x-axis). We see that the `blue` teams are more likely to win in short-lasting matches (15-30 minutes), with the `Blue Side Winrate Advantage` decreasing as `gamelength` increases.

## Interesting Aggregates:

In a round of League of Legends, there exist neutral objectives that spawn in fiexd locations in the map. These objectives are monsters that can be taken by either team, but doing so requires you to fight them (and usually the opposite team too). However, a team that manages to take one earns a large advantage, whether through bonus gold that can be used for purchasing powerful items, or a teamwide increase in the character's combat power. There are two major neutral objectives, the Elder Dragon and Baron Nashor, that spawn on different sides of the map that are especially impactful to the game, so we decided to test how much a team's starting side affects the likelihood for them to take either of those two. The Elder Dragon spawns closer to the blue side of the map, while Baron Nashor spawns closer to the red side of the map. 

| Baron Advantage |   Blue Winrate |   Red Winrate  |
|:----------------|---------------:|---------------:|
| Less Barons     |           9.29 |           7.15 |
| Same Barons     |          57.5  |          42.45 |
| 1 More Baron    |          92.82 |          90.1  |
| 2+ More Barons  |          92.91 |          92.17 |
{:.mbtablestyle}

| Elder Advantage |   Blue Winrate |   Red Winrate  |
|:----------------|---------------:|---------------:|
| Less Elders     |          16.76 |          19.4  |
| Same Elders     |          53.61 |          46.37 |
| 1 More Elder    |          80.42 |          83.23 |
| 2+ More Elders  |          83    |          83.33 |
{:.mbtablestyle}

## Assessment of missingness

## Hypothesis Testing
The first hypothesis test we did was on our initial research question:

**Does the side that a professional team is assigned influence the outcome of the game?**
<br><br>

**Null Hypothesis:** 
<br>In the population (of all games), the blue side does not have an advantage over the red side, so the difference in their win rates is due to random chance.

**Alternate Hypothesis:** 
<br>In the population (of all games), the blue side has some advantage over the red side, so the observed difference we saw cannot be attributed to random chance alone. 

**Test Statistic**: <br>Difference in group means: <i>Mean result of blue side - Mean result of red side</i>

***Significance Level***: 
<br>1% (0.01).<br> The outcome of games can vary due to a large number of variables, so we chose a stricter significance level to be more certain that the bias in win rates is not due to random chance. 

***p-value:***
</br> 0.0 (100,000 simulations)<br> 

<iframe src="assets/MD_blue_adv.html" width=800 height=600 frameBorder=0></iframe>

***Conclusion***
<br> We reject the null hypothesis.<br> 
 This means that the difference in the blue side win rate is not out of random chance. In other words, it is likely that starting on the blue side grants the team certain advantages over the red side team. 

 One such advantage could possibly be the positioning of neutral objectives like the Dragons, the Rift Heralds, and the Baron. We chose to explore the effects of the Rift Herald, which can provide the team that killed it an item that helps them earn gold and gain more control over the game. 
 
 The Rift Herald lives closer to the red side team, and so we formed a second question:

### Did the winrate for blue side change after the Rift Herald was added to the game?
To answer this question, we compared the data from patches before and after patch 5.22, the patch which marked the introduction of the Rift Herald

**Null Hypothesis:** 
<br>The differences in win rate from before and after the introduction of the Rift Herald came from the same distribution.

**Alternate Hypothesis:** 
<br>The difference in win rate has decreased after the introduction of the Rift Herald

**Test Statistic**: <br>Difference in group means: <i>Mean result of blue side - Mean result of red side</i>

***Significance Level***: 
<br>1% (0.01).<br> Same as before; we chose a stricter significance level to account for the high variability within League of Legends games

***p-value:***
</br> 0.0 (10,000 simulations)<br> 

<iframe src="assets/MD_blue_adv.html" width=800 height=600 frameBorder=0></iframe>

***Conclusion:***
<br> We reject the null hypothesis. 
<br> This means that the blue side win rate from before and after the introdcution of the Rift Herald are from different distributions.
<br> In other words, we have evidence to show that the introduction of the Rift Herald has balanced out the win rate difference between blue/red side.