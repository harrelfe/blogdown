+++
title = "Ideas for Future Articles"
date = 2017-01-16T10:52:00Z
modified = 2017-01-24T07:28:15Z
tags = ["2017"]
+++
Suggestions for future articles are welcomed as comments to this entry.
Some topics I intend to write about are listed below.

1.  Matching vs. covariate adjustment (see below from Arne Warnke)
1.  Statistical strategy for propensity score modeling and usage
1.  What is the full meaning of a posterior probability?
1.  Moving from pdf to html for statistical reporting
2.  Is machine learning statistics or computer science?
3.  Sample size calculation: Is it voodoo?
4.  Difference between Bayesian modeling and frequentist inference

------
A few weeks ago we had a small discussion at CrossValidated about the
pros and cons of
matching [here](http://stats.stackexchange.com/questions/248676).
I am sorry that I did not had enough time to elaborate further on the
support of matching procedures (in my field researchers do not focus
much on a bias-variance tradeoff but they prioritize on minimizing
biases. For that reason, they like matching
procedures).  Now, I have seen that you started a blog recently
(congratulations!).  I
would like to encourage to take up the topic of matching because it is
probably interesting for many applied researchers.
I think in your ‘philosophy’, this would belong to the point “Preserve
all the information in the
data”.  Here, perhaps some input for a blog post. Back then, you
wrote:

<small>
Matching on continuous variables results in an incomplete adjustment
because the variables have to be
binned.
</small>

What about propensity score
matching?

<small>
Matching throws away good data from observations that would be good
matches.

Extrapolation bias is only a significant problem if there is a
covariate by group interaction, and users of matching methods ignore
interactions
anyway.
</small>

Here, you go too far (in my view). You can add interactions, again
for example with propensity score matching. Imbens and Rubin (2015)
suggest a procedure using quadratic and interaction terms of the
covariates.

<small>Comment: Nice to know this exists but I've never seen a paper that used
matching attempt to explore interactions.  If you don't want to make regression assumptions that are unverifiable, remove observations outside the overlap region just as with matching.</small>

Which assumptions do you refer to? I think that treating everyone the
same (statistically) is also an unverifiable assumption (do you
disagree?). What is your opinion about weighted least
squares?

<small>
Comment: This is the
no-interaction assumption.  If you assume additivity then it's more OK
to have a no-overlap region, otherwise throw-away non-overlap regions
and do a conditional analysis.  Not clear on the need for weighting
here.  In general I like conditioning over
weighting.
</small>

Arne Jonas Warnke<br>
Labour Markets, Human Resources and Social Policy<br>
Internet: [zew.de](http://www.zew.de)
