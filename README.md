# SCS3250-Group-1-Final-Project
Predicting Basketball's Next Superstar

Team 1: Araceli Cuellar, Ghufran khan, Ketan Khanna, Owen Kewell, Ryan Acker

Table of Contents
1. Introduction
2. Objectives
3. Data Source
4. Pre-Preparation
5. Data Preparation
6. Build Training Set
7. Data Exploration & Visualization
8. Baseline Regression Model
9. Improving Model Performance
10. Use Cases
11. Limitations
12. Conclusions
13. Glossary
14. Sources



1. Introduction

The primary focus of an NBA team’s executive management is to win basketball games. This is done by assembling the players who can best contribute on both the offensive and defensive ends of the court. While traditional qualitative scouting of player performance remains indispensable, teams are increasingly relying on statistical analysis and predictive modeling to gain a competitive advantage, with many teams employing data scientists and statisticians (1). These quantitative assessments can help inform key managerial decisions, such as drafting and contract allocation, so as to ensure teams receive a positive return (in terms of basketball performance) on the investment they make in players. In particular, quantitative analysis can leverage data from an NBA player's past performance to predict their future value. Given that many members of our group are passionate sports and basketball fans, we have chosen to explore this topic for our project.

In order to predict player performance, we must first quantify performance. Thankfully, there are many sophisticated 'catch-all' statistics that capture a player's overall contributions within a single number. One such statistic is Player Efficiency Rating, or PER, as developed by ESPN.com columnist John Hollinger. In John's words, "PER sums up all a player's positive accomplishments, subtracts the negative accomplishments, and returns a per-minute rating of a player's performance" (2). Per is calculated each season for each player.

We have selected peak career PER as the target for our predictive model because we believe that PER is a strong measure of a player’s overall performance. PER is multi-faceted: the formula to calculate PER includes traditional metrics such as points, assists, blocks, rebounds (both offensive and defensive), steals and 3 pointers, alongside efficiency metrics like free throw percentage (FT%) and field goal percentage (%) (2). By combining these variables with one another, PER provides a strong (albeit imperfect) assessment of a basketball player's overall performance, and therefore their overall contributions toward winning basketball games.

Peak PER is defined as the max PER a player achieves during their career. PER is calculated each season. So the season with the highest PER is the max. This max reflects the highest level of performance a player is able to achieve during their career.

2. Objectives

In this analysis, our main objective is to develop a model capable of predicting a player's peak PER - in other words how good they were in their best season - by leveraging data from that player's first 3 seasons. We have chosen first 3 seasons as a representation of early-career performance.

Within this main objective, we hope to answer multiple key questions:

How accurately can we predict peak performance using a player's first 3 NBA seasons?
Which current young NBA players are most likely to achieve the highest peak performance? Who should teams target?
How should NBA managers use our findings?

Our hypothesis is that a player's first 3 seasons are highly predictive of their career peak PER. Anectodally, players such as Michael Jordan, Lebron James and Shaquille O’Neal all performed extremely well during their first 3 seasons, and continued to develop into some of the greatest basketball players of all time. Building a predictive model will allow us to validate or disprove this hypothesis. Additionally, our model will be useful in determining to what extent executive management can and should utilize data and predictive modeling to determine an NBA player's future productivity.

3. Data Source

Our primary dataset was found on Kaggle (3) and contains information on every NBA player between 1950 and 2017.

The data was broken down by season and featured three csv files:

Season_Stats.csv: In-game player statistics. This file was the most crucial for our analysis.
Players.csv: Information on the players such as height, weight and city of birth. We omitted this file from our analysis.
player_data.csv: Very similar to Players.csv, but with an important feature: the year the player began their career.

In order to get the most recent data (2018 and 2019), we turned to Basketball-reference.com (4). our Kaggle dataset came from this website, so as we will see merging the two datasets was straightforward. Basketball Reference also allows users to run queries, export directly to csv, and view a glossary containing definitions for every statistic in their database (5). We have included a number of these definitions in the 'Glossary' section to provide explanations of any technical basketball terminology.

