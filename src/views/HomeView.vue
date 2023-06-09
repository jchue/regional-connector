<script setup lang="ts">
import Slide from '@/components/Slide.vue';
import LineIcon from '@/components/LineIcon.vue';
import { onMounted } from 'vue';

onMounted(() => {
  const slides = document.querySelectorAll('.slide');
  slides.forEach((slide, index) => {
    slide.setAttribute('id', `slide-${index.toString(10)}`);
  });
});
</script>

<template>
  <Slide layout="header">
    <span class="title">LA Metro Regional Connector Explained</span>
  </Slide>

  <Slide layout="hero">
    <p>The Regional Connector opens June 16th. It will be a complete game-changer for the LA Metro system.</p>
    
    <p>Here's why.</p>
  </Slide>

  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/system-before.png" />
    </template>

    <div>
      <p>This is the LA Metro rail system before June 2023.</p>
      
      <p>The <LineIcon line="b"/> and <LineIcon line="d"/> Lines are heavy rail. The <LineIcon line="a"/> <LineIcon line="e"/> <LineIcon line="l"/> Lines, as well as the <LineIcon line="c"/> and <LineIcon line="k"/> Lines, are light rail.</p>

      <p>Notice the peculiar shape of the <LineIcon line="l" /> Line. And notice how most of the lines seem to originate or terminate downtown. A relic of the days when most people commuted downtown for work, this is in and of itself suboptimal, since many, or even most, people do not want to go downtown anymore, and because it creates a bottleneck.</p>

      <p>But let's take a closer look...</p>
    </div>
  </Slide>

  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/downtown.png" />
    </template>

    <p>Even though these lines go downtown, they terminate in different locations.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route.png" />
    </template>

    <p>This means that if you were at the Natural History Museum and wanted to get to the Japanese American National Museum, you would need to:</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-1.png" />
    </template>

    <p>Board the <LineIcon line="e" /> Line at Expo Park/USC and ride it four stops to its terminus at 7th Street / Metro Center.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-2.png" />
    </template>

    <p>Get off the train, head downstairs, transfer to the <LineIcon line="b" /> or <LineIcon line="d" /> Line (arrives every 7.5 minutes if you are very lucky).</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-3.png" />
    </template>

    <p>Take the <LineIcon line="b" /> or <LineIcon line="d" /> Line for three stops to the terminus at Union Station.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-4.png" />
    </template>

    <p>Get off the train, head upstairs, walk across the station to the platform of the <LineIcon line="l" /> Line (arrives every 10 minutes if you are lucky).</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-5.png" />
    </template>

    <p>Take the <LineIcon line="l" /> Line one stop to Little Tokyo / Arts District*.</p>

    <span class="footnote">*As part of the project, the Little Tokyo / Arts District is being relocated around the corner and underground, so this station is currently closed.</span>
  </Slide>
  <Slide layout="hero">
    <p>This trip is less than 6 miles, but it can take upward of 45 minutes. And it's faster to walk some portions.</p>
  </Slide>

  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/rc.png" />
    </template>

    <p>The Regional Connector adds a 1.9-mile tunnel to connect the disparate light-rail lines in Downtown.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/l-before.png" />
    </template>

    <p>As part of the project, the <LineIcon line="l" /> Line will be reconfigured. Here is its prior configuration.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/l-after.png" />
    </template>

    <div>
      <p>And here is its new configuration.</p>
      
      <p>The northern portion will be converted to be part of the <LineIcon line='a' /> Line, and the eastern portion will be converted to be part of the <LineIcon line='e' /> Line. However, the E Line will adopt the color of the former L Line and will become the <LineIcon line='e-new' /> Line.</p>

      <p>This is because the <LineIcon line='l' /> Line will be going away, and a yellow <LineIcon line='e-new' /> is easier than an aqua <LineIcon line='e-old' /> to differentiate from the blue <LineIcon line='a' />.</p>
    </div>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/focus-before.png" />
    </template>

    <p>As a result, the <LineIcon line='a' /> <LineIcon line='e-old' /> and <LineIcon line='l' /> Lines will go from this...</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/focus-after.png" />
    </template>

    <div>
      <p>...to this,</p>

      <p>with the boomerang shape of the former <LineIcon line='l' /> replaced by a more comprehensive <LineIcon line='a' /> Line that runs roughly north-south and an <LineIcon line='e-new' /> Line that runs roughly east-west.</p>
    </div>
  </Slide>

  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-after.png" />
    </template>

    <p>Let's take a look at our example trip again.</p>
  </Slide>
  <Slide layout="figure">
    <template v-slot:figure>
      <img alt="" src="@/assets/route-after-1.png" />
    </template>

    <div>
      <p>With the Regional Connector completed, you only need to...</p>
        
      <p>...board the <LineIcon line="e-new" /> Line at Expo Park/USC and ride seven stops until Little Tokyo / Arts District.</p>
    </div>
  </Slide>

  <Slide layout="hero">
    <p>That's it.</p>
  </Slide>

  <Slide layout="figure-full">
    <p class="center" style="margin-bottom: 0;">Here is a before-after look at the entire system with the Regional Connector marked in black:</p>

    <div class="figure">

      <img alt="" src="@/assets/system-before.png" />

      <img alt="" src="@/assets/system-after-rc.png" />
    </div>

    <p class="center" style="margin-bottom: 0;">It is a relatively small length of track, but it completely revolutionizes and streamlines the LA Metro Rail system.</p>
  </Slide>

  <Slide layout="figure-full" last="true">
    <p class="center">Here are some resources if you want to learn more:</p>

    <a href="https://www.metro.net/projects/connector-2/" target="_blank">LA Metro Project Page</a>
    <a href="https://www.metro.net/moredtla/" target="_blank">LA Metro Regional Connector Opening Site</a>
    <a href="https://en.wikipedia.org/wiki/Regional_Connector" target="_blank">Wikipedia Page</a>
  </Slide>
</template>

<style scoped>
img {
  max-width: 100%;
}
</style>