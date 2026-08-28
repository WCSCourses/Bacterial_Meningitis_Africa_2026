# Practical 3: Data Wrangling and Visualization

**Course:** Molecular and Genomic Approaches to Clinical Microbiology in Africa
**Session:** Lesson 3
**Duration:** ~1 hour 45 minutes

## Learning objectives
By the end of this practical you will be able to:
- Clean up inconsistent text entries with `mutate()` and `str_replace()`
- Group and summarise data with `group_by()` and `summarise()`
- Calculate proportions within groups
- Build and format a plot with `ggplot2`

Work through **Section A**, then **Section B**, then **Section C**. Stop at each checkpoint and compare with the expected output before continuing.

---

## Before you start — recap from Practical 2

Make sure the following runs without errors:

```r
library(dplyr)
library(janitor)
library(stringr)
library(stringi)
library(ggplot2)

data_01_Africa <- read.xlsx(xlsxFile = "datasets/African_meningococci.xlsx")
```

> ⚠️ If `data_01_Africa` is not loaded, revisit Practical 1 (Section C) before continuing.

---

## Section A: Cleaning text entries with `mutate()` and `str_replace()`

Real-world datasets are rarely perfectly consistent. In this dataset, some entries for Togo are written as `"Togo"` and the goal is to standardise them to `"TOGO"`.

### A1. Replace values in one column
```r
library(stringi)
library(stringr)

bonus_task <- data_01_Africa %>%
  clean_names() %>%
  mutate(country = str_replace(string = country, pattern = "Togo", replacement = "TOGO"))
```

**Expected outcome:** every occurrence of `"Togo"` in the `country` column is now `"TOGO"`. Check with:
```r
table(bonus_task$country)
```

### A2. Replace values in more than one column
You can chain two `mutate()` calls with the pipe:

```r
bonus_task <- data_01_Africa %>%
  clean_names() %>%
  mutate(country = str_replace(string = country, pattern = "Togo", replacement = "TOGO")) %>%
  mutate(isolate = str_replace(string = isolate, pattern = "Togo", replacement = "TOGO"))
```

...or combine them inside a single `mutate()` call using a comma:

```r
bonus_task <- data_01_Africa %>%
  clean_names() %>%
  mutate(
    country = str_replace(string = country, pattern = "Togo", replacement = "TOGO"),
    isolate = str_replace(string = isolate, pattern = "Togo", replacement = "TOGO")
  )
```

Both approaches give the same result — use whichever you find more readable.

**Checkpoint A:** Filter `bonus_task` to `country == "TOGO"` and check that the `isolate` column entries now start with `TOGO_` instead of `Togo_`.

---

## Section B: Grouping and summarising data

### B1. Count samples per group
`group_by()` splits the data into groups; `summarise()` then calculates one value per group. `n()` counts the number of rows in each group.

```r
data_03_Africa <- data_01_Africa %>%
  clean_names() %>%
  group_by(country, year) %>%
  summarise(count = n())
```

**Expected output (abridged):**
```
country       year  count
Benin         2012     41
Burkina Faso  2011     41
Burkina Faso  2012    167
Burkina Faso  2013     20
...
```

> 📸 **Screenshot to include:** A filtered/sorted view of `data_03_Africa` (Lesson 3 slide "The number of samples per year in each country").

### B2. Task — more grouping practice
Try these on your own before checking the solutions:
1. What clonal complexes were identified within each genogroup? *(hint: `group_by()` and `summarise()`)*
2. How many clonal complexes and genogroups were identified per country? *(hint: `group_by()` and `summarise()`)*

### B3. Add proportions with `mutate()`
`group_by()` + `summarise()` gives you counts; adding a `mutate()` step afterwards lets you turn those counts into proportions **within each group**.

```r
data_04_Africa <- data_01_Africa %>%
  clean_names() %>%
  group_by(country, clonal_complex_mlst) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)
```

**Expected output (abridged):**
```
country       clonal_complex_mlst   count      prop
Benin         ST-11 complex            38  92.68
Benin         ST-181 complex            3   7.32
Burkina Faso  ST-10217 complex          1   0.40
Burkina Faso  ST-11 complex           194  78.23
...
```

**Checkpoint B — Task:**
1. Which country has the highest proportion of serogroup **W**?
2. Build a single table showing the number (and proportion) of each serogroup by year **and** country. *(hint: `group_by(country, year, serogroup)`)*

(Solutions at the end of this document.)

---

## Section C: Visualizing your data with `ggplot2`

We'll build a bubble/point plot showing serogroup counts across countries and years, using the summarised table from Section B (`country`, `year`, `serogroup`, `count`, `prop`).

### C1. Prepare the summarised data
```r
data_05_Africa <- data_01_Africa %>%
  clean_names() %>%
  group_by(country, year, serogroup) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)
```

