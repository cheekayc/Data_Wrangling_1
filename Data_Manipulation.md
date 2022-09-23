Data_Manipulation with `dplyr`
================
Lectured by Christine Mauro
2022-09-22

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.1     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

I’m setting how my R Markdown document would look like:

``` r
knitr::opts_chunk$set(
  collapse = TRUE,
  fig.width = 6,
  fig.asp = 0.6,
  out.width = "90%"
)
```

Once you’ve imported data, you’re going to need to do some *cleaning
up*.

Before we begin cleaning up the data, I would change my printing option
(shown in console) because we will be looking at a lot of datasets.

``` r
options(tibble.print_min = 3)
```

Import and clean the datasets.

``` r
litters_data = read_csv("Example_Data/FAS_litters.csv")
litters_data = janitor::clean_names(litters_data)

pups_data = read_csv("Example_Data/FAS_pups.csv")
pups_data = janitor::clean_names(pups_data)
```

I have checked my datasets in the console, and they look good.

## Select

`Select` is useful when we only want to select a certain **columns**
(variables) from the imported datatset.

Here are several ways to do it:

1.  Specify the columns you want to keep by naming all of them.

``` r
select(litters_data, group, litter_number, gd0_weight, pups_born_alive)
## # A tibble: 49 × 4
##   group litter_number gd0_weight pups_born_alive
##   <chr> <chr>              <dbl>           <dbl>
## 1 Con7  #85                 19.7               3
## 2 Con7  #1/2/95/2           27                 8
## 3 Con7  #5/5/3/83/3-3       26                 6
## # … with 46 more rows
```

2.  We can specify a **range** of variables we want to include. This is
    a nice little shorthand, so we don’t need to list every variable
    out.

``` r
select(litters_data, group:gd_of_birth)
## # A tibble: 49 × 5
##   group litter_number gd0_weight gd18_weight gd_of_birth
##   <chr> <chr>              <dbl>       <dbl>       <dbl>
## 1 Con7  #85                 19.7        34.7          20
## 2 Con7  #1/2/95/2           27          42            19
## 3 Con7  #5/5/3/83/3-3       26          41.4          19
## # … with 46 more rows
```

3.  We can also specify the columns we would like to **remove**.

``` r
select(litters_data, -pups_survive, -group)
## # A tibble: 49 × 6
##   litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_b…¹
##   <chr>              <dbl>       <dbl>       <dbl>           <dbl>         <dbl>
## 1 #85                 19.7        34.7          20               3             4
## 2 #1/2/95/2           27          42            19               8             0
## 3 #5/5/3/83/3-3       26          41.4          19               6             0
## # … with 46 more rows, and abbreviated variable name ¹​pups_dead_birth
```

4.  We can **rename** variables in the process of selecting variables.
    Left side is new name, right side is original variable name.

``` r
select(litters_data, GROUP = group, LiTtEr_NuMbEr = litter_number)
## # A tibble: 49 × 2
##   GROUP LiTtEr_NuMbEr
##   <chr> <chr>        
## 1 Con7  #85          
## 2 Con7  #1/2/95/2    
## 3 Con7  #5/5/3/83/3-3
## # … with 46 more rows
```

5.  There are some handy helper functions for `select`; read about all
    of them using `?select_helpers`. **`starts_with()`**,
    **`ends_with()`**, and `contains()` are oftenly used, especially
    when the variables are named with suffixes or other standard
    patterns.

``` r
select(litters_data, starts_with("gd"))
## # A tibble: 49 × 3
##   gd0_weight gd18_weight gd_of_birth
##        <dbl>       <dbl>       <dbl>
## 1       19.7        34.7          20
## 2       27          42            19
## 3       26          41.4          19
## # … with 46 more rows
```

``` r
select(litters_data, ends_with("weight"))
## # A tibble: 49 × 2
##   gd0_weight gd18_weight
##        <dbl>       <dbl>
## 1       19.7        34.7
## 2       27          42  
## 3       26          41.4
## # … with 46 more rows
```

