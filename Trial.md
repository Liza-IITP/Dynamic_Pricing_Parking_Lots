Urban Parking Dynamic Pricing Simulation Report
===============================================

Background and Motivation
-------------------------
Urban parking spaces are a limited and highly demanded resource, particularly in high-traffic city areas. Traditional static pricing models, where fees remain the same regardless of occupancy or time of day, often lead to two primary inefficiencies: overcrowding during peak hours and underutilization during off-peak times.

To address these challenges, this project simulates a dynamic pricing engine that adjusts parking fees based on real-time usage data, using principles of microeconomics and lightweight data modeling. The long-term goal is to build an intelligent pricing system capable of optimizing utilization across multiple parking locations while being responsive to real-world traffic patterns and demand variations.

This report describes the initial trial implementation of such a model on **a single parking space**, as a proof-of-concept. The full implementation will later scale across all 14 urban parking locations with generalized models.

Data Description
----------------
The dataset used in this simulation captures 73 days of time-series data from an urban parking spot. The data is sampled 18 times per day at regular 30-minute intervals between 8:00 AM and 4:30 PM. Each entry records several features including:

- Occupancy and Capacity
- Queue Length
- Traffic Conditions Nearby
- Vehicle Type
- Timestamps
- Special Day Indicator

For this trial simulation, the following fields were removed to simplify the model for a single parking space:
- SystemCodeNumber
- Capacity
- Latitude
- Longitude

These will be reintroduced and appropriately handled in the generalized model covering all 14 parking spaces.

Feature Engineering
-------------------
Key preprocessing steps included:

1. **Timestamp construction**: Combined `LastUpdatedDate` and `LastUpdatedTime` into a single datetime field.
2. **Occupancy Rate**: Calculated as `Occupancy / Capacity`.
3. **Categorical Encoding**: One-hot encoded `VehicleType` into dummy variables (bike, cycle, truck, etc.), and mapped textual traffic descriptions into ordinal values (low=0, average=1, high=2).
4. **Day of Week**: Derived from the timestamp for weekly pattern analysis.
5. **Duplicate removal and sorting**: Ensured temporal consistency and cleanliness.

Simulation Models
-----------------
Three pricing models were considered in this trial:

**Model 1: Alpha-Based Linear Pricing**
- Inspired by simple demand-based economics.
- Simulates price evolution using a fixed base price and a parameter `α` that captures the influence of previous occupancy on price delta.
- `α` was estimated using a linear regression formula (computed from scratch using numpy).

**Model 2: Fluctuation-Based Daily Pricing**
- Calculates price based on daily occupancy range (max - min) to capture volatility in demand.
- Provides a responsive pricing model that penalizes unstable days with larger price jumps.

Technology Stack
----------------
- **Languages & Libraries**: Python, NumPy, Pandas, Matplotlib
- **Data Streaming & Visualization**: Pathway, Bokeh, Panel
- **Profiling**: ydata-profiling (for initial EDA)
- **Notebook Environment**: Google Colab

Conclusion
----------
This trial implementation successfully simulates a dynamic pricing strategy for urban parking based on occupancy and demand metrics. The pricing engine provides a flexible foundation for more advanced, data-driven urban infrastructure management. Future work will extend this prototype into a scalable, intelligent system that adapts to the needs of multiple parking zones and varying traffic scenarios.


