---
title: Why I Don't Like Percents
author: Frank Harrell
date: '2018-01-19'
updated: '2018-04-14'
slug: percent
draft: false
categories: []
tags:
  - metrics
  - 2018
summary: 'I prefer fractions and ratios over percents.  Here are the reasons.'
header:
  caption: ''
  image: 'headers/percent.jpg'
---
The numbers zero and one are special; zero because it is a minimum or center point for many measurements and because it is the addition identity constant (x + 0 = x), and one because it is the multiplication identity constant (x × 1 = x) and corresponds to units of measurements.  Many important quantities are between 0 and 1, including proportions of a whole and probabilities.  One hundred is not special in the same sense as unity, so percent (per 100) doesn't do anything for me (why not per thousand?).
<style>
img {
  height: auto;
  max-width: 70px;
  margin-left: auto;
  margin-right: auto;
  display: block;
}
</style>

When a quantity doubles, it gets back to its original value by halving.  When in increases by 100% it gets back to its original value by decreasing 50%.  Case almost closed.  Whereas an increase of 33.33% is balanced by a decrease of 25%, an increase by a factor of 4/3 is balanced by a decrease to a factor of 3/4 .  If you put 100 dollars into an account that yields 3% interest annually, you will have 100 * (1.03<sup>10</sup>) or 134 dollars after 10 years.  To get back to your original value you'd have to lose 2.91% per year for 10 years.

I like fractions like 3/4, or the decimal equivalent 0.75.  I like ratios, because they are symmetric.  Chaining together relative increases is simple with ratios.  An increase by a factor of 1.5 followed by an increase by a factor of 1.4 is an increase by a factor of 1.5 * 1.4 or 2.1.  A 50% increase followed by a 40% increase is an increase of 110%.  To get the right answer with percent increase you have to convert back to ratios, do the multiplication, then convert back to percent.

Many numbers that we quote are probabilities, and a probability is formally a number between 0 and 1.  So I don't like "the chance of rain is 10%" but prefer "the chance of rain is 0.1 or 1/10".  When discussing statistical analyses it is especially irksome to see statements such as "significance levels of 5% or power of 90%".  Probabilities are being discussed, so I prefer 0.05 and 0.9.

I have seen clinicians confused over statements such as "the chance of a stroke is 0.5%", interpreting this as 50%.  If we say "the chance of a stroke is 0.005" such confusion is less likely.  And I don't need percent signs everywhere.

Percent change has even more problems than percent.  I have often witnessed confusion from statements such as "the chance of stroke increased by 50%".  If the base stroke probability was 0.02 does the speaker mean that it is now 0.52?  Not very likely, but you can't be sure.  More likely she meant that the chance of stroke is now 0.02 + 0.5 * 0.02 = 0.03.  It would always be clear to instead say one of the following:

- The chance of stroke went from 0.02 to 0.03
- The chance of stroke increased by 0.01 (or the *absolute* chance of stroke increased by 0.01)
- The chance of stroke increased by a factor of 1.5

We need to achieve clarity by settling on a convention for wording fold-change decreases.  If the chance of stroke decreases from 0.03 to 0.02 and we feel compelled to summarize the *relative* decrease in risk, we could say that risk of stroke decreased by a factor of 1.5.  But even though it looks a bit awkward, I think it would be clearest to say the following, if 0.02 corresponded to treatment A and 0.03 corresponded to treatment B: treatment A multiplied the risk of stroke by 2/3 in comparison to treatment B.  Or you could say that treatment A modified the risk of stroke by a factor of 2/3, or that the A:B risk ratio is 2/3 or 0.667.

Many quantities reported in the scientific literature are naturally ratios.  For example, odds ratios and hazard ratios are commonly used.  If the ratio of stroke hazard rates treatment B compared to treatment A is 0.75, I prefer to report "the B:A stroke hazard ratio was 0.75."  There's no need to say that there was a 25% reduction in stroke hazard rate.

Percents have perhaps one good use.  When they represent fractions and we don't care to present but two decimal places of accuracy, i.e., the percents you calculate are all whole numbers, percents may be OK.  But I would still prefer numbers like 0.02, 0.86 and to avoid a symbol (%) when just dealing with numbers.

## Links to Other Resources
* [What is a percentage difference?](https://www.bmj.com/content/358/bmj.j3663) by TJ Cole and DG Altman
