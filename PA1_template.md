# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
raw_data <- read.csv("activity.csv")
intervals<-levels(factor(raw_data$interval))
dates <-levels(factor(raw_data$date))
mmm <- matrix(raw_data$steps, nrow = length(dates), ncol = length(intervals), dimnames = list(dates,intervals),byrow = TRUE)
```

## What is mean total number of steps taken per day?
 `total_step` is a variable to store total number of steps for each date

```r
total_step<-rowSums(mmm, na.rm = TRUE)
hist(total_step, xlab = "Total steps per day", main = "Total steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
mean(total_step)
```

```
## [1] 9354.23
```

```r
median(total_step)
```

```
## [1] 10395
```

## What is the average daily activity pattern?
`means_activity` is a variable to store average date of steps for each interval.
First number in output is time interval in format HMM, second number is ordinal number of this 5-minute interval throughtout of day.

```r
means_activity <-colMeans(mmm,na.rm = TRUE)
plot(dimnames(mmm)[[2]],means_activity,type = 'l',xlab = "time interval", main = "average activity", ylab = 'number of step')
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
names(which.max(means_activity))
```

```
## [1] "835"
```
## Imputing missing values
Total number of missing values in the dataset

```r
summary(raw_data$steps)[7]
```

```
## NA's 
## 2304
```
A strategy for filling in all of the missing values in the dataset:
I use mean for that 5-minute interval or 0 if NA.

```r
new_mmm<-mmm
for ( i in 1:nrow(new_mmm)){
  new_mmm[i,][is.na(mmm[i,])]<- means_activity[is.na(mmm[i,])]
}
total_step_new <-rowSums(new_mmm, na.rm = TRUE)
hist(total_step_new, xlab = "Total steps per day", main = "Total steps per day after filling NA's")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

```r
mean(total_step_new)
```

```
## [1] 10766.19
```

```r
median(total_step_new)
```

```
## [1] 10766.19
```
## Are there differences in activity patterns between weekdays and weekends?
