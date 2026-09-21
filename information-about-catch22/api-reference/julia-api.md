---
description: >-
  This is the class and function reference of catch22.jl. Please refer to the
  user guide for further details as the class and function specifications may
  not be sufficient to give full context.
cover: >-
  https://images.unsplash.com/photo-1557324232-b8917d3c3dcb?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw5fHxKdWxpYSUyMGNvZGV8ZW58MHx8fHwxNzEwMjg4MTk1fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Julia API

## Catch22 Module

The `Catch22` module provides functionality for extracting the _catch22_ feature set from time series data.

### Types

#### FeatureSet

```julia
FeatureSet(funcs::Vector{<:Function}, names::Vector{Symbol}, keywords::Vector{Vector{String}}, descriptions::Vector{String})
```

A collection of feature extraction functions, their names, keywords, and descriptions.

#### SuperFeatureSet

```julia
SuperFeatureSet(funcs::Vector{<:Function}, names::Vector{Symbol}, descriptions::Vector{String}, keywords::Vector{Vector{String}}, transform::Function)
```

A hierarchichal`FeatureSet` in which one or more evaluated features are non-independent, relying on some (potentially) common preprocessing transformations; e.g. the partial autocorrelation at successive lags. Feature sets of this type detect any common preprocessing transformations, as to only perform each calculation once. `Catch22.jl`uses this to compute the zscore transformation, required by all _catch22_ features.

Further details on the custom types used by the `Catch22.jl` package can be found at [`TimeseriesFeatures.jl`](https://github.com/brendanjohnharris/TimeseriesFeatures.jl).

### Functions

#### catch22

```julia
catch22(𝐱::Vector)
catch22(X::Array)
catch22[featurename::Symbol](X::Array)
```

Evaluate all _catch22_ features for a time series vector `𝐱` or the columns of an array `X`. Can be indexed by feature names (as symbols) to return a subset of the available features.

#### catch24

```
catch24(𝐱::Vector)
catch24(X::Array)
```

Evaluate all _catch22_ features along with the mean (`DN_Mean`) and standard deviation (`DN_Spread_Std`) for a time series vector `𝐱` or the columns of an array `X`.

#### c22

```julia
c22(𝐱::Vector)
c22(X::Array)
```

Evaluate all _catch22_ features with **shortened** names for a time series vector `𝐱` or the columns of an array `X`.

#### c24

```
c24(𝐱::Vector)
c24(X::Array)
```

Evaluate all _catch22_ features with **shortened** names, along with the mean (`mean`) and standard deviation (`std`) for a time series vector `𝐱` or the columns of an array `X`.

#### Individual Feature Methods

The `Catch22` module also provides direct access to the individual feature extraction methods. These methods can be called directly using `Catch22.{name}`, where `{name}` is the name of the feature method (as given by the **long** name in the [table of features](../feature-descriptions/feature-overview-table.md)).&#x20;

| Method                | Parameters    | Return Type | Description                                                                |
| --------------------- | ------------- | ----------- | -------------------------------------------------------------------------- |
| `DN_Mean`             | `𝐱` (Vector) | Float64     | Computes the mean of the input time series.                                |
| `DN_Spread_Std`       | `𝐱` (Vector) | Float64     | Computes the standard deviation of the input time series.                  |
| `DN_HistogramMode_5`  | `𝐱` (Vector) | Float64     | Computes the mode of the input time series using a histogram with 5 bins.  |
| `DN_HistogramMode_10` | `𝐱` (Vector) | Float64     | Computes the mode of the input time series using a histogram with 10 bins. |
| ...                   | ...           | ...         | ...                                                                        |

***

## Example Usage

```julia
using Catch22

# Load test data
𝐱 = Catch22.testdata[:test]

# Evaluate all catch22 features
features = catch22(𝐱)

# Evaluate specific feature
mode_5 = DN_HistogramMode_5(𝐱)

# Evaluate catch24 features (with mean and std)
features_with_stats = catch24(𝐱)

# Evaluate features with shortened names
short_features = c22(𝐱)

```

***
