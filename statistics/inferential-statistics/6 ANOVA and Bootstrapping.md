---
title: "6 ANOVA and Bootstrapping"
---

#course_coursera-inferential-stats #statistics

## Comparing more than two means

The data we'll consider here comes from the General Social Survey. We're interested in the average vocabulary score split by self-identified social class:

![[Pasted image 20230712222908.png]]

We can use Analysis of Variance (ANOVA) to analyse 3+ means (whereas the [[5 t-distribution and Comparing Two Means|t-distribution]] only works for comparing two means).

- Note that, as with the t-test, comparison groups with means that are further apart and with tighter variances will be more likely to be found as statistically significantly different from the other means.

- ANOVA works with a new test statistic called $F$, which is accompanied by the F-distribution

**ANOVA**

- As with in a t-test, ANOVA compares the means of multiple groups to see if they're so far apart that their difference cannot be reasonably attributed to sampling variability.

- $H_0: \mu_1 = \mu_2 = \dots = \mu_k, H_A = \text{At least one pair of means are different to each other}$
	- Where $k$ is the number of groups

- As with the t-statistic (a ratio of size of effect to standard error), the F-statistic is a ratio of the variability between groups over the variability within groups.
	- $F = \frac{\text{variability between groups}}{\text{variability within groups}}$
	- Recall that large test statistics lead to small p-values, which means you're more likely to find that there is at least one difference between a pair of means. Naturally, this means that you have a higher variability between groups relative to the variability within groups.

- The F-distribution is right skewed and always positive (as its a ratio of two variances, which can never be negative)

![[Pasted image 20230712222919.png]]

## ANOVA

ANOVA works on the idea of *variability partitioning*, which is the act of taking the overall variance and splitting it into:

- Variance caused by the independent variable (i.e. the between group variability)
- Variance caused by all other factors (i.e. the within group variability)

![[Pasted image 20230712222930.png]]

### Breaking down ANOVA output

Here's a look at what an ANOVA output table looks like; we'll go through each component step by step.

- The first row is about between group variability, the second is about within group variability.
- The third row displays the totals.

![[Pasted image 20230712222939.png]]

#### Sum of Squares column

*Sum of Squares Total (SST)*

The last value in this column is Sum of Squares Total (SST), and it measures the total variability in the response variable. It is calculated in a similar manner to variance, but without adjusting for sample size. In other words, this is the sum of squared deviation from the mean in the response variable.

$$SST = \sum^n_{i=1}\left(y_i-\bar{y}\right)^2$$

Where $y_i$ represents each observation in the dataset, and $\bar{y}$ is the grand mean across all observations.

*Sum of Squares between Groups (SSG)*

The first value in this column is called the Sum of Squares between Groups (SSG), and it measures the variability in the response variable that is explained by the independent variable.

This is calculated as the sum of deviations of group means from the overall mean, weighted by the sample size of each group:

$$SSG = \sum^k_{j=1}n_j\left(\bar{y}_j-\bar{y}\right)$$

Where $n_j$ is the number of observations in group $j$, $\bar{y}_j$ is the mean of the response variable in group $j$, and $\bar{y}$ is the grand mean across all observations.

For this example, the SSG is about 7.6% of the SST; i.e. 7.6% of total variability in the vocabulary scores is explained by difference in social class, and the remainder is explained by other factors.

*Sum of Squares Error (SSE)*

This is the second value in the Sum of Square column, and it measures the unexplained variability due to all other variables. The simplest way to calculate this is:

$$SSE = SST - SSG$$

#### Mean Square Values column

To get from the Sum of Squares column to the Mean Square Values column, we need to scale the sum of squares by a measure that takes into account both:

- Sample size; and,
- Number of groups

We can use degrees of freedom to represent this; the degrees of freedom associated with ANOVA are:

- Total: $df_T=n-1$
- Group: $df_G=k-1$
- Error: $df_E=df_T-df_G$

