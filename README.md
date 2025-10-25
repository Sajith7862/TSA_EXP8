# Ex.No: 08     MOVINTG AVERAGE MODEL AND EXPONENTIAL SMOOTHING
### Date: 
### NAME:MOHAMED HAMEEM SAJITH J
### REG_NO:212223240090

### AIM:
To implement Moving Average Model and Exponential smoothing Using Python.
### ALGORITHM:
1. Import necessary libraries
2. Read the electricity time series data from a CSV file,Display the shape and the first 20 rows of
the dataset
3. Set the figure size for plots
4. Suppress warnings
5. Plot the first 50 values of the 'Value' column
6. Perform rolling average transformation with a window size of 5
7. Display the first 10 values of the rolling mean
8. Perform rolling average transformation with a window size of 10
9. Create a new figure for plotting,Plot the original data and fitted value
10. Show the plot
11. Also perform exponential smoothing and plot the graph
### PROGRAM:
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.api import SimpleExpSmoothing

try:
    df_full = pd.read_csv('GlobalLandTemperaturesByCity.csv')
except FileNotFoundError:
    print("Error: 'GlobalLandTemperaturesByCity.csv' not found.")
    print("Please make sure the file is in the same directory as your script.")
    exit()

df_city = df_full[df_full['City'] == 'Ahmadabad'].copy()

df_city['Date'] = pd.to_datetime(df_city['dt'])
df_city = df_city.set_index('Date')

df = df_city[['AverageTemperature']]

df = df.dropna()
data_monthly = df['AverageTemperature'].resample('MS').mean()

data_monthly = data_monthly.ffill()

data = pd.DataFrame({'Temp': data_monthly})

print("Shape of the filtered dataset:", data.shape)
print("FIRST 10 ROWS of the dataset:")
print(data.head(10))
print("\n")

data['MA_5'] = data['Temp'].rolling(window=5).mean()
data['MA_10'] = data['Temp'].rolling(window=10).mean()

print("ROLLING MEAN (WINDOW 5) - FIRST 10 ROWS:")
print(data.head(10))
print("\n")

print("ROLLING MEAN (WINDOW 10) - FIRST 20 ROWS:")
print(data.head(20))
print("\n")

print("Displaying Moving Average Plot...")
plt.figure(figsize=(12, 6))
plt.plot(data['Temp'].iloc[-200:], label='Original Temperature')
plt.plot(data['MA_5'].iloc[-200:], label='Moving Average (Window=5)', color='orange', linestyle='--')
plt.plot(data['MA_10'].iloc[-200:], label='Moving Average (Window=10)', color='red', linestyle='--')
plt.title('Moving Average Model (Last 200 Months)')
plt.xlabel('Date')
plt.ylabel('Temperature (C)')
plt.legend()
plt.grid(True)
plt.show()

print("Displaying Exponential Smoothing Plot...")

model_es = SimpleExpSmoothing(data['Temp'])

fit_es = model_es.fit(smoothing_level=0.2)
data['ExpSmoothing'] = fit_es.fittedvalues

plt.figure(figsize=(12, 6))
plt.plot(data['Temp'].iloc[-200:], label='Original Temperature')
plt.plot(data['ExpSmoothing'].iloc[-200:], label='Exponential Smoothing (alpha=0.2)', color='green', linestyle='--')
plt.title('Simple Exponential Smoothing (Last 200 Months)')
plt.xlabel('Date')
plt.ylabel('Temperature (C)')
plt.legend()
plt.grid(True)
plt.show()

print("Experiment 8 Complete.")
```
### OUTPUT  :

### Plot Transform Dataset :

# FIRST 10 ROWS:

<img width="406" height="676" alt="image" src="https://github.com/user-attachments/assets/f7f13df8-76f9-44b5-a5ee-8c3b8e1c5bf6" />

# FIRST 20 ROWS:

<img width="433" height="531" alt="image" src="https://github.com/user-attachments/assets/9de4535b-af46-4c22-b4ca-897e5bdf0107" />

### Moving Average :
<img width="1009" height="547" alt="image" src="https://github.com/user-attachments/assets/e1d6d42d-cce1-42de-a9ee-340233e52837" />


### Exponential Smoothing :

<img width="1009" height="547" alt="image" src="https://github.com/user-attachments/assets/0669928e-baaa-482d-a1a8-7b9aaf5305fd" />



### RESULT:
Thus we have successfully implemented the Moving Average Model and Exponential smoothing using python.
