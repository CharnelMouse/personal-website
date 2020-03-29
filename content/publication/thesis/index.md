---
abstract: Approximate Bayesian Computation is a family of Monte Carlo methods used for likelihood-free Bayesian inference, where calculating the likelihood is intractable, but it is possible to generate simulated data, and calculate summary statistics. While these methods are easy to describe and implement, it is not trivial to optimise the mean square error of the resulting estimate. This thesis focuses on asymptotic results for the rate of convergence of ABC to the true posterior expectation as the expected computational cost increases. Firstly, we examine the asymptotic efficiency of the "basic" versions of ABC, which consists of proposal generation, followed by a simple accept-reject step. We then look at several simple extensions, including the use of a random accept-reject step, and the use of ABC to make kernel density estimates. The asymptotic convergence rate of the basic versions of ABC decreases as the summary statistic dimension increases. A naive conclusion from this result would be that, for an infinite-dimensional summary statistic, the ABC estimate would not converge. To show this need not be the case, we look at the asymptotic behaviour of ABC in the case of an observation that consists of a stochastic process over a fixed time interval. We find partial results for two different criteria for accepting proposals. We also introduce a new variant of ABC, referred to in the thesis as the ABCLOC estimate. This belongs to a family of variants, in which the parameter proposals are adjusted, to reduce the difference between the distribution of the accepted proposals and the true posterior distribution. The ABCLOC estimate does this using kernel regression. We give preliminary results for the asymptotic behaviour of the ABCLOC estimate, showing that it potentially has a faster asymptotic rate of convergence than the basic versions for high-dimensional summary statistics.
authors:
- mark-webster
Zdate: "2016-07-31T00:00:00Z"
featured: false
links:
- name: White Rose eTheses Online version
  url: http://etheses.whiterose.ac.uk/16197/
projects: ["thesis"]
publication: "University of Leeds / White Rose eTheses Online repository"
publication_types:
- "7"
publishDate: "2016-07-31T00:00:00Z"
summary: "We also introduce a new variant of ABC."
tags:
- Approximate Bayesian computation
title: Convergence properties of approximate Bayesian computation
---

This looked at the rate of convergence for several versions of the ABC algorithm. Some of the results were also published in [Barber et. al. 2015](../abc-paper). The other main section looks at a new variants, which has a better rate of convergence for posterior mean estimation.
