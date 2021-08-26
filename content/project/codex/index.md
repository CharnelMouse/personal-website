---
title: Codex
summary: Statistical modelling of a card game's forum tournament data.
date: 2020-05-02
lastmod: 2021-05-22
tags:
- Codex
- Side project

image:
  caption: ""
  focal_point: "Smart"
  preview_only: false
---

Codex is a card game by Sirlin Games. It is a non-collectible card game, that is a mix of Magic: The Gathering's combat mechanics, Dominion-like games' deck building, and a strategic layer themed around real-time strategy games, especially Warcraft III.

Official pages:

- [Codex page at Sirlin Games](http://sirlingames.com/codex), including links to articles on the game's design at [David Sirlin's design blog](http://www.sirlin.net).
- [Official Sirlin Games forum](http://forums.sirlingames.com/)

The game is played with decks consisting of three different "specs". Specs come in six different colours of three specs each, and the game is expected to be played with decks constructed from the three specs of a single colour. On the forums, it's popular to allow decks made of three specs of any colour.

Since the game was balanced around monocolour decks, it's expected that multicolour decks widely vary in strength. I've been slowly collecting data on tournament match results for tournaments run on the official forums, and creating statistical models using [R](https://www.r-project.org/) and [Stan](https://mc-stan.org/) to evaluate player skill levels and strengths of different decks, as seen in [this thread on the forums](http://forums.sirlingames.com/t/codex-data-thread/5326).

My personal results summaries are a bit more detailed than Discourse's post format allows: in particular, I have searchable tables of matchup predictions, since Rmarkdown has an implementation of [JavaScript's DataTables](https://www.datatables.net/). As such, I'm making this site the home for the model, so that I can make my personal summaries available to others.

If I get time, I'll make a proper dashboard to view this information, to include options to investigate performance of different specs against a particular opposing deck.

[Model description](/codex_desc): Description of the problem, the model and data used, and evaluation methods.

[Main model page](../../codex/codex_model.html): Post-hoc model performance on recorded matches; estimates for player skill levels, opposed-pair deck component effects, and monocolour matchups with uncertainty plots; estimate of importance of player skill compared to deck choices.

[Predictions for current tournament](../../codex/codex_current.html): Pre-tournament predictions for completed matches in the current forum tournament, including measures of matchup fairness, and Nash equilibria as a rough indicator of likely winners. Predictions are made using only data available the day before the start of the tournament. The current forum tournament is the Summer Seasonal Swiss (CAMS21).

[Optimal picking](../../codex/codex_optimal.html): Nash equilibria for deck choices, and good counters against known opposing decks, as considered by the model. These are expensive to run, and aren't updated as frequently.
