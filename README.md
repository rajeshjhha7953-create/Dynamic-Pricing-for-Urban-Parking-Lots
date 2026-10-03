# Dynamic-Pricing-for-Urban-Parking-Lots
# 💡 Smart Pricing System

A dynamic pricing solution that leverages machine learning to forecast demand and recommend optimal prices in response to market dynamics and competition.

---

## 📘 Project Overview

This project implements a **data-driven pricing strategy** to help businesses set prices that maximize revenue. By analyzing historical sales, competitor pricing, and temporal patterns, the system predicts demand under varying price points and identifies the most profitable pricing decisions.

---

## 🧑‍💻 Technologies Used

- **Python** (3.x)
- **pandas**, **NumPy** for data handling
- **scikit-learn**, **XGBoost** for machine learning
- **matplotlib**, **seaborn** for visualizations
- **Jupyter Notebook** for development
- **Git** for version control

---
Workflow Description
1. Data Preparation
Import raw data files containing past transactions, prices, and competitor benchmarks.

Clean inconsistencies, handle missing entries, and normalize columns.

2. Feature Engineering
Generate variables capturing seasonality (e.g., month, weekday).

Calculate lagged demand metrics and relative price positioning.

Encode categorical fields for model compatibility.

3. Demand Prediction
Train regression models to estimate demand based on price and other predictors.

Evaluate model performance using R² and error metrics.

Select the model with the highest predictive accuracy.

4. Pricing Optimization
Simulate a range of possible prices.

Use the trained model to forecast demand at each price point.

Compute projected revenue (price × predicted demand).

Identify the price that yields the highest expected revenue.

5. Recommendations Output
Generate reports summarizing optimal prices and expected outcomes.

Visualize demand curves and revenue simulation
Absolutely—here’s your content **rephrased clearly and professionally**, maintaining all technical detail but using **fresh wording**:

---

---

## 🧠 **Model 1: Real-Time Baseline Pricing**

This foundational model establishes a dynamic base price for each parking facility, adjusting in real time to daily occupancy variations. It leverages **Pathway's streaming framework** to process and aggregate data in **daily tumbling windows**.

---

### ⚙️ **Core Principle**

The model gauges **demand volatility** by measuring the spread between the highest and lowest occupancy rates recorded over a day. A wider spread signals increased variability and potentially higher demand, supporting price modulation.

---

### 📈 **Pricing Logic**

```
price = base_price + (max_occupancy - min_occupancy) / capacity
```

---

### 🧱 **Workflow Overview**

* **Data Input**
  Continuous streaming ingestion of CSV data via Pathway.

* **Data Preparation**
  Feature engineering (e.g., occupancy rate calculations, timestamp parsing).

* **Windowing Strategy**
  Daily tumbling windows (`windowby()`).

* **Aggregation Steps**

  * Maximum and minimum occupancy per day.
  * Capacity for each lot.

* **Price Computation**
  Apply the formula above for dynamic adjustment.

* **Output & Visualization**

  * Store results as JSONL.
  * Generate visual plots using Bokeh.

---

### 📈 **Outcome & Interpretation**

* **Objective:**
  Adapt parking prices dynamically based on intra-day occupancy shifts.

* **Findings:**
  Daily price curves vary across locations, reflecting real occupancy fluctuations.

* **Insight:**
  Lots exhibiting higher day-to-day occupancy swings see more substantial price adjustments, ensuring responsiveness without introducing excessive complexity.

---

---

## ⚡ **Model 2: Demand-Driven Dynamic Pricing**

Building on Model 1, this approach offers a richer, **multi-factor pricing strategy**, incorporating real-world demand influencers that impact parking pressure in cities.

---

### 🔍 **Composite Demand Calculation**

The demand score combines several weighted components:

```
Demand = α × (Occupancy / Capacity)
       + β × QueueLength
       − γ × TrafficLevel
       + δ × IsSpecialDay
       + ε × VehicleTypeWeight
```

**Parameters:**

* **α = 5.0**: Prioritizes occupancy level.
* **β = 0.4**: Accounts for queue build-up.
* **γ = 1.5**: Detracts for heavy traffic.
* **δ = 2.0**: Increases demand on special days.
* **ε = 1.0**: Reflects vehicle category impact (e.g., trucks vs. cars).

---

### 💰 **Dynamic Pricing Formula**

```
price = base_price × (1 + λ × NormalizedDemand)
```

**Details:**

* `base_price = 10` (baseline minimum)
* `λ = 0.2` (demand scaling factor)
* `NormalizedDemand = demand / max_demand` (capped between 0 and 1)
* **Price boundaries:**
  Clamped between 0.5× and 2.0× the base price.

---

### 🎯 **Design Rationale**

* Raises prices during high-demand conditions (e.g., long queues, special events).
* Discounts when demand is low or traffic is smooth.
* Maintains predictable price boundaries to avoid extremes.

---

### 🔄 **Architecture Overview**

* **Input**
  Streaming CSV data handled by Pathway.

* **Data Transformation**

  * Convert traffic levels and vehicle types to numeric encodings.
  * Calculate occupancy rates.

* **Windowing**
  Daily tumbling windows per parking lot.

* **Aggregation Metrics**

  * Average occupancy
  * Average queue length
  * Average traffic level
  * Dominant vehicle type
  * Special day flags

* **Demand Estimation**
  Apply the weighted formula.

* **Pricing Computation**
  Scale and clamp prices accordingly.

* **Output & Visualization**

  * Save as JSONL.
  * Create interactive Bokeh charts showing daily price evolution.

---

### 📈 **Outcome & Interpretation**

* **Goal:**
  Establish more granular pricing that reacts to multiple urban factors influencing parking demand.

* **Findings:**
  Compared to Model 1, this approach produces smoother, nuanced price variations that align with complex demand patterns.

* **Insight:**
  For example, festival days with high occupancy and congested traffic consistently drive higher prices, demonstrating the model’s sensitivity to combined stress factors.

---