6.  **`everything`** is handy for reorganizing columns without
    discarding anything. This way we pull the variables `litter_number`
    and `pups_survive` to the front, and every other variables follows
    after that. Kind of like rearranging the columns (variables) in the
    way we like it to demonstrate.

``` r
select(litters_data, litter_number, pups_survive, everything())
## # A tibble: 49 × 8
##   litter_number pups_survive group gd0_weight gd18_wei…¹ gd_of…² pups_…³ pups_…⁴
##   <chr>                <dbl> <chr>      <dbl>      <dbl>   <dbl>   <dbl>   <dbl>
## 1 #85                      3 Con7        19.7       34.7      20       3       4
## 2 #1/2/95/2                7 Con7        27         42        19       8       0
## 3 #5/5/3/83/3-3            5 Con7        26         41.4      19       6       0
## # … with 46 more rows, and abbreviated variable names ¹​gd18_weight,
## #   ²​gd_of_birth, ³​pups_born_alive, ⁴​pups_dead_birth
```

### Rename

If we just want to rename something, just use the `rename` instead of
`select`. This way we can rename some variables but also keep the rest
of the variables that we want.

``` r
rename(litters_data, GROUP = group, LITTER_number = litter_number)
## # A tibble: 49 × 8
##   GROUP LITTER_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
##   <chr> <chr>              <dbl>       <dbl>       <dbl>   <dbl>   <dbl>   <dbl>
## 1 Con7  #85                 19.7        34.7          20       3       4       3
## 2 Con7  #1/2/95/2           27          42            19       8       0       7
## 3 Con7  #5/5/3/83/3-3       26          41.4          19       6       0       5
## # … with 46 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth, ³​pups_survive
```

## Filter

`Filter` is for **selecting rows**. We filter rows based on logical
expressions using the `filter` function. Like `select`, the first
argument to `filter` is the data frame you are filtering; all subsequent
arguments are *logical expressions*.

Some common comparison operators: (`>`, `>=`, `<`, `<=`, `==`, and `!=`)

-   `=` is assigning values to an object.
-   `==` is “equal to”
-   `!=` is “not equal to”
-   `|` is “OR”
-   `!` is negate (like not…)
-   `%in%` detects if values appear in a set; useful for character
    variables.
-   `is.na()` to find missing values

The results of comparisons are logical – the statement is `TRUE` or
`FALSE` depending on the values you compare – and can be combined with
other comparisons using the logical operators `&` and `|` or negated
using `!`.

Example:

-   `gd_of_birth == 20` : Any rows with values not equal to 20 will be
    dropped.

-   `pups_born_alive >= 2` : Any rows with values less than 2 will be
    dropped.

-   `pups_survive != 4` : Rows with values not equal to 4 will be
    included. Rows equal to 4 will be dropped.

-   `!(pups_survive == 4)` : Will give the same results as above.

-   `!((pups_survive == 4) & (gd_of_birth == 20))` : Filter out the rows
    where `pups_survive` = 4 **and** `gd_of_birth` = 20, and keep the
    rest.

-   `group %in% c("Con7", "Con8")` : Select rows that has “Con7” or
    “Con8” on the `group` column.

-   `group == "Con7" & gd_of_birth == 20` : Select rows where `group` =
    “Con7” and `gd_of_birth` = 20.

Example code chunk:

``` r
filter(litters_data, gd_of_birth == 20)
## # A tibble: 32 × 8
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
##   <chr> <chr>              <dbl>       <dbl>       <dbl>   <dbl>   <dbl>   <dbl>
## 1 Con7  #85                 19.7        34.7          20       3       4       3
## 2 Con7  #4/2/95/3-3         NA          NA            20       6       0       6
## 3 Con7  #2/2/95/3-2         NA          NA            20       6       0       4
## # … with 29 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth, ³​pups_survive
```

