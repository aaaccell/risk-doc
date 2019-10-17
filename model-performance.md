### Model performance <a name="performance"/>

#### Accuracy benchmarking <a name="accuracy"/>

The unique combination of the versatile, heavy-tailed MGL distribution with asymmetric multivariate GARCH processes and the least-squares filter empowers the model exhibit the following key-features

* Accomodate extreme market events
* Dynamically and asymmetrically model risks and returns according to changing market regimes

Subsequently, we achieve more accurate forecasts, and our solution has a clear edge when measuring asset- and portfolio risks vis-a-vis the models commonly employed by the industry. The superiority of our approach is visible in the following table, which shows the Value-at-Risk violations at the 1% confidence level (i.e. percentage average when the market fell below the forecasted Value-at-Risk) for ten random assets from the SP500 over the last 15 years.


| Ticker | Gaussian | Gaussian-GARCH | StudentsT | StudentsT-GARCH | Laplace | Laplace-GARCH |
|--------|----------|----------------|-----------|-----------------|---------|---------------|
| APC    | 0.018    | 0.015          | 0.013     | 0.014           | 0.013   | 0.012         |
| AMZ    | 0.014    | 0.019          | 0.012     | 0.011           | 0.013   | 0.011         |
| BRYN   | 0.019    | 0.016          | 0.013     | 0.013           | 0.013   | 0.013         |
| JNJ    | 0.028    | 0.019          | 0.023     | 0.014           | 0.024   | 0.014         |
| CMC    | 0.033    | 0.021          | 0.021     | 0.017           | 0.021   | 0.017         |
| XONA   | 0.026    | 0.026          | 0.013     | 0.021           | 0.017   | 0.019         |
| GOOGL  | 0.026    | 0.022          | 0.015     | 0.014           | 0.015   | 0.013         |
| NCB    | 0.02     | 0.022          | 0.016     | 0.013           | 0.016   | 0.012         |
| PFE    | 0.022    | 0.025          | 0.016     | 0.019           | 0.016   | 0.016         |
| UNH    | 0.024    | 0.02           | 0.013     | 0.014           | 0.012   | 0.013         |
| **Avg.**   | **0.023**    | **0.0205**         | **0.0155**    | **0.015**           | **0.016**   | **0.014**         |

**Laplace-GARCH** enotes our model. Note that values closer to 1%, are better; that is, a perfect model with an infinite amount of sample data would exhibit **1% or 0.01** violations.

#### Execution times and scalability <a name="scalability"/>

In addition to superior performance, a strategic and highly efficient implementation results in a low execution time, which allows us to apply our approach to large-scale portfolios:

| Assets | Model-estimation | Value-at-Risk | Total   |
|--------|------------------|---------------|---------|
| 1      | 8.14             | 1.09          | **9.23**    |
| 5      | 16.7             | 1.45          | **18.15**   |
| 10     | 24.5             | 1.45          | **25.95**   |
| 50     | 107              | 1.53          | **108.53**  |
| 100    | 240              | 2.49          | **242.49**  |

The running times are given in 1/1000 seconds (i.e. it takes less than a 1/4 of a second to process a 100 asset portfolio). Note, we used the following lower mid-spec system: Core™ i3-8109U CPU @ 3.00GHz × 2 with 7.7 GB RAM.

Moreover, the following table shows that the computation times of our model is overall lower than that of available reference models:

``` shell

# Estimate model paramter:
Gaussian GARCH (arch-package),  100 loops, best of 3: 15.4 msec per loop
Student-t GARCH (arch-package): 100 loops, best of 3: 23.6 msec per loop
Laplace GARCH (our model):      100 loops, best of 3: 14.6 msec per loop

# Compute Value-at-Risk:
Gaussian GARCH (arch-package):  10000 loops, best of 3:   90 usec per loop
Student-t GARCH (arch-package): 10000 loops, best of 3:  191 usec per loop
Laplace GARCH (our model):      10000 loops, best of 3: 1120 usec per loop
```

The reference implementations are from the well-respected `arch` Python package.

Some notes about the comparison:
- These times refer to a single-asset portfolio, simply because there is no (publicly) available package in Python supporting GARCH in a multivariate setting.
- It takes much longer (relatively) to compute the Value-at-Risk with our model because there is no closed-form expression available and we need to resort to numerical integration. However, since the Value-at-Risk computation takes much less of the total computation time and scales sub-linearly, this does not matter.
- Note the significant difference vis-a-vis the Student-t model, which also features heavy tails is of similar complexity and computational requirements. While the difference in estimation time is small vis-a-vis the Gaussian model, this is expected as the estimation process of the Gaussian model is much less complicated than the ones of their heavy-tailed cases.
