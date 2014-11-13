---
title: Combining thoughts and code
subtitle: Reproducible research with R
author: Shlomi Lifshits 
job: shlomi.lifshits@yahoo.com
framework: io2012
highlighter : highlight.js
widgets     : []
mode: selfcontained
---

<!--
      .....~IIIIIIIIII777$$7... ... ..  
      .~II???III?????????III77$$...     
    .I???$$I=~+?7$ZZOOOOZZZ$$7I7$$:. .  
  .??~$$?:~=7ODD=.. .... ....~OZ$7$Z..  
 .??Z$?,~~88?..+????????????+=...=77ZZ..
,?,ZI=,~ZD:....IZDDDDDDDDDDD  DO+7$...7$~.
??Z$:.~ON......IZ??+,=====+=??+?IO...78.
?=$I.,$D..... .IZ???$M???+7O+7???77. .77
??$?.:ZD..  ...IZ???$N......?$??+78 ..?$
IIZ7,:$8..  ...IZ???$N.....~:D?+~Z=...7.
~77$+,=$$......IZ???7++++=:~O?=:OM...=I.
.:777I:+$O  ...IZ??+???????:ZODM....?M. 
..,777I?=?$O...IZ???I8OO7?+I7$.. .?I~.  
 ...+Z$7I?+?777?Z???ZN...$?I$8?ZIZ$...  
.   ...$Z$77I??IZ???$N777I7+???7O... .  
..   .....=OOZZIZ??+$M$ZOOO$Z++?IO....  
          .  ..IZ???$N.....,I$++?7O...  
         . ....I7===$N.. ...77+===IO..  
            ...+IIIII7. ..  .=+?++???.  
                         GlassGiant.com
-->

<style>
strong {
  font-weight: bold;
}
</style>

## Outline
1. The reproducibility crisis.
2. The elements of reproducible research.
3. Markdown and RMarkdown
4. Generating reproducible documents with RStudio
5. Dependency management.


---

## The Reproducibility crisis

Over the past few years, there has been a growing awareness that many experimentally established "facts" don't seem to hold up to repeated investigation.

