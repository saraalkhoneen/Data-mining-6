

### Project Motivation:
In this project, our primary goal is to classify customers as either good or bad credit risks. We aim to assist financial institutions in making informed decisions regarding loan approvals based on an analysis of customers' payment behavior and other relevant attributes, such as age, income, loan amount, and employment status. By utilizing this dataset, we will build predictive models to assess customers' creditworthiness and use these models as tools for risk assessment and decision-making.

### source
The dataset was sourced from the kaggle website in this URL:
  https://www.kaggle.com/datasets/ppb00x/credit-risk-customers 

## General information 
Origonaly our dataset consists of 21 attributes, but we only worked with 21 attributes that are going to help with our study of the credit risks of applicants.
1- Number of Attributes: 21
2- Number of Objects: 1001
3- class name and lable:
  The "class" attribute which describes whether the customer has a good or bad credit risks.

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
  |credit_amount |numeric     |This attribute represents the amount of credit being requested.  |
  |residence_since |numeric     |This attribute represents the number of years the loan applicant has been residing at their current residence>  |
  |other_parties |nominal     |This attribute represents the other debtors or guarantors associated with the 'loan. It can take one of the following values: 'none', 'co-applicant', or 'guarantor.|
  |foreign_worker |Nominal |This attribute represents whether the loan applicant is a foreign worker or 'not. It can take one of the following values: 'yes' or 'no.|
  |purpose |nominal     |This attribute represents the purpose of the credit for which the loan is being taken. It can take one of the following values: 'car (new)', 'car (used)', 'furniture/equipment', 'radio/television', 'domestic appliances', 'repairs', 'education', 'vacation', 'retraining', or ''business.|
  |existing_credits |nominal     |This attribute represents the number of existing credits the loan applicant has at the time of the loan application. |
  |num_dependents |numeric     |This attribute represents the number of dependents the loan applicant has. |
  |Own_telephone |numeric     |This attribute represents whether the loan applicant has their own 'telephone line or not. It can take one of the following values: 'none', or 'yes.|
  
## Summary of the dataset
  
```{r}
setwd("C:/Users/jory0/OneDrive/Desktop/DataMining Phase 3")
```

```{r}
dataset <- read.csv("credit_customers.csv")
```


```{r}
nrow(dataset) 
```

The output is 1000, which is the number of rows
```{r}
ncol(dataset)
```

The output is 21 , which is the number of columns

```{r}
summary(dataset)
```
This summary provides an initial understanding of the data's structure and characteristics. 
the min and max in different catagories would help us know the range of the attributes, therefore help us in dealing with the outliers.
the mean and median gave us insights that will be helpful during the preproccesing part.

now we want to calculate the variance for the numeric attributes to learn about the spread of the data and how close they are from the mean.

```{r}
var_duration <- var(dataset$duration)
```

```{r}
var_age <- var(dataset$age)
```

```{r}
var_amount <- var(dataset$credit_amount)
```


```{r}
print(var_duration)
```

A variance of 145.415 suggests that the data points in the "duration" attribute have some degree of variability. Data points are not tightly clustered around the mean, and there are variations in the durations.


```{r}
print(var_age)
```
129.4013 for the "age" attribute indicates that there is variability in the ages of individuals represented in the dataset. The ages are not highly consistent, and there are differences among individuals.

```{r}
print(var_amount)

```
A very high variance of 7,967,843 for the "credit_amount" attribute suggests a significant spread or variability in the credit amounts requested by individuals in the dataset. Some requests may be substantially higher or lower than the mean credit amount, indicating a wide range of credit requests.


we got a closer look at our data using these statical mearues: 
1- The length 
2- Class type 
3- Central tendancy (mode, mean and median) of each attribute with the Q1 and Q3
4- Variance 
now that we have a deeper insights, we want want to explore and visualize the data further using graphs. 

## Graphs 

1- We created a bar chart for the class label to know if our dataset is balanced or imbalanced 

```{r}
library(ggplot2)
```

```{r}
ggplot(dataset, aes(x = class)) +
  geom_bar() +
  labs(x = "Good Or Bad Credit Score", y = "Count") +
  ggtitle("Credit Risks Distribution") 

```
From this bar chart we can see that we do not have an balanced dataset, with 70% 'good' and 30% 'bad' labeled records this dataset is acceptable as it is and we do not need to take further actions regarding this matter.


2- 

```{r}
ggplot(dataset, aes(x = housing, fill = class)) +
  geom_bar() +
  labs(
    x = "Housing",
    y = "Count",
    title = "Credit Risks based on Housing",
    fill = "Credit Risk"
  )
```
From this graph we interpreted that most customers that have their own house have good credit risks implying that having an owned housing 
increases the chances of getting good credit risks, however there are 15% of people that have their own house still have bad credit risks 
meaning there are other factors in deciding the credits risks. 
renting and housing for free customers mostly have good credits risks. 


```{r}
forfree_bad_count <- nrow (subset(dataset,housing == "for free" & class == "bad"))

```

using this function, we discovered that more than 40% of customers who live in their houses for free have bad credits.


3- 
```{r}
ggplot(dataset, aes(x = checking_status, fill = class)) +
  geom_bar(position = "dodge", stat = "count") +
  labs(title = "Distribution of Checking Status by Class",
       x = "Checking Status",
       y = "Count",
       fill = "Class")
```
this graphs showed us how different checking status is distributed with our class atrribute
most costumers that have no checking status is considered having good credit risks 
meaning having no checking status increases the chances of getting good credit risks 

4-

```{r}
hist(dataset$age, main = "Histogram of Age", xlab = "Age", ylab = "Frequency", col = "lightblue")
```
We ceated a histogram for the "Age" variable to show the age groups for our sample of customers to give a better understanding for our overall variables values and results.  
most of the custemers age lies between (20 - 50) some custemers being a bit older 
there is some outliers in the age 70 that we will study in the outliers section.

5-

```{r}
plot(dataset$credit_amount, dataset$installment_duration,
     xlab = "Credit Amount", ylab = "duration",
     main = "Credit Amount and duration")
```
We created a Scatter plot for the "credit amount" and "duration" attributes to see if there is a correlation between them, from this scatter plot we can say that there is no clear correlation between the duration variable and the credit amount variable

6-
```{r}


# Create a grouped violin plot with 'checking_status' on the x-axis and 'credit_amount' on the y-axis
ggplot(dataset, aes(x = checking_status, y = credit_amount)) +
  geom_violin(fill = "lightblue", color = "blue") +
  labs(x = "Checking Status", y = "Credit Amount", title = "Grouped Violin Plot")


```

The graph illustrates how the credit amounts are distributed within different categories of checking status. Each violin represents a category of checking status, and its width indicates the density of data points. Also whither there are variations in credit amounts based on checking status which is based on the graph, the checkings status varies in the credit amount.


## Outliers

First we find the range of each one of the attributes which is collected in the statical measures we did in the
first step using the "summary()" function, using the Min and Max to find the range. 
this would help us in finding if the outliers detected fall in the range or not

credit_amount range: [250 , 18424 ]

duration range: [4.0 ,72.0]

Age range [19.00 , 75.00]

```{r}
library(outliers)
```

```{r}
OutAge <- outlier(dataset$age, logical = TRUE)
OutDuration <- outlier(dataset$duration, logical = TRUE)
Outamount <- outlier(dataset$credit_amount, logical = TRUE)
```

