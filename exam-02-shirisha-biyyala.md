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

## Exercises

``` r
# function: scrape_pac ---------------------------------------------------------

scrape_pac <- function(url) {

  # read the page
  page <- read_html(url)

  # extract the table
  # select node .DataTable-Partial (identified using the HTML code)
  pac <- page %>%
    html_node(".DataTable-Partial") %>%
    
    # parse table at node td into a data frame
    # table has a head and empty cells should be filled with NAs
    html_table("td", header = TRUE, trim = TRUE) %>%
    
    # convert to a tibble
    as_tibble()

  # rename variables
  pac <- pac %>%
    
    # rename columns (new name = old name)
    rename(
      name = `PAC Name (Affiliate)` ,
      country_parent = `Country of Origin/Parent Company`,
      total = `Total`,
      dems = `Dems`,
      repubs = `Repubs` 
    )

  # add year
  pac <- pac %>%
    
    # extract last 4 characters of the URL and save as year
    mutate(year = substr(url, nchar(url) - 3, nchar(url)))

  # return data frame
  pac

}

# test function ----------------------------------------------------------------

url_2022 <- "https://www.opensecrets.org/political-action-committees-pacs/foreign-connected-pacs/2022"
pac_2022 <- scrape_pac(url_2022)
```

    ## Warning: The `fill` argument of `html_table()` is deprecated as of rvest 1.0.0.
    ## ℹ An improved algorithm fills by default so it is no longer needed.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
url_2020 <- "https://www.opensecrets.org/political-action-committees-pacs/foreign-connected-pacs/2020"
pac_2020 <- scrape_pac(url_2020)

url_2018 <- "https://www.opensecrets.org/political-action-committees-pacs/foreign-connected-pacs/2018"
pac_2018 <- scrape_pac(url_2018)

# list of urls -----------------------------------------------------------------

# first part of url
root <- "https://www.opensecrets.org/political-action-committees-pacs/foreign-connected-pacs/"

# second part of url (election years as a sequence)
year <- seq(from = 2000, to = 2024, by = 2)

# construct urls by pasting first and second parts together
urls <- paste0(root, year)

# map the scrape_pac function over list of urls --------------------------------

pac_all <- map_dfr(urls, scrape_pac)

# write data -------------------------------------------------------------------

setwd(dirname(rstudioapi::getSourceEditorContext()$path))
write_csv(pac_all, file = "../data/pac-all.csv")
```

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
