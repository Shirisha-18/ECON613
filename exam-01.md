MILESTONE EXAM 1 - STUDENT TEMPLATE
================
Shirisha Biyyala
Due: October 2, 2024

## Academic Honesty Statement

I, Shirisha Biyyala, hereby state that I have not communicated with or
gained information in any way from my classmates or anyone other than
the Professor or TA during this exam, and that all work is my own.

## Load packages

``` r
# load required packages here
library(tidyverse)
library(nycflights13)
library(ggplot2)
library(dplyr)
library(knitr)
library(htmltools)
```

## Questions

#### Understand the dataset

This section allows us to view the entire `flights` dataset from the
`nycflights13` package. It also provides access to documentation for
understanding the datasets’ variables and attributes.

``` r
#view(flights)
#?flights
```

In this section, we extract the first few rows of the `flights` dataset
to get a **glimpse of the data**.

``` r
# Get the head of the dataset
flights_head <- head(flights)

# Create an HTML table
html_table <- knitr::kable(flights_head, format = "html", escape = FALSE)

# Print the scrollable table
HTML(paste0(
  "<div style='overflow-x: auto; max-width: 100%; height: 300px;'>",
  html_table,
  "</div>"
))
```

<div style='overflow-x: auto; max-width: 100%; height: 300px;'><table>
 <thead>
  <tr>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> month </th>
   <th style="text-align:right;"> day </th>
   <th style="text-align:right;"> dep_time </th>
   <th style="text-align:right;"> sched_dep_time </th>
   <th style="text-align:right;"> dep_delay </th>
   <th style="text-align:right;"> arr_time </th>
   <th style="text-align:right;"> sched_arr_time </th>
   <th style="text-align:right;"> arr_delay </th>
   <th style="text-align:left;"> carrier </th>
   <th style="text-align:right;"> flight </th>
   <th style="text-align:left;"> tailnum </th>
   <th style="text-align:left;"> origin </th>
   <th style="text-align:left;"> dest </th>
   <th style="text-align:right;"> air_time </th>
   <th style="text-align:right;"> distance </th>
   <th style="text-align:right;"> hour </th>
   <th style="text-align:right;"> minute </th>
   <th style="text-align:left;"> time_hour </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 517 </td>
   <td style="text-align:right;"> 515 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 830 </td>
   <td style="text-align:right;"> 819 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> UA </td>
   <td style="text-align:right;"> 1545 </td>
   <td style="text-align:left;"> N14228 </td>
   <td style="text-align:left;"> EWR </td>
   <td style="text-align:left;"> IAH </td>
   <td style="text-align:right;"> 227 </td>
   <td style="text-align:right;"> 1400 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> 2013-01-01 05:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 533 </td>
   <td style="text-align:right;"> 529 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 850 </td>
   <td style="text-align:right;"> 830 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> UA </td>
   <td style="text-align:right;"> 1714 </td>
   <td style="text-align:left;"> N24211 </td>
   <td style="text-align:left;"> LGA </td>
   <td style="text-align:left;"> IAH </td>
   <td style="text-align:right;"> 227 </td>
   <td style="text-align:right;"> 1416 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> 2013-01-01 05:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 542 </td>
   <td style="text-align:right;"> 540 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 923 </td>
   <td style="text-align:right;"> 850 </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> AA </td>
   <td style="text-align:right;"> 1141 </td>
   <td style="text-align:left;"> N619AA </td>
   <td style="text-align:left;"> JFK </td>
   <td style="text-align:left;"> MIA </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1089 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:left;"> 2013-01-01 05:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 544 </td>
   <td style="text-align:right;"> 545 </td>
   <td style="text-align:right;"> -1 </td>
   <td style="text-align:right;"> 1004 </td>
   <td style="text-align:right;"> 1022 </td>
   <td style="text-align:right;"> -18 </td>
   <td style="text-align:left;"> B6 </td>
   <td style="text-align:right;"> 725 </td>
   <td style="text-align:left;"> N804JB </td>
   <td style="text-align:left;"> JFK </td>
   <td style="text-align:left;"> BQN </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:right;"> 1576 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:left;"> 2013-01-01 05:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 554 </td>
   <td style="text-align:right;"> 600 </td>
   <td style="text-align:right;"> -6 </td>
   <td style="text-align:right;"> 812 </td>
   <td style="text-align:right;"> 837 </td>
   <td style="text-align:right;"> -25 </td>
   <td style="text-align:left;"> DL </td>
   <td style="text-align:right;"> 461 </td>
   <td style="text-align:left;"> N668DN </td>
   <td style="text-align:left;"> LGA </td>
   <td style="text-align:left;"> ATL </td>
   <td style="text-align:right;"> 116 </td>
   <td style="text-align:right;"> 762 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> 2013-01-01 06:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 554 </td>
   <td style="text-align:right;"> 558 </td>
   <td style="text-align:right;"> -4 </td>
   <td style="text-align:right;"> 740 </td>
   <td style="text-align:right;"> 728 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> UA </td>
   <td style="text-align:right;"> 1696 </td>
   <td style="text-align:left;"> N39463 </td>
   <td style="text-align:left;"> EWR </td>
   <td style="text-align:left;"> ORD </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 719 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 58 </td>
   <td style="text-align:left;"> 2013-01-01 05:00:00 </td>
  </tr>