We created a variable "OutAge" , "OutDuration" and "Outamount" to store the result of finding the outliers in the dataset , 
logical true which specifies the outliers with true .


```{r}

sum(OutAge)
sum(OutDuration)
sum(Outamount)


```

Then we calculated the sum of All the outliers, the result is 2 for the age / 1 for the duration. / 2  for the credit amount.
```{r}

Find_outlierAge <- which(OutAge == TRUE, arr.ind = TRUE)
Find_outlierDuration <- which(OutDuration == TRUE, arr.ind = TRUE)
Find_outlierAmount <- which(Outamount == TRUE, arr.ind = TRUE)

```

```{r}
outlierRowsAge <- dataset[Find_outlierAge, ]
outlierRowsDuration <- dataset[Find_outlierDuration, ]
outlierRowsAmount <- dataset[Find_outlierAmount, ]

```

```{r}
print(outlierRowsAge)
```

```{r}
print(outlierRowsDuration)
```

```{r}
print(outlierRowsAmount)
```



to find the exact row which the outliers are in.

```{r}
Find_outlierAge <- which(OutAge == TRUE, arr.ind = TRUE)
Find_outlierDuration <- which(OutDuration == TRUE, arr.ind = TRUE)
Find_outlierAmount <- which(Outamount == TRUE, arr.ind = TRUE)

```

Then we find the exact values of the outliers and study them. 

```{r}
cat("Outliers in credit_amount:", Find_outlierAmount, "\n")
cat("Outliers in duration:", Find_outlierDuration, "\n")
cat("Outliers in age:", Find_outlierAge, "\n")

```

Age: it is impossible to have an age of 331 or 537, this is obvesouly due to
data entry mistakes and/or human error. 
that is why we removed the Age outiers. 

credit_amount: in most financial contexts, a "credit amount" of 1 doesn't make sense. The credit amount is typically a numerical value representing the total amount of credit or loan that an individual or entity has borrowed. It is expected to be a positive numeric value that reflects the amount of money borrowed.
This is also due to data entry mistakes and/or human error. For the value 916 it falls in the range so we wont do anything to it. 
After removing the outlier with a value of '1' we are left with 1 outlier for the attribute 'credit_amount'. 


duration: Loan durations are typically measured in months, and values like 678 months (56+ years) and 1 month may indicate potential issues with the data which could also be due to data entry mistakes and/or human error. 
that is why we removed the duration outiers. 


```{r}
dataset <- dataset[-Find_outlierAge, ]
dataset <- dataset[!(678:nrow(dataset) %in% Find_outlierDuration), ]
dataset <- dataset[dataset$credit_amount != 1, ]
```

finaly removing the outliers for the reasons we mentioned above.



## Checking for Missing Values 

```{r}

total_missing_values <- sum(is.na(dataset))
print(total_missing_values)

```

our dataset has not missing values so no need for filling or deleting any rows in that sense. 


## Data Conversion (Encoding categorical data)
To prepare the data for analysis, we need to convert certain categorical attributes into numerical values. 

Let's take a closer look at how this is done for specific attributes:

 Encoding "checking_status"

The "checking_status" attribute represents the status of the existing checking account of the loan applicant.

```{r}
dataset$checking_status <- factor(dataset$checking_status, levels = c("no checking","<0", ">=200", "0<=X<200"), labels = c(1, 2, 3, 4))
```
We've encoded it as follows:
- "no checking" is labeled as 1.
- "<0" is labeled as 2.
- ">=200" is labeled as 3.
- "0<=X<200" is labeled as 4.

```{r}
dataset$class <- factor(dataset$class, levels = c("bad", "good"), labels = c(0, 1))

```
Encoding "class"
The "class" attribute describes whether the customer is a good or bad credit risk. We've encoded it as follows:

"bad" is labeled as 0.
"good" is labeled as 1

```{r}
dataset$housing = factor(dataset$housing,levels = c("own","for free", "rent"), labels = c(1, 2, 3))
```
Encoding "housing"
The "housing" attribute represents the housing situation of the applicants. We've encoded it as follows:

"own" is labeled as 1.
"for free" is labeled as 2.
"rent" is labeled as 3.

```{r}
dataset$foreign_worker <- factor(dataset$foreign_worker, levels = c("no", "yes"), labels = c(0, 1))
```
Encoding "foreign_worker"
The "foreign_worker" attribute represents whether the loan applicant is a foreign worker or not. We've encoded it as follows:

"no" is labeled as 0.
"yes" is labeled as 1.

```{r}
dataset$credit_history = factor(dataset$credit_history,levels = c("no credits/all paid","all paid", "existing paid", "delayed previously", "critical/other existing credit"), labels = c(1, 2, 3, 4, 5))
```
Encoding "credit_history"
The "credit_history" attribute represents the credit history for the lone applicant.
We've encoded it as follows (going from best case to worst) :

"no credits/all paid" is labeled as 1.
"all paid" is labeled as 2.
"existing paid" is labeled as 3.
"delayed previously" is labeled as 4.
"critical/other existing credit" is labeled as 5.


```{r}
 dataset$savings_status <- factor(dataset$savings_status, levels = c('no known savings', '<100', '100<=X<500', '500<=X<1000', '>=1000'), labels = c(1, 2, 3, 4, 5))
```
Encoding "savings_status"
The "savings_status" attribute represents the status of the savings account or bond of the loan applicant.
We've encoded it as follows (going from least to greatest) :

"no known savings" is labeled as 1.
"<100" is labeled as 2.
"100<=X<500" is labeled as 3.
"500<=X<1000" is labeled as 4.
">=1000" is labeled as 5.

```{r}
dataset$employment <- factor(dataset$employment, levels = c('unemployed', '<1', '1<=X<4', '4<=X<7', '>=7'), labels = c(1, 2, 3, 4, 5))
```
Encoding "employment"
The "empolyment" attribute represents the current employment status of the loan applicant in number of years. 

We've encoded it as follows (going from least to greatest) :

"unemployed" is labeled as 1.
"<1" is labeled as 2.
"1<=X<4" is labeled as 3.
"4<=X<7" is labeled as 4.
">=7" is labeled as 5.

```{r}
dataset$other_parties <- factor(dataset$other_parties, levels = c('none', 'guarantor', 'co applicant'), labels = c(1, 2, 3))
```
Encoding "other_parties"
The "other_parties" attribute represents the debtors or guarantors associated with the loan. 

We've encoded it as follows :

"none" is labeled as 1.
"guarantor" is labeled as 2.
"co applicant" is labeled as 3.

```{r}
 dataset$property_magnitude <- factor(dataset$property_magnitude, levels = c('no known property', 'car', 'life insurance', 'real estate'), labels = c(1, 2, 3, 4))
```
Encoding "property_magnitude"
The "property_magnitude" attribute represents the magnitude of the property owned by the loan applicant.
We've encoded it as follows (going from the least magnitude to the greatest) :

"no known property" is labeled as 1.
"car" is labeled as 2.
"life insurance" is labeled as 3.
"real estate" is labeled as 4.

```{r}
dataset$other_payment_plans <- factor(dataset$other_payment_plans, levels = c('none', 'bank', 'stores'), labels = c(1, 2, 3))
```
Encoding "other_payment_plans"
The "other_payment_plans" attribute represents other payment plans associated with the loan.

