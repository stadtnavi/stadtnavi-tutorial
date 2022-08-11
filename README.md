# stadtnavi tutorial
This is a step by step introduction how to setup a [stadtnavi](https://herrenberg.stadtnavi.de/) instance. It aims for developers wanting to get up and running quickly, using docker(-compose) as a deployment tool. For setups closer to production, please consult e.g. the [transportkollektiv digitransit cookbook](https://transportkollektiv.github.io/digitransit-setup/).

## [Step 1](01-installing-and-running-ui/README.md)
Step 1 illustrates, how to start and customize a [digitransit-ui](https://github.com/HSLdevcom/digitransit-ui.git) frontend, connecting to existining backend services.

## [Step 2](02-otp-and-graph-building/README.md)
Step 2 shows, how to run an [OpenTripPlanner](https://github.com/HSLdevcom/digitransit-ui.git) backend service and explains how to customize it for a specific region.

## [Step 3](03-running-otp-and-digitransit/README.md)
Step 3 shows, how to run the customized digitransit-ui, the OpenTripPlanner backend service and the map server to display POIs.

## Credits
This tutorial, as well the whole stadtnavi projects builds on work done by the OpenTripPlanner project, by HSLDevCom who developed Digitransit, as well as many inspiring ideas from the open transport community, especially the [Verschwoerhaus Ulm](https://verschwoerhaus.de) and [Code for Münster](https://codeformuenster.org). Last but not least, this project was sponsored by the German Ministry of Transportation ([BMVI](https://www.bmvi.de/)) and the city of [Herrenberg](https://herrenberg.de/). Thanks to all of you.