</tbody>
</table></div>

#### Exploratory Data Analysis

This section conducts an initial **exploratory analysis** by checking
the structure of the `flights` dataset using glimpse, which provides a
compact view of the data types and sample values. It counts the number
of missing values in each column to identify potential issues with data
completeness. Finally, **summary statistics** are generated for key
features like departure delay, arrival delay, distance, and airtime,
allowing for a better understanding of flight performance and trends.

``` r
# Check structure and missing values
glimpse(flights)
```

    ## Rows: 336,776
    ## Columns: 19
    ## $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2…
    ## $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    ## $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    ## $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, …
    ## $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 600, 600, …
    ## $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1…
    ## $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849,…
    ## $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 745, 851,…
    ## $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -1…
    ## $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "…
    ## $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 4…
    ## $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N394…
    ## $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA",…
    ## $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD",…
    ## $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 1…
    ## $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, …
    ## $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6…
    ## $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 0…
    ## $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 0…

``` r
# Count missing values in each column
sapply(flights, function(x) sum(is.na(x)))
```

    ##           year          month            day       dep_time sched_dep_time 
    ##              0              0              0           8255              0 
    ##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
    ##           8255           8713              0           9430              0 
    ##         flight        tailnum         origin           dest       air_time 
    ##              0           2512              0              0           9430 
    ##       distance           hour         minute      time_hour 
    ##              0              0              0              0

``` r
# Summary statistics for key features
summary(flights %>% select(dep_delay, arr_delay, distance, air_time))
```

    ##    dep_delay         arr_delay           distance       air_time    
    ##  Min.   : -43.00   Min.   : -86.000   Min.   :  17   Min.   : 20.0  
    ##  1st Qu.:  -5.00   1st Qu.: -17.000   1st Qu.: 502   1st Qu.: 82.0  
    ##  Median :  -2.00   Median :  -5.000   Median : 872   Median :129.0  
    ##  Mean   :  12.64   Mean   :   6.895   Mean   :1040   Mean   :150.7  
    ##  3rd Qu.:  11.00   3rd Qu.:  14.000   3rd Qu.:1389   3rd Qu.:192.0  
    ##  Max.   :1301.00   Max.   :1272.000   Max.   :4983   Max.   :695.0  
    ##  NA's   :8255      NA's   :9430                      NA's   :9430

### Question 1

To identify the **ten most common flight destinations** from NYC
airports in 2013, I grouped the flights dataset by the `dest` column and
counted the number of flights to each destination. After *sorting* the
results in descending order, I selected the top ten destinations.

``` r
flights %>% 
  group_by(dest) %>% 
  summarise(flight_count = n()) %>% 
  arrange(desc(flight_count)) %>% 
  top_n(10)   # Select the top 10 destinations
```

    ## Selecting by flight_count

    ## # A tibble: 10 × 2
    ##    dest  flight_count
    ##    <chr>        <int>
    ##  1 ORD          17283
    ##  2 ATL          17215
    ##  3 LAX          16174
    ##  4 BOS          15508
    ##  5 MCO          14082
    ##  6 CLT          14064
    ##  7 SFO          13331
    ##  8 FLL          12055
    ##  9 MIA          11728
    ## 10 DCA           9705

From the table, we can see that **ORD** had the highest number of
flights from NYC, followed closely by **ATL** and **LAX**. This
indicates that these cities were the most popular destinations for
travelers flying out of New York City in 2013.

### Question 2

To determine which airlines had the **most flights departing** from NYC
airports in 2013, I grouped the `flights` dataset by the `carrier`
column and calculated the total number of flights for each airline. I
then joined this data with the airlines dataset to retrieve the airline
names, sorted the results in descending order, and presented the flight
counts alongside their respective airline names.

