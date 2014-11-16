Reproductive Data Analysis: Assignment 1
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
attach(activity, warn.conflicts=F)
```

```
## Error in attach(activity, warn.conflicts = F): object 'activity' not found
```

```r
# Aggregate the sum of steps of activiy by date ignoring NA:
aggSteps<-aggregate(activity[,c(1,3)], by=list(date), FUN=sum, na.rm=TRUE)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(date), FUN = sum, na.rm = TRUE): object 'activity' not found
```

```r
# Create a Histogram of the above data:
hist(aggSteps$steps, main="Total Number of Steps Taken Each Day",xlab = "Steps")
```

```
## Error in hist(aggSteps$steps, main = "Total Number of Steps Taken Each Day", : object 'aggSteps' not found
```


2: Calculate the mean and median total number of steps per day:

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
# Aggregate the mean of steps of activity by the intervals:
aggInt<-aggregate(activity[,c(1,3)], by=list(interval), FUN=mean,na.rm=T)
```

```
## Error in aggregate(activity[, c(1, 3)], by = list(interval), FUN = mean, : object 'activity' not found
```

```r
# Load ggplot2:
library(ggplot2)

# Create the plot of the above data:
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





