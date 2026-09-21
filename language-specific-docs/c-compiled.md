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

2. Navigate to the folder called `c` in the cloned repository (`catch22/C`) and compile the C source files into a single executable named `run_features.` Choose your operating system from one of the tabs below to access OS-specific instructions:

{% tabs %}
{% tab title="Windows" %}
1) After having installed on your system, launch the UCRT64 environment by running `UCRT64.exe` (not `MSYS2.exe`). You will see a terminal that looks like this:

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption><p>emulated terminal</p></figcaption></figure>

2. Install the `mingw-w64 GCC` to start compiling _catch22._ You can do this by running the following command:

```bash
pacman -S mingw-w64-ucrt-x86_64-gcc
```

3. Now you can call `gcc` as you would from the MacOS or Linux platforms to compile the _catch22_ source files. First navigate to the folder containing the C source files from within the emulated environment:

```bash
$ cd /c/{your_fpath_in_cdrive}/catch22/c
```

Next, compile the source code using the following `gcc` command:

```
gcc -o run_features main.c CO_AutoCorr.c DN_HistogramMode_10.c DN_HistogramMode_5.c DN_OutlierInclude.c FC_LocalSimple.c IN_AutoMutualInfoStats.c MD_hrv.c PD_PeriodicityWang.c SB_BinaryStats.c SB_CoarseGrain.c SB_MotifThree.c SB_TransitionMatrix.c SC_FluctAnal.c SP_Summaries.c DN_Mean.c DN_Spread_Std.c butterworth.c fft.c helper_functions.c histcounts.c splinefit.c stats.c -lm
```

If the source code compiled successfully, you should now see an executable called `run_features.exe` in your folder. If you wish, you can copy this executable to a new folder to run _catch22_ with your time series data. See our usage guide for more information.&#x20;
{% endtab %}

{% tab title="MacOS/Linux" %}
Compile the source code using GCC by running the following command in bash:

<pre class="language-bash"><code class="lang-bash"><strong>gcc -o run_features *.c
</strong></code></pre>
{% endtab %}
{% endtabs %}



***

## Getting Started: Basic Usage

Here we outline how you can jump straight into computing _catch22_ features on your time series data with a basic usage example.

### Expected Input

The compiled `run_features` program only takes **one time series at a time**. Time series data are expected to be in the `.txt` or `.csv` file format. Each file contains **one univariate time series** and each line in the file corresponds to a single time series observation i.e., 12 lines for a time series of length 12:

{% code title="time_series_example.txt" %}
```python
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

### Running the executable

To use _catch22_, launch a terminal and run:

```bash
$ ./run_features <infile.txt> [outfile]
```

Note, this will be an executable (e.g., run\_features.exe) for Windows users. Here, specifying `<outfile>` is optional. If you do not wish to supply a file for saving the outputs, _catch22_ will print to `stdout` by default.&#x20;

As an example, here we run _catch22_ on a simple sinusoidal time series with 5000 observations (i.e., length T = 5000), provided as a `.txt` or `.csv` file:

```bash
$ ./run_features testSinusoid.txt testSinusoidOutput.txt
# enter either 0 for catch22 or 1 for catch24
```

Here we tell _catch22_ to store the results of the calculation in a`.txt` file called "testSinusoidOutput.txt". **Note** that we can also save the results as a comma separated values (`.csv`) file.&#x20;

### Expected output

Each line of the output (either saved or printed to the terminal) corresponds to one feature; the three comma-separated entries per line correspond to feature value, feature name and execution time in milliseconds in the following format:

```
feature value, feature name, execution time (ms)
```

For example:

```
0.29910714285714, CO_Embed2_Basic_tau.incircle_1, 0.341000
0.57589285714286, CO_Embed2_Basic_tau.incircle_2, 0.296000
...
```

And that's it! You now have all the knowledge you need to apply _catch22_ to your time series data. To access some of the additional functionality of _catch22_, keep reading for advanced usage tips and examples.

***

***

## Advanced Usage

Ready to explore some of the additional functionality of _catch22_?&#x20;

### 1. _Catch24_

If the location and spread of the raw time series distribution may be important for your application, you can enable _catch24._

{% tabs %}
{% tab title="Catch24" %}
_Catch24_ is an extension of the original _catch22_ feature set to include **mean** and **standard deviation,** yielding a total of **24** time series features. To access _catch24_, when using the `run_features` executable, the user will be prompted to enter either `0` (_catch22_) or `1` (_catch24_).
{% endtab %}
{% endtabs %}



### 2. Multiple Time Series

A typical analysis may involve more than one time series (e.g., classification). To run _catch22_ on multiple univariate time series, do the following:

{% tabs %}
{% tab title="Multiple Time Series" %}
1. For multiple time series, put them - one file for each - into a folder `timeSeries`. Ensure that the script `runAllTs.sh` is in the same location as the`timeSeries` folder.&#x20;

```
📂 timeSeries/
├─ 📜ts_data1.txt <- contains one univariate time series
├─ 📜ts_data2.txt
├─ 📜ts_data3.txt
📜 runAllTs.sh
```

2. Change the permissions of `runAllTS.sh` to executable by calling `chmod 755 runAllTS.sh:`

```bash
$ chmod 755 runAllTS.sh
```

3. Call the executable using the following command to compute _catch22_ features on each of the time series:

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
📜 runAllTs.sh
```
{% endtab %}
{% endtabs %}



***

***

## FAQ

***

Click one of the expandable tabs below to explore commonly asked questions about _pycatch22_:

<details>

<summary>How can I submit a bug report for <em>pycatch22?</em> </summary>

If you would like to submit a bug report, you can access our Bug Tracker [here](https://github.com/DynamicsAndNeuralSystems/catch22/issues). For guidelines on submitting an ideal bug report, see our page on [contributing to _catch22_](../information-about-catch22/contributing-to-catch22/)_._&#x20;

</details>

***
