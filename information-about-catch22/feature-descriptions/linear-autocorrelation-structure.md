---
description: >-
  Features quantifying linear autocorrelation structure (from the
  autocorrelation function or power spectrum).
cover: ../../.gitbook/assets/linear_autocorr_light.png
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

# Linear autocorrelation structure

_catch22_ contains **6** features which each capture some aspect of the linear autocorrelation structure of a time series. Select one of the cards below to discover more information:

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-1.-acf_timescale"><strong><code>acf_timescale</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-1.-acf_timescale">#id-1.-acf_timescale</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-2.-acf_first_min"><strong><code>acf_first_min</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-2.-acf_first_min">#id-2.-acf_first_min</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-3.-periodicity"><strong><code>periodicity</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-3.-periodicity">#id-3.-periodicity</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-4.-low_freq_power"><strong><code>low_freq_power</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-4.-low_freq_power">#id-4.-low_freq_power</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-5.-centroid_freq"><strong><code>centroid_freq</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-5.-centroid_freq">#id-5.-centroid_freq</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-6.-ami_timescale"><strong><code>ami_timescale</code></strong></a></td><td></td><td></td></tr></tbody></table>

***

## 1. `acf_timescale`

### What it does

The [`acf_timescale`](#user-content-fn-1)[^1] feature in _catch22_ computes the first 1/_e_ crossing of the autocorrelation function of the time series. In _hctsa_, this can be computed as `CO_FirstCrossing(x_z,'ac',1/exp(1),'discrete')`.

This feature measures the first time lag at which the autocorrelation function drops below 1/_e_ (= 0.3679).

### What it measures

`acf_timescale`captures the approximate scale of autocorrelation in a time series. This can be thought of as the number of steps into the future at which a value of the time series at the current point and that future point remain substantially (>1/_e_) correlated. For a continuous-time system, this statistic is high when the sampling rate is high relative to the timescale of the dynamics.

To give an intuition, below we plot some examples of the outputs of this feature for different scenarios:

{% tabs %}
{% tab title="Example 1: Uncorrelated Noise" %}
For uncorrelated noise, like the [Poisson-distributed series ](https://www.comp-engine.org/#!visualize/a02b59fd-3873-11e8-8680-0242ac120002)shown below, the autocorrelation function drops to \~0 immediately, and we obtain the minimum value of this statistic:&#x20;

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

### Feature output: `acf_timescale =`` `<mark style="color:red;">`1.000`</mark>
{% endtab %}

{% tab title="Example 2: Chirikov Map" %}
For processes with a greater level of autocorrelation, the autocorrelation function decays more slowly, and we can obtain a larger value of this feature. For example, consider this time series simulated from a [Chirikov map ](https://www.comp-engine.org/#!visualize/af22764b-3873-11e8-8680-0242ac120002)which gives moderate outputs for this feature:

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

### Feature output: `acf_timescale =`` `<mark style="color:red;">`6.000`</mark>
{% endtab %}

{% tab title="Example 3: Driven van der Pol oscillator " %}
We obtain even larger values for even more slowly varying time series, like this [driven van der Pol oscillator](https://www.comp-engine.org/#!visualize/bd37ac91-3870-11e8-8680-0242ac120002), measured at a very high sampling rate:

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

### Feature output: `acf_timescale =`` `<mark style="color:red;">`17.000`</mark>
{% endtab %}

{% tab title="Example 4: Financial time series" %}
Financial series (and many non-stationary stochastic processes) are highly autocorrelated, like [this series](https://www.comp-engine.org/#!visualize/85570a66-3872-11e8-8680-0242ac120002) for which we obtain very large values for `acf_timescale`:

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

### Feature output: `acf_timescale =`` `<mark style="color:red;">`176.000`</mark>
{% endtab %}
{% endtabs %}



***

## 2. `acf_first_min`

### What it does

Similar to the 1/_e_ crossing feature above, [`acf_first_min`](#user-content-fn-2)[^2] computes the **first minimum** of the autocorrelation function. It exhibits similar behaviour.

***

## 3. `periodicity`

### What it does

The feature [`periodicity`](#user-content-fn-3)[^3] returns the first peak in the autocorrelation function satisfying a set of conditions (after detrending the time series using a three-knot cubic regression spline).

It is based on a method by Wang et al. (2007) (described in their paper: _"_[_Structure-based Statistical Features and Multivariate Time Series Clustering_](https://ieeexplore.ieee.org/document/4470259/)_" )._

To give some intuition about the typical behaviour of the `periodicity` time series feature, consider these examples below:

{% tabs %}
{% tab title="Example 1: Duffing-van der Pol Oscillator " %}
Broadly, it gives **high values** to slowly-varying time series like this slow (on the timescale of $$\Delta t$$) [Duffing-van der Pol oscillator](https://www.comp-engine.org/#!visualize/4842839f-3872-11e8-8680-0242ac120002):

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### Feature output:`periodicity =`` `<mark style="color:red;">`62.000`</mark>
{% endtab %}

{% tab title="Example 2: Gingerbread Map" %}
For this fast varying (on the timescale of $$\Delta t$$) map, the [Gingerbread map](https://www.comp-engine.org/#!visualize/525d7bd9-3874-11e8-8680-0242ac120002), the feature assigns **low values**

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Feature output:`periodicity =`` `<mark style="color:red;">`4.000`</mark>
{% endtab %}
{% endtabs %}

***

## 4. `low_freq_power`

### What it does

The feature [`low_freq_power`](#user-content-fn-4)[^4] computes the relative power in the lowest 20% of frequencies (relative to the sampling rate of the data) \[the output `area_5_1` from the _hctsa_ code `SP_Summaries(x_z,'welch','rect',[],false)`].

### What it measures

It gives high values to time series with lots of power in low frequencies, and low values to time series that have most of their power in higher frequencies.

The area under the power spectrum is estimated in linear space, where the power spectral density is estimated using Welch's method (with a rectangular window).

{% tabs %}
{% tab title="Example 1: Stochastic Process" %}
Here's an example of a [slow-varying stochastic process](https://www.comp-engine.org/#!visualize/60e91ef6-3875-11e8-8680-0242ac120002) with a **very high value** for this feature, reflecting 98.7% of power is this low-frequency band (relevant portion of the power spectrum shaded red below):

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

### Feature output: `low_freq_power =`` `<mark style="color:red;">`0.987`</mark>
{% endtab %}

{% tab title="Example 2: Lozi Map" %}
This [Lozi map](https://www.comp-engine.org/#!visualize/90c3f445-3872-11e8-8680-0242ac120002) has a **low value** for this statistic (3% of power is in the red low-frequency band):

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

### Feature output: `low_freq_power =`` `<mark style="color:red;">`0.028`</mark>
{% endtab %}
{% endtabs %}

***

***

## 5. `centroid_freq`

### What it does

Like the previous feature, [`centroid_freq`](#user-content-fn-5)[^5] is also extracted from the power spectrum (estimated using a Welch's method with a rectangular window). But this time, it returns the frequency, $$f$$, at which the amount of power in frequencies low and higher than $$f$$ is the same: the "_**centroid**_".

### What it measures

It gives high values to time series that have their power in high frequencies.

{% tabs %}
{% tab title="Example 1: Birdsong " %}
Here's an example of audio of an animal sound (centroid point shown with a red circle) that has its power in high frequencies. It gives a **high value** for this statistical measure:&#x20;

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### Feature output: `centroid_freq =`` `<mark style="color:red;">`2.823`</mark>
{% endtab %}

{% tab title="Example 2: ECG Recording " %}
Low values are assigned to slower-varying time series like this snippet of an [electrocardiogram recording](https://www.comp-engine.org/#!visualize/37fe246f-387a-11e8-8680-0242ac120002) from a patient with congestive heart failure:

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

### Feature output: `centroid_freq =`` `<mark style="color:red;">`0.147`</mark>
{% endtab %}
{% endtabs %}

***

## 6. **`ami_timescale`**

## What it does

[`ami_timescale`](#user-content-fn-6)[^6] outputs a measure of the timescale of (potentially nonlinear) autocorrelation in the time series, as the minimum of the automutual information function. This is a common way of selecting the timescale, for a time-delay embedding.

Automutual information is estimated here using a Gaussian assumption on the data (and is thus a **nonlinear transformation** of the **linear autocorrelation function**). The _hctsa_ version maxes out at 40, meaning that if there has been no local minimum after 40 lags, this feature outputs the value 40.

High values reflect highly autocorrelated, long-memory processes (on the timescale of the sampling period), and low values reflect low-memory or noise processes.

[^1]: **Naming info:** The name `CO_f1ecac` derives from an earlier version of _hctsa_ (the current version of _hctsa_ names this feature as `first1e_acf_tau`). The _catch22_ short name is `acf_timescale`.

[^2]: **Naming info**: short name:`acf_first_min` in _catch22_ (long name: `CO_FirstMin_ac`) and matches the feature called `firstMin_acf` in _hctsa_

[^3]: **Naming info**: long name is `PD_PeriodicityWang_th0.01`

[^4]: **Naming info:** Long name (matching the _hctsa_ name) is `SP_Summaries_welch_rect_area_5_1`

[^5]: **Naming info**: _hctsa_ name is `SP_Summaries_welch_rect_centroid`.

[^6]: **Naming info**: This feature matches the _hctsa_ feature called `IN_AutoMutualInfoStats_40_gaussian_fmmi`