4. Pre-Preparation
Considering that PER is a quantitative variable, we decided that our model would be built using linear regression. To begin this process, we imported the libraries necessary for data wrangling, analysis, modelling, and visualization.Next, we read in our data from CSV's sourced from Kaggle and a scraping of basketball-reference.com. As we will see, these data files are quite structured and clean. We are quite confident in the quality of this data given that the NBA publishes statistical data for every game and season and has made substantial investments in the technology and infrastructure to ensure data accuracy (6).

5. Data Preparation

Below are the steps we took to prepare the data for modelling:

   Identified and removed columns made up of entirely missing data. These rows were empty placeholders that served to physically separate sections of data on basketball-reference.com (for visual purposes).
    Combined the 1950-2017 data with the 2018 and 2019 data. This was easy since they have matching features.
    Using our basketball domain knowledge, we identified and removed the features that we did not believe would be relevent to the model's prediction of PER. We did this to manage model complexity and ensure its ability to generalize to new data.
    Removed the rows where player name was missing. There were few of these rows and having a value for player name is essential to our analysis, so we chose to remove them rather than invest the time to impute them.
    Converted year from float to integer to remove the decimal.
    Addressed missing values for shooting statistics. Players with zero shot attempts were returning errors for shooting efficiency, so we imputed values of '0' in these cases.
    Merged the 1950-2019 dataset with the set containing player information (including draft year).
    Engineered a new feature called 'Season' because we only want our training set to contain data from players' first NBA 3 seasons. This feature was calculated by subtracting the year a player was drafted from the current year.
    Filtered out players who played less than 10 games in a season. Since there are 82 games per season we wanted to reduce outliers caused by insufficent sample size.

6. Build Training Set

Next, we set out to build the data on which our model would be trained. In order to do this, we made a number of decisions:

   We filtered to remove missing row values and only include data between 1984 and 2006. We wanted to limit our reliance on data from the 1950's, 1960's, and 1970's, as the game of basketball has changed substantially since then.
    We filtered on our engineered 'Season' feature to only include player data from their first 3 seasons.
    We deleted the features that were intentionally omitted from our model.
    A 3-year average was calculated for each remaining feature.
    Finally, for each player we calculated their career maximum PER.

7. Data Exploration & Visualization

Now that our data is prepared, we wanted to understand what trends, correlations, and patterns were emerging, starting with understanding which players had the most historically dominant seasons according to PER.
Among all players in the dataset, Darrell Armstrong had the highest peak PER (32.4 in 1996), followed by LeBron James (31.7 in 2009) and Dwyane Wade (30.4 in 2009).

While it is not surprising to see the names of so many outstanding players and future Hall of Famers in the top 10, it is surprising to see Darrell Armstrong at the top. Anecdotally, he is less known than any other player on the list. Investigating further, we can see this is because Armstrong was a relative 'one-hit wonder', registering PER above 23 in just a single season. Comparatively, LeBron James, who many consider to be the greatest player in the modern era, registered PER above 23 in 13 different seasons, including 4 seasons with PER above 30.

Next, we wanted to investigate the relationships between our target and each of our features, since independent linear relationships is an assumption of linear regression as a modelling technique.

Unsurprisingly single-season PER (PER_x) was most correlated with career max PER (PER_y) among all our features. Beyond this, the stats most correlated to PER_y were Win Share (WS), Points (PTS), True Shooting % (TS%), and Minutes Played (MP).

Leveraging our domain knowledge, these high correlations are logical because all 4 statistics are intuitively associated with basketball talent. Win Share estimates the number of wins contributed by a player (5), and is a catch-all advanced statistic similar to PER. Points are a key input to the PER calculation that can be understood by even the most casual of fans. True Shooting % is a measure of shooting talent that takes into account field goals, 3-pt field goals, and free throws (5), and finally it's logical minutes played would be correlated as coaches would give better players more minutes.

