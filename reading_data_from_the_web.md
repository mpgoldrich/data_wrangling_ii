Reading Data from the Web
================

Load the necessary libraries.

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

Import NSDUH data

``` r
nsduh_url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = 
  read_html(nsduh_url)
```

``` r
marj_use_df = 
  nsduh_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

Import star wars !

``` r
sw_url = "https://www.imdb.com/list/ls070150896/"

sw_html =
  read_html(sw_url)
```

``` r
sw_title_vec =
  sw_html |> 
  html_elements(".lister-item-header a") |> 
  html_text()

sw_gross_rev_vec =
  sw_html |> 
  html_elements(".text-small:nth-child(7) span:nth-child(5)") |> 
  html_text()

sw_df =
  tibble(
    title = sw_title_vec,
    gross_rev = sw_gross_rev_vec
  )
```

## APIs

Get water data from NYC.

``` r
nyc_water_df =
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") |> 
  content("parsed")
```

    ## Rows: 44 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

BRFSS Data

``` r
brfss_df =
  GET(
    "https://data.cdc.gov/resource/acme-vg9e.csv",
    query = list("$limit" = 5000)) |> 
  content()
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Try it now!

``` r
poke_df =
  GET("https://pokeapi.co/api/v2/pokemon/ditto") |> 
  content()
```
