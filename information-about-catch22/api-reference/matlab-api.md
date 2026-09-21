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

## <mark style="color:orange;">catch22\_all</mark>

_Automatically run every time series feature calculation included in the catch22/24 set._

***

### Syntax

```matlab
[featureValues, featureNamesLong, featureNamesShort] = catch22_all(data, doCatch24)
```

### Description

`[featureValues, featureNamesLong, featureNamesShort] = catch22_all(data, doCatch24)` returns a `24x1` array of feature outputs, a `24x1` array of long feature names and `24x1` array of short feature names for the input time series, `data`.

***

### Examples

<details>

<summary>Compute <em>catch22</em> features</summary>

Compute _catch22_ features on a Gaussian i.i.d. random number sequence.

<pre class="language-matlab"><code class="lang-matlab">tsData = rand(100, 1) % data as a column vector

% compute catch22 features
<strong>[featureValues, featureNamesLong, featureNameShort] = catch22_all(data, false)
</strong>
% compute cactch24 features
[featureValues, featureNamesLong, featureNameShort] = catch22_all(data)
</code></pre>

</details>

***

### Input Arguments

<details>

<summary>data - time series as column vector</summary>

Input univariate time series, specified as a column vector.&#x20;

**Data Types**:  double

</details>

<details>

<summary>doCatch24 - Include Mean and Std. Deviation</summary>

Whether to include mean and standard deviation (a total of 24 features). Default is `true`.&#x20;

**Data Types:** logical

</details>

***

### Output Arguments

<details>

<summary>featureValues </summary>

Computed feature values for the input data.

**Data Type:** cell array

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

***

## <mark style="color:orange;">GetAllFeatureNames</mark>

_Get all feature names for the catch22/24 feature set._&#x20;

***

### Syntax

```matlab
[featureNamesLong, featureNamesShort] = GetAllFeatureNames(doCatch24)
```

***

### Description

`[featureNamesLong, featureNamesShort] = GetAllFeatureNames(doCatch24)` retrieves all feature names for the _catch22_ (or _catch24_) feature set.&#x20;

***

### Examples

<details>

<summary>Retrieve <em>catch22/24</em> feature names</summary>

```matlab
% get catch24 feature names
[featureNamesLong24,featureNamesShort24] = GetAllFeatureNames(true)
% get catch22 feature names
[featureNamesLong22,featureNamesShort22] = GetAllFeatureNames(false)
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

***

***

## <mark style="color:orange;">catch22\_DN\_HistogramMode\_5</mark>

_Measures the mode of the data vector using histogram with 5 bins._&#x20;

{% hint style="info" %}
**Note:** This is a typical example of how an individual feature method is called. All features are called in this way and accept the same data types as inputs while returning the same data types as outputs. You can call each feature individually using its corresponding **long name** as per the [table of features](../feature-descriptions/feature-overview-table.md), e.g., `catch22_CO_f1ecac.`
{% endhint %}

### Syntax

<pre class="language-matlab"><code class="lang-matlab"><strong>% individual feature method of interest, e.g., DN_HistogramMode_5
</strong><strong>featureVal = catch22_DN_HistogramMode_5(data)
</strong></code></pre>

### Description

`featureVal = catch22_DN_HistogramMode_5(data)` computes the mode of the data vector using a histogram with 5 bins.&#x20;

### Examples

<details>

<summary>Compute DN_HistogramMode_5 for time series</summary>

```matlab
tsData = rand(1, 100) % time series as a row vector
featureVal = catch22_DN_HisogramMode_5(tsData)
```

</details>

### Input Arguments

<details>

<summary>data - time series row vector </summary>

Input univariate time series, specified as a row vector.&#x20;

**Data Types**:  double

</details>

### Output Arguments

<details>

<summary>featureVal - feature output</summary>

Output corresponding to the feature method.

**Data Type:** double

</details>

**Note**: All _catch22/24_ feature methods expect the same input arguments (and data types) and return the same output arguments (and data types).&#x20;

***
