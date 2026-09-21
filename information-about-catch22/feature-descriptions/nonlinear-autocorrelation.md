---
description: These features capture nonlinear autocorrelation properties of a time series.
cover: ../../.gitbook/assets/non_linear_autocorr_icon.png
coverY: 37.949400798934754
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

# Nonlinear autocorrelation

_catch22_ contains **2** features which each capture some aspect of the nonlinear autocorrelation properties of a time series. Select one of the cards below to discover more information:

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong><code>trev</code></strong></td><td align="center"></td><td><a href="nonlinear-autocorrelation.md#id-1.-trev">#id-1.-trev</a></td></tr><tr><td></td><td align="center"><strong><code>ami2</code></strong></td><td align="center"></td><td><a href="nonlinear-autocorrelation.md#id-2.-ami2">#id-2.-ami2</a></td></tr></tbody></table>

***

## 1. `trev`

### What it does

[`trev` ](#user-content-fn-1)[^1]computes computes the average across the time series of the cube of successive time-series differences. It will be close to zero for time series for which the distribution of successive decreases in the time series matches the distribution of successive increases, but will be positive if increases tend to be larger in magnitude and negative if decreases tend to be larger in magnitude.

It is computed as:

$$
\langle (x_{i+1} - x_i)^3\rangle_t\,,
$$

for time-series values, _x_, averaged across all possible time point&#x73;_, t_ (i.e., from index 1 to N-1, for a time series of length _N_).

It is based on a statistic used in nonlinear time-series analysis, cf. [Surrogate time series](https://doi.org/10.1016/S0167-2789\(00\)00043-9), Schreiber and Schmitz, _Physica D_, **142:** 346 (2000).

The original _hctsa_ implementation is the code `CO_trev(x_z,1)` , for a _z_-scored time series, `x_z`.

To give an intuition, below we plot some examples of the outputs of this feature for different scenarios:

{% tabs %}
{% tab title="Example 1: Chua Map" %}
This [Chua map time series](https://www.comp-engine.org/#!visualize/d34839c1-3872-11e8-8680-0242ac120002) has increments that are approximately symmetric (blue histogram), as are the cubic increments (orange histogram), and so this statistic has a value (the mean of the orange distribution) of approximately zero:

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Click to enlarge image. </p></figcaption></figure>

### **Feature output: `trev =`**<mark style="color:red;">`-0.021`</mark>
{% endtab %}

{% tab title="Example 2: Simple Piecewise linear chaotic flow" %}
This[ simple flow ](https://www.comp-engine.org/#!visualize/03552b98-3871-11e8-8680-0242ac120002)has a multi-modal distribution of successive increments (blue), with an asymmetry towards larger increases compared to smaller decreases. This is accentuated after taking a cube (orange), yielding a positive value of this statistic:

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p>Click to enlarge image.</p></figcaption></figure>

### **Feature output: `trev =`**<mark style="color:red;">`1.083`</mark>
{% endtab %}

{% tab title="Example 3: Lozi Map" %}
This[ Lozi map](https://www.comp-engine.org/#!visualize/8d547ec4-3872-11e8-8680-0242ac120002) is an example of the opposite behaviour: time-series decreases can be sudden and large in magnitude, whereas increases are relatively gradual. This leads to an asymmetry towards large negative values, leading to a negative value for this statistic:

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Click to enlarge image.</p></figcaption></figure>

### **Feature output: `trev =`**<mark style="color:red;">`-3.497`</mark>
{% endtab %}
{% endtabs %}

***

## 2. `ami2`

### What it does

[`ami2`](#user-content-fn-2)[^2] is a nonlinear version of the autocorrelation function: using a nonlinear correlation metric (mutual information) instead of a conventional linear correlation metric, evaluated using a histogram with 5 bins and at a time delay _τ_ = 2 (from the _hctsa_ code `CO_HistogramAMI(x_z,2,'even',5)`).

Explore the tabs below to see examples of the typical outputs of this feature for various time series:

{% tabs %}
{% tab title="Example 1: Chaotic Web Map" %}
This feature gives **high** values to time series like this [Chaotic Web map](https://www.comp-engine.org/#!visualize/23409069-3876-11e8-8680-0242ac120002):&#x20;

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

which has clear dependence structure of the time-series value at the current point, $$x_t$$, and the value two time points ahead, $$x_{t+2}$$_,_ yielding a high value for this feature of 1.25:

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `ami2 =`**<mark style="color:red;">`1.250`</mark>
{% endtab %}

{% tab title="Example 2: Tent Map" %}
This [Tent map](https://www.comp-engine.org/#!visualize/13df59a3-3873-11e8-8680-0242ac120002) also has a clear (nonlinear) dependence of present time-series value with that two-steps into the future (and a moderate value of this feature):

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

as seen in its embedding:

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

### **Feature output: `ami2 =`**<mark style="color:red;">`0.473`</mark>
{% endtab %}

{% tab title="Example 3: Heart Rhythm" %}
This feature gives a moderate value for this heart rhythm ECG [time series](https://www.comp-engine.org/#!visualize/ab05d12c-3878-11e8-8680-0242ac120002):

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

and its embedding:

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

### **Feature output:** `ami2 =`<mark style="color:red;">`0.25`</mark>
{% endtab %}

{% tab title="Example 4: Earthquake" %}
We obtain a low value for this [earthquake time series](https://www.comp-engine.org/#!visualize/2df74240-3874-11e8-8680-0242ac120002):

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

and embedding:

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

### **Feature output:** `ami2 =`<mark style="color:red;">`0.019`</mark>
{% endtab %}
{% endtabs %}

***

[^1]: **Naming info**: This matches the _hctsa_ feature `CO_trev_1_num.`

[^2]: **Naming info**: Original name matched an earlier _hctsa_ name `CO_HistogramAMI_even_2_5` but this feature has since been renamed in current _hctsa_ library to more clearly describe the binning, as `CO_HistogramAMI_even_5bin_ami2`
