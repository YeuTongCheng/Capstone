Case Study-Bellabeat
================
Nicole
2022-09-14

## About The Company

Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that
manufactures health-focused smart products. Sršen used her background as
an artist to develop beautifully designed technology that informs and
inspires women around the world. Collecting data on activity, sleep,
stress, and reproductive health has allowed Bellabeat to empower women
with knowledge about their own health and habits. Since it was founded
in 2013, Bellabeat has grown rapidly and quickly positioned itself as a
tech-driven wellness company for women.

## Business Task

Analyze smart device usage data in order to gain insight into how
consumers use non-Bellabeat smart devices.

Bellabeat product:

-   Bellabeat app: The Bellabeat app provides users with health data
    related to their activity, sleep, stress, menstrual cycle, and
    mindfulness habits. This data can help users better understand their
    current habits and make healthy decisions. The Bellabeat app
    connects to their line of smart wellness products.
-   Leaf: Bellabeat’s classic wellness tracker can be worn as a
    bracelet, necklace, or clip. The Leaf tracker connects to the
    Bellabeat app to track activity, sleep, and stress.
-   Time: This wellness watch combines the timeless look of a classic
    timepiece with smart technology to track user activity, sleep, and
    stress. The Time watch connects to the Bellabeat app to provide you
    with insights into your daily wellness.
-   Spring: This is a water bottle that tracks daily water intake using
    smart technology to ensure that you are appropriately hydrated
    throughout the day. The Spring bottle connects to the Bellabeat app
    to track your hydration levels.

## Data Resource

