---
title: Assessing Heterogeneity of Treatment Effect, Estimating Patient-Specific Efficacy, and Studying Variation in Odds ratios, Risk Ratios, and Risk Differences
author: Frank Harrell
date: '2019-03-25'
modified: ''
slug: varyor
categories: []
tags:
  - RCT
  - generalizability
  - medicine
  - metrics
  - personalized-medicine
  - prediction
  - subgroup
  - accuracy-score
  - 2019
summary: 'This article shows an example formally testing for heterogeneity of treatment effect in the GUSTO-I trial, shows how to use penalized estimation to obtain patient-specific efficacy, and studies variation across patients in three measures of treatment effect.' 
header:
  caption: ''
  image: ''
---

## Background
Heterogeneity of treatment effect (HTE), better called _differential treatment effect_, is variation in a measure of treatment effect on a scale for which it is mathematically possible that such variation be absent even if the treatment has a nonzero effect.  The most commonly used scales for measuring HTE are

* the original scale for continuous response variables that are properly transformed, in which case the treatment effect is the (adjusted) difference in means
* odds ratios, for binary or ordinal outcomes
* hazard ratios, for time-to-event outcomes

Even though the more complex hazard ratio seems to be well accepted as a summary measure of treatment effect in a time-to-event randomized clinical trial (RCT), there is still a good deal of resistence to odds ratios (OR) from some clinical researchers.  This resistence is difficult to understand, although it is clear that ORs are more difficult to understand than risk ratios (RR) or absolute risk reduction (ARR; risk difference).  The reasons for choosing ORs are:

* ORs come directly from logistic models, and logistic models are as likely as any model to fit patterns leading to binary responses.  This is primarily because the logistic model places no restrictions on the regression coefficients.
* ORs are capable of being constant over a range of baseline risk all the way from 0 to 1.0.
* In the multitude of forest plots present in journal articles depicting RCT results, the constancy of ORs over patient types is impressive.
* Unlike RRs, ORs are invariant to the choice of the "event" vs. the "no event".  If you interchange event and non-event you would get the reciprocal of the original OR, but the RR would change arbitrarily.

RR and ARR are not capable of being constant.  For example, a risk factor that doubles the risk of an outcome can only apply to a patient having a baseline risk of 0.5 or less, otherwise the risk with the risk factor would exceed 1.0.  A treatment that lowers the risk of an outcome of 0.04 cannot apply to a patient with a baseline risk of 0.02.

The purposes of this article are to show how

* a rigorous test for HTE in the frequentist domain is done for an actual large RCT with a binary endpoint
* the difficulty in estimating and testing interaction effects
* the OR varies over patient types if one allows all the baseline variables to interact with treatment, whether the interactions are "significant" or not
* RR varies over patient types
* ARR varies over patient types

## Difficulty of the Task