We've encoded it as follows :

"none" is labeled as 1.
"bank" is labeled as 2.
"stores insurance" is labeled as 3.

```{r}
dataset$job <- factor(dataset$job, levels = c('unemp/unskilled non res', 'unskilled resident', 'skilled', 'high qualif/self emp/mgmt'), labels = c(1, 2, 3, 4))
```
Encoding "job"
The "job" attribute represents the type of job of the loan applicant.

We've encoded it as follows (from least to highest) :

"unemp/unskilled non res u" is labeled as 1.
"unskilled resident" is labeled as 2.
"skilled" is labeled as 3.
"high qualif/self emp/mgmt" is labeled as 4.

```{r}
dataset$own_telephone <- factor(dataset$own_telephone, levels = c("none", "yes"), labels = c(0, 1))
```
Encoding "own_telephone"
The "own_telephone" attribute represents whether the loan applicant has their own telephone line or not.

We've encoded it as follows:

"none" is labeled as 0.
"yes" is labeled as 1.

```{r}
dataset$personal_status <- factor(dataset$personal_status, levels = c('female div/dep/mar', 'male div/sep', 'male mar/wid', 'male single'))
```
Encoding "personal_status"
The "personal_status" attribute represents the sex and marital status of the loan applicant.

Since this attribute's values does not have an actual ranking, we did not use any labels for them.
We used the encoding for this attribute to make it of type 'factor' instead of 'character' since that will help us later on with the classification process.

```{r}
dataset$purpose <- factor(dataset$purpose, levels = c('business', 'domestic appliance', 'education', 'furniture/equipment', 'new car', 'other', 'radio/tv', 'repairs', 'retraining', 'used car'))
```
Encoding "purpose"
The "purpose" attribute represents the purpose of the credit, which the loan is being taken.

Since this attribute's values does not have an actual ranking, we did not use any labels for them.
We used the encoding for this attribute to make it of type 'factor' instead of 'character' since that will help us later on with the classification process.


By encoding these attributes, we convert textual categories into numerical values, making them suitable for use in models.

Print the final preprocessed dataset
```{r}
print(head(dataset))
```






## Normalization
Normalization is an essential data preprocessing step to ensure that numeric attributes are on a consistent scale

Here, we perform min-max scaling to normalize specific attributes in our dataset.


Define the min_max_scaling() function
```{r}
min_max_scaling <- function(x) {return (x - min(x)) / (max(x)- min(x))}
```

We define a custom min-max scaling function, `min_max_scaling()`, which scales values to a range between 0 and 1. This scaling technique preserves the relationships between values while ensuring that all values are within the same range.

Now, let's apply the min-max scaling to the relevant attributes in our dataset:

Normalizing 'age'
We normalize the 'age' variable using min-max scaling:

```{r}
dataset$age <- min_max_scaling(dataset$age)
```


Normalize 'duration' variable

```{r}
dataset$duration <- min_max_scaling(dataset$duration)
```

Normalize 'credit_amount' variable
```{r}
dataset$credit_amount <- min_max_scaling(dataset$credit_amount)

```

After normalization, the values of these attributes will now fall within the range[0,1], ensuring that they are all on a common scale and ready for further analysis.


## First: chi square 

We will apply chi square for all nominal data in our dataset to measure weather the distribution of the variables and the dataset class is independent or not.

```{r}
tbl = table(dataset$class, dataset$purpose)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'purpose' values in a table where the rows represent 'class' and the columns 'purpose', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.0001157 which is less than 0.05. Based on the result we got, we learned that certain purposes for credit applications might be correlated with a higher or lower credit risk, affecting the likelihood of default or other credit-related outcomes.


```{r}
tbl = table(dataset$class, dataset$personal_status)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'personal_status' values in a table where the rows represent 'class' and the columns 'personal_status', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.02224 which is less than 0.05. Based on the result we got, we learned that certain personal statuses are correlated with a higher or lower credit risk, affecting the likelihood of default or other credit-related outcomes. 


```{r}
tbl = table(dataset$class, dataset$other_parties)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'other_parties' values in a table where the rows represent 'class' and the columns 'other_parties', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.03606 which is less than 0.05. Based on the result we got, we learned that certain parties are correlated with a higher or lower credit risk, affecting the likelihood of default or other credit-related outcomes. 

```{r}
tbl = table(dataset$class, dataset$property_magnitude)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'property_magnitude' values in a table where the rows represent 'class' and the columns 'property_magnitude', then we checked the chi square for the table.
The p-value we got as a result of this test was 2.858e-05 which is less than 0.05. Based on the result we got, we learned that certain property magnitudes are correlated with a higher or lower credit risk, affecting the likelihood of default or other credit-related outcomes. 

```{r}
tbl = table(dataset$class, dataset$housing)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'housing' values in a table where the rows represent 'class' and the columns 'housing', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.0001117 which is less than 0.05. Based on the result we got, we learned that certain housing types are correlated with a higher or lower credit risk, affecting the likelihood of default or other credit-related outcomes. 

```{r}
tbl = table(dataset$class, dataset$job)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'job' values in a table where the rows represent 'class' and the columns 'job', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.5966 which is more than 0.05. Which implies that a person skilles and qualifications in their job are not necessarily correlated with having a higher or lower credit risk.

```{r}
tbl = table(dataset$class, dataset$foreign_worker)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'foreign_worker' values in a table where the rows represent 'class' and the columns 'foreign_worker', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.01583 which is less than 0.05. Based on the result we got, we learned that whether someone is a foregin worker or not could lead to a higher or lower credit risk.

```{r}
tbl = table(dataset$class, dataset$own_telephone)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'own_telephone' values in a table where the rows represent 'class' and the columns 'own_telephone', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.2789 which is more than 0.05. Which implies that whether a person own a telephone or not does not necessarily correlate with having a higher or lower credit risk.

```{r}
tbl = table(dataset$class, dataset$other_payment_plans)
tbl 
chisq.test(tbl)
```
Here we put the attribute 'class' values and the attribute 'other_payment_plans' values in a table where the rows represent 'class' and the columns 'other_payment_plans', then we checked the chi square for the table.
The p-value we got as a result of this test was 0.001629 which is less than 0.05. Based on the result we got, we learned that the probability of weather someone will has a high or low credit risk might get affected if they had certain other payment plans.


After conducting the chi square codes and observing the results we noticed that 2 attributes (job and own_telephone) in our dataset might be not very corrlated with our class label (weather someone has a bad or good credit), which means it does not matter what value these attributes will take in each row as the results of the class label are less likely to be affected.

We will not drop the independent columns since we have a relatively small dataset, instead we will use the results we got to help us with the classification of our dataset. 




# Correlation between numeric attributes and the class label

```{r}
set.seed(123)
Data <- data.frame(
  duration = runif(999),
  age = runif(999),
  credit_amount = runif(999),
  residence_since = runif(999),
  num_dependents = runif(999),
  installment_commitment = runif(999),
  Own_telephone = sample(c(0, 1), 999, replace = TRUE),
  Class = sample(c(0, 1), 999, replace = TRUE)
)
```

# Calculate correlation between each numeric column and the 'Class' variable

```{r}
cor_with_class <- sapply(Data[, -ncol(Data)], function(x) cor(x, Data$Class))
```

