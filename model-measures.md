## Risk-measures overview

The four main risk-measures which can be computed from our model are as follows:

### Expected Volatility (Vol)

The expected volatility of the portfolio is a measure for the risk of a portfolio. In particular, it is the square root of the expected variance of the portfolio, which in turn is an aggregate of the asset variances taking into account the correlations between the portfolio constituents. Note that the variance measures how far a set of random numbers, i.e. the asset returns, are spread out from their mean.
Therefore, assets with high volatility (and hence high variance) are expected to exhibit a broader range of return values around their mean. On the other side, assets with small volatility, are expected to exhibit a smaller range of return values around their mean.

Therefore, assets with large volatility (and hence large variance) are expected to exhibit a wider range of return values around their mean, whereas assets with small volatility are expected to exhibit a smaller range of return values around their mean.

### Value-at-Risk (VaR)

The Value-at-Risk (VaR) is a powerful risk-measure that can help quantify portfolio risks by assigning losses to given probabilities. Formally, the VaR at level α is defined as the highest number y such that the probability that the portfolio returns Y fall below y is lower than α, i.e. α = P[Y < VaR(α)]. Hence, the VaR(α) gives us the estimated loss size for the given probability α, which is usually set to 1% or 0.1%.

For example, a daily Value-at-risk of -5.0% at the 1% level means that we expect the portfolio value to fall below -5.0% in 1 out of 100 cases. Alternatively, in other words, around 2.5x per year when assuming 250 trading days.

### Expected Shortfall (ES)

The Expected Shortfall (ES) quantifies risk by computing the average expected loss in case of extreme market events. Formally, the ES at level α is the conditional tail expectation of the portfolio returns falling below the Value-at-Risk at level α (i.e. ES(α) = E[Y|Y < VaR(α)]). The ES is related to the VaR and gives an estimate of the size of the loss in case daily returns fall below the VaR.

Let us assume the VaR is still 5%. Then an Expected Shortfall estimate of, e.g. -10.0% at the 1% level means that we can expect the average loss to be around 10% in the case the loss exceeds 5% as previously given by the VaR.

To wrap it up: VaR versus ES:
- Given a VaR of 5% at the 1% level we can expect our portfolio to lose more than 5% on any single day in 1 out of 100 cases.
- Given an ES of 10% at the 1% level, we can expect these losses to be around 10% on average.


### Risk-Contributions (RC)

The Risk Contribution (RC) of each asset measures how risky any single portfolio constituent is relative to the total risk of the portfolio, that is, the expected volatility of the portfolio. The RC is given as the normalized change in portfolio risk (Vol) brought forth by further positional changes in the given asset.

Note that due to the normalization, the sum of risk contributions always equals one and individual risk-contribution can only be compared relative to other assets. For example, a risk contribution of 0.10 for asset A and 0.20 for asset B means that asset B is twice as risky as asset A.