Establishing the existence of differential treatment effects in RCTs is difficult because RCTs are typically sized just large enough to detect an overall average treatment effect.  In the ideal situation for showing evidence of HTE, a potentially interacting binary covariate yields optimum precision and power when the covariate has equal sample sizes for its two values.  Even then, the precision of the interaction effect (a double difference) is four times as bad as the precision of the overall treatment effect.  And as [Andrew Gelman has discussed](https://statmodeling.stat.columbia.edu/2018/03/15/need-16-times-sample-size-estimate-interaction-estimate-main-effect), the power is as if the sample size is divided by 16 because one typically wants to detect a differential effect that is only half as large as the overall detectable treatment effect.  A related example will be shown below.

It is important to note that assessing treatment effect in an isolated subgroup defined by a categorical covariate **does not establish HTE** and results in unreliable estimates.  Differential treatment effect must be demonstrated.

## The GUSTO-I Study and Basic Outcome Model

The [GUSTO-I Study](https://www.nejm.org/doi/full/10.1056/NEJM199309023291001) was a 40,000 patient international trial comparing four treatment arms for 30-day mortality following acute myocardial infarction: streptokinase (SK) with subcutaneous heparin, SK with intravenous heparin, accelerated dosing of tissue plasminogen activator (t-PA) with intravenous heparin, and SK combined with t-PA and intravenous heparin.  The covariate-adjusted analysis that forms the basis for this article was done by [Steyerberg](https://www.springer.com/us/book/9780387772431), who replaced a limited number of missing covariate values with singly imputed values.  We will be primarily comparing the accelerated t-PA arm (n=10,320) with the two combined SK arms that did not involve t-PA (n=20,162).


```r
require(Hmisc)
```

```r
mu <- markupSpecs$html    # in Hmisc
knitrSet(lang='blogdown')
load(url('http://hbiostat.org/data/gusto.rda'))
gusto <- upData(gusto, keep=Cs(day30, tx, age, Killip, sysbp, pulse, pmi, miloc))
```

```
## Input object size:	 5241552 bytes;	 29 variables	 40830 observations
## Kept variables	day30,tx,age,Killip,sysbp,pulse,pmi,miloc
## New object size:	1476752 bytes;	8 variables	40830 observations
```


```r
html(describe(gusto), scroll=TRUE)
```

<!--html_preserve--><div style="width: 100ex; overflow: auto; height: 25ex;"> <meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
<script type="text/javascript">
<!--
    function expand_collapse(id) {
       var e = document.getElementById(id);
       var f = document.getElementById(id+"_earrows");
       if(e.style.display == 'none'){
          e.style.display = 'block';
          f.innerHTML = '&#9650';
       }
       else {
          e.style.display = 'none';
          f.innerHTML = '&#9660';
       }
    }
//-->
</script>
<style>
.earrows {color:silver;font-size:11px;}

fcap {
 font-family: Verdana;
 font-size: 12px;
 color: MidnightBlue
 }

smg {
 font-family: Verdana;
 font-size: 10px;
 color: &#808080;
}

hr.thinhr { margin-top: 0.15em; margin-bottom: 0.15em; }

span.xscript {
position: relative;
}
span.xscript sub {
position: absolute;
left: 0.1em;
bottom: -1ex;
}
</style>
 <font color="MidnightBlue"><div align=center><span style="font-weight:bold">gusto <br><br> 8  Variables   40830  Observations</span></div></font> <hr class="thinhr"> <span style="font-weight:bold">day30</span> <style>
 .hmisctable466403 {
 border: none;
 font-size: 85%;
 }
 .hmisctable466403 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable466403 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable466403">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Sum</th><th>Mean</th><th>Gmd</th></tr>
 <tr><td>40830</td><td>0</td><td>2</td><td>0.195</td><td>2851</td><td>0.06983</td><td>0.1299</td></tr>
 </table>
 <hr class="thinhr"> <span style="font-weight:bold">Killip</span>: Killip Class<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAA0AAAANCAMAAABFNRROAAAANlBMVEUAAAAdHR02NjZnZ2eioqKxsbG+vr6/v7/KysrOzs7U1NTZ2dna2trm5ubr6+vs7Oz09PT///+Wd0knAAAAMElEQVQImWNg4OZDAAYGfkEEIJ/HwYMApJnCyyyAxGNl4ITzuPlYGNj4uBjZ+XiZAGjBCT7UcDeJAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable223658 {
 border: none;
 font-size: 85%;
 }
 .hmisctable223658 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable223658 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable223658">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>40830</td><td>0</td><td>4</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value          I    II   III    IV
 Frequency  34825  5141   551   313
 Proportion 0.853 0.126 0.013 0.008
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">age</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAAB5lBMVEUAAAABAQECAgIDAwMEBAQFBQUGBgYHBwcICAgJCQkKCgoLCwsMDAwODg4PDw8QEBARERESEhITExMUFBQVFRUXFxcYGBgZGRkaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQlJSUmJiYnJycpKSkqKiorKystLS0uLi4vLy8wMDAxMTEyMjIzMzM0NDQ1NTU2NjY3Nzc4ODg5OTk6Ojo7Ozs8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERFRUVGRkZHR0dISEhJSUlKSkpMTExNTU1OTk5PT09QUFBRUVFSUlJXV1dYWFhZWVlbW1tcXFxdXV1eXl5hYWFjY2NlZWVmZmZra2tsbGxubm5vb29ycnJzc3N0dHR4eHh5eXl6enp8fHx9fX1/f3+AgICCgoKGhoaHh4eLi4uNjY2Ojo6Pj4+QkJCSkpKUlJSZmZmdnZ2kpKSlpaWnp6eqqqqrq6usrKyurq6zs7O0tLS2tra6urq+vr6/v7/ExMTGxsbHx8fLy8vMzMzNzc3Pz8/S0tLT09PV1dXW1tbY2NjZ2dna2trd3d3e3t7f39/g4ODl5eXm5ubn5+fp6enq6urr6+vx8fHy8vL09PT19fX4+Pj5+fn7+/v8/Pz+/v7///+WEMMvAAAB7ElEQVQ4jb3V2VPTUBQG8A+jFChKCxQRS7GY0qatRkjXpCGtSWwFiwsIdYe6oFVAEVTcWRRwQUUkIIj2P/X64AzjjExomH4v98zcO+f+5rwcaMayqGl3hhpZFr3n3twuPXQcuRyujzycM9hWQ95A1le/7M121MhgOcQbTJ1oiCPAQq7ddXngl5HG+bwB19iD0rQXKv66cOKPiyOuGlKOzxTbtfyuekC9lHP4wxCd/3GpOHy2eK757/mJsX3Jo1DKKhUwW7qcKc+9u0VxLU2ZMxF3DLxbl0tE2F97JVSo66POfBjpsVICmji0NoM3VURBswjYIcASgceHUB0p68LweRC1gIc9ANaFaIU9O6H3i83RO6/F11AtlApa2Ma8GCiVZcmSrz8LmJeeR+8njykc5MJcCqJ9jzZ23jX78rRLRtBfuMst4PH8wvaGtrVrLTPavyeFRsmYi4fE7M74ZjSDrh8rr97eH3129UwKif3kD+MugYZK2c6nx5+/+Lagy0V2UXeu4+IN9eaF2ODJdNab6io/xZTIsInwH0HMhjgO8mhlIFZREpwhBGlIJnM7WjhEHOTWKsLHQqgnZb0A1gvRSkpHBFwL2s0mCa4gQs2QqCoRTBsS9IFrie5byU+fk9Na19N/9uJwv/akh5y/AWZYWbtLTEmOAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable457461 {
 border: none;
 font-size: 85%;
 }
 .hmisctable457461 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable457461 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable457461">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>40830</td><td>0</td><td>5575</td><td>1</td><td>60.9</td><td>13.62</td><td>40.89</td><td>44.69</td><td>52.09</td><td>61.52</td><td>69.86</td><td>76.25</td><td>79.45</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :  19.027  19.031  20.289  20.781  20.969 ,  <font color="MidnightBlue">highest</font>:  92.953  95.000  96.547 108.000 110.000</span> <hr class="thinhr"> <span style="font-weight:bold">pulse</span>: Heart Rate <span style='font-family:Verdana;font-size:75%;'>beats/min</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAABjFBMVEUAAAABAQECAgIDAwMEBAQGBgYMDAwODg4QEBASEhITExMUFBQVFRUWFhYXFxcYGBgZGRkaGhobGxscHBwdHR0fHx8gICAiIiIlJSUmJiYnJycoKCgpKSkqKiosLCwwMDAyMjI0NDQ1NTU4ODg5OTk8PDw9PT0+Pj4/Pz9AQEBBQUFERERGRkZHR0dISEhKSkpLS0tMTExNTU1OTk5PT09QUFBRUVFSUlJUVFRWVlZXV1dYWFhZWVlaWlpbW1tcXFxdXV1eXl5fX19hYWFra2tsbGxtbW1ubm5wcHBxcXFzc3N4eHh7e3t9fX1+fn5/f3+AgICMjIyRkZGTk5OUlJSVlZWWlpaXl5ehoaGjo6Ompqapqamrq6usrKytra2urq6xsbGzs7O0tLS1tbW2tra3t7e7u7vExMTFxcXIyMjMzMzOzs7Pz8/R0dHS0tLT09PX19fa2trb29vc3Nzd3d3f39/h4eHo6Ojp6enq6urr6+vs7Ozw8PDy8vL29vb39/f4+Pj9/f3+/v7///98ZAbpAAABjUlEQVQ4jc3VV1PCQBQF4JtgpxmDoGIBGzaiQRRIRLFi712JErE3sKDGApI/7m5wnJEXMoOo5yFnH7KTb24eLkiqopn+Om7Csbo7OQVkVdHMLXqVw+VRCK7U3ckpql289f1c7hT8df/OFYEYLuRK5teEo8aViiouATyfLmr9X7gO4SbtAjLtIufz74plzyqENWM9llXkQrUCIjk+wai4l0uyz+v5XoDLzHlxdONUIiFHT/I2r6xvjDZgV5PNuoNcwX5Ts+LSAVwb1/z1f+V6clbXCODVGGhjCXJRlVqAIDntwS5igav9E1fqVi4tN1E0AhloBAJSb8JF0Gbk2iYm+2ov6qW3x9dfdZ2JEWKrIC3JcOlpHS7KbBmAfb25felhJtkrvr3IcuKnXGgXBXalAxd7h9fSOXsXCG34OhxddqbBUIwkuopSDNJRZbi0ShGZhf5xEdjtVbaSwjqns7XV2NLm9bEOB9PNcx43w7i4Ib+bjXMDw4NuV5wPS9IpG+dFaW8Yf3R59ttqFEbQ4wPTLlJvb1ep/AAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable920215 {
 border: none;
 font-size: 85%;
 }
 .hmisctable920215 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable920215 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable920215">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>40830</td><td>0</td><td>166</td><td>0.999</td><td>75.41</td><td>19.55</td><td> 50</td><td> 55</td><td> 62</td><td> 73</td><td> 86</td><td> 98</td><td>107</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0   1   6   8   9 ,  <font color="MidnightBlue">highest</font>: 200 205 210 220 246</span> <hr class="thinhr"> <span style="font-weight:bold">sysbp</span>: Systolic Blood Pressure <span style='font-family:Verdana;font-size:75%;'>mmHg</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAABtlBMVEUAAAACAgIDAwMEBAQFBQUGBgYICAgJCQkKCgoLCwsMDAwNDQ0ODg4PDw8QEBARERESEhITExMVFRUWFhYXFxcYGBgZGRkaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQmJiYnJycoKCgqKiouLi4wMDAxMTEyMjIzMzM0NDQ1NTU2NjY4ODg5OTk6Ojo8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERFRUVGRkZISEhKSkpLS0tMTExNTU1OTk5PT09RUVFSUlJTU1NVVVVXV1dZWVlbW1tcXFxdXV1fX19hYWFkZGRmZmZpaWltbW1ubm5vb29zc3N0dHR2dnZ4eHh5eXmAgICDg4OIiIiJiYmMjIyNjY2QkJCRkZGUlJSXl5eYmJiZmZmcnJydnZ2fn5+goKCjo6OlpaWqqqqrq6utra2urq6wsLCysrKzs7O0tLS7u7u9vb2/v7/FxcXGxsbJycnLy8vQ0NDR0dHS0tLW1tbY2NjZ2dna2trc3Nzd3d3f39/g4ODm5ubq6urr6+vt7e3u7u7y8vLz8/P29vb6+vr7+/v9/f3+/v7///9+57g8AAABq0lEQVQ4jWNopxCEMbTBmGWUmoUEGCZSCCIZJkycWKHbDSQYKig1DAGo4650huaJE0sZSqnhIgigxF2djRMHpbsCVCYOSnf5ykzEcFdvaDN13NVAFoiPAxKu4kAikKE6vyGaobShIZMhs6GhlCEGKOiSSZ6xCEBmeJkbQ8IrWS+SIZGpJ4WhucWlEBRezQzpEydOYIikOLzIdVeET6ebgHMoVyRDLEMmC4NsFkMKQ45U1YC7y1HTj59bFOKuBAYGhiSgu1IYcv2A7mrvB7urqZOu7rKLmugXDnKXBw+3sDubEoM8gzvQXUEMmgweQNcxcOazpzJE1nVPVAkAqm5soq27OkCEVV5aTYmUQrCagpyEtJAoDzcnBysTAzKQBmJLBmsGb8GQYCHb+spaS5u+3oldraS7C1gXeWWjVE3p9gE5vgVuxRbmJtomRkaGwppqOsrM4hq8iowS7CJ83Dx8YMDLw83FzsaC6i4YYGbh4eTikRSRFJNSUVSXEdUxUNURN9M3N9YyNjEyMTK2KLcxMTW3sLQs8s0JsLN3cHR0io4BkqEZdv7ZnkBHAABhT7qWPpX1uQAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable206738 {
 border: none;
 font-size: 85%;
 }
 .hmisctable206738 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable206738 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable206738">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>40830</td><td>0</td><td>209</td><td>0.999</td><td>129</td><td>26.56</td><td> 92</td><td>100</td><td>112</td><td>130</td><td>144</td><td>160</td><td>170</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0  30  32  36  40 ,  <font color="MidnightBlue">highest</font>: 270 274 275 276 280</span> <hr class="thinhr"> <span style="font-weight:bold">miloc</span>: MI Location<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAAoAAAANCAMAAACn6Q83AAAAGFBMVEUAAAABAQE8PDyWlpa4uLja2trr6+v///9eu+iWAAAALUlEQVQImWNgYIMBBgZ2GMDKZEYwGRFMBgYGVihgQBElhsnCAmECbWdiAjkBAM1mAsMRV8USAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable758175 {
 border: none;
 font-size: 85%;
 }
 .hmisctable758175 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable758175 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable758175">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>40830</td><td>0</td><td>3</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value      Inferior    Other Anterior
 Frequency     23495     1435    15900
 Proportion    0.575    0.035    0.389
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">pmi</span>: Previous MI <style>
 .hmisctable437039 {
 border: none;
 font-size: 85%;
 }
 .hmisctable437039 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable437039 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable437039">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>40830</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value         no   yes
 Frequency  34104  6726
 Proportion 0.835 0.165
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">tx</span>: Tx in 3 groups<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAAoAAAANCAMAAACn6Q83AAAAHlBMVEUAAAADAwM3Nzc7OztAQEC0tLS3t7fa2trr6+v///8hI0s8AAAAMElEQVQImWPgAAJmZhDJwAkELCwgkjCTDcZkZWBkZ2dnYgISjAwMMFEG0pgwNzAAAARyA2zJ9UAOAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable153018 {
 border: none;
 font-size: 85%;
 }
 .hmisctable153018 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable153018 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable153018">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>40830</td><td>0</td><td>3</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value      SK+tPA     SK    tPA
 Frequency   10320  20162  10348
 Proportion  0.253  0.494  0.253
 </pre>
 <hr class="thinhr"> </div><!--/html_preserve-->

