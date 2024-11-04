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

If you want to know more about types of machine learning, you can refer to my recommended <a href="https://www.linkedin.com/pulse/differences-supervised-unsupervised-reinforcement-learning-anu-shreya/"> LinkedIn post</a>.

### Conclusion
In this blog post, I demonstrated how to use web scraping techniques to create a custom dataset on MLB players’ physical attributes and their relationship to home run performance. By gathering data on 200 players from Baseball-Reference.com, I explored how height and weight could provide insights into batting power.

### Try It Out!
Curious about what else you can do with this dataset? You can explore how a player's handedness (lefty, righty, or switch-hitter) affects their batting average. Do left-handed batters really have an edge? Or maybe you can use another dataset for your own curiosity! You can use the same web scraping techniques we covered here to gather more data and try it out yourself. Don’t worry if you’re new to scraping. Just follow the steps in the code, and you’ll be collecting data like an expert in no time!