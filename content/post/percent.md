---
title: Why I Don't Like Percents
author: Frank Harrell
date: '2018-01-14'
slug: percent
draft: true
categories: []
tags:
  - metrics
summary: "I prefer fractions and ratios over percents.  Here are the reasons."
header:
  caption: ''
  image: 'headers/percent.jpg'
---
<style>
img {
  height: auto;
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
  display: block;
}
</style>

When a quantity doubles, it gets back to its original value by halving.  When in increases by 100% it gets back to its original value by decreasing 50%.  Case almost closed.

I like fractions like 3/4 , or the decimal equivalent 0.75.  Ratios are symmetric.  Whereas an increase of 33.33% is balanced by a decrease of 25%, an increase by a factor of 4/3 is balanced by a decrease to a factor of 3/4 .  If you put 100 dollars into an account that yields 3% interest annually, you will have 100 * (1.03<sup>10</sup>) or 134 dollars
after 10 years.  To get back to your original value you'd have to lose 2.91% per year.

Many numbers that we quote are probabilities, and a probability is formally a number between 0 and 1.  So I don't like "the chance of rain is 10%" but prefer "the chance of rain is 0.1 or 1/10".  When discussing statistical analyses it is especially irksome to see statements such as "significance levels of 5% or power of 90%".  Probabilities are being discussed, so I prefer 0.05 and 0.9.

I have seen clinicians confused over statements such as "the chance of a stroke is 0.5%", interpreting this as 50%.  If we say "the chance of a stroke is 0.005" such confusion is less likely.  And I don't need percent signs everywhere.

Percent change has even more problems than percent.  I have often witness confusion from statements such as "the chance of stroke increased by 50%".  If the base stroke probability was 0.02 does the speaker mean that it is now 0.52?  Not very likely, but you can't be sure.  More likely she meant that the chance of stroke is now 0.02 + 0.5 * 0.02 = 0.03.  It would always be clear to instead say one of the following:

- The chance of stroke went from 0.02 to 0.03
- The chance of stroke increased by 0.01
- The chance of stroke increased by a factor of 1.5

It can be a bit confusing when selecting wording for a fold-change decrease.  If the chance of stroke decreased from 0.03 to 0.02, the clearest statement (other than just giving the two chances) would be "the chance of stroke increased by a factor of 2/3" but by convention we say "the chance of stroke decreased by a factor of 2/3".  To be perfectly clear we could use the form "the chance of stroke was multiplied by 2/3" or "changed by a factor of 2/3".

Note that chaining together relative increases is simple with ratios.  An increase by a factor of 1.5 followed by an increase by a factor of 1.4 is an increase by a factor of 1.5 * 1.4 or 2.1.  A 50% increase followed by a 40% increase is an increase of 110%.