## Base Risk Model
A simplified covariate-adjusted risk model as developed by Steyerberg is fitted below.  For simplicity when later interacting baseline variables with treatment, the important age by Killip class interaction is omitted.  Pulse rate is modeled using a linear spline with a knot at 50 beats/minute.


```r
require(rms)
```

```r
options(prType='html')
dd <- datadist(gusto); options(datadist='dd')
f <- lrm(day30 ~ tx + age + Killip + pmin(sysbp, 120) +
         lsp(pulse, 50) + pmi + miloc, data=gusto,
         eps=0.005, maxit=30)
f
```


 <div align=center><strong>Logistic Regression Model</strong></div>
 
 <pre>
 lrm(formula = day30 ~ tx + age + Killip + pmin(sysbp, 120) + 
     lsp(pulse, 50) + pmi + miloc, data = gusto, eps = 0.005, 
     maxit = 30)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 40830</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 4143.09</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.243</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.820</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 37979</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 12</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.418</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.641</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2851</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 4.128</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.641</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-9</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.081</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.083</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.055</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;'></th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.4692</td>
<td style='min-width: 7em; text-align: right;'> 0.7095</td>
<td style='min-width: 7em; text-align: right;'> -4.89</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>tx=SK</td>
<td style='min-width: 7em; text-align: right;'>  0.0742</td>
<td style='min-width: 7em; text-align: right;'> 0.0513</td>
<td style='min-width: 7em; text-align: right;'>  1.45</td>
<td style='min-width: 7em; text-align: right;'>0.1483</td>
</tr>
<tr>
<td style='text-align: left;'>tx=tPA</td>
<td style='min-width: 7em; text-align: right;'> -0.1358</td>
<td style='min-width: 7em; text-align: right;'> 0.0609</td>
<td style='min-width: 7em; text-align: right;'> -2.23</td>
<td style='min-width: 7em; text-align: right;'>0.0258</td>
</tr>
<tr>
<td style='text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0796</td>
<td style='min-width: 7em; text-align: right;'> 0.0021</td>
<td style='min-width: 7em; text-align: right;'> 37.12</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>Killip=II</td>
<td style='min-width: 7em; text-align: right;'>  0.6011</td>
<td style='min-width: 7em; text-align: right;'> 0.0511</td>
<td style='min-width: 7em; text-align: right;'> 11.76</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>Killip=III</td>
<td style='min-width: 7em; text-align: right;'>  1.2092</td>
<td style='min-width: 7em; text-align: right;'> 0.1053</td>
<td style='min-width: 7em; text-align: right;'> 11.49</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>Killip=IV</td>
<td style='min-width: 7em; text-align: right;'>  1.9292</td>
<td style='min-width: 7em; text-align: right;'> 0.1391</td>
<td style='min-width: 7em; text-align: right;'> 13.87</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>sysbp</td>
<td style='min-width: 7em; text-align: right;'> -0.0391</td>
<td style='min-width: 7em; text-align: right;'> 0.0017</td>
<td style='min-width: 7em; text-align: right;'>-23.40</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>pulse</td>
<td style='min-width: 7em; text-align: right;'> -0.0210</td>
<td style='min-width: 7em; text-align: right;'> 0.0141</td>
<td style='min-width: 7em; text-align: right;'> -1.48</td>
<td style='min-width: 7em; text-align: right;'>0.1376</td>
</tr>
<tr>
<td style='text-align: left;'>pulse'</td>
<td style='min-width: 7em; text-align: right;'>  0.0406</td>
<td style='min-width: 7em; text-align: right;'> 0.0144</td>
<td style='min-width: 7em; text-align: right;'>  2.82</td>
<td style='min-width: 7em; text-align: right;'>0.0048</td>
</tr>
<tr>
<td style='text-align: left;'>pmi=yes</td>
<td style='min-width: 7em; text-align: right;'>  0.4714</td>
<td style='min-width: 7em; text-align: right;'> 0.0486</td>
<td style='min-width: 7em; text-align: right;'>  9.70</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'>miloc=Other</td>
<td style='min-width: 7em; text-align: right;'>  0.3144</td>
<td style='min-width: 7em; text-align: right;'> 0.1162</td>
<td style='min-width: 7em; text-align: right;'>  2.70</td>
<td style='min-width: 7em; text-align: right;'>0.0068</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>miloc=Anterior</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.5441</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0444</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 12.27</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

