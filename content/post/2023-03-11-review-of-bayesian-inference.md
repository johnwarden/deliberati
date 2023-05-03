---
layout: single
title:  "A Brief Review of Bayesian Inference"
date:   2021-10-26 00:00:00 +0200
toc: true
toc_sticky: true
sidebar:
  - nav: "distributed-bayesian-reasoning-related"
    title: "Related Articles"
header:
    teaser: /assets/images/distributed-bayesian-reasoning/bayesian-juror-posterior.svg

---

> “When you have eliminated the impossible, all that remains, no matter how improbable, must be the truth.”
> 
> -- Sherlock Holmes (Arthur Conan Doyle)

- Reallocating probability mass
- Joint probability distributions
- Not about Bayes rule

For a long time Bayesian inference was something I understood without really understanding it. I only really *got it* it after reading Chapter 2 of John K. Kruschke's textbook [*Doing Bayesian Data Analysis*](https://nyu-cdsc.github.io/learningr/assets/kruschke_bayesian_in_R.pdf), where he describes Bayesian Inference as [*Reallocation of Credibility Across Possibilities*](https://link.springer.com/article/10.3758/s13423-017-1272-1).

I now understand Bayesian Inference to be essentially Sherlock Holmes' pithy statement about eliminating the impossible quoted above, taken to its mathematical conclusion. This article is my own attempt to elucidate this idea, targeted for readers of my article on [Distributed Bayesian Reasoning](/distributed-bayesian-reasoning-introduction).

## Prior Beliefs

A Bayesian reasoner starts out with **prior** beliefs, which are a set of mutually exclusive and exhaustive possibilities for the situation at hand.

For example, suppose that Sherlock Holmes is investigating the Case of the Disappearing Duchess, and believes that there is a 50% chance that the Duke has kidnapped the Duchess and is holding her alive in captivity, a 25% that chance that the Duke has murdered her, and a 25% chance that she has been murdered by the Count. The total of the probabilities is 100%.


| Prior Beliefs
| Culprit   | Status    | Probability 
| --------- | --------- | -------------
| Duke      | Alive     | 50%          
| Duke      | Dead      | 25%         
| Count     | Dead      | 25%         
| ----------| --------- | ----
|           | TOTAL     | 100%

## Revising Beliefs

Bayesian reasoners then revise their beliefs when they acquire evidence according to very simple rules:

- reject any possibilities that are incompatible with the evidence
- reallocate probability to the remaining possibilities so that they sum to 100%

For example, if the Duchess's body is found buried under the Atrium, Holmes must eliminate the possibility that she is being held captive and alive by the Duke. He must then reallocate probability among the remaining possibilities.

So he concludes that she was either murdered by the Duke or murdered by the Count. These new beliefs are called his **posterior** beliefs.

| Posterior Beliefs
| Culprit   | Status    | Probability 
| --------- | --------- | -------------
| Duke      | Alive     | **0% (eliminated)**         
| Duke      | Dead      | 50%         
| Count     | Dead      | 50%         
| ----------| --------- | -------------
|           | TOTAL     | 100%

## Reallocation of Probability Mass

Another way of looking at this is that the 50% "probability mass" previously allocated to the first possibility is reallocated to the remaining possibilities. You can visualize this if we plot the prior and posterior probabilities as bar charts. The total volume of the bars in each of the two charts below is 100%. The 50% probability mass assigned to the first possibility in the prior is reallocated to the remaining two possibilities in the posterior.

<!--
     Prior Probabilities                        Posterior Probabilities (After body found)                
     -------------------------------            -------------------------------    
       50%                                                   50%        50%       
        #                                                     #          #         
        #         25%        25%                              #          #         
        #          #          #                               #          #          
        #          #          #                    0%         #          #          
     -------------------------------            -------------------------------    
      Alive/    Dead/      Dead                  Alive       Dead/     Dead/     
      Duke      Duke       Count                 Duke        Duke      Count  
-->



<!-- This image is generated using R. Source: relevance-delta-chart.R -->
<img src="/assets/images/distributed-bayesian-reasoning/reallocation-of-probabilities.svg"
     alt="Reallocation of Probabilities Example"
     style="display: block; margin-left: auto; margin-right: auto; max-height: 800px" />

<div style="font-size: smaller; padding-left: 20px; margin-bottom: 20px;">Illustration of the concept of "Reallocation" or Probability Mass. After the "Alive+Duke" scenario is eliminated, probability mass is reallocated to the remaining 2 scenarios.</div>


Reallocation of probability mass must be **proportional**. In the example above, the prior probabilities of last two possibilities are equal, so the posteriors must also be equal. On the other hand, if one possibility were twice as likely as the next in the priors, it would remain twice as likely in the posteriors. We will discuss the exact math for calculating posterior probabilities below.


## Sequential Updating

Suppose Holmes subsequently finds evidence that exonerates the Count. We now simply repeat the process of Bayesian inference: the posterior from the last piece of evidence become the prior for the next, and probability mass is again reallocated. This time, since only one possibility remains, all probability mass is reallocated to this possibility.

<!--
    
    
    Prior Beliefs                              Posterior Beliefs (Count Exonerated)                 
    -------------------------------            -------------------------------    
                                                            100%
                                                             #
                                                             #
                                                             #
                 50%        50%                              #               
                  #          #                               #                 
                  #          #                               #                 
                  #          #                               #                  
       0%         #          #                    0%         #                  
    -------------------------------            -------------------------------    
      Alive/    Dead/      Dead                  Alive       Dead/     Dead/     
      Duke      Duke       Count                 Duke        Duke      Count  
    
    Reallocation or Probability Mass after "Murdered by Count" is eliminated
-->

<img src="/assets/images/distributed-bayesian-reasoning/reallocation-of-probabilities-2.svg"
     alt="Reallocation of Probabilities Example"
     style="display: block; margin-left: auto; margin-right: auto; max-height: 800px" />

<div style="font-size: smaller; padding-left: 20px; margin-bottom: 20px;">Illustration of sequential updating. The posterior after the first piece of evidence becomes the prior for the next piece of evidence. After the "Dead+Count" scenario is eliminated, probability mass is reallocated to the remaining possibility.</div>

## Constantly Decreasing Entropy

So what happens now that we've reached a point where there are no more possibilities to eliminate? At this point, no more inferences can be made. There is nothing more to learn -- at least with respect to the Case of the Disappearing Duchess.

It might seem counter-intuitive that every time a Bayesian reasoner learns something, they eliminate rows from the probability distribution, in a sense **decreasing** the amount of information. Isn't Bayesian inference about increasing information, not decreasing it?

But in fact, from an information-theoretic perspective, eliminating possibilities is exactly what information is. It is the reduction of uncertainty. Every time a possibility is eliminated from the probability distribution, its entropy decreases, until only one possibility remains and there is no uncertainty, at which point entropy is zero.

Which brings us back to Sherlock Holmes: *When you have eliminated the impossible, all that remains, no matter how improbable, must be the truth.* Bayesian inference can be thought of as the process of eliminating the impossible, and then updating the probability of "all that remains" to be 1.

> When you have eliminated the impossible, the probability of all that remains, no matter how improbable, must be scaled to sum to 1.
> 
> -- Sherlock Thomas Bayes Holmes (Jonathan Warden)


## Updating Beliefs based on Evidence

What makes Bayesian inference so powerful is that learning one thing can change the probability of another thing, sometimes in non-intuitive ways.

For example, learning that the Duchess is dead **decreased** the probability that the Duke did it (from 75% to 50%), and **increased** the probability that the Count did it (from 25% to 50%), which you should be able to easily verify. This is illustrated in the four charts below

<img src="/assets/images/distributed-bayesian-reasoning/reallocation-of-probabilities-3.svg"
     alt="Reallocation of Probabilities Example"
     style="display: block; margin-left: auto; margin-right: auto; max-height: 800px" />

