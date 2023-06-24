<script setup lang="ts">
import { ref } from 'vue';
import { LGeoJson, LIcon, LMap, LMarker, LTileLayer, LTooltip } from "@vue-leaflet/vue-leaflet";
import 'leaflet/dist/leaflet.css';
import PinIcon from './PinIcon.vue';
import PinIconFilled from './PinIconFilled.vue';

interface Line {
  color: string,
  data: [],
  weight: number,
};

interface POI {
  coords: [],
  markerLabel: string,
  tooltipDirection: string,
  tooltipLabel: string,
}

const props = defineProps({
  lines: Array<Line>,
  center: Array,
  pois: Array<POI>,
  zoom: Number,
});

const map =ref();

/**For debugging map position
function onMapClick(e: { latlng: string; }) {
  alert("You clicked the map at " + e.latlng);
}

function onMapReady() {
  map.value.leafletObject.on('click', onMapClick);
}
*/
</script>

<template>
  <div id="map">
    <l-map ref="map" v-bind:center="center" v-bind:options="{ scrollWheelZoom: false, zoomAnimationThreshold: 2 }" v-bind:use-global-leaflet="false" v-bind:zoom="zoom">
      <l-tile-layer
        url="https://stamen-tiles-{s}.a.ssl.fastly.net/toner-lite/{z}/{x}/{y}{r}.png"
        layer-type="base"
        name="OpenStreetMap"
        attribution="Map tiles by <a href='http://stamen.com'>Stamen Design</a>, under <a href='http://creativecommons.org/licenses/by/3.0'>CC BY 3.0</a>. Data by <a href='http://openstreetmap.org'>OpenStreetMap</a>, under <a href='http://www.openstreetmap.org/copyright'>ODbL</a>."
      ></l-tile-layer>

      <l-geo-json v-for="(line, index) in lines" v-bind:key="index" v-bind:geojson="line.data" v-bind:optionsStyle=" () => ({ color: line.color, weight: line.weight || 3 })"></l-geo-json>

      <l-marker v-for="(poi, index) in pois" v-bind:key="index" v-bind:lat-lng="poi.coords">
        <l-icon v-bind:options="{ className: 'icon', iconSize: [48, 48] }">
          <span v-if="poi.markerLabel">
            <span class="icon-text"><span>{{  poi.markerLabel }}</span></span>
            <PinIconFilled />
          </span>
          <span v-else>
            <PinIcon />
          </span>
        </l-icon>

        <l-tooltip v-bind:content="poi.tooltipLabel" v-bind:options="{ className: 'tooltip', direction: poi.tooltipDirection, offset: [16, 0], opacity: 1, permanent: true }"/>
      </l-marker>
    </l-map>
  </div>
</template>

<style>
#map {
  height: 100vh;
  width: 60%;
  position: sticky;
  top: 0;
  z-index: 10;
}

@media (max-width: 768px) {
  #map {
    box-shadow: 0 16px 16px -16px rgba(0, 0, 0, 0.2);
    height: 50vh;
    min-width: 100vw;
    z-index: 20;
  }
}

.leaflet-container {
  background-color: #fff;
}

.icon {
  text-align: center;
  width: 60px;
}

.icon svg {
  margin-top: -1rem;
}

.icon .icon-text {
  display: block;
  position: absolute;
  text-align: center;
  width: 48px;
}

.icon .icon-text span {
  color: #fff;
  display: block;
  font-size: 16px;
  font-weight: bold;
  margin-top: -8px;
}

.tooltip {
  background-color: #fff !important;
  border-radius: 0.5rem;
  box-shadow: 0 0.125rem 0.5rem rgba(0, 0, 0, 0.8);
  font-family: Inter, sans-serif;
  font-weight: bold;
  padding: 0.5rem 1rem;
}

.tooltip::before {
  display: none;
}
</style>