## Semi-Saturated Model
The only way that ORs for treatment can vary in a logistic model is for there to be one or more interactions between baseline covariates and treatment.  Here we allow for all interactions with treatment, which will allow ORs, RRs, and ARRs to vary as the data dictate, assuming that three-way interactions are not needed, e.g., the infarct location-specific treatment effect does not depend on age.


```r
g <- lrm(day30 ~ tx * (age + Killip + pmin(sysbp, 120) +
         lsp(pulse, 50) + pmi + miloc), data=gusto,
         eps=0.005, maxit=60)
print(g, coefs=FALSE)
```

<!--html_preserve-->
 <div align=center><strong>Logistic Regression Model</strong></div>
 
 <pre>
 lrm(formula = day30 ~ tx * (age + Killip + pmin(sysbp, 120) + 
     lsp(pulse, 50) + pmi + miloc), data = gusto, eps = 0.005, 
     maxit = 60)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 40830</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 4159.64</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.244</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.821</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 37979</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 32</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.428</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.641</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2851</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 4.170</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.642</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-10</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.082</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.083</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.055</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

<!--/html_preserve-->

The likelihood ratio test is the gold-standard frequentist method for testing for added value of the treatment interactions, i.e., whether the treatment ORs are constant over covariate settings.


```r
lrtest(f, g)
```

