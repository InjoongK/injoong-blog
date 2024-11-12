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
  <img src="https://injoongk.github.io/injoong-blog/assets/img/batter1.JPG" alt="Batter Image" width="600" height="350">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://commons.wikimedia.org/wiki/File:David-ortiz-batters-box.JPG"> Wikimedia Commons</a></figcaption>
</div>

So I want to find a batter who hits the most home runs on a given hitting opportunity, assuming that I have become a scouter. And I want to find a player who has pure strong batting power. I’m focusing on power and efficiency.

## What is Web Scraping?
Web scraping is an automated method to gather information from websites by using a program to extract specific data (like text, images, or tables) and save it in a structured format, such as a spreadsheet or database. You can learn more about Web Scraping at <a href="https://thinkpalm.com/blogs/5-great-ways-web-scraping-tool-helps-businesses/"> this website</a>.

## Data Collection
#### Website Used 
For this project, the main data source is <a href="https://www.baseball-reference.com/leagues/majors/2023-standard-batting.shtml">. baseball-reference.com (2023 MLB Standard Batting standings) </a>

#### Final Sample Size
200 players with the highest number of at-bats during the 2023 MLB season

#### Variables
- **Player Name**
- **Age**
- **Team Name**
- **Home Runs**: Player's total number of homeruns in the 2023 season
- **At Bats**: The number of at-bats counts each time a batter completes a hit, excluding walks, sacrifice bunts, and batting interference.
- **SLG (Slugging Percentage)**: An expected figure of how many bases a batter can get on base after hitting.
- **BA (Batting Average)**: Determined by dividing a player's hits by their total at-bats.

With **Home Runs** and **At Bats**, I will try to get batters' number of **Home Runs per At Bat** to see their efficiency at home runs. And I’ll use **SLG** and **BA** to calculate **ISO** (Isolated Power), which represents a player’s pure slugging power by focusing on extra-base hits (doubles, triples, and home runs). ISO helps show if a player’s slugging depends on high batting averages or frequent extra-base hits.

## Web Scraping Process
Beautiful Soup is a Python package for parsing HTML and XML documents. It provides features to make it easier to scrape information from web pages and is useful for web scraping. You can check the information about Beautiful Soup and the basic syntax on <a href="https://beautiful-soup-4.readthedocs.io/en/latest/"> this website</a>.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Web_Scraping_Process.jpg" alt="Web Scraping Process" width="600" height="350">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://thinkpalm.com/blogs/5-great-ways-web-scraping-tool-helps-businesses/"> ThinkPalm</a></figcaption>
</div>

### Step 1: Import Necessary Libraries
```python
import requests # library that helps us fetch the HTML content of the web page
from bs4 import BeautifulSoup # Used for parsing and navigating through the HTML structure.
import pandas as pd # data manipulation tool for creating and managing data in tabular form
```

### Step 2: Define the Target URL
```python
# URL of the MLB 2023 Standard Batting page
base_url = 'https://www.baseball-reference.com' # specify the URL of the page we want to scrape.
main_url = f'{base_url}/leagues/majors/2023-standard-batting.shtml' # leads to the MLB 2023 Standard Batting statistics page on baseball-reference.
```

### Step 3: Fetch the HTML Content
```python
# Request the page and parse the HTML content
response = requests.get(main_url) # Sends a request to the URL and retrieves the HTML content.
response.encoding = 'utf-8'  # Set encoding to handle special characters like ñ correctly
soup = BeautifulSoup(response.text, 'html.parser') # Parses the HTML content into a format we can easily navigate and search.
```

### Step 4: Locate the Relevant HTML Section
```python
# Target the specific div ID for Player Standard Batting
batting_div = soup.find('div', id='div_players_standard_batting')
```

### Step 5: Extract Data
```python
if batting_div:
  for row in batting_div.find_all('tr')[1:]:  # Finds all tr (table row) elements inside the div, skipping the first row (header).
    player_tag = row.find('td', {'data-stat': 'name_display'}) 
    age_tag = row.find('td', {'data-stat': 'age'})
    team_tag = row.find('td', {'data-stat': 'team_name_abbr'})
    at_bats_tag = row.find('td', {'data-stat': 'b_ab'})
    home_runs_tag = row.find('td', {'data-stat': 'b_hr'})
    slg_tag = row.find('td', {'data-stat': 'b_slugging_perc'})
    ba_tag = row.find('td', {'data-stat': 'b_batting_avg'})
```

- **row.find('td', {'data-stat': 'name_display'}**): Finds the player's name. Specifically targets a <td> tag where data-stat="name_display". 
For other tags, same method was used.

### Step 6: Clean Data
```python
    player_name = player_tag.text.strip('*#')
    age = age_tag.text.strip()
    team_name = team_tag.text.strip()
    at_bats = at_bats_tag.text.strip()
    home_runs = home_runs_tag.text.strip()
    slg_perc = slg_tag.text.strip('.')
    batting_avg = ba_tag.text.strip('.')
```
## Visuals and Insights

### Relationship Between Home Runs and Isolated Power
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Graph1.png" alt="Relationship Between Home Runs and Isolated Power" width="550" height="350">
</div>

It shows a very strong positive correlation. While this might seem obvious, it demonstrates that home run efficiency is a reliable predictor of overall power-hitting ability. The nearly linear distribution of data points suggests a highly consistent relationship between these metrics.

### Top 10 by Home Runs per At Bats
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Graph2.png" alt="Top 10 by Home Runs per At Bats" width="550" height="350">
</div>

Aaron Judge leads the pack, followed closely by Matt Olson and Shohei Ohtani. The top 10 players show remarkably similar performance levels.

### Top 10 by Isolated Power
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Graph3.png" alt="Top 10 by Isolated Power" width="550" height="350">
</div>

Shohei Ohtani ranks first, with Aaron Judge in second place. Notable appearances by Corey Seager and Luis Robert Jr. are exclusively on this list. This suggests these players excel at producing various types of extra-base hits, not just home runs.

## Ethics
This blog post uses data from **baseball-reference.com**, a public site offering baseball stats for fans and researchers. I accessed only allowed sections, respecting robots.txt restrictions and avoiding excessive requests, in adherence to ethical standards for public data use. You can check the robots.txt of baseball-reference.com <a href="https://www.baseball-reference.com/robots.txt"> here</a>.

## Conclusion
In this blog post, I used web scraping techniques to create a custom dataset on MLB players' 2023 performance, focusing on home runs and slugging. Using Python libraries like requests and BeautifulSoup, I extracted player data, including names, ages, teams, at-bats, home runs, slugging percentage, and batting average.

## Try It Out!
Curious about what else you can do with this dataset? You could explore how player handedness (left, right, or switch-hitter) impacts batting averages, or explore other stats like on-base percentage or steal base. Using the same web scraping techniques, you can easily collect more data and start analyzing. You can check my entire code on my <a href="https://github.com/InjoongK/injoong-blog/blob/main/_posts/baseball_data.ipynb"> GitHub repository</a>. Just follow the code, and you'll be creating custom datasets in no time.