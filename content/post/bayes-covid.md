﻿---
title: "Bayesian Methods to Address Clinical Development Challenges for COVID-19 Drugs and Biologics"

author: Natalia Muhlemann, Rajat Mukherjee, Frank Harrell.
date: 2020-05-29
slug: bayes-covid 
categories: []
tags:
  - bayes
  - RCT
  - design
  - drug-evaluation
  - medicine
  - responder-analysis
  - covid-19

summary: The COVID-19 pandemic has elevated the challenge for designing and executing clinical trials with vaccines and drug/device combinations within a substantially shortened time frame.  Numerous challenges in designing COVID-19 trials include lack of prior data for candidate interventions / vaccines due to the novelty of the disease, evolving standard of care and sense of urgency to speed up development programmes. We propose sequential and adaptive Bayesian trial designs to help address the challenges inherent in COVID-19 trials. In the Bayesian framework, several methodologies can be implemented to address the complexity of the primary endpoint choice. Different options could be used for the primary analysis of the WHO Severity Scale, frequently used in COVID-19 trials.  We propose the longitudinal proportional odds mixed effects model using the WHO Severity Scale ordinal scale. This enables efficient utilization of all clinical information to optimize sample sizes and maximize the rate of acquiring evidence about treatment effects and harms.
---
Natalia Muhlemann, MD<br><small><tt>natalia.muhlemann@cytel.com</tt></small><br><small><tt>linkedin.com/in/natalia-m%C3%BChlemann-2a3a1841</tt></small><br><small><tt>cytel.com</tt></small><br><br>
Rajat Mukherjee, PhD<br><small><tt>rajat.mukherjee@cytel.com</tt></small><br><small><tt>linkedin.com/in/rajatmukherjeestats</tt></small><br><small><tt>cytel.com</tt></small><br><br>
Frank E Harrell Jr, PhD<br><small><tt>hbiostat.org/fh</tt></small>

##  Challenges of Drugs and Biologics Development for a New Disease 

The COVID-19 pandemic has elevated the challenge of designing and executing clinical trials within a substantially shortened time frame.  Instead of the usual months and years, the same goals have to be accomplished in weeks and months. 

The urgency of finding solutions, from the development of vaccines to drug/device combinations and drugs to treat critically ill COVID-19 patients has united clinical development stakeholders resulting in unprecedented speed and collaboration. Currently, there are 1265 trials according to https://covid19-trials.com. 

Numerous challenges in designing COVID-19 trials include lack of prior data for candidate interventions / vaccines due to the novelty of the disease, evolving standard of care due to the collaborative effort of healthcare professionals worldwide and sense of urgency to speed up development programmes. The sense of urgency incites clinical researchers to invoke innovative trial design approaches to expedite the identification of efficacious interventions without compromising patient safety and scientific rigor.  Bayesian statistical methods are very well suited to address these challenges due to their ability to adapt to knowledge that is gained during a trial. 

## Flexible Bayesian Designs for COVID-19 Trials 

Bayesian methods compute posterior probabilities (PP) of efficacy/harm superseded by current data.  PP are made for decision making and do not involve α-spending because previous data looks merely become obsolete in forward-in-time inference.  This gives trials extreme flexibility. First, learning can lead to adaptations such as population enrichment, dose selection, adding or dropping of arms, including new standard-of-care (SOC) arms and modifying the follow-up period. Second, highly sequential trial designs with unlimited data looks can be chosen to accelerate learning from data (see hbiostat.org/proj/covid19 for a detailed example). Sufficient evidence may exist and the trial may be stopped if PP(efficacy) is high or PP(harm) is moderately high, for example.  Bayesian predictive distributions can be used to compute the chance that a trial would not succeed even if it proceeded to the planned maximum sample size (futility).  

Well-chosen statistical methods can maximize the use of all available clinical information 

It is challenging to select an optimal primary endpoint without prior information on the intervention and with limited but rapidly accumulating data on the course of the disease and outcomes.  The endpoint must be clinically relevant and able to answer the research question within a reasonable follow-up duration and sample size. Endpoints for Phase 2 trials could focus on more sensitive laboratory indices or functional parameters, such as oxygen saturation measures for respiratory function. Vaccine trials could focus on immunogenicity. This should inform the decision whether the treatment delivers the expected effects based on known mechanisms of action in the shortest possible timeframe.  It is important to collect data on all clinical measures that could be the primary endpoint for a Phase 3 trial to enable Bayesian borrowing of Phase 2 data, or for continuous Bayesian learning without the need to distinguish Phases 2 and 3. 

