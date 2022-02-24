---
author: Yeung Ka Ming, CFA
date: 2022-02-24
excerpt: Technical Analysis
knit: "(function(inputFile, encoding) { rmarkdown::render(inputFile,
  encoding = encoding, output_dir = “../\\_posts”) })"
layout: post
output:
  md_document:
    preserve_yaml: true
    variant: gfm
tags: BollingerBands
title: Bollinger Bands
---

# Summary

Bollinger Bands

## R Libraries

``` r
library(quantmod)
library(TTR)
```

## Preparation of data

``` r
tickers <- c("^HSI","^GSPC")
getSymbols(Symbols = tickers,
           src = "yahoo",
           index.class = "POSIXct",
           from = "1997-01-01")
```

    ## [1] "^HSI"  "^GSPC"

## Prepare technical analysis of Bollinger Bands

``` r
bbands.HLC <- BBands(na.omit(HSI[,c("HSI.High","HSI.Low","HSI.Close")]))
```

## Plot

``` r
plot(bbands.HLC)
```

![](/images/BBands%20plot-1.png)<!-- -->

``` r
plot(bbands.HLC["2020/2022"])
```

![](/images/BBands%20plot-2.png)<!-- -->

``` r
plot(bbands.HLC["2020/2022"][,1:3])
```

![](/images/BBands%20plot-3.png)<!-- -->

``` r
plot(bbands.HLC["2020/2022"][,4])
```

![](/images/BBands%20plot-4.png)<!-- -->
