#coursera-inferential-stats #statistics 

## Table of contents

1. [[#Introduction]]
2. [[#Confidence interval (for a mean)]]
3. [[#Accuracy vs. Precision (of confidence intervals)]]
4. [[#Required sample size for Margin of Error]]
5. [[#CI (for the mean) examples]]

## Introduction

A plausible range of values for the population parameter is called a confidence interval.

- Using only a sample statistic to estimate a population parameter is like fishing in a murky lake with a spear. Using a confidence interval is like fishing with a net.

- In other words, if we report a point estimate, we are less likely to have the correct population parameter. However, if we report a confidence interval, we have a good shot at capturing the population parameter.

Based on one sample's mean, $\bar{x}$, how do we figure out what the range of plausible values is going to be?

- Our sample mean is indeed our best guess of what the population parameter is, so our interval will be centred around this value.

- From the [[1 Central Limit Theorem and Sampling Distributions|CLT]], we know $\bar{x}$ is distributed nearly normally, and the centre of the distribution is the unknown population mean.

- The final thing to note here is the 68%, 95%, 99.7% rule. This tells us that roughly 95% of sample means will be within 2 standard deviations of the population mean. So for 95% of random samples, the true population is going to be within 2 standard errors of that sample's mean.

- So a 95% confidence interval can be constructed as the sample mean plus or minus two standard errors; i.e. $\bar{x} \pm 2SE$

- What comes after the $\pm$ is referred to as the *Margin of Error* (ME).
![[Pasted image 20230711231359.png]]

### Example problem

One of the earliest examples of behavioral asymmetry is a preference in humans for turning the head to the right rather than to the left during the final weeks of gestation and for the first six months after birth. This is thought to influence subsequent development of perceptual and motor preferences. A study of 124 couples found that 64.5% turn their heads to the right when kissing. The standard error associated with this estimate is roughly 4%. Which of the below is false?

1. A higher sample size will yield a lower standard error.

2. The margin of error for a 95% CI for the percentage of kissers who turn their heads to the right is roughly 8%.

3. The 95% CI for the percentage of kissers who turn their heads to the right is roughly $64.5\% \pm 4\%$.

4. The 99.7% CI for the percentage of kissers who turn their heads to the right is roughly $64.5\% \pm 12\%$.

The false option is 3 - the correct margin of error for a 95% confidence interval is 2 $\times$ SE, so 8%.

## Confidence interval (for a mean)

More formally, the confidence interval for the population mean is the sample mean plus/minus the margin of error (which is the critical value corresponding to the middle x% of the normal distribution, multiplied against the standard error of the sampling distribution.)

$$\bar{x} \pm z^*\frac{s}{\sqrt{n}}$$

We mentioned before that the margin of error for a 95% confidence interval is 2 $\times$ the standard error. This is actually not exact.

- So how do we find the exact critical value we need?

- Remember that the confidence interval is the middle portion of the distribution, and that if we want a 95% confidence interval, we want the middle 95%.

- We could use a Z-score to calculate the probability of getting the remaining 5% in the tail ends; but this really just equates to finding the area under the curve between the Z-scores.

- In this case, since this is a probability distribution, the total area is 1; and therefore the critical value is $(1 - 0.95) / 2 = 0.025$

- Performing an inverse normal distribution calculation (in R, `qnorm(0.025`) on the critical value of 0.025, we get a Z-score of 1.96, which is our exact value.

```r
qnorm((1 - 0.95) / 2)
#> -1.95996398454005
```

There are some conditions that must be satisfied for this formula to apply, but since this confidence interval is based on the [[1 Central Limit Theorem and Sampling Distributions|CLT]], the conditions are the exact same:

1. Independence

2. Size and skew - need a larger sample size (n &ge; 30) if the population is very skewed

The second condition is a little stricter than what we saw with the CLT as it imposes a minimum sample size (30). We'll talk about what to do if our sample size is smaller than 30 in a later lecture; so let's focus on 'large' sample sizes for now.

## Accuracy vs. Precision (of confidence intervals)

- Accuracy is defined as whether or not the confidence interval contains the true population parameter.

- Precision is defined as the width of the confidence interval.

- There is a trade-off between these two aspects which we will discuss now.

**Confidence levels**

- Suppose we took many samples and built confidence intervals for each one using the equation $\text{point estimate} \pm 1.96 \times SE$.

- Then 95% of these confidence intervals will contain the true population mean, $\mu$.

- Obviously this isn't how we generally come up with confidence intervals since we generally only work with one sample.

- Further, the confidence level isn't something we calculate, and rather something we come up with that then affects the rest of our calculations.

Changing the confidence level simply means adjusting the critical value aspect of the confidence interval formula.

- Increasing the confidence level inherently means creating a wider confidence interval; in turn, this confidence interval is more likely to capture the true population parameter.

- In other words, increasing our accuracy by increasing our confidence level will also decrease our precision.

- For example, say the weather forecast predicts that the temperature tomorrow will be between $-20^\text{o}C$ and $100^\text{o}C$. Is this accurate? Yes, because it's likely to contain the true temperature for tomorrow. However, is it precise or in any way informative? No.

Is it possible to increase both accuracy and precision without making trade-offs? Yes - the answer is to increase sample size, which shrinks our standard error and therefore our margin of error.

#### Example

The General Social Survey (GSS) is a sociological survey used to collect data on demographic characteristics and attitudes of residents of the United States. In 2010, the survey collected responses from 1,154 U.S. residents. Based on the survey results, a 95% confidence interval for the average number of hours Americans have to relax or pursue activities that they enjoy after an average workday, was found to be 3.53 to 3.83 hours. Determine if each of the following statements are true or false.  

1. 95% of Americans spend 3.53 to 3.83 hours relaxing after a work day

- This is not true because the confidence interval is not about individuals in the population but instead about the true population parameter.

1. 95% of random samples of 1,154 Americans will yield confidence intervals that contain the true average number of hours Americans spend relaxing after a work day

- This is the exact definition of the confidence level: the percentage of random samples that will yield confidence intervals that contain the true population parameter.

1. 95% of the time the true average number of hours Americans spend relaxing after a work day is between 3.53 and 3.83 hours

- This is not true because the population parameter is not a moving target that is sometimes within an interval and sometimes outside of it.

1. We are 95% confident that Americans in this sample spend on average 3.53 to 3.83 hours relaxing after a work day

- This is not true because the confidence interval is not about the sample mean, but is instead about the population mean. We know with 100% certainty what the sample mean is here since we've observed this sample.

## Required sample size for Margin of Error

What would we need n to be to achieve a given ME?

- With a target margin of error, the confidence level, and information on the variability of the sample, we can determine the required sample size to achieve a certain margin of error.

- We do this by re-arranging the ME equation:

$$ME = z^*\frac{s}{\sqrt{n}} \to n = \left(\frac{z^*s}{ME}\right)^2$$

We can see from this that as sample size increases, ME will decrease.

#### Example
  ![[Pasted image 20230711231600.png]]

- The critical value associated with a 90% confidence level is 1.65 (`qnorm()`)

- Note that this means that the *minimum* required sample size is 55.13; so we need to round up to get our answer.

```r
qnorm((1 - 0.90) / 2)
#> -1.64485362695147
```

#### Example 2

![[Pasted image 20230711231635.png]]

- In other words, to cut your margin of error by half, you'll need to increase your sample size 22=4 fold.

## CI (for the mean) examples

#### Example

![[Pasted image 20230711231704.png]]
![[Pasted image 20230711231709.png]]
![[Pasted image 20230711231715.png]]
![[Pasted image 20230711231719.png]]