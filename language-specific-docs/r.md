---
description: A detailed usage guide for installing and using catch22 in R (Rcatch22).
cover: ../.gitbook/assets/chris-liverani-dBI_My696Rk-unsplash.jpg
coverY: -230.33315128345166
---

# R

## R Usage Guide

Select a card below to access R-specific usage information.&#x20;

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Installation</strong></td><td></td><td><a href="../.gitbook/assets/install_light.png">install_light.png</a></td><td><a href="r.md#installation">#installation</a></td></tr><tr><td></td><td align="center"><strong>Getting started</strong></td><td></td><td><a href="../.gitbook/assets/getting_started_light.png">getting_started_light.png</a></td><td><a href="r.md#getting-started-basic-usage">#getting-started-basic-usage</a></td></tr><tr><td></td><td align="center"><strong>Advanced usage</strong></td><td></td><td><a href="../.gitbook/assets/adv_usage_light.png">adv_usage_light.png</a></td><td><a href="r.md#advanced-usage">#advanced-usage</a></td></tr><tr><td></td><td align="center"><strong>Frequently Asked Questions</strong></td><td></td><td><a href="../.gitbook/assets/FAQ_light.png">FAQ_light.png</a></td><td><a href="r.md#faq">#faq</a></td></tr><tr><td></td><td align="center"><strong>API Reference</strong></td><td></td><td><a href="../.gitbook/assets/API_light.png">API_light.png</a></td><td><a href="../information-about-catch22/api-reference/r-api.md">r-api.md</a></td></tr></tbody></table>

***

***

## Installation

***

You can install the stable version of _Rcatch22_ from [CRAN](https://cran.r-project.org/web/packages/Rcatch22/readme/README.html) using the following:

```r
install.packages("Rcatch22")
```

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

_Rcatch22_ expects a **single univariate time series** as a **numeric** vector with length equal to the number of points in the time series. For example, consider a time series with length 100:

```r
library(Rcatch22)
data <- stats::rnorm(100) # your time series data
```

### Computing _catch22_ features

To compute _catch22_ features, call the `catch22_all` function on your time series data as follows:

```r
outs22 <- catch22_all(data)
```

### Expected output

A data frame is returned with two columns: `names` and `features` as shown below

```
outs22
##                                          names       values
## 1                           DN_HistogramMode_5  0.432653377
## 2                          DN_HistogramMode_10  0.174395817
## 3                                    CO_f1ecac  1.865299453
...                                ...                 ...
## 21            SP_Summaries_welch_rect_centroid  1.178097245
## 22                 FC_LocalSimple_mean3_stderr  1.201213638
```

To access the full list of feature names, you can also run `feature_list.`

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

***

## Advanced Usage

Ready to explore some of the additional functionality of _Rcatch22?_&#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features. An option to include the mean and standard deviation as features in addition to _catch22_ is available through setting the _catch24_ argument to `TRUE`:

```r
library(Rcatch22)

x <- 1 + 0.5 * 1:1000 + arima.sim(list(ma = 0.5), n = 1000)
features <- catch22_all(x, catch24 = TRUE)
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

If you do not wish to compute the full _catch22_ feature set on your time series, you can alternatively compute each feature (including mean and std. deviation) individually.

{% tabs %}
{% tab title="Computing Individual Features" %}
In _pycatch22_ each feature function can be accessed individually and takes **arrays** as **tuples** or To compute individual features in _Rcatch22_, you can access the feature functions using their unique names (as shown in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)). Note that feature functions are given by the feature long name e.g.,`IN_AutoMutualInfoStats_40_gaussian_fmmi`.

For example, let's compute the feature `IN_AutoMutualInfoStats_40_gaussian_fmmi` on a single univariate time series:

```r
library(Rcatch22)
x <- 1 + 0.5 * 1:1000 + arima.sim(list(ma = 0.5), n = 1000)
f <- IN_AutoMutualInfoStats_40_gaussian_fmmi(x)
```
{% endtab %}
{% endtabs %}

***

## FAQ

***

Click one of the expandable tabs below to explore commonly asked questions about _Rcatch22_:

<details>

<summary>How does computational time scale as a function of time series length in Rcatch22?</summary>

With features coded in C, _Rcatch22_ is highly computationally efficient, scaling nearly linearly with time series size. Computation time in seconds for a range of time series lengths is presented [here](https://github.com/hendersontrent/Rcatch22?tab=readme-ov-file#computational-performance).&#x20;

</details>

<details>

<summary>How can I submit a bug report for <em>Rcatch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/hendersontrent/Rcatch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

***
