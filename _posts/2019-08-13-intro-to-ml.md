---
title:  Introduction to Machine Learning
layout: post-page
---

Recently I took an online course called 'Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning' on Coursera. I found it to be a fantastic learning resource for AI enthusiasts like myself who want to learn more about the subject. In this post I want to summarise the fundamentals of what machine learning actually is, how it works and some applications where this technology can be applied to in the real world.

## What is Machine Learning?

To understand what machine learning is we first need to understand traditional programming. Let's say you want to create a computer program that calculates a person's BMI (Body Mass Index). This can be expressed mathematically as follows:

```python
BMI = weight / (height * height)
```

This equation (or rule) takes 2 variables, weight and height, and divides the person's weight in kilograms by the square of the person's height in metres. For simple problems like this, it is easy to define such a rule in a programming language like Python:

```python
def calc_bmi(weight, height):
  return weight / (height ** 2)
```

But let's say we have a more difficult problem, for example face recognition in images. How could we create rules to determine if a face is in an image or not? Everyone's faces are slightly different, so it would become very difficult to define these rules manually in code.

Below is a summary of what traditional programming looks like:

```python
Rules + Data = Results
```

Once we have defined our rules and input some data we can obtain our results. Machine Learning takes a different approach:

```python
Results + Data = Rules
```

The ML approach flips the original equation around. We input the results we expect to obtain given some input data and we obtain the rules. This means the machine is given the correct answers for a set of data and the computer 'learns' the relationship mapping from the input data to the results we provide. Let's say we have the following set of data:

```python
X = [ 1, 2, 3, 4, 5 ]
Y = [ 3, 8, 13, 18, 23 ]
```

What is the relationship between X and Y? To solve a problem like this we (humans) look at an element of X, say 1, and try adding/multiplying a number to get to the corresponding Y value. For X=1 we might try 2X + 1 which yields 3 correctly, however this rule doesn't work for X=2, so we must adjust our rule to ensure we obtain Y=8 when X=2. After attempting numerous equations we eventually come to Y=5X - 2.

Doing this manually however is tedious and becomes very difficult to compute manually with large datasets, and with varied datasets it may become difficult to manually infer mappings from input to output. What we can do instead is feed these inputs and outputs to a machine learning algorithm which will infer the rules for us!

This is more commonly known as a supervised learning problem, given we have our output labels and the algorithm tries to learn the mapping from the inputs to the labels. Another approach is unsupervised learning, where the algorithm tries to form its own representation of the data without requiring output labels. Other forms of learning exist, such as reinforcement learning, where an 'agent' takes actions in its environment to maximise a utility function, and is commonly seen in robotics and simulated environments.

Going back to the face recognition example if we are given a set of images, we can label each image to say if a face is present in the image or not (e.g. a yes or no label). An ML algorithm can be 'trained' on this labelled dataset and infer the rules for us, so that when presented with a new, unseen image it can tell us with a given level of confidence how likely it is a face is present in the new image!

The rise in the amount of available data present has been one of the main drivers for the success of modern machine learning. Self-driving cars, object detection algorithms and predictive models all use data to make informed decisions; without it, they are nothing.