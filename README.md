# Ames-Housing-Data
Price Prediction

## Ames Housing Data and Kaggle Challenge

---
### Problem Statement

Determining the essential coefficients and significance of variables to accurately price homes in Ames, Iowa; to effectively predict their eventual selling prices by means of the regularising Lasso Regression.
This is to ensure that houses that are being marketed can receive appropriate valuation, buyers and sellers can leverage on this model to make a informed decision. This model will predict the sale price of a house in Ames, Iowa, based on features selected by the Lasso algorithm.

Looking at the variables, it is clear that the variables do not account for income and purchasing powers of residents and total supply of other houses, which SalesPrices are inversely related towards.
Success is evaluated against the model has the highest R^2 numbers that are consistent throughout cross validation and train/test split, and minimized MSE error which can be verrified on the Kaggle Website.



---
### Executive Summary

Contents: Ames, Iwoa, Housing Data set. 
Data Documentation: http://jse.amstat.org/v19n3/decock/DataDocumentation.txt

- Inspecting the data
- EDA
- Pre-processing & Cleaning
- Data Gathering
- Exploratory Data Analysis
- Modelling
- Benchmarking
- Final Analysis

---
### My Process

#### EDA

The use of a heatmap enabled me to visually scan the data for visual color-sensing of all features correlation with each other. Many features were observed to have close to zero values of correlations with other features. It was noted that there were several white boxes that corresponded to null data in the respective features. Strong correlations exists between various features, but 'saleprice' was displayed the strongest correlation to many features. Top 10 features with the strongest correlation to 'saleprice' yielded possibly collinear results; garage area and basement area maybe overlapping features. residentials with 2 floors were not as common as those with just 1, but for those that did had a relatively large one. The middle of the year was the most common time that houses were sold/bought. It was also observed that some columns with numerical (int/float) type values in them, were categorical. 

---
#### Preprocessing

Given the data at hand and the correspending features gathered, a prediction model can be generated, and there was no requirement to exclude any features outside of PID and ID, which were unique to the residence as an identification form. All null data observed would be filled accordingly; certain columns/features contain null values as they represented a lack of the feature in its entirety (e.g. most house do not have a pool).
#### Gathering the Data

Of note, the kaggle submission requires for the full 879 entries in that data set. Hence, no rows in the test data set can be dropped.
The global datasets comes from the GitHub data repository for the 2019 Novel Coronavirus Visual Dashboard operated by the Johns Hopkins University Center for Systems Science and Engineering (JHU CSSE). This is also the source for the Kaggle competition dataset. In this part of the project, we go through with EDA on the data prepared.

Numerical data:
The data was collected from 2 sources:

Columns with more than 30% null values will be identified and possibly dropped to prevent accessing tempering when introducing randowm normalised valued later (most columns have less than 20% null values). However, should cross refencing with other columns and data documentation suggests that the null value is significant (e.g. null value for pool_qc is representative of the residence having no pool), the data will be changed to str'NA'. Null values were assigned to 0 for numeric data and 'null' for strings. 

Some outliers were identified during the EDA. The outliers seen in the 1st floor square footage vs sales price were addressed in the data documentation as true outliers and will be left in the data. The outlier in the garage year built vs sales price will be removed; insufficient data to correct the date error.
No API or loop was needed to collect the data.

Catergorical data:
#### EDA

Though the graphs are not drawn in relation to their quantities, barplots allowed for a visual scan between data of categorical features within a column in correlation with sales price. Amongst MS Zoning, the general zoning classification, floating village residentials observed the highest sale price, despite Iowa being a contral US state with few water bodies. It was observed that certain neighbourhoods also commanded a higher salesprice. The appropriate columns would be dummified for modelling.
The graphical plots enabled me to visually scan the data for possible relationsips between features. Top 10 countries with the most cases were different to those of deaths and recoveries. Scoping into ASEAN countries, we observe certain recovery for Brunei, Cambodia, Malaysia Thailand, Vietnam, with their efforts closest to "flattening the Curve". Countries with uncertain recovery include Laos and Singapore, with recovery rates not yet high enough but are hopeful to be "flattening". Burma (Myanmar), Indonesia, and Philippines showing very few recoveries or at a rate similar to those of deaths. Singapore data was also closely examine for age, nationality, means of transmission, etc. 

---
#### Modeling

