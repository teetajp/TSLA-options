# region imports
from AlgorithmImports import *
import torch
# endregion

class USEquityOptionsDataAlgorithm(QCAlgorithm):

    def initialize(self) -> None:
        self.set_start_date(2023, 1, 1)
        self.set_end_date(2023, 7, 1)
        self.set_cash(100000)
        # Requesting data
        self.underlying = self.add_equity("TSLA").symbol
        option = self.add_option("TSLA")
        self.option_symbol = option.symbol
        # Set our strike/expiry filter for this option chain
        option.set_filter(-10, 10, 0, 90)
        self.contract = None
        # #self.Schedule.On(self.DateRules.EveryDay("SPY"),
        #                  self.TimeRules.At(10, 0),
        #                  self.TradeOptions)
        self.underlying.volatility_model = StandardDeviationOfReturnsVolatilityModel(30)
        
        self.lookbackSTD = 30  
        self.lookbackSMA = 5   
        self.SetWarmup(self.lookbackSTD + self.lookbackSMA, Resolution.Daily)
        self.logr = self.LOGR(self.underlying, 1)
        self.std = IndicatorExtensions.Of(StandardDeviation(self.lookbackSTD), self.logr)
        self.stdAvg = IndicatorExtensions.SMA(self.std, self.lookbackSMA)

    
    def on_data(self, slice: Slice) -> None:
        if self.Portfolio.cash <= 0:
            return
        chain = slice.OptionChains.get(self.option_symbol, None)
        if not chain:
            return
        
        # Find ATM options with the nearest expiry
        expiry = min([x.Expiry for x in chain])
        contracts = sorted([x for x in chain if x.Expiry == expiry], key=lambda x: abs(chain.Underlying.Price - x.Strike))
        if float(self.std.Current.Value*np.sqrt(252)) - contracts[0].ImpliedVolatility < 0.05:
            self.log(float(self.std.Current.Value*np.sqrt(252)))
            self.log(contracts[0].ImpliedVolatility)
            self.log(f'Bought straddle at {self.underlying.volatility_model.volatility - contracts[0].ImpliedVolatility}')
            long_straddle = OptionStrategies.straddle(self.option_symbol, contracts[0].strike, expiry)
            self.buy(long_straddle, 1)
    
    # def on_data(self, slice: Slice) -> None:
    #     if self.portfolio[self.underlying].invested:
    #         self.liquidate(self.underlying)

    #     if self.contract is not None and self.portfolio[self.contract.symbol].invested:
    #         return

    #     chain = slice.option_chains.get(self.option_symbol)
    #     if chain:
    #         # Select call contracts
    #         calls = [contract for contract in chain if contract.right == OptionRight.CALL]
    #         if len(calls) == 0:
    #             return
            
    #         # Select the call contracts with the furthest expiration
    #         furthest_expiry = sorted(calls, key = lambda x: x.expiry, reverse=True)[0].expiry
    #         furthest_expiry_calls = [contract for contract in calls if contract.expiry == furthest_expiry]
            
    #         # From the remaining contracts, select the one with its strike closest to the underlying price
    #         self.contract = sorted(furthest_expiry_calls, key = lambda x: abs(chain.underlying.price - x.strike))[0]
    #         self.market_order(self.contract.symbol, 1)
                
                
    # def on_securities_changed(self, changes: SecurityChanges) -> None:
        
    #     for security in changes.added_securities:
    #         # Historical data
    #         history = self.history(security.symbol, 10, Resolution.MINUTE)
    #         self.debug(f"We got {len(history)} from our history request for {security.symbol}")
