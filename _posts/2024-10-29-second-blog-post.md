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
<p>Baseball is all about the numbers, and fans love to talk about stats. One big question is whether a player’s size makes a difference in hitting home runs. Do taller and heavier players really hit more homers, or is that just a stereotype?</p>
<p>To find out, I decided to use web scraping which is a handy tool in data science that helps us collect information from websites. I’ll be gathering data on MLB players’ heights, weights, and home run counts from a reliable website. This project is a great way to practice web scraping and satisfy a curiosity about how size might affect a player’s batting performance.</p>

### What is Web Scraping?
<p>Web scraping is a method for automatically gathering information from websites. Instead of manually copying data from a webpage, web scraping uses a program to access the webpage, find the specific information you’re looking for (like text, images, or tables), and save it in a structured format, such as a spreadsheet or database. This technique is especially useful for collecting large amounts of data quickly and efficiently, making it a powerful tool in data analysis, research, and many other fields.</p>


### Data Collection
<p>For this project, the main data source is Baseball-Reference.com, a widely respected website that provides comprehensive stats on MLB players. This includes player profiles and season statistics, making it ideal for extracting both physical and performance-related attributes.</p>

Website Used: Baseball-Reference.com (2023 MLB Standard Batting standings and player profile pages)
Final Sample Size: 200 players with the highest number of at-bats during the 2023 MLB season
Variables:
Player Name: Identification
Home Runs: Player's total number of homeruns in the 2023 season
Height: Player's height
Weight: Player's weight
At Bats: Used to filter 200 players who had the most exposure to hitting opportunities


### Web Scraping Process

#### Step 1: Import Necessary Libraries
##### Before we start, we need to import the required libraries. These libraries will help us send requests to the website and parse the HTML content.

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
# Initialize a dictionary to hold unique player records
unique_players = {}
```
I used **age** to distinguish players with the same name. And I used **team names** to identify players who played for multiple teams during the 2023 MLB season. This helped us ensure we gathered the integrated record for each player who switched teams in 2023.

### Conclusion
In this blog post, I demonstrated how to use web scraping techniques to create a custom dataset on MLB players’ physical attributes and their relationship to home run performance. By gathering data on 200 players from Baseball-Reference.com, I explored how height and weight could provide insights into batting power.

### Try It Out!
Curious about what else you can do with this dataset? You can explore how a player's handedness (lefty, righty, or switch-hitter) affects their batting average. Do left-handed batters really have an edge? Or maybe you can use another dataset for your own curiosity! You can use the same web scraping techniques we covered here to gather more data and try it out yourself. Don’t worry if you’re new to scraping. Just follow the steps in the code, and you’ll be collecting data like an expert in no time!