# Print the correlation values
```{r}
print(cor_with_class)
```

## classification 
our goal is to develop a predictive model that accurately categorizes instances into predefined classes based on the provided attributes. 
using different algorithms we want to build a model that can accurately predict the 'class' attribute for each instance in our dataset.
the success of the classification model will be evaluated based on its ability to accurately classify instances according to the specified 'class' attribute, considering the unique patterns and characteristics present in our dataset.

We wanted to use Cross-validation (try different folds) on our trees to evaluate our model on different training and test sets, the only way we were able to get different trees for different folds was by constructing the models with different folds inside a loop which performed the steps for each fold separately. We believe that the reason why we got different trees using this method is because the code itself repeat the process so there was no mistakes that caused that error.

Outstide the loop and after we reached the desired fold we printed, plotted and got the matrix for our trees.


## using Gini Index CART Tree 
The Gini index is a measure of impurity or purity used in decision tree algorithms, including CART (Classification and Regression Trees) which the one we used to plot the trees.

## 10 folds 

```{r}
install.packages("rpart")
library(rpart)
install.packages("caret")
library(caret)
set.seed(123)
k <- 10

folds <- createFolds(dataset$class, k = k, list = TRUE, returnTrain = FALSE)

# Initialize variables to store metrics
sensitivity_vec <- specificity_vec <- precision_vec <- numeric(k)

for (i in 1:k) {
  test_indices <- unlist(folds[[i]])

  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  tree_model <- rpart(class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
                        employment + installment_commitment + personal_status + other_parties + residence_since +
                        property_magnitude + age + other_payment_plans + housing + existing_credits + job +
                        num_dependents + own_telephone + foreign_worker, 
                      data = training_set, method = 'class', parms = list(split = 'gini'))
    
  predictions <- predict(tree_model, newdata = testing_set, type = 'class')

  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
  gini <- 1 - sum((table(predictions, testing_set$class) / length(predictions))^2)  # Corrected this line
  
  # Store metrics
  sensitivity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Sensitivity"]
  specificity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Specificity"]
  precision_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Pos Pred Value"]
  

}
cat(sprintf("Fold: %d, Accuracy: %.2f%%, Gini Index: %.4f\n", i, accuracy * 100, gini))
  print(confusion_matrix)
# Print or use the average values of Sensitivity, Specificity, Precision
cat(sprintf("Average Sensitivity: %.2f%%\n", mean(sensitivity_vec) * 100))
cat(sprintf("Average Specificity: %.2f%%\n", mean(specificity_vec) * 100))
cat(sprintf("Average Precision: %.2f%%\n", mean(precision_vec) * 100))

```
Fold: 10, Accuracy: 73.74%, Gini Index: 0.5661
Sensitivity (True Positive Rate, Recall): The average sensitivity is approximately 40.82%. This metric represents the ability of the model to correctly identify positive instances (class 1). A lower sensitivity suggests that the model may be missing a significant number of positive instances.

Specificity (True Negative Rate): The average specificity is approximately 88.06%. This metric represents the ability of the model to correctly identify negative instances (class 0). A higher specificity indicates that the model is good at avoiding false alarms for negative instances.

Precision (Positive Predictive Value): The average precision is approximately 59.46%. This metric represents the accuracy of positive predictions made by the model. A lower precision suggests that there are false positives in the predictions.

```{r}
rpart.plot(tree_model, uniform = TRUE ,cex = 0.5, main = "CART Tree - Fold 10")
text(tree_model, use.n = TRUE, cex = 0.8)
```

## five folds

```{r}

set.seed(123)
k <- 5

folds <- createFolds(dataset$class, k = k, list = TRUE, returnTrain = FALSE)

# Initialize variables to store metrics
sensitivity_vec <- specificity_vec <- precision_vec <- numeric(k)

for (i in 1:k) {
  test_indices <- unlist(folds[[i]])

  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  tree_model <- rpart(class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
                        employment + installment_commitment + personal_status + other_parties + residence_since +
                        property_magnitude + age + other_payment_plans + housing + existing_credits + job +
                        num_dependents + own_telephone + foreign_worker, 
                      data = training_set, method = 'class', parms = list(split = 'gini'))
    
  predictions <- predict(tree_model, newdata = testing_set, type = 'class')

  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
  gini <- 1 - sum((table(predictions, testing_set$class) / length(predictions))^2)  # Corrected this line
  
  # Store metrics
  sensitivity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Sensitivity"]
  specificity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Specificity"]
  precision_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Pos Pred Value"]
  

}
cat(sprintf("Fold: %d, Accuracy: %.2f%%, Gini Index: %.4f\n", i, accuracy * 100, gini))
  print(confusion_matrix)
# Print or use the average values of Sensitivity, Specificity, Precision
cat(sprintf("Average Sensitivity: %.2f%%\n", mean(sensitivity_vec) * 100))
cat(sprintf("Average Specificity: %.2f%%\n", mean(specificity_vec) * 100))
cat(sprintf("Average Precision: %.2f%%\n", mean(precision_vec) * 100))


```
fold 5: Accuracy: 71.36%, Gini Index: 0.5844
Average Sensitivity: 39.80% - The average proportion of true positives among all actual positives. Indicates the model's ability to identify positive instances.
Average Specificity: 87.34% - The average proportion of true negatives among all actual negatives. Indicates the model's ability to identify negative instances.
Average Precision: 59.04% - The average precision, or positive predictive value, measures the accuracy of positive predictions.
```{r}
rpart.plot(tree_model, uniform = TRUE,cex = 0.5, main = "CART Tree - Fold 5")
text(tree_model, use.n = TRUE, cex = 0.8)
```

## 3 folds

```{r}

set.seed(123)
k <- 3

folds <- createFolds(dataset$class, k = k, list = TRUE, returnTrain = FALSE)

# Initialize variables to store metrics
sensitivity_vec <- specificity_vec <- precision_vec <- numeric(k)

for (i in 1:k) {
  test_indices <- unlist(folds[[i]])

  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  tree_model <- rpart(class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
                        employment + installment_commitment + personal_status + other_parties + residence_since +
                        property_magnitude + age + other_payment_plans + housing + existing_credits + job +
                        num_dependents + own_telephone + foreign_worker, 
                      data = training_set, method = 'class', parms = list(split = 'gini'))
    
  predictions <- predict(tree_model, newdata = testing_set, type = 'class')

  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
  gini <- 1 - sum((table(predictions, testing_set$class) / length(predictions))^2)  # Corrected this line
  
  # Store metrics
  sensitivity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Sensitivity"]
  specificity_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Specificity"]
  precision_vec[i] <- confusionMatrix(predictions, testing_set$class)[["byClass"]]["Pos Pred Value"]
  

}
cat(sprintf("Fold: %d, Accuracy: %.2f%%, Gini Index: %.4f\n", i, accuracy * 100, gini))
  print(confusion_matrix)
# Print or use the average values of Sensitivity, Specificity, Precision
cat(sprintf("Average Sensitivity: %.2f%%\n", mean(sensitivity_vec) * 100))
cat(sprintf("Average Specificity: %.2f%%\n", mean(specificity_vec) * 100))
cat(sprintf("Average Precision: %.2f%%\n", mean(precision_vec) * 100))


```
Fold: 3: Accuracy: 71.60%, Gini Index: 0.5844

