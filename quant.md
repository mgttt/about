
# 量化交易随笔

## 区间经典

## 区间Plus

在正常区间基础上引入趋势影响因子

* 趋势影响因子 (-2,+2)

###  攀附型入货

```SAO

@(Helper) // merge this

,TrendTradePlus(){
  return Helper.tinyfsm(`
    LoopStart.OK => JudgeTrend
    JudgeTrend.Up => AlgoUp // Tie same as Up
      .Dn => AlgoDn
    AlgoUp.Act => ActionAlgo
      .OK => LoopEnd
    ActionAlgo.OK => LoopEnd
    LoopEnd.OK => LoopStart
   `);
}

,AlgoUp(){
  @ Config = Helper.Config;
  @ { TrendPlusUpDnValue=0, TrendPlusUpDnBkValue=0, TrendPlusUpDnRate=0, TrendPlusUpDnBkRate=0 } = Config;
  @ PriceLast = Helper.HasLastTrade ? Helper.PriceLastTrade : Helper.PriceStartToday;
  @ PriceBase = Helper.Max( PriceLast, Helper.PriceMaxToday );
  @ TrendPlusUpDn = PriceBase * (1 - TrendPlusUpDnRate)  - (TrendPlusUpDnValue);
  @ MarketPrice = Helper.MarketPrice;
  @? (! ( MarketPrice <= TrendPlusUpDn ) ) {
    Helper.AlgoUpFlag1 = false;
    Helper.AlgoUpFlag1Price = null;
    @~( {STS:'OK'} )
  }
  Helper.AlgoUpFlag1 = true;
  if(!Helper.AlgoUpFlag1Price) Helper.AlgoUpFlag1Price = MarketPrice;
  @? ( TrendPlusUpDnRate>0 || TrendPlusUpDnBkRate ) {
    @? (Helper.AlgoUpFlag1){
      //test flag2
      @ TrendPlusUpDnBack = Helper.AlgoUpFlag1Price * (1 + TrendPlusUpDnBkRate) + TrendPlusUpDnBkValue;
      @? (MarketPrice >= TrendPlusUpDnBack) {
        @~( {STS:'Act'} )
      }
    }
  } else {
    //just flag1
    @~( {STS:Act} )
  }
  @~( {STS:'OK'} )
}

```
