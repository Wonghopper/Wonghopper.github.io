- [ ] ADD THIS TO FINAL PROJECT

# Investigating Lifestyle Activities and its Impact on BMI in Teenagers
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

We can see from this most states also have an upward trend, though to varying degrees. 

## Developing a Model

_Code_

## 10-Fold Cross Validation

_Code_

## Conclusion
