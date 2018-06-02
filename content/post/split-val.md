+++
title = "Split-Sample Model Validation"
date = 2017-01-23
modified = 2018-04-13
tags = ["prediction", "bootstrap", "validation", "2017"]
+++
Methods used
to obtain unbiased estimates of future performance of statistical
prediction models and classifiers include data splitting and resampling.
 The two most commonly used resampling methods are cross-validation and
bootstrapping.  To be as good as the bootstrap, about 100 repeats of
10-fold cross-validation are required.

As discussed in more detail in Section 5.3 of [Regression Modeling
Strategies Course Notes](http://fharrell.com/doc/rms.pdf) and the
same section of the RMS book, data splitting is an unstable method for
validating models or classifiers, especially when the number of subjects
is less than about 20,000 (fewer if signal:noise ratio is high).  This
is because were you to split the data again, develop a new model on the
training sample, and test it on the holdout sample, the results are
likely to vary significantly.   Data splitting requires a significantly
larger sample size than resampling to work acceptably well.  See also
Section 10.11 of [BBR](http://fharrell.com/links).

There are also very subtle problems:


1.  When feature selection is done, data splitting validates just one of
    a myriad of potential models.  In effect it validates an example
    model.  Resampling (repeated cross-validation or the bootstrap)
    validate the process that was used to develop the model.  Resampling
    is honest in reporting the results because it depicts the
    uncertainty in feature selection, e.g., the disagreements in which
    variables are selected from one resample to the next.
2.  It is not uncommon for researchers to be disappointed in the test
    sample validation and to ask for a "re-do" whereby another split is
    made or the modeling starts over, or both.  When reporting the final
    result they sometimes neglect to mention that the result was the
    third attempt at validation.
3.  Users of split-sample validation are wise to recombine the two
    samples to get a better model once the first model is validated.
     But then they have no validation of the new combined data model.

There is a less subtle problem but one that is ordinarily not addressed
by investigators: unless both the training and test samples are huge,
split-sample validation is not nearly as accurate as the bootstrap.  See
for example the section *Studies of Methods Used in the
Text* [here](http://biostat.mc.vanderbilt.edu/rms).  As shown in a
simulation appearing there, bootstrapping is typically more accurate
than data splitting and cross-validation that does not use a large
number of repeats.  This is shown by estimating the "true" performance,
e.g., the R<sup>2</sup> or c-index on an infinitely large dataset (infinite
here means 50,000 subjects for practical purposes).  The performance of
an accuracy estimate is taken as the mean squared error of the estimate
against the model's performance in the 50,000 subjects.

Data are too precious to not be used in model development/parameter
estimation.  Resampling methods allow the data to be used for both
development and validation, and they do a good job in estimating the
likely future performance of a model.  Data splitting only has an
advantage when the test sample is held by another researcher to ensure
that the validation is unbiased.

Many investigators have been told that they must do an "external"
validation, and they split the data by time or geographical location.
 They are sometimes surprised that the model developed in one country or
time does not validate in another.  They should not be; this is an
indirect way of saying there are time or country effects.  Far better
would be to learn about and estimate time and location effects by
including them in a unified model.  Then rigorous internal validation
using the bootstrap, accounting for time and location all along the way.
 The end result is a model that is useful for prediction at times and
locations that were at least somewhat represented in the original
dataset, but without assuming that time and location effects are nil.

## Other Resources
* How to choose between [internal and external validation](/doc/bbr.pdf#nameddest=reg-choose-val)

