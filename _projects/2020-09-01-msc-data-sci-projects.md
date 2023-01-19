---
title: MSc Projects
desc: Some of the work from my Data Science Masters
date: 2020-09-01
thumbnail: /assets/images/projects/2020-09-01-msc-data-sci-projects/lancaster.png
layout: project-page
weight: 4
---

Here I will share some of the most interesting projects I did during my time at Lancaster studying for my Masters in Data Science (distinction). I have summarised each project, however I will provide links to download the technical reports where appropriate.

- [Enhancing Phrase Retrieval for Safety Data Sheet Authoring](#yordas)
- [Implementing and Evaluating Neural Networks](#nn-from-scratch)
- [Studying the Effect of Feature Extraction, Annotation and Preprocessing Methods for Supervised Twitter Sentiment Classification](#nlp-techniques)
- [Predicting Reconviction from a Dataset of Convicted Individuals](#reconviction)
- [Digging Grantmaking Data](#grantmaking-data)
- [Analysing Patient Data using Clustering Methods](#patient-data)
- [Exploring Relationships between Body Dimensions in Physically Active Individuals](#body-dimensions)
- [Exploring Risk Factors for Diabetes](#diabetes)
- [Analysing and Predicting the Size of Blackbirds using EDA and Linear Regression](#blackbirds)
- [Applying Data Preprocessing, Clustering and Classification Algorithms to Mushroom, Abalone and Pulsar Datasets](#data-mining)
- [Building Big Data Systems Project](#big-data)


### Enhancing Phrase Retrieval for Safety Data Sheet Authoring {#yordas}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/placement/thesis.pdf){:target="_blank"}.

My Master's project with Yordas involved integrating contemporary Data Science and Machine Learning techniques to create a series of smart predictive text models, enabling the authoring experts to be presented with smart search suggestions when searching for ~20,000 complex chemical phrases.


### Implementing and Evaluating Neural Networks {#nn-from-scratch}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/nns-from-scratch/report.pdf){:target="_blank"}.

As part of my Programming for Data Scientists module for my final coursework, I ambitiously decided to implement a deep neural network completely from scratch and evaluate it against an equivalent implementation in Keras for a binary classification problem using 10-fold cross-validation. I used validation accuracy as my evaluation metric and I computed the mean validation accuracy following 10-fold cross-validation. 95% confidence intervals were also computed for these mean validation accuracies to help compare mean accuracies between the implementation and the Keras versions of the same neural network architecture. I used a dataset of sonar observations and experimented with various hyperparameters (note: I used a fixed mini-batch size of 64 over 2000 epochs to simplify matters).

### Predicting Reconviction from a Dataset of Convicted Individuals {#reconviction}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/predicting-reconviction/report.pdf){:target="_blank"}.

This project involved implementing and evaluating machine learning models to predict reconviction from a range of criminological variables collected for offenders convicted in 1990. Specifically, I used decision trees, logistic regression and a neural network with a single hidden layer, and compared all models against a simple ZeroR baseline classifier (a model which predicts the majority class).
### Studying the Effect of Feature Extraction, Annotation and Preprocessing Methods for Supervised Twitter Sentiment Classification {#nlp-techniques}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/nlp-techniques/report.pdf){:target="_blank"}.

Sentiment classification has become a popular automated method for extracting sentiment from online text. In this paper we explore this topic, and evaluate how a range of techniques employed prior to classification can enhance performance. Using well-known machine learning methods, notably Naïve Bayes, Logistic Regression and Linear Support Vector Machines, we conduct a thorough experimental analysis of the combinations of different preprocessing, annotation and feature extraction methods using these models, including a comparison of the effect each combination has on sentiment classification performance. We conclude by identifying the models and their hyperparameters which yield high validation accuracies following cross-validation and evaluate model performance on test datasets. We find the use of lemmatisation during the annotation phase generally weakens performance compared to when it is not used, and we find including stopwords results in higher performance compared to when they are removed.

### Digging Grantmaking Data {#grantmaking-data}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/grantmaking/report.pdf){:target="_blank"}, to view the oral presentation click [here](/assets/2020-09-01-msc-data-sci-projects/grantmaking/slides.pdf){:target="_blank"}, or to read in blog post format, click [here](/2019/11/14/digging-grantmaking-data/).

As part of my Data Science degree, I took a module named Data Science Fundamentals in which I partook in a group project. The group was composed of 6 members in total; we were allocated a project with 360Giving which involved the analysis of grant data published by funders in the UK. This turned out to be a complex project, which not only allowed us to exercise our skills in mining large datasets but also provided an opportunity to explore new technologies whilst being able to tackle a real-world problem. In this post, I will give a general overview of the work that was undertaken by our group, with an emphasis on the machine learning aspects I worked on to answer our first research question.

### Analysing Patient Data using Clustering Methods {#patient-data}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/patient-data/report.pdf){:target="_blank"}.

Clustering methods can be used to facilitate the understanding of a dataset. In this report we apply both distance-based and model-based clustering algorithms to the data to aid in understanding of the dataset, such that we can identify distinct groups of observations to gain insight into the issues facing hospital patients based on a set of quality of life variables. The data used in this report consists of 22 quality of life variables, measured on a 5-point likert scale, including an additional 3 variables collected from 377 hospital patients (see R code for explanations of these variables). The primary objective of this report is to extract insights from this data using cluster analysis to determine if distinct groups of respondents can be found. An interpretation of the clusters formed will be given to aid understanding of the issues facing the hospitals patients. We hypothesise that there exists at least 2 distinct sets of respondents, with one set representing those patients who experience more severe factors affecting their qualities of life compared to those experiencing more milder factors.
Extensive research has been undertaken in the area of analysing patient data using various clustering techniques, such as partitioning around medoids to compare symptom cluster phenotypes in patients with cancer and various kidney diseases and using k-means clustering to cluster healthcare data. We utilise both of these techniques in this report, as well as model-based clustering techniques.

### Exploring Relationships between Body Dimensions in Physically Active Individuals {#body-dimensions}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/body-dimensions/report.pdf){:target="_blank"}.

Multiple linear regression was carried out to investigate the relationship between a person’s weight and a range of body measurements for physically active individuals. We fit a regression model to accurately predict a person’s weight based on multiple explanatory variables. We performed stepwise model selection to identify the most parsimonious model and identify interaction effects between the variables. Regression diagnostics were used to evaluate the model assumptions - the data met the assumptions of homogeneity of variance and linearity, and the residuals were approximately normally distributed. Through application of the Box-Cox transformation, we found that modifying the weight by the fourth root further improved model fit.


### Exploring Risk Factors for Diabetes {#diabetes}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/diabetes/report.pdf){:target="_blank"}.

Diabetes is a major lifelong disease which affects millions of people worldwide today. In this paper, we seek to explore and identify the risk factors for Diabetes by implementing a generalised linear model to predict if a patient has Diabetes. A dataset provided by the National Institute of Diabetes and Digestive and Kidney Diseases will enable us to fit a logistic model to predict if an individual is diabetic based on a set of diagnostic measurements.

### Analysing and Predicting the Size of Blackbirds using EDA and Linear Regression {#blackbirds}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/blackbirds/report.pdf){:target="_blank"}.

It has been shown that a reasonable estimate of the size of a blackbird can be obtained from measurements of its wing, given that the weight of a blackbird varies during the course of the year. In this paper, we begin by utilising a common data mining technique known as Exploratory Data Analysis (EDA) to analyse a data set of blackbirds collected in a garden over a period of 25 years. This involves understanding the types of variables in the data, calculating summary statistics and using visual methods to draw further insights from the data. Following EDA, we quantify the effect of the variables on the size of a blackbird using a quantitative method known as linear regression, which enables us to discover linear relationships between dependent and independent variables. We investigate how these variables affect the size of a given blackbird using the fitted linear model and highlight ones with the most significance. We will use R, a statistical analysis package to conduct our EDA and implement the linear regression model. It was found that the variables which influence the size of a blackbird were the age, weight and sex of the blackbird, with the time of year also influencing its size.

### Applying Data Preprocessing, Clustering and Classification Algorithms to Mushroom, Abalone and Pulsar Datasets {#data-mining}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/data-mining/report.pdf){:target="_blank"}.

This paper explores both supervised and unsupervised machine learning techniques, by implementing and evaluating supervised classification models trained on mushroom and abalone datasets, and unsupervised clustering algorithms trained on pulsar data. Before applying these algorithms, the data was preprocessed using traditional data preprocessing algorithms such as normalization and standardization. Principal Component Analysis (PCA) facilitated dimensionality reduction and extraction of new, orthogonal features. Synthetic Minority Over-sampling Technique (SMOTE) was applied to the abalone data to overcome the high-class imbalance problem. Imputation enabled missing values to be estimated for the mushroom data. We utilised the Python programming language to facilitate the implementation of data preprocessing algorithms and evaluation of classification and clustering algorithms. We find that the k-means algorithm performs best when separating data points into clusters, and the logistic regression classifier performs best for predicting the positive class for both abalone and mushroom datasets.

### Building Big Data Systems Project {#big-data}

To read the full report, click [here](/assets/2020-09-01-msc-data-sci-projects/big-data/report.pdf){:target="_blank"}.

This group project involved understanding the operational behaviour of one of Google's datacenter clusters. They wanted to better understand their user and software behaviour that executes within their cluster. The data used was a lightly modified version of operational trace data from one of Google’s production clusters (12,500+ servers). The trace data captured a wide selection of characteristics pertaining to task resource utilization, submissions patterns, as well as software and hardware failures.