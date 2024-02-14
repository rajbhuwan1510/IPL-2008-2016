# IPL DATA ANALYSIS

### Dashboard Link : https://app.powerbi.com/groups/me/reports/64f48a91-9b8b-4e4b-9f30-11a07638df91/ReportSection757ff08d9460d0b6c6a5?experience=power-bi

## Problem Statement

The primary purpose of this analysis is to find valuable insights, patterns, and trends within the IPL dataset. By leveraging data-driven methodologies using SQL quires, we aim to give as much as output from the given dataset. We learnt how to read the dataset and find the possible outcome from the same. We learnt how to use SQL language to find the perfect output and in final step we learnt how to use power bi for visualize our data using dashboards

1. Performance Evaluation: Evaluate the performance of individual players and teams over different seasons to identify strengths, weaknesses, and areas for improvement.

2. Strategy Formulation: Use insights derived from the analysis to formulate strategic decisions related to team composition, batting order, bowling strategies, fielding positions, etc.

3. Talent Identification: Identify talented players based on their performance metrics and potential contributions to the team. This can be useful for talent scouting, player recruitment, and selection processes.

4. Opponent Analysis: Analyze the performance of opposing teams to understand their strengths, weaknesses, and playing styles. This information can be used to devise tactics and strategies to counter them effectively.

5. Fan Engagement: Enhance fan engagement by providing insightful analysis and statistics about player and team performances. This could include creating interactive dashboards, infographics, and social media content to keep fans informed and engaged.

6. Coach and Player Development: Provide valuable feedback to coaches and players to aid in their development and improvement. This can involve identifying areas of improvement, tracking progress over time, and setting performance targets.

7. Decision Support: Support decision-making processes related to team management, player selection, match tactics, and investment in player development programs.

Overall, the purpose of this project is to leverage data-driven insights to enhance the performance, strategy, and decision-making processes within the cricket ecosystem, ultimately contributing to the success and growth of the sport.


## Steps followed 

### Working In SQL

- Step 1 : Load data into MySql Workbench, dataset is a csv file.
- Step 2  : Understand the whole data and by applying simple select queries and see the data inserted in all the tables
- Step 3: Apply queries to find respected insights like 'Winner of each season','Man of the match','Man of the season','Wicket taken','Total Runs','Total sixes and fours','Summary of each match'
-   Step 4: After applying the succesfull queries , export the respected tables from the Workbench into CSV files

### Working In Power Bi
- Step 1 : Load the exported data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that there is no errors and empty data because we already managed it with the SQL queries.
- Step 5 : For calculating the age of players we use dax functions and named it age 
- Step 6 : In the report view, under the view tab, theme was selected.
- Step 7 : Since the data contains various ratings, thus in order to represent ratings, a new visual was added using the three ellipses in the visualizations pane in report view. 
- Step 8 : Visual filters (Slicers) were added for four fields named "Years" "Team name".
- Step 9 : Two card visuals were added to the canvas, one representing average departure delay in minutes & other representing average arrival delay in minutes.
           Using visual level filter from the filters pane, basic filtering was used & null values were unselected for consideration into average calculation.
           
           Although, by default, while calculating average, blank values are ignored.
- Step 10 : A bar chart was also added to the report design area representing the number of satisfied & neutral/unsatisfied customers. While creating this visual, field named "Gender" was also added to the Legends bucket, thus number of customers are also seggregated according the gender. 
Step 11 : Card Visual was used to represent different details mentioned below,

    (a) Count Of total Wins

    (b) Total Runs
  
    (c) Total Wickets
  
    (d) Total no Balls
  
    (e) Age of Player
  
    (f) Extra Runs

    (g) Total Series Wins

Step 11 :Table Visual was used to represent different details mentioned below,
  
    (a) Winners Of each seasons
  
    (b) Orange,Purple cap Winners
  
    (c) Man Of Series
  
    (d) Total Boundries (Fours and Sixes)
  
    (e) Different Vemues
  
    (f) All Match Summary
  




Step query writing
- Players details

    (a) SELECT team.Team_Name,player.Player_Id,player.Player_Name, player.DOB,country.country_name as country,batting_style.batting_hand, bowling_style.bowling_skill 
    FROM team 
    JOIN player_match ON team.Team_Id = player_match.Team_Id
    JOIN player ON player_match.Player_Id = player.Player_Id
    JOIN batting_style ON player.Batting_hand=batting_style.Batting_Id
    JOIN bowling_style ON player.bowling_skill= bowling_style.bowling_id
    JOIN country ON player.Country_Name=country.country_id
    GROUP BY team.Team_Name,player.Player_Id,player.Player_Name,player.DOB,player.Batting_hand,player.Bowling_skill,player.Country_Name,batting_style.batting_hand,bowling_style.bowling_skill,country.country_name
    ORDER BY team.Team_Name, player.Player_Name; 
                

![Snap_1](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/22d62a96-83ad-42b2-b245-74601ec4c568)

- Most Man of the match winners

    (b) SELECT season.Season_Year,player.Player_Name AS ManOfTheSeries 
    FROM season
    JOIN player ON season.Man_of_the_Series = player.Player_Id
    ORDER BY season.Season_Year;

![Snap_Count](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/499dfb22-e51d-44cc-982c-531e16c6fcbf)
- Final match details

    (c) SELECT s.Season_Year,m.Match_Date,t1.Team_Name AS Team_1,t2.Team_Name AS Team_2,
    t.Team_Name AS Match_Winner,m.Win_Margin, win_by.Win_Type AS Win_By, p.Player_Name AS Man_of_the_Match 
    FROM matches m
    JOIN team t ON t.team_id = m.Match_Winner
    JOIN season s ON m.Season_Id = s.Season_Id
    JOIN player p ON m.Man_of_the_Match = p.Player_Id 
    JOIN team t1 ON m.Team_1 = t1.Team_Id
    JOIN team t2 ON m.Team_2 = t2.Team_Id
    JOIN win_by ON m.Win_Type = win_by.Win_Id
    WHERE (s.Season_Year, m.Match_Date) IN ( SELECT Season_Year, MAX(Match_Date) AS Last_Match_Date
    FROM matches
    WHERE Season_Id = s.Season_Id
    GROUP BY Season_Year)
    ORDER BY
    s.Season_Year;
 
 ![Snap_Percentage](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/d3dea029-68da-43a2-ad18-0a92abcd07ea)

 
 - Man of the Series each season
    
    (d) SELECT season.Season_Year, player.Player_Name AS ManOfTheSeries
    FROM season
    JOIN player ON season.Man_of_the_Series = player.Player_Id
    ORDER BY season.Season_Year;
 
 
 ![Snap_3](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/23092031-8e71-49df-8ee4-8419995ee3f9)
 
 - Step 18 : The report was then published to Power BI Service.
 # Report Snapshot (Power BI DESKTOP)
 
 
![Publish_Message](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/c908c8ed-8d96-4330-832b-1b878ba5a73b)

# Report Snapshot (Power BI DESKTOP)

![dashboard_snapo](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/e36c75ad-7e14-4965-9ecb-912df20929f7)

 
 # Report Snapshot (Power BI DESKTOP)

 
![Dashboard_upload](https://github.com/rajbhuwan1510/IPL-2008-2016/assets/92216824/8c4be3c0-72a3-48cf-a2de-bee86b4dde16)


