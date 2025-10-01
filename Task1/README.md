# Machine Data Analysis – Cyclone Sensor Data


## 📌 Project Overview
This project performs a **comprehensive data science workflow** on 3 years of cyclone machine sensor data (~370,000 records at 5-minute intervals). The analysis aims to deliver actionable insights for **preventive maintenance, anomaly detection, and operational forecasting**, helping optimize turbine efficiency and reliability.

**Key objectives:**
- Identify **shutdown and idle periods** of the machine.
- Segment **machine operational states via clustering**.
- Detect **contextual anomalies** within operational states.
- Conduct **short-term forecasting** of critical sensor parameter *Cyclone_Inlet_Gas_Temp*.

**Why this matters:**  
Accurate detection of operational patterns, anomalies, and forecasts enables informed maintenance decisions, reduces unplanned downtime, and enhances turbine performance.


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

## ⚡ Key Features

### Shutdown Detection
- Uses **sensor-specific 5th percentile thresholds** for adaptive detection.
- Ignores **short shutdowns (<10 minutes)** to reduce noise.
- Ensures detection of **meaningful shutdowns or idle periods** for accurate downtime representation.
- **Potential Improvements:**  
  - Apply **noise-cancellation or signal smoothing techniques** (e.g., moving average, Kalman filter) to reduce false shutdowns caused by sensor fluctuations.
  - Implement **dynamic threshold adjustment** based on operational context for more precise detection.

### Machine State Clustering
- Implements **KMeans clustering** to segment active machine operational states.
- Excludes shutdown periods to focus on active behavior.
- Produces clusters representing states such as Normal, Startup/Shutdown, High Load, and Degraded.
- Outputs cluster statistics covering mean, standard deviation, and occurrence frequency.
- **Portfolio Highlight:** Clear visualization of machine states helps non-technical stakeholders understand operational behavior.

### Contextual Anomaly Detection
- Uses **Isolation Forest models per cluster** for precise anomaly identification.
- Consolidates anomalies with timestamps, sensor readings, and cluster labels.
- Supports targeted **root cause analysis** and preventive measures.
- **Potential Improvements:** Combine anomaly detection with **noise reduction and clustering refinement** for higher accuracy.

### Short-Term Forecasting
- Forecasts **Cyclone_Inlet_Gas_Temp** for the next hour (12 steps ahead).
- Models compared:
  - Baseline persistence model (last known value)
  - ARIMA time series forecasting (order (5,1,0))
- Evaluated using **RMSE** and **MAE**, with predictions saved.
- Visualization highlights predictive performance and model comparison.
- **Potential Improvements:**  
  - Use **Auto-ARIMA** to automatically select optimal parameters, improving forecast accuracy without manual tuning.  
  - Explore hybrid models combining ARIMA with machine learning for better short-term predictions.

### Visualizations
- Sensor correlation heatmap to reveal interdependencies.
- Time series plots for one week and one year showing typical sensor behavior.
- Annual shutdown timeline annotated with strict (80%) and relaxed (60%) thresholds.
- Forecast comparison plot of actual vs predicted temperature trends.
- **Portfolio Standout:** Visualizations convey complex patterns in an easy-to-understand, story-like format.


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
   # Jupyter Notebook
      ```
      jupyter notebook task1_analysis.ipynb
      ```
   # OR Python script
   ```
   python task1_analysis.py
   ```
5. **Check results**  
   - Outputs (`.csv` files) will be in `Task1/outputs/`  
   - Plots (`.png` files) will be in `Task1/plots/`



5. **Review results**  
- Output CSV files will be saved in `Task1/outputs/`  
- Plot images will be saved in `Task1/plots/`


## 📊 Generated Outputs

| File                   | Description                                   |
| ---------------------- | --------------------------------------------- |
| summary_statistics.csv | Descriptive statistics of all sensor data     |
| shutdown_periods.csv   | Shutdown and idle event timestamps & duration |
| clusters_summary.csv   | Statistics and behavior summary per cluster   |
| anomalous_periods.csv  | Detected anomalies with contextual info       |
| forecasts.csv          | Actual vs predicted Cyclone_Inlet_Gas_Temp    |

Plots include:  
`correlation_matrix.png`, `one_week.png`, `one_year.png`, `shutdowns_year.png`, `forecast_comparison.png`


📝 Insights & Recommendations

Clustering provides valuable context into varying operating modes.

Dynamic shutdown detection improves accuracy of idle period quantification.

Contextual anomaly detection supports early fault alerts tailored to machine states.

Forecasting enables short-term operational planning for temperature-sensitive components.

Standout Note: Integration of noise-cancellation, Auto-ARIMA, and clear visualizations highlights the practical and technical value of this analysis for stakeholders.


## 🔮 Future Work

Develop real-time anomaly detection and alerting systems.

Integrate predictive maintenance triggers based on model outputs.

Explore advanced forecasting models such as Auto-ARIMA, LSTM, or Prophet for longer horizons and improved accuracy.

Apply noise-cancellation or signal-smoothing techniques to improve shutdown and anomaly detection reliability.

Build an interactive dashboard to visualize clusters, anomalies, shutdowns, and forecasts.

Extend methodology to other turbines or cyclone machinery for wider applicability.

Investigate hybrid modeling approaches combining time series and machine learning for enhanced predictive power.


## 📞 Support & Contributions

For issues or contributions, please open an issue or pull request in this repository.


*This project aims to deliver a robust monitoring and forecasting framework to improve cyclone machine efficiency, reliability, and maintenance strategy.*
