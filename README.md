# 🌊 Prediction of Ocean Salinity and Ocean Temperature

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Framework](https://img.shields.io/badge/Framework-TensorFlow%20%7C%20Keras%20%7C%20LightGBM-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📌 Overview

This project presents a novel, high-accuracy forecasting system for oceanographic parameters (Salinity and Temperature) across the entire water column (1m to 500m).

Unlike traditional "one-size-fits-all" models, we developed a **Hybrid "Team of Specialists" Architecture**. This system intelligently routes data to different specialized models based on the physical regime of the ocean layer:
* **Surface Layer (1m-5m):** Modeled by a **Gated Recurrent Unit (GRU)** to capture chaotic, weather-driven temporal dynamics.
* **Deep Layer (10m-500m):** Modeled by a **Light Gradient Boosting Machine (LightGBM)** to capture stable, physics-driven vertical structures.

The system fuses high-frequency in-situ buoy data from **NIOT** (National Institute of Ocean Technology) with global satellite reanalysis data from **Copernicus (ECMWF)** to achieve robust, multi-horizon forecasts up to 15 days in advance.

---

## 🎯 Key Objectives

* **Multi-Horizon Forecasting:** Predict salinity and temperature for 1 hour, 1 day, 3 days, 7 days, and 15 days into the future.
* **Full Depth Profile:** Provide accurate forecasts for all 11 depth levels (1m, 5m, 10m, 15m, 20m, 30m, 50m, 75m, 100m, 200m, 500m).
* **High Accuracy:** Achieve an R-squared ($R^2$) score of > 85% for key parameters.
* **Seasonal Robustness:** Ensure reliable performance across distinct seasons (Winter, Summer, Monsoon, Post-Monsoon).

---

## 🏗️ System Architecture

The project follows a modular pipeline architecture:

1.  **Data Ingestion:**
    * **In-Situ Data:** Time-series data from 3 NIOT buoys (`AD07`, `BD08`, `BD14`).
    * **External Data:** ERA5 Reanalysis data (SST, Wind, Precipitation, Pressure) from Copernicus.
2.  **Data Fusion & Preprocessing:**
    * Temporal resampling to hourly intervals.
    * Time-based interpolation to handle missing sensor data.
    * Fusion of buoy and satellite data based on timestamp and location.
3.  **Feature Engineering:**
    * Creation of temporal lag features and rolling means.
    * Engineering of **Vertical Gradient Features** ($\Delta T, \Delta S$) for deep-ocean stability modeling.
    * Generation of granular seasonal flags.
4.  **Hybrid Modeling ("Team of Specialists"):**
    * **Surface Specialist (GRU):** Trained on surface data.
    * **Deep Specialist (LightGBM):** Trained on transition and deep data.
5.  **Deployment:**
    * Interactive Streamlit Dashboard for real-time visualization.

---

## 📊 Results & Performance

### Overall Accuracy by Depth (1-Day Forecast)

Our hybrid approach successfully solved the "Performance Cliff" often seen in ocean modeling.

| Depth Profile | Salinity ($R^2$) | Temperature ($R^2$) | Model Used |
| :--- | :--- | :--- | :--- |
| **1m (Surface)** | **0.99** | **0.99** | GRU |
| **5m (Surface)** | **0.99** | **0.99** | GRU |
| **10m - 75m** | 0.32 - 0.88 | 0.35 - 0.85 | LightGBM |
| **100m (Deep)** | 0.52 | **0.87** | LightGBM |
| **200m (Deep)** | N/A | **0.83** | LightGBM |

> **Key Finding:** The Surface Model achieved exceptional accuracy (>99%). The Deep Model successfully predicted temperature (>83%) but identified deep salinity as a complex challenge requiring further research (e.g., Ocean Current data).

---

## 🚀 How to Run the Project

### Prerequisites
* Python 3.8+
* TensorFlow
* LightGBM
* Streamlit
* Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn

### Installation
1.  Clone the repository
2.  Install dependencies

### Running the Dashboard
To launch the interactive forecasting dashboard:
```bash
streamlit run 11_Ocean_Forecast_Dashboard.py
