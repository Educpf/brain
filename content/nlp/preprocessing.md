
# Introduction

In Natural Language Processing computers need to understand text, but they only work with numbers. 
This transformation requires some decisions that may not be obvious, so we will dive into them.


# What is a word?

It might seem obvious at first, but is **run** the same as **running**? **New York** as **New** _ **York**? **CAPS** as **caps**?

All this decisions need to be considered, we define how the model sees the text and understands it.

# Splitting text into sentences and tokens

After defining what counts as a word it's necessary to define how to split some text in those units. Both separating **sentences** and **word** within them. A naive approach such as splitting on spaces and punctionation would break easily (Dr. Edu | New York? ). Using **Regular Expression** can help us defining more intersting patterns.

# Vocabulary size problem

Reducing the vocabulary is often done to:

1. Reduce number of parameters a model needs to consider
2. Better generalization: variants overlap so there is more example of each token
3. No unkown words since they are converted into simpler forms

Keep in mind that all this transformations actually make the text lose some of its meaning. It's necessary to consider the use case to decide which operations to apply.

Let's see how this can be achived:

## Normalization - Morphologically

### Lemmatization

A slower and complex but accurate approach that reduces each token to their dictionary base form "lemma". ("am", "is", "are" -> "be")

### Stemming
A more crude but faster approach that simply chops off word endings based on some rules. Might convert to very different words to the same **stemma**, making mistakes.

## Compositionally

#### Byte-Pair Encoding (BPE)
Start with characters as base vocab and apply merges of most frequent adjacent token pairs

#### WordPiece (used in BERT)
• Similar to BPE, but prioritizes merging of pairs where individual parts are less frequent
• score = pair_freq/(first_element_freq × second_element_freq)

#### Unigram language model
Starts with a large vocabulary (characters, frequent sequences, space separated words) and reduces it
iteratively (remove tokens with lower probability) until the desired number of tokens is left

#### SentencePiece
Treats the input as a raw input stream, to address languages without space splitting between words
• Often used in combination with unigram language mode


## The balance

Every rule written will have two failure modes:

- False positives (too broad): individual meaning of specific words is lost
- False negatives (too narrow): your regex misses "The" with a capital T


