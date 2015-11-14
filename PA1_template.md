# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
The data is loaded using the read.csv. It is assumed that you have already downloaded data.


```r
Data <- read.csv('activity.csv',colClasses = c("integer", "Date", "integer"))
```

Process/transform the data


```r
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



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
