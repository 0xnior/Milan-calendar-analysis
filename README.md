# Milan-calendar-analysis
A Python project that uses the calendar module to analyze a Milan dataset, focusing on monthly and seasonal trends.
## For the following analysis we downlaod the real air bnb data from: 
https://data.insideairbnb.com/italy/lombardy/milan/2024-09-17/data/calendar.csv.gz
```diff
import pandas as pd
df = pd.read_csv('calendar.csv')
```
## 1. I want to know the number of available and unavailable
```diff
df['available'].value_counts()
```
## 2. Purpose : calculates the percentage of available (t) and unavailable (f) dates.
```diff
df['available'].value_counts(normalize=True)* 100
```

## 3. Let`s Count the busiest day!
```diff
busiest = df[df['available']=='f']['date'].value_counts()
print(f'Busiest Dates: {busiest.head()}')
```
