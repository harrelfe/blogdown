---
title: Data Methods Discussion Site
author: Frank Harrell
date: '2018-06-19'
modified: '2018-07-11'
slug: disc
categories: []
tags:
  - collaboration
  - teaching
  - 2018
summary: 'This article lays out the rationale and overall design of a new discussion site about quantitative methods.'
header:
  caption: ''
  image: ''
---
I have learned more from Twitter than I ever thought possible, from those I follow and from my followers.  Quick pointers to useful resources has been invaluable.  I have also gotten involved in longer discussions.  Some of those, particularly those related to design and interpretation of newly published studies (especially randomized clinical trials --- RCTs), have gotten very involved and controversial.  Twitter is not designed for in-depth discourse, and I soon lose track of the discussion and others' previous points.  This is particularly true if I'm away from a discussion for more than 24 hours.  Also, some Twitter discussions would have been more civil had there been a moderator.

There are excellent discussion boards related to statistics, e.g. [stats.stackexchange.com](http://stats.stackexchange.com), [medstats](https://groups.google.com/forum/#!forum/medstats), and [talkstats](http://talkstats.com), and a variety of sites related to medical research (including clinical trials), epidemiology,  and machine learning.  An informal [Twitter poll](https://twitter.com/f2harrell/status/989486563947098112) was conducted on 2018-04-26 - 2018-04-27, resulting in 242 responses from those in my Twitter sphere.  Of those, 0.71 were in favor of creating a new site vs. 0.29 who wanted to solely use Twitter for discussions on the intended topics.

After much research, I've chosen [discourse.org](http://discourse.org) for the platform for a new discussion board.  This will require putting up a server to host the site.  Fortunately all the software needed (linux, ruby, discourse, etc.) is free.  After the site is up and running, more moderators will be required.  The site name will be `datamethods.org`.  We hope to have it running in July 2018.

As the purpose of communication/collaboration between quantitative experts and clinical/translational researchers is a key function of the [Biostatistics Epidemiology and Research Design](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5263220) (BERD) program of the national [Clinical and Translational Science Award](https://ncats.nih.gov/ctsa) program of NIH/NCATS, the Vanderbilt BERD (Biostatistics Epidemiology and Research Design) program will support this discussion site under its CTSA funded [VICTR](https://victr.vanderbilt.edu) center, and the national CTSA BERD consortium is likely to also be involved.  This has the potention to bring dozens of experienced statisticians and epidemiologists to the table to assist clinical and translational investigators and research consumers with their study design, analysis, and interpretation questions.

discourse.org recognizes participation and helpfulness.  A good example may be found [here](http://discourse.mc-stan.org).  The software also makes it very easy to find your place in a large number of discussions, and to upvoted answers to question.

The areas that will be emphasized in the new discussion site follow.  Global emphasis is on fostering communication between quantitatively-skilled persons and researchers not specializing in the math side of things.

* quantitative methods in general, including enhancing numeracy of those participants who are not into math or statistics
* general statistical issues such as analysis of change scores and categorization of continuous variables
* measurement issues
* interpretation of published statistical analyses
* statistical design of particular studies/clinical trials
* statistical analysis issues in published biomedical and epidemiologic research papers
* choosing optimal statistical graphics for presenting study results
* discussing statistical models and machine learning for biomedical and epidemiologic problems

The site will be organized into the following major and minor categories, with lots of tags available to further distinguish and cross-reference topics.  <b>Your input about these categories is needed.</b>

* statistics and data analysis
   + descriptive and exploratory
   + formal analysis
   + uncertainty
   + missing data and measurement error
   + models
   + modeling strategies (including model specification, nonlinearities, interactions and heterogeneity of treatment effect, avoiding categorization)
   + variable selection
   + data reduction, clustering, unsupervised learning
   + accuracy and information measures
   + model validation and interpretation
   + bayes
   + machine learning
   + comparative methods performance
   + causal inference
   + comparison of nonrandomized treatments
   + probability
   + inference and generalizability
* math and numeracy
* research methods
   + measurement
   + study and experimental design
   + sample survey design
   + study interpretation
   + meta-analysis
* RCTs
   + general issues and ethics
   + design issues for a specific trial
   + statistical interpretation of specific trials
* computing
   + systems
   + tools
   + languages
   + web applications
   + databases
   + big data
* graphics
   + static
   + dynamic
   + programming
* education
   + teaching and learning methods
   + statistical
   + scientific method & design
   + collaboration
   + knowledge dissemination
   + career development
* topical areas
   + diagnosis
   + biomarker research
   + cardiology
   + cancer
   + psychology
   + nutrition
   + epidemiology
   + health policy
   + drug development
* journal articles
   + journal club
   + reviews
* news (courses, webcasts, meetings, etc.)
* meta (discussion, pointers, etiquette about the site)

More major categories can be added as needed.

To discuss this proposal, post a tweet mentioning @f2harrell, or use the commenting facility at the end of this post.

## Links to Resources
* [discourse.org civility guidelines](http://discourse.mc-stan.org/faq)
* [Using Discourse effectively with group chat](https://blog.discourse.org/2018/04/effectively-using-discourse-together-with-group-chat)

## Discourse Information
### Ways to Create an Account
* Link to your Google, Facebook, Twitter, Yahoo, or Github account
* Specify your email address and password
