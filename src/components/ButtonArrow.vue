<script setup lang="ts">
import ArrowDownIcon from '@/components/ArrowDownIcon.vue';
import ArrowLeftIcon from '@/components/ArrowLeftIcon.vue';
import ArrowRightIcon from '@/components/ArrowRightIcon.vue';
import ArrowUpIcon from '@/components/ArrowUpIcon.vue';

const props = defineProps<{
  direction?: 'down' | 'left' | 'right' | 'up';
}>();
</script>

<template>
  <ArrowDownIcon v-if="props.direction === 'down'" class="ml-1 text-white h-5" />
  <ArrowLeftIcon v-if="props.direction === 'left'" class="text-white h-5" />
  <ArrowRightIcon v-if="props.direction === 'right'" class="text-white h-5" />
  <ArrowUpIcon v-if="props.direction === 'up'" class="ml-1 text-white h-5" />
</template>
