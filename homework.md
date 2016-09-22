---
title: "Homework"
output: 
  html_document: 
    keep_md: yes
permalink: /homework/
layout: page
---



## Homework 2: Functions

### DUE: 10/7

The goal of this assignment is to get used to designing and writing functions. Writing functions involves thinking about how code should be divided up and what the interface/arguments should be. In addition, you need to think about what the function will return as output.

### Homework Submission

Please write up your homework using R markdown and knitr. Compile your document as an HTML file submit your HTML file to the dropbox on Courseplus. Please **show all your code** for each of the answers to the three parts.

### Part 1

Write a function that computes the factorial of an integer greater than or equal to 0. The function should have the following skeleton.


```r
Factorial <- function(n) {
        ## Your code goes here
}
```

The function should return an integer that is equal to n! = n * (n-1) * (n-2) * ... * 2 * 1. Note that 0! is defined to be 1. If the `Factorial` function is given an integer `n` that is less than 0, it should return the value `NaN`. 

* You can assume that the input value `n` will always be a *single* integer. 

* Do not worry about non-integer inputs.

* Do not use the functions `factorial()` or `gamma()`, which already exist in R.

Your function should produce output like the following:

```r
Factorial(4)
[1] 24
Factorial(0)
[1] 1
Factorial(-4)
[1] NaN
```

### Part 2

The data for this part come from a study of indoor air pollution and respiratory disease conducted here at Johns Hopkins. A high-resolution air pollution monitor was placed in each home to collect continuous levels of particulate matter over the period of a few days (each measurement represents a 5-minute average). In addition, measurements were taken in different rooms of the house as well as on multiple visits. 

Initially, we'd like to explore the data for each subject (indicated by the `id` column) and so we can make some exploratory time series plots of the data.

The following code plots the data from one subject (`id == 2`) on the baseline visit (`visit == 0`) and in the bedroom (`room == "bedroom"`). 


```r
library(readr)
library(dplyr)
library(ggplot2)
mie <- read_csv("data/MIE.csv", col_types = "iicdiDc")
```

```
Error: 'data/MIE.csv' does not exist in current working directory ('/Users/rdpeng/Biostat776/_homework').
```

```r
mie.sub <- filter(mie, id == 20 & visit == 0 
                  & room == "bedroom")
```

```
Error in filter_(.data, .dots = lazyeval::lazy_dots(...)): object 'mie' not found
```

```r
qplot(timepoint, value, data = mie.sub)
```

```
Error in ggplot(data, aesthetics, environment = env): object 'mie.sub' not found
```

While this code is useful, it only provides us information on one subject, on one visit, in one room. We could cut and paste this code to look at other subjects/visits/rooms, but that can be error prone and just plain messy.

The aim here is to **design** and **implement** a function that can be re-used to visualize all of the data in this dataset. 

1. There are 3 aspects that may vary in the dataset: The **id**, the **visit**, and the **room**. Note that not all combinations of id, visit, and room have measurements.

2. Your function should take as input **one** of the 3 possible factors. You need to decide which one of those it will be.

3. Given the input from the user, your function should present all of the data for that input. For example, if the input is `id = 2`, then you should plot the data for all rooms and all visits. If you the input is `visit = 0`, then you should plot data for all ids and all rooms, etc. It should be possible visualize the entire dataset with your function (through repeated calls to your function).

4. You may choose to write multiple functions to support your "main" function. That is okay.

4. If the user enters an input that does not exist in the dataset, your function should catch that and report an error (via the `stop()` function).

For your homework submission

1. Write a short description of how you chose to design your function and why. 

2. Present the code for your function in the R markdown document.

3. Include at least one example of output from your function. 


### Part 3

For this part you will write a function that computes a bootstrap 95% confidence interval for the median of a vector of numerical data. The median does not have a closed-form representation of the confidence interval like the mean does, so we typically have to resort to resampling techniques like the bootstrap to compute a confidence interval.

Your tasks for this part are

1. Write a specification for your function (in English, not code), describing how the function should work. In particular, it should specify and describe the **inputs** to the function and the **outputs**. If there are any requirements for the inputs (for example, there should not be missing values), then this should be mentioned in the specification. If your function returns errors for some inputs, this should also be described.

2. Implement your function in R.

3. Run your function on the [test vectors](../data/median_testdata.zip) provided for this part of the homework and present the output. Note that not everyone's function will give the same output for the test datasets, however the output that your function gives **should be described in the specification document**. If your function produces an output that is not specified, this is considered a bug.




---


## Homework 1: Tidy Data

### DUE: 9/16

The goal of the first assignment is to take datasets that are either messy or simply not tidy and to make them tidy datasets. The objective is to gain some familiarity with the functions in the `dplyr`, `tidyr`, and `readr` packages.

Before attempting this assignment, you should first install the `dplyr` and `tidyr` packages if you haven't already. This can be done with the `install.packages()` function.


```r
install.packages(c("dplyr", "tidyr", "readr"))
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

4. What is the difference in the average levels of "EC PM2.5 LC TOR" between California and Arizona

5. What are the median levels of "OC PM2.5 LC TOR" and "EC PM2.5 LC TOR" in the western and eastern U.S.? Define western as any monitoring location that has a `Longitude` less than -100.


### Part 3

Use the `readxl` package to read the file [aqs_sites.xlsx](../data/aqs_sites.xlsx) into R (you may need to install the package first). You may get some warnings when reading in the data but you can ignore these for now.

This file contains metadata about each of the monitoring sites in the EPA's monitoring system. In particular, the `Land Use` and `Location Setting` variables contain information about what kinds of areas the monitors are located in (i.e. "residential" vs. "forest").

Use the functions in the `dplyr` and `tidyr` packages to answer the following questions.

1. How many monitoring sites are labelled as both "RESIDENTIAL" for `Land Use` and "SUBURBAN" for `Location Setting`?

1. What are the median levels of "OC PM2.5 LC TOR" and "EC PM2.5 LC TOR" amongst monitoring sites that are labelled as both "RESIDENTIAL" and "SUBURBAN" in the eastern U.S., where eastern is defined as `Longitude` greater than or equal to -100?


