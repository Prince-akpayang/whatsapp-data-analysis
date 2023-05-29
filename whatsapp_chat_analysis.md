---
title: "Family whatsapp group analysis"
author: "Etini Akpayang"
date: "2031-07-15"
output:
  html_document: 
    toc: yes
    keep_md: yes
---


```r
knitr::opts_chunk$set(echo = F)
knitr::opts_chunk$set(eval = T)
knitr::opts_chunk$set(warning = F)
knitr::opts_chunk$set(error = F)
knitr::opts_chunk$set(cache = T)
#setwd("C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment folder/sentimentanals/")
```

## Introduction

This project is to showcase my skill in sentiment analysis, so i will be drawing insights from a family whatsapp group data. the following can be extected.

-Data wrangling

-Exploratory data analysis

-Visualizations 

-Analysis

-Inferences


## Loading required data and packages

The following packages will be loaded for this study incase needed.


```r
library(rwhatsapp)
library(ggplot2)
library(lubridate)
library(tidytext)
library(stopwords)
library(tidyverse)
library(tidymodels)
library(syuzhet)
library(RColorBrewer)
library(dplyr)
library(stringr)
library(igraph)
library(tm)
require(wordcloud)
```


This packages individually have their role to play assuming we’d need them, cause a few of them can do simultaneously what another can, so learning and expertly understanding just one or a few maybe enough, we will give a brief run of its significance.


### packages and uses

|   Package  |use/importance|
|------------|--------------|
|rwhatsapp	 |rwhatsapp provides some infrastructure to work with WhatsApp text data in R. It is a small and robust package.             |
|ggplot2|	It is a popular package which is used for building Plots and effective Visualizations.|
|lubridate|	This package is used for Lubricating the use of Dates and Times in any Analysis.|
|wordcloud	 | Used in building the wordcloud.|
|tidytext	   | Makes the text mining process easier and consistent.|
|topwords	   | For inclusion of Stopwords.|
|tidyverse	 | It is a collection of the few packages in R, so the packages get self-included on including tidyverse.                  |
|tidymodels|	Forms the basis of tidy machine learning.|
|syuzhet	   | Extracts the sentiment and sentiment-derived plot arcs from text using a variety of sentiment dictionaries.               |
|dplyr| Provides efficient set of tools for data manipulation and transformation.|
|stringr| offers loads of tools for string manipulation and text processing.|
|igraph| a powerful package for analysing and visualizing network data.|
|tm|specifically designed for text mining|


### Data

Its time to load the data downloaded from the whatsapp.



```
## [1] "time"       "author"     "text"       "source"     "emoji"     
## [6] "emoji_name"
```

```
## Rows: 874
## Columns: 6
## $ time       <dttm> 2022-12-04 13:38:56, 2022-12-04 11:51:56, 2022-12-04 11:51…
## $ author     <chr> NA, NA, NA, NA, "Mfonakem New", "Mfonakem New", "Mfonakem N…
## $ text       <chr> "Messages and calls are end-to-end encrypted. No one outsid…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <"💙", "💙">, <NULL>, <NULL>, <NULL>, <NULL>, <NUL…
## $ emoji_name <list> <NULL>, <"blue heart", "blue heart">, <NULL>, <NULL>, <NUL…
```

```
## # A tibble: 6 × 6
##   time                author       text                 source emoji  emoji_name
##   <dttm>              <chr>        <chr>                <chr>  <list> <list>    
## 1 2022-12-04 13:38:56 <NA>         "Messages and calls… C:/Us… <NULL> <NULL>    
## 2 2022-12-04 11:51:56 <NA>         "Nsikan Family crea… C:/Us… <chr>  <chr [2]> 
## 3 2022-12-04 11:51:56 <NA>         "Nsikan Family adde… C:/Us… <NULL> <NULL>    
## 4 2022-12-04 11:51:56 <NA>         "Nsikan Family chan… C:/Us… <NULL> <NULL>    
## 5 2022-12-04 11:59:56 Mfonakem New "<Media omitted>"    C:/Us… <NULL> <NULL>    
## 6 2022-12-04 12:21:56 Mfonakem New "<Media omitted>"    C:/Us… <NULL> <NULL>
```

