# weber-shandwick-case-study
Case study for Weber Shandwick through Qureos

This is my submission for the case study for the Junior Data Analyst position at Weber Shandwick. 

The final cleaned dataset for task 1 can be found [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Cleaned/movies_data_clean.csv).

The final visualization for task 2 can be found on my Tableau public profile [here](https://public.tableau.com/app/profile/sm3933/viz/EmployeeSalariesbyZip/Dashboard2).


# Table of Contents
1. **Assignment Description** 

2. **Task 1: Data Cleaning** -  [`data_cleaning.ipynb`](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/data_cleaning.ipynb)
    1. Details 
    2. Assumptions
    3. Cleaning process and walkthrough
    4. Final result

3. **Task 2: Data Visualization** -  [`data_visulaization.ipynb`](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/data_visualization.ipynb)
    1. Details
    2. Assumptions
    3. Data Cleaning
    4. Final result

---

# 1. Assignment Description

As part of this case study, I was given two sample datasets to work with, and demonstrate proficiency in data cleaning and data visualization. The sections below will describe my thought process in detail. 

---

# 2. Task 1: Data Cleaning

## 2.1 Details

Here we are given a dataset on movies and their performance, both in the box office and critically. The raw data can be found [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/movies_data.csv).

## 2.2 Assumptions

- We assume that **all numeric features here are normally distributed**. This is useful to determine whether or not to apply transforms to the features, and whether to choose a mean or a median for imputation. 
    - Since this is a sample dataset with only 14 rows, a distribution plot wouldn't be very meaningful, so this step is skipped. 


## 2.3 Cleaning process and walkthrough

A good first step with any new dataset is to take a quick look at the dataset to familiarize yourself with it. It is often a good idea to start out by printing out basic descriptive statistics about the dataset, the dtypes of each feature, as well as getting a gauge for missing values. As previously mentioned, we will be skipping looking at the frequency distribution of the features for this sample dataset. 

We can quantify and visualize missing values as shown below: 

<img  src="https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Images/missing-values.png" width="650" height="350">

The quantification is useful because we can see if any columns have a large number of missing values. Typically, if a column has more than 30-40% missing values, we need to think carefully about what imputation methods we follow. The larger the number of missing values in a dataset, the less useful imputation becomes. 

The visualization allows us to quickly identify trends in missing data. For instance, if we had a column called genre in the dataset, we could quickly gauge if for instance, action movies have more missing values than adventure movies. This also gives us a good idea of how we can later approach imputation. 

---
### Here are some key observations from the dataset: 
---

- "`DIRECTOR_facebook_likes`" should be an integer but is an object. 
    -  This suggests that the column may have strings that need to be converted to integers

-  "`num_voted_users`", "`Cast_Total_facebook_likes`", "`facenumber_in_poster`", and "`ACTOR_2_facebook_likes`" should be whole number integers, but are floats. 
    - While this isn't necessarily an issue, we may want to eventually convert them to ints for the sake of consistency. Note that this conversion to ints would need to happen AFTER imputation to avoid errors in handling null values 

- "`duration`" is the value in number of minutes and can be converted to int.

- Additionally, the column names do not follow a fixed naming convention. 
    - This should be fixed to make later stages of the data pipeline easier to implement. Readability is extremely important to ensure repeatability and maintenance of data models and dashboards. 
    - Here, I simply convert all characters to lower case

- From the visualization we can see that there are no clear patterns in the missing values of the dataset.

- The column "`title_year.1`" has 50% missing values and may be a repeat of the column "`title_year`"

- The data statistics do not reveal any immediate anomalies


---
### Develop the logic for cleaning:
---

- As discussed above, we assume the features are all normally distributed. 
    - Since we don't have a different column to groupby, such as genre, here we will simply impute a median where applicable. 

- We also do not have enough data to clearly define and detect outliers.
    - In a more realistic scenario, we would make a decision between choosing a mean or a median. For example, medians are robust to outliers, and would be preferred if we have a dataset with large number of outliers. 

- The column "`movie_title`" has extra characters at the end.
    -  These extra characters will be removed to improve readability.

- "`DIRECTOR_facebook_likes`" has string values and some values may have quotation marks. 
    - These special characters will be removed and the feature will be converted to int.
    - **Note:** We will first convert the feature to float values. This is important, because if our imputed value is a fraction, the int conversion will throw an error if null values are present.

- "`title_year`" and "`title_year.1`" do appear to be repeats. There are some values that are different, but upon observation, it is prudent to simply remove "`title_year.1`"
    - Note that it is possible a movie has two different release years, for example in different countries. However in this dataset, there is nothing to suggest that this is the case.

- Columns will be re-ordered to improve readability.

- Drop duplicate rows prior to imputation (and after cleaning) so that duplicate values don't interfere with median calculation. 


---
### Develop the logic for imputation:
---

- "`facenumber_in_poster`": Most, if not all movie posters have headshots of the actors. It is very unlikely for a poster to have no headshots, so imputing 0 may not be appropriate. 
    - Impute the median value.

- "`duration`", "`ACTOR_2_facebook_likes`", "`DIRECTOR_facebook_likes`", "`num_voted_users`": Impute the median value.

- "`Cast_Total_facebook_likes`": Simply imputing the median here is not logically sound, since actor 1, 2, 3 likes will be correlated to cast total. The sum of all other actor likes will logically be closer (but still not exactly the same) to the true value of total likes. 
    - Impute the sum of all the actor likes.


With the imputation complete, we can now convert all floats to ints for consistency. We will wrap these operations in a function to make the code a little modular. In case of future deployments or dashboard modifications, this will make the code much easier to work with. 

## 2.4 Final Result

The cleaned dataset can be viewed [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Cleaned/movies_data_clean.csv). Here is a snippet of the cleaned dataset.

<img  src="https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Images/movies-cleaned.png">

---

# 3. Task 2: Data visualization

## 3.1 Details

For this task, we are given a datasaet on employee salaries, and we need to create a dashboard with the following elements:

- A map of the gross pay by zip code

- A bar chart of gross pay by gender

- A bar chart of gross pay by number of years being hired

- A treemap of overtime pay and department name

The raw dataset can be found [here](https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Employee_Salaries.xlsx)


## 3.2 Assumptions

The task requires us to plot gross pay by zipcode, gender, and number of years hired. However, it does not specify whether we want the gross total (sum) pay or the average gross pay. A case can be made for the value of both. Here are two examples to illustrate the difference:

- If we consider the _total_ gross pay per zipcode, this will give us an indication of which zipcodes are generating more business.
    - In the absence of population statistics, this gives us information about different areas of the state/country. This will be of higher value to businesses or local governments.

- If we consider the _average_ gross pay per zipcode, we get an idea of how much the _average person_ makes per zipcode
    - Unlike the previous case, this information is more valuable to the individual person, for example if they wish to move to a certain area. 

**With these assumptions in mind, the plot will display both total AND average pays to account for both use cases.**


## 3.3 Data Cleaning

Before we begin visualization, we need to first ensure our data is in usable form. Upon observation, we note that the data is already clean with two exceptions:

- The column "`Position Under-Filled`" is comprised of over 88% null values. 
    - We observe that this feature has string values that cannot be easily classified or imputed. 
    - In an end-to-end project, the column would need to be dropped, as imputation is likely to be meaningless here. 
    - In this case however, we can simply ignore the column since it doesn't affect our visualization.

- The column "`2017 Overtime Pay`" has a negative value
    - After manual inspection, it seems fairly logical to replace this value with the absolute value. 

## 3.4 Final result

The final visualization was done using Tableau. The dashboard is interactive and filters all required plot elements based on the map and zipcode selection. It can be viewed [here](https://public.tableau.com/app/profile/sm3933/viz/EmployeeSalariesbyZip/Dashboard2). 

Shown below is a static image of the dashboard.

<img  src="https://github.com/s-mushnoori/weber-shandwick-case-study/blob/main/Images/dashboard-static.png">
