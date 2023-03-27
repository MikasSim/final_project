# Mikas Šimoliūnas Code Academy CADS final project

Hello, this is my final project where i tackle the problem of forecasting medicine quantity.
The problem is to forecast monthly data using Facebook Prophet and provide the business with a way to export the forecast.


- [Mikas Šimoliūnas Code Academy CADS final project](#mikas-šimoliūnas-code-academy-cads-final-project)
  - [Installation](#installation)
  - [EDA](#eda)
  - [Quickstart](#quickstart)
  - [Overview](#overview)
    - [data](#data)
    - [modules](#modules)
    - [nb\_eda](#nb_eda)
    - [Main.ipynb](#mainipynb)
  - [forecaster module methods](#forecaster-module-methods)
    - [retrieve\_data()](#retrieve_data)
    - [fit\_predict\_model(\*params):](#fit_predict_modelparams)
    - [plot\_prediction(model, forecast)](#plot_predictionmodel-forecast)
    - [detect\_anomalies(forecast)](#detect_anomaliesforecast)
    - [remove\_anomalies(dataframe, forecast)](#remove_anomaliesdataframe-forecast)
    - [evaluate\_model(model)](#evaluate_modelmodel)
    - [hyper\_tuner(dataframe,params)](#hyper_tunerdataframeparams)


## Installation

To install and run the project you will need the following:

```bash
python 3.11+
Visual studio c++ build tools
```

Create a virtual environment using the `requirements.txt` file.
The setup was tested on a Windows machine and theres potencial troubleshooting with the installation.

## EDA

You can find the EDA notebook in `nb_eda` folder.

## Quickstart

The program is tailored to this specific case and **IS NOT** a dynamic forecasting tool.

The data provided has to be in the given form:
| year | month | quantity |
| --- | --- | --- |
| int or varchar(4) | int or varchar(4) | int, float or real |

an example table of data would look like:

| year | month | quantity |
| ---- | ----- | -------- |
| 2020 | 1     | 1500     |
| 2020 | 2     | 4000     |
| 2020 | 5     | 7500     |
| 2020 | 10    | 10000    |

The data provided has to be in `.csv` format.

It automatically rounds and converts any decimal value to int.

Store the data in the `data` folder.

Data is provided so just open `Main.ipynb` and run the following cells to execute the forecast on the given data.

## Overview

### data

The folder that supplies data to the forecasting, hyper tuner and eda programs.

### modules

Houses the `forecaster.py` module that is used in `Main.ipynb`

### nb_eda

Folder that stores the eda part of the project and the tuner.
Exploratory data analysis can be found in `eda.ipynb` Every cell in eda is explained in a concise manner.
Hyper parameter tuning can be found in `Hyper_tuner.ipynb` Every cell in hyper tuner is explained in a concise manner.

### Main.ipynb

The main file of the project that overviews the functionality of the program and allows to have a bird's eye view of the functionality.

## forecaster module methods

It's the module that uses functions written in both EDA and hyper tuner stored in a single file for easy access and a quick documentation run.

### retrieve_data()

- returns pandas dataframe with transformed data.

### fit_predict_model(*params):
`fit_predict_model(dataframe, changepoint_prior_scale, seasonality_prior_scale, changepoint_range, periods)`
- Expects at least a dataframe and can run on default values and provide a decent forecast.
- The features are explained in more detail and can be found at [facebook.github.io/prophet](https://facebook.github.io/prophet/docs/trend_changepoints.html)

| feature                 | expected value                                         | default |
| ----------------------- | ------------------------------------------------------ | ------- |
| dataframe               | pandas dataframe object with features **ds** and **y** | None    |
| changepoint_prior_scale | int [0,1]                                              | 0.05    |
| seasonality_prior_scale | int                                                    | 10      |
| changepoint_range       | int [0,1]                                              | 0.8     |
| periods                 | int                                                    | 60      |

Returns: tuple

- Forecast (dataframe table)
- Model (Prophet object)

### plot_prediction(model, forecast)

- Expects Prophet model and forecast dataframe.<br>

Outputs a graph with confidence intervals, actual datapoints and the future forecast.

### detect_anomalies(forecast)

- Expects a forecast dataframe.

Returns: forecast dataframe with anomaly and importance features.

### remove_anomalies(dataframe, forecast)

- Expects dataframe and a forecast.

Returns the clipped mask of the dataframe without the anomalous data.

### evaluate_model(model)

- Expects Facebook prophet model.

Returns: Prints mape, mdape and smape metrics using cross validation over monthly data.

### hyper_tuner(dataframe,params)

- Expects dataframe and dict like param grid.
- Cross-validation takes some time so theres a tqdm progress bar to track progress.

```bash
# params dict example.
params = {
    'changepoint_prior_scale':[0.01,0.1,0.25,0.5],
    'seasonality_prior_scale':[0.01,0.1,0.5,1.0,5.0,10.0],
    'changepoint_range':[0.8,0.95]
}
```

Returns: Dict-like output of the best parameters.<br>
**`Note:`** if you want to fit the data onto the fit_predict_model method use the following syntax:

```bash
best = fc.hyper_tuner(data, params)
forecast, model = fc.fit_predict_model(data, *best.values())
```
