---
title: Unadjusted Odds Ratios are Conditional
author: Frank Harrell
date: '2020-09-13'
slug: marg
tags:
  - 2020
  - generalizability
  - RCT
  - regression
link-citations: yes
summary: 'This article discusses issues with unadjusted effect ratios such as odds ratios and hazard ratios, showing a simple example of non-generalizability of unadjusted odds ratios.'
header:
  caption: ''
  image: ''
---




# Background 

Effect ratios such as odds ratios (OR) and hazard ratios (HR) are useful measures of relative treatment effects and are used extensively in randomized clinical trials (RCT).  In their simplest form where they represent a non-covariate-adjusted treatment effect, they were designed for homogeneous patient populations, i.e., situations in which there are no known risk factors.  They were not designed for the case where there is substantial outcome heterogeneity even for patients receiving the same treatment.  When this outcome heterogeneity is present (e.g., strong risk factors exist), patients come from a mixture of distributions, and this mixing causes problems for many statistics that are not linear in the outcome data.  In the Cox proportional hazards (PH) model this mixing causes non-proportional hazards of treatments and in both Cox and logistic regression models the mixing causes an attenuation of the treatment effect.   This is explained in detail in Chapter 13 of [Biostatistics in Biomedical Research](https://hbiostat.org/doc/bbr.pdf).

In a linear model applied to an RCT, failure to adjust for covariates, or omitting a strong risk factor from the set of adjustment variables, results in residuals having larger absolute values, which increases the residual variance.  The residual variance absorbs such unaccounted heterogeneity in the outcome, and increasing this variance decreases the power of the study.  In Cox and logistic models there are no residual terms, and unaccounted outcome heterogeneity has nowhere to go.  So it goes into the regression coefficients that _are_ in the model, attenuating them towards zero.  Failure to adjust for easily accountable outcome heterogeneity in nonlinear models causes a loss of power as a result.  As detailed in Chapter 13 of the above referenced text, adjusting for outcome heterogeneity keeps Cox and logistic models from losing power, unlike the situation in linear models where adjusting actually increases power.  The important impact of covariate adjustment on power for making treatment comparisons has been studied extensively by [Steyerberg](/post/covadj).

The underlying issue is the non-collapsibility of ORs and HRs.  Non-collapsibility means that the conditional ratio is different from the marginal (unadjusted) ratio even in the complete absence of confounding (as in our example dataset below).  By the way, don't make the mistake of concluding that non-collapsibility is undesirable.  Any measure that has the potential for summarizing a treatment effect with one constant for all types of patients will be non-collapsible when the outcome is categorical or represents time to event^[An exception is a log-normal survival model which is a linear model in log(Y).].  Collapsible measures such as absolute risk reduction and relative risk reduction must vary over risk factors (creating mathematical but not subject-matter-relevant interactions), otherwise probabilities will arise that are outside the allowable range of [0,1].  Log odds and log hazard ratios have an unlimited ranges and can possibly apply to everyone.  This makes them good bases for studying heterogeneity of treatment effect.

When risk factors exist and a clinical trial enrolls patients having a variety of values of these risk factors, there will be outcome heterogeneity.  To easily account for known outcome heterogeneity, it is a good idea to pre-specify known-to-be-important covariates for the primary analysis of an RCT.  Failure to do so will result in a significant power loss.

I frequently hear criticism of adjusted effect ratios to the effect "How do you know your model is correct?  How do you know you haven't omitted important covariates?".  Fortunately, covariate adjustment usually performs well even if the model is not correct.  If the model captures a large proportion of the explainable outcome heterogeneity, covariate adjustment works extremely well and is robust to model misspecification.  And any covariate adjustment will improve estimation and power over unadjusted marginal treatment effects.  Conditioning does not have to be perfect.  One can think of an incompletely adjusted treatment effect as an approximation of a more completely adjusted one.  As an example, suppose that patient age is a major risk factor, and that one adjusted for age but not for five other risk factors.  The resulting adjusted treatment effect compares outlines for like vs. like with respect to age, but averages over unadjusted risk factors.  Estimating how an x year old patient on treatment A will fare in comparison to an x year old patient on treatment B, i.e., fully utilizing information about age, is better than not accounting for age.  In my experience, clinical investigators are quite accurate in choosing prognostic factors for adjustment.  They seldom miss one that explains a large proportion of patient-to-patient variability in outcomes.


# Odds Ratio Example

Beginning with a slight modification of an example provided by Mitch Gail presented in the ANCOVA chapter of BBR, consider a 4000 patient randomized clinical trial with one important covariate: patient sex.  This prognostic variable has perfect balance over treatments A and B, and has an odds ratio of 9 for the female:male effect within either treatment group.  The A:B treatment odds ratio is also 9, for either sex.  The data are shown below.

| Males | A | B | Total |  |
| :------- | ---: | ---: | ------: | --: |
|Y=0    | 500 | 900 | 1400 | |
|Y=1    | 500 | 100 |  600 | |
|Total  | 1000| 1000| 2000 | OR=9 |

<br>

|Females|A|B|Total|  |
| :--------- | ---: | ---: | -------: | --: |
|Y=0      | 100| 500| 600| |
|Y=1      | 900| 500|1400| |
|Total    |1000|1000|2000| OR=9 |

<br>

| Pooled | A | B | Total | |
| :-------- | ---: | ---: | -------: | --: |
|Y=0     |600 | 1400 | 2000 | |
|Y=1     | 1400 | 600 | 2000 | |
|Total   | 2000 | 2000 | 4000 | OR=5.44 |

By fitting a binary logistic regression model we see there is no sex x treatment interaction on the unrestricted log odds scale, and the adjusted odds ratios for both treatment and sex are 9.


```r
d <- expand.grid(y=0:1, tx=c('A','B'),sex=c('male', 'female'))
# Order treatment levels to get OR > 1
d$tx <- factor(d$tx, c('B','A'))
d$freq <- c(500, 500, 900, 100, 100, 900, 500, 500)
kable(d)
```



|  y|tx |sex    | freq|
|--:|:--|:------|----:|
|  0|A  |male   |  500|
|  1|A  |male   |  500|
|  0|B  |male   |  900|
|  1|B  |male   |  100|
|  0|A  |female |  100|
|  1|A  |female |  900|
|  0|B  |female |  500|
|  1|B  |female |  500|


```r
f <- lrm(y ~ tx * sex, weights=freq, data=d)
```

```r
f
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * sex, data = d, weights = freq)
 </pre>
 
 
 Sum of Weights by Response Category
 
 <pre>
    0    1 
 2000 2000 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 8</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 1472.26</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.411</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.800</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 4</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.883</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.600</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 4</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 6.575</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.851</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Sum of weights 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.343</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.300</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-9</sup></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.170</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -2.1972</td>
<td style='min-width: 7em; text-align: right;'> 0.1054</td>
<td style='min-width: 7em; text-align: right;'>-20.84</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=A</td>
<td style='min-width: 7em; text-align: right;'>  2.1972</td>
<td style='min-width: 7em; text-align: right;'> 0.1229</td>
<td style='min-width: 7em; text-align: right;'> 17.87</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>sex=female</td>
<td style='min-width: 7em; text-align: right;'>  2.1972</td>
<td style='min-width: 7em; text-align: right;'> 0.1229</td>
<td style='min-width: 7em; text-align: right;'> 17.87</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=A × sex=female</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0000</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.1738</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.00</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>1.0000</td>
</tr>
</tbody>
</table>

```r
# exp(coef(f)['tx=A']) = 9
```

The A:B odds ratio not conditioning on patient sex is 5.44.  Note that this is not a weighted average of 9 and 9.  The unadjusted OR applies neither to males nor females.  

A subtle issue is that pooled marginal estimates (OR=5.44 above) are difficult to interpret and do not generalize to clinical populations with covariate distributions (here, sex) that differ from the RCT sample composition.  As an example, let's take a probability sample of females from our 4000 patient study and see how the A:B treatment OR changes.  Compare the result from the whole RCT sample to the result after keeping the following proportions of females: 0.8, 0.6, 0.4, 0.2, 0.1, 0.05.


```r
fraction.females <- c(1, .8, .6, .4, .2, .1, 0.05)
OR <- numeric(7)
j <- 0
for(frac in fraction.females) {
    j <- j + 1
    s <- d
    i <- d$sex == 'female'
    s$freq[i] <- s$freq[i] * frac
    # Compute marginal OR
    g <- lrm(y ~ tx, weights=freq, data=s)
    OR[j] <- round(exp(coef(g)[2]), 2)
}
```


```r
cbind(fraction.females, OR)
```

```
     fraction.females   OR
[1,]             1.00 5.44
[2,]             0.80 5.47
[3,]             0.60 5.57
[4,]             0.40 5.84
[5,]             0.20 6.54
[6,]             0.10 7.33
[7,]             0.05 7.99
```

The marginal OR depends on the distribution of the sex variable in the sample, and does not transport to populations with a different sex ratio than the trial enrollment achieved.  It is conditional (adjusted) ORs that generalize to other populations.  These calculations illustrate that the sex-conditional OR equals the marginal OR only if the distribution is altered so that the conditioning doesn't matter (e.g., all the males or all the females are excluded).  But what is the exact interpretation of the original marginal OR of 5.44 since it involves hidden conditioning on a 1:1 sex ratio in our example?  A definition for this example is that 5.44 is the unconditional OR _only when there are equal numbers of males and females_ because that's how the sample was constituted.  But what is the interpretation when one wants to apply the RCT results to individual patients?  It would seem to apply only to those rare situations where the patient is being counseled but for some reason we don't know the patient's sex^[Note that the example is oversimplified because we are using an out-of-date binary conceptualization of sex.].  The marginal estimate needs the physician to conceptualize the clinical population (or at least the sex ratio) from which the patient came since it does not want to take into account the patient's actual sex.

Mitch Gail summarized this situation well as quoted by [Hauck, Anderson, and Marcus](https://www.sciencedirect.com/science/article/abs/pii/S0197245697001475):

> For use in a clinician--patient context, there is only a single person, that patient, of interest.  The subject-specific measure then best reflects the risks or benefits for that patient.  Gail has noted this previously [ENAR Presidential Invited Address, April 1990], arguing that one goal of a clinical trial ought to be to predict the direction and size of a treatment benefit for a patient with specific covariate values.  In contrast, population--averaged estimates of treatment effect compare outcomes in groups of patients.  The groups being compared are determined by whatever covariates are included in the model. The treatment effect is then a comparison of average outcomes, where the averaging is over all omitted covariates.

