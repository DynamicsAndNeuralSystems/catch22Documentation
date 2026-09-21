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

</details>

<details>

<summary>Alternative Installation Option 2: Manual Install</summary>

This should not be require&#x64;_,_ but if you find issues with the `pip` or `conda` install options, you can try installing using `setuptools.`

First, clone the [_pycatch22_](https://github.com/DynamicsAndNeuralSystems/pycatch22.git) repository to your local machine using:

```
git clone https://github.com/DynamicsAndNeuralSystems/pycatch22.git
```

Navigate to the location of the cloned repository and install manually using:

```bash
python3 setup.py build
python3 setup.py install
```

</details>

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

In _pycatch22,_ time series can be provided as arrays in the form of **tuples** or **lists** , or as `numpy` arrays:

```python
import pycatch22
tsData_asList = [1, 2, 3, 4] # (or more interesting data!)
tsData_as_numpy = np.array([1, 2, 3, 4])
```

### Computing _catch22_ features

All features are bundled in the method `catch22_all` which can simply be called on your time series data as shown below:

<pre class="language-python"><code class="lang-python"><strong># for time series data supplied as a list...
</strong><strong>results = pycatch22.catch22_all(tsData_asList)
</strong># alternatively, for time series data as a numpy array...
results_numpy = pycatch22.catch22_all(tsData_as_numpy)
</code></pre>

### Expected output

The results of your calculation will be stored in a **dictionary** with the keys: `names` and `values`:

* `names`: the feature names, as listed in the [feature table](../information-about-catch22/feature-descriptions/feature-overview-table.md), returned as a list of strings.&#x20;
* `values`: the feature outputs returned as a list of float64's.&#x20;

Note that the ordering of the feature names is the same as that in which the outputs are given i.e., the first feature in `names` has the output corresponding to the first (zeroth) index in `values.`

Continuing our example from above, we can view the features and their computed values with the following:

```python
for feature, output in zip(results['names'], results['values']):
    print(f"{feature}: {output}")
```

Which gives the following example output:

<pre><code><strong>DN_HistogramMode_5: -0.03625270501343825
</strong>DN_HistogramMode_10: 0.2711751186493062
CO_f1ecac: 0.6562407580313526
CO_FirstMin_ac: 2
...
FC_LocalSimple_mean3_stderr: 1.1654896899009854
</code></pre>

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
import pycatch22
​
tsData = [1, 2, 3, 4] # your ts data
c24_results = pycatch22.catch22_all(tsData, catch24 = True)
```
{% endtab %}
{% endtabs %}



### 2. Computing Individual Features

If you do not wish to compute the full _catch22_ feature set on your time series, you can alternatively compute each feature (including mean and std. deviation) individually.

{% tabs %}
{% tab title="Computing Individual Features" %}
In _pycatch22_ each feature function can be accessed individually and takes **arrays** as **tuples** or **lists** (not `numpy`arrays) as input. To call a feature function, make sure to use its corresponding **long name** (as in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)), e.g., using [`CO_trev_1_num`](../information-about-catch22/feature-descriptions/feature-overview-table.md) for computing [`trev`](../information-about-catch22/feature-descriptions/nonlinear-autocorrelation.md).

As example, let's compute the feature `trev` for a single univariate time series:

```python
import pycatch22
import numpy as np
​
x = np.linspace(0, 10*np.pi, 500)
y = 0.3 * np.sin(x + 0.2) + 0.1 * np.random.randn(500)
tsData = list(y) # your time series data as a list
​
trev_value = pycatch22.CO_trev_1_num(tsData)
print(f"trev feature output: {trev_value}")
```
{% endtab %}
{% endtabs %}



### 3. Short Names

For each feature, we also include a unique 'short name' for easier reference (as outlined in the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md)).&#x20;

{% tabs %}
{% tab title="Short Names" %}
In _pycatch22_ short names can be included in the output when calling `catch22_all()` by setting `short_names = True:`

```python
import pycatch22
import numpy as np
​
data = np.random.randn(100)
data = list(data)
​
# return short names
results = pycatch22.catch22_all(data, short_names=True)
```

The returned dictionary, `results`, now has three keys: `names`, `short_names` and `values.` As before, `names` is a list of the original long feature names (as strings), `short_names` is a list of short names (as strings) and `values` is a list of feature values.&#x20;

Continuing from the basic usage example, let's try printing the feature outputs along with their corresponding short names:

```python
for (short_name, val) in zip(results['short_names'], results['values']):
    print(f"{short_name}: {val}")
```

Here is the output from our example. A lot more readable!

```
mode_5: -0.7500426173295759
mode_10: 1.2786397188378562
acf_timescale: 0.5693516006124166
...
forecast_error: 1.1774630418671619
```
{% endtab %}
{% endtabs %}



### 4. Multithreading

When working with large time series datasets (containing many univariate time series instances), computing features can often become a significant computational bottleneck. This is where multi-threading comes into play, offering a powerful solution to accelerate feature computation through parallelisation.

{% tabs %}
{% tab title="Multithreading" %}
1. Let's start by importing all of the libraries we will need:

```python
import pycatch22 as catch22
import os
from joblib import Parallel, delayed
import numpy as np
```

and then create a large time series dataset consisting of `50 000` univariate time series instances, each of length `1000:`

<pre class="language-python"><code class="lang-python"><strong># generate list where each entry is a length 1000 time series
</strong>dataset = [np.random.randn(1000) for _ in range(50000)]
</code></pre>

2. Next, we will need to define an iterable function which takes in a single time series, applies _catch22_, and then returns the feature values:

```python
def compute_features(x):
    res = catch22.catch22_all(x)
    return res['values'] # just return the values 
```

3. We need to see how many cpu cores are available for multithreading. You can check by running `os.cpu_count()`. The value returned (num of cpu cores available) will differ depending on your hardware and setup:

```python
print(f"Number of cores available: {os.cpu_count()}")
```

4. Now we can parallelise our function using the following:

```python
threads_to_use = os.cpu_count()
results_list = Parallel(n_jobs=threads_to_use)(
    delayed(compute_features)(data[i]) for i in range(len(data))
)

# print the results for the first time series
print(results_list[1]) # prints list of feature outputs for time series 1
```

You should notice that the time to compute all _catch22_ features on the entire dataset of 50 000 time series is much faster than sequentially looping over each!&#x20;

For example, on 8 cores (Mac M1), it takes _\~27_ seconds to compute features on the entire dataset. Without multi-threading this would usually take _\~2.5_ minutes. **That's almost a 5.5x speedup!**
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

We have conducted thorough [unit testing](https://github.com/DynamicsAndNeuralSystems/pycatch22/blob/main/.github/workflows/run_unit_tests.yaml) across Python versions 3.8, 3.9, 3.10, and 3.11 to verify and guarantee compatibility. These tests have been carried out on various operating systems, including Windows, macOS, and Linux, to ensure consistent performance and reliability across different platforms.

</details>

<details>

<summary>How can I install <em>pycatch22</em> on my system?</summary>

We have a detailed installation guide [here](python.md#installation), including instructions for installing using _pip_, as well as alternative installation options including _conda_ and manual (local) install.&#x20;

</details>

<details>

<summary>How can I handle large datasets with many univariate time series? </summary>

While _pycatch22_ only takes a single univariate time series as input, when working with large time series datasets (containing many univariate time series instances), computing features can often become a significant computational bottleneck. While a simple for loop may suffice, such tasks are well suited to multi-threading approaches.

See above in our advanced usage guide for how you can compute _catch22_ features on large datasets using [multi-threading](python.md#id-4.-multithreading).&#x20;

</details>

<details>

<summary>How does <em>pycatch22</em> integrate with <em>sktime</em> for time series analysis? </summary>

[&#x73;_&#x6B;time_](https://github.com/sktime/sktime), a comprehensive Python library for time series machine learning, leverages _catch22_'s capabilities within a broader ecosystem of time series analysis tools for **classification**. For users looking to incorporate _catch22_'s feature extraction into their _sktime_ pipelines, they can do so by calling the [`Catch22Classifier`](https://sktime-backup.readthedocs.io/en/v0.11.4/api_reference/auto_generated/sktime.classification.feature_based.Catch22Classifier.html) which implements a random forest classifier using _catch22_ features calculated on time series data.&#x20;

Tutorials for using _catch22_ within the _sktime_ framework are also available [here](https://github.com/chlubba/sktime-catch22/tree/master/examples).

</details>

***

