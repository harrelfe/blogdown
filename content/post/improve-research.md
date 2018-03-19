---
title: 'Improving Research Through Safer Learning from Data'
author: Frank Harrell
date: '2018-03-08'
updated: '2018-03-19'
draft: false
slug: improve-research
categories: []
tags: ["design", "evidence", "generalizability", "inference", "judgment", "measurement", "prior", "bayes"]
summary: "What are the major elements of learning from data that should inform the research process?  How can we prevent having false confidence from statistical analysis?  Does a Bayesian approach result in more honest answers to research questions? Is learning inherently subjective anyway, so we need to stop criticizing Bayesians' subjectivity?  How important and possible is pre-specification?  When should replication be required?  These and other questions are discussed."
header:
  caption: ''
  image: ''
---

# Overview

There are two broad classes of data analysis.  The first class, exploratory data analysis, attempts to understand the data at hand, i.e., to understand _what happened_, and can use descriptive statistics, graphics, and other tools, including multivariable statistical models^[When many variables are involved, a statistical model is often the best descriptive tool, even when it's not used for inference.].  The second broad class of data analysis is inferential analysis which aims to provide evidence and assist in judgments about the process generating the one dataset.  Here the interest is in generalizability, and a statistical model is not optional^[Every statistical test is using a model.  For example, the Wilcoxon two-sample test is a special case of the proportional odds model and requires the proportional odds assumption to hold to achieve maximum power.].  Sometimes this is called population inference but it can be thought of less restrictively as understanding the data generating process.  Also there is prediction, which is a mode of inference distinct from judgment and decision making.

The following discussion concentrates on inference, although several of the concepts, especially measurement accuracy, fully pertain to exploratory data analysis.

The key elements of learning from data using statistical inference involve the following:

1.  pre-specification if doing formal inference, intending to publish, or intending to be reviewed by regulatory authorities
1.  choosing an experimental design
1.  considering the spectrum of per-subject information available
1.  considering information content, bias, and precision in measurements
1.  understanding variability in measurements
1.  specification of the statistical model
1.  incorporating beliefs of judges/regulators/consumers into the model parameters if Bayesian
1.  incorporating beliefs of judges/regulators/consumers into model interpretation if frequentist
1.  using the model to quantify evidence
1.  replication/validation, when needed
1.  translating the evidence to a decision or an action

# Pre-specification

Pre-specification of the study design and analysis are incredibly important components of reproducible research.  It is necessary unless one is engaging in exploratory learning (especially in the initial phase of research) and not intending for the results to be considered confirmatory.  Pre-specification controls investigator degrees of freedom (see below) and keeps the investigator from entering the [garden of forking paths](http://www.stat.columbia.edu/~gelman/research/unpublished/p_hacking.pdf).  A large fraction of studies that failed to validate can be traced to the non-existence of a prospective, specific data transformation and statistical analysis plan.  Randomized clinical trials require almost complete pre-specification.  Animal and observational human subjects research does not enjoy the same protections, and many an experiment has resulted in statistical disappointment that tempted the researcher to modify the analysis, choice of response variable, sample membership, computation of derived variables, normalization method, etc.  The use of cutoffs on p-values causes a large portion of this problem.

Frequentist and Bayesian analysis are alike with regard the need for pre-specification.  But a Bayesian approach has an advantage here: you can include parameters for what you don't know and hope you don't need (but are not sure).  For example, one could specify a model in which a dose-response relationship is linear, but add a parameter that allows a departure from linearity.  One can hope that interaction between treatment and race is absent, but [include parameters allowing for such interactions](https://www.ncbi.nlm.nih.gov/pubmed/9192445).  In these two examples, skeptical prior distributions for the "extra" parameters would favor a linear dose-response or absence of interaction, but as the sample size increases the data would allow these parameters to "float" as needed.  Bayes still provides accurate inferences when one is not sure of the model.  This is discussed further below.  Two-stage analyses as typically employed in the frequentist paradigm (e.g., pre-testing for linearity of dose-response) do not control type I error.  

# Experimental Design

The experimental design is all important, and is what allows interpretations to be causal.  For example, in comparing two treatments there are two types of questions:

1. Did treatment B work better in the group of patients receiving it in comparison to those patients who happened to receive treatment A?
1. Would this patient fare better were _she_ given treatment B vs. were _she_ given treatment A?

The first question is easy to answer using statistical models (estimation or prediction), not requiring any understanding of physicians' past treatment choices.  The second question is one of causal inference, and it is impossible for observational data to answer that question without additional unverifiable assumptions.  (Compare this to a randomized crossover study where the causal question can be almost directly answered.)

In a designed experiment, the experimenter usually knows exactly which variables to measure, and some of the variables are completely controlled.  For example, in a 3x3 randomized factorial design, two factors are each experimentally set to three different levels giving rise to 9 controlled combinations.  The experiment can block on yet other factors to explain outcome variation caused by them.  In a randomized crossover study, an investigator can estimate causal treatment differences per subject if carryover effects are washed out.  In an observational therapeutic effectiveness study it is imperative to measure a long list of relevant variables that explain outcomes **and** treatment choices.  Still not guaranteeing an ability to answer the causal therapeutic question, having a wide spectrum of accurately collected baseline data is required to begin the process.  Other design elements of observational studies are extremely important, including such aspects as when variables are measured, which subjects are included, what is the meaning of "time zero", and how does one avoid losses to follow-up.

# Measurements and Understanding Variability

Understanding what measurements really mean, what they do not capture, minimizing systematic bias, minimizing measurement error, and maximizing data resolution are key to optimizing statistical power and soundness of inference.  Resolution is related to data acquisition, variable definitions, and measurement errors.  Optimal statistical information comes from continuous measurements whose measurement errors are small.

Understanding sources of variability and incorporating those into the experimental design and the statistical model are important.  What is the disagreement in technical replicates (e.g. splitting one blood sample into two and running both through a blood analyzer)?  Are there batch effects?  Edge effects in a gene microarray?  Variation due to different temperatures in the lab each day?  Do patients admitted on Friday night inherently have longer hospital stays?  Other day of week effects?  Seasonal variation and other long-term time effects?  How about region, country, and lab variation?

# Beliefs Matter When Interpreting Results or Quantifying Absolute Evidence

Notice the inclusion of _beliefs_ in the original list.  Frequentists operate under the illusion of objectivity and believe that beliefs are not relevant.  This is an illusion, for four reasons.

1. IJ Good showed that all probabilities are subjective because they depend on the knowledge of the observer.   One of his examples is that a card player who knows that a certain card is sticky will know a different probability that the card will be at the top of the deck than will a player who doesn't know that.  
1. To compute p-values, one **must** know the _intentions_ of the investigator.  Did she intend to study 90 patients and happened to observe 10 bad outcomes, or did she intend to sample patients until 10 outcomes happened?  Did she intend to do an early data look?  Did she actually do an early data look but first wrote an affidavit affirming that she would not take any action as a result of the look?  Did she intend to analyze three dependent variables and was the one reported the one she would have reported even had she looked at the data for all three?  All of these issues factor into computation of a p-value.
1. The choice of the statistical model is always subjective (more below).
1. Interpretations are subjective.  Do you multiplicity-adjust a p-value?  Using which of the competing approaches?  What if other studies have results that are inconsistent with the new study?  How do we discount the current p-value for that?  But most importantly, the necessary conversion of a frequentist probability of data given a hypothesis into evidence about the hypothesis is entirely subjective.

Bayesian inference gets criticized for being subjective when in fact its distinguishing feature is that it is stating subjective assumptions clearly.

# Specification of the Statistical Model

The statistical model is the prior belief about the _structure_ of the problem.  Outside of mathematics and physics, its choice is all too arbitrary, and statistical results and their interpretation depend on this choice.  This applies equally to Bayesian and frequentist inference.  The model choice has more impact than the choice of a prior distribution; the model choice does not "wear off" nearly as much as the prior does as the sample size gets large.

[Investigator degrees of freedom](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5122713) greatly affects the reliability and generalizability of scientific findings.  This applies to measurement, experimental design, choice of variables, the statistical model, and other facets of the research process.  Turning to just the statistical model, there are aspects of modeling about which we continually delude ourselves in such a way as to have false confidence in results.  This happens primarily in two ways.  Either the investigator "plays with the data" to try different models and uses only the apparently best-fitting one, making confidence and credible intervals too narrow and p-values and standard errors too small, or she selects a model apriori and hopes that it fits "well enough".  The latter occurs even in confirmatory studies with rigorous pre-specification of the analysis.  Whenever we use a model that makes an assumption about data structure, including assumptions about linearity, interactions, which variables to include, the shape of the distribution of the response given covariates, constancy of variance, etc., the inference is conditional on all those assumptions being true.  The Bayesian approach provides an out: make all the assumptions you want, but allow for departures from those assumptions.  If the model contains a parameter for everything we know we don't know (e.g., a parameter for the ratio of variances in a two-sample t-test), the resulting posterior distribution for the parameter of interest will be flatter, credible intervals wider, and confidence intervals wider.  This makes them more likely to lead to the correct interpretation, and makes the result more likely to be reproducible.

Consider departures from the normality assumption.
[Box and Tiao](http://onlinelibrary.wiley.com/book/10.1002/9781118033197) show how to elegantly allow for non-normality in a Bayesian two-sample t-test.  This is done by allowing the data distribution to have a kurtosis (tail heaviness) that is different from what the normal curve allows.  They place a prior distribution on the kurtosis parameter favoring normality, but as the sample size increases, less and less normality is assumed.  When the data indicate that the tails are heavier than Gaussian, they showed that the resulting point estimates of the two means are very similar to trimmed means.  In the same way a prior distribution for the ratio of variances that may favor 1.0, a prior for the degree of interaction between treatment and a baseline variable that favors no interaction, and a prior for the degree of nonlinearity in an age effect that favors linearity for very small sample sizes could all be specified.  The posterior distribution for the main parameter of interest will reflect all of these uncertainties in an honest fashion.  This is related to penalized maximum likelihood estimation or shrinkage in the frequentist domain^[The frequentist paradigm does not provide confidence intervals or p-values when parameters are penalized.].

Besides the ability to handle more uncertainties, the Bayesian paradigm provides [direct evidentiary statements](/post/bayes-freq-stmts) such as the probability that the treatment reduces blood pressure.  This is in contrast with the frequentist paradigm, which results in a probability of getting observed effects greater than what we observed were the true effect exactly zero, the model correct, and the same experiment (other than changing H<sub>0</sub> to be true) were to be repeated indefinitely^[This implies that the exact experimental design that is **in effect** is known so that the p-value can be computed by re-running that exact design indefinitely often to compute the probability of finding a larger effect in those repeated experiments than the effect originally observed.].

# Using Statistical Models to Quantify Evidence

Since the model choice is subjective, if we want our quantified evidence for effects to be accurate and not overstated, we should use Bayesian models acknowledging what we don't know.  Nate Silvers in [The Signal and the Noise](https://www.amazon.com/Signal-Noise-Many-Predictions-Fail-but/dp/0143125087) eloquently wrote in detail about a different example of "what we don't know", related to causal inference from observational data.  In his description of the controversy about cigarette smoking and lung cancer he pointed out that many people believed Ronald Fisher when Fisher said that since one can't randomize cigarette exposure there is no way to draw trustworthy inference; therefore we should draw no conclusions (he also had a significant conflict of interest, as he was a consultant to the tobacco industry).  Silver showed that even an extremely skeptical prior distribution about the effect of cigarette smoking on lung cancer would be overridden by the data.  Because only the Bayesian approach allows insertion of skepticism at precisely the right point in the logic flow, one can think of a full Bayesian solution (prior + model) as a way to "get the model right", taking the design and context into account, to obtain reliable scientific evidence.  Note that possible failure to have all confounders measured can be somewhat absorbed into the skeptical prior distribution with a Bayesian approach.

In some tightly-controlled experiments, the statistical model is somewhat less relevant.

# Replication and Validation

Finally, turn to the complex issue of replication and/or validation.  First of all, it is important to know what replication is _not_.  [This article](/post/split-val) discusses split-sample validation as a data-wasting form of _internal validation_.  It does not demonstrate that other investigators with different measurement and survey techniques, different data cleaning procedures, and different subtle ways to "cheat" would arrive at the same answer.  It turns geographical differences and time trends into surprises rather than useful covariates.  A far better form of internal validation is the bootstrap, which has the added advantage (and burden) of requiring the researcher to completely specify all analytic steps.  Now contrast internal validation with true independent replication.  The latter has the advantages of validating the following:

1. the investigators and their hidden biases
1. the specificity of the statistical analysis plan
1. the technologies on which measurements are based (e.g., gene or protein expression)
1. the survey techniques including how subjects are interviewed (with respect to leading questions, etc.)
1. subject inclusion/exclusion criteria
1. subtle decisions that biased estimates such as treatment effects (e.g., deleting outliers, avoiding blinding and blinded data correction, remeasuring something when its value is suspect, etc.)
1. other systemic biases that one suspects would be different for different research teams

When is an independent replication or model validation warranted?  This is difficult to say, but is related to the potential impact of the result, on subjects and on future researchers.

The quickest and cheapest form of partial validation is to validate the investigators and code, in the following sense.  Have the investigators provide the pre-specified data manipulation (including computation of derived variables) and statistical analysis or machine learning plan, along with the raw data, to an independent team.  The independent team executes the data manipulation and analysis plan and compares the results to the results obtained by the original team.  Ideally the independent researchers would run the original code on their systems and also do some independent coding.  This process verifies code, computations, and specificity of the analysis plan and verifies that once the paper is published others will also be able to replicate the findings.  This approach would have entirely prevented the [Duke University Potti scandal](https://en.wikipedia.org/wiki/Anil_Potti) had the cancer biomarker investigators at Duke been interested in collaborating with an outside team.

If rigorous internal validation or attempted duplication of results by outsiders fails, there is no need to undertake an acquisition of new independent data to validate the original approach^[This especially pertains to prediction, and is less applicable to randomized trials.].

# Steps to Enhance the Scientific Process

1. Choose an experimental design that is appropriate for the question of interest, taking in account whether association or causation are central to the question
1. Choose the right measurements and measure them accurately or at least without a systemic bias favoring your viewpoint
1. Understand sources of variability and incorporate those into the design and the model
1. Formulate prior distributions for effect parameters that are informed by the subject matter and other reliable data.  Even if you use p-values this process will benefit the research.
1. Formulate a data model that is informed by subject matter, knowledge about the measurements, and experience with similar data
1. Add parameters to the model for what you don't know, putting priors on those parameters so as to favor your favorite model (e.g., normal distribution with equal variances for the t-test; absence of interactions) but not rule out departures from it.  If using a frequentist approach, parameters must be "all in", which will make confidence intervals honest but [wider than Bayesian credible intervals](https://www.ncbi.nlm.nih.gov/pubmed/9192445).
1. Independently validate code and calculations while verifying the specificity of the statistical analysis or machine learning plan
1. In many situations, especially when large scale policies are at stake, independently replicate the findings from scratch before believing them

-----

## Some Useful References
*  [How much does your data exploration overfit? Controlling bias via information usage](https://arxiv.org/abs/1511.05219) by D Russo and J Zou

-----

This article benefited from many thought-provoking discussions with Bert Gunter, who believes that replication and initial exploratory analysis are even more important than I do.  Chris Tong also provided valuable ideas.  Misconceptions are solely mine.

Footnotes:
