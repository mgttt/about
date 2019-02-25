
# 量化交易随笔

## 区间经典

## 区间Plus

在正常区间基础上引入趋势影响因子

* 趋势影响因子 (-2,+2)

```

@(Helper) // merge this

,TrendTradePlus(){
  LoopStart.OK => JudgeTrend
  JudgeTrend.Up => AlgoUp // Tie same as Up
    .Dn => AlgoDn
  AlgoUp.Act => ActionAlgo
    .OK => LoopEnd
  ActionAlgo.OK => LoopEnd
  LoopEnd.OK => LoopStart
}

,AlgoUp(){
  @ Config = Helper.Config;
  @ { TrendPlusUpDnValue, TrendPlusUpDnBkValue, TrendPlusUpDnRate, TrendPlusUpDnBkRate } = Config;
  @ PriceLast = Helper.HasLastTrade ? Helper.PriceLastTrade : Helper.PriceStartToday;
  @ PriceBase = Helper.Max( PriceLast, Helper.PriceMaxToday );
  @ TrendPlusUpDn = PriceBase - (TrendPlusUpDnValue||0) - PriceBase * (TrendPlusUpDnRate || 0.0);
  @ MarketPrice = Helper.MarketPrice;
  @ flag1 = (MarketPrice >= TrendPlusUpDn);
  @~( flag1 && flag2 )
}


```
