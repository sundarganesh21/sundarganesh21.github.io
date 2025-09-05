---
layout: none
---
# FX Trading and Mean Reversion

In this mini-project, we look into mean-reversion algorithms for FX trading. For some curency pairs whose countries have close economic and geographical ties (GBP-EUR, AUS-NZD), their exchange rate can be mean-reverting, i.e., short term fluctuations usually trend back towards a mean value. When an exchange rate displays such mean-reverting behaviour, a simple trading-strategy can be proposed where we buy/sell currency when there are large deviations of the exchange rate from its mean value, since we expect that it trends back to its mean value. 

## Research Question
The first question we aim to answer is the following - is the signal mean-reverting over any particular time window? If yes, what is the half-life of its mean-reversion? We want to know not just when/whether the signal reverts, but also how quickly it reverts. We work primarily with the EUR-GBP exchange rate in this analysis. We first want to test this on historical data, so let's pull up the last 20 years of EUR-GBP data at a frequency of 1 day. 
