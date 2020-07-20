---
title: RCT Analyses With Covariate Adjustment
author: Ewout Steyerberg
date: '2020-07-19'
slug: covadj
tags:
  - 2020
  - drug-evaluation
  - generalizability
  - medicine
  - personalized-medicine
  - prediction
  - RCT
  - regression
link-citations: yes
summary: 'This article summarizes arguments for the claim that the primary analysis of treatment effect in a RCT should be with adjustment for baseline covariates.  It reiterates some findings and statements from classic papers, with illustration in the GUSTO-I trial.'
header:
  caption: ''
  image: ''
---



<small><tt>e.w.steyerberg@lumc.nl</tt></small><br><small><tt> [Twitter: ESteyerberg](https://twitter.com/ESteyerberg) </tt></small><br><small><tt> [Google scholar](https://scholar.google.com/citations?user=_75LDyMAAAAJ&hl=nl) </tt></small><br><small><tt> [ORCID](https://orcid.org/0000-0002-7787-0122) </tt></small>

The **_[PATH](https://www.ncbi.nlm.nih.gov/pubmed/31711134)_** (Predictive Approaches to Treatment effect Heterogeneity) Statement outlines principles, criteria, and key considerations for applying predictive approaches to clinical trials to provide patient-centered evidence in support of decision making.
The focus of PATH is on modeling of “heterogeneity of treatment effect” (**_HTE_**), which refers to the nonrandom variation in the magnitude of the absolute treatment effect (**_'treatment benefit'_**) across individual patients.  A more focused definition is that HTE refers to variation of treatment effect on a scale for which it is possible that no such variation exists, even if the treatment has a nonzero effect on the average.

The recent PATH statement lists a number of principles and guidelines. A key principle is in *[Fig 2](https://www.ncbi.nlm.nih.gov/pubmed/31711134)*:

>    “A risk-modeling approach to RCT analysis is likely to be most valuable when an overall treatment effect is well established; 
>    subgroup results (including risk-based subgroup results) from overall null trials should be interpreted cautiously.”  

Here I discuss how we establish **_‘overall treatment effect’_**. I reiterate some findings and statements from classic papers in favor of covariate adjustment as the key analysis. 

### Illustration in the GUSTO-I trial
For illustration we may analyze 30,510 patients with an acute myocardial infarction as included in the GUSTO-I trial. This illustration starts as the blog by **Frank Harrell** on 
**_[examining HTE](https://www.fharrell.com/post/varyor/)_**.



```r
load(url('http://hbiostat.org/data/gusto.rda'))
# keep only SK and tPA arms; and selected set of covariates
gusto <- upData(gusto, subset=tx %in% c('SK', 'tPA'),
                tx=droplevels(tx),
                keep=Cs(day30, tx, age, Killip, sysbp, pulse, pmi, miloc, sex))
```

```
Input object size:	 5241552 bytes;	 29 variables	 40830 observations
Modified variable	tx
Kept variables	day30,tx,age,Killip,sysbp,pulse,pmi,miloc,sex
New object size:	1349744 bytes;	9 variables	30510 observations
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
 <font color="MidnightBlue"><div align=center><span style="font-weight:bold">gusto <br><br> 9  Variables   30510  Observations</span></div></font> <hr class="thinhr"> <span style="font-weight:bold">day30</span> <style>
 .hmisctable508078 {
 border: none;
 font-size: 85%;
 }
 .hmisctable508078 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable508078 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable508078">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Sum</th><th>Mean</th><th>Gmd</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td><td>0.195</td><td>2128</td><td>0.06975</td><td>0.1298</td></tr>
 </table>
 <hr class="thinhr"> <span style="font-weight:bold">sex</span>: Sex <style>
 .hmisctable169295 {
 border: none;
 font-size: 85%;
 }
 .hmisctable169295 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable169295 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable169295">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value        male female
 Frequency   22795   7715
 Proportion  0.747  0.253
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">Killip</span>: Killip Class<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAA0AAAANCAMAAABFNRROAAAAOVBMVEUAAAAdHR02NjZlZWWioqKwsLC+vr6/v7/KysrOzs7U1NTZ2dna2trl5eXm5ubr6+vs7Ozz8/P///+qr/yoAAAAMElEQVQImWNg4OZHAAYGASEEIJ/HwYMApJnCyyyIxGNl4ITzuPlZGNj4uRjZ+fmYAJouCcAuq6zZAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable312722 {
 border: none;
 font-size: 85%;
 }
 .hmisctable312722 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable312722 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable312722">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>4</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value          I    II   III    IV
 Frequency  26007  3857   417   229
 Proportion 0.852 0.126 0.014 0.008
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">age</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAAB2lBMVEUAAAABAQEDAwMEBAQFBQUGBgYHBwcICAgKCgoLCwsMDAwODg4QEBARERESEhIUFBQWFhYXFxcYGBgaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQmJiYnJycpKSkqKiorKystLS0uLi4vLy8wMDAxMTEzMzM0NDQ1NTU2NjY3Nzc4ODg5OTk6Ojo7Ozs8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERFRUVGRkZHR0dISEhJSUlKSkpLS0tMTExNTU1PT09QUFBRUVFSUlJUVFRWVlZXV1dYWFhZWVlbW1teXl5gYGBhYWFkZGRlZWVnZ2doaGhqampra2ttbW1vb29wcHBycnJzc3N1dXV3d3d4eHh6enp7e3t8fHx/f3+BgYGGhoaHh4eIiIiJiYmKioqLi4uMjIyPj4+RkZGWlpaXl5eZmZmdnZ2enp6hoaGioqKjo6OkpKSlpaWmpqanp6erq6usrKywsLCysrKzs7O4uLi8vLy9vb3AwMDCwsLGxsbJycnKysrPz8/S0tLV1dXY2NjZ2dna2tre3t7j4+Pk5OTl5eXp6enq6urr6+vt7e3u7u7v7+/x8fHy8vL29vb4+Pj5+fn7+/v8/Pz+/v7///80JPtyAAAB6klEQVQ4jb3V2VPTUBQG8K9BWlooW62lpbSFUAyQdEuapDVpEURQy+KGC4IoVXBXXKAosqiAoEIQVMD+r17HF8fRMdNov5d7Zu6dc39zXg404/mQ41px7a6WPdB1wTE3Z53IPn9rtCfyhvLFc+9OMINWFknXwQycCUQ5KFUlo1PG+uYNuHY2Rx5fR9pUpYBhkXAi/d3FMsSFDqx+Krrrc+jhjZnFq3EFkhcp/M6VAn26eC5tL7/5xj12FqqlPA06AvHPLm/vzfX1orj2t+2j2XYVfCNx2dS/uSRI3MlHY4W61vRmachJCfCwaGsAby6LwceAc0GAPYqmIMK1pKyNINiEmB08XBwYP2Jll3OvdH/xU/TOa/8d1WmnUvAJeuclIkJDtVmOlW4XMi89j5aXz2SSUApzqRiYXPkPLm1t0q8g1FK4q1HE7MrG13/o+shPP7AdR51szMVDbi4dir/eMOja3VlYffb05aXhQSQd5A/jLsGHFFV9bvzJ7PzWe10usov6ctrFCe3WeXG85+hIffcpSzdtUkw1IloOI16DBA7xaKMhVlAyvGGEfJDNVgkBFlE3ua0UEWQgOEjpEMA0Q6wkpTsKNgDJapbhDyHcAJmqkEC344jfM5zsv/9jDZ548ctevH1Fy/WR8xvGqUTi2QsCnAAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable543942 {
 border: none;
 font-size: 85%;
 }
 .hmisctable543942 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable543942 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable543942">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>5342</td><td>1</td><td>60.91</td><td>13.58</td><td>40.92</td><td>44.73</td><td>52.11</td><td>61.58</td><td>69.84</td><td>76.19</td><td>79.42</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :  19.0  20.8  21.0  21.0  21.4 ,  <font color="MidnightBlue">highest</font>:  91.9  92.3  96.5 108.0 110.0</span> <hr class="thinhr"> <span style="font-weight:bold">pulse</span>: Heart Rate <span style='font-family:Verdana;font-size:75%;'>beats/min</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAAB2lBMVEUAAAABAQECAgIDAwMEBAQGBgYHBwcICAgJCQkKCgoLCwsMDAwQEBARERESEhIUFBQVFRUWFhYXFxcYGBgZGRkaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQlJSUmJiYnJycoKCgpKSkqKiorKyssLCwtLS0wMDAyMjIzMzM0NDQ2NjY4ODg5OTk6Ojo7Ozs+Pj4/Pz9BQUFCQkJERERFRUVISEhLS0tMTExNTU1OTk5PT09QUFBRUVFTU1NVVVVWVlZXV1dZWVlaWlpbW1tcXFxdXV1eXl5fX19gYGBiYmJjY2NkZGRlZWVmZmZnZ2doaGhpaWlqampsbGxtbW1zc3N1dXV6enp8fHx9fX1/f3+BgYGDg4OEhISGhoaHh4eIiIiJiYmKioqLi4uRkZGSkpKTk5OUlJSYmJiZmZmcnJyenp6ioqKlpaWmpqaqqqqtra2urq6xsbGysrKzs7O4uLi5ubm9vb3BwcHCwsLIyMjLy8vMzMzOzs7Pz8/R0dHS0tLU1NTY2Nja2trb29ve3t7g4ODj4+Pl5eXm5ubn5+fo6Ojp6enq6urr6+vs7Ozu7u7w8PDy8vL19fX29vb39/f4+Pj5+fn6+vr9/f3+/v7///9/ur6QAAABy0lEQVQ4jc3V61cSQRgG8AeyDIoUS2xdL7u6qOQFyCVkJRUrNSowFG+oSJamVoB3NCw1i66sFZmX/V8dxNMX+7DeeT7MmTkz75nfeT/MQJSdoiZx9d+iwia/8CiBJDvsg/eI7s1+TUuGBvmFR8mhXAuIfFjYrpkex890cz1i/2JoDPF0cW2spFxtVOO+K3ZlKQ1coxmbKVcO9l1RzJ6ma01eerBM1wfxpiEL6O7HW+7OFCZ6TTKrDx95/Vr6M4zEwX55si2BVSkhxT6ffL/kHPqtGE+6KmnMJ12+Z6CK91waXPDNXFq3COfh2o7cxMgwGmnuMqhbxFVWAlB8J0acGih9rxEzW8/e9VHylgNV1cAN4lJzxMUUEFepEtpy4uoaxKzR+k29KG1+PTNX6F0EU/Y8oJD5j+uanrj0NBRFxg6MDtDwLH7xb/n7pO875GE5pov8Ra1h8UdTZyD1MbWHorZPz10Wm8tZZdYaaGSW5AJ0IZDDZkLFZhNkPqBjlNCwV6FkdEA+2dVxKqgMDK7rOXOG1Z53sbr5Llcn3OZrH7t40z1PC3/f3R53dDy1C+6Xa8Lyq4eCw9snigMvyKVBt/gkLMYdc0nBpJMMuyhT82Twv+sbAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable121111 {
 border: none;
 font-size: 85%;
 }
 .hmisctable121111 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable121111 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable121111">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>157</td><td>0.999</td><td>75.38</td><td>19.5</td><td> 50</td><td> 55</td><td> 62</td><td> 73</td><td> 86</td><td> 98</td><td>107</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0   1   6   9  20 ,  <font color="MidnightBlue">highest</font>: 191 200 205 210 220</span> <hr class="thinhr"> <span style="font-weight:bold">sysbp</span>: Systolic Blood Pressure <span style='font-family:Verdana;font-size:75%;'>mmHg</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAABxVBMVEUAAAACAgIDAwMEBAQFBQUGBgYICAgJCQkLCwsMDAwNDQ0ODg4PDw8QEBARERESEhIVFRUWFhYXFxcYGBgZGRkaGhobGxsdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQlJSUmJiYnJycoKCgpKSkrKysvLy8wMDAxMTEyMjI0NDQ3Nzc5OTk7Ozs8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERGRkZHR0dISEhKSkpLS0tNTU1OTk5PT09RUVFSUlJTU1NUVFRVVVVYWFhZWVlbW1tcXFxeXl5hYWFiYmJjY2NmZmZoaGhpaWltbW1ubm5wcHBxcXFzc3N0dHR2dnZ5eXl8fHyAgICBgYGHh4eIiIiJiYmKioqMjIyOjo6RkZGWlpaXl5eZmZmampqbm5ucnJydnZ2fn5+goKChoaGjo6Ompqapqamqqqqrq6usrKyvr6+xsbGysrKzs7O0tLS6urq7u7u+vr6/v7/BwcHFxcXGxsbKysrLy8vQ0NDU1NTW1tbX19fZ2dna2trb29vf39/h4eHo6Ojp6enq6urr6+vs7Ozu7u7w8PD09PT29vb4+Pj5+fn6+vr7+/v9/f3+/v7///8kMR30AAABrklEQVQ4jc3VV1PCQBQF4KugQKIoIvaKBaQYEOzYG9h77AULiopdUVHUiCX2wv5eV9GxPUmwnJ3sZHaSO9+clwWWY7rh7PV1h+usdwHEMTR4EToZfkDIBS6uw94SGNcsHCPkBGcgRL5wcXlv0YvLBp5/5OpRoX/pMsejLy7v9EVgXIxfcczjrVKGt1ZwrzMD4GQYO9gZZguG8GGRzb+xb/Gzr3y9r68JNQ1jvGsreA61S099ecCGSwOac1/+/Wagei2nlRHlXSIahsHOB/kcWGFJefDnLpPSIiakPtcoAIxjlxUWq7CLfXh2HZ3/qqusH7X1PblqSULSIFBAEtRgVxsYoRHrQLQsmALafYnSW/DXp2c/68Ljb1DF6uTeZmxcc1ZGWnRMpJQkhMKQYHifFPwU4NUg7myPLD7c3S8sub9DVyffd+G7qH7h8+3kMK9UbxgNOhWVS2kliiyNnJeoIFKDYgSSMIIMf04YSYgEofyPrtfw+KRQRMqi4mSx8uTsBGmOJlMty1PrVUpKl6vTUsbtIp3eYFivWTM7WuwdplLTCDtY2suyMyVN83VY8AjUEtiNCUnkQwAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable110768 {
 border: none;
 font-size: 74%;
 }
 .hmisctable110768 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable110768 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable110768">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>196</td><td>0.999</td><td>129</td><td>26.58</td><td> 92.0</td><td>100.0</td><td>112.0</td><td>129.5</td><td>144.0</td><td>160.0</td><td>170.0</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0  36  40  43  46 ,  <font color="MidnightBlue">highest</font>: 266 274 275 276 280</span> <hr class="thinhr"> <span style="font-weight:bold">miloc</span>: MI Location<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAAoAAAANCAMAAACn6Q83AAAAGFBMVEUAAAABAQE8PDyampq5ubna2trr6+v///8e6JE1AAAALUlEQVQImWNgYIMBBgZ2GMDKZEYwGRFMBgYGVihgQBElhsnCAmECbWdiAjkBAM1mAsMRV8USAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable253147 {
 border: none;
 font-size: 85%;
 }
 .hmisctable253147 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable253147 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable253147">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>3</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value      Inferior    Other Anterior
 Frequency     17582     1062    11866
 Proportion    0.576    0.035    0.389
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">pmi</span>: Previous MI <style>
 .hmisctable318661 {
 border: none;
 font-size: 85%;
 }
 .hmisctable318661 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable318661 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable318661">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value         no   yes
 Frequency  25452  5058
 Proportion 0.834 0.166
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">tx</span> <style>
 .hmisctable494098 {
 border: none;
 font-size: 85%;
 }
 .hmisctable494098 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable494098 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable494098">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value         SK   tPA
 Frequency  20162 10348
 Proportion 0.661 0.339
 </pre>
 <hr class="thinhr"> </div><!--/html_preserve-->

#### Overall treatment effect
The simplest analysis of treatment effect is by performing an intention-to-treat analysis of the randomized patients for the primary outcome (30-day mortality). In GUSTO-I, 10,348 patients were randomized to receive tPA; 20,162 to SK and had 30-day mortality status known. The 30-day mortality was 653/10,348 = 6.3% vs 1475/20,162 = 7.3%; an absolute difference of 1.0%, or an odds ratio of 0.85 [0.78-0.94].


```r
# simple cross-table
table1(~ as.factor(day30) | tx, data=gusto, digits=2)  
```

<!--html_preserve--><div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>SK<br><span class='stratn'>(N=20162)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>tPA<br><span class='stratn'>(N=10348)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=30510)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'><span class='varlabel'>as.factor(day30)</span></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>0</td>
<td>18687 (92.7%)</td>
<td>9695 (93.7%)</td>
<td>28382 (93.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>1</td>
<td class='lastrow'>1475 (7.3%)</td>
<td class='lastrow'>653 (6.3%)</td>
<td class='lastrow'>2128 (7.0%)</td>
</tr>
</tbody>
</table>
</div><!--/html_preserve-->

```r
# 'tpa' as 0/1 variable for tPA vs SK treatment
gusto$tpa <- 1 * (gusto$tx == 'tPA') # 0/1 coding for tPA treatment
label(gusto$tpa) <- "tPA"
levels(gusto$tpa) <- c("SK", "tPA")
tab2 <- table(gusto$day30, gusto$tpa)
result <- OddsRatio(tab2, conf.level = 0.95)
names(result) <- c("Odds Ratio", "Lower CI", "Upper CI")

kable(as.data.frame(t(result))) %>% kable_styling(full_width=F, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:right;"> Odds Ratio </th>
   <th style="text-align:right;"> Lower CI </th>
   <th style="text-align:right;"> Upper CI </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0.853 </td>
   <td style="text-align:right;"> 0.776 </td>
   <td style="text-align:right;"> 0.939 </td>
  </tr>
</tbody>
</table>

```r
# BinomDiffCI(x1 = events1, n1 = n1, x2 = events2, n2 = n2, ...)
CI      <- BinomDiffCI(x1 = tab2[2,1], n1 = sum(tab2[,1]), x2 = tab2[2,2], n2 = sum(tab2[,2]),
                       method = "scorecc")
colnames(CI) <- c("Absolute difference", "Lower CI", "Upper CI")

result <- round(CI, 3) # absolute difference with confidence interval
kable(as.data.frame(result)) %>% kable_styling(full_width=F, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:right;"> Absolute difference </th>
   <th style="text-align:right;"> Lower CI </th>
   <th style="text-align:right;"> Upper CI </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.004 </td>
   <td style="text-align:right;"> 0.016 </td>
  </tr>
</tbody>
</table>

### Adjustment for baseline covariates
The unadjusted odds ratio of 0.853 is a marginal estimate, while a lot can be said in favor of *conditional estimates*, where we adjust for prognostically important baseline characteristics.

There may be 3 compelling arguments in favor of conditioning on baseline covariates when we consider binary outcomes.

1.	Interpretation
2.	Statistical power
3.	Correction for baseline imbalance

#### Support from literature
Let’s look at some supportive points from references on these arguments. 

1.	Interpretation: 
*[Hauck et al, 1998](https://www.ncbi.nlm.nih.gov/pubmed/9620808)*, provide strong support.

>    **_Abstract:_** “The analyses of the primary objectives of randomized clinical trials often are not adjusted for covariates, except possibly for stratification variables. For analyses with linear models, adjustment is a precision issue only. … 
>    For nonlinear analyses, omitting covariates from the analysis of randomized trials leads to a loss of efficiency as well as a change in the treatment effect being estimated. We recommend that the primary analyses adjust for important prognostic covariates in order to come as close as possible to the clinically most relevant subject-specific measure of treatment effect. Additional benefits would be an increase in efficiency of tests for no treatment effect and improved external validity.”  
>    *Controlled Clin Trials 1998;19:249–256.*


* So, these authors emphasize argument 1 (*“to come as close as possible to the clinically most relevant subject-specific measure of treatment effect”*), and argument 2 (*“increase in efficiency of tests for no treatment effect”*); while also recognizing a remarkable issue in nonlinear models (*“a change in the treatment effect being estimated”*). This change is different from linear models, where the adjusted and unadjusted effects are on expectation equal. In nonlinear models such as the logistic regression model, effect estimates are **_non-collapsible_**.


2.	Statistical power:
*[Robinson & Jewell 1991](https://www.jstor.org/stable/1403444)* provide a fascinating paper on the impact of covariate adjustment in nonlinear models, such as the logistic regression model: the precision of the estimated treatment effect is worse than without adjustment, while conditioning makes that the expected effect is further from Null. **_Which impact is stronger?_** They show that efficiency is expected to increase (provided that the covariate is prognostic for the outcome).


3.	Correction for baseline imbalance
In RCTs, imbalance will arise by pure chance. It may hamper the interpretation of a treatment effect in a specific RCT if one group has a better prognosis according to baseline characteristics than another.  
Of course, we can only adjust for observed baseline characteristics. We argued in *[a 2000 AHJ paper](https://www.ncbi.nlm.nih.gov/pubmed/10783203)* that potential imbalances on other, unobserved patient characteristics do not invalidate attempts to correct for observed covariates.

### Practice in medical research
The statistical model for covariate adjustment can be simple or more complex. In various reviews researchers have noted that typically 5 to 10 baseline covariates are considered.  
Poor practice was noted for papers published in 2007. *[Pocock et al, Lancet 2000](https://www.ncbi.nlm.nih.gov/pubmed/10744093)* note:  

>    **_FINDINGS_**: Most trials presented baseline comparability in a table. These tables were often unduly large, and about half the trials inappropriately used significance tests for baseline comparison. Methods of randomisation, including possible stratification, were often poorly described. There was little consistency over whether to use covariate adjustment and the criteria for selecting baseline factors for which to adjust were often unclear. Most trials emphasised the simple unadjusted results and covariate adjustment usually made negligible difference. Two-thirds of the reports presented subgroup findings, but mostly without appropriate statistical tests for interaction. Many reports put too much emphasis on subgroup analyses that commonly lacked statistical power.
>
>    **_INTERPRETATION_**: Clinical trials need a predefined statistical analysis plan for uses of baseline data, especially covariate-adjusted analyses and subgroup analyses. Investigators and journals need to adopt improved standards of statistical reporting, and exercise caution when drawing conclusions from subgroup findings.

More recent papers show that covariate-adjusted analyses are far [more common](https://www.ncbi.nlm.nih.gov/pubmed/31269898):  

>    *Trials published in 2014 ... reported adjusted analyses in 87% with pre-specified adjustment in analyses in 95% ...*

Importantly, *[EMA guidance](https://www.ema.europa.eu/en/documents/scientific-guideline/guideline-adjustment-baseline-covariates-clinical-trials_en.pdf)* is available on how to do such analyses:

>    **_6.2. Number of covariates in the analysis_**  
>    No more than a few covariates should be included in the primary analysis. Even though methods of adjustment, such as analysis of covariance, can theoretically adjust for a large number of covariates it is safer to pre-specify a simple model.


## Illustration in GUSTO-I: adjust for age
A simple illustration is to examine the impact of age (which is a strong prognostic factor in many diseases) for [adjustment of the primary treatment effect in GUSTO-I](https://www.ncbi.nlm.nih.gov/pubmed/10783203).


```r
# Analyses
options(prType='html')
f0 <- lrm(day30 ~ tx, data=gusto)
print(f0) # coef tPA: -0.1586
```

<!--html_preserve-->
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = day30 ~ tx, data = gusto)
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
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 30510</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 10.82</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.517</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 28382</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 1</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.071</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.035</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2128</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) 0.0010</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.074</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.079</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 3×10<sup>-8</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.005</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.005</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.065</td>
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
<td style='min-width: 7em; text-align: right;'> -2.5392</td>
<td style='min-width: 7em; text-align: right;'> 0.0270</td>
<td style='min-width: 7em; text-align: right;'>-93.88</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=tPA</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -0.1586</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0486</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> -3.26</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.0011</td>
</tr>
</tbody>
</table>
<!--/html_preserve-->
So, we note that the **unadjusted** regression coefficient for tPA was -0.1586. 

Let's continue with age adjustment for the tPA effect.  
1. How different was the mean age between randomized groups?  
2. How much of the difference between adjusted and unadjusted effect estimate can be attributed to this imbalance?


```r
# Examine impact of age
table1(~ age | tx, data=gusto, digits=4) # age 61.03 in tPA vs 60.86 in SK group, delta: 0.17 years
```

<!--html_preserve--><div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>SK<br><span class='stratn'>(N=20162)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>tPA<br><span class='stratn'>(N=10348)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=30510)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'><span class='varlabel'>age</span></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>60.86 (11.87)</td>
<td>61.03 (11.97)</td>
<td>60.91 (11.90)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>61.58 [19.03, 110.0]</td>
<td class='lastrow'>61.57 [20.78, 108.0]</td>
<td class='lastrow'>61.58 [19.03, 110.0]</td>
</tr>
</tbody>
</table>
</div><!--/html_preserve-->

```r
f.age <- lrm(day30 ~ tpa + age, data=gusto)
print(f.age)
```

<!--html_preserve-->
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = day30 ~ tpa + age, data = gusto)
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
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 30510</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 1506.21</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.121</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.741</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 28382</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.118</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.482</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2128</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.060</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.483</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.062</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.063</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.061</td>
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
<td style='min-width: 7em; text-align: right;'> -7.9008</td>
<td style='min-width: 7em; text-align: right;'> 0.1634</td>
<td style='min-width: 7em; text-align: right;'>-48.34</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tpa</td>
<td style='min-width: 7em; text-align: right;'> -0.1878</td>
<td style='min-width: 7em; text-align: right;'> 0.0500</td>
<td style='min-width: 7em; text-align: right;'> -3.75</td>
<td style='min-width: 7em; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0821</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0023</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 35.23</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>
<!--/html_preserve-->
So, we note that the **adjusted** regression coefficient for tPA was -0.1878.  
   


```r
# Difference in tx effect by adjustment
d.tx.age <- f.age$coefficients[2] - f0$coefficients[2] 
# Impact of age difference on tx effect
d.age <- (mean(gusto$age[gusto$tpa==0]) - mean(gusto$age[gusto$tpa==1])) 
d.tx.ageimpact <- f.age$coefficients[3] * d.age

# Impact of stratification on age
# d.tx.age # -0.0291 stronger effect
# d.tx.ageimpact # -0.0138 because of prognostic difference: age difference between randomized groups
# d.tx.age - d.age # -0.0154 attributable to conditioning on age: stratification effect, non-collapsibility

f.unadjusted <- c("coef"=as.vector(f0$coefficients[2]) , SE=sqrt(f0$var[2,2]), d.coef=NA, d.SE=NA, 
                  "Imbalance (%)"=NA, "Stratification (%)"=NA)
f.age.adj <- c("coef"=as.vector(f.age$coefficients[2]) , SE=sqrt(f.age$var[2,2]), 
               d.coef=f.age$coefficients[2] / f0$coefficients[2] - 1, 
               d.SE=sqrt(f.age$var[2,2]) / sqrt(f0$var[2,2]) - 1 , 
               "Imbalance (%)"= d.tx.ageimpact / f0$coefficients[2], 
               "Stratification (%)"=(d.tx.age - d.tx.ageimpact)/ f0$coefficients[2] )
kable(as.data.frame(rbind(f.unadjusted, f.age.adj)), digits=3) %>% 
  kable_styling(full_width=F, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> coef </th>
   <th style="text-align:right;"> SE </th>
   <th style="text-align:right;"> d.coef </th>
   <th style="text-align:right;"> d.SE </th>
   <th style="text-align:right;"> Imbalance (%) </th>
   <th style="text-align:right;"> Stratification (%) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> f.unadjusted </td>
   <td style="text-align:right;"> -0.159 </td>
   <td style="text-align:right;"> 0.049 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> f.age.adj </td>
   <td style="text-align:right;"> -0.188 </td>
   <td style="text-align:right;"> 0.050 </td>
   <td style="text-align:right;"> 0.184 </td>
   <td style="text-align:right;"> 0.028 </td>
   <td style="text-align:right;"> 0.087 </td>
   <td style="text-align:right;"> 0.097 </td>
  </tr>
</tbody>
</table>

```r
# As Table III in Steyerberg 2000 paper
```


### Summary of impact of adjustment on coefficient and SE
Coefficient behavior: the unadjusted coeffient was -0.159; adjusted for age it is -0.188. This is a difference of -0.029, or +18% in estimate of the treatment effect *[Steyerberg, Bossuyt, Lee; AHJ 2000](https://www.sciencedirect.com/science/article/pii/S0002870300900012)*.  
Part of this change is attributable to a difference in age at baseline: the tPA group was slightly disadvantaged by a higher age (61.03 years) compared to the SK group (60.86 years). The difference of -0.168 years accounts for a change of -0.014 in the treatment effect estimate:  

>    d.age (in years) x f.age$coef[3] =  
>    -0.168 x 0.082 = -0.014.

The remaining difference is:

>    delta coefficient - delta attributable to age imbalance =   
>    d.tx.age - d.tx.ageimpact =  
>    -0.029 - -0.014 = -0.015.

So, the +18% more extreme effect estimate can be attributed for 8.7% to imbalance, and 9.7% to using a conditional rather than an unconditional model: stratification, or non-collapsibility (see also *[Gail et al, 1984](https://academic.oup.com/biomet/article/71/3/431/258100)*).


## Conclusions
The GUSTO-I serves well to illustrate the impact of conditioning on baseline covariates when we consider binary outcomes. The age-adjusted estimate of the overall treatment effect has a different interpretation than the unadjusted estimate: the effect for *‘Patients with acute MI’* versus *‘A patient with an acute MI of a certain age’*. The statistical power for testing of the adjusted effect is higher than that of the unadjusted effect. The required sample size is reduced by a factor of approximately (*1 – R^2^*). For age, the Nagelkerke *R^2^* was 12%. This implies that an analysis of the age-adjusted treatment effect with 88% of the sample size would have the same power as an **un**adjusted analysis in 100% of the sample. Finally, the age-adjusted treatment corrected for baseline imbalance.

### Implications for estimating heterogeneity of treatment effect
The unadjusted and adjusted estimate of the overall treatment effect discussed above are effect estimates on a relative scale: odds ratios on the odds scale. The translation from relative to absolute scale can be made by explicitly considering the baseline risk. If the baseline risk is low, the treatment benefit can only be small; If the baseline risk is high, the treatment benefit can be large.   
The recent Predictive Approaches to Treatment Effect Heterogeneity (*[PATH](https://www.ncbi.nlm.nih.gov/pubmed/31711134)*) Statement provides guidance on predictive approaches to heterogeneous treatment effects. The practical implementation of principles from the PATH statement is discussed in another [blog](post/PATH_statement). 

### References
  
#### Classics
MH Gail, S Wieand, S Piantadosi - Biometrika, 1984  
[Biased estimates of treatment effect in randomized experiments with nonlinear regressions and omitted covariates](https://academic.oup.com/biomet/article-abstract/71/3/431/258100)

SJ Senn - Statistics in medicine, 1989  
[Covariate imbalance and random allocation in clinical trials](https://onlinelibrary.wiley.com/doi/abs/10.1002/sim.4780080410?casa_token=4RTA9GQTbWUAAAAA:2u9pfwLTqo-6lcttSyhg7cX5UL6iKqDkuTQ3FNDovgs1rYXmTHo266tS54FAaGhVNRvrj2gfLiVMUNRZ)  

LD Robinson, NP Jewell - International Statistical Review, 1991
[Some surprising results about covariate adjustment in logistic regression models](https://www.jstor.org/stable/1403444)

WW Hauck, S Anderson, SM Marcus - Controlled clinical trials, 1998  
[Should we adjust for covariates in nonlinear regression analyses of randomized trials?](https://www.sciencedirect.com/science/article/pii/S0197245697001475)

SJ Pocock, SE Assmann, LE Enos… - Statistics in Medicine, 2002  
[Subgroup analysis, covariate adjustment and baseline comparisons in clinical trial reporting: current practice and problems](https://onlinelibrary.wiley.com/doi/abs/10.1002/sim.1296?casa_token=Rk_5yd2uqQ4AAAAA:qTn_uyj7pROZA4R36TGOyO7cEPGe-aFHsh94HQfCsSgJ1qFuKSbKDzeT-YTRrEwNfr_IrWX7AsR3_eYc)
   
  
#### Illustrations focused on neurotrauma RCTs
AV Hernández, EW Steyerberg, GS Taylor… - Neurosurgery, 2005  
[Subgroup analysis and covariate adjustment in randomized clinical trials of traumatic brain injury: a systematic review](https://academic.oup.com/neurosurgery/article-abstract/57/6/1244/2744714)

AV Hernández, EW Steyerberg, I Butcher… - Journal of Neurotrauma, 2006  
[Adjustment for strong predictors of outcome in traumatic brain injury trials: 25% reduction in sample size requirements in the IMPACT study](https://www.liebertpub.com/doi/abs/10.1089/neu.2006.23.1295?casa_token=J7RlMmSOCU4AAAAA:swfxgjHYssZtuezehaUj_8wWW4vTfyBBSoIcOxMOt1alyFeplks08KDUvsGPlNdCHS-ELqLI-MG1)

P Perel,…, EW Steyerberg, CRASH Trial Collaborators - Journal of clinical epidemiology, 2012  
[Covariate adjustment increased power in randomized controlled trials: an example in traumatic brain injury](https://www.sciencedirect.com/science/article/pii/S0895435611002770)
  
#### GUSTO-I references
EW Steyerberg, PMM Bossuyt, KL Lee - American heart journal, 2000  
[Clinical trials in acute myocardial infarction: should we adjust for baseline characteristics?](https://www.sciencedirect.com/science/article/pii/S0002870300900012)

Gusto Investigators - New England Journal of Medicine, 1993   
[An international randomized trial comparing four thrombolytic strategies for acute myocardial infarction](https://www.nejm.org/doi/full/10.1056/NEJM199309023291001)

Califf R, …, ML Simoons, EJ Topol, GUSTO-I Investigators - American heart journal, 1997
[Selection of thrombolytic therapy for individual patients: development of a clinical model](https://www.sciencedirect.com/science/article/pii/S0002870397701649)

#### Other references
EMA 2015: [Guideline on adjustment for baseline covariates in clinical trials](https://www.ema.europa.eu/en/documents/scientific-guideline/guideline-adjustment-baseline-covariates-clinical-trials_en.pdf)

BC Kahan, V Jairath, CJ Doré, TP Morris - Trials, 2014 [The risks and rewards of covariate adjustment in randomized trials: an assessment of 12 outcomes from 8 studies](https://trialsjournal.biomedcentral.com/articles/10.1186/1745-6215-15-139)

AV Hernández, MJC Eijkemans, EW Steyerberg - Annals of epidemiology, 2006  
[Randomized controlled trials with time-to-event outcomes: how much does prespecified covariate adjustment increase power?](https://www.sciencedirect.com/science/article/pii/S1047279705003248)

AV Hernández, EW Steyerberg… - Journal of clinical epidemiology, 2004  
[Covariate adjustment in randomized controlled trials with dichotomous outcomes increases statistical power and reduces sample size requirements](https://www.sciencedirect.com/science/article/pii/S0895435603003792)

DD Thompson, HF Lingsma, WN Whiteley, GD Murray, EW Steyerberg - Journal of clinical epidemiology, 2015  
[Covariate adjustment had similar benefits in small and large randomized controlled trials](https://www.sciencedirect.com/science/article/pii/S089543561400448X)
  

#### PATH Statement references
**The Predictive Approaches to Treatment effect Heterogeneity
(PATH) Statement**  
David M. Kent, MD, MS; Jessica K. Paulus, ScD; David van Klaveren, PhD; Ralph D’Agostino, PhD;
Steve Goodman, MD, MHS, PhD; Rodney Hayward, MD; John P.A. Ioannidis, MD, DSc; Bray Patrick-Lake, MFS; Sally Morton, PhD;
Michael Pencina, PhD; Gowri Raman, MBBS, MS; Joseph S. Ross, MD, MHS; Harry P. Selker, MD, MSPH; Ravi Varadhan, PhD;
Andrew Vickers, PhD; John B. Wong, MD; and Ewout W. Steyerberg, PhD  
*Ann Intern Med. 2020;172:35-45.*  

[Annals of Internal Medicine, main text](https://annals.org/acp/content_public/journal/aim/938321/aime202001070-m183667.pdf?casa_token=rjMhlqehmZYAAAAA:s2gnJxSo---fGeU5d3BYBJ0s3S8UMxMWhH7RQXNlK8WmHH7WvghrEZJODK1fI_8KsvW1bPavUew)

[Annals of Internal Medicine, Explanation and  Elaboration](https://annals.org/acp/content_public/journal/aim/938321/aime202001070-m183668.pdf?casa_token=HSOwDdYuIfUAAAAA:LprkkDo9-n-qcHqcOC5IHsDWQtjd3CO_SXrSdF0SimqTuIG5I6rKEwhT24ySEZwxTr7GLpn56vM)

[Editorial by Localio et al, 2020](https://annals.org/aim/fullarticle/2755584/advancing-personalized-medicine-through-prediction)
