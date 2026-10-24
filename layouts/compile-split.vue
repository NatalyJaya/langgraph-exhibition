<script setup lang="ts">
defineProps<{ heading?: string; step?: string }>()
</script>

<template>
  <div class="slidev-layout compile-split">
    <header class="cs-header">
      <span v-if="step" class="cs-step">{{ step }}</span>
      <h2 v-if="heading">{{ heading }}</h2>
    </header>
    <div class="cs-body">
      <div class="cs-code">
        <slot />
      </div>
      <div class="cs-diagram">
        <slot name="right" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.compile-split {
  --slidev-code-font-size: 10px;
  --slidev-code-line-height: 15px;

  box-sizing: border-box;
  height: 100%;
  padding: 20px 26px 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  background: #fbfaf7;
  color: #3f3d38;
}

.cs-header {
  display: flex;
  align-items: center;
  gap: 12px;
}

.cs-header h2 {
  margin: 0;
  font-size: 1.3rem;
  line-height: 1.2;
  color: #3f3d38;
}

.cs-step {
  flex: none;
  padding: 3px 9px;
  border-radius: 999px;
  background: #dbe8f8;
  color: #3b4d68;
  font-family: 'JetBrains Mono', monospace;
  font-size: 11px;
  font-weight: 500;
}

.cs-body {
  flex: 1;
  min-height: 0;
  display: grid;
  grid-template-columns: 1.35fr 1fr;
  gap: 14px;
}

.cs-code {
  min-width: 0;
  min-height: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.cs-code :deep(.slidev-code-wrapper) {
  margin: 0;
}

.cs-diagram {
  min-width: 0;
  min-height: 0;
}
</style>
