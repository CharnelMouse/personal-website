---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Thesis"
subtitle: "Convergence properties of Approximate Bayesian computation"
summary: "Convergence properties of Approximate Bayesian computation"
authors: [mark-webster]
tags:
- Approximate Bayesian computation
- Work project
categories: []
date: 2016-07-31
lastmod: 2016-07-31
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "Smart"
  preview_only: false

---

![](abc2.png)

Approximate Bayesian Computation is a Monte Carlo method that is rather naïve, because in its basic form it does not use any assumptions about the distributions involved, outside of the prior densities and the model process. This naiveté results in computational inefficiency, but means ABC can be applied to problems that are not yet well understood enough for more sophisticated methods.
However, one complication is that the algorithm requires a tolerance parameter, which determines how forgiving it is about simulation data being different from the original observations. Setting this parameter is a balance between bias introduced by being overly forgiving, and Monte Carlo error introduced by being too unforgiving, and not being left with enough simulated samples.
Outside of looking at particular model cases, this rapidly becomes a problem that requires use of asymptotics, where we look at the behaviour of the error, and the optimal choice of tolerance, as the amount of computation time for generating samples tends to infinity. The results can differ, depending on which variant of ABC is being used.
