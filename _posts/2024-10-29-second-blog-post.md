---
layout: post
title: "Find MLB Players with the Best Batting Power and Home Run Efficiency"
author: Injoong Kim
description: "Come and check out how I curated a data set of the top 200 players in the 2023 MLB season using web scraping."
image:
---
## Introduction
Are you a baseball fan? What do you love most about the baseball game? For me, there’s nothing more exciting than watching a batter crush a home run. It’s that magical moment when the game can shift in an instant. If your team is ahead, a home run will cement the lead, and if your team is behind, it will spark hope for a comeback and fire up the crowd. 

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/batter1.JPG" alt="Batter Image" width="750" height="450">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://commons.wikimedia.org/wiki/File:David-ortiz-batters-box.JPG"> Wikimedia Commons</a></figcaption>
</div>

So I want to find a batter who hits the most home runs on a given hitting opportunity, assuming that I have become a scouter. And I want to find a player who has pure strong batting power. I’m focusing on power and efficiency.

## What is Web Scraping?
Web scraping is an automated method to gather information from websites by using a program to extract specific data (like text, images, or tables) and save it in a structured format, such as a spreadsheet or database. You can learn more about Web Scraping at <a href="https://thinkpalm.com/blogs/5-great-ways-web-scraping-tool-helps-businesses/"> this website</a>.

## Data Collection
For this project, the main data source is Baseball-Reference.com, a widely respected website that provides comprehensive stats on MLB players. 

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/MLBPSB.png" alt="MLB Player Standard Batting Table" width="900" height="450">
</div>

#### Website Used 
<a href="https://www.baseball-reference.com/leagues/majors/2023-standard-batting.shtml"> baseball-reference.com (2023 MLB Standard Batting standings) </a>

#### Final Sample Size
200 players with the highest number of at-bats during the 2023 MLB season

#### Variables
- **Player Name**: Identification
- **Age**: Used to distinguish players who have the same name
- **Team Name**: Used to uniquely distinguish the records of players who played for different teams in a season
- **Home Runs**: Player's total number of homeruns in the 2023 season
- **At Bats**: The number of at-bats counts each time a batter completes a hit, excluding walks, sacrifice bunts, and batting interference. It’s used to filter the top 200 MLB players with the most at-bats in the 2023 season.
- **SLG**: The SLG (Slugging Percentage) is an expected figure of how many bases a batter can get on base after hitting.
- **BA**: The BA (Batting Average) is determined by dividing a player's hits by their total at-bats. I use it to get ISO (Isolated Power)

With **Home Runs** and **At Bats**, I will try to get batters' number of **Home Runs per At Bat**. Through this, I will be able to see their efficiency at home runs.

And I’ll use **SLG** and **BA** to calculate **ISO** (Isolated Power), which represents a player’s pure slugging power by focusing on extra-base hits (doubles, triples, and home runs). ISO, derived by subtracting batting average from slugging percentage, helps show if a player’s slugging depends on high averages or frequent extra-base hits.

## Web Scraping Process
Beautiful Soup is a Python package for parsing HTML and XML documents. It provides features to make it easier to scrape information from web pages and is useful for web scraping. You can check the information about Beautiful Soup and the basic syntax on <a href="https://beautiful-soup-4.readthedocs.io/en/latest/"> this website</a>.
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Web_Scraping_Process.jpg" alt="Web Scraping Process" width="750" height="450">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://thinkpalm.com/blogs/5-great-ways-web-scraping-tool-helps-businesses/"> ThinkPalm</a></figcaption>
</div>



### Step 1: Import Necessary Libraries
Before we start, we need to import the required libraries. These libraries will help us send requests to the website and parse the HTML content.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```
- **requests**: This library helps us fetch the HTML content of the web page.
- **BeautifulSoup**: Used for parsing and navigating through the HTML structure.
- **pandas**: A powerful data manipulation tool for creating and managing data in tabular form.

### Step 2: Define the Target URL
We specify the URL of the page we want to scrape.

```python
# URL of the MLB 2023 Standard Batting page
base_url = 'https://www.baseball-reference.com'
main_url = f'{base_url}/leagues/majors/2023-standard-batting.shtml'
```
This URL leads to the MLB 2023 Standard Batting statistics page on baseball-reference.com.

### Step 3: Fetch the HTML Content
Now, we fetch the HTML content of the page using **requests.get** and parse it with BeautifulSoup. In web development, fetch refers to the process of retrieving data from a server or another source, typically over a network.

```python
# Request the page and parse the HTML content
response = requests.get(main_url)
response.encoding = 'utf-8'  # Set encoding to handle special characters like ñ correctly
soup = BeautifulSoup(response.text, 'html.parser')
```

- **requests.get(main_url)**: Sends a request to the URL and retrieves the HTML content.
- **BeautifulSoup(response.text, 'html.parser')**: Parses the HTML content into a format we can easily navigate and search.

### Step 4: Locate the Relevant HTML Section and Initialize a Data Structure
We identify the section of the HTML containing the player data using its **div** ID. We use a dictionary to store unique player records. The dictionary will help us avoid duplicates, especially for players who switched teams mid-season.

```python
# Target the specific div for Player Standard Batting
batting_div = soup.find('div', id='div_players_standard_batting')

