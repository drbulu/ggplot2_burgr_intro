---
title       : BURGr ggplot2 intro workshop
subtitle    : pretty graphics R way!
author      : drbulu
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Overview

Presentation aims:

1. Noobz: Get you up and running with ggplot2

2. Mid: Get you confortable with ggplot2

3. Pro: Get you wanting to explore more

--- .class #id 


## Noobz 1: Setup - Geoms wanted!
Aim of this section: creating pretty plots!  
Intro to ggplot2 with a simple dataset "women", which comes with the base (standard) R setup


```r
# load ggplot2, then load data: 2 for 1 line special! 
library(ggplot2); data(women) 
## 1) will produce blank plot... no geom specified!
myBlankPlot = ggplot(women, aes(height, weight)) # recommended
myBlankPlot2 = ggplot(women) # contains data only
myBlankPlot3 = ggplot() # "empty" object
# 2) will produce basic line graph via geom_line()
myPlot = ggplot(women, aes(height, weight)) + geom_line()
```

<b style="color:red">Note:</b> ggplot2 enables iterative building of complex graphics in <b style="color:blue;">layers</b> via <span style="color:red;">&#10133;</span> (plus) operator. See <b>?ggplot</b> for more detail.

--- .class #id

## Noobz 2: our first result!


```r
myPlot # or you can use: plot(myPlot) or print(myPlot)
```

<img src="assets/fig/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

--- .class #id

## Noobz 3: alternative plot setup

These variations will produce the same output as myPlot (so one is not pondering the "right" way to setup plots)

This can be very useful when constructing complex plots or working with functions (<b>control flow!</b>)


```r
# These variants will all work... try them out!
ggplot(women) + aes(height, weight) + geom_line()
ggplot(women) + geom_line(mapping = aes(height, weight))
ggplot() + geom_line(data = women, mapping = aes(height, weight))
ggplot() + geom_line(data = women) + aes(height, weight)

#odd.. but makes sense
ggplot() + aes(height, weight) + geom_line(data = women) 
```

<br/>
<b style="color:red">Takehome:</b> There is quite a bit of flexibility in ggplot2 oject creation. 

--- .class #id

## Noobz 4: Aesthetics Geoms?
<b>Aesthetics:</b> define visual or other attributes of ggplot objects
* Can be specified as an argument to either <code class="r">aes()</code> or to the <b>geom</b> to be modified.
* Examples aesthetics: colour, point type, point size, line size...

<b>Geoms:</b> aka "geometrics objects"
* define the type of the plot produced
* There are soo many geoms

<b style="color:red">Note:</b>  
* these may mean different things to different geoms  
* some geoms may not accept certain aesthetics   
* these are good concepts to understand via "dirty hands"!  
* Useful reference: http://docs.ggplot2.org/current/index.html  

--- .class #id

## Noobz 5: Behold... the combo plot!

```r
# Examples: one plot... 3 geoms... many aesthetics!
myCustomPlot = ggplot(women, aes(height, weight)) + 
    geom_bar(stat="identity", color="blue", fill="yellow", size=2) + 
    geom_line(color="red", size = 1.5) + geom_point(color = "green", size = 4)
myCustomPlot # note draw order: layers stacked from bottom to top!!
```

<img src="assets/fig/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />
<b style="color:red;">Note:</b> stat="identity" means no stat stansformations (plot values as is)

--- .class #id

## Noobz 6: Labels anyone?


```r
myCustomPlot = myCustomPlot + labs(x = "female height", y = "female weight", 
                                   title = "women: height vs. weight")
myCustomPlot # or you can use: plot(myCustomPlot) or print(myCustomPlot)
```

<img src="assets/fig/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

Reference: http://docs.ggplot2.org/current/labs.html  
<b style="color:red;">Note:</b> another example of ggplot2 <b>iterative</b> plot building.

--- .class #id

## Mid 1: More useful features but...

... this makes no sense?  
Welcome to "iris"! A slightly more complex dataset (also available in base R)


```r
data(iris) # seriously... this is almost there but not quite..
ggplot(iris, aes(Sepal.Length, Sepal.Width)) + geom_point(aes(color=Species)) + 
    geom_line(aes(color=Species))
```

<img src="assets/fig/r-1.png" title="plot of chunk r" alt="plot of chunk r" style="display: block; margin: auto;" />

--- .class #id

## Mid 2: This makes more sense

Facets anyone?


```r
# facet_grid() call's formula: separate all data (.) according to "Species" variable
ggplot(iris, aes(Sepal.Length, Sepal.Width)) + geom_point(aes(color=Species)) + 
    geom_line(aes(color=Species)) + facet_grid(. ~ Species)
```

<img src="assets/fig/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

Reference: http://docs.ggplot2.org/current/facet_grid.html

--- .class #id

## Mid 3: Another way to wrap it up?

Wrap gives nice control over facet rows and columns!


```r
# facet_wrap() call's formula: as above, but no dot (go figure)
ggplot(iris, aes(Sepal.Length, Sepal.Width)) + geom_point(aes(color=Species)) + 
    geom_line() + facet_wrap(~ Species, ncol=2)
```

