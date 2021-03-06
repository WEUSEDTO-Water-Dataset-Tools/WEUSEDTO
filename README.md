# Water E-data Tools
Water E-data Tools (WET) is an open water end use consumption dataset and data analytics tools, have been released to help researcher, water utilities and companies to test models and algorithms on real water consumption data. The dataset combines with some notebook python able to analyze high-resolution water data in order to provide several tools to manage raw data, compute statistical analysis, learn fixture usage and generate synthetic simulation models.  

* [The dataset](#dataset)
* [Data analysis](#analysis)
* [The API documentation](#api)
* [Usage examples](#examples)

<a name="dataset"></a>
## The dataset
The software repository contains 9 time-series of raw measures exported as csv files. They correspond to the fixtures monitored in the apartment used as case study and, to the whole-house aggregate consumption. 
Data gathered have 1s resolution for disaggregate time series and 10s for the aggregate measurements, spanning 1 year from March 2019-October 2020. WCD at fixture level are collected using an Internet of things (IoT) water end use monitoring system, as reported  in [[1]](#bib1)  and  [[2]](#bib2). WCD at household level are gathered using an ultrasonic water meter based on LoRa wireless transmission technology
.
<a name="analysis"></a>
## Data Analysis
Examples of analysis of water flow timeseries are shown in two notebook:
* [Shower Analysis](https://github.com/25sal/waterseries/blob/main/src/notebooks/shower.ipynb)
* [Washbasin Analysis](https://github.com/25sal/waterseries/blob/main/src/notebooks/washbasin.ipynb)

<a name="API"></a>
## API Documentation
The API allow for processing raw flow data and are organized according to four packages:
* *timeseries*: splits data into many short series, corresponding to different water usages. Provides methods to identify and filter overlays.
* *model*: support the statistical charactarization of water usages.
* *learning*: cluster and predict water consumption profile of usages.
* *simulation*: according to statistical properties and machine learning techniques simulates usages at larger scale.

API documentation can be found [here](https://25sal.github.io//waterseries/docs/html/).

<a name="examples"></a>
## Usage examples

Four python modules are provided to show how APIs can be used.
* split
* model
* learn
* simulate

### Split timeseries
This module uses the APIs to split a timeseries detecting several usages. It allows to filter overlays according to criteria embedded into the code. Two splitting algoritms can be chosen: "SimpleSplitter" and "Splitter". 

```bash
$python3 src/examples/split.py -h
usage: split.py [-h] [--nosplit] [--nofilter] [--alg SPLITALG] FIXTURE

complete example

positional arguments:
  FIXTURE         the basename of csv timeseries to be analyzed

optional arguments:
  -h, --help      show this help message and exit
  --nosplit       skip the split stage
  --nofilter      skip the filter stage
  --alg SPLITALG

```
### Generate models
This module compute statistics from splitted timeseries and generate three kinds (global, monthly, weekly) of statistical representation of user behaviour in terms of fixter usage. It also compute clusters of usage according to duration, consumed volume and maximum flow speed. It also compute, for each cluster an average water consumption profile using an spline approximation.
It starts from the splitting phase that can be skipped.

```bash
$ python3 src/examples/model.py 
usage: model.py [-h] [--nosplit] [--nofilter] [--nomodel] [--nocluster] [--nospline] [--splitalg {SimpleSplitter,Splitter}] FIXTURE
model.py: error: the following arguments are required: FIXTURE
```

### Learn
This module use machine learning techniques to learn the cluster of the water usage according to the datetime it starts.

```bash
$ python3 src/examples/learn.py 
usage: learn.py [-h] [--nocluster] [--nospline] FIXTURE
learn.py: error: the following arguments are required: FIXTURE
```

### Simulate
This module use simulate the water consumption of multiple users generating timeseries exploiting the statistical characterization of usages and the learned models

```bash
$ python3 src/examples/simulate.py 
usage: simulate.py [-h] [--smodel {global,monthly,weekly}] [--month {1,2,3,4,5,6,7,8,9,10,11,12}] [--weekday {0,1,2,3,4,5,6}] [--users USERS] FIXTURE
simulate.py: error: the following arguments are required: FIXTURE
```


<a name="bib1">[1]</a>   A. Di Mauro, A. Di Nardo, G. F. Santonastaso, S. Venticinque, An IoT system for monitoring and data collection of residential water end-use consumption, in:  Proceedings - International Conference on Computer Communications  and  Networks,  ICCCN,  Vol.  2019-July,  IEEE,  2019,pp. 1–6.

<a name="bib2">[2]</a> A. Di Mauro, A. Di Nardo, G. F. Santonastaso, S. Venticinque, Development of an IoT System for the Generation of a Database of Residential Water End-Use Consumption Time Series, Environmental Sciences Proceedings 2 (1) (2020)