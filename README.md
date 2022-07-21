# Spatial Feature Engineering for Geomarketing

This is a quick guide for data scientists and analysts on the topic of working with geospatial data.
Geographical information in a dataset it can be used for feature generation and provide statistically important data.
This is called spatial feature engineering. In this guide I mainly focus on geomarketing task, but its concepts can be used in any field.

* [Spatial weights](https://github.com/Denikozub/Geomarketing/blob/main/README.md#spatial-weights)
* [Geostatistics](https://github.com/Denikozub/Geomarketing/blob/main/README.md#geostatistics)
* [Network analysis](https://github.com/Denikozub/Geomarketing/blob/main/README.md#network-analysis)
* [Geocoding](https://github.com/Denikozub/Geomarketing/blob/main/README.md#geocoding)
* [Text data](https://github.com/Denikozub/Geomarketing/blob/main/README.md#text-data)
* [Feature importance](https://github.com/Denikozub/Geomarketing/blob/main/README.md#feature-importance)

## Spatial weights

Spatial weights represent spatial structure of the data.
They define neighbouring spatial relations, so they can be treated as a weighted graph, stored in adjacency matrix or list.
If no relation is considered between objects i and j, corresponding w_ij = 0.
Spatial weight matrix is required to build models compute most geospatial indices.
Spatial relationships, represented in weights, can be defined in different ways.
For better understanding on the methods I recommend reading [this](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/modeling-spatial-relationships.htm) article by ArcGIS.

## Geostatistics

## Network analysis

In urban areas each object has a corresponding part of road graph (defined by [point and radius](https://osmnx.readthedocs.io/en/stable/osmnx.html#osmnx.truncate.truncate_graph_dist)).
It can be utilized for generating features using [osmnx.stats.basic_stats](https://osmnx.readthedocs.io/en/stable/osmnx.html#osmnx.stats.basic_stats).

1. Average circuity (circuity_avg)
2. Total edge length per km^2 (edge_density_km)
3. Intersection count per km^2 (intersection_density_km)
4. Self-loop proportion (self_loop_proportion)
5. Average number of streets per node (streets_per_node_avg)
6. Node count per km^2 (node_density_km)
7. Average street length (street_length_avg)

PySAL [spaghetti](https://pysal.org/spaghetti/index.html) module also provides various network statistics:

1. Moran’s I ([API](https://pysal.org/spaghetti/generated/spaghetti.Network.html#spaghetti.Network.Moran))
2. Point snap distance ([API](https://pysal.org/spaghetti/generated/spaghetti.Network.html#spaghetti.Network.compute_snap_dist))
3. Network weights ([Network](https://pysal.org/spaghetti/generated/spaghetti.Network.html#spaghetti-network) w_network attribute) used to find distances to network hotspots / K nearest clusters / ...

## Geocoding

## Text data

## Feature importance
