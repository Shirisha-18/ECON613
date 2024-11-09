Midterm 02
================
Shirisha Biyyala
2024-11-08

## Load packages and data

``` r
library(tidyverse)
library(robotstxt)
library(rvest)
library(scales)
```

``` r
library(robotstxt)
paths_allowed("https://www.opensecrets.org")
```

    ##  www.opensecrets.org

    ## [1] TRUE

``` r
url <- "https://www.gov.scot/publications/coronavirus-covid-19-update-first-ministers-speech-26-october/"
speech_page <- read_html(url)
speech_page
```

    ## {html_document}
    ## <html dir="ltr" lang="en">
    ## [1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF-8 ...
    ## [2] <body>\n    <input type="hidden" id="site-root-path" value="/"><!-- Googl ...

``` r
title <- speech_page %>%
    html_node(".ds_page-header__title") %>%
    html_text()

speech_page %>%
    html_node("#sg-meta__publication-date") %>%
    html_text()
```

    ## [1] "26 October 2020"

``` r
date <- speech_page %>%
    html_node("#sg-meta__publication-date") %>%
    html_text() %>%
    dmy()

location <- speech_page %>%
    html_node(".ds_metadata__item:nth-child(5) strong") %>%
    html_text()

abstract <- speech_page %>%
    html_node(".ds_no-margin--bottom") %>%
    html_text()

text <- speech_page %>%
    html_node(".publication-body") %>%
    html_text()

oct_26_speech <- tibble(
  title    = title,
  date     = date,
  location = location,
  abstract = abstract,
  text     = text,
  url      = url
)
oct_26_speech
```

    ## # A tibble: 1 × 6
    ##   title                                 date       location abstract text  url  
    ##   <chr>                                 <date>     <chr>    <chr>    <chr> <chr>
    ## 1 Coronavirus (COVID-19) update: First… 2020-10-26 St Andr… Stateme… "\n … http…

## Exercises

### Exercise 1

Remove this text, and add your answer for Exercise 1 here. Add code
chunks as needed. Don’t forget to label your code chunk. Do not use
spaces in code chunk labels.

### Exercise 2

Remove this text, and add your answer for Exercise 2 here. Add code
chunks as needed. Don’t forget to label your code chunk. Do not use
spaces in code chunk labels.

### Exercise 3

Remove this text, and add your answer for Exercise 3 here. Add code
chunks as needed. Don’t forget to label your code chunk. Do not use
spaces in code chunk labels.

### Exercise 4

…

### Exercise 5

…
