# 🚖 Analyzing Location Data in Albuquerque 🚖

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.4.2+-red.svg)
![Folium](https://img.shields.io/badge/Folium-0.12.1+-green.svg)
![Shapely](https://img.shields.io/badge/Shapely-1.7.1+-orange.svg)
![Geopy](https://img.shields.io/badge/Geopy-2.1.0+-purple.svg)

## 📋 Overview

The project involves analyzing cab locations and pick-up points in Albuquerque, defining polygons, calculating distances, and visualizing the data on maps.

## 🎯 Objectives

- 🗺️ Define two or more polygons and visualize them on a map.
- 📏 Improve the pick-up algorithm by calculating distances.
- 🔍 Filter data with a list comprehension to determine unavailable cabs.
- 📊 Visualize the polygons and cab locations on static and interactive maps.

## Visualizations Generated
Running the script will create:

A static map picture: Saved as **static_map_with_compass.png** in the project folder.
An interactive map: Saved as **Albuquerque_Cab_NOW_map.html** in the project folder.

### Defined Polygons and Cab Locations:
Using **Matplotlib** to create a static map showing the polygons and cab locations.

![static_map_with_compass](https://github.com/user-attachments/assets/b1bd4691-72f3-4ed0-847b-4af0634231d6)

### Interactive Map:
Using **Folium** to create an interactive map that allows users to interact with the map and explore the cab locations and regions.

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
## 🔍 Key Components
**1. Define Polygons**
We define two polygons using geographical coordinates to represent different regions(THE BLUE & RED zone on our maps) in Albuquerque.
```python
from shapely.geometry import Polygon

# Define coordinates for the polygons
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
```
**Distance Calculation**:
We calculate the distance between cab locations and the pick-up point, considering polygon boundaries. This helps in determining which cab is closest to the pick-up point, either directly or by calculating the shortest path around obstacles.

```python
from shapely.geometry import Point
from geopy.distance import distance

# Define points representing cab locations and a pick-up place
cab_locations = {
    'cab_1': Point(35.0800, -106.7200),  # Inside Polygon 2 (Southwest Region)
    'cab_2': Point(35.0580, -106.5700),  # Inside Polygon 1
    'cab_3': Point(35.0853, -106.6255),  # Near UNM
    'cab_4': Point(35.0750, -106.6000),  # Random location
    'pick_up': Point(35.0494, -106.6174)  # Albuquerque International Sunport
}

# Distance Calculation
entry_point = cab_locations['pick_up']  # Entry point for distance calculation (Pick-Up Location)
cab_points = {name: location for name, location in cab_locations.items() if name != 'pick_up'}
pick_up_point = cab_locations['pick_up']

def calculate_distance(cab_location, pick_up_location, polygon, entry_point):
    if cab_location.within(polygon):
        return round(distance((pick_up_location.x, pick_up_location.y), (cab_location.x, cab_location.y)).miles, 2)
    else:
        return round(distance((cab_location.x, cab_location.y), (entry_point.x, entry_point.y)).miles + distance((entry_point.x, entry_point.y), (pick_up_location.x, pick_up_location.y)).miles, 2)

distances = {}
for cab_name, cab_location in cab_points.items():
    if cab_location.within(poly1) or cab_location.within(poly2):
        polygon = poly1 if cab_location.within(poly1) else poly2
        distances[cab_name] = calculate_distance(cab_location, pick_up_point, polygon, entry_point)

closest_cab = min(distances, key=distances.get)
closest_distance = distances[closest_cab]
```
**Data Filtering**:
We filter the list of orders to determine unavailable cabs using list comprehension. This step helps in identifying which cabs are currently occupied and thus not available for new pick-ups.

```python
# Orders list for filtering data
orders = [
    ('order_039', 'open', 'cab_3'),
    ('order_034', 'open', 'cab_4'),
    ('order_032', 'closed', 'cab_2'),
    ('order_026', 'open', 'cab_1')
]

# Filter using list comprehension
unavailable_cabs = [order[2] for order in orders if order[1] == 'open']
available_distances = {cab: dist for cab, dist in distances.items() if cab not in unavailable_cabs}

if available_distances:
    closest_available_cab = min(available_distances, key=available_distances.get)
    closest_available_distance = available_distances[closest_available_cab]
else:
    closest_available_cab = None
    closest_available_distance = None
```
## Sample Output:
After running the script, the console will display information about the closest cab, unavailable cabs and the closest available cab:

![commandlne_output](https://github.com/user-attachments/assets/31b1b66e-df57-404e-99fa-52002883b238)


# 📈 Summary
The primary aim of this project is to analyze cab locations and optimize pick-up strategies in Albuquerque by utilizing geographical data. By defining specific regions and calculating distances, the project aims to improve the efficiency of cab allocation to minimize customer wait times.

*Scope*: The project involves:

- Defining two or more polygons representing different regions in Albuquerque.
- Calculating the distance between cabs and a pick-up point, considering the boundaries of the defined polygons.
- Filtering out unavailable cabs based on their current status.
- Visualizing cab locations and regions using both static and interactive maps.