Next, the Mean Squares are calculated as follows:

- Group: $MSG=SSG/df_G$
- Error: $MSE=SSE/df_E$

#### F-statistic

The F-statistic is the ratio between the average between and within group variabilities:

$$F = \frac{MSG}{MSE}$$

#### P-value

We can finally calculate our p-value which may be interpreted as the probability of observing our ratio of between and within group variabilities if fact the means of all groups are equal. A few things to note:

- The F-distribution uses two degrees of freedom: degrees of freedom between groups ($df_G$), and degrees of freedom of errors ($df_E$)

- The F-statistic can **never be negative** and hence we only consider the right side of the distribution
	- As a result of this, a more extreme statistic will always be more extreme in the positive direction

```r
pf(21.735, 3, 791, lower.tail = FALSE)
#> 1.55985531902116e-13
```

### Conclusion

As with previous [[3 Hypothesis Testing|hypothesis tests]]:

- If the p-value is small/less than $\alpha$, we reject the null hypothesis and conclude that there is evidence that at least one pair of means is different (although we can't tell which one at this stage).
- Otherwise if the p-value is larger than $\alpha$, we do not reject the null hypothesis and conclude that there is no evidence that any pair of means is statistically significantly different from the other.

## Conditions for ANOVA

There are three main conditions that need to be met for ANOVA to be valid:

1. Independence
	- Within groups: sampled observations must be independent
	- Between groups: the groups must be independent of each other (i.e. non-paired)

2. Approximate normality: each group must be approximately normal

3. Equal variance: groups should have roughly equal variance

### Independence

*Independence within groups*

We can be confidence that this is the case if we have random sampling/assignment, and if each sample size is less than 10% of the respective population.

*Independence between groups*

Groups must be independent of another, and this requires careful consideration of the results to ensure there is no paired structure that is being caused by the study design.

- Not the end of the world if this isn't met, there is a method called Repeated Measures ANOVA that can deal with this case.

### Approximately normal

Especially important when the sample sizes are small, but will also be harder to check if that's the case. We can use normal probability plots within each group to check this assumption.

### Constant/equal variance

i.e. homoskedastic groups. Especially important if sample sizes vary between groups. Can check this using side-by-side box-plots and by calculating standard deviation within each group.

## Multiple comparisons

Finding a statistically significant result with ANOVA only tells us that one pair of means is different - but not which pairs differ. The method for determining which pair of means differs is simply to perform many pair-wise t-tests between each group's means. This is referred to as 'multiple comparisons'. Note, to perform these tests, we'll need to reconsider our degrees of freedom and standard error; as well as our significance level.

Recall that the Type I error rate is the probability that you will commit a Type I error; performing many pair-wise tests simultaneously inflates your Type I error rate which is undesirable.

The solution to this is simple, use a modified, more conservative significance level. This is achieved by using the *Bonferroni correction*, which adjusts $\alpha$ by the number of comparisons being considered.

**Bonferroni correction**

The adjusted significance level is defined as:

$$\alpha^* = \alpha/K$$

Where $K = \frac{k\left(k-1\right)}{2}$ is the number of comparisons that will take place in your multiple comparisons process ($k$ being the number of means you have on hand to compare).

Back to our example, we have four means so $k = 4$, and our initial $\alpha$ is $0.05$. For this example, $K = 6$, and $\alpha^* = 0.05 / 6 \approx 0.0083$

**Standard error for multiple pairwise comparisons**

$$SE = \sqrt{\frac{MSE}{n_1} + \frac{MSE}{n_2}}$$

This is similar to the independent groups t-test, however the numerators are using the *mean squared error from the ANOVA output* instead of the individual sample standard deviations.

Recall that the mean squared error is essentially the average within group variance, so we're still getting at the same concept as when we were using the individual variances. If the constant variance assumption is satisfied, this measure will be very close to the sample standard deviations anyway.

**Degrees of freedom for multiple pairwise comparisons**

$$df = df_{E}$$
Instead of $min\left(n_1-1, n_2-1\right)$ in the independent means t-test, this is the degrees of freedom for the error in the ANOVA output.

#### Example

Let's pick a pair and compare the means of lower and middle class vocabulary scores.

- Lower class ~ n: 41, mean: 5.07
- Middle class ~ n: 331, mean: 6.76

On to the hypothesis test:

- $H_0: \mu_{middle} - \mu_{lower} = 0, H_A: \mu_{middle} - \mu_{lower} \neq 0$

- $T = \frac{\left(\bar{x}_{middle} - \bar{x}_{lower}\right) - 0}{\sqrt{\frac{MSE}{n_{middle}} + \frac{MSE}{n_{lower}}}} = \frac{6.76 - 5.07}{\sqrt{\frac{3.628}{41} + \frac{3.628}{331}}} = 5.365$

- $df = 791$ (from the table at the start of this article)

```r
pt(-5.365, 791) * 2
#> 1.06389466632633e-07
```

1.06e-07 < 0.0083, hence we reject the null hypothesis and note there is strong evidence that there is a statistically significant difference between the average vocabulary scores of lower and middle class Americans.

## Bootstrapping

Say we take a random sample of 20 rental properties in Durham and want to estimate the population median (and construct a confidence interval for it).

![[Pasted image 20230712222959.png]]

This is a small sample and the sample distribution looks fairly skewed, so CLT-based methods would not be reliable here. This is where we draw on simulation-based methods such as bootstrapping.

In bootstrapping, we assume that each observation in the sample represents others in the population at large. This means that the bootstrap population is made up of the same observations that are in your sample, repeated an arbitrary number of times.

We don't actually create the bootstrap population, but instead simulate it by drawing from the original sample multiple times *with replacement*. The overall bootstrapping methodology is:

- Randomly sample from your original sample with replacement, until you have the same number of observations as in your original sample

- Calculate your sample/bootstrap statistic with the bootstrapped sample

- Repeat the previous two steps as many times as you need to create a bootstrap distribution (which is a sampling distribution, just using bootstrapped samples instead of samples drawn from the population)

Once we have the bootstrap distribution, we can calculate confidence intervals in 2 ways:

1. Percentile method: estimate a 95% (for example) confidence interval as the middle 95% of the distribution - the bounds of which would be the 2.5th and 97.5th percentiles of the bootstrap distribution.

2. Standard error method (ostensibly more accurate): estimate the interval as $\text{sample statistic} \pm \space t^*_{df=n-1} \times SE_{boot}$, where $n$ is the original sample size.

**Example**

This dot plot shows the distribution of medians of 100 bootstrap samples from the original sample. We want to estimate the 90% bootstrap confidence interval for the median rent, based on this bootstrap distribution, using the standard error method.

![[Pasted image 20230712223008.png]]

First, to get our t-statistic:

```r
qt(0.05, 20 - 1, lower.tail = FALSE)
#> 1.72913281152137
```

Then, given that our sample median is 887, we get:

$$\text{sample median} \pm t^*_{19} \times SE_{boot} = 887 \pm 1.73 \times 89.5758 = (732.1, 1041.9)$$

We're 90% confident that the population median of rental prices in Durham falls between these bounds.

#### Percentile vs. Standard Error methods

We can see that the bounds using either method are close to each other - but are not exactly the same in this case.

![[Pasted image 20230712223017.png]]

#### Closing remarks

- Simulation methods do not have as rigorous assumptions as CLT-based methods, however a larger sample size will yield more reliable results as usual.

- If the bootstrap distribution is extremely skewed or sparse, the bootstrap interval might be unreliable.

- A representative sample is still required - garbage in, garbage out.

- Bootstrap vs sampling distributions
	- Sampling distributions are created by taking random samples with replacement from the population
	- Bootstrapping distributions are created by taking random samples with replacement from the sample
	- Both are distributions of sample statistics