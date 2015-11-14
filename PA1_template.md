# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
* The data is unzipped and loaded using the read.csv. 
* Note: It is assumed that you have already downloaded data.


```r
unzip(zipfile="activity.zip")
Data <- read.csv('activity.csv',colClasses = c("integer", "Date", "integer"))
head(Data)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```
## What is mean total number of steps taken per day?

* The total number of steps taken per day is calculated as per following using tapply function.

```r
totalsteps <- tapply(Data$steps, Data$date, sum, na.rm=TRUE)
head(totalsteps)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##          0        126      11352      12116      13294      15420
```

* Histogram is of the total number of steps taken each day is as follow:

```r
library(ggplot2)
qplot(totalsteps, xlab='Total steps each day', ylab = 'Frequency using binwith 1000', binwidth=1000)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

* The mean and median of the total number of steps taken per day is as follow:


```r
stepsperDayMean <- mean(totalsteps)
stepsperDayMean
```

```
## [1] 9354.23
```

```r
stepsperDayMedian <- median(totalsteps)
stepsperDayMedian
```

```
## [1] 10395
```

## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
