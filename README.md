## Experiment Design

At the time of this experiment, Udacity courses currently have two options on the home page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student
might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. [This screenshot][&screenshot] shows what the experiment looks like.

<img class="displayed" src="assets/images/screenshot.png" width="600px" height="auto">

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the
course.

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

### Metric Choice ###

Given a set of possible metrics, we first choose which ones to measure for this experiment. Each mertic is either rejected or chosen as an invariant metric or as an evaluation metric. The magnitude of practical significance boundary for each metric, that is, the difference that would have to be observed before that was a meaningful change for the business, is given in parentheses. All practical significance magnitudes are given as absolute changes.

Any place "unique cookies" are mentioned, the uniqueness is determined
by day. (That is, the same cookie visiting on different days would be
counted twice.) User-ids are automatically unique since the site does
not allow the same user-id to enroll twice.

#### Chosing Invariant Metrics

Since the following set of metrics measure quantities prior to the
screener, they can all be considered as invariant metrics between the
control and experiment groups:

* Number of cookies: That is, number of unique cookies to view the
  course overview page. (d<sub>min</sub>=3000)

* Number of clicks: That is, number of unique cookies to click the
  "Start free trial" button (which happens before the free trial
  screener is trigger). (d<sub>min</sub>=240)

* Click-through-probability: That is, number of unique cookies to
  click the "Start free trial" button divided by number of unique
  cookies to view the course overview page. (d<sub>min</sub>=0.01)

#### Chosing Evaluation Metrics

The hypothesis indicates that the screener reduces the number of users
that actually enroll in the free trial, without significantly reducing
the number of users that continue past the free trial.

If the screener reduces free-trial enrollment, both the following two metrics, gross conversion and retention, are expected to produce
significantly different values. While the value of the gross conversion should decrease, the value of retention, should increase. For each metric, the practical-significance margin, d<sub>min</sub> is indicated in parantheses. 

* Gross conversion: That is, number of user-ids to complete checkout
  and enroll in the free trial divided by number of unique cookies to
  click the "Start free trial" button. (d<sub>min</sub>= 0.01)

* Retention: That is, number of user-ids to remain enrolled past the
  14-day boundary (and thus make at least one payment) divided by
  number of user-ids to complete checkout. (d<sub>min</sub>=0.01)

Although both metrics measure the same effect, we would want to eventually settle on a single metric because composite metrics are tricky and hard to agree on. While gross conversion uses "cookies to click" as unit of diversion, retention uses "users to enroll", which is a much larger unit. Therefore, to detect a minimum effect (d<sub>min</sub>) of 0.1, retention metric will probably require more statistical power than gross conversion. We will know more about this in later sections after we have performed some more calculations.

Since we want to detect the same amount of significant effect (d<sub>min</sub>) for both metrics, However, instea
A separate metric is needed to test that the screener does not
significantly reduce the number of users that continue past the free
trial and pay. The following metric is expected to produce similar
values within bounds of statistical variability.

* Net conversion: That is, number of user-ids to remain enrolled past
  the 14-day boundary (and thus make at least one payment) divided by
  the number of unique cookies to click the "Start free trial"
  button. (d<sub>min</sub>= 0.0075)

Note that even though the following metric is affected by the screener we chose not to include it for reasons explained below.

* Number of user-ids: That is, number of users who enroll in the free
  trial. (d<sub>min</sub>=50)

Since the experiment uses number of cookies as unit of diversion it is
hard to ensure that the experiment and control groups contain similar
number of users. Therefore, the above metric is not well defined; even
if enrollment goes down, we cannot tell if the difference is caused
due to the screener or due to a smaller number of users in the
experiment group.


### Measuring Standard Deviation

The previous section chose the invariant and evaluation metrics for the
experiment. Now we make sure that the evaluation metrics do not have too much
variability to be appropriate for the experiment. Larger variability requires For this we make analytical estimates of the standard deviation from a sample of the following historical data collected prior to experiment.

| Metric | Value |
|--------|---------|
| Unique cookies to view page per day | 40000 |
| Unique cookies to click "Start Free Trial" button per day | 3200 |
| Enrollments per day | 660 |
| Click Through Probability (CTP) on "Start Free Trial" | 0.08 |
| Probability of enrolling, given click (Gross Conversion) | 0.20626 |
| Probability of payment, given enroll (Retention) | 0.53 |
| Probability of payment, given click (Net Conversion) | 0.1093125 |

From a smaple size of 5000 cookies visiting the course overview page, we compute the estimated probability p<sup>^</sup>, and then use it to compute the Standard Error (SE), which is an estimate the Standard Deviation [&stddevrblog], using the following formula

