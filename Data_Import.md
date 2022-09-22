Data Import
================

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

In this excercise we will be using relative path (not absolute path.
Absolute path = full path name).

-   Absolute: a file or folder’s “full address” on your computer

-   Relative: directions to a file or folder from your current working
    directory

# Import Data

## CSVs

To import a csv file, we’ll use a function from `readr` (this function
is included in the tidyverse package).

``` r
litters_data = read_csv("./Example_Data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_data = janitor::clean_names(litters_data)
```

Using `janitor::clean_names()` to clean up variable names after
importing data. Doing so will take whatever the column names are and
convert them to lower snake case.

The `package::function` syntax lets you use a function from a package
without loading the whole library. That’s really helpful, because some
packages have functions with the same name (e.g. `dplyr::filter` and
`stats::filter`), and R has to choose which one you mean. In general,
only load the packages you need to prevent this kind of confusion.

#### Looking at the data

The first thing to do after importing the data (unless `read_csv` gives
warnings) is to look at it. If there are unexpected results during data
import, you’ll catch a lot of them here. In addition to printing the
data, I often use `View`/`view` (use in console because they don’t work
well in rmd files), `str`, `head`, and `tail`:

``` r
litters_data
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
    ##    <chr> <chr>                <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 Con7  #85                   19.7        34.7       20       3       4       3
    ##  2 Con7  #1/2/95/2             27          42         19       8       0       7
    ##  3 Con7  #5/5/3/83/3-3         26          41.4       19       6       0       5
    ##  4 Con7  #5/4/2/95/2           28.5        44.1       19       5       1       4
    ##  5 Con7  #4/2/95/3-3           NA          NA         20       6       0       6
    ##  6 Con7  #2/2/95/3-2           NA          NA         20       6       0       4
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA         20       9       0       9
    ##  8 Con8  #3/83/3-3             NA          NA         20       9       1       8
    ##  9 Con8  #2/95/3               NA          NA         20       8       0       8
    ## 10 Con8  #3/5/2/2/95           28.5        NA         20       8       0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
    ## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive

``` r
head(litters_data, 5)
```

    ## # A tibble: 5 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 Con7  #85                 19.7        34.7          20       3       4       3
    ## 2 Con7  #1/2/95/2           27          42            19       8       0       7
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19       6       0       5
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19       5       1       4
    ## 5 Con7  #4/2/95/3-3         NA          NA            20       6       0       6
    ## # … with abbreviated variable names ¹​pups_born_alive, ²​pups_dead_birth,
    ## #   ³​pups_survive

``` r
tail(litters_data, 5)
```

    ## # A tibble: 5 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 Low8  #100                20          39.2          20       8       0       7
    ## 2 Low8  #4/84               21.8        35.2          20       4       0       4
    ## 3 Low8  #108                25.6        47.5          20       8       0       7
    ## 4 Low8  #99                 23.5        39            20       6       0       5
    ## 5 Low8  #110                25.5        42.7          20       7       0       6
    ## # … with abbreviated variable names ¹​pups_born_alive, ²​pups_dead_birth,
    ## #   ³​pups_survive

The code chunk below will show a neat summary of the dataset:

``` r
skimr::skim(litters_data)
```

|                                                  |              |
|:-------------------------------------------------|:-------------|
| Name                                             | litters_data |
| Number of rows                                   | 49           |
| Number of columns                                | 8            |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |              |
| Column type frequency:                           |              |
| character                                        | 2            |
| numeric                                          | 6            |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |              |
| Group variables                                  | None         |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| group         |         0 |             1 |   4 |   4 |     0 |        6 |          0 |
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |

**Variable type: numeric**

| skim_variable   | n_missing | complete_rate |  mean |   sd |   p0 |   p25 |   p50 |   p75 | p100 | hist  |
|:----------------|----------:|--------------:|------:|-----:|-----:|------:|------:|------:|-----:|:------|
| gd0_weight      |        15 |          0.69 | 24.38 | 3.28 | 17.0 | 22.30 | 24.10 | 26.67 | 33.4 | ▃▇▇▆▁ |
| gd18_weight     |        17 |          0.65 | 41.52 | 4.05 | 33.4 | 38.88 | 42.25 | 43.80 | 52.7 | ▃▃▇▂▁ |
| gd_of_birth     |         0 |          1.00 | 19.65 | 0.48 | 19.0 | 19.00 | 20.00 | 20.00 | 20.0 | ▅▁▁▁▇ |
| pups_born_alive |         0 |          1.00 |  7.35 | 1.76 |  3.0 |  6.00 |  8.00 |  8.00 | 11.0 | ▁▃▂▇▁ |
| pups_dead_birth |         0 |          1.00 |  0.33 | 0.75 |  0.0 |  0.00 |  0.00 |  0.00 |  4.0 | ▇▂▁▁▁ |
| pups_survive    |         0 |          1.00 |  6.41 | 2.05 |  1.0 |  5.00 |  7.00 |  8.00 |  9.0 | ▁▃▂▇▇ |

#### `read_csv` options …

In the best case, the data are stored in the csv file without any
weirdness – there are no blank lines or columns, the first row is the
variable name, missing values are stored in sensible ways. When this
isn’t the case, arguments to read_csv are helpful.

Examples:

-   `col_names`: Is the first row in the your dataset the variable
    names? By default, it is `TRUE`. If `FALSE`, column names are `X1`,
    `X1`, … . You can also supply column names.

-   `na`: Telling R what values you would want to treat as ‘missing
    values’. By default, R will treat empty rows and `NA` as missing
    values, but you may want to add other values (eg. 999, na, . ,
    etc.).

-   `skip`: number of rows to skip before reading data.

``` r
litters_data = read_csv("Example_Data/FAS_litters.csv", na = c("", "NA", 999, 88,) skip = 2)
```

###### `col_types`

`The read_*` functions will attempt to guess the data type stored in
each column. By default, these guesses are based on the first 1000 rows.
The guesses are also usually pretty good. In some cases, though, you’ll
want to give explicit column specifications (eg. maybe R guess the
variable type wrong, and you want to fix it). This is done using the
`cols` function, and each column is given a column type:

``` r
litters_data = read_csv(file = "./Example_data/FAS_litters.csv",
  col_types = cols(
    Group = col_character(),
    `Litter Number` = col_character(),
    `GD0 weight` = col_double(),
    `GD18 weight` = col_double(),
    `GD of Birth` = col_integer(),
    `Pups born alive` = col_integer(),
    `Pups dead @ birth` = col_integer(),
    `Pups survive` = col_integer()
  )
)
```

An easier way to do this:

``` r
litters_data = read_csv(file = "./Example_Data/FAS_litters.csv",
  col_types = "ccddiiii"
)
```

## Excel

`read_csv` is built in with RStudio, but `read_excel` is not. That’s why
we need to load the package `readxl` at the beginning. If we need more
information with `read_excel`, type `?read_excel` in the console.

``` r
mlb_df = read_excel("Example_Data/mlb11.xlsx")
```

Next step, check and see if your dataset looks good.

``` r
view(mlb_df)
```

##### `range` argument for Excel

``` r
lotr = read_excel(
  "Example_Data/LotR_Words.xlsx",
  range = "B3:D6"
)
```

## SAS

`haven`is used to import into R data files from SAS, Stata, and SPSS.
`haven` is not built in with `tidyverse`, so we need to load it at the
beginning.

``` r
pulse_df = read_sas("Example_data/public_pulse_data.sas7bdat")

