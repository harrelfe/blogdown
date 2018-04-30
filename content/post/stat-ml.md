---
title: Road Map for Choosing Between Statistical Modeling and Machine Learning
author: Frank Harrell
date: '2018-04-30'
updated: ''
slug: stat-ml
categories: []
tags:
  - data-science
  - machine-learning
  - prediction
summary: 'This article provides general guidance to help researchers choose between machine learning and statistical modeling for a prediction project.'
header:
  caption: ''
  image: ''
---
Statistical models (SMs) include ordinary regression, Bayesian regression, semiparametric models, generalized additive models, longitudinal models, time-to-event models, penalized regression, and others.  Penalized regression includes ridge regression, lasso, and elastic net.  Contrary to what some machine learning (ML) researchers believe, SMs easily allow for complexity (nonlinearity and second-order interactions) and an unlimited number of candidate features (if penalized maximum likelihood estimation or Bayesian models are used).  It is especially easy, using regression splines, to allow every continuous predictor to have a smooth nonlinear effect.

ML is taken to mean an algorithmic approach that does not use traditional identified statistical parameters, and for which a preconceived structure is not imposed on the relationships between predictors and outcomes.  ML usually does not attempt to isolate the effect of any single variable.  ML includes random forests, recursive partitioning (CART), bagging, boosting, support vector machines, neural networks, and deep learning.

ML and AI have had their greatest successes in high signal:noise situations, e.g., visual and sound recognition, language translation, and playing games with concrete rules.  What distinguishes these is quick feedback while training, and availability of **the** answer.  Things are different in the low signal:noise world of medical diagnosis and human outcomes.  A great use of ML is in pattern recognition to mimic radiologists' expert image interpretations.  For estimating the probability of a positive biopsy given symptoms, signs, risk factors, and demographics, not so much.


There are many published comparisons of predictive performance of SM and ML.  In many of the comparisons, only naive regression methods are used (e.g., everything is assumed to operate linearly), so the SM comparator is nothing but a straw man.  And not surprisingly, ML wins.  The reverse also happens, where the ML comparator algorithm uses poorly-chosen default parameters or the particular ML methods chosen for comparison are out of date.  As a side note, when the SM method is just a straw man, the outcry from the statistical community is relatively muted compared with the outcry from ML advocates when the "latest and greatest" ML algorithm was not used in the comparison with SMs.  ML seems to require more tweaking than SMs.  But SMs often require a time-consuming data reduction step (unsupervised learning) when the number of candidate predictors is very large and penalization (lasso or otherwise) is not desired.

Note that there are ML algorithms that provide superior [predictive discrimination](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3575184) but that pay insufficient attention to [calibration](http://fharrell.com/post/medml) (absolute accuracy).

Because SMs favor additivity as a default assumption, when additive effects dominate, SM requires far lower sample sizes (typically 20 events per candidate predictor) than ML, which typically requires [200 events](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-14-137) per candidate predictor.  Thus ML can sometimes create a demand for "big data" when small-moderate sized datasets will do.  I sometimes dislike ML solutions for particular medical problems because of ML's **lack** of assumptions.  But SMs are not very good at reliably finding non-pre-specified interactions; SM typically requires interactions to be pre-specified.  On the other hand, [AHRQ](https://www.ahrq.gov)-sponsored research I did on large medical outcomes datasets in the 1990s with the amazing University of Nevada Reno physician-statistician [Phil Goodman](https://www.legacy.com/obituaries/rgj/obituary.aspx?n=phil-goodman&pid=144885798), whom we lost at an all-too-early age, demonstrated that important non-additive effects are rare when predicting patient mortality.  As a result, neural networks were no better than logistic regression in terms of predictive discrimination in these datasets.

There are many current users of ML algorithms who falsely believe that one can [make reliable predictions from complex datasets with a small number of observations](http://fharrell.com/post/ml-sample-size).  Statisticians are pretty good at knowing the limitations caused by the effective sample size, and to stop short of trying to incorporate model complexity that is not supported by the information content of the sample.

Here are some rough guidelines that attempt to help researchers choose between the two approaches, for a prediction problem^[Note that as described [here](http://fharrell.com/post/classification), it is not appropriate to cast a prediction problem as a classification problem except in special circumstances that usually entail instant visual or sound pattern recognition requirements in a high signal:noise situation where the utility/cost/loss function cannot be specified.  ML practitioners frequently misunderstand this, leading them to use [improper accuracy scoring rules](http://www.fharrell.com/post/class-damage).].

**A statistical model may be the better choice if**

*  Uncertainty is inherent and the signal:noise ratio is not large---even with identical twins, one twin may get colon cancer and the other not; one should model tendencies instead of doing classification when there is randomness in the outcome
*  One doesn't have perfect training data, e.g., cannot repeatedly test one subject and have outcomes assessed without error
*  One wants to isolate effects of a small number of variables
*  Uncertainty in an overall prediction or the effect of a predictor is sought
*  Additivity is the dominant way that predictors affect the outcome, or interactions are relatively small in number and can be pre-specified
*  The sample size isn't huge
*  One wants to isolate (with a predominantly additive effect) the effects of "special" variables such as treatment or a risk factor
*  One wants the entire model to be interpretable

**Machine learning may be the better choice if**

*  The signal:noise ratio is large and the outcome being predicted doesn't have a strong component of randomness; e.g., in visual pattern recognition an object must be an `E` or not an `E`
*  The learning algorithm can be trained on an unlimited number of exact replications (e.g., 1000 repetitions of each letter in the alphabet or of a certain word to be translated to German)
*  Overall prediction is the goal, without being able to succinctly describe the impact of any one variable (e.g., treatment)
*  One is not very interested in estimating uncertainty in forecasts or in effects of selected predictors
*  Non-additivity is expected to be strong and can't be isolated to a few pre-specified variables (e.g., in visual pattern recognition the letter `L` must have both a dominating vertical component **and** a dominating horizontal component **and** these two must intersect at their endpoints) 
*  The sample size is [huge](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-14-137)
*  One does not need to isolate the effect of a special variable such as treatment
*  One does not care that the model is a "black box"


## Further Reading
*  [Big Data and Machine Learning in Health Care](https://jamanetwork.com/journals/jama/article-abstract/2675024) by AL Beam and IS Kohane
*  Harvard Business Review article [Why You're Not Getting Value From Your Data Science](https://hbr.org/2016/12/why-youre-not-getting-value-from-your-data-science), about regression vs. machine learning in business applications

#### Footnotes
