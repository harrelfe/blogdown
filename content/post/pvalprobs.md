+++
title = "p-values and Type I Errors are Not the Probabilities We Need"
date = 2017-01-14T08:14:00Z
modified = 2017-11-28T08:18:44Z
tags = ["judgment", "inference", "likelihood", "bayes", "multiplicity", "p-value", "prior", "hypothesis-testing", "2017"]
+++
In trying to guard against false conclusions, researchers often attempt to minimize
the risk of a "false positive" conclusion.  In the field of assessing
the efficacy of medical and behavioral treatments for improving
subjects' outcomes, falsely concluding that a treatment is effective
when it is not is an important consideration.   Nowhere is this more
important than in the drug and medical device regulatory environments,
because a treatment thought not to work can be given a second chance as
better data arrive, but a treatment judged to be effective may be
approved for marketing, and if later data show that the treatment was
actually not effective (or was only trivially effective) it is difficult
to remove the treatment from the market if it is safe.  The probability
of a treatment not being effective is the probability of "regulator's
regret."  One must be very clear on what is conditioned upon (assumed)
in computing this probability.  Does one condition on the true
effectiveness or does one condition on the available data?  Type I error
conditions on the treatment having no effect and does not entertain the
possibility that the treatment actually worsens the patients' outcomes.
 Can one quantify evidence for making a wrong decision if one assumes
that all conclusions of non-zero effect are wrong up front because H<sub>0</sub>
was assumed to be true?  Aren't useful error probabilities the ones that
are not based on assumptions about what we are assessing but rather just
on the data available to us?

Statisticians have convinced regulators that long-run operating
characteristics of a testing procedure should rule the day, e.g., if we
did 1000 clinical trials where efficacy was always zero, we want no more
than 50 of these trials to be judged as "positive."  Never mind that
this type I error operating characteristic does not refer to making a
correct judgment for the clinical trial at hand.  Still, there is a
belief that type I error is the probability of regulator's regret (a
false positive), i.e., that the treatment is not effective when the data
indicate it is.  In fact, clinical trialists have been sold a bill of
goods by statisticians.  No probability derived from an assumption that
the treatment has zero effect can provide evidence about that effect.
 Nor does it measure the chance of the error actually in question.  All
probabilities are conditional on *something*, and to be useful they must
condition on the *right thing*.  This usually means that what is
conditioned upon must be knowable.

The probability of regulator's regret is the probability that a
treatment doesn't work given the data. So the probability we really seek
is the probability that the treatment has no effect *or that it has a
backwards effect*.  This is precisely one minus the Bayesian posterior
probability of efficacy.

