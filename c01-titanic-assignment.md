RMS Titanic
================
Kathryn
2020-07-14

  - [Grading Rubric](#grading-rubric)
      - [Individual](#individual)
      - [Team](#team)
      - [Due Date](#due-date)
  - [First Look](#first-look)
  - [Deeper Look](#deeper-look)
  - [Notes](#notes)

*Purpose*: Most datasets have at least a few variables. Part of our task
in analyzing a dataset is to understand trends as they vary across these
different variables. Unless we’re careful and thorough, we can easily
miss these patterns. In this challenge you’ll analyze a dataset with a
small number of categorical variables and try to find differences among
the groups.

*Reading*: (Optional) [Wikipedia
article](https://en.wikipedia.org/wiki/RMS_Titanic) on the RMS Titanic.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category    | Unsatisfactory                                                                   | Satisfactory                                                               |
| ----------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Effort      | Some task **q**’s left unattempted                                               | All task **q**’s attempted                                                 |
| Observed    | Did not document observations                                                    | Documented observations based on analysis                                  |
| Supported   | Some observations not supported by analysis                                      | All observations supported by analysis (table, graph, etc.)                |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Team

<!-- ------------------------- -->

| Category   | Unsatisfactory                                                                                   | Satisfactory                                       |
| ---------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| Documented | No team contributions to Wiki                                                                    | Team contributed to Wiki                           |
| Referenced | No team references in Wiki                                                                       | At least one reference in Wiki to member report(s) |
| Relevant   | References unrelated to assertion, or difficult to find related analysis based on reference text | Reference text clearly points to relevant analysis |

## Due Date

<!-- ------------------------- -->

All the deliverables stated in the rubrics above are due on the day of
the class discussion of that exercise. See the
[Syllabus](https://docs.google.com/document/d/1jJTh2DH8nVJd2eyMMoyNGroReo0BKcJrz1eONi3rPSc/edit?usp=sharing)
for more information.

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.1     ✓ dplyr   1.0.0
    ## ✓ tidyr   1.1.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ──────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
df_titanic <- as_tibble(Titanic)
```

*Background*: The RMS Titanic sank on its maiden voyage in 1912; about
67% of its passengers died.

# First Look

<!-- -------------------------------------------------- -->

**q1** Perform a glimpse of `df_titanic`. What variables are in this
dataset?

``` r
## TASK: Perform a `glimpse` of df_titanic
glimpse(df_titanic)
```

    ## Rows: 32
    ## Columns: 5
    ## $ Class    <chr> "1st", "2nd", "3rd", "Crew", "1st", "2nd", "3rd", "Crew", "1…
    ## $ Sex      <chr> "Male", "Male", "Male", "Male", "Female", "Female", "Female"…
    ## $ Age      <chr> "Child", "Child", "Child", "Child", "Child", "Child", "Child…
    ## $ Survived <chr> "No", "No", "No", "No", "No", "No", "No", "No", "No", "No", …
    ## $ n        <dbl> 0, 0, 35, 0, 0, 0, 17, 0, 118, 154, 387, 670, 4, 13, 89, 3, …

**Observations**:

  - This is a dataset with few variables (class, sex, age, survival, and
    N (people falling into these categories))
  - There are a moderate number of rows, which are the complete set of
    possible combinations of the data. These are 4 \[class\] \* 2 \[sex
    M/F\] \* 2 \[adult/child\] \* 2 \[survived Y/N\] = 32 rows total.

**q2** Skim the [Wikipedia
article](https://en.wikipedia.org/wiki/RMS_Titanic) on the RMS Titanic,
and look for a total count of passengers. Compare against the total
computed below. Are there any differences? Are those differences large
or small? What might account for those differences?

``` r
## NOTE: No need to edit! We'll cover how to
## do this calculation in a later exercise.
df_titanic %>% summarize(total = sum(n))
```

    ## # A tibble: 1 x 1
    ##   total
    ##   <dbl>
    ## 1  2201

**Observations**:

  - The data lists a total of 2201 passengers, whereas the Wikipedia
    article lists 3,327. There is a discrepancy in the data, meaning the
    observations are incomplete, missing about 30% of passengers who
    were aboard the ship.
  - It is possible that many outcomes were unknown, and therefore did
    not make it into the dataset.

**q3** Create a plot showing the count of passengers who *did* survive,
along with aesthetics for `Class` and `Sex`. Document your observations
below.

*Note*: There are many ways to do this.

``` r
## TASK: Visualize counts against `Class` and `Sex`

df_titanic %>%
  group_by(Class, Sex, Survived) %>% 
  summarise(total = sum(n))
```

    ## `summarise()` regrouping output by 'Class', 'Sex' (override with `.groups` argument)

    ## # A tibble: 16 x 4
    ## # Groups:   Class, Sex [8]
    ##    Class Sex    Survived total
    ##    <chr> <chr>  <chr>    <dbl>
    ##  1 1st   Female No           4
    ##  2 1st   Female Yes        141
    ##  3 1st   Male   No         118
    ##  4 1st   Male   Yes         62
    ##  5 2nd   Female No          13
    ##  6 2nd   Female Yes         93
    ##  7 2nd   Male   No         154
    ##  8 2nd   Male   Yes         25
    ##  9 3rd   Female No         106
    ## 10 3rd   Female Yes         90
    ## 11 3rd   Male   No         422
    ## 12 3rd   Male   Yes         88
    ## 13 Crew  Female No           3
    ## 14 Crew  Female Yes         20
    ## 15 Crew  Male   No         670
    ## 16 Crew  Male   Yes        192

``` r
df_q3 <- filter(df_titanic, Survived == "Yes")

# Bar plot
ggplot(data = df_q3, aes(x = Class, y = n, fill = Sex)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Titanic survivors",
       x = "Class",
       y = "Count")
```

![](c01-titanic-assignment_files/figure-gfm/q3-task-1.png)<!-- -->

**Observations**:

  - For first and second class, the survivors are majority female
  - For third class, survivors are split evenly by sex
  - Surviving crew members are overwhelmingly male

# Deeper Look

<!-- -------------------------------------------------- -->

Raw counts give us a sense of totals, but they are not as useful for
understanding differences between groups. This is because the
differences we see in counts could be due to either the relative size of
the group OR differences in outcomes for those groups. To make
comparisons between groups, we should also consider *proportions*.\[1\]

The following code computes proportions within each `Class, Sex, Age`
group.

``` r
## NOTE: No need to edit! We'll cover how to
## do this calculation in a later exercise.
df_prop <-
  df_titanic %>%
  group_by(Class, Sex, Age) %>%
  mutate(
    Total = sum(n),
    Prop = n / Total
  ) %>%
  ungroup()
df_prop
```

    ## # A tibble: 32 x 7
    ##    Class Sex    Age   Survived     n Total    Prop
    ##    <chr> <chr>  <chr> <chr>    <dbl> <dbl>   <dbl>
    ##  1 1st   Male   Child No           0     5   0    
    ##  2 2nd   Male   Child No           0    11   0    
    ##  3 3rd   Male   Child No          35    48   0.729
    ##  4 Crew  Male   Child No           0     0 NaN    
    ##  5 1st   Female Child No           0     1   0    
    ##  6 2nd   Female Child No           0    13   0    
    ##  7 3rd   Female Child No          17    31   0.548
    ##  8 Crew  Female Child No           0     0 NaN    
    ##  9 1st   Male   Adult No         118   175   0.674
    ## 10 2nd   Male   Adult No         154   168   0.917
    ## # … with 22 more rows

**q4** Replicate your visual from q3, but display `Prop` in place of
`n`. Document your observations, and note any new/different observations
you make in comparison with q3.

``` r
# Filter proportion survival
df_prop_survived <- filter(df_prop, Survived == "Yes")
# df_prop_survived

# Bar plot
ggplot(data = df_prop_survived, aes(x = Class, y = Prop, fill = Sex, color = Age)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Titanic survivors",
       x = "Class",
       y = "Percent survivors")
```

    ## Warning: Removed 2 rows containing missing values (geom_bar).

![](c01-titanic-assignment_files/figure-gfm/q4-task-1.png)<!-- -->

**Observations**:

  - Women and children in first and second class almost all survived.
  - People in third class had less than a 50% chance of survival.
  - Most female crew members survived, whereas most male crew members
    did not.
  - Male third class passengers and crew members were least likely to
    survive.

**q5** Create a plot showing the group-proportion of passengers who
*did* survive, along with aesthetics for `Class`, `Sex`, *and* `Age`.
Document your observations below.

*Hint*: Don’t forget that you can use `facet_grid` to help consider
additional variables\!

``` r
# Filter proportion survival
df_prop_survived <- filter(df_prop, Survived == "Yes")

# Bar plot
ggplot(data = df_prop_survived, aes(x = Class, y = Prop, fill = Sex)) +
  geom_bar(stat = "identity", position = "dodge") + 
  facet_wrap(~ Age) +
  labs(title = "Titanic survivors",
       x = "Class",
       y = "Percent survivors")
```

    ## Warning: Removed 2 rows containing missing values (geom_bar).

![](c01-titanic-assignment_files/figure-gfm/q5-task-1.png)<!-- -->

**Observations**:

  - First and second class children were the likeliest to survive, but
    many third class children died
  - Women with higher-class tickets were more likely to survive than
    their lower ticket class counterparts
  - Female third-class passengers were the least likely to survive among
    all women
  - There are (thankfully\!) no children working on the crew

# Notes

<!-- -------------------------------------------------- -->

\[1\] This is basically the same idea as [Dimensional
Analysis](https://en.wikipedia.org/wiki/Dimensional_analysis); computing
proportions is akin to non-dimensionalizing a quantity.
