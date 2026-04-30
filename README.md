# Project 1: The Sensor "Truth" Script

## Project Overview
In a manufacturing environment, physical sensors are the heartbeat of the operation. However, real-world sensors often report "noisy" or impossible data due to electrical interference, hardware fatigue, or complete failure. 

This project demonstrates the transition from raw, unreliable sensor data to a validated "Sensor Truth." Using a proxy dataset (Metro Traffic), I identified critical hardware failures, implemented a sophisticated time-series imputation strategy, and engineered a proactive anomaly detection system.

## The Problem
A data audit revealed 10 instances of "dead" readings where sensors reported a temperature of 0 Kelvin (-273.15°C). Because absolute zero is physically impossible in this production environment, these readings represented hardware failures. Left uncorrected, these errors skewed monthly production averages by nearly 1%.

## The Solution
Instead of simply deleting the broken records (which would create gaps in the operational timeline), I established a "Sensor Truth" through:
1. **Data Imputation**: Developed a Python function utilizing a **centered rolling window** (3-hour mean) to reconstruct missing values based on adjacent historical trends.
2. **Safety Nets**: Implemented a forward-fill backup to catch any edge-case failures that the rolling mean could not resolve.
3. **Validation**: Verified that the final dataset contained zero null values and zero impossible physical readings.

## Feature Engineering: Anomaly Detection
To transform this script from a cleaning tool into an insight generator, I implemented a proactive monitoring flag:
* **Logic**: Applied the $3\sigma$ (three standard deviations) rule to the cleaned data.
* **Flag**: Created a boolean `is_outlier` column to automatically identify statistical anomalies, providing a trigger for maintenance teams to inspect equipment.

## Visual Proof
Below is a "zoom-in" on a sensor failure event (Index 11898). The red dashed line shows the raw sensor dropping to an impossible 0 Kelvin, while the green line shows the reconstructed "Sensor Truth" successfully bridging the gap.

*(Note: In your actual GitHub repo, you would insert a screenshot here)*

## Tools Used
* **Python**: Core logic and data manipulation.
* **Pandas**: Dataframe management and time-series rolling calculations.
* **Matplotlib/Seaborn**: Technical validation and data visualization.
