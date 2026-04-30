
# Text classification


The simple idea of having a trained model able to sort text into categories.

Email -> Spam or Not?
Review -> Positive or Negative?


There are 2 main ways of building such a classifier:

**Hand coded Rules**

Specific, hand crafted by specialists, rules that identify a category. Maintaining is hard because rules have may have to be changed constantly.

Ex:
Spam detection: **black-list** of addresses and specific keywords
Sentiment analysis: ratio of **word polarities**

**Machine learning**

Show the model thousands of labeled examples, and it figure outs the pattern on its own. Much more scalable

# How to represent text?

ML algorithms cannot read text directly, they need numbers, a set of features, so various different ways of representing text are used, each one with its tradeoffs.

### Bag of Words

Just count how many times each word appeared in the document, ignoring their order entirely. Grammar and context are lost, but works surprisingly well.

*"I love this movie, I really love it"*
**{I: 2, love: 2, this: 1, movie: 1, really: 1, it: 1}**

### Binary NB

Instead of counting how many times a word appears ( frequency ), simply note whether it appears or not. (Often better for sentiment analysis)

### N-grams

Instead of single words, n-grams as seem before, which can understand more context. **Problem**: Creates a huge sparse feature set...

## Tricks to increase meaning

## Negation Handling

Trick to flip meaning of words when negated ( ex. "not like" should flip meaning of "like" ). **tag** words after `"not/n't/never..."` with `_NOT` prefix, creating essentially a new feature.

# Naive-Bayes

## The theorem

Without going too much in depth, this theorem defines a mathematical formula used to calculate the probability of an event based on prior knowledge of conditions related to that event. Calculated as:


$$
    P(A|B) = \frac{P(B|A)\times P(A)}{P(B)}
$$

It helps understanding scenarios like. Disease $A$ is very rare, being $P(A)$ very low. But the test is very accurate $P(B|A)$ is high. $P(A|B)$ might still be low because the disease is super rare and there are false positives.

## The naive part

Using this algorihtm works well in practice but in reality a bid assumption is made, which denotes it **naive**. It treats every word as indepedent of every other word, but in reality as we know, that is not the case.


$$
    c = argmax_{c\in C} P(c|d) = \\
    .\\
    = ...\frac{P(d|c)P(c)}{P(d)} = \\.\\
    = argmax P(d|c)P(c)
$$

> Last works because we want the max and the denominator is the same for all scenarios

And the **conditional independence part that makes this naive but doable**
$$
    P(d|c)  = P(f1, f2, f3 ...fn | c) = \\.\\
    = P(f1 | c) \times P(f2|c) ... \times P(fn|c)
$$


$$
    P(w|c) = \frac{count(w, c)}{\sum _{w \in V} count(w, c)}
$$

#### Remember

To avoid very small numbers, use **log space** $log(P(c)) \times log(P(d|c))$

To avoid errors when handling data that didn't appear in training use smoothing (laplace: $\frac{count(w, c)+1}{\sum count(w, c) + V}$)




## Sentiment lexicons

Pre-build lists of words tagged as positive or negative. Useful when data is small.

# How to evaluate performance?

| Metric | Intuition | Formula | Explanation |
| ------ |  --| ------- |  ----------- |
|  Accuracy       | How many rights the model had? |    $right/total$ | Breaks down with imbalanced data. Scenario where 99% of emails are spam and model simply says all are|
| Precision | How many positives were true? | $TP / (TP + FP)$ | Analyse how many false alarms |
| Recall | How many positives did I catch? | $TP/(TP+FN)$ | Avoid missing things |
| F1 Score | Combine P and R | $\frac{2.P.R}{P+R}$ | One balanced number of the two measures |


# Multi-class 

When there are more than 2 classes:

#### **Multi-label**

Each item can be assigned more than one label. Run one classifier independently for each class.

#### **Multinominal**
Classes are mutually exclusive, text can only have 1 label. 
- One model can output the probabilities for all the classes and pick the best one (Multinominal Naive-Bayes, M... Logistic Regression).
- Train individually classifiers per class, run all of them, and choose the one with highest score


## Evaluation
In this cases, metrics need to be aggregated:

**Macro-averaging**: compute per-class scores, then average equally across classes (treats all classes equally regardless of size)
**Micro-averaging**: pool all the predictions together ( dominated by frequent classes )



# How to train? Overfitting...

The problem of **overfitting** is having a model too specific for the training and not being able to generalize well to unseen one, for that, all the tests have to be done with new data. 
- Training set: model learn from it
- Development set: tune settings and compare approaches
- Test set: only used once, at the end, for final honest validation

When data is scarse, **k-fold cross-validation** is smart:
- Divide data into k chunks
- Train on k-1 
- Test on the other
- Rotate k times
- Average results

> There are some variants of cross-validation, being this one the most popular



# Generative vs Discriminative Classifiers

|     | Generative | Discriminative |
| --- | ---------- | -------------- |
| Question asked | "How would class C generate this text?" | "What features distinguish the classes?" | 
| Computes | P(document \| class) × P(class) | P(class \| document) directly | 
| Examples | Naïve Bayes | Logistic Regression, SVM, Neural Networks | 

**Generatice**: build mental picture of each class
**Discriminative**: learns boundary between classes, and decides when looking at new input