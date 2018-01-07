+++
title = "Clinicians' Misunderstanding of Probabilities Makes Them Like Backwards Probabilities Such As Sensitivity, Specificity, and Type I Error"
date = 2017-01-25T06:16:00Z
updated = 2017-01-25T06:16:46Z
tags = ["specificity", "probability", "backward-probability", "forward-probability", "p-value", "bayes", "conditioning", "diagnosis", "decision-making", "dichotomization", "medicine", "bioinformatics", "biomarker", "sensitivity", "posterior"]
+++
Imagine watching a baseball game, seeing the batter get a hit, and hearing the
announcer say "The chance that the batter is left handed is now 0.2!"  
No one would care.  Baseball fans are interested in the chance that a
batter will get a hit conditional on his being right handed (handedness
being already known to the fan), the handedness of the pitcher, etc.
 Unless one is an archaeologist or medical examiner, the interest is in
forward probabilities conditional on current and past states.  We are
interested in the probability of the unknown given the known and the
probability of a future event given past and present conditions and
events.

Clinicians are people trained in the science and practice of medicine,
and most of them are very good at it.  They are also very good at many
aspects of research.  But they are generally not taught probability, and
this can limit their research skills.  Many excellent clinicians even
let their limitations in understanding probability make them believe
that their clinical decision making is worse than it actually is.  I
have taught many clinicians who say "I need a hard and fast rule so I
know how to diagnosis or treat patients.  I need a hard cutoff on blood
pressure, HbA1c, etc. so that I know what to do, and the fact that I
either treat or not treat the patient means that I don't want to
consider a probability of disease but desire a simple classification
rule."  This makes the clinician try to influence the statistician to
use inefficient, arbitrary methods such as categorization,
stratification, and matching.

In reality, clinicians do not act that way when treating patients.  They
are smart enough to know that if a patient has cholesterol just over
someone's arbitrary threshold they may not start statin therapy right
away if the patient has no other risk factors (e.g., smoking) going
against him.  They know that sometimes you start a patient on a lower
dose and see how she responds, or start one drug and try it for a while
and then switch drugs if the efficacy is unacceptable or there is a
significant side effect.

So I emphasize the need to understand probabilities when I'm teaching
clinicians.  A probability is a self-contained summary of the current
information, except for the patient's risk aversion and other utilities.
 Clinicians need to be comfortable with a probability of 0.5 meaning "we
don't know much" and not requesting a classification of disease/normal
that does nothing but cover up the problem.  A classification does not
account for gray zones or patient and physician utility functions.

Even physicians who understand the meaning of a probability are often
not understanding conditioning.  Conditioning is all important, and
conditioning on different things massively changes the meaning of the
probabilities being computed.  Every physician I've known has been
taught probabilistic medical diagnosis by first learning about
sensitivity (sens) and specificity (spec).  These are probabilities that
are in backwards time- and information flow order.  How did this happen?
Sensitivity, specificity, and receiver operating characteristic curves
were developed for radar and radio research in the military.  It was a
important to receive radio signals from distant aircraft, and to detect
an incoming aircraft on radar.  The ability to detect something that is
really there is definitely important.  In the 1950s, virologists
appropriated these concepts to measure the performance of viral
cultures.  Virus needs to be detected when it's present, and not
detected when it's not.  Sensitivity is the probability of detecting a
condition when it is truly present, and specificity is the probability
of not detecting it when it is truly absent.  One can see how these
probabilities would be useful outside of virology and bacteriology when
the samples are retrospective, as in a case-control studies.  But I
believe that clinicians and researchers would be better off if backward
probabilities were not taught or were mentioned only to illustrate how
**not** to think about a problem.

But the way medical students are educated, they assume that sens and
spec are what you first consider in a prospective cohort of patients!
 This gives the professor the opportunity of teaching  Bayes' rule and
requires the use of a supposedly unconditional probability known as
*prevalence* which is actually not very well defined.  The students
plugs everything into Bayes' rule and fails to notice that several
quantities cancel out.  The result is the following: the proportion of
patients with a positive test who have disease, and the proportion with
a negative test who have disease.  These are trivially calculated from
the cohort data without knowing anything about sens, spec, and Bayes.
 This way of thinking harms the student's understanding for years to
come and influences those who later engage in clinical and
pharmaceutical research to believe that type I error and p-values are
directly useful.

The situation in medical diagnosis gets worse when referral bias (also
called workup bias) is present.  When certain types of patients do not
get a final diagnosis, sens and spec are biased.  For example, younger
women with a negative test may not get the painful procedure that yields
the final diagnosis.  There are formulas that must be used to correct
sens and spec.  But wait!  When Bayes' rule is used to obtain the
probability of disease we needed in the first place, these corrections
completely cancel out when the usual correction methods are used!  Using
forward probabilities in the first place means that one just conditions
on age, sex, and result of the initial diagnostic test and no special
methods other than (sometimes) logistic regression are required.

There is an analogy to statistical testing.  p-values and type I error
are affected by sequential testing and a host of other factors, but
forward-time probabilities (Bayesian posterior probabilities) are not.
 Posterior probabilities condition on what is known and does not have to
imagine alternate paths to getting to what is known (as do sens and spec
when workup bias exists).  p-values and type I errors are
backwards-information-flow measures, and clinical researchers and
regulators come to believe that type I error is the error of interest.
 They also very frequently misinterpret p-values.  The p-value is one
minus spec, and power is sens.  The posterior probability is exactly
analogous to the probability of disease.

Sens and spec are so pervasive in medicine, bioinformatics, and
biomarker research that we don't question how silly they would be in
other contexts.  Do we dichotomize a response variable so that we can
compute the probability that a patient is on treatment B given a
"positive" response?  On the contrary we want to know the full
continuous distribution of the response given the assigned treatment.
Again this represents forward probabilities.


