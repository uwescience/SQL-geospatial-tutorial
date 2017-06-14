---
title: "Geospatial"
teaching: 10
exercises: 20
questions:
- "What are the various ways that we can represent geospatial information?"
- "How does a database store spatial information?"
objectives:
- "Learn the difference between geographic and projected coordinate systems"
- "Explore why understanding the coordinate reference frame matters when carrying out basic geospatial analysis"
- "Become familiar with some of the geospatial toolkits within PostGIS"
keypoints:
- "All geospatial analysis requires a knowledge of reference frames and coordinate systems"
- "many databases have extensions that encode geospatial information (e.g. PostGIS)"
- "PostGIS functions provide a wide range of tools to incorporate spatial analysis into your workflow"
---

## Calculating distances

* our last research question was: "What is the most common crime within 5 km of my house?"
* To answer this we'll need to understand something about mapping, and how databases encode spatial information.
* Note that the database currently has latitude and longitude information, for example:

~~~
SELECT latitude, longitude
FROM seattlecrimeincidents 
LIMIT 5;
~~~
{: .sql}

<br>
<hr>

| Latitude | Longitude |
| ---- | ---- |
| 47.6158384 | -122.3181689 |
| 47.60087709 | -122.3312162 |
| 47.59582098 | -122.3175691 | 
| 47.6140991 | -122.3174884 |
| 47.63148825 | -122.3125079 |

<hr>
<br>

* latitude and longitude are components of the _Geographic_ reference frame and are the most common ways to encode geospatial information:

<img src="../assets/img/databaseIntro/earthLatLong2.png" width = "600">

> ## What is the straight line distance between points 1 and 2?
> Point 1: 47.6158384, -112.3181689 
> 
> Point 2:  47.60087709, -112.3312162 
{: .challenge}

<!--
~~~
import numpy as np
latDist = 111.2 # one degree of latitude is 111.2 km
dLat = (47.6158384 - 47.60087709)
dLong = (-112.3181689 - -112.3312162)
distance = np.sqrt(dLat**2 + dLong**2) * latDist
print("Estimated distance is " + str(round(distance,3)) + " km")
~~~
{: .python}

~~~
Estimated distance is 2.207 km
~~~
{: .output}
-->

## Projection: deals with the problem of converting 3D earth to a 2D plane on which we can plot features

<img src="../assets/img/databaseIntro/projections2.png" width = "600">

## Projection: like shining a light through a transparent sphere and tracing the lines of lat/long

<img src="../assets/img/databaseIntro/projections.png" width = "500">

<img src="../assets/img/databaseIntro/projectionTypes.png" width = "650">

### How does the database currently encode the latitude and longitude information?

~~~
SELECT column_name, data_type 
FROM information_schema.columns 
WHERE table_name = 'seattlecrimeincidents' AND (column_name = 'Latitude' OR column_name = 'Longitude');
~~~
{: .sql}

### It would be better if the database understands latitude and longitude as locations rather than as double precision numbers.

### Implementation: first, add a geometry column:

~~~
ALTER TABLE seattlecrimeincidents ADD COLUMN geom geometry(Point, 4326);
~~~
{:. sql}

* 4326 is the code for geographic (latitude/longitude) coordinate system 


| Point | Latitude | Longitude | geom | 
| ---- | ---- | ---- | ---- |
| 1 | 47.6158384| -112.3181689 | |
| 2 | 47.60087709 | -112.3312162 | | 


### Next, populate the geometry column:

```SQL
UPDATE seattlecrimeincidents SET geom = ST_setSRID(ST_MakePoint(longitude,latitude),4326);
```

| Point | Latitude | Longitude | geom | 
| ---- | ---- | ---- | ---- |
| 1 | 47.6158384| -112.3181689 | 0101000020E6100000AD0617E15C945EC07CCFEDCAD3CE4740 |
| 2 | 47.60087709 | -112.3312162 | 0101000020E6100000F2B96EA532955EC09A3B5D8AE9CC4740 | 

## Now that the database has geometric encoding, we can use a wide range of geospatial functions
<br>
### First, let's transform our geometries into a _projected_ coordinate system so that we can calculate distances  

```SQL
ALTER TABLE seattlecrimeincidents 
ADD COLUMN geom_utm geometry(Point, 3717);

UPDATE seattlecrimeincidents 
SET geom_utm = ST_Transform(geom,3717);
```
```SQL
SELECT ST_Distance(a.geom_utm,b.geom_utm)
FROM seattlecrimeincidents AS a, seattlecrimeincidents AS b
WHERE a.gid=1 AND b.gid=2;
```

<br>
<hr>

| st_distance |
| ---- |
| 1930.45436426609 |