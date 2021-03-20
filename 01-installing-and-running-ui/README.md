## Customize stadtnavi Frontend, using existing back-end services
### 1. Requirements, installation
    - docker
    - git
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


### 3. Doing an initial build an see it works

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
```
The warnings are "ok".

To test the installation run:
`yarn run dev`

In the console you will see this message `Digitransit-ui available on port 8080`. Open http://localhost:8080/ and wait for the initial loading to finish. 

### 4. Starting a new config 
  - to add a new theme run: `yarn run add-theme <new_theme>`
  - for example `yarn run add-theme rt`
  - 3 file should be created and 1 modified:
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
    const APP_TITLE = 'Reutlingen';
    const APP_DESCRIPTION = 'stadtnavi - reutlingen';
    const API_URL = process.env.API_URL || 'https://api.stadtnavi.de';
    const MAP_URL = process.env.MAP_URL || 'https://tiles.stadtnavi.eu/streets/{z}/{x}/{y}{r}.png';
    const SEMI_TRANSPARENT_MAP_URL = process.env.SEMI_TRANSPARENT_MAP_URL ||         'https://api.maptiler.com/maps/ffa4d49e-c68c-46c8-ab3f-60543337cecb/256/{z}/{x}/{y}.png?key=eA0drARBA1uPzLR6StGD';
    const GEOCODING_BASE_URL = process.env.GEOCODING_BASE_URL || "https://photon.stadtnavi.eu/pelias/v1";
    const LOCATIONIQ_API_KEY = process.env.LOCATIONIQ_API_KEY;
    const YEAR = 1900 + new Date().getYear();
    const STATIC_MESSAGE_URL =
    process.env.STATIC_MESSAGE_URL ||
    '/assets/messages/message.hb.json';

    const walttiConfig = require('./config.waltti.js').default;

    const minLat = 48.395;
    const maxLat = 48.6075;
    const minLon = 9.0901;
    const maxLon = 9.3616;

    export default configMerger(walttiConfig, {
      CONFIG,
      URL: {
        OTP: process.env.OTP_URL || `${API_URL}/routing/v1/router/`,
        MAP: {
          default: MAP_URL,
          satellite: `${API_URL}/tiles/orthophoto/{z}/{x}/{y}.jpg`,
          semiTransparent: SEMI_TRANSPARENT_MAP_URL,
          bicycle: 'https://{s}.tile-cyclosm.openstreetmap.fr/cyclosm/{z}/{x}/{y}.png',
        },
        STOP_MAP: `${API_URL}/map/v1/stop-map/`,
        GEOCODING_BASE_URL: GEOCODING_BASE_URL,
        DYNAMICPARKINGLOTS_MAP: `${API_URL}/map/v1/hb-parking-map/`,
        ROADWORKS_MAP: `${API_URL}/map/v1/cifs/`,
        COVID19_MAP: `https://tiles.caresteouvert.fr/public.poi_osm_light/{z}/{x}/{y}.pbf`,
        CITYBIKE_MAP: `${API_URL}/map/v1/regiorad-map/`,
        WEATHER_STATIONS_MAP: `${API_URL}/map/v1/weather-stations/`,
        CHARGING_STATIONS_MAP: `${API_URL}/map/v1/charging-stations/`,
        BUS_POSITIONS_MAP: `${API_URL}/map/v1/bus-positions/`
      },

      appBarLink: false,

      availableLanguages: ['de', 'en'],
      defaultLanguage: 'de',

      mainMenu: {
        showDisruptions: false,
      },

      logo: 'hb/stadtnavi-logo.svg',

      colors: {
        primary: '#ff5300',
      },

      socialMedia: {
        title: APP_TITLE,
        description: APP_DESCRIPTION,
      },

      title: APP_TITLE,

      textLogo: false,

      feedIds: ['Reutlingen'],

      searchParams: {
        'boundary.rect.min_lat': minLat,
        'boundary.rect.max_lat': maxLat,
        'boundary.rect.min_lon': minLon,
        'boundary.rect.max_lon': maxLon,
      },
      
      map: {
        useRetinaTiles: true,
        tileSize: 256,
        zoomOffset: 0
      },

      areaPolygon: [
        [minLon, minLat],
        [minLon, maxLat],
        [maxLon, maxLat],
        [maxLon, minLat],
      ],

      defaultEndpoint: {
        address: 'Reutlingen',
        lat: 0.5 * (minLat + maxLat),
        lon: 0.5 * (minLon + maxLon),
      },

      defaultOrigins: [
        {
          icon: 'icon-icon_bus',
          label: 'Linja-autoasema, Reutlingen',
          lat: 63,
          lon: 27,
        },
      ],

      footer: {
        content: [
          { label: `© Reutlingen ${walttiConfig.YEAR}` },
          {},
          {
            name: 'about-this-service',
            nameEn: 'About this service',
            route: '/tietoja-palvelusta',
            icon: 'icon-icon_info',
          },
        ],
      },

      aboutThisService: {
        fi: [
          {
            header: 'Tietoja palvelusta',
            paragraphs: [
              'Tämän palvelun tarjoaa Reutlingen reittisuunnittelua varten Reutlingen alueella. Palvelu kattaa joukkoliikenteen, kävelyn, pyöräilyn ja yksityisautoilun rajatuilta osin. Palvelu perustuu Digitransit-palvelualustaan.',
            ],
          },
        ],

        sv: [
          {
            header: 'Om tjänsten',
            paragraphs: [
              'Den här tjänsten erbjuds av Reutlingen för reseplanering inom Reutlingen region. Reseplaneraren täcker med vissa begränsningar kollektivtrafik, promenad, cykling samt privatbilism. Tjänsten baserar sig på Digitransit-plattformen.',
            ],
          },
        ],

        en: [
          {
            header: 'About this service',
            paragraphs: [
              'This service is provided by Reutlingen for route planning in Reutlingen region. The service covers public transport, walking, cycling, and some private car use. Service is built on Digitransit platform.',
            ],
          },
        ],
      },
      // geojson config:
      
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