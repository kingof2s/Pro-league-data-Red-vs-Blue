# Pro-league-data-Red-vs-Blue
This is a project for the DSC80 course at UCSD where we perform cleaning, EDA, missingness assessment, and permutation testing for datasets. We present our findings in this website.

<b>Names:</b> David Lycke and Dhilan Sunanda Bong
# Introduction
Our dataset for this project is on all the professional League of Legends games played from 2013 until now. 
Each game of league of legends is played on a diagonal asymmetrical map, which roughly looks like 

<iframe src="assets/summoners_rift.jpg" width=642 height=460 frameBorder=0></iframe>

<br>

While the map layout has changed slightly over the years (something we will discuss later), it has generally remained consistent. There are 2 teams of 5 players each, the blue team which starts on the bottom left of the map, and the red team which starts on the top right of the map. As we can see, this map is asymmetrical, meaning that there are differences between playing on the red and blue side. We wanted to take a look at how those differences affect pro matches by asking the question:

  **Does the side that a professional team is assigned influence the outcome of the game?**
<br><br>
<p>
The data we have contains 12 row per match: for each of the two teams competing in a match, there are 5 rows for each of the 5 players plus 1 row for the overall team data. There are 113 columns of other data that records specific statistics in the game, but we have chosen to only keep the following:

- `'gameid'` is the specific ID that is unique for every match of LoL.
- `'datacompleteness'` is a self-reported column from the data source that 
- `'league'` is the name of the regional league that the player/team has competed in
- `'year'` is the year that the match was played in
- `'date'` is the date that the match was played in, in the format YYYY-MM-DD
- `'patch'` is the game patch/version that the match was played in
- `'side'` is the side of the game assigned to the competing team; it is either `red` or `blue`
- `'position'` is the name of the role that a player has in the team; it is either `top`, `jng`, `mid`, `bot`, or `sup`. For the summary team statistics, the value of this column is `team`
- `'playername'` is the name of the player in the row; for summary team statistics, it is NaN
- `'champion'` is the name of the League of Legends champion played by the player in that row; for summary team statistics, it is NaN
- `'gamelength'` is the duration of the game in seconds
- `'result'` is `0` when the team lost the game and `1` if they won the game
- `'barons'` is the number of Baron Nashors killed by the team in that game (details of the Baron is explained later)
- `'opp_barons'` is the number of Baron Nashors killed by the enemy team in that game
- `'dragons'` is the number of elemental dragons killed by the team in that game (details explained later)
- `'elders'` is the number of elder dragons killed by the team in that game (also explained later)
- `'opp_elders'` is the number of elder dragons killed by the enemy team in that game (also explained later)

# Cleaning 

1. We downloaded all the datasets from 2013 to 2023, importing only the `'gameid'`, `'datacompleteness'`, `'league'`, `'year'`, `'date'`, `'patch'`, `'side'`, `'position'`, `'playername'`, `'champion'`, `'gamelength'`, `'result'`, `'barons'`, `'opp_barons'`, `'dragons'`, `'elders'`, and `'opp_elders'` columns.
2. Iterated through the directory, merging all the csv sheets into one `'df'` DataFrame
3. Converted the date to datatime format, making it easier to look at data within a certain time interval
4. Removed all data after 10-01-2023, since the games were still ongoing at the time of our project.
5. Dropped the rows where `'datacompleteness' != 'complete'`, then drop the `'datacompleteness'` column
6. We separated `'df'` into `'df_teams'` and `'df_players'` based on whether `'position' == 'team'` or not.
7. For `'df_teams'`, we dropped the `'gameid'`, `'position'`, `'champion'`, `'year'`, and `'date'` columns. 
7. For `'df_players'`, we dropped the `'gameid'`, `'position'`, `'year'`, `'date'`, `'gameid'`, `'dragons'`, `'elders'`, `'opp_elders'`, `'barons'` and `'opp_barons'`

After completing these steps, here is the datasets we worked with:<br>
<br>`'df_teams':`