# Initialize a dictionary to hold unique player records
unique_players = {}
```

### Step 5: Extract Player Data
Loop through the table rows and extract players' info and data that I need.

```python
if batting_div:
  for row in batting_div.find_all('tr')[1:]:  # Skip the header row
    player_tag = row.find('td', {'data-stat': 'name_display'})
    age_tag = row.find('td', {'data-stat': 'age'})  # Age column
    team_tag = row.find('td', {'data-stat': 'team_name_abbr'})  # Team abbreviation
    at_bats_tag = row.find('td', {'data-stat': 'b_ab'})  # At-bats column
    home_runs_tag = row.find('td', {'data-stat': 'b_hr'})  # Home Runs column
    slg_tag = row.find('td', {'data-stat': 'b_slugging_perc'})  # Slugging Percentage column
    ba_tag = row.find('td', {'data-stat': 'b_batting_avg'})  # Batting Average column
```

- **batting_div.find_all('tr')[1:]**: Finds all tr (table row) elements inside the div, skipping the first row (header).
- **row.find('td', {'data-stat': 'name_display'}**): Finds the player's name. Specifically targets a <td> tag where data-stat="name_display". 
For other tags, same method was used.

### Step 6: Clean and Store Data
We clean and store the data in a dictionary, ensuring no missing values and handling duplicates.
```python
    player_name = player_tag.text.strip('*#')
    age = age_tag.text.strip()
    team_name = team_tag.text.strip()
    at_bats = at_bats_tag.text.strip()
    home_runs = home_runs_tag.text.strip()
    slg_perc = slg_tag.text.strip('.')
    batting_avg = ba_tag.text.strip('.')
             
    if not at_bats or not home_runs or not slg_perc or not batting_avg:
      continue
         
    at_bats = int(at_bats)
    home_runs = int(home_runs)

    # make string to float and make it percentage
    slg_perc = float(slg_perc)/1000.0
    batting_avg = float(batting_avg)/1000.0
        
    if "League Average" in player_name:
      continue
            
    # Create a unique key based on player name and age
    player_key = (player_name, age)
            
    # Add or update the player's record in the dictionary
    if player_key not in unique_players:
      unique_players[player_key] = {
      'Player Name': player_name,
      'Age': age,
      'Team': team_name,
      'At Bats': at_bats,
      'Home Runs': home_runs,
      'Slugging Percentage': slg_perc,
      'Batting Average': batting_avg
      }
```

### Step 7: Handle Multi-Team Players
Special handling for players who played for multiple teams in the season.
```python
  else:
    current_record = unique_players[player_key]
    current_team = current_record['Team']
    current_at_bats = current_record['At Bats']
                
    if current_team != '2TM' and team_name == '2TM':
      unique_players[player_key]['Team'] = team_name
     unique_players[player_key]['At Bats'] = at_bats
     unique_players[player_key]['Home Runs'] = home_runs
     unique_players[player_key]['Slugging Percentage'] = slg_perc
     unique_players[player_key]['Batting Average'] = batting_avg
    elif current_team == '2TM' and team_name == '3TM':
      continue  # Keep existing '2TM'
```
Some players may play for more than one team in a single season due to trade. On Baseball Reference, these players have their stats aggregated under team abbreviations like **2TM** (2 Teams) or **3TM** (3 Teams). This step ensures that we correctly record and update stats for such players.

### Step 8: Convert to DataFrame
We convert the collected data into a Pandas DataFrame for easier analysis.

```python
df_players = pd.DataFrame(unique_players.values())
```

### Step 9: Calculate Derived Metrics and Select Top Players
Add new columns and select the top 200 players by at-bats. And I will save it as csv file.

```python
# Add Home Runs per At-Bat column
df_players['Home Runs per At-Bat'] = df_players['Home Runs'] / df_players['At Bats']

# Add Isolated Power column
df_players['Isolated Power'] = df_players['Slugging Percentage'] - df_players['Batting Average']

# Reoder colums
df_players = df_players.loc[:,['Player Name','Age','Team', 'At Bats', 'Home Runs', 'Home Runs per At Bat', 'Slugging Percentage', 'Batting Average', 'Isolated Power']]

# Sort by 'At Bats' in descending order and select the top 200
top_200_players = df_players.sort_values(by='At Bats', ascending=False).head(200)
top_200_players.to_csv('baseball data.csv')
top_200_players
```

## Ethics
This blog post uses data from **baseball-reference.com**, a public site offering baseball stats for fans and researchers. I accessed only allowed sections, respecting robots.txt restrictions and avoiding excessive requests, in adherence to ethical standards for public data use. You can check the robots.txt of baseball-reference.com <a href="https://www.baseball-reference.com/robots.txt/"> here</a>.

## Conclusion
In this blog post, I used web scraping techniques to create a custom dataset on MLB players' 2023 performance, focusing on home runs and slugging. Using Python libraries like requests and BeautifulSoup, I extracted player data, including names, ages, teams, at-bats, home runs, slugging percentage, and batting average. From this, I calculated metrics like Home Runs per At-Bat and ISO to assess home run efficiency and extra-base hitting. By sorting and filtering, I identified the top 200 players with the most at-bats, concentrating on those with the highest playtime in the 2023 season.

## Try It Out!
Curious about what else you can do with this dataset? You could explore how player handedness (left, right, or switch-hitter) impacts batting averages, or explore other stats like on-base percentage or steal base. Using the same web scraping techniques, you can easily collect more data and start analyzing. Just follow the code, and you'll be creating custom datasets in no time. Just remember to adhere to ethical web scraping practices!