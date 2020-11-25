# Logistic-Regression-Generalized-linear-model-and-quantile-regression
---
title: "PROJECT MODE"
output:
  pdf_document: default
  html_document: default
  word_document: default
date: "November 2020"
theme: cerulean
---

<!-- For more info on RMarkdown see http://rmarkdown.rstudio.com/ -->


### Logistic Regression, Generalized linear model and quantile regression.

## install.packages("quantreg")


```{r}
#Logistics Regression
## Using the Plantgrowth data set in R
### Converting the weight variable (continuous) to a binary variable using the cut function.
philant.weight<- cut(PlantGrowth$weight, 2, labels = c("light", "heavy"))
### using a binomial response.
logis.regres<-glm(philant.weight~PlantGrowth$group, family = binomial)
summary(logis.regres)

```


```{r}
### Using other link functions 
### Using the probit model. which is the inverse of the normal CDF.

probit.regres<-glm(philant.weight~PlantGrowth$group, family = binomial(link = probit))
summary(probit.regres)

```

```{r}
### Using the complementary log- log model
comp.log.regres<-glm(philant.weight~PlantGrowth$group, family = binomial(link = cloglog))
summary(comp.log.regres)
```



## Generalized linear model.
```{r}
### Making an ANOVA for generalized linear model
##Remember this from above
logis.regres
anova(logis.regres, test="LRT")
```


```{r}
### Generating data from R for poison counts
counts<-c(12,34,56,78,94,67,87,23,45,56,78,11,52,34,56,16,54,67,43,20)
outcome<-gl(5,1,20)
treatment<-gl(5,4)
### Making it a dataframe
print(pat.data<-data.frame(treatment, outcome, counts))
pregres.model<- glm(counts~outcome+treatment,family = poisson())
summary(pregres.model)
```

```{r}
### ANOVA for the regression
anova(pregres.model, test = "LRT")
```


### Quantile linear regression. this helps to examine how the distribution of Y varies with X

```{r}
library(quantreg)
data(engel)
View(engel)

plot(engel$income, engel$foodexp, xlab = "Household Income" ,ylab = "Household Expenditure", type ="n", cex=.5)
points(engel$income, engel$foodexp, cex=.5, col="blue")
abline(lm(foodexp~income, data = engel), col="red", lty=2)
legend(3000, 500,c("mean fit"), col = c("red"), lty = c(2))
```

```{r}
### Adding a median regression fit 
abline(rq(foodexp~income,data = engel), col="blue")
### This line passes through the median quantile.
```

```{r}
### Setting several values for tau
taus<-c(.05,.1,.25,.75,.9,.95)
quan.fit<-rq(foodexp~income, tau=taus, data = engel)
summary(quan.fit)
```


```{r}
### Plotting the taus
x<-seq(min(engel$income), max(engel$foodexp), 100)
f<-coef(quan.fit)
y<-cbind(1,x)%*%f
for(i in 1:length(taus)){
  lines(x,y[,i], col="gray")
}

### Plots 
plot(summary(rq(foodexp~income,tau = 1:49/50, data = engel)))

```

















