---
description: A high-level summary table of all catch22 features.
cover: ../../.gitbook/assets/alina-grubnyak-ZiQkhI7417A-unsplash.jpg
coverY: 0
---

# Feature Overview Table

**Note**: All _catch22_ features are statistical properties of the _**z**_**-scored** time series - they aim to focus on the properties of the time-ordering of the data and are insensitive to the raw values in the time series.&#x20;

In the following table, we give the original feature name (from the Lubba et al. (2019) [paper](../../welcome-to-catch22/citing-catch22.md)), and a shorter name more suitable for use in feature descriptions. Features are also (loosely) categorised into broader conceptual groupings.

Each feature is listed according to the order in which it, along with its associated value, is returned by _catch22_ e.g., the first feature returned (i.e., #1) is always `DN_HistogramMode_5.`

<table data-full-width="false"><thead><tr><th width="69">#</th><th width="188">Feature name</th><th width="219">Short name</th><th width="150">Category</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td><code>DN_HistogramMode_5</code></td><td><code>mode_5</code></td><td><a href="distribution-shape.md"><mark style="color:orange;">Distribution shape</mark></a></td><td>5-bin histogram mode</td></tr><tr><td>2</td><td><code>DN_HistogramMode_10</code></td><td><code>mode_10</code></td><td><a href="distribution-shape.md"><mark style="color:orange;">Distribution shape</mark></a></td><td>10-bin histogram mode</td></tr><tr><td>3</td><td><code>DN_OutlierInclude_p_001_mdrmd</code></td><td><code>outlier_timing_pos</code></td><td><a href="extreme-event-timing.md"><mark style="color:orange;">Extreme event timing</mark></a></td><td>Positive outlier timing</td></tr><tr><td>4</td><td><code>DN_OutlierInclude_n_001_mdrmd</code></td><td><code>outlier_timing_neg</code></td><td><a href="extreme-event-timing.md"><mark style="color:orange;">Extreme event timing</mark></a></td><td>Negative outlier timing</td></tr><tr><td>5</td><td><code>CO_f1ecac</code></td><td><code>acf_timescale</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>First <span class="math">1/e</span> crossing of the ACF</td></tr><tr><td>6</td><td><code>CO_FirstMin_ac</code></td><td><code>acf_first_min</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>First minimum of the ACF</td></tr><tr><td>7</td><td><code>SP_Summaries_welch_rect_area_5_1</code></td><td><code>low_freq_power</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>Power in lowest 20% frequencies </td></tr><tr><td>8</td><td><code>SP_Summaries_welch_rect_centroid</code></td><td><code>centroid_freq</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>Frequency that splits the power spectrum in half (a median, despite the name)</td></tr><tr><td>9</td><td><code>FC_LocalSimple_mean3_stderr</code></td><td><code>forecast_error</code></td><td><a href="simple-forecasting.md"><mark style="color:orange;">Simple forecasting</mark></a></td><td>Error of 3-point rolling mean forecast</td></tr><tr><td>10</td><td><code>FC_LocalSimple_mean1_tauresrat</code></td><td><code>whiten_timescale</code></td><td><a href="incremental-differences.md"><mark style="color:orange;">Incremental differences</mark></a></td><td>Change in autocorrelation timescale after incremental differencing</td></tr><tr><td>11</td><td><code>MD_hrv_classic_pnn40</code></td><td><code>high_fluctuation</code></td><td><a href="incremental-differences.md"><mark style="color:orange;">Incremental differences</mark></a></td><td>Proportion of high incremental changes in the series</td></tr><tr><td>12</td><td><code>SB_BinaryStats_mean_longstretch1</code></td><td><code>stretch_high</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Longest stretch of above-mean values</td></tr><tr><td>13</td><td><code>SB_BinaryStats_diff_longstretch0</code></td><td><code>stretch_decreasing</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Longest stretch of decreasing values</td></tr><tr><td>14</td><td><code>SB_MotifThree_quantile_hh</code></td><td><code>entropy_pairs</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Entropy of successive pairs in symbolized series</td></tr><tr><td>15</td><td><code>CO_HistogramAMI_even_2_5</code></td><td><code>ami2</code></td><td><a href="nonlinear-autocorrelation.md"><mark style="color:orange;">Nonlinear autocorrelation</mark></a></td><td>Histogram-based automutual information (lag 2, 5 bins)</td></tr><tr><td>16</td><td><code>CO_trev_1_num</code></td><td><code>trev</code></td><td><a href="nonlinear-autocorrelation.md"><mark style="color:orange;">Nonlinear autocorrelation</mark></a></td><td>Time reversibility</td></tr><tr><td>17</td><td><code>IN_AutoMutualInfoStats_40_gaussian_fmmi</code></td><td><code>ami_timescale</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation structure</mark></a></td><td>First minimum of the AMI function</td></tr><tr><td>18</td><td><code>SB_TransitionMatrix_3ac_sumdiagcov</code></td><td><code>transition_variance</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Transition matrix column variance</td></tr><tr><td>19</td><td><code>PD_PeriodicityWang_th0_01</code></td><td><code>periodicity</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation structure</mark></a></td><td>Wang's periodicity metric</td></tr><tr><td>20</td><td><code>CO_Embed2_Dist_tau_d_expfit_meandiff</code></td><td><code>embedding_dist</code></td><td><a href="other.md"><mark style="color:orange;">Other</mark></a></td><td>Goodness of exponential fit to embedding distance distribution</td></tr><tr><td>21</td><td><code>SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1</code></td><td><code>rs_range</code></td><td><a href="self-affine-scaling.md"><mark style="color:orange;">Self-affine scaling</mark></a></td><td>Rescaled range fluctuation analysis (low-scale scaling)</td></tr><tr><td>22</td><td><code>SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1</code></td><td><code>dfa</code></td><td><a href="self-affine-scaling.md"><mark style="color:orange;">Self-affine scaling</mark></a></td><td>Detrended fluctuation analysis (low-scale scaling)</td></tr></tbody></table>

***

## _Catch24_ (_Catch22_ + Mean + Std)

In some cases, in which scale and spread of the raw time-series values may be relevant to class differences, the two simple distributional moment features (using the `catch24` flag in the software implementations) can be added. This will result in 24 features being calculated: the _catch22_ features in addition to the **mean** and **standard deviation**.&#x20;

<table data-full-width="false"><thead><tr><th width="136">#</th><th>Feature name</th><th>Short name</th><th>Description</th></tr></thead><tbody><tr><td>23</td><td><mark style="color:green;"><code>DN_Mean</code></mark></td><td><code>mean</code></td><td>Mean</td></tr><tr><td>24</td><td><mark style="color:green;"><code>DN_Spread_Std</code></mark></td><td><code>std</code></td><td>Standard deviation</td></tr></tbody></table>

***

## Feature Dependencies

Although the selection framework used to generate the _catch22_ feature set included a step to reduce redundancy, it was not designed to generate an independent set of features.

### Pairwise Spearman Correlations

Below is an example of the generic non-independence of features. We have plotted the Spearman correlation coefficient between all pairs of features, quantifying the similarity of their outputs across a diverse range of [1000 empirical time series](https://doi.org/10.6084/m9.figshare.5436136.v9):

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

* We find a large cluster of features sensitive to the autocorrelation of a time series.
* We also find a small cluster of two highly correlated features, `DN_HistogramMode_5` and `DN_HistogramMode_10`, which measure the mode of the z-scored time-series distribution using different numbers of bins.&#x20;

This dependency structure should be taken in mind when interpreting the result of catch22 analyses: _Does your dataset exhibit any of these generic dependencies, or some unique dependencies?_&#x20;

### PC Loadings

Below is a similar plot, but with colour overlayed according to the weights onto the first three principal components:

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

Broadly,

* The first two principal components capture different aspects of the autocorrelation structure.
* The third principal component captures different aspects of the distribution symmetry.&#x20;

***
