# 📈 kbar

`kbar` 提供一個簡單的 `KBar` 類別，專門用來繪製金融 K 線圖（candlestick chart）。  
它是 [`mplfinance`](https://github.com/matplotlib/mplfinance) 的輕量級封裝，並額外支援：  

- 🔹 內建成交量副圖（volume）  
- 🔹 自訂技術指標副圖（例如 RSI、MACD）  
- 🔹 自訂疊圖（例如 SMA、均線）  
- 🔹 自動設定中文字型（避免亂碼）  

---

## 🚀 安裝
```bash
pip install kbar
```

---

## 🛠 匯入
```python
from kbar import KBar
# 或
import kbar
```

---

## 📊 使用教學

### 1️⃣ 準備股票價量資料（以 yfinance 為例）
```python
import yfinance as yf
df = yf.download('0050.TW', start='2024-07-01', end='2024-08-21', auto_adjust=False)

# 轉換為單層欄位名稱（避免多層索引）
df.columns = df.columns.map(lambda x: x[0])
```

---

### 2️⃣ 繪製簡單的 K 線圖
```python
kb = KBar(df)
# 或 kb = kbar.KBar(df) 若使用 import kbar
kb.plot()
```

---

### 3️⃣ 繪製含成交量的 K 線圖
```python
kb = KBar(df)
kb.plot(volume=True)  # 成交量副圖會顯示在 panel=1
```

---

### 4️⃣ 添加自訂副圖（例：RSI 指標）
```python
from talib.abstract import RSI

# Ta-Lib 需要欄位小寫
df.columns = [c.lower() for c in df.columns]

rsi = RSI(df)

kb = KBar(df)
kb.addplot(rsi, panel=2, ylabel='RSI')  # RSI 畫在 panel=2
kb.plot(volume=True)
```

---

### 5️⃣ 繪製內建均線（mav）
```python
kb = KBar(df)
kb.plot(mav=5)        # 繪製 5 日均線
# kb.plot(mav=[3,5,7]) # 繪製多條均線
```

---

### 6️⃣ 添加自訂疊圖（例：SMA 均線）
```python
from talib.abstract import SMA

df.columns = [c.lower() for c in df.columns]

sma3 = SMA(df['close'], timeperiod=3)
sma5 = SMA(df['close'], timeperiod=5)
sma7 = SMA(df['close'], timeperiod=7)

kb = KBar(df)
kb.addplot(sma3, panel=0, color='blue', width=1)
kb.addplot(sma5, panel=0, color='orange', width=1)
kb.addplot(sma7, panel=0, color='green', width=1)

kb.plot(volume=True)
```

---

## 📚 API 文件

### `class KBar`

封裝 `mplfinance`，提供簡化的 K 線圖繪製與副圖管理。

#### 🔹 建構子
```python
KBar(df)
```

- **df** (`pandas.DataFrame`)：必須包含以下欄位：  
  - `Open`: 開盤價  
  - `High`: 最高價  
  - `Low`: 最低價  
  - `Close`: 收盤價  
  - `Volume`（可選）：成交量  

---

#### 🔹 方法：`addplot(data, **kwargs)`
添加自訂副圖或疊圖。  

- **data** (`Series` 或 `ndarray`)：要繪製的資料  
- **kwargs**：傳遞給 `mplfinance.make_addplot()` 的參數，例如：  
  - `color`：線條顏色  
  - `width`：線條寬度  
  - `panel`：放置的圖表區（0=主圖，1~9=副圖）  
  - `ylabel`：副圖標籤  

---

#### 🔹 方法：`plot(**kwargs)`
繪製 K 線圖，可同時顯示副圖與疊圖。  

- **kwargs**：傳遞給 `mplfinance.plot()` 的參數，例如：  
  - `volume`：是否顯示成交量副圖  
  - `mav`：均線（整數或整數清單）  
  - `returnfig`：若 `True`，回傳 `(fig, axes)`  

---

## 📘 範例
```python
import yfinance as yf
from kbar import KBar

df = yf.download("0050.TW", start="2024-08-20", end="2025-01-20")
df.columns = df.columns.map(lambda x: x[0])

kb = KBar(df)
kb.addplot(df['Close'].rolling(5).mean(), color='blue', width=1)
kb.plot(volume=True, mav=5)
```

---

## 📦 依賴套件
- `mplfinance`
- `matplotlib`
- `pandas`
- `numpy`
- `pyarrow`（pandas 內部需要）

---

## 📝 授權
MIT License
