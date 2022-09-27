---
title: Digging Grantmaking Data
desc: Understanding grant data with machine learning
date: 2019-11-14
thumbnail: /assets/images/360-giving.png
layout: project-page
---

As part of my Data Science degree I took a module named Data Science Fundamentals in which I partook in a group project. The group was composed of 6 members in total; we were allocated a project with 360Giving which involved the analysis of grant data published by funders in the UK. This turned out to be a complex project, which not only allowed us to exercise our skills with mining large datasets, but also provided an opportunity to explore new technologies whilst being able to tackle a real-world problem. In this post I will give a general overview of the work that was undertaken by our group, with an emphasis on the machine learning aspects I worked on to answer our first research question.

## Introduction

<a target="_blank" href="https://www.threesixtygiving.org/">360Giving</a> is a charity which supports grant-making bodies from the UK to publish grant data about who, where and what they fund in a uniform and consistent way. Their vision is for grant-making to be more informed, effective and strategic. All grants are accessible through <a href="https://grantnav.threesixtygiving.org/" target="_blank">GrantNav</a>, a publicly available database containing over 348,000 grants with 198,000 recipients and 115 funders, ranging from private foundations to The National Lottery, and individual charities themselves. In order to have a better understanding of the data in the GrantNav, we chose The Co-operative Group from the 115 registered funding bodies as our research target. This organization is one of the world’s largest consumer co-operatives, owned by its members who decide what they would like to support from 4,000 local causes located throughout the UK. 

The research questions for this project were the following:

1. What themes of grant types are awarded by the Co-operative Group?
2. Where are the local causes situated? Is there correlation between themes and locations?
3. Is there any geographical variation in the amount awarded to recipients based on theme?

I was mainly responsible for the first research question regarding the different types of grants awarded by the Co-operative Group. As a group we decided to divide the workload amongst all members, with each member focussing on their area of expertise - this would enable us to answer our research questions more efficiently, and give us time for all members to explore other questions once their work is complete. 

## Research Question 1

*What themes of grant types are awarded by the Co-operative Group?*

Following some basic exploratory data analysis, we discovered the subset of data we were working with contained 12021 rows (grants) and 79 columns, most of which were sparsely populated or descriptive, particularly geographical columns. Grants published in the 360Giving standard are required to have a title and description describing that particular grant. However, the grants were not given any themes/topics, so our first challenge was to decide how to categorise these grants into a set of themes which are representative of all grants we are dealing with. Each grant should be assigned to excatly one theme. This initially involved looking through the textual descriptions of the grants and deciding on appropriate names, alongside a list of typical grant areas we found online. We had to ensure themes were specific (e.g. education, healthcare) without being too vague or superficial. We initially decided to use very generic names for the themes, however a group member would come across a grant which couldn't be assigned to one of our predefined themes. This was a real issue, and hence we required a more sophisticated and automated solution to solve this problem.

Following an further group discussion, we decided the title and description columns provided sufficient information to determine the themes for all grants as they were mandatory columns to complete. Given we were dealing with textual data, a possible solution was to use Natural Language Processing (NLP) and machine learning to automatically assign themes to grants based on their textual descriptions. This problem was inherently unsupervised in nature, given we were not provided with labels (i.e. the themes for the grants). I envisiged an unsupervised machine learning classifier which could classify all grants into a set of labels/themes automatically would solve this problem; this is the technique we ultimately pursued. 

A challenge with this approach however was simply the complexity of the task. In machine learning this is more commonly known as Topic Modeling, in which we provide a set of inputs and output topics, and the algorithm assigns all inputs to their most appropriate output topic. Given the timescale for this project spanned only a few weeks, we felt developing a solution from scratch would not only take too much time, but a custom solution may not necessarily provide the best categorisation of grants. After extensive research I discovered the <a href="https://cloud.google.com/natural-language" target="_blank">Natural Language API</a> provided by the Google Cloud Platform (GCP). This would enable me to create a Python script to interact with the API programmatically and obtain themes for all grants using the text classification feature of the API without requiring labels. The main caveat however was the cost, but thankfully being a student I was given $300 dollars of free credits to work with - happy days! 

