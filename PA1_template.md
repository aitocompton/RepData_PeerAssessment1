Reproductive Data Analysis: Assignment 
==
### 11/16/2014 



### Loading and procesing the data


```r
activity<-read.csv("Downloads/activity.csv", quote="\"'", stringsAsFactors=F)
```

```
## Warning in file(file, "rt"): cannot open file 'Downloads/activity.csv': No
## such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```
1: Transforming the date variable of the activity data into the date class:


```r
activity$date<-as.Date(activity$date)
```

```
## Error in as.Date(activity$date): object 'activity' not found
```

### The Mean Total Number of Steps Taken Per Day

1: Make a histogram of the total number of steps taken each day:

```r
attach(activity)
```

```
## Error in attach(activity): object 'activity' not found
```

```r
aggSteps<-aggregate(activity[,c(1,3)], by=list(date), FUN=sum, na.rm=TRUE)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(date), FUN = sum, na.rm = TRUE): object 'activity' not found
```

```r
hist(aggSteps$steps, main="Total Number of Steps Taken Each Day",xlab = "Steps")
```

```
## Error in hist(aggSteps$steps, main = "Total Number of Steps Taken Each Day", : object 'aggSteps' not found
```


2: Calculate the mean and median total number of steps peer day:

```r
mean(aggSteps$steps)
```

```
## Error in mean(aggSteps$steps): object 'aggSteps' not found
```

```r
median(aggSteps$steps)
```

```
## Error in median(aggSteps$steps): object 'aggSteps' not found
```

### The Average Daily Activity Pattern
1: Make a time series plot of the 5-minutes interval (x-axis) and the average number of steps taken, averaged across all days (y-axis):

```r
aggInt<-aggregate(activity[,c(1,3)], by=list(interval), FUN=mean,na.rm=T)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(interval), FUN = mean, : object 'activity' not found
```

```r
library(ggplot2)
qplot(interval, steps,data=aggInt , geom="line")
```

```
## Error in ggplot(data, aesthetics, environment = env): object 'aggInt' not found
```

2: Find which 5-minute interval, on average across all the days in the dateset, contains the maximum number of steps:


```r
aggInt[which(aggInt$steps==max(aggInt$steps)),]$interval
```

```
## Error in eval(expr, envir, enclos): object 'aggInt' not found
```

### Imputing missing values

1: Calculate and report the totla number of missing values in the dataset:


```r
# Since missing values exist in the step data, the totla number of the missing value can be derived from this variable:
sum(with(activity, is.na(steps)))
```

```
## Error in with(activity, is.na(steps)): object 'activity' not found
```

2: Using the mean of the steps for 5-minute interval to fill in the missing values: 


```r
# create the vector matching the missing values of steps with the mean values of the steps for 5-minute interval:

actNA<-aggInt[match(activity[is.na(activity),3],aggInt[,3]),2]
```

```
## Error in eval(expr, envir, enclos): object 'aggInt' not found
```

```r
# fill in the missing values of the steps in the activity with the vector, actNA, creatd above:

activity[is.na(activity),]$steps<-actNA
```

```
## Error in eval(expr, envir, enclos): object 'actNA' not found
```

3: Aggregate the dataframe, activity, by the total number of steps taken each day: 

```r
aggSteps2<-aggregate(activity[,c(1,3)], by=list(date), FUN=sum, na.rm=TRUE)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(date), FUN = sum, na.rm = TRUE): object 'activity' not found
```

4: Make a histogram of the dataframe, aggSteps2, created above:

```r
hist(aggSteps2$steps, main="Total Number of Steps Taken Each Day", xlab="Steps")
```

```
## Error in hist(aggSteps2$steps, main = "Total Number of Steps Taken Each Day", : object 'aggSteps2' not found
```

5:Calculate and report the mean and median total number of steps taken per day: 

```r
mean(aggSteps2$steps)
```

```
## Error in mean(aggSteps2$steps): object 'aggSteps2' not found
```

```r
median(aggSteps2$steps)
```

```
## Error in median(aggSteps2$steps): object 'aggSteps2' not found
```

The mean and median total number of steps derived from aggSteps2 differ from the mean and median total number of steps derived from  aggSteps. The imputing missing data on the estimate of the total activity of daily number steps increased the mean and median of the daily steps. 

### Differences in activity patterns between weekdays and weekends

1: Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend:


```r
#create a factor variable classfying weekdayas and weekends of the dataframe, activity:

activity[which(weekdays(activity$date)=="Saturday"|weekdays(activity$date)=="Sunday"),4]<-"weekends"
```

```
## Error in activity[which(weekdays(activity$date) == "Saturday" | weekdays(activity$date) == : object 'activity' not found
```

```r
activity[which(!weekdays(activity$date)=="Saturday" & !weekdays(activity$date)=="Sunday"),4]<-"weekdays"
```

```
## Error in activity[which(!weekdays(activity$date) == "Saturday" & !weekdays(activity$date) == : object 'activity' not found
```

```r
# Convert the classification variable into the factor variable: 
activity[,4]<-as.factor(activity[,4])
```

```
## Error in is.factor(x): object 'activity' not found
```

2: Make a panel plot containing a time series plot of the 5-minute interval and the average number of steps taken, averaged across all weekdays or weekends:


```r
attach(activity)
```

```
## Error in attach(activity): object 'activity' not found
```

```r
aggAct<-aggregate(activity[,c(1,3)], by=list(interval,V4), FUN=mean,na.rm=T)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(interval, V4), FUN = mean, : object 'activity' not found
```

```r
# Creat the layer of the ggplot for aggData:
sp <- ggplot( aggAct, aes(x=interval, y=steps)) + geom_line()
```

```
## Error in ggplot(aggAct, aes(x = interval, y = steps)): object 'aggAct' not found
```

```r
# Split the plots according to weekdays and weekends:
sp +facet_grid(Group.2~.)
```

```
## Error in eval(expr, envir, enclos): object 'sp' not found
```
