# Peer Assessment 1


```r
data <- read.csv("activity.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'activity.csv': No such file
## or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
str(data)
```

```
## function (..., list = character(), package = NULL, lib.loc = NULL, 
##     verbose = getOption("verbose"), envir = .GlobalEnv)
```

```r
steps    = data$steps
```

```
## Error in data$steps: object of type 'closure' is not subsettable
```

```r
date     = data$date
```

```
## Error in data$date: object of type 'closure' is not subsettable
```

```r
interval = data$interval 
```

```
## Error in data$interval: object of type 'closure' is not subsettable
```

```r
# 5 minutes interval means 288 intervals per day

steps.per.day <- aggregate(steps ~ date,data=data,FUN=sum)
```

```
## Error in as.data.frame.default(data, optional = TRUE): cannot coerce class ""function"" to a data.frame
```

```r
barplot(steps.per.day$steps,names.arg=steps.per.day$date,xlab="Date",ylab="Number of Steps")
```

```
## Error in barplot(steps.per.day$steps, names.arg = steps.per.day$date, : object 'steps.per.day' not found
```

The mean and median steps taken per days given by


```r
mean(steps.per.day$steps)
```

```
## Error in mean(steps.per.day$steps): object 'steps.per.day' not found
```

```r
median(steps.per.day$steps)
```

```
## Error in median(steps.per.day$steps): object 'steps.per.day' not found
```

## What is the average daily acivity pattern?

### Time Series Interval of Steps


```r
steps.per.interval <- aggregate(steps ~ interval,data=data,FUN=mean)
```

```
## Error in as.data.frame.default(data, optional = TRUE): cannot coerce class ""function"" to a data.frame
```

```r
plot(steps.per.interval$interval,steps.per.interval$steps,type="l",xlab=("Intervals"),ylab="Number of Steps")
```

```
## Error in plot(steps.per.interval$interval, steps.per.interval$steps, type = "l", : object 'steps.per.interval' not found
```

```r
steps.per.interval$interval[which.max(steps.per.interval$steps)]
```

```
## Error in eval(expr, envir, enclos): object 'steps.per.interval' not found
```

## Imputing Missing Values

### Total Number of missing values. 


```r
sum(is.na(data))
```

```
## Warning in is.na(data): is.na() applied to non-(list or vector) of type
## 'closure'
```

```
## [1] 0
```

### Strategy for filling missing values. Since we have the result of the mean number of steps in 5-min intervals, we will use that result to fill the missing values.


```r
nas = is.na(data$steps)
```

```
## Error in data$steps: object of type 'closure' is not subsettable
```

```r
data <- merge(data, steps.per.interval, by = "interval", suffixes = c("",".y"))
```

```
## Error in as.data.frame.default(x): cannot coerce class ""function"" to a data.frame
```

```r
data$steps[nas] <- data$steps.y[nas]
```

```
## Error in data$steps.y: object of type 'closure' is not subsettable
```

```r
data <- data[, c(1:3)]
```

```
## Error in data[, c(1:3)]: object of type 'closure' is not subsettable
```

### We calculate again de mean and the median


```r
steps.per.day1 <- aggregate(steps ~ date,data=data,FUN=sum)
```

```
## Error in as.data.frame.default(data, optional = TRUE): cannot coerce class ""function"" to a data.frame
```

```r
barplot(steps.per.day$steps,names.arg=steps.per.day$date,xlab="Date",ylab="Number of Steps")
```

```
## Error in barplot(steps.per.day$steps, names.arg = steps.per.day$date, : object 'steps.per.day' not found
```

### The mean and median steps taken per dayis given by


```r
mean(steps.per.day1$steps)
```

```
## Error in mean(steps.per.day1$steps): object 'steps.per.day1' not found
```

```r
median(steps.per.day1$steps)
```

```
## Error in median(steps.per.day1$steps): object 'steps.per.day1' not found
```

### The impact of the missing data is neglegible, we obtain roughly the same results as without adding missing values. Of course, this is because we added values that are related to the data. If we would have added any other numbers, the mean and median would have probably changed.

## Are there differences in activity patterns between weekdays and weekends?


```r
data$days[weekdays(as.Date(data$date)) %in% c("Saturday", "Sunday")] <- "Weekend"
```

```
## Error in `*tmp*`$days: object of type 'closure' is not subsettable
```

```r
data$days[!weekdays(as.Date(data$date)) %in% c("Saturday", "Sunday")] <- "Weekday"
```

```
## Error in `*tmp*`$days: object of type 'closure' is not subsettable
```

```r
data$days <- as.factor(data$days)
```

```
## Error in data$days: object of type 'closure' is not subsettable
```

### Make a panel plot containing a time series and the average number of steps taken, averaged accross all weekday days or weekend days


```r
steps.per.weekday <- aggregate(steps~interval, data=data, subset
=data$days=="Weekday", FUN=mean)
```

```
## Error in as.data.frame.default(data, optional = TRUE): cannot coerce class ""function"" to a data.frame
```

```r
steps.per.weekend <- aggregate(steps~interval, data=data, subset=data$days=="Weekend", FUN=mean)
```

```
## Error in as.data.frame.default(data, optional = TRUE): cannot coerce class ""function"" to a data.frame
```

```r
par(mfrow=c(2,1))
plot(steps.per.weekday,type="l",main="Weekday")
```

```
## Error in plot(steps.per.weekday, type = "l", main = "Weekday"): object 'steps.per.weekday' not found
```

```r
plot(steps.per.weekend,type="l",main="Weekend")
```

```
## Error in plot(steps.per.weekend, type = "l", main = "Weekend"): object 'steps.per.weekend' not found
```

