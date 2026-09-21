---
description: >-
  The DN_OutlierInclude features measure the timing of extreme events relative
  to the start and end of the time series.
cover: ../../.gitbook/assets/extreme_event_light.png
coverY: 0
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

# Extreme event timing

_catch22_ contains two features based on the `DN_OutlierInclude` function in hcts&#x61;_:_

* `outlier_timing_pos` (the _hctsa_ feature `DN_OutlierInclude_p_001_mdrmd`) i.e, the `mdrmd` output from running `DN_OutlierInclude(x_z,'pos',0.01)` in _hctsa._
* `outlier_timing_neg` (the _hctsa_ feature `DN_OutlierInclude_n_001_mdrmd)` i.e., the `mdrmd` output from running`DN_OutlierInclude(x_z,'neg',0.01)` in _hctsa_).

## What these features do

These features involve the following steps:

1. _z_-score the input time series.&#x20;
2. Initialise an equally spaced set of increments, from zero to the maximum values of the time series, in the case of `outlier_timing_pos` (or from 0 to the minimum value of the time series in the case of `outlier_timing_neg`). In this way, a set of increasingly \`extreme' deviations from the mean (either deviations above-the-mean or below-the-mean) are analysed across the loop in Step (3).&#x20;
3. At each threshold set in Step (2):
   1. Determine the time points in which the time series is \`over-threshold'.
   2. Compute the median index of all such over-threshold time points, as `rmd` .
   3. For interpretation, and to appropriately compare time series of different lengths, we then linearly re-scale `rmd` such that a median right in the middle of the time series, at index `N/2`, maps to 0, a value at the end of the time series, at index `N`, maps to 1, and a value at the start of the time series, index `1`, maps to a -1.
4. The final statistic returns the median of all values of `rmd` values across all values of the threshold, as the output statistic.

***

## What it measures

These statistics measure whether over-threshold events (either positive or negative deviations from the mean) tend to be positioned relative near the start of the time series (output values near -1), approximately equally likely to be anywhere through the time series (output values near 0), or more likely to be near the end of the time series (output values near 1). These features thus capture something related to the stationarity of over-threshold events.

To give an intuition, below we plot some examples of how `rmd` at a fixed threshold (80% the maximum positive deviation) for the case of `outlier_timing_pos` ( note that the full statistic takes the median of `rmd` across a range of thresholds, as described above).

Consider these examples:

{% tabs %}
{% tab title="Example 1" %}
Time series, that have extreme events (red dots, relative to the threshold, shown as a dashed red line) distributed similarly across time, will yield values close to **zero** for this statistic (vertical blue line). For example these:

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Example 2" %}
Time series like these, for which large deviations from the mean tend to occur nearer to the end of the time series, will have values closer to **1:**

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Example 3" %}
Time series like these, for which large deviations from the mean tend to occur nearer the start of the time series, will have values nearer to **-1:**

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

***

***