The heatmap above shows the correlation coefficients for key numeric relationships. We can see strong relationship between almost all variables, except FTr (Free Throw Attempt Ratio) and 3PAr (3-Point Attempt Ratio). These statistics represent the distribution of different types of shots, and have very little correlation with our target. This is logical considering we replaced missing values with 0 for these features and wouldn't expect all basketball favours to favour certain types of shots over others. Some stars excel with 3-point shots for example, while others would attack the net more and get more of their points through Free Throws.

Next, we've chosen to visualize specific statistical relationships. Plotting points (x-axis below), assists, and field goal percentage, we can see that most players have relatively few assists and points, but there was a scattering of impressive dual-threat seasons with high point and assist totals. This measurement is valuable to understanding how a player creates offense - by themselves, or through their teammates.

8. Baseline Regression Model
We have prepared, analyzed, and visualized our data, we can begin to build our initial linear regression model and we can see the model run on 25 players. It is clear that the model has potential, as visually we can see that many of the predictions were within +/- 2 of actual PER. In the case of Mike Dunleavy, Terry Dehere and Kenny Green, the model was practically spot on!

When comparing actual and predicted PER, we can see that these factors are strongly correlated. Our model has an R-squared of 0.68, mean absolute error of ~2.19, mean squared error of ~8.36, and root mean squared error of ~2.89. 


9. Improving Model Performance

Now that we have established and evaluated a baseline model, we will explore a few methods that we believe may improve our model's performance. Models will be built following the same methodology as above, and commenting will be intentionally sparser to improve legibility.

10. Use Cases

The primary mission of an NBA team's management staff is to construct a championship-calibre roster by selecting which players to trade for and sign to contracts. Our findings - evaluating a player's career potential based on early-career performance - is material to both of these areas.

Understanding which players are most likely to become superstars can be used to inform who to sign to contracts and at what financial terms. Allocation of financial resources is critically important because of the salary cap, as teams face penalties when their cumulative player salaries exceed the league-wide cap and must manage this salary cap carefully (7). It is critical to understand a player's future potential when giving them a contract, and our analysis can help teams ensure they are investing in the right players who are most likely to win them games and championships over the duration of the contract. Similarly, our analysis can help inform which players to target in trades, as there may be market inefficiencies where other teams do not realize the potential of certain players.

To demonstrate these uses, we have applied our model to predict the career peak PER of many young NBA players drafted between 2017 and 2019. We can see that Nikola Jokic, Giannis Antetokounmpo, Joel Embiid, and Karl-Anthony Towns emerge as most likely to achieve high peak PER.

It is interesting to see Nikola Jokic ahead of Giannis Antetokounmpo. Giannis is considered the league's best young player while Jokic, despite being very good, is generally overlooked. If our model were accurate, it could present an opportunity for a data-driven team to aquire Jokic now, while he is still relatively underrated.

11. Limitations

Although PER is a strong measure of overall basketball talent, it is not perfect. There are two main areas in which it is deficient as a metric: 1) defensive talent and 2) qualitative 'intangibles'.

PER is worse at measuring defensive talent than offensive talent because defensive talent remains inherently more difficult to quantify. Defense can be thought of as the suppression (rather than creation) of scoring events, meaning that there are comparatively fewer basketball statistics that accurately meaure defensive ability. Good defense is often more subtle than good offense, and if a player fails to score on a given possession it is difficult to quantify the impact of how well they were defended. PER does its best by making use of many common defensive stats - blocks, rebounds, steals, etc. - but nonetheless remains an imperfect measure of defensive talent because it is hard to measures events (like successful baskets) that do not happen.

