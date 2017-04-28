# Minor Roads: A method to estimate the importance of minor roads in terms of traffic flow

### This method is presented in: Morley, D.W., Gulliver, J., Methods to improve trafficflow and noise exposure estimation on minor roads,  Environmental Pollution (2016), http://dx.doi.org/10.1016/j.envpol.2016.06.042 

Please see this paper for the background and rationale. The main aim of this study was to try and improve noise exposure estimates for people living in residential areas. Although a good coverage of traffic counts are available for major roads, minor roads are often assined a constant daily vehicle flow. This is a problem for exposure estimates as noise predictions are reliant on accurate traffic flow data. H ere, we attempt to use network routing (similar to SatNav applications) to indentify which minor roads are most commonly used within the road network and assign an importance index accordingly. This can then be used to relate to traffic levels.

All tools and data are freely available. Analysis is carried out using PostGIS with OpenStreetMap geographical data and UK Department of Transport traffic counts. It is assumed that you have already set up PostGIS and are able to import data and run queries.

The following steps give a demo to generate routing importance for the Isle of Wight (a managable sized dataset which is well defined geographically). For very large datasets (e.g. the whole UK), the geographic data need to be split into more managable chunks as Dijkstra is a greedy algorithm. More detail on what is actually going on is given in the scripts themselves

After downloading and importing the data, a method to assign actual counts to major roads is given before moving on to the method for assessing minor road importance.


### (1) SOFTWARE NEEDED
- PostGIS, a spatially enabled PostgreSQL database (http://postgis.net/)
- pgRouting, the routing extension for PostGIS (http://pgrouting.org/)
- osm2po, an application to download and import OSM data (http://osm2po.de/)
- QGIS, a desktop GIS very useful for viewing PostGIS output (http://www.qgis.org/en/site/)

### (2) DATA NEEDED
- DfT traffic counts (http://www.dft.gov.uk/traffic-counts/download.php) (2013 provided here in data folder)
- OSM data in various formats (see http://download.geofabrik.de/ for URLs needed in step 3)

### (3) DOWNLOAD OSM DATA
- Edit the file 'downloadOSM.bat' using a text editor
- Change the URL to your study area (make sure you use the .osm.bz2 data)
- Here, we have pre-defined the Isle of Wight
- Right-click > Open, to run the batch file. Results are stored in the 'C:/osm2po-5.0.0 folder'
- In addition, download from geofabrik the corresponding .shp.zip file and keep just the 'roads' shapefile

### (4) IMPORT OSM DATA TO POSTGIS
- Edit the file 'importOSM.bat' using a text editor
- Set your PostGIS details
- Set the location of the results from (3)
- Ensure '-d' points to an existing database 
- Right-click > Open, to run the batch file.
- Use the PostGIS shapefile importer to add the corresponding 'roads' shapefile from (3)

### (5) ASSIGN TRAFFIC COUNTS TO MAJOR ROADS
- The DfT major road traffic counts are fairly complete for all UK major roads
- The script 'major_assignment.sql' assigns counts to roads
- See the comments in the script for more details

### (6) PREPARE MINOR ROADS DATA
- Work through the 'minor_assignment_dataprep.sql' script 
- Minor roads are handled in groups defined by distinct areas bounded by major roads (that is distinct networks of minor roads only accessible to each other without a major road being crossed. Think of the holes a net pattern created by the major road network).

![s1](/png/iow1.png)
