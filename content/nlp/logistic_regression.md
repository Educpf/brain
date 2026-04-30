

# Logistic Regression

**Discriminative** classifier, a **baseline supervised** machine learning algorithm for classification.

It has a very close relationship with **Neural Networks**, which can be considered as a series of logistic regression classifiers stacked on each other

## Steps

#### 1. Turn text into numbers

Represent document as a list of numbers, such as:
- x1 = how many positive words appear
- x2 = count of personal pronouns 

#### 2. Combine into single score

Each features gets a **weight**, indicating how much the feature matters. Multiply each feature value by the weight and sum everything together, plus a **bias** ( baseline offset ):

$$
    z = w_1 \cdot x_1 + w_2 \cdot x_2 + ... + b
$$

#### 3. Convert score to probability ( Sigmoid )

The output value has no pre-defined limits: $z \in ]-\infty , +\infty[$
To solve this, make the values comparable, and get a nice percentage, we use the **sigmoid function**:


$$
    \sigma (z) = 1 / (1 + e^{-z})
$$

> This can be thought as a S-shaped squishing function. 
> Very large z -> close to 1
> Very negative z -> close to 0
> Exactly 0 -> 0.5

#### Scaling

Might be a good idea to rescale the feature values so that they are in comparable ranges. This will:

- Improve **convergence**, specially when using gradient-based
- Better comparison of feature importance
- More effective distances
- Better **generalization**

For that, there are 2 main approaches:

$$
    Z-score = \frac{x - \mu}{\sigma}
$$
> $\mu \rightarrow$ mean 
> $\sigma \rightarrow$ standard deviation

$$
    Normalization = \frac{x - min(x)}{max(x) - min(x)} = [0, 1]
$$

## How to learn the weights?

This is where training plays a role. The weights initially are zero or random. The model will naturally make bad predictions.
We need to measure how bad, and fix accordingly:

#### The loss function ( Cross-Entropy )

Measures how bad the prediction is, being the goal to minimize it:

$$
    Consider: p = \sigma(w \cdot x + b) \\
    P(y | x) = p^y(1-p)^{1-y}
$$

$$
    Cross\_entropy\_loss = -logP(y | x) = ]0, +\infty[
$$

#### Gradient Descent

The gradient measures the slope of the loss, in which direction the slope increases. THe model then moves in the opposite direction.
The size of the step is the **learning rate ($\eta$)**. 
- Too large: overshoot and bounce around
- Too small: take forever to get there

The process is repeated until no more improvements are made.

$$
    feature\rightarrow \frac{dL}{dw} = (p - y)x
    \\.\\
    w := w - \eta \frac{dL}{dw} 
$$
$$
    bias \rightarrow \frac{dL}{db} = (p - y)
    \\.\\
    b := b - \eta \frac{dL}{dw}
$$


## Training

- **Batch**: compute gradient over the entire dataset
- **mini-batch**: rtain on a group of m examples, more efficient + parallel execution

# Overfitting gand Regulatization

Sometimes the model learns things that can be true but are simply coincidences, present in the training dataset, meaning it won't generalize well. This is, again, **overfitting**

**Regularization** is the process of adding penalty to the loss of having **large** weights, forcing the model to not rely too heavily on a single feature.

- **L2(ridge)**: penalizes *square* of weights, keeping all small
- **L1(Lasso)**: penalizes *absolute* weights, tending to **zero out** many weights, giving a simpler model

$$
    loss \rightarrow data\ loss + \lambda \cdot penalty
$$

$$
    L2 \rightarrow penalty = \sum_j w_j^2
$$

$$
    L1 \rightarrow penalty = \sum_j |w_j|
$$

# More than 2 classes

For binary we use **sigmoid** as seen previously. But what if there are 6 categories for example?
**Softmax** is the generalization. Given the score of each class, it converts them into probabilities that add up to 1. We then choose the one with highest prob.

## Multinomial LR

Similar as having k differnet LR models, but the output of each one is:

$$
    P(y | x) = p = \frac{e^{z_k}}{\sum_{j=1}^K e^{z_j}}
$$

$$
    loss \rightarrow -log(p_c) (c\ is \ correct\ class)
$$