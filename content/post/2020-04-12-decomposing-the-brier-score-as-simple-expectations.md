---
title: Decomposing the Brier score as simple expectations
date: 2020-04-16
lastmod: 2023-12-16
author: ~
slug: decomposing-the-brier-score-as-simple-expectations
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

There are plenty of articles out there on decomposing the Brier score, but they're usually done in notation that's non-standard for probability and statistics. I give the decomposition in more standard notation, for the simple case where we predict a binary outcome. I use expectations, instead of the empirical means that would be calculated in practice, since this simplifies the notation. Converting them to empirical means is straightforward.

## Posterior / conditional decomposition

Suppose we are predicting the value of some variable $Y$, that can take values of 0 (failure) and 1 (success). We are given data $X=x$ to use in the prediction, and we predict a success with probability $p := p(x)$.

Once we observe $Y=y$, we can assign the prediction $p$ a score $S(p, y) := (p - y)^2$, which we seek to minimise. Before we observe $Y$, we can calculate the expected score, given the data $X$:
$$\begin{align*}
\mathbb{E}(S(p, Y)|X)
&= \mathbb{E}((p(X) - Y)^2|X)\\\\
&= \mathbb{E}((p(X) - \mathbb{E}(Y|X) + \mathbb{E}(Y|X) - Y)^2|X)\\\\
&= \mathrm{Var}(Y|X) + (\underbrace{p(X) - \mathbb{E}(Y|X)}\_{\textrm{bias}(p(X)|X)})^2.
\end{align*}$$

This works by inserting intermediate expressions for $\mathbb{E}(Y|X)$ in the squared term, and then splitting the square, using the fact that $p(X) - \mathbb{E}(Y|X)$ and $Y - \mathbb{E}(Y|X)$ are independent given $X$, since the former term has zero variance.

As an alternative approach, we notice that the starting expectation is the expectation of a squared expression, so we can split that into the variance and the square of the expectation:

$$\begin{align*}
\mathbb{E}(S(p, Y)|X)
&= \mathbb{E}((p(X) - Y)^2|X)\\\\
&= \mathrm{Var}(p(X) - Y|X) + \mathbb{E}(p(X) - Y|X)^2\\\\
&= \mathrm{Var}(Y|X) + (p(X) - \mathbb{E}(Y|X))^2.
\end{align*}$$

Again, this simplifies because $p(X)$ is invariant given $X$.

This is the standard variance-bias decomposition for the posterior mean squared error. It attains its minimum when $p(x)$ is equal to the posterior success probability $\mathbb{E}(Y|X=x) = \mathbb{P}(Y=1|X=x)$. We therefore have no reason to give a prediction that deviates from our true belief in the posterior success probability, so $S$ is a proper score function. This isn't surprising, given that $S$ is the Brier score.

## Prior / unconditional decomposition

If we also take the expectation over the data $X$, then the above approach doesn't simplify: $p(X)$ no longer has zero variance, and $p(X) - \mathbb{E}(Y)$ and $Y - \mathbb{E}(Y)$ aren't independent. We therefore get additional variance and covariance terms:
$$\begin{align*}
\mathbb{E}(S(p, Y))
=&\\, \mathrm{Var}(p(X) - Y) + (\mathbb{E}(p(X)) - \mathbb{E}(Y))^2\\\\
=&\\, \mathrm{Var}(Y) + (\mathbb{E}(p(X)) - \mathbb{E}(Y))^2 + \mathrm{Var}(p(X))\\\\
&- 2 \textrm{Cov}(p(X), Y).
\end{align*}$$

We can obtain a clearer decomposition by taking the expectation of the prior decomposition, using the Tower property:

$$\begin{align*}
\mathbb{E}(S(p, Y))
&= \mathbb{E}(\mathbb{E}(S(p, Y)|X))\\\\
&= \underbrace{\mathbb{E}(\mathrm{Var}(Y|X))}\_{\textrm{refinement}} + \underbrace{\mathbb{E}((p(X) - \mathbb{E}(Y|X))^2)}\_{\textrm{calibration}\ldots}\\\\
&= \underbrace{\mathrm{Var}(Y)}\_\textrm{uncertainty} - \underbrace{\mathrm{Var}(\mathbb{E}(Y|X))}\_{\textrm{resolution}} + \underbrace{\mathbb{E}((p(X) - \mathbb{E}(Y|X))^2)}\_{\ldots\textrm{ AKA reliability}},
\end{align*}$$
where the final line comes from applying the law of total variance.

These decomposition terms are all positive, and have intuitive meanings.

- Uncertainty is the inherent variability of the value of $Y$. Even if your predictions were perfect, with perfect information, this term would still remain. Since $Y$ is a Bernoulli variable, the uncertainty is largest when $\mathbb{E}(Y) = 1/2$. You want the uncertainty to be low, but it's usually outside of your control.
- Resolution measures how much the posterior mean $\mathbb{E}(Y|X)$ varies over different values of the data $X$. The more the posterior mean varies, the more information the data is providing. At zero resolution, the posterior mean does not vary, but is always equal to the prior mean: the data provides no information. Resolution can therefore be thought of as how potentially helpful a prediction can be, since it reflects the quality of the data used to make it. You want this to be large, and it is sometimes within your control, depending on whether you're in control of the data collection process.
- Calibration, or reliability, measures the deviation of the prediction function $p(x)$ from the optimal prediction $\mathbb{E}(Y|X=x)$. You want the calibration to be low, which is confusingly the opposite of resolution, the other positively-phrased term. Whoops! It should really have been called miscalibration. This is also within your control: resolution depends on the quality of the data, calibration depends on the quality of the prediction made with that data.
- Refinement is the difference between the uncertainty and the resolution. It's the expected score for a perfectly-calibrated prediction. At zero refinement, the data gives enough information to predict $Y=0$ or $Y=1$ with probability one; this is generally not possible, outside of degenerate cases such as knowing the outcome in advance, i.e. $X = Y$. Refinement is another positively-phrased term that you actually want to be small.