```
## # A tibble: 6 × 6
##   time                author        text                source emoji  emoji_name
##   <dttm>              <chr>         <chr>               <chr>  <list> <list>    
## 1 2023-05-17 17:01:56 Lady Diana    Abet if you no get… C:/Us… <NULL> <NULL>    
## 2 2023-05-17 17:06:56 Lady Diana    Serzli bro          C:/Us… <NULL> <NULL>    
## 3 2023-05-17 17:06:56 Lady Diana    Nice to watch want… C:/Us… <NULL> <NULL>    
## 4 2023-05-17 17:16:56 Mfonakem New  When the bough bre… C:/Us… <NULL> <NULL>    
## 5 2023-05-17 17:19:56 Lady Diana    Let me find it      C:/Us… <NULL> <NULL>    
## 6 2023-05-17 17:20:56 Nsikan Family Why?                C:/Us… <NULL> <NULL>
```
## Summary of the data

We can summarize the important features of the data as the emojis are in a list format so a summary will not be appropriate enough hence later on we'd look into those indeptly.


```
##       time                           author              text          
##  Min.   :2022-12-04 11:51:56.82   Length:874         Length:874        
##  1st Qu.:2023-01-17 15:21:26.82   Class :character   Class :character  
##  Median :2023-03-09 17:35:26.82   Mode  :character   Mode  :character  
##  Mean   :2023-02-27 00:37:16.04                                        
##  3rd Qu.:2023-04-06 10:34:26.82                                        
##  Max.   :2023-05-17 17:20:56.82
```
From the summary we see that tthe data ranges from 4th December, 2022 to 17th May, 2023.

##  Message Count Analysis:
Here we will visualize the total messages sent by the users in the group chat. 

![](sentiments_files/figure-html/Message Count Analysis-1.png)<!-- -->

```
## # A tibble: 8 × 2
##   author           Message_Count
##   <chr>                    <int>
## 1 Mfonakem New               337
## 2 Nsikan Family              142
## 3 Etini Akpayang             121
## 4 dad                        101
## 5 Diana                       63
## 6 Lady Diana                  25
## 7 Mrs Akpayang Mum            19
## 8 Dad                          9
```

<!-- ## Density plot for the times users made a post in the group chat -->

<!-- ```{r Message_Count density plot} -->
<!-- ggplot(whatsapp_data,aes(y=author,color=author))+ -->
<!--   geom_density()+ -->
<!--   facet_wrap(~author)+ -->
<!--   theme(axis.text.x = element_text(angle = 90,vjust = 0.5)) -->
<!-- ``` -->


##  Word Frequency Analysis:

This section we will visualize the words we use and our focus will be on the most frequently used one.

```
## Rows: 3,115
## Columns: 2
## $ .    <fct> the, to, and, a, of, you, in, i, is, or, it, with, at, that, for,…
## $ Freq <int> 524, 344, 280, 276, 266, 194, 193, 168, 162, 137, 130, 123, 119, …
```


From the data above there have been 3,115 distinct word used in this group till the date the data was collected lets view the top 50,


```
##       words Freq
## 1       the  524
## 2        to  344
## 3       and  280
## 4         a  276
## 5        of  266
## 6       you  194
## 7        in  193
## 8         i  168
## 9        is  162
## 10       or  137
## 11       it  130
## 12     with  123
## 13       at  119
## 14     that  111
## 15      for  103
## 16     this   92
## 17       be   87
## 18      not   85
## 19     your   82
## 20     from   78
## 21  credits   76
## 22      are   75
## 23       on   63
## 24     have   62
## 25      one   61
## 26       as   59
## 27       my   59
## 28     call   53
## 29     they   51
## 30  started   50
## 31      all   48
## 32       no   48
## 33       gl   47
## 34     fslc   44
## 35 obtained   44
## 36    state   44
## 37      two   44
## 38   family   43
## 39      out   43
## 40      can   42
## 41     will   42
## 42 diabetic   40
## 43    their   40
## 44    video   40
## 45      but   39
## 46     five   39
## 47  sitting   39
## 48      was   39
## 49      apc   38
## 50     four   38
```

we can still use a bar plot to see this 

