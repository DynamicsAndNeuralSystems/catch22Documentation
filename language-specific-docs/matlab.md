---
description: A detailed usage guide for installing and using catch22 in MATLAB.
cover: >-
  https://images.unsplash.com/photo-1581092580497-e0d23cbdf1dc?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw4fHxlbmdpbmVlcmluZ3xlbnwwfHx8fDE3MTEzMzU0Nzl8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# MATLAB

## MATLAB Usage Guide

Select a card below to access MATLAB-specific usage information.&#x20;

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Installation</strong></td><td></td><td><a href="../.gitbook/assets/install_light.png">install_light.png</a></td><td><a href="matlab.md#installation">#installation</a></td></tr><tr><td></td><td align="center"><strong>Getting started</strong></td><td></td><td><a href="../.gitbook/assets/getting_started_light.png">getting_started_light.png</a></td><td><a href="matlab.md#getting-started-basic-usage">#getting-started-basic-usage</a></td></tr><tr><td></td><td align="center"><strong>Advanced usage</strong></td><td></td><td><a href="../.gitbook/assets/adv_usage_light.png">adv_usage_light.png</a></td><td><a href="matlab.md#advanced-usage">#advanced-usage</a></td></tr><tr><td></td><td align="center"><strong>Frequently Asked Questions</strong></td><td></td><td><a href="../.gitbook/assets/FAQ_light.png">FAQ_light.png</a></td><td><a href="matlab.md#faq">#faq</a></td></tr><tr><td></td><td align="center"><strong>API Reference</strong></td><td></td><td><a href="../.gitbook/assets/API_light.png">API_light.png</a></td><td><a href="../information-about-catch22/api-reference/matlab-api.md">matlab-api.md</a></td></tr></tbody></table>

***

***

## Installation

***

1. To get started with installing _catch22_ in MATLAB, clone the repository to a location of your choice using the following command in `Bash/CMD`:

```bash
git clone https://github.com/DynamicsAndNeuralSystems/catch22.git
```

2. In MATLAB, navigate to the `wrap_Matlab` directory in the cloned repo and call `mexAll` from the MATLAB command window.
   This compiles the _catch22_ C code into a single MEX function, `catch22_mex`, which needs a C compiler set up for MATLAB (see `mex -setup C`).
   Ensure you include the folder in your MATLAB path to use the package:

```matlab
cd('catch22/wrap_Matlab')
addpath(pwd) % add folder in MATLAB path
mexAll
```

3. Optionally, check the installation by running `testing`, which compares `catch22_all` with the stored outputs of the C code for the time series in `catch22/testData/basic`:

```matlab
testing
```

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

_catch22_ MATLAB takes one univariate time series at a time, passed as a **vector** (row or column).
Consider the following example for a single time series of length 10 (i.e., 10 points):

```matlab
>> data = rand(10,1)
% example of the expected format for your time series data
data =
    0.5672
    0.9982
    0.1319
    ...
    0.0813
    0.6592
```

### Computing _catch22_ features

All features are bundled into a function called `catch22_all`.
By default, the extended _catch24_ feature set will be computed unless the second argument, `doCatch24`, is set to `false`:

* `true` (default) -> return the _catch24_ feature set (_catch22_ + mean + std. dev.).
* `false` -> return the standard _catch22_ feature set.

```matlab
% compute catch24 features
[vals, names] = catch22_all(data);
% for the catch22 feature set
[vals, names] = catch22_all(data, false);
```

### Expected output

A **column vector** of feature values and a **cell** of feature names (`24x1` if _catch24_, `22x1` if _catch22_) will be returned by MATLAB.
The order in which the feature outputs are returned corresponds to the order in which feature names are returned, e.g., the feature output in `vals(1)` corresponds to the feature name `names{1}`:

```matlab
% Print feature name and value
for i = 1:numel(names)
    fprintf('%s: %f\n', names{i}, vals(i));
end
```

