# Spatial Feature Engineering for Geomarketing

This is a quick guide for data scientists and analysts on the topic of working with geospatial data.
Geographical information in a dataset it can be used for feature generation and provide statistically important data.
This is called spatial feature engineering. In this guide I mainly focus on geomarketing task, but its concepts can be used in any field.

* [Spatial weights](https://github.com/Denikozub/Geomarketing/blob/main/README.md#spatial-weights)
* [Geostatistics](https://github.com/Denikozub/Geomarketing/blob/main/README.md#geostatistics)
  * [Basic features](https://github.com/Denikozub/Geomarketing/blob/main/README.md#basic-features)
  * [Advanced features](https://github.com/Denikozub/Geomarketing/blob/main/README.md#advanced-features)
* [Network analysis](https://github.com/Denikozub/Geomarketing/blob/main/README.md#network-analysis)
* [Geocoding](https://github.com/Denikozub/Geomarketing/blob/main/README.md#geocoding)
* [Text data](https://github.com/Denikozub/Geomarketing/blob/main/README.md#text-data)
* [Geospatial models](https://github.com/Denikozub/Geomarketing/blob/main/README.md#geospatial-models)
* [References](https://github.com/Denikozub/Geomarketing/edit/main/README.md#references)

## Spatial weights

Spatial weights represent spatial structure of the data.
They define neighbouring spatial relations, so they can be treated as a weighted graph, stored in adjacency matrix or list.
If no relation is considered between objects i and j, corresponding w_ij = 0.
Spatial weight matrix is required to build models compute most geospatial indices.
Spatial relationships, represented in weights, can be defined in different ways.
For better understanding on the methods I recommend reading [this](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/modeling-spatial-relationships.htm) article by ArcGIS.

## Geostatistics

### Basic features

1. Counting nearby objects (e.g. competitors) - [buffer](https://geopandas.org/en/stable/docs/reference/api/geopandas.GeoSeries.buffer.html) + [clip](https://geopandas.org/en/stable/docs/reference/api/geopandas.clip.html)
2. Distance to key objects (e.g. [feature center](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/mean-center.htm), [central feature](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/central-feature.htm) or [feature hotspot](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/hot-spot-analysis.htm))
    * Haversine distance
    * Manhattan distance with haversine formula
3. Distance to closest object (e.g. storage, shopping center or bus stop)
4. [Spatial lag](http://docs.momepy.org/en/stable/generated/momepy.WeightedCharacter.html#momepy.WeightedCharacter) - feature neighbour-weighted average
5. Building areas, perimeters and volumes ([momepy](http://docs.momepy.org/en/stable/api.html#dimension))
6. Building alignment, adjacency, shared walls ([momepy](http://docs.momepy.org/en/stable/api.html#spatial-distribution))
7. [Building intensity](http://docs.momepy.org/en/stable/api.html#intensity)
8. Neighbouring-based diversity indices ([momepy](http://docs.momepy.org/en/stable/api.html#diversity))
9. Elevation (altitude)

### Advanced features

1. Local spatial autocorrelation (cluster and outlier analysis)
    * [Local Moran](https://pysal.org/esda/generated/esda.Moran_Local.html#esda.Moran_Local)
    * [Local G](https://pysal.org/esda/generated/esda.G_Local.html#esda.G_Local)
    * [Local Geary](https://pysal.org/esda/generated/esda.Geary_Local.html#esda.Geary_Local)
    * [Local join counts](https://pysal.org/esda/generated/esda.Join_Counts_Local.html#esda.Join_Counts_Local) (for binary features)
2. [Shape measures](https://pysal.org/esda/notebooks/shape-measures.html) and [urban shape measures](http://docs.momepy.org/en/stable/api.html#shape) for polygonal objects
3. [Spatial accessibility metrics](https://access.readthedocs.io/en/latest/api.html)
4. Clustering
    * [AZP](https://pysal.org/spopt/generated/spopt.region.AZP.html#spopt.region.AZP) (automatic zoning procedure)
    * [Bottom-up agglomerative](https://pysal.org/spopt/generated/spopt.region.WardSpatial.html#spopt.region.WardSpatial)
    * [Regional K-Means](https://pysal.org/spopt/generated/spopt.region.RegionKMeansHeuristic.html#spopt.region.RegionKMeansHeuristic)
    * [A-DBSCAN](https://pysal.org/esda/generated/esda.adbscan.ADBSCAN.html#esda.adbscan.ADBSCAN)
    * [Multivariate](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/multivariate-clustering.htm)
5. Location set covering problem ([LSCP](https://pysal.org/spopt/generated/spopt.locate.coverage.LSCP.html#spopt.locate.coverage.LSCP))
6. [Silhouette samples](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_samples.html)
7. [Distance-preserving dimensionality reduction](https://geodacenter.github.io/workbook/7ab_mds/lab7ab.html)

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

In some cases reverse geocoding can provide useful features. It can be done using [GeoPy](https://geopy.readthedocs.io/en/stable/) or [reverse geocoder](https://github.com/thampiman/reverse-geocoder). Physical address can be used to generate categorical features (country, city, district, street) or can be treated as [text data](https://github.com/Denikozub/Geomarketing/blob/main/README.md#text-data).  

Another categorical feature, representing spatial proximity, is geohash, which is a unique identifier of a specific region on the Earth. It can be computed using [geohash](https://github.com/vinsci/geohash/) library.

## Text data

Text data can also be used for feature engineering. This has nothing to do with spatial data analysis, but I want to cover the majority of topics in this guide.
One way to utilize text data is to generate categorical data by parsing text using separators or regular expressions. A different approach involves using [word2vec](https://en.wikipedia.org/wiki/Word2vec) word embeddings for a single word. If you have a phrase of multiple words, it is worth taking average of word embeddings or (more precisely) average of word embeddings multiplied by their TF-IDF score. A complete different approach involves using [BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) text embeddings as features.

## Geospatial models

There are machine learning models that do not require any spatial feature engineering at all. They understand spatial relationships along with attributive information and can be a pipeline for quick metrics estimation, a good start for hypothesis testing or initial data overview. Since most models are linear, it is worth using feature transformations (log, power).

* [Geographically weighted regression (GWR)](https://mgwr.readthedocs.io/en/latest/generated/mgwr.gwr.GWR.html#mgwr.gwr.GWR)
* [Multiscale GWR (MGWR)](https://mgwr.readthedocs.io/en/latest/generated/mgwr.gwr.MGWR.html#mgwr.gwr.MGWR)
* [Ordinary least squares](https://pysal.org/spreg/generated/spreg.OLS.html#spreg.OLS)
* [Seemingly unrelated regression](https://pysal.org/spreg/generated/spreg.SUR.html#spreg.SUR)
* [Fixed](https://pysal.org/spreg/generated/spreg.Panel_FE_Lag.html#spreg.Panel_FE_Lag) and [random](https://pysal.org/spreg/generated/spreg.Panel_RE_Lag.html#spreg.Panel_RE_Lag) effects panels
* [Forest-based classification and regression](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/forestbasedclassificationregression.htm)
* [Tests](https://pysal.org/spreg/api.html#diagnostics) of homoskedasticity, normality, spatial randomness etc.

## References

1. [ArcGIS](https://pro.arcgis.com/en/pro-app/2.8/tool-reference/spatial-statistics/spatial-statistics-toolbox-sample-applications.htm) documentation
2. [GeoDa](https://geodacenter.github.io/documentation.html) documentation
3. [PySAL](https://pysal.org/) library
4. [Geographic Data Science with Python](https://geographicdata.science/book/intro.html)
