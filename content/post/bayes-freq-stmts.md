+++
title = "Bayesian vs. Frequentist Statements About Treatment Efficacy"
date = 2017-10-04
updated = 2018-04-01
tags = ["reporting", "inference", "p-value", "RCT", "bayes", "drug-evaluation", "evidence", "hypothesis-testing"]
+++
<p class="rquote">
To avoid "false positives" do away with "positive".<br><br>
A good poker player thinks to herself "The probability I can win with this hand is 0.91" and not "I'm going to win with this hand."<br><br>
State conclusions honestly, completely deferring judgments and actions to the ultimate decision makers.  Just as it is better to [make predictions than classifications](/post/classification) in prognosis and diagnosis, use the word "probably" liberally.
</p>
The following examples are intended to show the advantages of Bayesian reporting of
treatment efficacy analysis, as well as to provide examples contrasting
with frequentist reporting. As detailed
[here]({{< ref "post/pval-litany.md" >}}),
there are many problems with p-values, and some of those problems will
be apparent in the examples below. Many of the advantages of Bayes are
summarized [here]({{< ref "post/journey.md" >}}).
As seen below, Bayesian posterior probabilities prevent one from
concluding equivalence of two treatments on an outcome when the data do
not support that (i.e., the ["absence of evidence is not evidence of
absence"]({{< ref "post/errmed.md" >}}) error).

Suppose that a parallel group randomized clinical trial is conducted to
gather evidence about the relative efficacy of new treatment B to a
control treatment A. Suppose there are two efficacy endpoints: systolic
blood pressure (SBP) and time until cardiovascular/cerebrovascular
event. Treatment effect on the first endpoint is assumed to be
summarized by the B-A difference in true mean SBP. The second endpoint
is assumed to be summarized as a true B:A hazard ratio (HR). For the
Bayesian analysis, assume that pre-specified skeptical prior
distributions were chosen as follows. For the unknown difference in mean
SBP, the prior was normal with mean 0 with SD chosen so that the
probability that the absolute difference in SBP between A and B exceeds
10mmHg was only 0.05. For the HR, the log HR was assumed to have a
normal distribution with mean 0 and SD chosen so that the prior
probability that the HR>2 or HR<1/2 was 0.05. Both priors
specify that it is equally likely that treatment B is effective as it is
detrimental. The two prior distributions will be referred to as p1 and
p2.

### Example 1: So-called "Negative" Trial (Considering only SBP)

Frequentist Statement

-   Incorrect Statement: Treatment B did not improve SBP when compared
    to A (p=0.4)
-   Confusing Statement: Treatment B was not significantly different
    from treatment A (p=0.4)
-   Accurate Statement: We were unable to find evidence against the
    hypothesis that A=B (p=0.4). More data will be needed. As the
    statistical analysis plan specified a frequentist approach, the
    study did not provide evidence of similarity of A and B (but see the
    confidence interval below).
-   Supplemental Information: The observed B-A difference in means was
    4mmHg with a 0.95 confidence interval of [-5, 13]. If this study
    could be indefinitely replicated and the same approach used to
    compute the confidence interval each time, 0.95 of such varying
    confidence intervals would contain the unknown true difference in
    means.

Bayesian Statement

-   Assuming prior distribution p1 for the mean difference of SBP, the
    probability that SBP with treatment B is lower than treatment A is
    0.67. Alternative statement: SBP is probably (0.67) reduced with
    treatment B. The probability that B is inferior to A is 0.33.
    Assuming a minimally clinically important difference in SBP of
    3mmHg, the probability that the mean for A is within 3mmHg of the
    mean for B is 0.53, so the study is uninformative about the question
    of similarity of A and B.
-   Supplemental Information: The posterior mean difference in SBP was
    3.3mmHg and the 0.95 credible interval is [-4.5, 10.5]. The
    probability is 0.95 that the true treatment effect is in the
    interval [-4.5, 10.5]. [could include the posterior density
    function here, with a shaded right tail with area 0.67.]

### Example 2: So-called "Positive" Trial

Frequentist Statement

-   Incorrect Statement: The probability that there is no difference in
    mean SBP between A and B is 0.02
-   Confusing Statement: There was a statistically significant
    difference between A and B (p=0.02).
-   Correct Statement: There is evidence against the null hypothesis of
    no difference in mean SBP (p=0.02), and the observed difference
    favors B. Had the experiment been exactly replicated indefinitely,
    0.02 of such repetitions would result in more impressive results if
    A=B.
-   Supplemental Information: Similar to above.
-   Second Outcome Variable, If the p-value is Small: Separate
    statement, of same form as for SBP.

Bayesian Statement

-   Assuming prior p1, the probability that B lowers SBP when compared
    to A is 0.985. Alternative statement: SBP is probably (0.985)
    reduced with treatment B. The probability that B is inferior to A is
    0.015.
-   Supplemental Information: Similar to above, plus evidence about
    clinically meaningful effects, e.g.: The probability that B lowers
    SBP by more than 3mmHg is 0.81.
-   Second Outcome Variable: Bayesian approach allows one to make a
    separate statement about the clinical event HR and to state evidence
    about the joint effect of treatment on SBP and HR. Examples:
    Assuming prior p2, HR is probably (0.79) lower with treatment B.
    Assuming priors p1 and p2, the probability that treatment B both
    decreased SBP and decreased event hazard was 0.77. The probability
    that B improved **either** of the two endpoints was 0.991.

One would also report basic results. For SBP, frequentist results might
be chosen as the mean difference and its standard error. Basic Bayesian
results could be said to be the entire posterior distribution of the SBP
mean difference.

Note that if multiple looks were made as the trial progressed, the
frequentist estimates (including the observed mean difference) would
have to undergo complex adjustments. Bayesian results require no
modification whatsoever, but just involve reporting the latest available
cumulative evidence.
