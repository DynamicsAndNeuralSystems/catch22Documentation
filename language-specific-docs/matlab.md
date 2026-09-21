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

2. In MATLAB, navigate to the `wrap_MATLAB` directory in the cloned repo and call `mexAll` from the MATLAB command window. Ensure you include the folder in your MATLAB path to use the package:

```matlab
cd('catch22/wrap_MATLAB')
addpath('catch22/wrap_MATLAB') % add folder in MATLAB path
mexAll 
```

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

_catch22_ MATLAB only accepts one univariate time series at a time, with the input being passed in as a **column vector.** Consider the following example for a single time series of length 10 (i.e., 10 points):

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

All features are bundled into a function called `catch22_all`. By default, the extended _catch24_ feature set will be computed unless a boolean flag which corresponds to the argument `doCatch24` (which can either be set to `true` or `false)` is used in the function call:&#x20;

* `true` -> return the _catch24_ feature set (_catch22_ + mean + std. dev.).
* `false` -> return the standard _catch22_ feature set.

```matlab
% compute catch24 features
[vals, names] = catch22_all(data);
% for the catch22 feature set
[vals, names] = catch22_all(data, false);
```

### Expected output

An **array** of feature outputs and a **cell** of feature names (`24x1` if _catch24_, `22x1` if _catch22_) will be returned by MATLAB. The order in which the feature outputs are returned corresponds to the order in which feature names are returned, e.g., the feature output in `vals(1)` corresponds to the feature name `names(1)`:

```matlab
% Print feature name and value
for i = 1:size(names,1)
    fprintf('%s: %f\n', names{i}, vals(i));
end
```

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

***

## Advanced Usage

Ready to explore some of the additional functionality of _catch22_? Here we provide a usage guide for some of the &#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features. In MATLAB, users can enable the _catch24_ feature set by passing the boolean flag `true` into the `catch22_all` function call:

```matlab
tsData = rand(100,1) % create some time series
[vals, names] = catch22_all(tsData, true)
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

If you do not wish to compute the full _catch22_ feature set on your time series, you can alternatively compute each feature (including mean and std. deviation) individually.

{% tabs %}
{% tab title="Computing Individual Features" %}
To compute individual features in MATLAB, you can call the corresponding function given by `catch22_{featureName}` where `featureName` is the feature's long name (as in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)). For example, `catch22_DN_HistogramMode_5` can be called to compute the feature [`DN_HistogramMode_5`](../information-about-catch22/feature-descriptions/linear-autocorrelation-structure.md). You can retrive all feature names by calling `GetAllFeatureNames.`

As an example, let's compute only the feature `DN_HistogramMode_5` on a single univariate time series:

```matlab
tsData = rand(1, 1000) % create length 1000 time series
f = catch22_DN_HistogramMode_5(tsData) % returns a double
fprintf('DN_HistogramMode_5 value: %f\n', f) 
```

Note that here we pass our time series as a **row vector** (`1x1000`) to the individual feature function. On the other hand, the `catch22_all` function for computing the entire feature set only accepts time series as a **column vector** (`1000x1`)**.**&#x20;
{% endtab %}
{% endtabs %}



### 3. Short Names

For each feature, we also include a unique 'short name' for easier reference (as outlined in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).&#x20;

{% tabs %}
{% tab title="Short Names" %}
In MATLAB, short names can be included in the output when calling `catch22_all` by specifying an additional output:

```python
tsData = rand(1000, 1) % your time series data
[feature_vals, long_names, short_names] = catch22_all(tsData, false)
% print the short name and corresponding feature value
for i=1:size(feature_vals, 1):
    fprintf("%s : %f\n", short_names{i}, feature_vals(i))
end
```

Now, in addition to the cell of long names and feature values, we also obtain an additional cell (`22x1` for _catch22,_ `24x1` for _catch24_) of short feature names.&#x20;
{% endtab %}
{% endtabs %}



***

## FAQ

Click one of the expandable tabs below to explore commonly asked questions about _pycatch22_ in MATLAB:

<details>

<summary>How can I submit a bug report for c<em>atch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/DynamicsAndNeuralSystems/catch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

***

***
