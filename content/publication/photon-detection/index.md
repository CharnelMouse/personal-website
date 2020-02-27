---
abstract: The device physics of parallel-wire superconducting nanowire single photon detectors is based on a cascade process. Using nano-optical techniques and a parallel wire device with spatially separate pixels, we explicitly demonstrate the single- and multi-photon triggering regimes. We develop a model for describing efficiency of a detector operating in the arm-trigger regime. We investigate the timing response of the detector when illuminating a single pixel and two pixels. We see a change in the active area of the detector between the two regimes and find the two-pixel trigger regime to have a faster timing response than the one-pixel regime.
authors:
- "Robert M. Heath"
- "Michael G. Tanner"
- "Alessandro Casaburi"
- mark-webster
- "Lara San Emeterio Alvarez"
- "Weitao Jiang"
- "Zoe H. Barber"
- "Richard J. Warburton"
- "Robert H. Hadfield"
Zdate: "2014-02-10T00:00:00Z"
doi: "10.1063⁄1.4865199"
featured: false
links:
- name: ArXiv pre-print
  url: http://arxiv.org/abs/1402.2879
- name: Journal version
  url: http://scitation.aip.org/content/aip/journal/apl/104/6/10.1063/1.4865199
publication: "Applied Physics Letters, Volume 104, Issue 6"
publication_types:
- "2"
publishDate: "2014-02-10T00:00:00Z"
summary: "We explicitly demonstrate the single- and multi-photon triggering regimes."
tags:
title: Nano-optical observation of cascade switching in a parallel superconducting nanowire single photon detector
---

This looked at devices for detecting photons passing through it. If I remember correctly, the rough description of its workings is that we have several thin wires in parallel, and a if a photon hits a wire it increases the resistance enough for the wire to “trip”. If enough of the wires trip, the resistance of the whole circuit raises enough for everything to trip, and the device registers a detection. Every so often, all the wires will reset. There are also “dark counts”, where a wire trips due to natural fluctuations rather than being hit by a photon.
I helped work out the probability of a detection registration, given the probability of a photon tripping a wire, probability of a dark count, number of photons passing through the device, and so on. It’s a kind of urn-ball allocation model, where balls have a chance to miss all the urns and urns might receive fake balls. I don’t think the general probability formula was used in the end, just the very simple cases that occur in practice.
