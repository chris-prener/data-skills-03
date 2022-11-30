Data Skills 03 - Introduction to ggplot2 for Plotting
================
Christopher Prener, Ph.D.
(October 24, 2022)

## Dependencies

This notebook requires two packages from the `tidyverse` as well as two
additional packages:

``` r
# tidyverse packages
library(dplyr)     # data wrangling
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)   # read and write csv files

# manage file paths
library(here)      # manage file paths
```

    ## here() starts at /Users/prenec/Documents/GitHub/data_skills/data-skills-03

## Load Data

For today’s session, we’ll use the `penguins` data in the
`palmerpenguins` package.

``` r
penguins <- palmerpenguins::penguins
```

Notice how we can load a data set or function without including it in
our code chunk above where packages are loaded. Instead we use the
`package::object` or `package::function` syntax. This can be useful when
multiple packages have functions with the same name, when writing our
own functions, and when we need only a single instance of a function or
data object in an analysis.

## Introduction to Grammar of Graphics

The first step in thinking creatively about data visualization is to
appreciate that graphics are built upon an underlying grammar. There are
two key things to note about the grammar of graphics:

- Meaningful plots are built around matching the right data with the
  right method of visualization.
- Graphics are made up of distinct layers of grammatical elements.

The data we have are matched to the selected method of visualization
using **aesthetic mappings**. After we have defined our data source and
the appropriate mappings, we visualize the data with some combination of
**geometric objects** (or **geoms**) or statistical transformations.
**Scales** for each aesthetic mapping are used to define how they appear
(both in terms of numeric values and color), and plots can be customized
using a combination of **labels** and **themes**.

## Introduction to `ggplot2`

Hadley Wickham’s `ggplot2` package implements this grammar of graphics
in `R`. We’ll use the `penguins` data we loaded above today, but it is
important to note that most (if not all) actual data visualization
projects require some level of data cleaning ahead of time to make sure
the data are measured correctly and are in the right format (long
vs. wide).

Here is a very basic plot that we will iterate on below. Note that the
primary function here is `ggplot()` and that functions are chained
together with the `+` sign instead of `%>%`. Both of these are common
stumbling blocks when folks are starting out with `ggplot2`.

In our `ggplot()` function, we define the default data source
(`penguins`) and our **aesthetic mappings**. Once those have been
defined, we add `geom_point()` to make a scatter plot based on these
aesthetic mappings.

``` r
ggplot(data = penguins, mapping = aes(x = bill_length_mm, y = bill_depth_mm)) +
  geom_point()
```

    ## Warning: Removed 2 rows containing missing values (geom_point).

![](data-skills-03-complete_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

We can remove those missing values before we continue. They’re
automatically removed by `ggplot2`, but doing this ourselves removes the
warning that pops up:

``` r
penguins <- filter(penguins, is.na(bill_length_mm) == FALSE)
```

## Creating More Complex Plots

Instead of re-typing our code over and over again today, we’ll use the
following code to iterate on our plot. We’ll work to add the following:

1.  color the points by species
2.  change the colors utilized
3.  add best fit lines
4.  adjust the `x` and `y` axis scales
5.  adjust the `x` and `y` axis labels
6.  add a title, subtitle, and captions

``` r
p <- ggplot(data = penguins, mapping = aes(x = bill_length_mm, y = bill_depth_mm,
                                      color = species)) +
  geom_point() +
  geom_smooth(method = "lm", formula = y ~ x) +
  scale_x_continuous(limits = c(30, 60), breaks = seq(30, 60, by = 5)) +
  scale_y_continuous(limits = c(12, 22), breaks = seq(12, 22, by = 2)) + 
  scale_color_brewer(palette = "Dark2", name = "Species") +
  labs(
    title = "Penguin Bill Size",
    subtitle = "Multiple Islands",
    x = "Bill Length (mm)",
    y = "Bill Depth (mm)",
    caption = "Plot by Chris Prener"
  )
```

Now that we’ve finalized our plot, we can save it using `ggsave()`:

``` r
ggsave(filename = here("results", "bill_length_depth.png"), plot = p)
```

    ## Saving 7 x 5 in image

## Experimentation

Now, you try adjusting the plot above to fit new data, comparing
`flipper_length_mm` and `body_mass_g`. Save your plot when you’re done!

``` r
ggplot(data = penguins, mapping = aes(x = flipper_length_mm, y = body_mass_g,
                                      color = species)) +
  geom_point() +
  scale_color_brewer(palette = "Set1", name = "Species") +
  scale_x_continuous(
    limits = c(170, 240), 
    breaks = seq(170, 240, by = 10)
  ) +
  scale_y_continuous(
    limits = c(2500, 6500), 
    breaks = seq(2500, 6500, by = 500)
  ) +
  labs(
    title = "Body Mass and Flipper Length by Species",
    x = "Flipper Length (mm)",
    y = "Body Mass (g)"
  )
```

![](data-skills-03-complete_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
