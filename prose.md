- [ ] ADD THIS TO FINAL PROJECT

# Investigating Lifestyle and its Impact on BMI of Teenagers
_Jay Machado and Jeffrey Wong_

## Background and Motivation

The [Youth Risk Behaviour Survey System](https://www.cdc.gov/healthyyouth/data/yrbs/overview.htm) was created in the 1990s periodically survey schools across the United States and gather information about student health behavior. These surveys change from year to year and are often conducted by different branches and different levels of government. The CDC has attempted to aggregate data from different years and with different questions and publish the data on their website. The combined data is available for surveys on the national level, on the state level, and on the district level.

The survey measures behaviors related to sex, diet, physical activity, drug use, and mental health. We decided we wanted to look at trends in body mass index (BMI) across students using the surveys conducted at a state level over the years of the program. BMI is a calculation based on the weight and height of an individual that gives a general idea if someone is at a healthy weight for their size. While it has its [limitations](https://en.wikipedia.org/wiki/Body_mass_index#Limitations), it is easier to collect that information such as fat percentage. We want to find general trends in BMI values with respect to time, location, and behaviors in the survey, such as drinking milk or eating breakfast. Our hope is to find which predictors have a greater (if any) effect on BMI. We focused only on the state datasets because combining other levels might introduce overlapping data.

### Background Readings

The WHO calls childhood obesity ["one of the most serious public health challenges of the 21st century"](https://www.who.int/dietphysicalactivity/childhood/en/). The NIH hosts a decent article on [why obesity prevention is better than a cure](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4801195/). The WHO has an article on [why it matters](https://www.who.int/dietphysicalactivity/childhood_consequences/en/). The idea of this project is to be able to more easily identify students with at-risk behavior.

### Tutorial

At the end of this tutorial, you should be able to:

- Import YRBSS data from a csv file
- Tidy the data to be usable in pandas
- Plot BMI data with respect to time
- Run simple regressions to predict BMI over time
- Calculate and plot behavior prevalence
- Run regressions to predict BMI based on behavior

## Preliminary Data Import

The datasets are large and only available in a SAS or Access Database form. To make this more accessible to `pandas` without extra drivers, we exported the sets to .csv files. The original Access databases can be found [here](https://www.cdc.gov/healthyyouth/data/yrbs/data.htm). 

After exporting the relevant ones to .csv, the files can be read into Pandas and easily merged. A beefier computer is recommended, on a lower-end computer or a virtualization layer will likely require sampling and quickly discarding of the rest of the data.

## Tidying the Data

We could just jump right in but it would be more prudent to translate everything we need to human-readable values. This data was designed for using different software, so many of the values are represented numerically with a [key given](https://www.cdc.gov/healthyyouth/data/yrbs/pdf/2017/2017_yrbs_sadc_documentation.pdf). We want to keep only certain demographic and lifestyle columns so we will first collect only those. Specifically, we want to keep all information pertaining to BMI, race, sex, and behavior questions relating to diet and exercise that have been present in most of the data.

## Looking for Trends

The simplest place to begin is visualizing BMI across different time periods.

_Code_

We find that the above scatterplot is not extremely helpful, though it does point out that some BMI values are clearly incorrect. A 6' person with a weight of 1400 lbs would only have a BMI of about 190, so values above 200 must be incorrect. To make things simple, we will only look at the BMI range of 9 to 60. We will also only look at people ages 13 - 17 to keep ages in a range of known values.

_Code_

To make things easier to visualize, we will present a [violin plot](https://en.wikipedia.org/wiki/Violin_plot). Using this, we should be more easily able to understand where more of the values are, (the wider plots of the "violin").

_Code_

We would like to run a simple cubic regression on the data — this will look for the best cubic polynomial that fits the data. (Specifically, the cubic polynomial that will fit the data with the minimal least–squares error.) This is just to look for any possible trends over time.

_Code_

There appears to be an upward trend in BMI over time, but this could be the result of increased participation in the survey over the years. It would be prudent to look at trends for each state to see if they follow similar patterns. Since some states participated for fewer years, I will choose the degree based on the number of years they participated in. (We don't want to [overfit](https://en.wikipedia.org/wiki/Overfitting) for sets that participated in too few years.)

_Code_

We can see from this most states also have an upward trend, though to varying degrees. To illustrate differences in BMI per location we can show the mean BMI per state on a map.

_Code_

Before looking try to explain the trends, we first want to see if there is any signfication variation between BMI distributions of members of different sexes or races. We will use violin plots and regressions for this.

_Code_

There does not appear to be signification variation upon these demographics.

To try and see what might be causing the upward trend, we want to look at the behavior questions over time. To do this we will create a function that takes in a question name, calculates the percentage of students who answered affirmative and return the years and percentages. We can thus graph the percentage of students who drank milk daily, ate vegetables daily, drank soda daily, and who ate breakfast daily.

_Code_

There appears to be a downward trend in vegetable and milk consumption, but also a similar trend in soda consumption. Perhaps wit would be more useful to create a model that predicted BMI based on these variables. That might give us a better sense of which behaviors are more relevant.

## Developing a Model

We want to develop a linear model predicting BMI from a number of categorical predictor variables. The subset of data that includes the predictor variables and the dependent variable, BMI, is selected from the dataset. We also drop all of the incomplete data entries, accepting the error from using incomplete data.

_Code_

We will be training our model using validation techniques. In order to do so, we will designate 80% of our dataset for training and the remaining 20% as test data. Using the training data, we can develop a linear model.

_Code_

Once we have a model, we will use our testing data set to make predictions of BMI. We can calculate the residual from our predicted values and the actual value of BMI from our test data.

_Code_

Plotting the actual values versus our predicted values helps to show the accuracy of our model. A perfectly accurate model would have a perfectly linear trend with a slope of 1. As you can see from the graph, our model is quite conservative, estimating closer to average. This means that our model loses accuracy with individuals on the more extreme ends of the BMI scale. We have also listed Mean Absolute Error, Mean Squared Error, and Root Mean Squared Error, all of which are metrics to measure the error our model produces.

_Code_

We can go further in visualizing our error with a violin plot of the residuals. It shows that the residuals are heavily concentrated with a low magnitude, but are also heavily skewed by the more extreme values.

_Code_

### 10-Fold Cross–Validation

We want to see if we can further strengthen our model by expanding our training methodology. To do so we will implement a 10–Fold Cross–Validation with our data. A 10–Fold Cross–Validation technique means that we are going to divide the dataset into 10 subsets and run our analysis with each subset as the test data. The results of this are then averaged to find the total accuracy of the model.

_Code_

If we want to change our model to use whether an individual exercises 1 or more days per week rather than 7 days a week we can redo the previous models.

_Code_

## Conclusion

We found an upward trend in BMI over the years of the survey and we found decreasing trends in question responses such as milk and veggie consumption. In spite of this, it is important to keep in mind that correlation does not imply causation.
Furthermore, we were able to successfully develop a predictive model for BMI from a number of categorical, predictor variables. However, our model did not have particularly high accuracy. The model made very conservative predictions, resulting in fairly accurate predictions for individuals with average BMI, but a loss of accuracy towards the extremes. This indicates that alone the variables we used are not great predictors of a teenager's BMI. Therefore, further research and study is necessary before we would be comfortable making large generalizations about the effect of specific lifestyle choices on the BMI of a teenager.
