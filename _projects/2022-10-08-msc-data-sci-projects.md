---
title: MSc Data Science Projects
desc: A collection of some of my work from the Data Science Masters course at Lancaster
date: 2022-10-08
emoji: 👨🏻‍🎓
layout: project-page
---

> Note: this page is constantly being updated. Bear with me as I complete this page!

Here I will share some interesting projects I did during my time at Lancaster studying for my Masters in Data Science (distinction). I have summarised the key findings of each project, however I will provide links to download the technical reports where appropriate.

- [Enhancing Phrase Retrieval for Safety Data Sheet Authoring 📃](#yordas)
- [Implementing and Evaluating Neural Networks 🧠](#nn-from-scratch)
- [Predicting Reconviction from a Dataset of Convicted Individuals 👮🏻‍♂️](#reconviction)

### Enhancing Phrase Retrieval for Safety Data Sheet Authoring 📃 {#yordas}

To read the full report, click [here](/assets/2022-10-08-msc-data-sci-projects/placement/thesis.pdf){:target="_blank"}.

My Master's project with Yordas involved integrating contemporary Data Science and Machine Learning techniques to create a series of smart predictive text models, enabling the authoring experts to be presented with smart search suggestions when searching for ~20,000 complex chemical phrases.


### Implementing and Evaluating Neural Networks 🧠 {#nn-from-scratch}

To read the full report, click [here](/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/report.pdf){:target="_blank"}.

As part of my Programming for Data Scientists module for my final coursework, I ambitiously decided to implement a deep neural network completely from scratch and evaluate it against an equivalent implementation in Keras for a binary classification problem using 10-fold cross-validation. I used validation accuracy as my evaluation metric and I computed the mean validation accuracy following 10-fold cross-validation. 95% confidence intervals were also computed for these mean validation accuracies to help compare mean accuracies between the implementation and the Keras versions of the same neural network architecture. I used a dataset of sonar observations and experimented with various hyperparameters (note: I used a fixed mini-batch size of 64 over 2000 epochs to simplify matters):

- Firstly, I experimented with the choice of optimisation algorithm (mini-batch gradient descent, RMSProp and Adam), activation function (Leaky ReLU, ReLU, Sigmoid and Tanh) and learning rate (0.001, 0.01, 0.1, 1). Here I used a single hidden layer with 5 hidden units.
- Secondly, I experimented with the number of neurons by creating 10 different models having 1 to 10 neurons inclusive in the hidden layer, and then added a second hidden layer with neurons ranging from 1 to 10 inclusive. Here I used the ReLU activation function with a learning rate of 0.01 for all model architectures.
- Thirdly, I computed accuracy, precision, recall and F1-score for the classification results for a single hidden layer and 2 hidden layers. I also visualised performance using ROC curves to compare models and used a paired t-test to compare cross-validation accuracy means for the implemented and library versions of the classifier.

To summarise the research questions I formulated:

- **"How does the choice of optimiser and activation function affect validation accuracy across different learning rates?"** We found the Adam optimiser, the ReLU activation function and a learning rate of 0.01 were the optimal set of hyperparameters for the sonar dataset under consideration.

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/mini-batch-gd.png" alt="Mini-batch GD optimiser visualisation" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/rmsprop.png" alt="RMSProp optimiser visualisation" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/adam.png" alt="Adam optimiser visualisation" />
</figure>

- **"How does the number of hidden layers and the number of neurons in each layer affect validation accuracy?"** We found a very slight increase in validation accuracy for the library version when the number of neurons in the second hidden layer was increased. The accuracy for the implemented version varied significantly as the number of neurons increased for both the Adam and RMSprop optimisers. Interestingly, the validation accuracy remained constant at around 53% for the mini-batch gradient descent optimiser in the implemented version which means increasing the number of neurons had no effect on the final validation accuracy. We also found the Adam optimiser achieved a higher validation accuracy compared to the other optimisers for the implemented and library versions. Finally we found the effect of the second hidden layer for the library version did not significantly improve validation accuracy when compared to a single hidden layer containing 10 neurons.

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/1-hidden-layer.png" alt="1 hidden layer visualisation" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/2-hidden-layers.png" alt="2 hidden layers visualisation" />
</figure>

- **"Do the classifier evaluation metrics (accuracy, precision, recall, F1-score) differ significantly for the implemented and library versions of the classifier?"** We found the metrics differed more with 2 hidden layers compared to a single hidden layer.

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/1-hidden-layer-metrics.png" alt="1 hidden layer metrics visualisation" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/2-hidden-layers-metrics.png" alt="2 hidden layers metrics visualisation" />
</figure>

- **"Is there enough statistical evidence to suggest there exists a difference between cross-validation means collected from the implemented and library versions of the classifier?"** We found only the mini-batch gradient descent optimiser showed the cross-validation means were statistically different.

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/stats-evidence-gd.png" alt="GD statistical tests results" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/stats-evidence-rmsprop.png" alt="RMSProp statistical tests results" />
</figure>

<figure style="max-width: 100%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/nns-from-scratch/stats-evidence-adam.png" alt="Adam statistical tests results" />
</figure>

### Predicting Reconviction from a Dataset of Convicted Individuals 👮🏻‍♂️ {#reconviction}

To read the full report, click [here](/assets/2022-10-08-msc-data-sci-projects/predicting-reconviction/report.pdf){:target="_blank"}.

This project involved implementing and evaluating machine learning models to predict reconviction from a range of criminological variables collected for offenders convicted in 1990. Specifically, I used decision trees, logistic regression and a neural network with a single hidden layer, and compared all models against a simple ZeroR baseline classifier (a model which predicts the majority class).

Some observations I found from exploratory data analysis (EDA) were:

- A larger proportion of males were reconvicted compared to females across different age groups.

- A decreasing trend was found in the number of offenders who were reconvicted for both sexes across age groups, with age group 21/22 having the highest count and age group 36/37 having the smallest count.

- The median number of convictions prior to target conviction for reconvicted individ- uals was higher than for those who were not reconvicted. This could suggest the number of convictions prior to target conviction would aid in the prediction of reconviction.

<figure style="max-width: 90%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/predicting-reconviction/eda.png" alt="Exploratory data analysis" />
</figure>

Hence it would be reasonable to suggest both sex and age group of the offender play a key role in determining whether or not an offender is reconvicted.

For the classifier evaluation:

- For the decision tree with all covariates, following repeated cross-validation the number of convictions prior to target conviction (NUMCONV) was found to be the most important feature in determining reconviction. The second most important feature was the time in years from the start of convictions to the target conviction (LENPRECC). All other covariates were discarded following automatic pruning to prevent overfitting. The final model achieved an accuracy of 75.4% on the validation set.

- For the logistic regression model, we found the number of convictions prior to target conviction (NUMCONV), being previously convicted of theft and males in age groups 31/32 and 36/37 (older age groups) were all statistically significant at the 99.9% level. Those previously convicted of burglary or fraud and forgery, having a theft offence at target conviction of either criminal damage or drugs offences, and being male are all statistically significant at the 95% level. We found the odds of an individual reconvicting increases by 16.2% for every additional previous conviction the offender has. For those previously convicted of theft, the odds of an individual reconvicting increases by 65.1%. The final model achieved an accuracy of 74.7%.

- For the neural network model, the optimal architecture following cross-validation of hyperparameters was 2 hidden units in the hidden layer and a decay value of 0.2. The final model achieved an accuracy of 75.1% on the validation set.

<figure style="max-width: 95%;">
 <img src="/assets/2022-10-08-msc-data-sci-projects/predicting-reconviction/tree.png" alt="Decision tree results" />
</figure>

The decision tree was found to be the most ideal model for use in criminal courts, as it resulted in the greatest validation accuracy for the dataset when compared to the other models, and provides a high level of interpretability in terms of the covariates which affect reconviction (as you can inspect which variables contribute to the outcome unlike in neural networks for example). Despite a worse validation accuracy compared to the classification tree, the neural network model may be likely to achieve higher predictive performance on unseen observations due to the model’s complexity. The model accounted for all covariates, whereas the classification tree contained only 2. Hence with a larger sample, it is possible the neural network will surpass that of the classification tree in terms of predictive performance. However, this comes at a cost of a decrease in interpretability as observed in the classification tree.