```

Model 1: day30 ~ tx + age + Killip + pmin(sysbp, 120) + lsp(pulse, 50) + 
    pmi + miloc
Model 2: day30 ~ tx * (age + Killip + pmin(sysbp, 120) + lsp(pulse, 50) + 
    pmi + miloc)

L.R. Chisq       d.f.          P 
 16.552157  20.000000   0.681833 
```

We see no evidence to suggest HTE for t-PA or SK effects.  We can also use AIC to assess whether allowing for interactions will likely result in better patient-specific outcome predictions.


```r
c(AIC(f), AIC(g))
```

```
[1] 16558.31 16581.76
```

The simpler model has a lower (better) AIC, indicating that treatment interactions are not strong enough to overcome the overfitting that allowing for them would entail.  In other words, the interactions are likely more noise than signal.

We can also compare the two models by computing the relative explained variation on the risk scale as detailed [here](/post/addvalue).


```r
rexv <- var(predict(f, type='fitted')) /
        var(predict(g, type='fitted'))
rexv
```

```
[1] 0.9959584
```

From this we see that even without penalizing for overfitting, all the treatment interactions account for only 0.004 of the predictive information.  The no-interaction logit-additive model that assumes constancy of treatment ORs is at least 0.996 adequate on a scale from 0 to 1.