``` r
filter(litters_data, group == "Con7" & gd_of_birth == 20)
## # A tibble: 4 × 8
##   group litter_number   gd0_weight gd18_weight gd_of_b…¹ pups_…² pups_…³ pups_…⁴
##   <chr> <chr>                <dbl>       <dbl>     <dbl>   <dbl>   <dbl>   <dbl>
## 1 Con7  #85                   19.7        34.7        20       3       4       3
## 2 Con7  #4/2/95/3-3           NA          NA          20       6       0       6
## 3 Con7  #2/2/95/3-2           NA          NA          20       6       0       4
## 4 Con7  #1/5/3/83/3-3/2       NA          NA          20       9       0       9
## # … with abbreviated variable names ¹​gd_of_birth, ²​pups_born_alive,
## #   ³​pups_dead_birth, ⁴​pups_survive
```

### Delete missing values

A very common filtering step requires you to omit missing observations.
You can do this with `filter`, but I recommend using `drop_na` from the
`tidyr` package.

-   `drop_na(dataset)` will remove any row with a missing value.

-   `drop_na(dataset, column)` will remove rows with missing values in
    that specified column.

## Mutate

**Create new** variables or **change** existing variables using
`mutate`.

The example below creates a new variable measuring the difference
between `gd18_weight` and `gd0_weight` and modifies the existing `group`
variable.

``` r
mutate(litters_data,
       wt_gain = gd18_weight - gd0_weight,
       group = str_to_lower(group), # `str_to_lower` makes all the alphabets in the `group` column to lowercase
       wt_gain_kg = wt_gain * 2.2)
## # A tibble: 49 × 10
##   group litter…¹ gd0_w…² gd18_…³ gd_of…⁴ pups_…⁵ pups_…⁶ pups_…⁷ wt_gain wt_ga…⁸
##   <chr> <chr>      <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
## 1 con7  #85         19.7    34.7      20       3       4       3    15      33  
## 2 con7  #1/2/95…    27      42        19       8       0       7    15      33  
## 3 con7  #5/5/3/…    26      41.4      19       6       0       5    15.4    33.9
## # … with 46 more rows, and abbreviated variable names ¹​litter_number,
## #   ²​gd0_weight, ³​gd18_weight, ⁴​gd_of_birth, ⁵​pups_born_alive,
## #   ⁶​pups_dead_birth, ⁷​pups_survive, ⁸​wt_gain_kg
```

A few things in the example above are worth noting:

-   Your new variables can be functions of old variables.
-   New variables appear at the end of the dataset in the order that
    they are created.
-   You can overwrite old variables.
-   You can create new variable and immediately refer to (or change) it.

## Arrange

`Arrange` is like `Proc sort` in SAS; sorting data.

In the example below, I will sort the `litter_data` dataset on `group`
first, and then `pups_born_alive` in ascending order.

``` r
head(arrange(litters_data, group, pups_born_alive), 10) 
## # A tibble: 10 × 8
##    group litter_number   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
##    <chr> <chr>                <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
##  1 Con7  #85                   19.7        34.7       20       3       4       3
##  2 Con7  #5/4/2/95/2           28.5        44.1       19       5       1       4
##  3 Con7  #5/5/3/83/3-3         26          41.4       19       6       0       5
##  4 Con7  #4/2/95/3-3           NA          NA         20       6       0       6
##  5 Con7  #2/2/95/3-2           NA          NA         20       6       0       4
##  6 Con7  #1/2/95/2             27          42         19       8       0       7
##  7 Con7  #1/5/3/83/3-3/2       NA          NA         20       9       0       9
##  8 Con8  #2/2/95/2             NA          NA         19       5       0       4
##  9 Con8  #1/6/2/2/95-2         NA          NA         20       7       0       6
## 10 Con8  #3/6/2/2/95-3         NA          NA         20       7       0       7
## # … with abbreviated variable names ¹​gd_of_birth, ²​pups_born_alive,
## #   ³​pups_dead_birth, ⁴​pups_survive
```

