---
layout: none
---

The Jupyter notebook for this repository can be found [here](https://github.com/sundarganesh21/quantfin/tree/main/project1_fx_mean_reversion).
# FX Trading, Mean Reversion and Moving Averages

In this mini-project, we look into mean-reversion algorithms for FX trading. For some curency pairs whose countries have close economic and geographical ties (GBP-EUR, AUS-NZD), their exchange rate can be mean-reverting, i.e., short term fluctuations usually trend back towards a mean value. When an exchange rate displays such mean-reverting behaviour, a simple trading-strategy can be proposed where we buy/sell currency when there are large deviations of the exchange rate from its mean value, since we expect that it trends back to its mean value. 

## Research Question
The first question we aim to answer is the following - is the signal mean-reverting over any particular time window? If yes, what is the half-life of its mean-reversion? We want to know not just when/whether the signal reverts, but also how quickly it reverts. We work primarily with the EUR-GBP exchange rate in this analysis. We first want to test this on historical data, so let's pull up the last 20 years of EUR-GBP data at a frequency of 1 day. 

![Historical EUR-GBP Exchange Rate](../assets/images/project1/exchange_rate_all_time.png)

Just visually, we observe some mean-reversion from 2016 until around 2020-2021. However, we'd like to test this more concretely.

## ADF test for Mean-Reversion and Half-Life Extraction
Starting from 2005, we look at 3-year long windows, moving the window forwards by one quarter each time. For each snapshot, we run the Augmented Dickey-Fuller test. This confirms what we observed in the eyeball-norm, namely that from early 2016 to 2019, the ADF test predicts that the process is indeed mean reverting (see Jupyter notebook link above).

Now that we know the signal is mean-reverting, we wish to know how quickly it mean reverts. To this end, we fit an AR1 model on the time signal from 2016 to 2019. From the AR1 model coefficient, the half-life comes out to be 51 days. This half-life is very important for mean-reversion trading algorithms, as we'll see in the next section.

## Moving Average Algorithm
