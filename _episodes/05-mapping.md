---
title: "mapping"
teaching: 10
exercises: 20
questions:
objectives:
keypoints:
---

#### Plotting your data on a map:

* download the [QGIS](http://qgis.org/en/site/) software
* run the software and connect to the database:
   * click on "layer/add PostGIS layers"
   * click on the "new" button and enter the connection information (similar to what you did in exercise 1 for the pgadmin software)
   * click on the "connect" button
   * you should now see the database tables we've been working with. Add one of these and it should appear on the map.
* things to explore next:
   * change the symbology of the points, coloring them by a specific attribute so that the colors illustrate grouping in the data
   * add a background map "web\QuickMapServices\OSM"
   * issue the SQL query you built above to select only those points within 1 km of a point
     * click on "database\DBmanager" and you will see the database to which you have already connected.
     * issue a SQL query in the SQL command window (blue arrow in this screen capture):
<img src = "images/dbmanager.png" width = 600>
   * try to make a map like this!
<br>
<img src = "images/crimeradius.png" width = 300>
<br>
