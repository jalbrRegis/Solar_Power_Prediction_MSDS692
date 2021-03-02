# Solar Power Prediction
## Regis University MSDS 692
## Joseph Albrechta
## March 3, 2021

## Introduction
Solar power has long been a source for clean energy for residential and commercial properties but weighing the high installation cost against long term gains can be speculative and cumbersome. For this project weather data and solar data will be used combined with locational data to predict solar power generation. One can use the application to select a prospective area for solar installation and then predict how much energy can be produced. Homeowners and businesses can use the application to determine their brake even point and can use the predictions to understand how much energy they can expect from solar. With many corporations having “Go Green” initiatives it can also assist in estimating the cost of going carbon neutral. In all, estimating solar output would be helpful for anyone looking to reduce their carbon footprint but also ensuring energy and cost needs will be met in the future. 

## Environment Setup
For this project I used miniconda to manage my environment. Since this project was done in google Colab I installed everything with Bash and pip commands. The main reason this was done is to install the solar simulator package. The default Colab enviroment does not have the correct version of python to work with the solar simulator package and requires other packages to work correctly.

## Packages

Most of these packages are standard for data science including pandas, numpy, keras, sklearn, and tensorflow. Other packages are added in for statistical operations. The PySAM package is maintained by the (NREL) National Solar Radiation Database. This is package that will convert the radation data into solar energy data.

## The Data
The data used from this project is from the National Solar Radiation Database. It is downloaded with an API and contains yearly data on solar radion plus other meteroloical data. Geospatial data is also provided so a location can be selected and all relivant data can be downloaded. For this project I chose a location in Colorado, but any location can be chosen and the model retrained to predict solar output. The API limits the amount of data you can pull at a time so its best to do one location at a time. Also, pulling historical data can time out as such I found that pulling 5 years at a time was the thr max allowable at a time. The following shows an example of the meta data containing geospatial information as well as a sample of the data pulled:

### Meta Data
![image](https://user-images.githubusercontent.com/51838209/109591644-1f7a7180-7acb-11eb-9abf-d7a4130a50e7.png)

### Solar Radiation Data
![image](https://user-images.githubusercontent.com/51838209/109591570-f9ed6800-7aca-11eb-8d10-6d8af97712c0.png)

## The Solar Power Simulator
To convert the solar radion inputs to solar power a simulator package by the NREL is used. This solar package takes in geospatial data combinded with DNI, DHI, Temperature, and wind to simulate solar power produced in kilo-watts. Other inputs to the simulator are the type of solar panel, tilt, system capacity, ground coverage etc. For this project I used settings to represent panels afixed to a residential roof but all that could be changed in the simulator.

## EDA Data Discovery

