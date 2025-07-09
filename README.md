# Urban Parking Dynamic Pricing Simulation

## Overview

This project models and simulates dynamic pricing strategies for urban parking lots. Using real or synthetic datasets containing occupancy levels, traffic conditions, and spatial data, it tests three pricing algorithms:

- **Linear Occupancy-based Increase**: A simple reactive model that adjusts price based on occupancy.
- **Demand-based Pricing**: Considers multiple demand drivers (occupancy, queue length, traffic, special events, vehicle type).
- **Competitor-aware Demand Pricing**: Builds on demand-based pricing while also responding to prices of nearby competitor lots.

The goal is to evaluate how different pricing models adapt to demand variations over time and geography.

---

## Tech Stack

**Language:** Python 3 (Google Colab environment)

**Libraries:**
- `numpy` — numerical operations  
- `pandas` — data manipulation  
- `matplotlib` — visualization  
- `geopy` — geographic distance calculation  

**Data Input:** CSV file with parking lot data (locations, occupancy, traffic, queue, etc.)  
**Visualization:** Matplotlib plots of pricing over time

---

## Architecture Diagram

_ In the bottom there is a mermaid diagram_

---

## Project Architecture and Workflow

### 1. Data Loading and Cleaning

- CSV uploaded in Colab.
- Columns like `LastUpdatedDate`, `LastUpdatedTime` merged into a single DateTime.
- Categorical columns mapped to numerical codes:
  - Traffic levels → numeric scale.
  - Vehicle types → cost coefficients.

### 2. Feature Engineering

- Calculates `TrafficCode`, `VehicleCode`.
- Computes demand scores based on:
  - Occupancy relative to capacity.
  - Queue length (nonlinear weighting).
  - Traffic conditions.
  - Special event flags.
  - Vehicle type.

### 3. Distance Matrix Calculation

- Uses `geopy` to compute pairwise distances between all parking lots.
- Stores in a distance table.
- Enables competitor-aware pricing by finding nearby lots within a radius (e.g., 425 meters).

### 4. Pricing Models

Simulates over time steps:

#### Model 1: Simple Linear Increase

- Adjusts price proportionally to `occupancy / capacity`.
- Fixed learning rate (α).

#### Model 2: Demand-Based Pricing

- Demand score calculated using multiple weighted features.
- Robust normalization to avoid outliers.
- Price updated based on normalized demand score.

#### Model 3: Competitor-Aware Pricing

- Starts with demand-based price.
- Adjusts based on prices of nearby competitors:
  - If over-occupied → price drop if neighbors are cheaper.
  - If under-occupied → price increase if neighbors are costlier.

### 5. Simulation Loop

- Iterates over each unique timestamp.
- For each time step:
  - Processes all parking lots.
  - Computes new prices for each model.
  - Saves history of prices for plotting.

### 6. Visualization

- `matplotlib` plots price trajectories for all models and lots.
- Differentiated line styles show model behavior.
- Legend and labeling for clarity.

---

## Additional Notes

### Inputs Required

CSV file with fields like:
- `SystemCodeNumber`
- `Latitude`, `Longitude`
- `Occupancy`, `Capacity`
- `TrafficConditionNearby`
- `VehicleType`
- `QueueLength`
- `IsSpecialDay`
- `LastUpdatedDate`, `LastUpdatedTime`

### Tunable Parameters

- Demand weight configuration (occupancy, queue, traffic, special events)
- Competitor distance radius
- Initial base price
- Learning rates / multipliers

### Assumptions

- All pricing changes are applied in discrete time steps.
- Competitor influence is based purely on geographic distance.
- Demand features are weighted linearly but normalized robustly.


## Architecture Diagram

Below is a Mermaid diagram that can be viewed in [Mermaid Live Editor](https://mermaid.live/edit):

```mermaid
flowchart TD
    A[Dataset.csv] --> B[Data Loading and Cleaning]
    B --> C[Feature Engineering]
    C --> D[Distance Matrix Calculation]
    D --> E[Pricing Simulation Loop]
    E --> F1[Model 1: Linear Increase]
    E --> F2[Model 2: Demand-Based Pricing]
    E --> F3[Model 3: Competitor-Aware Pricing]
    F1 --> G[Price Records]
    F2 --> G
    F3 --> G
    G --> H[Visualization and Analysis]