Average Sensitivity: This is relatively low at 42.13%. It indicates the proportion of actual positive instances correctly predicted as positive. A higher sensitivity is desirable, especially if identifying positive instances is crucial.

Average Specificity: This is relatively high at 85.90%. It indicates the proportion of actual negative instances correctly predicted as negative. High specificity is generally good, but it might be due to the imbalance in the dataset.

Average Precision: Precision is the proportion of true positive predictions among all positive predictions. The average precision is 56.95%, suggesting that about 57% of the instances predicted as positive are true positives.

```{r}
rpart.plot(tree_model, uniform = TRUE,cex = 0.5, main = "CART Tree - Fold 3")
text(tree_model, use.n = TRUE, cex = 0.8)
```
## gain ratio (C4.5 algorithm) 
The C5.0 function is using the gain ratio for splitting nodes in the decision tree. The control argument is specifying additional parameters, and CF in this case is controlling the cost factor for misclassification. It doesn't affect the splitting criterion, which remains gain ratio by default.

## 10 folds
```{r}
set.seed(123)
myFormula <- class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
  employment + installment_commitment + personal_status + other_parties + residence_since +
  property_magnitude + age + other_payment_plans + housing + existing_credits + job +
  num_dependents + own_telephone + foreign_worker

# Define the number of folds
num_folds <- 10

# Initialize variables to store overall performance metrics
overall_sensitivity <- 0
overall_specificity <- 0
overall_precision <- 0

# Perform cross-validation with C5.0 and pruning
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)

for (i in 1:num_folds) {
  # Extract the indices for the current fold
  test_indices <- unlist(folds[[i]])

  # Create training and testing sets
  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  # Train the C5.0 model with pruning
  tree_model <- C5.0(myFormula, data = training_set, control = C5.0Control(CF = 0.01))

  # Make predictions on the testing set
  predictions <- predict(tree_model, newdata = testing_set)

  # Evaluate the model
  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)

  # Compute Sensitivity, Specificity, Precision using caret's confusionMatrix
  perf_metrics <- confusionMatrix(predictions, testing_set$class)$byClass

  # Aggregate metrics for overall performance
  overall_sensitivity <- overall_sensitivity + perf_metrics["Sensitivity"]
  overall_specificity <- overall_specificity + perf_metrics["Specificity"]
  overall_precision <- overall_precision + perf_metrics["Pos Pred Value"]

 
}
 cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", i, accuracy * 100))
  print(confusion_matrix)
  
# Calculate average metrics over all folds
avg_sensitivity <- overall_sensitivity / num_folds
avg_specificity <- overall_specificity / num_folds
avg_precision <- overall_precision / num_folds

# Print or use the average values as needed
cat(sprintf("Average Sensitivity: %.2f%%\n", avg_sensitivity * 100))
cat(sprintf("Average Specificity: %.2f%%\n", avg_specificity * 100))
cat(sprintf("Average Precision: %.2f%%\n", avg_precision * 100))


```
Fold 10: accuracy:68.69%.

Average Sensitivity: 40.13%
 measures the proportion of actual positive instances that were correctly identified by the model. In this case, the model, on average, correctly identified only 40.13% of the actual positive instances.

Average Specificity: 86.19%
Specificity (True Negative Rate) measures the proportion of actual negative instances that were correctly identified by the model. The model, on average, correctly identified 86.19% of the actual negative instances.

Average Precision: 56.80%
Precision (Positive Predictive Value) measures the proportion of positive instances among the instances predicted as positive by the model. On average, 56.80% of the instances predicted as positive by the model were actually positive.
```{r}
plot(tree_model, uniform = TRUE, cex = 0.7,main = "C4.5 Tree - Fold 10")

```

## 5 folds
```{r}

set.seed(123)
myFormula <- class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
  employment + installment_commitment + personal_status + other_parties + residence_since +
  property_magnitude + age + other_payment_plans + housing + existing_credits + job +
  num_dependents + own_telephone + foreign_worker
# Define the number of folds
num_folds <- 5

# Initialize variables to store overall performance metrics
overall_sensitivity <- 0
overall_specificity <- 0
overall_precision <- 0

# Perform cross-validation with C5.0 and pruning
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)

for (i in 1:num_folds) {
  # Extract the indices for the current fold
  test_indices <- unlist(folds[[i]])

  # Create training and testing sets
  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  # Train the C5.0 model with pruning
  tree_model <- C5.0(myFormula, data = training_set, control = C5.0Control(CF = 0.01))

  # Make predictions on the testing set
  predictions <- predict(tree_model, newdata = testing_set)

  # Evaluate the model
  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)

  # Compute Sensitivity, Specificity, Precision using caret's confusionMatrix
  perf_metrics <- confusionMatrix(predictions, testing_set$class)$byClass

  # Aggregate metrics for overall performance
  overall_sensitivity <- overall_sensitivity + perf_metrics["Sensitivity"]
  overall_specificity <- overall_specificity + perf_metrics["Specificity"]
  overall_precision <- overall_precision + perf_metrics["Pos Pred Value"]

 
}
 cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", i, accuracy * 100))
  print(confusion_matrix)
  
# Calculate average metrics over all folds
avg_sensitivity <- overall_sensitivity / num_folds
avg_specificity <- overall_specificity / num_folds
avg_precision <- overall_precision / num_folds

# Print or use the average values as needed
cat(sprintf("Average Sensitivity: %.2f%%\n", avg_sensitivity * 100))
cat(sprintf("Average Specificity: %.2f%%\n", avg_specificity * 100))
cat(sprintf("Average Precision: %.2f%%\n", avg_precision * 100))

```

fold5: Accuracy: 69.35%

Average Sensitivity: 38.49%:
Sensitivity (also called recall or true positive rate) is the ability of the model to correctly identify positive instances.
In this case, the average sensitivity is relatively low at 38.49%, suggesting that the model has difficulty correctly identifying positive cases.

Average Specificity: 85.18%:
Specificity is the ability of the model to correctly identify negative instances.
The average specificity is relatively high at 85.18%, indicating that the model is performing well in identifying negative cases.

Average Precision: 52.27%:
Precision is the ratio of correctly predicted positive observations to the total predicted positives.
An average precision of 52.27% suggests that, on average, around half of the instances predicted as positive by the model are true positives.
```{r}
plot(tree_model, uniform = TRUE, main = "C4.5 Tree - Fold 5")

```

## 3 folds
```{r}

set.seed(123)

# Define the number of folds
num_folds <- 3

myFormula <- class ~ checking_status + duration + credit_history + purpose + credit_amount + savings_status +
  employment + installment_commitment + personal_status + other_parties + residence_since +
  property_magnitude + age + other_payment_plans + housing + existing_credits + job +
  num_dependents + own_telephone + foreign_worker

# Perform cross-validation with C5.0 and pruning
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)

for (i in 1:num_folds) {
  # Extract the indices for the current fold
  test_indices <- unlist(folds[[i]])

  # Create training and testing sets
  training_set <- dataset[-test_indices, ]
  testing_set <- dataset[test_indices, ]

  # Train the C5.0 model with pruning
  tree_model <- C5.0(myFormula, data = training_set, control = C5.0Control(CF = 0.01))

  # Make predictions on the testing set
  predictions <- predict(tree_model, newdata = testing_set)

  # Evaluate the model
  confusion_matrix <- table(predictions, testing_set$class)
  accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)

 
}
 cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", i, accuracy * 100))
  print(confusion_matrix)
  


```

