<script setup lang="ts">
import { onMounted, ref } from 'vue';
import LineIcon from '@/components/LineIcon.vue';
import Slide from '@/components/Slide.vue';
import TransitMap from '@/components/TransitMap.vue';

const activeView = ref<string | null>(null);

onMounted(() => {
  const slides = document.querySelectorAll('.slide');
  const navigableSlides = document.querySelectorAll('.slide:not(.reset)');

  navigableSlides.forEach((slide, index) => {
    slide.setAttribute('id', `slide-${index.toString(10)}`);
  });

  slides.forEach((slide) => {
    const observer = new window.IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          if (slide.getAttribute('data-view')) {
            activeView.value = slide.getAttribute('data-view');
          } else {
            activeView.value = '';
          }

          return;
        }
      },
      {
        root: null,
        threshold: 0.4, // set offset 0.1 means trigger if atleast 10% of element in viewport
      }
    );

    observer.observe(slide);
  });
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
      <TransitMap v-bind:active-view="activeView"></TransitMap>

      <Slide layout="map" data-view="before">
        <template v-slot:figure> </template>

        <div>
          <p>This is a map of the LA Metro rail system before June 2023.</p>

          <p>
            The <LineIcon line="b" /> and <LineIcon line="d" /> Lines are heavy rail. The
            <LineIcon line="a" /> <LineIcon line="e" /> <LineIcon line="l" /> Lines, as well as the
            <LineIcon line="c" /> and <LineIcon line="k" /> Lines, are light rail.
          </p>

          <p>
            Notice the peculiar shape of the <LineIcon line="l" /> Line. And notice how most of the
            lines seem to originate or terminate downtown. A relic of the days when most people
            commuted downtown for work, this is in and of itself suboptimal, since many, or even
            most, people do not want to go downtown anymore, and because it created a bottleneck.
          </p>

          <p>But let's take a closer look...</p>
        </div>
      </Slide>

      <Slide layout="map" data-view="before-focus">
        <p>Even though these lines go downtown, they terminate in different locations.</p>
      </Slide>

      <Slide layout="map" data-view="route-before-overview">
        <p>
          This means that if you were at the Natural History Museum and wanted to get to the
          Japanese American National Museum, you would need to:
        </p>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="12" />

      <Slide layout="map" data-view="route-before-1">
        <p>
          Board the <LineIcon line="e" /> Line at Expo Park/USC and ride it four stops to its
          terminus at 7th Street / Metro Center.
        </p>
      </Slide>

      <Slide layout="map" data-view="route-before-2">
        <p>
          Get off the train, head downstairs, transfer to the <LineIcon line="b" /> or
          <LineIcon line="d" /> Line (arrives every 7.5 minutes if you are very lucky).
        </p>
      </Slide>

      <Slide layout="map" data-view="route-before-3">
        <p>
          Take the <LineIcon line="b" /> or <LineIcon line="d" /> Line for three stops to the
          terminus at Union Station.
        </p>
      </Slide>

      <Slide layout="map" data-view="route-before-4">
        <p>
          Get off the train, head upstairs, walk across the station to the platform of the
          <LineIcon line="l" /> Line (arrives every 10 minutes if you are lucky).
        </p>
      </Slide>

      <Slide layout="map" data-view="route-before-5">
        <p>Take the <LineIcon line="l" /> Line one stop to Little Tokyo / Arts District.</p>
      </Slide>

      <Slide layout="hero">
        <p>
          This trip was less than 6 miles, but it could take upward of 45 minutes. And it was faster
          to walk some portions.
        </p>
      </Slide>

      <Slide layout="map" data-view="regional-connector">
        <p>
          The Regional Connector added a 1.9-mile tunnel to connect the disparate light-rail lines
          in Downtown.
        </p>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="11" />

      <Slide layout="map" data-view="l-line-before">
        <p>
          As part of the project, the <LineIcon line="l" /> Line was reconfigured. Here is its prior
          configuration.
        </p>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="11" />

      <Slide layout="map" data-view="l-line-split-old">
        <div>
          <p>And here is its new configuration.</p>

          <p>
            The northern portion was converted to be part of the <LineIcon line="a" /> Line, and the
            southern portion was converted to be part of the <LineIcon line="e" /> Line.
          </p>
        </div>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="11" />

      <Slide layout="map" data-view="l-line-split-new">
        <div>
          <p>
            Except one thing. The E Line adopted the color of the former L Line and became the
            <LineIcon line="e-new" /> Line.
          </p>

          <p>
            Since the <LineIcon line="l" /> Line went away, a yellow <LineIcon line="e-new" /> is
            easier than an aqua <LineIcon line="e-old" /> to differentiate from the blue
            <LineIcon line="a" />.
          </p>
        </div>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="11" />

      <Slide layout="map" data-view="old-lines">
        <p>
          As a result, the <LineIcon line="a" /> <LineIcon line="e-old" /> and
          <LineIcon line="l" /> Lines went from this...
        </p>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="11" />

      <Slide layout="map" data-view="new-lines">
        <div>
          <p>...to this,</p>

          <p>
            with the boomerang shape of the former <LineIcon line="l" /> replaced by a more
            comprehensive <LineIcon line="a" /> Line that runs roughly north-south and an
            <LineIcon line="e-new" /> Line that runs roughly east-west.
          </p>
        </div>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="12" />

      <Slide layout="map" data-view="route-after-overview">
        <p>Let's take a look at our example trip again.</p>
      </Slide>

      <Slide layout="reset" v-bind:data-zoom="12" />

      <Slide layout="map" data-view="route-after">
        <div>
          <p>With the Regional Connector completed, you only need to...</p>

          <p>
            ...board the <LineIcon line="e-new" /> Line at Expo Park/USC and ride seven stops until
            Little Tokyo / Arts District.
          </p>
        </div>
      </Slide>
    </div>

    <Slide layout="hero">
      <p>That's it. And it takes less than 20 minutes.</p>
    </Slide>

    <Slide layout="figure-full" buttonLabel="Footnotes">
      <template v-slot:caption-top>
        <p class="center" style="margin-bottom: 0">
          Here is a before-after look at the entire system with the Regional Connector marked in
          black:
        </p>
      </template>

      <template v-slot:figure>
        <img alt="" src="@/assets/images/system-before.png" />

        <img alt="" src="@/assets/images/system-after-rc.png" />
      </template>

      <template v-slot:caption-bottom>
        <p class="center">
          It is a relatively small length of track, but it completely revolutionizes and streamlines
          the LA Metro Rail system.
        </p>
      </template>
    </Slide>

    <Slide last="true">
      <div style="display: flex; flex-direction: column; align-items: center; width: 100%">
        <p class="center">Here are some resources if you want to learn more:</p>

        <ul>
          <li>
            <a href="https://www.metro.net/moredtla/" target="_blank"
              >LA Metro Regional Connector Opening Site</a
            >
          </li>
          <li>
            <a href="https://en.wikipedia.org/wiki/Regional_Connector" target="_blank"
              >Wikipedia Page</a
            >
          </li>
          <li>
            <a
              href="https://thesource.metro.net/2023/05/22/the-regional-connector-and-three-new-downtown-l-a-stations-to-open-friday-june-16-with-a-weekend-of-free-rides/"
              target="_blank"
              >Opening Announcement Post from <em>The Source</em></a
            >
          </li>
        </ul>

        <ul class="horizontal-list">
          <li>
            <a
              href="https://www.dropbox.com/s/n26bfce3iwv5fbd/A%20Line_801_TT_06-16-23.pdf"
              target="_blank"
              >New <LineIcon line="a" /> Line Timetable</a
            >
          </li>
          <li>
            <a
              href="https://www.dropbox.com/s/gdatfhgct7etugu/E%20Line_804_TT_06-16-23.pdf"
              target="_blank"
              >New <LineIcon line="e-new" /> Line Timetable</a
            >
          </li>
        </ul>
      </div>
    </Slide>
  </div>
</template>