I was eager to try out the text classification provided by GCP on our data, so after following some tutorials on how to interact with the API, I used one grant as an example to get a feel for the accuracy we would expect to obtain for the other grants. We chose a grant relating to a golf course. Interestingly, the grant didn't contain the word 'golf' anywhere in its description, however the result we obtained from the API not only managed to classify it as sport correctly, but managed to classify it further as golf! This is really great as it managed to understand the textual description with a high level of accuracy - all group members were amazed with this level of accuracy.

I went away and developed a Python script which loaded all Co-operative Group grants into a list, with the title and description columns representing each grant. It was then very straightforward to call the Natural Language API and receive a response, which contained a list of categorised topics, with a confidence score assigned to each. The topic with the highest confidence score would be used as the classification for that particular grant. All this logic was automated using Python. I discovered however that not all grants were assigned a topic - it turns out some textual descriptions were really vague, with some Welsh grants not being classified at all (apparently the Natural Language API doesn't support Welsh!). 

Ideally, we wanted all grants to have a theme assigned to them. The size of our data consisted of only 12K grants, so it was in our best interest to classify all of them to gain as much information as possible in future analysis. After further research, I stumbled across Google AutoML, a machine learning platform which would allow us to use our already labelled grants as a training set and the unlabelled grants as the set to predict labels for. The algorithm would then assign labels to the test set which it thinks best represents them based on our already labelled data. This technology was very easy to use - input the data and labels to train the algorithm, and wait until training completes. This did take several hours however, but once complete we had a trained model with obtained reasonable classification metrics. We achieved 83% average recall for all themes (i.e. an average of 83% of the grants in each theme were correctly identified/recalled by the model) and 79% average precision (i.e. the classifier could predict, on average, 79% of grants in each theme correctly). Using the unlabelled grants with the trained model we finally achieved a fully labelled dataset. All results were exported to a single CSV file, containing the textual descriptions and the assigned topic which would be used for further analysis.

Following grant classification, other group members were able to proceed with the following 2 research questions they were assigned to, which involved creating geographical visualisations of the grants according to their themes, and noticing any geographical variation according to them.

## Research Question 2

*Where are the local causes situated? Is there correlation between themes and locations?*

We created a heat map to show the frequency and the total amount of grants awarded by The Co-operative Group over the past 2 years in different places across the UK:

<div class="custom-image" style="max-width: 400px;">
  <img src="uk-vis.png" alt="UK Heat Map Visualisation of total amount of grants awarded by the Co-operative Group" />
  <figcaption>UK heatmap visualisation</figcaption>
</div>

Each blue point in the map indicates a grant. A darker point denotes higher amount awarded, with lighter ones denoting grants with lower amounts. The plot provides evidence that larger UK cities have a higher concentration of grants, for example London and Manchester, and they both have a large concentration of grants compared to other cities. This may be due to the fact that these cities host many charity’s headquarters, and so the recipient of the grant is based’in these larger cities. This may also be due to the fact that larger cities come with a greater population size, typically with more deprived areas and thus community initiatives that the Co-operative Group typically funds.

The following chart shows the top 3 themes in each country and their proportions: 

<div class="custom-image" style="max-width: 750px;">
  <img src="vis-bar.png" alt="Themes Bar Chart" />
  <figcaption>Top 3 themes in each country</figcaption>
</div>

From the chart we see that grants in Northern Ireland are mostly categorized as Community (38.09% of all grants). Wales has the most grants relating to Hobbies and Leisure (45.26%). Health related grants dominate in Isle of Man with 48.35% of grants labeled as Heath. Grants categorized as both Community and Hobbies and Leisure in Scotland have 36.95% and 34.5% of total grants respectively. England has a more balanced category of grants with 36.62% of Jobs, 34.35% of Hobbies and Leisure and 29.03% of Community.

These 5 countries can be classified into 2 categories: one category containing Scotland and Isle of Man where Health related grants are one of the major grants, and the other category containing England, Wales and Northern Ireland, who all have grants of Hobbies and Leisure as their one of major grants instead of Heath related grants. This can be explained by the fact that different countries may have different dominating industries. In Scotland and Isle of Man, Health related industries may have a dominant power. In England, Wales and Northern Ireland, recreation related industry may be more prosperous than other countries.

## Research Question 3

*Is there any geographical variation in the amount awarded to recipients based on theme?*

In the following plot we show the geographical variation of themes for the largest recipients of grants by the Co-operative Group:

<div class="custom-image" style="max-width: 750px;">
  <img src="map-vis.png" alt="Geographical Varation Chart" />
  <figcaption>Major recipients geographically plotted based on theme and amount awarded</figcaption>
</div>

The size of the bubble for different cities denotes the amount awarded to an organization and the colour of the bubble denotes the theme. The recipient organizations who receive multiple grants do not necessarily receive grants in the same theme. For example, The British Red Cross receives grants for both Hobbies and Leisure as well as Support Groups. But it receives major grants for Hobbies and Leisure (£0.14 Million in 2017), so we have shown the British Red Cross belonging to the Hobbies and Leisure theme here. On the contrary, some organization’s grants belong solely to one theme. For example, Alzheimer Scotland and North Aberdeenshire Services receive grants for the Health theme only.

Since the British Red Cross was the largest recipient in 2017 (£256K) and its main headquarters are located in London, we observe that there is a large bubble around London belonging to the Hobbies and Leisure theme. Similarly, The Co-op Foundation has been the largest recipient of the grants in the year 2018 (£238K) with their main headquarters in the Greater Manchester Area, explaining the large bubble around Manchester belonging to the Community theme. These were the default local causes in each of the 2 years, suggesting there was substantial funding here. In contrast, other organizations like Girlguiding have different locations across the UK, and so their grants are more spread across different geographical regions. In total they receive more funding but are allocated into many smaller amounts.

It can be seen that Scotland has 2 major recipient organizations: Alzheimer Scotland – North Aberdeenshire Services (Health) and Arbroath Lifeboat Station (Community). This is also concluded from the analysis of our second research question that Health and Community are among top 3 major themes for grant recipients in Scotland.

The final visualisation we produce shows the total amount awarded in the top three themes for countries in the UK:

<div class="custom-image" style="max-width: 750px;">
  <img src="country-theme-bar.png" alt="Amount awarded in the top three themes of each country" />
  <figcaption>Amount awarded in the top three themes of each country</figcaption>
</div>

The visualisation shows that Scotland and Wales had majority of their grants being awarded to Hobbies and Leisure theme, with approximately £0.5 million and £262K respectively. On the contrary, Isle of Man had significantly lower preference for grant giving to Hobbies and Leisure and majority of its grants were awarded to Healthcare, amounting to £35K. This amount is much lower compared to Scotland and Wales, but accounting for the relative sizes of the countries, the number of recipients are also lower in Isle of Man. England received grants valued £3.5 million alone for Jobs and Education, which is the highest among all the countries. Thus we can observe an inclination on investing towards Jobs and Education for England, community for Northern Ireland, Hobbies and Leisure for Wales and Scotland and healthcare for Isle of Man.

## Conclusion

With The Co-operative Group frequently changing its application criteria for local causes and only two years of historical data currently being provided in GrantNav, it is difficult to investigate the themes over time and to predict the themes that individuals will support in the future as the application criteria changes. Therefore in further work, we could generalise to other times and reduce threats to validity following the release of The Co-operative Group local cause pay-outs in 2019, which would give an additional year of data and allow more forward-looking analysis to be performed. Alternatively we could select an alternative funder to see whether our findings can be generalised.

We have seen that Hobbies and Leisure, Jobs ans Education, and Community are most granted themes for years 2017-2018 together. Of the total value of £56.3 million, these 3 themes alone receive 43% of the total grants being awarded. This is also supported by the fact that the top 2 grants recipients for the year 2017-2018, The British Red cross and the Co-op foundation, typically recieve Hobbies and Leisure and Community related grants respectively. However at a microscopic level, the preference of themes for grants changes considerably. For Wales and Scotland, Hobbies and Leisure were the most awarded themes whereas for the Isle of Man, Healthcare received the maximum grants. Thus we can conclude that there is significant geographical variation in the amounts awarded for each theme for different countries.

In terms of grant labelling we envisage future work could involve training a supervised machine learning classifier on a larger portion or even the entire GrantNav database in an attempt to improve classification accuracy. Developing a bespoke classifier from scratch is also a potential area to explore, given we utilised an off-the-shelf solution (Google's Natural Language API). We also believe the set of labels we produced could be further subdivided into separate labels in order to better understand the distribution of grants across a broader range of themes.

Overall this was a great experience, working as a team helped us develop our team working skills, and working on a real-world project ensured our work was relevant to contemporary Data Science. Following our presentation and report submission, we received a distinction.