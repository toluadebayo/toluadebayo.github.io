# Which product type within the apparel product category do Amazon customers shop from the most in 2017?

## Data ##

The dataset used for this project was ComScore Web Behavior: 2017 Transactions, sourced from the Wharton Research Data Services (WRDS) database. The date range is January 1, 2017 to December 31, 2017. The data was filtered to Amazon apparel customers only. Below is a data dictionary for all the variables used in the dataset:
 
  - prod_category_id: unique identifier for category of product purchased
  - prod_name: name of product purchased
  - prod_qty: number of product purchased
  - prod_totprice: total price of product
  - event_date: yyyymmdd
  - household_size: household size 
  - census_region: census region 
  - household_income: household income
  - racial_background: race of customer
  -prod_category: product category
  
## Methodology ##

  1. Retrieve data from WRDS 
 	2. Clean data using Tableau Prep 
 	3. Import data into R 
 	4. Transform all ComScore common values into descriptions 
 	5. Perform analyses; exploratory data analysis, text mining, and prediction modeling (see below)
  
## Exploratory Data Analysis ##
1. Create separate datasets for each racial background. 

	`library(stringr)`
        
	`library(tidyverse)`
	
	`library(ggplot2)`
	
	`white = subset(df, subset = df$racial_background == "Caucasian")`
	
	`black = subset(df, subset = df$racial_background == "African American")`
	
	`asian = subset(df, subset = df$racial_background == "Asian")`
2. Create a barplot visualizing the amount of customers by household size and income. Repeat this step for other racial backgrounds.

	`income = c('Less than $25,000', '$25,000 – $39,999', '$40,000 – $59,999', '$60,000 – $74,999', '$75,000 – $99,999', 		'$100,000 – $149,999', '$150,000 – $199,999', '$200,000+')`

	`counts <- table(white$household_income, white$household_size)`
	
	`barplot(counts, main="Caucasian Customers by Size and Income",xlab="Household Size", ylab = "Count", 			col=c("darkblue", "darkblue", "red","red", "red", "red", "darkgreen", "darkgreen"), legend = income, beside=TRUE, ylim 		= c(0, 600), args.legend = list(x ='topright', ncol = 2, cex = 0.75))`

	

## Text Mining ## 

## Prediction Modeling ##


## About Me ##

Tolu Adebayo is a junior in the Wharton School, concentrating in Finance and Business Analytics. This data project aims to answer the following question: Which product type within the apparel product category do Amazon customers shop from the most in 2017? via 1) exploratory data analysis 2) text mining and 3) prediction models using selected features.  All of the coding, analysis, and visualizations were developed with R. Contact tadebayo@wharton.upenn.edu for further questions/comments/concerns!
