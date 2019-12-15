- [ ] ADD THIS TO FINAL PROJECT

# Investigating Weight and Diet in High Schools
_Jay Machado and Jeffrey Wong_

## Background and Motivation

The [Youth Risk Behaviour Survey System](https://www.cdc.gov/healthyyouth/data/yrbs/overview.htm) was created in the 1990s periodically survey schools across the United States and gather information about the health behavior. These surveys change from year to year and are often conducted by different branches and different levels of government. The CDC have attempted to aggregate data from different years and with different questions and publish the data on their website. The combined data is available for surveys on the national level, on the state level, and on the district level.

The survey measures behaviors related to sex, diet, physical activity, drug use, and mental health. We decided we wanted to look at trends in body mass index (BMI) across students using the surveys conducted at a state level over the years of the program. This would allow us to look for general trends in the data pertaining to time, location, and demographics included in the survey, such as race, gender, etc. 

## Preliminary Data Export

The datasets are large and only available in a SAS or Access Database form. To make this more accessible to `pandas` without extra drivers, we exported the sets to .csv files. The original Access databases can be found [here](https://www.cdc.gov/healthyyouth/data/yrbs/data.htm). 

After exporting the relevant ones to .csv, the files can by read into Pandas and easily merged. A beefier computer is recommend, on a lower-end computer or a virtualization layer will likely require sampling and quickly discarding of the rest of the data.



