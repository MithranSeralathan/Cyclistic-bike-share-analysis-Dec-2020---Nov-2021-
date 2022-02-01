## **Case study: How Does a Bike-Share Navigate Speedy Success?**
* Period : Dec 2020 to Nov 2021
* Completed on : Jan 2022

## Introduction
This case study serves as the capstone of the [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics?utm_source=google&utm_medium=institutions&utm_campaign=gwgsite-gDigital-paidha-sem-bk-data-exa-glp-br-null). Here, I assumed the role of a data analyst working on the marketing team at Cyclistic, a fictional bike-share company in Chicago. The director of marketing believes that the companies future depends upon maximizing the number of annual memberships. My goal was to answer the question: How do casual riders and annual members use Cyclistic bikes differently? I used the data analysis process of Ask, Prepare, Process, Analyze, Share and Act  to answer key business questions, as well as I used the tools such as Excel,Tableau,and R programming language.
The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement)


## Ask phase
The problem I'm trying to solve : How do annual members and casual riders use Cyclistic bikes differently?
My insights for this case study : The differences in the variables and their relations, would give key information to describe the
difference in user type (member or casual) and it will generate useful information to guide the future marketing program and the above question.


## Prepare phase
I have used Cyclistic’s historical trip data to analyze and identify trends from the website - [divvy-tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html).
This data that has been made publicly available has been scrubbed to omit rider's personal information.
  * Excel to remove unwanted columns like location data (latitude and longitude), station id and station names as the data are not consistance and create a new column called day_of_week to find the day
  * Tableau to create a dashboard for an interactive visualization like monthly Usage, week Usage, and percentage of user type
  * R programming to calculate ride length and Visualization of average ride length by customer type on each month and day of the week

Stakeholders : Director of marketing and Cyclistic executive team

## Process & Analyze phase

### Excel

* Remove unwanted columns such as start_station_name, start_station_id, end_station_name, end_station_id, start_lat, start_lng, end_lat and end_lng
* Aligned and rename the headers for 12 files to the same formate such as ride_id, rideable_type, start_at, end_at, and user_type
* Created a new column called day_of_week to find the day of the week from the start_at column using the function "=TEXT(C2,"dddd")
* And saved all the files in a CSV format into a new folder called cleaned CSV files

### Tableau
* Loaded all the 12 csv files in the tableau tool and combined all the data into a single data frame by using union 
* Created a visualization for monthly usage with counts of ride_id as a row and column as the start_at
* To differentiate and label the visualization I have included User_type in the color and lable mark
* Also included forecast indicator to predict till february 2022

