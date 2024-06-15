<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import { bbox } from '@turf/bbox';
import { featureCollection, lineString } from '@turf/helpers';
import { Map, Marker, NavigationControl, Popup } from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

import * as unchanged from '@/assets/geojson/unchanged.json';
import * as oldLines from '@/assets/geojson/old-lines.json';
import * as newLines from '@/assets/geojson/new-lines.json';
import * as regionalConnector from '@/assets/geojson/regional-connector.json';
import * as lLineBefore from '@/assets/geojson/l-line-before.json';
import * as lLineSplitOld from '@/assets/geojson/l-line-split-old.json';
import * as lLineSplitNew from '@/assets/geojson/l-line-split-new.json';
import * as routeBefore1 from '@/assets/geojson/route-before-1.json';
import * as routeBefore2 from '@/assets/geojson/route-before-2.json';
import * as routeBefore3 from '@/assets/geojson/route-before-3.json';
import * as routeAfter from '@/assets/geojson/route-after.json';

const systemBefore = featureCollection([
  ...(oldLines.features as GeoJSON.Feature[]),
  ...(unchanged.features as GeoJSON.Feature[]),
]);

const systemAfter = featureCollection([
  ...(newLines.features as GeoJSON.Feature[]),
  ...(unchanged.features as GeoJSON.Feature[]),
]);

const mapTilerKey = import.meta.env.VITE_MAPTILER_KEY;

const props = defineProps<{
  activeView: string | null;
}>();

interface POI {
  lnglat: [number, number];
  markerLabel: string;
  tooltipDirection?: string;
  tooltipLabel: string;
}

interface MapView {
  id: string;
  bounds?: number[][];
  lines?: GeoJSON.FeatureCollection | GeoJSON.Feature;
  bgLines?: GeoJSON.FeatureCollection | GeoJSON.Feature;
  pois?: POI[];
  zoom?: number;
}

const views: MapView[] = [
  {
    id: 'before',
    lines: systemBefore,
    zoom: 11,
  },
  {
    id: 'before-focus',
    bounds: [
      [-118.262816, 34.06678],
      [-118.228762, 34.052943],
      [-118.25688, 34.042305],
    ],
    lines: systemBefore,
    zoom: 14,
  },
  {
    id: 'route-before-overview',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: systemBefore,
    pois: [
      {
        lnglat: [-118.28879, 34.01708],
        markerLabel: '1',
        tooltipLabel: 'Natural History Museum of Los Angeles County',
      },
      {
        lnglat: [-118.23862, 34.04968],
        markerLabel: '2',
        tooltipLabel: 'Japanese American National Museum',
      },
    ],
  },
  {
    id: 'route-before-1',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: routeBefore1 as GeoJSON.FeatureCollection,
    bgLines: systemBefore,
    pois: [
      {
        lnglat: [-118.285696, 34.018222],
        markerLabel: '1',
        tooltipLabel: 'Expo Park / USC',
      },
      {
        lnglat: [-118.25846, 34.04871],
        markerLabel: '2',
        tooltipLabel: '7th Street / Metro Center',
      },
    ],
  },
  {
    id: 'route-before-2',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    bgLines: systemBefore,
    pois: [
      {
        lnglat: [-118.25846, 34.04871],
        markerLabel: '3',
        tooltipLabel: '7th Street / Metro Center',
      },
    ],
  },
  {
    id: 'route-before-3',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: routeBefore2 as GeoJSON.FeatureCollection,
    bgLines: systemBefore,
    pois: [
      {
        lnglat: [-118.25846, 34.04871],
        markerLabel: '4',
        tooltipDirection: 'left',
        tooltipLabel: '7th Street / Metro Center',
      },
      {
        lnglat: [-118.23471, 34.0564],
        markerLabel: '5',
        tooltipDirection: 'right',
        tooltipLabel: 'Union Station',
      },
    ],
  },
  {
    id: 'route-before-4',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    bgLines: systemBefore,
    pois: [
      {
        lnglat: [-118.23471, 34.0564],
        markerLabel: '6',
        tooltipLabel: 'Union Station',
      },
    ],
  },
  {
    id: 'route-before-5',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: routeBefore3 as GeoJSON.FeatureCollection,
    bgLines: systemBefore,
    pois: [
      {
        lnglat: [-118.23471, 34.0564],
        markerLabel: '7',
        tooltipDirection: 'right',
        tooltipLabel: 'Union Station',
      },
      {
        lnglat: [-118.2387, 34.04907],
        markerLabel: '8',
        tooltipDirection: 'right',
        tooltipLabel: 'Little Tokyo / Arts District',
      },
    ],
  },
  {
    id: 'regional-connector',
    bounds: [
      [-118.262816, 34.06678],
      [-118.228762, 34.052943],
      [-118.25688, 34.042305],
    ],
    lines: regionalConnector as GeoJSON.Feature,
    bgLines: systemBefore,
  },
  {
    id: 'l-line-before',
    lines: lLineBefore as GeoJSON.Feature,
  },
  {
    id: 'l-line-split-old',
    lines: lLineSplitOld as GeoJSON.FeatureCollection,
  },
  {
    id: 'l-line-split-new',
    lines: lLineSplitNew as GeoJSON.FeatureCollection,
  },
  {
    id: 'old-lines',
    lines: oldLines as GeoJSON.FeatureCollection,
  },
  {
    id: 'new-lines',
    lines: newLines as GeoJSON.FeatureCollection,
  },
  {
    id: 'route-after-overview',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: systemAfter,
    pois: [
      {
        lnglat: [-118.28879, 34.01708],
        markerLabel: '1',
        tooltipLabel: 'Natural History Museum of Los Angeles County',
      },
      {
        lnglat: [-118.23862, 34.04968],
        markerLabel: '2',
        tooltipLabel: 'Japanese American National Museum',
      },
    ],
  },
  {
    id: 'route-after',
    bounds: [
      [-118.248835, 34.062681],
      [-118.221835, 34.052254],
      [-118.295879, 34.010855],
    ],
    lines: routeAfter as GeoJSON.FeatureCollection,
    bgLines: systemAfter,
    pois: [
      {
        lnglat: [-118.285696, 34.018222],
        markerLabel: '1',
        tooltipLabel: 'Expo Park / USC',
      },
      {
        lnglat: [-118.2387, 34.04907],
        markerLabel: '2',
        tooltipDirection: 'right',
        tooltipLabel: 'Little Tokyo / Arts District',
      },
    ],
  },
];

