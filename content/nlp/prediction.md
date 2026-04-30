
# Introduction


**How to predict language?**

Imagine the case of an auto-complete, it needs to in some way predict which word comes considering the previous words. 
One approach is counting. For **want chinese** see how many times **chinese** appeared after **want** and how many times **want** appeared. P(chinese|want) = P(want chinese) / P(want).

Problem: Language is creative and infinity. New combination would just break the system.

# N-grams

Realistically the whole phrase has impact on which word appears next, but instead of conditioning on the entire preceding history usually systems only look at the last N-1 words. 

Instead of asking P(blue|The water of the river is so beautifully ), just ask P(blue|beautifully), which is much more present in the data. Trading accuracy with feasibility.

This is called **Markov assumption**, recent past is good enough to approximate the future.

## MLE: Maximum Likelihood Estimation

For **estimate probabilities** simply **count** and **divide**:

For a bigram:
**P(w~n~ | w~n-1~) = count(w~n-1~, w~n~) / count(w~n-1~)**
In general for n-grams:
**P(w~n~ | w~n-N:n-1~) = count(w~n-N:n-1~, w~n~) / count(w~n-N:n-1~)**

The times the combination appears divided by how often the prior appeared, giving the most likely probability given that observed data ( the name comes from there )

To get the probability of a **sentence**, multiply sequentially the probability of each n-gram:

<\s> i want chinese food </\s> =
= 𝑃(i|<\\s>) 𝑃(want|i) 𝑃(chinese|want) 𝑃(food|chinese) 𝑃(</\\s>|food) =
= .25 × .33 × .0065 × .52 × .68 = .000189618


### Log space trick

Probabilities are small and multiplying them together gives even smaller numbers, which for computers is a problem. One can add in log space, which is equivalent to multiplying in linear space:

**p1 x p2 x p3 = exp(log(p1) + log(p2) + log(p3))**

# Evaluating Language Models

There are two main types of evaluations:

**Intrinsic evaluation**
- Direct evaluation on the same type of data
- No external tasks
- Measure likelihood probability of phrases
**Extrinsic evaluation**
- Embedded in complex systems ( translation, classification, speech recognition etc...)
- Check if adding new models improves or not
- Expensive, time consuming

### Perplexity 

Inverse probability of the test set, normalized by the number of words. 
**Intuition:** It measures how surprised a model is on seeing a specific sentence, meaning higher perplexity = lower probability.

$$
PP(W) = P(w_1, w_2.., w_n)^{-\frac{1}{N}} = \\ \sqrt[N]{\frac{1}{P(w_1, w_2.., w_n)}}
$$


> **Math making sense**
> Normalize by length: To ensure metric can be used to compare sentence with different lengths
> Invert Probability: Easier to interpret bigger numbers 
> N-th root: instead of average for phrase, get avg for word ( since computed probability was a multiplication of prob of each word). Can be interpreted as number of choices per word.

# Sparsity and Unknown Words

N-grams work well for word prediction if the training and test corpus are similar, but the system should handle exceptions:

**Out of vocabulary(OOV) words**

- **open vocabulary**: model unknown words in **test** set to **<\UNK>** (more realistic)
- **closed voc**: define a list of all interest words and convert to **<\UNK>** any unknown word of the **training** set ( more restrictive )


**Zero probability n-grams**

If some specific combination of words (of size n) never appeared in the training set then there are 2 problems:

- Whole phrase gets probability zero, which is wrong
- Perplexity become impossible to compute

## Laplace smoothing | Add-one smoothing

**Idea**: assume all combination were seen at least once but adding one to **all** counts. The denominator grows by $V$ to keep probs summing to 1.


$$
    P(W_n | W_{n-1}) = \frac{C(W_{n-1} W_n)}{C(W_n)}\\
    P_{laplace}(W_n | W_{n-1}) = \frac{C(W_{n-1}W_n) + 1}{\sum_{w}(C(W_{n-1}W)+1)} = \frac{C(W_{n-1}W_n) + 1}{C(W_{n-1})+V}
$$


> **Intuition**
> Can be interpreted as stealing a bit of probability mass from more common events and redistribute them to unseen ones
> The sum of $V$ in denominator comes from the fact that the previous "word" count has to take into consideration all words ( even the ones that did not exist )

This smoothing is too **blunt** but its still used in NLP when the number of zeros is not too large

### Add-k smoothing

Move a little bit less of the mass, $K\in]0, 1[$
$$
    P_{add-k}(W_n | W_{n-1}) = \frac{C(W_{n-1}W_n) + k}{C(W_{n-1})+kV}
$$
### Back and interpolation

Blend smaller n-grams with trained weights.


## Web-scale  N-grams

- Efficient data structures
- Only store n-grams above a certain threshold
- Use entropy to prune less important n-grams

### Stupid backoff

- Give up the idea of making the language model a true probability distribution
- No smoothing, if some n-gram has 0 count, use a lower order n-gram, weighted by a fixed weight


# Problems with N-Grams


- No relationship between words. Synonyms are treated as totally different words
- Markov models have **limited context** ( n-1 previous words )
- Storage cost: Need to store all counts for all n-grams $O(exp(n))$

In sum: the models are **sparse**












