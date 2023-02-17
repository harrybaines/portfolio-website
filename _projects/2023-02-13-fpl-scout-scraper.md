---
title: FPL Scout Scraper
desc: Data scraping of the scout picks from the Fantasy Football Scout website
date: 2023-02-13
layout: project-page
tag: Data Scraping
code_link: https://github.com/harrybaines/FPL-discord-bot
weight: 5
---

As an avid Fantasy Premier League player since 2014, I'm always looking to optimise my team selection to achieve the greatest amount of points over the course of a season.

To aid me in my decision making, I use the Fantasy Football Scout website which provides a set of 'scout picks' for each gameweek. This provides fantasy managers like myself with an idea of which players may be likely to yield a good points total in that gameweek. Most of the time the scout tends to select players who are on a good run of form or play for a team who are likely to win their fixture. This can provide a good starting point for player selection for a given gameweek.

To view the scout picks, you can visit the [fantasyfootballscout.co.uk](https://www.fantasyfootballscout.co.uk/){:target="_blank"} page and they will be presented on the homepage.

<figure>
    <img src="/assets/images/projects/2021-11-05-fpl-scout-scraper/scout-example.png" width="50%" />
    <figcaption>Example Scout Picks</figcaption>
</figure>

The steps required to access these picks are as follows:

1. Open up a web browser.
2. Go to [fantasyfootballscout.co.uk](https://www.fantasyfootballscout.co.uk/){:target="_blank"}.
3. Dismiss the privacy popup notice.
4. Navigate to the scout picks section.

I found carrying out these steps (oftentimes on multiple occasions each week!) to be quite tedious. Therefore, I decided to build an app which automates this process by scraping this data and presenting it to the user via a Discord slash command in my private Discord server. Given that I always have my server open (and is also accessible via my phone), all I would have to do to access the picks is to invoke a single Discord slash command.

To build this app, I created a Discord bot that carries out the following steps:

1. Wait for a slash command (`/scout`) to be invoked.
2. Once invoked, run a Selenium Chrome webdriver in headless mode.
3. Navigate to [fantasyfootballscout.co.uk](https://www.fantasyfootballscout.co.uk/){:target="_blank"}.
4. Find the privacy notice popup accept button and click it to dismiss the popup.
5. Find the scout picks `<div>` element.
6. Take a screenshot of that element and return it to the user.

This saves me a lot of time when I want to check the scout picks!

The bot is deployed on Heroku. View the final result below:

<video width="100%" height="500" controls playsinline muted>
  <source src="/assets/images/projects/2021-11-05-fpl-scout-scraper/bot.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>
