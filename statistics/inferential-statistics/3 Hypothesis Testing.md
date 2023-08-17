---
title: "3 Hypothesis Testing"
---

#course_coursera-inferential-stats  #statistics

## Another Introduction to Inference

We'll start with an example of a first-principles hypothesis test that the last course ended on.

#### Example

In the previous course, we looked at an experiment done on bank managers who were given dossiers on an equal amount of male and female employees who were all equally qualified. The percentage of males promoted was 88%, whereas the percentage of females promoted was 58%:

![[Pasted image 20230711232609.png]]

Looking at this information, there are two competing claims we could make:

1. There is nothing going on - i.e. our Null Hypothesis

This posits that promotion and gender are *independent* and hence there is no gender discrimination. In other words, the observed difference in proportions is simply due to chance.

1. There is something going on - i.e. our Alternative Hypothesis

Promotion and gender are *dependent*, and there is gender discrimination here. The observed difference in proportions is not due to chance.

#### How do we decide which claim has more evidence?

This hypothesis test was approached with a simulation-based approach:

- The difference in proportions of males and females promoted was simulated *x* times under the assumption of the null hypothesis being true.
- The probability of obtaining the results we got in our actual data or something more extreme was small, so we reject the null hypothesis in favour of the alternative hypothesis.

![[Pasted image 20230711232624.png]]

### Recap of example

So to conduct this hypothesis test we:

- Set up a null hypothesis ($H_0$) that represents the status quo

- Set up an alternative hypothesis ($H_A$) that represents our research question/what we're testing for

- Conducted a hypothesis test under the assumption that the null hypothesis is true using either a simulation approach (as in the example) or theoretical methods (this course will focus on methods that rely on the [[1 Central Limit Theorem and Sampling Distributions|CLT]])

- If the test results show that the data do not provide convincing evidence for the alternative hypothesis we stick with the null hypothesis, and vice versa.

## Hypothesis Testing (for a mean)

> [!info] Note
> The hypotheses will always be about the ==population parameter==, and not the sample statistics. We already have the sample and therefore already know the sample statistics.

- $H_0$: Often a skeptical perspective or a claim to be tested. Generally set the parameter of interest equal to same value.

- $H_A$: An alternative claim under consideration, represented by a range of possible values (<, >, $\neq$)

The skeptic will not abandon $H_0$ unless the evidence in favour of $H_A$ is so strong that they reject $H_0$ in favour of $H_A$.

#### Example

Earlier we calculated a 95% [[2 Confidence Intervals|confidence interval]] for the average number of exclusive relationships college students have been in to be 2.7 to 3.7. Based on this confidence interval, do these data support the hypothesis that college students on average have been in more than three exclusive relationships?

Here, $H_0$: $\mu = 3$ & $H_A$: $\mu \gt 3$

Note that the 95% confidence interval includes 3 in it; and any value within the confidence interval could conceivably be the population mean. Therefore, we cannot reject the null hypothesis in favour of the alternative.

This is a quick and dirty approach to hypothesis testing; but doing it this way we cannot calculate the likelihood of observing certain outcomes, i.e. the p-value.

### p-value

The p-value is defined as: $P\left(\text{observed or more extreme outcome}|H_0\text{ is true}\right)$

In the context of the example above, if we queried a sample of 50 students and got a mean of 3.2; and we find that the sample standard deviation is 1.74:

$$n = 50; \bar{x} = 3.2; s = 174 \to SE = 0.246$$

1. Our p-value is then defined as $P\left(\bar{x} > 3.2 | H_0: \mu = 3\right)$

2. Since we're assuming the null hypothesis to be true, we can construct the sampling distribution based on the CLT: $\bar{x} \sim N\left(\mu = 3, SE = 0.246\right)$

