GDPandEducation\_CaseStudy1
================
Georges Michel, Jack Nelson, Nicole Shaver, Nathan Tuttle
February 25, 2017

Introduction
------------

In this study we will examine the Gross Domestic Product (GDP) rankings for countries across the world and analyze the relationship between GDP and their education levels. We will order the countries by GDP and evaluate the average GDP ranking for different education groups, visualize the GDP data for all countries by income group, and examine the relationship between income group and GDP. The analysis will utilize GDP and education data from the data-catalog at the world bank web site.

Code used to download, tidy and merge the data
----------------------------------------------

``` r
dir<-getwd()
source(paste(dir,"/source/download.R",sep=""))
source(paste(dir,"/source/cleanup_ED_GDP.R",sep=""))
source(paste(dir,"/source/merge_ED_GDP.R",sep=""))
```

Analysis Results
----------------

Q1. After merging the GDP and education datasets based on country code, 189 of the IDs match between the two datasets, as shown in the dataframe below, which has 189 rows after the matching between the education data and the GDP data is complete and NA's are excluded:

``` r
##use the str command to describe the number of observations.
##rows where the country code did not match were exluded by this line in merge_ED_GDP.R--
#combo2<-subset(combo,!is.na(Long.Name) & !is.na(GDP.Country))
str(combo2)
```

    ## 'data.frame':    189 obs. of  34 variables:
    ##  $ CountryCode                                      : chr  "ABW" "AFG" "AGO" "ALB" ...
    ##  $ GDP.ranking                                      : num  161 105 60 125 32 26 133 172 12 27 ...
    ##  $ GDP.Country                                      : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
    ##  $ GDP                                              : num  2584 20497 114147 12648 348595 ...
    ##  $ Long.Name                                        : chr  "Aruba" "Islamic State of Afghanistan" "People's Republic of Angola" "Republic of Albania" ...
    ##  $ Income.Group                                     : chr  "High income: nonOECD" "Low income" "Lower middle income" "Upper middle income" ...
    ##  $ Region                                           : chr  "Latin America & Caribbean" "South Asia" "Sub-Saharan Africa" "Europe & Central Asia" ...
    ##  $ Lending.category                                 : chr  NA "IDA" "IDA" "IBRD" ...
    ##  $ Other.groups                                     : chr  NA "HIPC" NA NA ...
    ##  $ Currency.Unit                                    : chr  "Aruban florin" "Afghan afghani" "Angolan kwanza" "Albanian lek" ...
    ##  $ Latest.population.census                         : chr  "2000" "1979" "1970" "2001" ...
    ##  $ Latest.household.survey                          : chr  NA "MICS, 2003" "MICS, 2001, MIS, 2006/07" "MICS, 2005" ...
    ##  $ Special.Notes                                    : chr  NA "Fiscal year end: March 20; reporting period for national accounts data: FY." NA NA ...
    ##  $ National.accounts.base.year                      : chr  "1995" "2002/2003" "1997" NA ...
    ##  $ National.accounts.reference.year                 : int  NA NA NA 1996 NA NA 1996 NA 2007 NA ...
    ##  $ System.of.National.Accounts                      : int  NA NA NA 1993 NA 1993 1993 NA 1993 1993 ...
    ##  $ SNA.price.valuation                              : chr  NA "VAB" "VAP" "VAB" ...
    ##  $ Alternative.conversion.factor                    : chr  NA NA "1991-96" NA ...
    ##  $ PPP.survey.year                                  : int  NA NA 2005 2005 NA 2005 2005 NA 2005 2005 ...
    ##  $ Balance.of.Payments.Manual.in.use                : chr  NA NA "BPM5" "BPM5" ...
    ##  $ External.debt.Reporting.status                   : chr  NA "Actual" "Actual" "Actual" ...
    ##  $ System.of.trade                                  : chr  "Special" "General" "Special" "General" ...
    ##  $ Government.Accounting.concept                    : chr  NA "Consolidated" NA "Consolidated" ...
    ##  $ IMF.data.dissemination.standard                  : chr  NA "GDDS" "GDDS" "GDDS" ...
    ##  $ Source.of.most.recent.Income.and.expenditure.data: chr  NA NA "IHS, 2000" "LSMS, 2005" ...
    ##  $ Vital.registration.complete                      : chr  NA NA NA "Yes" ...
    ##  $ Latest.agricultural.census                       : chr  NA NA "1964-65" "1998" ...
    ##  $ Latest.industrial.data                           : int  NA NA NA 2005 NA 2001 NA NA 2004 2004 ...
    ##  $ Latest.trade.data                                : int  2008 2008 1991 2008 2008 2008 2008 2007 2008 2008 ...
    ##  $ Latest.water.withdrawal.data                     : int  NA 2000 2000 2000 2005 2000 2000 1990 2000 2000 ...
    ##  $ X2.alpha.code                                    : chr  "AW" "AF" "AO" "AL" ...
    ##  $ WB.2.code                                        : chr  "AW" "AF" "AO" "AL" ...
    ##  $ Table.Name                                       : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
    ##  $ Short.Name                                       : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...

Q2. The country with the 13th lowest GDP (determined by sorting the data in ascending order by GDP, placing the United States last) is St. Kitts and Nevis. There were no NA values in the GDP column.

``` r
#sort in order of GDP
sorted_GDP<-combo2[order(combo2$GDP),]
#pick out the 13th row from the dataframe, just the columns needed to answer the question
GDP_13<-sorted_GDP[13,c(1,2,3,4)]
##display the 13th row
GDP_13
```

    ##     CountryCode GDP.ranking         GDP.Country GDP
    ## 109         KNA         178 St. Kitts and Nevis 767