This was highlighted in a 2010 article in the [New Yorker](http://www.newyorker.com/magazine/2010/12/13/the-truth-wears-off) in which it is claimed that we are facing with a [reproducibility crisis](http://theconversation.com/science-is-in-a-reproducibility-crisis-how-do-we-resolve-it-16998).

# "Before the effectiveness of a drug can be confirmed, it must be tested and tested again. Different scientists in different labs need to repeat the protocols and publish their results. The test of replicability, as it's known, is the foundation of modern research. Replicability is how the community enforces itself. It's a safeguard for the creep of subjectivity."

<div align="right">
  Jonah Lehrer [New yorker]
</div>

<img src="./img/new_yorker.jpg" alt="Drawing" style="width: 200px;"/>


---

## The repliction v.s reproduction

We will focus on **Reproducible Computational Research**.

# "Reproducible Research: Make analytic data and code available so that others may reproduce findings."

<div align="right">
  Roger Peng
</div>

<img src="./img/coursera.png" alt="Drawing" style="width: 400px;"/>

---


## Ten Simple Rules for Reproducible Computational Research

1. For Every Result, Keep Track of How It Was Produced.
2. Avoid Manual Data Manipulation Steps.
3. Archive the Exact Versions of All External Programs Used.
4. Version Control All Custom Scripts.
5. Record All Intermediate Results, When Possible in Standardized Formats.
6. For Analyses That Include Randomness, Note Underlying Random Seeds.
7. Always Store Raw Data behind Plots.
8. Generate Hierarchical Analysis Output, Allowing Layers of Increasing Detail to Be Inspected.
9. Connect Textual Statements to Underlying Results.
10. Provide Public Access to Scripts, Runs, and Results.

From [PLOS](http://www.ploscompbiol.org/article/info%3Adoi%2F10.1371%2Fjournal.pcbi.1003285)

---

## Where to start

Start with this article by [FitzJohn et al.](http://ropensci.org/blog/2014/06/09/reproducibility/).

<img src="./img/open_sci.png" alt="Drawing" style="width: 200px;"/>


- Course on [Coursera](https://www.coursera.org/course/repdata).
- Video from [Roger Peng](http://youtu.be/aH8dpcirW1U).


---

## Book

A book about reproducible research

<img src="./img/Gandrud.jpg" alt="Drawing" style="width: 300px;"/>

---


## Reproducible research in academic research

You can download this paper along with the [data](http://datadryad.org/resource/doi:10.5061/dryad.g7jq6) (the compilation of the Latex document includes the execution of all the statistical analysis).

<img src="./img/dryad.png" alt="Drawing" style="width: 400px;"/>


---

## Reproducible research in academic research

- Apart from [Dryad](http://datadryad.org), you can store you reserach data in [figshare](http://figshare.com/about).
- [figshare](http://figshare.com/about) is a repository where users can make all of their research outputs available in a citable, shareable and discoverable manner.

<img src="./img/figshare.png" alt="Drawing" style="width: 400px;"/>

---

## Number of internet users (World Bank)

- Internet users (per 100 people). Internet users are "people with access to the worldwide network".
- The data is extracted from the World Bank using the library `WDI` and averaged over time (2005-2011). 


```r
library(dplyr)
library(magrittr)
library(WDI)
library(googleVis)
op <- options(gvis.plot.tag='chart')
```


---


## Number of internet users (World Bank)



```r
avg.internet.users<- WDI(indicator="IT.NET.USER.P2") %>% 
                    group_by(iso2c) %>% 
                    summarise(mean.num.users=round(mean(IT.NET.USER.P2),digit=1))

WDI(indicator="IT.NET.USER.P2") %>% filter(iso2c=="IL")
```

```
##   iso2c country IT.NET.USER.P2 year
## 1    IL  Israel          68.87 2011
## 2    IL  Israel          67.50 2010
## 3    IL  Israel          63.12 2009
## 4    IL  Israel          59.39 2008
## 5    IL  Israel          48.13 2007
## 6    IL  Israel          27.88 2006
## 7    IL  Israel          25.19 2005
```


---

## Number of internet users (World Bank)



```r
avg.internet.users %>% filter(iso2c=="IL")
```

```
## Source: local data frame [1 x 2]
## 
##   iso2c mean.num.users
## 1    IL           51.4
```


---

## Number of internet users (World Bank)



```r
FCMap <- gvisGeoMap(data = avg.internet.users,
                    locationvar = "iso2c",
                    numvar = "mean.num.users",
                    options = list(
                      colors = "[0xECE7F2, 0xA6BDDB, 0x2B8CBE]",
                      width = "780px",
                      height = "500px"))
```


---

## Number of internet users (World Bank)



```r
plot(FCMap)
```

<!-- GeoMap generated in R 3.1.2 by googleVis 0.5.2 package -->
<!-- Thu Nov 13 09:46:53 2014 -->


<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoMapID30e39a573c4 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
 "1A",
18.9 
],
[
 "1W",
23.5 
],
[
 "4E",
19.6 
],
[
 "7E",
24.3 
],
[
 "8S",
5.1 
],
[
 "AD",
66.9 
],
[
 "AE",
60.9 
],
[
 "AF",
2.8 
],
[
 "AG",
38.7 
],
[
 "AL",
27.1 
],
[
 "AM",
13.6 
],
[
 "AO",
5.9 
],
[
 "AR",
31.8 
],
[
 "AS",
null 
],
[
 "AT",
70.2 
],
[
 "AU",
71.4 
],
[
 "AW",
46.5 
],
[
 "AZ",
25 
],
[
 "B8",
49 
],
[
 "BA",
37 
],
[
 "BB",
61.7 
],
[
 "BD",
2.5 
],
[
 "BE",
67.5 
],
[
 "BF",
1.3 
],
[
 "BG",
37.1 
],
[
 "BH",
45.6 
],
[
 "BI",
0.8 
],
[
 "BJ",
2.3 
],
[
 "BM",
78.3 
],
[
 "BN",
46.8 
],
[
 "BO",
14.8 
],
[
 "BR",
34.2 
],
[
 "BS",
35.9 
],
[
 "BT",
8.9 
],
[
 "BW",
5.6 
],
[
 "BY",
null 
],
[
 "BZ",
12.3 
],
[
 "C4",
null 
],
[
 "C5",
null 
],
[
 "C6",
null 
],
[
 "C7",
null 
],
[
 "C8",
null 
],
[
 "C9",
null 
],
[
 "CA",
76.8 
],
[
 "CD",
0.5 
],
[
 "CF",
1.1 
],
[
 "CG",
3.7 
],
[
 "CH",
78.9 
],
[
 "CI",
1.8 
],
[
 "CL",
39.7 
],
[
 "CM",
3.3 
],
[
 "CN",
22.7 
],
[
 "CO",
25.8 
],
[
 "CR",
31.1 
],
[
 "CU",
13.1 
],
[
 "CV",
16.9 
],
[
 "CW",
null 
],
[
 "CY",
44.5 
],
[
 "CZ",
57.4 
],
[
 "DE",
76.6 
],
[
 "DJ",
3.4 
],
[
 "DK",
86.4 
],
[
 "DM",
42.9 
],
[
 "DO",
23.1 
],
[
 "DZ",
10.1 
],
[
 "EC",
18.3 
],
[
 "EE",
69.3 
],
[
 "EG",
22.5 
],
[
 "ER",
null 
],
[
 "ES",
58.4 
],
[
 "ET",
0.5 
],
[
 "EU",
62.8 
],
[
 "F1",
4.1 
],
[
 "FI",
82.4 
],
[
 "FJ",
15.3 
],
[
 "FM",
15.8 
],
[
 "FO",
74.3 
],
[
 "FR",
64.7 
],
[
 "GA",
6.3 
],
[
 "GB",
78 
],
[
 "GD",
24.1 
],
[
 "GE",
15.8 
],
[
 "GH",
5.7 
],
[
 "GL",
61.5 
],
[
 "GM",
7.1 
],
[
 "GN",
0.9 
],
[
 "GQ",
3.6 
],
[
 "GR",
38.4 
],
[
 "GT",
8.6 
],
[
 "GU",
48.5 
],
[
 "GW",
2.3 
],
[
 "GY",
null 
],
[
 "HK",
66.1 
],
[
 "HN",
10 
],
[
 "HR",
46 
],
[
 "HT",
7.6 
],
[
 "HU",
56.5 
],
[
 "ID",
7.3 
],
[
 "IE",
62.1 
],
[
 "IL",
51.4 
],
[
 "IM",
null 
],
[
 "IN",
5.2 
],
[
 "IQ",
1.8 
],
[
 "IR",
11.9 
],
[
 "IS",
91.3 
],
[
 "IT",
45 
],
[
 "JG",
null 
],
[
 "JM",
23.3 
],
[
 "JO",
22.6 
],
[
 "JP",
74.4 
],
[
 "KE",
11.3 
],
[
 "KG",
15.4 
],
[
 "KH",
1 
],
[
 "KI",
7.1 
],
[
 "KM",
3.4 
],
[
 "KN",
59.7 
],
[
 "KP",
0 
],
[
 "KR",
80.1 
],
[
 "KV",
null 
],
[
 "KW",
44.2 
],
[
 "KY",
56.5 
],
[
 "KZ",
17.4 
],
[
 "L4",
null 
],
[
 "L5",
null 
],
[
 "L6",
null 
],
[
 "L7",
null 
],
[
 "LA",
4.2 
],
[
 "LB",
27.5 
],
[
 "LC",
32.9 
],
[
 "LI",
71.8 
],
[
 "LK",
7.1 
],
[
 "LR",
null 
],
[
 "LS",
3.5 
],
[
 "LT",
53 
],
[
 "LU",
81.7 
],
[
 "LV",
61 
],
[
 "LY",
8.7 
],
[
 "MA",
32.7 
],
[
 "MC",
67.7 
],
[
 "MD",
25.1 
],
[
 "ME",
32.6 
],
[
 "MF",
null 
],
[
 "MG",
1.2 
],
[
 "MH",
5.3 
],
[
 "MK",
42.5 
],
[
 "ML",
1.3 
],
[
 "MM",
0.3 
],
[
 "MN",
null 
],
[
 "MO",
49.6 
],
[
 "MP",
null 
],
[
 "MR",
2.2 
],
[
 "MT",
52.6 
],
[
 "MU",
22.8 
],
[
 "MV",
20.4 
],
[
 "MW",
1.3 
],
[
 "MX",
24.8 
],
[
 "MY",
55 
],
[
 "MZ",
2.2 
],
[
 "NA",
7 
],
[
 "NC",
37.3 
],
[
 "NE",
0.6 
],
[
 "NG",
14.9 
],
[
 "NI",
6.1 
],
[
 "NL",
87.1 
],
[
 "NO",
88.7 
],
[
 "NP",
3.4 
],
[
 "NZ",
73.6 
],
[
 "OE",
62.6 
],
[
 "OM",
23.2 
],
[
 "PA",
29.5 
],
[
 "PE",
28 
],
[
 "PF",
36 
],
[
 "PG",
1.6 
],
[
 "PH",
12.3 
],
[
 "PK",
7.3 
],
[
 "PL",
52.6 
],
[
 "PR",
35.6 
],
[
 "PS",
27.2 
],
[
 "PT",
45.1 
],
[
 "PW",
null 
],
[
 "PY",
15 
],
[
 "QA",
46.6 
],
[
 "RO",
31.9 
],
[
 "RS",
34.8 
],
[
 "RU",
29.4 
],
[
 "RW",
null 
],
[
 "S1",
13.2 
],
[
 "S2",
9.9 
],
[
 "S3",
28.8 
],
[
 "S4",
8 
],
[
 "SA",
32.1 
],
[
 "SB",
3.2 
],
[
 "SC",
null 
],
[
 "SD",
null 
],
[
 "SE",
88.3 
],
[
 "SG",
67.1 
],
[
 "SI",
59.6 
],
[
 "SK",
65.6 
],
[
 "SL",
0.4 
],
[
 "SM",
null 
],
[
 "SN",
11 
],
[
 "SO",
null 
],
[
 "SR",
20.9 
],
[
 "SS",
null 
],
[
 "ST",
16.2 
],
[
 "SV",
10.4 
],
[
 "SX",
null 
],
[
 "SY",
14.2 
],
[
 "SZ",
8.1 
],
[
 "TC",
null 
],
[
 "TD",
1.2 
],
[
 "TG",
2.5 
],
[
 "TH",
19.5 
],
[
 "TJ",
7.8 
],
[
 "TL",
0.3 
],
[
 "TM",
2.2 
],
[
 "TN",
25.3 
],
[
 "TO",
11 
],
[
 "TR",
30.9 
],
[
 "TT",
39.2 
],
[
 "TV",
null 
],
[
 "TZ",
2.1 
],
[
 "UA",
13.7 
],
[
 "UG",
7.3 
],
[
 "US",
71.2 
],
[
 "UY",
37.5 
],
[
 "UZ",
13.4 
],
[
 "VC",
24.4 
],
[
 "VE",
26.4 
],
[
 "VI",
29.1 
],
[
 "VN",
23.8 
],
[
 "VU",
7.1 
],
[
 "WS",
5.9 
],
[
 "XC",
63.3 
],
[
 "XD",
63 
],
[
 "XE",
3.1 
],
[
 "XJ",
27 
],
[
 "XL",
2.7 
],
[
 "XM",
2.6 
],
[
 "XN",
8.7 
],
[
 "XO",
14.3 
],
[
 "XP",
16.2 
],
[
 "XQ",
16.1 
],
[
 "XR",
37.6 
],
[
 "XS",
69 
],
[
 "XT",
23.7 
],
[
 "XU",
71.7 
],
[
 "XY",
null 
],
[
 "YE",
7.3 
],
[
 "Z4",
25.5 
],
[
 "Z7",
47.2 
],
[
 "ZA",
14.2 
],
[
 "ZF",
6.4 
],
[
 "ZG",
6.4 
],
[
 "ZJ",
27.5 
],
[
 "ZM",
6.5 
],
[
 "ZQ",
19.3 
],
[
 "ZW",
11.2 
] 
];
data.addColumn('string','iso2c');
data.addColumn('number','mean.num.users');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoMapID30e39a573c4() {
var data = gvisDataGeoMapID30e39a573c4();
var options = {};
options["dataMode"] = "regions";
options["width"] = "780px";
options["height"] = "500px";
options["colors"] = [0xECE7F2, 0xA6BDDB, 0x2B8CBE];

    var chart = new google.visualization.GeoMap(
    document.getElementById('GeoMapID30e39a573c4')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geomap";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoMapID30e39a573c4);
})();
function displayChartGeoMapID30e39a573c4() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoMapID30e39a573c4"></script>
 
<!-- divChart -->
  
<div id="GeoMapID30e39a573c4"
  style="width: 780pxpx; height: 500pxpx;">
</div>


---

## Reproducible computing

This document was generated together with the statistical analysis and the graphics generation.

# Q: Is its reproducible?

<!-- 
A: 
  not next year when the database is changed and the average is done over more years.
  not shure anout the behavior of R and all the other libraries.
-->

- Store the source files and the textual analysis together in a version control system.
- Store the raw data (cvs) or as objects (serialization) in a version control system .
- Save the software environment details.

--- 
  
## Markdown 

Markdown enables writing in an **easy-to-read**, **easy-to-write** plain text format, and convert it to HTML.

# "The overriding design goal for Markdown's formatting syntax is to make it as **readable** as possible. The idea is that a Markdown-formatted document should be **publishable as-is, as plain text**, without looking like it's been marked up with tags formatting instructions."

<div align="right">
  John Gruber
</div>


--- 
  
## Markdown 

Learning [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet) is easy using [Dillinger](http://dillinger.io/). 

<img src="./img/dillinger.png" alt="Drawing" style="width: 700px;"/>

---

## knitr (Yihui Xie)

- [knitr](http://yihui.name/knitr/) implement the analysis in a **literate programming** style.
- Replaces `SWeave`.
- Document includes **Code chunks**.

<img src="./img/knit.png" alt="Drawing" style="width: 200px;"/>


---


## R Markdown (by RStudio)

# [R Markdown](http://rmarkdown.rstudio.com/) is a variant of Markdown that has embedded R **code chunks**, to be used with **knitr** to make it easy to create reproducible web-based reports.

With R Markdown you can:
- Compile HTML, PDF, or MS Word notebooks from R scripts.
- Create Beamer, ioslides and [Slidy](http://www.w3.org/Talks/Tools/Slidy2/#(1) presentations.
- Create interactive R Markdown documents using Shiny.
  - My Shiny [workshop](https://drive.google.com/viewerng/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzaGxvbWlsaWZzaGl0c3xneDoyNjIyNzllYTAzYmY2OWYx) and [code](https://github.com/shlomyli/shiny).
- RStudio has also a [presentation viewer](https://support.rstudio.com/hc/en-us/articles/200486468-Authoring-R-Presentations).

--- 

## R Markdown

- View this [video](https://www.youtube.com/watch?v=2yvW0O_7xOg#t=41).
- Another [link](https://support.rstudio.com/hc/en-us/articles/200552086-Using-R-Markdown)

<img src="./img/rmarkdown1.png" alt="Drawing" style="width: 400px;"/>

---

## Why use HTML and not Pdf

- It is tetxual.
- **Infographics** 
  - browsers supports SVG and enable interactions
  -[googleVis](http://cran.r-project.org/web/packages/googleVis/vignettes/Using_googleVis_with_knitr.html) 
  - The [d3](http://d3js.org/) javascript library. See nice [example](http://glimmer.rstudio.com/timelyportfolio/shiny-d3-showreel/).
- Browser supports math (MathJax).
- Hosting of web pages on Github.

---

## Tables

```
| val           | str   |
| ------------- | ----- |
| 1             | a     |
| 2             | b     |
```

| val           | str   |
| ------------- | ----- |
| 1             | a     |
| 2             | b     |


---

## Tables


```r
library(xtable)
df<-data.frame(x=c(1,2),y=c("a","b"))
tbl<-xtable(df)
print(tbl,type="html")
```

<!-- html table generated in R 3.1.2 by xtable 1.7-3 package -->
<!-- Thu Nov 13 09:46:53 2014 -->
<TABLE border=1>
<TR> <TH>  </TH> <TH> x </TH> <TH> y </TH>  </TR>
  <TR> <TD align="right"> 1 </TD> <TD align="right"> 1.00 </TD> <TD> a </TD> </TR>
  <TR> <TD align="right"> 2 </TD> <TD align="right"> 2.00 </TD> <TD> b </TD> </TR>
   </TABLE>

---


## RStudio Git integration

[here](http://nicercode.github.io/git/rstudio.html)

<img src="./img/git.png" alt="Drawing" style="width: 400px;"/>


---


## RStudio Packrat integration

[Packrat](http://rstudio.github.io/packrat) is a dependency management system for R.
Creates a private package library for a given R project (i.e. working directory).

See [here](http://www.r-bloggers.com/creating-reproducible-software-environments-with-packrat/) 
and [here](http://rstudio-pubs-static.s3.amazonaws.com/21739_5ef7fdf9080346db8a7bafefabca5788.html#2).

<img src="./img/packrat.png" alt="Drawing" style="width: 400px;"/>

When you start an R session in a packrat project directory, R will only look for packages in your private library; and anytime you install or remove a package, those changes will be made to your private library.

---


## The Packrat dependency management library

- **Isolated**: Installing a new or updated package for one project won't break your other projects, and vice versa. That's because packrat gives each project its own private package library.
- **Portable**: Easily transport your projects from one computer to another, even across different platforms. Packrat makes it easy to install the packages your project depends on.
- **Reproducible**: Packrat records the exact package versions you depend on, and ensures those exact versions are the ones that get installed wherever you go.

---

## Checkpoint (Revolution R)

Revolution stores daily snapshots of all CRAN packages. You can access these daily snapshots using the checkpoint package, which installs and consistently use these packages just as they existed at the snapshot date. 

<img src="./img/checkpoint.jpg" alt="Drawing" style="width: 700px;"/>

---

## Docker

- People have been successfull in using [Docker](http://melissagymrek.com/science/2014/08/29/docker-reproducible-research.html) and R.
- Docker has a **repository of images** called Docker hub that is pretty analagous to github except it's for Docker images instead of for code. You can pull, push, track, and modify images and share them with other people.

[paper]( http://arxiv.org/abs/1410.0846)

---

## RNG seed

- Random number generators generate pseudo-random numbers based on an initial seed.
- Setting the seed allows for the stream of random numbers to be exactly reproducible.
- In R you can use the `set.seed()` function to set the seed and to specify the random number generator to use.


```r
set.seed(42)
rnorm(5)
```

```
## [1]  1.3710 -0.5647  0.3631  0.6329  0.4043
```

---

## RNG seed


```r
rnorm(5)
```

```
## [1] -0.10612  1.51152 -0.09466  2.01842 -0.06271
```

```r
set.seed(42)
rnorm(5)
```

```
## [1]  1.3710 -0.5647  0.3631  0.6329  0.4043
```

---

## Software Environment

```r
sessionInfo()
```

```
## R version 3.1.2 (2014-10-31)
## Platform: x86_64-pc-linux-gnu (64-bit)
## 
## locale:
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] xtable_1.7-3    googleVis_0.5.2 WDI_2.4         RJSONIO_1.2-0.2
## [5] magrittr_1.0.1  dplyr_0.3.0.2   slidify_0.4.5  
## 
## loaded via a namespace (and not attached):
##  [1] assertthat_0.1  codetools_0.2-9 DBI_0.3.1       digest_0.6.4   
##  [5] evaluate_0.5.5  formatR_0.10    knitr_1.6       lazyeval_0.1.9 
##  [9] markdown_0.7    parallel_3.1.2  Rcpp_0.11.3     stringr_0.6.2  
## [13] tools_3.1.2     whisker_0.3-2   yaml_2.1.13
```

