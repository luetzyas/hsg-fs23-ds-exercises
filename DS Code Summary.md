# hsg-fs23-ds-exercises Code Summary
This File is a notesheet with all the code for the module data science.
1. [Exercise 02](#exercise-02)
    1. [Load the data](#1-load-the-data)
    2. [Clean the data](#2-clean-the-data)
    3. [Simple statistics](#3-simple-statistics)
    4. [Average  cards per game](#4-average-cards-per-game)
    5. [Average number of cards per country](#5-average-number-of-cards-per-country)

X. [More](#more)

# Exercise 02[.pdf](doc/E02.pdf)

## 1. Load the data
Import Pandas library -> has to be installed on the python environment
```shell
import pandas as pd
```
read from csv file
```shell
df = pd.read_csv("https://drive.switch.ch/index.php/s/UEpTFv2Bfa5C1dd/download") #https
#OR
df = pd.read_csv('data/CrowdstormingDataJuly1st.csv') #csv
```

### Dataframe workaround
display the data from the read csv-file -> table
```shell
df.head()
```
look at the columns
```shell
df.columns
```
display df contenct information
```shell
df.shape #(rows,columns)
# OR
df.len()
```

## 2. Clean the data
remove [NaN](#nan-and-nat)
- You can use the [`dropna()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dropna.html) method to remove any rows or columns that contain `NaN` values from a DataFrame.
- You can use the [`fillna()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.fillna.html) method to replace `NaN` values with a specific value or a method (like forward fill or backward fill).
```shell
df = df.dropna()
#OR
df = df.fillna() #needs parameters 
```
confirm that the size went down
```shell
df.shape
```
display df length
```shell
len(df)
```

## 3. Simple statistics
describe the dataframe (count of non null values etc.)
```shell
df.describe()
```
calculate median on all columns
```shell
df.median() #the issue with this is that it does not calculate the median

numeric_columns = df.select_dtypes(include=[np.number]).columns
numeric_columns
```
select numeric columns
```shell
df[numeric_columns].head()
```
now calculate with the median function
```shell
df[numeric_columns].median()  # by default column-wise!
```

## 4. Average  cards per game
calculate number of cards for each player
```shell
df['total_cards'] = df['yellowCards'] + df['redCards']
df['total_cards'].head()
```
calculate the average number of cards per game for each player
```shell
df['avg_cards_per_game'] = df['total_cards'] / df['games']
df['avg_cards_per_game'].head()
```
sort the players by the average number of cards per game
```shell
avg_cards_per_game_df = df.sort_values(by='avg_cards_per_game', ascending=False)
avg_cards_per_game_df
```
print out the top 5 players
```shell
avg_cards_per_game_df[:5]
```

## 5. Average number of cards per country
group data by country
```shell
grouped_by_country = df.groupby('leagueCountry')
#OR
grouped_by_country.groups.keys()
```
calculate the average number of cards per game for each country
```shell
(grouped_by_country['yellowCards'].sum() + grouped_by_country['redCards'].sum()) / grouped_by_country['games'].sum()
```

## 6. Correlation

```shell

```

## 6. 

```shell

```

# More
## NaN and NaT
Note that `NaN != NaN`!

You can find a detailed explanation of why this is the case [here](https://stackoverflow.com/a/1573715/5320601).

In short:
- `a == b` should hold if `(a - b) == 0`
- but then what is `(a - NaN)`?

The way to check for `NaN` values is to use [`.isna()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isna.html).

There is also [`NaT`](https://pandas.pydata.org/docs/reference/api/pandas.NaT.html), which is used for missing time values (like a datetime column), and [`NA`](https://pandas.pydata.org/docs/user_guide/missing_data.html#missing-data-na) which is still experimental and whose behaviour could change without warning between versions of `pandas`.

```shell
a = np.nan
b = np.nan

a == b

#output
false

print(f'{"NaN and None equality:":<30} {np.nan == None}')
print(f'{"None and None equality:":<30} {None == None}')
print(f'{"NaN and NaN equality:":<30} {np.nan == np.nan}')

#output
NaN and None equality:         False
None and None equality:        True
NaN and NaN equality:          False
```

Keep in mind that this can behave unexpectedly if you are not aware of it.
Here we create a DataFrame with integers, and add a row with a missing value. For the missing value we use `None`.
```shell
_numbers = pd.DataFrame(
    [[0, 1, 2], [3, 4, 5]],
    columns=['a', 'b', 'c']
)
_numbers

#output

    a	b	c
0	0	1	2
1	3	4	5
```

We see that this `None` becomes `NaN` in the DataFrame. That is because the dtype of the column is `float64` and `None` is not a number (`NaN`).
```shell
_numbers.loc[2] = [6, 7, None]
_numbers

#output
	a	    b	    c
0	0.0	    1.0	    2.0
1	3.0	    4.0	    5.0
2	6.0	    7.0	    NaN
```
```shell
_numbers.dtypes

#output
a       float64
b       float64
c       float64
dtype:  object
```

When we do the same with strings though, this is different.
```shell
_strings = pd.DataFrame(
    [['Apple', 'red'], ['Banana', 'yellow']],
    columns=['Fruit', 'Color']
)
_strings

#output
    Fruit	Color
0	Apple	red
1	Banana	yellow
```
```shell
_strings.loc[2] = ['Cherry', None]
_strings

#output
    Fruit	Color
0	Apple	red
1	Banana	yellow
2	Cherry	None
```
```shell
_strings.dtypes

#output
Fruit    object
Color    object
dtype:   object
```
```shell
_strings

#output
	Fruit	Color
0	Apple	red
1	Banana	yellow
2	Cherry	None
```
```shell
_strings.dropna()

#output
    Fruit	Color
0	Apple	red
1	Banana	yellow
```
```shell

```
```shell

[def]: s