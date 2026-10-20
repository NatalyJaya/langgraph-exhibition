<script setup lang="ts">
import LavaBackground from '../components/LavaBackground.vue'
</script>

<template>
  <div class="slidev-layout cover text-center">
    <LavaBackground />
    <div class="my-auto w-full">
      <slot />
    </div>
  </div>
</template>