Note that the resolution and the reliability conveniently split two different modelling decisions: the resolution reflects the quality of the data used, and the reliability reflects the quality of the prediction made with that data.

A prediction with low resolution and low calibration is like a mathematician's answer: technically correct, but unhelpful. A prediction with high resolution and high calibration is like a bad pundit's answer: it takes account of lots of information, but in such a way that any prediction is likely to be far off the mark.

We can also see why we shouldn't use Brier scores to compare predictions on different variables: since the uncertainty term depends only on what is being predicted, rather than the performance of the prediction, a predictor for a variable with lower uncertainty has the advantage of a smaller uncertainty term. This is true even if all the predictors being compared are perfectly calibrated, and use the same data.

As a simple example, we can make a predictor that we flip a fair coin and get a head, and a predictor that we flip a fair coin two times and get two heads. Both predictors are perfectly calibrated, but the former predictor is for a variable with a larger uncertainty, so it has a larger expected score.

## The effect of binning

This section follows Stephenson et al. 2008, with changed notation.

One issue with empirically calculating the resolution and calibration is that, unless the predictions come from a small number of possible values, they are difficult to estimate well in practice, since you're estimating $\mathbb{E}(Y|X)$ with very small samples. Therefore, most people put the predictions $p(X)$ into bins.

We effectively choose a summary function $b$ that bins the prediction probabilities, $b(p(s(X))).$ This is usually a simple step function, making the bins contiguous. Since $b$ is a function of a single prediction $p$, this also excludes binning processes that consider all observations at once, such as $k$-means clustering, which simplifies things considerably. It does include more common options, such as $b$ rounding $p$ to the nearest multiple of $0.05$, a common choice on calibration plots.

We begin as above for the prior decomposition, but we use the expectation conditional on $B$ rather than on $X$. Since $p(X)$ is not invariant given $B$, this adds new covariance terms, like in the Tower-less prior case:
$$\begin{align*}
\mathbb{E}(S(p, Y)|B)
=&\\, \mathrm{Var}(p(X) - Y|B) + \mathbb{E}(p(X) - Y|B)^2\\\\
=&\\, \mathrm{Var}(p(X)|B) + \mathrm{Var}(Y|B)\\\\
&+ (\mathbb{E}(p(X)|B) - \mathbb{E}(Y|B))^2 - 2\textrm{Cov}(p(X), Y|B).
\end{align*}$$
These terms remain unsimplified in the unconditional expectation:
$$\begin{align*}
\mathbb{E}(S(p, Y))
=&\\, \mathbb{E}(\mathrm{Var}(Y|B)) + \mathbb{E}(\mathrm{Var}(p(X)|B))\\\\
&+ \mathbb{E}((\mathbb{E}(p(X)|B) - \mathbb{E}(Y|B))^2) - 2\mathbb{E}(\textrm{Cov}(p(X), Y|B))\\\\
=&\\, \mathrm{Var}(Y) - \underbrace{\mathrm{Var}(\mathbb{E}(Y|B))}\_\textrm{binned resolution} + \underbrace{\mathbb{E}((\mathbb{E}(p(X)|B) - \mathbb{E}(Y|B))^2)}\_\textrm{binned reliability}\\\\
&+ \underbrace{\mathbb{E}(\mathrm{Var}(p(X)|B))}\_\textrm{within-bin variance} - 2\underbrace{\mathbb{E}(\textrm{Cov}(p(X), Y|B))}\_\textrm{within-bin covariance}.
\end{align*}$$
The first three terms are clear analogues to the ones we had before. The last two terms are new, arising from $p(X)$ not being removed from the conditional variance term. They exactly counter the change in the resolution and reliability terms: for decent prediction functions $p$, this should mean that their sum is negative, since the resolution-reliability sum should increase due to the loss of information when binning.

Special cases:
- If $p$ only depends on $X$ through $B := b(X)$ as a summary statistic, then $p(X)$ is invariant conditional on $B$, and the within-bin terms are equal to zero. Let $\bar{p}(b(x)) := p(x)$. This results in the expressions $$\mathbb{E}(\bar{p}(B) - \mathbb{E}(Y|X))^2 - \mathbb{E}(\bar{p}(B) - \mathbb{E}(Y|B))^2$$and
$$\mathrm{Var}(\mathbb{E}(Y|X)) - \mathrm{Var}(\mathbb{E}(Y|B))$$
being equal.
- If $b$ is the identity function, we have as many bins as possible prediction values. The within-bin terms are equal to zero, and we get the unconditional decomposition from before.

References:
- Bröcker, J. (2009), Reliability, sufficiency, and the decomposition of proper scores. Q.J.R. Meteorol. Soc., 135: 1512-1519. doi:10.1002/qj.456
- Stephenson, D.B., Coelho, C.A., and Jolliffe, I.T., 2008: Two extra components in the Brier score decomposition. Wea. Forecasting, 23, 752–757, doi:10.1175/2007WAF2006116.1
