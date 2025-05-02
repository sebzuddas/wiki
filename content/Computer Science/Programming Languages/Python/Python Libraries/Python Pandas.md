---
aliases:
  - Pandas
  - pandas
---
# Introduction

---
# Standard Pandas

## Useful Operations
### Resampling
Useful for filling in timeseries data when there are gaps
https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.resample.html
`filled_df = df.resample('1T').asfreq().fillna(0)`
#### Example
```python
def resample_timeseries(self, df) -> pd.DataFrame:

"""
This Python function resamples a time series DataFrame to a common time interval based on the most common time difference between samples.

:param df: The `resample_timeseries` function takes a DataFrame `df` as input and resamples it based on the most common time interval found in the index of the DataFrame. The function calculates the most common time interval between samples, rounds it to the nearest second, and then uses this rounded interval to 

:return: The function `resample_timeseries` is returning a resampled DataFrame with forward-filled
values based on the most common time interval found in the input DataFrame `df`.
"""

time_diffs = df.index.to_series().diff() # get the difference between sample times
rounded_interval = time_diffs.mode()[0]# find most common time interval
common_interval_seconds = round(rounded_interval.total_seconds()) # find the most common and round
rounded_sample_time = pd.Timedelta(seconds=common_interval_seconds) # make these a Timedelta object
df = df.resample(rounded_sample_time).ffill()
return df

```

### Creating Columns Based on Existing Columns

Used to create data based on existing data
#### Example
Creating an 'input' column. 
When the state is not zero, make the input 1, else zero ([[Signals and Systems#^8d8915|step function]]).
```python

# Using np.where
df["input"] = np.where(df["state"] != 0, 1, 0) # 


#Alternatively, you can use `.apply` with a lambda function 

df["input"] = df["state"].apply(lambda x: 1 if x != 0 else 0)


```

### Turning Dataframes into Numpy Arrays
Simply use `df.values` 
#### Example
```python
numpy_dataset = df.values
```

### Dropping NaN values
Standard `dropna()` will drop all rows where, within any column, it finds a nan.

Can select a specific column to drop the values:

```python 
df = df.dropna(subset=['column_name'])
```



---









---


# GeoPandas


---
# PandasAI

---