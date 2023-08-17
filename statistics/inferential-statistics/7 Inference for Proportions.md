---
title: "7 Inference for Proportions"
---

#course_coursera-inferential-stats #statistics 

## Introduction

This week we discuss categorical variables where the metric of interest is a proportion (e.g. proportion of elderly patients taking antihypertensive medications that experience falls).

- We'll start off by looking at one categorical variable at a time, focusing on binary categorical variables first
    - Then we'll expand to one categorical variable with multiple levels (e.g. socioeconomic status = low/medium/high)

- Then we'll move to looking at two categorical variables where both are binary (e.g. does gender influence whether or not someone goes into a scientific field?)
    - Finally we'll look at the case where there are two categorical variables and both have multiple levels (e.g. does socioeconomic status affect education attainment?)

## Sampling variability and CLT for proportions

This lesson begins by setting up the premise of sampling distributions just as the first lesson did; however in this case, within each sample we are calculating a proportion based on a categorical variable (e.g. x% of the sample are smokers) - this is just another sample statistic.

- Your sampling distribution will be made up of sample proportions; and,
- Given the [[#Conditions for CLT for proportions|CLT for proportions assumptions]] are met, the sampling distribution for the proportion will be approximately normal:

$$\hat{p} \sim N\left(\text{mean} = p, SE = \sqrt{\frac{p(1-p)}{n}}\right)$$

### Conditions for CLT for proportions

These are similar to the conditions for the normal CLT:

1. Independence
	- Random sampling/assignment
	- If sampling without replacement, n < 10% of the population

2. Sample Size/Skew
	- There should be at least 10 successes and 10 failures in each sample, i.e. $np \ge 10$ & $n(1-p) \ge 10$ (if $p$ is unknown, use $\hat{p}$)

#### What if the Sample Size/Skew assumption is not met?

- The center of the sampling distribution will still be around the true population proportion

- The standard error will still adhere to the equation shown earlier

However, the shape of the distribution will depend on whether the true population proportion is closer to 0 or 1.

- Notice that a failure to meet the Sample Size/Skew assumption may be seen as due to having a sample size that is too small

- This means that standard error will be high, and the sampling distribution will be spread much more loosely around the mean

- Since proportions are bounded at 0 and 1, a true population proportion close to either side will yield a distribution that is cut off - a low $p$ will exhibit a right-skewed distribution, a high $p$ will exhibit a left-skewed distribution

![[Pasted image 20230712224406.png]]

If the sample size is high enough, the distribution will be tighter around the population proportion, making the skew less of an issue.

**Example**

We're told that 90% of all plant species are classified as angiosperms. If you were to randomly sample 200 plants from the list of all known plant species, what is the probability that at least 95% of the plants in your sample will be flowering plants?

First we check assumptions:

- Observations are randomly sampled, and 200 < 10% of the total population of all plants
- $200 \times 0.9 \ge 10$ & $200 \times 0.1 \ge 10$

We know that:

- $n = 200$
- $p = 0.9$
- Hence, $\hat{p} \sim N\left(0.9, \sqrt{\frac{0.9 \times 0.1}{200}} \approx 0.0212\right)$

And we want to find $P(\hat{p} > 0.95)$.

- The Z-score of 0.95 is $\frac{0.95 - 0.9}{0.0212} \approx 2.36$, which is a fairly high Z-score, and so…

```r
pnorm(2.36, lower.tail = FALSE)
#> 0.00913746753057268
```

## Confidence interval for a proportion

**Confidence Interval Example**

We are shown that 571 of 670 (~85%) of sampled Americans have good intuition for experimental design. What proportion of all Americans have good intuition for experimental design?

Note that:

- We want to estimate $p$, the proportion of all Americans that have good intuition for experimental design; using,

- $\hat{p}$, the proportion of sampled Americans that have good intuition for experimental design.

Note also that this scenario meets the assumptions for CLT, with a randomly sampled sample that is less than 10% of all Americans, and with the number of successes (571) and failures (99) being &ge; 10.

The formula for a confidence interval around a proportion estimate is:
$$\text{proportion point estimate} \pm z^* \times SE_{\hat{p}} = \text{proportion point estimate} \pm z^* \times \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} = 0.85 \pm 1.96\sqrt{\frac{0.85\times0.15}{670}} = (0.823, 0.877)$$
So we are 95% confident that 82.3% to 87.7% of all Americans have good intuition for experimental design.

**Margin of Error Example**

The margin of error for the previous example was 2.7%, say we want to decrease this to 1% while keeping the confidence level constant. How large of a sample will we need?

$$ME = z^*SE_{\hat{p}} \Rightarrow 0.01 = 1.96\times\sqrt{\frac{0.85 \times 0.15}{n}} \Rightarrow n = 4898.04$$

So we need at least `ceiling(4898.04) =` 4,899 people in our sample to get a 1% margin of error.

**Closing Remarks about Margin of Error**

- Note that if there has been a previously estimated $\hat{p}$, we can use that in our standard error calculation as seen above.
- However, if we have no idea what $\hat{p}$ might be, we use $0.5$ instead.
	- This is because $\hat{p} = 0.5$ is the most conservative estimate and would lead to the largest sample size required
	- It is also not a bad guess if the outcome were binary (dubious)

## Hypothesis test for a proportion

Recall that the steps to complete a hypothesis test are as follows:

1. Assert the two hypotheses: $H_0$ and $H_A$
2. Calculate the point estimate: $\hat{p}$
3. Check independence and sample size/skew conditions
4. Draw sampling distribution, shade p-value, calculate the Z-score noting that $p$ will be the value in the null hypothesis
5. Make a decision and interpret it in the context of your scenario - if p-value is &le; $\alpha$, then the data provides convincing evidence for $H_A$

To clarify the usage of $p$ vs. $\hat{p}$; we use the sample proportion when there is nothing else known (i.e. in confidence intervals), but we use the hypothesised population proportion when doing hypothesis tests.

![[Pasted image 20230712224422.png]]

Why are we just talking about this now when we went through 3 weeks of performing inference for means? This is because we had no need for a null value for $\mu$ as the standard error equation for a mean did not include the mean itself. For proportions, this is not the case - you need a null value for proportions to calculate the corresponding standard error. This will also come into play for when we perform inference for the difference between two proportions; most dramatically when attempting hypothesis tests (since that requires a null value to be asserted).

**Example**

A 2013 Pew Research poll found that 60% of 1,983 randomly sampled American adults believe in evolution. Does this provide convincing evidence that majority of Americans believe in evolution?

1. $H_0: p = 0.5$, $H_A: p > 0.5$
2. $\hat{p} = 0.6$
3. Sample is randomly picked and is smaller than 10% of the American adult population. Successes and failures are both &ge; 10. CLT assumptions are met.
4. $Z = \frac{\hat{p} - p}{\sqrt{\frac{p(1-p)}{n}}} = \frac{0.6-0.5}{0.0122} \approx 8.92$, we know this is a high Z-score and hence will have a very small p-value.

```r
pnorm(8.92, lower.tail = FALSE)
#> 2.33144105408587e-19
```

1. Hence, the data provide convincing evidence for the alternative hypothesis, which is that the majority of Americans believe in evolution.

This p-value is interpreted as that there is almost 0% chance of obtaining a random sample of 1,983 Americans where 60% or more believe in evolution, if in fact 50% of Americans believe in evolution.

## Estimating the difference between two proportions

**Example**

We'll be comparing a Gallup poll (U.S. only) against a Coursera poll (international) where participants are asked whether or not they believe there should be laws restricting the ability to possess handguns. The results are shown below:

| Sample | 'Yes' | n | $\hat{p}$ |
| :- | -: | -: | -: |
| Gallup | 275 | 1028 | 0.25 |
| Coursera | 59 | 83 | 0.71 |

How do Coursera students and the American public at large compare with respect to their views on laws banning possession of handguns? Use a 95% confidence level.

- The parameter of interest we are estimating is $p_{Coursera} - p_{US}$, the difference between the proportions of all Coursera users and all Americans who believe there should be a ban on handguns.

- The point estimate is $\hat{p}_{Coursera} - \hat{p}_{US}$, the difference between the proportions of sampled Coursera users and sampled Americans who believe there should be a ban on handguns.

Before we dive in, let's check that the assumptions hold:

- Independence
	- Within groups: sampled observations must be independent of each other
		- Gallup - randomly sampled, and n &le; 10% of the American population
		- Coursera - a voluntary poll, so not randomly sampled. Must be careful about generalising to the total Coursera student population.
	- Between groups: the two groups must be independent of each other (i.e. non-paired)
		- This is met here.

- Sample Size/Skew: each sample must meet the success-failure condition
	- $n_1p_1 \ge 10$ & $n_1(1-p_1) \ge 10$
	- $n_2p_2 \ge 10$ & $n_2(1-p_2) \ge 10$
		- This is met in both groups

Since these assumptions are mostly met, we can assume that the difference between the proportions is approximately normally distributed.

The formula for estimating the confidence interval for a difference in proportions is as follows:

$$\text{point estimate} \pm \text{margin of error} \Rightarrow \left(\hat{p}_1 - \hat{p}_2\right) \pm z^*SE_{\hat{p}_1 - \hat{p}_2}, \text{where } SE_{\hat{p}_1 - \hat{p}_2} = \sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n_1} + \frac{\hat{p}_2(1 - \hat{p}_2)}{n_2}}$$
$$\therefore \left(\hat{p}_{Coursera} - \hat{p}_{US}\right) \pm z^*SE_{\hat{p}_{Coursera} - \hat{p}_{US}} \Rightarrow (0.71 - 0.25) \pm 1.96 \times \sqrt{\frac{0.71\times0.29}{83} + \frac{0.25\times0.75}{1028}} = (0.36, 0.56)$$

