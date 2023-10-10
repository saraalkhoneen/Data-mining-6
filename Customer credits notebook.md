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
|checking_status    |Nominal    |status of existing checking acount of loan applicant  |
|duration           |Numeric    |duration of loan in months         |
|credit_history     |Nominal    |credit history of loan applicant                |
|savings_status     |Nominal    |status of savings accounts or bonds of the loan applicant    |
|age                |Numeric    |Age of the loan applicant years       |
|employment         |Nominal    |current employment status of the loan applicants in number of years              |
|other_payment_plans|Nominal |represents other payment plans associated with the loan applicant                |
|personal_status    |Nominal     | represents the sex and martial status of the loan applicant   |
|housing            |Nominal     |The housing situation of the applicants| 
|class              |Nominal     |represent wether a credit risk exists or not |
|propetry_magnitude |Nominal     |the magnitude of the property owned by the loan applicant         |

## Summary of the dataset

```R
dataset = read.csv('dataset.csv')
```

```R
nrow(dataset) 
```
The output is 1000, which is the number of rows

```R
 ncol(dataset)
```
The output is 11, which is the number of columns

```R
 summary(dataset)
```
```R
var_duration <- var(dataset$duration)
```
```R
var_age <- var(dataset$age)
```
we got a closer look at our data.
1- The length 
2- Class type 
3- Central tendancy (mode, mean and median) of each attribute with the Q1 and Q3
4- Variance 

## Missing Values and Null Values

```R
is.na(dataset)
```
The output is false for all atrributes , this indicates that the corresponding element in our dataset is not missing and contains a valid value.

```R
sum(is.na(dataset))
```
We used this function to reassure we dont have missing values or Nulls in the the entire dataset the output is 0. 

## Outliers

```R
library(outliers)
```
```R
OutAge <- outlier(dataset$age, logical = TRUE)
OutDuration <- outlier(dataset$duration, logical = TRUE)
```
We created a variable "OutAge" , "OutDuration" to store the result of finding the outliers in the dataset , 
logical true which specifies the outliers with true .

```R
sum(OutAge)
sum(OutDuration)
```
Then we calculated the sum of All the outliers, the result is 2 for the age / 1 for the duration. 

```R
Find_outlierAge <- which(OutAge == TRUE, arr.ind = TRUE)
Find_outlierDuration <- which(OutDuration == TRUE, arr.ind = TRUE)
```
To find the row nummbers with the Outliers 

```R
dataset <- dataset[-Find_outlierAge ,-Find_outlierDuration , ]
```

Finally we removed the outliers , out dataset after remvoing the outliers have 997 objects.
## Data Conversion (Encoding categorical data)/discretization

```R
dataset$checking_status <- factor(dataset$checking_status, levels = c("<0", "0<=X<200", "no checking"), labels = c(1, 2, 3))
```
```R
dataset$class <- factor(dataset$class, levels = c("bad", "good"), labels = c(0, 1))
```
```R
dataset$housing = factor(dataset$housing,levels = c("own","for free", "rent"), labels = c(1, 2, 3))
```
 Print the final preprocessed dataset
```R
print(dataset)
```

## Normalization

Define the min_max_scaling() function
```R
min_max_scaling <- function(x) {return (x - min(x)) / (max(x)- min(x))}
```

Normalize 'age' variable
```R
dataset$age <- min_max_scaling(dataset$age)
```

Normalize 'duration' variable
```R
dataset$duration <- min_max_scaling(dataset$duration)
```

## Graphs 

```R
hist(dataset$age, main = "Histogram of Age", xlab = "Age", ylab = "Frequency", col = "lightblue")
```
We ceated a histogram for the "Age" attribute for its importance in deciding a customers credits risks 
, what we learned from the histogram is the age distribution of our dataset, which is an important factor in 
deciding the credit risks of a customer. 

```R
barplot(table(dataset$checking_status), main = "Bar Plot of Checking Status", 
+         xlab = "Checking Status", ylab = "Frequency", col = "lightgreen")
```
This chart will show the distribution of individuals across different checking status categories which then provide insights into the financial standing of the customers. 

```R
library(ggplot2)
```
```R
ggplot(data = dataset, aes(x = credit_history, fill = class)) +
     geom_bar(position = "stack") +
     labs(title = "Credit History vs. Credit Risk",
          x = "Credit History",
          y = "Count" ,
   fill = "credit history" )
```
The bar chart shows the distribution of credit history categories and how they are associated with good and bad credit risks, we used our class label which is the "class" attribute, to learn and understand how the credit history 
affects the decision when deciding a good or a bad credit risks for a customer.

```R
ggplot(dataset, aes(x = housing, fill = class)) +
    geom_bar() +
      labs(
          x = "Housing",
          y = "Count",
          title = "Housing vs. Credit Risk",
          fill = "Credit Risk"
      )
```
This bar chart shows what kind of credit risk customers have based on their housing, we noticed that a big percentage of "for free" type of housing have a bad credit, 
but to assist that we decided to define functions that will help us determine if our analysis to the chart was correct.

```R
forfree_bad_count <- nrow (subset(dataset,housing == "for free" & class == "bad"))
```
using this function, we discovered that more than 40% of customers who live in their houses for free have bad credits.


```R
ggplot(dataset, aes(x = employment, fill = class)) +
   geom_bar() +
     labs(
         x = "employment",
         y = "Count",
         title = "employment vs. Credit Risk",
         fill = "Credit Risk"
     )
```


## Feature Selection : 
Insuring that caret package is installed on install it : 

```R
if (!require(caret)) {
  install.packages("caret")
}

library(caret)
```

Choosing the feature matrix (X) and target variable (Y) :

```R
X <- dataset[, c("credit_history", "savings_status", "employment")]
y <- dataset$class
```

Defining the number of cross-validation folds :

```R
CV_folds <- 10
train_control <- trainControl(method = "cv", number = CV_folds)
```

Use glm method :

```R
method <- "glm"
model <- train(X, y, method = method, trControl = train_control, metric = "AUC")
selected_features <- varImp(model)
print(selected_features)
```
