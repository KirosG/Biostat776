---
title: "Homework"
output: 
  html_document: 
    keep_md: yes
permalink: /homework/
layout: page
---



## Homework 1: Tidy Data

### DUE: 9/16

The goal of the first assignment is to take datasets that are either messy or simply not tidy and to make them tidy datasets. The objective is to gain some familiarity with the functions in the `dplyr`, `tidyr`, `readxl`, and `readr` packages.

Before attempting this assignment, you should first install the packages if you haven't already. This can be done with the `install.packages()` function.


```r
install.packages(c("dplyr", "tidyr", "readr", "readxl"))
```

Running this function will also install a host of other packages on which these two depend so it make take a minute or two.

### Homework Submission

Please write up your homework using R markdown and knitr. Compile your document as an HTML file submit your HTML file to the dropbox on Courseplus. Please **show all your code** for each of the answers to the three parts.

### Reading

For this homework, the following readings may be helpful:

* [Chapter 7 of R Programming for Data Science](https://bookdown.org/rdpeng/rprogdatascience/using-the-readr-package.html)

* [Chapter 13 of R Programming for Data Science](https://bookdown.org/rdpeng/rprogdatascience/managing-data-frames-with-the-dplyr-package.html)

* [Sections 1.2 through 1.5 of Mastering Software Development in R](http://rdpeng.github.io/RProgDA/the-importance-of-tidy-data.html)


### Part 1

Load the `WorldPhones` dataset in the `datasets` package with


```r
library(datasets)
data(WorldPhones)
WorldPhones
```

```
     N.Amer Europe Asia S.Amer Oceania Africa Mid.Amer
1951  45939  21574 2876   1815    1646     89      555
1956  60423  29990 4708   2568    2366   1411      733
1957  64721  32510 5230   2695    2526   1546      773
1958  68484  35218 6662   2845    2691   1663      836
1959  71799  37598 6856   3000    2868   1769      911
1960  76036  40341 8220   3145    3054   1905     1008
1961  79831  43173 9053   3338    3224   2005     1076
```

This dataset gives the number of telephones in various regions of the world (in thousands). The regions are: North America, Europe, Asia, South America, Oceania, Africa, Central America and data are available for the years 1951, 1956, 1957, 1958, 1959, 1960, 1961.

Use the functions in `dplyr` and `tidyr` to produce a data frame that looks like this.

```
   year  country number
1  1951   N.Amer  45939
2  1956   N.Amer  60423
3  1957   N.Amer  64721
4  1958   N.Amer  68484
5  1959   N.Amer  71799
6  1960   N.Amer  76036
7  1961   N.Amer  79831
8  1951   Europe  21574
9  1956   Europe  29990
10 1957   Europe  32510
11 1958   Europe  35218
12 1959   Europe  37598
13 1960   Europe  40341
14 1961   Europe  43173
15 1951     Asia   2876
16 1956     Asia   4708
17 1957     Asia   5230
18 1958     Asia   6662
19 1959     Asia   6856
20 1960     Asia   8220
21 1961     Asia   9053
22 1951   S.Amer   1815
23 1956   S.Amer   2568
24 1957   S.Amer   2695
25 1958   S.Amer   2845
26 1959   S.Amer   3000
27 1960   S.Amer   3145
28 1961   S.Amer   3338
29 1951  Oceania   1646
30 1956  Oceania   2366
31 1957  Oceania   2526
32 1958  Oceania   2691
33 1959  Oceania   2868
34 1960  Oceania   3054
35 1961  Oceania   3224
36 1951   Africa     89
37 1956   Africa   1411
38 1957   Africa   1546
39 1958   Africa   1663
40 1959   Africa   1769
41 1960   Africa   1905
42 1961   Africa   2005
43 1951 Mid.Amer    555
44 1956 Mid.Amer    733
45 1957 Mid.Amer    773
46 1958 Mid.Amer    836
47 1959 Mid.Amer    911
48 1960 Mid.Amer   1008
49 1961 Mid.Amer   1076
```

You may need to use functions outside these packages to obtain this result.

### Part 2

Use the `readr` package to read the [SPEC_2014.csv.bz2](../data/SPEC_2014.csv.bz2) data file in to R. This file contains daily levels of fine particulate matter (PM2.5) chemical constituents across the United States. The data are measured at a network of federal, state, and local monitors and assembled by the EPA. 

In this dataset, the `Sample.Value` column provides the level of the indicated chemical constituent and the `Parameter.Name` column provides the name of the chemical constituent. The combination of a `State.Code`, a `County.Code`, and a `Site.Num`, uniquely identifies a monitoring site (the location of which is provided by the `Latitude` and `Longitude` columns).

For all of the questions below, you can ignore the missing values in the dataset, so when taking averages, just remove the missing values before taking the average.

Use the functions in the `dplyr` package to answer the following questions:

1. What is average value of "Bromine PM2.5 LC" in the state of Wisconsin in this dataset?

2. Calculate the average of each chemical constituent across all states/monitors and all time points. Which constituent has the highest average level?

3. Which monitoring site has the highest levels of "Sulfate PM2.5 LC"? Indicate the state code, county code, and site number.

4. What is the difference in the average levels of "EC PM2.5 LC TOR" between California and Arizona?

5. What are the median levels of "OC PM2.5 LC TOR" and "EC PM2.5 LC TOR" in the western and eastern U.S.? Define western as any monitoring location that has a `Longitude` less than -100.


### Part 3

Use the `readxl` package to read the file [aqs_sites.xlsx](../data/aqs_sites.xlsx) into R (you may need to install the package first). This file contains metadata about each of the monitoring sites in the EPA's monitoring system. In particular, the `Land Use` and `Location Setting` variables contain information about what kinds of areas the monitors are located in (i.e. "residential" vs. "forest").

Use the functions in the `dplyr` and `tidyr` packages to answer the following questions.

1. How many monitoring sites are labelled as both "RESIDENTIAL" for `Land Use` and "SUBURBAN" for `Location Setting`?

1. What are the median levels of "OC PM2.5 LC TOR" and "EC PM2.5 LC TOR" amongst monitoring sites that are labelled as both "RESIDENTIAL" and "SUBURBAN" in the eastern U.S., where eastern is defined as `Longitude` greater than or equal to -100?


