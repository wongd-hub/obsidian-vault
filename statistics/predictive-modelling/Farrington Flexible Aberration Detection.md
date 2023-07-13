#excess-mortality

- A quasi-Poisson regression-based aberration detection algorithm

## Motivation

- Need a way to detect outbreaks quickly and accurately to allow time to respond
- Initial motivation was detection of bio-terrorist attacks but this can be used in any environment that requires swift and automated anomaly detection.

## Method summary

- General idea is to model baseline counts of events (generally trained over 5 years of baseline data)
- The expected counts in the current week is then projected
- If the observed value in this week is far enough away from the expected that the *exceedance score* is breached, this week is flagged as a possible outbreak.

## Method details

- Since the events in question are often counts, they may be best modelled by the Poisson distribution. However, this restricts variance to be equal to the mean, and variance often exceeds the mean. Hence, this algorithm uses an *overdispersed* Poisson regression to model counts - ie. Negative Binomial regression, a generalisation of Poisson regression.
- In addition, there are a few extra terms:
	- A time-varying trend is always fitted, regardless of if it is statistically significant
	- To capture seasonality, a yearly 10-level factor, whose reference period comprises comparable weeks in previous years.
- The functional form is as follows ($i$ refers to the week number):

$$log(\mu_i) = \theta + \beta t_i + \delta_{j(t_i)}$$

Where:
- $\mu_i$ is the mean (expected) count, with variance $\phi\mu_i$
- $\theta$ is the constant/intercept
- $\delta_{j(t_i)}$ is the 10-level 
