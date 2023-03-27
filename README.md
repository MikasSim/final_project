# Mikas Šimoliūnas Code Academy CADS final project
 Hello, this is my final project where i tackle the problem of forecasting medicine quantity.
 The problem is to forecast monthly data using Facebook Prophet and provide the business with a way to export the forecast.

## Table Of Content

## Installation

To install and run the project you will need the following:

```bash
python 3.11+
Visual studio c++ build tools (for prophet)
```

Create a virtual environment using the `requirements.txt` file. 
The setup was tested on a Windows machine and theres potencial troubleshooting with the installation.

## Quickstart

Open `Main.ipynb` and run the following cells to execute the forecast on the given data.

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

## Forecaster module methods
The program is tailored to this specific case and **IS NOT** a dynamic forecasting tool.

The data provided has to be in the given form:
| year | month | quantity |
| --- | --- | --- |
| int or varchar(4) | int or varchar(4) | int, float or real | 