# Machine Data Analysis – Cyclone Sensor Data

## 📌 Project Overview
This project performs a **full data science workflow on 3 years of cyclone machine sensor data** to provide actionable insights for preventive maintenance, anomaly detection, and operational forecasting.  

Key objectives:
- Detect **shutdown/idle periods**
- Segment **machine operational states using clustering**
- Identify **contextual anomalies**
- Perform **short-term forecasting of Cyclone_Inlet_Gas_Temp**

---

## 📂 Folder Structure
```
Task1/
│
├── outputs/
│   ├── summary_statistics.csv          # Descriptive statistics of all sensors
│   ├── shutdown_periods.csv            # Detected shutdown/idle periods
│   ├── anomalous_periods.csv           # Contextual anomalies with sensor readings
│   ├── clusters_summary.csv            # Operational states cluster summary
│   └── forecasts.csv                   # True vs predicted Cyclone_Inlet_Gas_Temp
│
├── plots/
│   ├── correlation_matrix.png          # Sensor correlation heatmap
│   ├── one_week.png                    # Sample 1-week sensor trends
│   ├── one_year.png                    # Sample 1-year sensor trends
│   ├── shutdowns_year.png              # Shutdown periods highlighted
│   └── forecast_comparison.png         # Forecast vs actual comparison
│
└── task1_analysis.ipynb                   # Full analysis script
```

---

## ⚡ Key Features

### Shutdown Detection
- Uses **sensor-specific percentiles** instead of fixed thresholds.
- Avoids false negatives for idle periods.
- Option to ignore **short shutdowns (<10 min)** to remove noise.

   1️⃣ Why "ignore short shutdowns (<10 min)"?

   - In sensor data from industrial machines, there are often tiny dips or fluctuations in readings that look like a shutdown, but the machine is actually running.

   - These very short periods (<10 minutes) are usually noise — maybe a minor operational hiccup, sensor lag, or brief power fluctuation.

   - If we count these as real shutdowns, your analysis will overestimate idle periods and create false alerts, which is misleading for maintenance planning.

   - So, by ignoring short shutdowns, you:
   - ✅ Remove false positives
   - ✅ Highlight meaningful, real shutdowns
   - ✅ Make the results cleaner and more actionable

### Machine State Clustering
- **KMeans clustering** differentiates operational states.
- Produces interpretable statistics for each cluster.

### Contextual Anomaly Detection
- **Isolation Forest** applied within clusters for precision.
- Produces consolidated anomaly logs with timestamps and cluster info.

### Short-Term Forecasting
- Focus: **Cyclone_Inlet_Gas_Temp**
- Implements:
  - **Persistence model** (baseline)
  - **ARIMA model** (advanced)
- Evaluated using **RMSE & MAE**

### Visualizations
- Correlation heatmap
- Sample **1-week and 1-year** trends
- Shutdown highlights
- Forecast vs actual plots

---

## 🛠️ Setup Instructions

1. **Clone folder / move into directory**
   ```
   cd Task1
   ```

2. **Install dependencies**
   ```
   pip install -r requirements.txt
   ```

3. **Add dataset**  
   Place your `cyclone_data.xlsx` file in the same directory as `task1_analysis.py`.

4. **Run the analysis**
   ```
   python task1_analysis.py
   ```

5. **Check results**  
   - Outputs (`.csv` files) will be in `Task1/outputs/`  
   - Plots (`.png` files) will be in `Task1/plots/`  

---

## 📊 Generated Outputs

- **summary_statistics.csv** → Descriptive statistics of all sensors  
- **shutdown_periods.csv** → Start, end, duration of shutdowns  
- **clusters_summary.csv** → Cluster-level operational statistics  
- **anomalous_periods.csv** → Contextual anomalies with cluster info  
- **forecasts.csv** → Actual vs predicted Cyclone_Inlet_Gas_Temp  

Plots:
- `correlation_matrix.png`  
- `one_week.png`  
- `one_year.png`  
- `shutdowns_year.png`  
- `forecast_comparison.png`

---

## 📝 Insights & Recommendations
- Clustering provides **contextual understanding** of machine states.  
- **Dynamic shutdown detection** ensures realistic idle detection.  
- Anomaly detection enables **early fault identification**.  
- Short-term forecasting improves **operational planning**.
  
## This project delivers a **robust monitoring and forecasting framework** for cyclone machinery, helping improve **efficiency, reliability, and preventive maintenance planning**.

## 🔮 Future Work

- **Real-Time Anomaly Detection:** Extend the current analysis into a live monitoring system for early fault detection.  
- **Predictive Maintenance Alerts:** Integrate threshold-based and model-based alerts to notify operators before failures occur.  
- **Advanced Forecasting:** Apply more sophisticated models (LSTM, Prophet) for longer-term prediction of critical sensor values.  
- **Integration with Dashboard:** Visualize shutdowns, anomalies, clusters, and forecasts in an interactive dashboard for easier operational decision-making.  
- **Expansion to Other Machines:** Adapt the workflow to other turbine or cyclone machinery for generalized monitoring and predictive maintenance.  


### ⚠️ Note on Empty Shutdown Periods

- The `shutdown_periods.csv` file may appear empty after running the analysis.  
- **Reason:** There were **no shutdowns or idle periods detected** in the dataset according to the detection criteria (e.g., sensor thresholds, minimum duration).  
- You can adjust detection parameters in the script if you want to capture shorter or less prominent idle periods:
  - Lower the **percentile threshold** for sensor inactivity.
  - Reduce the **minimum shutdown duration** (currently set to ignore very short idle periods <10 min).


***