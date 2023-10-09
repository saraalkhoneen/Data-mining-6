### Project Motivation:
we will use this data set to assist financial institutions make informed decisions regarding loan approvals and deciding weather a customer has good or bad cedit risks by analyzing customers’ payment behavior, as well as all their relevant attributes such as age, income, loan amount, employment status, etc. By utilizing this data, predictive models can be built to forecast customers’ creditworthiness and may be used as a tool for risk assessment and decision-making.

### source

The dataset was sourced from the kaggle website in this URL:
https://www.kaggle.com/datasets/ppb00x/credit-risk-customers 

## General information 

1- Number of Attributes: 11
2- Number of Objects: 1001
3- class name and lable:
The "class" attribute which describes whether the customer is a good or bad credit risks.

## Attribute description

| Attribute name    | Data type |            Description             |
|-------------------|-----------|------------------------------------|
|checking_status    |Nominal    |the validity of credit card number  |
|duration           |Numeric    |duration                            |
|credit_history     |Nominal    |dept and other loans                |
|savings_status     |Nominal    |if the customer is saving or not    |
|age                |Numeric    |Age Of The credit customer          |
|employment         |Nominal    |The employment status               |
|other_payment_plans|Nominal |Other ways of payment                  |
|personal_status    |Nominal     |single or married, male/female     |
|housing            |Nominal     |The housing situation rent or owned| 
|class              |Nominal     |good or bad credit risks           |


## Summary of the dataset

```{r}
dataset = read.csv('dataset.csv')
```

```{r}
nrow(dataset) 
```
The output is 1000, which is the number of rows

```{r}
 ncol(dataset)
```
The output is 11, which is the number of columns

```{r}
 summary(dataset)
```
```{r}
var_duration <- var(dataset$duration)
```
```{r}
var_age <- var(dataset$age)
```
we got a closer look at our data.
1- The length 
2- Class type 
3- Central tendancy (mode, mean and median) of each attribute with the Q1 and Q3
4- Variance 

## Missing Values and Null Values

```{r}
is.na(dataset)
```
The output is false for all atrributes , this indicates that the corresponding element in our dataset is not missing and contains a valid value.

```{r}
sum(is.na(dataset))
```
We used this function to reassure we dont have missing values or Nulls in the the entire dataset the output is 0. 

## Outliers

```{r}
library(outliers)
```
```{r}
OutAge <- outlier(dataset$age, logical = TRUE)
OutDuration <- outlier(dataset$duration, logical = TRUE)
```
We created a variable "OutAge" , "OutDuration" to store the result of finding the outliers in the dataset , 
logical true which specifies the outliers with true .

```{r}
sum(OutAge)
sum(OutDuration)
```
Then we calculated the sum of All the outliers, the result is 2 for the age / 1 for the duration. 

```{r}
Find_outlierAge <- which(OutAge == TRUE, arr.ind = TRUE)
Find_outlierDuration <- which(OutDuration == TRUE, arr.ind = TRUE)
```
To find the row nummbers with the Outliers 

```{r}
dataset <- dataset[-Find_outlierAge ,-Find_outlierDuration , ]
```

Finally we removed the outliers , out dataset after remvoing the outliers have 997 objects.
## Data Conversion (Encoding categorical data)/discretization

```{r}
dataset$checking_status <- factor(dataset$checking_status, levels = c("<0", "0<=X<200", "no checking"), labels = c(1, 2, 3))
```
```{r}
dataset$class <- factor(dataset$class, levels = c("bad", "good"), labels = c(0, 1))
```
```{r}
dataset$housing = factor(dataset$housing,levels = c("own","for free", "rent"), labels = c(1, 2, 3))
```
 Print the final preprocessed dataset
```{r}
print(dataset)
```
## Feature Scaling
Apply feature scaling to non-categorical data
```{r}
code here 
```
## Normalization

Define the min_max_scaling() function
```{r}
min_max_scaling <- function(x) {return (x - min(x)) / (max(x)- min(x))}
```

 Normalize 'age' variable
 ```{r}
dataset$age <- min_max_scaling(dataset$age)
```

 Normalize 'duration' variable
 ```{r}
dataset$duration <- min_max_scaling(dataset$duration)
```

## Graphs 

```{r}
hist(dataset$age, main = "Histogram of Age", xlab = "Age", ylab = "Frequency", col = "lightblue")
```
We ceated a histogram for the "Age" attribute for its importance in deciding a customers credits risks 
, what we learned from the histogram is the age distribution of our dataset, which is an important factor in 
deciding the credit risks of a customer. 

```{r}
barplot(table(dataset$checking_status), main = "Bar Plot of Checking Status", 
+         xlab = "Checking Status", ylab = "Frequency", col = "lightgreen")
```
This chart will show the distribution of individuals across different checking status categories which then provide insights into the financial standing of the customers. 

```{r}
library(ggplot2)
```
```{r}
ggplot(data = dataset, aes(x = credit_history, fill = class)) +
    geom_bar(position = "stack") +
     labs(title = "Credit History vs. Credit Risk",
          x = "Credit History",
          y = "Count") +
     scale_fill_manual(values = c("good" = "green", "bad" = "red")) 

```
The bar chart shows the distribution of credit history categories and how they are associated with good and bad credit risks, we used our class label which is the "class" attribute, to learn and understand how the credit history 
affects the decision when deciding a good or a bad credit risks for a customer.


 