![](sentiments_files/figure-html/top 50 words used plot-1.png)<!-- -->


we can still visualize this words by individuals too to know each individuals most used words, first we modify the data by individuals


```
## [1] NA                 "Nsikan Family"    "Mfonakem New"     "Etini Akpayang"  
## [5] "Mrs Akpayang Mum" "dad"              "Diana"            "Lady Diana"      
## [9] "Dad"
```

```
## Rows: 82
## Columns: 6
## $ time       <dttm> 2022-12-04 12:42:56, 2022-12-05 16:48:56, 2022-12-05 20:40…
## $ author     <chr> "Nsikan Family", "Nsikan Family", "Nsikan Family", "Nsikan …
## $ text       <chr> "congratulations 🎧", "good evening how una dey", "good eve…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> "🎉", <NULL>, "😎", "😁", <NULL>, <NULL>, <NULL>, <NULL>, …
## $ emoji_name <list> "party popper", <NULL>, "smiling face with sunglasses", "b…
```

```
## Rows: 115
## Columns: 6
## $ time       <dttm> 2022-12-04 13:03:56, 2022-12-04 13:44:56, 2022-12-04 16:17…
## $ author     <chr> "Mfonakem New", "Mfonakem New", "Mfonakem New", "Mfonakem N…
## $ text       <chr> "theres more", "thank you", "thank you for making my day to…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, "❤️", <"😂", "😂", "😂", "😂">, <NULL>, <NU…
## $ emoji_name <list> <NULL>, <NULL>, "red heart", <"face with tears of joy", "f…
```

```
## Rows: 91
## Columns: 6
## $ time       <dttm> 2022-12-04 13:52:56, 2022-12-04 14:16:56, 2022-12-05 10:30…
## $ author     <chr> "Etini Akpayang", "Etini Akpayang", "Etini Akpayang", "Etin…
## $ text       <chr> "weldon oga", "guy better do give away oh", "world cup 🂬", …
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, "🍵", <NULL>, <NULL>, <NULL>, <NULL>, <NUL…
## $ emoji_name <list> <NULL>, <NULL>, "teacup without handle", <NULL>, <NULL>, <…
```

```
## Rows: 12
## Columns: 6
## $ time       <dttm> 2022-12-04 17:57:56, 2023-01-10 11:08:56, 2023-01-10 11:09…
## $ author     <chr> "Mrs Akpayang Mum", "Mrs Akpayang Mum", "Mrs Akpayang Mum",…
## $ text       <chr> "congratulations my baby boy", "hi girl congratulation tell…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
```

```
## Rows: 70
## Columns: 6
## $ time       <dttm> 2022-12-05 20:38:56, 2022-12-05 20:39:56, 2022-12-05 22:36…
## $ author     <chr> "dad", "dad", "dad", "dad", "dad", "dad", "dad", "dad", "da…
## $ text       <chr> "wow da congratulations  see you soon", "wow da congratulat…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
```

```
## Rows: 47
## Columns: 6
## $ time       <dttm> 2023-03-12 11:33:56, 2023-03-12 11:46:56, 2023-03-12 11:49…
## $ author     <chr> "Diana", "Diana", "Diana", "Diana", "Diana", "Diana", "Dian…
## $ text       <chr> "httpsyoutubecomshortsxoxmwurdkfeatureshare", "httpsyoutube…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "🙉", <NUL…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "hear-no-e…
```

```
## Rows: 12
## Columns: 6
## $ time       <dttm> 2023-04-11 19:35:56, 2023-04-15 12:31:56, 2023-04-15 12:32…
## $ author     <chr> "Lady Diana", "Lady Diana", "Lady Diana", "Lady Diana", "La…
## $ text       <chr> "have finally sold her problem solved", "please corrections…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
```

```
## Rows: 8
## Columns: 6
## $ time       <dttm> 2023-04-14 19:29:56, 2023-04-15 04:53:56, 2023-04-15 05:01…
## $ author     <chr> "Dad", "Dad", "Dad", "Dad", "Dad", "Dad", "Dad", "Dad"
## $ text       <chr> "yes welcome to my whatsapp business", "do you mean that yo…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "👑", <NULL>, <NULL…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "crown", <NULL>, <…
```


The next task is to clean out the words from the individual chats