<div style="font-size: smaller; padding-left: 20px; margin-bottom: 20px;">Illustration of how Holmes belief in the probability of guilt of the two suspects changes after learning that the Duchess is dead. Probability mass is reallocated proportionally to the remaining two possibilities, as illustrated in the top two charts. But although this results in an increase in the total probability that the Count did it, it results in an decrease in the total probability that the Duke did it, as illustrated in the bottom two charts.</div>



## Beliefs as Joint Probability Distributions 

Evidence about one proposition can only change beliefs about another proposition because the prior beliefs are beliefs in **combinations** of propositions, not individual propositions. 

Holmes' prior beliefs are not simply *there is a 75% chance that the Duke did it* or *there is a 50% chance that the Duchess is dead*. If his beliefs were so simple, learning that the Duchess was murdered would not tell Holmes anything about whether it was the Duke or the Count that did it.

Rather his beliefs are a joint probability distribution in which there is positive correlation between the Duchess being dead and Count having done it. It is these correlations that encode the knowledge that enables Homes to make inferences about the culprit upon learning of the Duchess's death.

I think that understanding prior beliefs as a belief in a **join probability distribution** is key to understanding Bayesian Inference.

## Math for Reallocating Probability

Here again are Homes's prior beliefs before the Countess was found buried under the Atrium. 

| Prior Beliefs
| Culprit   | Status    | Probability 
| --------- | --------- | -------------
| Duke      | Alive     | 50%          
| Duke      | Dead      | 25%         
| Count     | Dead      | 25%         
| ----------| --------- | ----
|           | TOTAL     | 100%

After eliminating the first possibility, the probability of the second two possibilities must be scaled so that they sum to 100%.

To scale any set of numbers so that they sum to 100%, just divide each by the total. The total probability of the last two possibilities is 25% + 25% = 50%. So to make these two possibilities sum to 100%, we divided each by 50%. Since 25%/50% = 50%, the posterior probability of each remaining possibility is 50%.


Generalizing, **the total prior probability of the possibilities compatible with the evidence is just the total prior probability of the evidence**. So in this case, the evidence is that the countess is dead, and the total prior probability that the countess is dead is 25%+25% = 50%.

Using $$P$$ to indicate the prior probability, we can write this mathematically as:

$$
\begin{aligned}
    P(Dead) &= P(Duke, Dead) + P(Count, Dead) \newline
            &= 25\% + 25\% = 50\%
\end{aligned}
$$

We use $$P'$$ to indicate the posterior probability. To find the posterior probability of the remaining possibilities, we divide each by $$P(Dead) = 50\%$$

$$
\begin{aligned}
    P'(Duke, Dead) &= \frac{P(Duke, Dead)}{P(Dead)} &= \frac{25\%}{50\%} = 50\% \newline
    P'(Count, Dead) &= \frac{P(Count, Dead)}{P(Dead)} &= \frac{25\%}{50\%} = 50\%
\end{aligned}
$$

## Conditioning

Now since we know that the Duchess is dead, $$P'(Duke, Dead)$$ and $$P'(Count, Dead)$$ are redundant: they are just equal to $$P'(Duke)$$ and $$P'(Count)$$ respectively. So:

$$
\begin{aligned}
    P'(Duke) &= \frac{P(Duke, Dead)}{P(Dead)}\newline
    P'(Count) &= \frac{P(Count, Dead)}{P(Dead)}
\end{aligned}
$$


We can generalize this formula. After learning some evidence (e.g. the Duchess is Dead), a Bayesian reasoner's posterior probability of some hypothesis (e.g. the Duke or the Count did it), should be:

$$
\begin{aligned}
    P'(Hypothesis) &= \frac{P(Hypothesis, Evidence)}{P(Evidence)}
\end{aligned}
$$

----------

## Summary

So far, we have engaged in Bayesian inference without using the famous Bayes' Theorem, or the concept of conditional probability. Once you understand the above conditioning rule, these will be very easy to understand. but I won't explain them here, as I think is where my ability to contribute some unique insight about Bayesian inference ends.

Here's a summary of the idea presented in this essay:

- Start with prior beliefs as a joint probability distribution 
- Eliminate possibilities inconsistent with new evidence
- Reallocate probability proportionally to remaining possibilities
  - By dividing their probability by the prior probability of the evidence
