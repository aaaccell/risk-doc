### Model specification <a name="specification"/>

The Portfolio- and Risk-management engine is based upon a Multivariate Generalized Laplace (MGL) distribution. Because its tails are not exponentially bounded, the MGL distribution belongs to the class of so-called "heavy-tailed" distributions which can accommodate extreme market events. To model different return regimes, multivariate asset returns are filtered using a least-squares filter with L1-penalization. Additionally, the engine models asset volatility as multivariate asymmetric Generalized Autoregressive Conditional Heteroscedasticity (GARCH) process, which allows the model to reflect many of the stylized facts of financial markets accurately.