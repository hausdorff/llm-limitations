# Limitations of transformers

> To put our results into perspective let us recall the situation in the early 1960s: Many people were impressed by the fact that initially unstructured networks composed of very simple devices could be made to perform many interesting tasks—by processes that could be seen as remarkably like some forms of learning.
> 
> **A different fact seemed to have impressed only a few people: While those networks did well on certain tasks and failed on certain other tasks, there was no theory to explain what made the difference—particularly when they seemed to work well on small ("toy") problems but broke down with larger problems of the same kind.**
>
> — "What Perceptrons Can't Do", Epilogue of _Perceptrons_, 1988, Marvin Minsky and Seymore Papert

Are transformers, [as some believe][dario], a new kind of general reasoning and prediction engine?

It is hard to answer this question without a clear picture of what transformers can do.
This document is a living collection of things transformers have a hard time doing, or can't do, under which constraints, and why.

## Encoder-decoder transformers with arbitrarily-large numbers, infinite external memory, and decoders that can run for an arbitrary number of steps...

### ...Are Turing Complete

* [On the turing completeness of modern neural network architecture][perez]
* [On the computational power of transformers and its implications in sequence modeling][bhattamishra]

## Single-pass transformers with arbitrarily-large numbers but NO external memory, and self-attention masked so that each position attends to only previous positions...

### ...[with hard attention] cannot match regexes with Kleene stars (_e.g._, $(aa)*$) on arbitrary-sized input

[Theory] [Theoretical Limitations of Self-Attention in Neural Sequence Models][hahn]

[Practice] [On the Ability and Limitations of Transformers to Recognize Formal Languages][bhattamishra2]

* In general, this class of model is **strictly less powerful** than both **finite state machines** and **regular expressions that use the Kleene star.**
* No trained model in this class can match the regex $(aa)*$ on arbitrarily long input.
* To match this regex for longer maximum input length $n$, you must increase either the number of attention heads, or the number of layers in the model.
* Said differently: for any choice of $n$, we can construct a model in this class to match this regex for input length < $n$.
  But, _all_ models will have some maximum input length $n$ they can match.

### ...[with soft attention] can be _constructed_ to match regexes with Kleene stars (_e.g._, $(aa)*$) on arbitrary-sized input

[Overcoming a Theoretical Limitation of Self-Attention][chiang]

* A response to [Hahn][hahn].
* Contains an explicit construction of a transformer with the above constraints, that matches $(aa)*$ (among other regexes) perfectly.
* Uses layer normalization to get around a core theoretic objection of [Hahn][hahn]. Hahn argues that in problems where any bit in an $n$-bit vector can change the outcome, as $n$ gets large, the confidence of the prediction must be very low, with acceptances being slightly over 0.5, and rejects being slightly below. [Chiang's argument][chiang] is that layer normalization brings this arbitrarily close to 0.

### ...[with soft attention] seem to not be able to _learn_ to match regexes with Kleene stars (_e.g._, $(aa)*$) on arbitrary-sized input

[On the Ability and Limitations of Transformers to Recognize Formal Languages][bhattamishra2]

[Overcoming a Theoretical Limitation of Self-Attention][chiang]

[Neural Networks and the Chomsky Hierarchy][deletang]





## Single-pass transformers with bounded-size numbers and NO external memory...

### ...Cannot learn to generalize regexes with Kleene stars, _e.g._,  $(aa)*$ on arbitrary-sized input

> However, doing the same task via self-attention mechanism without using any internal memory could be highly non-trivial and potentially impossible. Languages such as (aa)∗ and Parity are among the simplest non-star-free languages, and hence limitations in recognizing such languages carry over to a larger class of languages. The results above may suggest that the star-free languages are precisely the regular languages recognizable by Transformers. As we will see in the next section, this is not so.

Positional encoding is required for parity because the language is unary.

LSTMs can solve this problem trivially.

* [On the Ability and Limitations of Transformers to Recognize Formal Languages][bhattamishra2] (does _not_ generalize non-star-free languages )

### ...Cannot balance parentheses

Dyck-2

* [Theoretical Limitations of Self-Attention in Neural Sequence Models][hahn] (theoretical)


### Are universal approximators of sequence-to-sequence functions given arbitrary precision

* [Are Transformers Universal Approximators of Sequence-to-Sequence Functions?][yun]

## Questions/conclusions

* We probably are not learning what we think we are, if it takes so much work to do things like parity.
* What do these limitations have to say about how complicated language is?


[perez]: https://arxiv.org/pdf/1901.03429.pdf
[bhattamishra]: https://arxiv.org/pdf/2006.09286.pdf
[bhattamishra2]: https://arxiv.org/pdf/2009.11264.pdf
[yun]: https://arxiv.org/pdf/1912.10077.pdf
[hahn]: https://arxiv.org/pdf/1906.06755.pdf
[dario]: https://youtu.be/gAaCqj6j5sQ?t=592
[chiang]: https://aclanthology.org/2022.acl-long.527.pdf
[deletang]: https://openreview.net/pdf?id=WbxHAzkeQcn
