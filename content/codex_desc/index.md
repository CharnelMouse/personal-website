---
title: Codex model description
summary: Statistical modelling of a card game's forum tournament data.
date: 2021-01-02
lastmod: 2021-01-02
tags:
- Codex
- Side project
math: true
---

## Problem space: Codex game

Here's the information available when starting a Codex match between two players:

- The two players involved, whose skill levels can vary widely.
- The players' decks. Each consists of a starter deck, which are the cards the player starts play with, and three "specs", which are side decks from which cards can be added during the match. The specs also determine which heroes the player has access to throughout the match, so a spec can affect a match even if its cards are never used. Some decks might be significantly advantaged against others: six of the possible decks are the "monocolour" decks, which the game was balanced around during development, and allowing the rest is considered to be a variant. Most tournaments on the Sirlin Games forums use this variant, however.
- Who starts. Codex is an asymmetric game, so going first can play very differently from going second, depending on the decks involved.

Our goal is to take this information, and use it to predict the match outcome. For the sake of simplicity, I've defined this as the probability that Player 1 wins.

Since most of the recorded matches take place on the forum, where the whole game state is viewable, even information considered private for the players during the match, we could try to make a really complex analysis, that takes account of the actions players take during the match. I'm going to ignore all of that, because it requires a much more sophisticated approach.

## Model approach

We have two players, and a deck matchup where, for reasons I'll explain in a bit, I consider the decks together, rather than separately. A simple way to handle these different factors is to assign them each a numerical effect on the outcome probability, so that the total effect is their sum.

### Player skill effects

For the player effects, we use the Bradley-Terry model: the log-odds of Player 1 winning are equal to Player 1's skill, minus Player 2's skill. For example, a skill effect equal to $\log(2)$ increases the log-odds in favour of that player winning by $\log(2)$, i.e. it doubles their win odds, while an effect of $-\log(2) = -\log(1/2)$ halves their win odds. A match between two equally-skilled players has total win log-odds of zero, i.e. odds of 1-to-1, giving a win probability of 50%.

### Deck matchup effects

The deck matchup effects are treated as an additional additive factor to the log-odds of Player 1 winning. The simple way to do this would be to assign each deck a strength effect and add the difference to the total log-odds, in a similar fashion to the player skill effects.

However, this simple approach has a few problems:

- The deck strengths are on a single scale, so if Deck A is advantaged against Deck B, and Deck B is advantaged against Deck C, then Deck A must be advantaged against Deck C. There's no way to account for decks countering decks that otherwise tend to perform better, and the conclusion would be that there is one deck that is advantaged against all the others, and should always be used.
- The only information we get about a deck's strength comes from matches that it appears in. This is a problem, since there are 3084 possible decks, and we only have 771 recorded matches that can be used for modelling. We'd therefore like a deck's performance to partially count for inferring the strength of decks that are similar, i.e. shares a starter deck or some specs.
- Turn order effect is not counted for at all: decks can perform very differently for player 1 and player 2.

To address these problems, I compose the deck effects from 16 separate components:

- 1 starter vs. starter effect;
- 3 starter vs. spec effects (Player 1's starter vs. each of Player 2's specs);
- 3 spec vs. starter effects;
- 9 spec vs. spec effects.

For each component, the part from Player 1's deck is given first. For example, Red vs. Green is for Player 1 Red starter vs. Player 2 Green starter, and its effect can be different from that of Green vs. Red. This allows us to account for turn order.

Note that these effects cannot be evaluated on their own. For example, the Red vs. Green effect doesn't judge how good the Red starter is against the Green starter, it judges how good decks with Red starter tend to do against decks with the Green starter. Similarly, a spec vs. spec effect judges how well decks with those specs tend to perform against each other, whether they're used during the match or not.

### Weaknesses

This approach does have some remaining problems:

- There is no accounting for within-deck synergies. Some pairing of specs are very strong, and this system has no way to capture them.
- Player skill and deck strength don't interact. This can be a problem, since players tend to be more familiar with some deck components than others.
- Player skills don't vary over time. This favours players who were already veterans before the creation of the current forum, since their learning period isn't included in the recorded matches.

Unfortunately, addressing these problems, especially the first two, requires more computing power than I can feasibly draw on locally.

### Partial pooling

For each type of effect -- player skill, starter vs. starter, and so on for other deck interactions -- we do not know the expected spread. This is important, because we are interested in comparing the player skill effect spread and the total deck matchup effect spread: it's a useful measure of how much a matchup depends on the skill of the players, compared to the decks involved. If the player skills contribute more variation, the player skill levels have the dominant effect; if the decks contribute more, the match is mostly decided by the decks used, and player skill is not as important. Ideally, we'd like the see the former, since it would show that player skill is the more important factor.

We therefore use something called partial pooling. To use the player skills as an example, we have the player skill effects be independent of each other, given a value for their shared spread parameter. However, we also do inference on the spread parameter. This has the effect of regularising the posterior estimates for players' skill effects, preventing overfitting.

For example, suppose a player's match data is such that they've won all their matches. Without regularisation, the posterior (maximum-likelihood) estimate for their skill effect would be extremely high, beyond the point of reason, where they'd be guaranteed to win regardless of other circumstances. With partial pooling, such a large skill effect is considered less likely, because it's counter-weighed by the much smaller values of the other players' effects, which share the same spread parameter.