Categorical variables were converted to dummies and the numeric data was scaled using scikit-learn StandardScaler. A linear regression model was built first but yielded extremely negative mean R^2 values. All the R^2 scores are negative in crossvalidation. The linear regression performed far worse than the baseline (naive model based on mean of saleprice) on the test sets. It is probably dramatically overfitting and the redundant variables are affecting the coefficients in weird ways. Following this, the ridge, lasso, and elastic net regressions were introduced for regularisation.

The ridge model was vastly better than the Linear Regression. There was likely so much multicollinearity in the data that "vanilla" regression overfits and had bogus coefficients on predictors. Ridge was able to manage the multicollinearity and get a good out-of-sample result.
3 regression technique were used to determine the best forecast for the COVID-19 spread; Linear Regression, Random Forest regression and eXtreme Gradient Boosting regression. Root Mean Squared Error was the chosen metric to judge the best performing model. in the experiment with the data i had gthered from the source url, the linear regression model performed worst on the training set but best on the test. On the Kaggle datasets, however, it was observed that random forest regressor was the best performing model. 

The lasso model performed way better than the Ridge, but similarly. Lasso deals primarily with the feature selection of valuable variables, eliminating ones that are not useful. This also takes care of multicollinearity, but in a different way: it chooses the "best" of the correlated variables and zero-out the other redundant ones. There may also have been useless variables in the data which it simply gets rid of entirely.

The Elastic Net performs very similarly to lasso, albiet just slighty poorer. This was to be expected given the l1_ration was (almost) 1 (full lasso).

---
#### Analysing Coefficients

Given that the Lasso model was the best performing regularisation model, anyalysing its coefficients will give us a better idea of the variables most strongly affect the sales price of a real estate, be it positively or otherwise.

Going through the coefficients, it is observed that elevators as a miscellaneous feature and clay tile as a roofing material has a negative correlation with salesprice. 

The rest of the top 20 absolute values for the coefficients have a positive correlation to the sales price. Amongst which, the following 3 collective features are the easiest to be targetted at achieving better sales prices:
- Square footage was a key feature in influencing sales price; ground living area and basement square footage showed the strongest positive relationship. (Though garage area also had a strong influence on sales price, it is not included in the measurement of american homes square footage)
- Excellent quality exteriors, kitchens and basements were also crucial to bring up a property sale price.
- Properties in Stone Brook, Northridge Heights, and Green Hills also enjoyed higher sales prices than other neighbourhoods.
Upon submission to kaggle, i had a score of 0.68772, ranking this model in the top 230 spots for predictors at that time.

---
#### Further Improvements

Despite having generated a relatively reliable predictive model, the following changes can still be made to better it:
i was conflicted as to why the kaggle dataset allowed my models to perform better than with the data i gathered. Despite having generated a relatively reliable predictive model, the following changes can still be made to better it:

- The current datasets have very little features to work with which led to me breaking up the datetime aspects of it to be used as engineered features.
- The data could be supplemented with additional features like weather info, public transport info, global events with participation rates and locations (i.e. music festivals/olympics). 

- The data contained features of specific aspects of a residence, e.g. baths being half/full or in the basement, basement finishing/quality/exposure that can be engineered to haved a better picture of the effect of a good/bad basement on the property price. Hence, the house could be feature engineered with collated scorings based on the interior/exterior/(types of) rooms/kitchen/garage/etc.
- There were a number of features (e.g. basement/garage/kitchen) that did not include data on types of material used or even color &/or paint. These were traits that may have influenced the ratings used to qualify the features as to the excellent/good/fair/typical/poor scale used. More of such data pertaining to materials and paints may prove useful when aligned with the progress of time from being built or last remodelling date.
- More data on the neighbourhood facilities/amentities would be useful in improving the average sales price within poorer performing ones (e.g. clinics/hospitals/schools/shopping centers/bus stops/train stations/jails/chemical plants/etc),

---
### Recommendation

The model can serve as a prediction model for current home owners and buyers. Additionally, both stakeholders can be more discerning of housing values given the identified features; e.g. home owners looking to sell could possibly upgrade their basements or kitchens; buyers could weigh the ground floor living space against their intended budget for a particular neighbourhood.
The model can serve as a prediction model for countries and businesses. Additionally, citizens can be more discerning of the potential for number of cases and deaths to rise (or fall) with time.

Notwithstanding this, more data, as mentioned above, will serve to optimise the model. The model can also serve as a starting point, to be extrapolated to other similar housing markets throughout the states. Additional research should be done into quantifying materials categories.
Notwithstanding this, more data, as mentioned above, will serve to optimise the model. The model can also serve as a starting point, to be extrapolated to individual countries with additional research to be done marrying the features together.
