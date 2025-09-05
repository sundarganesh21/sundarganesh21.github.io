---
layout: none
---

The Jupyter notebook for this repository can be found [here](https://github.com/sundarganesh21/quantfin/tree/main/project1_fx_mean_reversion).
# FX Trading, Mean Reversion and Moving Averages

In this mini-project, we look into mean-reversion algorithms for FX trading. For some curency pairs whose countries have close economic and geographical ties (GBP-EUR, AUS-NZD), their exchange rate can be mean-reverting, i.e., short term fluctuations usually trend back towards a mean value. When an exchange rate displays such mean-reverting behaviour, a simple trading-strategy can be proposed where we buy/sell currency when there are large deviations of the exchange rate from its mean value, since we expect that it trends back to its mean value. 

## Research Question and Analysis

### Initial Analysis
The first question we aim to answer is the following - is the signal mean-reverting over any particular time window? If yes, what is the half-life of its mean-reversion? We want to know not just when/whether the signal reverts, but also how quickly it reverts. We work primarily with the EUR-GBP exchange rate in this analysis. We first want to test this on historical data, so let's pull up the last 20 years of EUR-GBP data at a frequency of 1 day. 

![Historical EUR-GBP Exchange Rate](../assets/images/project1/exchange_rate_all_time.png)

Just visually, we observe some mean-reversion from 2016 until around 2020-2021. However, we'd like to test this more concretely.

### ADF Test for Mean-Reversion and Half-Life Extraction
Starting from 2005, we look at 3-year long windows, moving the window forwards by one quarter each time. For each snapshot, we run the Augmented Dickey-Fuller test. This confirms what we observed in the eyeball-norm, namely that from early 2016 to 2019, the ADF test predicts that the process is indeed mean reverting (see Jupyter notebook link above).

Now that we know the signal is mean-reverting, we wish to know how quickly it mean reverts. To this end, we fit an AR1 model on the time signal from 2016 to 2019. From the AR1 model coefficient, the half-life comes out to be 51 days. This half-life is very important for mean-reversion trading algorithms, as we'll see in the next section.

## Trading Algorithm
### Moving averages
To use this signal for trading, we deploy a moving-average algorithm. Although the ADF test tells us that the signal reverts to a static mean value, this mean value may in practice change over time, albeit more slowly than the signal itself. To accommodate for this, we use a moving-average window rather than a fixed average value. 

The rationale behind the algorithm is quite straightforward. If the exchange rate is abnormally high, then GBP is doing "too" well against EUR. We buy GBP/sell EUR, since a reversion to the mean is about to happen. Once the exchange rate returns to its mean value, we can then sell the GBP that we bought when the exchange rate was high, exiting the trade and realizing our profits. Vice versa for when the exchange rate drops to an abnormally low level. 

We then need to decide two aspects of the algorithm - the 'when' and the 'how much'.

### The 'When'

We use a Z-score to compute how far the exchange rate is from its mean value as follows: Z-score = (exchange rate - moving average )/(moving standard deviation). When the Z-score exceeds a certain enter-value, we initiate the buy or sell. When the Z-score drops below a certain exit-value, we "close" our position. 

An important note here is that with FX, there is no notion of "asset" that you buy/sell using cash. Hence, there is often a necessity to treat one currency as the base currency and the other currency as the "asset" currency. When you hear "exit" in these contexts, it usually means that we buy/sell until we return the balance of our base currency to the value that it was when we entered the position.

For testing, we propose an enter-value of 1.5 and an exit value of 0.5. In practice, these are parameters that can be optimized using hyperparameter optimization.

### The 'How Much'
Right, so we've defined when to enter and exit the position. The next question is - how much do we trade each time we get a signal to trade? There are multiple ways to do this. The first and most simple way is to trade the same fixed amount whenever we get an enter or exit signal. The second way is to trade an amount proportional to the Z-score. This way, the larger the departure is from the average value, the more you are willing to stake on the trade. We'll stick to the first algorithm in this project.

## Backtesting and Performance Analysis
