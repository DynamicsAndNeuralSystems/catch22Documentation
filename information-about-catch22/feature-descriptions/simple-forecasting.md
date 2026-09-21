---
description: Feature of a 3-point mean forecast.
cover: ../../.gitbook/assets/forecasting_light.png
coverY: -111.8295605858855
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

# Simple forecasting

## `forecast_error`

### What it does

[`forecast_error`](#user-content-fn-1)[^1] returns the a measure of error from using the mean of the previous 3 values of the time series to predict the next value. The error statistic used here is the standard deviation of the residuals from the full set of simple 1-step forecasts. Because the input time series is _z_-scored (standard deviation of 1), the residuals should have a standard deviation less than 1 if this forecasting method is doing something (at least minimally) useful.

{% tabs %}
{% tab title="Example 1: Predator-Prey System" %}
Here's an example of a predator-prey system (black) for which the dynamics are varying on a timescale of 3 steps, such that mean-3 predictions (blue) are very poor forecasts. For this time series, the residuals have a higher variance than the original time series:

<figure><img src="../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Example 2: ODE" %}
This ODE system, by contrast is very highly sampled, such that the time series is a similar value in 3 steps, and the forecasts (blue) are very close to the original time series, yielding residuals with a very low standard deviation (0.03):

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

***



[^1]: **Naming info**: This feature matches the _hctsa_ feature called `FC_LocalSimple_mean3_stderr`. It computes the `stderr` output from running the code `FC_LocalSimple(x_z,'mean',3)` in _hctsa_.&#x20;
