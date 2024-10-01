## Customize stadtnavi Frontend, using existing back-end services

This introduction will use a node:20 docker image to avoid a potentially tedious setup.

### 1. Requirements, installation
    - docker
    - git
    
### Installing Docker (more info [here](https://docs.docker.com/engine/install/debian/))
- don't forget to uninstall conflicting packages
- use the [Docker repository](https://docs.docker.com/engine/install/debian/#install-using-the-repository)
- create a group and add user https://docs.docker.com/engine/install/linux-postinstall/
- try `docker run hello-world` and `docker --version`


### Installing git and cURL:
`apt-get install git curl -y`

check installation (exact versions may differ):
```
$ git --version
git version 2.39.2
```

### 2. Checking out digitransit

In your local projects directory, checkout the digitransit-ui:

```
$ git clone https://github.com/stadtnavi/digitransit-ui.git
Cloning into 'digitransit-ui'...
remote: Enumerating objects: 190376, done.
remote: Counting objects: 100% (4619/4619), done.
remote: Compressing objects: 100% (1604/1604), done.
remote: Total 190376 (delta 3173), reused 4243 (delta 3004), pack-reused 185757
Receiving objects: 100% (190376/190376), 202.79 MiB | 7.42 MiB/s, done.
Resolving deltas: 100% (136603/136603), done.
```

### 3. Installing dependencies and running digitransit

To install digitransit and it's dependencies, run

```
$ docker run -ti --rm -p 8080:8080 -v $PWD/digitransit-ui:/digitransit-ui node:20 /bin/bash 

# cd digitransit-ui
# git checkout next
# yarn install
➤ YN0065: Yarn will periodically gather anonymous telemetry: https://yarnpkg.com/advanced/telemetry
➤ YN0065: Run yarn config set --home enableTelemetry 0 to disable

➤ YN0000: ┌ Resolution step
➤ YN0002: │ @digitransit-component/digitransit-component@workspace:digitransit-component/packages/digitransit-component doesn't provide @digitransit-component/digitransit-component-dialog-modal (p4d433), requested by @digitransit-component/digitransit-component-autosuggest
➤ YN0002: │ @digitransit-component/digitransit-component@workspace:digitransit-component/packages/digitransit-component doesn't provide @digitransit-component/digitransit-component-dialog-modal (p03091), requested by @digitransit-component/digitransit-component-favourite-editing-modal
➤ YN0002: │ @digitransit-component/digitransit-component@workspace:digitransit-component/packages/digitransit-component doesn't provide @hsl-fi/container-spinner (p20867), requested by @digitransit-component/digit
...
➤ YN0000: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code
➤ YN0000: └ Completed
➤ YN0000: ┌ Fetch step
➤ YN0013: │ yazl@npm:2.5.1 can't be found in the cache and will be fetched from the remote registry
...


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
The warnings are "ok" (well, rather currently expected than ok).

To test the installation run:
`CONFIG=herrenberg yarn run dev`

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
const APP_TITLE = 'stadtnavi Reutlingen';
const APP_DESCRIPTION = 'Gemeinsam Mobilität neu denken - die intermodale Verbindungssuche mit offenen, lokalen Daten';
const API_URL = process.env.API_URL || 'https://api.stadtnavi.de';
const MAP_URL = process.env.MAP_URL || 'https://tiles.stadtnavi.eu/streets/{z}/{x}/{y}{r}.png';
const SEMI_TRANSPARENT_MAP_URL = process.env.SEMITRANSPARENT_MAP_URL || "https://tiles.stadtnavi.eu/satellite-overlay/{z}/{x}/{y}{r}.png";
const GEOCODING_BASE_URL = process.env.GEOCODING_BASE_URL || "https://photon.stadtnavi.eu/pelias/v1";
const YEAR = 1900 + new Date().getYear();
const STATIC_MESSAGE_URL =
    process.env.STATIC_MESSAGE_URL ||
    '/assets/messages/message.empty.json';

const parentConfig = require('./config.stadtnavi.js').default;

const minLat = 47.6020;
const maxLat = 49.0050;
const minLon = 8.4087;
const maxLon = 9.9014;

export default configMerger(parentConfig, {
    CONFIG,

    colors: {
        primary: '#333333',
    },

    socialMedia: {
        title: APP_TITLE,
        description: APP_DESCRIPTION,

        image: {
            url: '/img/stadtnavi-social-media-card.png',
            width: 600,
            height: 300,
        }
    },
    
    title: APP_TITLE,
    
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

    cityBike: {
        minZoomStopsNearYou: 10,
        showStationId: false,
        useSpacesAvailable: false,
        showCityBikes: true,
        networks: {
           bolt_reutlingen_tuebingen: {
             icon: "brand_bolt",
             operator: "bolt",
             name: {
               de: "Bolt OÜ"
             },
             type: "scooter",
             form_factors: ['scooter', 'bicycle'],
             hideCode: true,
             enabled: true,
             url: {
               de: "https://www.bolt.eu/"
             }
           }
        }
    },

    staticMessagesUrl: STATIC_MESSAGE_URL,

    // no live bus locations
    vehicles: false,
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

### 7. Building a Docker image

When your happy with your changes, you may quit the docker container (via `exit`), and build docker image we will reuse in subsequent tutorial steps.

  1. In the `digitransit-ui directory, build a docker image, run: `docker build -t stadtnavi/digitransit-ui .` (NOTE the "." at the end)
  2. run the image: `docker run -p 8080:8080 -e CONFIG=rt stadtnavi/digitransit-ui`
    - any environment variable can be specified after the `-e` option
    - more information [here](https://github.com/HSLdevcom/digitransit-ui/blob/master/docs/Docker.md)

### More information
visit: https://github.com/HSLdevcom/digitransit-ui
