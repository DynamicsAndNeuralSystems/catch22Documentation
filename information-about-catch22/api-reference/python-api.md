---
description: >-
  This is the class and function reference of pycatch22. Please refer to the
  user guide for further details as the class and function specifications may
  not be sufficient to give full context.
cover: >-
  https://images.unsplash.com/photo-1623282033815-40b05d96c903?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxBUEklMjB8ZW58MHx8fHwxNzEwMjg4MTcxfDA&ixlib=rb-4.0.3&q=85
coverY: 0
layout:
  width: default
  cover:
    visible: true
    size: full
    mask: none
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
---

# Python API

This page describes _pycatch22_ v1.0, which computes _catch22_ v1.0.

## <mark style="color:purple;">catch22\_all</mark>

Extract the _catch22_ feature set from an input time series.

### **Syntax**

<pre class="language-python"><code class="lang-python"><strong>pycatch22.catch22_all(data, catch24=False, short_names=False, mask_nans=True, return_masked=False)
</strong></code></pre>

### Parameters

| Parameter       | Type                                                                                    | Description                                                                                                                                                                                                                                       |
| --------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`          | [array-like](https://numpy.org/doc/stable/user/basics.creation.html#array-like-objects) | Input time-series data: any sequence of numbers (e.g., a list, tuple, or `numpy` array). Other input raises a `TypeError`.                                                                                                                         |
| `catch24`       | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, include the two catch24 features (mean and standard deviation) in the output. Default is False.                                                                                                                                          |
| `short_names`   | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, also include the short names of the features in the output. Default is False.                                                                                                                                                            |
| `mask_nans`     | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, a feature that cannot be estimated (e.g., for a constant or very short time series) is [masked](../feature-descriptions/feature-overview-table.md#masking): set to a fixed value at the appropriate end of its range. If False, it is NaN. Default is True. |
| `return_masked` | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, also include whether each feature value was masked in the output. Default is False.                                                                                                                                                      |

A time series that is empty or contains NaN or Inf gives NaN for every feature (whatever `mask_nans` is), with a `RuntimeWarning`.

***

### Return Value

The function returns a [dictionary](https://docs.python.org/3/library/stdtypes.html#dict) with the following keys:

| Key             | Type                                                         | Description                                                                 |
| --------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------- |
| `'names'`       | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of feature names.                                                      |
| `'values'`      | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of corresponding feature values (floats).                              |
| `'short_names'` | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of short feature names (if `short_names=True`).                        |
| `'masked'`      | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of bools, True for each masked feature value (if `return_masked=True`). |

The features are in the order of the [table of features](../feature-descriptions/feature-overview-table.md).

***

### Example Usage

```python
import numpy as np
import pycatch22

data = np.random.default_rng(42).standard_normal(100)
features = pycatch22.catch22_all(data)
print(features['names'])
# Output: ['DN_HistogramMode_5', 'DN_HistogramMode_10', 'DN_OutlierInclude_p_001_mdrmd', ..., 'SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1']
print(features['values'])
# Output: [0.1874330746477385, 0.4509074548583164, -0.17000000000000004, ..., 0.38095238095238093]

features_with_catch24 = pycatch22.catch22_all(data, catch24=True)
print(features_with_catch24['names'][-2:])
# Output: ['DN_mean', 'DN_Spread_std']

features_with_short_names = pycatch22.catch22_all(data, short_names=True)
print(features_with_short_names['short_names'])
# Output: ['mode_5', 'mode_10', 'outlier_timing_pos', ..., 'rs_range', 'dfa']

features_with_masked = pycatch22.catch22_all(data[:50], short_names=True, return_masked=True)  # first 50 samples: too short for dfa
print([name for name, m in zip(features_with_masked['short_names'], features_with_masked['masked']) if m])
# Output: ['dfa']
```

***

***

## <mark style="color:purple;">feature\_table</mark>

Describe the features, in the order of `catch22_all`.

### Syntax

```python
pycatch22.feature_table(catch24=False)
```

### Parameters

| Parameter | Type                                                                   | Description                                                            |
| --------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `catch24` | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values) | If True, include the two catch24 features. Default is False.           |

### Return Value

A [list](https://docs.python.org/3/library/stdtypes.html#list) with one [dictionary](https://docs.python.org/3/library/stdtypes.html#dict) per feature, with the keys `'name'`, `'short_name'`, `'legacy_name'` (the feature's name before v1.0) and `'hctsa_call'` (the _hctsa_ v3.0.0 code that reproduces it exactly, on `x_z`, the _z_-scored time series, or `x`, the raw time series).

### Example Usage

```python
import pycatch22

print(pycatch22.feature_table()[4])
# Output: {'name': 'CO_FirstCrossing_acf_1e_pointOfCrossing', 'short_name': 'acf_timescale', 'legacy_name': 'CO_f1ecac', 'hctsa_call': "CO_FirstCrossing(x_z,'ac',1/exp(1),'both').pointOfCrossing"}
```

***

***

## <mark style="color:purple;">compute\_feature</mark>

Compute a single feature by its long or short name.
Only that feature is computed, and its value is exactly the one that `catch22_all` gives.

### Syntax

```python
pycatch22.compute_feature(data, name, mask_nans=True)
```

### Parameters

| Parameter   | Type                                                                                    | Description                                                        |
| ----------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `data`      | [array-like](https://numpy.org/doc/stable/user/basics.creation.html#array-like-objects) | Input time-series data (as for `catch22_all`).                     |
| `name`      | [str](https://docs.python.org/3/library/stdtypes.html#str)                              | The feature's long or short name (e.g., `'CO_trev_1_num'` or `'trev'`), as in `feature_table(catch24=True)`. An unknown name (including a name from before v1.0) raises a `ValueError`. |
| `mask_nans` | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | Whether to mask the feature if it cannot be estimated (as for `catch22_all`). Default is True. |

### Return Value

The feature value, as a [float](https://docs.python.org/3/library/functions.html#float).

### Example Usage

```python
import numpy as np
import pycatch22

data = np.random.default_rng(42).standard_normal(100)
print(pycatch22.compute_feature(data, 'mean'))
# Output: -0.05026961148385783
```

***

***

## Individual Feature Methods

Each feature can also be computed on its own as `pycatch22.{name}`, where `{name}` is the feature's **long** name in the [table of features](../feature-descriptions/feature-overview-table.md).
Only that feature is computed, and its value is exactly the one that `catch22_all` gives.

### Syntax

```python
pycatch22.DN_mean(data, mask_nans=True)
```

### Parameters

| Parameter   | Type                                                                                    | Description                                                        |
| ----------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `data`      | [array-like](https://numpy.org/doc/stable/user/basics.creation.html#array-like-objects) | Input time-series data (as for `catch22_all`).                     |
| `mask_nans` | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | Whether to mask the feature if it cannot be estimated (as for `catch22_all`). Default is True. |

### Return Values

| Value     | Type                                                         | Description               |
| --------- | ------------------------------------------------------------ | ------------------------- |
| `feature` | [float](https://docs.python.org/3/library/functions.html#float) | Individual feature value. |

### Example Usage

```python
import numpy as np
from pycatch22 import DN_mean

data = np.random.default_rng(42).standard_normal(100)
individual_feature = DN_mean(data)
print(individual_feature)
# Output: -0.05026961148385783
```

{% hint style="warning" %}
**Deprecated names:** The long names used before v1.0 (e.g., `pycatch22.CO_f1ecac`, `pycatch22.DN_Mean`) still work, but are deprecated: they compute the v1.0 feature and issue a `DeprecationWarning`.
{% endhint %}

***
