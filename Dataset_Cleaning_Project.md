---
title: "Dataset_Cleaning_Project"
author: "Ian Lynch"
date: "9/22/2021"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Load packages that are required for the cleaning process.
```{r}
library(rapportools)
```

Load dataset, check structure and first few records.
```{r}
dataset <- read.csv("C:/Users/ianmc/Documents/Game Business Intelligence/Porftolio Work/vgsales_Kaggle.csv")
head(dataset)
summary(dataset)
```


Delete out two missing Game Titles from the dataset.
After reviewing the dataset, I identified two records that had missing names. Given the size of the dataset and the limited information available about the missing records, I decided it was best to remove the two records from the dataset.
```{r}
sum(is.empty(dataset$Name))
dataset<- (dataset[-c(660,14247),])
```

Verify records have been removed
```{r}
dataset[655:665,]
dataset[14244:14250,]
```

Check for missing data in Platform field. No missing records.
```{r}
sum(is.empty(dataset$Platform))
sum(is.null(dataset$Platform))
sum(is.na(dataset$Platform))
```



Count of total missing records in the "Year_of_Release" field. After completing data cleaning in R, I Will manually impute missing data with correct release year records.
```{r}
sum(grepl("N/A", dataset$Year_of_Release))
```

Count of total missing records in the "Genre" field.
Upon original inspection of the uncleaned dataset, 2 missing records were identified in the Genre field. After deleting the records that contained missing observations in the Name field, it also removed the missing records for the Genre field as well.
```{r}
sum(is.empty(dataset$Genre))
sum(is.null(dataset$Genre))
sum(is.na(dataset$Genre))
```

Check for missing data in Publisher field. No missing records.
```{r}
sum(is.empty(dataset$Publisher))
sum(is.null(dataset$Publisher))
sum(is.na(dataset$Publisher))
```


Check for missing data in Developer field. 
Results: Over 6000 developer records are missing, nearly 40% of all records in the dataset. Due to the excessive amount of missing records, I will have the publisher stand in for the missing developer data. While not a perfect fix, it will help the variable to retain some amount of usefulness as many of the developers were also the publisher.
I Will fix this issue in the exported .CSV file

```{r}
sum(is.empty(dataset$Developer))
```

Check for missing data in Rating field. Similar too the developer field, over 6000k records are missing. There is no good work around to address this issue. Manually updating is not going to work. Using another field as a stand in will not work either. The Rating system is a specific set of ratings and deleting 40% of the observation from the dataset is not a good tradeoff for a field/variable that is not the focus of this case study.
Result: Leave missing data as-is.
```{r}
sum(is.empty(dataset$Rating))
```



Export cleaned data into .csv
```{r}
write.csv(dataset,"C:\\Users\\ianmc\\Documents\\Game Business Intelligence\\Porftolio Work\\R_Cleaned_Dataset.csv", row.names = FALSE)
```