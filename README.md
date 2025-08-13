## 教學文件
###  安裝
```
pip install kbar
```
### 匯入類別
```
from kbar import KBar
# 或 import kbar
```
### 準備股票價量資料 (以 yfinance 為例)
```
import yfinance as yf
df=yf.download('0050.tw', start='2024-07-01', end='2024-08-21', auto_adjust=False)
df.columns=df.columns.map(lambda x: x[0])  # 改為單層欄位
```
### 繪製簡單的 K 線圖 (預設不顯示成交量副圖)
```
kb=KBar(df)
# 或 kb=kbar.KBar(df) 若使用 import kbar
kb.plot()
```
### 繪製有內建成交量副圖 (參數 volume) 之 K 線圖
```
kb=KBar(df)
# 或 kb=kbar.KBar(df) 若使用 import kbar
kb.plot(volume=True) # 成交量副圖預設佔用 panel=1
```
### 繪製有自訂副圖 (RSI 指標) 之 K 線圖 
```
from talib.abstract import RSI
kb=KBar(df)
# 將欄位名稱改為小寫 (Ta-Lib 的要求)
df.columns=[column.lower() for column in df.columns]
rsi=RSI(df) # 計算 RSI 指標
# 添加 RSI 副圖 (panel=1~9, 使用 panel=2)
kb.addplot(rsi, panel=2, ylabel='RSI') 
kb.plot(volume=True) # 成交量副圖預設佔用 panel=1
# 註 : 若 volume=False 則自訂副圖可使用 panel=1
```
### 繪製有內建均線疊圖 (參數 mav) 之 K 線圖
```
kb=KBar(df)
kb.plot(mav=5) # 繪製 5 日均線疊圖
# kb.plot(mav=[3, 5, 7]) # 繪製 3, 5, 7 日均線疊圖
# 註 : 疊圖不論幾條都佔用 panel=0
```
### 繪製有自訂疊圖 (SMA 指標) 之 K 線圖
```
from talib.abstract import SMA
kb=KBar(df)
# 將欄位名稱改為小寫 (Ta-Lib 的要求)
df.columns=[column.lower() for column in df.columns]
# 計算 SMA 指標
sma3=SMA(df['close'].values, timeperiod=3)
sma5=SMA(df['close'].values, timeperiod=5) 
sma7=SMA(df['close'].values, timeperiod=7)
# 添加 SMA 疊圖 (疊圖都在 panel=0)
kb.addplot(sma3, panel=0) # 3 日均線疊圖
kb.addplot(sma5, panel=0) # 5 日均線疊圖
kb.addplot(sma7, panel=0) # 7 日均線疊圖
kb.plot(volume=True) # 成交量副圖預設佔用 panel=1
# 註 : 疊圖不論幾條都佔用 panel=0
```

## Documentation
'kbar.py' provides a simple class 'KBar' for handling financial
candlestick chart plotting. This class is a wrapper around the
'mplfinance' library and supports additional features such as
adding auxiliary plots.
### Classes and Methods
#### Class: 'KBar'
The 'KBar' class simplifies the process of drawing candlestick
charts and supports custom styles and auxiliary plots.
##### Constructor
KBar(df)
- ##### Parameters: 
  - 'df' (pandas.DataFrame): A DataFrame containing financial data, which should include the following columns:
    - 'Open': Opening prices.
    - 'High': Highest prices.
    - 'Low': Lowest prices.
    - 'Close': Closing prices.
    - 'Volume' (optional): Trading volume.
---
##### Method: 'addplot(data, **kwargs)'
Adds auxiliary plots to the candlestick chart.

addplot(data, **kwargs)
- ##### Parameters:
  - 'data' (Series or ndarray): The data to be plotted.
  - '**kwargs': Arguments passed to 'mplfinance.make_addplot()',                including:
    - 'color': Line color.
    - 'width': Line width.
    - 'scatter': Whether to plot as a scatter plot.
    - 'markersize': Marker size for scatter plots.
    - 'marker': Marker style for scatter plots.
- ##### Functionality:
  - This method appends auxiliary plots to the 'KBar' object for
    display during plotting.    
---
##### Method: 'plot(**kwargs)'
Plots the candlestick chart with auxiliary plots and custom styles.

plot(**kwargs)
- ##### Parameters:
  - '**kwargs': Arguments passed to 'mplfinance.plot()'.
                Supported options include:
    - 'type': Chart type (fixed as 'candle').
    - 'style': Chart style (fixed as use 'Microsoft JhengHei' font).
    - 'volume': Whether to show the volume bar chart.
    - 'returnfig': If 'True', returns the 'Figure' object for the plot.
    - Other arguments supported by 'mplfinance.plot()'.

- ##### Functionality:
  - Draws a candlestick chart with auxiliary plots.
  - Returns the 'matplotlib.figure.Figure' and axes objects if
    'returnfig=True'.
---
### Example Usage
#### Inport classes, moduals, or packages
import yfinance as yf
from kbar import KBar
#### Download financial data
df = yf.download('0050.TW', start='2024-08-20', end='2025-01-20')
df.columns=df.columns.map(lambda x: x[0])  # single layer 
#### Initialize KBar
kb = KBar(df)
#### Add auxiliary plot
kb.addplot(df['Close'].rolling(window=5).mean(), color='blue', width=1)
#### Plot candlestick chart
kb.plot(volume=True)

---
### Dependencies
- 'mplfinance': Used for financial chart plotting.
- 'matplotlib': Underlying plotting library.
---
### More examples
- 
