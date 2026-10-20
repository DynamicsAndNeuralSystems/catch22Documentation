---
description: A detailed usage guide for using compiled catch22 executables.
cover: >-
  https://images.unsplash.com/photo-1569605803663-e9337d901ff9?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxiaW5hcnl8ZW58MHx8fHwxNzExMzkyOTExfDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# C-compiled

## C-compiled Usage Guide

Select a card below to access C-compiled-specific usage information.&#x20;

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Installation</strong></td><td></td><td><a href="../.gitbook/assets/install_light.png">install_light.png</a></td><td><a href="c-compiled.md#installation">#installation</a></td></tr><tr><td></td><td align="center"><strong>Getting started</strong></td><td></td><td><a href="../.gitbook/assets/getting_started_light.png">getting_started_light.png</a></td><td><a href="c-compiled.md#getting-started-basic-usage">#getting-started-basic-usage</a></td></tr><tr><td></td><td align="center"><strong>Advanced usage</strong></td><td></td><td><a href="../.gitbook/assets/adv_usage_light.png">adv_usage_light.png</a></td><td><a href="c-compiled.md#advanced-usage">#advanced-usage</a></td></tr><tr><td></td><td align="center"><strong>Frequently Asked Questions</strong></td><td></td><td><a href="../.gitbook/assets/FAQ_light.png">FAQ_light.png</a></td><td><a href="c-compiled.md#faq">#faq</a></td></tr></tbody></table>

***

***

## Installation

***

{% hint style="info" %}
**Note:** In order to compile the C source files, you will need to ensure you have `GCC` installed on your system. **MacOS** users can follow [these instructions ](http://cs.millersville.edu/~gzoppetti/InstallingGccMac.html)to install GCC using homebrew. In most **Linux** distributions, GCC comes pre-installed, however if it is unavailable, it can be easily installed using `sudo apt-get install gcc.` **Windows** users can follow [these instructions](https://www.msys2.org) to install the MSYS2 platform which includes support for `GCC.`
{% endhint %}

1. To get started with using _catch22_, clone the repository to a location of your choice using the following command in `Bash/CMD`:

```bash
git clone https://github.com/DynamicsAndNeuralSystems/catch22.git
```

2. From the cloned repository (`catch22`), compile the C source files in `catch22/C` into a single executable named `run_features`.
   Choose your operating system from one of the tabs below to access OS-specific instructions:

{% tabs %}
{% tab title="Windows" %}
1) After having installed on your system, launch the UCRT64 environment by running `UCRT64.exe` (not `MSYS2.exe`). You will see a terminal that looks like this:

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption><p>emulated terminal</p></figcaption></figure>

2. Install the `mingw-w64 GCC` to start compiling _catch22._ You can do this by running the following command:

```bash
pacman -S mingw-w64-ucrt-x86_64-gcc
```

3. Now you can call `gcc` as you would from the MacOS or Linux platforms to compile the _catch22_ source files.
   First navigate to the cloned repository from within the emulated environment:

```bash
$ cd /c/{your_fpath_in_cdrive}/catch22
```

Next, compile the source code using the following `gcc` command:

```bash
gcc -O2 C/*.c -o run_features -lm
```

If the source code compiled successfully, you should now see an executable called `run_features.exe` in your folder. If you wish, you can copy this executable to a new folder to run _catch22_ with your time series data. See our usage guide for more information.&#x20;
{% endtab %}

{% tab title="MacOS/Linux" %}
From the cloned repository (`catch22`), compile the source code using GCC by running the following command in bash:

<pre class="language-bash"><code class="lang-bash"><strong>gcc -O2 C/*.c -o run_features -lm
</strong></code></pre>
{% endtab %}
{% endtabs %}

***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

The compiled `run_features` program takes **one time series at a time**.
Each input file (e.g., `.txt`) contains **one univariate time series**, with one observation per line, e.g., 12 lines for a time series of length 12:

{% code title="testShort.txt" %}
```
-0.89094
-0.86099
-0.82438
-0.78214
-0.73573
-0.68691
-0.63754
-0.58937
-0.54342
-0.50044
-0.46082
-0.42469
```
{% endcode %}

The file is read until the first entry that is not a number, so it should contain no header or other text.

### Running the executable

To use _catch22_, launch a terminal and run:

```bash
$ ./run_features [--catch22 | --catch24] [--no-mask] <infile> [<outfile>]
```

Note, this will be an executable (e.g., `run_features.exe`) for Windows users.
Specifying `<outfile>` is optional: without it, the results are printed to `stdout`.
Use `--catch22` for the 22 features or `--catch24` to add the mean and standard deviation (see [_Catch24_](c-compiled.md#id-1.-catch24) below).
If you give neither, `run_features` asks interactively (enter `0` for _catch22_ or `1` for _catch24_), so always pass one of them in scripts.

As an example, here we run _catch22_ on a simple sinusoidal time series with 5000 observations (i.e., length T = 5000), [`testSinusoid.txt`](https://github.com/DynamicsAndNeuralSystems/catch22/blob/main/testData/basic/testSinusoid.txt) from the repository's `testData/basic` folder:

```bash
$ ./run_features --catch22 testSinusoid.txt testSinusoidOutput.txt
```

Here we tell _catch22_ to store the results of the calculation in a`.txt` file called "testSinusoidOutput.txt".

### Expected output

Each line of the output (either saved or printed to the terminal) corresponds to one feature; the three comma-separated entries per line correspond to feature value, feature name and execution time in milliseconds in the following format:

```
feature value, feature name, execution time (ms)
```

For example, `testSinusoidOutput.txt` begins and ends:

```
1.27292807660985, DN_HistogramMode_5, 0.037000
-1.12758607744598, DN_HistogramMode_10, 0.041000
-0.13777244551090, DN_OutlierInclude_p_001_mdrmd, 0.076000
...
0.75510204081633, SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1, 0.238000
```

The features are listed in the order of the [feature overview table](../information-about-catch22/feature-descriptions/feature-overview-table.md), followed by a blank line.
Execution times depend on your machine.

{% hint style="info" %}
**catch22 v1.0** changed the values of most features (to match _hctsa_ v3.0.0 exactly) and renamed several of them, so outputs are not comparable with those of v0.x.
See the [feature descriptions](../information-about-catch22/feature-descriptions/) for what changed.
{% endhint %}

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

***

## Advanced Usage

Ready to explore some of the additional functionality of _catch22_?&#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features.
To compute _catch24_ with the `run_features` executable, pass the `--catch24` flag:

```bash
$ ./run_features --catch24 testSinusoid.txt testSinusoidOutput.txt
```

The mean (`DN_mean`) and standard deviation (`DN_Spread_std`) are added as the last two lines of the output.
{% endtab %}
{% endtabs %}



### 2. Masking

Some features cannot be estimated for some time series, e.g., any feature of a constant time series, or `dfa` for a time series shorter than 79 samples.
By default, such features are [masked](../information-about-catch22/feature-descriptions/feature-overview-table.md#masking): set to a fixed value at the appropriate end of the feature's range, so that 22 real numbers are always returned.

{% tabs %}
{% tab title="Masking" %}
For example, `testShort.txt` (above) has only 12 samples, too few for `rs_range` and `dfa`, which are masked to 1:

```bash
$ ./run_features --catch22 testShort.txt
...
1.00000000000000, SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1, 0.001000
1.00000000000000, SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1, 0.000000
```

Pass `--no-mask` to get `nan` instead:

```bash
$ ./run_features --catch22 --no-mask testShort.txt
...
nan, SC_FluctAnal_2_rsrangefit_50_1_logi_prop_r1, 0.001000
nan, SC_FluctAnal_2_dfa_50_1_2_logi_prop_r1, 0.000000
```

A time series that is empty or contains `NaN` or `Inf` gives `nan` for every feature (it is never masked), with a warning printed to `stderr`.
{% endtab %}
{% endtabs %}



### 3. Listing the Features

{% tabs %}
{% tab title="Listing the Features" %}
`run_features --list` prints a tab-separated table of the 24 features, in output order: each feature's name, short name, name before v1.0, and the _hctsa_ code that reproduces it:

```bash
$ ./run_features --list
name	short_name	legacy_name	hctsa_call
DN_HistogramMode_5	mode_5	DN_HistogramMode_5	DN_HistogramMode(x_z,5,true,false)
...
CO_FirstCrossing_acf_1e_pointOfCrossing	acf_timescale	CO_f1ecac	CO_FirstCrossing(x_z,'ac',1/exp(1),'both').pointOfCrossing
...
```

Use the `legacy_name` column to match v1.0 outputs to those of earlier versions (whose values differ).
The same table is in [`featureList.txt`](https://github.com/DynamicsAndNeuralSystems/catch22/blob/main/featureList.txt) in the repository.
{% endtab %}
{% endtabs %}



### 4. Multiple Time Series

A typical analysis may involve more than one time series (e.g., classification). To run _catch22_ on multiple univariate time series, do the following:

{% tabs %}
{% tab title="Multiple Time Series" %}
1. Copy the script `C/runAllTS.sh` from the repository into the folder containing your compiled `run_features` (the script calls `run_features` from its own folder).
   Put your time series - one file for each - into a folder `timeSeries` in the folder you run the script from:

```
📂 timeSeries/
├─ 📜ts_data1.txt <- contains one univariate time series
├─ 📜ts_data2.txt
├─ 📜ts_data3.txt
📜 run_features
📜 runAllTS.sh
```

2. Change the permissions of `runAllTS.sh` to executable by calling `chmod 755 runAllTS.sh:`

```bash
$ chmod 755 runAllTS.sh
```

3. Call the script using the following command to compute _catch22_ features on each of the time series (add `-s 1` for _catch24_):

```bash
$ ./runAllTS.sh
```

The output (one output file per input file) will be written to a folder `featureOutput`:

```
📂 timeSeries/
├─ 📜ts_data1.txt
├─ 📜ts_data2.txt
├─ 📜ts_data3.txt
📂 featureOutput/ <- new folder containing results
├─ 📜ts_data1output.txt <- contains 22 lines, each corresp. to a feature output
├─ 📜ts_data2output.txt
├─ 📜ts_data3output.txt
📜 run_features
📜 runAllTS.sh
```

Run `./runAllTS.sh -h` for options to set the input and output folders (`-i`, `-o`) and the suffix of the output file names (`-a`).
{% endtab %}
{% endtabs %}



### 5. Calling _catch22_ from Your Own C Code

To compute the features inside your own program, include `catch22.h` and call `catch22_compute`, the function that every _catch22_ wrapper (Python, MATLAB) calls.

{% tabs %}
{% tab title="C API" %}
```c
#include <stdio.h>
#include "catch22.h"

int main(void)
{
    const double x[] = {0.1, 0.4, 0.3, 0.9, 0.5, 0.2, 0.7, 0.6, 0.8, 0.3};
    const int n = sizeof x / sizeof x[0];
    const int catch24 = 0;   // 1 to add the mean and standard deviation
    const int mask_nans = 1; // 0 to get NaN for features that cannot be estimated

    double out[CATCH24_N_FEATURES];
    int masked[CATCH24_N_FEATURES];
    if (catch22_compute(x, n, catch24, mask_nans, out, masked) != CATCH22_OK) {
        fprintf(stderr, "The time series is empty or contains NaN/Inf\n");
        return 1;
    }
    for (int i = 0; i < catch22_n_features(catch24); i++) {
        printf("%-20s %.6g%s\n", catch22_short_name(i), out[i], masked[i] ? " (masked)" : "");
    }
    return 0;
}
```

Compile it together with the _catch22_ sources, leaving out `main.c` (which contains the `run_features` program):

```bash
gcc -O2 -Icatch22/C my_program.c $(ls catch22/C/*.c | grep -v main.c) -o my_program -lm
```

Running `./my_program` gives:

```
mode_5               -0.526649
mode_10              -0.67712
outlier_timing_pos   0.4
outlier_timing_neg   -0.3
acf_timescale        0.559617
...
embedding_dist       0.43659
rs_range             1 (masked)
dfa                  1 (masked)
```

`catch22_compute(y, n, catch24, mask_nans, out, masked)` writes the 22 (or 24) feature values of `y[0..n-1]` into `out`, and sets `masked[i]` to 1 for each masked value (pass `NULL` for `masked` if you don't need it).
It returns `CATCH22_OK`, or `CATCH22_BAD_INPUT` if the time series is empty or contains `NaN` or `Inf` (all outputs are then `NaN`).
`catch22_n_features(catch24)` gives the number of features, and `catch22_feature_name(i)`, `catch22_short_name(i)`, `catch22_legacy_name(i)` (the name before v1.0) and `catch22_hctsa_call(i)` describe feature `i`.
`catch22_compute_timed` also returns the computation time of each feature.
See [`C/catch22.h`](https://github.com/DynamicsAndNeuralSystems/catch22/blob/main/C/catch22.h) for details.
{% endtab %}
{% endtabs %}



***

***

## FAQ

***

Click one of the expandable tabs below to explore commonly asked questions about _catch22_:

<details>

<summary>How can I submit a bug report for <em>catch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/DynamicsAndNeuralSystems/catch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

<details>

<summary>Why do my feature values differ from an earlier version of <em>catch22</em>?</summary>

From v1.0, each feature exactly reproduces a feature of _hctsa_ v3.0.0, which changed the values of most features, and several features were renamed.
Use `./run_features --list` to match the new names to the old ones, and see the [feature descriptions](../information-about-catch22/feature-descriptions/) for how each feature changed.
When reporting results, state the version of _catch22_ you used (see [Citing _catch22_](../welcome-to-catch22/citing-catch22.md)).

</details>

***
