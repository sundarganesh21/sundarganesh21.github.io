---
layout: none
---

The Jupyter notebook for this project can be found [here](https://github.com/sundarganesh21/quantfin/tree/main/project2_fx_mean_reversion_part2).

# Project 2: Mean Reversion Algorithms - Part 2: Improvements

## 1. Research Question
Now that we have a basic strategy, let's explore some improvements to this strategy. In this article, we mainly want to explore the following improvements:
- Using exponential weighted averages rather than moving averages
- Changing the strategy to trade proportional to the Z-score, rather than using a constant amount throughout

## 2. First improvement: Weighted moving average
The simple moving average that we've used in Part 1 used equal weights for all points in the 51-day moving average window. Instead, we propose using an exponential weighted average, where the weights decay geometrically by a factor $0 < \alpha < 1$:

$$\begin{align}
\mu_{i} &= \frac{1}{W} \left[ x_i + \alpha x_{i-1} + \alpha^2 x_{i-2} + ... + \alpha^n x_{i-n} \right] \\
\text{where } W &\coloneqq \sum_{j=0}^n \alpha^j = \frac{(1-\alpha^{n+1})}{(1-\alpha)}
\end{align}$$

It's important that for any moving average, the weights must sum up to 1 in order to maintain the unbiased nature of the estimator; hence the leading $1/W$ factor. Let's compute the exponential moving average and compare it to the simple moving average.
