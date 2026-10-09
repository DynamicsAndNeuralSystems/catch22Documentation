---
description: Features based on a discrete symbolisation of real-valued time-series data.
cover: ../../.gitbook/assets/symbolic_light (1).png
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

# Symbolic

_catch22_ contains **4** features which are each based on a discrete symbolisation of real-valued time-series data. Select one of the cards below to discover more information:

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong><code>entropy_pairs</code></strong></td><td></td><td><a href="symbolic.md#id-1.-entropy_pairs">#id-1.-entropy_pairs</a></td></tr><tr><td></td><td align="center"><strong><code>transition_variance</code></strong></td><td></td><td><a href="symbolic.md#id-2.-transition_variance">#id-2.-transition_variance</a></td></tr><tr><td></td><td align="center"><strong><code>stretch_high</code></strong></td><td></td><td><a href="symbolic.md#id-3.-stretch_high">#id-3.-stretch_high</a></td></tr><tr><td></td><td align="center"><strong><code>stretch_decreasing</code></strong></td><td></td><td><a href="symbolic.md#id-4.-stretch_decreasing">#id-4.-stretch_decreasing</a></td></tr></tbody></table>

***

## 1. `entropy_pairs`

### What it does

[`entropy_pairs`](#user-content-fn-1)[^1] is computed as the following:

1. Converts each value in the time series into one of three symbols ('A', 'B', or 'C') using an equi-probable binning in which the lowest 3rd of values are assigned 'A', the middle 3rd 'B', and the highest 3rd of values are given 'C'.
2. It then analyses the probabilities of all two-letter sequences ('AA', 'AB', 'BB', …) and outputs the (Shannon) entropy of this set of nine probabilities, using the natural logarithm. The maximum value, for which all nine pairs are equally probable, is $$\log 9 \approx 2.20$$.

This feature is based on the _hctsa_ code `SB_MotifThree(x_z,'quantile')`, returning the `hh` output.

Time series that have predictable two-letter sequences (i.e., some sequences are far more probable than others) will have low values, and time series that are less predictable (i.e., all two-letter sequences are approximately equi-probable) will have high values.

{% tabs %}
{% tab title="Example 1: Ricker Map" %}
This [Ricker map](https://www.comp-engine.org/#!visualize/75defda7-3873-11e8-8680-0242ac120002) is highly predictable, with letters (A=red, B=blue, C=green) shown below, converting to "ABCABCABCABC…", such that "AB", "BC", and "CA" become highly probable sequences. It has a very **low value** for this feature:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `entropy_pairs =`**<mark style="color:red;">`1.099`</mark>
{% endtab %}

{% tab title="Example 2" %}
Here is another very predictable series, which has a very long string of C, then B, then A, such that "AA", "BB", and "CC" are highly probable sequences yielding **low entropy:**

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `entropy_pairs =`**<mark style="color:red;">`1.144`</mark>
{% endtab %}

{% tab title="Example 3" %}
Time series that have predictable patterns on longer than 2-length windows, or that require more granularity than a 3-letter symbolisation to resolve, have high values, like this map:

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `entropy_pairs =`**<mark style="color:red;">`2.176`</mark>
{% endtab %}
{% endtabs %}

***

## 2. **`transition_variance`**

### What it does

[`transition_variance` first](#user-content-fn-2)[^2] computes $$\tau$$ as the first zero-crossing of the autocorrelation function and downsamples the time series by taking every $$\tau$$th value. It then symbolises the downsampled series into a 3-letter equiprobable alphabet (by quantile) and counts the transitions between successive symbols as a `3x3` matrix. Each count is divided by the total number of transitions, so each entry is the joint probability of a (source, target) pair, and all nine entries sum to 1. With equiprobable symbols, each row sums to approximately 1/3, so each entry is approximately one-third of the corresponding conditional transition probability. This feature then returns the sum of column-wise variances of this matrix (i.e., the trace of the covariance matrix between its columns).

It is a measure of the specificity of source states given a target state. A minimum value would be given to a noisy series where the transition probabilities are approximately uniform, and a maximum value for a highly ordered series with very specific state transition rules. The subtlety with this feature is that it computes the transitions on a timescale of $$\tau$$ such that it measures such order on the timescale at which the linear autocorrelation has faded.

***

## Consecutive Stretches

_catch22_ contains two features that capture the maximum length of time over which similar consecutive local patterns are observed:

* `stretch_high` measures the longest successive period of above-average values.
  * The feature called `SB_BinaryStats_mean_longstretch1` in _hctsa_ (the `longstretch1` output from running `SB_BinaryStats(x_z,'mean')` in _hctsa_);
* `stretch_decreasing` measures the longest successive period of successive decreases.
  * The feature called `SB_BinaryStats_diff_longstretch0` in _hctsa_ (the `longstretch0` output from running `SB_BinaryStats(x_z,'diff')` in _hctsa_).

## 3. `stretch_high`

### What it does

`stretch_high` computes the longest sequence of successive values in the time series that are greater than the mean. Algorithmically, this is achieved in two steps:

1. Transform the time series into a binary sequence: time-series values that are greater than the mean are set to `1` and time-series values that are less than or equal to the mean are set to `0`.
2. Return the longest sequence of successive values that are `1`.

{% tabs %}
{% tab title="Example 1: MA Process" %}
Low values are given to time series that tend not to linger too much on either side of the mean, like this moving average process, which has a maximum duration of 8 samples (red) above the mean (zero: dashed line):

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

### **Feature output:** `stretch_high`**`=`**<mark style="color:red;">`8`</mark>
{% endtab %}

{% tab title="Example 2: Stochastic SIne Map" %}
High values are given to time series that have at least one long duration of time spent above the mean, like this stochastic sine map with 107 successive time points above the mean):

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### **Feature output:** `stretch_high`**`=`**<mark style="color:red;">`107`</mark>
{% endtab %}
{% endtabs %}

***

## 4. `stretch_decreasing`

### What it does

`stretch_decreasing` is similar to the above, but it calculates the longest sequence of successive steps in the time series that _decrease_. Algorithmically, this is achieved in two steps:

1. Transform the time series into a binary sequence: each time-series value is converted to a `1` if it is higher than (or equal to) the previous time point, and `0` if it is lower than the previous time point (starting from the second point in the time series, and thus yielding a sequence of length `N-1`, where `N` is the length of the original time series).
2. Return the longest sequence of successive values that are `0`.

{% tabs %}
{% tab title="Example 1: Complex Butterfly Map" %}
Here is a time series from the complex butterfly map, with the longest period of 30 successive decreases highlighted in red:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

### **Feature output:** `stretch_decreasing =`` `<mark style="color:red;">`30`</mark>
{% endtab %}
{% endtabs %}

***

[^1]: **Naming info**: This features matches the _hctsa_ feature called `SB_MotifThree_quantile_hh`

[^2]: **Naming info**: This feature matches the _hctsa_ feature called `SB_TransitionMatrix_3ac_sumdiagcov`.
