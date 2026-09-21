---
description: >-
  This is the class and function reference of Rcatch22. Please refer to the user
  guide for further details as the class and function specifications may not be
  sufficient to give full context on their
cover: >-
  https://images.unsplash.com/photo-1615525137689-198778541af6?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw1fHxSJTIwY29kZXxlbnwwfHx8fDE3MTAyODgyMTR8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# R API

The `Rcatch22` package provides functionality for extracting the _catch22_ feature set from time series data.

## Functions

### <mark style="color:purple;">catch22\_all</mark>

Automatically run every time-series feature calculation included in the _catch22_ set.

```r
catch22_all(data, catch24 = FALSE)
```

#### Parameters

| Parameter | Type           | Description                                                                                                             |
| --------- | -------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `data`    | numeric vector | A numerical vector representing the input time series.                                                                  |
| `catch24` | logical        | A logical value indicating whether to include the mean and standard deviation features (_catch24_). Default is `FALSE`. |

#### Returns

A data frame containing the calculated feature values. The data frame has two columns: `names` (feature names) and `values` (corresponding feature values):

| Key      | Type | Description                               |
| -------- | ---- | ----------------------------------------- |
| `names`  | list | A list of catch22/catch24 feature names.  |
| `values` | list | Corresponding list of feature values.     |

#### Example Usage

<pre class="language-r"><code class="lang-r"><strong>data &#x3C;- rnorm(100)
</strong><strong>
</strong><strong>% features with standard catch22
</strong>features &#x3C;- catch22_all(data)

% features with catch24
features_with_catch24 &#x3C;- catch22_all(data, catch24 = TRUE)
</code></pre>

***

### Individual Feature Methods

The `Rcatch22` package provides direct access to the individual feature extraction methods implemented in C. These methods can be called directly using their respective function names (as given by the **long** name in the [table of features](../feature-descriptions/feature-overview-table.md)):

#### Syntax

```
% example of a single feature, replace with feature name of interest
DN_HistogramMode_5(data)
```

#### Parameters

| Parameter | Type           | Description                                            |
| --------- | -------------- | ------------------------------------------------------ |
| `data`    | numeric vector | A numerical vector representing the input time series. |

#### Returns

| Value   | Type   | Description                    |
| ------- | ------ | ------------------------------ |
| `value` | double | Corresponding feature output.  |

#### Example Usage

```r
data <- rnorm(100)
mode_5 <- DN_HistogramMode_5(data)
mean_value <- DN_Mean(data)
```

***