Fold 3, Accuracy: 71.60%:
True Positive (TP): 200
True Negative (TN): 37
False Positive (FP): 31
False Negative (FN): 63
This fold shows that the model correctly identified 200 instances of class 1 and 37 instances of class 0. However, it misclassified 31 instances of class 0 as class 1 and 63 instances of class 1 as class 0.

```{r}
plot(tree_model, uniform = TRUE, main = "C4.5 Tree - Fold 3")

```

## information gain ID3 


## 3 folds 

```{r}
num_folds <- 3
 
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)
for (fold in seq_along(folds)) {
 # Extract training and testing sets for the current fold
 test_indices <- folds[[fold]]
 train_data <- dataset[-test_indices, ]
 test_data <- dataset[test_indices, ]
   
    # Build the decision tree
    id3_model <- rpart(class ~ ., data = train_data, method = "class", control = rpart.control(cp = 0.01), parms = list(split = "information"))
 }


     # Make predictions on the test set
predictions <- predict(id3_model, newdata = test_data, type = "class")
    
     # Print confusion matrix for the current fold
    confusion_matrix <- table(predictions, test_data$class)
# Compute accuracy
accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", fold, accuracy * 100))

     print(confusion_matrix)


```

Fold: 3, Accuracy: 71.99%
True Positive (TP): 194 - The number of instances correctly predicted as class 1.
True Negative (TN): 45 - The number of instances correctly predicted as class 0.
False Positive (FP): 38 - The number of instances incorrectly predicted as class 1.
False Negative (FN): 55 - The number of instances incorrectly predicted as class 0.


```{r}
# Print and plot the decision tree
 print(id3_model)
 prp(id3_model, uniform = TRUE, main = paste("Pruned Decision Tree - Fold", fold))
 text(id3_model, use.n = TRUE, all = TRUE, cex = 0.6)
  
```

## 5 folds 



```{r}
num_folds <- 5
 
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)
for (fold in seq_along(folds)) {
 # Extract training and testing sets for the current fold
 test_indices <- folds[[fold]]
 train_data <- dataset[-test_indices, ]
 test_data <- dataset[test_indices, ]
   
    # Build the decision tree
    id3_model <- rpart(class ~ ., data = train_data, method = "class", control = rpart.control(cp = 0.01), parms = list(split = "information"))
    
 }

  
 

     # Make predictions on the test set
predictions <- predict(id3_model, newdata = test_data, type = "class")
    
     # Print confusion matrix for the current fold
    confusion_matrix <- table(predictions, test_data$class)
# Compute accuracy
accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", fold, accuracy * 100))
     print(confusion_matrix)

```

Fold: 5, Accuracy: 73.37%
True Positive (TP): 123 - The number of instances correctly predicted as class 1.
True Negative (TN): 23 - The number of instances correctly predicted as class 0.
False Positive (FP): 16 - The number of instances incorrectly predicted as class 1.
False Negative (FN): 37 - The number of instances incorrectly predicted as class 0.

```{r}
# Print and plot the decision tree
 print(id3_model)
 prp(id3_model, uniform = TRUE, main = paste("Pruned Decision Tree - Fold", fold))
 text(id3_model, use.n = TRUE, all = TRUE, cex = 0.6)
  
```
## 10 folds 

```{r}
num_folds <- 10
 
folds <- createFolds(dataset$class, k = num_folds, list = TRUE, returnTrain = FALSE)
for (fold in seq_along(folds)) {
 # Extract training and testing sets for the current fold
 test_indices <- folds[[fold]]
 train_data <- dataset[-test_indices, ]
 test_data <- dataset[test_indices, ]
   
    # Build the decision tree
    id3_model <- rpart(class ~ ., data = train_data, method = "class", control = rpart.control(cp = 0.01), parms = list(split = "information"))
    
 }

  
     # Make predictions on the test set
predictions <- predict(id3_model, newdata = test_data, type = "class")
    
     # Print confusion matrix for the current fold
    confusion_matrix <- table(predictions, test_data$class)
    # Compute accuracy
accuracy <- sum(diag(confusion_matrix)) / sum(confusion_matrix)
cat(sprintf("Fold: %d, Accuracy: %.2f%%\n", fold, accuracy * 100))
     print(confusion_matrix)
```
Fold: 10, Accuracy: 76.53%
True Positive (TP): 59 - The number of instances correctly predicted as class 1.
True Negative (TN): 16 - The number of instances correctly predicted as class 0.
False Positive (FP): 10 - The number of instances incorrectly predicted as class 1.
False Negative (FN): 13 - The number of instances incorrectly predicted as class 0.

```{r}
# Print and plot the decision tree
 print(id3_model)
 prp(id3_model, uniform = TRUE, main = paste("Pruned Decision Tree - Fold", fold))
 text(id3_model, use.n = TRUE, all = TRUE, cex = 0.6)
  
```
As mentioned earlier, we constructed 9 different trees using different algorithms and folds and this is what we found:

In terms of algorithms we found that the information gain (ID3) algorithm performed the best compared to the other 2 with accuracy between 76.53% - 71.99% for different folds. We noticed that models were consistent with their accuracy using fold (3) and that's probably because the data in fold 3 was a representative well-balanced sample of the dataset or that the chosen algorithms were well-suited for the characteristics of the data in this fold. 

On the other hand, fold 10 gave the highest (with Information Gain) and the lowest (with Ratio Gain) accuracy on the same time, we believe that fold ten data's characteristics was well fitted for the information gain split but it might targeted the sensitivity of the ratio gain tree.

Since more than half of our dataset attributes were of categorial (nominal) type, it was expected for Ratio Gain to perform worse than the others since it does not handle nominal data as effective as them.

if we were looking at it in terms of accuracy we highly recommend using information gain with 10 folds (or any fold) to classifiy this model since it gave the highest accuracy out of the three algorithms.


## clustring: 


```{r}

# Load the data
data <- read.csv("credit_customers.csv")

# Convert numeric columns to numeric types (if they are not already)
numeric_columns <- c("duration", "credit_amount", "installment_commitment", "age", "existing_credits", "num_dependents")
data[numeric_columns] <- lapply(data[numeric_columns], as.numeric)

# One-hot encode categorical variables
categorical_columns <- c("checking_status", "credit_history", "purpose", "savings_status", "employment",
                         "personal_status", "other_parties", "property_magnitude", "other_payment_plans", 
                         "housing", "job", "own_telephone", "foreign_worker", "class")
data <- dummy_cols(data, select_columns = categorical_columns)
```
```{r}
# Remove the original categorical columns after encoding
data <- data %>% select(-all_of(categorical_columns))
# Handle missing values (if any)
data <- na.omit(data) 
```


1. **Load Data:** Reads a CSV file named "credit_customers.csv" into the variable 'data.'

2. **Numeric Conversion:** Converts specific columns (duration, credit_amount, installment_commitment, age, existing_credits, num_dependents) to numeric types if they are not already.

3. **One-Hot Encoding:** Performs one-hot encoding on categorical variables listed in 'categorical_columns,' creating dummy variables for each category. This expands the dataset with binary columns representing each category.