Note that if we were asked to perform a hypothesis test on this same scenario, we would end up rejecting the null hypothesis (that posits there is no difference between the two proportions) since this confidence interval does not include 0.

#### Does the order matter?

What would happen if we switch the order of the Coursera proportion vs. the US proportion? Let's break down the confidence interval equation:

- $\left(\hat{p}_1 - \hat{p}_2\right)$ can be either positive or negative
- $z^*$ must always be positive
- $SE_{\hat{p}_1 - \hat{p}_2}$ must always be positive - hence the margin of error will always be positive

So if we switch the order, the overall conclusion will be the same - just that the order of comparison will be different.

![[Pasted image 20230712224441.png]]

So for the above:

- In the first case we can interpret this as we are 95% confident that the Coursera proportion is 36-56% higher than the US proportion.
- In the second, we are 95% confident that the US proportion is 36-56% lower than the Coursera proportion which is the converse and therefore still correct and equivalent.

## Hypothesis test for comparing two proportions

Recall that for confidence intervals, we estimate bounds using the sample proportion (and we get these from our samples). For hypothesis tests, we use the null value for the proportion in our calculations (since that's assumed to be true). However, for the difference between proportions, there *is no null value asserted*; the only assertion is that $p_1 = p_2$.

Hence, for our calculations when performing hypothesis tests on the difference between two proportions, we need to use the concept of a *pooled proportion*.

$$H_0: p_1 = p_2 = ? \Rightarrow \hat{p}_{pool} = \frac{\text{total successes}}{\text{total n}} = \frac{\text{num of successes}_1 + \text{num of successes}_2}{n_1 + n_2}$$

To recap, when working with two proportions:

![[Pasted image 20230712224207.png]]

**Example**

Consider a study in which parents are asked whether or not their kids have been bullied at school. Only one parent can answer of the pair of them. There are some reasons why the proportion of bullied kids may vary by the parent that answers the survey:

- Same-sex parents may slightly skew the results
- Male vs. female parents may differ in their likelihood to be aware of bullying or to even be told by their children

The study results are as follows:

| Result | Male | Female |
| :- | -:| -: |
| Yes | 34 | 61 |
| No | 52 | 61 |
| Not sure | 4 | 0 |
| Total | 90 | 122 |
| $\hat{p}$ | 0.38 | 0.5|

So:

- $\hat{p}_{pool} = \frac{34 + 61}{90 + 122} \approx 0.45$

Checking assumptions before we dive into the hypothesis test

- Independence is met
	- Within groups: All sampled individuals are independent of each other
	- Between groups: No reason to suspect that the samples are paired (only one partner can answer per parent pair)

- Sample Size/Skew is met (note that we use pooled proportions here)
	- Males: $90 \times 0.45 = 40.5$ & $90 \times 0.55 = 49.5$
	- Females: $122 \times 0.45 = 54.9$ & $122 \times 0.55 = 67.1$

Conditions for CLT are met, so $\left(\hat{p}_{male} - \hat{p}_{female}\right) \sim N\left(0, SE = \sqrt{\frac{0.45\times0.55}{90} + \frac{0.45\times0.55}{122}}\approx0.0691\right)$

We can now perform our hypothesis test

1. $H_0: p_{male} - p_{female} = 0, H_A: p_{male} - p_{female} \neq 0$
2. $\hat{p}_{male} - \hat{p}_{female} = -0.12$
3. As noted earlier, CLT assumptions are met
4. $Z = \frac{\left(\hat{p}_{male} - \hat{p}_{female}\right) - 0}{SE_{\hat{p}_{male} - \hat{p}_{female}}} = \frac{-0.12 - 0}{0.0691} \approx -1.74$, and noting this is a two-tailed hypothesis test…

```r
pnorm(-1.74) * 2
#> 0.0818590179576147
```

1. Based on this p-value, we can conclude that there is evidence that there is no difference in males and females with respect to likelihood of reporting their kids being bullied.