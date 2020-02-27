---
title: Codex
summary: Statistical modelling of a card game's forum tournament data.
tags:
- Codex
- Side project
date: "2016-04-27"
lastmod: "2016-04-27"

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

[Main model page](../../codex/codex_model.html)\
[Predictions for current tournament](../../codex/codex_current.html)