const map = ref();
const mapContainer = ref();

let popups: Popup[] = [];
let markers: Marker[] = [];

function addLines() {
  if (!props.activeView) return;

  const view = views.find((view) => {
    return view.id === props.activeView;
  });

  if (!view) return;

  /**
   * Background lines
   */
  if (map.value?.getLayer('bg-lines')) {
    map.value?.removeLayer('bg-lines');
  }

  if (view.bgLines) {
    map.value?.getSource('bg-lines').setData(view.bgLines);

    map.value.addLayer({
      id: 'bg-lines',
      type: 'line',
      source: 'bg-lines',
      layout: {
        'line-join': 'round',
        'line-cap': 'round',
      },
      paint: {
        'line-color': ['get', 'color'],
        'line-opacity': 0.3,
        'line-width': 3,
      },
    });
  }

  /**
   * Lines
   */
  if (map.value?.getLayer('lines')) {
    map.value?.removeLayer('lines');
  }

  if (view.lines) {
    map.value?.getSource('lines').setData(view.lines);

    map.value.addLayer({
      id: 'lines',
      type: 'line',
      source: 'lines',
      layout: {
        'line-join': 'round',
        'line-cap': 'round',
      },
      paint: {
        'line-color': ['get', 'color'],
        'line-width': 3,
      },
    });
  }

  // Clear markers and popups
  markers.forEach((marker) => {
    marker.remove();
  });
  markers = [];

  popups.forEach((popup) => {
    popup.remove();
  });
  popups = [];

  // Add markers and popups
  if (view.pois && view.pois.length) {
    view.pois.forEach((poi, index) => {
      const poiPopup = new Popup({
        closeButton: false,
        closeOnClick: true,
        offset: 15,
      }).setText(poi.tooltipLabel);

      popups.push(poiPopup);

      const el = document.createElement('div');
      //el.setAttribute('data-id', poi.id);
      el.className = 'marker';
      el.innerHTML = poi.markerLabel;

      const newMarker = new Marker({ element: el }).setLngLat(poi.lnglat).setPopup(poiPopup);

      markers.push(newMarker);

      if (map.value != null) newMarker.addTo(map.value).togglePopup();
    });
  }

  /**
   * Zoom
   */
  if (view.bounds) {
    // If bounds are provided
    const lineStringed = lineString(view.bounds);
    const bboxed = bbox(lineStringed);

    map.value?.fitBounds(bboxed, {
      padding: { top: 100, bottom: 100, left: 100, right: 100 },
    });
  } else if (view.zoom) {
    // If zoom is provided
    map.value.zoomTo(view.zoom);
  } else if (view.pois && view?.pois.length) {
    // If POIs are provided
    const coords = view.pois.map((poi) => poi.lnglat);
    const lineStringed = lineString(coords);
    const bboxed = bbox(lineStringed);

    map.value?.fitBounds(bboxed, {
      padding: { top: 100, bottom: 100, left: 100, right: 100 },
    });
  } else if (view.lines) {
    // If lines are provided
    const bboxed = bbox(view.lines);

    map.value?.fitBounds(bboxed, {
      padding: { top: 100, bottom: 100, left: 100, right: 100 },
    });
  }
}

