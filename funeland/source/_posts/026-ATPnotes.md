---
title: Algorithmic Trading Using Python Notes
date: 2023-01-01
updated: 2023-01-03
tags: [quantitative finance]
categories: [tools]
description: study notes https://youtu.be/xfzGZB4HhEE.
---

## Section 1: Algorithmic Trading Fundamentals

Algorithmic trading means using computers to make investment decisions.

#### Algorithmic Trading Landscape

- Renaissance Technology
- AQR Capital Management
- Citadel Security

#### Algorithmic Trading and Python

- Python is most popular for algorithmic trading
- Python can be slow, call functions from libraries written in other languages (e.g., Numpy)

#### Algorithmic Trading Process

1. collect data
2. develop a hypothesis for a strategy
3. backtest that strategy 
4. implement the strategy in production

backtest: formulate the strategy, see how it would have performed historically over time
- take the strategy back as far in time as one can
- across as many markets as one can 
- example: hypothesis is that the largest firm outperforms. Should test this back in time in the U.S. and also in other countries such as Canada and Japan.
 
## Section 2: Course Configuration & API (Application Programming Interface) basics

API is a way to let your software interact with others' software. This class will use the IEX Cloud API (only GET requests).

- GET: receive data from API
- POST: push data to API
- PUT: add and overwrite data to API
- DELETE: delete data from API


[list of public APIs](https://github.com/public-apis/public-apis)

## Section 3: Building An Equal-Weight S&P 500 Index Fund

S&P 500 is a market capital weighted stock market index. Purpose of this project is to create an alternative version of S&P which does not weight capital of companies.

Read data using `stocks = pd.read_csv("")`.

Aquiring API data requires authentication. Create a `secrets.py` file to store API token. This file can be included in the `.gitignore` file so it would not be uploaded to github. Import API token using `from secrets import IEX_CLOUD_API_TOKEN`

Making the API call by using the following f-string. 

```Python
symbol = 'AAPL' 
api_url = f'https://sandbox.iexapis.com/stable/stock/{symbol}/quote/?token={IEX_CLOUD_API_TOKEN}' 
data = requests.get(api_url).json() # a response object
data.status_code
```

Data can be accessed using python string such as `market_cap = data['marketCap']`. A pandas data can be made using the following. Pandas series is different from pandas dataframe as dataframes has columns and rows but serez

```Python
my_columns = ['Ticker', 'Price','Market Capitalization', 'Number Of Shares to Buy']
final_dataframe = pd.DataFrame(columns = my_columns)
final_dataframe = final_dataframe.append(
                                        pd.Series(['AAPL', 
                                                   data['latestPrice'], 
                                                   data['marketCap'], 
                                                   'N/A'], 
                                                  index = my_columns), 
                                        ignore_index = True)
```


Executing requests in python is slow, so looping is not recommended. Batch requests can be used. First split the pandas dataframe into sublists.

```Python
# Function sourced from 
# https://stackoverflow.com/questions/312443/how-do-you-split-a-list-into-evenly-sized-chunks
def chunks(lst, n):
    """Yield successive n-sized chunks from lst."""
    for i in range(0, len(lst), n):
        yield lst[i:i + n]

symbol_groups = list(chunks(stocks['Ticker'], 100))
symbol_strings = []
for i in range(0, len(symbol_groups)):
    symbol_strings.append(','.join(symbol_groups[i]))

```

Then batch api calls can be performed using requests and the `symbol_strings`.

Then do calculations.
- position: total profolio size (total money)/length of list or number of all stocks