### C2. Build a basic plot
```r
library(ggplot2)

plot_Africa <- data_05_Africa %>%
  ggplot(aes(x = year, y = country)) +
  geom_point(aes(color = serogroup, size = count), alpha = 0.6)

plot_Africa
```

> ⚠️ Note: `alpha` controls point transparency and should be a value between 0 (fully transparent) and 1 (fully opaque) — use something like `alpha = 0.6`, not `alpha = 6`.

**Expected outcome:** a scatter plot with `year` on the x-axis, `country` on the y-axis, points coloured by `serogroup`, and point size proportional to `count`.

> 📸 **Screenshot to include:** The resulting bubble plot (Lesson 3 slide "Function: ggplot").

### C3. Format the plot
```r
plot_Africa <- data_05_Africa %>%
  ggplot(aes(x = year, y = country)) +
  geom_point(aes(color = serogroup, size = count), alpha = 0.6) +
  xlab("Year") +
  ylab("Country") +
  theme_minimal() +
  theme(
    axis.title  = element_text(size = 14),
    axis.text   = element_text(size = 10),
    legend.text = element_text(size = 10),
    legend.title = element_text(size = 11),
    legend.key.size = unit(1, "lines")
  )

plot_Africa
```

### C4. Alternative: a line plot of cases over time (from the R script example)
This example shows the same grouping/plotting logic applied to a second dataset shape — total cases per WHO region per month — and demonstrates converting text month labels into real dates with `lubridate`.

```r
library(lubridate)

data_summary <- data_01_Africa %>%
  clean_names() %>%
  group_by(who_region, month_lab) %>%
  summarise(total_cases = sum(cases)) %>%
  mutate(
    month_lab_full = paste0("01-", month_lab),
    month_date = dmy(month_lab_full)
  ) %>%
  arrange(month_date)

ggplot(data_summary, aes(x = month_date, y = total_cases, color = who_region)) +
  geom_line(linewidth = 1.2) +
  geom_point(size = 2) +
  labs(x = "Month", y = "Total Cases", color = "WHO Region") +
  scale_x_date(date_labels = "%b-%y", date_breaks = "1 month") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

> Note: this step uses columns (`who_region`, `month_lab`, `cases`) that come from a different example dataset (mpox line-list) rather than `African_meningococci.xlsx`. It is included here to show the pattern of preparing dates and plotting trends over time — adapt the column names to whichever dataset you're using.

**Checkpoint C — Task:** Adapt the code from C3 to plot only **serogroup C**, showing its distribution by country across years. *(hint: `filter(serogroup == "C")` before piping into `ggplot()`)*

---

## End-of-practical summary
You should now be able to:
- [ ] Clean inconsistent text entries with `mutate()` + `str_replace()`
- [ ] Group data with `group_by()` and summarise with `summarise()` and `n()`
- [ ] Calculate within-group proportions with `mutate()`
- [ ] Build a `ggplot2` scatter/bubble plot with `aes()`, `geom_point()`
- [ ] Format axis labels, themes and legend text size in `ggplot2`

---

## Solutions

### B2 task
```r
# 1. Clonal complexes by genogroup
data_01_Africa %>%
  clean_names() %>%
  group_by(genogroup, clonal_complex_mlst) %>%
  summarise(count = n())

# 2. Number of clonal complexes and genogroups per country
data_01_Africa %>%
  clean_names() %>%
  group_by(country) %>%
  summarise(
    n_clonal_complexes = n_distinct(clonal_complex_mlst),
    n_genogroups        = n_distinct(genogroup)
  )
```

### B3 checkpoint task
```r
data_serogroup <- data_01_Africa %>%
  clean_names() %>%
  group_by(country, year, serogroup) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)

# 1. Country with the highest proportion of serogroup W
data_serogroup %>%
  filter(serogroup == "W") %>%
  arrange(desc(prop)) %>%
  head(1)
```
(From the training dataset, Benin has one of the highest proportions of serogroup W, at roughly 93% of its samples — confirm this against your own output, as exact values depend on the dataset version.)

### C4 checkpoint task
```r
plot_serogroup_C <- data_05_Africa %>%
  filter(serogroup == "C") %>%
  ggplot(aes(x = year, y = country)) +
  geom_point(aes(size = count), color = "darkred", alpha = 0.6) +
  xlab("Year") +
  ylab("Country") +
  theme_minimal()

plot_serogroup_C
```

---
*Previous: [Practical 2 — Exploring and Cleaning Data with R](Practical_2_Exploring_and_Cleaning_Data.md)*

## Further resources
- R for Data Science (free online book): https://r4ds.hadley.nz/
- Comprehensive R programming reference: https://ggnindia.dronacharya.info/Downloads/Subinfo/RelatedBook/8thSem/R-PROGRAMMING-text-book-2.pdf
