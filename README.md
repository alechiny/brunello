# Predicting Vintage Rating of the Red Italian wine Brunello di Montalcino 

### Introduction
Brunello di Montalcino is a red wine produced in the area of Montalcino, in the province of Siena, Italy.  Brunello di Montalcino is an excellent wine, priced USD50.00 and up. Selected Riserva bottles of desirable vintage years can cost well over USD100.00.<br />
Brunello is produced with grapes of the Sangiovese variety, traditionally referred to as Brunello in this area.  The regulations governing the production of this wine are very strict.   The maximum production of grapes per hectare must be less than 8 tons/ha (approximately 52 hl/ha of wine). Rules dictates the date of the wine being released onto the market, which is January 1st of the fifth year after harvesting. During the five years of aging period, the wine must spend at least two years in wooden barrels and age at least four months in the bottle. The Riserva wine must age at least 6 months in the bottle, and is released a year later onto the market (https://www.consorziobrunellodimontalcino.it/).<br />
Montalcino has one of the warmest and driest climates in Tuscany with the grapes in the area ripening up to a week earlier than in nearby Montepulciano. It is the most arid Tuscan DOCG, receiving an average annual rainfall of around 700 mm, in contrast to the Chianti region which receives an average of 900 mm. As with all of the Northern Hemisphere, the north-facing slopes receive fewer hours of sunlight and are generally cooler than the south-facing slopes. Thus, vineyards planted on the north-facing slopes ripen more slowly and tend to produce wines that are racier and more aromatic. Vineyards on the southern and western slopes receive more intense exposure to sunlight and more maritime winds which produces wines with more power and complexity. The top producers in the area have vineyards on both slopes, and make use of a blend of both styles (https://en.wikipedia.org/wiki/Brunello_di_Montalcino).<br />
The best vintages take advantage of the right combination of rainfall, temperature, and sun radiation.  Since the vineyards are not irrigated, weather patters play an important role in vine quality.
Wine makers, vendors and experts produce vintage charts that can help consumers to understand the overall quality of a vintage and wines from different producers. Vintage charts can be star-based, with a range from one to five stars or be on  scale from zero to 100, with most wines scoring 50 or above.  Wine score for unreleased vintges is based on bottle tasting (https://www.wine-searcher.com/wine-scores). <br />  


### Objective
The objective of this research is to investigate which weather patterns, if any, can be correlated to wine quality.  The analysis of meteorological information can provide viticulturists with operational and forecasting tools for improving the management of vineyards.  Researchers at the University of Florence, Italy conducted a similar work trying to correlate Italian wine quality with weather data available to the public. The study concluded that Results highlight strong relationships between meteorological conditions and wine quality. Higher-quality wines were obtained in the years characterized by a reduction in rainfall and high temperature patterns (https://www.ajevonline.org/content/57/3/339).<br />

### Existing Body of Work
Researchers at the University of Florence, Italy conducted a similar work trying to correlate Italian wine quality with weather data available to the public(https://www.ajevonline.org/content/57/3/339). The study concluded that Results highlight strong relationships between meteorological conditions and wine quality. Higher-quality wines were obtained in the years characterized by a reduction in rainfall and high temperature patterns.  For this study five different wines from central and norther Italy were used. Instead of regular data from weather station the team used a new methodology that they described as "... Past observations (from ground, satellite, soundings), generally characterized by spatial/temporal discontinuity, were reprocessed and combined with output of weather forecast models in order to derive a more comprehensive spatial/temporal description of the environment at a globallevel. This method is known as 'reanalysis' and makes possible the reconstruction of atmospheric analyses (and, consequently, of all meteorological variables) for the entire earth???s surface with spatial/temporal continuity".<br/>

### Data Sources
Precipitation and temperature data were downloaded from the website of 'Settore Idrologico e Geologico Regionale' (Regional Hydrological and Geological Office) of the Italian region of Tuscany (http://www.sir.toscana.it/consistenza-rete). The weather station of Radicofani (TOS11000061) , in the Siena province was selected becouse of it's proximity to Montalcino. Historic data were available for the period 1993 (incomplete year) through 2022.<br/>
Precipitation data a comma as decimal separator, this is common in many European countries.<br/>
Ratings for the vintage charts were copied from the website of the Consorzio del Vino Brunello di Montalcino (https://www.consorziobrunellodimontalcino.it/en/home/home), The Consortium that promotes and regulates the wine industry in Montalcino. The score is based on a 1 to 5 scale. Ratings were compared  to those available on wine journals for accuracy. For this purpose I scraped data from WineSpectator website (https://www.winespectator.com/vintage-charts/region/tuscany-brunello-di-montalcino) but the rating was not used in this work.<br/>

### Missing Values
For both, temperature and precipitation some data were missing.  The source doesn't mention anything about missing data. We can only assume that some date were not recoreded or went missing. In the line of codes below I will address the issue of the missing data. The missing date are for these periods (dd/mm/yyyy):

* 01/10/1994 through 19/01/1995 for both, temperature and precipitation
* 14/03/1995 through 21/05/1995 for both, temperature and precipitation
* 01/11/1997 through 21/01/1998 for temperature
* 01/11/1997 through 31/12/1997 for precipitation
* 25/04/1998 through 07/05/1998 for both, temperature and precipitation

Instead of dropping the missing values and having to drop all the years with incomplete data I will fill the missing data with values created by the interpolation between the data values that bookend the NaNs. I will do this under the assumption that weather values follow a linear pattern

### Data Manipulation
Columns were renamed so they could be easily identifie.  Values were transformed to the proper format and as datetime was created.  The datetime column was set as an index allowing to sort the values according to the needs.  The following new variables were created according to an extension work by Vercesi et al.(http://www.precision-farming.com/web/pdf/IA_2003_14.pdf):
* Mean temperature - average between max and minimum temperatures
* Delta - difference between max and minimum temperature. During the ripening process a wide difference between daily max and minimum temperature promotes quality
* Degree Days -  they are a measurement of heat units over time.  Living organisms have a temperature below which activity they are not active.  For grapewine this temperature is 10C.  Degree Days were calculated by subtracting 10C to average daily temperature.  If the value is below zero (the average temperature for that day is below 10C) the degree day will be zero.
* Sums and means of temperatures, precipitations, degree days for both, the month and the year.
New datasets to be ulizided for both EDA and modeling were created for monthly and annual summaries.<br/>

### EDA
Data were plotted to analize trends and to have insight over the conditions under which the plants are growing.  The EDA clearly show how the climate suits so well the growth of the grapes.  According to the Winkler Index, the Sangiovese variety requires between 1600 and 1800 degree days to thrive.  These requirements are well over the observed period.  Temperature deltas at the highest in the summer months and stay high in early fall, contributing to enhancing the quality of the grapes.  By analyzing the plots of both, years with great vintages and years with poor ones nothing striking emerged.  The most obvious patters seems to be rain distribution during the ripening period.  Heat maps didn't show any correlation that was ove 0.25.  Multiple scenarios were considered and the best correlations were observed in data taken from July to September, coinciding with the ripening process.  The best correlation was observed between score and precipitation, both monthly mean and monthly sum.    

### Modeling
A pipe line was setup containing the following models:
* LinearRegression()
* KNeighborsRegressor()
* DecisionTreeRegressor(random_state=42)
* BaggingRegressor()
* RandomForestRegressor(random_state=42)
* GradientBoostingRegressor(random_state=42)

Classifiers performed better on the training test but performed poorly of the test sets. All R2 scores were very low.  Models run on the datasets that showed better correlations did not stand out.<br/>
Keras was used to understand if neural networks could give better results than the models in the pipeline.  Unfortunately the predictions were not accurate and the R2 generated by the models were negative numbers.

### Conclusions
With the data I gathered I was unable to create a model that could successfully predict the vintage rating for the Brunello di Montalcino. <br/>
Some of the reasons that prevented achieving the goal could be:  
* Montalcino consistently produces good to excellent wines.  Too many scores are concentrated at the top, making it difficult for the model to identify any significant difference 
* Montalcino has an exceptionally unique climate, more arid than the bordering areas.  Weather data may not fully describe the unique climate and how it affects wine production.
* Temperature and precipitation data are not enough to predict the quality of this particular wine
* Most quality wines are produced in areas where yield is capped to a maximize quality.  This practice can mitigate the effect of climatic factors on quality.

To succeed in finding a model that can predict vintage quality the following strategies could adopted:
* Use the same approach for another wine/s from a larger area.  Other varieties in different territories may have a stronger correlation with climatic data.  This is in line with the Italian study mentioned earlier.
* Identify which period during the year is more affected by weather and add more variables. 
* Include chemical analysis.
* Analyze data coming from precision agriculture, especially those that monitor the ripening process and soil properties.