<img class="displayed" src="assets/images/stddev_formula.png" width="125px" height="auto">

In the above formula the sample size N is measured in units of analysis which is different for different metrics: 

1. For gross and net conversion, unit of analysis is the number of pageviews, and there are 400 (3200 x 5000 / 40000) units in the sample. For N=5000, the standard deviations of gross conversion and net conversion are 0.0202 and 0.156 respectively. 
2. For retention, unit of analysis is the number of enrollments, and there are 82.5 (660 x 5000 / 40000) units in the sample. For N=82.5, the standard deviation of retention is 0.53.

The analytical estimates of variance presented above assume that the underlying data for each metric follow Binomial Distribution. This assumption holds because all the evaluation metrics measure success probabilities of two-outome events, where events are independent and have identical probability distributions. Additionally the sample size and probabilities in each case also satisfy the Normal approximation condition: Np<sup>^</sup> > 5 and N(1-p<sup>^</sup>)>5.  

Anthough, under ideal conditions, analytical variability estimates of gross conversion and retention metrics are valid, they can significantly underestimate the empirical variance when the experiment system is less than ideal and has issues regarding randomization, bias, or population effects etc. In such cases, it is safer to perform A vs A experiments as a sanity check for the experiment system. A vs A experiments require more effort that involves either hundreds of A vs A tests,  or a really large A vs A test, or the use of bootstrapping techniques etc.

For more complicated metrics such a median or ratios, the distributions cannot be assumed to be Binomial. In such cases analytical estimates vastly underestimate empirical variability, and should not be used.


### Sizing

#### Number of Samples vs. Power

Even if the changes in the evaluation metrics are found to be substantive as per their magnitudes of practical significance d<sub>min</sub>, they are not repeatable unless they are statistically significant. This section discusses how to determine the sample size, the number of pageviews in the control and the experiment groups, needed to achieve a statistically significant change in each metric.

To be statistcally significant, changes need to safeguard against two types of variability effects:

1. they are detected even though they didn't not actually exist; this is measured with *confidence-level* 1-&alpha;, where &alpha; is the probability of false positives.

2. they were not detected even though they did exist; this is measured with *sensitivity* 1-&beta;, where &beta; is the probabilty of false negatives.

The sample size must be large enough to provide the statistical power so that each metric has adequate confidence level and sensitivity. The required sample size goes up with decreasing values of &alpha;, &beta;, and d<sub>min</sub>, and goes down with  baseline probability moving farther away from 0.5.

For an &alpha; of 0.05 and a &beta; of 0.2, the sample size for enough statistical power can be determined as follows.

1. Use this [online sample size calculator][&onlinecalc] to get the sample size of one group in the unit of analysis of the corresponding metric.

2. Since we need two samples, one for control and one for experiment, multiply the result with 2

3. Express all the quantities in pageviews using the following rule.
  * clicks to pageviews: multiply by 40000/3200
  * enrolls to pageviews: mulitply by 40000/660

4. Take the maximum numer of pageviews over all the metrics.

Combining multiple metrics increases the probability of false positives, because any metric producing a false positive causes the combined metric to produce a false positive. A coservative approach is to apply the Bonferroni correction (divide &alpha; by the number of metrics) while sizing. However, since the metrics are highly correlated, the Bonferroni method would have been too conservative. Hence we avoided the Bobferroni correction.


| Metric | Baseline | d<sub>min</sub>|Size in Unit of Analysis | Size in Unit of diversion |
|--------|---------|-----------------|---------------------------|---------------------------|
| Probability of enrolling, given click (Gross Conversion) | 0.20626 | 1% | 51670 | 645845 |
| Probability of payment, given enroll (Retention) | 0.53 | 1% | 78230 | 4741212 |
| Probability of payment, given click (Net Conversion) | 0.1093125 | 0.75% | 54826 | 685325 |

Since the retention metric uses number of enrollments as units of analysis, its sample size really grows up after converting it to pageviews. This makes retention a little more challenging as a metric, because it will need a fair amount of user data to make that work.  At this point it is just not really realistic.
It's going to take a very long time to get that much data, it's a big investment. We would rather retention from our list of retention metrics, because we will still have two other metrics to work with.

After dropping retention, the take the largest of the pageview numbers of the remaining metrics, 685325, and the number of pageviews needed for our experiment.

#### Duration vs. Exposure

Assuming there are no other experiments going on, it is safe to divert the entire traffic to this experiment becasue the experiment does not collect any private data, and does not leave any permanent footprint. Hence we chose to divert 100% of the traffic to this experiment. 

