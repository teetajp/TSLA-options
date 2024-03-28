# TSLA-options


Group Members: Ronje Roy, TJ Pavaritpong, Ben Zhao

For this project, we are utilizing our knowledge of options theory to profit from volatility mispricing on Tesla options. 

Initially, our objectives were the following:

1. Conduct in-depth research on Tesla stocks and options
2. Explore and backtest volatility arbitrage strategies to identify straddle opportunities
3. Develop ML models to forecast vol term structure
4. Ensure proper delta hedging through backtesting, ideally through automated processes


To accomplish this, our first step was to learn more about options theory. Throughout the semester, we are reading through Sheldon Natenberg's "Option Volatility and Pricing" Book to gain a strong foundation in theory. 
Aspects we looked into included gaining a foundational understanding of risk management measures (Options Greeks), delta hedging, different options combinations (particularly ATM straddles), and other volatility arbitrage strategies. 

For our project, we then needed to source volatility data. The first step was to access data regarding implied volatility for future Tesla options, which we were able to do through Bloomberg Terminal in the Margolis Market Information Lab. 
We now plan to forecast future volatility though machine learning so that we can take advantage of mispriced options based on IV. 

Eventually, we will apply the option combinations we deem optimal to best profit from differences between implied volatility and future realized volatility. 
We will also delta hedge in order to minimize variance in our profits, and backtest our different strategies on prior data to gain a sense for how successful we would be within a larger portfolio. 

**Data Sources**
https://www.kaggle.com/datasets/kylegraupe/tsla-daily-eod-options-quotes-2019-2022/data

   
