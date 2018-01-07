+++
title = "Interactive Statistical Graphics: Showing More By Showing Less"
date = 2017-02-05T09:43:00Z
updated = 2017-02-26T06:48:43Z
tags = ["survival-analysis", "graphics", "r"]
+++
Version 4 of the R [`Hmisc`](http://biostat.mc.vanderbilt.edu/Hmisc) package and version
5 of the R [`rms`](http://biostat.mc.vanderbilt.edu/Rrms) package
interfaces with interactive [`plotly`](https://plot.ly/r/) graphics, which
is an interface to the `D3` javascript graphics library.  This allows
various results of statistical analyses to be viewed interactively, with
pre-programmed drill-down information.  More examples will be added
here.  We start with a video showing a new way to display survival
curves.

Note that plotly graphics are best used with RStudio Rmarkdown html
notebooks, and are distributed to reviewers as self-contained (but
somewhat large) html files. Printing is discouraged, but possible, using
snapshots of the interactive graphics.

Concerning the second bullet point below, boxplots have a high
ink:information ratio and hide bimodality and other data features.  Many
statisticians prefer to use dot plots and violin plots.  I liked those
methods for a while, then started to have trouble with the choice of a
smoothing bandwidth in violin plots, and found that dot plots do not
scale well to very large datasets, whereas spike histograms are useful
for all sample sizes.  Users of dot charts have to have a dot stand for
more than one observation if N is large, and I found the process too
arbitrary.  For spike histograms I typically use 100 or 200 bins.  When
the number of distinct data values is below the specified number of
bins, I just do a frequency tabulation for all distinct data values,
rounding only when two of the values are very close to each other.  A
spike histogram approximately reduces to a rug plot when there are no
ties in the data, and I very much like rug plots.

-   `rms survplotp` [video](https://youtu.be/EoIB_Obddrk): plotting
    survival curves
-   `Hmisc histboxp` [interactive html
    example](http://data.vanderbilt.edu/fh/R/Hmisc/examples.html#better_demonstration_of_boxplot_replacement):
    spike histograms plus selected quantiles, mean, and Gini's mean
    difference - replacement for boxplots - show all the data!  Note
    bimodal distributions and zero blood pressure values for patients
    having a cardiac arrest.

{{< figure src="/img/histboxp.png" width="100%" >}}
