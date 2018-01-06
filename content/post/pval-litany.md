+++
title = "A Litany of Problems With p-values"
date = 2017-02-05T08:39:00Z
updated = 2017-02-05T09:00:02Z
tags = ["decision-making", "bayes", "multiplicity", "p-values", "hypothesis-testing"]
+++
In my opinion, null hypothesis testing and p-values have done significant harm
to science.  The purpose of this note is to catalog the many problems
caused by p-values.  As readers post new problems in their comments,
more will be incorporated into the list, so this is a work in progress.

The American Statistical Association has done a great service by issuing
its [Statement on Statistical Significance and
P-values](http://www.amstat.org/asa/files/pdfs/P-ValueStatement.pdf).
 Now it's time to act.  To create the needed motivation to change, we
need to fully describe the depth of the problem.

It is important to note that no statistical paradigm is perfect.
 Statisticians should choose paradigms that solve the greatest number of
real problems and have the fewest number of faults.  This is why I
believe that the Bayesian and likelihood paradigms should replace
frequentist inference.

Consider an assertion such as "the coin is fair", "treatment A yields
the same blood pressure as treatment B", "B yields lower blood pressure
than A", or "B lowers blood pressure at least 5mmHg before A."  Consider
also a compound assertion such as "A lowers blood pressure by at least
3mmHg and does not raise the risk of stroke."

### A. Problems With Conditioning

1.  p-values condition on what is unknown (the assertion of interest;
    H~0~) and do not condition on what is known (the data).
2.  This conditioning does not respect the flow of time and information;
    p-values are backward probabilities.

### B. Indirectness

1.  Because of A above, p-values provide only indirect evidence and are
    problematic as evidence metrics.  They are sometimes monotonically
    related to the evidence (e.g., when the prior distribution is flat)
    we need but are not properly calibrated for decision making.
2.  p-values are used to bring indirect evidence against an assertion
    but cannot bring evidence in favor of the assertion.  
3.  As
    detailed [here](http://www.fharrell.com/2017/01/null-hypothesis-significance-testing.html),
    the idea of proof by contradiction is a stretch when working with
    probabilities, so trying to quantify evidence for an assertion by
    bringing evidence against its complement is on shaky ground.
4.  Because of A, p-values are difficult to interpret and very few
    non-statisticians get it right.  The best article on
    misinterpretations I've found
    is [here](http://www.citeulike.org/user/harrelfe/article/14042559).

### C. Problem Defining the Event Whose Probability is Computed

1.  In the continuous data case, the probability of getting a result as
    extreme as that observed with our sample is zero, so the p-value is
    the probability of getting a result *more extreme* than that
    observed.  Is this the correct point of reference?
2.  How does *more extreme* get defined if there are sequential analyses
    and multiple endpoints or subgroups?  For sequential analyses do we
    consider planned analyses are analyses intended to be run even if
    they were not?

### D. Problems Actually Computing p-values

1.  In some discrete data cases, e.g., comparing two proportions, there
    is tremendous disagreement among statisticians about how p-values
    should be calculated.  In a famous 2x2 table from an ECMO adaptive
    clinical trial, 13 p-values have been computed from the same data,
    ranging from 0.001 to 1.0.  And many statisticians do not realize
    that Fisher's so-called "exact" test is not very accurate in many
    cases.
2.  Outside of binomial, exponential, and normal (with equal variance)
    and a few other cases, p-values are actually very difficult to
    compute exactly, and many p-values computed by statisticians are of
    unknown accuracy (e.g., in logistic regression and mixed effects
    models). The more non-quadratic the log likelihood function the more
    problematic this becomes in many cases. 
3.  One can compute (sometimes requiring simulation) the type-I error of
    many multi-stage procedures, but actually computing a p-value that
    can be taken out of context can be quite difficult and sometimes
    impossible.  One example: one can control the false discovery
    probability (incorrectly usually referred to as a rate), and ad hoc
    modifications of nominal p-values have been proposed, but these are
    not necessarily in line with the real definition of a p-value.

### E. The Multiplicity Mess

1.  Frequentist statistics does not have a recipe or blueprint leading
    to a unique solution for multiplicity problems, so when many
    p-values are computed, the way they are penalized for multiple
    comparisons results in endless arguments.  A Bonferroni multiplicity
    adjustment is consistent with a Bayesian prior distribution
    specifying that the probability that all null hypotheses are true is
    a constant no matter how many hypotheses are tested.  By contrast,
    Bayesian inference reflects the facts that P(A ∪ B) ≥ max(P(A),
    P(B)) and P(A ∩ B) ≤ min(P(A), P(B)) when A and B are assertions
    about a true effect.
2.  There remains controversy over the choice of 1-tailed vs. 2-tailed
    tests.  The 2-tailed test can be thought of as a multiplicity
    penalty for being potentially excited about either a positive effect
    or a negative effect of a treatment.  But few researchers want to
    bring evidence that a treatment harms patients; a pharmaceutical
    company would not seek a licensing claim of harm.  So when one
    computes the probability of obtaining an effect larger than that
    observed if there is no true effect, why do we too often ignore the
    sign of the effect and compute the (2-tailed) p-value?
3.  Because it is a very difficult problem to compute p-values when the
    assertion is compound, researchers using frequentist methods do not
    attempt to provide simultaneous evidence regarding such assertions
    and instead rely on ad hoc multiplicity adjustments.
4.  Because of A1, statistical testing with multiple looks at the data,
    e.g., in sequential data monitoring, is ad hoc and complex.
     Scientific flexibility is discouraged.  The p-value for an early
    data look must be adjusted for future looks.  The p-value at the
    final data look must be adjusted for the earlier inconsequential
    looks.  Unblinded sample size re-estimation is another case in
    point.  If the sample size is expanded to gain more information,
    there is a multiplicity problem and some of the methods commonly
    used to analyze the final data effectively discount the first wave
    of subjects.  How can that make any scientific sense?
5.  Most practitioners of frequentist inference do not understand that
    multiplicity comes from chances you give data to be extreme, not
    from chances you give true effects to be present.

### F. Problems With Non-Trivial Hypotheses

1.  It is difficult to test non-point hypotheses such as "drug A is
    similar to drug B".
2.  There is no straightforward way to test compound hypotheses coming
    from logical unions and intersections. 

### G. Inability to Incorporate Context and Other Information

1.  Because extraordinary claims require extraordinary evidence, there
    is a serious problem with the p-value's inability to incorporate
    context or prior evidence.  A Bayesian analysis of the existence of
    ESP would no doubt start with a very skeptical prior that would
    require extraordinary data to overcome, but the bar for getting a
    "significant" p-value is fairly low. Frequentist inference has a
    greater risk for getting the direction of an effect wrong
    (see [here](http://andrewgelman.com/) for more).
2.  p-values are unable to incorporate outside evidence.  As a converse
    to 1, strong prior beliefs are unable to be handled by p-values, and
    in some cases the results in a lack of progress.  Nate Silver
    in *The Signal and the Noise* beautifully details how the conclusion
    that cigarette smoking causes lung cancer was greatly delayed (with
    a large negative effect on public health) because scientists
    (especially Fisher) were caught up in the frequentist way of
    thinking, dictating that only randomized trial data would yield a
    valid p-value for testing cause and effect.  A Bayesian prior that
    was very strongly against the belief that smoking was causal is
    obliterated by the incredibly strong observational data.  Only by
    incorporating prior skepticism could one make a strong conclusion
    with non-randomized data in the smoking-lung cancer debate.
3.  p-values require subjective input from the producer of the data
    rather than from the consumer of the data.

### H. Problems Interpreting and Acting on "Positive" Findings

1.  With a large enough sample, a trivial effect can cause an
    impressively small p-value (statistical significance ≠ clinical
    significance).
2.  Statisticians and subject matter researchers (especially the latter)
    sought a "seal of approval" for their research by naming a cutoff on
    what should be considered "statistically significant", and a cutoff
    of p=0.05 is most commonly used.  Any time there is a threshold
    there is a motive to game the system, and gaming (p-hacking) is
    rampant.  Hypotheses are exchanged if the original H~0~ is not
    rejected, subjects are excluded, and because statistical analysis
    plans are not pre-specified as required in clinical trials and
    regulatory activities, researchers and their all-too-accommodating
    statisticians play with the analysis until something "significant"
    emerges.
3.  When the p-value is small, researchers act as though the point
    estimate of the effect is a population value.
4.  When the p-value is small, researchers believe that their conceptual
    framework has been validated.  

### I. Problems Interpreting and Acting on "Negative" Findings

1.  Because of B2, large p-values are uninformative and do not assist
    the researcher in decision making (Fisher said that a large p-value
    means "get more data").

------
More recommended reading:

*   William Briggs' [Everything Wrong With P-values Under One Roof](http://wmbriggs.com/post/9338)
