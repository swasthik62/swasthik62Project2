# Case Study 2: How Can a Wellness Technology Company Play It Smart?
## scenario
Here in this project as a data analyst i will be working for Bellabeat, a high-tech manufacturer of health-focused products for women, and
meet different characters and team members. Bellabeat is a successful small company, but they have the potential to become a larger player in the
global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart
device fitness data could help unlock new growth opportunities for the company. I have been asked to focus on one of
Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The
insights im discover will then help guide marketing strategy for the company. I will present my analysis to the Bellabeat
executive team along with my high-level recommendations for Bellabeat’s marketing strategy.

## Methodology Used
In order to approach this business task, I have utilized the Ask, Prepare, Process, Analyze, Share and Act methodology. Each step is detailed below along with the crucial guiding questions and answers as we progress through the analysis.

# Ask Stage 
This is the first stage in the data analysis process where i need to required to ask a comprehensive list of questions about the project, its goals and the stakeholder expectations. I will Analyze smart device usage data in order to gain insight into how people are already using their smart devices. Then, using this information, give high-level recommendations for how these trends can inform Bellabeat marketing strategy.

**Stake Holders**
1. Urška Sršen: Bellabeat's cofounder and Chief Creative Oﬃcer
2. Sando Mur: Mathematician and Bellabeat's cofounder; key member of the Bellabeat executive team
3. Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and

# Prepare
After the Ask stage i have moved to Preparing of data.I downloaded the data from the from Fitbit Fitness that explores smart device users daily habits and stored in desktop.

I have observed all the dataset and Analysis how to solve the issue with given dataset

I took 5 data sheets from the selected zip file

Based on the analysis Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits. It is a good data set that can be used to analyze the behavior of the users, and it has a wide variety of information. It is reliable, original, credible and current.

**packages and library**
In this initial step i have installed the necessary packages for further data analysis operations

```bash
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(readr)
library(janitor)
library(data.table)
library(skimr)
```

Result
![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/libraries%20-prepare.jpg)


**Importing Data**
We will be using the FitBit dataset, which was already downloaded. I also looked at the database before upload it. I will be using the data from dailyActivity_merged and sleepDay_merged.csv files as the information from most of the same files were located in dailyActivity_merged file. The weightLogInfo data is very limited and will not be used.

Before importing data we need to set the track for our data hence i have gone through the below me ntioned command
```bash
getwd()
setwd("C:/Swasthik pr/Project 2/datset")
```
Once the path has been created iam going to import the required dataset for our further analysis

```bash
activity = read.csv("dailyActivity merged.csv")
calories = read.csv("hoursly calories.csv")
intensities = read.csv("hourlyIntensities_merged.csv")
sleep = read.csv("sleepDay_merged.csv")
weight = read.csv("weightLogInfo meged.csv")
```

Result

![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/prepare%20data%20import.jpg)

# Processing Stage

Here in the process stage We will look look for data type errors, duplicates, validation errors, null values, data inconsitency, etc.
Once the Dataset has been imported we need to review the dataset for better results hence im using below mentioned command to verify the Columns and Dataset
We need to check for the clarification of the Data Types so we need to run below mentioned command

```bash
head(activity)
head(calories)
head(intensities)
head(sleep)
head(weight)
```

Result

![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/head%20process.jpg)

**Data Cleaning**

**1. And we can see there is lots of Duplicates residue in the Dataset and we need to tremove those duplicate entries**

```bash
activity$Id[!duplicated(activity$Id,)]
calories$Id[!duplicated(calories$Id,)]
intensities$Id[!duplicated(intensities$Id,)]
sleep$Id[!duplicated(sleep$Id,)]
weight$Id[!duplicated(weight$Id,)]
```

Result

![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/removing%20duplicated%20process.jpg)

**2. Once all the duplicated has been cleared we need to cross check whether any duplicates are remaining in the dataset**

```bash
sum(duplicated(activity))
sum(duplicated(calories))
sum(duplicated(intensities))
sum(duplicated(sleep))
sum(duplicated(weight))
```
**3. And we can see the Id's which are in 'Num' format and we need to convert it as 'Char' hence i have used below mentioned command**

```bash
calories$Id = as.character(calories$Id)
activity$Id = as.character(activity$Id)
intensities$Id = as.character(intensities$Id)
sleep$Id = as.character(sleep$Id)
weight$Id = as.character(weight$Id)
```
Result

![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/checking%20for%20duplicates.jpg)


**4.Once the Dataset has been imported and cleaned we need to do the each column is concies and uniform because if we merge the file we should not get an error.
In the below step im goinng to format date of the dataset**

