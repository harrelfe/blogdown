---
title: Implications of Interactions in Treatment Comparisons
author: Frank Harrell
date: '2020-03-03'
modified: '2020-07-19'
slug: ia
categories: []
tags:
  - RCT
  - drug-evaluation
  - generalizability
  - medicine
  - observational
  - personalized-medicine
  - prediction
  - subgroup
  - 2020
summary: 'This article explains how the generalizability of randomized trial findings depends primarily on whether and how patient characteristics modify (interact with) the treatment effect.  For an observational study this will be related to overlap in the propensity to receive treatment.' 
header:
  caption: ''
  image: ''
output:
  blogdown::html_page:
    fig_width: 7
    fig_height: 5
    dev: "svg"
---


<style>
summary {
 font-size:70%;
 color:DarkBlue;
 background-color:#FFF8DC;
 margin-left: -25%;
 word-wrap:break-word;
 width:23%;
 white-space: normal;
 }
body, html {
    margin-left:0%;
    margin-right:0%;
    width:100%;
} 
span.lm {
  display:block;
  margin-left: -25%;
  font-size:70%;
  color:DarkBlue;
	word-wrap:break-word;
	width: 23%;
	white-space: normal;
}

#leftbox { 
  float:left;  
  background:#FFF8DC;
	margin-left:-25%;
  width:23%;
	font-size:70%;
	color:DarkBlue;
  } 
#rightbox{ 
  float:right; 
  width:100%; 
	}
div.clear{clear:both}
</style>  

<style type="text/css">
#TOC {
    margin: 25px 0px 20px 0px;
}
div.tocify {
    width: 20%;
    max-width: 260px;
    max-height: 85%;
}
.tocify {
    width: 20%;
    max-height: 90%;
    overflow: auto;
    margin-left: 2%;
    position: fixed;
    border: 1px solid #ccc;
    border-radius: 6px;
}
</style>

<p class="rquote">
Transportability of treatment effect estimates depends on the nature of interactions.  In the absence of interactions, an effect estimated on a highly selected sample will apply to a much different population.  In an observational study, the corresponding condition is that there need be no overlap in the baseline distribution of non-interacting factors.  When there is an interaction, one can live with only a small to moderate amount of overlap in characteristics (between randomized vs. population target or between characteristics of treated vs. non-treated patients in an observational study) <b>if</b> the interaction is of a simple form.  With more overlap, interactions can be complex (if modeled) and results from analysis of the sample will allow estimation of treatment effect in a different population.  In the absence of significant overlap, confidence bands allowing for interaction properly inform the researcher about uncertainties in treatment effects.<br><br>Randomized clinical trials do not require representative patients; they require representative treatment effects.  Generalizability of randomized trial findings for relative efficacy comes from one of three things: (1) true absence of interactions, (2) interacting factors have a similar distribution in the RCT as in the target population, or (3) the RCT sample has enough representation of the distribution of interacting factors to allow them to be modeled and used to estimate treatment effects in target patients, and the researcher knows to include these interactions in her model.
</p>

<b>Note</b>: For shaded boxes marked with ⮩ click on the box to view the associated text.

<details><summary>Table of Contents ⮩</summary>
<div id="boxes"><div id="leftbox">
Hyperlinking to a section from the table of contents will not work unless that section is currently expanded.
</div><div id="rightbox">
{{% toc %}}
</div></div><div class="clear"></div>
</details>
  
# Background

It is a commonly held belief that clinical trials, to provide treatment effects that are generalizable to a population, must use a sample that reflects that population's characteristics.  The confusion stems from the fact that if one were interested in estimating an average outcome for patients given treatment A, one would need a random sample from the target population.  But clinical trials are not designed to estimate _absolutes_; they are designed to estimate _differences_ as discussed further [here](http://fharrell.com/post/rct-mimic).   These differences, when measured on a scale for which treatment differences are allowed mathematically to be constant (e.g., difference in means, odds ratios, hazard ratios), show remarkable constancy as judged by a large number of published forest plots.   What would make a treatment estimate (relative efficacy) not be transportable to another population?  A requirement for non-generalizability is the existence of interactions with treatment such that the interacting factors have a distribution in the sample that is much different from the distribution in the population.

A related problem is the issue of overlap in observational studies.   Researchers are taught that non-overlap makes observational treatment comparisons impossible.  This is only true when the characteristic whose distributions don't overlap between treatment groups interacts with treatment.  The purpose of this article is to explore interactions in these contexts.

As a side note, if there is an interaction between treatment and a covariate, standard propensity score analysis will completely miss it.

For the following I assume that there is at most one variable (patient age) interacting with treatment effect.  The data generating and analysis models are logistic models containing only age and treatment.  I also assume that the goal is to assess efficacy isolated from net clinical benefit, e.g., we are not taking side effects into account.

Situations addressed below come from the following 2 × 2 × 2 × 3 combinations:

* randomized vs. observational study
* true data generating process has no interaction with age vs. has a linear interaction
* overlap in ages between RCT sample and target population (or between treatments in an observational study) is completely absent vs. partial overlap in the age distribution
* fitted model is linear with no interaction, has a linear interaction, or has a spline function of age interacting with treatment

I'll explore what happens when interaction should be included in a model but is omitted, when an interaction is not needed but is included in the model, and when the interaction is simple but is allowed to be complex.




# Generalizability of Randomized Clinical Trials



