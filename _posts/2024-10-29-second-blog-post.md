---
layout: post
title: "Breaking Stereotypes: Do Bigger MLB Players Really Hit More Homeruns?"
author: Injoong Kim
description: "Do bigger players really hit more home runs? To answer this, I used web scraping to gather data on the height, weight, and home run stats of the 200 MLB players in 2023! Through this project, I hope you’ll get more familiar with web scraping and maybe even learn a few tips for your own data adventures. Come check it out!"
image:
---

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/batter1.JPG" alt="Batter Image" width="750" height="450">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://commons.wikimedia.org/wiki/File:David-ortiz-batters-box.JPG"> Wikimedia Commons</a></figcaption>
</div>

### Introduction
Baseball is all about the numbers, and fans love to talk about stats. One big question is whether a player’s size makes a difference in hitting home runs. Do taller and heavier players really hit more homers, or is that just a stereotype?

To find out, I decided to use web scraping which is a handy tool in data science that helps us collect information from websites. I’ll be gathering data on MLB players’ heights, weights, and home run counts from a reliable website. This project is a great way to practice web scraping and satisfy a curiosity about how size might affect a player’s batting performance.

### What is Web Scraping?
Web scraping is a method for automatically gathering information from websites. Instead of manually copying data from a webpage, web scraping uses a program to access the webpage, find the specific information you’re looking for (like text, images, or tables), and save it in a structured format, such as a spreadsheet or database. This technique is especially useful for collecting large amounts of data quickly and efficiently, making it a powerful tool in data analysis, research, and many other fields.

### Data Collection
For this project, the main data source is Baseball-Reference.com, a widely respected website that provides comprehensive stats on MLB players. This includes player profiles and season statistics, making it ideal for extracting both physical and performance-related attributes.

Website Used: Baseball-Reference.com (2023 MLB Standard Batting standings and player profile pages)
Final Sample Size: 200 players with the highest number of at-bats during the 2023 MLB season
Variables:
Player Name: Identification
Age:
Home Runs: Player's total number of homeruns in the 2023 season
At Bats: Used to filter 200 players who had the most exposure to hitting opportunities



### Web Scraping Process
우리는 웹스크래핑을위해 BeautifulSoup를 이용할꺼야. 
뷰티풀수프는 이거야.
이웹사이트에서 뷰티풀수프에대한 정보와 기본 syntax를 확인할수있어.
기본적인 syntax는 이거야. 

#### Step 1: Import Necessary Libraries
Before we start, we need to import the required libraries. These libraries will help us send requests to the website and parse the HTML content.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

- **requests**: This library helps us fetch the HTML content of the web page.
- **BeautifulSoup**: Used for parsing and navigating through the HTML structure.
- **pandas**: A powerful data manipulation tool for creating and managing data in tabular form.

#### Step 2: Define the Target URL
We specify the URL of the page we want to scrape.

```python
# URL of the MLB 2023 Standard Batting page
base_url = 'https://www.baseball-reference.com'
main_url = f'{base_url}/leagues/majors/2023-standard-batting.shtml'
```
This URL leads to the MLB 2023 Standard Batting statistics page on baseball-reference.com.

#### Step 3: Fetch the HTML Content
Now, we fetch the HTML content of the page using **requests.get** and parse it with BeautifulSoup. In web development, fetch refers to the process of retrieving data from a server or another source, typically over a network.

```python
# Request the page and parse the HTML content
response = requests.get(main_url)
soup = BeautifulSoup(response.text, 'html.parser')
```

- **requests.get(main_url)**: Sends a request to the URL and retrieves the HTML content.
- **BeautifulSoup(response.text, 'html.parser')**: Parses the HTML content into a format we can easily navigate and search.

#### Step 4: Locate the Relevant HTML Section
We identify the section of the HTML containing the player data using its **div** ID

```python
# Target the specific div for Player Standard Batting
batting_div = soup.find('div', id='div_players_standard_batting')
```

This **div** contains the entire table of player statistics.

#### Step 5: Initialize a Data Structure
We use a dictionary to store unique player records. The dictionary will help us avoid duplicates, especially for players who switched teams mid-season.

```python
# Initialize a dictionary to hold unique player records
unique_players = {}
```