```
## Rows: 251
## Columns: 2
## $ .    <fct> i, the, is, good, my, a, it, me, you, for, and, have, morning, no…
## $ Freq <int> 19, 12, 9, 8, 8, 7, 7, 7, 7, 6, 5, 5, 5, 5, 5, 5, 4, 4, 4, 4, 4, …
```

```
## Rows: 342
## Columns: 2
## $ .    <fct> the, to, and, i, you, im, not, yes, get, at, is, it, my, on, oo, …
## $ Freq <int> 18, 17, 13, 9, 8, 7, 7, 7, 6, 5, 5, 5, 5, 5, 5, 5, 4, 4, 4, 4, 4,…
```

```
## Rows: 505
## Columns: 2
## $ .    <fct> and, the, a, to, that, in, of, she, her, it, you, de, eh, was, al…
## $ Freq <int> 35, 35, 30, 21, 18, 16, 16, 16, 12, 11, 11, 10, 8, 7, 6, 6, 6, 6,…
```

```
## Rows: 52
## Columns: 2
## $ .    <fct> are, girl, them, you, 😂😂, a, akwa, and, baby, be, boss, boy, co…
## $ Freq <int> 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
```

```
## Rows: 2,500
## Columns: 2
## $ .    <fct> the, to, of, and, a, in, is, you, or, i, at, with, it, for, that,…
## $ Freq <int> 440, 283, 239, 214, 183, 167, 140, 135, 133, 122, 112, 108, 101, …
```

```
## Rows: 231
## Columns: 2
## $ .    <fct> you, the, to, i, be, no, and, me, my, that, calls, for, he, her, …
## $ Freq <int> 18, 10, 10, 9, 8, 8, 7, 6, 6, 6, 5, 5, 5, 5, 5, 5, 5, 4, 4, 4, 4,…
```

```
## Rows: 63
## Columns: 2
## $ .    <fct> highly, movie, no, the, watch, a, abet, again, and, appreciated, …
## $ Freq <int> 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
```

```
## Rows: 76
## Columns: 2
## $ .    <fct> the, it, my, not, you, and, do, grades, is, unwana, whatsapp, abo…
## $ Freq <int> 6, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
```


we can go on to plot the top 20 words mostly used bu members in this group;

![](sentiments_files/figure-html/top 20 words used by each membeers plotted-1.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-2.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-3.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-4.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-5.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-6.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-7.png)<!-- -->![](sentiments_files/figure-html/top 20 words used by each membeers plotted-8.png)<!-- -->



##  Sentiment Analysis:
This has to do with the general theme of the message delivered, it can be grouped in different contexts such as angry, happy, negative or positive but in this project we will only consider positive and negative tones.

Sentiment analysis, in simple terms, is the process of determining the emotional tone or attitude behind a piece of text or spoken words. It involves using computer algorithms to analyze and understand whether the expressed sentiment is positive, negative, or neutral. By examining the language and context, sentiment analysis can help identify the overall sentiment of a given text, such as a social media post, customer review, or news article.



```
## Joining with `by = join_by(word)`
```

```
## Rows: 743
## Columns: 7
## $ time       <dttm> 2022-12-04 12:42:56, 2022-12-04 13:44:56, 2022-12-04 14:16…
## $ author     <chr> "Nsikan Family", "Mfonakem New", "Etini Akpayang", "Mfonake…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> "🎉", <NULL>, <NULL>, "❤️", "❤️", <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> "party popper", <NULL>, <NULL>, "red heart", "red heart", …
## $ word       <chr> "congratulations", "thank", "better", "thank", "appreciate"…
## $ sentiment  <chr> "positive", "positive", "positive", "positive", "positive",…
```

```
##  Factor w/ 2 levels "negative","positive": 2 2 2 2 2 2 2 2 2 2 ...
```

```
## Rows: 16
## Columns: 3
## $ author    <fct> dad, Dad, Diana, Etini Akpayang, Lady Diana, Mfonakem New, M…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 246, 1, 10, 14, 2, 9, 2, 5, 313, 3, 7, 61, 7, 21, 9, 33
```

![](sentiments_files/figure-html/Sentiment Analysis for the whole group-1.png)<!-- -->

