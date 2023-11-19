# Pro-league-data-Red-vs-Blue
Used in dsc80 project 3 
# Introduction
Our dataset for this project is the set of all professional League of Legends games played, beginning in 2014. 
Each game of league of legends is played on a diagonal asymmetrical map, which roughly looks like ![map not found](https://static.wikia.nocookie.net/leagueoflegends/images/5/53/Summoner%27s_Rift_Update_Map.png/revision/latest?cb=20170223053555)
While the map layout has changed slightly over the years (something we will discuss later), it has generally remained consistent. There are 2 teams of 5 players each, the blue team which starts on the bottom left of the map, and the red team which starts on the top right of the map. As we can see, this map is asymmetrical, meaning that there are differences between playing on the red and blue side.

We wanted to take a look at how those differences affect pro matches by asking the question:

  **Does the side that a professional team is assigned influence the outcome of the game?**

## Cleaning 
In order to answer this question, our first step was to get the data into usable format. We downloaded each tracked year of professional league games as csvs from oracle elixir, and our first step was to combine them into a single dataset. When doing this we dropped all columns we didn't need. We then changed the date column from the dataset to use a datetime format, which made it easier if we wanted to look at data within a specific time interval. We also cut off any games from after 2023-10-01, since the league is still ongoing at the time of this project. 