``` r
flights %>% 
  group_by(carrier) %>% 
  summarise(flight_count = n()) %>% 
  arrange(desc(flight_count)) %>%            
  left_join(airlines, by = "carrier") %>% 
  rename(airline_name = name)
```

    ## # A tibble: 16 × 3
    ##    carrier flight_count airline_name               
    ##    <chr>          <int> <chr>                      
    ##  1 UA             58665 United Air Lines Inc.      
    ##  2 B6             54635 JetBlue Airways            
    ##  3 EV             54173 ExpressJet Airlines Inc.   
    ##  4 DL             48110 Delta Air Lines Inc.       
    ##  5 AA             32729 American Airlines Inc.     
    ##  6 MQ             26397 Envoy Air                  
    ##  7 US             20536 US Airways Inc.            
    ##  8 9E             18460 Endeavor Air Inc.          
    ##  9 WN             12275 Southwest Airlines Co.     
    ## 10 VX              5162 Virgin America             
    ## 11 FL              3260 AirTran Airways Corporation
    ## 12 AS               714 Alaska Airlines Inc.       
    ## 13 F9               685 Frontier Airlines Inc.     
    ## 14 YV               601 Mesa Airlines Inc.         
    ## 15 HA               342 Hawaiian Airlines Inc.     
    ## 16 OO                32 SkyWest Airlines Inc.

From the table, we can see that **United Air Lines Inc.** (58665) had
the highest number of flights from NYC, followed by **JetBlue
Airways**(54635) and **ExpressJet Airlines Inc.**(54173) This highlights
the dominance of these airlines in the NYC air travel market during
2013.

### Question 3

To analyze the **arrival delays of flights** with non-missing delay
information, I filtered the `flights` dataset to exclude any rows where
the `arr_delay` value is missing. I then grouped the data by the
`carrier` column, calculating the *mean arrival delay for each airline*.
After sorting the results, I identified the carrier with the **highest
and lowest mean arrival delays**, along with their corresponding carrier
codes.

``` r
flights %>% 
  filter(!is.na(arr_delay)) %>% 
  group_by(carrier) %>% 
  summarise(mean_arr_delay = round(mean(arr_delay), 4)) %>%
  left_join(airlines, by= "carrier") %>% 
  arrange(desc(mean_arr_delay))
```

    ## # A tibble: 16 × 3
    ##    carrier mean_arr_delay name                       
    ##    <chr>            <dbl> <chr>                      
    ##  1 F9              21.9   Frontier Airlines Inc.     
    ##  2 FL              20.1   AirTran Airways Corporation
    ##  3 EV              15.8   ExpressJet Airlines Inc.   
    ##  4 YV              15.6   Mesa Airlines Inc.         
    ##  5 OO              11.9   SkyWest Airlines Inc.      
    ##  6 MQ              10.8   Envoy Air                  
    ##  7 WN               9.65  Southwest Airlines Co.     
    ##  8 B6               9.46  JetBlue Airways            
    ##  9 9E               7.38  Endeavor Air Inc.          
    ## 10 UA               3.56  United Air Lines Inc.      
    ## 11 US               2.13  US Airways Inc.            
    ## 12 VX               1.76  Virgin America             
    ## 13 DL               1.64  Delta Air Lines Inc.       
    ## 14 AA               0.364 American Airlines Inc.     
    ## 15 HA              -6.92  Hawaiian Airlines Inc.     
    ## 16 AS              -9.93  Alaska Airlines Inc.

#### Highest mean arrival delay

The carrier with the highest mean arrival delay is **Frontier Airlines
Inc. (F9)**, with an average delay of **21.92 minutes**. This indicates
that, on average, flights operated by Frontier Airlines arrived nearly
**22 minutes later** than their scheduled time, suggesting potential
operational challenges affecting punctuality.

#### Lowest mean arrival delay

Conversely, **Alaska Airlines Inc. (AS)** had the lowest mean arrival
delay, averaging **-9.93 minutes**. This negative value indicates that,
on average, Alaska Airlines flights arrived about **10 minutes earlier**
than scheduled. This suggests that Alaska Airlines was the most punctual
among the carriers analyzed, performing well in terms of timely
arrivals.

### Question 4

\[Enter code and narrative here.\]

### Question 5

\[Enter code and narrative here.\]

### Question 6

\[Enter code and narrative here.\]

### Question 7

\[Enter code and narrative here.\]

### Question 8

\[Enter code and narrative here.\]

### Extra Credit

\[Enter code and narrative here.\]