<img src="assets/fig/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

Reference: http://docs.ggplot2.org/current/facet_wrap.html

--- .class #id

## Mid 4: The model citizen!

simple linear model application via <b style="color:red;">geom_smooth()</b>!


```r
myLmPlot = ggplot(iris, aes(Sepal.Length, Sepal.Width)) + geom_point(aes(color=Species)) + 
    facet_wrap(~ Species, ncol=2) + geom_smooth(method="lm", se = FALSE) 
myLmPlot
```

<img src="assets/fig/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

Reference: http://docs.ggplot2.org/current/geom_abline.html  
<b style="color:red;">Note:</b> see the docs for many ways to apply lines!

--- .class #id

## Mid 5: What's my, What's my THEME!

Themes... are basically the CSS of ggplot2 (absolute POWER!)


```r
simplePlotTheme = theme( # plot title formatting
                    plot.title = element_text(size = 12, colour = "red"),
                    # plot background: neatened up via element_blank()
                    panel.background = element_blank(),
                    # legend modifications
                    legend.position = "top",
                    legend.title = element_text(size = rel(1.5)),
                    legend.text = element_text(size = rel(1.2)))
# Note: sizes can be relative or absolute as contrasted above!
myLmPlot = myLmPlot + simplePlotTheme #Apply theme
```
Reference: http://docs.ggplot2.org/current/theme.html  
<b style="color:red;">More</b> on themes: https://www.r-bloggers.com/ggplot2-themes-examples/

--- .class #id

## Mid 6: That's my THEME!

<img src="assets/fig/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

--- .class #id

## Pro 1: Working with lists
For finer plot control and greater flexibility than <b>facet</b>.


```r
# create base plot: note the placement of color aesthetic!
myIrisPlot = ggplot(iris, aes(Sepal.Length, Sepal.Width, color=Species)) + geom_point()

# create list of plots (plotList)
irisPlotList = list()
irisPlotList[["points"]] = myIrisPlot
irisPlotList[["line"]] = myIrisPlot + geom_line()
irisPlotList[["lm"]] = myIrisPlot + geom_smooth(method = "lm", se=FALSE)
# content check
names(irisPlotList)
```

```
## [1] "points" "line"   "lm"
```

--- .class #id

## Pro 2: Moo - Cowplot

The lazy man's way to pretty plot list organisation


```r
library(cowplot) # note: masks some ggplot2 functions when loaded
plot_grid(plotlist = irisPlotList, labels = c("A", "B", "C"), nrow=1)
```

<img src="assets/fig/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" style="display: block; margin: auto;" />
For more see:  
https://cran.r-project.org/web/packages/cowplot/vignettes/introduction.html

--- .class #id


## Pro 3: functional plots 1

This is a reasonably simple function to aid list handling.  
Note: you can make hideously complicated funcions... it can be worth it, but "caveat emptor"!  
gridExtra is a package worth exploring for the adventurous:  
https://cran.r-project.org/web/packages/gridExtra/index.html  


```r
applyThemesToPlots = function(plotList, themeList){
    errorMsg = "The list of plots and the list of themes MUST be the same length!"
    if (length(plotList) != length(themeList)) stop(errorMsg)
    for(i in 1:length(plotList)) plotList[[i]] = plotList[[i]] + themeList[[i]]
    return(plotList)
}
# incidentally: you can use lapply() to quickly apply ONE theme to MANY plots
# you can even wrap this in a basic function "shell" for convenience.
sameTheme = lapply(irisPlotList, FUN = function(x, t) return(x + t), 
                   t = simplePlotTheme)
```

--- .class #id

## Pro 4: functional plots 2
The results!


```r
miscThemeList = list(theme_minimal(), theme_dark(), simplePlotTheme)
themedIrisPlotList = applyThemesToPlots(plotList = irisPlotList, themeList = miscThemeList)
plot_grid(plotlist = themedIrisPlotList, labels = c("A", "B", "C"), nrow=1)
```

<img src="assets/fig/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" style="display: block; margin: auto;" />

--- .class #id

## Pro 5: library vs require?

Why use require() over library()?


```r
isLoadedByLibrary = library(ggplot2); isLoadedByLibrary
```

```
##  [1] "gridExtra" "cowplot"   "ggplot2"   "stats"     "graphics" 
##  [6] "grDevices" "utils"     "datasets"  "methods"   "base"
```

```r
isLoadedByRequire = require(ggplot2); isLoadedByRequire
```

```
## [1] TRUE
```

<b style="color:red;"> Takehome</b>: require can be used in control flow (e.g. dependency checking) in functions and other situations

--- .class #id

## Conclusion

Remember: ggplot plots are objects (lists) that contain the data required to plot them... therefore include only the data that you need!

```r
names(myIrisPlot)
```

```
## [1] "data"        "layers"      "scales"      "mapping"     "theme"      
## [6] "coordinates" "facet"       "plot_env"    "labels"
```

```r
head(myIrisPlot$data, 2)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
```

<b style="color:red;">Note:</b> This also means that you can extract information from plots for "downstream applications".

