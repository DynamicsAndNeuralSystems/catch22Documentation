---
description: A detailed usage guide for installing and using catch22 in Julia (Catch22.jl).
cover: ../.gitbook/assets/ian-battaglia-9drS5E_Rguc-unsplash.jpg
coverY: 0
---

# Julia

## Julia Usage Guide

Select a card below to access Julia-specific usage information.&#x20;

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Installation</strong></td><td></td><td><a href="../.gitbook/assets/install_light.png">install_light.png</a></td><td><a href="julia.md#installation">#installation</a></td></tr><tr><td></td><td align="center"><strong>Getting started</strong></td><td></td><td><a href="../.gitbook/assets/getting_started_light.png">getting_started_light.png</a></td><td><a href="julia.md#getting-started-basic-usage">#getting-started-basic-usage</a></td></tr><tr><td></td><td align="center"><strong>Advanced usage</strong></td><td></td><td><a href="../.gitbook/assets/adv_usage_light.png">adv_usage_light.png</a></td><td><a href="julia.md#advanced-usage">#advanced-usage</a></td></tr><tr><td></td><td align="center"><strong>Frequently Asked Questions</strong></td><td></td><td><a href="../.gitbook/assets/FAQ_light.png">FAQ_light.png</a></td><td><a href="julia.md#faq">#faq</a></td></tr><tr><td></td><td align="center"><strong>API Reference</strong></td><td></td><td><a href="../.gitbook/assets/API_light.png">API_light.png</a></td><td><a href="../information-about-catch22/api-reference/julia-api.md">julia-api.md</a></td></tr></tbody></table>

***

***

## Installation

***