When the outcome generating process in a randomized clinical trial (RCT) is such that treatment does not interact with any baseline patient characteristic over the range of characteristics seen in the union of the RCT and the population, the estimate of relative efficacy (e.g., odds ratio) of treatment applies to every patient in the population.  In addition to computing patient-specific odds ratios (ORs), one can easily compute patient-specific absolute risk reductions from treatment as shown [here](https://www.fharrell.com/post/ehrs-rcts).  If one allows for needless interaction terms, it is possible to get poor extrapolation of treatment benefit in unrepresented patient groups, unless the estimated interaction effects are very small (not guaranteed until the sample size is very large).



## True Model Has No Treatment Interactions



### Estimation of Treatment Effect in Population With No Overlap



As an example, consider a simulated study of about 2500 patients with a binary outcome where the model contains only age and treatment, the age effect is linear, and there is no interaction.  The following R code simulates the data from a hypothetical population, fits a binary logistic model, and displays estimated log odds of outcome vs. age and treatment (left panel) and the estimated age-specific treatment effect (right panel), along with the true population values for both panels over the whole range of age.  The RCT excluded those with age < 50 but the target population is exactly those patients.  Tick marks on fitted curves display treatment-specific raw data spike histograms.

The original age range is 13.3 - 88.1 in 5000 subjects, and the clinical trial includes 2461 of the subjects.<br>

Age distributions in the target population as compared to the RCT sample are shown below.


<img src="/post/ia_files/figure-html/main-1.svg" width="500" />



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 211.90</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.117</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.679</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1735</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.773</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.357</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 726</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.167</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.357</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.150</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.190</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.3508</td>
<td style='min-width: 7em; text-align: right;'> 0.4387</td>
<td style='min-width: 7em; text-align: right;'> -9.92</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.0992</td>
<td style='min-width: 7em; text-align: right;'> 0.0959</td>
<td style='min-width: 7em; text-align: right;'>-11.46</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0674</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0074</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  9.05</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>131.30</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'> 81.95</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>189.63</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-2.svg" width="672" />



Extrapolation to the younger population is fine even with no younger patients in the RCT.  Confidence bands are computed under the assumption of an additive linear age effect.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This model included an unnecessary linear interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 212.02</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.117</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.679</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1735</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.778</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.357</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 726</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.176</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.357</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 5×10<sup>-6</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.150</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.190</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.2193</td>
<td style='min-width: 7em; text-align: right;'> 0.5852</td>
<td style='min-width: 7em; text-align: right;'>-7.21</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.3998</td>
<td style='min-width: 7em; text-align: right;'> 0.8939</td>
<td style='min-width: 7em; text-align: right;'>-1.57</td>
<td style='min-width: 7em; text-align: right;'>0.1174</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0652</td>
<td style='min-width: 7em; text-align: right;'> 0.0100</td>
<td style='min-width: 7em; text-align: right;'> 6.53</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0051</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0150</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.34</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.7351</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>131.11</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.7351</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 82.19</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.7351</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.7351</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>188.87</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-3.svg" width="672" />



Note that the close-to-zero estimated interaction in the trial sample led to an appropriate extrapolation to the target population of age < 50, with the confidence bands getting wider.

Now see what happens when age is unnecessarily allowed to have a nonlinear non-additive effect, by fitting a model that is quadratic in age interacting with treatment.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model included unnecessary linear and nonlinear interaction terms.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 214.13</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.119</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.679</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1735</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 7</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.771</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.358</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 726</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.162</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.358</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-5</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.149</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.190</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.0988</td>
<td style='min-width: 7em; text-align: right;'> 1.2779</td>
<td style='min-width: 7em; text-align: right;'>-2.42</td>
<td style='min-width: 7em; text-align: right;'>0.0153</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.3609</td>
<td style='min-width: 7em; text-align: right;'> 2.0910</td>
<td style='min-width: 7em; text-align: right;'>-0.65</td>
<td style='min-width: 7em; text-align: right;'>0.5151</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0445</td>
<td style='min-width: 7em; text-align: right;'> 0.0231</td>
<td style='min-width: 7em; text-align: right;'> 1.93</td>
<td style='min-width: 7em; text-align: right;'>0.0542</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'>  0.2124</td>
<td style='min-width: 7em; text-align: right;'> 0.2213</td>
<td style='min-width: 7em; text-align: right;'> 0.96</td>
<td style='min-width: 7em; text-align: right;'>0.3372</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age''</td>
<td style='min-width: 7em; text-align: right;'> -0.4384</td>
<td style='min-width: 7em; text-align: right;'> 0.4906</td>
<td style='min-width: 7em; text-align: right;'>-0.89</td>
<td style='min-width: 7em; text-align: right;'>0.3716</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'>  0.0043</td>
<td style='min-width: 7em; text-align: right;'> 0.0376</td>
<td style='min-width: 7em; text-align: right;'> 0.11</td>
<td style='min-width: 7em; text-align: right;'>0.9087</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; text-align: right;'>  0.0067</td>
<td style='min-width: 7em; text-align: right;'> 0.3136</td>
<td style='min-width: 7em; text-align: right;'> 0.02</td>
<td style='min-width: 7em; text-align: right;'>0.9829</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age''</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0141</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.6574</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-0.02</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.9829</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>131.33</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.9910</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 85.26</td>
<td style='padding-left:3ex; text-align: right;'>6</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.9910</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  2.11</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.7151</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  0.11</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.9910</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.00</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.9998</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.00</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.9998</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  2.11</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.7151</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  2.23</td>
<td style='padding-left:3ex; text-align: right;'>5</td>
<td style='padding-left:3ex; text-align: right;'>0.8162</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>190.78</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>7</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-4.svg" width="672" />



By allowing not only an unnecessary interaction but also allowing the interaction to be nonlinear, when the predicted values are for a region completely outside the sample data, the extrapolation is still reasonable but the (honest) confidence intervals are much wider.  We don't know much at all about the relative treatment effect with age < 50 when we allowed the two treatments to have differently shaped age effects in the trial data.  A Bayesian model that put a skeptical prior on either the nonlinear or the interaction effect would have credible intervals that are not so wide on the left.



<br></div></div><div class="clear"></div><br>



</details>



### RCT Sample Partially Overlaps with Target Population



Instead of having non-overlapping age distributions between the RCT sample and the target population, let's include screened patients with probabilities that are functions of age as shown in the first graph below.  Then again consider the simplest logistic model.


<img src="/post/ia_files/figure-html/main-5.svg" width="500" />

The original age range is 13.3 - 88.1 in 5000 subjects, and the clinical trial includes 3556 of the subjects.<br>

Age distributions in the target population as compared to the RCT sample are shown below.


<img src="/post/ia_files/figure-html/main-6.svg" width="500" />



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 329.69</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.131</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.697</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2658</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.856</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.394</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 898</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.353</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.394</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 4×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.148</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.171</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.4121</td>
<td style='min-width: 7em; text-align: right;'> 0.2941</td>
<td style='min-width: 7em; text-align: right;'>-15.00</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.0470</td>
<td style='min-width: 7em; text-align: right;'> 0.0846</td>
<td style='min-width: 7em; text-align: right;'>-12.38</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0682</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0052</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 13.04</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>153.17</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>169.92</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>286.93</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-7.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This model included an unnecessary linear interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 330.73</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.131</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.697</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2658</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.840</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.394</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 898</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.317</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.394</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 6×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.147</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.171</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.6606</td>
<td style='min-width: 7em; text-align: right;'> 0.3849</td>
<td style='min-width: 7em; text-align: right;'>-12.11</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -0.4360</td>
<td style='min-width: 7em; text-align: right;'> 0.6056</td>
<td style='min-width: 7em; text-align: right;'> -0.72</td>
<td style='min-width: 7em; text-align: right;'>0.4715</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0726</td>
<td style='min-width: 7em; text-align: right;'> 0.0069</td>
<td style='min-width: 7em; text-align: right;'> 10.56</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0108</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0106</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -1.02</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.3089</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>154.98</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.04</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3089</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>169.90</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.04</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3089</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  1.04</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3089</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>292.05</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-8.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model included unnecessary linear and nonlinear interaction terms.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 333.43</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.132</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.697</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2658</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 7</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.834</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.395</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 898</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.302</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.395</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-11</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.149</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.149</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.171</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.7933</td>
<td style='min-width: 7em; text-align: right;'> 0.5949</td>
<td style='min-width: 7em; text-align: right;'>-8.06</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.6350</td>
<td style='min-width: 7em; text-align: right;'> 0.9700</td>
<td style='min-width: 7em; text-align: right;'> 0.65</td>
<td style='min-width: 7em; text-align: right;'>0.5127</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0752</td>
<td style='min-width: 7em; text-align: right;'> 0.0113</td>
<td style='min-width: 7em; text-align: right;'> 6.67</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.0274</td>
<td style='min-width: 7em; text-align: right;'> 0.1673</td>
<td style='min-width: 7em; text-align: right;'>-0.16</td>
<td style='min-width: 7em; text-align: right;'>0.8699</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age''</td>
<td style='min-width: 7em; text-align: right;'>  0.0392</td>
<td style='min-width: 7em; text-align: right;'> 0.4011</td>
<td style='min-width: 7em; text-align: right;'> 0.10</td>
<td style='min-width: 7em; text-align: right;'>0.9221</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0319</td>
<td style='min-width: 7em; text-align: right;'> 0.0183</td>
<td style='min-width: 7em; text-align: right;'>-1.74</td>
<td style='min-width: 7em; text-align: right;'>0.0820</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; text-align: right;'>  0.2854</td>
<td style='min-width: 7em; text-align: right;'> 0.2321</td>
<td style='min-width: 7em; text-align: right;'> 1.23</td>
<td style='min-width: 7em; text-align: right;'>0.2188</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age''</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.5634</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.5254</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-1.07</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.2836</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>155.86</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  3.03</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.3873</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>175.89</td>
<td style='padding-left:3ex; text-align: right;'>6</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  3.03</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.3873</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  2.73</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.6041</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  3.03</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.3873</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.93</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.3813</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.93</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.3813</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  2.73</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.6041</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  3.82</td>
<td style='padding-left:3ex; text-align: right;'>5</td>
<td style='padding-left:3ex; text-align: right;'>0.5755</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>299.44</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>7</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-9.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



## Case Where Treatment Truly Interacts with Age



### Estimation of Treatment Effect in Population With No Overlap



Next turn to the case where the true data generating model has a linear treatment by age interaction which we may or may not include in our model.  The true model has a treatment effect of -1.0 for age 50 patients, and each year below 50 results in a further reduction by 0.03 in the effect.



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This model failed to include a needed interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 257.32</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.144</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.701</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1787</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.888</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.403</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 674</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.429</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.403</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 9×10<sup>-6</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.161</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.160</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.178</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.8181</td>
<td style='min-width: 7em; text-align: right;'> 0.4509</td>
<td style='min-width: 7em; text-align: right;'> -8.47</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.4027</td>
<td style='min-width: 7em; text-align: right;'> 0.1020</td>
<td style='min-width: 7em; text-align: right;'>-13.76</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0583</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0077</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  7.61</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>189.24</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'> 57.92</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>223.45</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-10.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 258.51</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.144</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.702</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1787</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.873</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.403</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 674</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.394</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.403</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.162</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.160</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.178</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.2193</td>
<td style='min-width: 7em; text-align: right;'> 0.5852</td>
<td style='min-width: 7em; text-align: right;'>-7.21</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -0.3879</td>
<td style='min-width: 7em; text-align: right;'> 0.9367</td>
<td style='min-width: 7em; text-align: right;'>-0.41</td>
<td style='min-width: 7em; text-align: right;'>0.6788</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0652</td>
<td style='min-width: 7em; text-align: right;'> 0.0100</td>
<td style='min-width: 7em; text-align: right;'> 6.53</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0171</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0157</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-1.09</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.2765</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>191.47</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.2765</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 58.35</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.2765</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  1.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.2765</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>228.30</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-11.svg" width="672" />



Because of the more limited age range in the trial there was insufficient power to provide definitive statistical evidence for an interaction, but the point estimate for the interaction effect is not unreasonable.  Though confidence bands are wide because of no overlap, the extrapolated treatment effects are reasonable as a result.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model correctly captures linear interaction, but also allows for unnecessary nonlinear interaction.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 2539    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2461</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 261.24</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.146</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.702</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1787</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 7</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.866</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.403</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 674</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.377</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.404</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 4×10<sup>-13</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.161</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.161</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.178</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.0988</td>
<td style='min-width: 7em; text-align: right;'> 1.2779</td>
<td style='min-width: 7em; text-align: right;'>-2.42</td>
<td style='min-width: 7em; text-align: right;'>0.0153</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.1305</td>
<td style='min-width: 7em; text-align: right;'> 2.1946</td>
<td style='min-width: 7em; text-align: right;'> 0.06</td>
<td style='min-width: 7em; text-align: right;'>0.9526</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0445</td>
<td style='min-width: 7em; text-align: right;'> 0.0231</td>
<td style='min-width: 7em; text-align: right;'> 1.93</td>
<td style='min-width: 7em; text-align: right;'>0.0542</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'>  0.2124</td>
<td style='min-width: 7em; text-align: right;'> 0.2213</td>
<td style='min-width: 7em; text-align: right;'> 0.96</td>
<td style='min-width: 7em; text-align: right;'>0.3372</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age''</td>
<td style='min-width: 7em; text-align: right;'> -0.4384</td>
<td style='min-width: 7em; text-align: right;'> 0.4906</td>
<td style='min-width: 7em; text-align: right;'>-0.89</td>
<td style='min-width: 7em; text-align: right;'>0.3716</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0267</td>
<td style='min-width: 7em; text-align: right;'> 0.0396</td>
<td style='min-width: 7em; text-align: right;'>-0.68</td>
<td style='min-width: 7em; text-align: right;'>0.4996</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; text-align: right;'>  0.0955</td>
<td style='min-width: 7em; text-align: right;'> 0.3313</td>
<td style='min-width: 7em; text-align: right;'> 0.29</td>
<td style='min-width: 7em; text-align: right;'>0.7731</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age''</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.1934</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.6939</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-0.28</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.7805</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>191.14</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.19</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.7545</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 61.60</td>
<td style='padding-left:3ex; text-align: right;'>6</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.19</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.7545</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  2.67</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.6150</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  1.19</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.7545</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.08</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.9590</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.08</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.9590</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  2.67</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.6150</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  3.78</td>
<td style='padding-left:3ex; text-align: right;'>5</td>
<td style='padding-left:3ex; text-align: right;'>0.5816</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>229.68</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>7</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-12.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



### RCT Sample Partially Overlaps with Target Population



Sticking with the true treatment effect interacting with age, we generate data with the same partial overlap as before, and repeat the same analyses.  We start by fitting a model that is oblivious to interaction.



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This model failed to include a needed interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 323.95</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.130</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.699</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2702</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.870</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.397</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 854</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.387</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.397</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 1×10<sup>-11</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.145</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.145</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.165</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.7735</td>
<td style='min-width: 7em; text-align: right;'> 0.2939</td>
<td style='min-width: 7em; text-align: right;'>-12.84</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.2305</td>
<td style='min-width: 7em; text-align: right;'> 0.0876</td>
<td style='min-width: 7em; text-align: right;'>-14.04</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0566</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0052</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 10.80</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>197.20</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>116.73</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>281.79</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-13.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 338.39</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.136</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.700</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2702</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.818</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.400</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 854</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.265</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.401</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 8×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.145</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.146</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.164</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.6606</td>
<td style='min-width: 7em; text-align: right;'> 0.3849</td>
<td style='min-width: 7em; text-align: right;'>-12.11</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  1.0820</td>
<td style='min-width: 7em; text-align: right;'> 0.6129</td>
<td style='min-width: 7em; text-align: right;'>  1.77</td>
<td style='min-width: 7em; text-align: right;'>0.0775</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0726</td>
<td style='min-width: 7em; text-align: right;'> 0.0069</td>
<td style='min-width: 7em; text-align: right;'> 10.56</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0412</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0109</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -3.79</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.0002</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>210.59</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 14.36</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>125.47</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 14.36</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 14.36</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>310.44</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-14.svg" width="672" />



The amount of interaction estimated from the larger older sample extrapolated well to the younger target population.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model correctly captures linear interaction, but also allows for unnecessary nonlinear interaction.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 Frequencies of Missing Values Due to Each Variable
 <pre>
    y   tx  age 
 1444    0    0 
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3556</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 343.94</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.138</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.700</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2702</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 7</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.813</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.401</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 854</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.255</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.401</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 4×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.146</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.146</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.164</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.7933</td>
<td style='min-width: 7em; text-align: right;'> 0.5949</td>
<td style='min-width: 7em; text-align: right;'>-8.06</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  2.5181</td>
<td style='min-width: 7em; text-align: right;'> 0.9368</td>
<td style='min-width: 7em; text-align: right;'> 2.69</td>
<td style='min-width: 7em; text-align: right;'>0.0072</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0752</td>
<td style='min-width: 7em; text-align: right;'> 0.0113</td>
<td style='min-width: 7em; text-align: right;'> 6.67</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.0274</td>
<td style='min-width: 7em; text-align: right;'> 0.1673</td>
<td style='min-width: 7em; text-align: right;'>-0.16</td>
<td style='min-width: 7em; text-align: right;'>0.8699</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age''</td>
<td style='min-width: 7em; text-align: right;'>  0.0392</td>
<td style='min-width: 7em; text-align: right;'> 0.4011</td>
<td style='min-width: 7em; text-align: right;'> 0.10</td>
<td style='min-width: 7em; text-align: right;'>0.9221</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0698</td>
<td style='min-width: 7em; text-align: right;'> 0.0179</td>
<td style='min-width: 7em; text-align: right;'>-3.91</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; text-align: right;'>  0.4148</td>
<td style='min-width: 7em; text-align: right;'> 0.2398</td>
<td style='min-width: 7em; text-align: right;'> 1.73</td>
<td style='min-width: 7em; text-align: right;'>0.0836</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age''</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.8163</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.5469</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-1.49</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.1355</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>211.50</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 19.06</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>132.65</td>
<td style='padding-left:3ex; text-align: right;'>6</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 19.06</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  5.66</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.2263</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 19.06</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  3.97</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.1374</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  3.97</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.1374</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  5.66</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'>0.2263</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'> 20.91</td>
<td style='padding-left:3ex; text-align: right;'>5</td>
<td style='padding-left:3ex; text-align: right;'>0.0008</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>316.28</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>7</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-15.svg" width="672" />



The spline interaction estimates did not extrapolate well to the very young.



<br></div></div><div class="clear"></div><br>



</details>



# Ability to Compare Treatments in Observational Studies



When a categorical baseline characteristic has all of the patients in one of its categories getting a single treatment, one cannot estimate efficacy in that category unless (1) there is no interaction between treatment and the variable or (2) the baseline variable does not relate to outcome.  If one of these conditions is not satisfied, one would have to do a conditional analysis, i.e., to estimate efficacy in patients not in the offending category.  If a continuous baseline variable has an interval for which every patient in that interval received only one treatment, then one cannot estimate the treatment effect in that interval unless (1) there is no interaction (linear or nonlinear) between the variable and treatment or (2) the variable is irrelevant to outcome.  So when you hear that non-overlap implies that only a conditional treatment comparison can be done, be aware of the true underlying assumptions.  Note that non-overlap on a variable implies non-overlap for one or more values of a propensity score.

To illustrate these points, consider an observational study where the data generating process is from the same population logistic model as used above, but instead of considering generalizability to a population, consider the in-sample comparison of treatments a and b adjusted for age.  We start with a sample in which all those with age ≥ 50 got treatment b and all those with age < 50 got treatment a.



## True Model Has No Treatment Interactions



### No Overlap Between Treatment Groups







All patients age < 50 had treatment a, and those ≥ 50 had b.



Age distributions in the observed treatment groups are shown below.


<img src="/post/ia_files/figure-html/main-16.svg" width="500" />



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 87.42</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.046</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.616</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2431</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.496</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.232</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 568</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.643</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.232</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 8×10<sup>-9</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.073</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.071</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.149</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.5743</td>
<td style='min-width: 7em; text-align: right;'> 0.3532</td>
<td style='min-width: 7em; text-align: right;'>-12.95</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.0283</td>
<td style='min-width: 7em; text-align: right;'> 0.1608</td>
<td style='min-width: 7em; text-align: right;'> -6.40</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0714</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0080</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  8.96</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>40.90</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>80.26</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>82.33</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-17.svg" width="672" />



The model-based covariate adjustment provided the correct estimate of the treatment effect even with no overlap, since the specified model is the correct model.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This model included an unnecessary linear interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 88.00</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.047</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.616</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2431</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.486</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.231</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 568</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.626</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.232</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 5×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.073</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.071</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.149</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.2550</td>
<td style='min-width: 7em; text-align: right;'> 0.5399</td>
<td style='min-width: 7em; text-align: right;'>-7.88</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.6471</td>
<td style='min-width: 7em; text-align: right;'> 0.8237</td>
<td style='min-width: 7em; text-align: right;'>-2.00</td>
<td style='min-width: 7em; text-align: right;'>0.0455</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0641</td>
<td style='min-width: 7em; text-align: right;'> 0.0124</td>
<td style='min-width: 7em; text-align: right;'> 5.19</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0124</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0161</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.77</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.4430</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>40.98</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.59</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4430</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>81.73</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.59</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4430</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 0.59</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4430</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>84.33</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-18.svg" width="672" />



With no age overlap, the treatment by age interaction is estimated very inefficiently and in this case incorrectly.  Wide confidence bands correctly capture the difficulty of the task of estimating age-specific treatment effects.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model included unnecessary linear and nonlinear interaction terms.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 89.34</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.047</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.617</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2431</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 5</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.492</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.233</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 568</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.635</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.234</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 4×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.072</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.072</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.149</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -5.0833</td>
<td style='min-width: 7em; text-align: right;'> 0.9349</td>
<td style='min-width: 7em; text-align: right;'>-5.44</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.4348</td>
<td style='min-width: 7em; text-align: right;'> 7.1719</td>
<td style='min-width: 7em; text-align: right;'> 0.06</td>
<td style='min-width: 7em; text-align: right;'>0.9517</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0868</td>
<td style='min-width: 7em; text-align: right;'> 0.0241</td>
<td style='min-width: 7em; text-align: right;'> 3.60</td>
<td style='min-width: 7em; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.1233</td>
<td style='min-width: 7em; text-align: right;'> 0.1090</td>
<td style='min-width: 7em; text-align: right;'>-1.13</td>
<td style='min-width: 7em; text-align: right;'>0.2580</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0361</td>
<td style='min-width: 7em; text-align: right;'> 0.1482</td>
<td style='min-width: 7em; text-align: right;'>-0.24</td>
<td style='min-width: 7em; text-align: right;'>0.8073</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.1420</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.1520</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.93</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.3502</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>40.98</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 1.50</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.4734</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>80.92</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 1.50</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.4734</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'> 1.31</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5193</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 1.50</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.4734</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.87</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3502</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.87</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3502</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'> 1.31</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5193</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'> 1.90</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.5925</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>83.33</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>5</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-19.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



### Some Overlap in Ages Between Treatments



Now consider an observational study, again with confounding by indication, related to age.  Partial overlap is defined by specifying separate study inclusion probability functions.  Given the age and treatment these functions specify the probability that a patient would be included in the observational study.  The first plot shows the inclusion probabilities separately for treatments a and b.  Then the result of a linear no-interaction model are shown.  As before, blue curves depict true data-generating model log odds.



The graph below shows the probability of selection into each of the observed treatment groups as a function of age.


<img src="/post/ia_files/figure-html/main-20.svg" width="500" />



Age distributions in the observed treatment groups are shown below.


<img src="/post/ia_files/figure-html/main-21.svg" width="500" />



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 474.99</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.212</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.744</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2236</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.257</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.488</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 795</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.514</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.489</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 9×10<sup>-6</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.190</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.189</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.166</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.8959</td>
<td style='min-width: 7em; text-align: right;'> 0.3443</td>
<td style='min-width: 7em; text-align: right;'>-14.22</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.0104</td>
<td style='min-width: 7em; text-align: right;'> 0.1787</td>
<td style='min-width: 7em; text-align: right;'> -5.65</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0769</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0061</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 12.53</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'> 31.98</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>156.93</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>335.25</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-22.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This model included an unnecessary linear interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 475.20</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.212</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.744</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2236</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.273</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.488</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 795</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.572</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.489</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 1×10<sup>-8</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.190</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.189</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.166</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.8638</td>
<td style='min-width: 7em; text-align: right;'> 0.3512</td>
<td style='min-width: 7em; text-align: right;'>-13.85</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -1.6044</td>
<td style='min-width: 7em; text-align: right;'> 1.3432</td>
<td style='min-width: 7em; text-align: right;'> -1.19</td>
<td style='min-width: 7em; text-align: right;'>0.2323</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0763</td>
<td style='min-width: 7em; text-align: right;'> 0.0063</td>
<td style='min-width: 7em; text-align: right;'> 12.18</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0141</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0314</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.45</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.6546</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 32.33</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.20</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6546</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>157.04</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.20</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6546</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  0.20</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6546</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>330.68</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-23.svg" width="672" />



The confidence bands are correctly registering that there is little information about the treatment effect outside of the heavier overlap region.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model included unnecessary linear and nonlinear interaction terms.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 476.54</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.213</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.744</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2236</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 5</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.279</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.489</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 795</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.591</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.489</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.190</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.189</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.166</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -6.2165</td>
<td style='min-width: 7em; text-align: right;'> 1.3037</td>
<td style='min-width: 7em; text-align: right;'>-4.77</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.2424</td>
<td style='min-width: 7em; text-align: right;'> 2.3347</td>
<td style='min-width: 7em; text-align: right;'> 0.10</td>
<td style='min-width: 7em; text-align: right;'>0.9173</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.1058</td>
<td style='min-width: 7em; text-align: right;'> 0.0280</td>
<td style='min-width: 7em; text-align: right;'> 3.78</td>
<td style='min-width: 7em; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.0262</td>
<td style='min-width: 7em; text-align: right;'> 0.0240</td>
<td style='min-width: 7em; text-align: right;'>-1.09</td>
<td style='min-width: 7em; text-align: right;'>0.2761</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0291</td>
<td style='min-width: 7em; text-align: right;'> 0.0579</td>
<td style='min-width: 7em; text-align: right;'>-0.50</td>
<td style='min-width: 7em; text-align: right;'>0.6155</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.1247</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.2974</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.42</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.6749</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 20.25</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.26</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.8794</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>154.87</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.26</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.8794</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.30</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5229</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  0.26</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.8794</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6749</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6749</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  1.30</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5229</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  1.51</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.6793</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>330.06</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>5</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-24.svg" width="672" />



<br></div></div><div class="clear"></div><br>



</details>



## Case Where Treatment Truly Interacts with Age



### No Overlap Between Treatment Groups



Return to the data generating model used in the RCT simulation, for which there is truly a linear interaction with treatment, and start with the no overlap case.



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This model failed to include a needed interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 39.09</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.022</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.591</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2493</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.353</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.181</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 506</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.424</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.181</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 4×10<sup>-11</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.048</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.051</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.139</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.5659</td>
<td style='min-width: 7em; text-align: right;'> 0.3538</td>
<td style='min-width: 7em; text-align: right;'>-10.08</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -0.9499</td>
<td style='min-width: 7em; text-align: right;'> 0.1661</td>
<td style='min-width: 7em; text-align: right;'> -5.72</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0481</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0081</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  5.96</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'>32.71</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>35.51</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>37.94</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-25.svg" width="672" />



The treatment effect is incorrect example at age=50.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 42.28</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.023</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.592</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2493</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.369</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.184</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 506</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.446</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.185</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-8</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.050</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.052</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.138</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.2550</td>
<td style='min-width: 7em; text-align: right;'> 0.5399</td>
<td style='min-width: 7em; text-align: right;'>-7.88</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.5441</td>
<td style='min-width: 7em; text-align: right;'> 0.8578</td>
<td style='min-width: 7em; text-align: right;'> 0.63</td>
<td style='min-width: 7em; text-align: right;'>0.5259</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0641</td>
<td style='min-width: 7em; text-align: right;'> 0.0124</td>
<td style='min-width: 7em; text-align: right;'> 5.19</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0295</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0167</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>-1.77</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.0767</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>36.26</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 3.13</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0767</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>36.45</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 3.13</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0767</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 3.13</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.0767</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>40.05</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-26.svg" width="672" />



The extrapolation is excellent.  Now try the overkill model.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model correctly captures linear interaction, but also allows for unnecessary nonlinear interaction.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 2999</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 44.02</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.024</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.592</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2493</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 5</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.387</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.184</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 506</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.473</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.185</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-5</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.050</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.052</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.138</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -5.0833</td>
<td style='min-width: 7em; text-align: right;'> 0.9349</td>
<td style='min-width: 7em; text-align: right;'>-5.44</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -3.5002</td>
<td style='min-width: 7em; text-align: right;'> 7.5833</td>
<td style='min-width: 7em; text-align: right;'>-0.46</td>
<td style='min-width: 7em; text-align: right;'>0.6444</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0868</td>
<td style='min-width: 7em; text-align: right;'> 0.0241</td>
<td style='min-width: 7em; text-align: right;'> 3.60</td>
<td style='min-width: 7em; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.1233</td>
<td style='min-width: 7em; text-align: right;'> 0.1090</td>
<td style='min-width: 7em; text-align: right;'>-1.13</td>
<td style='min-width: 7em; text-align: right;'>0.2580</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'>  0.0483</td>
<td style='min-width: 7em; text-align: right;'> 0.1568</td>
<td style='min-width: 7em; text-align: right;'> 0.31</td>
<td style='min-width: 7em; text-align: right;'>0.7581</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0498</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.1569</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.32</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.7508</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>32.65</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.97</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.6143</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>35.52</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.97</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.6143</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'> 1.70</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.4269</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 0.97</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.6143</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.10</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.7508</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'> 0.10</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.7508</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'> 1.70</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.4269</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'> 4.59</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.2041</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>39.24</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>5</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-27.svg" width="672" />



Extrapolation failed, and the failure was thankfully signaled by very wide confidence bands for age-specific treatment effects.



<br></div></div><div class="clear"></div><br>



</details>



### Some Overlap in Ages Between Treatments



<details><summary>No-Interaction Model ⮩</summary>

#### Model Without Interaction



<div id="boxes"><div id="leftbox">This model failed to include a needed interaction term.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 421.49</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.189</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.732</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2221</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.131</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 810</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.098</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 1×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.182</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.182</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.170</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.7976</td>
<td style='min-width: 7em; text-align: right;'> 0.3406</td>
<td style='min-width: 7em; text-align: right;'>-14.09</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'> -0.7412</td>
<td style='min-width: 7em; text-align: right;'> 0.1640</td>
<td style='min-width: 7em; text-align: right;'> -4.52</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0751</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0061</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 12.37</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx</td>
<td style='padding-left:3ex; text-align: right;'> 20.41</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='padding-left:3ex; text-align: right;'>153.01</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>319.21</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-28.svg" width="672" />



The no-interaction model missed the boat.  Now fit the correct linear interaction model.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Linear Interaction Model ⮩</summary>

#### Model With Linear Interaction



<div id="boxes"><div id="leftbox">This is the correct model.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx * age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 422.14</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.189</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.732</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2221</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.112</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 810</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.040</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 1×10<sup>-5</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.182</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.182</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.170</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -4.8638</td>
<td style='min-width: 7em; text-align: right;'> 0.3512</td>
<td style='min-width: 7em; text-align: right;'>-13.85</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  0.1333</td>
<td style='min-width: 7em; text-align: right;'> 1.0813</td>
<td style='min-width: 7em; text-align: right;'>  0.12</td>
<td style='min-width: 7em; text-align: right;'>0.9019</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0763</td>
<td style='min-width: 7em; text-align: right;'> 0.0063</td>
<td style='min-width: 7em; text-align: right;'> 12.18</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.0208</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0255</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.81</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.4151</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 20.56</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.66</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4151</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>153.46</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.66</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4151</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  0.66</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.4151</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>326.02</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-29.svg" width="672" />



There is reasonable extrapolation.  Now for the overkill model.



<br></div></div><div class="clear"></div><br>



</details>



<details><summary>Spline Interaction Model ⮩</summary>

#### Model with Spline Interaction



<div id="boxes"><div id="leftbox">This model correctly captures linear interaction, but also allows for unnecessary nonlinear interaction.</div><div id="rightbox">
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = form, data = d, tol = 1e-12)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 3031</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 423.48</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.190</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.732</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 2221</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 5</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.121</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 810</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.069</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.464</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-5</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.183</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.182</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.170</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -6.2165</td>
<td style='min-width: 7em; text-align: right;'> 1.3037</td>
<td style='min-width: 7em; text-align: right;'>-4.77</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b</td>
<td style='min-width: 7em; text-align: right;'>  1.8369</td>
<td style='min-width: 7em; text-align: right;'> 1.9527</td>
<td style='min-width: 7em; text-align: right;'> 0.94</td>
<td style='min-width: 7em; text-align: right;'>0.3468</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.1058</td>
<td style='min-width: 7em; text-align: right;'> 0.0280</td>
<td style='min-width: 7em; text-align: right;'> 3.78</td>
<td style='min-width: 7em; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> -0.0262</td>
<td style='min-width: 7em; text-align: right;'> 0.0240</td>
<td style='min-width: 7em; text-align: right;'>-1.09</td>
<td style='min-width: 7em; text-align: right;'>0.2761</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=b × age</td>
<td style='min-width: 7em; text-align: right;'> -0.0602</td>
<td style='min-width: 7em; text-align: right;'> 0.0477</td>
<td style='min-width: 7em; text-align: right;'>-1.26</td>
<td style='min-width: 7em; text-align: right;'>0.2067</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=b × age'</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.1114</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.2592</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.43</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.6672</td>
</tr>
</tbody>
</table>




