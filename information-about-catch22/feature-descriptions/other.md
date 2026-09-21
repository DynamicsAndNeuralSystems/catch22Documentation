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

[`embedding_dist`](#user-content-fn-1)[^1] first represents the time series in a two-dimensional time-delay embedding space (using a time delay equal to the first zero-crossing of the autocorrelation function). It then computes successive distances between points in this 2D embedding space and analyses the probability distribution of these distances. This feature outputs the mean absolute error of an exponential fit to this distribution. It will give low values to time series where the probability distribution of the distance between consecutive time-series values in the 2D embedding space is well approximated by an exponential distribution.

***

[^1]: **Naming info**: This feature matches the _hctsa_ feature called `CO_Embed2_Dist_tau_d_expﬁt_meandiff`.