At the rate of 40000 page view per day, it will require 18 days to collect 685325 page views from 100% of the traffic. This is a reasonable duration for an experiment without much risk to Udacity.

## Experiment Analysis

The data for you to analyze is here. This data contains the raw
information needed to compute the above metrics, broken down day by
day. Note that there are two sheets within the spreadsheet - one for
the experiment group, and one for the control group.

The meaning of each column is:

Pageviews: Number of unique cookies to view the course overview page that day.

Clicks: Number of unique cookies to click the course overview page that day.

Enrollments: Number of user-ids to enroll in the free trial that day.

Payments: Number of user-ids who who enrolled on that day to remain
enrolled for 14 days and thus make a payment. (Note that the date for
this column is the start date, that is, the date of enrollment, rather
than the date of the payment. The payment happened 14 days
later. Because of this, the enrollments and payments are tracked for
14 fewer days than the other columns.)

### Sanity Checks

Before looking at click-through probabilities to determine what happened in the experiment, we have to do some sanity checks to ensure that the experiment was run properly.

We start by checking whether the invariant metrics are equivalent
between the two groups. The first two metrics, number of cookies and number of clicks, are populatin sizing metrics that
should be randomly split between the 2 groups, so we can use a Binomial
test as demonstrated in Lesson 5. For the third invariant metric, click through probability, needs a confidence interval for a difference in proportions using
a similar strategy as in Lesson 1, then check whether the difference
between group values falls within that confidence level.  If your
sanity checks fail, look at the day by day data and see if you can
offer any insight into what is causing the problem.

For each of your invariant metrics, give the 95% confidence interval
for the value you expect to observe, the actual observed value, and
whether the metric passes your sanity check. (These should be the
answers from the "Sanity Checks" quiz.)

For any sanity check that did not pass, explain your best guess as to
what went wrong based on the day-by-day data. Do not proceed to the
rest of the analysis unless all sanity checks pass.

| Metric | Observed | Lower Bound | Upper Bound | Passes|
|--------|----------|-------------|-------------|-------|
| Unique cookies to view page per day | 0.5006 | 0.4988 | 0.5011 | Yes |
| Unique cookies to click "Start Free Trial" button per day | 0.5005 | 0.49588 | 0.50412 | Yes |
| Click Through Probability (CTP) on "Start Free Trial" | 0.0001 | -0.0013 | 0.0013 | Yes|


### Result Analysis

#### Effect Size Tests

Next, for your evaluation metrics, calculate a confidence interval for
the difference between the experiment and control groups, and check
whether each metric is statistically and/or practically
significance. A metric is statistically significant if the confidence
interval does not include 0 (that is, you can be confident there was a
change), and it is practically significant if the confidence interval
does not include the practical significance boundary (that is, you can
be confident there is a change that matters to the business.)

If you have chosen multiple evaluation metrics, you will need to
decide whether to use the Bonferroni correction. When deciding, keep
in mind the results you are looking for in order to launch the
experiment. Will the fact that you have multiple metrics make those
results more likely to occur by chance than the alpha level of 0.05?

For each of your evaluation metrics, give a 95% confidence interval
around the difference between the experiment and control
groups. Indicate whether each metric is statistically and practically
significant. (These should be the answers from the "Effect Size Tests"
quiz.)

| Metric | Lower Bound | Upper Bound | 
|--------|--------------|-------------|
| Probability of enrolling, given click (Gross Conversion) | -0.0291 | -0.0120 |
| Probability of payment, given click (Net Conversion) | -0.0116 | 0.0119 | 


#### Sign Tests

For each evaluation metric, do a sign test using the day-by-day
breakdown. If the sign test does not agree with the confidence
interval for the difference, see if you can figure out why.

For each of your evaluation metrics, do a sign test using the
day-by-day data, and report the p-value of the sign test and whether
the result is statistically significant. (These should be the answers
from the "Sign Tests" quiz.)

| Metric | p-value | Statistically Signifiant | 
|--------|--------------|-------------|
| Probability of enrolling, given click (Gross Conversion) | 0.0026 | Yes |
| Probability of payment, given click (Net Conversion) | 0.6776 | No | 


#### Summary

State whether you used the Bonferroni correction, and explain why or
why not. If there are any discrepancies between the effect size
hypothesis tests and the sign tests, describe the discrepancy and why
you think it arose.

### Recommendation

Finally, make a recommendation. Would you launch this experiment, not
launch it, dig deeper, run a follow-up experiment, or is it a judgment
call? If you would dig deeper, explain what area you would
investigate. If you would run follow-up experiments, briefIy describe
that experiment. If it is a judgment call, explain what factors would
be relevant to the decision.

