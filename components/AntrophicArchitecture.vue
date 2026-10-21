<template>
  <div ref="stage" class="stage">
    <div class="canvas" :style="{ transform: `translate(-50%, -50%) scale(${scale})` }">

      <!-- ================= TÍTULO ================= -->
      <h1 class="ctr title" style="left: 696px; top: 50px">
        High-level Architecture of Advanced Research
      </h1>

      <!-- ================= PANELES ================= -->
      <div class="panel" style="left: 49px; top: 109px; width: 354px; height: 613px"></div>
      <div class="panel" style="left: 574px; top: 109px; width: 768px; height: 613px"></div>

      <h2 class="ctr panel-title" style="left: 226px; top: 150px">Claude.ai chat</h2>
      <h2 class="ctr panel-title" style="left: 973px; top: 150px">Multi-agent research system</h2>

      <!-- ================= COLUMNA CHAT ================= -->
      <img
        src="../components/assets/anthropic-logo.png"
        alt="Anthropic Logo"
        class="logo"
        style="left: 75px; top: 262px"
      />

      <div class="prompt-title" style="left: 133px; top: 262px">
        What’s on your<br />mind tonight?
      </div>

      <svg class="user-icon" style="left: 76px; top: 340px" width="42" height="42" viewBox="0 0 24 24" fill="none" stroke="#7a7872" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round">
        <circle cx="12" cy="12" r="11" />
        <circle cx="12" cy="9.5" r="3.6" />
        <path d="M5.2 19.2c1.6-2.6 4-3.9 6.8-3.9s5.2 1.3 6.8 3.9" />
      </svg>

      <div class="prompt-text" style="left: 135px; top: 337px">
        what are all the<br />
        companies in the<br />
        united states working<br />
        on AI agents in 2025?<br />
        make a list of at<br />
        least 100. for each<br />
        company, include<br />
        the name, website,<br />
        product, description<br />
        of what they do,<br />
        type of agents<br />
        they build, and their<br />
        vertical/industry.
      </div>

      <!-- ================= ETIQUETAS DE FLECHAS ================= -->
      <span class="ctr arrow-label" style="left: 488px; top: 268px">User request</span>
      <span class="ctr arrow-label" style="left: 488px; top: 440px">Final report</span>

      <!-- ================= SISTEMA MULTI-AGENTE ================= -->
      <!-- Lead agent -->
      <div class="box lead" style="left: 806px; top: 263px; width: 316px; height: 177px">
        <div class="lead-title">Lead agent<br />(orchestrator)</div>
        <div class="lead-tools">
          Tools: search tools + MCP tools<br />
          + memory + run_subagent +<br />
          complete_task
        </div>
      </div>

      <!-- Citations -->
      <div class="box ctr-flex" style="left: 611px; top: 303px; width: 142px; height: 80px">
        Citations<br />subagent
      </div>

      <!-- Memory -->
      <div class="box ctr-flex" style="left: 1195px; top: 303px; width: 111px; height: 80px">
        Memory
      </div>

      <!-- Search subagents -->
      <div class="box ctr-flex" style="left: 623px; top: 527px; width: 200px; height: 60px">Search subagent</div>
      <div class="box ctr-flex" style="left: 864px; top: 527px; width: 200px; height: 60px">Search subagent</div>
      <div class="box ctr-flex" style="left: 1105px; top: 527px; width: 200px; height: 60px">Search subagent</div>

      <!-- ================= FLECHAS (SVG) ================= -->
      <svg class="arrows" width="1396" height="740" viewBox="0 0 1396 740" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <marker id="arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="11" markerHeight="11" markerUnits="userSpaceOnUse" orient="auto-start-reverse">
            <path d="M1 1 L9 5 L1 9" fill="none" stroke="#77756f" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" />
          </marker>
        </defs>

        <!-- User request → -->
        <path d="M405 288 H566" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- ← Final report -->
        <path d="M572 457 H410" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- Lead ← Citations -->
        <path d="M806 347 H762" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- Lead ↔ Memory -->
        <path d="M1134 347 H1186" stroke="#77756f" stroke-width="1.4" fill="none" marker-start="url(#arr)" marker-end="url(#arr)" />

        <!-- Lead ↔ Search 1 (diagonal doble) -->
        <path d="M800 444 L738 510" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
        <path d="M750 520 L812 454" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- Lead ↔ Search 2 (vertical doble) -->
        <path d="M958 520 V455" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
        <path d="M970 455 V515" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- Lead ↔ Search 3 (diagonal doble) -->
        <path d="M1128 444 L1190 510" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
        <path d="M1178 520 L1116 454" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />

        <!-- Bucles de cada search subagent -->
        <path d="M636 587 C636 640 720 640 720 591" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
        <path d="M877 587 C877 640 961 640 961 591" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
        <path d="M1118 587 C1118 640 1202 640 1202 591" stroke="#77756f" stroke-width="1.4" fill="none" marker-end="url(#arr)" />
      </svg>
    </div>
  </div>
</template>

<script setup>
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'

const props = defineProps({
  escala: {
    type: Number,
    default: 1,
  },
})

const stage = ref(null)
const scale = ref(0.9)
let observer

function updateScale() {
  if (!stage.value) return
  const width = stage.value.clientWidth
  const height = stage.value.clientHeight
  if (!width || !height) return
  scale.value = Math.min(width / 1396, height / 740) * 0.96 * props.escala
}

watch(() => props.escala, updateScale)

onMounted(() => {
  updateScale()
  observer = new ResizeObserver(updateScale)
  observer.observe(stage.value)
})

onBeforeUnmount(() => {
  observer?.disconnect()
})
</script>

<style scoped>
.stage {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #fbfaf7;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  color: #7a7872;
}

.canvas {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 1396px;
  height: 740px;
  transform-origin: center;
}

.ctr {
  position: absolute;
  transform: translate(-50%, -50%);
  white-space: nowrap;
  text-align: center;
  margin: 0;
}

.title {
  font-size: 38px;
  font-weight: 700;
  color: #1e1e1e;
  letter-spacing: -0.2px;
}

.panel {
  position: absolute;
  background: #ecebe6;
  border-radius: 24px;
}

.panel-title {
  font-size: 30px;
  font-weight: 600;
  color: #7a7872;
  line-height: 1.1;
}

.logo {
  position: absolute;
  width: 44px;
  height: 44px;
  border-radius: 8px;
  object-fit: cover;
}

.prompt-title {
  position: absolute;
  font-size: 22px;
  font-weight: 700;
  line-height: 26px;
  color: #7a7872;
  white-space: nowrap;
}

.prompt-text {
  position: absolute;
  font-size: 16.5px;
  font-weight: 500;
  line-height: 20.8px;
  color: #7a7872;
  white-space: nowrap;
}

.user-icon {
  position: absolute;
}

.arrow-label {
  font-size: 16px;
  font-weight: 600;
  color: #7a7872;
}

.box {
  position: absolute;
  background: #b8d3f1;
  border-radius: 6px;
  color: #3b4d68;
  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  text-align: center;
}

.ctr-flex {
  display: flex;
  align-items: center;
  justify-content: center;
  white-space: nowrap;
}

.lead {
  padding-top: 20px;
  box-sizing: border-box;
}

.lead-title {
  font-size: 22px;
  font-weight: 700;
  line-height: 26px;
}

.lead-tools {
  margin-top: 26px;
  font-size: 16px;
  font-weight: 600;
  line-height: 20px;
  white-space: nowrap;
}

.arrows {
  position: absolute;
  left: 0;
  top: 0;
  pointer-events: none;
  overflow: visible;
}
</style>