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

A good first step with any new dataset is to take a quick look at the dataset to familiarize yourself with it. It is often a good idea to start out by printing out basic descriptive statistics about the dataset, the dtypes of each feature, as well as getting a gauge for missing values. 
