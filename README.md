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
<img width="318" alt="image" src="https://github.com/user-attachments/assets/1199fc29-58e5-4680-b083-b6f1f8b02365" />

## 2. Purpose : calculates the percentage of available (t) and unavailable (f) dates.
```diff
df['available'].value_counts(normalize=True)* 100
```

## 3. Let`s Count the busiest day!
```diff
busiest = df[df['available']=='f']['date'].value_counts()
print(f'Busiest Dates: {busiest.head()}')
```
## 4. Plot a bar graph to show availability percentage
```diff
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="569" alt="image" src="https://github.com/user-attachments/assets/088d2d48-61ac-4878-b0d0-3a34ca8b93bf" />

## 5. Plot the busiest day
```diff
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="586" alt="image" src="https://github.com/user-attachments/assets/7913f297-fa98-431f-bb26-e9c542e1ed3f" />

## 📄 Download listings dataset of Hawaii from
https://data.insideairbnb.com/italy/lombardy/milan/2024-09-17/visualisations/listings.csv
<img width="586" alt="image" src="https://github.com/user-attachments/assets/01973be3-f876-46d2-93b5-6aab3ca75bb7" />

## ✅ Room type and price is given seperately
Let`s Combine and visualize them
```diff
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="569" alt="image" src="https://github.com/user-attachments/assets/c85ff8f8-f39f-4fc4-aa97-12bec88982bd" />

## ✅ Which are the top 10 neighborhoods with the most listings?
```diff
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="591" alt="Screenshot 1446-08-03 at 12 15 49 PM" src="https://github.com/user-attachments/assets/18012e94-941a-490d-84e5-84a28b091eab" />

## ✅ Geographical Distribution of Listings (Price Colored)
```diff
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="863" alt="Screenshot 1446-08-03 at 12 16 59 PM" src="https://github.com/user-attachments/assets/111cc2a2-c62e-485e-9cba-1ceb5deaa7d2" />

## Let us see the listings on a real map
High Density: The areas in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas.
Popular Locations: These regions might be more popular or in high demand. They could be near tourist attractions, iconic neighborhoods, or central areas in Milan where people tend to stay more often, such as near the Duomo, Galleria Vittorio Emanuele II, or Navigli.

Colder Areas (Green/Blue):

Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available.
Less Popular Locations: These areas might be less popular or further from key attractions. Lower density could imply more affordable areas or less tourist traffic, which could suit those seeking quieter, less crowded places.

Density Patterns:

Clustered Areas: Clusters of heatmap intensity represent hotspots. These may correspond to high-traffic areas such as the city center, popular piazzas, or cultural landmarks like the Sforza Castle or La Scala.
Spread-Out Listings: A more uniform distribution on the heatmap suggests that listings are spread across Milan, reflecting a more balanced demand for rentals in various neighborhoods.
```diff
import folium
from folium.plugins import HeatMap
import pandas as pd


milan_data = listings[['latitude', 'longitude', 'price']]


m = folium.Map(location=[45.470078, 9.181165], zoom_start=12)


heat_data = [[row['latitude'], row['longitude']] for index, row in milan_data.iterrows()]


HeatMap(heat_data).add_to(m)


m.save('milan_heatmap.html')


m
```
<img width="1012" alt="Screenshot 1446-08-03 at 11 45 16 AM" src="https://github.com/user-attachments/assets/880f323a-3ab5-47aa-b8ff-38286ea565b5" />

## Which day had the fewest unavailable listings
```diff
least_busy = df[df['available']=='f']['date'].value_counts().sort_values(ascending=True)
print(f'Least Busy Dates: {least_busy.head()}')
```
<img width="301" alt="image" src="https://github.com/user-attachments/assets/e93a6780-026f-4516-b225-4419fd1eca17" />

## How does the trend of available listings change over time?
```diff
import matplotlib.pyplot as plt

# Filter for available listings, count them by date, and sort by date for a proper time series
daily_available = df[df['available']=='t']['date'].value_counts().sort_index()

plt.figure(figsize=(10,5))
plt.plot(daily_available.index, daily_available.values, marker='o', linestyle='-', color='blue')
plt.title('Daily Trend of Available Listings')
plt.xlabel('Date')
plt.ylabel('Count of Available Listings')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
<img width="1005" alt="Screenshot 1446-08-03 at 11 51 45 AM" src="https://github.com/user-attachments/assets/a8658e9d-9948-47e9-9d38-d6e38777ced1" />
