
### Useful Cheat Sheets


```python
# Drop columns
df = df.drop(['col1', 'col2', 'col3'], axis=1)

# Count NaN values for each column
df.isnull().sum()

# Get rid of non-numeric values in a column
df['A'] = df['A'].str.replace(r'[^\d]+', '') # Delete ^ to do the opposite

# Get names of all non-numeric columns
numerics = ['int16', 'int32', 'int64', 'float16', 'float32', 'float64']
nonNumericColumns = df.select_dtypes(exclude=numerics).columns

# Concatenate two columns into a new column
df['newcol'] = df['col1'].map(str) + df['col2'].map(str)

# Set column values based on other column values
df['column_to_change'][(df['column1'] == some_value) & (df['column2'] == some_other_value)] = new_value

# Sum of column 'A' group by 'Key'
df['Sum A'] = df['A'].groupby(df['Key']).transform('sum')

# Pivot
df.pivot(index='date', # stay as columns
         columns='variable', # data values in this column become their own column
         values='value' # value filling the new cell
        )

# Cross-tabulation (A as index, B as column) with percentages instead of frequencies
pd.crosstab(df.A, df.B).apply(lambda r: r/r.sum(), axis=1) 

    

##########################################
# Create a clean list of correlations :  #
#                                        #
#     level_0  level_1  Correlation      #
#        col1     col2       corr12      #
#        col1     col3       corr13      #
##########################################
corrList = df.corr().stack().reset_index()
corrList = corrList.rename(columns={0: 'Correlation'})
corrList = corrList[corrList['level_0'] != corrList['level_1']] # Drop correlations between the same columns
corrList['Correlation'] = corrList['Correlation'].abs() # Absolute value
# Drop duplicates
sizeBefore = corrList.shape[0]
corrList = corrList.drop_duplicates('Correlation') # Ugly but easiest way of doing it ...
assert corrList.shape[0] == sizeBefore / 2 # At least, test if you removed half of the df
# Get N best or worst correlations
corrList = corrList.sort(columns='Correlation', ascending=False)
corrList.head(N)
corrList.tail(N)
```
