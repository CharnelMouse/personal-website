---
title: "Papers"
author: "Mark Webster"
date: 2019-12-01
---

## 2016

CONVERGENCE PROPERTIES OF APPROXIMATE BAYESIAN COMPUTATION\
PhD Thesis.\
This looked at the rate of convergence for several versions of the ABC algorithm. Some of the results were also published in Barber et. al. 2015. The other main section looks at a new variants, which has a better rate of convergence for posterior mean estimation.\
[White Rose eTheses Online version](http://etheses.whiterose.ac.uk/16197/)

## 2015

THE RATE OF CONVERGENCE FOR APPROXIMATE BAYESIAN COMPUTATION\
with Stuart Barber and Jochen Voss.\
Electronic Journal of Statistics, Volume 9, Number 1, Pages 80--105. DOI:10.1214/15-EJS988\
This looked at the rate of convergence for the most basic version of the ABC algorithm, where either the number of proposals or the number of accepted proposals is fixed, and the estimate is the mean of the desired function of the accepted proposals, without adjustment. There are also some theorems giving conditions for the estimate to converge to the correct answer.\
[ArXiv version](http://arxiv.org/abs/1311.2038) [Journal version](http://projecteuclid.org/euclid.ejs/1423229751)

## 2014

NANO-OPTICAL OBSERVATION OF CASCADE SWITCHING IN A PARALLEL SUPERCONDUCTING NANOWIRE SINGLE PHOTON DETECTOR\
with Robert M. Heath, Michael G. Tanner, Alessandro Casaburi, Lara San Emeterio Alvarez, Weitao Jiang, Zoe H. Barber, Richard J. Warburton, and Robert H. Hadfield.\
Applied Physics Letters, Volume 104, Issue 6, ID 063503. DOI:10.1063/1.4865199\
This looked at devices for detecting photons passing through it. If I remember correctly, the rough description of its workings is that we have several thin wires in parallel, and a if a photon hits a wire it increases the resistance enough for the wire to "trip". If enough of the wires trip, the resistance of the whole circuit raises enough for everything to trip, and the device registers a detection. Every so often, all the wires will reset. There are also "dark counts", where a wire trips due to natural fluctuations rather than being hit by a photon.\
I helped work out the probability of a detection registration, given the probability of a photon tripping a wire, probability of a dark count, number of photons passing through the device, and so on. It's a kind of urn-ball allocation model, where balls have a chance to miss all the urns and urns might receive fake balls. I don't think the general probability formula was used in the end, just the very simple cases that occur in practice.\
[ArXiv version](http://arxiv.org/abs/1402.2879) [Journal version](http://scitation.aip.org/content/aip/journal/apl/104/6/10.1063/1.4865199)
