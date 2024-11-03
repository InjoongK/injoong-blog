---
layout: post
title: "Breaking Stereotypes: Do Bigger MLB Players Really Hit More Homeruns?"
author: Injoong Kim
description: "Do bigger players really hit more home runs? To answer this, I used web scraping to gather data on the height, weight, and home run stats of the 200 MLB players in 2023! Through this project, I hope you’ll get more familiar with web scraping and maybe even learn a few tips for your own data adventures. Come check it out!"
image:
---

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/AI.jpg" alt="AI image" width="600" height="480">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://commons.wikimedia.org/wiki/File:Artificial_Intelligence,_AI.jpg"> Wikimedia Commons</a></figcaption>
</div>

<p class="intro"><span class="dropcap" style="float: left; padding: 0px; font-size: 94.2857px; line-height: 0px; margin-top: 14.3px; height: 66px;">I<span style="display: inline-block; height: 66px;"></span></span>n the data-driven world of sports, every detail matters, especially in baseball, where statistics and analytics play a massive role in understanding player performance. One question that has always intrigued me is whether a player's physical characteristics have any real impact on their power at batting.</p>
</br>
<p>This project also serves as an opportunity to explore web scraping, a crucial data collection skill in data science. Through this data, I hope to answer the following question. Do physical attributes like height and weight influence the number of home runs hit by MLB players? This question reflects my personal curiosity.</p>

### What is Web Scraping?
Web scraping is a method for automatically gathering information from websites. Instead of manually copying data from a webpage, web scraping uses a program to access the webpage, find the specific information you’re looking for (like text, images, or tables), and save it in a structured format, such as a spreadsheet or database. This technique is especially useful for collecting large amounts of data quickly and efficiently, making it a powerful tool in data analysis, research, and many other fields.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/AI-ML-DL.png" alt="AI subset image" width="452.275" height="500">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://commons.wikimedia.org/wiki/File:AI-ML-DL.svg"> Wikimedia Commons</a></figcaption>

</div>

#### How It Works
- **Algorithms Learn from Data**: Machine learning works by studying lots of data to find patterns. It's like teaching a computer by showing it examples. The more it learns, the better it gets at making predictions.
- **Decision-Making**: Once trained, the model can make predictions or decisions based on new data. Instead of following predefined rules, it uses statistical relationships learned during training to infer outcomes, continuously improving its accuracy over time.

#### Types of Machine Learning

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/MachineLearningTypes.png" alt="Types of Machine Learning" width="770" height="385"> <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://www.linkedin.com/pulse/differences-supervised-unsupervised-reinforcement-learning-anu-shreya/"> LinkedIn</a></figcaption>
</div>

- **Supervised Learning**: In supervised learning, the model is trained using labeled data, meaning each input comes with the correct output. The model learns by comparing its predictions to the actual labels and adjusting until it can accurately predict outcomes.
- **Unsupervised Learning**: In unsupervised learning, the computer gets data without any labels. It has to figure out patterns all by itself, like trying to group things that are similar. 
- **Reinforcement Learning**: Reinforcement learning involves training a model to make decisions through trial and error, and receiving feedback through rewards or penalties.

If you want to know more about types of machine learning, you can refer to my recommended <a href="https://www.linkedin.com/pulse/differences-supervised-unsupervised-reinforcement-learning-anu-shreya/"> LinkedIn post</a>.

#### Real-World Applications
- **Spam Detection**: Gmail uses supervised learning algorithms to filter out spam. The algorithm analyzes email features, such as sender reputation and the presence of certain keywords, to classify emails as spam or not. Over time, it learns from user feedback, improving its accuracy.
- **Recommendation Systems**: Netflix employs collaborative filtering techniques to suggest shows and movies. It analyzes user viewing habits and preferences, comparing them to those of other users to recommend content that similar users enjoyed.
- **Stock Price Prediction**: Financial firms like Goldman Sachs use regression algorithms to predict stock prices based on historical price data, economic indicators, and market sentiment analysis, helping investors make informed decisions.


