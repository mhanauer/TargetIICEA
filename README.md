---
title: "Test"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Load packages
```{r}
library(FinCal)
```
SNET Cost Effect Analysis (CEA)
Need to simluate the number of people trained and then an approximate number of people that they will serve.  
```{r}
set.seed(123)
nTrainedSamp = c(10:20)
nTrained = sample(nTrainedSamp, 1) 
id = 1:nTrained[1]
nServedSamp = c(10:30)
nServed = sample(nServedSamp, 1)
served = rep(sample(nServedSamp, length(id)))
costDat = data.frame(id = id, served = served)
head(costDat)
```
Now reductions in sucicide rates maybe like 5% reduction
Now reductions in sucicide attempts 10%
```{r}
set.seed(123)
sucRate <- seq(.01, .05, by = .001) 
for(i in sucRate) { 
} 
sucRate = sample(sucRate, 1)
sucRate

sucAttempt <- seq(.01, .1, by = 0.005) 
for(i in sucAttempt) { 
} 
sucAttempt = sample(sucAttempt, 1)
sucAttempt
```
I want to take the number of people served times the suc rate and suc attempt and those will be my reductions.
```{r}
reductSucRate = costDat$served*sucRate
reductSucAttempt = costDat$served*sucAttempt
```
Then I want to randomly sample the time horizon for each clincian 
```{r}
timeSample = c(3:14)
time = rep(sample(timeSample, length(id), replace = TRUE))
time = as.vector(time)
```
Now we need to estimate the medical costs 
We will need to do something above, because all the time horizons are different.
Just assume lost wages are $30,000 per year
Will need to figure out how to increase this amount later on

Just figuring out from rate first figure out attempt later on
```{r}
wagesPerYearLostSamp = c(0:50000)
wagesPerYearLost = rep(sample(wagesPerYearLostSamp, length(id)), replace = TRUE)
wagesPerYearLost = wagesPerYearLost*reductSucRate
wagesPerYearLost = as.vector(wagesPerYearLost)
wagesPerYearLost
```
Now we need to wages to rep for the length of time 
Because I want the wages to be the same for each time point
```{r}
output = NULL
for(i in 1:length(id)){
  output[[i]] = npv(r = .03, rep(wagesPerYearLost[[i]], time[[i]]))
}
output

# So it is only working for the last one.

# This works.  So why can't I run a loop over this????
npv(r = .03, rep(wagesPerYearLost[13], time[13]))


length(wagesPerYearLost)
length(time)
```






