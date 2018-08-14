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
nTrainedSamp = c(10:20)
nTrained = sample(nTrainedSamp, 1) 
id = 1:nTrained[1]
nServedSamp = c(10:30)
nServed = sample(nServedSamp, 1)
served = rep(sample(nServedSamp, length(id)))
costDat = data.frame(id = id, served = served)
head(costDat)
```
Now I am trying to estimate the reductions in sucicde rates and attempts.  Just go with 1% for rates and 5% for attempts and sample them from a uniform distribution.
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
This is the time horizon for the number of clients being served
These numbers are from the Rand study.
```{r}
timeClientsSample = c(3:14)
timeClients = rep(sample(timeSample, length(id), replace = TRUE))
timeClients = as.vector(time)
```
Now we need to estimate the medical costs 
We will need to do something above, because all the time horizons are different.
We are looking at lost wages due to death, which can be from $0 to $50000 and lost wages for attempt which is one day of the year so $0 to $50000 / 365.   

For lost wages per year, need to figure out how to increase that amount over time.
May need to create a sample for each person
```{r}
wagesPerYearLostSamp = c(0:50000)
wagesPerYearLost = rep(sample(wagesPerYearLostSamp, length(id)), replace = TRUE)
wagesPerYearLost = wagesPerYearLost*reductSucRate
wagesPerYearLost = as.vector(wagesPerYearLost)
wagesPerYearLost

wagesDayLostSamp = round(c(0:50000/365),2)
wagesDayLost = rep(sample(wagesDayLostSamp, length(id)), replace = TRUE)
wagesDayLost = wagesDayLost*reductSucAttempt
wagesDayLost = as.vector(wagesDayLost)
```
Assume that we are working with adults who range in age from 18 to 65.  
Then get the differnece between the age and 65 and that is the number years for the lost wages
```{r}
set.seed(123)
timeWagesSamp = c(18:65)
timeWages = sample(timeWagesSamp, length(id), replace = TRUE)
timeWages = 65-timeWages
timeWages
```
Now we need to wages to rep for the length of time 
Because I want the wages to be the same for each time point

If we have their actual wages, do we need to discount for demographics and or add in an unemployment factor?
```{r}
benefitsWagesRate = NULL
for(i in 1:length(id)){
  benefitsWagesRate[[i]] = npv(r = .03, rep(wagesPerYearLost[[i]], timeWages[[i]]))
}
benefitsWagesRate = sum(benefitsWagesRate); benefitsWagesRate

benefitsWagesAttempt = NULL
for(i in 1:length(id)){
  benefitsWagesAttempt[[i]] = npv(r = .03, rep(wagesDayLost[[i]], timeWages[[i]]))
}
benefitsWagesAttempt = sum(benefitsWagesAttempt); benefitsWagesAttempt
```
Need benefits for sucide attempts now just wage losses for one day of work

For medical costs, assume they are drawn from a normal distribution with mean of $30,000

Then mulitply the medical costs for the number of people to get medical costs savings per therapist

Served is per year so it is fine to rep the medical costs saved over the years the client is working.
```{r}
medicalCostsRate = round(rnorm(mean = 30000, sd = 1000, n = length(id)),2)
medicalCostsRate = round(medicalCosts*served,2)
medicalCostsRate

benefitsMedicalRate = NULL
for(i in 1:length(id)){
  benefitsMedicalRate[[i]] = npv(r = .03, rep(medicalCostsRate[[i]], timeClients[[i]]))
}
benefitsMedicalRate = sum(benefitsMedicalRate); benefitsMedicalRate

medicalCostsAttempt = round(rnorm(mean = 5000, sd = 1000, n = length(id)),2)

benefitsMedicalAttempt = NULL
for(i in 1:length(id)){
  benefitsMedicalAttempt[[i]] = npv(r = .03, rep(medicalCostsAttempt[[i]], timeClients[[i]]))
}
benefitsMedicalRate = sum(benefitsMedicalRate); benefitsMedicalRate


totalBenefits = sum(benefitsWagesRate, benefitsWagesAttempt, benefitsMedicalAttempt, benefitsMedicalRate)
```
So now get the program costs.  Assume a therapists makes about $50,000
So we have length(id) therapists, then they make distibution around $50,000 for timeClient
```{r}
therpWage = rnorm(mean = 50000, sd = 2000, n = length(id))
therpWage

costsTherpWage = NULL
for(i in 1:length(id)){
  costsTherpWage[[i]] = npv(r = .03, rep(therpWage[[i]], timeClients[[i]]))
}
costsTherpWage = sum(costsTherpWage); costsTherpWage
```
Now get the BCA ratio 
```{r}
totalBenefits/costsTherpWage
```








