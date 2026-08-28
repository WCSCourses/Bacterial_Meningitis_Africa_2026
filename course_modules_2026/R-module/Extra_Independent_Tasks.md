# Independent Practice Tasks: African Meningococci Dataset

**Course:** Molecular and Genomic Approaches to Clinical Microbiology in Africa
**Applies skills from:** Lessons 1–3 (loading data, `select`/`filter`, `group_by`/`summarise`, `mutate`/`str_replace`, `ggplot2`)

## How to use this page
Five tasks are listed below, roughly in order of difficulty. **Tasks 1–2** are a warm-up using skills from Lessons 1–2. **Tasks 3–5 are more challenging** and combine several skills from Lesson 3 (grouping, proportions, string cleaning and visualization) — try them without looking at the solution first.

Each task has a **"View solution"** dropdown underneath it — click to expand the code and expected output once you've had a go yourself. Don't peek before attempting the task; that's where the real learning happens.

> 💡 This dataset (`African_meningococci.xlsx`) is the same one from Lessons 1–3. If you have your own dataset (for example, a line list with columns like `pathogen`, `ct_value`, `sample_id`, `collection_date`), try repeating these tasks on it — the logic is exactly the same, only the column names change.

---

## Before you start
```r
library(openxlsx)
library(dplyr)
library(janitor)
library(stringr)
library(ggplot2)

data_01_Africa <- read.xlsx(xlsxFile = "datasets/African_meningococci.xlsx") %>%
  clean_names()
```

Check it loaded correctly:
```r
dim(data_01_Africa)
```
Expected: `716  22`

---

## Task 1 — Getting oriented (Lesson 1–2 skills)

Load the dataset, inspect its structure, and find out how many isolates were collected from each **country**.

**Steps to guide you:**
1. Use `str()` or `glimpse()` to inspect the dataset.
2. Use `table()` on the `country` column to count isolates per country.

<details>
<summary><b>View solution</b></summary>

```r
# Structure
glimpse(data_01_Africa)

# Count of isolates per country
table(data_01_Africa$country)
```

**Expected output:**
```
             Benin              Burkina Faso                  Cameroon 
                41                       248                         4 
Central African Republic                      Chad                    Guinea 
                       30                        96                        13 
              Ivory Coast                      Mali                     Niger 
                        8                        52                       119 
                  Nigeria                      Togo 
                       35                        70 
```

Burkina Faso has the most isolates (248), Cameroon the fewest (4).
</details>

---

## Task 2 — Select, filter and chain with the pipe (Lesson 2 skills)

Using the pipe operator `%>%`, build one chain of code that:
1. Selects the columns `id`, `isolate`, `country`, `year`, `serogroup`
2. Filters the data to **Burkina Faso** only
3. Then, using `table()`, count how many samples were collected in Burkina Faso in **each year**.

<details>
<summary><b>View solution</b></summary>

```r
burkina_faso <- data_01_Africa %>%
  select(id, isolate, country, year, serogroup) %>%
  filter(country == "Burkina Faso")

table(burkina_faso$year)
```

**Expected output:**
```
2011 2012 2013 2014 2015 2016 
  41  167   20    4   11    5 
```

Most Burkina Faso samples in this dataset were collected in 2012.
</details>

---

## Task 3 — Grouping, summarising and proportions (harder)

For **each country**, calculate:
- the number of isolates belonging to each **serogroup**
- the **proportion** (%) that each serogroup represents *within that country*

Then answer: **which country has the highest proportion of serogroup W?**

**Hints:**
- `group_by(country, serogroup)` then `summarise(count = n())`
- `mutate(prop = count / sum(count) * 100)` calculates proportions *within* each group created by `group_by()`
- `arrange(desc(prop))` and `filter(serogroup == "W")` will help you find the answer

<details>
<summary><b>View solution</b></summary>

```r
serogroup_by_country <- data_01_Africa %>%
  group_by(country, serogroup) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)

serogroup_by_country %>%
  filter(serogroup == "W") %>%
  arrange(desc(prop))
```

**Expected output (top rows):**
```
country                    serogroup  count    prop
Central African Republic   W            30  100.00
Togo                       W            68   97.14
Benin                      W            38   92.68
Ivory Coast                W             7   87.50
Burkina Faso               W           191   77.02
```

**Answer:** Central African Republic has the highest proportion of serogroup W in its samples (100% — every isolate collected from CAR in this dataset was serogroup W). Togo is a close second, at ~97%.

> ⚠️ Note: a proportion of 100% here is partly influenced by sample size (CAR only has 30 isolates in total, all W). Always check the underlying `count`, not just the `prop`, before drawing conclusions — a small country with few isolates can easily show 100%.
</details>

---

## Task 4 — Cross-tabulating and visualizing over time (hard)

Build a single summarised table showing the number of samples of **each serogroup**, broken down by **year** and **country**, with a proportion column. Then use `ggplot2` to visualize it as a bubble plot: `year` on the x-axis, `country` on the y-axis, point colour by `serogroup`, and point size by `count`.