<table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">y</code></td></tr>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 16.32</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.0010</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.94</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.3784</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>151.01</td>
<td style='padding-left:3ex; text-align: right;'>4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.94</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.3784</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>  1.30</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5233</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  1.94</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.3784</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6672</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>  0.18</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.6672</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>  1.30</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.5233</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  1.98</td>
<td style='padding-left:3ex; text-align: right;'>3</td>
<td style='padding-left:3ex; text-align: right;'>0.5774</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>325.31</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>5</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>






<img src="/post/ia_files/figure-html/main-30.svg" width="672" />



Extrapolated treatment effect estimates are so-so.



<br></div></div><div class="clear"></div><br>



</details>

# Summary

With respect to relative efficacy (or both relative and absolute efficacy for continuous repsonse variables), clinical trials do not require having a representative sample of the target clinical population if there are no interactions with treatment.  If there are interactions, let M denote the levels of interacting factors that are well represented in the target population.  In our examples, M represents younger patients.  Even with interaction present, the randomized trial still does not need to be on a representative patient sample if either (1) the trial sample <em>is</em> representative with respect to M (in which case omitting interactions from the model is not fatal), or (2) there is just enough representation in the sample with respect to M,  and those interacting factors are appropriately modeled in the randomized trial.  Unless M is richly represented in the trial, using statistical testing to decide on which interactions to include in the model is not advised, due to low power of such interaction tests.  Suspected interactions, even statistically weak ones, should be included in the model when some degree of extrapolation is sought.  When M is very poorly represented in the trial, extrapolation to the target population makes strong model assumptions.  Thankfully confidence intervals for extrapolated efficacy estimates will be properly wide to reflect the weak basis for such extrapolation.

