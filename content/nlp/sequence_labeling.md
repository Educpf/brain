
# What is it?

The process of assigning a fixed number of tags to each element of a sequence

# Applications

- **Part-of-speech**: assign a morphosyntactic category (verb, name, ...) to each token
- **Named Entity Recognition**: find and classify spans of specific target interest ( name, places, dates...)
- **Chunking**: groups words together into structured grammatical phrases
- **Semantic Role Labeling (SRL): identify the roles words play in a sentence ( predicate, agent, patient...)
- **Code Switching**: find segments of different languages in a multilingual text

# Parts of Speech (POS)

The category of a word tells us about likely neighboring and syntatic structure, which is a key aspect of parsing. Can help us predict or understand bethe text.
- Closed Classes (prepositions, determiners, pronouns ) vs Open Classes (nouns, verbs, adjectives )
- Ambiguous words can have more than one possible POS ( book, that, liga(pt) ) 
- Only 15% is ambiguous but consist on 60% of word tokens in running text (depending on the genre)

## Baseline Algorithm

The goal is to solve those ambiguities, sometimes is easy because the possible tags are not equally likely! A very simple algorithm is to simple choose the **most frequent tag** in the training corpus. This achieves already an accuracy of **90%**, which compared to the human and STOTA **97%**, is already very good.

## Evaluation

Simply done via **accuracy** since each word matters uniformly.

# Named Entity Recofnition (NER)

The goal is not to tag every single token, but to identify specific targets, which can be referred as entities. (Person, location, date, time, prices ... )

***image***


**Usefullness**: this is important because many of our daily tasks focus on specific entities so having information relative to those is important:

- Entity monitoring/sentiment analysis: what is the review about? What are people saying about this entity?
- Search algorithms: which documents mention the target entity? 
- Machine translation: Ensure correct translations
- Entity relations and linking

As before, this also suffers from **ambiguity**:

- **Segmentation**: where are the entity boundaries?
- **Type**: different entities with diff types can have the same name


## Algorithm

NER can actually be treated as word-by-word sequence labeling task such as POS. That works very well for the segmentation problem. There are two main implementations of tag types:

- **BIO or IOB**: Beginning | Inside | Outside
- **BILOU or BIOES**: Beginning | Inside | Last/End | Outside | Unit/Single

## Evaluation

Needs to be done using **Precision**, **Recall** and **F1-Score** because of the very big amount of Outside words.
Evaluation is done at entity level, because at word level getting only part of an entity span would count both as false positive and false negative, which would be too severe.


# Markov Chains and Hidden Markov Models (HWW)

To solve this systems we can use **Markov Chains**, explained in [[markov_chains]], which simplifies with the assumption that the next step depends only on the current step.
In this case, because we want to predict the **tags** but only see the **words** we are actually dealing with a **Hidden Markov Model**, explained in more detail in [[markov_chains#Hidden Markov Chains]]

## Viterbi Algorithm


This is a [[../algorithms/dynamic_programming]] algorithm used to find the combination of states with the highest possible probability in a smarter and more efficient way than simple brute force, but still with $O(N^2T)$

The idea, as for all dynamic algorithms is to make the computations work in a way that reuse previous calculated values. 

For this scenario in particular, the core idea is the following:

The algorithm processes the observations from left to right. At each time step t ( observation t ), it computes, for every possible new state, the best probability of reaching that state. This is done by considering all possible t-1 state, choosing the predecessor that yields the best prob. At each step we have stored what is the hightest prob for each state and what was the predecessor. This uses a matrix $[N(states), T(observations)]$.

## Extensions

The use of **trigrams** can also improve the results since we are now looking more into the "past", which requires in the case of Viterbi looking back into 2 columns, which increases the complexity to $O(N^3T)$.

- Reduce complexity by not keeping all the possible states for each step but just a subset of them considering the score. 
- Trigram sparsity ( never occurring ) can be tolerated if interpolated all possible n-grams $λ1​P(st​∣st−1​,st−2​)+λ2​P(st​∣st−1​)+λ3​P(st​)$, considering that $λ1​+λ2​+λ3​=1$

# Maximum Entropy Markov Models ( MEMM )

Normal Markov Chain has a problem when dealing with words that never appeared in training since it relies directly on the probability of the tag considering the word ( emission probability ). MEMM solves this by using [[logistic_regression]] and feeding it with the features of the words and not the words directly (suffix, Capitalization, prefixes etc... ), plus the previous tag.

To turn this logistic regression into a **sequence model** we can use a **greedy** approach and choose the best one from left to right, or use the **viterbi decoding**

## Label Bias

Because the model analyses each step at a time, the output probabilities sum to 1. This makes it so states with very few outgoing paths options will have artificially high probabilities ( close to 1), even if they fit poorly when looking at the future context. The problem is not having the whole context ( since it can be given as features ), is not having the context of what the future decisions(tags) are, and doing a global comparison.

A way to improve this is to do multiple passes, from left-to-right and right-to-left. This can be considered using **greedy** approach where we simply select the tag with highest score from both passes. Or the **viterbi** where we look into the sequences and choose the one with highest score.


# Conditional Random Fields (CRF)

Solves the previous presented problem, assigning a single probability to an entire tag sequence, skipping local decisions. It uses still **viterbi** algorithm to avoid actually computing all those individually.
