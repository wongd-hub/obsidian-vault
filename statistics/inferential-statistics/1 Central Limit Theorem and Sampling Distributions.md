#coursera-inferential-stats #statistics

## Table of contents

1. [[#Introduction]]
2. [[#Sampling variability and Central Limit Theorem]]
	1. [[#Sampling distribution]]
	2. [[#The Central Limit Theorem]]
3. [[#CLT (for the mean), further examples]]

## Introduction

We start this course off with an example study by Pew Research which quotes:

- **41%** of the public believe that young adults, rather than middle-aged or older adults, are having the toughest time in our economy.
- Among all 18 to 34-year-olds, **49%** say they've taken a job they didn't want, just to pay the bills.

Further into the study, the margins of error are reported:

- Margin of error is plus or minus **2.9** percentage points for results based on the total sample at the **95%** confidence level.
- Margin of error is plus or minus **4.4** percentage points for adults aged 18-34 at the **95%** confidence level

This ultimately means that:

- We are 95% confident that 38.1% to 43.9% (41% ± 2.9%) of the public believe that young adults have the toughest time in this economy.
- We are 95% confident that 44.6% to 53.4% (49% ± 4.4%) of adults aged 18-34 have taken a job they didn't want, just to pay the bills.

The 41% and 49% both come from sample data. Querying the entire US population to come up with population estimates is clearly infeasible, so <u>we use sample statistics (such as the 41% and 49%) as point estimates of unknown population parameters of interest</u>.

The issue is that these sample statistics vary from sample to sample. Quantifying how they vary helps to provide a margin of error associated with the point estimate.

This unit will focus on sampling variability. Within this, we'll introduce the central limit theorem which describes shapes, centers and spreads of sampling distributions when certain conditions about the population, as well as the sampling scheme are met. After this, we'll move on to highlight techniques that are considered pillars of statistical inference, such as confidence intervals and hypothesis tests.

## Sampling variability and Central Limit Theorem
### Sampling distribution

Say have a population, and we take a few different samples from that population. Say we then calculate some sample statistic from each distribution.

![[Pasted image 20230711152456.png]]

Each sample is it's own 'sample distribution', which is made up of randomly sampled observations from the population. The sample statistics themselves make up the 'sampling distribution', which is no longer made up of single randomly sampled observations but rather summary statistics from each of those samples.

**Example**

Imagine we want to find the mean height of all women in the U.S.; $N$ = population size

- If we had the height of all women, we could simply calculate $\mu = \frac{x_1 + x_2 + \dots + x_n}{N}$. The standard deviation would then be $\sigma = \sqrt{\frac{\sum^N_{i=1}\left(x_i - \bar{x}\right)^2}{N}}$.

- Assuming we don't have this information, assume we sample 1,000 women from each state and calculate the mean from each sample

- AL: $x_{AL,1} + x_{AL,2} + x_{AL,1000} \to \bar{x}_{AL}$

- NC: $x_{NC,1} + x_{NC,2} + x_{NC,1000} \to \bar{x}_{NC}$, etc

- The mean of all $\bar{x}_{STATE}$ will be a close approximation to the population mean, $\mu$ ($mean(\bar{x}) \approx \mu$)

- However $SD(\bar{x}) \lt \sigma$; we would expect the average height from each state to be pretty close to one another

- The standard deviation of the sample means is called the *standard error*

- The mean of a random sample of 1,000 women is very unlikely to be very low or very high

- So as $n$ increases, we expect each sample to yield more consistent sample statistic estimates, and hence the standard error decreases

- The standard deviation of each sample should be roughly equal to the population sample deviation

**Another example**

See [here](https://gallery.shinyapps.io/CLT_mean/) for an interactive example of how the Central Limit Theorem works.

- We can see that increasing the sample size decreases the standard error

### The Central Limit Theorem

> [!info]+ Central Limit Theorem (CLT)
> The distribution of sample statistics is nearly normal, centred at the population mean, and with a standard deviation equal to the population standard deviation divided by the square root of the sample size. i.e. $\bar{x} \sim N\left(mean = \mu, SE = \frac{\sigma}{\sqrt{n}}\right)$; where $\sigma$ is the population standard deviation, however we can use a sample standard deviation, $s$ instead.

> [!tip] Obsidian Callouts
> **Note to self** - [more documentation on callouts in Obsidian](https://help.obsidian.md/Editing+and+formatting/Callouts)

- This is called the 'central' limit theorem, since it's central to much of statistical inference theory.
- The CLT tells us about the shape (normal), the center (the mean), and the spread of the sampling distribution (standard error).

> [!warning] Conditions for the CLT
> 1. **Independence**: Sampled observations must be independent; this is difficult to verify, but if the following conditions are met then this is more likely to be the case  
>     - Random sampling/assignment
>     - If sampling without replacement, n < 10% of the population. We've previously said that we like large samples, and now we're saying to curb their size. We'll discuss this in more detail later.
> 2. **Sample size/skew**: Either the population distribution is normal, or if the population distribution is skewed, the sample size is large (e.g. n > 3%). The more skewed the population, the larger we need the sample size to be.
>     - It's hard to verify that your population distribution is normal, but if you plot a sample distribution and it looks approximately normal, you can start to gain confidence that your population is nearly normal as well.

Focusing in on the 10% condition: 'If sampling without replacement, n < 10% of the population.'

- Say you live in a very small town, the population is only 1,000 people; and your family lives here as well.
    
- Say a genetic research wants to take random samples from your town.
    - If we want to take a sample of 10 out of 1,000; the likelihood that your family will be in this sample as well as you is low
    - If we instead take a sample of 100 out of 1,000; the likelihood that your family will be in this sample as well as you will be higher

- So while a larger sample is better, we also want to keep the size of our sample proportional to our population.
    
- When we're sampling without replacement (which isn't something we do in survey settings since you don't want to survey someone twice), the probability that we'll get both you and your family in a sample is static, which is why the 10% rule matters less there.

Focusing in on the sample size/skew condition

- A smaller sample size leads to a sampling distribution that mimics the population distribution; which isn't what we want if the population distribution is skewed.

![[Pasted image 20230711153848.png]]

- However, a larger sample size starts to approximate a normal distribution

![[Pasted image 20230711153902.png]]

Why are we obsessed with having a normal distribution? Because once we have a normal sampling distribution, we can start calculating probabilities, [[2 Confidence Intervals|confidence intervals]], and p-values for [[3 Hypothesis Testing|hypothesis tests]].

**Example**

See [here](https://gallery.shinyapps.io/CLT_mean/); and pick a skewed or uniform distribution for an interactive example of this works.

- We can see that increasing the sample size increases the likelihood that the sampling distribution will be approximately normal.

## CLT (for the mean), further examples

### Example 1

Suppose my iPod has 3,000 songs; the distribution of song lengths is shown below.

- We also know that the mean length of songs on this iPod is 3.45 minutes, and the standard deviation is 1.63 minutes.
- Calculate the probability that a randomly selected song lasts more than 5 minutes.

![[Pasted image 20230711153925.png]]

- How we usually approach these questions is to calculate a z-score and then find the associated probability.
- However, this method can only be used when the distribution we're looking at is normal; and this is not the case for this example - our distribution is right-skewed.
- What we *can* do here is use the histogram and the height of the bars to estimate what percentage of songs fall between 4-5 minutes, 5-6 minutes, etc.
- Eyeballing these bars, here's what we get:

![[Pasted image 20230711153937.png]]

### Example 2

I'm going on a 6 hour roadtrip, and I make a playlist of 100 songs. What is the probability that my playlist lasts the entire drive?

![[Pasted image 20230711153948.png]]

- We haven't worked with sums of probabilities yet, but this is equivalent to the average song being at least 3.6 minutes (360 minutes / 100 songs).

- Given we're working with the sample mean ($\bar{x}$), we can use the Central Limit Theorem to figure out how the sample mean is distributed.

- The CLT says $\bar{x}$ will be distributed normally with mean equal to the population mean ($\mu =$ 3.45 minutes), and standard error equal to the population standard deviation divided by the square root of the sample size ($SE = \frac{\sigma}{\sqrt{n}} = \frac{1.63}{\sqrt{100}} = 0.163$); i.e. $\bar{x} \sim N\left(3.45, 0.165)\right)$

- Having all this information on how $\bar{x}$ is distributed should prompt you to draw a curve to properly visualise what's going on.

- To calculate probabilities over a certain range of the distribution, we'll use a Z-score. Note we divide by the standard error - not the population standard deviation, since we're looking at the distribution of the mean song length; not looking at a single song.
  
$$Z = \frac{\bar{x} - \mu}{SE} = \frac{3.6 - 3.45}{0.163} = 0.92$$

- Using a standard normal distribution ($N\left(0, 1\right)$), we can find $P(Z > 0.92)$, which is 0.179. This is the probability that the playlist will last the entire roadtrip.

![[Pasted image 20230711154025.png]]

### Example 3

![[Pasted image 20230711154034.png]]

- 2 -> B, since a sample from a population should yield roughly the same distribution as the parent population.
- 3 -> A, since samples from non-normal distributions with smaller sizes will approach but not reach normality.
- 4 -> C, since larger samples from non-normal distributions will follow the CLT and be normal.