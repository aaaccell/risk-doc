## Data cleaning

For numerical reasons inherent to some the risk engine models and to ensure the best possible accuracy of our risk-measures we require multiple years of historical asset prices for each risk estimate that we compute. While our data provider takes care of economic adjustments like dividend payments and stock splits, it remains for us to fill and/or generate missing values where they simply do not exist.

The following sections contain an overview our data preprocessing methodologies.


### Conditional interpolation and mapping

From empirical tests using synthetic data with different number of missing values and various degrees of correltion between risk-factors and target asset, we deduced the following heuristic to conditionally switch between interpolation and the more complex and compute-intensive mapping approach

1. Assess the number of missing values of the target asset
2. If we have more than 50% missing values:
    1. Get asset metadata (i.e. asset-type, country, and sector) of target asset
    2. Create risk-factor mapping table by selecting a suitable subsection of the mapping universe based on this metadata (see below for methodology)
    3. If the average correlation between target risk-factor and mapping table is larger than 0.5
        * Apply the risk-factor mapping methodology outlined below and be done
    4. Else:
        * Use the interpolation-based approach (outlined in the next section)
3. Else:
    * Use the interpolation-based approach (outlined in the next section)


The following table contains the detailed results of the empirical tests:

| Avg. corr | i10% | m10% | i25% | m25% | i50% | m50% |
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| 0.0 | 0.1526 | 1.1400 | 0.4100 | 1.1400 | 0.7719 | 1.1400 |
| 0.1 | 0.1286 | 0.8903 | 0.3896 | 0.8903 | 0.6387 | 0.8903 |
|  0.2 | 0.1906 | 0.9732 | 0.3005 | 0.9732 | 0.6253 | 0.9732 |
| 0.3 | 0.1557 | 0.8013 | 0.3691 | 0.8013 | 0.6272 | 0.8013 |
| 0.4 | 0.1410 | 0.6490 | 0.4205 | 0.6490 | 0.6898 | 0.6490 |
| 0.5 | 0.1445 | 0.5389 | 0.3651 | 0.5382 | 0.7301 | 0.5382 |
| 0.6 | 0.1695 | 0.4588 | 0.3793 | 0.4588 | 0.7205 | 0.4588 |
| 0.7 | 0.2368 | 0.3874 | 0.3244 | 0.3874 | 0.6242 | 0.3874 |
| 0.8 | 0.1566 | 0.2230 | 0.3280 | 0.2230 | 0.7690 | 0.2230 |
| 0.9 | 0.1734 | 0.1127 | 0.4629 | 0.1127 | 0.6714 | 0.1127 |
| Threshold |  | **0.9** |  | **0.8** |  | **0.4** |

where we used the following methodology: 
* Construct three M=10 zero-drift synthetic data sets of [(N+1)xK] price time series with varying correlation from 0.0 to 0.9 in steps of 0.1, with N=1000 and K=10, where the M (N+1)'th price time series will be used as regressands in the current and proposed risk-factor mapping framework with 10%, 25% and 50% missing values. 
* Norm the regressand series to 1% standard deviation of the initial price level
* Compute the scaled (1e+06) Mean-Squared Error (MSE) between actual values and either interpolated (i) or risk-factor mapped (m) values
* Define threshold as correlation level where risk-factor mapping leads to lower MSE than interpolation


### Interpolation methodolgy

@Marc, fill in details here!


### Risk-factor mapping methodolgy

Given a data set consisting of [Nx1] target asset prices (including missing values) and [NxK] risk-factor values, we apply the following approach to regress the risk-factor values on the target asset:

1. Split price data set into [Nx1] regressand and [NxK] regressors. We use a fixed set of risk-factors as regressors. 
2. Extract M available/valid price data points from regressand and repack into contiguous [Mx1] array
3. Extract price data points from regressors having the same index as the just extracted regressand price data points into a [MxK] matrix 
4. Turn regressand and regressor price data sets into [(M-1)x1] and [(M-1)xK] regressand and regressor return data sets. Note that we can have mixed, i.e. not necessary daily frequency returns, in this data set. 
5. Apply constrained Ordinary Least-Squares regression the return data sets from the previous step to get a [(K+1)x1] vector of coefficients that sum to one.
6. Turn initial [NxK] regressor price data points into [NxK] regressor return data points, where the first return is assumed to be zero
7. Estimate [(N-M)x1] missing/invalid return data points using [(N-M)xK] return data points from the previous step
8. Starting at the last valid regressand price observation, forward and backward fill missing/invalid prices using the estimated returns from the previous step.

Note, that this algorithm requires that the regressor price data set does not have any missing/invalid data points at all. 


### Risk-factor universe generation

We are using the following data-driven algorithm to generate (or update) the risk-factor universe matching that best represents our current asset data base:

1. Use hierarchical tree clustering on all assets in the database that have valid prices for the most recent window period from the time of running the risk-factor universe generation algorithm. For example, we find that 5'000 assets in our database have valid prices over the last five years.
2. Re-arrange the clustered correlations such that we get a quasi-diagonal correlation matrix for theses assets
3. Pre-definig the total size of the risk-factor universe one can now use a fixed step size and sample ISINs as risk-factors that are well diversified and representative of the frequency of occurence of assets. For example, from the 5'000 valid assets we sample every 50th asset to get a total 100 risk factors.


### Risk-factor table selection

Given the metadata each asset carries, we can generate suitable risk-factor mapping tables on-the-fly by selecting suitable risk-factors from the universe generated earlier based on target asset metadata characteristics. For example, if we want to generate historical prices for a newly issued CHF denominated bond of a consumer-staples company, we would select Swiss fixed-income and consumer staples related risk-factors.

