# stadtnavi Tutorial - OpenTripPlanner setup

## Building and running OpenTripPlanner with default config

```sh
$ docker-compose up -d
...
10:35:13.093 INFO (Graph.java:980) Summary (number of each type of annotation):
10:35:13.136 INFO (Graph.java:988) 	BogusShapeDistanceTraveled       -      866
10:35:13.137 INFO (Graph.java:988) 	BogusShapeGeometryCaught         -       99
10:35:13.138 INFO (Graph.java:988) 	GraphConnectivity                -     7037
10:35:13.138 INFO (Graph.java:988) 	Graphwide                        -        1
10:35:13.139 INFO (Graph.java:988) 	HopSpeedFast                     -    23461
10:35:13.139 INFO (Graph.java:988) 	HopSpeedSlow                     -       39
10:35:13.139 INFO (Graph.java:988) 	HopZeroTime                      -    33549
10:35:13.139 INFO (Graph.java:988) 	LevelAmbiguous                   -      753
10:35:13.140 INFO (Graph.java:988) 	ParkAndRideUnlinked              -       12
10:35:13.140 INFO (Graph.java:988) 	StopLinkedTooFar                 -      191
10:35:13.140 INFO (Graph.java:988) 	StopNotLinkedForTransfers        -      643
10:35:13.140 INFO (Graph.java:988) 	StopUnlinked                     -      573
10:35:13.140 INFO (Graph.java:988) 	TurnRestrictionBad               -      496
10:35:13.140 INFO (Graph.java:988) 	TurnRestrictionException         -      106
10:35:13.140 INFO (Graph.java:988) 	TurnRestrictionUnknown           -       59
10:35:13.362 INFO (Graph.java:826) Main graph size: |V|=1113693 |E|=2926193
10:35:13.362 INFO (Graph.java:827) Writing graph /opt/opentripplanner/build/default/Graph.obj ...
...
Successfully built cf2ff3e4936f
Successfully tagged 02-otp-and-graph-building_graph:latest
WARNING: Image for service graph was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating 02-otp-and-graph-building_graph_1 ... done
Creating 02-otp-and-graph-building_opentripplanner_1 ... done
$
```

To verify that the OpenTripPlanner started successfully, you may check for the following lines in the logs:
```sh
$ docker logs 02-otp-and-graph-building_opentripplanner_1

...
12:14:53.850 INFO (NetworkListener.java:755) Started listener bound to [0.0.0.0:8080]
12:14:53.863 INFO (NetworkListener.java:755) Started listener bound to [0.0.0.0:8081]
12:14:53.865 INFO (HttpServer.java:300) [HttpServer] Started.
12:14:53.865 INFO (GrizzlyServer.java:154) Grizzly server running.
```

Now OpenTripPlanner is ready and you may access the interal OTP front end via http://localhost:8090 (Note the port is 8090 as 8080 is mapped to 8090 in the docker-compose.yml).

If that worked, you may adapt the config to supply your proper data.

## Building and running OpenTripPlanner with your config

If you want to build and run OpenTripPlanner with your own GTFS/OSM data, copy `.env`to e.g. `.env.mycity`, edit especially the GTFS_URL and OSM_URL variables to point to your data:

```
# URL to the GTFS feed used for routing
GTFS_URL=https://www.openvvs.de/dataset/e66f03e4-79f2-41d0-90f1-166ca609e491/resource/bfbb59c7-767c-4bca-bbb2-d8d32a3e0378/download/google_transit.zip
# URL to the OSM region used for routing.
OSM_URL=https://download.geofabrik.de/europe/germany/baden-wuerttemberg/stuttgart-regbez-latest.osm.pbf
# Memory allocated to OpenTripPlanner when building the Graph
GRAPH_BUILD_MEMORY=5G
# Port to access OpenTripPlanner on the host machine
OTP_PORT=8090

# Image used to build (and run) OpenTripPlanner
OTP_IMAGE=mfdz/opentripplanner
# Image tag used to build (and run) OpenTripPlanner
OTP_TAG=latest
```

Finally run docker-compose specifying your adapted config file (be sure to pass the --build flag, so containers are rebuilt):

*Note*: Please make sure that you have at least `docker-compose` 1.25.0. 

```sh
$ docker-compose --env-file=.env.mycity up --build -d
```
