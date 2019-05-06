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
  - prod_category: product category
  
## Exploratory Data Analysis ##
1. Install packages.

        library(stringr)
        library(tidyverse)
        library(ggplot2)	
2. Create separate datasets for each racial background. 

        white = subset(df, subset = df$racial_background == "Caucasian")
        black = subset(df, subset = df$racial_background == "African American")
        asian = subset(df, subset = df$racial_background == "Asian")
3. Create a barplot visualizing the amount of customers by household size and income and repeat for other racial backgrounds. Note that the blue bars represent lower-income, red bars represent the middle-income, and green bars represent upper-income. 

        income = c('Less than $25,000', '$25,000 – $39,999', '$40,000 – $59,999', '$60,000 – $74,999', '$75,000 – $99,999',
        '$100,000 – $149,999', '$150,000 – $199,999', '$200,000+')
        counts <- table(white$household_income, white$household_size)
        barplot(counts, main="Caucasian Customers by Size and Income",xlab="Household Size", ylab = "Count",
        col=c("darkblue", "darkblue", "red","red", "red", "red", "darkgreen", "darkgreen"), legend = income, beside=TRUE, ylim
        = c(0, 600), args.legend = list(x ='topright', ncol = 2, cex = 0.75))
<img width="699" alt="Screen Shot 2019-05-05 at 10 05 32 PM" src="https://user-images.githubusercontent.com/50304903/57204667-2eec0a80-6f87-11e9-83f5-719299b5b71c.png">
<img width="700" alt="Screen Shot 2019-05-05 at 10 08 08 PM" src="https://user-images.githubusercontent.com/50304903/57204665-2eec0a80-6f87-11e9-9e2b-abf4b4e4e281.png">
<img width="701" alt="Screen Shot 2019-05-05 at 10 08 02 PM" src="https://user-images.githubusercontent.com/50304903/57204666-2eec0a80-6f87-11e9-82fe-af035c3c1d14.png">
4. Create a histogram visualizing the distribution of price levels for each racial background and repeat for other racial backgrounds. Note that the bars are colored by count. 

        ggplot(white, aes(x=prod_totprice)) + geom_histogram(binwidth = 2, aes(fill = ..count..)) + ggtitle("Distribution of 
        Product Prices for White Customers") + xlab("Price Points") + ylab("Count") + scale_x_continuous(lim = c(0,100),
        breaks = seq(0,100,5))