**Hints:**
- `group_by(country, year, serogroup)` → `summarise(count = n())` → `mutate(prop = count / sum(count) * 100)`
- `ggplot(aes(x = year, y = country)) + geom_point(aes(color = serogroup, size = count), alpha = 0.6)`

<details>
<summary><b>View solution</b></summary>

```r
data_serogroup_year <- data_01_Africa %>%
  group_by(country, year, serogroup) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)

ggplot(data_serogroup_year, aes(x = year, y = country)) +
  geom_point(aes(color = serogroup, size = count), alpha = 0.6) +
  xlab("Year") +
  ylab("Country") +
  theme_minimal() +
  theme(
    axis.title = element_text(size = 14),
    axis.text  = element_text(size = 10)
  )
```

**Expected outcome:** a bubble plot with one row per country and one column per year, where bubble colour shows which serogroup dominated and bubble size shows how many isolates were collected. You should be able to see, for example, that serogroup W (a distinct colour) dominates most country/year combinations, while serogroup A appears mainly in Chad in earlier years.

> 📸 Compare your plot to the "Function: ggplot" example from Lesson 3 — the shape of the plot should look similar, though the exact colours and point positions will depend on your dataset.
</details>

---

## Task 5 — Cleaning categorical text and cross-referencing with vaccine coverage (hardest)

The `bexsero_reactivity` column records whether each isolate reacted with the Bexsero vaccine antigens, using the categories `"exact match"`, `"cross-reactive"`, `"insufficient data"` and `"none"`. These labels are a bit long for a plot legend.

1. Use `mutate()` with `str_replace()` (or `case_when()`) to create a new column called `vaccine_match`, recoding:
   - `"insufficient data"` → `"unknown"`
   - `"none"` → `"no match"`
   - leave `"exact match"` and `"cross-reactive"` unchanged
2. Then, group by `serogroup` and your new `vaccine_match` column, and calculate the count and proportion of each `vaccine_match` category *within each serogroup*.
3. Finally, answer: **which serogroup has the highest proportion of isolates with "insufficient data"/"unknown" vaccine reactivity?**

**Hints:**
- `str_replace()` only works on exact substrings — for this task, `case_when()` may be cleaner:
  ```r
  mutate(vaccine_match = case_when(
    bexsero_reactivity == "insufficient data" ~ "unknown",
    bexsero_reactivity == "none" ~ "no match",
    TRUE ~ bexsero_reactivity
  ))
  ```
- Remember: `mutate(prop = count / sum(count) * 100)` calculates proportions **within** whatever groups were set by your most recent `group_by()`.

<details>
<summary><b>View solution</b></summary>

```r
vaccine_summary <- data_01_Africa %>%
  mutate(vaccine_match = case_when(
    bexsero_reactivity == "insufficient data" ~ "unknown",
    bexsero_reactivity == "none" ~ "no match",
    TRUE ~ bexsero_reactivity
  )) %>%
  group_by(serogroup, vaccine_match) %>%
  summarise(count = n()) %>%
  mutate(prop = count / sum(count) * 100)

vaccine_summary %>%
  filter(vaccine_match == "unknown") %>%
  arrange(desc(prop))
```

**Expected output:**
```
serogroup  vaccine_match   count     prop
C          unknown           124   100.00
X          unknown            60    98.36
NG         unknown             2    25.00
```
(Serogroups A, W and Y have 0% "unknown" — they were almost entirely "exact match" or "cross-reactive".)

**Answer:** Serogroup **C** has the highest proportion of "unknown" (insufficient data) vaccine reactivity — every serogroup C isolate in this dataset (124/124) lacked sufficient data on Bexsero reactivity. Serogroup X is a close second at ~98%.

**Optional extension — visualize it:**
```r
ggplot(vaccine_summary, aes(x = serogroup, y = prop, fill = vaccine_match)) +
  geom_col(position = "stack") +
  labs(x = "Serogroup", y = "Proportion of isolates (%)", fill = "Vaccine reactivity") +
  theme_minimal()
```
This produces a stacked bar chart, showing at a glance which serogroups have the most vaccine-reactivity data gaps — useful for prioritising which serogroups need further antigen characterisation.
</details>

---

## Wrap-up
If you got through all five tasks, you've now practised the full workflow taught across Lessons 1–3:
- [ ] Loading and inspecting a dataset
- [ ] Selecting and filtering with `%>%`
- [ ] Grouping, summarising and calculating proportions
- [ ] Visualizing grouped data with `ggplot2`
- [ ] Cleaning/recoding categorical text with `mutate()` + `case_when()`/`str_replace()`

**Try it on your own data:** if you have a sequencing line list (e.g. columns like `sample_id`, `pathogen`, `ct_value`, `collection_date`, `region`), try repeating Tasks 3–5 by swapping `serogroup` for `pathogen` and `country` for `region` — the code structure barely changes.
