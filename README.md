---
title: "Test"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
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
```{r}
output = NULL
for(i in length(id)){
  output[[i]] = NPV(rep(0, length(id)), cf = rep(wagesPerYearLost[[i]], length(id)), times = time[[i]])
}

NPV(0, , times = c(1,2,3), i = .03)
51.75*2*3+69*3
```


Need to get indiviudal estimates for 
```{r}
library(FinancialMath)

```