## Back to the Difficulty of the Task
Covariate-specific treatment effects are combinations of main effects and interaction effects, and one needs evidence that the interaction effect is non-zero before covariate-specific relative efficacy is interesting.  As an example of how difficult it is to estimate differential treatment effects (here, double differences on the logit scale, or ratios of odds ratios), let's temporarily re-fit the logistic model including only a single treatment interaction---with location of infarct.  Consider just inferior vs. anterior infarcts, which are the dominant categories and are not too imbalanced.

We estimate the double difference in log odds for t-PA vs. SK in anterior infarcts minus t-PA vs. SK in inferior infarcts.  Then we get the frequency-weighted overall treatment effect in these two groups, which is similar to dropping the interaction from the model.


```r
i <- lrm(day30 ~ tx * miloc + age + Killip + pmin(sysbp, 120) +
         lsp(pulse, 50) + pmi, data=gusto,
         eps=0.005, maxit=30)
contrast(i, list(tx='tPA', miloc='Inferior'),
            list(tx='SK',  miloc='Inferior'),
            list(tx='tPA', miloc='Anterior'),
            list(tx='SK',  miloc='Anterior'))
```

```
    Contrast      S.E.       Lower     Upper    Z Pr(>|z|)
11 0.1546134 0.1081337 -0.05732489 0.3665516 1.43   0.1528

Confidence intervals are 0.95 individual intervals
```

