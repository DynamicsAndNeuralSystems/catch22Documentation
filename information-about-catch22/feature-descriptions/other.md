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

[`embedding_dist`](#user-content-fn-1)[^1] first represents the time series in a two-dimensional time-delay embedding space, $$(x_t, x_{t+\tau})$$, using a time delay, $$\tau$$, equal to the first zero-crossing of the autocorrelation function (capped at one-tenth of the time-series length). It then computes the Euclidean distances between successive points in this 2D embedding space (i.e., points one time step apart, $$(x_t, x_{t+\tau})$$ and $$(x_{t+1}, x_{t+1+\tau})$$) and estimates the probability density of these distances using a histogram of equal-width bins spanning their range, normalised as a probability density (count/(_n_ × bin width), for _n_ distances).
The number of bins is the larger of those given by Sturges' rule and the Freedman–Diaconis rule (_hctsa_'s `'auto'` rule).
This feature outputs the mean absolute difference, across histogram bins, between this density and the density of an exponential distribution with the same mean (the maximum-likelihood exponential fit), evaluated at each bin centre. It will give low values to time series where the probability distribution of the distance between consecutive time-series values in the 2D embedding space is well approximated by an exponential distribution.

In _hctsa_, this feature is the `d_expfit_meandiff` output of `CO_Embed2_Dist(x_z,'tau')`.

When all the distances are equal (to within rounding error), their distribution is as far from exponential as it can be, and the feature cannot be estimated.
In this case, and for a constant time series (which cannot be _z_-scored), the feature returns a large value by default (with masking): $$1/(\sqrt{2} \times 0.001) + (N - 2)/\sqrt{2(N-1)/N} \approx 707 + 0.71N$$ for a time series of length _N_.
This is a practical ceiling, far above the values of real time series, rather than a strict upper bound: distances that are nearly, but not exactly, equal can give larger values.

{% hint style="info" %}
**Changes in v1.0**: The name is unchanged, but the feature now matches _hctsa_, which differs from earlier versions in two ways:

* Earlier versions normalised the histogram as probabilities (count/_n_), rather than as a probability density, before comparing it with the exponential density, so values depended on the bin width.
* Earlier versions set the number of bins by Scott's rule ($$\lceil n^{1/3} \times \mathrm{range}/(3.5\,\sigma) \rceil$$, for distances with standard deviation $$\sigma$$), and returned 0 when the standard deviation of the distances was below 0.001 (and `NaN` for a constant time series).

Values therefore change for all time series, often substantially.
{% endhint %}

***

[^1]: **Naming info**: Short name `embedding_dist`; long name `CO_Embed2_Dist_tau_d_expfit_meandiff` (unchanged in v1.0), matching the _hctsa_ feature of the same name. Reproduced in _hctsa_ by `CO_Embed2_Dist(x_z,'tau').d_expfit_meandiff`.