```
##              author sentiment Freq
## 1               dad  negative  246
## 2               Dad  negative    1
## 3             Diana  negative   10
## 4    Etini Akpayang  negative   14
## 5        Lady Diana  negative    2
## 6      Mfonakem New  negative    9
## 7  Mrs Akpayang Mum  negative    2
## 8     Nsikan Family  negative    5
## 9               dad  positive  313
## 10              Dad  positive    3
## 11            Diana  positive    7
## 12   Etini Akpayang  positive   61
## 13       Lady Diana  positive    7
## 14     Mfonakem New  positive   21
## 15 Mrs Akpayang Mum  positive    9
## 16    Nsikan Family  positive   33
```
Not the interpretation is based on bing dictionary that sees some words as negative/positive and automatically classify based on this as it was taught.

So the context may not be too negative but it is classified thus thats why more than too classifications can be better at explaining context or sentiment behind a text.



```
## Rows: 6,176
## Columns: 4
## $ author    <fct> dad, Dad, Diana, Etini Akpayang, Lady Diana, Mfonakem New, M…
## $ word      <fct> abnormal, abnormal, abnormal, abnormal, abnormal, abnormal, …
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
```

![](sentiments_files/figure-html/Top 20 sentiment words-1.png)<!-- -->


## Time Series Analysis:
Time series is always about observation with respect to time we can view our observations as a group with respect to time.


![](sentiments_files/figure-html/Time series analysis-1.png)<!-- -->


And individually too;

![](sentiments_files/figure-html/individual TS plots-1.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-2.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-3.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-4.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-5.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-6.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-7.png)<!-- -->![](sentiments_files/figure-html/individual TS plots-8.png)<!-- -->


Models could be fitted using this data to predict future message counts per day, but since it is not large may not predict accurately.


## Emoji Analysis:
This project will not be complete without our most favourite option analysis **"The Emoji"**

Lets see the most frequently used emojis by us;

and who uses which most emojis and their favorite go to emoji.


```
## 
##    😂    🤣 🏃🏼‍♀️    😭    ❗    😎    🥲     ❤️    🌞    🎉    💙    😁    🥸 
##    23     6     4     4     3     3     3     2     2     2     2     2     2 
##     ☺️     ☘️    🇨🇦    🍀    🍵    🐛    👑    💸    😇    😊    😌    😍    😤 
##     1     1     1     1     1     1     1     1     1     1     1     1     1 
##    🙉  🙏🏻 
##     1     1
```

```
## 
##                face with tears of joy         rolling on the floor laughing 
##                                    23                                     6 
##                    loudly crying face woman running: medium-light skin tone 
##                                     4                                     4 
##              ⊛ smiling face with tear                      exclamation mark 
##                                     3                                     3 
##          smiling face with sunglasses                      ⊛ disguised face 
##                                     3                                     2 
##        beaming face with smiling eyes                            blue heart 
##                                     2                                     2 
##                          party popper                             red heart 
##                                     2                                     2 
##                         sun with face                                   bug 
##                                     2                                     1 
##                                 crown             face with steam from nose 
##                                     1                                     1 
##                          flag: Canada         folded hands: light skin tone 
##                                     1                                     1 
##                      four leaf clover                   hear-no-evil monkey 
##                                     1                                     1 
##                      money with wings                         relieved face 
##                                     1                                     1 
##                              shamrock                          smiling face 
##                                     1                                     1 
##                smiling face with halo          smiling face with heart-eyes 
##                                     1                                     1 
##        smiling face with smiling eyes                 teacup without handle 
##                                     1                                     1
```

![](sentiments_files/figure-html/emoji general analysis-1.png)<!-- -->


On a less general note, now lets go individually, Oh!!!!;

```
## Rows: 82
## Columns: 7
## $ time       <dttm> 2022-12-04 12:42:56, 2022-12-05 16:48:56, 2022-12-05 20:40…
## $ author     <chr> "Nsikan Family", "Nsikan Family", "Nsikan Family", "Nsikan …
## $ text       <chr> "congratulations 🎧", "good evening how una dey", "good eve…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> "🎉", <NULL>, "😎", "😁", <NULL>, <NULL>, <NULL>, <NULL>, …
## $ emoji_name <list> "party popper", <NULL>, "smiling face with sunglasses", "b…
## $ Timestamp  <dttm> 2022-12-04 12:42:56, 2022-12-05 16:48:56, 2022-12-05 20:40…
```

