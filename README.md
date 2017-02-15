# GA_YOY_Productcompare
R script that returns a year over year comparison of revenue from products in GA profile

```
library(RGA)
library(ggplot2)
library(scales)
library(lubridate)
library(zoo)

#Authorization for RGA
authorize()

##Enter your view ID here
profile <- "XXXXXXXX"


##Enter your start and end date here
start.date = "2015-01-01"
end.date = "2016-09-30"

##RGA script
ga <- get_ga(profile, start.date, end.date, metrics = "ga:itemRevenue",
             dimensions = "ga:yearMonth,ga:year,ga:productName")

##Zoo script converts YYYYMM format date to YYYY-MM-DD
ga$yearMonth <- as.Date(as.yearmon(ga$yearMonth, "%Y%m"))

##Lubridate script converts month to monthname for X axis of graphs
ga$monthname <- month(ga$yearMonth, label = TRUE)

##GGplot script creates line graph
ggplot(ga, aes(monthname, itemRevenue, color = year, group = year)) + geom_line(size = 1) +

##converts any integer Y axis to continuous scale and adds a comma for easy reading   
scale_y_continuous(labels = comma)  +
  
##creates a facet for multiple graphs by productname and forces Y axis to independent scales  
facet_wrap(~ productName, ncol=4, scales = "free_y")```