For a simpler example of applying partial pooling, with only one uncontested player skill effect and no deck effects, see [Hierarchical Partial Pooling for Repeated Binary Trials by Bob Carpenter](https://mc-stan.org/users/documentation/case-studies/pool-binary-trials.html), which looks at historical baseball batting records.

### Full prior
$$
q_i = p_{f_i} - p_{s_i} + m_i,
$$$$
p_\cdot \sim \mathrm{N}(0, \sigma_p^2),
$$
where
- $q_i$ is the log-odds of Player 1 winning match $i,$
- $p_k$ is the skill of player $k,$
- $m_i$ is the deck matchup effect for match $i,$
- $f_i$ and $s_i$ are the first and second player in match $i$,
- $\sigma_p$ is the prior player effect spread for all players.
$$
m_i = a_{g_i, t_i} + \sum_{k = 1}^3 b_{g_i, u_{i, k}} + \sum_{j = 1}^3 c_{h_{i, j}, t_i} + \sum_{j,k = 1}^3 d_{h_{i, j}, u_{i, k}},
$$$$
a_{g, t} \sim \textrm{N}(0, \sigma_\textrm{StSt}^2),
$$$$
b_{g, u} \sim \textrm{N}(0, \sigma_\textrm{StSp}^2),
$$$$
c_{h, t} \sim \textrm{N}(0, \sigma_\textrm{StSp}^2),
$$$$
d_{h, u} \sim \textrm{N}(0, \sigma_\textrm{SpSp}^2),
$$
where
- $a_{g, t}$ is the starter vs. starter effect for starters $g$ and $t$,
- $b_{g, u}$ is the starter vs. spec effect between starter $g$ and spec $u$,
- similarly for $c_{h, t}$ (spec vs. starter) and $d_{h, u}$ (spec vs. spec),
- $g_i$ is Player 1's starter for match $i$,
- $t_i$ is Player 2's starter for match $i$,
- $h_{i, j}$ is Player 1's $j^\textrm{th}$ spec for match $i$,
- $u_{i, j}$ is Player 2's $j^\textrm{th}$ spec for match $i$,
- $\sigma_\cdot$ are effect spread parameters shared between all effects of their type, with starter vs. spec and spec vs. starter effects counted as the same type (StSp).

The pooled spread parameters all use the same log-normal distribution:
$$
\log \sigma_p \sim \mathrm{N}(0, 1/2),
$$$$
\log \sigma_\textrm{StSt} \sim \mathrm{N}(0, 1/2),
$$$$
\log \sigma_\textrm{StSp} \sim \mathrm{N}(0, 1/2),
$$$$
\log \sigma_\textrm{SpSp} \sim \mathrm{N}(0, 1/2).
$$
This has mean and variance
$$\mu = \exp(1/8) \simeq 1.13, \, \sigma^2 = (\exp(1/4) - 1) \exp(1/4) \simeq 0.36.$$

At some point I plan to redo the spread distributions: the current one is *ad hoc*, and the player spread and the total deck spread should be expected to be equal *a priori*.

## Data

The data mostly consists of tournament matches played on the Sirlin Games forums. There are a lot of casual matches also played on the forums, but there are a few reasons I haven't used them:

- Time. There's no uniform way in which a match thread ends -- e.g. players sometimes play extra matches in the same thread, even in a tournament match thread -- so I recorded matches manually. Recording casual matches manually as well would require a much greater amount of time, more than I practically have, especially since I'd be adding a backlog of about 5 years. Automating the process would also have problems, since I occasionally have to mark a match as not counting, due to, e.g., a player disappearing halfway through a match, or having to stop for reasons outside of the game. That's a judgement call I don't want to entrust to an automated system.
- Players might play very differently in casual matches than they do in tournament matches. I could probably account for this in the model, but evaluating decks in high-level play is the goal, so I'm not sure how much difference including them would make.

## Inference

The model is run in R and Stan. Put simply, this means that we get Monte Carlo samples from the posterior distribution for the effects, rather than the simple maximum-likelihood estimate we'd get from classical regression.

While more expensive to compute, this gives us extra information that we can use in several ways:

- The spread of an effect's drawn values shows its remaining level of uncertainty. Some deck component matchups are used much more often than others, and their resulting lower level of uncertainty is shown accordingly.
- If there are summary statistics of interest we can calculate from the effects (e.g. Nash weights for the decks), we can do so for each sample. We then have samples for the summaries, so we get an idea of uncertainty for them too.

## Evaluation

The main tool for evaluation is the Brier score for match result predictions, except I take the mean over individual match scores rather than the sum, to keep it in the region $[0, 1].$ This is a proper scoring rule: if the true probability for Player to win is known to be $p,$ the Brier score is maximised by forecasting the win probability as $p,$ rather than under- or over-estimating, so it keeps the forecaster honest.

We also get a simple benchmark to compare against: the naive forecast of 50% Player 1 win probability for each match always has a score of 0.25. If a model has a score higher than 0.25 after a few matches, then something's wrong, since it's doing worse than a simple coin flip.

Usually I just look at the score over the matches used to fit the model, comparing it against earlier models. While a tournament is running, I also look at the score over the matches for that tournament, which is an out-of-sample forecast, and so is more informative for the model's performance.
