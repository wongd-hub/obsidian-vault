#excess-mortality #regression

- A quasi-Poisson regression-based aberration detection algorithm

## Useful links

* [R `surveillance` function definition for `farringtonFlexible()`](https://github.com/r-forge/surveillance/blob/master/pkg/R/farringtonFlexible.R)
	* [Official documentation](https://surveillance.r-forge.r-project.org/pkgdown/reference/farringtonFlexible.html)
* [*Comparison of statistical algorithms for daily syndromic surveillance aberration detection*, Noufaily et al. 2019](https://academic.oup.com/bioinformatics/article/35/17/3110/5301313#151566659)
* *An improved algorithm for outbreak detection in multiple surveillance systems*, Noufaily et al. 2012, doi:10.1002/sim.5595 - best detailed look at Farrington Flexible
* *Monitoring count time series in R: Aberration detection in public health surveillance*, Salmon et al. 2016, [doi:10.18637/jss.v070.i10](https://doi.org/10.18637/jss.v070.i10) - paper on the `surveillance` package, explaining how Farrington Flexible is implemented

## Motivation

- Need a way to detect outbreaks quickly and accurately to allow time to respond
- Initial motivation was detection of bio-terrorist attacks but this can be used in any environment that requires swift and automated anomaly detection.

## Method summary

- General idea is to model baseline counts of events (generally trained over 5 years of baseline data)
- The expected counts in the current week is then projected
- If the observed value in this week is far enough away from the expected that the *exceedance score* is breached, this week is flagged as a possible outbreak.

## Method details

- Since the events in question are often counts, they may be best modelled by the Poisson distribution. However, this restricts variance to be equal to the mean, and variance often exceeds the mean. Hence, this algorithm uses an *overdispersed* [[Poisson regression]] to model counts - ie. [[Negative Binomial regression]], a generalisation of Poisson regression.
- In addition, there are a few extra terms:
	- A time-varying trend is always fitted, regardless of if it is statistically significant
	- To capture seasonality, a yearly 10-level factor, whose reference period comprises comparable weeks in previous years.
- The functional form is as follows ($i$ refers to the week number):

$$log(\mu_i) = \theta + \beta t_i + \delta_{j(t_i)}$$

Where:
- $\mu_i$ is the mean (expected) count, with variance $\phi\mu_i$
- $\theta$ is the constant/intercept
- $\delta_{j(t_i)}$ is the 10-level 



## Example

We'll use the example data provided in the R `surveillance` package which is [`salmonella.agona`](https://rdrr.io/cran/surveillance/man/salmonella.agona.html), a dataset containing reported number of cases of the Salmonella Agona serovar in the UK 1990-1995.

