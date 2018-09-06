Hypothesis testing: when distribution is skewed --> use median instead of mean because mean is very sensitive to the extreme values.

# INTRO TO STATISTICS

## General notes/ Definitions

### Notes
- Percentage of data can add up to more than 100%: when observations can be in more than 1 group.
- Percentage of data can add up to less than 100%: when there are missing values.
- Only round the final result.

### Definitions
- Frequency vs. Relative Frequency: the latter is ratio
- Cumulative Frequency


## 1. Evaluation for statistics result

- SAMPLE:
  - Samples: must represent the population.
  - Self-selected samples: responses by those who choose to response.
  - Sample size: as large as possible, but not necessary in some cases.
  - Outdated sample: not represent population anymore.

- STUDY CREATOR:
  - Undue influence: collecting data or asking questions that influence answers.
  - Self-funded or self-interest studies: to back up their point.
  - Misleading use of data: incomplete data, lack of context, graphs.

- DATA:
  - Confounding variables: some cannot be captured.
  - Causality vs. Correlation.

## 2. Level of measurement

- Nominal scale: categorical, no order.
  - Ex: name of company, labels, colors
- Ordinal scale: categorical, with order.
- Interval scale: numerical, has definite order. But ratio does not have meaning (ex. temperature 4 times hotter 40 ~ 80 doesn't make sense).
- Ratio scale: numerical, ratio has meaning.

## 3.  Graphs for descriptive data

- Histogram:
  - y-axis: can either be frequency, or relative frequency -- will have same shape if use either way
- Frequency polygons: same as histogram, but use line chart instead of bar chart.
- Time series chart

## 4. Identify Outliers

- Interquartile range = Q3 - Q1
  - Outliers: < 1.5IQR below Q1, or > 1.5IQR above Q3.
- standard deviation (2, 2.5, 3 sd above or below the mean) 
- z-score

## 5. Skewness and Mean, Median, Mode

- If distribution is perfectly symmetrical, mean = median
- Left skewed: Outliers on the left - mean < median < mode
- Right skewed: Outliers on the right - mean > median > mode

- For ANY data set, no matter what the distribution of the data is:
  - At least 75% of the data is within two standard deviations of the mean.
  - At least 89% of the data is within three standard deviations of the mean.
  - At least 95% of the data is within 4.5 standard deviations of the mean.
  - This is known as Chebyshev's Rule.

- For data having a distribution that is BELL-SHAPED and SYMMETRIC:
  - Approximately 68% of the data is within one standard deviation of the mean.
  - Approximately 95% of the data is within two standard deviations of the mean.
  - More than 99% of the data is within three standard deviations of the mean.
  - This is known as the Empirical Rule.
  - It is important to note that this rule only applies when the shape of the distribution of the data is bell-shaped and symmetric.

## 11. Confidence Interval

> If repeated measurements over and over again, p% of the observed value would lie within the p% confidence interval.

## 6. Variance, standard deviation, covariance, Pearson Correlation Coefficient

- Variance: mean square error of each data point to the mean of the data
- Standard deviation: mean error of each data point to the mean of the data
- Covariance: how 2 variables correlate to each other. However, if we want to have a more general measure of correlation, without any unit --> use Pearson Correlation Coefficient (`= Covariance/(std of x)(std of y)`)

## 7. Different Distribution:

Discrete variables can be Binomial or Poisson distributed.

Continuous variables can be Normal or Exponential distributed.

### 7.1 Binomial distribution

Follow Bernoulli trials: success or failure (`p` vs `1-p`)

```
# Take 10,000 samples out of the binomial distribution: n_defaults
n_defaults = np.random.binomial(n = 100, p = 0.05, size = 10000)
```

### 7.2 Poisson Distribution

Follow Poisson process: if the next event happens independently from the previous one.

Ex: The number of `r` hits on a website in one hour with an average hit rate of 6 per hour is Poisson distributed.

- Poisson distribution is a limit of the Binomial distribution for rare events. (rare events are events that rarely happens)

> This makes sense if you think about the stories. Say we do a Bernoulli trial every minute for an hour, each with a success probability of 0.1. We would do 60 trials, and the number of successes is Binomially distributed, and we would expect to get about 6 successes. This is just like the Poisson story we discussed in the video, where we get on average 6 hits on a website per hour. So, the Poisson distribution with arrival rate equal to np approximates a Binomial distribution for n Bernoulli trials with probability p of success (with n large and p small).

> When we have rare events (low p, high n), the Binomial distribution is Poisson. This has a single parameter, the mean number of successes per time interval, in our case the mean number of no-hitters per season.

### 7.5 Normal Distribution
The standard deviation of this distribution, called the standard error of the mean, or SEM.

`sem = np.std(data) / np.sqrt(len(data))`

### 7.4 Exponential Distribution

The Exponential distribution describes the waiting times between rare events, or between Poisson process. (events that are independent from each other)

## 8. Linear Regression

2D visualization: Create a line that best tell the relationship between 2 variables.

Algorithm-wise: Create a formula that best calculate the y based on the x, minimizing residual sum of square --> Optimize Least Square.

## 9. Bootstrapping

With bootstrapping, we can see how distribution of a statistic is spread. This is a great method to estimate the range of  mean, median.

- Boostrap replicate: a single value of a statistic computed from a bootstrap process.

- For normal distribution, the standard deviation of the mean after bootstrapping process is approximately equal to the SEM of original data.

### 9.1 Pairs Bootstrapping

- Pairs bootstrap involves resampling pairs of data
- Instead of resampling the observation, resample index of observation (because 1 observation contains data from both variables, they need to go together).

## 10. Permutation

Permutation sampling is a great way to simulate the hypothesis that two variables have identical probability distributions.

A permutation sample of two arrays having respectively n1 and n2 entries.

- Step 1: Concatenate 2 arrays together.
- Step 2: Scramble the contents of the concatenated array.
- Step 3: Taking the first n1 entries as the permutation sample of the first array.
- Step 4: Taking last n2 entries as the permutation sample of the second array.
- Step 5: Perform hypothesis testing

## Hypothesis Testing

When we want to test whether 2 variables have similar distribution, or if they are different from each other.

- Depending on each question, we decide which **test statistic** we want to use: mean, median, etc.
- p-value: imagine we measure data over and over again, p-value is probability of getting values equal or larger than the extreme value.
  - Extreme value: example -- mean difference between 2 population from the original data.
  - If p-value is large, extreme value is no longer *extreme*. It is normal now.
  - Statistical significant is different from practical significant. If 2 population really have a difference in mean, but it is just a small difference and cost a lot to implement --> will not implement. Remember to compute the cost and benefit gained from each change. (only 5 customers click more but they pay a lot, would that revenue cover the cost?)

- 1 sample test: When we have a sample, and want to test the difference between that and **a value**.
- Two-sample test

### **Difference between permutation and bootstrapping for hypothesis test:**
- Besides only allow 2-sample test, permutation test if 2 population have similar mean and **come from the same distribution.**
- Bootstrapping is more versatile, testing if 2 population have the same mean, but not necessarily come from a same distribution.

If we have 2 samples, using permutation or bootstrapping makes sense. If we have 1 sample, using bootstrapping makes sense.