|    | league   |   patch | side   |   gamelength |   result |   dragons |   elders |   opp_elders |   barons |   opp_barons | Win?   |
|---:|:---------|--------:|:-------|-------------:|---------:|----------:|---------:|-------------:|---------:|-------------:|:-------|
| 10 | EU LCS   |    3.15 | Blue   |         1924 |        1 |         0 |        0 |            0 |        1 |            0 | Win    |
| 11 | EU LCS   |    3.15 | Red    |         1924 |        0 |         0 |        0 |            0 |        0 |            1 | Loss   |
| 22 | EU LCS   |    3.15 | Blue   |         2474 |        1 |         0 |        0 |            0 |        1 |            0 | Win    |
| 23 | EU LCS   |    3.15 | Red    |         2474 |        0 |         0 |        0 |            0 |        0 |            1 | Loss   |
| 34 | EU LCS   |    3.15 | Blue   |         2629 |        0 |         0 |        0 |            0 |        0 |            1 | Loss   |
{:.mbtablestyle}

<br>

`'df_players':`

|    | league   |   patch | side   | playername   | champion   |   gamelength |   result | Win?   |
|---:|:---------|--------:|:-------|:-------------|:-----------|-------------:|---------:|:-------|
|  0 | EU LCS   |    3.15 | Blue   | sOAZ         | Trundle    |         1924 |        1 | Win    |
|  1 | EU LCS   |    3.15 | Blue   | Cyanide      | Vi         |         1924 |        1 | Win    |
|  2 | EU LCS   |    3.15 | Blue   | xPeke        | Orianna    |         1924 |        1 | Win    |
|  3 | EU LCS   |    3.15 | Blue   | Rekkles      | Jinx       |         1924 |        1 | Win    |
|  4 | EU LCS   |    3.15 | Blue   | YellOwStaR   | Annie      |         1924 |        1 | Win    |
{:.mbtablestyle}

<br>

# Univariate Analysis: 

<iframe src="assets/univariate.html" width=640 height=400 frameBorder=0></iframe>

This pie chart shows that the `blue` side tends to win more games than the `'red'` side, suggesting that starting on the `blue` side might have provided certain advantages over the `'red'` side.

<br>

# Bivariate Analysis:

For our bivariate analysis, we included several plots. One of the ones we tracked is the `'result'` on each `'side'` by `'gamelength'`, where we split up our win rate by side into several different `'gamelength'` buckets. We found that the win rate advantage for `blue` side in shorter games (15-30) min was very high, above a 10% difference, while in longer games it seems that the `'red'` team tend to have a higher win rate.   

<iframe src="assets/bivariate.html" width=640 height=400 frameBorder=0></iframe>

This vertical bar chart shows the `'Blue Side Winrate Advantage'` (y-axis), defined as `Blue side winrate - Red side winrate`, as grouped by "buckets" of `'gamelength'` (x-axis). We see that the `blue` teams are more likely to win in short-lasting matches (15-30 minutes), with the `'Blue Side Winrate Advantage'` decreasing as `'gamelength'` increases.

# Interesting Aggregates:

In a round of League of Legends, there exist neutral objectives that spawn in fixed locations in the map. These objectives are monsters that can be taken by either team, but doing so requires you to fight them (and usually the opposite team too). However, a team that manages to take one earns a large advantage, whether through bonus gold that can be used for purchasing powerful items, or a teamwide increase in the character's combat power. There are two major neutral objectives, the Elder Dragon and Baron Nashor, that spawn on different sides of the map that are especially impactful to the game, so we decided to test how much a team's starting side affects the likelihood for them to take either of those two. The Elder Dragon spawns closer to the blue side of the map, while Baron Nashor spawns closer to the `red` side of the map. 

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
**NMAR**<br>
One column we identified as having NMAR data was the `'patch'` column. The `'patch'` column data appears to be completely missing between patches 3.15 and 4.03. Upon doing research, there existed patches 4.01 and 4.02 in the history of the game, but that was not indicated in the dataframe. This means that the `'patch'` column is NMAR, since the missing values are in either 4.01 or 4.02, so their missingness is not at random, and can be determined based on the `'dates'` they are played.

