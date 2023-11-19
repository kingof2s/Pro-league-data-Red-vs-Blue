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

Our univariate analysis is very simple, we looked at the winrate distribution between red and blue side. Blue side had a minor but noticable winrate advantage overall, which lines up with By plotting a chart, we get 

<iframe src="plots/univariate.html" width=800 height=600 frameBorder=0></iframe>

## Bivariate Analysis:

For our bivariate analysis, we included several plots. One of the ones we tracked is win percentage by side by game length, where we split up our win rate by side into several different game length buckets. We found that the win rate advantage for blue side in shorter games (15-30) min was very high, above a 10% difference, while in longer games the winrate advantage for blue side was barely noticeable.   

<iframe src="plots/bivariate.html" width=800 height=600 frameBorder=0></iframe>

In a game of League of Legends, there exist neutral objectives around the map. These objectives can be taken by either team, but doing so requires you to fight them (and usually the opposite team as well). However, a team that manages to take one gets a large advantage for doing so. Since 2 of these objectives, Elder Dragon and Baron Nashor spawn on different sides of the map, we decided to test how much a team's side affects their likelihood to take each of these neutral objectives.  

| Baron Bins     |   ('Winrate', 'Blue') |   ('Winrate', 'Red') |
|:---------------|----------------------:|---------------------:|
| Less Barons    |                  9.29 |                 7.15 |
| Same Barons    |                 57.5  |                42.45 |
| 1 More Baron   |                 92.82 |                90.1  |
| 2+ More Barons |                 92.91 |                92.17 |
{:.mbtablestyle}