### Deep Learning: The Advanced Side of Machine Learning
Deep Learning (DL) is a subset of machine learning that uses deep neural networks, which mimic the brain’s structure. These networks consist of multiple layers of neurons, enabling the model to automatically learn complex patterns from large datasets. Unlike traditional machine learning, which requires manual feature extraction, deep learning models excel in tasks like image recognition and speech processing by identifying features directly from the raw data.

<div style="text-align: center;">
  <img src="https://injoongk.github.io/injoong-blog/assets/img/DL_Layers.jfif" alt="Deep Learning Layers" width="740" height="416.25">
  <figcaption style="font-style: italic; color: #5e5e5e;">Source:<a href="https://www.pexels.com/photo/an-artist-s-illustration-of-artificial-intelligence-ai-this-image-was-inspired-by-neural-networks-used-in-deep-learning-it-was-created-by-novoto-studio-as-part-of-the-visualising-ai-pr-17483874/"> Pexels</a></figcaption>
</div>

#### How It Works
- **Mimicking the Human Brain**: Deep learning models are designed using artificial neural networks that mimic the way neurons in the human brain communicate. These networks consist of layers, where each layer extracts increasingly abstract features from the data, allowing the model to build a deep understanding of complex patterns.
- **Complex Tasks**: Deep learning excels at processing vast amounts of data for tasks such as image and speech recognition. The multi-layered structure allows it to identify intricate patterns that traditional algorithms struggle to detect, making it ideal for high-dimensional data. High-dimensional data is data with many different factors, like images, which have color, shape, and texture.

#### Real-World Applications
- **Autonomous Driving**: Tesla's Autopilot system uses deep neural networks to analyze data from cameras and sensors, allowing the car to navigate roads, recognize traffic signs, and make driving decisions. The system continuously learns from millions of miles driven, improving its accuracy and safety.
- **Natural Language Processing**: Google's BERT (Bidirectional Encoder Representations from Transformers) uses deep learning to understand the context of words in search queries. This allows it to deliver more relevant search results and improve voice recognition capabilities for Google Assistant.
- **Predictive Analytics**: Mount Sinai Health System employs deep learning models to predict patient outcomes based on electronic health records. By analyzing historical data, the models can forecast complications, helping healthcare providers tailor treatment plans.


### How Deep Learning Extends Machine Learning
Despite their relationship, machine learning and deep learning have distinct characteristics.

#### Shared Foundations
- Both ML and DL utilize data-driven models and algorithms to learn and make decisions. Data-driven model is a computational model that uses historical data to establish relationships between variables in a system or process

#### Data Requirements
- **Machine Learning**: Can work with smaller datasets.
- **Deep Learning**: Requires large datasets to train effectively.

#### Complexity
- **Machine Learning**: Utilizes simpler algorithms.
- **Deep Learning**: Requires more computational power due to the complexity of neural networks.

#### Feature Extraction
Feature Extraction is a process that identifies important information from data that helps make decisions.
- **Machine Learning**: Often necessitates manual feature engineering.
- **Deep Learning**: Automatically learns features from raw data.

#### Time and Resources
- **Machine Learning**: Models typically take less time to train.
- **Deep Learning**: Models demand more time and computational resources for training.


### Conclusion
In summary, deep learning is an advanced subset of machine learning, extending its capabilities through deep neural networks. Each has its strengths: machine learning is effective for tasks with smaller datasets and simpler algorithms, while deep learning excels in handling large datasets and more complex tasks. As both technologies continue to evolve, they are driving significant advancements in AI, shaping innovations in industries such as healthcare, autonomous systems, and natural language processing.

### Try It Out!
Now that you've learned about machine learning and deep learning, why not explore some videos or articles that demonstrate how to create simple deep learning models? Platforms like <a href="https://www.youtube.com/watch?v=J4Qsr93L1qs"> YouTube</a> and educational websites (such as <a href="https://learning.linkedin.com/"> LinkedIn Learning</a> or <a href="https://www.coursera.org/"> Coursea</a>) offer many tutorials that guide you step-by-step through the process. This will help you see how these concepts work in practice and deepen your understanding!

### What’s Coming Next?
Curious about how to bring your data to life? In the next post, we’ll explore various data visualization tools that can help you effectively communicate insights.