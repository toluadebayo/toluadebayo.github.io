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
1. Install packages.

        library(stringr)
        library(tidyverse)
        library(ggplot2)	
2. Create separate datasets for each racial background. 

        white = subset(df, subset = df$racial_background == "Caucasian")
        black = subset(df, subset = df$racial_background == "African American")
        asian = subset(df, subset = df$racial_background == "Asian")
3. Create a barplot visualizing the amount of customers by household size and income and repeat for others. Note that the blue bars represent lower-income, red represents the middle-income, and green represents upper-income. 

        income = c('Less than $25,000', '$25,000 – $39,999', '$40,000 – $59,999', '$60,000 – $74,999', '$75,000 – $99,999',
        '$100,000 – $149,999', '$150,000 – $199,999', '$200,000+')
        counts <- table(white$household_income, white$household_size)
        barplot(counts, main="Caucasian Customers by Size and Income",xlab="Household Size", ylab = "Count",
        col=c("darkblue", "darkblue", "red","red", "red", "red", "darkgreen", "darkgreen"), legend = income, beside=TRUE, ylim
        = c(0, 600), args.legend = list(x ='topright', ncol = 2, cex = 0.75))
4. Create a histogram visualizing the distribution of priec levels for each racial background and repeat for others. Note that the bars are colored by count. 

        ggplot(white, aes(x=prod_totprice)) + geom_histogram(binwidth = 2, aes(fill = ..count..)) + ggtitle("Distribution of 
        Product Prices for White Customers") + xlab("Price Points") + ylab("Count") + scale_x_continuous(lim = c(0,100),
        breaks = seq(0,100,5))
5. Create a stacked area chart visualizing the total spend per month of each racial backgrounda and repeat for others.

        ggplot(white, aes(x=as.numeric(month), y=prod_totprice, colour=household_income, fill=household_income)) + 		geom_area() + scale_x_continuous(lim = c(1,12), breaks = seq(0,12,1)) + ggtitle("Caucasian Customers: Total Spend per 		Month") + xlab("Month") + ylab("Price Total") + scale_y_continuous(lim = c(0,40000), breaks = seq(0,40000,5000))
## Text Mining ## 
### Part One ###
1. Install packages.

        library(tm)
        library(wordcloud)
        library(dplyr)
        library(stringr)
2. Create a dictionary with assorted product types and convert to lowercase. 

        prod_type = c("shirt", "dress", "pant", "legging", "bra", "top", "sock", "hoodie", "sweatshirt", "sweater", "vest",
        "underwear", "tank", "crewneck", "hat", "beanie", "scarf", "jacket", "short", "skirt", "jumpsuit", "lingerie",
        "romper", "belt")
        df$prod_name = tolower(df$prod_name)`
3. Write a for loop that will detect if one of the terms in the above dictionary appears in the prodict name and create a new column for each of the results. 

        prod = data.frame(nrow = 11398)
        for (i in 1:24){
                prod = as.numeric(str_detect(df$prod_name, prod_type[i])) %>% data.frame(prod)
                print(prod)
        }
        prod = prod[,1:24]
        names(prod) = c("belt", "romper", "lingerie", "jumpsuit", "skirt", "short", "jacket", "scarf", "beanie", "hat",
        "crewneck", "tank", "underwear", "vest", "sweater", "sweatshirt", "hoodie", "sock", "top", "bra", "legging", "pant",
        "dress", "shirt")
4. Create a dataframe that provides the frequency percentage of each term within the dictionary. Visualize this in a barplot.

        q = colSums(prod)
        gg = data.frame(Word = factor(names(q), levels = names(q)), Count = (as.vector(q) / 11398) * 100)
        ggg <- gg[order(gg$Count,decreasing = TRUE),]
        barplot(ggg$Count, names = ggg$Word, ylab = "Percent (%)", main = "Product Type by % of Total Products", las = 2,
        ylim=c(0,25), col = rainbow(24))
### Part  ###
1. 



## Prediction Modeling ##


## About Me ##

Tolu Adebayo is a junior in the Wharton School, concentrating in Finance and Business Analytics. This data project aims to answer the following question: Which product type within the apparel product category do Amazon customers shop from the most in 2017? via 1) exploratory data analysis 2) text mining and 3) prediction models using selected features.  All of the coding, analysis, and visualizations were developed with R. Contact tadebayo@wharton.upenn.edu for further questions/comments/concerns!