**1st Missingness Test (playername by league)**<br>
In our missingness test, we decided to look at the missingness of the the `'playername'` column, and we identified one column that appeared to influence it and another column which didn't appear to influence it. 
We first tested missingness of `'playername'` with `'league'`, which describes the league that the pro match is played in. For example North American pro games are played in 'LCS', while Korean pro games are in 'LCK'.
For this test we used the total variation difference between `'playername'` and `'league'`, and would reject with a standard p value < 0.05

**Result (dependent)** <br>
For this test we got a p-value of 0 with 10,000 repitions. This means that the `'playername'` is more likely to be missing if they are from certain reigons, meaning the `'playername'` column is MAR. 


<iframe src="assets/missingness_league.html" width=640 height=400 frameBorder=0></iframe>


**2nd Missingness Test (playername by side)**<br>
In this missingness test, we looked at the missingness of the `'playername'` column with the `'side'` column. We used the same test statistic of total variation distance, and the p value 0.05. 

**Result (independent)**<br>
For this test we got a p-value of 0.708 with 10,000 repitions. This means that the missingness is `'playername'` is likely independent to the `'side'` they played on.

**Result (independent)** 


<iframe src="assets/missingness_side.html" width=640 height=400 frameBorder=0></iframe>


# Hypothesis Testing
The first hypothesis test we did was on our initial research question:

**Does the side that a professional team is assigned influence the outcome of the game?**
<br><br>

**Null Hypothesis:** 
<br>In the population (of all games), the `blue` side does not have an advantage over the `red` side, so the difference in their win rates is due to random chance.

**Alternate Hypothesis:** 
<br>In the population (of all games), the `blue` side has some advantage over the `red` side, so the observed difference we saw cannot be attributed to random chance alone. 

**Test Statistic**: <br>Difference in group means: <i>Mean result of `blue` side - Mean result of `red` side</i>

***Significance Level***: 
<br>1% (0.01).<br> The outcome of games can vary due to a large number of variables, so we chose a stricter significance level to be more certain that the bias in win rates is not due to random chance. 

***p-value:***
</br> 0.0 (100,000 simulations)<br> 


<iframe src="assets/MD_blue_adv.html" width=640 height=400 frameBorder=0></iframe>


***Conclusion***
<br> We reject the null hypothesis.
<p> This means that the difference in the `blue` side win rate is not out of random chance. In other words, it is likely that starting on the `blue` side grants the team certain advantages over the `red` side team. 

 One such advantage could possibly be the positioning of neutral objectives like the Dragons, the Rift Heralds, and the Baron. We chose to explore the effects of the Rift Herald, which can provide the team that killed it an item that helps them earn gold and gain more control over the game. 
 
 The Rift Herald lives closer to the `red` side team, and so we formed a second question:

### Did the winrate for `blue` side change after the Rift Herald was added to the game?
To answer this question, we compared the data from `patch` before and after patch 5.22, the patch which marked the introduction of the Rift Herald.

**Null Hypothesis:** 
<br>The differences in win rate from before and after the introduction of the Rift Herald came from the same distribution.

**Alternate Hypothesis:** 
<br>The difference in win rate has decreased after the introduction of the Rift Herald

**Test Statistic**: <br>Difference in group means: <i>Mean result of `blue` side - Mean result of `red` side</i>

***Significance Level***: 
<br>1% (0.01).<br> Same as before; we chose a stricter significance level to account for the high variability within League of Legends games

***p-value:***
<br> 0.0031 (10,000 simulations)


<iframe src="assets/MD_rift_diff.html" width=640 height=400 frameBorder=0></iframe>


***Conclusion:***
<br> We reject the null hypothesis. 
<p> This means that the `blue` side win rate from before and after the introdcution of the Rift Herald are <b>from different distributions</b>.
<p> In other words, we have evidence to show that the introduction of the Rift Herald has helped to balance out the win rate difference between blue/red side.
