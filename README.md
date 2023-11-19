# Pro-league-data-Red-vs-Blue
Used in dsc80 project 3 
# Introduction
Our dataset for this project is the set of all professional League of Legends games played, beginning in 2014. 
Each game of league of legends is played on a diagonal asymmetrical map, which roughly looks like ![map not found](https://static.wikia.nocookie.net/leagueoflegends/images/5/53/Summoner%27s_Rift_Update_Map.png/revision/latest?cb=20170223053555)
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
