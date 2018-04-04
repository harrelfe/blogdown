---
title: Musings on Multiple Endpoints in RCTs
author: Frank Harrell
date: '2018-03-26'
modified: '2018-04-04'
slug: ymult
categories: []
tags:
  - RCT
  - bayes
  - design
  - drug-evaluation
  - evidence
  - hypothesis-testing
  - medicine
  - multiplicity
  - p-value
  - posterior
  - endpoints
summary: 'This article discusses issues related to alpha spending, effect sizes used in power calculations, multiple endpoints in RCTs, and endpoint labeling.  Changes in endpoint priority is addressed.  Included in the the discussion is how Bayesian probabilities more naturally allow one to answer multiple questions without all-too-arbitrary designations of endpoints as "primary" and "secondary".  And we should not quit trying to learn.'
header:
  caption: ''
  image: ''
---
<p class="rquote">
Learning is more productive than avoiding mistakes.  And if one wishes to just avoid mistakes, make sure the mistakes are real.  Question whether labeling endpoints is productive, and whether type I error risks are valuable in quantifying evidence for effects and should interfere with asking questions.
</p>

The [NHLBI](https://www.nhlbi.nih.gov)-funded [ISCHEMIA](https://www.ischemiatrial.org) multinational randomized clinical trial^[Disclosure: I lead the independent statistical team at Vanderbilt that supports the DSMB for the ISCHEMIA trial and was involved in the trial design. As the DSMB reporting statistician I am unblinded to treatment assignment and outcomes. I had no interaction with the blinded independent advisory committee that recommended the primary endpoint change or with the process that led to that recommendation.] is designed to assess the effect of cardiac catheterization-guided coronary revascularization strategy (which includes optimal medical management) compared to optimal medical management alone (with cardiac cath reserved for failure of medical therapy) for patients with stable coronary artery disease.  It is unique in that the use of cardiac catheterization is randomized, so that the entire "strategy pipeline" can be studied.  Previous studies performed randomization after catheterization results were known, allowing the so-called "oculo-stenotic reflex" of cardiologists to influence adherence to randomization to a revascularization procedure.

As well summarized [here](https://www.tctmd.com/news/ischemia-fracas-amid-charges-moving-goalposts-investigators-come-out-swinging) and [here](http://circoutcomes.ahajournals.org/content/11/4/e004744), the ISCHEMIA trial recently created a great deal of discussion in the cardiology community when the primary outcome was changed from cardiovascular death or nonfatal myocardial infarction to a 5-category endpoint that also includes hospitalization for unstable angina or heart failure, and resuscitated cardiac arrest.  The 5-component endpoint was the trial's original primary endpoint and was the basis for the NIH grant funding.  The possibility and procedure for reverting back to this 5-component endpoint was thought out even before the study began.  The change was pragmatic as is usually the case: the accrual and event rates seldom go as hoped.  The main concern in the cardiology community is the use of so-called "soft" endpoints.  The original two-component endpoint is now an important secondary endpoint.

The purpose of this article is not to discuss ISCHEMIA but to discuss the general study design, endpoint selection, and analysis issues ISCHEMIA raises that apply to a multitude of trials.

# Power Voodoo: Even With Only One Endpoint, Sizing a Study is Challenging
Before discussing power, recall that the type I error α is the probability (risk) of making an assertion of a nonzero effect when the true effect is zero.  In any given study we don't know if a type I error has been committed.  A type I error is not an error in the usual sense; it is a long-run operating characteristic, i.e., the chance of someone observing data **more** extreme than ours if they could indefinitely repeat our experiment but with a treatment effect of exactly zero magically inserted.  Type I error is the chance of making an **assertion** of efficacy *in general*, when there is no efficacy.

Power calculations, and sample size calculations based on power, have long been thought by statisticians to be more voodoo than science.  Besides all the problems related to null hypothesis testing in general, and arbitrariness in the setting of α and power (1 - type II error β), a significant difficulty and chance for arbitrariness is the choice of the effect size δ to detect with probability 1 - β.  δ is invariably manipulated, at least partly, to result in a sample size that meets budget constraints.  What if instead a fully [sequential trial](/post/bayes-seq) was done and budgeting were incremental depending on the promise shown by current results?  δ could be held at the original effect size determined by clinical experts, and a Bayesian approach could be used in which no single δ was assumed.  Promising evidence for a more-than-clinically-trivial effect could result in the release of more funds^[This sequential funding approach assumes that outcomes occur quickly enough to influence the assessment.].  Total program costs could even be reduced, by more quickly stopping studies with a high risk of being futile.  A sequential approach makes it less necessary to change an endpoint for pragmatic reasons once the study begins.  So would adoption of a Bayesian approach to evidence generation, as a replacement for null hypothesis significance testing.  If one "stuck it out" with the original endpoint no matter what the accrual and event frequency, and found that the treatment efficacy assessment is not "definitive" but that the posterior probability of efficacy was 0.93 at the planned study end, many would regard the result as providing good evidence (i.e., a betting person would not make money by betting against the new treatment).  On the other hand, p > 0.05 would traditionally be seen as "the study is uninformative since statistical significance was not achieved^[Interpreting the p-value in conjunction with a 0.95 confidence interval would help, but there are two problems.  First, most users of frequentist theory are hung up on the p-value.  Second, the confidence interval has endpoints that are not controllable by the user, in contrast to Bayesian posterior probabilities of treatment effects being available for any user-specified interval endpoints.  For example, one may want to compute Prob(blood pressure reduction > 3mmHg).]."  To some extent the perceived need to change endpoints in a study^[Which is ethically done, by decision makers using only pooled treatment data.] occurs because study leaders and especially sponsors are held hostage by the null hypothesis significance testing/power paradigm.

Speaking of Bayes and sample size calculations, the Bayesian philosophy is to not have any unknowns in any calculation.  Posterior probabilities are conditional on current cumulative data and do not use a single value for δ.  An entire prior distribution is used for δ.  By allowing for uncertainty in δ, Bayesian power calculations are more honest than frequentist calculations.
Some useful references are [here](http://www.citeulike.org/search/username?q=tag%3Asample-size*+%26%26+tag%3Abayes*&search=Search+library&username=harrelfe).

One of the challenges in power and sample size calculations, and knowing when to stop a study, is that there are competing goals.  One might be interested in concluding any of the following:

* the treatment is beneficial (working in the right direction)
* the treatment is more than trivially beneficial
* the estimate of the magnitude of the treatment effect has sufficient precision (e.g., the multiplicative margin of error in a hazard ratio)

In the frequentist domain, planning studies around [precision](http://www.citeulike.org/search/username?q=tag%3Aprecision&search=Search+library&username=harrelfe) frees the researcher from having to choose δ.  The ISCHEMIA study, in addition to doing traditional power calculations, also emphasized having a sufficient sample size to estimate the hazard ratio for the two most important endpoints to within an adequate multiplicative margin of error with 0.95 confidence.  Bayesian precision can likewise be determined using the half width of the 0.95 credible interval for the treatment effect^[If there are multiple data looks, the traditional frequentist confidence interval is no longer valid and a complicated adjustment is needed.  The adjusted confidence interval would be seen by Bayesians as conservative.].



# Multiple Endpoints and Endpoint Prioritization
To a large extent, the perceived need to adjust/penalize for asking multiple questions (about multiple endpoints) or at least the need for prioritization of endpoints arises from the perceived need to control overall type I error (also known as α spending).  The chance of making an "effectiveness" assertion if any of three endpoints shows evidence against a null hypothesis is greater than α for any one endpoint.  As an aside, [Cook and Farewell](http://www.citeulike.org/user/harrelfe/article/13263921) give a persuasive argument for prioritization of endpoints but not adjusting their p-values for multiplicity when one is asking separate questions regarding the endpoints^[That is, when the endpoint comparisons are to be interpreted marginally.].  Think of prioritization of endpoints as pre-specification of the order for publication and how the study results are publicized.  It is OK to announce a "significant" third endpoint as long as the "insignificant" first and second endpoints are announced first, and the context for the third endpoint is preserved.

Having been privy to dozens of hours of discussions among clinical trialists during protocol writing for many randomized clinical trials, I can confidently say that the reasoning for the final choices comes from a mixture of practical, clinical, and patient-oriented considerations, perhaps with too much emphasis on the pragmatic statistical question "for which endpoint that the treatment possibly affects are we likely to to have sufficient number of events?".  Though statistical considerations are important, this approach is not fully satisfying because

* the final choices remain too arbitrary and are not purely clinically/public health motivated
* binary endpoints [are not statistically efficient anyway](/post/ordinal-info)
* using separate binary endpoints does not combine the endpoints into an overall patient-utility scale^[An ordinal "what's the worst thing that happened to the patient" scale would have few assumptions, would increase power, and would give credit to a treatment that has more effect on more serious outcomes than it has on less serious ones.].

Having multiple pre-specified endpoints also sets the stage for a blinded committee to change the endpoint priority for pragmatic reasons, related to the "slavery to statistical power and null hypothesis testing" discussed above.

It is important to note for ISCHEMIA and in general that having a primary endpoint does not prevent anyone interpreting the study's final result from emphasizing a secondary or tertiary endpoint.

# Joint Modeling of Multiple Endpoints

joint modeling of multiple outcomes allows uncovering relationships of multiple outcome variables, and quantifying joint evidence for all outcomes simultaneously, while providing the usual marginal outcome evidence (for each outcome separately).  As discussed [here](/post/bayes-freq-stmts) and [here](/post/journey), Bayesian posterior inference has many advantages in this context.  For example, the final analysis of a clinical trial with three endpoints E<sub>1</sub>, E<sub>2</sub>, E<sub>3</sub> might be based on posterior probabilities of the following forms:

                                       |                
------------------------------|---------------
Prob(E<sub>1</sub> > 0 or E<sub>2</sub> > 0 or E<sub>3</sub> > 0)| Prob(efficacy) on **any** endpoint
Prob(E<sub>1</sub> > 0 and E<sub>2</sub> > 0) | Prob(efficacy) on both of the first two endpoints
Prob(E<sub>1</sub> > 0 or (E<sub>2</sub> > 3 and E<sub>3</sub> > 4)) | Prob(any mortality reduction or large reductions on two nonfatal endpoints)
Prob(at least two of E<sub>1</sub> > 0, E<sub>2</sub> > 0, E<sub>3</sub> > 0) | Prob(hitting any two of the three efficacy targets)
Prob(-1 < E<sub>1</sub> < 1) | Prob(similarity of E<sub>1</sub> outcome)

One can readily see that once you get away from null hypothesis testing, many clinically relevant possibilities exist, and multiplicity considerations are cast aside.  A reasonable strategy would be to demand an extra-high probability of hitting any one of three targets, or a somewhat lower probability of hitting any two of the three targets.  More about this way of thinking may be found [here](/post/bayes-freq-stmts)^[Just as one can compute the probability of rolling a six on either of two dice, or rolling a total greater than 9, direct predictive-mode probabilities may be computed as often as desired with no multiplicity.  Multiplicity with backwards probabilities comes from giving data more chances to be extreme (the frequentist sample space) and not from the chances you give more efficacy parameters to be positive.].

Posterior probabilities also provide the direct forward predictive type of evidence that leads to optimum decisions.  Barring cost considerations, a treatment that has a 0.93 chance of reducing mortality may be deemed worthwhile, especially if a skeptical prior was used.

Joint Bayesian modeling of multiple endpoints also allows one to uncover interrelationships among the endpoints as described in the recent paper by [Costa and Drury](https://onlinelibrary.wiley.com/doi/abs/10.1002/pst.1852).  One of the methods proposed by the authors, the one based on a multivariate copula, has several advantages.  First, one obtains the usual marginal treatment effects on each endpoint separately.  Second, the Bayesian analysis they describe allows one to estimate the amount of dependence between two endpoints, which is interesting in its own right and will help in estimating power when planning future studies.  Third, the amount of such dependence can be allowed to vary by treatment.  For example, if one endpoint is an efficacy endpoint (or continuous measurement) and another is the occurrence of an adverse event,  placebo subjects may randomly experience the adverse event such that there is no within-person correlation between it and the efficacy response.  On the other hand, subjects on the active drug may experience the efficacy and safety outcomes together.  E.g., subjects getting the best efficacy response may be those with more adverse events.  Estimation of between-outcome dependencies is of real clinical interest.

Most importantly, Bayesian analysis of clinical trials, when multiple endpoints are involved, allows the results for each endpoint to be properly interpreted marginally.  That is because the prior state of knowledge, which may reasonably be encapsulated into a skeptical prior (i.e., a prior distribution that assumes large treatment effects are unlikely) leads to a posterior probability of efficacy for each endpoint that is straightforwardly interpreted regardless of context.  Because Bayes deals with [forward probabilities](/post/pvalprobs), these posterior probabilities of efficacy are calibrated by their priors.  For example, the skepticism with which we view efficacy of a treatment on endpoint E<sub>2</sub> comes from the data about the E<sub>2</sub> effect and the prior skepticism about the E<sub>2</sub> effect, no matter what the effect on E<sub>1</sub>.  This way of thinking shows clearly the value of trying to learn more from a study by asking multiple questions.  One should not be penalized for curiosity.

### Further Reading
* [Composite end points in randomized trials: There is no free lunch](https://jamanetwork.com/journals/jama/article-abstract/185214?redirect=true)
* [Miscellaneous papers](http://www.citeulike.org/user/harrelfe/tag/multiple-endpoints) on multiple endpoints

### Footnotes