head(pulse_df, 5)
```

    ## # A tibble: 5 × 7
    ##      ID   age Sex   BDIScore_BL BDIScore_01m BDIScore_06m BDIScore_12m
    ##   <dbl> <dbl> <chr>       <dbl>        <dbl>        <dbl>        <dbl>
    ## 1 10003  48.0 male            7            1            2            0
    ## 2 10015  72.5 male            6           NA           NA           NA
    ## 3 10022  58.5 male           14            3            8           NA
    ## 4 10026  72.7 male           20            6           18           16
    ## 5 10035  60.4 male            4            0            1            2

# Export Data

All the code chunks above only allow us to read different types of
datafile in R, but it will not be saved in a folder. Let’s say we edited
a datafile in a way that we want it to present. If we want that dataset
to be saved in a folder, we need to export it.

``` r
write_csv(lotr, file = "Results/lotr.csv")
```

The code chunk above indicates that I want to save a csv file(name of
the dataset you want to save, save it to a path)

## Why not Base R???

Check this out.

``` r
dont_do_this_df = read.csv("Example_Data/FAS_litters.csv")
```

`read.csv` is old version (Base R). It will import csv as data frame,
and it will show us the entire data frame (imagine when we have a huge
dataset..thousands of rows..) because R prints out the entire data
frame. Sometimes it will make the character vector a factor vector, and
we probably don’t want that.

`read_csv` imports csv as tibbles but not data frame.