The data source used for this case study is [FitBit Fitness Tracker
Data](https://www.kaggle.com/arashnic/fitbit)

This Kaggle data set contains personal fitness tracker from thirty
fitbit users. Thirty eligible Fitbit users consented to the submission
of personal tracker data, including minute-level output for physical
activity, heart rate, and sleep monitoring. It includes information
about daily activity, steps, and heart rate that can be used to explore
users’ habits.

## Install Packages and Libraries

I use the following packages for the analysis:

-   tidyverse
-   ggplot2
-   dplyr
-   GGallyr

``` r
install.packages("tidyverse")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
library(tidyverse)
```

    ## ── Attaching packages
    ## ───────────────────────────────────────
    ## tidyverse 1.3.2 ──

    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
install.packages("ggplot2")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
library(ggplot2)
install.packages("dplyr")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
library(dplyr)
install.packages("GGallyr")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

    ## Warning: package 'GGallyr' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

``` r
library(GGally)
```

    ## Registered S3 method overwritten by 'GGally':
    ##   method from   
    ##   +.gg   ggplot2

## Importing and Previwing Dataset

Since other datasets have either limited observations(weightLogInfo has
only 8 observations) or smallunit of
time(minuteCaloriesNarrow,minuteIntensitiesNarrow,etc), I will only
focus on 5 datasets to complete my analysis:

-   dailyActivity
-   dailySteps
-   hourlySteps
-   dailyCalories
-   hourlyCalories

``` r
dailyActivity<-read_csv(file="dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hourlySteps<-read_csv(file="hourlySteps_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hourlyCalories<-read_csv(file="hourlyCalories_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
sleepDay<-read_csv(file="sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(dailyActivity)
```

    ## # A tibble: 6 × 15
    ##       Id Activ…¹ Total…² Total…³ Track…⁴ Logge…⁵ VeryA…⁶ Moder…⁷ Light…⁸ Seden…⁹
    ##    <dbl> <chr>     <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 1.50e9 4/12/2…   13162    8.5     8.5        0    1.88   0.550    6.06       0
    ## 2 1.50e9 4/13/2…   10735    6.97    6.97       0    1.57   0.690    4.71       0
    ## 3 1.50e9 4/14/2…   10460    6.74    6.74       0    2.44   0.400    3.91       0
    ## 4 1.50e9 4/15/2…    9762    6.28    6.28       0    2.14   1.26     2.83       0
    ## 5 1.50e9 4/16/2…   12669    8.16    8.16       0    2.71   0.410    5.04       0
    ## 6 1.50e9 4/17/2…    9705    6.48    6.48       0    3.19   0.780    2.51       0
    ## # … with 5 more variables: VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>, and
    ## #   abbreviated variable names ¹​ActivityDate, ²​TotalSteps, ³​TotalDistance,
    ## #   ⁴​TrackerDistance, ⁵​LoggedActivitiesDistance, ⁶​VeryActiveDistance,
    ## #   ⁷​ModeratelyActiveDistance, ⁸​LightActiveDistance, ⁹​SedentaryActiveDistance

``` r
head(hourlySteps)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          StepTotal
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       373
    ## 2 1503960366 4/12/2016 1:00:00 AM        160
    ## 3 1503960366 4/12/2016 2:00:00 AM        151
    ## 4 1503960366 4/12/2016 3:00:00 AM          0
    ## 5 1503960366 4/12/2016 4:00:00 AM          0
    ## 6 1503960366 4/12/2016 5:00:00 AM          0

``` r
head(hourlyCalories)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       81
    ## 2 1503960366 4/12/2016 1:00:00 AM        61
    ## 3 1503960366 4/12/2016 2:00:00 AM        59
    ## 4 1503960366 4/12/2016 3:00:00 AM        47
    ## 5 1503960366 4/12/2016 4:00:00 AM        48
    ## 6 1503960366 4/12/2016 5:00:00 AM        48

``` r
head(sleepDay)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay              TotalSleepRecords TotalMinutesAsleep TotalT…¹
    ##        <dbl> <chr>                             <dbl>              <dbl>    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM                 1                327      346
    ## 2 1503960366 4/13/2016 12:00:00 AM                 2                384      407
    ## 3 1503960366 4/15/2016 12:00:00 AM                 1                412      442
    ## 4 1503960366 4/16/2016 12:00:00 AM                 2                340      367
    ## 5 1503960366 4/17/2016 12:00:00 AM                 1                700      712
    ## 6 1503960366 4/19/2016 12:00:00 AM                 1                304      320
    ## # … with abbreviated variable name ¹​TotalTimeInBed

``` r
str(dailyActivity)
```

    ## spec_tbl_df [940 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                      : num [1:940] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityDate            : chr [1:940] "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
    ##  $ TotalSteps              : num [1:940] 13162 10735 10460 9762 12669 ...
    ##  $ TotalDistance           : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ TrackerDistance         : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ LoggedActivitiesDistance: num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveDistance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
    ##  $ ModeratelyActiveDistance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
    ##  $ LightActiveDistance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
    ##  $ SedentaryActiveDistance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveMinutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
    ##  $ FairlyActiveMinutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
    ##  $ LightlyActiveMinutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
    ##  $ SedentaryMinutes        : num [1:940] 728 776 1218 726 773 ...
    ##  $ Calories                : num [1:940] 1985 1797 1776 1745 1863 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   ActivityDate = col_character(),
    ##   ..   TotalSteps = col_double(),
    ##   ..   TotalDistance = col_double(),
    ##   ..   TrackerDistance = col_double(),
    ##   ..   LoggedActivitiesDistance = col_double(),
    ##   ..   VeryActiveDistance = col_double(),
    ##   ..   ModeratelyActiveDistance = col_double(),
    ##   ..   LightActiveDistance = col_double(),
    ##   ..   SedentaryActiveDistance = col_double(),
    ##   ..   VeryActiveMinutes = col_double(),
    ##   ..   FairlyActiveMinutes = col_double(),
    ##   ..   LightlyActiveMinutes = col_double(),
    ##   ..   SedentaryMinutes = col_double(),
    ##   ..   Calories = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(hourlySteps)
```

    ## spec_tbl_df [22,099 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id          : num [1:22099] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityHour: chr [1:22099] "4/12/2016 12:00:00 AM" "4/12/2016 1:00:00 AM" "4/12/2016 2:00:00 AM" "4/12/2016 3:00:00 AM" ...
    ##  $ StepTotal   : num [1:22099] 373 160 151 0 0 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   ActivityHour = col_character(),
    ##   ..   StepTotal = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(hourlyCalories)
```

    ## spec_tbl_df [22,099 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id          : num [1:22099] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityHour: chr [1:22099] "4/12/2016 12:00:00 AM" "4/12/2016 1:00:00 AM" "4/12/2016 2:00:00 AM" "4/12/2016 3:00:00 AM" ...
    ##  $ Calories    : num [1:22099] 81 61 59 47 48 48 48 47 68 141 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   ActivityHour = col_character(),
    ##   ..   Calories = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(sleepDay)
```

    ## spec_tbl_df [413 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                : num [1:413] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ SleepDay          : chr [1:413] "4/12/2016 12:00:00 AM" "4/13/2016 12:00:00 AM" "4/15/2016 12:00:00 AM" "4/16/2016 12:00:00 AM" ...
    ##  $ TotalSleepRecords : num [1:413] 1 2 1 2 1 1 1 1 1 1 ...
    ##  $ TotalMinutesAsleep: num [1:413] 327 384 412 340 700 304 360 325 361 430 ...
    ##  $ TotalTimeInBed    : num [1:413] 346 407 442 367 712 320 377 364 384 449 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   SleepDay = col_character(),
    ##   ..   TotalSleepRecords = col_double(),
    ##   ..   TotalMinutesAsleep = col_double(),
    ##   ..   TotalTimeInBed = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

## Cleaning Data

Make sure the datasets have no duplicate or null data.

``` r
dailyActivity<-dailyActivity %>%
  distinct(Id,ActivityDate, .keep_all = TRUE) %>% 
  drop_na()
hourlyCalories<-hourlyCalories%>% 
  distinct(Id,ActivityHour, .keep_all = TRUE)%>% 
  drop_na()
hourlySteps <-hourlySteps %>% 
  distinct(Id,ActivityHour, .keep_all = TRUE)%>% 
  drop_na()
sleepDay<-sleepDay %>%
  distinct(Id,SleepDay, .keep_all = TRUE) %>% 
  drop_na()
```

## Formatting Data

Make sure each dataset have consistent format to prepare for the merger.

``` r
dailyActivity<-dailyActivity %>%
  rename(Date=ActivityDate) %>% 
  mutate(Date=as.Date(Date, "%m/%d/%y"))
sleepDay<-sleepDay%>% 
  rename(Date=SleepDay) %>% 
  mutate(Date = as.Date(Date, "%m/%d/%y"))
hourlySteps<-hourlySteps %>%
  rename(Date_time=ActivityHour) %>% 
  mutate(Date_time = as.POSIXct(Date_time,format ="%m/%d/%Y %I:%M:%S %p"))
hourlyCalories<-hourlyCalories%>% 
  rename(Date_time=ActivityHour) %>% 
  mutate(Date_time = as.POSIXct(Date_time,format ="%m/%d/%Y %I:%M:%S %p"))
```

## Merging Data

First, merge the hourlyCalories and hourlySteps by using Id and
Date_time

Then, merge the dailyActivity and sleepDay by using Id and Date

``` r
activity_sleep <- merge(dailyActivity, sleepDay, by=c ("Id", "Date"))
hourly_calories_steps <- merge(hourlyCalories, hourlySteps, by=c ("Id", "Date_time"))
```

## Plotting and Analyzing Data-Activity Status vs Sleeping Status

### Chech the correlation between daily steps and daily calories

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](README_figs/README-correlation%20plot1-1.png)<!-- -->