The features are returned in the order of the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md), with the names of _catch22_ v1.0.

{% hint style="info" %}
**catch22 v1.0** changed the values of most features (to match _hctsa_ v3.0.0 exactly) and renamed several of them, so outputs are not comparable with those of v0.x.
See the [feature descriptions](../information-about-catch22/feature-descriptions/) for what changed.
{% endhint %}

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

***

## Advanced Usage

Ready to explore some of the additional functionality of _catch22_?

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features.
In MATLAB, _catch24_ is computed by default; you can also request it explicitly by passing `true` as the second argument of `catch22_all`:

```matlab
tsData = rand(100,1); % create some time series
[vals, names] = catch22_all(tsData, true);
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

{% tabs %}
{% tab title="Computing Individual Features" %}
From v1.0, the MATLAB wrapper no longer has a separate function for each feature: compute them all with `catch22_all` and select the one you need by its name (as in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).
For example, to get only `DN_HistogramMode_5`:

```matlab
tsData = rand(1000,1); % create length 1000 time series
[vals, names] = catch22_all(tsData, false);
f = vals(strcmp(names, 'DN_HistogramMode_5'));
fprintf('DN_HistogramMode_5 value: %f\n', f)
```
{% endtab %}
{% endtabs %}



### 3. Short Names

For each feature, we also include a unique 'short name' for easier reference (as outlined in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).&#x20;

{% tabs %}
{% tab title="Short Names" %}
In MATLAB, short names can be included in the output when calling `catch22_all` by specifying an additional output:

```matlab
tsData = rand(1000, 1); % your time series data
[featureVals, longNames, shortNames] = catch22_all(tsData, false);
% print the short name and corresponding feature value
for i = 1:numel(featureVals)
    fprintf('%s: %f\n', shortNames{i}, featureVals(i));
end
```

Now, in addition to the cell of long names and feature values, we also obtain an additional cell (`22x1` for _catch22,_ `24x1` for _catch24_) of short feature names.

To get the names without computing any features, use `GetAllFeatureNames`, which also returns each feature's name before v1.0 and the _hctsa_ code that reproduces it:

```matlab
[longNames, shortNames, legacyNames, hctsaCalls] = GetAllFeatureNames(false);
```
{% endtab %}
{% endtabs %}



### 4. Masking

Some features cannot be estimated for some time series, e.g., any feature of a constant time series, or `dfa` for a time series shorter than 79 samples.

{% tabs %}
{% tab title="Masking" %}
By default, such features are [masked](../information-about-catch22/feature-descriptions/feature-overview-table.md#masking): set to a fixed value at the appropriate end of the feature's range, so that real numbers are always returned.
The fourth output of `catch22_all` says which values were masked, and setting the third argument, `maskNaNs`, to `false` gives `NaN` instead:

```matlab
tsData = rand(50,1); % too short for dfa
[vals, names, shortNames, isMasked] = catch22_all(tsData, false); % dfa is masked (to 1)
shortNames(isMasked)
vals = catch22_all(tsData, false, false); % dfa is NaN
```

A time series containing `NaN` or `Inf` gives `NaN` for every feature (it is never masked).
{% endtab %}
{% endtabs %}



***

## FAQ

Click one of the expandable tabs below to explore commonly asked questions about _catch22_ in MATLAB:

<details>

<summary>How can I submit a bug report for c<em>atch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/DynamicsAndNeuralSystems/catch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

<details>

<summary>How do I reproduce a <em>catch22</em> feature in <em>hctsa</em>?</summary>

From v1.0, each _catch22_ feature exactly reproduces a call to _hctsa_ v3.0.0 code, applied to the _z_-scored time series, `x_z` (or, for the mean and standard deviation, to the raw time series, `x`).
These calls are returned as the fourth output of `GetAllFeatureNames`, and listed in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md#feature-names-before-v1.0-and-hctsa-code).

</details>

***

***
