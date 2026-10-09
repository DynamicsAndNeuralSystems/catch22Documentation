---
description: >-
  Features that capture the properties of long-range correlations in time
  series.
cover: ../../.gitbook/assets/self_affine_light.png
coverY: 13.496671105193087
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

# Self-affine scaling

_catch22_ contains two features based on fluctuation analysis, which aim to capture potential long-range correlations in time series, derived from the `SC_FluctAnal` function in [_hctsa_](https://github.com/benfulcher/hctsa). Select one of the cards below to discover more information:

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong><code>rs_range</code></strong></td><td></td><td><a href="self-affine-scaling.md#id-1.-rs_range">#id-1.-rs_range</a></td></tr><tr><td></td><td align="center"><strong><code>dfa</code></strong></td><td></td><td><a href="self-affine-scaling.md#id-2.-dfa">#id-2.-dfa</a></td></tr></tbody></table>

***

Here is a reference for understanding scaling methods: [_Power spectrum and detrended fluctuation analysis: Application to daily temperatures_](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.62.150) Talkner and Weber, _Phys. Rev. E_ **62:** 150 (2000).

### Summary of the Method

The main steps of the method are as follows:

1. Compute a cumulative sum of the time series
2. Compute the level of fluctuation (e.g., root-mean-square deviations from local low-order trends) across windows corresponding to a given timescale. Different methods exist for detrending time-series windows at a given timescale, including (relevant to these two features):
   1. Rescaled range analysis computes the range of the detrended points in each window (Caccia et al., Physica A, 1997). In _catch22_, the trend removed is a least-squares line fitted to each window, and the range is not divided by the window's standard deviation (as it would be in a conventional rescaled range statistic). The fluctuation at that timescale is the root-mean-square of the ranges across windows.
   2. DFA fits a _k_-order polynomial to each window (linear, _k_ = 1, in _catch22_) and computes the root-mean-square of the residuals from this fit across all windows.
3. Looks for linear scaling in the log(timescale)–log(fluctuation) plot.

In _catch22_, fluctuations are computed at up to 50 logarithmically spaced timescales between 5 samples and half the time-series length (rounded to the nearest integer, with duplicates removed). If fewer than 12 distinct timescales remain (i.e., for short time series), both features return 0.

Some time series exhibit a 'crossover' in their scaling: there are different scaling rules at different ranges of timescales. The two _catch22_ features fit straight lines to two distinct scaling regimes in the log–log plot (each regime containing at least 6 timescales), choosing the split point that minimises the combined fitting error. They return the proportion of the evaluated timescales that fall in the first (shorter-timescale) regime, up to and including the split point, rather than the timescale itself.

Note that these features make quite strong assumptions about the data and can be unstable for time series that do not exhibit scaling, or that exhibit a single scaling regime across all timescales.

***

## 1. `rs_range`

### What it does

[`rs_range`](#user-content-fn-1)[^1] uses rescaled range analysis described above to estimate points in logarithmic timescale–fluctuation space.

{% tabs %}
{% tab title="Example 1: AR(2) Process" %}
Time series (the best approximation to two distinct scaling regimes: the first scaling regime is indicated with red markers in the plots below) that change at a short timescale, like this [AR(2) process](https://www.comp-engine.org/#!visualize/8c0fd998-3873-11e8-8680-0242ac120002), receive **low values** of this feature:

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `rs_range = 0.200`**&#x20;
{% endtab %}

{% tab title="Example 2" %}
This feature gives **high values** for time series where the scaling regime changes at longer timescales:

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `rs_range =`**<mark style="color:red;">**`0.694`**</mark>
{% endtab %}

{% tab title="Example 3: Complex Butterfly Map" %}
This feature gives **high values** for time series where the scaling regime changes at longer timescales. Consider this [complex butterfly map:](https://www.comp-engine.org/#!visualize/76112226-3871-11e8-8680-0242ac120002)

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `rs_range =`**<mark style="color:red;">**`0.760`**</mark>
{% endtab %}
{% endtabs %}

***

***

## 2. `dfa`

### What it does

[`dfa`](#user-content-fn-2)[^2] measures the same property as above, but from a timescale–fluctuation curve estimated using de-trended fluctuation analysis (DFA), using linear de-trending in each window, after down-sampling the time series by a factor of 2.

{% tabs %}
{% tab title="Example 1: Duffing-van der Pol Oscillator" %}
It displays qualitatively similar behaviour to`rs_range`, e.g., with **low value** for this [Duffing-van der Pol oscillator](https://www.comp-engine.org/#!visualize/6ec42454-3872-11e8-8680-0242ac120002) (which has a change in the scaling relationship at short timescales):

<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `dfa =`**<mark style="color:red;">**`0.204`**</mark>
{% endtab %}

{% tab title="Example 2: Opening Share Price" %}
This feature assigns **higher values** for time series that exhibit strong scaling through to longer timescales, like this log-return series of an [opening share price](https://www.comp-engine.org/#!visualize/b1edede6-3871-11e8-8680-0242ac120002):

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `dfa =`**<mark style="color:red;">**`0.800`**</mark>
{% endtab %}
{% endtabs %}

***

***





[^1]: **Naming info**: This feature matches the feature in _hctsa_ named `SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1`

[^2]: **Naming info**: This feature matches the _hctsa_ feature called `SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1`