watch(
  () => props.activeView,
  () => {
    if (!map.value.loaded()) return;

    addLines();
  }
);

onMounted(() => {
  map.value = new Map({
    attributionControl: { compact: true },
    container: mapContainer.value, // container id
    style: `https://api.maptiler.com/maps/dataviz/style.json?key=${mapTilerKey}`, // style URL
    center: [-118.2566603, 34.029918], // starting position [lng, lat]
    zoom: 10, // starting zoom
  });

  map.value.on('load', () => {
    map.value.addControl(new NavigationControl(), 'top-right');

    /**
     * Initialize empty sources
     */
    map.value.addSource('bg-lines', {
      type: 'geojson',
      data: {
        type: 'Feature',
        properties: {},
        geometry: {
          type: 'Polygon',
          coordinates: [],
        },
      },
    });

    map.value.addSource('lines', {
      type: 'geojson',
      data: {
        type: 'Feature',
        properties: {},
        geometry: {
          type: 'Polygon',
          coordinates: [],
        },
      },
    });

    map.value.addSource('pois', {
      type: 'geojson',
      data: {
        type: 'Feature',
        properties: {},
        geometry: {
          type: 'Polygon',
          coordinates: [],
        },
      },
    });
  });
});
</script>

<template>
  <div id="map" ref="mapContainer"></div>
</template>

<style>
#map {
  position: sticky;
  height: 100vh;
  top: 0;
  width: 60%;
  z-index: 10;
}

.marker {
  align-items: center;
  background-color: black;
  border-radius: 50%;
  color: white;
  cursor: pointer;
  display: flex;
  font-family: 'Nimbus Sans L', Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: bold;
  height: 2rem;
  justify-content: center;
  width: 2rem;
}

.maplibregl-popup {
  max-width: none !important;
}

.maplibregl-popup-content {
  background-color: #fff !important;
  border-radius: 0.5rem;
  box-shadow: 0 0.125rem 0.5rem rgba(0, 0, 0, 0.8);
  font-family: 'Nimbus Sans L', Helvetica, Arial, sans-serif;
  font-size: 0.8rem;
  font-weight: bold;
  padding: 0.5rem 1rem;
}

@media (max-width: 768px) {
  #map {
    box-shadow: 0 16px 16px -16px rgba(0, 0, 0, 0.2);
    height: 50vh;
    min-width: 100vw;
    z-index: 20;
  }
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
  font-family: 'Nimbus Sans L', Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: bold;
  margin-top: -8px;
}

.tooltip {
  background-color: #fff !important;
  border-radius: 0.5rem;
  box-shadow: 0 0.125rem 0.5rem rgba(0, 0, 0, 0.8);
  font-family: 'Nimbus Sans L', Helvetica, Arial, sans-serif;
  font-size: 0.8rem;
  font-weight: bold;
  padding: 0.5rem 1rem;
}

.tooltip::before {
  display: none;
}
</style>
