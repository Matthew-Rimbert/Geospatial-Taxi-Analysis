# 📍 Geospatial Cab Analysis

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.4.2+-red.svg)
![Folium](https://img.shields.io/badge/Folium-0.12.1+-green.svg)
![Shapely](https://img.shields.io/badge/Shapely-1.7.1+-orange.svg)
![Geopy](https://img.shields.io/badge/Geopy-2.1.0+-purple.svg)

## 📋 Purpose
This project focuses on analyzing location data by defining polygons, visualizing the data on static and interactive maps, calculating distances, and filtering cabs based on availability. It demonstrates the use of Matplotlib and Folium for geospatial data visualization, Shapely for geometric operations, and Geopy for distance calculations.

## 🛠️ Scope
- Define two or more polygons using geographical coordinates
- Visualize polygons and taxi cab locations on static and interactive maps
- Calculate distances between taxi cabs and a pick-up point, considering polygon boundaries
- Identify the closest available cab based on filtering criteria

## 🚀 Installation
1. **Clone the repository:**
    ```bash
    git clone https://github.com/Matthew-Rimbert/Geospatial-Taxi-Analysis.git
    ```
2. **Navigate to the project directory:**
    ```bash
    cd Geospatial-Taxi-Analysis
    ```
3. **Install required Python packages:**
    ```bash
    pip install matplotlib folium shapely geopy
    ```

## ▶️ Usage
1. **Run the script:**
    ```bash
    python geospatial_taxi_analysis.py
    ```

2. **View the outputs** in the terminal and generated plots.

## 📊 Data
The dataset used in this project includes:
- **Taxi Cab Locations**
- **Polygon Coordinates**
- **Pick-Up Points**

## 🖼️ Example Outputs

### Defined Polygons and Cab Locations:
![static_map_with_compass](https://github.com/user-attachments/assets/b1bd4691-72f3-4ed0-847b-4af0634231d6)

### Interactive Map:
![Interactive Map of Cab Locations and Polygons](https://github.com/user-attachments/assets/fdd16655-1581-460d-aecf-0b8e4c108739)


## 🧩 Code Snippets

### Loading and Defining Data
```python
import matplotlib.pyplot as plt
import folium
from shapely.geometry import Point, Polygon
from geopy.distance import distance

# Define coordinates for the two polygons
coords_poly1 = [
    (35.1046911691531, -106.62879386046905),
    (35.02388440629614, -106.63617056713393),
    (35.05805862717621, -106.56947996462094),
    (35.06703883926599, -106.56582106044395),
    (35.06773391532695, -106.49864180586857),
    (35.068548804338526, -106.49621986543428),
    (35.10309822812283, -106.587021598642)
]

coords_poly2 = [
    (35.10388605727355, -106.63249131051455),
    (35.10553893305533, -106.70566070848766),
    (35.064299180854206, -106.7869096288846),
    (35.02395052888935, -106.79328452602253),
    (35.02535627274973, -106.71844016632903),
    (35.02344323227261, -106.6409678651649),
    (35.074730379142785, -106.64125656403539)
]

# Create Polygon objects
poly1 = Polygon(coords_poly1)
poly2 = Polygon(coords_poly2)

# Define points representing cab locations and a pick-up place
cab_locations = {
    'cab_1': Point(35.0800, -106.7200),  # Inside Polygon 2 (Southwest Region)
    'cab_2': Point(35.0580, -106.5700),  # Inside Polygon 1
    'cab_3': Point(35.0853, -106.6255),  # Near UNM
    'cab_4': Point(35.0750, -106.6000),  # Random location
    'pick_up': Point(35.0494, -106.6174)  # Albuquerque International Sunport
}
```
