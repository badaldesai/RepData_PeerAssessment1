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
* A time series plot of the 5 minute interval and average number of steps taken, averaged across all days.

```r
intervalAvgData <- aggregate(x=list(meanSteps=Data$steps), by=list(interval=Data$interval), FUN=mean, na.rm=TRUE)

ggplot(data=intervalAvgData, aes(x=interval, y=meanSteps))+
  geom_line() + 
  xlab("5 minute interval")+
  ylab("avg number of steps taken")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

* To determine which 5-minute interval on average across all days in the dataset contains the maximum number of steps will use which.max()


```r
maxnumofsteps<-intervalAvgData[which.max(intervalAvgData$meanSteps),]
maxnumofsteps
```

```
##     interval meanSteps
## 104      835  206.1698
```

* The interval 835 has the maximum number of steps on average(206 steps).

## Imputing missing values

* The total number of missing values in the dataset will be given as:


```r
sum(is.na(Data$steps))
```

```
## [1] 2304
```

* Strategy to create a new data set is to replace a NA value with the mean.


```r
library(Hmisc)
ImputedData<-Data
ImputedData$steps<- impute(Data$steps, fun=mean)
```

* Histogram of the total number of steps taken each day with new data

```r
totalstepseachday <- tapply(ImputedData$steps, ImputedData$date, sum)
qplot(totalstepseachday, xlab='Total steps taken each day', ylab='Frequency using binwith=1000', binwidth=1000)
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png) 

* Mean and Median of total number of steps taken per day.

```r
mean(totalstepseachday)
```

```
## [1] 10766.19
```

```r
median(totalstepseachday)
```

```
## [1] 10766.19
```

* Without the missing data, the gap between mean and median is reduced is almost same.

## Are there differences in activity patterns between weekdays and weekends?

* A new factor variable in the dataset with two levels-"weekday" and "weekend" is created as follows:

```r
ImputedData$daytype <- ifelse(as.POSIXlt(ImputedData$date)$wday %in% c(0,6), 'weekend','weekday')

head(ImputedData)
```

```
##     steps       date interval daytype
## 1 37.3826 2012-10-01        0 weekday
## 2 37.3826 2012-10-01        5 weekday
## 3 37.3826 2012-10-01       10 weekday
## 4 37.3826 2012-10-01       15 weekday
## 5 37.3826 2012-10-01       20 weekday
## 6 37.3826 2012-10-01       25 weekday
```

* A time series plot of the full data is created with following R-functions.

```r
intervalAvgDataImputed<- aggregate(steps ~ interval + daytype, data=ImputedData, mean)

ggplot(intervalAvgDataImputed, aes(interval, steps))+
  geom_line()+
  facet_grid(daytype ~ .)+
  xlab("5 minute interval")+
  ylab("avg number of steps taken")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png) 
