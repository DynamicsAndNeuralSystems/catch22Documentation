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

This feature measures the first time lag at which the autocorrelation function drops below 1/_e_ (= 0.3679). The _catch22_ implementation linearly interpolates between the two lags either side of the crossing, so it generally returns a non-integer value (e.g., about 0.63 for uncorrelated noise, rather than 1). If the autocorrelation function never drops below 1/_e_, it returns the length of the time series.

{% hint style="info" %}
**Note**: The example outputs below were computed with an earlier version of _catch22_ that returned the integer lag (as in the `'discrete'` _hctsa_ call above), so current versions return slightly smaller, non-integer values.
{% endhint %}

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

Similar to the 1/_e_ crossing feature above, [`acf_first_min`](#user-content-fn-2)[^2] computes the **first minimum** of the autocorrelation function: the first lag at which the autocorrelation is lower than at the lags immediately before and after it. For an oscillatory time series, this is approximately half the period of the oscillation (e.g., 10 samples for a sine wave with a period of 20 samples). For time series whose autocorrelation function decays without oscillating, the first local minimum is set by estimation noise in the tail of the autocorrelation function, so values are harder to interpret. If there is no local minimum, the feature returns the length of the time series.

***

## 3. `periodicity`

### What it does

The feature [`periodicity`](#user-content-fn-3)[^3] returns the first peak in the autocorrelation function satisfying a set of conditions (after detrending the time series using a three-knot cubic regression spline).

Specifically:

1. Fit a cubic spline with breakpoints at the start, middle, and end of the time series (i.e., two cubic pieces) and subtract it, removing slow (including nonlinear) trends.
2. Compute the autocorrelation function of the detrended series up to a lag of one-third of the time-series length.
3. Find all local peaks and troughs of the autocorrelation function.
4. Return the lag of the first peak that (i) is preceded by a trough, (ii) is at least 0.01 higher than that trough, and (iii) has a non-negative autocorrelation. If no peak satisfies these conditions, the feature returns 0.

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

The feature [`low_freq_power`](#user-content-fn-4)[^4] computes the power in the lowest 20% of frequencies (i.e., the lowest fifth of the frequency range from zero to the Nyquist frequency) \[the output `area_5_1` from the _hctsa_ code `SP_Summaries(x_z,'welch','rect',[],false)`].

### What it measures

It gives high values to time series with lots of power in low frequencies, and low values to time series that have most of their power in higher frequencies.

The area under the power spectrum is estimated in linear space. The power spectral density is estimated using Welch's method with a rectangular window that spans the whole time series (i.e., a single segment, so in effect this is a periodogram).

Note that the feature returns the area itself, without dividing by the total power. But because the input time series is _z_-scored (variance 1), the total area under the power spectrum is approximately 1, so the output can be read as the fraction of power in this low-frequency band (e.g., about 0.2 for uncorrelated noise).

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

Like the previous feature, [`centroid_freq`](#user-content-fn-5)[^5] is also extracted from the power spectrum (estimated in the same way, using a single rectangular window). But this time, it returns the frequency, $$f$$, at which the amount of power in frequencies lower and higher than $$f$$ is the same. Specifically, it returns the first frequency at which the cumulative power exceeds half of the total power.

Frequencies are angular frequencies, in radians per sample, ranging from 0 to $$\pi$$ (e.g., about $$\pi/2 \approx 1.57$$ for uncorrelated noise, and $$2\pi/20 \approx 0.31$$ for a sine wave with a period of 20 samples).

{% hint style="warning" %}
**The name "centroid" is a misnomer**: this feature computes the _median_ frequency of the power spectrum, not its centroid. A centroid is the power-weighted mean frequency, $$\sum_f f\,S(f) / \sum_f S(f)$$. The two differ whenever the power spectrum is skewed: a long tail of high-frequency power pulls the centroid (the mean) up, but barely shifts the median. For example, for an AR(1) process with $$\phi = 0.9$$, `centroid_freq` returns 0.107 (the median), whereas the true spectral centroid is 0.27. The name is retained for consistency with _hctsa_ and earlier versions of _catch22_.
{% endhint %}

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

### What it does

[`ami_timescale`](#user-content-fn-6)[^6] outputs a measure of the timescale of autocorrelation in the time series, as the first local minimum of the automutual information function (computed over lags 1 to 40). This is a common way of selecting the timescale, for a time-delay embedding.

Automutual information is estimated here using a Gaussian assumption on the data (and is thus a **nonlinear transformation** of the **linear autocorrelation function**). As a result, this feature is sensitive only to linear autocorrelation, not to nonlinear dependence. Because the Gaussian automutual information, $$-\frac{1}{2}\log(1 - r^2)$$, increases with the magnitude of the autocorrelation, $$|r|$$, its first minimum occurs at the first minimum of $$|r|$$. For an oscillatory time series, this is close to the first zero-crossing of the autocorrelation function (rather than its first minimum, as in `acf_first_min`).

The feature maxes out at 40 (or half the time-series length, if shorter), meaning that if there has been no local minimum after 40 lags, this feature outputs the value 40.

High values reflect highly autocorrelated, long-memory processes (on the timescale of the sampling period), and low values reflect low-memory or noise processes.

[^1]: **Naming info:** The name `CO_f1ecac` derives from an earlier version of _hctsa_ (the current version of _hctsa_ names this feature as `first1e_acf_tau`). The _catch22_ short name is `acf_timescale`.

[^2]: **Naming info**: short name:`acf_first_min` in _catch22_ (long name: `CO_FirstMin_ac`) and matches the feature called `firstMin_acf` in _hctsa_

[^3]: **Naming info**: long name is `PD_PeriodicityWang_th0_01`

[^4]: **Naming info:** Long name (matching the _hctsa_ name) is `SP_Summaries_welch_rect_area_5_1`

[^5]: **Naming info**: _hctsa_ name is `SP_Summaries_welch_rect_centroid`.

[^6]: **Naming info**: This feature matches the _hctsa_ feature called `IN_AutoMutualInfoStats_40_gaussian_fmmi`
