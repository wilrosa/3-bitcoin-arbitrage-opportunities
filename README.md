# Bitcoin Arbitrage Opportunities

This repo contains the results of the module 3 challenge. The assignment is to apply the three phases of financial analysis to determine if any arbitrage opportunities exist for Bitcoin accross two exchanges. As Bitcoin trades on markets across the globe, we attempt to capitalize on simultaneous price dislocations in those markets by using the Pandas library. For this assignment, we sorted through historical trade data for Bitcoin on two exchanges: Bitstamp and Coinbase.

We created a Jupyter notebook that contains the code for data collection, preparation, and analysis, including any visualizations. Within the same notebook file, we use comments and text to document our analysis.

To accomplish these tasks, we perform the following phases of financial analysis:

(1) Collect CSV data in a Jupyter notebook file.
(2) Prepare the datasets for analysis by cleaning missing and erroneous data.
(3) Analyze the data at a high level through summary statistics and visualizations, and use this information to select areas for deeper analysis. Specifically, we select time periods in which to identify arbitrage opportunities.

---

## Technologies

This project leverages python 3.7 with the following packages:

* [pandas](https://github.com/pandas-dev/pandas) - For manipulating data

* [matplotlib](https://github.com/matplotlib/matplotlib) - For creating static, animated, and interactive visualizations

---
## Key Functions

### **Preparing the Data**

To prepare and clean your data for analysis, we completed the following steps for each datafram: 

(1) For the bitstamp and coinbase DataFrame, we drop all NaN, or missing, values in the DataFrame.

```python
bitstamp = bitstamp.dropna().copy()
coinbase = coinbase.dropna().copy()
```

(2) We used the str.replace function to remove the dollar signs from the values in the Close column.

```python
bitstamp["Close"] = bitstamp["Close"].str.replace("$", "")
coinbase["Close"] = coinbase["Close"].str.replace("$", "")
```

(3) The data type of the Close column was then converted to a float.

```python
bitstamp["Close"] = bitstamp["Close"].astype("float")
coinbase["Close"] = coinbase["Close"].astype("float")
```

### **Analyzing the Data**

Our analysis consisted of the following:

(1) Choosing the columns of data on which to focus our analysis. Here, we selected the data using `loc` to select the "Timestamp (index)" and "Close" columns for both the bitstamp and coinbase DataFrames.

```python
bitstamp_sliced = bitstamp['Close'].loc['2018-01-01' : '2018-03-31']
coinbase_sliced = coinbase['Close'].loc['2018-01-01' : '2018-03-31']
```

(2) Getting the summary statistics plot data. Here, we sorted through the time series data associated with the bitstamp and coinbase DataFrames to identify potential arbitrage opportunities.

```python
bitstamp_sliced.describe()
coinbase_sliced.describe()

bitstamp_sliced.plot(
    legend=True, figsize=(15, 10), title="Bitstamp v. Coinbase Q1 2018", color="blue", label="Bitstamp")
coinbase_sliced.plot(
    legend=True, figsize=(15, 10), color="orange", label="Coinbase")
```

(3) Focusing our analysis on specific dates. In this section, we selected three dates to evaluate for arbitrage profitability. We chose January 16, 2018, as a date that’s early in the dataset, February 24, 2018, as a date from the middle of the dataset, and March 26, 2018 as a date from the later part of the time period.

```python
arbitrage_spread_early = coinbase['Close'].loc['2018-01-16'] - bitstamp['Close'].loc['2018-01-16']
arbitrage_spread_middle = coinbase['Close'].loc['2018-02-24'] - bitstamp['Close'].loc['2018-02-24']
arbitrage_spread_late = coinbase['Close'].loc['2018-03-26'] - bitstamp['Close'].loc['2018-03-26']

arbitrage_spread_early.describe()
arbitrage_spread_middle.describe()
arbitrage_spread_late.describe() 
```

(4) Calculating the arbitrage profits. Our goal was to determine whether arbitrage opportunities still exist in the Bitcoin market.

---
## Installation Guide

Using the Pandas read_csv function and the Path module, import the data from bitstamp.csv and coinbase.csv files, and create a DataFrame called bitstamp and a DataFrame called coinbase. Set the DatetimeIndex as the Timestamp column, and be sure to parse and format the dates.

```python
  from pathlib import Path
    
    bitstamp = pd.read_csv(
    Path('../Resources/bitstamp.csv'), 
    index_col="Timestamp", 
    parse_dates=True, 
    infer_datetime_format=True
)
    
    coinbase = pd.read_csv(
    Path('../Resources/coinbase.csv'), 
    index_col="Timestamp", 
    parse_dates=True, 
    infer_datetime_format=True
)
    
```
---
## Analysis
### **Calculating the Arbitrage Profits**

1. For each of the three dates, we measured the arbitrage spread between the two exchanges by subtracting the lower-priced exchange from the higher-priced one. Then used a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame, where the spread is greater than zero.

```python
arbitrage_spread_early = coinbase['Close'].loc['2018-01-16'] - bitstamp['Close'].loc['2018-01-16']
arbitrage_spread_middle = coinbase['Close'].loc['2018-02-24'] - bitstamp['Close'].loc['2018-02-24']
arbitrage_spread_late = coinbase['Close'].loc['2018-03-26'] - bitstamp['Close'].loc['2018-03-26']

display(arbitrage_spread_early[arbitrage_spread_early>0].describe())
display(arbitrage_spread_middle[arbitrage_spread_middle>0].describe())
display(arbitrage_spread_late[arbitrage_spread_late>0].describe())
```

2. For each of the three dates, we calculated the spread returns. 

```python
spread_return_early = arbitrage_spread_early[arbitrage_spread_early>0] / bitstamp['Close'].loc['2018-01-16']
spread_return_middle = arbitrage_spread_middle[arbitrage_spread_middle>0] / bitstamp['Close'].loc['2018-02-24']
spread_return_late = arbitrage_spread_late[arbitrage_spread_late>0] / bitstamp['Close'].loc['2018-03-26']

display(spread_return_early.head())
display(spread_return_middle.head())
display(spread_return_late.head())      
```

3. For each of the three dates, we narrowed down our trading opportunities even further. To do so, we determined the number of times our trades with positive returns exceed the 1% minimum threshold needed to cover our costs. 

```python
profitable_trades_early = spread_return_early[spread_return_early > .01]
profitable_trades_middle = spread_return_middle[spread_return_middle > .01]
profitable_trades_late = spread_return_late[spread_return_late > .01]

# Review the first five profitable trades
display(profitable_trades_early.head())
display(profitable_trades_middle.head())
display(profitable_trades_late.head())
```

---
## Contributors

Brought to you by Wilson Rosa. https://www.linkedin.com/in/wilson-rosa-angeles/.

---
## License

MIT
