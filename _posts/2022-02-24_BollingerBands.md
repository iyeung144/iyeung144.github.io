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

![](/images/BBands%20plot-1.png)

    plot(bbands.HLC["2020/2022"])

![](/images/BBands%20plot-2.png)

    plot(bbands.HLC["2020/2022"][,1:3])

![](/images/BBands%20plot-3.png)

    plot(bbands.HLC["2020/2022"][,4])

![](/images/BBands%20plot-4.png)
