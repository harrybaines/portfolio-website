---
title: Horizon Scanning
desc: Forecasting updates to substances and regulations using AI
date: 2021-10-18
thumbnail: /assets/images/projects/2021-10-08-horizon-scanning/graph.png
layout: project-page
usemathjax: true
weight: 2
---

Horizon Scanning was a research project I worked on during my time at Yordas Digital which involved investigating the use of AI techniques to forecast probabilities of substances and regulations updating in the next 3 months. Regulatory update data was recorded in internal databases over time since 2008. This data was very valuable - using predictive techniques, we could determine which substances and regulations could update soon (e.g. the next 3 months) based on recorded update data. This enabled us to achieve all of the following:

- Inform clients of which substances in their product were likely to update soon
- Prioritise which internal regulatory checks to perform (e.g. if one substance has a 98% chance of updating in the next 3 months relative to another substance with a 20% of updating, that is very useful information)
- Provide programmatic access to update probabilities through an API

The main research questions we formulated were as follows:

1. Can we develop a model to forecast the likelihood of an update to a regulation in the next 3 months?
2. Can we develop a model to forecast the likelihood of an update to a substance in the next 3 months?

<figure>
 <img src="/assets/images/projects/2021-10-08-horizon-scanning/models.png" width="90%" />
 <figcaption>Model Diagram</figcaption>
</figure>

## Version 1: Machine Learning Methods

The original method used machine-learning-based methods to compute probabilities for all substances and regulations using classification techniques (XGBoost and MLP neural networks). Given the lack of data describing each update, feature extraction techniques were used to create new features for each update, such as the day, month, year, and days since the last update. We utilised a full data science pipeline to process and predict from the data:

<figure>
 <img src="/assets/images/projects/2021-10-08-horizon-scanning/pipeline.png" />
 <figcaption>Data Science Pipeline</figcaption>
</figure>

 After thorough testing, however, the output probabilities were found to be slightly inaccurate and difficult to validate, and given the black-box nature of the algorithms used, the probabilities were uninterpretable.

## Version 2: Poisson Point Process

The second iteration of the system used a much simpler approach. We used the Poisson point process model, which models a series of discrete events where the exact timing of the events is unknown (stochastic) but the average time between events is known [1]. In addition to this, the arrival of an event is independent of the previous event (i.e. the waiting time is memoryless). This means we can get two events on back-to-back days, or two events a year apart from each other.

The Poisson point process has some assumptions:

1. Events are independent (i.e. knowing one event has occurred doesn’t affect the probability another event will occur)
2. The average time between events is the same (i.e. events per time period)
3. Events cannot be simultaneous (i.e. two events can’t happen at the same time)

We can use this model for forecasting substance/regulation updates as we can say updates are independent and not simultaneous, and we can compute the average time between events using the recorded data.

To compute probability scores we can use the Poisson distribution PMF (probability mass function) given our data:

$$
f(x) = \frac{e^{-\lambda}\lambda^x}{x!}
$$

where $$\lambda$$ is the average number of events per interval, and $$x$$ is the number of events in each interval.

A useful equation we can derive from this PMF will allow us to calculate the probability we expect to wait less than or equal to a time:

$$
P(T \leq t) = 1 - e^{-\lambda t}
$$

In this case, we are only interested in whether 1 update occurs in the next 3 months, but we could also calculate the probability a certain number of updates occur in the next $$x$$ months as well.

For example, let’s say the substance 'Formaldehyde' has updated 15 times from 2012-2021. We want to calculate the probability of an update in the next 3 months from today. To do this we first calculate $$\lambda$$, the average number of events occurring in that 9 year timeframe (using months as the unit of time):

$$\lambda = \frac{15}{9 * 12} = 0.1388888889$$

Then, we calculate the probability there is no update in the next 3 months and subtract it from 1:

$$P(T \leq 3) = 1 - e^{-0.1388888889 * 3} = 0.34$$

So there’s a 34% chance of substance X updating in the next 3 months from today.

This procedure was carried out on a daily basis, which created rolling update probability scores for all regulations and substances we monitored, which were then stored in a relational database.

I implemented the procedure in Python and created a Streamlit application to visualise regulation and substance updates through a web browser. This was deployed to our internal server and connected to the live substance database to read regulatory update data from. An example of the output for chromium trioxide in the app is shown below:

<figure>
 <img src="/assets/images/projects/2021-10-08-horizon-scanning/substance_id_1_updates_62_annotated.png" alt="Horizon scanning app" />
 <figcaption>Horizon Scanning Application</figcaption>
</figure>

References:

1. <a href="https://towardsdatascience.com/the-poisson-distribution-and-poisson-process-explained-4e2cb17d459" target="_blank">Useful blog post about Poisson distribution and Poisson process</a>
2. <a href="https://medium.com/@ns2586/using-poisson-distribution-to-forecast-the-next-earthquake-38a4406ab71#:~:text=It%20is%20one%20prominent%20way,statistical%20distribution%20called%20Poisson%20Distribution.&text=It%20is%20a%20discrete%20probability,interval%20of%20time%20or%20space" target="_blank">Useful blog post, predicts next earthquake</a>

