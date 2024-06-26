<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import { bbox } from '@turf/bbox';
import { featureCollection, lineString } from '@turf/helpers';
import { Map, Marker, NavigationControl, Popup, type MapLayerEventType } from 'maplibre-gl';
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
const popup = ref(
  new Popup({
    closeButton: false,
    closeOnClick: false,
  }).trackPointer()
);

let hoveredLineIndex: string | number | undefined;

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
        'line-width': ['case', ['boolean', ['feature-state', 'hover'], false], 5, 3],
      },
    });

    map.value?.on('mousemove', 'lines', (e: MapLayerEventType['mousemove']) => {
      // Stop hover from triggering lower layers
      e.originalEvent.stopPropagation();

      if (e.features != null && e.features.length > 0) {
        // Emphasize line on hover
        if (hoveredLineIndex !== undefined) {
          map.value?.setFeatureState({ source: 'lines', id: hoveredLineIndex }, { hover: false });
        }

        hoveredLineIndex = e.features[0].id;

        map.value?.setFeatureState({ source: 'lines', id: hoveredLineIndex }, { hover: true });

        // Show popup on hover
        const coordinates = e.lngLat;
        const description = e.features[0].properties.name as string;

        if (map.value != null)
          popup.value.setLngLat(coordinates).setHTML(description).addTo(map.value);
      }
    });

    map.value?.on('mouseleave', 'lines', () => {
      // Hide popup
      popup.value.remove();

      // Remove emphasis
      if (hoveredLineIndex !== undefined) {
        map.value?.setFeatureState({ source: 'lines', id: hoveredLineIndex }, { hover: false });
      }

      hoveredLineIndex = undefined;
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
      generateId: true,
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
      generateId: true,
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
  <div
    ref="mapContainer"
    class="!sticky top-0 shadow-[0_16px_16px_-16px_rgba(0,0,0,0.2)] md:shadow-none h-[50vh] md:h-screen min-w-full md:min-w-0 w-7/12 z-20 md:z-10"
  ></div>
</template>

<style>
.marker {
  @apply flex;
  @apply items-center;
  @apply justify-center;
  @apply bg-neutral-950;
  @apply cursor-pointer;
  @apply font-sans;
  @apply font-bold;
  @apply rounded-full;
  @apply text-base md:text-lg;
  @apply text-white;
  @apply h-8 md:h-9;
  @apply w-8 md:w-9;
}

.maplibregl-popup {
  @apply !max-w-none;
}

.maplibregl-popup-content {
  @apply bg-white;
  @apply font-sans;
  @apply font-semibold;
  @apply px-4 py-2;
  @apply rounded;
  @apply shadow;
  @apply text-xs md:text-sm;
}
</style>