```
## Rows: 115
## Columns: 7
## $ time       <dttm> 2022-12-04 13:03:56, 2022-12-04 13:44:56, 2022-12-04 16:17…
## $ author     <chr> "Mfonakem New", "Mfonakem New", "Mfonakem New", "Mfonakem N…
## $ text       <chr> "theres more", "thank you", "thank you for making my day to…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, "❤️", <"😂", "😂", "😂", "😂">, <NULL>, <NU…
## $ emoji_name <list> <NULL>, <NULL>, "red heart", <"face with tears of joy", "f…
## $ Timestamp  <dttm> 2022-12-04 13:03:56, 2022-12-04 13:44:56, 2022-12-04 16:17…
```

```
## Rows: 91
## Columns: 7
## $ time       <dttm> 2022-12-04 13:52:56, 2022-12-04 14:16:56, 2022-12-05 10:30…
## $ author     <chr> "Etini Akpayang", "Etini Akpayang", "Etini Akpayang", "Etin…
## $ text       <chr> "weldon oga", "guy better do give away oh", "world cup 🂬", …
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, "🍵", <NULL>, <NULL>, <NULL>, <NULL>, <NUL…
## $ emoji_name <list> <NULL>, <NULL>, "teacup without handle", <NULL>, <NULL>, <…
## $ Timestamp  <dttm> 2022-12-04 13:52:56, 2022-12-04 14:16:56, 2022-12-05 10:30…
```

```
## Rows: 47
## Columns: 7
## $ time       <dttm> 2023-03-12 11:33:56, 2023-03-12 11:46:56, 2023-03-12 11:49…
## $ author     <chr> "Diana", "Diana", "Diana", "Diana", "Diana", "Diana", "Dian…
## $ text       <chr> "httpsyoutubecomshortsxoxmwurdkfeatureshare", "httpsyoutube…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "🙉", <NUL…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "hear-no-e…
## $ Timestamp  <dttm> 2023-03-12 11:33:56, 2023-03-12 11:46:56, 2023-03-12 11:49…
```

```
## Rows: 12
## Columns: 7
## $ time       <dttm> 2023-04-11 19:35:56, 2023-04-15 12:31:56, 2023-04-15 12:32…
## $ author     <chr> "Lady Diana", "Lady Diana", "Lady Diana", "Lady Diana", "La…
## $ text       <chr> "have finally sold her problem solved", "please corrections…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ Timestamp  <dttm> 2023-04-11 19:35:56, 2023-04-15 12:31:56, 2023-04-15 12:32…
```

```
## Rows: 70
## Columns: 7
## $ time       <dttm> 2022-12-05 20:38:56, 2022-12-05 20:39:56, 2022-12-05 22:36…
## $ author     <chr> "dad", "dad", "dad", "dad", "dad", "dad", "dad", "dad", "da…
## $ text       <chr> "wow da congratulations  see you soon", "wow da congratulat…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ Timestamp  <dttm> 2022-12-05 20:38:56, 2022-12-05 20:39:56, 2022-12-05 22:36…
```

```
## Rows: 8
## Columns: 7
## $ time       <dttm> 2023-04-14 19:29:56, 2023-04-15 04:53:56, 2023-04-15 05:01…
## $ author     <chr> "Dad", "Dad", "Dad", "Dad", "Dad", "Dad", "Dad", "Dad"
## $ text       <chr> "yes welcome to my whatsapp business", "do you mean that yo…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "👑", <NULL>, <NULL…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "crown", <NULL>, <…
## $ Timestamp  <dttm> 2023-04-14 19:29:56, 2023-04-15 04:53:56, 2023-04-15 05:01…
```

