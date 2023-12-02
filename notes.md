## Reference Sites & Links
https://www.programiz.com/r/dataframe

https://www.infoworld.com/article/3404276/how-to-calculate-month-over-month-changes-in-r.html

https://rfortherestofus.com/2019/11/how-to-make-beautiful-tables-in-r

## Function Reference
* `floor()`  rounds a decimal down to the next integer
* `ceiling()` function will round up to the next integer.
* `install.packages('package-name')` - Install a new package - only needed the first time 
* `library(package-name)` - call a new package for the script currently in use
### Working with Data Frames
* `df <- read_csv('my_csv_file.csv')` importing CSV file into a dataframe
* `write_csv(df,'new_csv_file.csv')` writing data frame to a csv file
* The `head()` function returns the first 6 rows of a data frame.
- The function `summary()` will return summary statistics such as mean, median, minimum and maximum for each numeric column while providing class and length information for non-numeric columns.
- The dplyr  pipe operator, or `%>%`, helps increase the readability of data frame code by piping the value on its left into the first argument of the function that follows it. 
	- ```customers %>%  select(age,gender)```
	- When using the pipe, you can read the code as: from the `customers` table, `select()` the `age` and `gender` columns. 
- `select()` takes a data frame as its first argument (or pipe into it) and all additional arguments are the desired columns to select. `select()` returns a new data frame containing only the desired columns
	-  To exclude columns, use a minus sign - `customers %>% select(-name,-phone)
- In addition to subsetting a data frame by columns, you can also subset a data frame by rows using dplyr’s `filter()` function and comparison operators.
	- `orders %>% filter(shoe_material == 'faux-leather',price > 25)
- `arrange()` will sort the rows of a data frame in ascending order by the column provided as an argument.  To sort by descending order - `arrange(desc(<column>))`
- `mutate()` takes a name-value pair as an argument. The name will be the name of the new column you are adding, and the value is an expression defining the values of the new column in terms of the existing columns.  
	- Example: `list %>% mutate(new_column = column1 * .5)`
	- `dogs <- dogs %>%
		`mutate(avg_height = (height_low_inches + height_high_inches)/2,
		`avg_weight = ((weight_low_lbs + weight_high_lbs)/2),
		`rank_change_13_to_16 = rank_2016 - rank_2013
		``)
- `transmute()` function will add new columns while dropping the existing columns that may no longer be useful for your analysis. To keep an existing column use an argument like `column1 = column1`
- `rename()` can take any number of arguments, where each new column name is assigned to replace an old column name in the format `new_column_name = old_column_name`. `rename()` returns a new data frame with the updated column names.
## Notes
The dplyr package in R is designed to make data manipulation tasks simpler and more intuitive than working with base R [functions](https://www.codecademy.com/resources/docs/r/functions) only. Called a “grammar of data manipulation,” dplyr provides functions that solve many challenges that arise when organizing tabular data (i.e., data in a table with rows and columns). Tabular data has a lot of the same functionality as tables from SQL or Excel, but dplyr adds the power of R.

dplyr and readr are a part of the tidyverse, a collection of R packages designed for data science.

Note: when working with `dplyr`, you might see functions that take a data frame as an argument and output something called a tibble. Tibbles are modern versions of data frames in R, and they operate in essentially the same way. The terms tibble and data frame are often used interchangeably.