```r
w <- with(gusto, c(sum(miloc=='Inferior'), sum(miloc=='Anterior')))
contrast(i, list(tx='tPA', miloc=c('Inferior', 'Anterior')),
            list(tx='SK',  miloc=c('Inferior', 'Anterior')),
            type='average', weights=w)
```

```
    Contrast       S.E.     Lower       Upper     Z Pr(>|z|)         var
1 -0.1838079 0.05588118 -0.293333 -0.07428276 -3.29    0.001 0.003122706

Confidence intervals are 0.95 individual intervals
```

The first contrast (double difference; differential treatment effect) has a standard error that is twice that of the second contrast (treatment effect averaged over the two infarct locations), and only the latter overall treatment effect has evidence for being non-zero (i.e., a benefit of t-PA).
               
## Penalization
Bayesian modeling of HTE uses hierarchical random effects with shrinkage of interaction effects towards zero, e.g., shrinkage of "subgroup" effects towards the grand mean treatment effect.  Borrowing information in this fashion is an optimal way to get covariate-specific treatment effect estimates.  The frequentist counterpart that handles mixtures of continuous and categorical baseline variables is penalized maximum likelihood estimation.  We will leave the main effects unpenalized, favoring an additive (in the log odds) model with many fewer parameters but allowing the completely unpenalized model to also have a chance at winning, and find the penalty for interaction terms that optimizes the effective AIC, i.e., the AIC computed using the effective degrees of freedom.  Note that the `pentrace` function uses the likelihood ratio χ<sup>2</sup> scale for AIC unlike the traditional AIC formula, i.e., here bigger AIC is better.


```r
h <- update(g, x=TRUE, y=TRUE)
p <- pentrace(h,
              list(simple=0,
                   interaction=1000*
                    c(1, 5, 10, 15, 20, 30, 40, 100, 200, 500, 1000)),
              maxit=30)
p
```

```

Best penalty:

 simple interaction       df
      0       1e+06 12.03211

 simple interaction       df      aic      bic    aic.c
      0        1000 20.33720 4111.002 3935.753 4110.981
      0        5000 15.75213 4115.700 3979.961 4115.687
      0       10000 14.32236 4117.036 3993.618 4117.025
      0       15000 13.69510 4117.613 3999.600 4117.603
      0       20000 13.33778 4117.937 4003.003 4117.928
      0       30000 12.94300 4118.289 4006.757 4118.280
      0       40000 12.72881 4118.477 4008.791 4118.469
      0      100000 12.30905 4118.837 4012.768 4118.829
      0      200000 12.15779 4118.963 4014.197 4118.955
      0      500000 12.06394 4119.040 4015.083 4119.032
      0     1000000 12.03211 4119.066 4015.383 4119.058
```

```r
h <- update(g, penalty=list(simple=0, interaction=30000),
            maxit=30, eps=0.005)
effective.df(h)
```

```

Original and Effective Degrees of Freedom

                         Original Penalized
All                            32     12.94
Simple Terms                   11     11.00
Interaction or Nonlinear       21      1.94
Nonlinear                       3      1.05
Interaction                    20      0.94
Nonlinear Interaction           2      0.05
```

The following plots show first the main effect coefficients (with interaction effects penalized vs. not penalized), and second the interaction effect coefficients.  Note that main effect coefficients can change just because of penalizing interactions, just as removing interactions from models can greatly change main effects.


```r
ia <- grep('\\*', names(coef(g)))
x <- coef(g)[-c(1,ia)]; y <- coef(h)[-c(1,ia)]
r <- range(c(x, y))
plot(x, y,
     xlab='Unpenalized Coefficients',
     ylab='Penalized Coefficients',
     main='Main Effects', xlim=r, ylim=r)
abline(a=0, b=1, col=gray(.85))
x <- coef(g)[ia]; y <- coef(h)[ia]
r <- range(c(x, y))
plot(x, y,
     xlab='Unpenalized Coefficients',
     ylab='Penalized Coefficients',
     main='Interaction Parameters', xlim=r, ylim=r)
abline(a=0, b=1, col=gray(.85))
```

