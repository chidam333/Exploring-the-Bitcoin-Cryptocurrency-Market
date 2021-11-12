```python
# Importing pandas
import pandas as pd

# Importing matplotlib and setting aesthetics for plotting later.
import matplotlib.pyplot as plt
%matplotlib inline
%config InlineBackend.figure_format = 'svg' 
plt.style.use('fivethirtyeight')

# Reading datasets/coinmarketcap_06122017.csv into pandas
dec6 = pd.read_csv('coinmarketcap_06122017.csv')
# Selecting the 'id' and the 'market_cap_usd' columns
market_cap_raw = dec6[['id','market_cap_usd']]

# Counting the number of values
market_cap_raw.count()
# ... YOUR CODE FOR TASK 2 ...
```




    id                1326
    market_cap_usd    1031
    dtype: int64




```python
# Filtering out rows without a market capitalization
cap = market_cap_raw.query('market_cap_usd>0')

# Counting the number of values again
market_cap_raw.count()
cap.count()
# ... YOUR CODE FOR TASK 3 ...
```




    id                1031
    market_cap_usd    1031
    dtype: int64




```python
#Declaring these now for later use in the plots
#TOP_CAP_TITLE = 'Top 10 market capitalization'
#TOP_CAP_YLABEL = ' od total cap '

# Selecting the first 10 rows and setting the index
cap10 = cap.head(10)
cap10.set_index('id',inplace=True)
# Calculating market_cap_perc
cap10['market_cap_perc'] =(cap10['market_cap_usd']/cap['market_cap_usd'].sum())*100
print(cap10['market_cap_perc'])

# Plotting the barplot with the title defined above 
ax =cap10.plot.bar(y="market_cap_perc",title='Top 10 market capitalization',rot=60,fontsize='xx-small')

# Annotating the y axis with the label defined above
ax.set_ylabel('% od total cap')
# ... YOUR CODE FOR TASK 4 ...
```

    id
    bitcoin         56.918669
    ethereum        11.629410
    bitcoin-cash     6.758088
    iota             3.941238
    ripple           2.502063
    dash             1.547956
    litecoin         1.505323
    bitcoin-gold     1.314454
    monero           1.157262
    cardano          0.863312
    Name: market_cap_perc, dtype: float64
    

    C:\Users\dell\AppData\Local\Temp/ipykernel_13232/777314912.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      cap10['market_cap_perc'] =(cap10['market_cap_usd']/cap['market_cap_usd'].sum())*100
    




    Text(0, 0.5, '% od total cap')




    
![svg](output_2_3.svg)
    



```python
# Colors for the bar plot
COLORS = ['orange', 'green', 'orange', 'cyan', 'cyan', 'blue', 'silver', 'orange', 'red', 'green']

# Plotting market_cap_usd as before but adding the colors and scaling the y-axis  
ax = cap10.plot.bar(y='market_cap_perc',title='Top 10 market capitalization',rot=60,fontsize='xx-small',logy=True)#,colors=COLORS

# Annotating the y axis with 'USD'
ax.set_ylabel('USD')
# ... YOUR CODE FOR TASK 5 ...

# Final touch! Removing the xlabel as it is not very informative
ax.set(xlabel="")
# ... YOUR CODE FOR TASK 5 ...
```




    [Text(0.5, 0, '')]




    
![svg](output_3_1.svg)
    



```python
# Selecting the id, percent_change_24h and percent_change_7d columns
volatility = dec6[['id','percent_change_24h','percent_change_7d']]

# Setting the index to 'id' and dropping all NaN rows
volatility = volatility.set_index('id').dropna()

# Sorting the DataFrame by percent_change_24h in ascending order
volatility = volatility.sort_values(by=['percent_change_24h'])

# Checking the first few rows
volatility.head()
# ... YOUR CODE FOR TASK 6 ...
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>percent_change_24h</th>
      <th>percent_change_7d</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>flappycoin</th>
      <td>-95.85</td>
      <td>-96.61</td>
    </tr>
    <tr>
      <th>credence-coin</th>
      <td>-94.22</td>
      <td>-95.31</td>
    </tr>
    <tr>
      <th>coupecoin</th>
      <td>-93.93</td>
      <td>-61.24</td>
    </tr>
    <tr>
      <th>tyrocoin</th>
      <td>-79.02</td>
      <td>-87.43</td>
    </tr>
    <tr>
      <th>petrodollar</th>
      <td>-76.55</td>
      <td>542.96</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Defining a function with 2 parameters, the series to plot and the title
def top10_subplot(volatility_series, title):
    # Making the subplot and the figure for two side by side plots
    fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(10, 6))
    
    # Plotting with pandas the barchart for the top 10 losers
    ax = volatility_series[0:10].plot.bar(ax=axes[0],rot=60)
    
    # Setting the figure's main title to the text passed as parameter
    fig.suptitle(title)
    # ... YOUR CODE FOR TASK 7 ...
    
    # Setting the ylabel to '% change'
    ax.set_ylabel("% change")
    
    # ... YOUR CODE FOR TASK 7 ...
    
    # Same as above, but for the top 10 winners
    ax =  volatility_series[-10:].plot.bar(ax=axes[1])
    
    # Returning this for good practice, might use later
    return fig, ax

DTITLE = "24 hours top losers and winners"
#DTITLE1 = "7 days top losers and winners"
# Calling the function above with the 24 hours period series and title DTITLE  
fig, ax = top10_subplot(volatility.percent_change_24h,DTITLE)
#fig, ax = top10_subplot(volatility.percent_change_7d,DTITLE1)
```


    
![svg](output_5_0.svg)
    



```python
# Sorting in ascending order
volatility7d = volatility.sort_values(by=['percent_change_7d'])

WTITLE = "Weekly top losers and winners"

# Calling the top10_subplot function
fig, ax = top10_subplot(volatility7d.percent_change_7d,WTITLE)
```


    
![svg](output_6_0.svg)
    



```python
# Selecting everything bigger than 10 billion 
largecaps = cap.query("market_cap_usd>10000000000")

# Printing out largecaps
print(largecaps)
# ... YOUR CODE FOR TASK 9 ...
```

                 id  market_cap_usd
    0       bitcoin    2.130493e+11
    1      ethereum    4.352945e+10
    2  bitcoin-cash    2.529585e+10
    3          iota    1.475225e+10
    


```python
# Making a nice function for counting different marketcaps from the
# "cap" DataFrame. Returns an int.
# INSTRUCTORS NOTE: Since you made it to the end, consider it a gift :D
def capcount(query_string):
    return cap.query(query_string).count().id

# Labels for the plot
LABELS = ["biggish", "micro", "nano"]

# Using capcount count the biggish cryptos
biggish = capcount("market_cap_usd>300000000");

# Same as above for micro ...
micro = capcount("market_cap_usd>50000000 and market_cap_usd<300000000");

# ... and for nano
nano = capcount("market_cap_usd<50000000");

# Making a list with the 3 counts
values =[biggish,micro,nano]
df = pd.DataFrame({'count': values }, index=LABELS)
# Plotting them with matplotlib
plt.bar(LABELS,values)

# ... YOUR CODE FOR TASK 10 ...
```




    <BarContainer object of 3 artists>




    
![svg](output_8_1.svg)
    



```python

```


```python

```