4. **Remove Original Categorical Columns:** Removes the original categorical columns after one-hot encoding to avoid redundancy.

5. **Handle Missing Values:** Uses `na.omit` to remove rows with missing values from the dataset.

In short, the code prepares the dataset for analysis by converting numeric types, one-hot encoding categorical variables, and handling missing values.
```{r}


# Z-score Standardization
# Apply standardization only to numeric columns
data_standardized <- data
data[numeric_columns] <- scale(data[numeric_columns])
# Assuming you have already standardized your data and it is stored in `data_standardized`

# Step 2: Perform PCA
pca_result <- prcomp(data_standardized, center = FALSE, scale. = FALSE)  # Data is already standardized

# Step 3: Analyze PCA Results
# View summary of PCA results
print(summary(pca_result))

# Decide how many principal components to keep based on variance explained
# For example, you might decide to keep components that explain 80% of the variance
# This is just an example, adjust based on your analysis
cum_var_explained <- cumsum(pca_result$sdev^2 / sum(pca_result$sdev^2))
num_components <- which(cum_var_explained >= 0.8)[1]

# Step 4: Use Principal Components for Clustering
# Select the number of principal components to use
data_pca <- pca_result$x[, 1:num_components]

loadings <- pca_result$rotation[, 1]
print(loadings)

data_pca <- pca_result$x[, 1]
k2_pca <- kmeans(data_pca, centers = 2, nstart = 25)

data_for_plot <- cbind(data_pca, rep(0, length(data_pca)))
data_pca_df <- as.data.frame(data_pca)
# Extracting the first three principal components
data_pca <- pca_result$x[, 1:3]

# Convert the PCA results to a data frame 
data_pca_df <- as.data.frame(data_pca)
```

```{r}
# Load necessary library for visualization
library(factoextra)

# Clustering and plotting for k = 2, 3, and 4
set.seed(123)  # For reproducibility

# Clustering for k = 2
k2 <- kmeans(data_pca_df, centers = 2, nstart = 25)
fviz_cluster(k2, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=2)")
print(paste("Total WSS for k=2:", k2$tot.withinss))


# Clustering for k = 3
k3 <- kmeans(data_pca_df, centers = 3, nstart = 25)
fviz_cluster(k3, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=3)")
print(paste("Total WSS for k=3:", k3$tot.withinss))


# Clustering for k = 4
k4 <- kmeans(data_pca_df, centers = 4, nstart = 25)
fviz_cluster(k4, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=4)")
print(paste("Total WSS for k=4:", k4$tot.withinss))


```

1. **Load Data:** Reads a CSV file named "credit_customers.csv" into the variable 'data.'

2. **Numeric Conversion:** Converts specific columns (duration, credit_amount, installment_commitment, age, existing_credits, num_dependents) to numeric types if they are not already.

3. **One-Hot Encoding:** Performs one-hot encoding on categorical variables listed in 'categorical_columns,' creating dummy variables for each category. This expands the dataset with binary columns representing each category.

4. **Remove Original Categorical Columns:** Removes the original categorical columns after one-hot encoding to avoid redundancy.

5. **Handle Missing Values:** Uses `na.omit` to remove rows with missing values from the dataset.

In short, the code prepares the dataset for analysis by converting numeric types, one-hot encoding categorical variables, and handling missing values.


```{r}
set.seed(123)  # For reproducibility

# Perform k-means clustering with different K values
k2 <- kmeans(data, centers = 2, nstart = 25)
k3 <- kmeans(data, centers = 3, nstart = 25)
k4 <- kmeans(data, centers = 4, nstart = 25)

# Print cluster centers for a quick check
print(k2$centers)
print(k3$centers)
print(k4$centers)
```
```{r}
# Silhouette width for each K
sil_width2 <- silhouette(k2$cluster, dist(data))
sil_width3 <- silhouette(k3$cluster, dist(data))
sil_width4 <- silhouette(k4$cluster, dist(data))

# Calculate average silhouette width for each K
avg_sil_width2 <- mean(sil_width2[, "sil_width"])
avg_sil_width3 <- mean(sil_width3[, "sil_width"])
avg_sil_width4 <- mean(sil_width4[, "sil_width"])
```


```{r}
# Print average silhouette widths
print(avg_sil_width2)
print(avg_sil_width3)
print(avg_sil_width4)

# Total within-cluster sum of squares for each K
total_withinss2 <- k2$tot.withinss
total_withinss3 <- k3$tot.withinss
total_withinss4 <- k4$tot.withinss

# Print total within-cluster sum of squares
print(total_withinss2)
print(total_withinss3)
print(total_withinss4)

# BCubed precision and recall (requires additional computations, shown here as placeholders)
# bcubed_precision2 <- ...
# bcubed_recall2 <- ...
# Similarly for k3 and k4
```


The output includes evaluation metrics for clustering:

- **Average Silhouette Width:** Measures cluster separation. Higher values(e.g., 0.722 for K=2) indicate better-defined clusters.
  
- **Total Within-Cluster Sum of Squares:** Reflects cluster compactness. Smaller values suggest more compact clusters (e.g., 2,405,448,167 for K=2).

These metrics favor K=2 as it has higher silhouette width and lower within-cluster sum of squares. Additional computations are needed for BCubed precision and recall.




```{r}
# Load necessary library for visualization
library(factoextra)

# Plot for K=2
fviz_cluster(k2, data = data)

# Plot for K=3
fviz_cluster(k3, data = data)

# Plot for K=4
fviz_cluster(k4, data = data)

```
`

- **K=2:**
  - **Cluster 1:** Higher credit, longer duration, likely good credit.
  - **Cluster 2:** Lower credit, shorter duration, likely bad credit.

- **K=3:**
  - **Cluster 1:** Similar to K=2 Cluster 2.
  - **Cluster 2:** Similar to K=2 Cluster 1, more varied.
  - **Cluster 3:** Moderate values between Clusters 1 and 2.

- **K=4:**
  - **Cluster 1:** Higher credit, longer duration, likely good credit.
  - **Cluster 2:** Lower credit, shorter duration, likely bad credit.
  - **Cluster 3:** Moderate values, more likely to have a car.
  - **Cluster 4:** Similar to K=4 Cluster 1, more varied.
  
  Increasing overlap in clusters with higher K suggests that the algorithm may be creating more detailed and potentially noise-driven clusters. It's crucial to balance model metrics and consider the meaningful interpretation of clusters. Reevaluate K using methods like the elbow method and silhouette analysis and incorporate domain knowledge for context.
  

```{r}

# Perform k-medoids clustering on categorical data
dissimilarity_matrix <- daisy(categorical_data, metric = "gower")
kmedoids_model <- pam(dissimilarity_matrix, k = 3)

# Print the k-medoids clustering results
cat("\nK-Medoids Clustering Results:\n")
print(kmedoids_model)


