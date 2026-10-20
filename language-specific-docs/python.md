---
description: A detailed usage guide for installing and using catch22 in Python.
cover: >-
  https://images.unsplash.com/photo-1614189313880-13b09111b4de?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHxpbnN0YWxsYXRpb258ZW58MHx8fHwxNzExMzMxMzM5fDA&ixlib=rb-4.0.3&q=85
coverY: 169
---

# Python

## Python Usage Guide

Select a card below to access Python-specific usage information.&#x20;

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Installation</strong></td><td></td><td><a href="../.gitbook/assets/install_light.png">install_light.png</a></td><td><a href="python.md#installation">#installation</a></td></tr><tr><td></td><td align="center"><strong>Getting started</strong></td><td></td><td><a href="../.gitbook/assets/getting_started_light.png">getting_started_light.png</a></td><td><a href="python.md#getting-started-basic-usage">#getting-started-basic-usage</a></td></tr><tr><td></td><td align="center"><strong>Advanced usage</strong></td><td></td><td><a href="../.gitbook/assets/adv_usage_light.png">adv_usage_light.png</a></td><td><a href="python.md#advanced-usage">#advanced-usage</a></td></tr><tr><td></td><td align="center"><strong>Frequently Asked Questions</strong></td><td></td><td><a href="../.gitbook/assets/FAQ_light.png">FAQ_light.png</a></td><td><a href="python.md#faq">#faq</a></td></tr><tr><td></td><td align="center"><strong>API Reference</strong></td><td></td><td><a href="../.gitbook/assets/API_light.png">API_light.png</a></td><td><a href="../information-about-catch22/api-reference/python-api.md">python-api.md</a></td></tr></tbody></table>

***

***

## Installation&#x20;

***