- Update beliefs sequentially by eliminating possibilities as new evidence is learned
- Make inferences about hypotheses based on evidence by simply calculating the total posterior probability of the hypothesis

## Summary

Which brings us back to Sherlock Holmes: *When you have eliminated the impossible, all that remains, no matter how improbable, must be the truth.* Paraphrasing this gives us a good summary of the idea of Bayesian inference as reallocation of credibility across possibilities:

> When you have eliminated the impossible, the probability of all that remains, no matter how improbable, must be scaled to sum to 1.
> 
> -- Sherlock Thomas Bayes Holmes (Jonathan Warden)




----






This rule for updating the posterior probability of a hypothesis is sometimes called the rule for **conditioning**.

## Conditional Probabilities

The right-hand-side of the above equation is the definition of **conditional probability**. The conditional probablity is often written simply as $$P(Hypothesis \vert Evidence)$$. So the rule for conditioning can be written:

$$
\begin{aligned}
    P'(Hypothesis) &= P(Hypothesis|Evidence) 
\end{aligned}
$$

The conditional probability $$P(Hypothesis \vert Evidence)$$ should be thought of as the Bayesian reasoner's **hypothetical** prior belief: what they believe the probability of the hypothesis **would be** if the evidence were true. Whereas the posterior probability $$P'(Hypothesis)$$ represents their posterior belief **after** learning actually learning that the evidence is true.

For example, $$P(Count \vert Dead)$$ is Holmes' belief, before learning that the Duchess is dead, of the probability that the Count did it if the Duchess **were** dead. When Holmes learns that the Duchess is in fact dead, his posterior belief should be updated to equal precisely this probability.

## Bayes Theorem

Note all the Bayesian inference we have done without even touching Bayes theorem


## Summary

Which brings us back to Sherlock Holmes: *When you have eliminated the impossible, all that remains, no matter how improbable, must be the truth.* Paraphrasing this gives us a good summary of the idea of Bayesian inference as reallocation of credibility across possibilities:

> When you have eliminated the impossible, the probability of all that remains, no matter how improbable, must be scaled to sum to 1.
> 
> -- Sherlock Thomas Bayes Holmes (Jonathan Warden)





--------------




# OLd

## The Math



For example, if a Bayesian reasoner draws a random card from a standard 52-card deck without looking at it, her prior beliefs are that the probability of each card is exactly $$\frac{1}{52}$$. The total of the probabilities is $$\frac{1}{52}\times52=1$$.


<img src="/assets/images/distributed-bayesian-reasoning/bayesian-gambler-prior.svg"
     alt="A Bayesian Gambler's Priors Beliefs"
     style="display: block; margin-left: auto; margin-right: auto; height: 400px" />

## Revising Beliefs

Bayesian reasoners then revise their beliefs when they acquire evidence according to very simple rules:

- reject any possibilities that are incompatible with the evidence
- reallocate probability to the remaining possibilities so that they sum to 1 

For example, if the Bayesian card player peeks under one corner and learns that the card is a heart, she must update her beliefs as follows:

- eliminate the 13×3=39 possible cards that are not hearts
- reallocate probability to the remaining 13 cards so that they sum to 1

The result is that the 13 remaining hearts now each have a 1/13 probability, and the total probability still adds up to 1.

<img src="/assets/images/distributed-bayesian-reasoning/bayesian-gambler-posterior.svg"
     alt="A Bayesian Gambler's Posterior Beliefs"
     style="display: block; margin-left: auto; margin-right: auto; height: 450px" />

These new beliefs are the Bayesian reasoners **posterior** beliefs.

This might seem too simple to warrant all the ado about Bayesian inference. But in fact this is all that Bayesian inference is, whether it is used for winning at cards or cracking the Enigma code. 

Unfortunately real-world application of Bayesian inference can be difficult because priors are often complex, hard to describe, or not fully known. 

For example, suppose the Bayesian card player peaks under one corner of the card but doesn't see it clearly, and concludes there is a 75% chance that it is a heart, but 25% chance that it is a diamond? Or what if the prior beliefs are described by a continuous probability distribution?

