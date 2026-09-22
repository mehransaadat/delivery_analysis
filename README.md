# 🛵 Last-Mile Delivery Performance & Delay Analysis

**Python (Pandas, NumPy) · Tableau Public · Jupyter**

## Business Problem
Delivery times on our on-demand platform have surged over the past quarter and customer complaints are rising. Management needs to know whether the bottleneck is **kitchen prep delays, traffic/transit time, or peak hours**.

## 🔗 Live Dashboard
**[View the interactive dashboard on Tableau Public](PASTE_YOUR_TABLEAU_PUBLIC_LINK_HERE)**

![Dashboard screenshot](images/dashboard.png)


## Key Insights
1. **Transit, not the kitchen, is the bottleneck.** Transit accounts for ~63% of average delivery time (16.9 of 26.7 min). Delayed orders (>30 min) spend **15 more minutes in transit** than on-time orders, but only **0.4 more minutes in prep**. Transit time correlates 0.90 with total time; prep time only 0.10.
2. **Dinner Rush is the peak-hour hotspot.** 41% of orders are placed 17:00–21:00, and this window has a **43% delay rate** vs. 10% in the morning rush and 16% in the afternoon lull.
3. **Traffic and weather drive the delays.** Orders in *Jam* traffic are late **50%** of the time vs. **9%** in low traffic; *Cloudy* and *Fog* conditions show ~45% delays vs. 13% in sunny weather. Motorcycles have the highest delay rate (34%) vs. scooters (~26%) and electric scooters (~25%).

**Recommendation:** focus on transit — dynamic routing/ETA buffers during Jam traffic and dinner rush, more courier capacity in 17:00–21:00, and vehicle mix review — rather than kitchen-side fixes.

## Data Preparation (see notebook)
| Step | What was done |
|---|---|
| Raw data | 45,593 orders (Kaggle Food Delivery Dataset, `train.csv`) |
| Cleaning | Stripped whitespace, converted `'NaN'` strings to nulls, parsed dates/times, fixed midnight crossovers (831 orders) |
| Missing values | Dropped rows with no order time (3.8%); median/mode/`Unknown` for the rest |
| Invalid values | Removed orders where total time ≤ prep time (2,054); flagged placeholder (0,0) coordinates |
| Outliers | IQR rule on total & transit time (1.0% removed) |
| Features | `Prep_Time`, `Transit_Time`, `Total_Delivery_Time`, `Distance_km` (haversine), `Time_of_Day`, `Is_Peak`, `Is_Delayed` (SLA = 30 min) |
| Output | **41,372 clean rows** → `clean_delivery_data.csv` |

## Limitations
- The dataset has no delivery timestamp, so Transit Time = Total Time − Prep Time.
- Prep time only takes three values (5/10/15 min), which limits kitchen-side analysis.
- All bicycle orders lacked an order timestamp and were dropped.
- Festival and multi-delivery orders show ~100% delay rates on small samples — treat as directional.

## Repository
```
├── README.md
├── delivery_analysis.ipynb      # full cleaning + analysis
├── clean_delivery_data.csv      # Tableau-ready dataset
├── train.csv                    # raw data (Kaggle)
└── images/                      # charts + dashboard screenshot
```
