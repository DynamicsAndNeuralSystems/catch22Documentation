---
description: Time series features which do not fall into any of the other categories.
cover: ../../.gitbook/assets/other_light.png
coverY: -70.54327563249001
layout:
  width: default
  cover:
    visible: true
    size: hero
    mask: none
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
---

# Other

## Embedding Distance (`embedding_dist)`

### What it does

[`embedding_dist`](#user-content-fn-1)[^1] first represents the time series in a two-dimensional time-delay embedding space, $$(x_t, x_{t+\tau})$$, using a time delay, $$\tau$$, equal to the first zero-crossing of the autocorrelation function (capped at one-tenth of the time-series length). It then computes the Euclidean distances between successive points in this 2D embedding space (i.e., points one time step apart, $$(x_t, x_{t+\tau})$$ and $$(x_{t+1}, x_{t+1+\tau})$$) and estimates the probability distribution of these distances using a histogram (with the number of bins set by Scott's rule). This feature outputs the mean absolute difference, across histogram bins, between this distribution and an exponential distribution with the same mean (evaluated at each bin centre). Note that no exponential is fitted: its rate is set directly from the mean distance. It will give low values to time series where the probability distribution of the distance between consecutive time-series values in the 2D embedding space is well approximated by an exponential distribution.

***

[^1]: **Naming info**: This feature matches the _hctsa_ feature called `CO_Embed2_Dist_tau_d_expfit_meandiff`.