TDOO: not hard

## Reallocating Probabilities

Reallocating probability among the remaining 13 cards in this example was trivial because we know each card has equal probability. Other scenarios may be more complex. The general rule for reallocating probability is to scale up the probabilities of each remaining possibility such that they sum 1.

To scale a set of numbers such that they sum to 1, just divide each by the total. In this case, the total probability of a heart was $$\frac{1}{4}$$. So the posterior probability of each remaining heart is its prior probability divided by $$\frac{1}{4}$$: $$\frac{1/52}{1/4} = \frac{1}{13}$$

## Repeating Inferences

Suppose Sherlock Holmes has three suspects in the case of the Disappearing Duchess. His prior beliefs in the probability of guilt of each of the 3 suspects is shown in the chart below.

          Prior Beliefs 
     -------------------------
      50%                     
       #     37.5%            
       #       #              
       #       #     12.5%    
       #       #       #      
     --------------------------
      The     The     The      
      Duke    Count   Gardener 

Now suppose Holmes finds evidence that rules out the Duke. The total probability of the possibilities that are compatible with the evidence (the remaining two suspects) is 37.5% + 12.5% = 50%. So the probability of the each of the remaining two possibilities is divided by 50%.

But another way of looking at this is that the 50% "probability mass" previously allocated to The Duke is reallocated to the other two possibilities. This is illustrated in the chart below:

          Prior Beliefs                  Posteriors Beliefs
     -------------------------        ---------------------------
                                                75%               
                                                 #                
      50%                                        #                
       #     37.5%                               #                
       #       #                                 #      25%       
       #       #     12.5%                       #       #        
       #       #       #                 0%      #       #        
     --------------------------        --------------------------
      The     The     The               The     The     The      
      Duke    Count   Gardener          Duke    Count  Gardener  

Suppose Holmes subsequently finds evidence that rules out the Count. We now simply repeat the process. The posterior after the last piece of evidence become the prior for the next. So The probability of the remaining possibility, the Gardener, is now 20%/20% = 100%. All probability mass is reallocated from The Count to The Gardener, as illustrated below.

          Prior Beliefs                   Posteriors Beliefs
     --------------------------        -------------------------
                                                        100%    
              75%                                        #      
               #                                         #      
               #                                         #      
               #                                         #      
               #                                         #      
               #      25%                                #      
               #       #                                 #      
       0%      #       #                 0%     0%       #      
     --------------------------        -------------------------
      The     The     The               The     The     The      
      Duke    Count   Gardener          Duke    Count  Gardener  


## Joint Probabilities

In the real world, the set of possibilities are more complex. They usually involve a **combination** of propositions such as *The Count Did It*, and their **joint probabilities**.

For example, suppose that after exonerating the Duke, Holmes thinks there is a 50% probability that the Count kidnapped the Duchess, a 25% that probability that the Count murdered the Duchess, and a 25% chance that the Gardener murdered the Duchess.

| Prior Beliefs
| How       | Who      | Probability 
| --------- | ---------|--------------
| Kidnapped | Count    | 50%          
| Murdered  | Count    | 25%         
| Murdered  | Gardener | 25%         

Note the total probability that the Count did it is 75%, and the total probability that the Gardener did it is 25%.

Suppose that the Duchess's body is found buried under the Atrium. We now must eliminate the first possibility, and scale up the other two possibilities.

| Posterior Beliefs
| How       | Who      | Probability 
| --------- | ---------|--------------
| Kidnapped | Count    | 0%          
| Murdered  | Count    | 50%         
| Murdered  | Gardener | 50%         

Notice that the probability that the Gardener did it increased, but the probability that the Count did it decreased! 

This is because when probabilities are reallocated in Bayesian inference, they are reallocated among **possibilities**, not **propositions**. Each of the possibilities is scaled up in equal proportion. But the probability of an individual proposition may increase, decrease, or stay the same.

## The Math




In general, the total probability of the possibilities compatible with the evidence is just the total probability of the evidence. If we say that $$P(E)$$ is the probability of the evidence, then Bayesian inference basically comes down to dividing by $$P(E)$$.

