---
math: true
abstract: Approximate Bayesian Computation (ABC) is a popular computational method for likelihood-free Bayesian inference. The term “likelihood-free” refers to problems where the likelihood is intractable to compute or estimate directly, but where it is possible to generate simulated data $X$ relatively easily given a candidate set of parameters $\theta$ simulated from a prior distribution. Parameters which generate simulated data within some tolerance $\delta$ of the observed data $x^*$ are regarded as plausible, and a collection of such $\theta$ is used to estimate the posterior distribution $\theta|X=x^∗$. Suitable choice of $\delta$ is vital for ABC methods to return good approximations to $\theta$ in reasonable computational time.<br><br>While ABC methods are widely used in practice, particularly in population genetics, rigorous study of the mathematical properties of ABC estimators lags behind practical developments of the method. We prove that ABC estimates converge to the exact solution under very weak assumptions and, under slightly stronger assumptions, quantify the rate of this convergence. In particular, we show that the bias of the ABC estimate is asymptotically proportional to $\delta^2$ as $\delta \downarrow 0$. At the same time, the computational cost for generating one ABC sample increases like $\delta^{-q}$ where $q$ is the dimension of the observations. Rates of convergence are obtained by optimally balancing the mean squared error against the computational cost. Our results can be used to guide the choice of the tolerance parameter $\delta$.
authors:
- "Stuart Barber"
- "Jochen Voss"
- mark-webster
Zdate: "2015-02-06T00:00:00Z"
doi: "10.1214/15-EJS988"
featured: false
links:
- name: ArXiv pre-print
  url: http://arxiv.org/abs/1311.2038
- name: Journal version
  url: https://projecteuclid.org/euclid.ejs/1423229751
projects: ["thesis"]
publication: "Electronic Journal of Statistics, Volume 9, Number 1"
publication_types:
- "2"
publishDate: "2015-02-06T00:00:00Z"
summary: "Our results can be used to guide the choice of the tolerance parameter."
tags:
- Approximate Bayesian computation
title: The rate of convergence for approximate Bayesian computation
---

This looked at the rate of convergence for the most basic version of the ABC algorithm, where either the number of proposals or the number of accepted proposals is fixed, and the estimate is the mean of the desired function of the accepted proposals, without adjustment. There are also some theorems giving conditions for the estimate to converge to the correct answer. The results in this paper were later included in [my thesis](../thesis).
