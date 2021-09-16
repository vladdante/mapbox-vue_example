<template>
  <div id="map"/>
</template>

<script>
  import mapboxgl from "mapbox-gl"
  import { token } from "../private_mapbox_config.json"

  export default {
    name: 'App',
    data: () => ({
      map: {
        style: "mapbox://styles/mapbox/streets-v11",
        center: [37.62, 55.75],
        zoom: 16
      },
      shape: {
        coordinates: [[
          [37.618610077382044, 55.750976522976856],
          [37.616848273079, 55.749336368113006],
          [37.62308744471318, 55.74973895796529],
          [37.621497846844335, 55.75248242262731],
          [37.618610077382044, 55.750976522976856]
        ]],
        color: "#ff00fb",
        opacity: 0.5
      }
    }),
    mounted() {
      mapboxgl.accessToken = token

      const map = new mapboxgl.Map({
        container: "map",
        style: this.map.style,
        center: this.map.center,
        zoom: this.map.zoom
      })

      map.on("load", () => {
        map.addSource("shape", {
          "type": "geojson",
          "data": {
            "type": "Feature",
            "geometry": {
              "type": "Polygon",
              "coordinates": this.shape.coordinates
            }
          }
        })
        map.addLayer({
          "id": "shape",
          "type": "fill",
          "source": "shape",
          "paint": {
            "fill-color": this.shape.color,
            "fill-opacity": this.shape.opacity
          }
        })
      })
    }
  }
</script>

<style>
  body { margin: 0; overflow: hidden; }

  .mapboxgl-canvas { height: 100vh !important; width: 100vw !important; }

  .mapboxgl-control-container { display: none; }
</style>
