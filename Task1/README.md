# Machine Data Analysis – Cyclone Sensor Data

## 📌 Project Overview
This project performs a **comprehensive data science workflow** on 3 years of cyclone machine sensor data (~370,000 records at 5-minute intervals) aimed at delivering actionable insights for preventive maintenance, anomaly detection, and operational forecasting.

**Key objectives include:**
- Identifying **shutdown and idle periods** of the machine
- Segmenting **machine operational states via clustering**
- Detecting **contextual anomalies** within operational states
- Conducting **short-term forecasting** of critical sensor parameter *Cyclone_Inlet_Gas_Temp*

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
- Utilizes **sensor-specific 5th percentile thresholds** instead of fixed values to adapt to sensor variability.
- **Ignores short shutdowns (<10 minutes)** to reduce noise caused by minimal fluctuations or sensor lag.
- Ensures detection of **meaningful and sustained shutdowns or idle periods** for accurate machine downtime representation.

### Why ignore shutdowns shorter than 10 minutes?
- Minor operational fluctuations or sensor noise often mimic shutdowns.
- Including these falsely inflates idle time and complicates maintenance scheduling.
- Filtering generates cleaner and more actionable shutdown event logs.

### Machine State Clustering
- Implements **KMeans clustering** for segmentation of active machine operational states.
- Excludes shutdown data to focus on active behavior patterns.
- Produces interpretable clusters that represent different machine states such as Normal, Startup/Shutdown, High Load, and Degraded.
- Outputs cluster statistics covering mean, standard deviation, and occurrence frequency.

### Contextual Anomaly Detection
- Applies **Isolation Forest models separately on each cluster** for precise anomaly identification sensitive to operational context.
- Consolidates anomalous events with timestamps, sensor readings, and cluster labels for further investigation.
- Enables targeted root cause exploration and informed preventive measures.

### Short-Term Forecasting
- Focused on forecasting **Cyclone_Inlet_Gas_Temp** for the next hour (12 steps ahead).
- Compares:
  - Baseline persistence model (last known value)
  - ARIMA time series forecasting model (order (5,1,0))
- Evaluated using **RMSE (Root Mean Square Error)** and **MAE (Mean Absolute Error)**, with all predictions saved.
- Visualization highlights predictive performance and model comparison.

### Visualizations
- Sensor correlation heatmap to reveal interdependencies.
- Time series plots for one week and one year showing typical sensor behavior.
- Annual shutdown timeline annotated with strict (80%) and relaxed (60%) detection thresholds.
- Forecast comparison plot presenting actual vs predicted temperature trends.

---

## 🛠️ Setup Instructions

1. **Clone or navigate to the project directory**  
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


5. **Review results**  
- Output CSV files will be saved in `Task1/outputs/`  
- Plot images will be saved in `Task1/plots/`

---

## 📊 Generated Outputs

| File                      | Description                                 |
|---------------------------|---------------------------------------------|
| summary_statistics.csv    | Descriptive statistics of all sensor data  |
| shutdown_periods.csv      | Shutdown and idle event timestamps & duration |
| clusters_summary.csv      | Statistics and behavior summary per cluster |
| anomalous_periods.csv     | Detected anomalies with contextual info     |
| forecasts.csv             | Actual vs predicted Cyclone_Inlet_Gas_Temp |

Plots include:  
`correlation_matrix.png`, `one_week.png`, `one_year.png`, `shutdowns_year.png`, `forecast_comparison.png`

---

## 📝 Insights & Recommendations

- Clustering provides **valuable context** into varying operating modes for targeted monitoring.
- Dynamic shutdown detection approach improves **accuracy of idle period quantification**.
- Contextual anomaly detection supports **early fault alerts** tailored to machine states.
- Forecasting allows better **short-term operational planning** for temperature-sensitive components.

---

## 🔮 Future Work

- Develop **real-time anomaly detection and alerting systems**.
- Integrate **predictive maintenance triggers** based on model outputs.
- Explore more advanced forecasting models such as **LSTM or Prophet** for longer horizons.
- Build an **interactive dashboard** to visualize clusters, anomalies, shutdowns, and forecasts.
- Extend methodology to **other turbines or cyclone machinery** for wider applicability.

---

## 📞 Support & Contributions

For issues or contributions, please open an issue or pull request in this repository.

---

*This project aims to deliver a robust monitoring and forecasting framework to improve cyclone machine efficiency, reliability, and maintenance strategy.*
