<template>
  <div id="container">
    <div id="mapContainer"></div>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import L from "leaflet";
import "leaflet.vectorgrid"

export default {
  name: "Map",
  data() {
    return {
      center: [-0.789275,113.921327],
      zoom: 5,
      map: null
    };
  },
  methods: {
    setupLeafletMap () {
      this.map = L.map("mapContainer").setView(this.center, this.zoom);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(this.map)

      var vectorTileOptions = {
        vectorTileLayerStyles: {
            // A plain set of L.Path options.
            jabar3: {
                weight: 1,
                fillColor: '#9e2a2b',
                fillOpacity: 1,
                fill: true,
                color: 'black'
            },
            jateng: {
                weight: 1,
                fillColor: '#e09f3e',
                fillOpacity: 1,
                fill: true,
                color: 'black'
            },
            jatim: {
                weight: 1,
                fillColor: '#335c67',
                fillOpacity: 1,
                fill: true,
                color: 'black'
            },
          }
      }
      // var vectorTileOptions = {
      //   interactive: true, pane: 'OverlayPane',
      //   vectorTileLayerStyles: {
      //       jabar3: {
      //           weight: 0,
      //           fillColor: '#9bc2c4',
      //           fillOpacity: 1,
      //           fill: true
      //       }
      //   }
      // };
      // L.vectorGrid.protobuf("http://localhost:8080/geoserver/gwc/service/wmts?REQUEST=GetTile&SERVICE=WMTS&VERSION=1.0.0&LAYER=JawaBarat:jabar3&STYLE=&TILEMATRIX=EPSG:4326:{z}&TILEMATRIXSET=EPSG:4326&FORMAT=application/vnd.mapbox-vector-tile&TILECOL={x}&TILEROW={y}", vectorTileOptions).addTo(this.map);
      L.vectorGrid.protobuf("http://localhost:8080/geoserver/gwc/service/tms/1.0.0/JawaBarat:jabar3@EPSG%3A900913@pbf/{z}/{x}/{-y}.pbf", vectorTileOptions).addTo(this.map);
      L.vectorGrid.protobuf("http://localhost:8080/geoserver/gwc/service/tms/1.0.0/JawaTengah:jateng@EPSG%3A900913@pbf/{z}/{x}/{-y}.pbf", vectorTileOptions).addTo(this.map);
      L.vectorGrid.protobuf("http://localhost:8080/geoserver/gwc/service/tms/1.0.0/JawaTimur:jatim@EPSG%3A900913@pbf/{z}/{x}/{-y}.pbf", vectorTileOptions).addTo(this.map);
    },
  },
  mounted() {
    this.setupLeafletMap();
  },
};
</script>

<style scoped>
#container {
  display: flex;
}
#mapContainer {
  width: 100vw;
  height: 100vh;
}
</style>