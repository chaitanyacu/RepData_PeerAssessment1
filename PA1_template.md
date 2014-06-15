# Reproducible Research Peer Assginment 1
========================================================

## Loading Foot Step Activity Data


```r
activityDF<-read.csv("activity.csv")
summary(activityDF)
```

```
##      steps               date          interval   
##  Min.   :  0.0   2012-10-01:  288   Min.   :   0  
##  1st Qu.:  0.0   2012-10-02:  288   1st Qu.: 589  
##  Median :  0.0   2012-10-03:  288   Median :1178  
##  Mean   : 37.4   2012-10-04:  288   Mean   :1178  
##  3rd Qu.: 12.0   2012-10-05:  288   3rd Qu.:1766  
##  Max.   :806.0   2012-10-06:  288   Max.   :2355  
##  NA's   :2304    (Other)   :15840
```
##
Compute steps per day

```r
totalstepsperdayDF<-aggregate(activityDF$steps ~ activityDF$date, data = activityDF, sum,na.rm = TRUE)
```


## Histogram

```r
hist(totalstepsperdayDF[,2],xlab='Total Steps per day',ylab='Number of days',main="Histogram of number of steps each day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

## Mean and Median total number of steps taken per day

Mean = 1.0766 &times; 10<sup>4</sup>
Median = 10765


## Average daily activity pattern

```r
averagestepsperintervalDF<-aggregate(activityDF$steps ~ activityDF$interval, data = activityDF, mean,na.rm = TRUE)
plot( averagestepsperintervalDF[,1], averagestepsperintervalDF[,2],ylim=c(0,500),ylab="Average Steps", xlab="Intervals" ,type="l") 
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 



Interval with Max Average steps = 835


## Imputing missing values



Total number of missing values in the dataset = 2304


## Filling in all of the missing values in the dataset with mean interval steps


```r
colnames(averagestepsperintervalDF)[1] <- "interval"
colnames(averagestepsperintervalDF)[2] <- "avg_steps"

for(i in 1:nrow(activityDF)) {
  
    if(is.na(activityDF[i,]$steps)){
      
      activityDF[i,]$steps<-averagestepsperintervalDF[averagestepsperintervalDF$interval==activityDF[i,]$interval,2]
     
    }
}
```

### Histogram after refilling NA's


Compute steps per day

```r
totalstepsperdayDF<-aggregate(activityDF$steps ~ activityDF$date, data = activityDF, sum,na.rm = TRUE)
```

New Historgram after refilling NA's


```r
hist(totalstepsperdayDF[,2],xlab='Total Steps per day',ylab='Number of days',main="Histogram of number of steps each day")
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 


New Mean and Median total number of steps taken per day after refilling NA's


Mean = 1.0766 &times; 10<sup>4</sup>
Median = 1.0766 &times; 10<sup>4</sup>


## differences in activity patterns between weekdays and weekends