Python users can install _pycatch22_ via [pip](https://pypi.org/project/pycatch22/) using the following command:

```bash
pip install pycatch22
```

_pycatch22_ supports Python 3.8 and later, and has no runtime dependencies (`numpy` is optional).
_pycatch22_ v1.0 uses _catch22_ v1.1.0.

Additionally, _pycatch22_ can also be installed using the following alternative installation options. Click the expandable drop-down tabs to explore these options:

<details>

<summary>Alternative Installation Option 1: Anaconda/Mamba</summary>

_pycatch22_ is also available as a package on [anaconda](https://anaconda.org/conda-forge/pycatch22) which you can install via `conda` or `mamba` using:

```bash
conda install -c conda-forge pycatch22
```

_OR_

<pre class="language-bash"><code class="lang-bash"><strong>mamba install -c conda-forge pycatch22
</strong></code></pre>

The conda-forge package can lag behind PyPI, so check that it gives you version 1.0 or later (`conda list pycatch22`).

</details>

<details>

<summary>Alternative Installation Option 2: Manual Install</summary>

This should not be require&#x64;_,_ but if you find issues with the `pip` or `conda` install options, you can build _pycatch22_ from source (this needs a C compiler).

First, clone the [_pycatch22_](https://github.com/DynamicsAndNeuralSystems/pycatch22.git) repository to your local machine, including the _catch22_ C code (a git submodule), using:

```
git clone --recurse-submodules https://github.com/DynamicsAndNeuralSystems/pycatch22.git
```

Navigate to the location of the cloned repository and install manually using:

```bash
pip install .
```

</details>

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

{% hint style="info" %}
**Upgrading from pycatch22 v0.x?**
_pycatch22_ v1.0 computes _catch22_ v1.0, which changed the values of most features (to match _hctsa_ v3.0.0 exactly) and renamed several of them, so results are not comparable with v0.x.
The features are also returned in a different order (e.g., the outlier-timing features are now 3rd and 4th), so select features by name rather than by position.
See the [feature descriptions](../information-about-catch22/feature-descriptions/) for what changed.
{% endhint %}

### Expected Input

In _pycatch22,_ a time series can be any sequence of numbers: a **list**, a **tuple**, or a `numpy` array:

```python
import numpy as np
import pycatch22

rng = np.random.default_rng(42)
x = np.sin(np.linspace(0, 20*np.pi, 1000)) + 0.5*rng.standard_normal(1000)  # (or more interesting data!)
tsData_asList = list(x)
```

Input that is not a sequence of numbers raises a `TypeError`.

### Computing _catch22_ features

All features are bundled in the method `catch22_all` which can simply be called on your time series data as shown below:

<pre class="language-python"><code class="lang-python"><strong># for time series data as a numpy array...
</strong><strong>results = pycatch22.catch22_all(x)
</strong># alternatively, for time series data supplied as a list...
results_list = pycatch22.catch22_all(tsData_asList)
</code></pre>

### Expected output

The results of your calculation will be stored in a **dictionary** with the keys: `names` and `values`:

* `names`: the feature names, as listed in the [feature table](../information-about-catch22/feature-descriptions/feature-overview-table.md), returned as a list of strings.&#x20;
* `values`: the feature outputs returned as a list of floats.&#x20;

Note that the ordering of the feature names is the same as that in which the outputs are given i.e., the first feature in `names` has the output corresponding to the first (zeroth) index in `values.`

Continuing our example from above, we can view the features and their computed values with the following:

```python
for feature, output in zip(results['names'], results['values']):
    print(f"{feature}: {output}")
```

Which gives the following output:

<pre><code><strong>DN_HistogramMode_5: 0.0335441836925168
</strong>DN_HistogramMode_10: -0.7462245693351899
DN_OutlierInclude_p_001_mdrmd: 0.014000000000000012
DN_OutlierInclude_n_001_mdrmd: 0.131
CO_FirstCrossing_acf_1e_pointOfCrossing: 15.700534880150066
CO_FirstMin_ac: 2.0
...
SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1: 0.5652173913043478
</code></pre>

To look up a feature by name, you can make a dictionary of the results, e.g., `dict(zip(results['names'], results['values']))['CO_FirstMin_ac']`.

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

## Advanced Usage

Ready to explore some of the additional functionality of pycatch22?&#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features. To access _catch24_, you can pass an additional parameter to the `catch22_all` method, telling it to compute the _catch24_ feature set as follows:

```python
c24_results = pycatch22.catch22_all(x, catch24=True)
print(c24_results['names'][-2:])  # ['DN_mean', 'DN_Spread_std']
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

If you only need one feature, you can compute it on its own (including the mean and standard deviation).

{% tabs %}
{% tab title="Computing Individual Features" %}
In _pycatch22_ each feature has its own function, named by the feature's **long name** (as in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)), e.g., [`CO_trev_1_num`](../information-about-catch22/feature-descriptions/nonlinear-autocorrelation.md) for computing [`trev`](../information-about-catch22/feature-descriptions/nonlinear-autocorrelation.md).
It takes the same input as `catch22_all` (and the same `mask_nans` option) and returns a float.

As example, let's compute the feature `trev` for the time series `x` from above:

```python
trev_value = pycatch22.CO_trev_1_num(x)
print(f"trev feature output: {trev_value}")
# trev feature output: -0.08153158906199524
```

You can also compute a feature by its long or short name with `compute_feature`:

```python
trev_value = pycatch22.compute_feature(x, 'trev')  # or 'CO_trev_1_num'
# -0.08153158906199524
```

Either way, only that feature is computed, and its value is exactly the one that `catch22_all` gives.

The long names used before v1.0 (e.g., `CO_f1ecac`, now `CO_FirstCrossing_acf_1e_pointOfCrossing`) still work as deprecated aliases: they compute the v1.0 feature and issue a `DeprecationWarning`.
{% endtab %}
{% endtabs %}



### 3. Short Names

For each feature, we also include a unique 'short name' for easier reference (as outlined in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).&#x20;

{% tabs %}
{% tab title="Short Names" %}
In _pycatch22_ short names can be included in the output when calling `catch22_all()` by setting `short_names = True:`

```python
results = pycatch22.catch22_all(x, short_names=True)
```

The returned dictionary, `results`, now has three keys: `names`, `values` and `short_names.`
As before, `names` is a list of the long feature names (as strings), `values` is a list of feature values, and `short_names` is a list of short names (as strings).

Continuing from the basic usage example, let's try printing the feature outputs along with their corresponding short names:

```python
for (short_name, val) in zip(results['short_names'], results['values']):
    print(f"{short_name}: {val}")
```

Here is the output from our example. A lot more readable!

```
mode_5: 0.0335441836925168
mode_10: -0.7462245693351899
outlier_timing_pos: 0.014000000000000012
outlier_timing_neg: 0.131
acf_timescale: 15.700534880150066
...
dfa: 0.5652173913043478
```

`pycatch22.feature_table()` lists every feature's name, short name, name before v1.0 (`legacy_name`), and the _hctsa_ code that reproduces it (`hctsa_call`), as a list of dictionaries:

```python
pycatch22.feature_table()[4]
# {'name': 'CO_FirstCrossing_acf_1e_pointOfCrossing', 'short_name': 'acf_timescale', 'legacy_name': 'CO_f1ecac', 'hctsa_call': "CO_FirstCrossing(x_z,'ac',1/exp(1),'both').pointOfCrossing"}
```
{% endtab %}
{% endtabs %}



### 4. Masking

Some features cannot be estimated for some time series, e.g., any feature of a constant time series, or `dfa` for a time series shorter than 79 samples.

{% tabs %}
{% tab title="Masking" %}
By default, such features are [masked](../information-about-catch22/feature-descriptions/feature-overview-table.md#masking): set to a fixed value at the appropriate end of the feature's range, so that 22 real numbers are always returned.
Set `return_masked=True` to also return `masked`, which says which values were masked, and `mask_nans=False` to get `NaN` instead:

```python
y = np.random.default_rng(1).standard_normal(50)  # too short for dfa
res = pycatch22.catch22_all(y, short_names=True, return_masked=True)
print([(name, value) for name, value, m in zip(res['short_names'], res['values'], res['masked']) if m])
# [('dfa', 1.0)]

res = pycatch22.catch22_all(y, mask_nans=False)
print(res['values'][-1])
# nan
```

A time series that is empty or contains `NaN` or `Inf` gives `NaN` for every feature (it is never masked), with a `RuntimeWarning`.
{% endtab %}
{% endtabs %}



### 5. Multithreading

When working with large time series datasets (containing many univariate time series instances), computing features can often become a significant computational bottleneck. This is where multi-threading comes into play, offering a powerful solution to accelerate feature computation through parallelisation.

{% tabs %}
{% tab title="Multithreading" %}
From v1.0, _pycatch22_ releases Python's global interpreter lock (GIL) while it computes features, so features of different time series can be computed in parallel threads (with earlier versions, threads gave no speedup).

1. Let's start by importing all of the libraries we will need:

```python
import os
from concurrent.futures import ThreadPoolExecutor
import numpy as np
import pycatch22
```

and then create a large time series dataset consisting of `10 000` univariate time series instances, each of length `1000:`

<pre class="language-python"><code class="lang-python"><strong># generate list where each entry is a length 1000 time series
</strong>rng = np.random.default_rng(0)
dataset = [rng.standard_normal(1000) for _ in range(10000)]
</code></pre>

2. Next, we will need to define a function which takes in a single time series, applies _catch22_, and then returns the feature values:

```python
def compute_features(x):
    return pycatch22.catch22_all(x)['values']
```

3. Now we can compute the features of all the time series in parallel, with one thread per CPU core (`os.cpu_count()`):

```python
with ThreadPoolExecutor(max_workers=os.cpu_count()) as executor:
    feature_matrix = np.array(list(executor.map(compute_features, dataset)))

print(feature_matrix.shape)  # (10000, 22): one row per time series
```

The speedup grows with the number of threads: for example, 4 threads computed the features of 200 time series of length 5000 about 3.8 times faster than a single thread (on Apple Silicon).
{% endtab %}
{% endtabs %}

***

## FAQ

***

***

Click one of the expandable tabs below to explore commonly asked questions about _pycatch22_:

<details>

<summary>How can I submit a bug report for <em>pycatch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/DynamicsAndNeuralSystems/pycatch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

<details>

<summary>Which Python versions are compatible with <em>pycatch22</em>? </summary>

We have conducted thorough [unit testing](https://github.com/DynamicsAndNeuralSystems/pycatch22/blob/main/.github/workflows/run_unit_tests.yaml) across Python versions 3.8 to 3.13 to verify and guarantee compatibility.
These tests are run on Windows, macOS, and Linux, to ensure consistent performance and reliability across different platforms.

</details>

<details>

<summary>How can I install <em>pycatch22</em> on my system?</summary>

We have a detailed installation guide [here](python.md#installation), including instructions for installing using _pip_, as well as alternative installation options including _conda_ and manual (local) install.&#x20;

</details>

<details>

<summary>How can I handle large datasets with many univariate time series? </summary>

While _pycatch22_ only takes a single univariate time series as input, when working with large time series datasets (containing many univariate time series instances), computing features can often become a significant computational bottleneck. While a simple for loop may suffice, such tasks are well suited to multi-threading approaches.

See above in our advanced usage guide for how you can compute _catch22_ features on large datasets using [multi-threading](python.md#id-5.-multithreading).&#x20;

</details>

<details>

<summary>Why do my feature values differ from an earlier version of <em>pycatch22</em>?</summary>

_pycatch22_ v1.0 computes _catch22_ v1.0, in which each feature exactly reproduces a feature of _hctsa_ v3.0.0.
This changed the values of most features, renamed several of them, and changed the order in which they are returned.
Use `pycatch22.feature_table()` to match the new names to the old ones, and see the [feature descriptions](../information-about-catch22/feature-descriptions/) for how each feature changed.
When reporting results, state the version you used (see [Citing _catch22_](../welcome-to-catch22/citing-catch22.md)).

</details>

<details>

<summary>How does <em>pycatch22</em> integrate with <em>sktime</em> for time series analysis? </summary>

[&#x73;_&#x6B;time_](https://github.com/sktime/sktime), a comprehensive Python library for time series machine learning, leverages _catch22_'s capabilities within a broader ecosystem of time series analysis tools for **classification**. For users looking to incorporate _catch22_'s feature extraction into their _sktime_ pipelines, they can do so by calling the [`Catch22Classifier`](https://sktime-backup.readthedocs.io/en/v0.11.4/api_reference/auto_generated/sktime.classification.feature_based.Catch22Classifier.html) which implements a random forest classifier using _catch22_ features calculated on time series data.&#x20;

Tutorials for using _catch22_ within the _sktime_ framework are also available [here](https://github.com/chlubba/sktime-catch22/tree/master/examples).

</details>

***
