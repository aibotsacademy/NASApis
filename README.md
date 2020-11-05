# 🤖 [NASA Space Apps 2020](https://2020.spaceappschallenge.org/locations/san-jose/event) 🛰️ 
### Getting started with the NASA APIs and Build a Web App using Satellite Images & Earth Observation Data for the Space Apps challenge 2020
###### Developer: Leo Camacho, Space Apps Local Lead in San Jose.
###### ||Data Sources: [NASA API](https://api.nasa.gov/), [GIBS API](https://wiki.earthdata.nasa.gov/display/GIBS/GIBS+API+for+Developers). ||
###### ||Global Competition 🌎 || [2020 Challenges Categories](https://2020.spaceappschallenge.org/challenges/): Observe, Inform, Sustain, Create, Confront, Connect or Invent your own challenge! || 

[![Pitch](https://img.youtube.com/vi/6D3bOMDwhJA/0.jpg)](https://youtu.be/6D3bOMDwhJA)

#### In this workshop we will learn more about using the NASA APIs for the Web and access data such as: Satellite Imagery, Earth Data Observation, Climate models, the Moon, Rover mission in Mars and many others! 

![Test Image 4](https://github.com/aibotsacademy/IntroAPIsNASA/blob/master/IMG/model.png)

#### Let's start!

> Step 1: Create an account in NASA Open APIs: https://api.nasa.gov/

> Step 2: Generate your credentials and get your API Key:

```
 https://api.nasa.gov/planetary/apod?api_key=YOUR_API_KEY
```
Try the Earth Observation API:
```
https://api.nasa.gov/planetary/earth/assets?lon=-98.83&lat=19.70&date=2020-01-01&&dim=0.10&api_key=DEMO_KEY
```

Try the Rover Mission in Mars API:
```
https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos?earth_date=2020-1-2&api_key=DEMO_KEY
```

![Mars](https://github.com/aibotsacademy/IntroAPIsNASA/blob/master/IMG/MARS.jpg) 
> Step 3: [Welcome](sM9dReXlARhfcp9ctZGxUt8wItACbqJTLMCW3YiI) to the Global Imagery Browsing Services: [GIBS API](https://earthdata.nasa.gov/eosdis/science-system-description/eosdis-components/gibs), this API is recommended for projects interested on building Machine Learning, Mixed Reality and other models, based on Satellite Images provided from missions such as [Terra](https://www.nasa.gov/mission_pages/terra/spacecraft/index.html). located in a circular sun-synchronous polar orbit that takes it from north to south (on the daylight side of the Earth) every 99 minutes.

-Step 4: Download Demo > [SpaceApps2020](https://github.com/leoaiassistant/NASA_APIs_SpaceApps2020/)


-Step 5: Integrate GIBS API with the Web:
 
This project shows how to use the GIBS API to visualize data layers of satellite image on-board the 🛰️ Satellite Terra & Aqua in the low Earth orbit, for uses for example of analysis and have a better understanding of Climate events, Natural Disasters, also applied to Agriculture, Oceans and many other events and variables, through the meassurements and data generated by the  instrument [MODIS](https://wiki.earthdata.nasa.gov/display/GIBS/GIBS+Available+Imagery+Products#expand-CorrectedReflectance17Products) that tracks a wide range of vital signs of the earth 🌎  such as:

- surface temperature (ground and ocean), fire detection
- ocean color (sediments, phytoplankton)
- global vegetation mapping, change detection
- characteristics of cloudiness
- aerosol concentrations

This project shows how to use GIBS as a tile source for Satellite Images in OpenLayers. We use NASA's Global Image Exploration services [GIBS API](https://wiki.earthdata.nasa.gov/display/GIBS/GIBS+API+for+Developers) as a provider of a pyramid of image mosaics satellites (from the OGC WMTS 1.0.0 specification) and visualize the data layers of the *** Web Map Mosaic Service (WMTS) *** based on parameters such as: date, tile, lat&long, layer, resolution, zoom and other parameters. 

- Include the following functions on your ***Main.js***
```
window.onload = function () {
  // Seven day slider based off today, remember what today is
  var today = new Date();

  // Selected day to show on the map
  var day = new Date(today.getTime());

  // GIBS needs the day as a string parameter in the form of YYYY-MM-DD.
  // Date.toISOString returns YYYY-MM-DDTHH:MM:SSZ. Split at the 'T' and
  // take the date which is the first part.
  var dayParameter = function () {
    return day.toISOString().split('T')[0];
  };

  var map = new ol.Map({
    view: new ol.View({
      maxResolution: 0.5625,
      projection: ol.proj.get('EPSG:4326'),
      extent: [-180, -90, 180, 90],
      center: [-80.87, 24.21],
      zoom: 6,
      maxZoom: 8
    }),
    target: 'map',
    renderer: ['canvas', 'dom']
  });

  function update() {
    // There is only one layer in this example, but remove them all
    // anyway
    clearLayers();

    // Add the new layer for the selected time
    map.addLayer(createLayer());

    // Update the day label
    document.querySelector('#day-label').textContent = dayParameter();
  };

  function clearLayers() {
    // Get a copy of the current layer list and then remove each
    // layer.
    var activeLayers = map.getLayers().getArray();
    for (var i = 0; i < activeLayers.length; i++) {
      map.removeLayer(activeLayers[i]);
    }
  };

  function createLayer() {
    var source = new ol.source.WMTS({
      url: 'https://gibs-{a-c}.earthdata.nasa.gov/wmts/epsg4326/best/wmts.cgi?TIME=' + dayParameter(),
      layer: 'MODIS_Terra_SurfaceReflectance_Bands121',
      format: 'image/jpeg',
      matrixSet: 'EPSG4326_250m',
      tileGrid: new ol.tilegrid.WMTS({
        origin: [-193, 94],
        resolutions: [
          0.5625,
          0.28125,
          0.140625,
          0.0703125,
          0.03515625,
          0.017578125,
          0.0087890625,
          0.00439453125,
          0.002197265625
        ],
        matrixIds: [0, 1, 2, 3, 4, 5, 6, 7, 8],
        tileSize: 512
      })
    });

    var layer = new ol.layer.Tile({ source: source });
    return layer;
  };

  update();

  // Slider values are in 'days from present'.
  document.querySelector('#day-slider')
    .addEventListener('change', function (event) {
      // Add the slider value (effectively subracting) to today's
      // date.
      var newDay = new Date(today.getTime());
      newDay.setUTCDate(today.getUTCDate() +
        Number.parseInt(event.target.value));
      day = newDay;
      update();
    });
};
```


- URL: https://gibs.earthdata.nasa.gov/wmts/
- Layer: (e.g. "MODIS / Terra")
- Matrix Set:'EPSG_(n)m'
- Origin:[Lat, Long]
- Resolution:[0...2]
- Tile: Open Layers

> Create Layer function:

```
 function createLayer() {
    var source = new ol.source.WMTS({
      url: 'https://gibs.earthdata.nasa.gov/wmts/epsg4326/best/wmts.cgi?SERVICE='
      layer: 'MODIS_Terra_SurfaceReflectance_Bands121',
      format: 'image/jpeg',
      matrixSet: 'EPSG_(N)m',
      tileGrid: new ol.tilegrid.WMTS({
        origin: [Lat, Long],
        resolutions: [
          0...(n)
        ],
        matrixIds: [0, 1, 2, 3, 4, 5, 6, 7, 8],
        tileSize: 512
      })
    });
```

Key Terms:

***MODIS (or Moderate Resolution Imaging Spectroradiometer)***: is the instrumnet on board of the satellite Terra (originally known as EOS AM-1) y Aqua (originalmente conocido como EOS PM-1). 


> What's next? El Reto!
The challenge is that with all this resources, you can create an impactfull solutions for your city, country or the world!

+Tutorials:
- Intro to Web Development & NASA API: Mars Rover Photos!
- By Brandon Escamilla, Space Apps Lead en México

[![Brandon Escamilla](https://img.youtube.com/vi/KcyGr_onNiM/1.jpg)](https://youtu.be/KcyGr_onNiM)

+Resources:

[Satelite Terra](https://terra.nasa.gov/about/terra-instruments/modis)

[MODIS](https://modis.gsfc.nasa.gov/data/)
 
[Open Layers](https://openlayers.org/)

[Earth Data Layers](https://wiki.earthdata.nasa.gov/display/GIBS/GIBS+Available+Imagery+Products#expand-CorrectedReflectance17Products)
 
[Earth Observation](https://earthdata.nasa.gov/earth-observation-data/near-real-time/download-nrt-data/modis-nrt
)
[GIBS API](https://wiki.earthdata.nasa.gov/display/GIBS/GIBS+API+for+Developers#GIBSAPIforDevelopers-ImageryAPI/Services)
