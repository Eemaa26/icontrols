---
title:  Interactive Controls and rCharts
framework: bootstrap
hitheme: twitter-bootstrap
highlighter: prettify
mode: standalone
---

<link href='http://fonts.googleapis.com/css?family=Lora|Lato' rel='stylesheet' type='text/css'>

<link rel="icon" 
      type="image/png" 
      href="http://rcharts.io/img/slidify_logo_notext.png">

<style>
.container {
  width: 1000px;
}
p {
  font-family: "Lora";
  font-size: 15px;
  line-height: 24px;
  text-align: justify;
}
h2 {
  font-family: "Lato"
}
</style>

## Interactive Controls and rCharts

<p id='author' class='text-muted'>by <a href='http://twitter.com/ramnath_vaidya'>Ramnath Vaidyanathan</a>, Dec 25, 2013</p>

<!-- AddThis Button BEGIN -->
<div class="addthis_toolbox addthis_default_style ">
<a class="addthis_button_facebook_like" fb:like:layout="button_count"></a>
<a class="addthis_button_tweet"></a>
<a class="addthis_button_pinterest_pinit"></a>
<a class="addthis_counter addthis_pill_style"></a>
</div>
<script type="text/javascript">var addthis_config = {"data_track_addressbar":true};</script>
<script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-4fdfcfd4773d48d3"></script>
<!-- AddThis Button END -->
<br/>

There was a recent blog post by [Sheri Gilley](https://twitter.com/Sheri_G) of [Revolution Analytics](http://www.revolutionanalytics.com/) on [Combining the Power of DeployR, rCharts, and AngularJS](http://blog.revolutionanalytics.com/2013/12/combining-the-power-of-deployr-rcharts-and-angularjs.html). It is a very cool application that showcases the power of [R](http://www.r-project.org/) and [Revolution R Enterprise](http://www.revolutionanalytics.com/enterprise-deployment), in being able to integrate frameworks like [rCharts](http://rcharts.io) and [AngularJS](http://angularjs.org). Although I haven't had the opportunity to play with [DeployR](http://www.revolutionanalytics.com/enterprise-deployment), I believe that along with [Shiny](http://www.rstudio.com/shiny/) and [OpenCPU](http://opencpu.org), it presents a bright future for building and deploying interactive R applications on the web.

Now, what some of you may not know is that integration of simple interactive controls using  [AngularJS](http://angularjs.org) and [DatGUI](http://davidwalsh.name/dat-gui) has been available in [rCharts](http://rcharts.io) for quite some time now. The nice thing about using a framework like AngularJS is that a lot of the interactivity can be handled on the client side with minimal amount of code, after the chart has been created from R.

Let us start by creating a simple scatterplot of mileage vs weight of cars from the ubiquitous `mtcars` dataset.






```r
library(rCharts)
n1 <- rPlot(mpg ~ wt, data = mtcars, color = "gear", type = "point")
n1
```

<iframe src='
chart1.html
' scrolling='no' seamless></iframe>
<style>iframe{ width: 100%; height: 400px;}</style>


Suppose, we want to let the user choose the `x`, `y` and `color` variables interactively. This can be done using the `addControls` method, which accepts three arguments: (1) the variable to control, (2) it's current value and (3) the possible set of values to choose from. By default, `addControls` uses AngularJS to add the controls.


```r
n1$addControls("x", value = "wt", values = names(mtcars))
n1$addControls("y", value = "wt", values = names(mtcars))
n1$addControls("color", value = "gear", values = names(mtcars))
n1
```

<iframe src='
chart2.html
' scrolling='no' seamless></iframe>
<style>iframe{ width: 100%; height: 400px;}</style>



Let us build another chart with interactive controls, this time using the [NVD3](http://nvd3.org) library for the chart and the [DatGUI](http://davidwalsh.name/dat-gui) javascript library for interactive controls. As before, the code stays almost the same, except that we swap the controls template to use `datgui` instead of `angularjs`.


```r
HairEye <- subset(as.data.frame(HairEyeColor), Sex == "Male")
n1 <- nPlot(Freq ~ Eye, data = HairEye, group = 'Hair', type = 'multiBarChart')
n1$addControls("type", "multiBarChart", 
  values = c('multiBarChart', 'multiBarHorizontalChart')
)
n1$setTemplate(script = system.file('libraries', 'nvd3', 'controls', 'datgui.html', package = 'rCharts'))
n1$set(width = 650)
n1
```

<iframe src='
chart3.html
' scrolling='no' seamless></iframe>
<style>iframe{ width: 100%; height: 400px;}</style>


Currently, rCharts only supports simple controls, and my plan is to extend this support. In my next post, I will discuss how rCharts can integrate with a server side framework like [Shiny](http://rstudio.com/shiny) or [OpenCPU](http://opencpu.org) to allow for greater level of interactivity, that might involve more extensive manipulation of data.

<a href="http://github.com/rcharts/icontrols"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>




