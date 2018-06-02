+++
title = "Scoring Multiple Variables, Too Many Variables and Too Few Observations: Data Reduction"
date = 2017-11-21T15:40:00Z
modified = 2017-11-26T07:27:29Z
tags = ["variability", "data-reduction", "2017"]
+++
This post will grow to cover questions about data reduction methods, also known as *unsupervised learning* methods. These are intended primarily for two
purposes:

-   collapsing correlated variables into an overall score so that one
    does not have to disentangle correlated effects, which is a
    difficult statistical task
-   reducing the effective number of variables to use in a regression or
    other predictive model, so that fewer parameters need to be
    estimated

The latter example is the "too many variables too few subjects" problem.
 Data reduction methods are covered in Chapter 4 of my book *Regression
Modeling Strategies*, and in some of the book's case studies.

------------------------------------------------------------------------

### Sacha Varin writes
<small>
I want to add/sum some variables having different units. I decide to
standardize (Z-scores) the values and then, once transformed in
Z-scores, I can sum them.  The problem
is that my variables distributions are non Gaussian (my distributions
are not symmetrical (skewed), they are long-tailed, I have all types of
weird distributions, I guess we can say the distributions are
intractable.  I know that my distributions don't need
to be gaussian to calculate Z-scores, however, if the distributions are
not close to gaussian or at least symmetrical enough, I guess the
classical Z-score transformation: (Value - Mean)/SD is not valid, that's why I decide, because my distributions are skewed and long-tailed to use the Gini's mean difference (robust and efficient
estimator). 

1.  If the distributions are skewed and long-tailed, can I standardize
    the values using that formula (Value - Mean)/GiniMd ?  Or the mean is not a good estimator in presence of skewed and long-tailed distributions?  What
    about (Value - Median)/GiniMd ?  Or what else with
    GiniMd for a formula to standardize?
2.  In presence of outliers, skewed and long-tailed distributions, for
    standardization, what formula is better to use
    between (Value - Median)/MAD (=median
    absolute deviation) or Value - Mean)/GiniMd ?  And
    why?  My situation is not the predictive modeling case, but I want to sum the variables.
</small>

------------------------------------------------------------------------

These are excellent questions and touch on an interesting side issue.
 My opinion is that standard deviations (SDs) are not very applicable to
asymmetric (skewed) distributions, and that they are not very robust
measures of dispersion.  I'm glad you mentioned [Gini's mean
difference](https://arxiv.org/pdf/1405.5027.pdf), which is the mean of
all absolute differences of pairs of observations.  It is highly robust
and is surprisingly efficient as a measure of dispersion when compared
to the SD, even when normality
holds.

The questions also touch on the fact that when normalizing more than
one variable so that the variables may be combined, there is no magic
normalization method in statistics.  I believe that Gini's mean
difference is as good as any and better than the SD.  It is also more
precise than the mean absolute difference from the mean or median, and
the mean may not be robust enough in some instances.  But we have a rich
history of methods, such as principal components (PCs), that use
SDs.

What I'm about to suggest is a bit more
applicable to the case where you ultimately want to form a predictive
model, but it can also apply when the goal is to just combine several
variables.  When the variables are continuous and are on different
scales, scaling them by SD or Gini's mean difference will allow one to
create unitless quantities that may possibly be added.  But the fact
that they are on different scales begs the question of whether they are
already "linear" or do they need separate nonlinear transformations to
be "combinable".

I think that nonlinear PCs may be a better choice than just adding
scaled variables.  When the predictor variables are correlated,
nonlinear PCs learn from the interrelationships, even occasionally
learning how to optimally transform each predictor to ultimately better
predict Y.  The transformations (e.g., fitted spline functions) are
solved for to maximize predictability of a predictor, from the other
predictors or PCs of them.  Sometimes the way the predictors move
together is the same way they relate to some ultimate outcome variable
that this undersupervised learning method does not have access to.  An
example of this is in Section 4.7.3 of my book.

With a little bit of luck, the transformed predictors have more
symmetric distributions, so ordinary PCs computed on these transformed
variables, with their implied SD normalization, work pretty well.  PCs
take into account that some of the component variables are highly
correlated with each other, and so are partially redundant and should
not receive the same weights ("loadings") as other
variables.

The R transcan function in the Hmisc package has various options for nonlinear PCs, and these ideas are generalized in the R
[homals](https://cran.r-project.org/web/packages/homals)
package.

How do we handle the case where the number of candidate predictors p is
large in comparison to the effective sample size n?  Penalized maximum
likelihood estimation (e.g., ridge regression) and Bayesian regression
typically have the best performance, but data reduction methods are
competitive and sometimes more interpretable.  For example, one can use
variable clustering and redundancy analysis as detailed in the RMS book
and course notes.  Principal components (linear or nonlinear) can also
be an excellent approach to lowering the number of variables than need
to be related to the outcome variable Y.  Two example approaches
are:

1.  Use the 15:1 rule of thumb to estimate how many predictors can
    reliably be related to Y.  Suppose that number is k.  Use the first
    k principal components to predict Y.
2.  Enter PCs in decreasing order of variation (of the system of Xs)
    explained and chose the number of PCs to retain using AIC.  This is
    far from stepwise regression which enters variables according to
    their p-values with Y.  We are effectively entering variables in a
    pre-specified order with incomplete principal component
    regression.

Once the PC model is formed, one may attempt to interpret the model by
studying how raw predictors relate to the principal components or to the
overall predicted values.

Returning to Sacha's original setting,
if linearity is assumed for all variables, then scaling by Gini's mean
difference is reasonable.  But psychometric properties should be
considered, and often the scale factors need to be derived from subject
matter rather than statistical
considerations.
