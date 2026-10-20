<template>
  <div ref="stage" class="relative h-full w-full overflow-hidden bg-[#f4f3ef] font-serif text-[#5c5c5c]">
    <div class="architecture-canvas" :style="{ transform: `translate(-50%, -50%) scale(${scale})` }">
      <div class="flex h-full w-full gap-4">
        <!-- ================= COLUMNA 1: CHAT ================= -->
        <div class="flex h-full w-[400px] flex-col rounded-[2rem] bg-[#edeae3] p-8">
          <h2 class="mb-12 text-[2.8rem] font-bold leading-[1.1] text-[#7a7771]">
            Claude.ai<br />chat
          </h2>

          <div class="space-y-8">
            <!-- Input Simulado (Logo + Texto) -->
            <div class="flex items-start gap-4">
              <img
                src="../components/assets/anthropic-logo.png"
                alt="Anthropic Logo"
                class="h-12 w-12 shrink-0 rounded-xl object-cover"
              />

              <div class="text-[1.4rem] font-bold leading-tight text-[#7a7771]">
                What's on<br />your<br />mind<br />tonight?
              </div>
            </div>

            <!-- Prompt del usuario -->
            <div class="flex items-start gap-4">
              <div class="flex h-12 w-12 shrink-0 items-center justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="#7a7771" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="5" /><path d="M20 21a8 8 0 0 0-16 0" /></svg>
              </div>

              <div class="text-[1.05rem] font-medium leading-[1.3] text-[#7a7771]">
                what are all the<br />
                companies in<br />
                the<br />
                united states<br />
                working<br />
                on AI agents in<br />
                2025?<br />
                make a list of at<br />
                least 100. for<br />
                each<br />
                company,<br />
                include<br />
                the name,<br />
                website,<br />
                product, description<br />
                of what they do,<br />
                type of agents<br />
                they build, and their<br />
                vertical/industry.
              </div>
            </div>
          </div>
        </div>

        <!-- ================= COLUMNA 2: FLECHAS CENTRALES ================= -->
        <div class="flex h-full w-[100px] flex-col items-center justify-between py-32">
          <!-- Flecha User Request -->
          <div class="flex w-full flex-col items-center">
            <span class="mb-2 text-center text-[1.1rem] font-bold leading-tight text-[#7a7771]">User<br />request</span>
            <svg width="90%" height="20" viewBox="0 0 100 20" preserveAspectRatio="none" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M0 10H95M95 10L85 3M95 10L85 17" stroke="#7a7771" stroke-width="1.5" />
            </svg>
          </div>

          <!-- Flecha Final Report -->
          <div class="flex w-full flex-col items-center">
            <span class="mb-2 text-center text-[1.1rem] font-bold leading-tight text-[#7a7771]">Final<br />report</span>
            <svg width="90%" height="20" viewBox="0 0 100 20" preserveAspectRatio="none" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M100 10H5M5 10L15 3M5 10L15 17" stroke="#7a7771" stroke-width="1.5" />
            </svg>
          </div>
        </div>

        <!-- ================= COLUMNA 3: SISTEMA MULTI-AGENTE ================= -->
        <div class="relative flex h-full w-[868px] flex-col rounded-[2rem] bg-[#edeae3] p-8">
          <h2 class="mb-8 shrink-0 text-center text-[2.8rem] font-bold leading-[1.1] text-[#7a7771]">
            Multi-agent research system
          </h2>

          <!-- Contenedor del Grid -->
          <div class="relative flex h-[650px] w-full flex-col justify-between px-6 pb-14">
            <!-- Fila Superior (Citations, Lead, Memory) -->
            <div class="relative z-10 flex h-[200px] w-full items-center justify-between">
              <!-- Citations Subagent -->
              <div class="w-[160px] rounded-xl bg-[#b9d2f1] p-5 text-center text-[1.25rem] font-bold leading-tight text-[#3b4c66] shadow-sm">
                Citations<br />subagent
              </div>

              <!-- Lead agent -->
              <div class="z-20 w-[360px] rounded-xl bg-[#b9d2f1] p-6 text-center text-[#3b4c66] shadow-sm">
                <div class="mb-3 text-[1.7rem] font-bold leading-tight">Lead agent<br />(orchestrator)</div>
                <div class="text-[1.05rem] font-semibold leading-tight">
                  Tools: search tools + MCP tools<br />
                  + memory + run_subagent +<br />
                  complete_task
                </div>
              </div>

              <!-- Memory -->
              <div class="w-[140px] rounded-xl bg-[#b9d2f1] p-5 text-center text-[1.25rem] font-bold leading-tight text-[#3b4c66] shadow-sm">
                Memory
              </div>
            </div>

            <!-- Flechas Conectoras -->
            <svg class="pointer-events-none absolute inset-y-0 left-6 right-6 z-0" viewBox="0 0 756 650" preserveAspectRatio="none" fill="none" xmlns="http://www.w3.org/2000/svg">
              <defs>
                <marker id="arch-arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
                  <path d="M0 0L10 5L0 10Z" fill="#7a7771" />
                </marker>
              </defs>

              <!-- Citations -> Lead -->
              <path d="M160 100H198" stroke="#7a7771" stroke-width="1.5" marker-start="url(#arch-arrow)" marker-end="url(#arch-arrow)" />

              <!-- Lead -> Memory -->
              <path d="M558 100H616" stroke="#7a7771" stroke-width="1.5" marker-start="url(#arch-arrow)" marker-end="url(#arch-arrow)" />

              <!-- Lead -> Search subagents -->
              <path d="M368 200L80 498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
              <path d="M388 200L100 498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
              <path d="M368 200V498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
              <path d="M388 200V498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
              <path d="M368 200L656 498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
              <path d="M388 200L676 498" stroke="#7a7771" stroke-width="1.5" marker-end="url(#arch-arrow)" />
            </svg>

            <!-- Fila Inferior (Search Subagents) -->
            <div class="relative z-10 flex h-[96px] w-full items-start justify-between">
              <!-- Search 1 -->
              <div class="relative flex w-[180px] flex-col items-center">
                <div class="w-full rounded-xl bg-[#b9d2f1] p-5 text-center text-[1.25rem] font-bold leading-tight text-[#3b4c66] shadow-sm">
                  Search<br />subagent
                </div>
                <svg class="absolute -bottom-10 left-1/2 -translate-x-1/2" width="70" height="40" viewBox="0 0 70 40" fill="none">
                  <path d="M10 0C10 25 60 25 60 0" stroke="#7a7771" stroke-width="1.5" />
                  <path d="M60 0L54 8M60 0L66 8" stroke="#7a7771" stroke-width="1.5" />
                </svg>
              </div>

              <!-- Search 2 -->
              <div class="relative flex w-[180px] flex-col items-center">
                <div class="w-full rounded-xl bg-[#b9d2f1] p-5 text-center text-[1.25rem] font-bold leading-tight text-[#3b4c66] shadow-sm">
                  Search<br />subagent
                </div>
                <svg class="absolute -bottom-10 left-1/2 -translate-x-1/2" width="70" height="40" viewBox="0 0 70 40" fill="none">
                  <path d="M10 0C10 25 60 25 60 0" stroke="#7a7771" stroke-width="1.5" />
                  <path d="M60 0L54 8M60 0L66 8" stroke="#7a7771" stroke-width="1.5" />
                </svg>
              </div>

              <!-- Search 3 -->
              <div class="relative flex w-[180px] flex-col items-center">
                <div class="w-full rounded-xl bg-[#b9d2f1] p-5 text-center text-[1.25rem] font-bold leading-tight text-[#3b4c66] shadow-sm">
                  Search<br />subagent
                </div>
                <svg class="absolute -bottom-10 left-1/2 -translate-x-1/2" width="70" height="40" viewBox="0 0 70 40" fill="none">
                  <path d="M10 0C10 25 60 25 60 0" stroke="#7a7771" stroke-width="1.5" />
                  <path d="M60 0L54 8M60 0L66 8" stroke="#7a7771" stroke-width="1.5" />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>
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
const scale = ref(0.86)
let observer

function updateScale() {
  if (!stage.value) return
  const width = stage.value.clientWidth
  const height = stage.value.clientHeight
  if (!width || !height) return
  scale.value = Math.min(width / 1400, height / 800) * 0.96 * props.escala
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
.architecture-canvas {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 1400px;
  height: 800px;
  transform-origin: center;
}

/* Forzamos una fuente Serif clásica para igualar la imagen */
.font-serif {
  font-family: 'Georgia', 'Times New Roman', serif;
}
</style>
