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

## <mark style="color:purple;">catch22\_all</mark>

Extract the _catch22_ feature set from an input time series.

### **Syntax**

<pre class="language-python"><code class="lang-python"><strong>pycatch22.catch22_all(data, catch24=False, short_names=False)
</strong></code></pre>

### Parameters

| Parameter     | Type                                                                                    | Description                                                                                              |
| ------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `data`        | [array-like](https://numpy.org/doc/stable/user/basics.creation.html#array-like-objects) | Input time-series data.                                                                                  |
| `catch24`     | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, include the two catch24 features (mean and standard deviation) in the output. Default is False. |
| `short_names` | [bool](https://docs.python.org/3/library/stdtypes.html#boolean-values)                  | If True, also include the short names of the features in the output. Default is False.                   |

***

### Return Value

The function returns a [dictionary](https://docs.python.org/3/library/stdtypes.html#dict) with the following keys:

| Key             | Type                                                         | Description                                          |
| --------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| `'names'`       | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of feature names.                               |
| `'values'`      | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of corresponding feature values.                |
| `'short_names'` | [list](https://docs.python.org/3/library/stdtypes.html#list) | List of short feature names (if `short_names=True`). |

***

### Example Usage

```python
import numpy as np
import pycatch22 as catch22

data = np.random.rand(100)
features = catch22_all(data)
print(features['names'])
# Output: ['DN_HistogramMode_5', 'DN_HistogramMode_10', 'CO_f1ecac', ..., 'SP_Summaries_welch_rect_centroid', 'FC_LocalSimple_mean3_stderr']
print(features['values'])
# Output: [0.23, 0.18, 1.52, ..., 0.04, 0.67]

features_with_catch24 = catch22_all(data, catch24=True)
print(features_with_catch24['names'][-2:])
# Output: ['DN_Mean', 'DN_Spread_Std']

features_with_short_names = catch22_all(data, short_names=True)
print(features_with_short_names['short_names'])
# Output: ['mode_5', 'mode_10', 'acf_timescale', ..., 'low_freq_power', 'forecast_error']

```

***

***

## Individual Feature Methods

The `catch22` module provides direct access to the individual feature extraction methods implemented in C. These methods can be called directly using `pycatch22.{name}`, where `{name}` is the name of the feature method (as given by the **long** name in the [table of features](../feature-descriptions/feature-overview-table.md)).&#x20;

### Syntax

```python
pycatch22.DN_Mean(data)
```

### Parameters

| Parameter | Type                                                                                                                             | Description             |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `data`    | ([list](https://docs.python.org/3/library/stdtypes.html#list) or [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)) | Input time-series data. |

### Return Values

| Value     | Type                                                                                                                             | Description                                                                          |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `feature` | ([Int](https://docs.python.org/3/library/functions.html#int) or [Float](https://docs.python.org/3/library/functions.html#float)) | Individual feature value. Either integer or float depending on the feature method.   |

### Example Usage

```python
import numpy as np
from pycatch22 import DN_Mean

data = list(np.random.randn(100))
individual_feature = DN_Mean(data)
print(individual_feature)
# Output: -0.11125506527054776
```

***

