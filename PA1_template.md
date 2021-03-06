# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Load the data

```r
if(!file.exists("activity.csv")) {
                temp <- tempfile()
                temp <- "activity.zip"
                unzip(temp, "activity.csv")
                unlink(temp)
        }
amd <- read.csv("activity.csv")
head(amd)
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

```r
tail(amd)
```

```
##       steps       date interval
## 17563    NA 2012-11-30     2330
## 17564    NA 2012-11-30     2335
## 17565    NA 2012-11-30     2340
## 17566    NA 2012-11-30     2345
## 17567    NA 2012-11-30     2350
## 17568    NA 2012-11-30     2355
```

Process/transform the data (if necessary) into a format suitable for your analysis


```r
amd2 <- amd[!(is.na(amd$steps)), ]
```

## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

Calculate the total number of steps taken per day


```r
library(plyr)
```

```
## Warning: package 'plyr' was built under R version 3.2.2
```

```r
ss <- ddply(amd2, "date", summarise, sum.steps = sum(steps))
head(ss)
```

```
##         date sum.steps
## 1 2012-10-02       126
## 2 2012-10-03     11352
## 3 2012-10-04     12116
## 4 2012-10-05     13294
## 5 2012-10-06     15420
## 6 2012-10-07     11015
```

```r
tail(ss)
```

```
##          date sum.steps
## 48 2012-11-24     14478
## 49 2012-11-25     11834
## 50 2012-11-26     11162
## 51 2012-11-27     13646
## 52 2012-11-28     10183
## 53 2012-11-29      7047
```
If you do not understand the difference between a histogram and a barplot, research the difference

between them. Make a histogram of the total number of steps taken each day

```r
hist(ss$sum.steps, 
     main = c("Histogram of the total number of steps taken each day"), 
     xlab = c("Total number of steps per day"))
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 
 

Calculate and report the mean and median of the total number of steps taken per day

```r
mean.steps <- mean(ss$sum.steps)
print(mean.steps)
```

```
## [1] 10766.19
```

```r
median.steps <- median(ss$sum.steps)
print(median.steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
as <- ddply(amd2, "interval", summarise, average = mean(steps))
plot(as$interval, as$average, type="l", 
     main = c("Time series plot of average number of steps"), 
     xlab = c("5-minute interval"), ylab = c("Average number of steps taken"))
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png) 
 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
max(as$average)
```

```
## [1] 206.1698
```

```r
max.interval <- as[(as$average > 206),1]
print(max.interval)
```

```
## [1] 835
```


## Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded asÂ NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset (i.e. the total number of rows withÂ NAs)

```r
NA.amd <- count(amd[is.na(amd$steps), ])
total.NA <- sum(NA.amd$freq)
print(total.NA)
```

```
## [1] 2304
```
 

Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

 

Create a new dataset that is equal to the original dataset but with the missing data filled in.

 

Make a histogram of the total number of steps taken each day and Calculate and report theÂ meanÂ andmedianÂ total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?



## Are there differences in activity patterns between weekdays and weekends?
For this part theÂ weekdays()Â function may be of some help here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels â???" â???oweekdayâ??? and â???oweekendâ??? indicating whether a given date is a weekday or weekend day.

 

Make a panel plot containing a time series plot (i.e.Â type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

