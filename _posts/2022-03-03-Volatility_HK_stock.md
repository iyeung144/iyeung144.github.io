---
author: Yeung Ka Ming, CFA
date: 2022-03-03
excerpt: Volatility
knit: "(function(inputFile, encoding) { rmarkdown::render(inputFile,
  encoding = encoding, output_dir = “../\\_posts”) })"
layout: post
output:
  md_document:
    preserve_yaml: true
    variant: gfm
tags: StandardDeviation
title: Volatility of Hong Kong Stock
---

# Summary

Volatility of Hong Kong Stock - 0388.HK

## R Libraries

``` r
library(tidyverse)
library(quantmod)
library(ggplot2)
```

## Preparation of data

Direct download by getSymbols

``` r
tickers <- c("0388.HK")
HKEX <- getSymbols(Symbols = tickers,
           src = "yahoo",
           index.class = "POSIXct",
           from = "1997-01-01",
           auto.assign = FALSE)
# auto.assign is FALSE, so a variable is used here. Unlike US stocks,
# the tickers in HK stock market is by number, which cannot be the variable
# name in R.
```

## Plot data

``` r
# HKEX
HKEX.new <- na.omit(HKEX)
colnames(HKEX.new) <- c("HKEX.Open", "HKEX.High", "HKEX.Low", "HKEX.Close", "HKEX.Volume","HKEX.Adjusted")
HKEX.new <- merge(HKEX.new, dailyReturn(HKEX.new$HKEX.Adjusted))
colnames(HKEX.new)[7] <- "HKEX.adj.ret"
HKEX.new <- merge(HKEX.new,rollapply(HKEX.new$HKEX.adj.ret, 252, sd))
colnames(HKEX.new)[8] <- "HKEX.adj.std"
HKEX.new <- merge(HKEX.new, HKEX.new$HKEX.adj.std*sqrt(252))
colnames(HKEX.new)[9] <- "HKEX.adj.std.annualised"
HKEX.new.tbl <- as_tibble(fortify(HKEX.new))
p <- ggplot(HKEX.new.tbl, aes(x=Index,y=HKEX.adj.std.annualised))
p+geom_line()
```

![](/images/HK0388-plot-1.png)<!-- -->

``` r
avg10yr <- mean(HKEX.new$HKEX.adj.std.annualised["2011/2022"])
avg15yr <- mean(HKEX.new$HKEX.adj.std.annualised["2006/2022"])
avg20yr <- mean(HKEX.new$HKEX.adj.std.annualised["2001/2022"])
avg25yr <- mean(na.omit(HKEX.new$HKEX.adj.std.annualised["1997/2022"]))
p+geom_line()+
  geom_hline(yintercept = avg10yr, color = "red")+
  geom_hline(yintercept = avg15yr, color = "blue")+
  geom_hline(yintercept = avg20yr, color = "green")+
  geom_hline(yintercept = avg25yr, color = "black")
```

![](/images/HK0388-plot-2.png)<!-- -->

``` r
HKEX.new2.tbl <- HKEX.new.tbl %>% 
  pivot_longer(cols = c("HKEX.Adjusted","HKEX.adj.std.annualised"), 
               names_to ="Series", 
               values_to = "Values") %>% 
  select(c(Index,Series,Values))

p1 <- ggplot(HKEX.new2.tbl, aes(x=Index,y=Values))
p1 + geom_line() +
  facet_grid(Series ~ ., scales = "free") +
  scale_x_datetime(name = "Date", date_breaks = "1 year", date_labels = "%Y")
```

![](/images/HK0388-plot-3.png)<!-- -->