``` r
#count the NA in the GDP column
sum(is.na(sorted_GDP$GDP))
```

    ## [1] 0

Q3. The average GDP ranking for the "High Income: OECD" group is 32.96667. The average GDP ranking for the "High Income: nonOECD" group is 91.91304. There were no NAs in the GDP ranking column.

``` r
# Create subset dataframe containing only 'High income: OECD' and 
# 'High income: nonOECD' income group countries
combo2.HighIncome <- subset(combo2, combo2$Income.Group == 'High income: OECD' | combo2$Income.Group == 'High income: nonOECD')

# Take the average GDP.ranking by income group of the subset with NA values removed
aggregate(combo2.HighIncome$GDP.ranking~combo2.HighIncome$Income.Group, combo2.HighIncome, mean, na.rm = TRUE)
```

    ##   combo2.HighIncome$Income.Group combo2.HighIncome$GDP.ranking
    ## 1           High income: nonOECD                      91.91304
    ## 2              High income: OECD                      32.96667

``` r
#count the number of NA values in GDP ranking
sum(is.na(combo2.HighIncome$GDP.ranking))
```

    ## [1] 0

``` r
sum(is.na(combo2.HighIncome$GDP))
```

    ## [1] 0

Q4. Plotting the GDP for all countries, coloring by Income Group, we note that most countries are lined up on a tight region near the bottom. Only a few countries in the next group and very few farthest region. The highest point in the High.income: nonOECD group. We can see how the income groups are spread out a little better by using the log of the values.

``` r
ggplot(data=combo2, aes(x=GDP, y=CountryCode, color=Income.Group))+geom_point()+geom_text(aes(label=combo2$CountryCode),hjust=0, vjust=0)+ggtitle("Country's by GDP colored by Income Group nonLog") 
```

![](GDPandEducation_CaseStudy1_files/figure-markdown_github/question%204-1.png)

``` r
ggplot(data=combo2, aes(x=log(GDP), y=CountryCode, color=Income.Group))+geom_point()+ggtitle("Country by GDP colored by Income Group Log")+geom_text(aes(label=combo2$CountryCode),hjust=0, vjust=0)+ theme(axis.title.y=element_blank(),
        axis.text.y=element_blank(),
        axis.ticks.y=element_blank(),
        plot.title = element_text(hjust = 0.5))
```

![](GDPandEducation_CaseStudy1_files/figure-markdown_github/question%204-2.png)

``` r
sum(is.na(combo2$CountryCode))
```

    ## [1] 0

``` r
sum(is.na(combo2$Income.Group))
```

    ## [1] 0

``` r
sum(is.na(combo2$GDP))
```

    ## [1] 0

Q5. Breaking the GDP ranking into 5 separate quantile groups, and then summarizing the Income Groups, wne see, as shown in the table below, that 5 countries are in the "Lower Middle Income" Group but are among the 38 ations with the highest GDP.

``` r
#make a table that doesn't have any NA for GDP.Ranking
ranking<-subset(combo2,!is.na(GDP.ranking))
#count the number of NAs that were in that column before the subset--answer is 35
sum(is.na(combo2$GDP.ranking))
```

    ## [1] 0

``` r
#determine how many members should be in each group
QuantSep <- ceiling(sum(!is.na(ranking$GDP.ranking))/5)
#divide the GDP ranking into 5 quantile groups
ranking$GDP.quant.group[ranking$GDP.ranking<=QuantSep]<-"G0_to_20"
ranking$GDP.quant.group[ranking$GDP.ranking>QuantSep & ranking$GDP.ranking<=(2*QuantSep)]<-"G20_to_40"
ranking$GDP.quant.group[ranking$GDP.ranking>(2*QuantSep) & ranking$GDP.ranking<=(3*QuantSep)]<-"G40_to_60"
ranking$GDP.quant.group[ranking$GDP.ranking>(3*QuantSep) & ranking$GDP.ranking<=(4*QuantSep)]<-"G60_to_80"
ranking$GDP.quant.group[ranking$GDP.ranking>(4*QuantSep)]<-"G80_to_100"
#summarize the data by IncomeGroup and GDP ranking group, print the result
QuantIncTable <- data.frame(table(ranking$Income.Group,ranking$GDP.quant.group))
names(QuantIncTable) <- c('IncomeGroup','Ranking','Freq')
QuantIncTable <- spread(QuantIncTable,Ranking,Freq)

QuantIncTable
```

    ##            IncomeGroup G0_to_20 G20_to_40 G40_to_60 G60_to_80 G80_to_100
    ## 1 High income: nonOECD        4         5         8         4          2
    ## 2    High income: OECD       18        10         1         1          0
    ## 3           Low income        0         1         9        16         11
    ## 4  Lower middle income        5        13        12         8         16
    ## 5  Upper middle income       11         9         8         8          9

Conclusion
----------

In this study of GDP and Education levels, using data from the world bank, we find that there are 189 countries from which we can make conclusions about the GDP and education levels. The country with the 13th lowest GDP is St. Kitts and Nevis. The High.IDncome:nonOCE groups have an average ranking of 91.91, while the High.Income:OECD groups have an average ranking of 32.97. Thus, within a given income group, being a member of the OCED leads to lower ranking (and thus higher GDP). Of the 38 countries with the highest GDP, five of the are classified as "Lower Middle Income". Examining GDP by country code shows many countries hold a a certain GDP threshold while only a small number have higher income groups. The highest point is not in the High.Income: nonOECD group but the High income: OECD group.
