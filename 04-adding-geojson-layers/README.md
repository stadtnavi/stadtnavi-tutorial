### Adding custom map Layer
We use geojson files to render static map layers(e.g. bike park layer). Downloading and modifying these files require a lot of repetetive work, that is why we implemented automated python scripts. These scripts are in the `digitransit-overpass-layers` repository. By default they are configured for Herrenberg but with some environmental variables, they can be used for any other city. In the following steps, we show how to download and customize geojson files, using the python scripts, and then how to add it to the digitransit-ui.

Requirement: python3.6 or newer installed

1. clone the `digitransit-overpass-layers` repository to the same folder where `digitransit-ui` is located
  - run: `git clone https://github.com/stadtnavi/digitransit-overpass-layers.git`
  - then: `cd digitransit-overpass-layers`
2. set some environmental variable
  - `ENV_BBOX` specifies the place where the overpass-turbo API should look for different POIs
  - `ENV_DDIR` is used to declare the target folder, where the geojson files should be downloaded
  - `ENV_ICNSRC` declares the directory of the icons for each layer
First lets specify the `ENV_BBOX`:
```
$ export ENV_BBOX="48.395,9.0901,48.6075,9.3616"
$ echo $ENV_BBOX
48.395,9.0901,48.6075,9.3616
```
then the `ENV_DDIR`:
```
$ export ENV_DDIR=../digitransit-ui/static/assets/geojson/rt-layers/
$ echo $ENV_DDIR 
../digitransit-ui/static/assets/geojson/rt-layers/
```
3. install python packages
`pip install overpass osmtogeojson`
4. run the scripts
`python generate-layers.py`
*Note: If you get `MultipleRequestError` just try to re-run the script if it does not help download the data from the stadnavi-tutorial repository. The files are located at `04-adding-geojson-layers/` from where you can copy the `rt-layers` folder to `digitransit-ui/static/assets/geojson/`.*
5. add some configuration
paste the following code to the end of `app/configurations/config.rt.js` file after the comment '// geojson config:'
```
  geoJson: {
    layers: [
      // bicycleinfrastructure includes shops, repair stations,
      {
        name: {
          fi: '',
          en: 'Bicycle parking',
          de: "Fahrradstellplatz",
        },
        url: '/assets/geojson/rt-layers/bicycle-parking.geojson',
      },
      {
        name: {
          fi: '',
          en: 'Bicycle infrastructure',
          de: "Rund um's Fahrrad",
        },
        url: '/assets/geojson/rt-layers/bicycleinfrastructure.geojson',
      },
      {
        name: {
          fi: '',
          en: 'Car park',
          de: "Autoparkplatz",
        },
        url: '/assets/geojson/rt-layers/car-parking.geojson',
      },
      // Charging stations
      {
        name: {
          fi: '',
          en: 'Charging stations',
          de: 'Ladestationen',
        },
        url: '/assets/geojson/rt-layers/charging.geojson',
      },
      // Nette Toilette layer
      {
        name: {
          fi: '',
          en: 'Public Toilets',
          de: 'Nette Toilette',
        },
        url: '/assets/geojson/rt-layers/toilet.geojson',
        isOffByDefault: true,
      },
    ],
  }
```
6. run the application
`$ CONFIG=rt yarn run dev`
7. check the layers
Now you should be able to enable or disable each layer from the map layers selector.