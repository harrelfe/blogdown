+++
date = 2019-11-08  # Schedule page publish date.
title = "R for Clinical Trial Reporting"
time_start = 2019-09-13
time_end   = 2019-09-13
abstract = "Statisticians and statistical programmers spend a great deal of time analyzing data and producing reports for clinical trials, both for final trial reports and for interim reports for data monitoring committees. Point and Click interfaces and copy-and-paste are now believed to be bad models for reproducible research. Instead, there are advantages to developing a high-level language for producing common elements of reports related to accrual, exclusions, descriptive statistics, adverse events, time to event, and longitudinal data.</p><p>It is well appreciated in the statistical and graphics design communities that graphics are much better than tables for conveying numeric information. There are thus advantages for having statistical reports for clinical trials that are almost completely graphical. Instead of devoting space to tables, <code>HTML5</code> and <code>Javascript</code> in R html reports makes it easy to show tabular information in pop-up text when hovering the mouse over a graphical element.</p><p>In this talk I will describe R packages <code>greport</code> (using a <code>LaTeX</code> pdf model) and <code>hreport</code> (using an html model). <code>knitr</code> and <code>Rmarkdown</code> are used to compose the reproducible reports. <code>greport</code> and <code>hreport</code> compose all figure and table captions. They contain high-level abstractions of common clinical trial reporting tasks to minimize programming by the use. Before showing examples of these report-making packages, I’ll show some of the new graphical building blocks in the <code>Hmisc</code> and <code>rms</code> packages. These new functions make use of the <code>plotly</code> package to create interactive graphics using <code>Javascript</code> and <code>D3</code>.</p>"
abstract_short = ""
event = "<ul><li>R Medicine 2019, Boston MA 2019-09-13</li><li>Trial Innovation Network Collaboration Webinar 2019-11-04</li></ul>"
event_url = "https://r-medicine.com"
location = ""

# Is this a selected talk? (true/false)
selected = true

# Projects (optional).
#   Associate this talk with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
# projects = [""]

# Links (optional).
url_pdf = ""
url_slides = "http://hbiostat.org/talks/rmedicine19.html"
url_video = "https://trialinnovationnetwork.org/wp-content/uploads/2019/11/A-New-Model-for-Statistical-Reports-for-RCT-Data-Monitoring-Committees.mp4"
url_code = "http://hbiostat.org/talks/rmedicine19.Rmd"

# Does the content use math formatting?
math = true

# Does the content use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "headers/bubbles-wide.jpg"
caption = "My caption :smile:"

+++
