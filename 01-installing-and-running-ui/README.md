## Customize stadtnavi frontend, using existing backend services
### 1. Requirements, installation
    - git
    - watchman
    - nodejs(v10.24.0)
    - yarn(LTS)
    - build-essential (for the node dependencies)
    - Docker
 

### ON DEBIAN 10 ONLY:
  - add `export OPENSSL_CONF=/etc/ssl/` to your `~/.bashrc` file 
  - run `source ~/.bashrc`

### Installing Docker(more info [here](https://docs.docker.com/engine/install/debian/))
- don't forget to uninstall conflicting packages
- use the [Docker repository](https://docs.docker.com/engine/install/debian/#install-using-the-repository)
- create a group and add user https://docs.docker.com/engine/install/linux-postinstall/
- try `docker run hello-world` and `docker --version`

### Install build-essentials:
build-essentials contains a lot of packages like gcc, g++, make, etc.  

run:  
`apt install build-essential`


check installation:  
```
$ make -v
GNU Make 4.2.1
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

### Installing git:
`apt-get install git -y`

check installation:
```
$ git --version
git version 2.20.1
```

### Installing watchman:

On Mac run

```
brew install watchman
```

On Debian 10 it is rather difficult to install. Please follow the instructions
on the [Watchman installation page](https://facebook.github.io/watchman/docs/install.html).

### Installing npm, nodejs and yarn:

#### install nodejs version 10(more info [here](https://github.com/nodesource/distributions)):
`curl -fsSL https://deb.nodesource.com/setup_10.x | sudo -E bash -`
`apt-get install nodejs -y`

check nodejs installation:
```
$ nodejs -v
v10.24.0
```

#### install npm:
`apt install npm -y`

check npm installation:
```
$ npm --version
5.8.0
``` 

#### install yarn(pkg)
prefered way:
```
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```
#### OR
`apt install yarnpkg`

check yarnpkg installation:
```
$ yarnpkg --version
1.13.0
$ alias yarn=yarnpkg
$ yarn --version
1.13.0
```

On Debian 10 yarn is installed as yarnpkg. To invoke it with the short name enter this in your shell or .bashrc: `alias yarn=yarnpkg`.

### 2. Checking out stadtnavi

```
$ git clone https://github.com/stadtnavi/digitransit-ui.git
Klone nach 'digitransit-ui' ...
remote: Enumerating objects: 112, done.
remote: Counting objects: 100% (112/112), done.
remote: Compressing objects: 100% (60/60), done.
remote: Total 140942 (delta 64), reused 79 (delta 52), pack-reused 140830
Empfange Objekte: 100% (140942/140942), 142.87 MiB | 5.35 MiB/s, Fertig.
Löse Unterschiede auf: 100% (98939/98939), Fertig.

$ cd digitransit-ui
```


### 3. Doing an initial build to see if it works

```
$ yarn install
yarn install v1.13.0
[1/5] Validating package.json...
[2/5] Resolving packages...
[3/5] Fetching packages...
info There appears to be trouble with your network connection. Retrying...
info fsevents@1.2.13: The platform "linux" is incompatible with this module.
info "fsevents@1.2.13" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.1.3: The platform "linux" is incompatible with this module.
info "fsevents@2.1.3" is an optional dependency and failed compatibility check. Excluding it from installation.
[4/5] Linking dependencies...
warning "workspace-aggregator-020d5fc9-854e-4496-a355-2b26ddcf9918 > isomorphic-relay-router@0.8.6" has incorrect peer dependency "isomorphic-relay@https://github.com/michaelstockton/isomorphic-relay.git#feature-relay-1-public".
warning "workspace-aggregator-020d5fc9-854e-4496-a355-2b26ddcf9918 > isomorphic-relay-router@0.8.6" has incorrect peer dependency "react@^15.0.1".
warning " > react-leaflet@2.6.1" has unmet peer dependency "leaflet@^1.6.0".
warning "enzyme-adapter-react-16 > react-test-renderer@16.13.1" has incorrect peer dependency "react@^16.13.1".
warning "eslint-config-airbnb > eslint-config-airbnb-base@13.2.0" has incorrect peer dependency "eslint-plugin-import@^2.17.2".
[5/5] Building fresh packages...
warning Your current version of Yarn is out of date. The latest version is "1.22.5", while you're on "1.13.0".
Done in 339.80s.

$ yarn setup

...
lerna success run Ran npm script 'clean' in 2 packages in 1.8s:
lerna success - @digitransit-store/digitransit-store-common-functions
lerna success - @digitransit-store/digitransit-store-future-route
info filter [ '@digitransit-store/*',
info filter   '!@digitransit-component/digitransit-component',
info filter   '!@digitransit-component/digitransit-component-with-breakpoint',
info filter   '!undefined' ]

/tmp/digitransit-ui/digitransit-store/packages/digitransit-store-common-functions/src/index.js → digitransit-store/packages/digitransit-store-common-functions/lib...
created digitransit-store/packages/digitransit-store-common-functions/lib in 876ms

/tmp/digitransit-ui/digitransit-store/packages/digitransit-store-future-route/src/index.js → digitransit-store/packages/digitransit-store-future-route/lib...
created digitransit-store/packages/digitransit-store-future-route/lib in 1.8s

```
The warnings are "ok".

To test the installation run:
`yarn run dev`

In the console you will see this message `Digitransit-ui available on port 8080`. Open http://localhost:8080/ and wait for the initial loading to finish. 

### 4. Starting a new config 
  - to add a new theme run: `yarn run add-theme <new_theme>`
  - for example `yarn run add-theme rt`
  - 3 files should be created and 1 modified:
    1. app/configurations/config.<new_theme>.js
    2. sass/themes/<new_theme>/_theme.scss 
    3. sass/themes/<new_theme>/main.scss
    4. app/configurations/config.default.js has been modified: `themeMap` should have a new property `<new_theme>: '<new_theme>'`


### 5. Do some customization
* Colors
  * edit sass/themes/<new_theme>/_theme.scss file
  * changing the already declared scss variables will change the colors of the new config immediately
  * e.g. change the primary color to orange: `$primary-color: #ff7300;` 
* City Name 
    * modify the `APP_TITLE` variable located in app/configurations/config.<new_theme>.js
* `config.rt.js`
    * Replace the content of `config.rt.js` with the following code:
    ```
  /* eslint-disable */
import configMerger from '../util/configMerger';

const CONFIG = 'rt';
const APP_TITLE = 'stadtnavi Reutlingen';
const APP_DESCRIPTION = 'Gemeinsam Mobilität neu denken - die intermodale Verbindungssuche mit offenen, lokalen Daten';
const API_URL = process.env.API_URL || 'https://api.stadtnavi.de';
const MAP_URL = process.env.MAP_URL || 'https://tiles.stadtnavi.eu/streets/{z}/{x}/{y}{r}.png';
const SEMI_TRANSPARENT_MAP_URL = process.env.SEMITRANSPARENT_MAP_URL || "https://tiles.stadtnavi.eu/satellite-overlay/{z}/{x}/{y}{r}.png";
const GEOCODING_BASE_URL = process.env.GEOCODING_BASE_URL || "https://photon.stadtnavi.eu/pelias/v1";
const YEAR = 1900 + new Date().getYear();
const STATIC_MESSAGE_URL =
    process.env.STATIC_MESSAGE_URL ||
    '/assets/messages/message.hb.json';

const walttiConfig = require('./config.waltti.js').default;

const realtimeHbg = require('./realtimeUtils').default.hbg;
const hostname = new URL(API_URL);
realtimeHbg.mqtt = `wss://${hostname.host}/mqtt/`;

const minLat = 47.6020;
const maxLat = 49.0050;
const minLon = 8.4087;
const maxLon = 9.9014;

export default configMerger(walttiConfig, {
    CONFIG,
    URL: {
        OTP: process.env.OTP_URL || `${API_URL}/routing/v1/router/`,
        MAP: {
            default: MAP_URL,
            satellite: 'https://tiles.stadtnavi.eu/orthophoto/{z}/{x}/{y}.jpg',
            semiTransparent: SEMI_TRANSPARENT_MAP_URL,
            bicycle: 'https://{s}.tile-cyclosm.openstreetmap.fr/cyclosm/{z}/{x}/{y}.png',
        },
        STOP_MAP: `${API_URL}/routing/v1/router/vectorTiles/stops/`,
        DYNAMICPARKINGLOTS_MAP: `${API_URL}/routing/v1/router/vectorTiles/parking/`,
        ROADWORKS_MAP: `${API_URL}/map/v1/cifs/`,
        CITYBIKE_MAP: `${API_URL}/routing/v1/router/vectorTiles/citybikes/`,
        BIKE_PARKS_MAP: `${API_URL}/routing/v1/router/vectorTiles/parking/`,
        WEATHER_STATIONS_MAP: `${API_URL}/map/v1/weather-stations/`,
        CHARGING_STATIONS_MAP: `${API_URL}/tiles/charging-stations/`,
        PELIAS: `${process.env.GEOCODING_BASE_URL || GEOCODING_BASE_URL}/search`,
        PELIAS_REVERSE_GEOCODER: `${
            process.env.GEOCODING_BASE_URL || GEOCODING_BASE_URL
        }/reverse`,
        PELIAS_PLACE: `${
            process.env.GEOCODING_BASE_URL || GEOCODING_BASE_URL
        }/place`,
        FARES: `${API_URL}/fares`,
        FONT: '' // Do not use Google fonts.
    },

    mainMenu: {
        showDisruptions: false,
    },

    availableLanguages: ['de', 'en'],
    defaultLanguage: 'de',

    /* disable the "next" column of the Route panel as it can be confusing sometimes: https://github.com/stadtnavi/digitransit-ui/issues/167 */
    displayNextDeparture: false,
    maxWalkDistance: 15000,

    optimize: "TRIANGLE",

    defaultSettings: {
        optimize: "TRIANGLE",
        safetyFactor: 0.4,
        slopeFactor: 0.3,
        timeFactor: 0.3,
    },

    defaultOptions: {
        walkSpeed: [0.83, 1.38, 1.94],
    },

    itinerary: {
        delayThreshold: 60,
    },

    appBarLink: {
        name: 'Feedback',
        href: 'https://stadtnavi.de/feedback',
        target: '_blank'
    },

    contactName: {
        de: 'transportkollektiv',
        default: 'transportkollektiv',
    },

    sprites: 'assets/svg-sprite.hb.svg',

    dynamicParkingLots: {
       showDynamicParkingLots: true,
        dynamicParkingLotsSmallIconZoom: 14,
        dynamicParkingLotsMinZoom: 14
    },

    bikeParks: {
        show: true,
        smallIconZoom: 14,
        minZoom: 14
    },

    roadworks: {
        showRoadworks: true,
        roadworksSmallIconZoom: 16,
        roadworksMinZoom: 10
    },

    weatherStations: {
        show: true,
        smallIconZoom: 17,
        minZoom: 15
    },

    chargingStations: {
        show: true,
        smallIconZoom: 14,
        minZoom: 14
    },

    cityBike: {
        minZoomStopsNearYou: 10,
        showStationId: false,
        useSpacesAvailable: false,
        showCityBikes: true,
        networks: {
            regiorad: {
                icon: 'regiorad',
                name: {
                    de: 'RegioRad',
                    en: 'RegioRad',
                },
                type: 'citybike',
                url: {
                    de: 'https://www.regioradstuttgart.de/de',
                    en: 'https://www.regioradstuttgart.de/',
                },
                visibleInSettingsUi: true,
            }
        }
    },

    mergeStopsByCode: true,

    title: APP_TITLE,

    favicon: './app/configurations/images/hbnext/favicon.png',

    meta: {
        description: APP_DESCRIPTION,
    },

    modeToOTP: {
        carpool: 'CARPOOL',
    },

    logo: 'hbnext/stadtnavi-logo.svg',

    GTMid: '',

    // get newest version from: https://github.com/moment/moment-timezone/blame/develop/data/packed/latest.json
    timezoneData: 'Europe/Berlin|CET CEST CEMT|-10 -20 -30|01010101010101210101210101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010|-2aFe0 11d0 1iO0 11A0 1o00 11A0 Qrc0 6i00 WM0 1fA0 1cM0 1cM0 1cM0 kL0 Nc0 m10 WM0 1ao0 1cp0 dX0 jz0 Dd0 1io0 17c0 1fA0 1a00 1ehA0 1a00 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1fA0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1fA0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1cM0 1fA0 1o00 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00 11A0 1qM0 WM0 1qM0 WM0 1qM0 WM0 1qM0 11A0 1o00 11A0 1o00|41e5',

    map: {
        useRetinaTiles: true,
        tileSize: 256,
        zoomOffset: 0,

        showZoomControl: true, // DT-3470, DT-3397
        showStreetModeSelector: false, // DT-3470
        showLayerSelector: true, // DT-3470
        showStopMarkerPopupOnMobile: false, // DT-3470
        showScaleBar: true, // DT-3470, DT-3397
        genericMarker: {
            popup: {
                offset: [0,0],
                maxWidth: 250,
                minWidth: 250,
            }
        },
        attribution: {
            'default': '© <a tabindex=-1 href=http://osm.org/copyright>OpenStreetMap Mitwirkende</a>, <a tabindex=-1 href=https://www.nvbw.de/aufgaben/digitale-mobilitaet/open-data/>Datensätze der NVBW GmbH</a> und <a tabindex=-1 href=https://www.openvvs.de/dataset/gtfs-daten>VVS GmbH</a>',
            'satellite': '© <a tabindex=-1 href=http://osm.org/copyright>OpenStreetMap Mitwirkende</a>, © <a tabindex=-1 href="https://www.lgl-bw.de/">LGL BW</a>, <a tabindex=-1 href=https://www.nvbw.de/aufgaben/digitale-mobilitaet/open-data/>Datensätze der NVBW GmbH</a> und <a tabindex=-1 href=https://www.openvvs.de/dataset/gtfs-daten>VVS GmbH</a>',
            'bicycle': '© <a tabindex=-1 href=http://osm.org/copyright>OpenStreetMap Mitwirkende</a>, © <a tabindex=-1 href=https://www.cyclosm.org/#map=12/52.3728/4.8936/cyclosmx>CyclOSM</a>, © <a tabindex=-1 href="https://www.openstreetmap.fr/">OSM-FR</a>, <a tabindex=-1 href=https://www.nvbw.de/aufgaben/digitale-mobilitaet/open-data/>Datensätze der NVBW GmbH</a> und <a tabindex=-1 href=https://www.openvvs.de/dataset/gtfs-daten>VVS GmbH</a>',
        },
    },

    feedIds: ['hbg'],

    realtime: { hbg: realtimeHbg },

    searchSources: ['oa', 'osm'],

    searchParams: {
        'boundary.rect.min_lat': 48.34164,
        'boundary.rect.max_lat': 48.97661,
        'boundary.rect.min_lon': 9.95635,
        'boundary.rect.max_lon': 8.530883,
        'focus.point.lat': 48.5957,
        'focus.point.lon': 8.8675
    },

    areaPolygon: [
        [minLon, minLat],
        [minLon, maxLat],
        [maxLon, maxLat],
        [maxLon, minLat],
    ],

    nationalServiceLink: { name: 'Fahrplanauskunft efa-bw', href: 'https://www.efa-bw.de' },

    defaultEndpoint: {
        lat: 48.4929,
        lon: 9.208,
    },


    menu: {
        copyright: {
            label: `© Stadt Herrenberg ${YEAR}`
        },
        content: [
            {
                name: 'about-this-service',
                nameEn: 'About this service',
                route: '/dieser-dienst',
                icon: 'icon-icon_info',
            },
            {
                name: 'imprint',
                nameEn: 'Imprint',
                href: 'https://www.herrenberg.de/impressum',
            },
            {
                name: 'privacy',
                nameEn: 'Privacy',
                href: 'https://www.herrenberg.de/datenschutz',
            },
        ],
    },

    aboutThisService: {
        de: [
            {
                header: 'Über diesen Dienst',
                paragraphs: [
                    'stadtnavi ist eine Reiseplanungs-Anwendung für die Stadt Herrenberg und Umgebung. Dieser Dienst umfasst ÖPNV, Fußwege, Radverkehr, Straßen- und Parkplatzinformationen, Ladeinfrastruktur und Sharing-Angebote. Mobilitätsangebote werden durch intermodales Routing miteinander vernetzt.',
                    'Gefördert durch <br>',
                    '<a href="https://www.herrenberg.de/stadtluft"><img src="https://www.herrenberg.de/ceasy/resource/?id=4355&predefinedImageSize=rightEditorContent"/></a>',

                ],
            },
            {
                header: 'Mitmachen',
                paragraphs: [
                    'Die Stadt Herrenberg hat diese App im Rahmen der Modellstadt, gefördert durch das Bundesministerium für Verkehr und digitale Infrastruktur (BMVI) entwickelt. stadtnavi Anwendung ist eine Open Source Lösung und kann von anderen Kommunen und Akteuren unter ihrem Namen und Erscheinungsbild verwendet und an individuelle Bedürfnisse angepasst und weiterentwickelt werden (White Label Lösung). Mitmachen ist gewünscht!',
                ]
            },
            {
                header: 'Digitransit Plattform',
                paragraphs: [
                    'Dieser Dienst basiert auf der Digitransit Platform und dem Backend-Dienst OpenTripPlanner. Alle Software ist unter einer offenen Lizenzen verfügbar. Vielen Dank an alle Beteiligten.',
                    'Der gesamte Quellcode der Plattform, die aus vielen verschiedenen Komponenten besteht, ist auf <a href="https://github.com/stadtnavi/">Github</a> verfügbar.'
                ],
            },
            {
                header: 'Datenquellen',
                paragraphs: [
                    'Kartendaten: © <a target=new href=https://www.openstreetmap.org/>OpenStreetMap Mitwirkende</a>',
                    'ÖPNV-Daten: Datensätze der <a target=new href=https://www.nvbw.de/aufgaben/digitale-mobilitaet/open-data/>NVBW GmbH</a> und der <a target=new href=https://www.openvvs.de/dataset/gtfs-daten>VVS GmbH</a>, Shapes (d.h. Geometrien der Streckenverläufe) jeweils angereichert mit OpenStreetMap-Daten © OpenStreetMap Mitwirkende',
                    'Alle Angaben ohne Gewähr.'
                ],
            },
        ],
        en: [
            {
                header: 'About this service',
                paragraphs: [
                    'stadtnavi is a travel planning application for the city of Herrenberg and its surroundings. This service includes public transport, footpaths, cycling, street and parking information, charging infrastructure and sharing offerings. The mobility offerings are connected through intermodal routing.',
                    '<a href="https://www.herrenberg.de/stadtluft"><img src="https://www.herrenberg.de/ceasy/resource/?id=4355&predefinedImageSize=rightEditorContent"/></a>',
                ],
            },
            {
                header: 'Contribute',
                paragraphs: [
                    'The city of Herrenberg has developed this app, funded by the Federal Ministry of Transport and Digital Infrastructure (BMVI), as model city. The stadtnavi app is an open source solution and can be used, customized and further developed by other municipalities to meet individual needs (white lable solution). Participation is welcome!',
                ]
            },
            {
                header: 'Digitransit platform',
                paragraphs: [
                    'The Digitransit service platform is an open source routing platform developed by HSL and Traficom. It builds on OpenTripPlanner by Conveyal. Enhancements by Transportkollektiv and MITFAHR|DE|ZENTRALE. All software is open source. Thanks to everybody working on this!',
                ],
            },
            {
                header: 'Data sources',
                paragraphs: [
                    'Map data: © <a target=new href=https://www.openstreetmap.org/>OpenStreetMap contributors</a>',
                    'Public transit data: Datasets by <a target=new href=https://www.nvbw.de/aufgaben/digitale-mobilitaet/open-data/>NVBW GmbH</a> and <a target=new href=https://www.openvvs.de/dataset/gtfs-daten>VVS GmbH</a>, Shapes (d.h. Geometrien der Streckenverläufe) enhanced with OpenStreetMap data © OpenStreetMap contributors',
                    'No responsibility is accepted for the accuracy of this information.'
                ],
            },
        ],
    },

    redirectReittiopasParams: true,

    showTicketInformation: true,
    showTicketPrice: true,
    availableTickets: { 'hbg' : {}},
    fareMapping: function mapHbFareId(fareId) {
        return {
            en: "Adult",
            de: "Regulär",
        };
    },
    displayFareInfoTop: false,

    showRouteSearch: false,
    showNearYouButtons: false,

    // adding assets/geoJson/hb-layers layers
    geoJson: {
        layers: [
            // bicycleinfrastructure includes shops, repair stations,
            {
                name: {
                    fi: '',
                    en: 'Service stations and stores',
                    de: "Service Stationen und Läden",
                },
                url: '/assets/geojson/hb-layers/bicycleinfrastructure.geojson',
            },
            /* Charging stations
            {
                name: {
                    fi: '',
                    en: 'Charging stations',
                    de: 'Ladestationen',
                },
                url: '/assets/geojson/hb-layers/charging.geojson',
            },*/
            // LoRaWan map layer
            {
                name: {
                    fi: '',
                    en: 'LoRaWAN Gateways',
                    de: 'LoRaWAN Gateways',
                },
                url: '/assets/geojson/hb-layers/lorawan-gateways.geojson',
                isOffByDefault: true,
            },
            // Nette Toilette layer
            {
                name: {
                    fi: '',
                    en: 'Public Toilets',
                    de: 'Nette Toilette',
                },
                url: '/assets/geojson/hb-layers/toilet.geojson',
                isOffByDefault: true,
            },
        ],
    },
    staticMessagesUrl: STATIC_MESSAGE_URL,


    suggestCarMinDistance: 800,
    suggestWalkMaxDistance: 3000,
    suggestBikeAndPublicMinDistance: 3000,
    suggestBikeAndParkMinDistance: 3000,

    // live bus locations
    vehicles: true,
    showVehiclesOnSummaryPage: false,
    showVehiclesOnStopPage: true,

    showBikeAndPublicItineraries: true,
    showBikeAndParkItineraries: true,
    showStopAndRouteSearch: false,
    showTimeTableOptions: false,

    viaPointsEnabled: false,
});

    ```
 
### 6. Show results

To start the application in production mode:
  - run `yarn run build`
  THEN
  - run `yarn run start`

To start the application in development mode:
  - run `yarn run dev`

Environment variables:
  - CONFIG=<...>
  - API_URL=<...>
  - MAP_URL=<...>
  - OTP_URL=<...>
  - GEOCODING_BASE_URL=<...>
  - ASSET_URL=<...>

Running stadtnavi instance in dev/prod mode: 
  - `CONFIG=rt yarn run dev` (development mode)
  OR
  - `yarn build` then `CONFIG=rt yarn run start` (production mode)

### 7. Running in Docker
  1. build a docker image, run: `docker build -t stadtnavi/digitransit-ui .` (NOTE the "." at the end)
  2. run the image: `docker run -p 8080:8080 -e CONFIG=rt stadtnavi/digitransit-ui`
    - any environment variable can be specified after the `-e` option
    - more information [here](https://github.com/HSLdevcom/digitransit-ui/blob/master/docs/Docker.md)

### More information
visit: https://github.com/HSLdevcom/digitransit-ui
