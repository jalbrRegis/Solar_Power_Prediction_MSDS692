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
For identifying the features I was interested in the selection process was fairly straighforward. The simulator designated the inputs of DNI, DHI, Temperature, and Wind Speed for the location selected so I was going to investigate these features and how they relate to each other and to the solar power they generate. I first inspected the values visualally by plotting a sample day of power generation. The following graph shows what a typical day on August 5th, 2013 looks like:

![image](https://user-images.githubusercontent.com/51838209/109593982-39b64e80-7acf-11eb-8978-5140d9a40c73.png)

In this chart you see another value GHI which is calculated from DNI and DHI. the solid red line indicates the power generated by our simulated solar array. As you can see it follows GHI closely but the relationship to the other values is not quite apparent. DNI seems to follow power generation the closest as a non-derived value. To see these features relationship at a more quantifyable level I contructed a coralation matrix of values show in the following image:

![image](https://user-images.githubusercontent.com/51838209/109594354-e0025400-7acf-11eb-9a8e-28553f102ae0.png)

Confirmed by our daily chart GHI has the highiest correlation with Solar Power (b'generation') with each of the input values to the simulator having some effect on the power generation it seemed they would all be valid in model predicting solar power. Common sense would say if they are an input to the simulator the are important to the value it produces.

## Time-Series Data Discovery

For our project we are trying top predict future solar power production so not only do we need to know what features to use but what time scale. I started out investingating weekly averages as they relate to total solar power and generated a graph to view the weekly averages over a span of 5 years. The result of the graph is as follows:

![image](https://user-images.githubusercontent.com/51838209/109595182-4045c580-7ad1-11eb-9f0f-55b5bcbfb3bd.png)

From the graph you can see on a weekly scale some of the features follow a yearly trend close than others. Temporature and DHI you can see the values vary by month in a more predicable way then the other variables. Still, it is the power generation that we are interested in and since the other values are inputs to the solar power simulator perhaps a univarate time-series should be tried first as its the simplest solution. First however, we must determine if the weekly values are stationary if they are to be used in a time series prediction. That brings us to the dickey-fuller test.

## Test for Stationary Data

The Dickey-Fuller test is used for time series data to rule out an overall trend in the data. In other words, we want our data overall stationary for better predictions. This can be tricky with weather data especially if there is an effect of climate change over the timespan. This trend could be apperant in monthly data not not so much in weekly or daily. Before performing the test on my weekly data I looked at the data over time grouped by day, week, month and year. The results are below:

![image](https://user-images.githubusercontent.com/51838209/109595760-5acc6e80-7ad2-11eb-95af-6cbfb66faae8.png)