In reality, there is unlikely to exist a treatment that has exactly zero
effect.  As [Tukey argued in
1991](http://www.citeulike.org/user/harrelfe/article/10529649), the
effects of treatments A and B are always different, to some decimal
place.  So the null hypothesis is always false and the type I error
could be said to be always zero.

The best paper I've read about the many ways in which p-values are
misinterpreted is [Statistical tests, P values, confidence intervals,
and power: a guide to
misinterpretations](http://www.citeulike.org/user/harrelfe/article/14042559) written by a group of renowned statisticians.  One of my favorite quotes from
this paper is

<small>
Thus to claim that the null P value is the probability that chance
alone produced the observed association is completely backwards: The P
value is a probability computed assuming chance was operating alone.
The absurdity of the common backwards interpretation might be
appreciated by pondering how the P value, which is a probability
deduced from a set of assumptions (the statistical model), can
possibly refer to the probability of those assumptions.
</small>

In 2016 the American Statistical Association took
a [stand](https://matloff.wordpress.com/2016/03/07/after-150-years-the-asa-says-no-to-p-values) against
over-reliance on p-values. This would have made a massive impact on all
branches of science had it been issued 50 years ago but better late than
never.

### Update 2017-01-19

Though believed to be true by many non-statisticians, p-values are not
the probability that H<sub>0</sub> is true, and to turn them into such
probabilities requires Bayes' rule.  If you are going to use Bayes' rule
you might as well formulate the problem as a full Bayesian model.  This
has many benefits, not the least of them being that you can select an
appropriate prior distribution and you will get exact inference.
 Attempts by several authors to convert p-values to probabilities of
interest (just as sensitivity and specificity are converted to
probability of disease once one knows the prevalence of disease) have
taken the prior to be discontinuous, putting a high probability on H<sub>0</sub>
being exactly true.  In my view it is much more sensible to believe that
there is no discontinuity in the prior at the point represented by H<sub>0</sub>,
encapsulating prior knowledge instead by saying that values near H<sub>0</sub>
are more likely if no relevant prior information is available.

Returning to the non-relevance of type I error as discussed above, and
ignoring for the moment that long-run operating characteristics do not
directly assist us in making judgments about the current experiment,
there is a subtle problem that leads researchers to believe that by
controlling type I "error" they think they have quantified the
probability of misleading evidence.  As discussed at length by my
colleague [Jeffrey
Blume](http://www.citeulike.org/user/harrelfe/author/Blume), once an
experiment is done the probability that positive evidence is misleading
is **not** type I error.  And what exactly does "error" mean in "type I
error?"  It is the probability of rejecting H<sub>0</sub> when H<sub>0</sub> is exactly
true, just as the p-value is the probability of obtaining data more
impressive than that observed given H<sub>0</sub> is true.  Are these really
error probabilities?  Perhaps ... if you have been misled earlier into
believing that we should base conclusions on how unlikely the observed
data would have been observed under H<sub>0</sub>.  Part of the problem is in the
loaded word "reject."  Rejecting H<sub>0</sub> by seeing data that are unlikely
if H<sub>0</sub> is true is perhaps the real error.

The "error quantification" truly needed is the probability that a
treatment doesn't work given all the current evidence, which as stated
above is simply one minus the Bayesian posterior probability of positive
efficacy.


### Update 2017-01-20

Type I error control is an indirect way to being careful about claims of
effects.  It should never have been the preferred method for achieving
that goal.  Seen another way, we would choose type I error as the
quantity to be controlled if we wanted to:

-   require the experimenter to visualize an infinite number of
    experiments that might have been run, and assume that the current
    experiment could be exactly replicated
-   be interested in long-run operating characteristics vs. judgments
    needing to be made for the one experiment at hand
-   be interested in the probability that other replications result in
    data more extreme than mine if there is no treatment effect
-   require early looks at the data to be discounted for future looks
-   require past looks at the data to be discounted for earlier
    inconsequential looks
-   create other multiplicity considerations, all of them arising from
    the chances you give data to be extreme as opposed to the chances
    that you give effects to be positive
-   data can be more extreme for a variety of reasons such as trying to
    learn faster by looking more often or trying to learn more by
    comparing more doses or more drugs

The Bayesian approach focuses on the chances you give effects to be
positive and does not have multiplicity issues (potential issues such as
examining treatment effects in multiple subgroups are handled by the
shrinkage that automatically results when you use the 'right' Bayesian
hierarchical model).

The p-value is the chance that someone else would observe data more
extreme than mine if the effect is truly zero (if they could exactly
replicate my experiment) and not the probability of no (or a negative)
effect of treatment given my data.

### Update 2017-05-10

As discussed in Gamalo-Siebers at al DOI: 10.1002/pst.1807 the type I
error is the probability of making an assertion of an effect when no
such effect exists. It is **not** the probability of regret for a
decision maker, e.g., it is not the probability of a drug regulator's
regret. The probability of regret is the probability that the drug
doesn't work or is harmful when the decision maker had decided it was
helpful. It is the probability of harm or no benefit when an assertion
of benefit is made. This is best thought of as the probability of harm
or no benefit given the data which is one minus the probability of
efficacy. Prob(assertion|no benefit) is not equal to
1-Prob(benefit|data).

### Update 2017-11-28

Type I ("false positive") error probability would be a useful concept
while a study is being designed.  Frequentists speak of type I error
control, but after a study is completed, the only way to commit a type I
error is to know with certainty that an effect is exactly zero.  But
then the study would not have been necessary.  So type I error remains a
long-run operating characteristic for a sequence of **hypothetical**
studies.

Thinking of p-values that a sequence of hypothetical studies might
provide, when the type I error is α this means P(p-value &lt; α | zero
effect) = α.   Neither a single p-value nor α is the probability of a
decision error.   They are "what if" probabilities, **if** the effect is
zero.  The p-value for a single study is merely the probability that
data more extreme than ours would have been observed had the effect been
exactly zero and the experiment was capable of being re-run infinitely
often.  It is nothing more than this.  It is not a false positive
probability for the experiment at hand.  To compute the false positive
probability one would need a prior distribution for the effect (and for
the p-value to be perfectly accurate which is rare), and one might as
well be fully Bayesian and enjoy all the Bayesian benefits.
