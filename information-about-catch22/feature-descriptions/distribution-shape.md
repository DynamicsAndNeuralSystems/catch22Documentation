---
description: >-
  The DN_HistogramMode features measure properties of the shape of the
  distribution of time-series values.
cover: ../../.gitbook/assets/dist_shape_light.png
coverY: -67.48335552596538
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

# Distribution shape

_catch22_ contains two features involving the `DN_HistogramMode` function in _hctsa:_

* `mode_5` (the _hctsa_ feature `DN_HistogramMode_5`)
* `mode_10` (the _hctsa_ feature `DN_HistogramMode_10`)

{% hint style="info" %}
**Note**: The C implementation of these features (in _catch22)_ does not map perfectly onto the _hctsa_ implementation, due to slight differences in how the histogram bins are constructed. But the trends are similar.&#x20;
{% endhint %}

## What it does

These functions involve computing the mode of the z-scored time-series through the following steps:

1. z-score the input time series.
2. Compute a histogram using a given number of equal-width bins spanning the range of the data (from its minimum to its maximum), e.g., 5 bins for `mode_5` and 10 bins for `mode_10`.
3. Return the centre of the bin with the most counts (if several bins tie for the most counts, the average of their centres is returned).

Because the bin edges are set by the minimum and maximum values, the output takes only a small number of possible values and depends on the most extreme values in the time series.

## What these features measure

Being distributional properties, these features are completely insensitive to the time-ordering of values in the time series. Instead, they capture how the most probable time-series values are positioned relative to the mean.

{% tabs %}
{% tab title="Example 1: Gaussian-distributed noise " %}
Time series with a **symmetric** distribution, with a central peak, will have a mode near the center, and value close to zero. Here is an example of [Gaussian-distributed noise](https://www.comp-engine.org/#!visualize/5dc3827a-3873-11e8-8680-0242ac120002):&#x20;

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:red;">Feature output:</mark> <mark style="color:red;"></mark><mark style="color:red;">`-0.36`</mark>
{% endtab %}

{% tab title="Example 2: Chirikov Map" %}
Time series with a **symmetric** distribution but with density far from the origin, like this [Chirikov map ](https://www.comp-engine.org/#!visualize/41411951-3871-11e8-8680-0242ac120002)obtain high (positive or negative) values:

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:red;">Feature output:</mark> <mark style="color:red;"></mark><mark style="color:red;">`1.26`</mark>
{% endtab %}

{% tab title="Example 3: Beta-distributed noise" %}
Time series with **positively** skewed distributions, like this example of [beta-distributed noise ](https://www.comp-engine.org/#!visualize/20320f55-3872-11e8-8680-0242ac120002)obtain negative values as shown below:&#x20;

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:red;">Feature output:</mark> <mark style="color:red;"></mark><mark style="color:red;">`-0.805`</mark>

_Similarly, a **negatively** skewed distribution will yield positive values._&#x20;
{% endtab %}
{% endtabs %}

