# 🌍 Air Quality Prediction (PM2.5) — ID2223 Project

This project predicts **PM2.5 air quality** for a given sensor location using **Hopsworks Feature Store**, **XGBoost**, and automated pipelines for feature ingestion, model training, and batch inference.

---

## 🏙️ 1. Sensor Location

We used **Beijing - Dongchengdongsi** as the target air quality monitoring station.

| Parameter | Value |
|------------|--------|
| **Country** | China 🇨🇳 |
| **City** | Beijing |
| **Street** | Dongchengdongsi |
| **AQICN URL** | `https://api.waqi.info/feed/@446` |

These parameters are defined in the `.env` file along with the `AQICN_API_KEY` and `HOPSWORKS_API_KEY`.

---

## 🧩 2. Grade ‘E’ Tasks — Base Implementation

For the **Grade E** tasks, we followed the provided notebook template and implemented all required pipelines.

### ✅ Feature Groups Created
| Feature Group | Version | Description |
|----------------|----------|--------------|
| **air_quality** | v1 | Historical PM2.5 data (from AQICN API) |
| **weather** | v1 | Historical and forecast weather data (from Open-Meteo API) |

### ✅ Feature View Created
| Feature View | Version | Description |
|----------------|----------|--------------|
| **air_quality_fv** | v1 | Joined `air_quality v1` and `weather v1` features for model training |

### ✅ Model Trained & Registered
| Model Name  | Description |
|-------------|--------------|
| **air_quality_xgboost_model**  | XGBoost trained on `air_quality_fv` (base features only) |

### 📊 Model Performance
| Metric | Value |
|--------|--------|
| **MSE** | 3568.29 |
| **R²** | −1.93 |

> The base model shows low accuracy, which is expected since no temporal dependencies were included.

---

## ⚙️ 3. Grade ‘C’ Tasks — Added Lag Features

To improve performance, we added **three new lagged PM2.5 features**:  
`pm25_lag1`, `pm25_lag2`, and `pm25_lag3`.

### ✅ New Feature Group & View
| Component | Version | Description |
|------------|----------|--------------|
| **air_quality** | v2 | Extended air quality data with lag features (`pm25_lag1`, `pm25_lag2`, `pm25_lag3`) |
| **weather** | v1 | Same as before |
| **air_quality_fv_v2** | v3 | Joined `air_quality v2` + `weather v1` (including lag features) |

### ✅ New Model Trained & Registered
| Model Name | Version | Description |
|-------------|-----------|--------------|
| **air_quality_xgboost_model_C** | v2 | XGBoost trained on lagged PM2.5 features (Grade C model) |

### 📈 Model Performance (after adding lag features)
| Metric | Before (E) | After (C) | Change |
|--------|-------------|-----------|--------|
| **MSE** | 3568.29 | **1224.84** | ↓ −65.7 % |
| **R²** | −1.93 | **−0.0068** | ↑ closer to 0 |

> Adding lagged PM2.5 values significantly improved model stability and prediction accuracy by introducing short-term temporal dependencies.

---

## 📊 4. Results & Visualizations

Two plots were generated and uploaded to the Hopsworks Model Registry:

1. 🟢 **Hindcast graph (`pm25_hindcast.png`)** — showing predicted vs. true PM2.5 levels.  
2. 🟣 **Feature importance plot (`feature_importance.png`)** — confirming that lag features contributed most to prediction accuracy.

---

## 🧠 5. Interpretation

> The lag features (`pm25_lag1`, `pm25_lag2`, `pm25_lag3`) capture short-term memory in PM2.5 dynamics,  
> allowing the model to learn daily pollution persistence patterns.  
> This addition reduces noise and leads to a substantial drop in MSE.

---

## 🗂️ 6. Hopsworks Components Overview

| Component | Name / Version | Status |
|------------|----------------|---------|
| **Feature Group** | `air_quality v2`, `weather v1` | ✅ Registered |
| **Feature View** | `air_quality_fv_v2 v3` | ✅ Active |
| **Model Registry** | `air_quality_xgboost_model_C v2` | ✅ Saved |
| **Artifacts Uploaded** | `model.json`, `schema.json`, `pm25_hindcast.png`, `feature_importance.png` | ✅ Uploaded |

---

## ✅ 7. Summary

| Task | Status |
|------|---------|
| Backfill feature pipeline | ✅ Done |
| Daily feature pipeline | ✅ Done |
| Training pipeline | ✅ Done |
| Batch inference dashboard | ✅ Done |
| Lag feature extension (C grade) | ✅ Done |
| README & explanation | ✅ Completed |

---

### 🏁 Final Remarks
The model with lagged PM2.5 features demonstrates that **temporal dynamics** are crucial for improving air quality predictions.  
While R² remains slightly negative due to data variability, the large reduction in MSE confirms that lag-based features provide meaningful predictive power.

---

✍️ *Author: Xin Tang (KTH ID2223 — Air Quality Project, 2025)*