```
The output shows the results of k-medoids clustering:

- **Medoids:** Representative data points for each cluster.
- **Clustering vector:** Assigns each data point to a cluster.
- **Objective function:** The optimization criterion for clustering.

The clusters are formed based on the dissimilarity matrix. Each data point is assigned to the medoid (representative) that minimizes the dissimilarity within its cluster.

In our specific output:
- Cluster 1: Medoids at IDs 308, 52, 887
- Cluster 2: Medoids at IDs 309, 53, 892
- Cluster 3: Medoids at IDs 52, 53, 892

The "Objective function" values represent the quality of the clustering, with lower values indicating better-defined clusters. The "build" and "swap" values show the contributions to the objective function during the build and swap phases of the algorithm.

Overall, the k-medoids algorithm has divided your data into three clusters, and the medoids represent central points within each cluster.
```{r}
# Elbow method for determining the optimal number of clusters (k-means)
wss <- numeric(length = 10)
for (k in 1:10) {
  kmeans_model <- kmeans(normalized_numeric_data, centers = k, nstart = 10)
  wss[k] <- sum(kmeans_model$withinss)
}

# Plot the Elbow method graph with adjusted margins
par(mar = c(5, 4, 4, 2) + 0.1)
plot(1:10, wss, type = "b", main = "Elbow Method for Optimal k (k-means)",
     xlab = "Number of Clusters (k)", ylab = "Within-Cluster Sum of Squares (WSS)")

```

```{r}
# Load necessary library for visualization
library(factoextra)

# Clustering and plotting for k = 2, 3, and 4
set.seed(123)  # For reproducibility

# Clustering for k = 2
k2 <- kmeans(data_pca_df, centers = 2, nstart = 25)
fviz_cluster(k2, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=2)")
print(paste("Total WSS for k=2:", k2$tot.withinss))


```


````{r}

# Clustering for k = 3
k3 <- kmeans(data_pca_df, centers = 3, nstart = 25)
fviz_cluster(k3, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=3)")
print(paste("Total WSS for k=3:", k3$tot.withinss))
```

````{r}

# Clustering for k = 4
k4 <- kmeans(data_pca_df, centers = 4, nstart = 25)
fviz_cluster(k4, data = data_pca_df, geom = "point", 
             stand = FALSE, frame = FALSE, 
             main = "Cluster Visualization (k=4)")
print(paste("Total WSS for k=4:", k4$tot.withinss))
```


```{r}
set.seed(123)  # For reproducibility

# Perform k-means clustering with different K values
k2 <- kmeans(data, centers = 2, nstart = 25)
k3 <- kmeans(data, centers = 3, nstart = 25)
k4 <- kmeans(data, centers = 4, nstart = 25)

# Print cluster centers for a quick check
print(k2$centers)
print(k3$centers)
print(k4$centers)
```


```{r}
# Silhouette width for each K
sil_width2 <- silhouette(k2$cluster, dist(data))
sil_width3 <- silhouette(k3$cluster, dist(data))
sil_width4 <- silhouette(k4$cluster, dist(data))

# Calculate average silhouette width for each K
avg_sil_width2 <- mean(sil_width2[, "sil_width"])
avg_sil_width3 <- mean(sil_width3[, "sil_width"])
avg_sil_width4 <- mean(sil_width4[, "sil_width"])
```


```{r}
# Print average silhouette widths
print(avg_sil_width2)
print(avg_sil_width3)
print(avg_sil_width4)

# Total within-cluster sum of squares for each K
total_withinss2 <- k2$tot.withinss
total_withinss3 <- k3$tot.withinss
total_withinss4 <- k4$tot.withinss

# Print total within-cluster sum of squares
print(total_withinss2)
print(total_withinss3)
print(total_withinss4)

# BCubed precision and recall (requires additional computations, shown here as placeholders)
# bcubed_precision2 <- ...
# bcubed_recall2 <- ...
# Similarly for k3 and k4
```


The output includes evaluation metrics for clustering:

- **Average Silhouette Width:** Measures cluster separation. Higher values(e.g., 0.722 for K=2) indicate better-defined clusters.
  
- **Total Within-Cluster Sum of Squares:** Reflects cluster compactness. Smaller values suggest more compact clusters (e.g., 2,405,448,167 for K=2).

These metrics favor K=2 as it has higher silhouette width and lower within-cluster sum of squares. Additional computations are needed for BCubed precision and recall.




```{r}
# Load necessary library for visualization
library(factoextra)

# Plot for K=2
fviz_cluster(k2, data = data)


```
````{r}
# Plot for K=3
fviz_cluster(k3, data = data)
```

```{r evaluate-clusters}
# Plot for K=4
fviz_cluster(k4, data = data)
```

- **K=2:**
  - **Cluster 1:** Higher credit, longer duration, likely good credit.
  - **Cluster 2:** Lower credit, shorter duration, likely bad credit.

- **K=3:**
  - **Cluster 1:** Similar to K=2 Cluster 2.
  - **Cluster 2:** Similar to K=2 Cluster 1, more varied.
  - **Cluster 3:** Moderate values between Clusters 1 and 2.

- **K=4:**
  - **Cluster 1:** Higher credit, longer duration, likely good credit.
  - **Cluster 2:** Lower credit, shorter duration, likely bad credit.
  - **Cluster 3:** Moderate values, more likely to have a car.
  - **Cluster 4:** Similar to K=4 Cluster 1, more varied.
  
  Increasing overlap in clusters with higher K suggests that the algorithm may be creating more detailed and potentially noise-driven clusters. It's crucial to balance model metrics and consider the meaningful interpretation of clusters. Reevaluate K using methods like the elbow method and silhouette analysis and incorporate domain knowledge for context.
  

```{r}
# Specify the numeric and categorical attributes
numeric_attributes <- c("duration", "credit_amount", "age")
categorical_attributes <- setdiff(names(dataset), c("duration", "credit_amount", "age", "class"))

# Extract only the categorical attributes from the dataset
categorical_data <- dataset[, categorical_attributes]

# Perform k-medoids clustering on categorical data
dissimilarity_matrix <- daisy(categorical_data, metric = "gower")
kmedoids_model <- pam(dissimilarity_matrix, k = 3)

# Print the k-medoids clustering results
cat("\nK-Medoids Clustering Results:\n")
print(kmedoids_model)

```
The output shows the results of k-medoids clustering:

- **Medoids:** Representative data points for each cluster.
- **Clustering vector:** Assigns each data point to a cluster.
- **Objective function:** The optimization criterion for clustering.

The clusters are formed based on the dissimilarity matrix. Each data point is assigned to the medoid (representative) that minimizes the dissimilarity within its cluster.

In our specific output:
- Cluster 1: Medoids at IDs 308, 52, 887
- Cluster 2: Medoids at IDs 309, 53, 892
- Cluster 3: Medoids at IDs 52, 53, 892

The "Objective function" values represent the quality of the clustering, with lower values indicating better-defined clusters. The "build" and "swap" values show the contributions to the objective function during the build and swap phases of the algorithm.

Overall, the k-medoids algorithm has divided your data into three clusters, and the medoids represent central points within each cluster.




```{r}
# Elbow method for determining the optimal number of clusters (k-means)
wss <- numeric(length = 10)
for (k in 1:10) {
  kmeans_model <- kmeans(data_standardized, centers = k, nstart = 10)
  wss[k] <- sum(kmeans_model$withinss)
}

# Plot the Elbow method graph with adjusted margins
par(mar = c(5, 4, 4, 2) + 0.1)
plot(1:10, wss, type = "b", main = "Elbow Method for Optimal k (k-means)",
     xlab = "Number of Clusters (k)", ylab = "Within-Cluster Sum of Squares (WSS)")

```
