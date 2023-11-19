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
Here are some steps we took in order to gather and clean the data 
1. Download all the data from years 2014-2023 from oracle as csv files, open them and combine into 1 dataset
2. Convert the date to datatime, making it easier to look at data within a certain time interval
3. Remove all data after 10-01-2023, since it is ongoing at the time of our project
4. Fill missing playernames with "unknown player" (we will take a look at this in assessment of missingness).

After completing these steps, here is the dataset we worked with.
<iframe> src="plots/univariate.html" width=800 height=600 frameBorder=0></iframe>
Univariate Analysis: 

Our univariate analysis is very simple, we looked at the winrate distribution between red and blue side. Blue side had a minor but noticable winrate advantage overall, which lines up with By plotting a chart, we get 
<iframe src="plots/univariate.html" width=800 height=600 frameBorder=0></iframe>

For our bivariate analysis we included several plots. One of the ones we tracked is win percentage by side by game length, where we split up our win rate by side into several different game length buckets. We found that the win rate advantage for blue side in shorter games (15-30) min was very high, above 10% difference, while in longer games the winrate advantage for blue side was barely noticible.   
\<iframe src="plots/bivariate.html" width=800 height=600 frameBorder=0></iframe>

In a game of League of Legends, there exist neutral objectives around the map. These objectives can be taken by either team, but doing so requires you to fight them (and usually the opposite team as well). However, a team that manages to take one gets a large advantage for doing so. Since 2 of these objectives, Elder Dragon and Baron Nashor spawn on different sides of the map, we decided to test how much a team's side affects their liklihood to take each of these neutral objectives.  
<iframe src="baron_wr.md" width=800 height=600 frameBorder=0></iframe>
