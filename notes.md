# R Reference Library

## Reference Sites & Links

https://www.programiz.com/r/dataframe

https://www.infoworld.com/article/3404276/how-to-calculate-month-over-month-changes-in-r.html

https://rfortherestofus.com/2019/11/how-to-make-beautiful-tables-in-r

## Function Reference

- `floor()` rounds a decimal down to the next integer
- `ceiling()` function will round up to the next integer.
- `install.packages('package-name')` - Install a new package - only needed the first time
- `library(package-name)` - call a new package for the script currently in use

### Working with Data Frames

- `df <- read_csv('my_csv_file.csv')` importing CSV file into a dataframe
- `write_csv(df,'new_csv_file.csv')` writing data frame to a csv file
- The `head()` function returns the first 6 rows of a data frame.
- The function `summary()` will return summary statistics such as mean, median, minimum and maximum for each numeric column while providing class and length information for non-numeric columns.
- The dplyr pipe operator, or `%>%`, helps increase the readability of data frame code by piping the value on its left into the first argument of the function that follows it.
    - `customers %>% select(age,gender)`
    - When using the pipe, you can read the code as: from the `customers` table, `select()` the `age` and `gender` columns.
- `select()` takes a data frame as its first argument (or pipe into it) and all additional arguments are the desired columns to select. `select()` returns a new data frame containing only the desired columns
    - To exclude columns, use a minus sign - `customers %>% select(-name,-phone)
- In addition to subsetting a data frame by columns, you can also subset a data frame by rows using dplyr’s `filter()` function and comparison operators.
    - `orders %>% filter(shoe_material == 'faux-leather',price > 25)
- `arrange()` will sort the rows of a data frame in ascending order by the column provided as an argument. To sort by descending order - `arrange(desc(<column>))`
- `mutate()` takes a name-value pair as an argument. The name will be the name of the new column you are adding, and the value is an expression defining the values of the new column in terms of the existing columns.
    - Example: `list %>% mutate(new_column = column1 * .5)`
    - `dogs <- dogs %>%` mutate(avg_height = (height_low_inches + height_high_inches)/2,
    `avg_weight = ((weight_low_lbs + weight_high_lbs)/2),` rank_change_13_to_16 = rank_2016 - rank_2013
    ``)
- `transmute()` function will add new columns while dropping the existing columns that may no longer be useful for your analysis. To keep an existing column use an argument like `column1 = column1`
- `rename()` can take any number of arguments, where each new column name is assigned to replace an old column name in the format `new_column_name = old_column_name`. `rename()` returns a new data frame with the updated column names.
- `filter` is used to get data from particular rows, `select` is used to get data from particular columns

## Cleaning Data

You’ve seen most of the functions we often use to diagnose a dataset for cleaning. Some of the most useful ones are:

- `head()` — display the first 6 rows of the table
- `summary()` — display the summary statistics of the table
- `colnames()` — display the column names of the table
- `nrow()` - number of rows in df

If data is not appropriately on rows - i.e. each observation does not have its own row...

- `substr()` - for taking a string and splitting at certain character spaces per example below

```r
add gender and age columns
Currently stored as "m14" for male 14.

students <- students %>% mutate(
gender = substr(gender_age,1,1),
age = substr(gender_age,2,3)
)
head(students)

```

- `separate()` to split this column into two, separate columns:

```r
# Create the 'user_type' and 'country' columns
df %>%
  separate(type,c('user_type','country'),'_')
#`type` is the column to split
#`c('user_type','country')` is a vector with the names of the two new columns
#`'_'` is the character to split on
```

- `str(df)` will give you the structure of each data element
- `as.numeric()` - converting string to numbers

```r
# remove % from score column
students <- students %>% mutate(score = gsub('\\%','',score)) %>%
mutate(score = as.numeric(sc
```

## Shaping Data

Since we want

- Each variable as a separate column
- Each row as a separate observation

We would want to reshape a table like:

| Account | Checking | Savings |
| --- | --- | --- |
| “12456543” | 8500 | 8900 |
| “12283942” | 6410 | 8020 |
| “12839485” | 78000 | 92000 |

Into a table that looks more like:

| Account | Account Type | Amount |
| --- | --- | --- |
| “12456543” | “Checking” | 8500 |
| “12456543” | “Savings” | 8900 |
| “12283942” | “Checking” | 6410 |
| “12283942” | “Savings” | 8020 |
| “12839485” | “Checking” | 78000 |
| “12839485” | “Savings” | 920000 |
- `gather()` - We can use tidyr’s `gather()` function to do this transformation. `gather()` takes a data frame and the columns to unpack:

```r
df %>%  gather('Checking','Savings',key='Account Type',value='Amount')

```

The arguments you provide are:

- `df`: the data frame you want to gather, which can be piped into `gather()`
    - `Checking` and `Savings`: the columns of the old data frame that you want to turn into variables
    - `key`: what to call the column of the new data frame that stores the variables
    - `value`: what to call the column of the new data frame that stores the values
- `duplicated()` - To check for duplicates, we can use the base R function `duplicated()`, which will return a logical vector telling us which rows are duplicate rows.
- `distinct()` - We can use the dplyr `distinct()` function to remove all rows of a data frame that are duplicates of another row.
- **How to find and count duplicated rows in updated data frame**

```r
updated_duplicates <- unique_students %>%
duplicated() %>%'
table()
```

### Combining Data

You can combine the base R [functions](https://www.codecademy.com/resources/docs/r/functions)`list.files()` and `lapply()` with readr and dplyr to organize this data better, as shown below:

```r
files <- list.files(pattern = "file_.*csv")
df_list <- lapply(files,read_csv)
df <- bind_rows(df_list)
```

## Notes

The dplyr package in R is designed to make data manipulation tasks simpler and more intuitive than working with base R [functions](https://www.codecademy.com/resources/docs/r/functions) only. Called a “grammar of data manipulation,” dplyr provides functions that solve many challenges that arise when organizing tabular data (i.e., data in a table with rows and columns). Tabular data has a lot of the same functionality as tables from SQL or Excel, but dplyr adds the power of R.

dplyr and readr are a part of the tidyverse, a collection of R packages designed for data science.

Note: when working with `dplyr`, you might see functions that take a data frame as an argument and output something called a tibble. Tibbles are modern versions of data frames in R, and they operate in essentially the same way. The terms tibble and data frame are often used interchangeably.

First two libraries to usually load:
