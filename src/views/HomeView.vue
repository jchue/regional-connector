<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Map from '@/components/Map.vue';
import Slide from '@/components/Slide.vue';
import LineIcon from '@/components/LineIcon.vue';

import * as aLineBeforeData from '@/assets/geojson/a-line-before.json';
import * as bLineData from '@/assets/geojson/b-line.json';
import * as cLineData from '@/assets/geojson/c-line.json';
import * as dLineData from '@/assets/geojson/d-line.json';
import * as eLineBeforeData from '@/assets/geojson/e-line-before.json';
import * as kLineData from '@/assets/geojson/k-line.json';
import * as lLineBeforeData from '@/assets/geojson/l-line-before.json';

import * as routeBefore1Data from '@/assets/geojson/route-before-1.json';
import * as routeBefore2BData from '@/assets/geojson/route-before-2b.json';
import * as routeBefore2DData from '@/assets/geojson/route-before-2d.json';
import * as routeBefore3Data from '@/assets/geojson/route-before-3.json';

import * as rcData from '@/assets/geojson/rc.json';

import * as lSplitAData from '@/assets/geojson/l-split-a.json';
import * as lSplitEData from '@/assets/geojson/l-split-e.json';

import * as aLineAfterData from '@/assets/geojson/a-line-after.json';
import * as eLineAfterData from '@/assets/geojson/e-line-after.json';

import * as routeAfter1Data from '@/assets/geojson/route-after-1.json';

const unchanged = ref([
  {
    data: bLineData,
    color: '#e3131b',
  },
  {
    data: cLineData,
    color: '#58a738',
  },
  {
    data: dLineData,
    color: '#a05da5',
  },
  {
    data: kLineData,
    color: '#e96bb0',
  },
]);

const unchangedInactive = ref([
  {
    data: bLineData,
    color: '#f4b5b8',
  },
  {
    data: cLineData,
    color: '#aedd9b',
  },
  {
    data: dLineData,
    color: '#e0bae2',
  },
  {
    data: kLineData,
    color: '#f2cde1',
  },
]);

const oldLines = ref([
  {
    data: aLineBeforeData,
    color: '#0072bc',
  },
  {
    data: eLineBeforeData,
    color: '#5bc2e7',
  },
  {
    data: lLineBeforeData,
    color: '#fdb913',
  },
]);

const oldLinesInactive = ref([
  {
    data: aLineBeforeData,
    color: '#a0cbe8',
  },
  {
    data: eLineBeforeData,
    color: '#d7ecf4',
  },
  {
    data: lLineBeforeData,
    color: '#f9da89',
  },
]);

const newLines = ref([
  {
    data: aLineAfterData,
    color: '#0072bc',
  },
  {
    data: eLineAfterData,
    color: '#fdb913',
  },
]);

const newlinesInactive = ref([
  {
    data: aLineAfterData,
    color: '#a0cbe8',
  },
  {
    data: eLineAfterData,
    color: '#f9da89',
  },
]);

const systemBefore = ref([
  ...unchanged.value,
  ...oldLines.value,
])

const systemBeforeInactive = ref([
  ...unchangedInactive.value,
  ...oldLinesInactive.value,
]);

const systemAfter = ref([
  ...unchanged.value,
  ...newLines.value,
]);

const rc = ref([
  ...systemBeforeInactive.value,
  {
    data: rcData,
    color: '#000',
    weight: 7,
  },
]);

const routeBefore1 = ref([
  ...systemBeforeInactive.value,
  {
    data: routeBefore1Data,
    color: '#5bc2e7',
    weight: 5,
  },
]);

const routeBefore2 = ref([
  ...systemBeforeInactive.value,
  {
    data: routeBefore2BData,
    color: '#e3131b',
    weight: 5,
  },
  {
    data: routeBefore2DData,
    color: '#a05da5',
    weight: 5,
  },
]);

const routeBefore3 = ref([
  ...systemBeforeInactive.value,
  {
    data: routeBefore3Data,
    color: '#fdb913',
    weight: 5,
  },
]);

const routeAfter1 = ref([
  ...unchangedInactive.value,
  ...newlinesInactive.value,
  {
    data: routeAfter1Data,
    color: '#fdb913',
  },
]);

const lines = ref([]);
const center = ref([34.029918,-118.2566603]);
const pois = ref([]);
const zoom = ref(10);

onMounted(() => {
  const slides = document.querySelectorAll('.slide');
  const navigableSlides = document.querySelectorAll('.slide:not(.reset)');

  navigableSlides.forEach((slide, index) => {
    slide.setAttribute('id', `slide-${index.toString(10)}`);
  })

  slides.forEach((slide, index) => {
    const observer = new window.IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        if (slide.getAttribute('data-lines')) {
          lines.value = JSON.parse(String(slide.getAttribute('data-lines')));
        } else {
          lines.value = [];
        }

        if (slide.getAttribute('data-center-lat') && slide.getAttribute('data-center-lng')) {
          center.value = [Number(slide.getAttribute('data-center-lat')), Number(slide.getAttribute('data-center-lng'))];
        } else {
          center.value = [34.052514, -118.246365];
        }

        if (slide.getAttribute('data-zoom')) {
          zoom.value = Number(slide.getAttribute('data-zoom'));
        } else {
          zoom.value = 10
        }

        if (slide.getAttribute('data-pois')) {
          pois.value = JSON.parse(String(slide.getAttribute('data-pois')));
        } else {
          pois.value = [];
        }

        return;
      }
    }, {
      root: null,
      threshold: 0.4, // set offset 0.1 means trigger if atleast 10% of element in viewport
    })

    observer.observe(slide);
  });

  const map = document.querySelector('#map');  
});
</script>

<template>
  <div class="slide-container">
    <Slide layout="header">
      <span class="title">LA Metro Regional Connector Explained</span>
    </Slide>

    <Slide layout="hero">
      <p>The Regional Connector opened on June 16th. 🎉</p>
      
      <p>It is a complete game-changer for the LA Metro system. Here's why.</p>
    </Slide>

    <div>

        <Map v-bind:lines="lines" v-bind:center="center" v-bind:zoom="zoom" v-bind:pois="pois"></Map>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemBefore)"
          v-bind:data-zoom="11"
        >
          <template v-slot:figure>
          </template>

          <div>
            <p>This is a map of the LA Metro rail system before June 2023.</p>
            
            <p>The <LineIcon line="b"/> and <LineIcon line="d"/> Lines are heavy rail. The <LineIcon line="a"/> <LineIcon line="e"/> <LineIcon line="l"/> Lines, as well as the <LineIcon line="c"/> and <LineIcon line="k"/> Lines, are light rail.</p>

            <p>Notice the peculiar shape of the <LineIcon line="l" /> Line. And notice how most of the lines seem to originate or terminate downtown. A relic of the days when most people commuted downtown for work, this is in and of itself suboptimal, since many, or even most, people do not want to go downtown anymore, and because it created a bottleneck.</p>

            <p>But let's take a closer look...</p>
          </div>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemBefore)"
          v-bind:data-zoom="14"
        >
          <p>Even though these lines go downtown, they terminate in different locations.</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemBefore)"
          v-bind:data-center-lat="34.04034"
          v-bind:data-center-lng="-118.257179"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.01708, -118.28879],
              markerLabel: '1',
              tooltipLabel: 'Natural History Museum of Los Angeles County'
            },
            {
              coords: [34.04968, -118.23862],
              markerLabel: '2',
              tooltipLabel: 'Japanese American National Museum'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <p>This means that if you were at the Natural History Museum and wanted to get to the Japanese American National Museum, you would need to:</p>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="12"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(routeBefore1)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.01822, -118.28941],
              markerLabel: '1',
              tooltipLabel: 'Expo Park / USC'
            },
            {
              coords: [34.04871, -118.25846],
              markerLabel: '2', 
              tooltipLabel: '7th Street / Metro Center'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <p>Board the <LineIcon line="e" /> Line at Expo Park/USC and ride it four stops to its terminus at 7th Street / Metro Center.</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemBeforeInactive)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.04871, -118.25846],
              markerLabel: '3',
              tooltipLabel: '7th Street / Metro Center'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <p>Get off the train, head downstairs, transfer to the <LineIcon line="b" /> or <LineIcon line="d" /> Line (arrives every 7.5 minutes if you are very lucky).</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(routeBefore2)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.04871, -118.25846],
              markerLabel: '4',
              tooltipDirection: 'left',
              tooltipLabel: '7th Street / Metro Center'
            },
            {
              coords: [34.0564, -118.23471],
              markerLabel: '5',
              tooltipDirection: 'right',
              tooltipLabel: 'Union Station'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <p>Take the <LineIcon line="b" /> or <LineIcon line="d" /> Line for three stops to the terminus at Union Station.</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemBeforeInactive)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.0564, -118.23471],
              markerLabel: '6',
              tooltipLabel: 'Union Station'
            },
          ])"
          v-bind:data-zoom="14"
        >
          <p>Get off the train, head upstairs, walk across the station to the platform of the <LineIcon line="l" /> Line (arrives every 10 minutes if you are lucky).</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(routeBefore3)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.0564, -118.23471],
              markerLabel: '7',
              tooltipDirection: 'right',
              tooltipLabel: 'Union Station'
            },
            {
              coords: [34.04907, -118.2387],
              markerLabel: '8',
              tooltipDirection: 'right',
              tooltipLabel: 'Little Tokyo / Arts District'
            },
          ])"
          v-bind:data-zoom="14"
        >
          <p>Take the <LineIcon line="l" /> Line one stop to Little Tokyo / Arts District.</p>
        </Slide>

        <Slide layout="hero">
          <p>This trip was less than 6 miles, but it could take upward of 45 minutes. And it was faster to walk some portions.</p>
        </Slide>

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(rc)"
          v-bind:data-center-lat="34.0724"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="14"
        >
          <p>The Regional Connector added a 1.9-mile tunnel to connect the disparate light-rail lines in Downtown.</p>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="11"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify([{
            data: lLineBeforeData,
            color: '#fdb913',
          }])"
          v-bind:data-center-lat="34.0524"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="11"
        >
          <p>As part of the project, the <LineIcon line="l" /> Line was reconfigured. Here is its prior configuration.</p>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="11"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify([{
            data: lSplitAData,
            color: '#0072bc',
          }, {
            data: lSplitEData,
            color: '#5bc2e7',
          }])"
          v-bind:data-center-lat="34.0524"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="11"
        >
          <div>
            <p>And here is its new configuration.</p>
            
            <p>The northern portion was converted to be part of the <LineIcon line='a' /> Line, and the southern portion was converted to be part of the <LineIcon line='e' /> Line.</p>
          </div>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="11"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify([{
            data: lSplitAData,
            color: '#0072bc',
          }, {
            data: lSplitEData,
            color: '#fdb913',
          }])"
          v-bind:data-center-lat="34.0524"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="11"
        >
          <div>
            <p>Except one thing. The E Line adopted the color of the former L Line and became the <LineIcon line='e-new' /> Line.</p>

            <p>Since the <LineIcon line='l' /> Line went away, a yellow <LineIcon line='e-new' /> is easier than an aqua <LineIcon line='e-old' /> to differentiate from the blue <LineIcon line='a' />.</p>
          </div>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="11"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(oldLines)"
          v-bind:data-center-lat="34.0524"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="11"
        >
          <p>As a result, the <LineIcon line='a' /> <LineIcon line='e-old' /> and <LineIcon line='l' /> Lines went from this...</p>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="11"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(newLines)"
          v-bind:data-center-lat="34.0524"
          v-bind:data-center-lng="-118.2461"
          v-bind:data-zoom="11"
        >
          <div>
            <p>...to this,</p>

            <p>with the boomerang shape of the former <LineIcon line='l' /> replaced by a more comprehensive <LineIcon line='a' /> Line that runs roughly north-south and an <LineIcon line='e-new' /> Line that runs roughly east-west.</p>
          </div>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="12"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(systemAfter)"
          v-bind:data-center-lat="34.04034"
          v-bind:data-center-lng="-118.257179"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.01708, -118.28879],
              markerLabel: '1',
              tooltipLabel: 'Natural History Museum of Los Angeles County'
            },
            {
              coords: [34.04968, -118.23862],
              markerLabel: '2',
              tooltipLabel: 'Japanese American National Museum'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <p>Let's take a look at our example trip again.</p>
        </Slide>

        <Slide
          layout="reset"
          v-bind:data-zoom="12"
        />

        <Slide
          layout="map"
          v-bind:data-lines="JSON.stringify(routeAfter1)"
          v-bind:data-pois="JSON.stringify([
            {
              coords: [34.01822, -118.28941],
              markerLabel: '1',
              tooltipLabel: 'Expo Park / USC'
            },
            {
              coords: [34.04907, -118.2387],
              markerLabel: '2',
              tooltipDirection: 'right',
              tooltipLabel: 'Little Tokyo / Arts District'
            },
          ])"
          v-bind:data-zoom="12"
        >
          <div>
            <p>With the Regional Connector completed, you only need to...</p>
              
            <p>...board the <LineIcon line="e-new" /> Line at Expo Park/USC and ride seven stops until Little Tokyo / Arts District.</p>
          </div>
        </Slide>

      </div>

    <Slide layout="hero">
      <p>That's it. And it takes less than 20 minutes.</p>
    </Slide>

    <Slide layout="figure-full" buttonLabel="Footnotes">
      <template v-slot:caption-top>
        <p class="center" style="margin-bottom: 0;">Here is a before-after look at the entire system with the Regional Connector marked in black:</p>
      </template>

      <template v-slot:figure>
        <img alt="" src="@/assets/images/system-before.png" />

        <img alt="" src="@/assets/images/system-after-rc.png" />
      </template>

      <template v-slot:caption-bottom>
        <p class="center">It is a relatively small length of track, but it completely revolutionizes and streamlines the LA Metro Rail system.</p>
      </template>
    </Slide>

    <Slide last="true">
      <div style="display: flex; flex-direction: column; align-items: center; width: 100%;">
        <p class="center">Here are some resources if you want to learn more:</p>

        <ul>
          <li><a href="https://www.metro.net/moredtla/" target="_blank">LA Metro Regional Connector Opening Site</a></li>
          <li><a href="https://en.wikipedia.org/wiki/Regional_Connector" target="_blank">Wikipedia Page</a></li>
          <li><a href="https://thesource.metro.net/2023/05/22/the-regional-connector-and-three-new-downtown-l-a-stations-to-open-friday-june-16-with-a-weekend-of-free-rides/" target="_blank">Opening Announcement Post from <em>The Source</em></a></li>
        </ul>

        <ul class="horizontal-list">
          <li><a href="https://www.dropbox.com/s/n26bfce3iwv5fbd/A%20Line_801_TT_06-16-23.pdf" target="_blank">New <LineIcon line="a" /> Line Timetable</a></li>
          <li><a href="https://www.dropbox.com/s/gdatfhgct7etugu/E%20Line_804_TT_06-16-23.pdf" target="_blank">New <LineIcon line="e-new" /> Line Timetable</a></li>
        </ul>
      </div>
    </Slide>
  </div>
</template>
