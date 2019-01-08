
<!-- README.md is generated from README.Rmd. Please edit that file -->
One R Package a Day
===================

Checking my assumption rOpenSci packages were picked relatively often by [the RLangPackage Twitter account](https://twitter.com/RLangPackage) (read [its creation story](https://stevenmortimer.com/one-r-package-a-day/)).

Get RLangPackage's timeline
---------------------------

Cf <https://rtweet.info/articles/auth.html>

``` r
timeline <- rtweet::get_timeline("RLangPackage", n = 3200)
```

Parse URLs
----------

I'll keep only tweets with a single URL, that needs to be a GitHub URL (a few times the account answered tweets of other accounts and included URLs)

``` r
urls <- timeline$urls_expanded_url[lengths(timeline$urls_expanded_url) == 1]
urls <- unlist(urls)
urls <- urls[grepl("github\\.com", urls)]
urls <- urltools::url_parse(urls)
urls <- dplyr::group_by(urls, path)
urls <- dplyr::mutate(urls, 
                      account = gsub("\\/.*", "", path), 
                      repo = gsub(".*\\/", "", path))
urls <- dplyr::ungroup(urls)
```

THE ANSWER
----------

Out of 159 featured packages, 8 (`sum(urls$account == "ropensci" | urls$account == "ropenscilabs")`) were rOpenSci packages. It's not a lot but...

``` r
library("magrittr")
dplyr::count(urls, account, sort = TRUE) %>%
  dplyr::filter(n >= 2) %>%
  knitr::kable()
```

| account      |    n|
|:-------------|----:|
| ropensci     |    7|
| hadley       |    4|
| mhahsler     |    4|
| rstudio      |    4|
| hrbrmstr     |    3|
| stan-dev     |    3|
| yihui        |    3|
| csgillespie  |    2|
| davidgohel   |    2|
| eddelbuettel |    2|
| edwindj      |    2|
| gforge       |    2|
| jeroen       |    2|
| renkun-ken   |    2|
| tpq          |    2|
| wahani       |    2|
| yanyachen    |    2|

The ropensci GitHub organization is the most represented one!
