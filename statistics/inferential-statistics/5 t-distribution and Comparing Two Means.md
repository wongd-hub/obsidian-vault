---
title: "5 t-distribution and Comparing Two Means"
---

#course_coursera-inferential-stats #statistics 

## Inference for Numerical Variables

We'll build upon the tools we've used to perform inference on numerical variables in this unit. We'll be looking at:

- Comparing multiple means against one another (two, or more using [[6 ANOVA and Bootstrapping#ANOVA|ANOVA]])
- A technique that allows one to create confidence intervals for estimators that do not adhere to the Central Limit Theorem (such as the median): [[6 ANOVA and Bootstrapping#Bootstrapping|bootstrapping]]
    - This is a simulation-based method which does not impose as rigid conditions as using the CLT does
    - We'll learn how to bootstrap as well as when to bootstrap
- Working with small samples (n < 30) - spoiler: the t-distribution is used here

## t-distribution

The t-distribution will be useful for describing the distribution of the sample mean when the population $\sigma$ is unknown, which is almost always. Let's discuss the conditions for inference so far, as a motivation for why we need the t-distribution.

### Conditions

- What purpose does a large sample serve? As long as your observations are independent, and the population distribution is not extremely skewed, a large sample will ensure that:
	- The sampling distribution of the mean is nearly normal; and,
	- The estimate of the standard error is reliable.
		- Recall that $SE = \frac{s}{\sqrt{n}}$, where $s$ is the standard deviation of our sample which is our best estimate of the unknown population standard deviation. If our sample is large enough, it'll be more likely that $s$ is a good estimate of the population standard deviation, and therefore the standard error estimation is reliable.

What if your sample size is small?

- The uncertainty of the standard error estimate is addressed by using the t-distribution.

### t-distribution details

- When the population $\sigma$ is unknown (almost always), use the t-distribution to address the uncertainty of the standard error estimate

- Similar to the normal distribution, but with thicker tails, and a less tall modal point

![[Pasted image 20230712220139.png]]

- This means that observations are more likely to fall greater than 2 standard deviations from the mean
	- As a result, [[2 Confidence Intervals|confidence intervals]] constructed from the t-distribution will be wider, and therefore more conservative than those from the normal distribution

i.e. these thick tails are helpful for mitigating the effect of a less reliable estimate of the standard error of the sampling distribution.

- The t-distribution is centred at 0 like the normal distribution, and has one parameter - degrees of freedom (df) - which determines the thickness of the tails.
	- As a reminder, the normal distribution has two parameters - the mean and standard deviation
	- As the df increases, the t-distribution approaches the normal distribution

### t-statistic

How do we actually use the t-distribution in statistical inference?

- Use the t-distribution on a single mean, or for comparing two means, when the population standard deviation is unknown (almost always).

- Calculate the T-statistic just as you would a Z-score: $T = \frac{\text{obs} - \text{null}}{SE}$, then find the corresponding p-value from the t-distribution

#### Examples

We can see that the t-statistic estimate using the same Z-score is closer to the normal distribution estimate when the degrees of freedom are higher. As the degrees of freedom decrease, the tails become heavier, and the p-value associated with a given T-statistic increases.

```r
# P(|Z| > 2)
pnorm(-2) * 2
#> 0.0455002638963584

#P(|t(df = 50)| > 2)
pt(-2, df = 50) * 2
#> 0.0509470687376933

#P(|t(df = 10)| > 2)
pt(-2, df = 10) * 2
#> 0.0733880347707404
```

If we were considering a [[3 Hypothesis Testing|hypothesis test]] with a test statistic of 2, under which of these scenarios would you reject a null hypothesis at the 5% level?

1. You would reject the null hypothesis

2. You would not reject the null hypothesis

3. You would not reject the null hypothesis

As we get more conservative with a t-distribution with lower degrees of freedom, we also become less likely to reject the null hypothesis. We'll discuss how to calculate degrees of freedom later, but in general - the smaller your sample size, the lower the df, and the harder it is to reject the null hypothesis.

### Origins of the t-distribution

- Developed by William Gosset (whose pseudonym was Student, hence Student's t-distribution)
- Gosset worked at Guinness Brewing Company as the Head Experimental Brewer, and his job was to gradually create a more consistent and economical Guinness brew.
- Working under constraints, Gosset generally only had small sample sizes to work with, so most of the groundwork laid for this distribution was done in service to the Guinness stout.

## Inference for a mean

#### Example

Research paper discussing and analysing the effects of being distracted while you eat, and how it affects your recall of what you ate. Hypothesis is that people who did not recall what they ate for lunch will be more prone to snacking later on.

44 people in this study, 22 men and 22 women. Randomised into two groups; one played solitaire whilst eating, and one who just ate. After lunch, they were offered biscuits to snack on

Results are as follows. The goal for this video is to estimate average snacking level for distracted eaters.

![[Pasted image 20230712220330.png]]

**Estimating the mean**

Let's aim to estimate the mean biscuit intake of the sample who played video games.

- To estimate the mean, we'll create a confidence interval. For t-distributions, the calculation of the standard error is the same as the normal distribution, so the only thing that changes in our CI formula is the critical value:
$$\text{point estimate} \pm t_{df}^* \times \frac{s}{\sqrt{n}}$$

- When working with data from one sample, and working with one single mean,
$$df = n - 1$$

- We lose one degree of freedom because we're estimating the standard error of the sample mean using the sample standard deviation.

- To get the critical t-statistic, we use the inverse t-distribution function, `qt(percentile, df)`:

```r
qt(0.025, df = 22 - 1) # since the sample size was 22
#> -2.07961384472768
```

We now have our mean, sample standard deviation, sample size, and critical t-statistic. Our confidence interval is then:

$$52.1 \pm 2.08 \times \frac{45.1}{\sqrt{22}} = \left(32.1, 72.1\right)$$

Therefore, we are 95% confident that distracted eaters consume between 32.1g and 72.1g of snacks post-meal.

**Performing a hypothesis test**

Next, consider that the suggested serving size of biscuits is 30g; do these data suggest that there is a statistically significant difference between the suggested serving size and the amount that distracted eaters eat?

- $H_0: \mu = 30, H_A: \mu \neq 30$
- $T = \frac{52.1 - 30}{9.62} = 2.3$
- $df = 22 - 1$

```r
pt(-2.3, 21) * 2
#> 0.0318022759865702
```

This is less than the significance level of 5%, so we reject the null hypothesis and conclude that distracted eaters consume, on average, more than the suggested serving size of 30.

**Conditions**

Looping back around to check that the conditions are met:

- **Independence**: assignment was random, and 22 < 10% of all distracted eaters
- **Sample size/skew**: we are given the sample mean and sample standard deviation, we can tell that the distribution will be cut off near 0 since the standard deviation is so close to value of the mean.

![[Pasted image 20230712221251.png]]

## Inference for comparing two independent means

**Confidence intervals**

To produce confidence intervals for the difference between two means, we do the following:

$$\left(\bar{x}_1 - \bar{x}_2\right) \pm t^*_{df}\times\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$

Note that we add the two variances even though we're looking for the standard error of the difference between the two means. Conceptually, this is bringing together two measures with an inherent variability around them. When you bring two unknowns together, the result should always be more variable (regardless of if you're adding or subtracting them).

$$df = min(n_1-1, n_2-1)$$

This is a conservative (since it relies on the lower df) estimate, because the exact df value is tedious to compute by hand.

**Conditions**

- Independence
	- Within groups: sampled observations must be independent
		- Random sampling/assignment
		- If sampling without replacement, both $n_1$ and $n_2$ to be < 10% of their respective populations
	- Between groups: groups must be independent of one another (non-paired)
		- If they are paired, we can use other methods
- Sample size/skew
	- The more skew in the population distibution, the larger the sample size we need

**Example**

Coming back to our example,

$$\left(\bar{x}_{wd} - \bar{x}_{wod}\right) \pm t^*_{df}\times\sqrt{\frac{s_{wd}^2}{n_{wd}} + \frac{s_{wod}^2}{n_{wod}}} = \left(52.1 - 27.1\right) \pm 2.08 \times \sqrt{\frac{45.1^2}{22} + \frac{26.4^2}{22}} = \left(1.83, 48.17\right)$$

So we are 95% confident that those who eat with distractions consume 1.83 g and 48.17 g *more* snacks than those who eat without distractions, on average.

**Hypothesis tests**

Do these data provide convincing evidence of a difference in post-meal consumption between the two groups?

- $H_0: \mu_{wd} - \mu_{wod} = 0, H_A: \mu_{wd} - \mu_{wod} \neq 0$

- $T_{21} = \frac{25 - 0}{11.24} = 2.24$; 25 is the difference between the two means, and 11.24 is the previously calculated SE

- The corresponding p-value is:

```r
pt(-2.24, 21) * 2
#> 0.0360369621567888
```

Hence, there is strong evidence in favour of the alternative hypothesis that there is a difference between the two groups. Note this result agrees with our confidence interval which does not include 0.

## Inference for comparing two paired means

We discuss here methods for dealing with two means that are dependent on one another, this is especially useful for studies that take multiple measurements over the same group of people. We could also design studies as paired if we were studying groups that we suspect are not independent, such as twins or partners. We'll start with an example:

![[Pasted image 20230712221306.png]]

- The median writing score is higher than the median reading score
- Both distributions seem fairly symmetric, but the reading score is slightly more right skewed (the median is closer to the left side of the distribution)

Can a student's reading and writing score be assumed independent of one another? Likely not, as these are adjacent skillsets.

- When two sets of datasets have this special correspondence, they are said to be *paired*.
- When looking at paired distributions, it's often useful to look at the difference between the two: diff = read - write

Here, the parameter of interest is $\mu_{diff}$, the average difference between the reading and writing scores of *all* high school students.

- Since we don't have this, we'll estimate the average difference between the reading and writing scores of the sampled high school students: $\bar{x}_{diff}$

For the hypothesis test - we're now doing a hypothesis test on a *single* population mean:

- $H_0: \mu_{diff} = 0, H_A: \mu_{diff} \neq 0$
- We're also told the following: $\bar{x}_{diff} = -0.545, s_{diff} = 8.887, n_{diff} = 200$
- So $T = \frac{-0.545 - 0}{\frac{8.887}{\sqrt{200}}} = -0.87$; $df = 200 - 1 = 199$

```r
pt(-0.87, 199) * 2
#> 0.385348599393971
```

So the probability is 38.5% to obtain a random sample of 200 students where the average difference between reading and writing is at least 0.545, if there truly is no average difference between reading and writing scores.

## Power

Recall that the power of the test is the probability of correctly rejecting $H_0$ when it is indeed false. Recall also that there is a trade-off between $\alpha$ (the probability of committing a Type I error) and $\beta$ (probability of Type II error), and that one way to decrease both is by raising the sample size.

Two competing considerations when designing experiments:

- We want to detect enough data so that we can detect important effects
- But collecting data is expensive

In this lecture, we'll look at the example of designing a medicine study so that we have an 80% chance of detecting a practically significant effect (i.e. we have 80% power - a commonly used power cut-off).

#### Example

Suppose a pharmaceutical company has developed a new drug for lowering blood pressure and they are preparing a clinical trial to test the drug's effectiveness. They recruit people who are taking a particular standard blood pressure medication, and half of the subjects are given the new drug, this is the treatment group. And the other half continued to take their medication through generic-looking pills to ensure blinding, this is our control group. What are the hypotheses for a two-sided hypothesis test in this context?

- $\mu$ will be the average blood pressure in any one of our groups.
	- $H_0: \mu_{treatment} - \mu_{control} = 0, H_A: \mu_{treatment} - \mu_{control} \neq 0$

Suppose researchers would like to run this clinical trial on patients with systolic blood pressures between 140 and 180 millimeter of mercury. Suppose previously published studies suggest that the standard deviation of the patients' blood pressures will be about 12 millimeters of mercury and that the distribution of patients' blood pressures will be approximately symmetric.

If we had 100 patients per group, what would be the approximate standard error for difference in sample means of the treatment and control groups?

- This is a test of difference between two independent groups, therefore the standard error is defined as:

$$SE = \sqrt{\frac{12^2}{100} + \frac{12^2}{100}} = 1.70 mmHg$$

For what values of the difference between the observed average of blood pressure in treatment and control groups would we reject the null hypothesis at the 5% level?

```r
qnorm(0.025, 0, 1.7); qnorm(0.975, 0, 1.7)
#> -3.33193877371809
#> 3.33193877371809

pnorm(-0.2)
#> 0.420740290560897
```

- Any difference between the means outside of this range would cause a rejection of the null hypothesis.

**Power from first principles**

Suppose that the company researchers care about finding any effect on blood pressure that is 3 millimeters of mercury or larger versus the standard medication. What is the power of the test that can detect this effect?

- 3mmHg is the minimum effect size of interest

- Power is the probability that we will reject the null hypothesis, conditional on the alternative hypothesis being true
	- If the alternative hypothesis were true, the true distribution of differences will be normally distributed (as previously given to us about the original distribution) around the -3 difference
	- We also know that we will reject the null hypothesis if the difference were at least -3.33

We have enough information to calculate the probability now: 
$$Z = \frac{-3.33 - (-3)}{1.70} = -0.20 \to P(Z < -0.20) = \dots$$

```r
pnorm(-0.20)
#> 0.420740290560897
```

![[Pasted image 20230712221400.png]]

So the power of this test is around 42% when the minimum effect size is -3 and the group has a sample size of 100. This is clearly lower than the desired 80% power.

**Deriving the sample size from a given power level**

- Note that the minimum effect size remains at -3, but the standard error will change since this varies with the sample size

- Now that we know the power of the test is equivalent to the probability to the left of the critical value that would cause the null hypothesis to be rejected
	- So we will revisit the distribution of differences if the true difference were -3, calculate the Z-score where the area to the left of the distribution is 0.8, then see where that sits on the null hypothesis distribution.

The Z-score that marks the 80th percentile of the normal curve is 0.84:

```r
qnorm(0.8)
#> 0.841621233572914
```

So the distance between the centre of the alternative hypothesis distribution and the cutoff region is $0.84 * SE$, where the $SE$ is still unknown.

![[Pasted image 20230712221425.png]]

Another piece of information is that we know the cutoff region is 1.96SE from the mean of the null hypothesis distribution, so 0 - 1.96SE. What we're about to do assumes that the distribution between the alternative distribution and the null distribution are the same, and this would be true if the drug reduced the average blood pressure, but did not change its overall variability.

From this, we know now that the distance between 3mmHg is spanned by 0.84 + 1.96 standard errors, and we can solve for $n$:

![[Pasted image 20230712221435.png]]

(Instructor really got in the way here)

One final point is that, we know that power increases as sample sizes does, but there are diminishing returns past a certain point. The range of powers that correspond to the following sample sizes are charted as follows:

![[Pasted image 20230712221441.png]]