Julia users can install the latest version of [_catch22_](https://docs.juliahub.com/General/Catch22/stable/) using the following:&#x20;

```julia
using Pkg
Pkg.add("Catch22")
```

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

The input time series can be provided as a `Vector{Float64}` or `Array{Float64, 2}`. If an array is provided, the time series must occupy its _columns_. For example, this package contains a few test time series from [_catch22_](https://www.github.com/chlubba/catch22):

```julia
using Catch22
𝐱 = Catch22.testdata[:testSinusoid] # a Vector{Float64}
X = randn(1000, 10) # an Array{Float64, 2} with 10 time series samples
```

### Computing _catch22_ features

Once your time series data is in the correct form, all _catch22_ features can then be evaluated with:

```julia
𝐟 = catch22(𝐱) # for a time series vector (1 instance)
F = catch22(X) # for a time series matrix (many instances)
```

### Expected output

If an **array** is provided (multiple univaraite time series), containing one time series in each of N columns:

* A `22 x N` `FeatureArrray` of feature values will be returned (a subtype of `AbstractDimArray`).&#x20;

If a **vector** is provided (a single time series) :&#x20;

* A vector of feature values will be returned as a `FeatureVector`, a one dimensional `FeatureArray`.

<details>

<summary>Example FeatureVector output</summary>

Let's have a look at the typical output for a FeatureVector:

```julia
FeatureVector output
22-element FeatureArray{Float64,1}  with dimensions:
  Dim{:feature} Categorical{Symbol} Symbol[DN_HistogramMode_5, DN_HistogramMode_10, …, SB_TransitionMatrix_3ac_sumdiagcov, PD_PeriodicityWang_th0_01] Unordered
 :DN_HistogramMode_5                             1.27293
 :DN_HistogramMode_10                           -1.12759
 :CO_Embed2_Dist_tau_d_expfit_meandiff           1.6463
 :CO_f1ecac                                      119.741
 :CO_FirstMin_ac                                 2.0
 :CO_HistogramAMI_even_2_5                       1.0795
 :CO_trev_1_num                                 -0.000175093
 :DN_OutlierInclude_p_001_mdrmd                 -0.137772
 :DN_OutlierInclude_n_001_mdrmd                  0.136873
 :FC_LocalSimple_mean1_tauresrat                 0.00625
 :FC_LocalSimple_mean3_stderr                    0.140511
 :IN_AutoMutualInfoStats_40_gaussian_fmmi        1.0
 :MD_hrv_classic_pnn40                           0.822
 :SB_BinaryStats_diff_longstretch0               6.0
 :SB_BinaryStats_mean_longstretch1               301.0
 :SB_MotifThree_quantile_hh                      1.38226
 :SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1    0.77551
 :SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1         0.693878
 :SP_Summaries_welch_rect_area_5_1               0.988172
 :SP_Summaries_welch_rect_centroid               0.00997088
 :SB_TransitionMatrix_3ac_sumdiagcov             0.010059
 :PD_PeriodicityWang_th0_01                      628.0

```

Note that all features are returned as **Float64**'s, even though some may be constrained to integers.

</details>

You can access the output for each SPI individually using the corresponding **symbol** i.e., **`:DN_HistogramMode_5`** for the DN\_HistogramMode\_5 feature.&#x20;

***

***

## Advanced Usage

Ready to explore some of the additional functionality of Catch22.jl?&#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features. To access _catch24_ you can use the `catch24` FeatureSet:

```julia
​# your time series data 
x = Catch22.testData[:testSinusoid]
​
features24 = catch24(x)
​
# alternatively, if you would like to use short names...
f24 = c24(x)
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

If you do not wish to compute the full _catch22_ feature set on your time series, you can alternatively compute each feature (including mean and std. deviation) individually.

{% tabs %}
{% tab title="Computing Individual Features" %}
Each feature can be evaluated for a time series **array** or **vector** with the _catch22_ `FeatureSet`. You can do this by indexing the _catch22_ `FeatureSet` using the feature names (as in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)) as a **symbol.** In Julia,  you can obtain a list of features (as **symbols**) by using `getnames(catch22)`.

As an example, let's compute only the feature [`DN_HistogramMode_5`](../information-about-catch22/feature-descriptions/distribution-shape.md) using its **symbol**. Here we will compute the feature for both a single univariate time series (as a **vector**) and an **array** with 10 univariate time series:

<pre class="language-julia"><code class="lang-julia"><strong>using Catch22
</strong>​
# your time series
x = catch22.testdata[:testSinusoid] # a Vector{Float64}
X = randn(1000, 10) # an Array{Float64, 2} with 10 time series
​
# evaluate feature using the corresponding symbol :feature_name
f = catch22[:DN_HistogramMode_5](x) # returns a scalar Float64
F = catch22[:DN_HistogramMode_5](X) # returns a 1x10 Matrix{Float64}
</code></pre>

For our time series vector, we obtain a **scalar** Float64 corresponding to the value of [`DN_HistogramMode_5`](../information-about-catch22/feature-descriptions/distribution-shape.md) for our single time series. Similarly, for an array of multiple time series, we obtain a **matrix** of Float64 values, corresponding to the value of the feature for each time series in the original array.

Alternatively, functions that calculate each feature individually are exported. For example,  [`DN_HistogramMode_5`](../information-about-catch22/feature-descriptions/distribution-shape.md)can be evaluated with:

```julia
f = DN_HistogramMode_5(𝐱)
```
{% endtab %}
{% endtabs %}



### 3. Short Names

For each feature, we also include a unique 'short name' for easier reference (as outlined in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).&#x20;

{% tabs %}
{% tab title="Short Names" %}
1. **Returning short names as outputs**

In Julia, short names can be returned in place of the corresponding long feature names by calling `c22` (instead of `catch22`) on your time series data. Since `c22` is also a FeatureSet type, it accepts the same inputs as `catch22:`

<pre class="language-julia"><code class="lang-julia"><strong>using Catch22
</strong>𝐱 = Catch22.testdata[:testSinusoid]
f = c22(x) # 22 element feature array
</code></pre>

Here is an example of (part of) the `FeatureArray` returned:

```python
:mode_5                 1.27293
:mode_10               -1.12759
:embedding_dist         1.6463
:acf_timescale          119.741
```

You can also access the individual results using their associated short name symbol e.g., `:mode5`. To obtain the full list of short names, you can use `getnames(c22)`.

2.**Computing individual features by their short name**

Using the `c22` `FeatureSet`, you can index individual time series features by their short name (as symbols). For example, to compute the feature `FC_LocalSimple_mean3_stderr` with short name `forecast_error`, you can do the following:

```julia
forecast_error_val = c22[:forecast_error](x)
```
{% endtab %}
{% endtabs %}



***

***

## FAQ

***

Click one of the expandable tabs below to explore commonly asked questions about _Catch22_ in Julia:

<details>

<summary>Does <em>catch22.jl</em> make use of Julia's native multithreading?</summary>

Yes, by default, _catch22.jl_ uses Julia's powerful native multithreading to parallelise over

To get an idea of how the performance of _catch22_ scales as a function of time series length (number of samples), both in terms of computation time (for the full feature set) and memory usage, see [here](https://github.com/brendanjohnharris/Catch22.jl?tab=readme-ov-file#single-threaded-performance) for plots which compare single-threaded and multithreaded performance in Julia.&#x20;

</details>

<details>

<summary>How is <em>catch22</em> implemented in Julia? </summary>

The [_catch22_](https://www.github.com/chlubba/catch22) repository provides these 22 features, originally coded in Matlab as part of the [_hctsa_](https://github.com/benfulcher/hctsa) toolbox, as C functions (in addition to Matlab and Python wrappers). The _catch22.jl_ package simply uses Julia's `ccall` to wrap these C functions from a shared library that is accessed through [catch22\_jll](https://github.com/JuliaBinaryWrappers/catch22_jll.jl) and compiled by the fantastic [BinaryBuilder](https://github.com/JuliaPackaging/BinaryBuilder.jl) package.

</details>

<details>

<summary>How can I submit a bug report for C<em>atch22.jl?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/brendanjohnharris/Catch22.jl/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

***
