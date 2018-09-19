+++
date = 2017-11-16  # Schedule page publish date.

title = "Using R, Rmarkdown, RStudio, knitr, plotly, and HTML for the Next Generation of Reproducible Statistical Reports"
time_start = 2017-11-16
time_end   = 2017-11-16
abstract = "The Vanderbilt Department of Biostatistics has two policies currently in effect:<br>1. All statistical reports will be reproducible<br>2. All reports should include all the code used to produce the report, in some fashion<br><br>We have succeeded with 1. (mainly using knitr in R) and to a large extent with 2.  Some biostatisticians have been concerned about interspersing code with the contents of the report.  It has also been challenging to copy some PDF report components (e.g., advanced tables) into word processing documents.<br><br>Fortunately R and RStudio have recently added a number of new features that allow for easy creation of HTML notebooks that are viewed with any web browser.  This solves the problems listed above and adds new possibilities such as interactive graphics that appear in a self-contained HTML file to post on a collaboration web server or send to a collaborator.  Interactive graphics allow the analyst to create more detail (e.g., confidence bands for multiple confidence levels; confidence bands for group differences as well as those for each group individually) with the collaborator able to easily select which details to view.<br><br>I have made major revisions in the R Hmisc and rms packages to provide new capabilities that fit into the R/RStudio Rmarkdown HTML notebook framework.  Interactive plotly graphics (based on Javascript and D3) and customized HTML output are the main new ingredients.  In this talk the rationale for this approach is discussed, and the new features are demonstrated with two statistical reports.  A few miscellaneous topics will also be covered, e.g. how to cite bibliographic references in Rmarkdown and how to interface R to citeulike.org for viewing or extracting bibliographic references.<br><br>For more information see<br><br>https://www.r-project.org<br>https://www.rstudio.com<br>http://rmarkdown.rstudio.com<br>http://rmarkdown.rstudio.com/r_notebooks.html<br>http://yihui.name/knitr<br>http://biostat.mc.vanderbilt.edu/Hmisc<br>https://plot.ly/r<br>https://plot.ly/r/getting-started<br>ggplotly: a function that converts any ggplot2 graphic to a plotly interactive graphic: https://plot.ly/ggplot2"
abstract_short = ""
event = "UNC Collaborative Studies Coordinating Center, Department of Biostatistics"
event_url = ""
location = "Chapel Hill, NC"

# Is this a selected talk? (true/false)
selected = true

# Projects (optional).
#   Associate this talk with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
# projects = [""]

# Links (optional).
url_pdf = ""
url_slides = "http://hbiostat.org/talk/nextgenReports.html"
url_video = ""
url_code = ""

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