```bash
# intensities
intensities$ActivityHour=as.POSIXct(intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
intensities$time <- format(intensities$ActivityHour, format = "%H:%M:%S")
intensities$date <- format(intensities$ActivityHour, format = "%m/%d/%y")

# calories
calories$ActivityHour = as.POSIXct(calories$ActivityHour, format = "%m/%d/%y %H:%M", tz = Sys.timezone())
calories$time <- format(calories$ActivityHour, format = "%H:%M:%S")
calories$date <- format(calories$ActivityHour, format = "%m/%d/%y")

# activity
activity$ActivityDate=as.POSIXct(activity$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
activity$date <- format(activity$ActivityDate, format = "%m/%d/%y")

# sleep
sleep$sleepday = as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$date <- format(sleep$SleepDay, format = "%m/%d/%y")
sleep$SleepDay = weekdays(sleep$sleepday)
```
Result


![App Screenshot](https://github.com/swasthik62/swasthik62Project2/blob/main/date%20format.jpg)

# Analyze Stage
 Now we have our Dataset clean and tready for visualization hence im go ahead and analyze the trends of FitBit users and determine if this insight will be beneficial to achieving the business task.
 
```bash
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()

activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()

calories %>%
  select(Calories) %>%
  summary()

sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()

weight %>%
  select(WeightKg, BMI) %>%
  summary()
  ```
  
Result
  
  ![image](https://user-images.githubusercontent.com/125183564/225397238-1da62325-ffeb-44d5-9358-aee38f8f15e9.png)

Outcome : Average total steps per day are 7638 which a little bit less for having health benefits for according to the CDC research. They found that taking 8,000 steps per day was associated with a 51% lower risk for all-cause mortality (or death from all causes). Taking 12,000 steps per day was associated with a 65% lower risk compared with taking 4,000 steps.

**Data Visualization**

```bash
ggplot(data=activity, aes(x=TotalSteps, y=Calories)) + 
  geom_point() + geom_smooth() + labs(title="Total Steps vs. Calories")
```
Result

![image](https://user-images.githubusercontent.com/125183564/225398513-43b3e815-0282-4ff2-ab74-a03427ba826d.png)

I see positive correlation here between Total Steps and Calories, which is obvious - the more active we are, the more calories we burn.

```bash
ggplot(data=sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + 
  geom_point()+ labs(title="Total Minutes Asleep vs. Total Time in Bed")
 ```
Result
 
 ![image](https://user-images.githubusercontent.com/125183564/225399268-32aef694-03f6-4ff5-8a4e-3e512048bcc8.png)

The relationship between Total Minutes Asleep and Total Time in Bed looks linear. So if the Bellabeat users want to improve their sleep, we should consider using notification to go to sleep.

```bash
int_new <- intensities %>%
  group_by(time) %>%
  drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

ggplot(data=int_new, aes(x=time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title="Average Total Intensity vs. Time")
```
Result:

![image](https://user-images.githubusercontent.com/125183564/225400229-239cae35-066c-4a7d-920b-f01662676c45.png)

1. This visualization clearly provide the activenessand  I found out that people are more active between 5 am and 10pm.
2. Most activity happens between 5 pm and 7 pm - I suppose, that people go to a gym or for a walk after finishing work. We can use this time in the Bellabeat app to remind and motivate users to go for a run or walk.


# Share and Act

Once the analysis is completed and we have obtained the insights, now we have to share this information to the relevant key stakeholders. In this case, a presentation will be made for the stakeholders outlining the business task, assumptions made using the data we had and translating the insights from the analysis to actionable recommendations.

Recommendations for the Bellabeat Spring Device:

1. They need to share their testimonial and success stories and also they should collect the feedback from their customer
2. Adjust the algorithm in the Spring bottle tracking device to sync with the Ivy health tracker to alert users “aggresively” between 5 pm and 7 pm to increase their water intake.
2. Women who work full-time jobs  and spend a lot of time at the computer/in a meeting/ focused on work they are doing.
3. These women do some light activity to stay healthy (according to the activity type analysis). Even though they need to improve their everyday activity to have health benefits. They might need some knowledge about developing healthy habits or motivation to keep going.
4. Now is an excellent time to start a virtual community. Users will feel more supported if community-building is emphasized throughout the messaging and campaign.
5. The pandemic caused a lot of losses, now that it is almost in the past, Bellabeat has the option to remarket her products to current users or those who have recently stopped using their products.


Script: https://github.com/swasthik62/swasthik62.github.io/blob/main/script.R
