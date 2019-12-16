- [ ] ADD THIS TO FINAL PROJECT

# Investigating Lifestyle and its Impact on BMI of Teenagers
_Jay Machado and Jeffrey Wong_

## Background and Motivation

The [Youth Risk Behaviour Survey System](https://www.cdc.gov/healthyyouth/data/yrbs/overview.htm) was created in the 1990s periodically survey schools across the United States and gather information about the health behavior. These surveys change from year to year and are often conducted by different branches and different levels of government. The CDC have attempted to aggregate data from different years and with different questions and publish the data on their website. The combined data is available for surveys on the national level, on the state level, and on the district level.

The survey measures behaviors related to sex, diet, physical activity, drug use, and mental health. We decided we wanted to look at trends in body mass index (BMI) across students using the surveys conducted at a state level over the years of the program. BMI is a calculation based on the weight and height of an individual that gives a general idea if someone is at a healthy weight for their size. While it has its [limitations](https://en.wikipedia.org/wiki/Body_mass_index#Limitations), it is easier to collect that information such as fat percentage. We want to find general trends in BMI values with respect to time, location, and demographics included in the survey, such as race and gender. We focused only on the state datasets because combining other levels might introduce overlapping data.

## Preliminary Data Import

The datasets are large and only available in a SAS or Access Database form. To make this more accessible to `pandas` without extra drivers, we exported the sets to .csv files. The original Access databases can be found [here](https://www.cdc.gov/healthyyouth/data/yrbs/data.htm). 

After exporting the relevant ones to .csv, the files can by read into Pandas and easily merged. A beefier computer is recommend, on a lower-end computer or a virtualization layer will likely require sampling and quickly discarding of the rest of the data.

## Tidying the Data

We could just jump right in but it would be more prudent to translate everything we need to human-readable values. This data was designed for using different software, so many of the values are represented numerically with a [key given](https://www.cdc.gov/healthyyouth/data/yrbs/pdf/2017/2017_yrbs_sadc_documentation.pdf). We want to keep only certain demographic and lifestyle columns so we will first collect only those. Specifically, we want to keep all information pertaining to BMI, 

## Looking for Trends

The simplest place to begin is visualizing BMI across different time periods.

_Code_

We find that the above scatterplot is not extremely helpful, though it does point out that some BMI values are clearly incorrect. A 6' person with a weight of 1400 lbs would only have a BMI of about 190, so values above 200 must be incorrect. To make things simple, we will only look in the BMI range of 9 to 60. We will also only look at people ages 13 - 17 to keep ages in a range of known values.

_Code_

To make things easier to visualize, we will present a [violin plot](https://en.wikipedia.org/wiki/Violin_plot). Using this, we should be more easily able to understand where more of the values are, (the wider plots of the "violin").

_Code_

I would like to run a simple cubic regression on the data — this will look for the best cubic polynomial that fits the data. (Specifically, the cubic polynomial that will fit the data with the minimal least–squares error.) This is just to look for any possible trends over time.

_Code_

There appears to be an upward trend in BMI over time, but this could be the result of increased participation in the survey over the years. It would be prudent to look at trends for each state to see if they follow similar patterns. Since some states participated for fewer years, I will choose the degree based on the number of years they participated in. (We don't want to [overfit](https://en.wikipedia.org/wiki/Overfitting) for sets that participated in too few years.)

_Code_

We can see from this most states also have an upward trend, though to varying degrees. To illustrate differences in location we can show the mean BMI per state on a map.

_Code_

Let's try and find what may be causing this overall upward trend in BMI: we can look at questions about diet and exercise and see how they changed

## Developing a Model

We want to develop a linear model predicting BMI from a number of categorical predictor variables. The predictor variables we will use are:

The subset of data that includes the predictor variables and the dependent variable, BMI, is selected from the dataset. We also drop all of the incomplete data entries, accepting the error from using incomplete data.

_Code_

We will be training our model using validation techniques. In order to do so, we will designate 80% of our dataset for training and the remaining 20% as test data. Using the trainning data, we can develop a linear model.

_Code_

Once we have a model, we will use our testing data set to make predictions of BMI. We can calculate the residual from our predicated values and the actual value of BMI from our test data.

_Code_

Plotting the actual values versus our predicted values helps to show the accuracy of our model. A perfectly accurate model would have a perfectly linear trend with a slope of 1. As you can see from the graph, our model is quite conservative, estimating closer to average. This means that our model loses accuracy with individuals on the more extreme ends of the BMI scale. We have also listed Mean Absolute Error, Mean Squared Error, and Root Mean Squared Error, all of which are metrics to measure the error our model produces.

_Code_

We can go further in visualizing our error with a violin plot of the residuals. It shows that the resdiuals are heavily concentrated with a low magnitude, but are also heavily skewed by the more extreme values.

_Code_

## 10-Fold Cross Validation

We want to see if we can further strengthen our model by expanding our training methodology. To do so we will implement a 10-Fold Cross Validation with our data. A 10-Fold Cross Validation technique means that we are going to divide the dataset into 10 subsets and run our analysis with each subset as the test data. The results of this are then averaged to find the total accuracy of the model.

_Code_

If we want to change our model to use whether an individual exercises 1 or more day per a week rather than 7 days a week we can redo the previous models.

## Conclusion
We found an upward trend in BMI over the years of the survey and we found decreasing trends in question responses such as milk and veggie consumption. In spite of this, we can Though our training model had some accuracy, it appears that these questions alone are not great predictors of a student's BMI.