### Note

As we expected, there is a positive correlation between daily steps and
calories burned (the more steps walked the more calories may be burned)

### Chech the correlation between daily steps and daily sleep

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](README_figs/README-correlation%20plot2-1.png)<!-- -->

    ## [1] -0.1903439

### Note

There is a slightly negative correlation(-0.19) between daily steps and
daily sleep time. We can utilize this finding to discover more insights.

### Check the correlation between daily activity and daily sleep

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](README_figs/README-correlation%20plot-1.png)<!-- -->

    ## [1] -0.06929398

### Note

There is no correlation(-0.06) between daily active time and daily sleep
time. We can conclude that daily sleep time only negatively correlated
with daily steps.

### Categorizing Users

Create a datasets with mean_steps,mean_calories, and mean_sleep for each
users, then categorize their activity status and sleeping status by
using the mean value of steps and sleep time. I do not compute the mean
of TotalActiveMinutes because we just found that active time and sleep
time are not correlated.

I categorize the activity status as following:

-   Sedentary - Less than 5000 steps a day.
-   Lightly active - Between 5000 and 7499 steps a day.
-   Fairly active - Between 7500 and 9999 steps a day.
-   Very active - More than 10000 steps a day. Classification has been
    made through this
    [website](https://www.10000steps.org.au/articles/healthy-lifestyles/counting-steps/)

I categorize the sleeping status as following:

-   Not adequate-sleep less than 6 hours a day.
-   Adequate-sleep between 6 to 9 hours a day.
-   Too much-sleep more than 9 hours a day. Classification has been made
    through this
    [website](https://www.sleepfoundation.org/how-sleep-works/how-much-sleep-do-we-really-need)

``` r
mean<-activity_sleep %>% 
  group_by(Id) %>% 
  summarize(mean_steps=mean(TotalSteps),mean_calories=mean(Calories),mean_sleep=mean(TotalMinutesAsleep))
mean<-mean %>% 
  mutate(Activity_status = case_when(
    mean_steps < 5000 ~ "Sedentary",
    mean_steps >= 5000 & mean_steps < 7499 ~ "Lightly active", 
    mean_steps >= 7500 & mean_steps < 9999 ~ "Fairly active", 
    mean_steps >= 10000 ~ "Very active"
  )) %>% 
  mutate(Sleep_status=case_when(mean_sleep < 360 ~ "Not adequate",
    mean_sleep >= 360 & mean_sleep <= 540 ~ "Adequate", 
    mean_sleep > 540 ~ "Too much"))
mean$Activity_status <- factor(mean$Activity_status, levels = c("Very active", "Fairly active", "Lightly active","Sedentary"))
```

### Creating Bar Chart

Check the overall sleeping status of users and categorizing with
activity status. ![](README_figs/README-status%20plot-1.png)<!-- -->

### Note

We can found that only one user sleep too much, who is considered
“Sedentary”. Moreover, most of the people who are considered “Fairly
active” have adequate sleep. However, the sleeping status for users with
“Lightly active” and “Very active” activity status has no apparent
differentiation.

### Activity Status Distribution

Create a pie chart to understand the distribution of users’ activity
status. First, I calculate the percentage of each activity status Then,
creating a pie chart by using the “percentage_activity” data frame

``` r
per<-mean %>% 
  group_by(Activity_status) %>% 
  summarise(sum_by_status=n()) %>% 
  mutate(percentage=sum_by_status/sum(sum_by_status))
percentage_activity<-select(per,Activity_status,percentage)
percentage_activity$Activity_status <- factor(percentage_activity$Activity_status, levels = c("Very active", "Fairly active", "Lightly active","Sedentary"))
```

![](README_figs/README-distribution%20pie%20chart-1.png)<!-- -->

## Recommandation

We can remind users with “Sedentary” activity status to take some walks
and let them know the recommended amount of sleep based on the users’
age. Moreover, for users with “Lightly active” and “Very active”
activity status, we should collect more data so as to discover more
insights for these users. For users with “Fairly active” activity
status, we can notify them of the functions that help users set sleep
schedules.

## Plotting and Analyzing Data-Usage of Device

### Determine the Distribution of Device Usage Frequency

I calculate the distribution by using “activity_sleep” data frame, and
categorize the device usage frequency as following:

-   High-use more than 20 days a month.
-   Medium-use between 20 and 11 days a month.
-   Low-use less than 11 days a month.

Then, create a pie chart to visualize the distribution of frequency.

``` r
fre_month<-activity_sleep %>% 
  group_by(Id) %>% 
  summarise(total_use=n()) %>% 
  mutate(usage_freq = case_when(
    total_use >20 ~ "High",
    total_use <= 20 & total_use >= 11 ~ "Medium", 
    total_use < 11 ~ "Low")) 
fre_month<-select(fre_month,Id,total_use,usage_freq)

fre_month<-fre_month %>%
  group_by(usage_freq) %>% 
  summarise(total=n()) %>% 
  mutate(total_use_per=total/sum(total))
fre_month$usage_freq <- factor(fre_month$usage_freq, levels = c("High", "Medium", "Low"))
```

![](README_figs/README-frequency%20pie%20chart-1.png)<!-- -->

### Note

Half of the users use our devices with high frequency. However, it’s our
goal to increase users with medium frequency.

### Recommendation

We should collect additional information of users with medium frequency
and low frequency, including the reason that prevent them from using
devices, their opinion to the products and their working day as well as
leisure day,etc.

## Plotting and Analyzing Data-Activity Level Thoughout a Day

I use the “hourly_calories_steps” data frame to determine average of
activity level in hourly basis.

    ##           Id       Date     Time Calories StepTotal
    ## 1 1503960366 2016-04-12 00:00:00       81       373
    ## 2 1503960366 2016-04-12 01:00:00       61       160
    ## 3 1503960366 2016-04-12 02:00:00       59       151
    ## 4 1503960366 2016-04-12 03:00:00       47         0
    ## 5 1503960366 2016-04-12 04:00:00       48         0
    ## 6 1503960366 2016-04-12 05:00:00       48         0

![](README_figs/README-daily%20activity%20level-1.png)<!-- -->

### Note

Users are most active in 12pm\~2pm and 5\~7pm, and are least active in
12am\~5am

## Conclusion

To improve users experience and promote our products, we should:

-   Include more users data, since our datasets are relatively small and
    might be skewed.
-   Differentiate our marketing strategy based on user activity status,
    as measured by average steps taken per month.
-   Collect information from users with low frequency by creating online
    questionnaires or hosting activities, and come up with solutions to
    increase usage frequency. We can also collect information from users
    with medium frequency to identify the differences between these two
    groups.
-   Avoid updating our products during the most active hour, and
    12am\~5am is the most recommended duration to update products.
