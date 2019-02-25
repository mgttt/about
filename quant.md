
# 量化交易

## 区间经典

```
```

## 区间Plus

在正常区间基础上引入趋势影响因子

* 趋势影响因子 (-2,+2)

###  攀附型入货

```SAO

@(Helper) // NOTE: merge "this"

,TrendTradePlus(){
  return Helper.fsm(`
    LoopStart.OK => JudgeTrend
    JudgeTrend.Up => AlgoUp // Tie same as Up
      .Dn => AlgoDn
    AlgoUp.OK => LoopEnd
      .Act1 => ActionAlgoUp
      .Act2 => ActionAlgoUp
    ActionAlgoUp.OK => LoopEnd
    LoopEnd.OK => LoopStart
   `);
}

,ActionAlgoUp(){
  Helper.ClearCache('AlgoUpFlag*') //IMPORTANT
  @~ Helper.QOK()
}

,AlgoUp(){
  @ Config = Helper.Config;
  @ { TrendPlusUpDnValue=0, TrendPlusUpDnBkValue=0, TrendPlusUpDnRate=0, TrendPlusUpDnBkRate=0 } = Config;
  @ PriceLast = Helper.HasLastTrade ? Helper.PriceLastTrade : Helper.PriceStartToday;
  @ PriceBase = Helper.Max( PriceLast, Helper.PriceMaxToday );
  @ TrendPlusUpDn = PriceBase * (1 - TrendPlusUpDnRate)  - TrendPlusUpDnValue;
  @ MarketPrice = Helper.MarketPrice;
  @? ( MarketPrice <= TrendPlusUpDn ) {
    Helper.AlgoUpFlag1 = true;
    Helper.AlgoUpFlag1Price = MarketPrice;
  }
  @ STS = 'OK';
  @? ( Helper.AlgoUpFlag1 ){
    @? ( TrendPlusUpDnRate >0 || TrendPlusUpDnBkRate >0 ) {
      @ TrendPlusUpDnBack = Helper.AlgoUpFlag1Price * (1 + TrendPlusUpDnBkRate) + TrendPlusUpDnBkValue;
      @? (MarketPrice >= TrendPlusUpDnBack) {
        Helper.AlgoUpFlag2 = true;
        Helper.AlgoUpFlag2Price = MarketPrice;
        STS = 'Act2';
      }
    } @: {
      STS = 'Act1';
    }
  }
  @~ Helper.QSTS(STS)
}

```
