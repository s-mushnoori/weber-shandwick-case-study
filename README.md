# weber-shandwick-case-study
Case study for Weber Shandwick through Qureos

This is my submission for the case study for the Junior Data Analyst position at Weber Shandwick. 

The final cleaned dataset for task 1 can be found [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Cleaned/movies_data_clean.csv).

The final visualization for task 2 can be dound on my Tableau public profile [here](https://public.tableau.com/app/profile/sm3933/viz/EmployeeSalariesbyZip/Dashboard2).


# Table of Contents
1. **Assignment Description** 

2. **Task 1: Data Cleaning** -  [`data_cleaning.ipynb`](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/data_cleaning.ipynb)
    1. Details 
    2. Assumptions
    3. Cleaning process and walkthrough
    4. Final result

3. **Task 2: Data Visualization** -  [`data_visulaization.ipynb`](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/data_visualization.ipynb)
    1. Details
    2. Data Cleaning
    3. Final result

---

## 1. Assignment Description

As part of this case study, I was given two sample datasets to work with, and demonstrate proficiency in data cleaning and data visualization. The sections below will describe my thought process in detail. 

---

## 2. Task 1: Data Cleaning

### 2.1 Details

Here we are given a dataset on movies and their performance, both in the box office and critically. The raw data can be found [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/movies_data.csv).

### 2.2 Assumptions

1. We assume that **all numeric features here are normally distributed**. This is useful to determine whether or not to apply transforms to the features, and whether to choose a mean or a median for imputation. 
    - Since this is a sample dataset with only 14 rows, a distribution plot wouldn't be very meaningful, so this step is skipped. 


### 2.3 Cleaning process and walkthrough

A good first step with any new dataset is to take a quick look at the dataset to familiarize yourself with it. It is often a good idea to start out by printing out basic descriptive statistics about the dataset, the dtypes of each feature, as well as getting a gauge for missing values. As previously mentioned, we will be skipping looking at the frequency distribution of the features for this sample dataset. 

We can quantify and visualize missing values as shown below: 

<img  src="https://github.com/s-mushnoori/ames-housing/blob/main/Images/Neighborhood.PNG">

The quantification is useful becasue we can see if any columns have a large number of missing values. Typically, if a column has more than 30-40% missing values, we need to think carefully about what imputation methods we follow. The larger the number of missing values in a dataset, the less useful imputation becomes. 

The visualization allows us to quickly identify trends in missing data. For instance, if we had a column called genre in the dataset, we could quickly gauge if for instance, action movies have more missing values than adventure movies. This also gives us a good idea of how we can later approach imputation. 


**Here are some key observations from the dataset:** 

- "`DIRECTOR_facebook_likes`" should be an integer but is an object. 
    -  This suggests that the column may have strings that need to be converted to integers

-  "`num_voted_users`", "`Cast_Total_facebook_likes`", "`facenumber_in_poster`", and "`ACTOR_2_facebook_likes`" should be whole number integers, but are floats. 
    - While this isn't necessarily an issue, we may want to eventually convert them to ints for the sake of consistency. Note that this conversion to ints would need to happen AFTER imputation to avoid errors in handling null values 

- "`duration`" is the value in number of minutes and can be converted to int.

- Additionally, the column names do not follow a fixed naming convention. 
    - This should be fixed to make later stages of the data pipeline easier to implement. Readability is extremely important to ensure repeatability and maintenance of data models and dashboards. 

- From the visualization we can see that there are no clear patterns in the missing values of the dataset.

- The column "`title_year.1`" has 50% missing values and may be a repeat of the column "`title_year`"

- The data statistics do not reveal any immediate anomalies


**Develop the logic for imputation:**

- As discussed above, we assume the features are all normally distributed. 
    - Since we don't have a different column to groupby, such as genre, here we will simply impute a median where applicable. 

- We also do not have enough data to clearly define and detect outliers.
    - In a more realistic scenario, we would make a decision between choosing a mean or a median. For example, medians are robust to outliers, and would be preferred iif we have a dataset with large number of outliers. 

- The column "`movie_title`" has extra characters at the end which will be removed to improve readability.

- 
