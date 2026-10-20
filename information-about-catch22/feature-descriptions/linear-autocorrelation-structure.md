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

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-1.-acf_timescale"><strong><code>acf_timescale</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-1.-acf_timescale">#id-1.-acf_timescale</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-2.-acf_first_min"><strong><code>acf_first_min</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-2.-acf_first_min">#id-2.-acf_first_min</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-3.-periodicity"><strong><code>periodicity</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-3.-periodicity">#id-3.-periodicity</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-4.-low_freq_power"><strong><code>low_freq_power</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-4.-low_freq_power">#id-4.-low_freq_power</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-5.-median_freq"><strong><code>median_freq</code></strong></a></td><td></td><td><a href="linear-autocorrelation-structure.md#id-5.-median_freq">#id-5.-median_freq</a></td></tr><tr><td></td><td align="center"><a href="linear-autocorrelation-structure.md#id-6.-ami_timescale"><strong><code>ami_timescale</code></strong></a></td><td></td><td></td></tr></tbody></table>

***

## 1. `acf_timescale`

### What it does

The [`acf_timescale`](#user-content-fn-1)[^1] feature in _catch22_ computes the first 1/_e_ crossing of the autocorrelation function of the time series. In _hctsa_, this is the `pointOfCrossing` output of [`CO_FirstCrossing(x_z,'ac',1/exp(1),'both')`](https://github.com/benfulcher/hctsa/blob/main/Operations/CO_FirstCrossing.m) (the _hctsa_ feature `firstCrossing_1e_acf_point`).

This feature measures the first time lag at which the autocorrelation function drops below 1/_e_ (= 0.3679). The _catch22_ implementation linearly interpolates between the two lags either side of the crossing, so it generally returns a non-integer value (e.g., about 0.63 for uncorrelated noise, rather than 1).
If the autocorrelation function never drops below 1/_e_, it returns _N_ − 1 for a time series of length _N_ (the largest lag at which the autocorrelation can be computed).
A constant time series, which cannot be _z_-scored, is treated in the same way (an autocorrelation that never decays): by default (with masking), it also returns _N_ − 1.

{% hint style="info" %}
**Changes in v1.0**: The long name changed from `CO_f1ecac` to `CO_FirstCrossing_acf_1e_pointOfCrossing`, following _hctsa_.
The values are unchanged, except in two edge cases: when the autocorrelation function never drops below 1/_e_, earlier versions returned _N_ (now _N_ − 1), and for a constant time series they returned 0 (now _N_ − 1).
{% endhint %}

{% hint style="info" %}
**Note**: The example outputs below were computed with an earlier version of _catch22_ (before v0.3) that returned the integer lag (equivalent to the `firstCrossing` output of `CO_FirstCrossing` in _hctsa_, the feature `firstCrossing_1e_acf_tau`), so current versions return slightly smaller, non-integer values.
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

Similar to the 1/_e_ crossing feature above, [`acf_first_min`](#user-content-fn-2)[^2] computes the **first minimum** of the autocorrelation function: the first lag at which the autocorrelation is lower than at the lags immediately before and after it. For an oscillatory time series, this is approximately half the period of the oscillation (e.g., 10 samples for a sine wave with a period of 20 samples). For time series whose autocorrelation function decays without oscillating, the first local minimum is set by estimation noise in the tail of the autocorrelation function, so values are harder to interpret. If there is no local minimum, the feature returns the length of the time series, _N_.
In _hctsa_, this feature is computed as `CO_FirstMin(x_z,'ac')`.

The values are unchanged in v1.0.
When the feature cannot be computed (for a constant time series, which cannot be _z_-scored, or a series shorter than three samples), it returns _N_ by default (with masking), as when no minimum is found.
Earlier versions returned 0 for a constant time series.

***

## 3. `periodicity`

### What it does

The feature [`periodicity`](#user-content-fn-3)[^3] returns the first peak in the autocorrelation function satisfying a set of conditions (after detrending the time series using a three-knot cubic regression spline).

Specifically:

1. Fit a cubic spline with breakpoints at the start, middle, and end of the time series (i.e., two cubic pieces) and subtract it, removing slow (including nonlinear) trends.
2. Compute the autocorrelation function of the detrended series up to a lag of one-third of the time-series length.
3. Find all local peaks and troughs of the autocorrelation function.
4. Return the lag of the first peak that (i) is preceded by a trough, (ii) is at least 0.01 higher than that trough, and (iii) has a non-negative autocorrelation. If no peak satisfies these conditions, the feature returns 0.

In _hctsa_, this is the `th0_01` output of `PD_PeriodicityWang(x_z)`.
The feature is unchanged in v1.0, and a constant time series (which cannot be _z_-scored) returns 0 by default (with masking), as in earlier versions.

{% hint style="info" %}
**Earlier versions**: Before v0.6.0, the C implementation returned the lag minus one (e.g., 19 rather than 20 for a sine wave with a period of 20 samples).
{% endhint %}

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

The feature [`low_freq_power`](#user-content-fn-4)[^4] computes the power in the lowest 20% of frequencies (i.e., the lowest fifth of the frequency range from zero to the Nyquist frequency) \[the output `area_5_1` from the _hctsa_ code `SP_Summaries(x_z,'fft',[],[],false)`].

### What it measures

It gives high values to time series with lots of power in low frequencies, and low values to time series that have most of their power in higher frequencies.

The area under the power spectrum is estimated in linear space.
The power spectral density is estimated as a periodogram, from the fast Fourier transform of the whole time series, zero-padded to the next power of two ($$N_{\mathrm{FFT}}$$).
This gives $$N_{\mathrm{FFT}}/2$$ frequency bins above zero frequency, up to and including the Nyquist frequency (the zero-frequency bin is dropped, since it is zero for a _z_-scored series), and the feature sums the power in the lowest fifth of these bins.
As in _hctsa_, the spectrum is floored at $$10^{-12}$$ of its maximum before any statistic is computed.

Note that the feature returns the area itself, without dividing by the total power. But because the input time series is _z_-scored (variance 1), the total area under the power spectrum is approximately 1, so the output can be read as the fraction of power in this low-frequency band (e.g., about 0.2 for uncorrelated noise).

A constant time series, which cannot be _z_-scored, is treated as the limit of an infinitely slow time series (with all its power at the lowest frequencies): by default (with masking), it returns 1.

{% hint style="info" %}
**Changes in v1.0**: The long name changed from `SP_Summaries_welch_rect_area_5_1` to `SP_Summaries_fft_area_5_1`, following _hctsa_, whose `SP_Summaries(x_z,'welch','rect')` now gives a different, segment-averaged Welch estimate.
Before v1.0, the spectrum was estimated with Welch's method using a single rectangular window spanning the whole time series (in effect, the same periodogram), but its frequency bins included the zero-frequency bin, and the low-frequency band was the lowest fifth of these $$N_{\mathrm{FFT}}/2 + 1$$ bins.
Since the zero-frequency bin is empty for a _z_-scored series, for most lengths (all except, e.g., 65–128 and 1025–2048 samples, for which the bands coincide) the band now contains one more non-zero bin at its upper edge.
Values usually change only slightly, but can change substantially when a lot of power lies at the edge of the band, and for very short time series (e.g., for _N_ = 12 the earlier band contained only the zero-frequency bin, giving 0).
Earlier versions returned `NaN` for a constant time series.
The example outputs below were computed with an earlier version of _catch22_.
{% endhint %}

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

## 5. `median_freq`

### What it does

Like the previous feature, [`median_freq`](#user-content-fn-5)[^5] is also extracted from the power spectrum (estimated in the same way, as a periodogram of the whole time series). But this time, it returns the frequency, $$f$$, at which the amount of power in frequencies lower and higher than $$f$$ is the same. Specifically, it returns the first frequency at which the cumulative power reaches half of the total power: the median frequency \[the output `wq_50` from the _hctsa_ code `SP_Summaries(x_z,'fft',[],[],false)`].

Frequencies are angular frequencies, in radians per sample, ranging from 0 to $$\pi$$ (e.g., about $$\pi/2 \approx 1.57$$ for uncorrelated noise, and $$2\pi/20 \approx 0.31$$ for a sine wave with a period of 20 samples).

A constant time series, which cannot be _z_-scored, is treated as the limit of an infinitely slow time series: by default (with masking), it returns 0.

{% hint style="info" %}
**Changes in v1.0**: This feature was called `centroid_freq` (long name `SP_Summaries_welch_rect_centroid`) before v1.0.
It always computed the _median_ frequency, so it was renamed `median_freq` (long name `SP_Summaries_fft_wq_50`), following _hctsa_, where `wq_50` is the 50% point of the frequency quantiles `wq_5`, …, `wq_99`.
A centroid would be the power-weighted mean frequency, $$\sum_f f\,S(f) / \sum_f S(f)$$, which differs from the median whenever the power spectrum is skewed: a long tail of high-frequency power pulls the centroid (the mean) up, but barely shifts the median. For example, for an AR(1) process with $$\phi = 0.9$$, the median frequency is about 0.11, whereas the spectral centroid is 0.27.

The spectrum also changed slightly (see `low_freq_power` above).
Before v1.0, the Nyquist-frequency bin had half the weight of the other bins, the zero-frequency bin was included, and the feature returned the first frequency at which the cumulative power _exceeded_ (rather than reached) half of the total power.
Values are identical for most time series, but can differ substantially for series with a lot of power near the Nyquist frequency.
Earlier versions returned `NaN` for a constant time series.
{% endhint %}

### What it measures

It gives high values to time series that have their power in high frequencies.

{% tabs %}
{% tab title="Example 1: Birdsong " %}
Here's an example of audio of an animal sound (median frequency shown with a red circle) that has its power in high frequencies. It gives a **high value** for this statistical measure:&#x20;

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### Feature output: `median_freq =`` `<mark style="color:red;">`2.823`</mark>
{% endtab %}

{% tab title="Example 2: ECG Recording " %}
Low values are assigned to slower-varying time series like this snippet of an [electrocardiogram recording](https://www.comp-engine.org/#!visualize/37fe246f-387a-11e8-8680-0242ac120002) from a patient with congestive heart failure:

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

### Feature output: `median_freq =`` `<mark style="color:red;">`0.147`</mark>
{% endtab %}
{% endtabs %}

***

## 6. **`ami_timescale`**

### What it does

[`ami_timescale`](#user-content-fn-6)[^6] outputs a measure of the timescale of autocorrelation in the time series, as the first local minimum of the automutual information function (computed over lags 1 to 40). This is a common way of selecting the timescale, for a time-delay embedding.

Automutual information is estimated here using a Gaussian assumption on the data (and is thus a **nonlinear transformation** of the **linear autocorrelation function**). As a result, this feature is sensitive only to linear autocorrelation, not to nonlinear dependence. Because the Gaussian automutual information, $$-\frac{1}{2}\log(1 - r^2)$$, increases with the magnitude of the autocorrelation, $$|r|$$, its first minimum occurs at the first minimum of $$|r|$$. For an oscillatory time series, this is close to the first zero-crossing of the autocorrelation function (rather than its first minimum, as in `acf_first_min`).

The feature maxes out at 40 (or half the time-series length, if shorter), meaning that if there has been no local minimum after 40 lags, this feature outputs the value 40.
A constant time series, which cannot be _z_-scored, is treated in the same way: by default (with masking), it returns 40 (or half the time-series length, rounded up, if shorter).

In _hctsa_, this feature is the `fmmi` output of `IN_AutoMutualInfoStats(x_z,40,'gaussian')`.
Its values are unchanged in v1.0, which only floors $$1 - r^2$$ at $$10^{-12}$$ (as in _hctsa_), affecting only autocorrelations within rounding error of ±1.

{% hint style="info" %}
**Earlier versions**: Before v0.6.0, the C implementation returned the lag of the first minimum minus one.
Before v1.0, a constant time series gave `NaN`.
{% endhint %}

High values reflect highly autocorrelated, long-memory processes (on the timescale of the sampling period), and low values reflect low-memory or noise processes.

[^1]: **Naming info:** Short name `acf_timescale`; long name `CO_FirstCrossing_acf_1e_pointOfCrossing` (before v1.0: `CO_f1ecac`, a name derived from an earlier version of _hctsa_). Reproduced in _hctsa_ by `CO_FirstCrossing(x_z,'ac',1/exp(1),'both').pointOfCrossing`, the _hctsa_ feature `firstCrossing_1e_acf_point` (the integer-lag version is `firstCrossing_1e_acf_tau`).

[^2]: **Naming info**: Short name `acf_first_min`; long name `CO_FirstMin_ac` (unchanged in v1.0). Reproduced in _hctsa_ by `CO_FirstMin(x_z,'ac')`, the _hctsa_ feature `firstMin_acf`.

[^3]: **Naming info**: Short name `periodicity`; long name `PD_PeriodicityWang_th0_01` (unchanged in v1.0). Reproduced in _hctsa_ by `PD_PeriodicityWang(x_z).th0_01`.

[^4]: **Naming info:** Short name `low_freq_power`; long name (matching the _hctsa_ name) `SP_Summaries_fft_area_5_1` (before v1.0: `SP_Summaries_welch_rect_area_5_1`). Reproduced in _hctsa_ by `SP_Summaries(x_z,'fft',[],[],false).area_5_1`.

[^5]: **Naming info**: Short name `median_freq` (before v1.0: `centroid_freq`); long name (matching the _hctsa_ name) `SP_Summaries_fft_wq_50` (before v1.0: `SP_Summaries_welch_rect_centroid`). Reproduced in _hctsa_ by `SP_Summaries(x_z,'fft',[],[],false).wq_50`.

[^6]: **Naming info**: Short name `ami_timescale`; long name `IN_AutoMutualInfoStats_40_gaussian_fmmi` (unchanged in v1.0), matching the _hctsa_ feature of the same name. Reproduced in _hctsa_ by `IN_AutoMutualInfoStats(x_z,40,'gaussian').fmmi`.