3. The z-score for a sample mean of 3.2 is then $Z = \frac{3.2 - 3}{0.246} = 0.81$ (this is also called a test statistic since we're going to be using it to calculate our p-value)

4. The p-value is then equal to $P(Z > 0.81) = 0.209$ (`pnorm` gives the area under the Cumulative Distribution Function to the left of the entered z-score)

If the population mean of exclusive relationships in college students is truly 3, then there is a 21% chance that a random sample of 50 college students would yield a sample mean of 3.2 or higher.

```r
1 - pnorm(0.81)
#> 0.208970087871602
```

**Decision based on the p-value**

We just used the z-score to calculate the p-value, which is the probability of observing data at least as favourable to the alternative hypothesis as our current dataset - given that the null hypothesis is true.

- If the p-value is low (lower than the *significance level*, $\alpha$) we say that it would be very unlikely to observe the data if the null hypothesis were true, and therefore reject $H_0$.

- If the p-value is high, then it is very likely to observe the data we currently have given that the null hypothesis were true, and we do not reject $H_0$.

Our p-value in the previous sample was higher than the standard significance level of 5%, and therefore we do not reject the null hypothesis. In the context of the example, that means that there is not enough evidence to reject the null hypothesis that sets the population average of number of exclusive relationships college students have been in to 3. This suggests that the sample mean of 3.2 that the original sample yielded is likely due to chance or sampling variability.

### Two-sided tests

Often, instead of looking for divergence from the null hypothesis in a specific direction, we're interested in divergence in any direction. Tests that look at this are called two-tailed.

The definition of the p-value is the same across oneand two-tailed tests; however the calculation is slightly different since we need to consider extremity on two sides of the distribution.

In the context of our previous example, the p-value would instead be equal to $P\left(\bar{x} > 3.2 \text{ OR } \bar{x} < 2.8| H_0: \mu = 3\right) = P(Z > 0.81) + P(Z < -0.81) = 0.209 \times 2 = 0.418$

### Recap

Recall that asserting that the shape of the sampling distribution is normal means that we're using the [[1 Central Limit Theorem and Sampling Distributions|CLT]]; so the same conditions for CLT will apply here.

![[Pasted image 20230711233136.png]]

## HT for the mean examples

#### Example 1

Researchers investigating characteristics of gifted children collected data from schools in a large city on a random sample of 36 children who were identified as gifted children soon after they reached the age of four. In this study, along with variables on the children, the researchers also collected data on their mothers' IQ scores. The histogram shows the distribution of these data, and also provided our some sample statistics.

![[Pasted image 20230711233151.png]]

We're asked to perform a hypothesis test to evaluate if these data provide convincing evidence of a difference between the average IQ score of mothers of gifted children and the average IQ score for the population at large, which happens to be 100. We're also asked to use a significance level of 0.01.

Our parameter of interest is the mean here, so we set $\mu = \text{average IQ score for mothers of gifted children}$.

1. Set the hypotheses: Our null hypothesis is that $H_0: \mu = 100$; and our alternative is $H_A: \mu \neq 100$

2. Calculate the point estimate: This is given to us as our sample mean ($\bar{x} = 118.2$)

3. Check the conditions:

- We have a random sample, and 36 < 10% of all gifted children; so we can assume independence (i.e. the IQ of one mother in this sample is independent of another one)

- We know that the sample size is &ge; 30, and the sample distribution appears non-skewed so we can expect a normally distributed sampling distribution

- Knowing this, we can assert that, if the null hypothesis is true, then $\bar{x} \sim N\left(100, \frac{6.5}{\sqrt{36}} \approx 1.083\right)$

1. Calculate the test statistic:

$$Z = \frac{118.2 - 100}{1.083} = 16.8 \to P(Z > 16.8) + P(Z < -16.8) \approx 0$$

1. Make a decision and interpret in context of the research question

The smaller the p-value, the stronger the evidence is against the null hypothesis. Since our p-value is smaller than the significance level, we can conclude that the data provides strong enough evidence that the average IQ of mothers of gifted children is different to the average IQ score for the population at large.

#### Example 2

A statistics student interested in sleep habits of domestic cats took a random sample of 144 cats and monitored their sleep. The cats slept an average of 16 hours per day. According to our online resources, domestic dogs actually sleep on average 14 hours a day. We want to find out if these data provide convincing evidence of different sleeping habits for domestic cats and dogs with respect to how much they sleep. Note that the test statistic calculated was 1.73.

We're told that our sample mean is 16 hours, and that dogs sleep 14 hours per day. For us, $\mu = \text{average hours cats sleep}$, and from the sample, $\bar{x} = 16$.

1. Set the hypothesis: $H_0: \mu = 14$; $H_A: \mu \neq 14$

2. Calculate the point estimate: $\bar{x} = 16$

3. Check the conditions: Here we assume the conditions are okay, or have already been checked

4. Calculate the test statistic:

$$Z = \frac{16 - 14}{\frac{s}{\sqrt{144}}} = 1.73 \to P(Z > 1.73) + P(Z < -1.73) = 0.0836$$

```r
pnorm(-1.73) * 2
#> 0.0836302752271899
```

1. What is the interpretation of this p-value in the context of the research question?

The probability of obtaining a random sample of 144 cats whose average house slept per day is greater than 16 or smaller than 12 - conditional on cats truly sleeping 14 hours per day on average - is 8.4%.