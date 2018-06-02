+++
title = "Null Hypothesis Significance Testing Never Worked"
date = 2017-01-14T07:15:00Z
mosidiws = 2017-01-15T20:25:45Z
tags = ["logic", "inference", "bayes", "p-value", "hypothesis-testing", "inductive-reasoning", "2017"]
+++
Much has been written about problems with our most-used statistical
paradigm: frequentist null hypothesis significance testing (NHST),
p-values, type I and type II errors, and confidence intervals.
 Rejection of straw-man null hypotheses leads researchers to believe
that their theories are supported, and the unquestioning use of a
threshold such as p&lt;0.05 has resulted in hypothesis substitution,
search for subgroups, and other gaming that has badly damaged science.
 But we seldom examine whether the original idea of NHST actually
delivered on its goal of making good decisions about effects, given the
data.

NHST is based on something akin to proof by contradiction.  The best
non-mathematical definition of the p-value I've ever seen is due
to [Nicholas Maxwell](http://www.citeulike.org/user/harrelfe/article/14166520): "the
degree to which the data are embarrassed by the null hypothesis."
 p-values provide evidence against something, never in favor of
something, and are the basis for NHST.  But proof by contradiction is
only fully valid in the context of rules of logic where assertions are
true or false without any uncertainty.  The classic paper [The Earth is
Round
(p<.05)](http://www.citeulike.org/user/harrelfe/article/10529649) by
Jacob Cohen has a beautiful example pointing out the fallacy of
combining probabilistic ideas with proof by contradiction in an attempt
to make decisions about an effect.

> 
> The following is almost but not quite the reasononing of null
> hypothesis rejection: 

> If the null hypothesis is correct, then this datum (D) can not occur.
> It has, however, occurred.
> Therefore the null hypothesis is false. 

> If this were the reasoning of H<sub>0</sub> testing, then it would be formally
> correct. … But this is not the reasoning of NHST.  Instead, it makes
> this reasoning probabilistic, as follows: 

> If the null hypothesis is correct, then these data are highly
> unlikely.
> These data have occurred.
> Therefore, the null hypothesis is highly unlikely. 

> By making it probabilistic, it becomes invalid.  … the syllogism
> becomes formally incorrect and leads to a conclusion that is not
> sensible: 

> If a person is an American, then he is probably not a member of
> Congress.  (TRUE, RIGHT?)
> This person is a member of Congress.
> Therefore, he is probably not an American. (Pollard & Richardson,
> 1987) 

> … The illusion of attaining improbability or the Bayesian Id's wishful
> thinking error …
>   

> Induction has long been a problem in the philosophy of science.  Meehl
> (1990) attributed to the distinguished philosopher Morris Raphael
> Cohen the saying "All logic texts are divided into two parts.  In the
> first part, on deductive logic, the fallacies are explained; in the
> second part, on inductive logic, they are committed."

Sometimes when an approach leads to numerous problems, the approach
itself is OK and the problems can be repaired.  But besides all the
other problems caused by NHST (including need for arbitrary multiplicity
adjustments, need for consideration of investigator intentions and not
just her actions, rejecting H<sub>0</sub> for trivial effects, incentivizing
gaming, interpretation difficulties, etc.) it may be the case that the
overall approach is defective and should not have been adopted.

With all of the amazing things Ronald Fisher gave us, and even though he
recommended against the unthinking rejection of H<sub>0</sub>, his frequentist
approach and dislike of the Bayesian approach did us all a disservice.
 He called the Bayesian method invalid and was possibly intellectually
dishonest when he labeled it as "inverse probability."  In fact the
p-value is an indirect inverse probability and Bayesian posterior
probabilities are direct forwards probabilities that do not condition on
a hypothesis, and the Bayesian approach has not only been shown to be
valid, but it actually delivers on its promise.