In a similar vein, observational treatment comparisons can be appropriate if factors that do not overlap between treatment groups do not interact with treatment.  So a key to understanding both overlap and clinical trial generalizability is interactions.

----
# Further Reading

* [Why representativeness should be avoided](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3888189) by Rothman, Gallacher, and Hatch
* [Treatment effects may remain the same even when trial participants differed from the target population](https://www.jclinepi.com/article/S0895-4356(19)30818-2/pdf) by MJ Bradburn et al.

----
# Questions and Discussion

Go [here](http://datamethods.org/t/rct-generalizability-and-interactions-with-treatment).

----
# R Code | [R Markdown](https://github.com/harrelfe/blogdown/blob/master/content/post/ia.Rmarkdown)

<details><summary>Code ⮩</summary>

```r
gendatrct <- function(n, popmodel, inclusion, pr=TRUE) {
    set.seed(1)
    age <- rnorm(n, 50, 10)
		ager <- round(range(age), 1)
    tx  <- sample(c('a', 'b'), n, replace=TRUE)
    y   <- ifelse(runif(n) <= plogis(popmodel(age, tx)), 1, 0)
    # Inclusion in the RCT is marked by having non-missing outcome
    # inclusion is a function of age computing P(include in RCT)
    y   <- ifelse(inclusion(age) >= runif(n), y, NA)
    d   <- data.frame(age, tx, y)
		if(pr)
    cat('The original age range is', ager[1], '-', ager[2], 'in', n,
        'subjects, and the clinical trial includes', sum(! is.na(d$y)),
        'of the subjects.<br>')
    d
}

gendatobs <- function(n, popmodel, incla, inclb) {
    set.seed(1)
    age <- rnorm(n, 50, 10)
    tx  <- sample(c('a', 'b'), n, replace=TRUE)
    y   <- ifelse(runif(n) <= plogis(popmodel(age, tx)), 1, 0)
    # incla is a function of age computing P(include in study if a)
    # inclb is the same for tx=b
    prob <- ifelse(tx == 'a', incla(age), inclb(age))
    i <- runif(n) <= prob
    data.frame(age, tx, y)[i, ]
}


# Plot fitted relationship between logit(y) and age by treatment
# along with theoretical true relationship
pl <- function(popmodel, ylim=NULL) {
    ar <- range(d$age)
    g <- ggplot(Predict(f, age=seq(ar[1], ar[2], length=100), tx),
           rdata=subset(d, ! is.na(y)), ylim=ylim) +
         annotate('line', x=ar, y=popmodel(ar, 'a'), col='blue', alpha=.4) +
         annotate('line', x=ar, y=popmodel(ar, 'b'), col='blue', alpha=.4) +
         theme(legend.position=c(.9, .17)) +
         labs(caption='Estimated log odds of outcome along with true (blue) values.\nTick marks show raw data density stratified by treatment')
    g
}

# Estimated and true treatment effect as a function of age
plc <- function(popmodel, ages=10:80, ylim=c(-3, 1)) {
    k <- contrast(f, list(tx='b', age=ages),
                     list(tx='a', age=ages))
    w <- as.data.frame(k[c('age','Contrast','Lower','Upper')])
    g <- ggplot(w, aes(x=age, y=Contrast)) +
        geom_ribbon(aes(ymin=Lower, ymax=Upper), fill='grey80') +
        geom_line() +
        xlab('Age') + ylab('b - a Treatment Difference') +
        annotate('line', x=ages,
                 y=popmodel(ages, 'b') - popmodel(ages, 'a'),   
                 col='blue', alpha=.4) +
        ylim(ylim) +
        labs(caption='Estimated (black) and true (blue)\ntreatment effect as a function of age\nBands are 0.95 confidence bands')
		g
}

plb <- function(popmodel) {
    g <- cowplot::plot_grid(pl(popmodel), plc(popmodel))
		cat('\n')
    print(g)
		cat('\n\n')
    invisible()
    }
ct <- function(...) cat('\n\n', ..., '\n\n', sep='')

# Function to manually output html figure inclusion markup
# With loops even with fig.show='asis' knitr doesn't always output
# figures soon enough.  Taking control in this way also allows
# figure sizes to vary within the loop

figincl <- function(figno, type='svg', width=672) {
  cname <- knitr::opts_current$get('label')
  filename <- knitr::fig_chunk(cname, type, figno)
  cat('\n<img src="', filename, '" width="', width, '" />\n\n', sep='')
	invisible()
}
# <img src="/post/ia_files/figure-html/main-30.svg" width="672" />


# Function to enclose a left margin note in a div and to open a div for the
# usual right hand side content
lmargin  <- function(x) c('<div id="boxes"><div id="leftbox">', x,
                          '</div><div id="rightbox">')
closebox <- function() ct('<br></div></div><div class="clear"></div><br>')

h1 <- c(rct = 'Generalizability of Randomized Clinical Trials',
        obs = 'Ability to Compare Treatments in Observational Studies')
h2 <- c(noia = 'True Model Has No Treatment Interactions',
        ia   = 'Case Where Treatment Truly Interacts with Age')

figno <- 0
for(design in c('rct', 'obs')) {
  ct('# ', h1[design])
  h3 <- switch(design,
               rct = c(none    = 'Estimation of Treatment Effect in Population With No Overlap',
                       partial = 'RCT Sample Partially Overlaps with Target Population'),
               obs = c(none    = 'No Overlap Between Treatment Groups',
                       partial = 'Some Overlap in Ages Between Treatments'))
  txt(design)
  for(model in c('noia', 'ia')) {
    ct('## ', h2[model])
    popmodel <- switch(model,
                       noia = function(age, tx) -1 + (age - 50) / 15 - 1.0 * (tx == 'b'),
                       ia   = function(age, tx) -1 + (age - 50) / 15 +
                                 (tx == 'b') * (-1 - 0.03 * (age - 50)) )

    for(overlap in c('none', 'partial')) {
      ct('### ', h3[overlap])
      txt(design, model, overlap)

      # Function if age that is P(inclusion in RCT)
      incl <- switch(overlap,
                     none    = function(age) ifelse(age >= 50, 1, 0),
                     partial = function(age)
                        approx(c(0,    25,   40,  45, 50),
                               c(0, 0.075, 0.15, 0.5,  1), rule=2, xout=age)$y)
      # Functions of age that are P(selection to be in tx a group) and also for b group
      incla <- switch(overlap,
                      none    = function(age) ifelse(age < 50, 1, 0),
                      partial = function(age)
                         approx(c(0,    25,   40,  45, 50, 55),
                                c(0, 0.075, 0.15, 0.5,  1,  1),
                                xout=age, rule=2)$y)
      inclb <- function(age) 1 - incla(age)

      if(model == 'noia') {
        switch(overlap,
          none = switch(design,
                        rct = '',
                        obs = ct('All patients age < 50 had treatment a, and those ≥ 50 had b.')),
          partial = {
            a <- seq(10, 80, length=100)
            switch(design,
              rct = {
              plot(a, incl(a), type='l', xlab='Age', ylab='P(Inclusion)', ylim=c(0,1))
							figno <- figno + 1; figincl(figno, width=500)
              },
            obs = {
              ct('The graph below shows the probability of selection into each of the observed treatment groups as a function of age.')
              w <- list(a=list(x=a, y=incla(a)), b=list(x=a, y=inclb(a)))
              labcurve(w, pl=TRUE, xlab='Age', ylab='P(Inclusion)')
							figno <- figno + 1; figincl(figno, width=500)
            })}) 
        }
     # Generate a sample that meets our requirements
     d <- switch(design,
                 rct = gendatrct(5000, popmodel, inclusion=incl,
                                 pr=model == 'noia'),
                 obs = gendatobs(6000, popmodel, incla=incla, inclb=inclb))
     dd <- datadist(d, q.display=c(0,1)); options(datadist='dd')
		 if(model == 'noia') switch(design,
		        rct = {ct('Age distributions in the target population as compared to the RCT sample are shown below.')
						       with(d, histbackback(age[is.na(y)], age[! is.na(y)],
                        brks=13:89, xlab=c('Target Population','RCT'), ylab='Age'))
                   figno <- figno + 1; figincl(figno, width=500) },
            obs = {ct('Age distributions in the observed treatment groups are shown below.')
                   with(d, histbackback(age[tx == 'a'], age[tx == 'b'],
                        brks=13:89, xlab=c('a','b'), ylab='Age'))
                   figno <- figno + 1; figincl(figno, width=500) }
     )

     ct('<details><summary>No-Interaction Model ⮩</summary>\n\n#### Model Without Interaction')
		 marg <- switch(model,
		   noia = 'This is the correct model.',
		   ia   = 'This model failed to include a needed interaction term.')
     f <- lrm(y ~ tx + age, data=d)
     # To render html from a chunk in a loop, must create html character strings
     # with print() then output the strings with cat() (called by ct())
     # Earlier options(prType='html') made print() create html for fit objects, anova
     ct(lmargin(marg), print(f))
     ct(print(anova(f)))
		 plb(popmodel)
		 figno <- figno + 1; figincl(figno)
     txt(design, model, overlap, part='interp additive fit')
		 closebox()
		 ct('</details>')

		 ct('<details><summary>Linear Interaction Model ⮩</summary>\n\n#### Model With Linear Interaction')
		 marg <- switch(model,
       noia = 'This model included an unnecessary linear interaction term.',
       ia   = 'This is the correct model.')
     f <- lrm(y ~ tx * age, data=d)
     ct(lmargin(marg), print(f))
     ct(print(anova(f)))
     plb(popmodel)
		 figno <- figno + 1; figincl(figno)

     txt(design, model, overlap, part='interp linear ia')
     closebox()
     ct('</details>')

     ct('<details><summary>Spline Interaction Model ⮩</summary>\n\n#### Model with Spline Interaction')
     marg <- switch(model,
       noia = 'This model included unnecessary linear and nonlinear interaction terms.',
       ia   = 'This model correctly captures linear interaction, but also allows for unnecessary nonlinear interaction.')

     form <- switch(design,
                    rct = y ~ tx * rcs(age, c(55, 60, 70, 80)),
                    obs = y ~ tx * rcs(age, c(35, 50, 65)))
     f <- lrm(form, tol=1e-12, data=d)
     ct(lmargin(marg), print(f))
     ct(print(anova(f)))
     plb(popmodel)
		 figno <- figno + 1; figincl(figno)
 
     txt(design, model, overlap, part='interp nonlinear ia')
     closebox()
     ct('</details>')
    }
  }
}
```

## Portion of the `txt` Function
Here is part of the `txt` function, which is called by the code above to insert sentences at appropriate places.


```r
txt <- function(design, model='', overlap='', part='') {
  w <- list(
    list(design='rct', text='When the outcome generating process in a randomized clinical trial (RCT) is such that treatment does not interact with any baseline patient characteristic over the range of characteristics seen in the union of the RCT and the population ...'),

    list(design='rct', model='noia', overlap='none', text='As an example consider a simulated study of about 2500 patients with a ...')

  # Several more lists are defined in txt
  )
  g <- function(x, y) (! length(x) && y == '') || (length(x) && y == x)

  for(x in w) {
    if(g(x$design, design) && g(x$model, model) && g(x$overlap, overlap) &&
		   g(x$part,   part)) {
      ct(x$text)
      break
    }
  }
  return(invisible())
}
```

</details>
