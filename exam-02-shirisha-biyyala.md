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
library(stringr)
library(dplyr)
library(tidyr)
library(ggplot2)
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

# Set the  directory to the exam-02 folder
setwd("C:/Users/shiri/OneDrive/Documents/RStudio/exam-02")

# Write the data to pac-all.csv
write_csv(pac_all, file = "pac-all.csv")
```

#### References:

1.  <https://www.geeksforgeeks.org/extract-first-or-last-n-characters-from-string-in-r/>

### Exercise 1

``` r
# Load pac-all.csv
pac_all <- read_csv("pac-all.csv")
```

    ## Rows: 2638 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (5): name, country_parent, total, dems, repubs
    ## dbl (1): year
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
# Data description
head(pac_all)
```

    ## # A tibble: 6 × 6
    ##   name                      country_parent              total dems  repubs  year
    ##   <chr>                     <chr>                       <chr> <chr> <chr>  <dbl>
    ## 1 7-Eleven                  Japan/Ito-Yokado            $8,5… $1,5… $7,000  2000
    ## 2 ABB Group                 Switzerland/Asea Brown Bov… $46,… $17,… $28,5…  2000
    ## 3 Accenture                 UK/Accenture plc            $75,… $23,… $52,9…  2000
    ## 4 ACE INA                   UK/ACE Group                $38,… $12,… $26,0…  2000
    ## 5 Acuson Corp (Siemens AG)  Germany/Siemens AG          $2,0… $2,0… $0      2000
    ## 6 Adtranz (DaimlerChrysler) Germany/DaimlerChrysler AG  $10,… $10,… $500    2000

``` r
glimpse(pac_all)
```

    ## Rows: 2,638
    ## Columns: 6
    ## $ name           <chr> "7-Eleven", "ABB Group", "Accenture", "ACE INA", "Acuso…
    ## $ country_parent <chr> "Japan/Ito-Yokado", "Switzerland/Asea Brown Boveri", "U…
    ## $ total          <chr> "$8,500", "$46,000", "$75,984", "$38,500", "$2,000", "$…
    ## $ dems           <chr> "$1,500", "$17,000", "$23,000", "$12,500", "$2,000", "$…
    ## $ repubs         <chr> "$7,000", "$28,500", "$52,984", "$26,000", "$0", "$500"…
    ## $ year           <dbl> 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2…

``` r
view(pac_all)

# Report number of observations and variables
n_obs_vars <- str_glue("The dataset has {nrow(pac_all)} observations and {ncol(pac_all)} variables.")
n_obs_vars
```

    ## The dataset has 2638 observations and 6 variables.

## Data Cleaning

### Exercise 2

``` r
# Use separate_wider_delim() to split the country_parent column
pac_all <- pac_all %>%
  separate_wider_delim(country_parent, 
                       delim = "/", 
                       names = c("country", "parent_company"), 
                       too_many = "merge")

# Print the top 10 rows to check the result
head(pac_all, 10)
```

    ## # A tibble: 10 × 7
    ##    name                          country parent_company total dems  repubs  year
    ##    <chr>                         <chr>   <chr>          <chr> <chr> <chr>  <dbl>
    ##  1 7-Eleven                      Japan   Ito-Yokado     $8,5… $1,5… $7,000  2000
    ##  2 ABB Group                     Switze… Asea Brown Bo… $46,… $17,… $28,5…  2000
    ##  3 Accenture                     UK      Accenture plc  $75,… $23,… $52,9…  2000
    ##  4 ACE INA                       UK      ACE Group      $38,… $12,… $26,0…  2000
    ##  5 Acuson Corp (Siemens AG)      Germany Siemens AG     $2,0… $2,0… $0      2000
    ##  6 Adtranz (DaimlerChrysler)     Germany DaimlerChrysl… $10,… $10,… $500    2000
    ##  7 AE Staley Manufacturing (Tat… UK      Tate & Lyle    $24,… $10,… $14,0…  2000
    ##  8 AEGON USA (AEGON NV)          Nether… Aegon NV       $58,… $10,… $47,7…  2000
    ##  9 AIM Management Group          UK      AMVESCAP       $25,… $10,… $15,0…  2000
    ## 10 Air Liquide America           France  L'Air Liquide… $0    $0    $0      2000

### Exercise 3

``` r
# Convert contribution amounts to numeric
pac_all <- pac_all %>%
  mutate(
    total = as.numeric(gsub("[^0-9.-]", "", total)),  # Remove non-numeric characters and convert to numeric
    dems = as.numeric(gsub("[^0-9.-]", "", dems)),    
    repubs = as.numeric(gsub("[^0-9.-]", "", repubs)) 
  )

# Print the top 10 rows to verify the changes
head(pac_all, 10)
```

    ## # A tibble: 10 × 7
    ##    name                          country parent_company total  dems repubs  year
    ##    <chr>                         <chr>   <chr>          <dbl> <dbl>  <dbl> <dbl>
    ##  1 7-Eleven                      Japan   Ito-Yokado      8500  1500   7000  2000
    ##  2 ABB Group                     Switze… Asea Brown Bo… 46000 17000  28500  2000
    ##  3 Accenture                     UK      Accenture plc  75984 23000  52984  2000
    ##  4 ACE INA                       UK      ACE Group      38500 12500  26000  2000
    ##  5 Acuson Corp (Siemens AG)      Germany Siemens AG      2000  2000      0  2000
    ##  6 Adtranz (DaimlerChrysler)     Germany DaimlerChrysl… 10500 10000    500  2000
    ##  7 AE Staley Manufacturing (Tat… UK      Tate & Lyle    24000 10000  14000  2000
    ##  8 AEGON USA (AEGON NV)          Nether… Aegon NV       58250 10500  47750  2000
    ##  9 AIM Management Group          UK      AMVESCAP       25000 10000  15000  2000
    ## 10 Air Liquide America           France  L'Air Liquide…     0     0      0  2000

#### References:

1.  <https://www.geeksforgeeks.org/remove-all-special-characters-from-string-in-r/>
2.  <https://stackoverflow.com/questions/10294284/remove-all-special-characters-from-a-string-in-r>

### Exercise 4

``` r
# Filter data for Canada and Mexico, group by year and country
can_vs_mexico <- pac_all %>%
  filter(country %in% c("Canada", "Mexico")) %>%
  group_by(year, country) %>%
  summarize(total_contributions = sum(total, na.rm = TRUE))
```

    ## `summarise()` has grouped output by 'year'. You can override using the
    ## `.groups` argument.

``` r
# Create the line plot
ggplot(can_vs_mexico, aes(x = year, y = total_contributions, color = country)) +
  geom_line(size = 1) +                   # Line plot with line width = 1
  labs(
    title = "Total Contributions from Foreign-connected PACs in Canada and Mexico",
    subtitle = "Comparison of Contributions in Canada and Mexico Over the Years",
    x = "Year",
    y = "Total Contributions ($)",
    color = "Country"
  ) +
  theme_minimal() +                       
  scale_color_manual(values = c("Canada" = "lightblue", "Mexico" = "rosybrown"))  
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](exam-02-shirisha-biyyala_files/figure-gfm/line-plot-1.png)<!-- -->

#### Interpretation:

### Exercise 5