Make a recommendation and briefly describe your reasoning.

## Follow-Up Experiment

If you wanted to reduce the number of frustrated students who cancel
early in the course, what experiment would you try? Give a brief
description of the change you would make, what your hypothesis would
be about the effect of the change, what metrics you would want to
measure, and what unit of diversion you would use. Include an
explanation of each of your choices.

Give a high-level description of the follow up experiment you would
run, what your hypothesis would be, what metrics you would want to
measure, what your unit of diversion would be, and your reasoning for
these choices.

### References

[\[gitRepo\] Analyze New York City Subway Ridership][&gitRepo]

[\[onlinecalc\] How many subjects are needed for an A/B test ?][&onlinecalc]

[\[stddevrblog\]: Standard Deviation vs Standard Error] [&stddevrblog]

[\[mta050711\] MTA Turnstile Data, May 2011] [&mta050711] 

[\[weatherData\] Underground Weather Data] [&weatherData]

[\[combinedData\] Combined MTA and underground weather data.] [&combinedData]

[\[scipyMH\] scipy.stats.mannwhitneyu] [&scipyMH]

[\[udacityMH\] Understanding the Mann-Whitney U Test] [&udacityMH]

[\[tailTests\] scipy.stats.mannwhitneyu] [&tailTests]

[\[tailed12\] One-tail vs. two-tail P values, GraphPad Software] [&tailed12]

[\[paired\] Unpaired and paired two-sample t-tests] [&paired]

[\[olsWiki\] Ordinary least squares, Wikipedia] [&olsWiki]

[\[olsStats\] Statsmodels regression OLS] [&olsStats]

[\[nistEstats\] NIST Engineering Statistics Handbook] [&nistEstats]

[\[normTest\] Interpreting results: Normality tests, GraphPad Software] [&normTest]

[\[rnauRs\] What's a good value for R-squared ?] [&rnauRs]


[&gitRepo]: https://github.com/samitchaudhuri/abtest-screener "A/B Test Free Trial Screener"
[&screenshot]: https://drive.google.com/file/d/0ByAfiG8HpNUMakVrS0s4cGN2TjQ/view ""
[&onlinecalc]: http://www.evanmiller.org/ab-testing/sample-size.html "How many subjects are needed for an A/B test ?"

[&stddevrblog]: https://www.r-bloggers.com/standard-deviation-vs-standard-error/ "Standard Deviation vs Standard Error"

[&mta050711]: http://web.mta.info/developers/data/nyct/turnstile/turnstile_110507.txt "MTA Turnstile Data, May 2011"
[&weatherData]: https://www.dropbox.com/s/7sf0yqc9ykpq3w8/weather_underground.csv "Underground Weather Data"
[&combinedData]: https://www.dropbox.com/s/meyki2wl9xfa7yk/turnstile_data_master_with_weather.csv "Combined MTA and underground weather data."
[&scipyMH]: http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mannwhitneyu.html "sipy.stats.mannwhitneyu"
[&udacityMH]: https://storage.googleapis.com/supplemental_media/udacityu/649959144/MannWhitneyUTest.pdf "Understanding the Mann-Whitney U Test"
[&tailTests]: http://www.ats.ucla.edu/stat/mult_pkg/faq/general/tail_tests.htm "What are the differences between one-tailed and two-tailed tests ?"
[&tailed12]: http://graphpad.com/guides/prism/6/statistics/index.htm?one-tail_vs__two-tail_p_values.htm "One-tail vs. two-tail P values, GraphPad Software"
[&paired]: https://en.wikipedia.org/wiki/Student%27s_t-test#Unpaired_and_paired_two-sample_t-tests "Unpaired and paired two-sample t-tests"
[&nistEstats]: http://www.itl.nist.gov/div898/handbook/pri/section2/pri24.htm "Are the model residuals well behaved?, NIST Engineering Statistics Handbook"
[&normTest]: http://www.graphpad.com/guides/prism/6/statistics/index.htm?stat_interpreting_results_normality.htm "Interpreting results: Normality tests, GraphPad Software"
[&olsWiki]: http://en.wikipedia.org/wiki/Ordinary_least_squares "Ordinary least squares, Wikipedia"
[&olsStats]: http://statsmodels.sourceforge.net/devel/generated/statsmodels.regression.linear_model.OLS.html "Statsmodels regression OLS"
[&rnauRs]: http://people.duke.edu/~rnau/rsquared.htm#punchline "What's a good value for R-squared ?"



