## Project Motivation:
we will use this data set to assist financial institutions make informed decisions regarding loan approvals by analyzing customers’ payment behavior, as well as all their relevant attributes such as age, income, loan amount, employment status, etc. By utilizing this data, predictive models can be built to forecast customers’ creditworthiness and may be used as a tool for risk assessment and decision-making.

## source

The dataset was sourced from the kaggle website in this URL:
https://www.kaggle.com/datasets/ppb00x/credit-risk-customers 

## General information 

1- Number of Attributes: 11
2- Number of Objects: 1001
3- Type of Attributes:
4- class name and lable:
The "class" which describes whether the customer is a good or bad credit risks.

## attribute description

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
the output is 1000, which is the number of rows

```{r}
 ncol(dataset)
```
the output is 11, which is the number of columns

```{r}
 summary(dataset)
```
we got a closer look at our data.
1- the length 
2- class type 
3- central tendancy (mode, mean and median) of each attribute with the Q1 and Q3

## Missing Values 

```{r}
sum(is.na(dataset))
```
the output is 0 , which means there is not missing values
