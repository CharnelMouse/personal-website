---
title: Decomposing general scores as simple expectations
date: 2021-09-12
lastmod: 2021-09-12
author: ~
slug: decomposing-general-scores-as-simple-expectations
categories: []
tags:
- Scoring rules
subtitle: ''
summary: ''
authors: []
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
math: true
---

[Previously](../decomposing-the-brier-score-as-simple-expectations), we decomposed the Brier score into simple expectations. We saw that, unlike the posterior decomposition, where we get the standard variance-bias decomposition for mean squared error, the prior decomposition splits into three terms of similar form. For a binary outcome $Y$, data $X$, and scoring rule $S(p, y) := (p - y)^2$, where $p(X)$ is the predicted probability that $Y = 1$, we found that
$$\begin{align*}
\mathbb{E}(S(p, Y))
&= \underbrace{\mathbb{E}(\mathrm{Var}(Y|X))}_{\textrm{refinement}} + \underbrace{\mathbb{E}((p(X) - \mathbb{E}(Y|X))^2)}_{\textrm{calibration}\ldots}\\\\
&= \underbrace{\mathrm{Var}(Y)}_\textrm{uncertainty} - \underbrace{\mathrm{Var}(\mathbb{E}(Y|X))}_{\textrm{resolution}} + \underbrace{\mathbb{E}((p(X) - \mathbb{E}(Y|X))^2)}_{\ldots\textrm{ AKA reliability}}.
\end{align*}$$

I'll now generalise this decomposition to cover all proper scoring rules. I'll be following the approach in Bröcker 2009 again, more closely than before now that we're looking at general score functions. I'll still write things in terms of expectations, since I found Bröcker's notation hard to keep track of when I first read the paper.

## Divergence

Instead of limiting $Y$ to be a binary variable, we can now let it be either a discrete categorical variable, or a continuous real variable. To cover both cases, we proceed in terms of distributions functions, rather than the probabilities we used before. Our prediction $P$ is now a probability distribution function, and the score function $S(P, Y)$ takes a distribution function as its first parameter. The prediction distribution for a value $y$ of $Y$, given data $x$, is $P(y|x)$. This lets us more easily refer to possible values of $Y$.

Key to what follows is the concept of *divergence*. Suppose that the real distribution of $Y$, given $X$, is $Q(X)$. Then the divergence is defined as
$$d(P, Q) := \mathbb{E}_Q(S(P(X), Y) - S(Q(X), Y)),$$
the expected score penalty for using the prediction $P$ instead of the true prediction $Q$. Divergence is important, because we want it to be positive: if the divergence can be negative, it means there is some prediction $p$ that performs better than the true distribution, and the scoring function rewards not telling the truth: it can be "gamed".

In fact, we can define proper scoring rules in terms of their divergence function: a scoring rule is *proper* if $d(P, Q) \geq 0$ for all $P$ and all $Q$, and a proper scoring rule is *strictly proper* if $d(P, Q) = 0$ if and only if $P = Q$.

## Conditional decomposition

Let's start with the conditional decomposition again, making use of the true distribution $Q$. We can say that
$$\begin{align*}
\mathbb{E}_Q(S(P, Y) | X)
&= \mathbb{E}_Q(S(Q, Y) - S(Q, Y) + S(P, Y) | X)\\\\
&= \mathbb{E}_Q(S(Q, Y) | X) + \mathbb{E}_Q(S(P, Y) - S(Q, Y) | X).
\end{align*}$$
There's nothing to interesting here, except to note that these two terms directly relate to the terms in the variance-bias decomposition: instead of the variance, we have a term that only depends on $Q$, and not $P$.

## Unconditional decomposition

