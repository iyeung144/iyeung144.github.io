# Summary

Bollinger Bands

## R Libraries

    library(quantmod)
    library(TTR)

## Preparation of data

    tickers <- c("^HSI","^GSPC")
    getSymbols(Symbols = tickers,
               src = "yahoo",
               index.class = "POSIXct",
               from = "1997-01-01")

    ## [1] "^HSI"  "^GSPC"

## Prepare technical analysis of Bollinger Bands

    bbands.HLC <- BBands(na.omit(HSI[,c("HSI.High","HSI.Low","HSI.Close")]))

## Plot

    plot(bbands.HLC)

![](D:\OneDrive%20-%20The%20Chinese%20University%20of%20Hong%20Kong\GitHub_posts\2022-02-24_BollingerBands_files/figure-markdown_strict/BBands%20plot-1.png)

    plot(bbands.HLC["2020/2022"])

![](D:\OneDrive%20-%20The%20Chinese%20University%20of%20Hong%20Kong\GitHub_posts\2022-02-24_BollingerBands_files/figure-markdown_strict/BBands%20plot-2.png)

    plot(bbands.HLC["2020/2022"][,1:3])

![](D:\OneDrive%20-%20The%20Chinese%20University%20of%20Hong%20Kong\GitHub_posts\2022-02-24_BollingerBands_files/figure-markdown_strict/BBands%20plot-3.png)

    plot(bbands.HLC["2020/2022"][,4])

![](D:\OneDrive%20-%20The%20Chinese%20University%20of%20Hong%20Kong\GitHub_posts\2022-02-24_BollingerBands_files/figure-markdown_strict/BBands%20plot-4.png)
