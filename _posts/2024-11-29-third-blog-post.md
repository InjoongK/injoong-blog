---
layout: post
title: "Analyzing MLB Power Hitting Through Strength and Efficiency"
date: 2024-11-29
author: Injoong Kim
description: "Come see how I analyzed the top 200 players' power metrics like Home Runs per At-Bat and Isolated Power from the 2023 MLB season! Plus, explore it with my interactive Streamlit app!"
image:
---
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/hitter_homerun.jpg" alt="hitter homerun" width="800" height="450">
</div>

## Introduction
From the previous blog, we explored MLB player performance for the 2023 season, focusing on home run efficiency and pure batting power. In this blog, I will share some insights from our last blog, and I'm going to introduce an app called Streamlit that uses Python to help us explore our dataset more interactively.

## Key Insights from the Data
#### 1. Who are the most efficient power hitters?
One key metric I explored is Home Runs per At Bat (HR/AB), which highlights batters' efficiency in hitting home runs relative to their opportunities at the plate. For example, **Aaron Judge** emerged as a top performer, with a remarkable HR/AB rate that sets them apart from other players.

The visualization below shows the top players ranked by HR/AB, offering a glimpse into how certain players dominate this efficiency metric.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/HR_AT-BAT.png" alt="Homeruns Per At Bats Graph" width="800" height="400">
</div>

#### 2. Isolated Power (ISO) reveals a different angle
While HR/AB emphasizes efficiency, Isolated Power (ISO) provides insights into a batter’s raw slugging potential by focusing on extra-base hits. Interestingly, some players who excel in HR/AB rank lower in ISO, reflecting differences in overall hitting styles and strategies.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/ISO.png" alt="Isolated Power Graph" width="800" height="400">
</div>

#### 3. Correlation between HR/AB and ISO
To examine the relationship between Home Runs per At-Bat (HR/AB) and Isolated Power (ISO), I analyzed their correlation and observed a strong positive association. The scatterplot shows that players who consistently hit home runs relative to their opportunities tend to exhibit higher ISO values. This trend underscores the interconnected nature of these metrics, as both reflect a player’s ability to deliver impactful, power-driven hits.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Cor_HR_ISO.png" alt="Correlation of Homeruns Per At Bats and Isolated Power Graph" width="800" height="400">
</div>

## The Streamlit App
### What is the Streamlit?
Streamlit is a tool for building and sharing interactive data apps easily, even if you’re new to programming. With just a few lines of Python, you can create dashboards or data visualizations that look professional and work in real time. It’s perfect for projects like exploring datasets or analyzing trends interactively. Streamlit handles layouts, widgets, and visualizations for you, so you can focus on your data and insights rather than worrying about designing a website or interface.

### Purpose
The MLB Power Hitting Analysis App is a great tool for baseball fans, analysts, and coaches who want to explore hitting stats in a way that's both fun and easy to use interactively. Whether you want to identify top performers, analyze team dynamics, or customize your own analyses, this app offers a user-friendly platform to dive into the data.

### Interactive Features
My Streamlit app offers the following interactive features.

#### Power Metrics Overview
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Power_Metrics_Overview.png" alt="Power Metrics Overview" width="800" height="400">
</div>

- **Top Players by HR/AB & ISO**: View rankings and stats of players by their Home Runs per At Bat (HR/AB) and Isolated Power (ISO).
- **Number of players to display**: Allows users to control how many top-performing players to show in the Power Metrics Overview section. By entering a value, users can adjust the number of players displayed in the rankings based on metrics like Home Runs per At Bat (HR/AB) or Isolated Power (ISO).
- **Player Search**: Let users search for specific players by name. Users can type a player's name in the search box, and the app will show a list of matching players. If there are multiple results, users can select the player they are interested in, and the app will display detailed stats for that player.
- **Correlation Analysis**: Visualize the relationship between Home Runs per At Bat (HR/AB) and Isolated Power (ISO) using a scatter plot. This analysis helps users explore how these two power-hitting metrics are related. The app also includes a trendline that highlights any patterns in the data, allowing users to identify potential correlations between a player's efficiency in hitting home runs and their overall power.

#### Team Power Analysis
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Team_Power_Analysis.png" alt="Team Power Analysis" width="800" height="400">
</div>

The Team Power Analysis feature allows users to compare MLB teams based on various power metrics. Users can select between Average HR/AB, Average ISO, or Total Home Runs to analyze and compare team performance. The app presents these metrics in bar charts, helping users identify which teams excel in specific areas, such as hitting home runs efficiently or generating isolated power.

#### Custom Analysis
<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/Custom_Analysis.png" alt="Custom Analysis" width="800" height="400">
</div>

The Custom Analysis feature in the app allows users to create their own visualizations by selecting metrics for both the X and Y axes. For example, users can compare any two metrics, such as Home Runs vs. Batting Average or ISO vs. Age, to explore relationships between them. This feature also includes a regression line to reveal trends, helping users uncover patterns in the data.

#### User Example
You can compare HR/AB with batting average to see if players with high power metrics tend to have lower batting averages, or if there’s a balance between power and consistency. This analysis can reveal whether power hitters sacrifice contact for home runs or manage to maintain a solid batting average while hitting for power.

Or you can identify undervalued power hitters whom with consistent HR/AB rates but fewer home runs. These players may not hit the most home runs, but their ability to hit home runs efficiently can make them valuable in certain lineups, contributing in ways that might not be immediately obvious from their total home run count.

## Explore the App
Ready to find hidden insights in MLB data? Access the Streamlit app <a href="https://injoongk-streamlit-blog-post-3-streamlit-rws7j6.streamlit.app"> here </a>. For those interested in the code, you can find it in my <a href="https://github.com/InjoongK/Streamlit/blob/main/blog_post_3_streamlit.py"> GitHub Repository</a>.

## What’s Next?
In this blog post, I explored MLB player performance for the 2023 season, focusing on home run efficiency and pure batting power, but now it’s your turn to dive deeper! Can you identify hidden trends in the data that I may have missed? Or perhaps you can explore how player attributes beyond just HR/AB and ISO  influence their power metrics. Challenge yourself to experiment with different combinations of metrics and share your findings.

And you can easily make your own streamlit app, too. See the <a href="https://docs.streamlit.io/"> document tab on the Streamlit website </a>.