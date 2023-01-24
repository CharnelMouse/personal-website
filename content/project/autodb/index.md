---
title: autodb
summary: Automatic database normalisation for R data frames.
date: 2023-01-23
lastmod: 2023-01-23
tags:
- R
- Side project

image:
  caption: ""
  focal_point: "Smart"
  preview_only: false
---

[Github link](https://github.com/CharnelMouse/autodb)\
[Main vignette](../../autodb_vignette)

*autodb* is an R package I wrote to help with a common problem I have at work:

- Third-party data for one-off statistical analysis;
- Not being stored in our own proper database;
- Limited or absent data documentation;
- Data can fit in local memory, e.g. in a single R session;
- R will be used for the analysis, so I might as well keep to the same language.

It took initial inspiration from [Alteryx's autonormalise library for Python](https://github.com/alteryx/autonormalize); I've gone back to the original papers to touch up the algorithm implementations, made some fixes, and added on a few extra features, like allowing for LTK form, and displaying keys in the database and database schema plots. It doesn't include every feature of the Python library, due to having a tighter focus: the Python library is intended to be used within a larger Machine Learning pipeline.

The package takes a single flat table of data -- a *data.frame* in R -- and attempts to convert it into a database in third normal form. This is done using only functional dependences inferable from the given data, so the resulting layout of the data reflects what is present.

This aids with data checks that can be expressed in terms of the structure of the data, rather than the particular values involved. This removes a lot of busywork, allowing the user to more quickly find discrepancies between the data and their understanding of it.

For example, the database shown above is from a meta-analysis study, where the data is expected to have a simple hierarchical structure: publications contain studies, which contain effect sizes, each with a designated ID column. We would therefore expect each ID to identify the row in its own layer's table, i.e. to be a key, as indicated by the black cells. However, the publication ID isn't a key for its own layer: the publication title is the only key, suggesting either a misunderstanding of the data, or problems with the data itself. In this case, two papers were mistakenly assigned the same publication ID, but the title is unique across all papers, so can be used instead.
