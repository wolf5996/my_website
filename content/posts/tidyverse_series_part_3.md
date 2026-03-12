---
title: "Tidyverse Series – Post 3: Reshaping & Cleaning Data with tidyr"
author: "Badran Elshenawy"
date: 2025-02-17T11:00:00Z
categories:
  - "Data Science"
  - "R"
  - "Tidyverse"
  - "Bioinformatics"
tags:
  - "Tidyverse"
  - "Data Wrangling"
  - "tidyr"
  - "R"
  - "Bioconductor"
  - "Data Cleaning"
description: "A comprehensive guide to mastering data reshaping and cleaning with tidyr in R. Learn about pivoting, separating, uniting, and handling missing values efficiently."
slug: "tidyverse-tidyr"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/tidyverse_tidyr/"
summary: "Discover how tidyr makes data reshaping and cleaning in R simple, readable, and efficient with functions like pivot_longer, pivot_wider, separate, unite, and more."
featured: true
rmd_hash: a287766ee860fe19

---

# 🔬 Tidyverse Series -- Post 3: Reshaping & Cleaning Data with `tidyr`

## 🛠 Why `tidyr`?

Data often comes in **messy, inconsistent, or improperly structured formats**. `tidyr` is designed to **reshape, clean, and structure data** into a tidy format that's easy to analyze and visualize. Whether you need to **pivot, separate, unite, or handle missing values**, `tidyr` makes it seamless.

### ✔️ Why Use `tidyr`?

-   **Transforms messy data into structured formats**
-   **Works perfectly with `dplyr`** for smooth data wrangling
-   **Simplifies complex reshaping tasks**

Let's explore the **key functions in `tidyr`**, with **detailed explanations, code examples, and expected outputs**!

------------------------------------------------------------------------

## 📚 Essential `tidyr` Functions

### ➡️ `pivot_longer()`: Convert Wide Data to Long Format

In many datasets, values are stored in **wide format**, making them difficult to analyze. `pivot_longer()` **reshapes wide data into long format**, making it easier to **filter, summarize, and visualize**.

#### 🔹 Example: Reshaping Gene Expression Data

#### **Before (`wide format`)**

| Gene  | Sample_1 | Sample_2 | Sample_3 |
|-------|----------|----------|----------|
| TP53  | 12.3     | 10.5     | 14.2     |
| BRCA1 | 8.9      | 9.2      | 10.1     |

#### **Using `pivot_longer()`**

``` r
library(tidyr)
library(dplyr)

df_long <- df %>%
  pivot_longer(cols = starts_with("Sample"),
               names_to = "Sample",
               values_to = "Expression")
```

#### **After (`long format`)**

| Gene | Sample   | Expression |
|------|----------|------------|
| TP53 | Sample_1 | 12.3       |
| TP53 | Sample_2 | 10.5       |
| TP53 | Sample_3 | 14.2       |

✅ Now, this structure allows for easy filtering and statistical analysis!

------------------------------------------------------------------------

### ➡️ `pivot_wider()`: Convert Long Data to Wide Format

Sometimes, data stored in **long format** needs to be expanded **back into wide format**.

#### **Example: Converting Long Format Back to Wide**

``` r
df_wide <- df_long %>%
  pivot_wider(names_from = "Sample",
              values_from = "Expression")
```

📌 This will **recreate the original wide format**, reversing the `pivot_longer()` operation.

------------------------------------------------------------------------

### ➡️ `separate()`: Splitting One Column into Multiple Columns

Often, a single column contains **multiple pieces of information** that should be **split into separate columns**.

#### **Example: Splitting Sample Names into Condition & Replicate**

``` r
df_separated <- df_long %>%
  separate(Sample, into = c("Condition", "Replicate"), sep = "_")
```

#### **Before**

| Gene | Sample    | Expression |
|------|-----------|------------|
| TP53 | Control_1 | 12.3       |
| TP53 | Control_2 | 10.5       |

#### **After**

| Gene | Condition | Replicate | Expression |
|------|-----------|-----------|------------|
| TP53 | Control   | 1         | 12.3       |
| TP53 | Control   | 2         | 10.5       |

✅ Now, **Condition** and **Replicate** are separate columns, making analysis easier.

------------------------------------------------------------------------

### ➡️ `unite()`: Combining Multiple Columns into One

`unite()` is the opposite of `separate()`. It **merges multiple columns into a single column**, with a specified separator.

#### **Example: Creating a Unique Identifier from Multiple Columns**

``` r
df_united <- df_separated %>%
  unite("Sample_ID", Condition, Replicate, sep = "_")
```

#### **Before**

| Gene | Condition | Replicate |
|------|-----------|-----------|
| TP53 | Control   | 1         |
| TP53 | Control   | 2         |

#### **After**

| Gene | Sample_ID |
|------|-----------|
| TP53 | Control_1 |
| TP53 | Control_2 |

✅ Now, the **Condition and Replicate columns are combined into a single Sample_ID column**.

------------------------------------------------------------------------

### ➡️ `drop_na()`: Removing Missing Values

Handling missing values is essential to **ensure clean data**.

#### **Example: Removing Rows with Missing Values**

``` r
df_clean <- df %>%
  drop_na()
```

✅ This **removes all rows that contain missing (`NA`) values**.

------------------------------------------------------------------------

### ➡️ `replace_na()`: Replacing Missing Values

Instead of removing missing values, you might want to **replace them with a default value**.

#### **Example: Replacing Missing Values with Zero**

``` r
df_filled <- df %>%
  replace_na(list(Expression = 0))
```

✅ This **replaces all `NA` values in the `Expression` column with `0`**.

------------------------------------------------------------------------

## 📊 Complete Workflow: Cleaning & Reshaping Data

Let's go through a **complete example**, from **messy data** to **clean, structured data**.

``` r
library(tidyr)
library(dplyr)

# Sample messy dataset
df <- data.frame(
  Gene = c("TP53", "BRCA1", "EGFR"),
  Control_1 = c(12.3, NA, 7.8),
  Control_2 = c(10.5, 9.2, 8.9)
)

# Reshape & clean
df_cleaned <- df %>%
  pivot_longer(cols = starts_with("Control"), names_to = "Sample", values_to = "Expression") %>%
  separate(Sample, into = c("Condition", "Replicate"), sep = "_") %>%
  drop_na()
```

✅ This pipeline **reshapes, cleans, and structures the dataset**, making it easier to analyze.

------------------------------------------------------------------------

## 📈 Key Takeaways

✅ `tidyr` is essential for **reshaping and cleaning data**.  
✅ `pivot_longer()` and `pivot_wider()` make restructuring **seamless**.  
✅ `separate()` and `unite()` allow flexible column manipulation.  
✅ Handling missing values is easy with `drop_na()` and `replace_na()`.  
✅ Works perfectly alongside `dplyr` for **efficient data workflows**.

📌 **Next up: Combining Data Efficiently -- Joins & Merging with `dplyr`!** Stay tuned! 🚀

👇 **How often do you reshape data in your analysis? Let's discuss!**

#Tidyverse #tidyr #RStats #DataScience #Bioinformatics #OpenScience #ComputationalBiology

