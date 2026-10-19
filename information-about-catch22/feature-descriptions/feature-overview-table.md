---
description: A high-level summary table of all catch22 features.
cover: ../../.gitbook/assets/alina-grubnyak-ZiQkhI7417A-unsplash.jpg
coverY: 0
---

# Feature Overview Table

**Note**: All _catch22_ features are statistical properties of the _**z**_**-scored** time series - they aim to focus on the properties of the time-ordering of the data and are insensitive to the raw values in the time series.&#x20;

In the following table, we give each feature's name (as of _catch22_ v1.0, which follows _hctsa_), and a shorter name more suitable for use in feature descriptions. Features are also (loosely) categorised into broader conceptual groupings.
Several long names changed in v1.0: the names used in earlier versions (and in the Lubba et al. (2019) [paper](../../welcome-to-catch22/citing-catch22.md)) are listed [below](#feature-names-before-v1.0-and-hctsa-code), together with the _hctsa_ code that reproduces each feature, and the [changes in v1.0](#changes-in-v1.0) are summarised at the end of this page.

Each feature is listed according to the order in which it, along with its associated value, is returned by _catch22_ e.g., the first feature returned (i.e., #1) is always `DN_HistogramMode_5.`

<table data-full-width="false"><thead><tr><th width="69">#</th><th width="188">Feature name</th><th width="219">Short name</th><th width="150">Category</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td><code>DN_HistogramMode_5</code></td><td><code>mode_5</code></td><td><a href="distribution-shape.md"><mark style="color:orange;">Distribution shape</mark></a></td><td>5-bin histogram mode</td></tr><tr><td>2</td><td><code>DN_HistogramMode_10</code></td><td><code>mode_10</code></td><td><a href="distribution-shape.md"><mark style="color:orange;">Distribution shape</mark></a></td><td>10-bin histogram mode</td></tr><tr><td>3</td><td><code>DN_OutlierInclude_p_001_mdrmd</code></td><td><code>outlier_timing_pos</code></td><td><a href="extreme-event-timing.md"><mark style="color:orange;">Extreme event timing</mark></a></td><td>Positive outlier timing</td></tr><tr><td>4</td><td><code>DN_OutlierInclude_n_001_mdrmd</code></td><td><code>outlier_timing_neg</code></td><td><a href="extreme-event-timing.md"><mark style="color:orange;">Extreme event timing</mark></a></td><td>Negative outlier timing</td></tr><tr><td>5</td><td><code>CO_FirstCrossing_acf_1e_pointOfCrossing</code></td><td><code>acf_timescale</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>First <span class="math">1/e</span> crossing of the ACF</td></tr><tr><td>6</td><td><code>CO_FirstMin_ac</code></td><td><code>acf_first_min</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>First minimum of the ACF</td></tr><tr><td>7</td><td><code>SP_Summaries_fft_area_5_1</code></td><td><code>low_freq_power</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>Power in lowest 20% frequencies </td></tr><tr><td>8</td><td><code>SP_Summaries_fft_wq_50</code></td><td><code>median_freq</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation</mark></a></td><td>Median frequency: the frequency that splits the power spectrum in half</td></tr><tr><td>9</td><td><code>FC_LocalSimple_mean3_stde</code></td><td><code>forecast_error</code></td><td><a href="simple-forecasting.md"><mark style="color:orange;">Simple forecasting</mark></a></td><td>Error of 3-point rolling mean forecast</td></tr><tr><td>10</td><td><code>FC_LocalSimple_mean1_taurat</code></td><td><code>whiten_timescale</code></td><td><a href="incremental-differences.md"><mark style="color:orange;">Incremental differences</mark></a></td><td>Change in autocorrelation timescale after incremental differencing</td></tr><tr><td>11</td><td><code>MD_pNN_pnn40</code></td><td><code>high_fluctuation</code></td><td><a href="incremental-differences.md"><mark style="color:orange;">Incremental differences</mark></a></td><td>Proportion of high incremental changes in the series</td></tr><tr><td>12</td><td><code>SB_BinaryStats_mean_longstretch1</code></td><td><code>stretch_high</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Longest stretch of above-mean values</td></tr><tr><td>13</td><td><code>SB_BinaryStats_diff_longstretch0</code></td><td><code>stretch_decreasing</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Longest stretch of non-increasing values</td></tr><tr><td>14</td><td><code>SB_MotifThree_quantile_hh</code></td><td><code>entropy_pairs</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Entropy of successive pairs in symbolized series</td></tr><tr><td>15</td><td><code>CO_HistogramAMI_even_5bin_ami2</code></td><td><code>ami2</code></td><td><a href="nonlinear-autocorrelation.md"><mark style="color:orange;">Nonlinear autocorrelation</mark></a></td><td>Histogram-based automutual information (lag 2, 5 bins)</td></tr><tr><td>16</td><td><code>CO_trev_1_num</code></td><td><code>trev</code></td><td><a href="nonlinear-autocorrelation.md"><mark style="color:orange;">Nonlinear autocorrelation</mark></a></td><td>Time reversibility</td></tr><tr><td>17</td><td><code>IN_AutoMutualInfoStats_40_gaussian_fmmi</code></td><td><code>ami_timescale</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation structure</mark></a></td><td>First minimum of the AMI function</td></tr><tr><td>18</td><td><code>SB_TransitionMatrix_quantile_3_ac_sumdiagcov</code></td><td><code>transition_variance</code></td><td><a href="symbolic.md"><mark style="color:orange;">Symbolic</mark></a></td><td>Transition matrix column variance</td></tr><tr><td>19</td><td><code>PD_PeriodicityWang_th0_01</code></td><td><code>periodicity</code></td><td><a href="linear-autocorrelation-structure.md"><mark style="color:orange;">Linear autocorrelation structure</mark></a></td><td>Wang's periodicity metric</td></tr><tr><td>20</td><td><code>CO_Embed2_Dist_tau_d_expfit_meandiff</code></td><td><code>embedding_dist</code></td><td><a href="other.md"><mark style="color:orange;">Other</mark></a></td><td>Goodness of exponential fit to embedding distance distribution</td></tr><tr><td>21</td><td><code>SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1</code></td><td><code>rs_range</code></td><td><a href="self-affine-scaling.md"><mark style="color:orange;">Self-affine scaling</mark></a></td><td>Rescaled range fluctuation analysis (low-scale scaling)</td></tr><tr><td>22</td><td><code>SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1</code></td><td><code>dfa</code></td><td><a href="self-affine-scaling.md"><mark style="color:orange;">Self-affine scaling</mark></a></td><td>Detrended fluctuation analysis (low-scale scaling)</td></tr></tbody></table>

## Feature names before v1.0 and _hctsa_ code

From v1.0, each _catch22_ feature exactly reproduces a call to the _hctsa_ code (v3.0.0), applied to the _z_-scored time series, `x_z`, as listed below (the output field is selected by the trailing `.field`).
Long names follow _hctsa_'s name for that output where _hctsa_ registers it as a feature.
The three features that _hctsa_ does not register (`high_fluctuation`, `ami2`, and `transition_variance`) take the name that _hctsa_'s naming convention gives the call.

The names used before v1.0 are listed for comparison with earlier results.
Some of them now refer to different features (or to none) in current _hctsa_, so use the _hctsa_ code listed here to reproduce a _catch22_ feature in _hctsa_.

<table data-full-width="false"><thead><tr><th width="69">#</th><th>Short name</th><th>Name before v1.0</th><th><em>hctsa</em> code</th><th>Masked value</th><th>Change in v1.0 (from v0.6.0)</th></tr></thead><tbody><tr><td>1</td><td><code>mode_5</code></td><td>(unchanged)</td><td><code>DN_HistogramMode(x_z,5,true,false)</code></td><td>0</td><td>Binning (values differ by about 10<sup>-6</sup>)</td></tr><tr><td>2</td><td><code>mode_10</code></td><td>(unchanged)</td><td><code>DN_HistogramMode(x_z,10,true,false)</code></td><td>0</td><td>Binning (values differ by about 10<sup>-6</sup>)</td></tr><tr><td>3</td><td><code>outlier_timing_pos</code></td><td>(unchanged)</td><td><code>DN_OutlierInclude(x_z,'pos',0.01).mdrmd</code></td><td>0</td><td>None</td></tr><tr><td>4</td><td><code>outlier_timing_neg</code></td><td>(unchanged)</td><td><code>DN_OutlierInclude(x_z,'neg',0.01).mdrmd</code></td><td>0</td><td>None</td></tr><tr><td>5</td><td><code>acf_timescale</code></td><td><code>CO_f1ecac</code></td><td><code>CO_FirstCrossing(x_z,'ac',1/exp(1),'both').pointOfCrossing</code></td><td><em>N</em> − 1</td><td>Name; <em>N</em> − 1 (was <em>N</em>) if no crossing</td></tr><tr><td>6</td><td><code>acf_first_min</code></td><td>(unchanged)</td><td><code>CO_FirstMin(x_z,'ac')</code></td><td><em>N</em></td><td>None</td></tr><tr><td>7</td><td><code>low_freq_power</code></td><td><code>SP_Summaries_welch_rect_area_5_1</code></td><td><code>SP_Summaries(x_z,'fft',[],[],false).area_5_1</code></td><td>1</td><td>Name; frequency bins</td></tr><tr><td>8</td><td><code>median_freq</code></td><td><code>SP_Summaries_welch_rect_centroid</code></td><td><code>SP_Summaries(x_z,'fft',[],[],false).wq_50</code></td><td>0</td><td>Name and short name (was <code>centroid_freq</code>); frequency bins</td></tr><tr><td>9</td><td><code>forecast_error</code></td><td><code>FC_LocalSimple_mean3_stderr</code></td><td><code>FC_LocalSimple(x_z,'mean',3).stde</code></td><td>0</td><td>Name</td></tr><tr><td>10</td><td><code>whiten_timescale</code></td><td><code>FC_LocalSimple_mean1_tauresrat</code></td><td><code>FC_LocalSimple(x_z,'mean',1).taurat</code></td><td>0</td><td>Name; interpolated zero-crossings</td></tr><tr><td>11</td><td><code>high_fluctuation</code></td><td><code>MD_hrv_classic_pnn40</code></td><td><code>MD_pNN(x_z).pnn40</code></td><td>0</td><td>Name</td></tr><tr><td>12</td><td><code>stretch_high</code></td><td>(unchanged)</td><td><code>SB_BinaryStats(x_z,'mean').longstretch1</code></td><td>0</td><td>None (corrected in v0.6.0)</td></tr><tr><td>13</td><td><code>stretch_decreasing</code></td><td>(unchanged)</td><td><code>SB_BinaryStats(x_z,'diff').longstretch0</code></td><td>0</td><td>None (corrected in v0.6.0)</td></tr><tr><td>14</td><td><code>entropy_pairs</code></td><td>(unchanged)</td><td><code>SB_MotifThree(x_z,'quantile').hh</code></td><td>0</td><td>Bias correction</td></tr><tr><td>15</td><td><code>ami2</code></td><td><code>CO_HistogramAMI_even_2_5</code></td><td><code>CO_HistogramAMI(x_z,1:5,'even',5).ami2</code></td><td>0</td><td>Name; bin edges; bias correction</td></tr><tr><td>16</td><td><code>trev</code></td><td>(unchanged)</td><td><code>CO_trev(x_z,1).num</code></td><td>0</td><td>None</td></tr><tr><td>17</td><td><code>ami_timescale</code></td><td>(unchanged)</td><td><code>IN_AutoMutualInfoStats(x_z,40,'gaussian').fmmi</code></td><td>min(40, ⌈<em>N</em>/2⌉)</td><td>None (corrected in v0.6.0)</td></tr><tr><td>18</td><td><code>transition_variance</code></td><td><code>SB_TransitionMatrix_3ac_sumdiagcov</code></td><td><code>SB_TransitionMatrix(x_z,'quantile',3,'ac').sumdiagcov</code></td><td>1/9</td><td>Name; anti-aliased downsampling</td></tr><tr><td>19</td><td><code>periodicity</code></td><td>(unchanged)</td><td><code>PD_PeriodicityWang(x_z).th0_01</code></td><td>0</td><td>None (corrected in v0.6.0)</td></tr><tr><td>20</td><td><code>embedding_dist</code></td><td>(unchanged)</td><td><code>CO_Embed2_Dist(x_z,'tau').d_expfit_meandiff</code></td><td>≈ 707 + 0.71<em>N</em></td><td>Histogram binning and normalisation</td></tr><tr><td>21</td><td><code>rs_range</code></td><td>(unchanged)</td><td><code>SC_FluctAnal(x_z,2,'rsrangefit',50,1,[],true).prop_r1</code></td><td>1</td><td>Two-regime fit; timescales</td></tr><tr><td>22</td><td><code>dfa</code></td><td>(unchanged)</td><td><code>SC_FluctAnal(x_z,2,'dfa',50,1,2,true).prop_r1</code></td><td>1</td><td>Two-regime fit; timescales</td></tr></tbody></table>

The 'Masked value' column gives the value that each feature takes, by default, when it cannot be estimated (see [Masking](#masking)), for a time series of length _N_.
The 'Change in v1.0' column summarises how v1.0 differs from v0.6.0, apart from masking: see each feature's description for details.

### Masking

Some features cannot be estimated for some time series.
Most commonly, this is a constant time series, which cannot be _z_-scored, but it also includes time series that are too short (e.g., `dfa` needs at least 79 samples, and `rs_range` at least 40, as in _hctsa_), and a few degenerate cases (e.g., `embedding_dist` when all embedding distances are equal).
Such features have no value (`NaN`).
From v1.0, _catch22_ instead **masks** them by default, setting each to the fixed value listed above, at the appropriate end of the feature's range, so that 22 real numbers are always returned.
A constant time series is treated as the limit of an infinitely slow, perfectly predictable time series (e.g., `acf_timescale` = _N_ − 1, an autocorrelation that never decays, and `low_freq_power` = 1, all power at the lowest frequencies), and a failure as the absence of the structure that a feature looks for (e.g., a single scaling regime for `rs_range` and `dfa`).
Masking can be switched off to obtain `NaN` instead.

Time series containing `NaN` or `Inf` values are never masked: all outputs are then `NaN`.
Before v1.0, features that could not be estimated returned `NaN` or, in some cases, 0 (e.g., `acf_timescale`, `acf_first_min`, and `periodicity` for a constant time series, `rs_range` and `dfa` for short time series, and `embedding_dist` for nearly equal distances).

***

## _Catch24_ (_Catch22_ + Mean + Std)

In some cases, in which scale and spread of the raw time-series values may be relevant to class differences, the two simple distributional moment features (using the `catch24` flag in the software implementations) can be added. This will result in 24 features being calculated: the _catch22_ features in addition to the **mean** and **standard deviation**.&#x20;

<table data-full-width="false"><thead><tr><th width="136">#</th><th>Feature name</th><th>Short name</th><th>Description</th><th>Name before v1.0</th><th><em>hctsa</em> code</th></tr></thead><tbody><tr><td>23</td><td><mark style="color:green;"><code>DN_mean</code></mark></td><td><code>mean</code></td><td>Mean</td><td><code>DN_Mean</code></td><td><code>DN_Mean(x,'norm')</code></td></tr><tr><td>24</td><td><mark style="color:green;"><code>DN_Spread_std</code></mark></td><td><code>std</code></td><td>Standard deviation</td><td><code>DN_Spread_Std</code></td><td><code>DN_Spread(x,'std')</code></td></tr></tbody></table>

These two features are computed on the raw time series, `x` (not _z_-scored), and their values are unchanged in v1.0.
Their long names changed in v1.0 only in case, to match _hctsa_.
They are not masked (e.g., the standard deviation of a single value is `NaN`).

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

## Changes in v1.0

_catch22_ v1.0 matches each feature exactly to the _hctsa_ code listed [above](#feature-names-before-v1.0-and-hctsa-code).
Results computed with v1.0 are therefore not directly comparable with earlier versions for the following features (each feature's description has a note giving the details):

* **Values change substantially for most time series**: `whiten_timescale` (interpolated zero-crossings), `ami2` (bin edges and bias correction), `transition_variance` (anti-aliased downsampling), `embedding_dist` (histogram binning and normalisation), and `rs_range` and `dfa` (two-regime fit).
* **Values change slightly for most time series**: `mode_5` and `mode_10` (by about $$10^{-6}$$, from the bin edges), `entropy_pairs` (bias correction, at most $$2/(N-1)$$), and `low_freq_power` (one more frequency bin, which occasionally makes a substantial difference).
* **Values change for a few time series**: `median_freq` (mostly those with a lot of power near the Nyquist frequency).
* **Values unchanged** (apart from masking and, for `acf_timescale`, the value when there is no crossing): `outlier_timing_pos`, `outlier_timing_neg`, `acf_timescale`, `acf_first_min`, `forecast_error`, `high_fluctuation`, `stretch_high`, `stretch_decreasing`, `trev`, `ami_timescale`, `periodicity`, and the _catch24_ features, `mean` and `std`.
* **Long names**: eight _catch22_ features, and the two _catch24_ features, have new long names (listed above), and `centroid_freq` is now `median_freq`.
* **Masking**: features that cannot be estimated are [masked](#masking) by default.

Earlier changes: in v0.6.0, four features were corrected to match their _hctsa_ definitions (`periodicity` and `ami_timescale` had returned the lag minus one, and `stretch_high` and `stretch_decreasing` had miscounted run lengths), and in v0.3, `acf_timescale` changed from an integer lag to a linearly interpolated value.
