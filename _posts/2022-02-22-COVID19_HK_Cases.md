---
author: Yeung Ka Ming, CFA
date: 2022-02-22
excerpt: COVID-19
knit: "(function(inputFile, encoding) { rmarkdown::render(inputFile,
  encoding = encoding, output_dir = “../\\_posts”) })"
layout: post
output:
  md_document:
    preserve_yaml: true
    variant: gfm
tags: histogram
title: COVID-19 Confirmed Cases in Hong Kong
---

# Summary

COVID-19 confirmed cases by age and gender in Hong Kong

## R Libraries

``` r
library(ggplot2)
library(tidyverse)
```

## Preparation of data

Download data from WHO website

``` r
enhanced_sur_covid_19_eng <- read.csv("d:/Users/ivan/Downloads/enhanced_sur_covid_19_eng.csv")
```

``` r
hkcovid.tbl <- as_tibble(enhanced_sur_covid_19_eng)

hkcovid1.tbl <- hkcovid.tbl %>% 
  mutate(Report.date = as.Date(Report.date, format = "%d/%m/%Y"), 
         Date.of.onset = as.Date(Date.of.onset, format = "%d/%m/%Y"))
```

## Simple plot date vs confirmed cases

``` r
hkcovid1.tbl %>% 
  group_by(Report.date) %>% 
  summarise(n = n()) %>% 
  plot(type = "l")
```

![](/images/simple%20plot%20date%20vs%20confirmed%20cases-1.png)<!-- -->

## date vs gender

``` r
hkcovid2.tbl <- hkcovid1.tbl %>%
  group_by(Report.date, Gender) %>%
  summarise(n = n())
p <- ggplot(hkcovid2.tbl, aes(x=Report.date, y=n, color=Gender))
p+geom_line()
```

![](/images/ggplot%20date%20vs%20gender-1.png)<!-- -->

``` r
hkcovid2.tbl <- hkcovid1.tbl %>%
  group_by(Report.date, Gender) %>% 
  filter(Gender == 'M' | Gender == 'F') %>% 
  summarise(n = n())
p1 <- ggplot(hkcovid2.tbl, aes(x=Report.date, y=n, color=Gender))
p1+geom_line(aes(linetype=Gender))+scale_size_manual(values=c(2, 2.5))
```

![](/images/ggplot%20date%20vs%20gender-2.png)<!-- -->

## histogram

``` r
hkcovid3.tbl <- hkcovid1.tbl %>% 
  group_by(Age,Gender) %>% 
  filter(Gender == 'M' | Gender == 'F')
hkcovid4.tbl <- hkcovid3.tbl %>% 
  mutate(Age = as.integer(Age))
p2 <- ggplot(hkcovid4.tbl, 
            aes(x=Age, color=Gender, fill=Gender)) + 
  geom_histogram(binwidth=1, color="Black", position="identity", alpha=0.25)
p2
```

![](/images/histogram-1.png)<!-- -->

``` r
p3 <- ggplot(hkcovid4.tbl,
             aes(x=Age, color=Gender, fill=Gender)) + 
  geom_histogram(binwidth=1, 
                 color="Black", 
                 position="identity", 
                 alpha=0.25,
                 aes(y=..density..)) + 
  geom_density(alpha=.2, fill="#FF6666")
p3
```

![](/images/histogram-2.png)<!-- -->
