# 🚖 NYC Taxi Report: Operational Insights Dashboard
<img width="1268" height="717" alt="image" src="https://github.com/user-attachments/assets/b6c4b422-e90f-4640-8d15-5b75c6d63997" />



## 📊 Project Overview
This Power BI project provides a comprehensive analysis of the New York City Yellow Taxi fleet, processing a massive dataset of **12.21 Million trips** from March 2016. The report is designed as a "Command Center" for fleet managers to monitor operational efficiency, revenue streams, and traffic patterns in real-time.

By visualizing over 12 million data points, this dashboard uncovers critical insights into passenger behavior, payment preferences, and the correlation between trip distance and fare amount.

* **Tools Used:** Power BI, DAX, Power Query
* **Data Volume:** 12.21 Million Rows
* **Key Focus:** Operational Analytics & Big Data Performance Optimization

---

## 💾 Data Source
The dataset used is the official NYC Yellow Taxi Trip records for March 2016.
* **Dataset Link:** [NYC Yellow Taxi Trip Data (Kaggle)](https://www.kaggle.com/datasets/elemento/nyc-yellow-taxi-trip-data?select=yellow_tripdata_2016-03.csv)
* **File Used:** `yellow_tripdata_2016-03.csv`

---

## 📈 Dashboard Highlights & Insights

### 1. Key Performance Indicators (KPIs)
The top-level cards provide an instant snapshot of fleet performance:
* **Total Trips:** 12.21 Million
* **Total Revenue:** $195.93 Million
* **Avg Fare:** $16.05
* **Avg Trip Distance:** 6.13 miles
* **Avg Trip Duration:** 15.6 Minutes (0.26 Hours)

### 2. Distance vs. Fare Correlation (Scatter Plot)
* **Visual:** A high-density scatter plot analyzing the relationship between `Trip Distance` and `Fare Amount`.
* **Insight:**
    * **Standard Metered Trips:** Visible as a dense diagonal cluster (Linear correlation).
    * **Fixed Rate Trips:** Distinct horizontal lines appearing at fixed fare amounts (e.g., $52 for JFK Airport trips), regardless of distance.
    * **Outliers:** Identified and filtered operational errors where distance was recorded but fare was negligible, or vice versa.

### 3. Trip Volume & Distance Analysis
* **Donut Chart:** Break down of trips by distance categories.
    * **Finding:** The majority of trips (**~57%**) are **Short Trips (< 2 miles)**, indicating high usage for "last-mile" connectivity within the city grid.
    * **Medium Trips (2-5 mi):** Account for ~28% of the volume.

### 4. Temporal Patterns (Rush Hour Analysis)
* **Line Chart & Bar Charts:** Analyzes traffic volume by `Day of Week` and `Hour of Day`.
* **Insight:**
    * Demand peaks significantly during evening hours (6 PM - 9 PM).
    * "Morning" and "Evening" shifts show distinct crossover patterns in trip volume, helping in driver shift planning.

### 5. Payment Behavior
* **Bar Chart:** Comparison of payment methods.
* **Insight:** Credit Card usage is dominant, driving a significant portion of the revenue, which correlates with higher tip amounts compared to cash transactions.

---

## 🛠️ Data Engineering & DAX Formulas

To handle the scale of 12 million rows effectively, specific calculations and performance optimizations were implemented.

### 1. Time & Duration Logic
Extracting granular time buckets for trend analysis.
```dax
Pickup_Hour = HOUR('yellow_tripdata_2016-03'[tpep_pickup_time])

Time_Segment = SWITCH(
    TRUE(),
    'yellow_tripdata_2016-03'[Pickup_Hour] >= 5 && 'yellow_tripdata_2016-03'[Pickup_Hour] < 12, "Morning",
    'yellow_tripdata_2016-03'[Pickup_Hour] >= 12 && 'yellow_tripdata_2016-03'[Pickup_Hour] < 17, "Afternoon",
    'yellow_tripdata_2016-03'[Pickup_Hour] >= 17 && 'yellow_tripdata_2016-03'[Pickup_Hour] < 21, "Evening",
    "Night"
)

Duration_Hours = DATEDIFF('yellow_tripdata_2016-03'[tpep_pickup_datetime], 'yellow_tripdata_2016-03'[tpep_dropoff_datetime], MINUTE) / 60
```

### 2. Grouping & Binning

Categorizing continuous variables to simplify visualizations (e.g., for the Donut Chart).
```dax
Distance_Group = SWITCH(
    TRUE(),
    'yellow_tripdata_2016-03'[trip_distance] < 2, "Short (< 2 mi)",
    'yellow_tripdata_2016-03'[trip_distance] < 5, "Medium (2-5 mi)",
    'yellow_tripdata_2016-03'[trip_distance] < 10, "Long (5-10 mi)",
    "Very Long (> 10 mi)"
)
```

### 3. Measures

Dynamic calculations for weighted averages.
```dax
Avg Speed (mph) = DIVIDE(SUM('yellow_tripdata_2016-03'[trip_distance]), SUM('yellow_tripdata_2016-03'[Duration_Hours]))
```

---

## 🚀 Optimization Strategy (Handling 12M+ Rows)

Rendering a dashboard with 12 million rows requires strategic performance tuning:

1. **Top N Filtering:** Applied to the "Distance vs. Fare" Scatter Plot to visualize only the top 5,000 trips by revenue. This prevents browser crashes while still accurately representing the data distribution and outliers.
2. **Data Cleaning:**
* Filtered out trips with `Trip Distance <= 0.1` miles (System errors).
* Removed `Fare Amount < 0` (Refunds/Disputes).
* Excluded extreme outliers (`Trip Distance > 100 miles`) to maintain scale in charts.

---

## 📬 Contact

* **Author:** Shabd Kumar
