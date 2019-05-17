
# 量化交易 （需要订制更多高端策略请联系）

## 区间经典

```
```

## 区间Plus

在正常区间基础上引入趋势影响因子

* 趋势影响因子 (-2,+2)

###  攀附型入货

```SAO
Helper:@('Helper') // import Helper from Sanbox Caller

@ST = {

	,FSM: () => Helper.fsm(/* this as fsm logic: */ @@, `
		LoopStart.OK => JudgeTrend
		JudgeTrend.Up => AlgoUp // Tie same as Up
			.Dn => AlgoDn
		AlgoUp.OK => LoopEnd
			.Act1 => ActionAlgoUp
			.Act2 => ActionAlgoUp
		ActionAlgoUp.OK => LoopEnd
		LoopEnd.OK => LoopStart
	 `)

	,ActionAlgoUp(){
		Helper.ClearCache('AlgoUpFlag*')
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
}//ST

```

## 正股与期权联动

e.g. JD
```SAO
@ { LimitAlgoLevelDn=-1, AlgoCallPosRatioLimit=5, AlgoPosCallRatioLimit=2 } = Helper.Config;
@ FlagToDoBuy = true, FlagToDoSell = true,
//正股 1_PCT_T
//@? (AlgoLevel < LimitAlgoLevelDn || AlgoLevel > LimitAlgoLevelUp) @~ //如果超过层级上下限，跳过正股
//@? (AlgoPos < AlgoCallPos / AlgoCallPos/AlgoCallPosRatioLimit ) FlagToDoBuy = false; //小于比例跳过渣
//(-AlgoPos > AlgoCallPos / AlgoCallPos/AlgoPosCallRatioLimit) FlagToDoSell = false //跳过沽
//聚合（下周、下下周、下下下周 接近1且未开仓，已开仓）=> 根据 PosLevel做 5_PCT_T, 但超过10张不做渣，小于3张不做沽
@~ Helper.POK()
```