PER is blind to all qualitative and intangible factors. Basketball is played on the court, not on a spreadsheet, and so there are many things that impact individual and team-level success that do not show up in a player's statistics. For example, PER does not tell us whether a player is a good leader, whether they are dedicated to their craft and committed to improving, how open they are to feedback from coaches, how they interact with teammates, or what impact they have on team morale. These are all important considerations for NBA managers as they assemble their teams, but PER is blind to these considerations and others like them. For these reasons, we believe that statistics such as PER are most powerful when used to supplement traditional evaluations of player talent ("the eye test"), not replace them altogether.

In terms of our model's predictions, there were many instances where our model was way off. Let's explore a few of these misses.

12. Conclusion

We can take this opportunity to answer the questions we set out in our objectives.

  How accurately can we predict peak performance using a player's first 3 NBA seasons?
As we can see with our model's strong R-squared value, a player's first 3 seasons are quite predictive of their future peak performance. This validates the legitimacy of our analysis, as we believe NBA managers should use our findings in evaluating the future potential of young players as outlined above.
An area of future analysis would be improving the model's ability to predict stars who have underwhelming early seasons.

   Which current young NBA players are most likely to achieve the highest peak performance?
As shown above, the young NBA players most likely to become superstars according to our model are Nikola Jokic, Giannis Antetokounmpo, Joel Embiid, and Karl-Anthony Towns. NBA managers should prioritize acquiring these high-potential players.

   How should NBA managers use our findings?
As outlined above, NBA managers should use our findings to supplement their evaluations of young NBA players to identify future potential. Quantitative analysis serves a key role in this process, and should be used in conjunction with an understanding of the NBA landscape to identify market inefficiencies and inform which players to target in trades and sign to contracts.

13. Glossary

Player Efficiency Rating: Sums up all a player's positive accomplishments, subtracts the negative accomplishments, and returns a per-minute rating of a player's performance
Free Throws (FT/FTM): An attempt at the basket from the free throw line (worth one point) given to a player following a foul or other infraction
Field Goals (FG/FGM): A basket scored while the ball is in play. This includes both 2 pointers and 3 pointers and excludes free throws
Points (PTS): Accumulated by making field goals or free throws. At the end of regulation, the team with the most points wins the game. Assists (AST): The number of passes that lead directly to a made field goal by a player
Blocks (BL): The number of times a player on the defensive end blocks an attempted shot by the offence
Rebounds (RB): When a player recovers the ball after a missed shot (on either offense or defense)
Steals (ST): When a player on defense is able gain possession of the ball through a legal turnover.
2 Pointers: Field goals made inside or on the three-point line (worth 2 points)
3 Pointers: Field goals made outside the three-point line (worth 3 points)
Field Goal Percentage (FG%): Percentage of field goals made to field goals attempted
Free Throw Percentage (FT%): Percentage of free throws made to free throws attempted
True Shooting %: A shooting percentage that accounts for the value of the shot made (3 pters, 2 pters and free throws)
MIN: Minutes Played
FTr (Free Throw Attempt Ratio): Ratio of free throws attempted per field goal attempted
3PAr (3-Point Attempt Ratio): Ratio of 3-Points attempted per field goal attempted

14. Sources

    Why Nerds Rule the NBA (https://www.nytimes.com/2019/11/27/sports/basketball/nba-analytics.html)
    Calculating PER (https://www.basketball-reference.com/about/per.html)
    NBA Player Stats Since 1950 (https://www.kaggle.com/drgilermo/nba-players-stats)
    Basketball Reference Home Page (https://www.basketball-reference.com/)
    Basketball Reference Glossary (https://www.basketball-reference.com/about/glossary.html)
    The NBA's Six Year, $250M Data Deal (https://www.forbes.com/sites/darrenheitner/2016/09/22/the-nbas-six-year-250-million-data-deal/#4b0d85eb481d)
    NBA 2019-2020 Cap Tracker (https://www.spotrac.com/nba/cap/)


