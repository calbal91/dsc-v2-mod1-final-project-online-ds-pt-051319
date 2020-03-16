# Analysis of the King County Housing Market


## The Objective
Use Linear Regression to predict the sale price of a house in King County, WA, based on its features.

## The Motivation
The King County housing market is worth over $11bn annually, with 60 properties being sold every day. Being able to predict a property’s value based on its characteristics allows a real estate consultancy to:
* Identify and invest in undervalued property
* Evaluate the profitability of renovation work done to properties
* Ensure that property is not sold below its intrinsic value

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/Seattle.jpg)

## The Technologies Used
* Pandas for Data Manipulation
* Matplotlib / Seaborn for Data Visualisation
* Statsmodels / Scikit Learn for Linear Regression

## The Process Overview
Having been provided with a dataset, we will need to inspect and clean it as required.
This will involve:
* Checking for (and potentially replacing) Null values
* Checking for outliers and removing as required
* Identifying and binning categorical values

The modelling process will then involve:
* Checking for multicollinearity, and removing features as required
* Transform data where required to encourage normal distributions of features
* Final selection of features
* Modelling using Scikit Learn

We should then verify our model using:
* Standard measures such as R-Squared
* Overfitting measures such as k-fold validation


## The EDA

### Data Wrangling
We first address missing spurious data, dropping columns that are mostly NAN values.

We can create histograms of each feature to check for outliers. We can trim any properties from the dataset that are vastly unrepresentitive of the property market (e.g. houses selling for more than $1m, or with more than 7 bedrooms, etc.)

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/Distributions.png)

There may also be hidden outliers, which are unrepresentitive, but may not show up if we look at the features in isolation. For example, properties may be very expensive relative to their size, but may have a price that might not look silly in and of itself. We can thus create new features, such as price per square foot, which will help us further narrow down the dataset.

We can also visualise non-obvious categorical variables using scatter plots:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/Scatters.png)

These will need to be one hot encoded ahead of modelling.

### Geography
One key visualisation that comes out of the EDA is a scatter plot of house latitude vs longitude. As expected, this produces a map.

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ScatterMap.png)

As for the effect of geography on price, we can see that there appears to be some kind a north/south divide, however this is not universal (the very North of the map is mostly blue, lower value properties, having been mostly purple slightly further South). Thus, we can not assume a linear relationship between price, and either of longitude or latitude.

However, it is clear that expensive properties are clustered. It may therefore be worth demarkating 'areas' within the dataset, based on the zipcode data. These can then be used as categories in a multivariate linear regression. Zipcode data is categoried (A-I) as per the zipcode map on the King County website: https://www.kingcounty.gov/services/gis/Maps/vmc/Boundaries.aspx

* A - Seattle, Shoreline, Lake Forest Park
* B - Kirkland, Kenmore, Bothell, Redmond, Woodinville
* C - Bellevue, Mercer Island, Newcastle
* D - Renton, Kent
* E - Burien, Normandy Park, Des Moines, SeaTac, Tukwilla, Vashon Island
* F - Federal Way, Auburn, Algona, Milton, Pacific
* G - Sammamish, Issaquah, Carnation, Duvall
* H - Covington, Maple Valley, Black Diamond, Enumclaw
* I - Snoquaimie, North Bend

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/KCZipCodes.jpg)

## The Model Building

To settle on the final features, we one-hot encode the categories as required, then perform some log transformations on a few variables to make them more 'normal' looking.

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/LogScaled.png)

We then go through an iterative process of removing features that have high levels of collinearity. This is necessary, since it is hard to seperate the effects of two features with high levels of collinearity - bad for the quality of the model.

Having done this, Scikit Learn generates a linear model with an R^2 of about 0.67 - in other words, the model can explain about two-thirds of the variance we observe in the dependent variable. We can see the general accuracy of the model's predictive capacity:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelPredictions.png)


## The Model In Action

By extracting the coefficients from the trained model, we can visualise how it predicts a house value in the following way:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelOutline.jpg)


We can then use the model to help us solve potential business cases pertaining to the King County housing market:

### Example 1

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase1.jpg)


### Example 2

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase2.jpg)


### Example 3

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase3.jpg)