#### Step 6: Extract Player Data
We loop through the table rows and extract player names, ages, and team abbreviations.

```python
if batting_div:
  for row in batting_div.find_all('tr')[1:]:  # Skip the header row
    player_tag = row.find('td', {'data-stat': 'name_display'})
    age_tag = row.find('td', {'data-stat': 'age'})  # Age column
    team_tag = row.find('td', {'data-stat': 'team_name_abbr'})  # Team abbreviation
    at_bats_tag = row.find('td', {'data-stat': 'b_ab'})  # At-bats column
    home_runs_tag = row.find('td', {'data-stat': 'b_hr'})  # Home Runs column
```
I used **age** to distinguish players with the same name. And I used **team names** to identify players who played for multiple teams during the 2023 MLB season. This helped us ensure we gathered the integrated record for each player who switched teams in 2023. And I also extracted the At Bats column to filter out the top 200 who played the most at-bats in the 2023 MLB season.

- **batting_div.find_all('tr')[1:]**: Finds all tr (table row) elements inside the div, skipping the first row (header).
- **row.find('td', {'data-stat': 'name_display'}**): Finds the player's name. Specifically targets a <td> tag where data-stat="name_display". 
For age, team, at bats and home runs codes, same method was used.

#### Step 7: Clean and Store Data
We clean and store the data in a dictionary, ensuring no missing values and handling duplicates.
```python
if player_tag and age_tag and team_tag and at_bats_tag and home_runs_tag:
  player_name = player_tag.text.strip()
  age = age_tag.text.strip()
  team_name = team_tag.text.strip()
  at_bats = at_bats_tag.text.strip()
  home_runs = home_runs_tag.text.strip()

  # Skip rows with missing at-bats or home runs
  if not at_bats or not home_runs:
    continue

  at_bats = int(at_bats)
  home_runs = int(home_runs)

  # Skip the League Average row
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
      'Home Runs': home_runs
    }
```
- Missing data and unwanted rows (like "League Average") are filtered out.
- Player records are uniquely identified by a combination of name and age.

#### Step 8: Handle Multi-Team Players
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
    elif current_team == '2TM' and team_name == '3TM':
      continue  # Keep existing '2TM'
```
In Major League Baseball (MLB), some players may be traded or play for more than one team in a single season. On Baseball Reference, these players have their stats aggregated under team abbreviations like 2TM (for players who played for two teams) or 3TM (for players who played for three teams). This step ensures that we correctly record and update stats for such players.

#### Step 9: Convert to DataFrame
We convert the collected data into a Pandas DataFrame for easier analysis.

```python
df_players = pd.DataFrame(unique_players.values())
```

#### Step 10: Calculate Derived Metrics and Select Top Players
Add a new column for home runs per at-bat and select the top 200 players by at-bats.

```python
df_players['Home Runs per At-Bat'] = df_players['Home Runs'] / df_players['At Bats']
top_200_players = df_players.sort_values(by='At Bats', ascending=False).head(200)
```

- **df_players['Home Runs'] / df_players['At Bats']**: A new metric is calculated to evaluate player efficiency in hitting home runs.
- **df_players.sort_values(by='At Bats', ascending=False).head(200)**: The top 200 players by at-bats are selected for further analysis.

### Conclusion
In this blog post, I demonstrated how to use web scraping techniques to create a custom dataset on MLB players' performance, particularly focusing on their at-bats and home runs. By utilizing Python libraries like requests and BeautifulSoup, I extracted data from Baseball-Reference.com, collecting information on player names, ages, teams, at-bats, and home runs for the 2023 season. This data enabled me to calculate a performance metric, Home Runs per At-Bat, to assess player efficiency in hitting home runs.

Through sorting and filtering the data, I identified the top 200 players with the most at-bats, ensuring the analysis was centered on players with the most at-bats in the 2023 MLB.

### Try It Out!
Curious about what else you can do with this dataset? You could explore how player handedness (left-handed, right-handed, or switch-hitter) affects their batting average. Do left-handed batters have an edge? Or perhaps you'd like to delve into other player stats, like on-base percentage or slugging percentage. Using the same web scraping techniques outlined in this post, you can easily collect additional data and start your analysis. Even if you're new to web scraping, just follow the code, and you'll be gathering and analyzing your custom datasets in no time!