Let's now try to find the unconditional decomposition using the Tower property, as we did before:
$$\begin{align*}
\mathbb{E}_Q(S(P, Y))
&= \mathbb{E}(\mathbb{E}_Q(S(P, Y) | X))\\\\
&= \mathbb{E}(\mathbb{E}_Q(S(Q, Y) | X)) + \mathbb{E}(\mathbb{E}_Q(S(P, Y) - S(Q, Y) | X))\\\\
&= \underbrace{\mathbb{E}(\mathbb{E}_Q(S(Q, Y) | X))}_{\textrm{refinement}} + \underbrace{d(P, Q)}_{\textrm{calibration}}.
\end{align*}$$
This is similar to before. However, when we further decomposed the refinement for the Brier score, we did it using the law of total variance, and we can't use that here.

However, we know that, when doing the decomposition for the Brier score, we had an uncertainty term in terms of the unconditional behaviour of $Y$, instead of the conditional behaviour. Therefore, we can progress here by introducing the true prior distribution $\bar{Q}$, and comparing it to $Q(X)$:
$$\begin{align*}
\mathbb{E}(\mathbb{E}_Q(S(Q, Y) | X))
&= \mathbb{E}(\mathbb{E}_Q(S(\bar{Q}, Y) - S(\bar{Q}, Y) +  S(Q(X), Y) | X))\\\\
&= \mathbb{E}(\mathbb{E}_Q(S(\bar{Q}, Y) | X)) - \mathbb{E}_Q(S(\bar{Q}, Y) - S(Q(X), Y) | X))\\\\
&= \mathbb{E}_Q(S(\bar{Q}, Y)) - d(\bar{Q}, Q).
\end{align*}$$
Finally, we can rewrite the first term as
$$\begin{align*}
\mathbb{E}_Q(S(\bar{Q}, Y))
&= \mathbb{E}(\mathbb{E}_Q(S(\bar{Q}, Y) | X))\\\\
&= \mathbb{E}\left(\int S(\bar{Q}, y) \\, \textrm{d}Q(y|X)\right)\\\\
&= \iint S(\bar{Q}, y) \\, \textrm{d}Q(y|x) \\, \\textrm{d}\Pi(x)\\\\
&= \iint S(\bar{Q}, y) q(y|x) \\, \textrm{d}y \\, \\textrm{d}\Pi(x)\\\\
&= \int S(\bar{Q}, y) \left(\int q(y|x) \\, \\textrm{d}\Pi(x)\right) \\, \textrm{d}y \quad \textrm{(by Fubini's theorem)}\\\\
&= \int S(\bar{Q}, y) \bar{q}(y) \\, \textrm{d}y\\\\
&= \int S(\bar{Q}, y) \\, \textrm{d}\bar{Q}(y)\\\\
&= \mathbb{E}_{\bar{Q}} (S(\bar{Q}, Y)),
\end{align*}$$
where $\Pi$ is the distribution function of $X$. (Fubini's theorem is not general enough to cover cases where the true expected absolute score $\mathbb{E}_Q(|S(Q, Y)|)$ is infinite, but these are bizarre cases that don't matter in practice.)

We therefore have the final set of decompositions,
$$\begin{align*}
\mathbb{E}_Q(S(P, Y))
&= \underbrace{\mathbb{E}(\mathbb{E}_Q(S(Q, Y) | X))}_{\textrm{refinement}} + \underbrace{d(P, Q)}_{\textrm{calibration...}}\\\\
&= \underbrace{\mathbb{E}_\bar{Q}(S(\bar{Q}, Y))}_{\textrm{uncertainty}} - \underbrace{d(\bar{Q}, Q)}_{\textrm{resolution}} + \underbrace{d(P, Q)}_{\textrm{i.e. reliability}}.
\end{align*}$$
Isn't that nice and simple? The expression $\mathbb{E}_P(S(P, Y))$ is referred to as the *entropy* of $P$, and is common enough here that we could write it as $e(P) := \mathbb{E}_P(S(P, Y))$. The uncertainty is thus the entropy of the true prior distribution $\bar{Q}$, and we have
$$\mathbb{E}_Q(S(P, Y)) = e(\bar{Q}) - d(\bar{Q}, Q) + d(P, Q).$$

Once again, we have an uncertainty term dependent only on the prior distribution $\bar{Q}$, a resolution term based on the information provided by the data, and a reliability term based on the information lost by not using the true conditional distribution $Q$. Since $S$ is a proper scoring, we also know that the two latter terms, being divergences, are positive, as before, so we can apply the same intuitive explanations of the terms as we did before.

This decomposition even works for non-proper scoring rules, too. However, in that case the two divergence terms need not be positive, so the uncertainty term needn't be the best-case expected score, and we lose the intuitive nature of the terms.

## Examples

A few simple examples of score decompositions, via entropy and divergence. For more examples, look at Bröcker 2009.

#### Brier score revisited, and proper linear score

Switching back to using a probability $p$ rather than a distribution function for a moment, in the case of the Brier score for a binary variable $Y$ we had the score function $S(p, Y) = (p - Y)^2$. This gives us an expected score under real probability $q$ of
$$\mathbb{E}_q(S(p, Y)) = (1-q)p^2 + q(1-p)^2 = (p-q)^2 + q(1-q).$$
Writing this as on the right-hand side makes it clear that we have entropy $e(p) = p(1-p)$, and divergence $d(p,q) = (p-q)^2$, which is symmetric in $P$ and $Q$. This gives us the decomposition
$$\mathbb{E}_q(S(p, Y)) = \bar{q}(1 - \bar{q}) - (q - \bar{q})^2 + (p - q)^2.$$
Since $\mathrm{Var}(Y) = \bar{q}(1 - \bar{q})$ for a binary variable $Y$, and $\mathrm{Var}(q) = (q - \bar{q})^2$, this is indeed the same decomposition as we found before.

As a generalisation, we can look at the proper linear score (PLS), which works in the general case, not just the two-category binary case. In addition to a term for the square probability of $Y$ not being equal to the observed value, we also have a term for the square probability of $Y$ being equal to each unobserved value. This is the sum/integral of $(1 - P'(y))^2$ at the observed $y$, and $P'(z)^2$ at all $z \neq y$, i.e. $S(P, Y) = \int P'(z)^2 \\, \textrm{d}z - 2P'(Y) + 1$. Usually, the $1$ is left off for simplicity, which allows the score to be negative too.

#### Logarithmic score

The other standard proper score is the logarithmic score, $S(P, Y) = -\log(P'(Y))$. This has expected score
$$\mathbb{E}_Q(S(P, Y)) = -\mathbb{E}_Q(\log(P'(Y))),$$
with entropy $e(P) = -\mathbb{E}_P(\log(P'(Y)))$ and divergence $d(P, Q) = \mathbb{E}_Q(\log(Q'(Y)/P'(Y)))$. These are a bit more messy to work with. However, the logarithmic score is simple to use, and it ties neatly into information theory, since $-\log(P'(y))$ is the negative of the information, i.e. the surprisal, gained when observing $y$, and the divergence $d(P, Q)$ is the Kullback-Leibler divergence / relative entropy from $P$ to $Q$.

#### Conditional ranked probability score

The idea behind the conditional ranked probability score (CRPS) is that, for a single observation, the ideal distribution function to forecast it with is the Heaviside step function centred on the observation, and the prediction distribution should be penalised by the square of its deviation from that step function:
$$S(P, Y) = \int (P(x) - H(Y-x))^2 \\, \textrm{d}x.$$
More generally, if we add together the score from $N$ observations $y_n$, we get
$$\begin{align*}
S_N(P, Y)
&= \sum_n \int (P(x) - H(y_n - x))^2 \\, \textrm{d}x\\\\
&= \int \sum_n (P(x) - H(y_n - x))^2 \\, \textrm{d}x \quad \textrm{(by Fubini's theorem)}\\\\
&= N \int \left(\left(P(x) - \bar{H}(x)\right)^2 + \bar{H}(x) - \bar{H}(x)^2\right) \\, \textrm{d}x\\\\
&= N \int (P(x) - \bar{H}(x, Y))^2 \\, \textrm{d}x + \int \bar{H}(x, Y) (1 - \bar{H}(x, Y)) \\, \textrm{d}x,
\end{align*}$$
where $\bar{H}(x, Y) := \sum_{n=1}^N H(y_n - x)/N$ is the mean of the step functions, i.e. the empirical distribution of the observations $y_n$. We can therefore see that the CRPS compares the prediction distribution against the empirical distribution, with an additional term that reflects the entropy of the empirical distribution, and depends on Q, not P. For a single observation, this term vanishes, since the square of a step function is equal to the step function.

The expected simple CRPS is
$$\begin{align*}
\mathbb{E}_Q(S(P, Y))
&= \iint (P(x) - H(x-y))^2 \\, \textrm{d}x \\, \textrm{d}Q(y)\\\\
&= \iint (P(x)^2 - 2P(x)H(x-y) + H(x-y)) \\, \textrm{d}Q(y) \\, \textrm{d}x\\\\
&= \int P(x)^2 \\, \textrm{d}x + \int (1 - 2P(x)) Q(x) \\, \textrm{d}x\\\\
&= \int (P(x) - Q(x))^2 \\, \textrm{d}x + \int Q(x)(1 - Q(x)) \\, \textrm{d}x,
\end{align*}$$
which trivially shows the entropy to be $e(P) = \int P(x)(1 - P(x)) \\, \textrm{d}x$ and the divergence to be $d(P, Q) = \int (P(x) - Q(x))^2 \\, \textrm{d}x$.

The integrand for the scoring rule, entropy term, and divergence term look very similar to those for the Brier score. Ignoring the integral and fixing $x$ for the moment, we can see that, compared to the Brier score, we replace the non-occurrence probability $p$ with $P(x)$, and the observed value $Y$ with $H(Y - x)$, where $Y$ can now be continuous. In other words, the integrands take a problem about predicting the value of $Y$, and turn it into a problem about predicting the probability that $Y$ exceeds a threshold value $x$. Reintroducing the integral, we then integrate the Brier score for this prediction over all possible values of $x$.

This means that the CRPS can be viewed as enhancing the Brier score, by incorporating uncertainty about a threshold-driven decision rule via an improper flat prior.

#### Naïve score

For a non-proper example, we can look at the case where $S(P, Y) = -P'(Y)$: in other words, the score is the forecast probability / density of the observed outcome, times minus one to make it a penalty. We then have expected score
$$\mathbb{E}_Q(S(P, Y)) = -\int P'(y) \\, \textrm{d}Q(y) = -\int P'(y) Q'(y) \\, \textrm{d}y,$$
which is symmetric in $P$ and $Q$. We therefore have entropy $e(P) = -\mathbb{E}_P(P'(Y))$, and divergence $d(P, Q) = \mathbb{E}_Q(Q'(Y) - P'(Y))$.

The divergence, and therefore the reliability term, can clearly be negative: we just make the density/mass $P'$ larger than $Q'$ when $Q'$ is large. In the most extreme case, $P$ is entirely concentrated at the value of $Y$ with the largest probability/density.

This encourages extreme overconfidence, since expected score is maximised by predicting the most likely outcome will occur 100% of the time. I think of it as rewarding punditry.

## A note on notation

For those following along with Bröcker, the entropy and devergence notation is the same, but the forecasting notation is different. I have used forecast distribution $P(X)$ and real distribution $Q(X)$ to refer to what Bröcker calls the probabilistic forecasting scheme $\gamma$ and the conditional observation probability $\pi^{(\gamma)}$. Also notable is that, while I've introduced data $X$ here to brings things more in line with standard mathematical notation, Bröcker doesn't make this distinction, and has $\gamma$ as the only random input. This doesn't affect the results: we just replace every instance of $X$ above with $P$.

References:
- Bröcker, J. (2009), Reliability, sufficiency, and the decomposition of proper scores. Q.J.R. Meteorol. Soc., 135: 1512-1519. doi:10.1002/qj.456
