#excess-mortality #regression

- A quasi-Poisson regression-based aberration detection algorithm

## Useful links

- [R `surveillance` function definition for `farringtonFlexible()`](https://github.com/r-forge/surveillance/blob/master/pkg/R/farringtonFlexible.R)
	- [Official documentation](https://surveillance.r-forge.r-project.org/pkgdown/reference/farringtonFlexible.html)
- [*Comparison of statistical algorithms for daily syndromic surveillance aberration detection*, Noufaily et al. 2019](https://academic.oup.com/bioinformatics/article/35/17/3110/5301313#151566659)
- *An improved algorithm for outbreak detection in multiple surveillance systems*, Noufaily et al. 2012, doi:10.1002/sim.5595 - best detailed look at Farrington Flexible
- *Monitoring count time series in R: Aberration detection in public health surveillance*, Salmon et al. 2016, [doi:10.18637/jss.v070.i10](https://doi.org/10.18637/jss.v070.i10) - paper on the `surveillance` package, explaining how Farrington Flexible is implemented

## Motivation

- We need a way to detect outbreaks quickly and accurately to allow time to respond
- Initial motivation was detection of bio-terrorist attacks but this can be used in any environment that requires swift and automated anomaly detection.

## Method summary

- General idea is to model baseline counts of events (generally trained over 5 years of baseline data)
- The expected counts in the current week is then projected
- If the observed value in this week is far enough away from the expected that the *exceedance score* is breached, this week is flagged as a possible outbreak.

## Method details

- Since the events in question are often counts, they may be best modelled by the Poisson distribution. However, this restricts variance to be equal to the mean, and variance often exceeds the mean. Hence, this algorithm uses an *overdispersed* [[Poisson regression]] to model counts - ie. [[Negative Binomial regression]], a generalisation of Poisson regression.
- The original method took seasonality into account by **only** using a window around the current week in previous years as training data - so the model is trained on a subset of each year. Noufaily et al.'s method uses all available historical data and takes seasonality into account with the 10-level factor.
- Noufaily et al.'s method also excludes the last 26 weeks before $t_0$ (the week we're predicting over to detect an outbreak) in an attempt to make the method less sensitive to outbreaks that may have started recently.
- In addition, there are a few extra terms:
	- A time-varying trend is always fitted, regardless of if it is statistically significant
	- To capture seasonality, a yearly 10-level factor, whose reference period comprises comparable weeks in previous years (as discussed above).
- The functional form is as follows ($i$ refers to the week number):

$$log(\mu_i) = \theta + \beta t_i + \delta_{j(t_i)}$$

Where:
- $\mu_i$ is the mean (expected) count, with variance $\phi\mu_i$
- $\theta$ is the constant/intercept
- $\delta_{j(t_i)}$ is the 10-level factor

- An exceedance score is then calculated finding the distance between the observed count in the target week and what was modelled for it with the quasi-Poisson regression. 
- The count is flagged as an outbreak if this distance is larger than the distance between the modelled mean and the 95% (or $\alpha$%) upper threshold of the Negative Binomial quantile.
	- In other words the exceedance score, $X$ is calculated; and if $X \ge 1$ then the week's count is flagged as an outbreak, where:
$$X = \frac{y_0 - \hat{\mu_0}}{U-\hat{\mu_0}}$$
Where:
- $y_0$ is the observed count in the target week
- $\hat{\mu_0}$ is the expected count, calculated as $\hat{\mu_0} = \hat\theta + \hat{\beta}t_0+\delta_{j(t_0)}$
- $U$ is the upper threshold, the $100(1-\alpha)\%$ Negative Binomial quantile

## Example

We'll use the example data provided in the R `surveillance` package ([`salmonella.agona`](https://rdrr.io/cran/surveillance/man/salmonella.agona.html)), a dataset containing reported number of cases of the Salmonella Agona serovar in the UK 1990-1995.

We need this to be in `sts` form to work with functions in the `surveillance` package. The `salmonella.agona` dataset is stored in `disProg` (the old data type used by `surveillance`), so we need to run:

```r
data("salmonella.agona")

# Create the corresponding sts object from the old disProg object
salm <- disProg2sts(salmonella.agona)
```

- [ ] Todo: graph this data using ggplot

Next, we need to provide a list of controls to the `farringtonFlexible` function. See below for the 'Flexible' controls (as per Salmon et al. 2016):

```r
controls <- list(
  range = 282:312, # date/week range to operate within
  noPeriods = 10, # 10 seasonal factors
  b = 4, w = 3, # controls relating to the window size TODO: FIGURE THIS OUT
  weightsThreshold = 2.58,
  pastWeeksNotIncluded = 26, # not including last few weeks to dampen impact of recent epidemics
  pThresholdTrend = 1,
  alpha = 0.1
)
```