In the Bayesian framework, several methodologies can be implemented to address the complexity of the chosen primary endpoint. The WHO Severity Scale can be used in COVID-19 trials and reflects clinical improvement as well as hospital and ICU resource utilization.  Different options could be used for the primary analysis of the WHO Severity Scale.  

The WHO Severity Scale can be analysed using the proportional odds model at a pre-defined time, usually at Day 14 or Day 28. The strength of this approach is that it gives a clear outcome of the disease severity evolution by a certain timepoint. However, the choice of time point can lead to reduced sensitivity to detect treatment benefit. For example, in moderate or severe COVID-19 patients an investigational treatment could speed up recovery and enable earlier discharge from hospital.  Many investigators have embraced “responder analysis” to create a simple binary outcome, including using time to “response” as an outcome.  As detailed in bit.ly/datamethods-responder this is arbitrary and requires far greater sample sizes.  Statistical problems magnify when the “responder” is based on a change in state (e.g. a 2-point shift on the WHO ordinal COVID-19 Severity Scale).  To minimize sample sizes and maximize the rate of acquiring evidence about treatment effects and harms, it is imperative to use all of the clinical information available.  If the WHO ordinal scale is measured at more than one follow-up timepoint, a powerful full-information approach that respects the ordinal nature of the outcome is the proportional odds ordinal logistic model with random (subject) effects (hbiostat.org/proj/covid19). This longitudinal model includes a time effect so that early vs. late treatment benefits can be estimated.  An alternative is an ordinal state transition model ([Lee K, Daniels MJ. 2007](https://doi.org/10.1111/j.1541-0420.2007.00800.x)).

For critically ill COVID-19 patients, it is very important to take into consideration mortality and the need for mechanical ventilation. A treatment targeted to reduce requirement for mechanical ventilation and increase Ventilator Free Day (VFD) also needs to demonstrate that it does not increase mortality. A composite endpoint approach can take mortality and other outcomes of interest into consideration. However, this method is criticized for handling all endpoints with equal clinical importance unless pre-specified or data-driven weighting is introduced, which further complicates the interpretability of the results. A composite/ordinal endpoint with clinical hierarchy ([Finkelstein DM, Schoenfeld DA. 1999](https://doi.org/10.1002/(SICI)1097-0258(19990615)18:11<1341::AID-SIM129>3.0.CO;2-7), [Pocock SJ et al. 2012](https://doi.org/10.1093/eurheartj/ehr352)) overcomes the drawbacks of the traditional composite approach while maintaining the benefits of increasing study power.  For illustration, a hierarchical composite of mortality, requirement for extracorporeal membrane oxygenation (ECMO) and VFD, based on clinical priority for critically ill COVID-19 patients, allows mortality to be considered as the most important outcome. With no difference in mortality, the comparison moves to the next endpoint in the hierarchy. 

Since the majority of COVID-19 patients are expected to be cured, interest in time to death as well as proportion cured can be handled using mixture cure models. This can also be formulated in the Bayesian framework with non-informative as well as informative priors ([Psioda MA, Ibrahim JA. 2018](https://doi.org/10.1002/sim.7846)).

Increase benefits to the patient population without compromising the scientific integrity of the trial

COVID-19 studies recruit very fast and early looks can be very important for clinical practice, allowing reduced patient exposure to futile or harmful health technologies, and rapid re-focus on potentially more promising trials. 

Patients with different disease severity can respond differently to therapy. Bayesian analysis based on continuous learning from study subpopulations can inform decisions to drop non-responding sub-groups and focus enrolment on promising subgroups, therefore enabling the enrolment of more patients with a higher likelihood to benefit from a treatment. Bayesian design allows for adaptations as new standards of care emerge. It may be possible to borrow from external data on new standards of care. 

## Conclusion 

Sequential and adaptive Bayesian trial designs are very well suited to help address the challenges inherent in COVID-19 trials and reflect uncertainty in the truly needed sample size. The longitudinal proportional odds mixed effects model using the WHO Severity Scale ordinal scale enables efficient utilization of all clinical information to optimize sample sizes and maximize the rate of acquiring evidence about treatment effects and harms. 

## Resources

* [hbiostat.org/proj/covid19](https://hbiostat.org/proj/covid19)
* [datamethods.org](https://discourse.datamethods.org/tag/covid-19)

