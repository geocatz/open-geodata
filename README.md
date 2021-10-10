# open-geodata
A collection of interesting and open data sources available for public use.

## Schema

Each data source (found in `dataSources.json`) looks something like this:

```json
{
  "id": "NASA_COUNTRY_BOUNDARIES",
  "url": "https://data.nasa.gov/api/geospatial/7zbq-j77a?method=export&format=KML",
  "title": "NASA Geopolitcal Boundaries"
}
```

`id` - a unique identifier that can be used to find a layer on a map
`url` - a url to be passed into `loadDataLayer`
`title` - a human-readable titleription of the data served by the above `url`
`src` - (optional) a URL linking to a website containing human-readable information

## Usage
It's recommended you use this package with our mapping framework [geokit](https://github.com/geocatz/geokit). To use it, see the example below:

```jsx
import React from 'react'
import {
  Map,
  BasemapContainer,
  ContextMenu,
  Controls,
  LayerPanel,
  Popup,
  loadDataLayer
} from '@bayer/ol-kit'

class App extends React.Component {

  onMapInit = async map => {
    // nice to have map set on the window while debugging
    window.map = map

    const worldBoundaries = {
      "id": "NASA_COUNTRY_BOUNDARIES",
      "url": "https://data.nasa.gov/api/geospatial/7zbq-j77a?method=export&format=KML",
      "title": "NASA Geopolitcal Boundaries"
    }

    // put the data on the map and return a reference to the openlayers layer
    const dataLayer = await loadDataLayer(map, worldBoundaries.url)

    // set the title and ID on the layer to show in LayerPanel
    dataLayer.set('title', worldBoundaries.title)
    dataLayer.set('id', worldBoundaries.id)
  }

  render () {
    return (
      <Map onMapInit={this.onMapInit} fullScreen>
        <BasemapContainer />
        <ContextMenu />
        <Controls />
        <LayerPanel />
        <Popup />
      </Map>
    )
  }
}

export default App
```