---
description: >-
  This is the class and function reference of catch22. Please refer to the user
  guide for further details as the class and function specifications may not be
  sufficient to give full context.
cover: >-
  https://images.unsplash.com/photo-1625535069654-cfeb8f829088?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxNQVRMQUJ8ZW58MHx8fHwxNzEwMjg4MjQzfDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# MATLAB API

The MATLAB functions are in the `wrap_Matlab` folder of the [_catch22_ repository](https://github.com/DynamicsAndNeuralSystems/catch22).
Run `mexAll` once to compile `catch22_mex`, which they call (see [Installation](../../language-specific-docs/matlab.md#installation)).

## <mark style="color:orange;">catch22\_all</mark>

_Automatically run every time series feature calculation included in the catch22/24 set._

***

### Syntax

```matlab
[featureValues, featureNamesLong, featureNamesShort, isMasked] = catch22_all(data, doCatch24, maskNaNs)
```

### Description

`[featureValues, featureNamesLong, featureNamesShort, isMasked] = catch22_all(data, doCatch24, maskNaNs)` returns the 24 (or, if `doCatch24` is `false`, 22) feature values of the input time series, `data`, as a column vector, together with column cell arrays of the long and short feature names and a logical column vector marking masked values.

***

### Examples

<details>

<summary>Compute <em>catch22</em> features</summary>

Compute _catch22_ features on a uniformly distributed random number sequence.

<pre class="language-matlab"><code class="lang-matlab">tsData = rand(100, 1); % data as a vector

% compute catch22 features
<strong>[featureValues, featureNamesLong, featureNamesShort] = catch22_all(tsData, false);
</strong>
% compute catch24 features
[featureValues, featureNamesLong, featureNamesShort] = catch22_all(tsData);

% compute catch22 features, with NaN for features that cannot be estimated
featureValues = catch22_all(tsData, false, false);
</code></pre>

</details>

***

### Input Arguments

<details>

<summary>data - time series as a vector</summary>

Input univariate time series, specified as a vector (row or column).&#x20;

**Data Types**:  double (other numeric types are converted to double)

</details>

<details>

<summary>doCatch24 - Include Mean and Std. Deviation</summary>

Whether to include mean and standard deviation (a total of 24 features). Default is `true`.&#x20;

**Data Types:** logical

</details>

<details>

<summary>maskNaNs - Mask features that cannot be estimated</summary>

Whether to [mask](../feature-descriptions/feature-overview-table.md#masking) features that cannot be estimated (e.g., for a constant or very short time series), setting each to a fixed value at the appropriate end of its range so that real numbers are always returned.
If `false`, such features are `NaN`.
A time series containing `NaN` or `Inf` gives `NaN` for every feature either way.
Default is `true`.

**Data Types:** logical

</details>

***

### Output Arguments

<details>

<summary>featureValues </summary>

Computed feature values for the input data, as a column vector (`22x1` or `24x1`), in the order of the [feature overview table](../feature-descriptions/feature-overview-table.md).

**Data Type:** double

</details>

<details>

<summary>featureNamesLong</summary>

Long feature names corresponding to the feature values.&#x20;

**Data Type**: cell array

</details>

<details>

<summary>featureNamesShort</summary>

Short feature names corresponding to the feature values.&#x20;

**Data Type**: cell array

</details>

<details>

<summary>isMasked</summary>

`true` for each feature value that was masked.

**Data Type**: logical

</details>

***

## <mark style="color:orange;">GetAllFeatureNames</mark>

_Get all feature names for the catch22/24 feature set._&#x20;

***

### Syntax

```matlab
[featureNamesLong, featureNamesShort, legacyNames, hctsaCalls] = GetAllFeatureNames(doCatch24)
```

***

### Description

`[featureNamesLong, featureNamesShort, legacyNames, hctsaCalls] = GetAllFeatureNames(doCatch24)` retrieves all feature names for the _catch22_ (or _catch24_) feature set, in the order of `catch22_all`, together with each feature's name before v1.0 and the _hctsa_ code that reproduces it.

***

### Examples

<details>

<summary>Retrieve <em>catch22/24</em> feature names</summary>

```matlab
% get catch24 feature names
[featureNamesLong24, featureNamesShort24] = GetAllFeatureNames(true)
% get catch22 feature names
[featureNamesLong22, featureNamesShort22] = GetAllFeatureNames(false)
% also get the names before v1.0 and the hctsa code
[featureNamesLong22, featureNamesShort22, legacyNames22, hctsaCalls22] = GetAllFeatureNames(false)
```

</details>

***

### Input Arguments

<details>

<summary>doCatch24</summary>

Whether to include mean and standard deviation as features. Default is `true`.&#x20;

**Data Types:** logical

</details>

***

### Output Arguments

<details>

<summary>featureNamesLong</summary>

Long feature names corresponding to the feature values.&#x20;

**Data Type**: cell array

</details>

<details>

<summary>featureNamesShort</summary>

Short feature names corresponding to the feature values.

**Data Type**: cell array

</details>

<details>

<summary>legacyNames</summary>

Each feature's long name in _catch22_ v0.x, for matching earlier results (the values of most features changed in v1.0, so they are not comparable).

**Data Type**: cell array

</details>

<details>

<summary>hctsaCalls</summary>

The _hctsa_ v3.0.0 code that reproduces each feature exactly, on `x_z` (the _z_-scored time series) or `x` (the raw time series), e.g., `'CO_FirstMin(x_z,''ac'')'`.

**Data Type**: cell array

</details>

***

***

## Individual features

{% hint style="info" %}
**Note:** From v1.0, the MATLAB wrapper has no separate function for each feature (such as `catch22_DN_HistogramMode_5` in earlier versions).
Compute all features with `catch22_all` and select the one you need by name, e.g., `vals(strcmp(names, 'DN_HistogramMode_5'))`.
{% endhint %}

***
