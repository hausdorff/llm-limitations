# Attention is not suitable as the basis for AGI

Modern LLMs have access to rich "knowledge" about the world via embeddings learned during training.
Unfortunately, their inference routines render them mechanically incapable of using this knowledge effectively.

This article attempts to characterize these 

<!-- Like many people in CS, I am earnestly hoping for AGI.
In the pantheon of views I am opposite the x-risk and existential threat people.
I eagerly welcome robust investment in the sector and encourage people working on it every chance I get.

And, like many people I was hugely optimistic about recent advances in various AI models, especially LLMs, and particularly the transformer.
I enthusiastically joined the optimism of the many famous [theoreticians][sparks], the Turing Award laureates who made this all possible, and an entire peanut gallery of onlookers and amateurs.

So, it is with great regret that I am forced to conclude that the transformer architecture is completely unfit as the basis for AGI.

This article contains all the reasons why. -->

## How transformers work

Transformers can be used to predict _tokens_.
Tokens are sometimes, but not always, words.
Simplifying greatly, prediction essentially works as follows:

1. We have a "prompt" of $t$ existing tokens.
1. We convert these $t$ tokens into $t$ vectors of $d$ dimensions.
   We do this with an "embedding" which we _learned_ during training.
   An "embedding" is basically a function that takes a token and some position information, and turns it into a $d$-dimensional vector.
1. We take these $t$ vectors, and we compute a _weighted average_ of all of them.
   This produces a single $d$-dimensional vector.
   This weighted average is called "attention" but [really][smoothing], it's an elementary "first year of grad stats" type of Kernel smoothing called _Nadaraya-Watson smoothing_.
1. This single $d$-dimensional vector is fed through a feed-forward neural network (with some fancy tricks) to produce a probability distribution over all token types.
1. Finally, we "predict" a new token by sampling from this distribution.
   If we always choose the #1 most likely token type, we say the _temperature_ is 0.

## Problems

The critical limitations of modern LLMs arise from step (3).

Modern LLMs have access to rich "knowledge" about the world via embeddings learned during training.
The core challenge is that their inference routines render them mechanically incapable of using this knowledge effectively.




LLMs have access to rich "knowledge" of the world, but the inference routines are mechanically incapable of using it effectively.

This "knowledge" comes primarily from the positional embeddings of the $t$ tokens of the prompt.
All the shortcomings of the 

Whatever the model "knows" comes from the positional embeddings of the previous $t$ tokens.
The tokens it predicts are a function of these embeddings—a complicated function, but a function nonetheless.

Broadly 

To build intuition for why, let us consider a simple scenario.



Fundamentally step 3 involves taking a user-provided prompt of $t$ tokens, computing a fancy average of the embeddings of those tokens, and using that to compute a distribution from which to predict the next token.

An LLM like ChatGPT does not control the prompt provided by the user.

It will recieve $t$ tokens, 

* LLMs don't control the prompt, which leads to several fundamental limitations:
* LLMs do "know" facts via the learned embeddings, but, mechanically, inference cannot reliably retrieve it.
* 
* LLMs CANNOT retrieve an answer unless it appears "close" to at least 1 embedding in the previous n tokens.
* LLMs CANNOT prevent being "tricked" into giving the wrong answer, because a single token will move the average away from the correct solution.
* There is NO EMBEDDING can be constructed which mitigates these issues.
* Scaling up CANNOT solve this problem.

Things that we now know are not true:

* "Predicting the next token perfectly implies perfect knowledge of the world." Maybe, but 

## Problem 1: Transformers are pathologically susceptible to 

There are very few things everyone agrees an AGI should be able to do.
But one of those things is "produce correct answers to trivial questions like 2+2."