```
## Rows: 12
## Columns: 7
## $ time       <dttm> 2022-12-04 17:57:56, 2023-01-10 11:08:56, 2023-01-10 11:09…
## $ author     <chr> "Mrs Akpayang Mum", "Mrs Akpayang Mum", "Mrs Akpayang Mum",…
## $ text       <chr> "congratulations my baby boy", "hi girl congratulation tell…
## $ source     <chr> "C:/Users/Etini Akpayang/Documents/CODE/ANALYSIS/sentiment …
## $ emoji      <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ emoji_name <list> <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <N…
## $ Timestamp  <dttm> 2022-12-04 17:57:56, 2023-01-10 11:08:56, 2023-01-10 11:09…
```

```
## 
## 🏃🏼‍♀️    😭    🌞    😂    🤣    🎉    😁    😊    😎 
##     4     3     2     2     2     1     1     1     1
```

```
## 
## woman running: medium-light skin tone                    loudly crying face 
##                                     4                                     3 
##                face with tears of joy         rolling on the floor laughing 
##                                     2                                     2 
##                         sun with face        beaming face with smiling eyes 
##                                     2                                     1 
##                          party popper        smiling face with smiling eyes 
##                                     1                                     1 
##          smiling face with sunglasses 
##                                     1
```

```
## 
## 😂 🥲  ❤️  ☺️ 😁 😌 😍 😤 😭 
## 17  3  2  1  1  1  1  1  1
```

```
## 
##         face with tears of joy       ⊛ smiling face with tear 
##                             17                              3 
##                      red heart beaming face with smiling eyes 
##                              2                              1 
##      face with steam from nose             loudly crying face 
##                              1                              1 
##                  relieved face                   smiling face 
##                              1                              1 
##   smiling face with heart-eyes 
##                              1
```

```
## 
## 😎 🥸 🍵 🎉 😇 
##  2  2  1  1  1
```

```
## 
##             ⊛ disguised face smiling face with sunglasses 
##                            2                            2 
##                 party popper       smiling face with halo 
##                            1                            1 
##        teacup without handle 
##                            1
```

```
## 
## 🤣 🙉 
##  4  1
```

```
## 
## rolling on the floor laughing           hear-no-evil monkey 
##                             4                             1
```

```
## 
##   ❗    ☘️   🇨🇦   🍀   🐛 🙏🏻 
##    3    1    1    1    1    1
```

```
## 
##              exclamation mark                           bug 
##                             3                             1 
##                  flag: Canada folded hands: light skin tone 
##                             1                             1 
##              four leaf clover                      shamrock 
##                             1                             1
```

```
## 
## 😂 💸 
##  4  1
```

```
## 
## face with tears of joy       money with wings 
##                      4                      1
```

![](sentiments_files/figure-html/emoji personal analysis-1.png)<!-- -->![](sentiments_files/figure-html/emoji personal analysis-2.png)<!-- -->![](sentiments_files/figure-html/emoji personal analysis-3.png)<!-- -->![](sentiments_files/figure-html/emoji personal analysis-4.png)<!-- -->![](sentiments_files/figure-html/emoji personal analysis-5.png)<!-- -->![](sentiments_files/figure-html/emoji personal analysis-6.png)<!-- -->

So from the analysis so far we have seen two group members have two whatsapp line they use in chattin and Dads line 2 and Dianas line 2 has not used emojis yet up to the time this data was collected.


## Create a word cloud
Yes, the last visuals we will consider in this project is the word cloud.

We saw the most words most likely to be used and used in the group in table and bar plot but this another beautiful representation that from mere looking you get the most used word.

We will do this for the words and the words classifed under the sentiment analysis too.

![Word cloud showing most frequently used words in the group chat by all users](sentiments_files/figure-html/word cloud for favourite words1-1.png)



### How about individuals favourite words used;

![Top words used most by Nsikan in the chat](sentiments_files/figure-html/word cloud for favourite words2-1.png)


![Top words used most by Mfonakem in the chat](sentiments_files/figure-html/word cloud for favourite words3-1.png)

![Top words used most by Etini in the chat](sentiments_files/figure-html/word cloud for favourite words4-1.png)

![Top words used most by Diana line 1 in the chat](sentiments_files/figure-html/word cloud for favourite words5-1.png)


![Top words used most by Diana line 2 in the chat](sentiments_files/figure-html/word cloud for favourite words6-1.png)


![Top words used most by Dad line 1 in the chat](sentiments_files/figure-html/word cloud for favourite words7-1.png)