<img src="/post/varyor_files/figure-html/showshrink-1.png" width="672" /><img src="/post/varyor_files/figure-html/showshrink-2.png" width="672" />

Of the penalties studied, the best penalty by both AIC and corrected AIC is 1e+06 which effectively makes the model estimate 12.03 parameters instead of the original 12 parameters.  The optimum penalty is really infinity, consistent the the likelihood ratio test for presence of any interactions with treatment.  To give the benefit of the doubt we will use a penalty of 30000 to achieve an effective d.f. for interactions of 0.94.

### Using the Penalized Semi-Saturated Model to Estimate Distributions of Various Effects
Even though there is no statistical evidence for HTE (i.e., interactions with treatment on an appropriate scale), we will still allow for all interactions, to give the benefit of the doubt.  But we are penalizing the interaction parameters closer to what will cross-validate.

Let's start with what I believe is the best way to communicate results to an individual patient: estimating the absolute risk of mortality under two treatment options, without hinting how the patient should summarize the two estimates (absolute vs. absolute difference, etc.).  The baseline covariate values for one patient are listed below.


```r
d <- data.frame(tx=c('SK', 'tPA'), Killip='II', age=55, pulse=40, sysbp=100,
                miloc='Inferior', pmi='no')
p <- predict(h, d, type='fitted')
names(p) <- c('SK', 't-PA')
p
```

```
        SK       t-PA 
0.04043122 0.03302851 
```

Now turn to estimating treatment effects for all the patients in the RCT.  To estimate patient-specific treatment effects on various scales, we estimate risks of dying within 30 days for each patient with their own covariate settings (baseline covariate values).  This is done first with treatment set to SK, no matter which treatment the patient actually received.  Then predictions are done setting treatment to t-PA.


```r
d <- gusto
d$tx <- 'SK'
psk  <- predict(h, d, type='fitted')
d$tx <- 'tPA'
ptpa <- predict(h, d, type='fitted')
k  <- c(5,10,15,20,50,100,200,400,800,1600,3200,20000)
pl <- c(0, quantile(psk, 0.99))
j  <- pmax(psk, ptpa) <= pl
ggfreqScatter(psk[j], ptpa[j], cuts=k,
              xlab='P(death|SK)', ylab='P(death|t-PA)')
```

<img src="/post/varyor_files/figure-html/risks-1.png" width="672" />

Here is the distribution of estimated ARR across all patient types.


```r
hist(psk - ptpa, xlim=c(-.05, .1), nclass=200, main=NULL)
```

<img src="/post/varyor_files/figure-html/darr-1.png" width="672" />

And the distribution of estimated RR.


```r
hist(ptpa / psk, nclass=200, xlim=c(.75, 1), main=NULL)
```

<img src="/post/varyor_files/figure-html/drr-1.png" width="672" />

And finally the distribution of estimated ORs.


```r
or <- exp(qlogis(ptpa) - qlogis(psk))
hist(or, nclass=200, xlim=c(.75, 1), main=NULL)
```

<img src="/post/varyor_files/figure-html/dor-1.png" width="672" />

The following scatterplot shows the relationship between estimated RRs and estimate ORs.


```r
ggfreqScatter(or, ptpa / psk, cuts=k, xlab='OR', ylab='RR')
```

<img src="/post/varyor_files/figure-html/rror-1.png" width="672" />
See that the variation in absolute or relative risk reduction is large, but adjusted odds ratios for t-PA vs. SK have a very narrow distribution.  If we relied on the formal interaction test, this distribution would have a single value.

To see how the various measures depend on baseline risk, see the following graphs.


```r
xl <- 'Risk Under Control Treatment (SK)'
j <- psk <= pl
ggfreqScatter(psk[j], psk[j] - ptpa[j], cuts=k, xlab=xl, ylab='ARR')
```

```r
ggfreqScatter(psk[j], ptpa[j] / psk[j], cuts=k, xlab=xl, ylab='RR')
```

```r
ggfreqScatter(psk[j], or[j], cuts=k, xlab=xl, ylab='OR')
```

<img src="/post/varyor_files/figure-html/vsbase-1.png" width="672" /><img src="/post/varyor_files/figure-html/vsbase-2.png" width="672" /><img src="/post/varyor_files/figure-html/vsbase-3.png" width="672" />
------

## Discussion
Add your comments, suggestions, and criticisms on [datamethods.org](http://datamethods.org/t/discussion-of-assessing-heterogeneity-of-treatment-effect-estimating-patient-specific-efficacy-and-studying-variation-in-odds-ratios-risk-ratios-and-risk-differences)

------