![Monthly Usage](https://user-images.githubusercontent.com/98275485/151932061-d037a928-0085-4a17-bb70-fddcd7c90071.png)

Please [click here](https://public.tableau.com/app/profile/mithran.seralathan/viz/Casestudyfordec2020toNov2021/CyclisticCasestudy) for a interactive view


* Created a another visualization by user type on each day of the week
* with day_of_week and user_type as a row and column as the count of ride_id
* To differentiate the user type I have included User_type in the color mark

![Week Usage](https://user-images.githubusercontent.com/98275485/151932155-cb8da5f2-4877-48da-8047-a8d585017cce.png)

Please [click here](https://public.tableau.com/app/profile/mithran.seralathan/viz/Casestudyfordec2020toNov2021/CyclisticCasestudy) for a interactive view


* Created a dashboard with some other useful visualization like percentage of user type and rideable type Vs. the total ride which is presented below for your reference

![Dashboard](https://user-images.githubusercontent.com/98275485/151932169-8fa14ed7-81ad-4f9f-8c8d-10d35162014e.png)

Please [click here](https://public.tableau.com/app/profile/mithran.seralathan/viz/Casestudyfordec2020toNov2021/CyclisticCasestudy) for a interactive view


### R programming

* Installing and reading Packages


```{r}
install.packages("tidyverse", repos = "http://cran.us.r-project.org",warn.conflicts=F, quietly=T)
install.packages("lubridate", repos = "http://cran.us.r-project.org",warn.conflicts=F, quietly=T)

library(tidyverse)
library(lubridate)  
library(ggplot2)
library(readr)

```
* Load datasets which is in CSV format

```{r}
# COLLECT DATA
X2020_12 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2020_12.csv")
X2021_01 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_01.csv")
X2021_02 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_02.csv")
X2021_03 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_03.csv")
X2021_04 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_04.csv")
X2021_05 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_05.csv")
X2021_06 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_06.csv")
X2021_07 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_07.csv")
X2021_08 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_08.csv")
X2021_09 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_09.csv")
X2021_10 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_10.csv")
X2021_11 <- read_csv("~/Case study Cyclistic/Cleaned CSV file/2021_11.csv")

```

* Combine ino a single data frame

```{r}
# COMBINE INTO A SINGLE FILE

str(X2020_12)
str(X2021_01)
str(X2021_02)
str(X2021_03)
str(X2021_04)
str(X2021_05)
str(X2021_06)
str(X2021_07)
str(X2021_08)
str(X2021_09)
str(X2021_10)
str(X2021_11)

all_trips <- bind_rows(X2020_12, X2021_01, X2021_02, X2021_03, X2021_04, X2021_05, X2021_06, X2021_07, X2021_08,
                       X2021_09, X2021_10, X2021_11)
View(all_trips)
```

* Changing date format

```{r}
#Formating the date for R programming to read
all_trips$start_at= strptime(all_trips$start_at,format='%d-%m-%Y %H:%M')
all_trips$end_at= strptime(all_trips$end_at,format='%d-%m-%Y %H:%M')
head(all_trips)

```

* Calculating ride length

```{r}
#Finding ride length
all_trips$ride_length <- difftime(all_trips$end_at,all_trips$start_at)
#convert the seconds to minutes
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))/60
head(all_trips)
```

* order by day of week and month

```{r}
  #order by day of week 
all_trips$day_of_week <- 
  ordered(all_trips$day_of_week, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", 
                                                 "Friday", "Saturday", "Sunday"))
  #Create a new column for month
all_trips$month <- format(as.Date(all_trips$start_at),'%b-%y')

  #order by day of month
all_trips$month <- 
  ordered(all_trips$month, levels=c("Dec-20", "Jan-21", "Feb-21", "Mar-21", 
                                           "Apr-21", "May-21","Jun-21","Jul-21", 
                                           "Aug-21", "Sep-21", "Oct-21","Nov-21"))
head(all_trips)
```

* Eliminating negative value from ride length

```{r}
# remove negative from ride length
all_trips <- all_trips[all_trips$ride_length >= 0,]
```

## Share Phase
* Statistical summary of trip_duration by customer_type

```{r}
# statictical summary of trip_duration for all trips
summary(all_trips$ride_length)

all_trips %>%
  group_by(user_type) %>%
  summarise(min_ride_length = min(ride_length),max_trip_duration = max(ride_length),
            median_trip_duration = median(ride_length), mean_trip_duration = mean(ride_length))
```

* Average number of trips by customer type and month

```{r message=FALSE, warning=FALSE}

unique(all_trips$month)

all_trips %>% 
  group_by(user_type, month) %>%  
  summarise(number_of_rides = n(),average_duration_mins = mean(ride_length))
```

* Visualization of average ride length by customer type on each day of week

```{r message=FALSE, warning=FALSE}
all_trips %>%  
  group_by(user_type, day_of_week) %>% 
  summarise(average_ride_length = mean(ride_length)) %>%
  ggplot(aes(x = day_of_week, y = average_ride_length, fill = user_type)) +
  geom_col(width=0.7, position = position_dodge(width=0.7)) + 
  labs(title ="Average ride length by customer type Vs. Day of week")
```
![image](https://user-images.githubusercontent.com/98275485/152029484-473868b3-2406-4756-a19e-7eb971e4ebbe.png)


* Visualization of average ride length by customer type on each month

```{r message=FALSE, warning=FALSE}
all_trips %>%  
  group_by(user_type, month) %>% 
  summarise(average_ride_length = mean(ride_length)) %>%
  ggplot(aes(x = month, y = average_ride_length, fill = user_type)) +
  geom_col(width=0.7, position = position_dodge(width=0.7)) + 
  labs(title ="Average ride length by customer type Vs. Month")
```
![image](https://user-images.githubusercontent.com/98275485/152029550-2f75a07e-5b8a-419b-afd7-04925d86e609.png)


## Act phase

To guide the marketing campaign to convert casual riders into annual members, we now have some data driven insights on how casual riders and annual members use Cyclistic bikes differently. The key findings and my recommendations for the marketing campaign are as follows:

1.  Casual riders prefer to take longer trips averaging 32.17 minutes per trip compared to members who average only 13.7 minutes (Data from the share phase statistical summary of trip_duration by customer_type)
> Use this statistic to show casual riders how they could save more money in the long run by becoming a member instead of paying for rides based on trip duration

2. Casual riders prefer to use cyclistic bikes on the weekends where the number of users are more as users in the week days (From Visualization of average ride length by customer type on each month)
> Develop a weekend membership plan whereby rides on the weekends are included in the base price while members have the option to book weekday rides at a lower rate
> or Offer discounted pricing during non-busy hours so that casual riders might choose to use bikes more often and level out demand over the day

## Resources

[Kaggle community](https://www.kaggle.com/)

[Stack Overflow](https://stackoverflow.com/)

[Lots of Google](http://https://www.google.co.in/?gws_rd=ssl)

[Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics)