You can also sort in descending order.

``` r
arrange(litters_data, desc(group), desc(pups_born_alive))
## # A tibble: 49 × 8
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
##   <chr> <chr>              <dbl>       <dbl>       <dbl>   <dbl>   <dbl>   <dbl>
## 1 Mod8  #5/93               NA          41.1          20      11       0       9
## 2 Mod8  #2/95/2             28.5        44.5          20       9       0       9
## 3 Mod8  #97                 24.5        42.8          20       8       1       8
## # … with 46 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth, ³​pups_survive
```

## Piping `%>%`

We’ve seen several commands you will use regularly for data manipulation
and cleaning. You will rarely use them in isolation. For example,
suppose you want to load the data, clean the column names, remove
`pups_survive`, create `wt_gain`, and remove missing values in the
`wt_gain` column. There are a couple of options for this kind of
multi-step data manipulation.

1.  Step-by-step

``` r
litters_data_raw = read_csv("Example_Data/FAS_litters.csv",
                    col_types = "ccddiiii")

litters_data_clean_names = janitor::clean_names(litters_data_raw)

litters_data_selected_cols = select(litters_data_clean_names, -pups_survive)

litters_data_with_vars = mutate(litters_data_selected_cols,
                                wt_gain = gd18_weight - gd0_weight,
                                group = str_to_lower(group))

litters_data_with_vars_without_missing = drop_na(litters_data_with_vars, wt_gain)

litters_data_with_vars_without_missing
## # A tibble: 31 × 8
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² wt_gain
##   <chr> <chr>              <dbl>       <dbl>       <int>   <int>   <int>   <dbl>
## 1 con7  #85                 19.7        34.7          20       3       4    15  
## 2 con7  #1/2/95/2           27          42            19       8       0    15  
## 3 con7  #5/5/3/83/3-3       26          41.4          19       6       0    15.4
## # … with 28 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth
```

2.  Nesting

``` r
litters_data_clean = 
  drop_na(
    mutate(
      select(
        janitor::clean_names(
          read_csv("Example_Data/FAS_litters.csv", col_types = "ccddiiii")
        ),
      -pups_survive
      ),
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    ),
    wt_gain
)

litters_data_clean
## # A tibble: 31 × 8
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² wt_gain
##   <chr> <chr>              <dbl>       <dbl>       <int>   <int>   <int>   <dbl>
## 1 con7  #85                 19.7        34.7          20       3       4    15  
## 2 con7  #1/2/95/2           27          42            19       8       0    15  
## 3 con7  #5/5/3/83/3-3       26          41.4          19       6       0    15.4
## # … with 28 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth
```

The two examples above are both confusing and bad. The first one gets
confusing and clutters our workspace. The second one has to be read
inside out and you have to keep track of the `()`.

Piping solves this problem. It allows you to turn the nested approach
into a sequential chain by passing the results of one function call as
an argument to the next function call.

3.  **Piping**

``` r
litters_data =
  read_csv("./Example_Data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) %>%
  drop_na(wt_gain)

litters_data
## # A tibble: 31 × 8
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² wt_gain
##   <chr> <chr>              <dbl>       <dbl>       <int>   <int>   <int>   <dbl>
## 1 con7  #85                 19.7        34.7          20       3       4    15  
## 2 con7  #1/2/95/2           27          42            19       8       0    15  
## 3 con7  #5/5/3/83/3-3       26          41.4          19       6       0    15.4
## # … with 28 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth
```

All three approaches above result in the same dataset, but the piped
commands are the most straightforward.

**`%>%`** is read *“and then”*. The keyboard shortcut for `%>%` is
`Ctrl` + `Shift` + `M`.