<img width="699" alt="Screen Shot 2019-05-05 at 10 54 52 PM" src="https://user-images.githubusercontent.com/50304903/57204935-ecc3c880-6f88-11e9-9f1e-1f0283a123bf.png">
<img width="698" alt="Screen Shot 2019-05-05 at 10 08 34 PM" src="https://user-images.githubusercontent.com/50304903/57204664-2eec0a80-6f87-11e9-9f31-13167470f42c.png">
<img width="698" alt="Screen Shot 2019-05-05 at 10 08 40 PM" src="https://user-images.githubusercontent.com/50304903/57204663-2eec0a80-6f87-11e9-99e9-372c38d5bc54.png">
5. Create a stacked area chart visualizing the total spend per month of each racial background and repeat for other racial backgrounds.

        ggplot(white, aes(x=as.numeric(month), y=prod_totprice, colour=household_income, fill=household_income)) + 
        geom_area() + scale_x_continuous(lim = c(1,12), breaks = seq(0,12,1)) + ggtitle("Caucasian Customers: Total Spend per
        Month") + xlab("Month") + ylab("Price Total") + scale_y_continuous(lim = c(0,40000), breaks = seq(0,40000,5000))
<img width="700" alt="Screen Shot 2019-05-05 at 10 13 08 PM" src="https://user-images.githubusercontent.com/50304903/57204660-2e537400-6f87-11e9-924a-5a35ad1eb64b.png">
<img width="699" alt="Screen Shot 2019-05-05 at 10 13 18 PM" src="https://user-images.githubusercontent.com/50304903/57204659-2e537400-6f87-11e9-851c-d6097caf56c6.png">
<img width="696" alt="Screen Shot 2019-05-05 at 10 13 23 PM" src="https://user-images.githubusercontent.com/50304903/57204658-2e537400-6f87-11e9-94a1-ae68c715538a.png">
## Text Mining ## 
### Part One ###
1. Install packages.

        library(tm)
        library(wordcloud)
        library(dplyr)
        library(stringr)
2. Create a dictionary with assorted product types and convert the text to lowercase. 

        prod_type = c("shirt", "dress", "pant", "legging", "bra", "top", "sock", "hoodie", "sweatshirt", "sweater", "vest",
        "underwear", "tank", "crewneck", "hat", "beanie", "scarf", "jacket", "short", "skirt", "jumpsuit", "lingerie",
        "romper", "belt")
        df$prod_name = tolower(df$prod_name)`
3. Write a for loop that will detect if one of the terms in the above dictionary appears in the product name and create a new column for each of the results. 

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
<img width="697" alt="Screen Shot 2019-05-05 at 10 13 40 PM" src="https://user-images.githubusercontent.com/50304903/57204657-2e537400-6f87-11e9-8f1d-641262bd3693.png">
### Part Two ###
1. Clean up the text corpus in the product name column of the dataframe. 

        corp.original = VCorpus(VectorSource(df$prod_name))
        corp = tm_map(corp.original, content_transformer(tolower)) 
        corp = tm_map(corp, removeNumbers)
        corp = tm_map(corp, removeWords, stopwords("SMART"))
        corp = tm_map(corp, removeWords, c("amazon.com", "llc", "size", "amazon"))
        corp = tm_map(corp, stripWhitespace)
        corp = tm_map(corp, removePunctuation) 
2. Create a document-term matrix from the cleaned data and remove sparse terms. 

        dtm = DocumentTermMatrix(corp)
        dtms = removeSparseTerms(dtm, .97)
        dtms = as.matrix(dtm)
3. Identify the top 30 words in the document-term matrix and plot them according to frequency in a bar chart.

        word.freq = colSums(dtms)
        word.freq = sort(word.freq, decreasing=T)
        top = data.frame(Word = factor(names(word.freq[1:30]), levels = names(word.freq[1:30])), Count =
        as.vector(word.freq[1:30]))
        barplot(top$Count, names = top$Word, ylab = "Count", main = "Top 30 Words According to Product Name", las = 2, col
        rainbow(30))
<img width="696" alt="Screen Shot 2019-05-05 at 10 14 00 PM" src="https://user-images.githubusercontent.com/50304903/57204656-2e537400-6f87-11e9-89d0-d928315a7132.png">
## Prediction Modeling ##
1. Identify the dependent variable of interest (racial background). Compute the correlations between words and the outcome and keep only the 50 most important words. Repeat this step for other racial backgrounds. 

        df$white = df$racial_background == "Caucasian"
        corr = cor(df$white, dtms)
        corr = abs(corr)
        top50 = order(corr, decreasing=T)[1:50]
        top50words = colnames(corr)[top50]
        top50words
2. Create a data frame including the dependent varaible and the narrowed list of words. Then run a logistic regression. Repeat this step for the other racial backgrounds. 

        foo = as.data.frame(cbind(white = df$white, dtms[,top50words]))
        model = glm(white ~., foo, family=binomial)
        summary(model)
3. Which words are the most important for predicting racial background? Search for the words with positive coefficients only. (Positive coefficients mean the presence of a word increases the chance of a product being bought by a particular racial background.) Repeat this step for the other racial backgrounds. 

        coef = coef(model)[-1]
        positive.terms = coef[coef>0]
        top.positive = sort(positive.terms,decreasing=T)[1:10]
4. Create a wordcloud visualizing the words with positive coefficients. Repeat this step for the other racial backgrounds. 

        wordcloud(names(top.positive)[1:5], as.vector(top.positive)[1:5], random.order=FALSE, rot.per=0.35,
        use.r.layout=FALSE, colors= brewer.pal(8, "Dark2"))
<img width="698" alt="Screen Shot 2019-05-05 at 10 18 08 PM" src="https://user-images.githubusercontent.com/50304903/57204653-2e537400-6f87-11e9-94ae-7948260b5472.png">
<img width="700" alt="Screen Shot 2019-05-05 at 10 17 52 PM" src="https://user-images.githubusercontent.com/50304903/57204654-2e537400-6f87-11e9-80e8-265e2f91680c.png">
<img width="702" alt="Screen Shot 2019-05-05 at 10 17 29 PM" src="https://user-images.githubusercontent.com/50304903/57204655-2e537400-6f87-11e9-8592-b959cfccce03.png">




## About Me ##

Tolu Adebayo is a junior in the Wharton School, concentrating in Finance and Business Analytics. This data project aims to answer the following question: Which product type within the apparel product category do Amazon customers shop from the most in 2017? via 1) exploratory data analysis 2) text mining and 3) prediction models using selected features.  All of the coding, analysis, and visualizations were developed with R. Contact tadebayo@wharton.upenn.edu for further questions/comments/concerns!