TODO: don't use H. The possibilities are the combined possibilities. States of the world?

$$
\begin{aligned}
    P'(H) &= 0  &&~~~\text{if H is not compatible with E}\\
    P'(H) &= \frac{P(H)}{P(E)} &&~~~\text{if H is compatible with E}
\end{aligned}
$$

Where $$P'$$ is the posterior probability, $$P$$ is the prior probability, $$H$$ is any possibility (often called a *hypothesis*), and $$E$$ is the evidence.

## Another Example

Suppose Sherlock Holmes has three suspects in the case of the Disappearing Duchess. His beliefs in the probability of guilt of each of the 3 suspects is:

1. The Duke: 70%
1. The Caretaker: 20%
1. The Mail Carrier: 10%

Suppose evidence is found exonerating the Duke completely. What are Holmes new beliefs?

The total probability of the possibilities that are compatible with the evidence is 20% + 10% = 30%. So Holmes' posterior beliefs are:

1. The Duke: 0%
1. The Caretaker: 20%/30% = 67% 
1. The Mail Carrier: 10%  = 33%



Perhaps counter-intuitively, this is also the probability of the evidence. To understand this, consider that Sherlock believed there was only a 30% chance that the Duke was not guilty. 





<!--
Or more generally:

$$
    P'(\text{Remaining Possibility}) = \frac{P(\text{Remaining Possibility})}{P(\text{Evidence})}
$$



-->



<style>
.custom-aside
{
  margin: auto;
  background-color: lightgrey;
  border: 1px solid black;
  max-width: 600px;
  padding-top: 1em;
  padding-bottom: 0px;
  padding-left: 1em;
  padding-right: 1em;
  margin-bottom:  1em;
}

aside h3 {
    margin-top: 0px;
}

</style>

<aside class="custom-aside" markdown="1">

### We Don't Need No Conditional Probabilities

You may be familiar with the more common form of the formula for Bayesian inference written in terms of **conditional probabilities**.

$$
    P'(H) = P(H \vert E)
$$

And of course the conditional probability is often calculated using Bayes rule.

But Bayesian inference does not actually require using conditional probabilities or Bayes rule. Here's why:

The conditional probability is defined as:

$$
     P(H|E) = \frac{P(H,E)}{P(E)} 
$$

Where $$P(H,E)$$ is the probability that both $$H$$ and $$E$$ are true. 

But for possibilities that are incompatible with the evidence, $$P(H,E)=0$$. For example the ace of spades can't be a heart, so: 

$$
    P(\text{Ace of Spades}, \text{Heart})=0
$$

Thus:

$$
    P(\text{Ace of Spades} \vert \text{Heart})=0
$$

For the possibilities that *are* compatible with the evidence, $$P(H,E)$$ is just $$P(H)$$. For example:

$$
    P(\text{Ace of Hearts}, \text{Heart})=P(\text{Ace of Hearts})
$$

Thus

$$
\begin{aligned}
    P(H|E) = \frac{P(H,E)}{P(E)} &= 0  &&\text{if H is not compatible with E}\\
    P(H|E) = \frac{P(H,E)}{P(E)} &= \frac{P(H)}{P(E)} &&\text{if H is compatible with E}
\end{aligned}
$$

So the rule $$P'(E) = P(H \vert E)$$ can be formulated as:

$$
\begin{aligned}
    P'(H) &= 0  &&~~~\text{if H is not compatible with E}\\
    P'(H) &= \frac{P(H)}{P(E)} &&~~~\text{if H is  compatible with E}
\end{aligned}
$$

</aside>

## Summary

Which brings us back to Sherlock Holmes: *When you have eliminated the impossible, all that remains, no matter how improbable, must be the truth.* Paraphrasing this gives us a good summary of the idea of Bayesian inference as reallocation of credibility across possibilities:

> When you have eliminated the impossible, the probability of all that remains, no matter how improbable, must be scaled to sum to 1.
> 
> -- Sherlock Thomas Bayes Holmes (Jonathan Warden)



