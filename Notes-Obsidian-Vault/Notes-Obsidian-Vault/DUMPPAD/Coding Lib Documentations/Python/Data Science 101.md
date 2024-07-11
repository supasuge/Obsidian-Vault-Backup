## Table of Contents

    - [Pandas](#Pandas)

Collection: Collecting the raw Data

Processing: Turning the raw data into a standard format that can be analyzed.

Mining: Creating relationships between the data, finding patterns and correlations that can start to provide some insight.

Analysis: data is explored to provide answers to a particular solution

Visualization: Charts, maps etc.

### Pandas
**Series**: Similar to singular column in a table. Uses a key-value pair.
	- Key is the index number, and the value is the data stored in said index
1. Create a list: `transportation = ['Train', 'Plane', 'Car']`
2. Create a new variable to store the series by providing the list from above: `transportation_series = pd.Series(transportation)`
3. Now, let's print the series: `print(transportation_series)`
```python
import pandas as pd
transportation = ['Train', 'Plane', 'Car']
T_series = pd.Series(transportation)
```

|   |   |
|---|---|
|**Key (Index)**|**Value**|
|0|Train|
|1|Plane|
|2|Car|


**DataFrames**: Extend `series` by grouping them. Can be compared to a spreadsheet or a database, they can be though of as a table with rows and columns.
For this, we will create a two-dimensional list. Remember, a DataFrame has rows and columns, so we will need to provide each row with data in the respective column.  

1. `data = [['Ben', 24, 'United Kingdom'], ['Jacob', 32, 'United States of America'], ['Alice', 19, 'Germany']]`
2. Now we create a new variable (df) to store the DataFrame using the list from above. We will need to specify the columns in the order of the list. For example:
    
3. Ben (Name)
4. 24 (Age)
5. United Kingdom (Country of Residence)
    
6. `df = pd.DataFrame(data, columns=['Name', 'Age', 'Country of Residence'])`

`df.count()` - the amounnt of entries

``