![Top words used most by Dad line 2 in the chat](sentiments_files/figure-html/word cloud for favourite words8-1.png)


![Top words used most by Mum in the chat](sentiments_files/figure-html/word cloud for favourite words9-1.png)

### sentiment clouds


How about our individual themes/thoughts inferred from chats, first lets get data group sentiment and from individual members of the group.



```
## Rows: 42
## Columns: 4
## $ author    <fct> Nsikan Family, Nsikan Family, Nsikan Family, Nsikan Family, …
## $ word      <fct> appreciated, beautiful, better, congratulations, dead, died,…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, …
```

```
## Rows: 44
## Columns: 4
## $ author    <fct> Mfonakem New, Mfonakem New, Mfonakem New, Mfonakem New, Mfon…
## $ word      <fct> appreciate, balanced, breaks, clear, clearly, congratulation…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, …
```

```
## Rows: 124
## Columns: 4
## $ author    <fct> Etini Akpayang, Etini Akpayang, Etini Akpayang, Etini Akpaya…
## $ word      <fct> amazed, amazing, awe, beautiful, best, better, bonus, bright…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, …
```

```
## Rows: 20
## Columns: 4
## $ author    <fct> Diana, Diana, Diana, Diana, Diana, Diana, Diana, Diana, Dian…
## $ word      <fct> better, clean, hell, killed, like, ordeal, refused, stress, …
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 1, 1, 0, 1, 1, 1, 0, 5, 1, 1, 0, 0, 4, 0, 0, 0, 1, 0
```

```
## Rows: 18
## Columns: 4
## $ author    <fct> Lady Diana, Lady Diana, Lady Diana, Lady Diana, Lady Diana, …
## $ word      <fct> appreciated, enough, fallen, good, love, nice, problem, reco…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1
```

```
## Rows: 656
## Columns: 4
## $ author    <fct> dad, dad, dad, dad, dad, dad, dad, dad, dad, dad, dad, dad, …
## $ word      <fct> abnormal, accessible, admire, admonish, advantage, aggressiv…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 2, 0, 0, 2, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 2, 1, 0, 1, 0, …
```

```
## Rows: 8
## Columns: 4
## $ author    <fct> Dad, Dad, Dad, Dad, Dad, Dad, Dad, Dad
## $ word      <fct> easy, nonsense, popular, welcome, easy, nonsense, popular, w…
## $ sentiment <fct> negative, negative, negative, negative, positive, positive, …
## $ Freq      <int> 0, 1, 0, 0, 1, 0, 1, 1
```

```
## Rows: 22
## Columns: 4
## $ author    <fct> Mrs Akpayang Mum, Mrs Akpayang Mum, Mrs Akpayang Mum, Mrs Ak…
## $ word      <fct> congratulation, congratulations, fine, fun, funny, good, hap…
## $ sentiment <fct> negative, negative, negative, negative, negative, negative, …
## $ Freq      <int> 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, …
```


Having obtained all the necessary data set needed, we proceed to plotting the sentiment word cloud 

General sentiment

![Word cloud showing most frequently used sentimental words in the group chat by all users](sentiments_files/figure-html/word cloud for most sentiment word1-1.png)

Nsikan sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Nsikan](sentiments_files/figure-html/word cloud for most sentiment word2-1.png)


Mfonakem sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Mfonakem](sentiments_files/figure-html/word cloud for most sentiment word3-1.png)


Etini sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Nsikan](sentiments_files/figure-html/word cloud for most sentiment word4-1.png)


Diana line 1 sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Diana line 1](sentiments_files/figure-html/word cloud for most sentiment word5-1.png)


Diana line 2

![Word cloud showing most frequently used sentimental words in the group chat by Diana line 2](sentiments_files/figure-html/word cloud for most sentiment word6-1.png)

Dad line 1 sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Dad line 1](sentiments_files/figure-html/word cloud for most sentiment word7-1.png)

Dad line 2 sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Dad line 2](sentiments_files/figure-html/word cloud for most sentiment word8-1.png)

Mum sentiment

![Word cloud showing most frequently used sentimental words in the group chat by Mum](sentiments_files/figure-html/word cloud for most sentiment word9-1.png)
