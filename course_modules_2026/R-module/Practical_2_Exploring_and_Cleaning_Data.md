# Practical 2: Exploring and Cleaning Data with R

**Course:** Molecular and Genomic Approaches to Clinical Microbiology in Africa
**Session:** Lesson 2
**Duration:** ~1 hour 30 minutes

## Learning objectives
By the end of this practical you will be able to:
- View and understand the structure of a dataset
- Clean up messy column names
- Select and filter data
- Chain multiple operations together using the pipe operator `%>%`
- Answer simple questions about a dataset using `table()`

Work through **Section A**, then **Section B**, then **Section C**. Stop at each checkpoint and compare with the expected output before continuing.

---

## Before you start — recap from Practical 1

Make sure you can run the following without errors before continuing:

```r
library(dplyr)
library(janitor)

male   <- c(20, 30, 40)
female <- c(40, 30, 20)
month  <- c("Jan", "Feb")

data_01_Africa <- read.xlsx(xlsxFile = "datasets/African_meningococci.xlsx")
```

> ⚠️ If `data_01_Africa` is not in your Environment pane, re-do Section C of Practical 1 before continuing — everything in this practical depends on that object.

---

## Section A: Viewing and understanding your data

### A1. Check the structure of the dataset
```r
str(data_01_Africa)

# OR

glimpse(data_01_Africa)
```

**Expected output (abridged):**
```
Rows: 716
Columns: 22
$ id                <chr> ...
$ isolate           <chr> ...
$ country           <chr> ...
$ year              <dbl> ...
...
```

### A2. View the column names
```r
names(data_01_Africa)

# OR

colnames(data_01_Africa)
```

### A3. Clean up the column names
Raw column names from Excel files often contain spaces, capital letters or special characters, which are awkward to type in R. The `clean_names()` function (from the `janitor` package) converts them to a consistent, lower-case, underscore-separated format.

```r
data_02_Africa <- clean_names(data_01_Africa)

# Confirm the names have changed
names(data_01_Africa)
names(data_02_Africa)
```

**Expected outcome:** `data_01_Africa` keeps its original column names; `data_02_Africa` has tidied, lower-case column names (e.g. `Clonal Complex (MLST)` becomes `clonal_complex_mlst`).

### A4. View the first and last rows
```r
data_03_Africa <- head(data_01_Africa, 10)  # first 10 rows (default is 6)
data_04_Africa <- tail(data_01_Africa)      # last 6 rows (default)
```

**Checkpoint A:** Run `dim(data_01_Africa)`. You should see `716 22` — confirming 716 rows and 22 columns.

---

## Section B: Selecting columns and filtering rows

### B1. Select specific columns
```r
data_01_subset <- select(data_01_Africa, id, isolate, country)
```

### B2. Select by removing columns
```r
data_02_subset <- select(data_01_Africa, -id, -isolate)
```

### B3. Filter rows by a condition
```r
Nigeria <- filter(data_01_Africa, country == "Nigeria")
Rwanda  <- filter(data_01_Africa, country == "Rwanda")
```

> ⚠️ Note the double equals sign `==` for "is equal to" — a single `=` will not work for filtering conditions.

### B4. Count samples per category
```r
table(data_01_Africa$country)
```

**Expected output (abridged):** a table listing each country and the number of samples for it, e.g.:
```
     Benin Burkina Faso     Cameroon ...
        41          247            4 ...
```

**Checkpoint B — Try it yourself:**
1. How many samples were collected in each year? *(hint: `table()`)*
2. How many samples belong to each serogroup? *(hint: `table()`)*
3. How many samples were collected in each year, but only for Togo? *(hint: `filter()` then `table()`)*

(Solutions at the end of this document.)

### B5. Tidy up your environment
Once you've confirmed your subsets worked, it's good practice to remove intermediate objects you no longer need:

```r
rm(data_02_Africa, data_03_Africa, data_04_Africa, Nigeria, Rwanda,
   data_01_subset, data_02_subset)
```

---

## Section C: Chaining steps with the pipe operator

Typing every step into a new variable name (`data_01`, `data_02`, `data_03`...) quickly becomes unmanageable. The **pipe operator** `%>%` (from `dplyr`, loaded via `library(dplyr)`) lets you chain several operations together, feeding the output of one step directly into the next.

### C1. A single step with the pipe
```r
data_02_Africa <- data_01_Africa %>%
  clean_names()
```

### C2. Two steps with the pipe
```r
data_02_Africa <- data_01_Africa %>%
  clean_names() %>%
  select(id, isolate, year, country)
```

### C3. Three steps with the pipe
```r
data_02_Africa <- data_01_Africa %>%
  clean_names() %>%
  select(id, isolate, year, country) %>%
  filter(country == "Burkina Faso")
```

**Expected outcome:** `data_02_Africa` contains only the `id`, `isolate`, `year` and `country` columns, and only rows where `country` is `"Burkina Faso"`.

> 💡 Read `%>%` as "and then." The code above reads as: *"take `data_01_Africa`, and then clean the names, and then select these columns, and then filter to Burkina Faso."*

**Checkpoint C:** Using the pipe operator, write one chain of code that:
1. Starts from `data_01_Africa`
2. Cleans the column names
3. Selects `id`, `isolate`, `country`, `year`, `serogroup`
4. Filters to only `"Togo"`

Confirm your result with `nrow(your_object)` — how many rows does Togo have?

---

## End-of-practical summary
You should now be able to:
- [ ] Inspect a dataset's structure with `str()` / `glimpse()`
- [ ] Clean column names with `clean_names()`
- [ ] View first/last rows with `head()` / `tail()`
- [ ] Select columns with `select()` (by keeping or dropping)
- [ ] Filter rows with `filter()` and `==`
- [ ] Count occurrences with `table()`
- [ ] Chain multiple steps together with `%>%`

---

## Solutions

### B4 checkpoint
**1. Samples per year:**
```r
table(data_01_Africa$year)
```
```
2011 2012 2013 2014 2015 2016
 115  272   60   26  158   85
```

**2. Samples per serogroup:**
```r
table(data_01_Africa$serogroup)
```
```
  A   C  NG   W   X   Y
 90 124   8 431  61   2
```

**3. Samples per year, Togo only:**
```r
Togo <- filter(data_01_Africa, country == "Togo")
table(Togo$year)
```
```
2014 2015 2016
  16   12   42
```

### C3 checkpoint
```r
data_togo <- data_01_Africa %>%
  clean_names() %>%
  select(id, isolate, country, year, serogroup) %>%
  filter(country == "Togo")

nrow(data_togo)
```

---
*Previous: [Practical 1 — Getting Started with R](Practical_1_Getting_Started_with_R.md)*
*Next: [Practical 3 — Data Wrangling and Visualization](Practical_3_Data_Wrangling_and_Visualization.md)*
